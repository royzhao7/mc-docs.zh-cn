---
title: 快速入门：创建 Azure DB for MariaDB - ARM 模板
description: 本快速入门文章介绍如何使用 Azure 资源管理器模板创建 Azure DB for MariaDB 服务器。
author: WenJason
ms.service: mariadb
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: v-jay
origin.date: 05/14/2020
ms.date: 09/14/2020
ms.openlocfilehash: 1d9eae8d14c14ed40583867f9493b2d4fb3a840d
ms.sourcegitcommit: 5116a603d3cac3cbc2e2370ff857f871f8f51a5f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/08/2020
ms.locfileid: "89512952"
---
# <a name="quickstart-use-an-arm-template-to-create-an-azure-database-for-mariadb-server"></a>快速入门：使用 ARM 模板创建 Azure Database for MariaDB 服务器

Azure Database for MariaDB 是一种托管服务，可用于在云中运行、管理和缩放高可用性的 MariaDB 数据库。 在本快速入门中，使用 Azure 资源管理器模板（ARM 模板）在 Azure 门户、PowerShell 或 Azure CLI 中创建 Azure Database for MariaDB 服务器。

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

如果你的环境满足先决条件，并且你熟悉如何使用 ARM 模板，请选择“部署到 Azure”按钮。 Azure 门户中会打开模板。

[![部署到 Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.cn/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-managed-mariadb-with-vnet%2fazuredeploy.json)

## <a name="prerequisites"></a>先决条件

# <a name="portal"></a>[门户](#tab/azure-portal)

具有活动订阅的 Azure 帐户。 [创建一个试用帐户](https://wd.azure.cn/pricing/1rmb-trial-full)。

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

* 具有活动订阅的 Azure 帐户。 [创建一个试用帐户](https://wd.azure.cn/pricing/1rmb-trial-full)。
* 若要在本地运行代码，请安装 [Azure PowerShell](https://docs.microsoft.com/powershell/azure/)。

# <a name="cli"></a>[CLI](#tab/CLI)

* 具有活动订阅的 Azure 帐户。 [创建一个试用帐户](https://wd.azure.cn/pricing/1rmb-trial-full)。
* 若要在本地运行代码，请安装 [Azure CLI](/cli/)。

---

## <a name="review-the-template"></a>查看模板

创建 Azure Database for MariaDB 服务器时，请使用定义好的一组计算和存储资源。 若要了解详细信息，请参阅 [Azure DB for MariaDB 定价层](concepts-pricing-tiers.md)。 请在 [Azure 资源组](../azure-resource-manager/management/overview.md)中创建该服务器。

本快速入门中使用的模板来自 [Azure 快速启动模板](https://azure.microsoft.com/resources/templates/101-managed-mariadb-with-vnet/)。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serverName": {
            "type": "string",
            "metadata": {
                "description": "Server Name for Azure database for MariaDB"
            }
        },
        "administratorLogin": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Database administrator login name"
            }
        },
        "administratorLoginPassword": {
            "type": "securestring",
            "minLength": 8,
            "metadata": {
                "description": "Database administrator password"
            }
        },
        "skuCapacity": {
            "type": "int",
            "defaultValue": 2,
            "metadata": {
                "description": "Azure database for MariaDB compute capacity in vCores (2,4,8,16,32)"
            }
        },
        "skuName": {
            "type": "string",
            "defaultValue": "GP_Gen5_2",
            "metadata": {
                "description": "Azure database for MariaDB sku name "
            }
        },
        "skuSizeMB": {
            "type": "int",
            "defaultValue": 51200,
            "metadata": {
                "description": "Azure database for MariaDB Sku Size "
            }
        },
        "skuTier": {
            "type": "string",
            "defaultValue": "GeneralPurpose",
            "metadata": {
                "description": "Azure database for MariaDB pricing tier"
            }
        },
        "skuFamily": {
            "type": "string",
            "defaultValue": "Gen5",
            "metadata": {
                "description": "Azure database for MariaDB sku family"
            }
        },
        "mariadbVersion": {
            "type": "string",
            "allowedValues": [
                "10.2",
                "10.3"
            ],
            "defaultValue": "10.3",
            "metadata": {
                "description": "MariaDB version"
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "Location for all resources."
            }
        },
        "backupRetentionDays": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "MariaDB Server backup retention days"
            }
        },
        "geoRedundantBackup": {
            "type": "string",
            "defaultValue": "Disabled",
            "metadata": {
                "description": "Geo-Redundant Backup setting"
            }
        },
        "virtualNetworkName": {
            "type": "string",
            "defaultValue": "azure_mariadb_vnet",
            "metadata": {
                "description": "Virtual Network Name"
            }
        },
        "subnetName": {
            "type": "string",
            "defaultValue": "azure_mariadb_subnet",
            "metadata": {
                "description": "Subnet Name"
            }
        },
        "virtualNetworkRuleName": {
            "type": "string",
            "defaultValue": "AllowSubnet",
            "metadata": {
                "description": "Virtual Network RuleName"
            }
        },
        "vnetAddressPrefix": {
            "type": "string",
            "defaultValue": "10.0.0.0/16",
            "metadata": {
                "description": "Virtual Network Address Prefix"
            }
        },
        "subnetPrefix": {
            "type": "string",
            "defaultValue": "10.0.0.0/16",
            "metadata": {
                "description": "Subnet Address Prefix"
            }
        }
    },
    "variables": {
        "firewallrules": {
            "batch": {
                "rules": [
                    {
                        "Name": "rule1",
                        "StartIpAddress": "0.0.0.0",
                        "EndIpAddress": "255.255.255.255"
                    },
                    {
                        "Name": "rule2",
                        "StartIpAddress": "0.0.0.0",
                        "EndIpAddress": "255.255.255.255"
                    }
                ]
            }
        }
    },
    "resources": [
        {
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('virtualNetworkName')]",
            "location": "[parameters('location')]",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[parameters('vnetAddressPrefix')]"
                    ]
                }
            },
            "resources": [
                {
                    "apiVersion": "2018-06-01",
                    "type": "subnets",
                    "location": "[parameters('location')]",
                    "name": "[parameters('subnetName')]",
                    "dependsOn": [
                        "[parameters('virtualNetworkName')]"
                    ],
                    "properties": {
                        "addressPrefix": "[parameters('subnetPrefix')]"
                    }
                }
            ]
        },
        {
            "apiVersion": "2018-06-01",
            "type": "Microsoft.DBforMariaDB/servers",
            "location": "[parameters('location')]",
            "name": "[parameters('serverName')]",
            "sku": {
                "name": "[parameters('skuName')]",
                "tier": "[parameters('skuTier')]",
                "capacity": "[parameters('skuCapacity')]",
                "size": "[parameters('skuSizeMB')]",
                "family": "[parameters('skuFamily')]"
            },
            "properties": {
                "version": "[parameters('mariadbVersion')]",
                "administratorLogin": "[parameters('administratorLogin')]",
                "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
                "storageProfile": {
                    "storageMB": "[parameters('skuSizeMB')]",
                    "backupRetentionDays": "[parameters('backupRetentionDays')]",
                    "geoRedundantBackup": "[parameters('geoRedundantBackup')]"
                }
            },
            "resources": [
                {
                    "name": "[parameters('virtualNetworkRuleName')]",
                    "type": "virtualNetworkRules",
                    "apiVersion": "2018-06-01",
                    "properties": {
                        "virtualNetworkSubnetId": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworkName'), parameters('subnetName'))]",
                        "ignoreMissingVnetServiceEndpoint": true
                    },
                    "dependsOn": [
                        "[concat('Microsoft.DBforMariaDB/servers/', parameters('serverName'))]"
                    ]
                }
            ]
        },
        {
            "name": "[concat(parameters('serverName'),'/',variables('firewallrules').batch.rules[copyIndex()].Name)]",
            "type": "Microsoft.DBforMariaDB/servers/firewallRules",
            "apiVersion": "2018-06-01",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.DBforMariaDB/servers/', parameters('serverName'))]"
            ],
            "copy": {
                "name": "firewallRulesCopy",
                "mode": "Serial",
                "batchSize": 1,
                "count": "[length(variables('firewallrules').batch.rules)]"
            },
            "properties": {
                "startIpAddress": "[variables('firewallrules').batch.rules[copyIndex()].StartIpAddress]",
                "endIpAddress": "[variables('firewallrules').batch.rules[copyIndex()].EndIpAddress]"
            }
        }
    ]
}
```

该模板定义了五个 Azure 资源：

* [**Microsoft.Network/virtualNetworks**](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks)
* [**Microsoft.Network/virtualNetworks/subnets**](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks/subnets)
* [**Microsoft.DBforMariaDB/servers**](https://docs.microsoft.com/azure/templates/microsoft.dbformariadb/servers)
* [**Microsoft.DBforMariaDB/servers/virtualNetworkRules**](https://docs.microsoft.com/azure/templates/microsoft.dbformariadb/servers/virtualnetworkrules)
* [**Microsoft.DBforMariaDB/servers/firewallRules**](https://docs.microsoft.com/azure/templates/microsoft.dbformariadb/servers/firewallrules)

可以在 [Azure 快速入门模板](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Dbformariadb&pageNumber=1&sort=Popular)中找到更多 Azure DB for MariaDB 模板示例。

## <a name="deploy-the-template"></a>部署模板

# <a name="portal"></a>[门户](#tab/azure-portal)

选择以下链接以在 Azure 门户中部署 Azure DB for MariaDB 服务器模板：

[![部署到 Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.cn/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-managed-mariadb-with-vnet%2fazuredeploy.json)

在“部署具有 VNet 的 Azure DB for MariaDB”页上：

1. 对于“资源组”，请选择“新建”，输入新资源组的名称，然后选择“确定”  。

2. 如果你创建了一个新资源组，请为该资源组和新服务器选择一个位置。

3. 输入服务器名称、管理员登录名和管理员登录密码  。

    ![“部署具有 VNet 的 Azure DB for MariaDB”窗口，Azure 快速入门模板，Azure 门户](./media/quickstart-create-mariadb-server-database-arm-template/deploy-azure-database-mariadb-vnet.png)

4. 根据需要更改其他默认设置：

    * **订阅**：要用于服务器的 Azure 订阅。
    * **SKU 容量**：vCore 容量，值可以是 2（默认值）、4、8、16、32 或 64     。
    * **SKU 名称**：SKU 层前缀、SKU 系列和 SKU 容量，以下划线联接，例如 B_Gen5_1、GP_Gen5_2（默认值）或 MO_Gen5_32  。
    * **SKU 大小(MB)** ：Azure Database for MariaDB 服务器的存储大小，以 MB 为单位（默认值为 51200）。
    * **SKU 层**：部署层，例如“基本”、“常规用途”（默认值）或“内存优化”  。
    * **SKU 系列**：“Gen4”或“Gen5”（默认值），表示服务器部署的硬件代系 。
    * **Mariadb 版本**：要部署的 MariaDB 服务器版本，例如 10.2 或 10.3（默认值） 。
    * **备份保留天数**：异地冗余备份的所需保留期，以天为单位（默认值为 7）。
    * **异地冗余备份**：“已启用”或“已禁用”（默认值），具体取决于异地灾难恢复 (Geo-DR) 要求 。
    * **虚拟网络名称**：虚拟网络的名称（默认值为 azure_mariadb_vnet）。
    * **子网名称**：子网的名称（默认值为 azure_mariadb_subnet）。
    * **虚拟网络规则名称**：允许子网的虚拟网络规则的名称（默认值为 AllowSubnet）。
    * **VNet 地址前缀**：虚拟网络的地址前缀（默认值为 10.0.0.0/16）。
    * **子网前缀**：子网的地址前缀（默认值为 10.0.0.0/16）。

5. 阅读条款和条件，并选择“我同意上述条款和条件”。

6. 选择“购买”。

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

使用以下交互式代码通过模板创建新的 Azure Database for MariaDB 服务器。 该代码会提示你输入新服务器名称、新资源组的名称和位置，以及管理员帐户名称和密码。

```azurepowershell
$serverName = Read-Host -Prompt "Enter a name for the new Azure Database for MariaDB server"
$resourceGroupName = Read-Host -Prompt "Enter a name for the new resource group where the server will exist"
$location = Read-Host -Prompt "Enter an Azure region (for example, chinaeast2) for the resource group"
$adminUser = Read-Host -Prompt "Enter the Azure Database for MariaDB server's administrator account name"
$adminPassword = Read-Host -Prompt "Enter the administrator password" -AsSecureString

New-AzResourceGroup -Name $resourceGroupName -Location $location # Use this command when you need to create a new resource group for your deployment
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
    -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-managed-mariadb-with-vnet/azuredeploy.json `
    -serverName $serverName `
    -administratorLogin $adminUser `
    -administratorLoginPassword $adminPassword

Read-Host -Prompt "Press [ENTER] to continue: "
```

# <a name="cli"></a>[CLI](#tab/CLI)

使用以下交互式代码通过模板创建新的 Azure Database for MariaDB 服务器。 该代码会提示你输入新服务器名称、新资源组的名称和位置，以及管理员帐户名称和密码。

```azurecli
read -p "Enter a name for the new Azure Database for MariaDB server:" serverName &&
read -p "Enter a name for the new resource group where the server will exist:" resourceGroupName &&
read -p "Enter an Azure region (for example, chinaeast2) for the resource group:" location &&
read -p "Enter the Azure Database for MariaDB server's administrator account name:" adminUser &&
read -p "Enter the administrator password:" adminPassword &&
params='serverName='$serverName' administratorLogin='$adminUser' administratorLoginPassword='$adminPassword &&
az group create --name $resourceGroupName --location $location &&
az deployment group create --resource-group $resourceGroupName --parameters $params --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-managed-mariadb-with-vnet/azuredeploy.json &&
read -p "Press [ENTER] to continue: "
```

---

## <a name="review-deployed-resources"></a>查看已部署的资源

# <a name="portal"></a>[Portal](#tab/azure-portal)

按照以下步骤查看新 Azure Database for MariaDB 服务器的概览：

1. 在 [Azure 门户](https://portal.azure.cn)中，搜索并选择“Azure Database for MariaDB 服务器”。

2. 在数据库列表中选择你的新服务器。 随后会显示新 Azure Database for MariaDB 服务器的“概览”页面。

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

运行以下交互代码来查看有关 Azure Database for MariaDB 服务器的详细信息。 必须输入新服务器的名称。

```azurepowershell
$serverName = Read-Host -Prompt "Enter the name of your Azure Database for MariaDB server"
Get-AzResource -ResourceType "Microsoft.DbForMariaDB/servers" -Name $serverName | ft
Read-Host -Prompt "Press [ENTER] to continue: "
```

# <a name="cli"></a>[CLI](#tab/CLI)

运行以下交互代码来查看有关 Azure Database for MariaDB 服务器的详细信息。 必须输入新服务器的名称和资源组。

```azurecli
read -p "Enter your Azure Database for MariaDB server name: " serverName &&
read -p "Enter the resource group where the Azure Database for MariaDB server exists: " resourcegroupName &&
az resource show --resource-group $resourcegroupName --name $serverName --resource-type "Microsoft.DbForMariaDB/servers" &&
read -p "Press [ENTER] to continue: "
```

---

## <a name="clean-up-resources"></a>清理资源

如果不再需要该资源组，可以将其删除，这将删除资源组中的资源。

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. 在 [Azure 门户](https://portal.azure.cn)中，搜索并选择“资源组”。

2. 在资源组列表中，选择你的资源组的名称。

3. 在资源组的“概览”页中，选择“删除资源组” 。

4. 在确认对话框中，键入资源组的名称，然后选择“删除”。

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Read-Host -Prompt "Press [ENTER] to continue: "
```

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli
read -p "Enter the Resource Group name: " resourceGroupName &&
az group delete --name $resourceGroupName &&
read -p "Press [ENTER] to continue: "
```

---

## <a name="next-steps"></a>后续步骤

有关引导你完成 ARM 模板创建过程的分步教程，请参阅：

> [!div class="nextstepaction"]
> [教程：创建和部署你的第一个 ARM 模板](../azure-resource-manager/templates/template-tutorial-create-first-template.md)
