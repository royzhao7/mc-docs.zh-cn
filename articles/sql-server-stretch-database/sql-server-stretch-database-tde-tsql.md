---
title: 为 Stretch Database 启用透明数据加密 (T-SQL)
description: 为 Azure TSQL 上的 SQL Server Stretch Database 启用透明数据加密 (TDE)
services: sql-server-stretch-database
documentationcenter: ''
ms.assetid: 27753d91-9ca2-4d47-b34d-b5e2c2f029bb
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.topic: article
origin.date: 01/23/2017
ms.date: 01/19/2020
author: rockboyfor
ms.author: v-yeche
ms.reviewer: jroth
manager: digimobile
ms.custom: seo-lt-2019
ms.openlocfilehash: e086408da425ea2d530fe9a292f47835989d1a8e
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "76270023"
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>为 Azure 上的 Stretch Database 启用透明数据加密 (TDE) (Transact-SQL)
> [!div class="op_single_selector"]
> * [Azure 门户](sql-server-stretch-database-encryption-tde.md)
> * [TSQL](sql-server-stretch-database-tde-tsql.md)
>
>

透明数据加密 (TDE) 无需更改应用程序，即可对静止的数据库、关联的备份和事务日志执行实时加密和解密，帮助防止恶意活动的威胁。

TDE 使用称为数据库加密密钥的对称密钥来加密整个数据库的存储。 数据库加密密钥由内置服务器证书保护。 内置服务器证书对每个 Azure 服务器都是唯一的。 Azure 至少每隔 90 天自动轮换这些证书。 有关 TDE 的一般说明，请参阅[透明数据加密 (TDE)]。

## <a name="enabling-encryption"></a>启用加密

对于存储从启用延伸的 SQL Server 数据库迁移的数据的 Azure 数据库，若要启用 TDE，请执行以下操作：

1. 使用在 master 数据库中是管理员或 **dbmanager** 角色成员的登录名，连接到托管数据库的 Azure 服务器上的 *master* 数据库
2. 执行以下语句来加密数据库。

    ```sql
    ALTER DATABASE [database_name] SET ENCRYPTION ON;
    ```

## <a name="disabling-encryption"></a>禁用加密

对于存储从启用延伸的 SQL Server 数据库迁移的数据的 Azure 数据库，若要禁用 TDE，请执行以下操作：

1. 使用在 master 数据库中充当管理员或 **dbmanager** 角色成员的登录名，连接到 *master* 数据库
2. 执行以下语句来加密数据库。

    ```sql
    ALTER DATABASE [database_name] SET ENCRYPTION OFF;
    ```

## <a name="verifying-encryption"></a>验证加密

若要验证存储从启用延伸的 SQL Server 数据库迁移的数据的 Azure 数据库的加密状态，请执行以下操作：

1. 使用在 master 数据库中充当管理员或 **dbmanager** 角色成员的登录名，连接到 *master* 数据库或实例数据库
2. 执行以下语句来加密数据库。

    ```sql
    SELECT
        [name],
        [is_encrypted]
    FROM
        sys.databases;
    ```

结果 ```1``` 表示数据库已加密，```0``` 表示数据库未加密。

<!--Anchors-->

[透明数据加密 (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx

<!--Image references-->

<!--Link references-->

<!-- Update_Description: update meta properties, wording update, update link -->