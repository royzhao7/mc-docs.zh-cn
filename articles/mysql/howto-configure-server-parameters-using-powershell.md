---
title: 配置服务器参数 - Azure PowerShell - Azure Database for MySQL
description: 本文介绍如何使用 PowerShell 在 Azure Database for MySQL 中配置服务参数。
author: WenJason
ms.author: v-jay
ms.service: mysql
ms.devlang: azurepowershell
ms.topic: conceptual
origin.date: 4/29/2020
ms.date: 06/26/2020
ms.openlocfilehash: 827806e030862d3335efc9d42e7e2722cdb239d3
ms.sourcegitcommit: 3a8a7d65d0791cdb6695fe6c2222a1971a19f745
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2020
ms.locfileid: "85516728"
---
# <a name="configure-server-parameters-in-azure-database-for-mysql-using-powershell"></a>使用 PowerShell 在 Azure Database for MySQL 中配置服务器参数

可以使用 PowerShell 列出、显示和更新 Azure Database for MySQL 服务器的配置参数。 在服务器级别会公开引擎配置的一个子集，并可以进行修改。

## <a name="prerequisites"></a>先决条件

若要完成本操作指南，需要：

- 在本地安装了 [Az PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)。
- [Azure Database for MySQL 服务器](quickstart-create-mysql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> 尽管 Az.MySql PowerShell 模块为预览版，但必须使用以下命令从 Az PowerShell 模块单独安装它：`Install-Module -Name Az.MySql -AllowPrerelease`。
> Az.MySql PowerShell 模块正式版推出后，它会包含在将来的 Az PowerShell 模块发行版中。

使用 [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/Connect-AzAccount) cmdlet 连接到 Azure 帐户。

## <a name="list-server-configuration-parameters-for-azure-database-for-mysql-server"></a>列出 Azure Database for MySQL 服务器的服务器配置参数

若要列出服务器中的所有可修改参数及其值，请运行 `Get-AzMySqlConfiguration` cmdlet。

以下示例列出了资源组 myresourcegroup 中服务器 mydemoserver 的服务器配置参数。

```azurepowershell
Get-AzMySqlConfiguration -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

有关每个列出参数的定义，请参阅[服务器系统变量](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html)上的 MySQL 引用部分。

## <a name="show-server-configuration-parameter-details"></a>显示服务器配置参数详细信息

若要显示有关服务器的特定配置参数的详细信息，请运行 `Get-AzMySqlConfiguration` cmdlet 并指定 Name 参数。

本示例显示了资源组“myresourcegroup”下服务器 mydemoserver 的服务器配置参数 slow\_query\_log 的详细信息。

```azurepowershell
Get-AzMySqlConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

## <a name="modify-a-server-configuration-parameter-value"></a>修改服务器配置参数值

此外，你还可以修改某个服务器配置参数的值，这会更新 MySQL 服务器引擎的基础配置值。 若要更新配置，请使用 `Update-AzMySqlConfiguration` cmdlet。

更新资源组“myresourcegroup”下服务器 mydemoserver 的服务器配置参数 slow\_query\_log。

```azurepowershell
Update-AzMySqlConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver -Value On
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 PowerShell 实现 Azure Database for MySQL 服务器存储的自动增长](howto-auto-grow-storage-powershell.md)。
