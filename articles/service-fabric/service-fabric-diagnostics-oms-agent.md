---
title: 使用 Azure Monitor 日志进行性能监视
description: 了解如何设置 Log Analytics 代理以监视 Azure Service Fabric 群集的容器和性能计数器。
ms.topic: conceptual
origin.date: 04/16/2018
author: rockboyfor
ms.date: 09/14/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: bd021ea60134c69dd2cc09cc938bf8719c26e0af
ms.sourcegitcommit: e1cd3a0b88d3ad962891cf90bac47fee04d5baf5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2020
ms.locfileid: "89655732"
---
<!--VM extension exists on Azure China Cloud-->
# <a name="performance-monitoring-with-azure-monitor-logs"></a>使用 Azure Monitor 日志进行性能监视

本文介绍如何逐步将 Log Analytics 代理作为虚拟机规模集扩展添加到群集并将其连接到现有的 Azure Log Analytics 工作区。 这可收集关于容器、应用程序和性能监视的诊断数据。 通过将其作为扩展添加到虚拟机规模集资源，Azure 资源管理器可确保它安装在每个节点上，即使在缩放群集时也是如此。

> [!NOTE]
> 本文假定已设置了 Azure Log Analytics 工作区。 如果尚未设置，请转到[设置 Azure Monitor 日志](service-fabric-diagnostics-oms-setup.md)

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="add-the-agent-extension-via-azure-cli"></a>通过 Azure CLI 添加代理扩展

将 Log Analytics 代理添加到群集的最佳方法是使用 Azure CLI 提供的虚拟机规模集 API。 如果尚未安装 Azure CLI，请[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

<!--Not Available on [Cloud Shell](../cloud-shell/overview.md)-->

1. 请求本地 Shell 之后，请确保在资源所在的订阅中工作。 请使用 `az account show` 进行检查，并确保“名称”值与群集订阅的值匹配。

2. 在门户中，导航到 Log Analytics 工作区所在的资源组。 单击进入日志分析资源（资源类型为 Log Analytics 工作区）。 进入资源概述页后，单击左侧菜单中“设置”部分下面的“高级设置”。 

    :::image type="content" source="media/service-fabric-diagnostics-oms-agent/oms-advanced-settings.png" alt-text="日志分析属性页":::

3. 若要建立 Windows 群集，请单击“Windows 服务器”；若要创建 Linux 群集，请单击“Linux 服务器”   。 此页将显示 `workspace ID` 和 `workspace key`（在门户中列为“主键”）。 下一步骤需要使用这两个值。

4. 运行以下命令使用 `vmss extension set` API 将 Log Analytics 代理安装到群集中：

    对于 Windows 群集：

    ```azurecli
    az vmss extension set --name MicrosoftMonitoringAgent --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<Log AnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<Log AnalyticsworkspaceKey>'}"
    ```

    对于 Linux 群集：

    ```azurecli
    az vmss extension set --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<Log AnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<Log AnalyticsworkspaceKey>'}"
    ```

    以下示例展示如何将 Log Analytics 代理添加到 Windows 群集。

    :::image type="content" source="media/service-fabric-diagnostics-oms-agent/cli-command.png" alt-text="Log Analytics 代理 cli 命令":::

5. 15 分钟内即可将代理成功添加到节点上。 可使用 `az vmss extension list` API 验证是否已添加代理：

    ```azurecli
    az vmss extension list --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType>
    ```

## <a name="add-the-agent-via-the-resource-manager-template"></a>通过资源管理器模板添加代理

部署 Azure Log Analytics 工作区并将代理添加到每个节点的示例资源管理器模板可用于 [Windows](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-OMS-UnSecure) 或 [Linux](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeType-Secure-OMS)。

可下载和修改此模板以部署最适合需求的群集。

## <a name="view-performance-counters"></a>查看性能计数器

添加 Log Analytics 代理后，请转到 Log Analytics 门户，选择要收集的性能计数器。

1. 在 Azure 门户中，转到在其中创建 Service Fabric 分析解决方案的资源组。 选择 ServiceFabric\<nameOfLog AnalyticsWorkspace\>。

2. 单击“Log Analytics”  。

3. 单击“高级设置”  。

4. 单击“数据”  ，然后单击“Windows 或 Linux 性能计数器”  。 此时会显示一个可以选择的默认计数器列表，此外还可以设置收集间隔。 还可以添加要收集的[其他性能计数器](service-fabric-diagnostics-event-generation-perf.md)。 此[参考文章](https://docs.microsoft.com/windows/win32/perfctrs/specifying-a-counter-path)中介绍了正确的格式。

5. 单击“保存”，然后单击“确定”   。

6. 关闭“高级设置”边栏选项卡。

7. 在“常规”标题下，单击“工作区摘要”  。

8. 将看到每个已启用的解决方案的图形形式的磁贴，包括 Service Fabric 的磁贴。 单击 **Service Fabric** 图形以转到 Service Fabric 分析解决方案。

9. 将会看到一些带有有关操作通道和 Reliable Services 事件的图形的磁贴。 已选择的计数器的流入数据的图形表示形式将会显示在“节点指标”下。

10. 单击“容器指标”图形可了解更多详细信息。 还可以使用 Kusto 查询语言，像查询群集事件一样查询性能计数器数据，以及基于节点、性能计数器名称和值进行筛选。

![Log Analytics 性能计数器查询](media/service-fabric-diagnostics-event-analysis-oms/oms_node_metrics_table.PNG)

## <a name="next-steps"></a>后续步骤

* 收集相关[性能计数器](service-fabric-diagnostics-event-generation-perf.md)。 若要配置 Log Analytics 代理以收集特定性能计数器，请查看[配置数据源](../azure-monitor/platform/agent-data-sources.md#configuring-data-sources)。
* 配置 Azure Monitor 日志，以便设置有助于检测和诊断的[自动警报](../azure-monitor/platform/alerts-overview.md)
* 作为替代方法，可以通过 [Azure 诊断扩展收集性能计数器并将其发送到 Application Insights](service-fabric-diagnostics-event-aggregation-wad.md#add-the-application-insights-sink-to-the-resource-manager-template)

<!-- Update_Description: update meta properties, wording update, update link -->