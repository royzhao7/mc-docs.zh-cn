---
title: 高级威胁防护
titleSuffix: Azure SQL Database, SQL Managed Instance, & Azure Synapse Analytics
description: 高级威胁防护检测到异常的数据库活动，这些异常指示 Azure SQL 数据库、Azure SQL 托管实例和 Azure Synapse Analytics 中有潜在的安全威胁。
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.custom: sqldbrb=2
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: vanto, carlrab
origin.date: 02/05/2020
ms.date: 07/13/2020
tags: azure-synapse
ms.openlocfilehash: aff4143b05ea265b9a585cc91f75be7cf5ef651a
ms.sourcegitcommit: fa26665aab1899e35ef7b93ddc3e1631c009dd04
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86227684"
---
# <a name="advanced-threat-protection-for-azure-sql-database-sql-managed-instance-and-azure-synapse-analytics"></a>适用于 Azure SQL 数据库、SQL 托管实例和 Azure Synapse Analytics 的高级威胁防护
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

适用于 [Azure SQL 数据库](sql-database-paas-overview.md)、[Azure SQL 托管实例](../managed-instance/sql-managed-instance-paas-overview.md)和 [Azure Synapse Analytics](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) 的高级威胁防护可检测异常活动，这些活动指示访问或利用数据库的异常和潜在有害尝试。

高级威胁防护包含在[高级数据安全](advanced-data-security.md)产品/服务中，这是用于高级 SQL 安全功能的统一软件包。 可通过中心 SQL ADS 门户访问和管理高级威胁防护。

## <a name="overview"></a>概述

高级威胁防护提供新的安全层，在发生异常活动时会提供安全警报，让客户检测潜在威胁并做出响应。 出现可疑数据库活动、潜在漏洞、SQL 注入攻击和异常数据库访问和查询模式时，用户将收到警报。 高级威胁防护将警报与 [Azure 安全中心](/security-center/)集成，其中包含可疑活动的详细信息以及有关如何调查和缓解威胁的建议操作。 不必是安全专家，也不需要管理先进的安全监视系统，就能使用高级威胁防护轻松解决数据库的潜在威胁。

为了提供完整的调查体验，建议启用审核，它会将数据库事件写入到 Azure 存储帐户中的审核日志。  若要启用审核，请参阅 [Azure SQL 数据库和 Azure Synapse 的审核](../../azure-sql/database/auditing-overview.md)或 [Azure SQL 托管实例的审核](../managed-instance/auditing-configure.md)。

## <a name="alerts"></a>警报

Azure SQL 数据库的高级威胁防护可检测异常活动，指出有人在访问或利用数据库时的异常行为和可能有害的尝试。 有关 Azure SQL 数据库的警报列表，请参阅 [Azure 安全中心内关于 SQL 数据库和 SQL 数据仓库的警报](/security-center/alerts-reference#alerts-sql-db-and-warehouse)。

## <a name="explore-detection-of-a-suspicious-event"></a>浏览检测到的可疑事件

检测到异常数据库活动时，将收到电子邮件通知。 电子邮件将提供可疑安全事件的相关信息，包括异常活动的性质、数据库名称、服务器名称、应用程序名称和事件时间。 此外，电子邮件还会提供可能原因和建议操作的相关信息，帮助调查和缓解数据库的潜在威胁。

![异常活动报告](./media/threat-detection-overview/anomalous_activity_report.png)

1. 单击电子邮件中“查看最近的 SQL 警报”链接以启动 Azure 门户并显示“Azure 安全中心警报”页，该页面提供在数据库上检测到的活动威胁的概述。

   ![活动威胁](./media/threat-detection-overview/active_threats.png)

1. 单击特定警报可获得其他详细信息以及用于调查此威胁和解决潜在威胁的操作。

   例如，SQL 注入是 Internet 上最常见的 Web 应用程序安全问题之一，用于攻击数据驱动的应用程序。 攻击者利用应用程序漏洞将恶意 SQL 语句注入应用程序入口字段，以破坏或修改数据库中的数据。 对于 SQL 注入警报，警报的详细信息包括被利用的有漏洞的 SQL 语句。

   ![特定警报](./media/threat-detection-overview/specific_alert.png)

## <a name="explore-alerts-in-the-azure-portal"></a>在 Azure 门户中浏览警报

高级威胁防护将其警报与 [Azure 安全中心](/security-center/)集成。 Azure 门户中数据库和 SQL ADS 边栏选项卡内的实时 SQL 高级威胁防护磁贴会跟踪活动威胁的状态。

单击“高级威胁防护警报”以启动“Azure 安全中心警报”页，并获取在数据库中检测到的活动 SQL 威胁的概述。

   ![高级威胁防护警报](./media/threat-detection-overview/threat_detection_alert.png)

   ![高级威胁防护警报 2](./media/threat-detection-overview/threat_detection_alert_atp.png)

## <a name="next-steps"></a>后续步骤

- 详细了解 [Azure SQL 数据库和 Azure Synapse 中的高级威胁防护](threat-detection-configure.md)。
- 详细了解 [Azure SQL 托管实例中的高级威胁防护](../managed-instance/threat-detection-configure.md)。
- 详细了解[高级数据安全性](advanced-data-security.md)。
- 详细了解 [Azure SQL 数据库审核](../../azure-sql/database/auditing-overview.md)
- 详细了解 [Azure 安全中心](/security-center/security-center-intro)
- 有关定价的详细信息，请参阅 [Azure SQL 数据库定价页](https://azure.cn/pricing/details/sql-database/)  
