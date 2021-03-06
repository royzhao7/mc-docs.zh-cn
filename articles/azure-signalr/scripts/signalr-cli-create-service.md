---
title: Azure CLI 脚本示例 - 创建 SignalR 服务
description: 继续学习示例脚本，在名称随机生成的新资源组中创建新的 Azure SignalR 服务。
author: sffamily
ms.service: signalr
ms.devlang: azurecli
ms.topic: sample
origin.date: 11/13/2018
ms.date: 08/21/2020
ms.author: v-tawe
ms.custom: mvc
ms.openlocfilehash: ede18eb2cc1dbf4a04692379b4e482b7924aa3ea
ms.sourcegitcommit: 2e9b16f155455cd5f0641234cfcb304a568765a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/21/2020
ms.locfileid: "88715296"
---
# <a name="create-a-signalr-service"></a>创建 SignalR 服务 

此示例脚本在名称随机生成的新资源组中创建新的 Azure SignalR 服务资源。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI]( /cli/install-azure-cli)。 

## <a name="sample-script"></a>示例脚本

此脚本创建新的 SignalR 服务资源和新的资源组。 

```azurecli
#!/bin/bash

# Generate a unique suffix for the service name
let randomNum=$RANDOM*$RANDOM

# Generate a unique service and group name with the suffix
SignalRName=SignalRTestSvc$randomNum
#resource name must be lowercase
mySignalRSvcName=${SignalRName,,}
myResourceGroupName=$SignalRName"Group"

# Create resource group 
az group create --name $myResourceGroupName --location chinaeast2

# Create the Azure SignalR Service resource
az signalr create \
  --name $mySignalRSvcName \
  --resource-group $myResourceGroupName \
  --sku Standard_S1 \
  --unit-count 1 \
  --service-mode Default

# Get the SignalR primary connection string 
primaryConnectionString=$(az signalr key list --name $mySignalRSvcName \
  --resource-group $myResourceGroupName --query primaryConnectionString -o tsv)

echo "$primaryConnectionString"
```

记下为新资源组生成的实际名称。 如果要删除所有组资源，将使用该资源组名称。

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

表中的每条命令均链接到特定于命令的文档。 此脚本使用以下命令：

| Command | 说明 |
|---|---|
| [az group create](/cli/group#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az signalr create](/cli/signalr#az-signalr-create) | 创建 Azure SignalR 服务资源。 |
| [az signalr key list](/cli/signalr/key#az-signalr-key-list) | 列出密钥，使用 SignalR 推送实时内容更新时，应用程序将使用这些密钥。 |


## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)。

可在 [Azure SignalR 服务文档](../signalr-reference-cli.md)中找到其他 Azure SignalR 服务 CLI 脚本示例。
