---
title: “引入”概述 - Azure 时序见解第 2 代 | Microsoft Docs
description: 了解如何将数据引入到 Azure 时序见解第 2 代中。
author: lyrana
ms.author: v-junlch
manager: deepakpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 08/04/2020
ms.custom: seodec18
ms.openlocfilehash: 777c31e076cf2f2838d7552355e2f384cb5e15a5
ms.sourcegitcommit: 36e7f37481969f92138bfe70192b1f4a2414caf7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/05/2020
ms.locfileid: "87801826"
---
# <a name="azure-time-series-insights-gen2-data-ingestion-overview"></a>Azure 时序见解 Gen2 数据引入概述

Azure 时序见解第 2 代环境包含一个用于收集、处理和存储流时序数据的引入引擎。 当数据到达事件源时，Azure 时序见解第 2 代将以准实时的方式使用和存储数据。

[![引入概述](./media/concepts-ingress-overview/ingress-overview.png)](./media/concepts-ingress-overview/ingress-overview.png#lightbox)

## <a name="ingestion-topics"></a>“引入”主题

以下文章深入介绍了数据处理，包括要遵循的最佳做法：

* 阅读有关[事件源](./concepts-streaming-ingestion-event-sources.md)的信息以及有关选择事件源时间戳的指南。

* 查看受支持的[数据类型](./concepts-supported-data-types.md)

* 了解“引入”引擎如何将一组[规则](./concepts-json-flattening-escaping-rules.md)应用于 JSON 属性，以创建存储帐户列。

* 查看环境[吞吐量限制](./concepts-streaming-ingress-throughput-limits.md)，以便针对缩放需求进行规划。

## <a name="next-steps"></a>后续步骤

* 继续详细了解 Azure 时序见解第 2 代环境的[事件源](./concepts-streaming-ingestion-event-sources.md)。 

