---
title: 使用备份资源管理器监视备份
description: 本文介绍如何使用备份资源管理器跨保管库、订阅、区域和租户对备份进行实时监视。
ms.reviewer: dcurwin
ms.topic: conceptual
author: Johnnytechn
ms.date: 06/09/2020
ms.author: v-johya
ms.openlocfilehash: cdd17a9d25af39d7d462489fda5e5ba3b76eb9fc
ms.sourcegitcommit: 285649db9b21169f3136729c041e4d04d323229a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/11/2020
ms.locfileid: "84683999"
---
# <a name="monitor-your-backups-with-backup-explorer"></a>使用备份资源管理器监视备份

随着组织在云中备份的计算机不断增多，有效监视这些备份就变得越发重要。 开始进行这种监视的最佳方式是在一个中心位置查看整个大型资产的运行信息。

备份资源管理器是一个内置的 Azure Monitor 工作簿，可为 Azure 备份客户提供这种单一中心位置。 备份资源管理器可帮助你跨租户、位置、订阅、资源组和保管库监视 Azure 中的整个“备份”资产的运行活动。 从广义上讲，备份资源管理器提供以下功能：

* **大规模透视图**：获取整个资产中备份项、作业、警报、策略以及尚未配置为要进行备份的资源的聚合视图。
* **下钻式分析**：在一个位置显示有关每个作业、警报、策略和备份项的详细信息。
* **可操作的接口**：确定问题后，可以无缝转到相关备份项或 Azure 资源来解决该问题。

与 Azure Resource Graph 和 Azure Monitor 工作簿的原生集成直接提供了这些功能。

> [!NOTE]
>
> * 备份资源管理器目前仅适用于 Azure 虚拟机 (VM) 数据。
> * 备份资源管理器充当一个操作仪表板，可用于查看有关备份在过去 7 天（最长时间）的信息。
> * 国家/地区云目前不支持备份资源管理器。
> * 目前不支持自定义备份资源管理器模板。
> * 我们不建议针对 Azure Resource Graph 数据编写自定义的自动化程序。

## <a name="get-started"></a>入门

可以通过转到任一恢复服务保管库，并在“概述”窗格中选择“备份资源管理器”链接，来访问备份资源管理器。 

![保管库快速链接](./media/backup-azure-monitor-with-backup-explorer/vault-quick-link.png)

选择此链接会打开备份资源管理器，其中提供了你有权访问的所有保管库和订阅的聚合视图。 如果你使用 Azure Lighthouse 帐户，则可以查看你有权访问的所有租户的数据。 有关详细信息，请参阅本文末尾的“跨租户视图”部分。

![备份资源管理器登陆页](./media/backup-azure-monitor-with-backup-explorer/explorer-landing-page.png)

## <a name="backup-explorer-use-cases"></a>备份资源管理器用例

备份资源管理器显示多个选项卡，每个选项卡提供有关特定备份项目（例如备份项、作业或策略）的详细信息。 本部分提供其中每个选项卡的简要概述。

### <a name="the-summary-tab"></a>“摘要”选项卡

“摘要”选项卡提供备份资产整体状况的概览。 例如，可以查看受保护的项数、尚未启用保护的项数，或者过去 24 小时成功的作业数。

### <a name="the-backup-items-tab"></a>“备份项”选项卡

可按订阅、保管库及其他特征筛选和查看每个备份项。 选择备份项的名称可以打开该项的“Azure”窗格。 例如，在该表格中，你可能已注意到项“X”的上次备份失败。选择“X”可以打开该项的“备份”窗格，可在其中触发按需备份操作。

### <a name="the-jobs-tab"></a>“作业”选项卡

选择“作业”选项卡可以查看过去 7 天触发的所有作业的详细信息。 在此处，可按“作业操作”、“作业状态”和“错误代码”（对于失败的作业）进行筛选。  

### <a name="the-alerts-tab"></a>“警报”选项卡

选择“警报”选项卡可以查看过去 7 天针对保管库生成的所有警报的详细信息。 可按类型（“备份失败”或“还原失败”）、当前状态（“活动”或“已解决”）、严重性（“严重”、“警告”或“信息”）筛选警报。       还可以选择相应的链接转到 Azure VM 并采取任何必要措施。

### <a name="the-policies-tab"></a>“策略”选项卡

可以选择“策略”选项卡查看有关在整个备份资产中创建的所有备份策略的重要信息。 可以查看与每个策略关联的项数，以及策略指定的保留期范围和备份频率。

### <a name="the-backup-not-enabled-tab"></a>“未启用备份”选项卡

应为需要保护的所有计算机启用备份。 借助备份资源管理器，备份管理员可以快速识别组织中的哪些计算机尚未通过备份进行保护。 若要查看此信息，请选择“未启用备份”选项卡。

“未启用备份”窗格中会显示一个表格，其中列出了未受保护的计算机。 组织可将不同的标记分配到生产计算机和测试计算机，或者分配到提供多种功能的计算机。 由于每类计算机需要不同的备份策略，因此按标记进行筛选有助于查看特定于每台计算机的信息。 选择任一计算机的名称会重定向到该计算机的“配置备份”窗格，在其中可以选择应用适当的备份策略。

## <a name="export-to-excel"></a>导出到 Excel

可将任何表或图表的内容导出为 Excel 电子表格。 内容将按原样导出，并应用现有的筛选器。 若要导出更多表行，可以使用每个选项卡顶部的“每页行数”下拉列表，增加要在页面上显示的行数。

## <a name="pin-to-the-dashboard"></a>固定到仪表板

可以选择每个表或图表顶部的“固定”图标，将该表或图表固定到 Azure 门户仪表板。 固定此信息有助于创建一个定制的仪表板来显示对你而言最重要的信息。

<!-- Not available in Azure Lighthouse -->
## <a name="next-steps"></a>后续步骤

[了解如何使用 Azure Monitor 获取有关备份数据的见解](/backup/backup-azure-monitoring-use-azuremonitor)

