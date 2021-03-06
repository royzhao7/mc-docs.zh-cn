---
title: DNS 别名（PowerShell 和 Azure CLI）
description: PowerShell 和 Azure CLI cmdlet 可以将新客户端连接重定向到不同的 Azure SQL 服务器，而无需触摸任何客户端配置。
keywords: dns sql database
ms.custom: seo-lt-2019 sqldbrb=1
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.devlang: PowerShell
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: genemi, amagarwa, maboja, jrasnick, vanto
origin.date: 05/14/2019
ms.date: 07/13/2020
ms.openlocfilehash: b3f3e684016232ee1326b4614186754f3e4289b0
ms.sourcegitcommit: fa26665aab1899e35ef7b93ddc3e1631c009dd04
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86228170"
---
# <a name="powershell-for-dns-alias-to-azure-sql-database"></a>用于管理 Azure SQL 数据库 DNS 别名的 PowerShell
[!INCLUDE[appliesto-sqldb-asa](../includes/appliesto-sqldb-asa.md)]

本文提供了一个 PowerShell 脚本，演示如何为托管 Azure SQL 数据库的 [SQL 服务器](logical-servers.md)管理 DNS 别名。

> [!NOTE]
> 本文进行了更新，以便使用 Azure PowerShell Az 模块或 Azure CLI。 你仍然可以使用 AzureRM 模块，至少在 2020 年 12 月之前，它将继续接收 bug 修补程序。
>
> 若要详细了解 Az 模块和 AzureRM 兼容性，请参阅 [Azure Powershell Az 模块简介](https://docs.microsoft.com/powershell/azure/new-azureps-module-az)。 有关安装说明，请参阅[安装 Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps) 或[安装 Azure CLI](/cli/install-azure-cli)。

## <a name="dns-alias-in-connection-string"></a>连接字符串中的 DNS 别名

若要连接[逻辑 SQL 服务器](logical-servers.md)，如 SQL Server Management Studio (SSMS) 客户端可以提供的 DNS 别名名称而不是真正的服务器名称。 在下面的示例服务器字符串中，别名 *any 的唯一别名的名称-* 替换四个节点服务器字符串中的第一个圆点分隔节点：

   `<yourServer>.database.chinacloudapi.cn`

## <a name="prerequisites"></a>先决条件

如果想要运行演示这篇文章中提供的 PowerShell 脚本，适用以下先决条件：

- Azure 订阅和帐户。如需 1 元试用版，请参阅 [Azure 试用版](https://www.azure.cn/pricing/1rmb-trial/)
- 两台服务器

## <a name="example"></a>示例

以下代码示例一开始就将文字值分配给多个变量。

若要运行此代码，请编辑占位符值，使之与系统中的实际值匹配。

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

使用的 cmdlet 如下：

- [New-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/New-azSqlServerDnsAlias)：在 Azure SQL 数据库服务系统中创建 DNS 别名。 该别名指代服务器 1。
- [Get-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Get-azSqlServerDnsAlias)：获取并列出分配给服务器 1 的所有别名。
- [Set-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Set-azSqlServerDnsAlias)：将别名根据配置引用的服务器名称从“服务器 1”修改为“服务器 2”。
- [Remove-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Remove-azSqlServerDnsAlias)：使用别名的名称从服务器 2 删除别名。

若要进行安装或升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)。

在 *powershell\_ise.exe* 中使用 `Get-Module -ListAvailable Az` 来查找版本。

```powershell
$subscriptionName = '<subscriptionName>';
$sqlServerDnsAliasName = '<aliasName>';
$resourceGroupName = '<resourceGroupName>';  
$sqlServerName = '<sqlServerName>';
$resourceGroupName2 = '<resourceGroupNameTwo>'; # can be same or different than $resourceGroupName
$sqlServerName2 = '<sqlServerNameTwo>'; # must be different from $sqlServerName.

# login to Azure
Connect-AzAccount -SubscriptionName $subscriptionName -Environment AzureChinaCloud;
$subscriptionId = Get-AzSubscription -SubscriptionName $subscriptionName;

Write-Host 'Assign an alias to server 1...';
New-AzSqlServerDnsAlias -ResourceGroupName $resourceGroupName -ServerName $sqlServerName `
    -Name $sqlServerDnsAliasName;

Write-Host 'Get the aliases assigned to server 1...';
Get-AzSqlServerDnsAlias -ResourceGroupName $resourceGroupName -ServerName $sqlServerName;

Write-Host 'Move the alias from server 1 to server 2...';
Set-AzSqlServerDnsAlias -ResourceGroupName $resourceGroupName2 -TargetServerName $sqlServerName2 `
    -Name $sqlServerDnsAliasName `
    -SourceServerResourceGroup $resourceGroupName -SourceServerName $sqlServerName `
    -SourceServerSubscriptionId $subscriptionId.Id;

Write-Host 'Get the aliases assigned to server 2...';
Get-AzSqlServerDnsAlias -ResourceGroupName $resourceGroupName2 -ServerName $sqlServerName2;

Write-Host 'Remove the alias from server 2...';
Remove-AzSqlServerDnsAlias -ResourceGroupName $resourceGroupName2 -ServerName $sqlServerName2 `
    -Name $sqlServerDnsAliasName;
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

使用的命令如下：

- [az sql server dns-alias create](https://docs.azure.cn/zh-cn/cli/sql/server/dns-alias?view=azure-cli-latest#az-sql-server-dns-alias-create)：为服务器创建 DNS 别名。 该别名指代服务器 1。
- [az sql server dns-alias show](https://docs.azure.cn/zh-cn/cli/sql/server/dns-alias?view=azure-cli-latest#az-sql-server-dns-alias-show)：获取并列出分配给服务器 1 的所有别名。
- [az sql server dns-alias set](https://docs.azure.cn/zh-cn/cli/sql/server/dns-alias?view=azure-cli-latest#az-sql-server-dns-alias-set)：将别名根据配置引用的服务器名称从“服务器 1”修改为“服务器 2”。
- [az sql server dns-alias delete](https://docs.azure.cn/zh-cn/cli/sql/server/dns-alias?view=azure-cli-latest#az-sql-server-dns-alias-delete)：使用别名的名称从服务器 2 删除别名。

若要安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

```azurecli
$subscriptionName = '<subscriptionName>';
$sqlServerDnsAliasName = '<aliasName>';
$resourceGroupName = '<resourceGroupName>';  
$sqlServerName = '<sqlServerName>';
$resourceGroupName2 = '<resourceGroupNameTwo>'; # can be same or different than $resourceGroupName
$sqlServerName2 = '<sqlServerNameTwo>'; # must be different from $sqlServerName.

# login to Azure
az cloud set --name AzureChinaCloud
az login -SubscriptionName $subscriptionName;
$subscriptionId = az account list[0].i -SubscriptionName $subscriptionName;

Write-Host 'Assign an alias to server 1...';
az sql server dns-alias create --resource-group $resourceGroupName --server $sqlServerName `
    --name $sqlServerDnsAliasName;

Write-Host 'Get the aliases assigned to server 1...';
az sql server dns-alias show --resource-group $resourceGroupName --server $sqlServerName;

Write-Host 'Move the alias from server 1 to server 2...';
az sql server dns-alias set --resource-group $resourceGroupName2 --server $sqlServerName2 `
    --name $sqlServerDnsAliasName `
    --original-resource-group $resourceGroupName --original-server $sqlServerName `
    --original-subscription-id $subscriptionId.Id;

Write-Host 'Get the aliases assigned to server 2...';
az sql server dns-alias show --resource-group $resourceGroupName2 --server $sqlServerName2;

Write-Host 'Remove the alias from server 2...';
az sql server dns-alias delete --resource-group $resourceGroupName2 --server $sqlServerName2 `
    --name $sqlServerDnsAliasName;
```

* * *

## <a name="next-steps"></a>后续步骤

有关 SQL 数据库的 DNS 别名功能的完整说明，请参阅 [Azure SQL 数据库的 DNS 别名](../../sql-database/dns-alias-overview.md)。
