---
title: Azure Site Recovery 中的 VMware/物理灾难恢复支持列表
description: 汇总了使用 Azure Site Recovery 将 VMware VM 和物理服务器灾难恢复到 Azure 的支持。
ms.topic: conceptual
origin.date: 07/14/2020
author: rockboyfor
ms.date: 09/14/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: 2e74fd61c6eeb538c7a2c1f1968a71307d232e90
ms.sourcegitcommit: e1cd3a0b88d3ad962891cf90bac47fee04d5baf5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2020
ms.locfileid: "89655469"
---
# <a name="support-matrix-for-disaster-recovery--of-vmware-vms-and-physical-servers-to-azure"></a>将 VMware VM 和物理服务器灾难恢复到 Azure 时的支持矩阵

本文汇总了使用 [Azure Site Recovery](site-recovery-overview.md) 服务执行从 VMware VM 和物理服务器到 Azure 的灾难恢复时支持的组件和设置。

- [详细了解](vmware-azure-architecture.md) VMware VM/物理服务器灾难恢复体系结构。
- 遵循我们的[教程](tutorial-prepare-azure.md)尝试进行灾难恢复。

> [!NOTE]
> Site Recovery 不会将客户数据移到或存储在目标区域之外，目标区域中已为源计算机设置了灾难恢复。 如果客户愿意，可以从其他地区选择恢复服务保管库。 恢复服务保管库包含元数据，但不包含实际的客户数据。

## <a name="deployment-scenarios"></a>部署方案

**方案** | **详细信息**
--- | ---
VMware VM 的灾难恢复 | 将本地 VMware VM 复制到 Azure。 可以在 Azure 门户中部署此方案，也可使用 [PowerShell](vmware-azure-disaster-recovery-powershell.md) 进行部署。
物理服务器的灾难恢复 | 将本地 Windows/Linux 物理服务器复制到 Azure。 可以在 Azure 门户中部署此方案。

## <a name="on-premises-virtualization-servers"></a>本地虚拟化服务器

**Server** | **要求** | **详细信息**
--- | --- | ---
vCenter Server | 版本 7.0、6.7、6.5、6.0 或 5.5 | 建议在灾难恢复部署中使用 vCenter 服务器。
vSphere 主机 | 版本 7.0、6.7、6.5、6.0 或 5.5 | 建议 vSphere 主机和 vCenter 服务器与进程服务器位于同一网络。 默认情况下，进程服务器在配置服务器上运行。 [了解详细信息](vmware-physical-azure-config-process-server-overview.md)。

## <a name="site-recovery-configuration-server"></a>Site Recovery 配置服务器

配置服务器是运行 Site Recovery 组件的本地计算机，这些组件包括配置服务器、进程服务器和主目标服务器。

- 对于 VMware VM，请下载用于创建 VMware VM 的 OVF 模板来设置配置服务器。
- 对于物理服务器，请手动设置配置服务器计算机。

**组件** | **要求**
--- |---
CPU 核心数 | 8
RAM | 16 GB
磁盘数目 | 3 磁盘<br/><br/> 磁盘包括 OS 磁盘、进程服务器缓存磁盘和用于故障回复的保留驱动器。
磁盘可用空间 | 提供 600 GB 空间用作进程服务器缓存。
磁盘可用空间 | 为保留驱动器提供 600 GB 空间。
操作系统  | Windows Server 2012 R2，或具有桌面体验的 Windows Server 2016 <br/><br /> 如果计划使用此设备的内置主目标进行故障回复，请确保操作系统版本与复制的项相同或为更高版本。|
操作系统区域设置 | 英语 (en-us)
[PowerCLI](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) | 在配置服务器版本 [9.14](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery) 或更高版本中不需要。
Windows Server 角色 | 不要启用 Active Directory 域服务、Internet Information Services (IIS) 或 Hyper-V。
组策略| - 阻止访问命令提示符。 <br/> - 阻止访问注册表编辑工具。 <br/> - 信任文件附件的逻辑。 <br/> - 打开脚本执行。 <br/> - [了解详细信息](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-7/gg176671(v=ws.10))|
IIS | 确保：<br/><br/> - 无预先存在的默认网站 <br/> - 启用[匿名身份验证](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731244(v=ws.10)) <br/> - 启用 [FastCGI](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753077(v=ws.10)) 设置  <br/> - 端口 443 上没有预先存在的网站/应用侦听<br/>
NIC 类型 | VMXNET3（部署为 VMware VM 时）
IP 地址类型 | 静态
端口 | 443，用于控制通道协调<br/>9443，用于数据传输

## <a name="replicated-machines"></a>复制的计算机

Site Recovery 支持复制在支持的计算机上运行的任何工作负荷。

**组件** | **详细信息**
--- | ---
计算机设置 | 复制到 Azure 的计算机必须满足 [Azure 要求](#azure-vm-requirements)。
计算机工作负载 | Site Recovery 支持复制在支持的计算机上运行的任何工作负荷。 [了解详细信息](../site-recovery/site-recovery-workload.md)。
计算机名称 | 确保计算机的显示名称不属于 [Azure 保留的资源名称](../azure-resource-manager/templates/error-reserved-resource-name.md)<br/><br/> 逻辑卷名称不区分大小写。 请确保设备上不存在两个卷具有相同名称的情况。 例如：无法通过 Azure Site Recovery 保护名称为“voLUME1”、“volume1”的卷。

### <a name="for-windows"></a>对于 Windows

**操作系统** | **详细信息**
--- | ---
Windows Server 2019 | 从[更新汇总 34](https://support.microsoft.com/help/4490016) 开始受支持（移动服务版本 9.22 和更高版本）。
Windows Server 2016 64 位 | 支持服务器核心、带桌面体验的服务器。
Windows Server 2012 R2/Windows Server 2012 | 。
Windows Server 2008 R2 SP1 及更高版本。 | 。<br/><br/> 在移动服务代理的 [9.30](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery) 版本中，需要在运行 Windows 2008 R2 SP1 或更高版本的计算机上安装[服务堆栈更新 (SSU)](https://support.microsoft.com/help/4490628) 和 [SHA-2 更新](https://support.microsoft.com/help/4474419)。 从 2019 年 9 月开始不再支持 SHA-1，如果未启用 SHA-2 代码签名，则无法按预期方式安装/升级代理扩展。 详细了解 [SHA-2 升级和要求](https://aka.ms/SHA-2KB)。
Windows Server 2008 SP2 或更高版本（64 位/32 位） |  仅支持迁移。 [了解详细信息](migrate-tutorial-windows-server-2008.md)。<br/><br/> 在移动服务代理的 [9.30](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery) 版本中，需要在 Windows 2008 SP2 计算机上安装[服务堆栈更新 (SSU)](https://support.microsoft.com/help/4493730) 和 [SHA-2 更新](https://support.microsoft.com/help/4474419)。 从 2019 年 9 月开始不再支持 SHA-1，如果未启用 SHA-2 代码签名，则无法按预期方式安装/升级代理扩展。 详细了解 [SHA-2 升级和要求](https://support.microsoft.com/help/4472027/2019-sha-2-code-signing-support-requirement-for-windows-and-wsus)。
Windows 10 | 。
包含 SP1 的 Windows 7 64 位 | 从[更新汇总 36](https://support.microsoft.com/help/4503156) 开始受支持（移动服务版本 9.22 和更高版本）。 <br /><br /> 在移动服务代理的 [9.30](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery) 中，需要在 Windows 7 SP1 计算机上安装[服务堆栈更新 (SSU)](https://support.microsoft.com/help/4490628) 和 [SHA-2 更新](https://support.microsoft.com/help/4474419)。  从 2019 年 9 月开始不再支持 SHA-1，如果未启用 SHA-2 代码签名，则无法按预期方式安装/升级代理扩展。 详细了解 [SHA-2 升级和要求](https://support.microsoft.com/help/4472027/2019-sha-2-code-signing-support-requirement-for-windows-and-wsus)。

<!-- Not Available on Windows 8.1, Windows 8 are not supported.-->

### <a name="for-linux"></a>对于 Linux

**操作系统** | **详细信息**
--- | ---
Linux | 仅支持 64 位系统。 不支持 32 位系统。<br/><br/>每个 Linux 服务器上应该装有 [Linux Integration Services (LIS) 组件](https://www.microsoft.com/download/details.aspx?id=55106)。 测试故障转移/故障转移后，需要在 Azure 中启动该服务器。 如果缺少内置 LIS 组件，请确保在启用复制之前安装这些[组件](https://www.microsoft.com/download/details.aspx?id=55106)，使计算机在 Azure 中启动。 <br/><br/> Site Recovery 会协调故障转移，以在 Azure 中运行 Linux 服务器。 但是，Linux 供应商可能会限制仅支持尚未达到使用寿命的分发版本。<br/><br/> 在 Linux 发行版中，仅支持属于分发次要版本/更新的原版内核。<br/><br/> 不支持跨主要 Linux 发行版升级受保护的计算机。 若要升级，请禁用复制，升级操作系统，然后再重新启用复制。<br/><br/> [详细了解](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure) Azure 中的 Linux 和开源技术支持。
Linux：CentOS | 5.2 到 5.11</b><br/> 6.1 到 6.10</b><br/> <br /> 7.0、7.1、7.2、7.3、7.4、7.5、7.6、[7.7](https://support.microsoft.com/help/4528026/update-rollup-41-for-azure-site-recovery)、[7.8](https://support.microsoft.com/help/4564347/)、[7.9](https://support.microsoft.com/help/4578241/) <br /> [8.0](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery)、8.1、[8.2](https://support.microsoft.com/help/4570609) <br/><br/> 运行 CentOS 5.2-5.11 和 6.1-6.10 的服务器上较旧的内核基本都预装了 [Linux Integration Services (LIS) 组件](https://www.microsoft.com/download/details.aspx?id=55106)。 如果缺少内置 LIS 组件，请确保在启用复制之前安装这些[组件](https://www.microsoft.com/download/details.aspx?id=55106)，使计算机在 Azure 中启动。
Ubuntu | Ubuntu 14.04 LTS 服务器[（查看支持的内核版本）](#ubuntu-kernel-versions)<br/><br/>Ubuntu 16.04 LTS 服务器[（查看支持的内核版本）](#ubuntu-kernel-versions) <br /> Ubuntu 18.04 LTS 服务器[（查看支持的内核版本）](#ubuntu-kernel-versions)；[9.36](https://support.microsoft.com/help/4578241/) 开始支持 Ubuntu 18.4.03（内核 v5.4） <br /> Ubuntu 20.04 LTS 服务器[（查看支持的内核版本）](#ubuntu-kernel-versions)
Debian | Debian 7/Debian 8（包括对所有 7. x、8. x 版本的支持）[（查看支持的内核版本）](#debian-kernel-versions)
SUSE Linux | SUSE Linux Enterprise Server 12 SP1、SP2、SP3、SP4、[SP5](https://support.microsoft.com/help/4570609)[（查看支持的内核版本）](#suse-linux-enterprise-server-12-supported-kernel-versions) <br/> SUSE Linux Enterprise Server 15、15 SP1 [（查看支持的内核版本）](#suse-linux-enterprise-server-15-supported-kernel-versions) <br/> SUSE Linux Enterprise Server 11 SP3。 [请确保在配置服务器上下载最新的移动代理安装程序](vmware-physical-mobility-service-overview.md#download-latest-mobility-agent-installer-for-suse-11-sp3-server)。 <br /> SUSE Linux Enterprise Server 11 SP4 <br /> **注意**：不支持将复制计算机从 SUSE Linux Enterprise Server 11 SP3 升级到 SP4。 若要升级，请禁用复制并在升级后重新启用它。 <br/>|

<!-- Not Available on Linux Red Hat Enterprise: -->
<!-- Not Available on Oracle Linux -->

> [!Note]
> 对于每个 Windows 版本，Azure Site Recovery 仅支持[长期服务渠道 (LTSC)](https://docs.microsoft.com/windows-server/get-started-19/servicing-channels-19#long-term-servicing-channel-ltsc) 生成。  目前不支持[半年渠道](https://docs.microsoft.com/windows-server/get-started-19/servicing-channels-19#semi-annual-channel)版本。

### <a name="ubuntu-kernel-versions"></a>Ubuntu 内核版本

**支持的版本** | **移动服务版本** | **内核版本** |
--- | --- | --- |
14.04 LTS | [9.32][9.32 UR]、[9.33](https://support.microsoft.com/help/4564347/)、[9.34](https://support.microsoft.com/help/4570609)、[9.35](https://support.microsoft.com/help/4573888/)、[9.36](https://support.microsoft.com/help/4578241/) | 3.13.0-24-generic 到 3.13.0-170-generic、<br/>3.16.0-25-generic 到 3.16.0-77-generic、<br/>3.19.0-18-generic 到 3.19.0-80-generic、<br/>4.2.0-18-generic 到 4.2.0-42-generic、<br/>4.4.0-21-generic 到 4.4.0-148-generic、<br/>4.15.0-1023-azure 到 4.15.0-1045-azure |
|||
16.04 LTS | [9.36](https://support.microsoft.com/help/4578241/)| 4.4.0-21-generic 到 4.4.0-186-generic、<br/>4.8.0-34-generic 到 4.8.0-58-generic、<br/>4.10.0-14-generic 到 4.10.0-42-generic、<br/>4.11.0-13-generic 到 4.11.0-14-generic、<br/>4.13.0-16-generic 到 4.13.0-45-generic、<br/>4.15.0-13-generic 到 4.15.0-112-generic<br/>4.11.0-1009-azure 到 4.11.0-1016-azure、<br/>4.13.0-1005-azure 到 4.13.0-1018-azure <br/>4.15.0-1012-azure 到 4.15.0-1092-azure |
16.04 LTS | [9.35](https://support.microsoft.com/help/4573888/) | 4.4.0-21-generic 到 4.4.0-184-generic、<br/>4.8.0-34-generic 到 4.8.0-58-generic、<br/>4.10.0-14-generic 到 4.10.0-42-generic、<br/>4.11.0-13-generic 到 4.11.0-14-generic、<br/>4.13.0-16-generic 到 4.13.0-45-generic、<br/>4.15.0-13-generic 到 4.15.0-106-generic<br/>4.11.0-1009-azure 到 4.11.0-1016-azure、<br/>4.13.0-1005-azure 到 4.13.0-1018-azure <br/>4.15.0-1012-azure 到 4.15.0-1089-azure |
16.04 LTS | [9.34](https://support.microsoft.com/help/4570609) | 4.4.0-21-generic 到 4.4.0-178-generic、<br/>4.8.0-34-generic 到 4.8.0-58-generic、<br/>4.10.0-14-generic 到 4.10.0-42-generic、<br/>4.11.0-13-generic 到 4.11.0-14-generic、<br/>4.13.0-16-generic 到 4.13.0-45-generic、<br/>4.15.0-13-generic 到 4.15.0-101-generic<br/>4.11.0-1009-azure 到 4.11.0-1016-azure、<br/>4.13.0-1005-azure 到 4.13.0-1018-azure <br/>4.15.0-1012-azure 到 4.15.0-1083-azure |
16.04 LTS | [9.33](https://support.microsoft.com/help/4564347/) | 4.4.0-21-generic 到 4.4.0-178-generic、<br/>4.8.0-34-generic 到 4.8.0-58-generic、<br/>4.10.0-14-generic 到 4.10.0-42-generic、<br/>4.11.0-13-generic 到 4.11.0-14-generic、<br/>4.13.0-16-generic 到 4.13.0-45-generic、<br/>4.15.0-13-generic 到 4.15.0-99-generic<br/>4.11.0-1009-azure 到 4.11.0-1016-azure、<br/>4.13.0-1005-azure 到 4.13.0-1018-azure <br/>4.15.0-1012-azure 到 4.15.0-1082-azure|
16.04 LTS | [9.32][9.32 UR] | 4.4.0-21-generic 到 4.4.0-171-generic、<br/>4.8.0-34-generic 到 4.8.0-58-generic、<br/>4.10.0-14-generic 到 4.10.0-42-generic、<br/>4.11.0-13-generic 到 4.11.0-14-generic、<br/>4.13.0-16-generic 到 4.13.0-45-generic、<br/>4.15.0-13-generic 到 4.15.0-74-generic<br/>4.11.0-1009-azure 到 4.11.0-1016-azure、<br/>4.13.0-1005-azure 到 4.13.0-1018-azure <br/>4.15.0-1012-azure 到 4.15.0-1066-azure|
|||
18.04 LTS | [9.36](https://support.microsoft.com/help/4578241/) | 4.15.0-20-generic 到 4.15.0-112-generic <br /> 4.18.0-13-generic 到 4.18.0-25-generic <br /> 5.0.0-15-generic 到 5.0.0-58-generic <br /> 5.3.0-19-generic 到 5.3.0-64-generic <br /> 5.4.0-37-generic 到 5.4.0-42-generic<br /> 4.15.0-1009-azure 到 4.15.0-1092-azure <br /> 4.18.0-1006-azure 到 4.18.0-1025-azure <br /> 5.0.0-1012-azure 到 5.0.0-1036-azure <br /> 5.3.0-1007-azure 到 5.3.0-1032-azure <br /> 5.4.0-1020-azure 到 5.4.0-1022-azure|
18.04 LTS | [9.35](https://support.microsoft.com/help/4573888/) | 4.15.0-20-generic 到 4.15.0-108-generic <br /> 4.18.0-13-generic 到 4.18.0-25-generic <br /> 5.0.0-15-generic 到 5.0.0-52-generic <br /> 5.3.0-19-generic 到 5.3.0-61-generic <br /> 4.15.0-1009-azure 到 4.15.0-1089-azure <br /> 4.18.0-1006-azure 到 4.18.0-1025-azure <br /> 5.0.0-1012-azure 到 5.0.0-1036-azure <br /> 5.3.0-1007-azure 到 5.3.0-1031-azure|
18.04 LTS | [9.34](https://support.microsoft.com/help/4570609) | 4.15.0-20-generic 到 4.15.0-101-generic <br /> 4.18.0-13-generic 到 4.18.0-25-generic <br /> 5.0.0-15-generic 到 5.0.0-48-generic <br /> 5.3.0-19-generic 到 5.3.0-53-generic <br /> 4.15.0-1009-azure 到 4.15.0-1083-azure <br /> 4.18.0-1006-azure 到 4.18.0-1025-azure <br /> 5.0.0-1012-azure 到 5.0.0-1036-azure <br /> 5.3.0-1007-azure 到 5.3.0-1022-azure|
18.04 LTS | [9.33](https://support.microsoft.com/help/4564347/) | 4.15.0-20-generic 到 4.15.0-99-generic <br /> 4.18.0-13-generic 到 4.18.0-25-generic <br /> 5.0.0-15-generic 到 5.0.0-47-generic <br /> 5.3.0-19-generic 到 5.3.0-51-generic <br /> 4.15.0-1009-azure 到 4.15.0-1082-azure <br /> 4.18.0-1006-azure 到 4.18.0-1025-azure <br /> 5.0.0-1012-azure 到 5.0.0-1036-azure <br /> 5.3.0-1007-azure 到 5.3.0-1020-azure|
18.04 LTS | [9.32][9.32 UR]| 4.15.0-20-generic 到 4.15.0-74-generic <br /> 4.18.0-13-generic 到 4.18.0-25-generic <br /> 5.0.0-15-generic 到 5.0.0-37-generic <br /> 5.3.0-19-generic 到 5.3.0-24-generic <br /> 4.15.0-1009-azure 到 4.15.0-1037-azure <br /> 4.18.0-1006-azure 到 4.18.0-1025-azure <br /> 5.0.0-1012-azure 到 5.0.0-1028-azure <br /> 5.3.0-1007-azure 到 5.3.0-1009-azure|
|||
20.04 LTS |[9.36](https://support.microsoft.com/help/4578241/) | 5.4.0-26-generic 到 5.4.0-42 <br /> -generic 5.4.0-1010-azure 到 5.4.0-1022-azure

### <a name="debian-kernel-versions"></a>Debian 内核版本

**支持的版本** | **移动服务版本** | **内核版本** |
--- | --- | --- |
Debian 7 | [9.32][9.32 UR]、[9.33](https://support.microsoft.com/help/4564347/)、[9.34](https://support.microsoft.com/help/4570609)、[9.35](https://support.microsoft.com/help/4573888/)、[9.36](https://support.microsoft.com/help/4578241/)| 3.2.0-4-amd64 到 3.2.0-6-amd64、3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | [9.35](https://support.microsoft.com/help/4573888/)、[9.36](https://support.microsoft.com/help/4578241/) | 3.16.0-4-amd64 到 3.16.0-11-amd64、4.9.0-0.bpo.4-amd64 到 4.9.0-0.bpo.11-amd64 |
Debian 8 | [9.32][9.32 UR]、[9.33](https://support.microsoft.com/help/4564347/)、[9.34](https://support.microsoft.com/help/4570609) | 3.16.0-4-amd64 到 3.16.0-10-amd64、4.9.0-0.bpo.4-amd64 到 4.9.0-0.bpo.11-amd64 |

### <a name="suse-linux-enterprise-server-12-supported-kernel-versions"></a>SUSE Linux Enterprise Server 12 支持的内核版本

**版本** | **移动服务版本** | **内核版本** |
--- | --- | --- |
SUSE Linux Enterprise Server 12（SP1、SP2、SP3、SP4、SP5） | [9.36](https://support.microsoft.com/help/4578241/) | 支持所有[库存 SUSE 12 SP1、SP2、SP3、SP4 内核](https://www.suse.com/support/kb/doc/?id=000019587)。<br /><br /> 4.4.138-4.7-azure 到 4.4.180-4.31-azure、<br />4.12.14-6.3-azure 到 4.12.14-6.43-azure <br /> 4.12.14-16.7-azure 到 4.12.14-16.22-azure  |
SUSE Linux Enterprise Server 12（SP1、SP2、SP3、SP4、SP5） | [9.35](https://support.microsoft.com/help/4573888/) | 支持所有[库存 SUSE 12 SP1、SP2、SP3、SP4 内核](https://www.suse.com/support/kb/doc/?id=000019587)。<br /><br /> 4.4.138-4.7-azure 到 4.4.180-4.31-azure、<br />4.12.14-6.3-azure 到 4.12.14-6.43-azure <br /> 4.12.14-16.7-azure 到 4.12.14-16.19-azure  |
SUSE Linux Enterprise Server 12（SP1、SP2、SP3、SP4、SP5） | [9.34](https://support.microsoft.com/help/4570609) | 支持所有[库存 SUSE 12 SP1、SP2、SP3、SP4 内核](https://www.suse.com/support/kb/doc/?id=000019587)。<br /><br /> 4.4.138-4.7-azure 到 4.4.180-4.31-azure、<br />4.12.14-6.3-azure 到 4.12.14-6.43-azure <br /> 4.12.14-16.7-azure 到 4.12.14-16.13-azure  |
SUSE Linux Enterprise Server 12（SP1、SP2、SP3、SP4） | 9.32、[9.33](https://support.microsoft.com/help/4564347/) | 支持所有[库存 SUSE 12 SP1、SP2、SP3、SP4 内核](https://www.suse.com/support/kb/doc/?id=000019587)。<br /><br /> 4.4.138-4.7-azure 到 4.4.180-4.31-azure、<br />4.12.14-6.3-azure 到 4.12.14-6.34-azure  |

### <a name="suse-linux-enterprise-server-15-supported-kernel-versions"></a>SUSE Linux Enterprise Server 15 支持的内核版本

**版本** | **移动服务版本** | **内核版本** |
--- | --- | --- |
SUSE Linux Enterprise Server 15 和 15 SP1 | [9.36](https://support.microsoft.com/help/4578241/)  | 默认情况下，支持所有[库存 SUSE 15 和 15 SP1 内核](https://www.suse.com/support/kb/doc/?id=000019587)。<br /><br /> 4.12.14-5.5-azure 到 4.12.14-5.47-azure <br /><br /> 4.12.14-8.5-azure 到 4.12.14-8.38-azure
SUSE Linux Enterprise Server 15 和 15 SP1 | [9.35](https://support.microsoft.com/help/4573888/)  | 默认情况下，支持所有[库存 SUSE 15 和 15 SP1 内核](https://www.suse.com/support/kb/doc/?id=000019587)。<br /><br /> 4.12.14-5.5-azure 到 4.12.14-5.47-azure <br /><br /> 4.12.14-8.5-azure 到 4.12.14-8.33-azure 
SUSE Linux Enterprise Server 15 和 15 SP1 | [9.34](https://support.microsoft.com/help/4570609)  | 默认情况下，支持所有[库存 SUSE 15 和 15 SP1 内核](https://www.suse.com/support/kb/doc/?id=000019587)。<br /><br /> 4.12.14-5.5-azure 到 4.12.14-5.47-azure <br /><br /> 4.12.14-8.5-azure 到 4.12.14-8.30-azure 
SUSE Linux Enterprise Server 15 和 15 SP1 | [9.33](https://support.microsoft.com/help/4564347/) | 默认情况下，支持所有[库存 SUSE 15 和 15 SP1 内核](https://www.suse.com/support/kb/doc/?id=000019587)。<br /><br /> 4.12.14-5.5-azure 到 4.12.14-5.47-azure <br /><br /> 4.12.14-8.5-azure 到 4.12.14-8.30-azure |
SUSE Linux Enterprise Server 15 和 15 SP1 | [9.32](https://support.microsoft.com/help/4550047/) | 默认情况下，支持所有[库存 SUSE 15 和 15 SP1 内核](https://www.suse.com/support/kb/doc/?id=000019587)。 <br /><br /> 4.12.14-5.5-azure 到 4.12.14-8.22-azure

## <a name="linux-file-systemsguest-storage"></a>Linux 文件系统/来宾存储

**组件** | **支持**
--- | ---
文件系统 | ext3、ext4、XFS、BTRFS（适用条件如本表所述）
逻辑卷管理 (LVM) 预配| 复杂预配 - 是 <br /> 精简预配 - 否
卷管理器 | - 支持 LVM。<br/> - 从[更新汇总 31](https://support.microsoft.com/help/4478871/)（移动服务版本 9.20）开始支持 LVM 上的 /boot。 早期移动的服务版本不支持它。<br/> - 不支持多个 OS 磁盘。
半虚拟化存储设备 | 不支持半虚拟化驱动程序导出的设备。
多队列块 IO 设备 | 不支持。
具有 HP CCISS 存储控制器的物理服务器 | 不支持。
设备/装入点命名约定 | 设备名称或装入点名称应是唯一的。<br/> 请确保两个设备/装入点的名称不仅仅是只区分大小写。 例如，不支持将同一 VM 的两个设备命名为 *device1* 和 *Device1*。
目录 | 如果运行的移动服务版本低于 9.20（在[更新汇总 31](https://support.microsoft.com/help/4478871/) 中发布），则存在以下限制：<br/><br/> - 这些目录（如果设置为单独的分区/文件系统）必须位于源服务器上的同一 OS 磁盘：/ (root)、/boot、/usr、/usr/local、/var 和 /etc。<br /> - /boot 目录应位于磁盘分区上，而不是位于 LVM 卷上。<br/><br/> 从版本 9.20 开始，这些限制不适用。 
启动目录 | - 启动磁盘不能采用 GPT 分区格式。 这是一种 Azure 体系结构限制。 支持将 GPT 磁盘作为数据磁盘。<br/><br/> VM 上不支持多个启动磁盘<br/><br/> - 不支持跨多个磁盘的 LVM 卷上的 /boot。<br/> - 无法复制没有启动磁盘的计算机。
可用空间要求| /root 分区上的 2 GB <br/><br/> 安装文件夹中的 250 MB
XFSv5 | 支持 XFS 文件系统上的 XFSv5 功能，例如元数据校验和（从移动服务版本 9.10 开始）。<br/> 可使用 xfs_info 实用工具来检查分区的 XFS 超级块。 如果 `ftype` 设置为 1，则表示正在使用 XFSv5 功能。
BTRFS | 从[更新汇总 34](https://support.microsoft.com/help/4490016)（移动服务版本 9.22）开始支持 BTRFS。 如果存在以下情况，则不支持 BTRFS：<br/><br/> - 启用保护后 BTRFS 文件系统子卷发生更改。<br /> - BTRFS 文件系统分散在多个磁盘中。<br /> - BTRFS 文件系统支持 RAID。

## <a name="vmdisk-management"></a>VM/磁盘管理

**操作** | **详细信息**
--- | ---
调整复制的 VM 上的磁盘大小 | 在故障转移之前，可直接在源 VM 上的 VM 属性中进行调整。 无需禁用/重新启用复制。<br/><br/> 如果在故障转移后更改源 VM，则这些更改不会被捕获。<br/><br/> 如果在故障转移之后更改 Azure VM 上的磁盘大小，则在进行故障回复时，Site Recovery 将创建一个包含更新的新 VM。
在复制的 VM 上添加磁盘 | 不支持。<br/> 为 VM 禁用复制，添加磁盘，然后重新启用复制。

> [!NOTE]
> 不支持对磁盘标识进行任何更改。 例如，如果磁盘分区已从 GPT 更改为 MBR（或反过来），就会更改磁盘标识。 在这种情况下，复制将中断，并且需要全新的设置。 

## <a name="network"></a>网络

**组件** | **支持**
--- | ---
主机网络 NIC 组合 | 对于 VMware VM，受支持。 <br/><br/>对于物理计算机复制，不支持。
主机网络 VLAN | 是的。
主机网络 IPv4 | 是的。
主机网络 IPv6 | 否。
来宾/服务器网络 NIC 组合 | 否。
来宾/服务器网络 IPv4 | 是的。
来宾/服务器网络 IPv6 | 否。
来宾/服务器网络静态 IP (Windows) | 是的。
来宾/服务器网络静态 IP (Linux) | 是的。 <br/><br/>VM 配置为在故障回复时使用 DHCP。
来宾/服务器网络多个 NIC | 是的。

<!--Not Available on Private link access to Site Recovery service | Yes. [Learn more](hybrid-how-to-enable-replication-private-endpoints.md)-->

## <a name="azure-vm-network-after-failover"></a>Azure VM 网络（故障转移后）

**组件** | **支持**
--- | ---
Azure ExpressRoute | 是
ILB | 是
ELB | 是
Azure 流量管理器 | 是
多 NIC | 是
保留 IP 地址 | 是
IPv4 | 是
保留源 IP 地址 | 是
Azure 虚拟网络服务终结点<br/> | 是
加速网络 | 否

<a name="support-for-storage"></a>
## <a name="storage"></a>存储
**组件** | **支持**
--- | ---
动态磁盘 | OS 磁盘必须是基本磁盘。 <br/><br/>数据磁盘可以是动态磁盘
Docker 磁盘配置 | 否
主机 NFS | 在 VMware 上支持<br/><br/> 在物理服务器上不支持
主机 SAN (iSCSI/FC) | 是
主机 vSAN | 在 VMware 上支持<br/><br/> 在物理服务器上不适用
主机多路径 (MPIO) | 是，针对以下项进行了测试：Microsoft DSM、EMC PowerPath 5.7 SP4、EMC PowerPath DSM for CLARiiON
主机虚拟卷 (VVols) | 在 VMware 上支持<br/><br/> 在物理服务器上不适用
来宾/服务器 VMDK | 是
来宾/服务器共享群集磁盘 | 否
来宾/服务器加密磁盘 | 否
来宾/服务器 NFS | 否
来宾/服务器 iSCSI | 对于迁移 - 是<br/>对于灾难恢复 - 否，iSCSI 将作为附加磁盘故障回复到 VM
来宾/服务器 SMB 3.0 | 否
来宾/服务器 RDM | 是<br/><br/> 在物理服务器上不适用
> 1 TB 的来宾/服务器磁盘 | 是，磁盘必须大于 1024 MB<br/><br/>复制到托管磁盘时高达 8,192 GB（9.26 版及更高版本）<br /> 复制到存储帐户时高达 4,095 GB
逻辑和物理扇区大小均为 4K 的来宾/服务器磁盘 | 否
逻辑扇区大小为 4K 且物理扇区大小为 512 字节的来宾/服务器磁盘 | 否
包含 > 4 TB 的条带化磁盘的来宾/服务器卷 | 是
逻辑卷管理 (LVM)| 复杂预配 - 是 <br /> 精简预配 - 否
来宾/服务器 - 存储空间 | 否
来宾/服务器热添加/删除磁盘 | 否
来宾/服务器 - 排除磁盘 | 是
来宾/服务器多路径 (MPIO) | 否
来宾/服务器 GPT 分区 | 从[更新汇总 37](https://support.microsoft.com/help/4508614/)（移动服务版本 9.25）开始支持五个分区。 以前支持四个。
ReFS | 出行服务版本 9.23 或更高版本支持可复原文件系统
来宾/服务器 EFI/UEFI 启动 | - Site Recovery 移动代理版本 9.30 及更高版本支持所有 [Azure 市场 UEFI OS](../virtual-machines/windows/generation-2.md#generation-2-vm-images-in-azure-marketplace)。 <br/> - 不支持安全 UEFI 启动类型。 [了解详细信息。](../virtual-machines/windows/generation-2.md#on-premises-vs-azure-generation-2-vms)

## <a name="replication-channels"></a>复制通道

|**复制类型** |**支持**  |
|---------|---------|
|卸载的数据传输 (ODX)    |       否  |
|脱机设定种子        |   否      |
| Azure Data Box | 否

## <a name="azure-storage"></a>Azure 存储

**组件** | **支持**
--- | ---
本地冗余存储 | 是
异地冗余存储 | 是
读取访问异地冗余存储 | 是
冷存储 | 否
热存储| 否
块 Blob | 否
静态加密 (SSE)| 是
静态加密 (CMK)| 是（通过 PowerShell Az 3.3.0 及更高版本模块）
静态双重加密 | 是（通过 PowerShell Az 3.3.0 及更高版本模块）。 详细了解 [Windows](../virtual-machines/windows/disk-encryption.md) 和 [Linux](../virtual-machines/linux/disk-encryption.md) 支持的区域。
高级存储 | 是
安全传输选项 | 是
导入/导出服务 | 否
VNet 的 Azure 存储防火墙 | 是的。<br/> 在目标存储/缓存存储帐户上配置（用于存储复制的数据）。
常规用途 v2 存储帐户（热层和冷层） | 是（与 V1 相比，V2 的事务成本高得多）

## <a name="azure-compute"></a>Azure 计算

**功能** | **支持**
--- | ---
可用性集 | 是
HUB | 是
托管磁盘 | 是

<!--Not Available on Availability zones | No-->

<a name="failed-over-azure-vm-requirements"></a>
## <a name="azure-vm-requirements"></a>Azure VM 要求

复制到 Azure 的本地 VM 必须满足此表中汇总的 Azure VM 要求。 Site Recovery 运行复制先决条件检查时，如果不符合某些要求，检查将会失败。

**组件** | **要求** | **详细信息**
--- | --- | ---
来宾操作系统 | 验证复制的计算机[支持的操作系统](#replicated-machines)。 | 如果不支持，检查会失败。
来宾操作系统体系结构 | 64 位。 | 如果不支持，检查会失败。
操作系统磁盘大小 | 最大 2,048 GB。 | 如果不支持，检查会失败。
操作系统磁盘计数 | 1 | 如果不支持，检查会失败。
数据磁盘计数 | 64 或更少。 | 如果不支持，检查会失败。
数据磁盘大小 | 复制到托管磁盘时高达 8,192 GB（9.26 版及更高版本）<br />复制到存储帐户时高达 4,095 GB| 如果不支持，检查会失败。
网络适配器 | 支持多个适配器。 |
共享 VHD | 不支持。 | 如果不支持，检查会失败。
FC 磁盘 | 不支持。 | 如果不支持，检查会失败。
BitLocker | 不支持。 | 为计算机启用复制之前，必须先禁用 BitLocker。 |
VM 名称 | 1 到 63 个字符。<br/><br/> 限制为字母、数字和连字符。<br/><br/> 计算机名称必须以字母或数字开头和结尾。 |  请在 Site Recovery 中的计算机属性中更新该值。

## <a name="resource-group-limits"></a>资源组限制

若要了解可以在单个资源组下保护的虚拟机数量，请参阅有关[订阅限制和配额](../azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits)的文章。

## <a name="churn-limits"></a>变动率限制

下表提供了 Azure Site Recovery 限制。
- 这些限制基于我们的测试，但不涵盖所有可能的应用 I/O 组合。
- 实际结果可能因应用程序 I/O 组合而异。
- 为了获得最佳结果，我们强烈建议[运行部署规划器工具](site-recovery-deployment-planner.md)并通过使用测试性故障转移命令来执行广泛的应用程序测试，以获得应用的真实性能图。

**复制目标** | **平均源磁盘 I/O 大小** |**平均源磁盘数据变动量** | **每天的总源磁盘数据变动量**
---|---|---|---
标准存储 | 8 KB    | 2 MB/秒 | 每个磁盘 168 GB
高级 P10 或 P15 磁盘 | 8 KB    | 2 MB/秒 | 每个磁盘 168 GB
高级 P10 或 P15 磁盘 | 16 KB | 4 MB/秒 |    每个磁盘 336 GB
高级 P10 或 P15 磁盘 | 至少 32 KB | 8 MB/秒 | 每个磁盘 672 GB
高级 P20、P30、P40 或 P50 磁盘 | 8 KB    | 5 MB/秒 | 每个磁盘 421 GB
高级 P20、P30、P40 或 P50 磁盘 | 至少 16 KB |20 MB/秒 | 每个磁盘 1684 GB

**源数据变动量** | **最大限制**
---|---
VM 上所有磁盘的峰值数据变动量 | 54 MB/秒
进程服务器支持的每日最大数据变动量 | 2 TB

- 这是在假设存在 30% 的 I/O 重叠的情况下给出的平均数。
- Site Recovery 能够根据重叠率、较大的写入大小和实际工作负荷 I/O 行为处理更高的吞吐量。
- 这些数字假设通常情况下存在大约 5 分钟的积压工作。 也就是说，数据在上传后会在 5 分钟内进行处理并创建恢复点。

## <a name="vault-tasks"></a>保管库任务

**操作** | **支持**
--- | ---
跨资源组移动保管库 | 否
在订阅内部和订阅之间移动保管库 | 否
跨资源组移动存储、网络和 Azure VM | 否
在订阅内部和订阅之间移动存储、网络、Azure VM。 | 否

## <a name="obtain-latest-components"></a>获取最新组件

**名称** | **说明** | **详细信息**
--- | --- | ---
配置服务器 | 在本地安装。<br/> 协调本地 VMware 服务器或物理机与 Azure 之间的通信。 | - [了解](vmware-physical-azure-config-process-server-overview.md)配置服务器。<br/> - [了解](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server)如何升级到最新版本。<br/> - [了解](vmware-azure-deploy-configuration-server.md)如何设置配置服务器。
进程服务器 | 默认安装在配置服务器上。<br/> 接收复制数据，通过缓存、压缩和加密对其进行优化，然后将数据发送到 Azure。<br/> 随着部署扩大，可以另外添加进程服务器来处理更大的复制流量。 | - [了解](vmware-physical-azure-config-process-server-overview.md)进程服务器。<br/> - [了解](vmware-azure-manage-process-server.md#upgrade-a-process-server)如何升级到最新版本。<br/> - [了解](vmware-physical-large-deployment.md#set-up-a-process-server)如何设置横向扩展进程服务器。
移动服务 | 在想要复制的 VMware VM 或物理服务器上安装。<br/> 协调本地 VMware 服务器/物理服务器与 Azure 之间的复制。| - [了解](vmware-physical-mobility-service-overview.md)移动服务。<br/> - [了解](vmware-physical-manage-mobility-service.md#update-mobility-service-from-azure-portal)如何升级到最新版本。<br/>

## <a name="next-steps"></a>后续步骤
[了解如何](tutorial-prepare-azure.md)为 VMware VM 的灾难恢复准备 Azure。

[9.32 UR]: https://support.microsoft.com/en-in/help/4538187/update-rollup-44-for-azure-site-recovery
[9.31 UR]: https://support.microsoft.com/en-in/help/4537047/update-rollup-43-for-azure-site-recovery
[9.30 UR]: https://support.microsoft.com/en-in/help/4531426/update-rollup-42-for-azure-site-recovery
[9.29 UR]: https://support.microsoft.com/en-in/help/4528026/update-rollup-41-for-azure-site-recovery
[9.28 UR]: https://support.microsoft.com/en-in/help/4521530/update-rollup-40-for-azure-site-recovery
[9.27 UR]: https://support.microsoft.com/en-in/help/4517283/update-rollup-39-for-azure-site-recovery
[9.26 UR]: https://support.microsoft.com/en-in/help/4513507/update-rollup-38-for-azure-site-recovery
[9.25 UR]: https://support.microsoft.com/en-in/help/4508614/update-rollup-37-for-azure-site-recovery
[9.24 UR]: https://support.microsoft.com/en-in/help/4503156
[9.23 UR]: https://support.microsoft.com/en-in/help/4494485/update-rollup-35-for-azure-site-recovery
[9.22 UR]: https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery
[9.21 UR]: https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery
[9.20 UR]: https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery
[9.19 UR]: https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30
[9.18 UR]: https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery

<!-- Update_Description: update meta properties, wording update, update link -->