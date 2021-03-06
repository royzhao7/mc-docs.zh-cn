---
title: Azure Service Fabric 监视最佳做法
description: 使用 Azure Service Fabric 监视群集和应用程序的最佳做法和设计注意事项。
ms.topic: conceptual
origin.date: 01/23/2019
author: rockboyfor
ms.date: 09/14/2020
ms.testscope: yes|no
ms.testdate: 09/14/2020null
ms.author: v-yeche
ms.openlocfilehash: 74a5f167c966b5e7594ba03007c6e9aaa066a801
ms.sourcegitcommit: e1cd3a0b88d3ad962891cf90bac47fee04d5baf5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2020
ms.locfileid: "89655762"
---
<!--Verified successfully-->
# <a name="monitoring-and-diagnostic-best-practices-for-azure-service-fabric"></a>Azure Service Fabric 的监视和诊断最佳做法

在任何云环境中开发、测试和部署工作负荷时，[监视和诊断](./service-fabric-diagnostics-overview.md)至关重要。 例如，可以跟踪应用程序的使用方式、Service Fabric 平台所采取的操作、带性能计数器的资源利用率以及群集的总体运行状况。 可以使用此信息来诊断和更正问题，避免将来发生此类问题。

## <a name="application-monitoring"></a>应用程序监视

应用程序监视跟踪应用程序的功能和组件的使用方式。 监视应用程序可以确保捕获影响用户的问题。 应用程序监视是开发应用程序及其服务的人员的责任，因为它特定于应用程序的业务逻辑。 建议使用 [Application Insights](./service-fabric-tutorial-monitoring-aspnet.md)（Azure 的应用程序监视工具）来设置应用程序监视。

## <a name="cluster-monitoring"></a>群集监视

Service Fabric 的目标之一是使应用程序能够灵活应对硬件故障。 为了实现此目的，可以通过平台的系统服务功能检测基础结构问题，并快速将工作负荷故障转移到群集中的其他节点。 但是，系统服务本身出现问题该怎么办？ 或者，在尝试部署或移动工作负荷时，如果违反服务位置的规则该怎么办？ Service Fabric 为这些问题以及其他问题提供诊断，确保你了解 Service Fabric 平台与应用程序、服务、容器和节点的交互情况。

对于 Windows 群集，建议使用[诊断代理](./service-fabric-diagnostics-event-aggregation-wad.md)和 [Azure Monitor 日志](./service-fabric-diagnostics-oms-setup.md)来设置群集监视。

对于 Linux 群集，Azure Monitor 日志也是建议用于 Azure 平台和基础结构监视的工具。 Linux 平台诊断要求不同的配置，详见 [Syslog 中的 Service Fabric Linux 群集事件](./service-fabric-diagnostics-oms-syslog.md)。

## <a name="infrastructure-monitoring"></a>基础结构监视

若要监视群集级别的事件，建议使用 [Azure Monitor 日志](./service-fabric-diagnostics-oms-agent.md)。 使用上一链接中介绍的工作区配置 Log Analytics 代理以后，即可收集性能指标，例如：CPU 使用率、.NET 性能计数器（例如进程级别的 CPU 使用率）、Service Fabric 性能计数器（例如来自 Reliable Service 的异常数），以及容器指标（例如 CPU 使用率）。  需将容器日志写入 stdout 或 stderr，使之在 Azure Monitor 日志中可用。

## <a name="watchdogs"></a>监视器

监视器通常是一个独立的服务，用于监视各个服务的运行状况和负载、ping 终结点，以及报告群集中的异常运行状况事件。 这有助于防止出现仅基于单个服务的性能所检测不到的错误。 监视器也是一个托管代码的好选择，无需用户交互即可执行补救措施，例如每隔特定时间就清理一次存储中的日志文件。 若要查看示例监视器服务实现，请参阅 [Syslog 中的 Service Fabric Linux 群集事件](https://github.com/Azure-Samples/service-fabric-watchdog-service)。

## <a name="next-steps"></a>后续步骤

* 开始检测应用程序：[应用程序级别事件和日志生成](service-fabric-diagnostics-event-generation-app.md)。
* 通过[在 Service Fabric 上监视和诊断 ASP.NET Core 应用程序](service-fabric-tutorial-monitoring-aspnet.md)，完成为应用程序设置 Application Insights 的步骤。
* 详细了解如何监视平台以及 Service Fabric 提供的事件：[平台级别事件和日志生成](service-fabric-diagnostics-event-generation-infra.md)。
* 配置 Azure Monitor 日志与 Service Fabric 的集成：[为群集设置 Azure Monitor 日志](service-fabric-diagnostics-oms-setup.md)
    
    <!--Not Available on * Learn how to set up Azure Monitor logs for monitoring containers: [Monitoring and Diagnostics for Windows Containers in Azure Service Fabric](service-fabric-tutorial-monitoring-wincontainers.md)-->
    
* 查看 Service Fabric 的示例诊断问题和解决方案：[诊断常见方案](service-fabric-diagnostics-common-scenarios.md)

<!--Not Available on [Best Practices - Monitoring and diagnostics](https://docs.microsoft.com/azure/architecture/best-practices/monitoring)-->

<!-- Update_Description: new article about service fabric best practices monitoring -->
<!--NEW.date: 09/14/2020-->