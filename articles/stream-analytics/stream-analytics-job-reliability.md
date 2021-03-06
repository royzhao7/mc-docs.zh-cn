---
title: 使用 Azure 流分析作业避免服务中断
description: 本文介绍有关生成流分析作业升级复原力的指南。
author: Johnnytechn
ms.author: v-johya
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
origin.date: 06/21/2019
ms.date: 08/20/2020
ms.custom: seodec18
ms.openlocfilehash: 1dac253300209c70d66c4627e45b0470bc686b38
ms.sourcegitcommit: 09c7071f4d0d9256b40a6bf700b38c6a25db1b26
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/21/2020
ms.locfileid: "88715771"
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a>在服务更新期间保证流分析作业可靠性

作为完全托管服务的一部分的是快速引入新服务功能和改进的能力。 因此，流分析可以每周（或更频繁地）进行服务更新部署。 无论进行多少次测试，由于引入了 bug，仍存在现有正在运行的作业可能会中断的风险。 如果运行的是任务关键型作业，则需要避免这些风险。 可以遵循 Azure 的[配对区域](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)  模型来降低此风险。 

## <a name="how-do-azure-paired-regions-address-this-concern"></a>Azure 配对区域如何解决此问题？

流分析可以保证在单独的批处理中更新配对区域中的作业。 因此，在更新之间具有足够的时间间隔来识别潜在问题并修复它们。

**同一组中**多个区域中的部署可能会**同时**进行。
<!-- Notice: Remove the India exception -->
**[可用性和配对区域](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** 一文具有关于配对区域的最新信息。

建议将相同的作业部署到这两个配对区域。 然后，应该[监视这些作业](/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor)，以便在发生意外情况时收到通知。 如果其中一个作业在流分析服务更新后以[失败状态](/stream-analytics/job-states)结束，则可以联系客户支持以帮助确定根本原因。 还应将任何下游使用者故障转移到正常作业输出。

## <a name="next-steps"></a>后续步骤

* [流分析简介](stream-analytics-introduction.md)
* [流分析入门](stream-analytics-real-time-fraud-detection.md)
* [缩放流分析作业](stream-analytics-scale-jobs.md)
* [流分析查询语言参考](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [流分析管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn835031.aspx)

