---
title: 使用 Application Insights 监视 Azure Kubernetes 服务 (AKS) 上的应用程序 - Azure Monitor | Microsoft Docs
description: Azure Monitor 与 Kubernetes 上运行的应用程序无缝集成，可用来立即查明应用中的问题。
ms.topic: conceptual
author: Johnnytechn
ms.author: v-johya
ms.date: 07/15/2020
ms.openlocfilehash: e8596d4e77f195b2a96e99bc8ba881d073ec4fff
ms.sourcegitcommit: 403db9004b6e9390f7fd1afddd9e164e5d9cce6a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/17/2020
ms.locfileid: "86440601"
---
# <a name="zero-instrumentation-application-monitoring-for-kubernetes---azure-monitor-application-insights"></a>针对 Kubernetes 的零检测应用程序监视 - Azure Monitor Application Insights

> [!IMPORTANT]
>  目前，无需对代码进行检测，即可为 Kubernetes 上运行的 Java 应用启用监视 - 为此，可以使用 [Java 独立代理](/azure-monitor/app/java-in-process-agent)。 针对其他语言的用于无缝实现应用程序监视的解决方案正在开发中，暂时可以使用 SDK 来监视 AKS 上运行的应用：[ASP.NET Core](/azure-monitor/app/asp-net-core)、[ASP.NET](/azure-monitor/app/asp-net)、[Node.js](/azure-monitor/app/nodejs)、[JavaScript](/azure-monitor/app/javascript) 和 [Python](/azure-monitor/app/opencensus-python)。

## <a name="application-monitoring-without-instrumenting-the-code"></a>无需对代码进行检测即可监视应用程序
目前，只有 Java 允许在不检测代码的情况下启用应用程序监视。 若要监视以其他语言编写的应用程序，请使用 SDK。 

## <a name="java"></a>Java
启用后，Java 代理将自动从最广泛使用的库和框架收集大量请求、依赖项、日志和指标。

请按照[详细说明](/azure-monitor/app/java-in-process-agent)监视 Kubernetes 应用及其他环境中运行的 Java 应用。 

## <a name="other-languages"></a>其他语言

对于以其他语言编写的应用程序，我们目前建议使用 SDK：
* [ASP.NET Core](/azure-monitor/app/asp-net-core)
* [ASP.NET](/azure-monitor/app/asp-net)
* [Node.js](/azure-monitor/app/nodejs) 
* [JavaScript](/azure-monitor/app/javascript)
* [Python](/azure-monitor/app/opencensus-python)

## <a name="next-steps"></a>后续步骤

* 详细了解 [Azure Monitor](/azure-monitor/overview) 和 [Application Insights](/azure-monitor/app/app-insights-overview)
* 查看[分布式跟踪](/azure-monitor/app/distributed-tracing)的概述，并了解[应用程序映射](/azure-monitor/app/app-map?tabs=net)能够对业务起到的作用

