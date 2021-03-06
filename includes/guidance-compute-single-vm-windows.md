本文概述了在 Azure 上运行 Windows 虚拟机 (VM) 的一套经过验证的做法，这些做法注重可扩展性、可用性、可管理性和安全性。

> [!NOTE]
> Azure 有两个不同的部署模型： [Azure Resource Manager][resource-manager-overview] 和经典模型。 本文使用资源管理器，Azure 建议将它用于新部署。
>
>

我们不建议为任务关键工作负荷使用单个 VM，因为它会导致单点故障。 若要获得更高的可用性，请在 [可用性集][availability-set]中部署多个 VM。

## <a name="architecture-diagram"></a>体系结构关系图

在 Azure 中预配 VM 涉及更多移动部件，而不只是 VM 本身。 有计算、网络和存储元素。

> 可从 [Microsoft 下载中心][visio-download]下载包含此体系结构图的 Visio 文档。 此图位于“计算 - 单一 VM”页上。
>
>

![[0]][0]

* **资源组。** [*资源组*][resource-manager-overview] 是一个容器，包含相关资源。 创建资源组以保存此 VM 的资源。
* **VM**。 可以基于已发布映像列表或上传到 Azure Blob 存储的虚拟硬盘 (VHD) 文件预配 VM。
* **OS 磁盘。** OS 磁盘是存储在 [Azure 存储][azure-storage]中部署多个 VM。 这意味着它一直存在，即使主机出现故障，也是如此。
* **临时磁盘。** 使用临时磁盘（Windows 上的 `D:` 盘）创建 VM。 此磁盘存储在主机的物理驱动器上。 它 *不* 保存在 Azure 存储中，并且可能会在重新启动和其他 VM 生命周期事件过程中被删除。 只使用此磁盘存储临时数据，如页面文件或交换文件。
* **数据磁盘。** [数据磁盘][data-disk] 是用于应用程序数据的持久性 VHD。 数据磁盘像 OS 磁盘一样，存储在 Azure 存储中。
* **虚拟网络 (VNet) 和子网。** Azure 中的每个 VM 都部署在 VNet 中，后者进一步划分为多个子网。
* **公共 IP 地址。** 需要使用公共 IP 地址与 VM 通信 &mdash; 例如，通过远程桌面 (RDP)。
* **网络接口 (NIC)**。 NIC 使 VM 能够与虚拟网络进行通信。
* **网络安全组 (NSG)**。 [NSG][nsg] 用于允许/拒绝到子网的网络流量。 可以将 NSG 与单个 NIC 或子网相关联。 如果将 NSG 与一个子网相关联，则 NSG 规则适用于该子网中的所有 VM。
* **诊断。** 诊断日志记录对于 VM 管理和故障排除至关重要。

## <a name="recommendations"></a>建议

以下建议适用于大多数情况。 请遵循这些建议，除非有更优先的特定要求。

### <a name="vm-recommendations"></a>VM 建议

Azure 可提供多种虚拟机大小，但建议使用 DS 和 GS 系列，因为相关计算机大小支持 [高级存储][premium-storage]。 请选择其中一个计算机大小，除非存在专用工作负荷（如高性能计算）。 有关详细信息，请参阅 [virtual machine sizes][virtual-machine-sizes]（虚拟机大小）。

如果要将现有工作负荷转移到 Azure，开始时请先使用与本地服务器最匹配的 VM 大小。 然后测量与 CPU、内存和每秒磁盘输入/输出操作次数 (IOPS) 有关的实际工作负荷的性能，并根据需要调整大小。 如果 VM 需要多个 NIC，请注意 NIC 的最大数量是 [VM 大小][vm-size-tables]的函数。   

预配 VM 和其他资源时，必须指定区域。 通常应选择离内部用户或客户最近的区域。 但是，并非所有 VM 大小都可在所有区域中使用。 若要查看给定区域中可用 VM 大小的列表，请运行以下 Azure 命令行接口 (CLI) 命令：

```
azure vm sizes --location <location>
```

有关选择已发布的 VM 映像的信息，请参阅 [使用 PowerShell 或 CLI 在 Azure 中浏览和选择 Windows 虚拟机映像][select-vm-image]。

### <a name="disk-and-storage-recommendations"></a>磁盘和存储建议

为获得最佳磁盘 I/O 性能，建议使用[高级存储][premium-storage]，它在固态硬盘 (SSD) 上存储数据。 成本取决于预配磁盘的大小。 IOPS 和吞吐量也取决于磁盘大小，因此，在预配磁盘时，请考虑所有三个因素（容量、IOPS 和吞吐量）。

为避免达到存储帐户的 IOPS 限制，请为每个 VM 创建单独的 Azure 存储帐户来存放虚拟硬盘 (VHD)。

添加一个或多个数据磁盘。 在创建新 VHD 时，它未设置格式。 登录到 VM 对磁盘进行格式化。 如果有大量数据磁盘，请注意存储帐户的总 I/O 限制。 

如果可能，请将应用程序安装在数据磁盘上，而不是 OS 磁盘上。 但是，某些旧版应用程序可能需要将组件安装在 C: 驱动器上。 在这种情况下，可以使用 PowerShell [调整 OS 磁盘的大小][resize-os-disk]。

为获得最佳性能，请创建单独的存储帐户来存储诊断日志。 标准的本地冗余存储 (LRS) 帐户足以存储诊断日志。

### <a name="network-recommendations"></a>网络建议

公共 IP 地址可以是动态的或静态的。 默认是动态的。

* 如果需要不会更改的固定 IP 地址 &mdash; 例如，如果需要在 DNS 中创建 A 记录，或者需要将 IP 地址添加到安全列表，请保留[静态 IP 地址][static-ip]。
* 还可以为 IP 地址创建完全限定的域名 (FQDN)。 然后，可以在 DNS 中注册指向 FQDN 的 [CNAME 记录][cname-record]。 有关详细信息，请参阅[在 Azure 门户中创建完全限定的域名][fqdn]。

所有 NSG 都包含一组 [默认规则][nsg-default-rules]，其中包括阻止所有入站 Internet 流量的规则。 无法删除默认规则，但其他规则可以覆盖它们。 若要启用 Internet 流量，请创建允许特定端口的入站流量的规则 &mdash; 例如，将端口 80 用于 HTTP。  

若要启用 RDP，请添加允许 TCP 端口 3389 的入站流量的 NSG 规则。

## <a name="scalability-considerations"></a>可伸缩性注意事项

可以通过[更改 VM 大小](../articles/virtual-machines/windows/sizes.md)来扩大或缩小 VM。 若要水平横向扩展，请将两个或更多 VM 放入负载均衡器后面的可用性集中。

## <a name="availability-considerations"></a>可用性注意事项

若要获得更高的可用性，请在可用性集中部署多个 VM。 这样还可提供更高的[服务级别协议][vm-sla] (SLA)。

VM 可能会受到[计划内维护][planned-maintenance]或[计划外维护][manage-vm-availability]的影响。 可以使用 [VM 重新启动日志][reboot-logs] 来确定 VM 重新启动是否是由计划内维护导致的。

VHD 存储在 [Azure 存储][azure-storage]中，Azure 存储将进行复制以实现持久性和可用性。

若要防止在正常操作期间意外丢失数据（例如，由于用户错误），则还应使用 [Blob 快照][blob-snapshot]或其他工具实施时间点备份。

## <a name="manageability-considerations"></a>可管理性注意事项

**资源组。** 将共享相同生命周期的紧密耦合资源放入同一[资源组][resource-manager-overview]中部署多个 VM。 资源组可让你以组的形式部署和监视资源，并按资源组汇总计费成本。 还可以删除作为集的资源，这对于测试部署非常有用。 为资源指定有意义的名称。 这样，可更轻松地找到特定资源并了解其角色。

**VM 诊断。** 启用监视和诊断，包括基本运行状况指标、诊断基础结构日志和 [启动诊断][boot-diagnostics]中部署多个 VM。 如果 VM 陷入不可启动状态，启动诊断可帮助你诊断启动故障。 使用 [Azure 日志收集][log-collector]扩展收集 Azure 平台日志并将其上传到 Azure 存储。   

以下 CLI 命令可启用诊断：

```
azure vm enable-diag <resource-group> <vm-name>
```

**停止 VM。** Azure 对“已停止”和“已解除分配”状态进行了区分。 VM 状态为“已停止”时，将计费，但 VM 为“已解除分配”状态时，则不计费。

使用以下 CLI 命令可解除分配 VM：

```
azure vm deallocate <resource-group> <vm-name>
```

在 Azure 门户中，“停止”按钮可释放 VM。 但是，如果在已登录时通过 OS 关闭，VM 将停止，但*不*会解除分配，因此仍将收费。

**删除 VM。** 如果删除 VM，则不会删除 VHD。 这意味着可以安全地删除 VM，而不会丢失数据。 但是，仍将收取存储费用。 若要删除 VHD，请从 [Blob 存储][blob-storage]中删除相应文件。

若要防止意外删除，请使用 [资源锁][resource-lock] 锁定整个资源组或锁定单个资源（如 VM）。

## <a name="security-considerations"></a>安全注意事项

**修补程序管理。** 如果启用，安全中心将检查是否缺少安全更新和关键更新。 使用 VM 上的 [组策略设置][group-policy] 可启用自动系统更新。

**反恶意软件。** 如果启用，安全中心将检查是否已安装反恶意软件。 还可以使用安全中心从 Azure 门户中安装反恶意软件。

**操作。** 使用[基于角色的访问控制][rbac] (RBAC) 控制对部署的 Azure 资源的访问。 RBAC 允许将授权角色分配给开发运营团队的成员。 例如，“读者”角色可以查看 Azure 资源，但不能创建、管理或删除这些资源。 某些角色特定于特定的 Azure 资源类型。 例如，“虚拟机参与者”角色可以执行重启或解除分配 VM、重置管理员密码、创建新的 VM 等操作。 可能对此参考体系结构有用的其他[内置 RBAC 角色][rbac-roles]包括[开发测试实验室用户][rbac-devtest]和[网络参与者][rbac-network]。 可将用户分配给多个角色，并且可以创建自定义角色以实现更细化的权限。

> [!NOTE]
> RBAC 不限制已登录到 VM 的用户可以执行的操作。 这些权限由来宾 OS 上的帐户类型决定。   
>
>

若要重置本地管理员密码，请运行 `vm reset-access` Azure CLI 命令。

```
azure vm reset-access -u <user> -p <new-password> <resource-group> <vm-name>
```

使用[审核日志][audit-logs]可查看预配操作和其他 VM 事件。

**数据加密。** 如果需要加密 OS 磁盘和数据磁盘，请考虑使用 [Azure 磁盘加密][disk-encryption]。

## <a name="solution-deployment"></a>解决方案部署

[GitHub][github-folder]中提供了此参考体系结构的部署。 它包括 VNet、NSG 和单个 VM。 若要部署体系结构，请遵循以下步骤：

1. 右键单击下面的按钮，并选择“在新选项卡中打开链接”或“在新窗口中打开链接”。  
    [![“部署到 Azure”](./media/guidance-compute-single-vm-linux/deploybutton.png)](https://portal.azure.cn/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fvirtual-machines%2Fsingle-vm%2Fazuredeploy.json)
2. 在 Azure 门户中打开该链接后，必须输入某些设置的值：

   * 参数文件中已定义“资源组”名称，因此请选择“新建”，然后在文本框中输入 `ra-single-vm-rg`。
   * 从“位置”  下拉框中选择区域。
   * 不要编辑“模板根 URI”或“参数根 URI”文本框。
   * 在“OS 类型”下拉框中选择“Windows”。
   * 查看条款和条件，并单击“我同意上述条款和条件”复选框。
   * 单击“购买”  按钮。
3. 等待部署完成。
4. 参数文件包含硬编码的管理员用户名和密码，强烈建议马上更改这两项信息。 在 Azure 门户中单击名为 `ra-single-vm0 ` 的 VM。 然后，在“支持 + 故障排除”边栏选项卡中单击“重置密码”。 在“模式”下拉框中选择“重置密码”，然后选择新**用户名**和**密码**。 单击“更新”  按钮来持久保存新的用户名和密码。

有关部署此参考体系结构的其他方式的信息，请参阅 [guidance-single-vm][github-folder] GitHub 文件夹中的自述文件。

## <a name="customize-the-deployment"></a>自定义部署
如果需要更改部署以满足需求，请按照 [readme][github-folder] 中的说明操作。

## <a name="next-steps"></a>后续步骤
若要获得更高的可用性，请为负载均衡器部署两个或以上 VM。

<!-- links -->

[audit-logs]: https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]:../articles/virtual-machines/windows/create-availability-set.md
[azure-cli]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/storage/storage-about-disks-and-vhds-windows.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md

[fqdn]:../articles/virtual-machines/windows/portal-create-fqdn.md
[github-folder]: http://github.com/mspnp/reference-architectures/tree/master/virtual-machines/single-vm
[group-policy]: https://technet.microsoft.com/library/dn595129.aspx
[log-collector]: https://azure.microsoft.com/blog/simplifying-virtual-machine-troubleshooting-using-azure-log-collector/
[manage-vm-availability]:../articles/virtual-machines/windows/manage-availability.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[planned-maintenance]:../articles/virtual-machines/windows/planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-labs-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/blog/viewing-vm-reboot-logs/
[resize-os-disk]:../articles/virtual-machines/windows/expand-os-disk.md
[Resize-VHD]: https://technet.microsoft.com/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[security-center]: https://www.azure.cn/home/features/security-center/
[select-vm-image]:../articles/virtual-machines/windows/cli-ps-findimage.md
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-price]: https://www.azure.cn/pricing/details/storage/
[virtual-machine-sizes]: ../articles/virtual-machines/windows/sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-resize]:../articles/virtual-machines/linux/change-vm-size.md
[vm-sla]: https://www.azure.cn/support/sla/virtual-machines
[vm-size-tables]: ../articles/virtual-machines/windows/sizes.md
[0]: ./media/guidance-blueprints/compute-single-vm.png "Azure 中的单一 Windows VM 体系结构"
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[blocks]: https://github.com/mspnp/template-building-blocks

