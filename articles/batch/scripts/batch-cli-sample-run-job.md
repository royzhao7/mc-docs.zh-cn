---
title: Azure CLI 脚本示例 - 运行批处理作业
description: 此脚本将创建一个批处理作业，并将一系列任务添加到该作业。 它还演示了如何监视作业及其任务。
ms.topic: sample
author: rockboyfor
origin.date: 12/12/2019
ms.date: 08/24/2020
ms.testscope: yes|no
ms.testdate: 08/24/2020null
ms.author: v-yeche
ms.custom: devx-track-azurecli
ms.openlocfilehash: 0c0b0f3814eb71da283bf8a7ebd003faf969a60d
ms.sourcegitcommit: e633c458126612223fbf7a8853dbf19acc7f0fa5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/20/2020
ms.locfileid: "88654928"
---
# <a name="cli-example-run-a-job-and-tasks-with-azure-batch"></a>CLI 示例：使用 Azure Batch 运行作业和任务

此脚本将创建一个批处理作业，并将一系列任务添加到该作业。 它还演示了如何监视作业及其任务。 

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0.20 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。 

## <a name="example-script"></a>示例脚本

```azurecli
#!/bin/bash

# Create a resource group.
az group create --name myResourceGroup --location chinanorth

# Create a general-purpose storage account in your resource group.
az storage account create \
    --resource-group myResourceGroup \
    --name mystorageaccount \
    --location chinanorth \
    --sku Standard_LRS

# Create a Batch account.
az batch account create \
    --name mybatchaccount \
    --storage-account mystorageaccount \
    --resource-group myResourceGroup \
    --location chinanorth

# Authenticate against the account directly for further CLI interaction.
az batch account login \
    --name mybatchaccount \
    --resource-group myResourceGroup \
    --shared-key-auth

# Create a new Linux pool with a virtual machine configuration. 
az batch pool create \
    --id mypool \
    --vm-size Standard_A1 \
    --target-dedicated 2 \
    --image canonical:ubuntuserver:16.04-LTS \
    --node-agent-sku-id "batch.node.ubuntu 16.04"

# Create a new job to encapsulate the tasks that are added.
az batch job create \
    --id myjob \
    --pool-id mypool

# Add tasks to the job. Here the task is a basic shell command.
az batch task create \
    --job-id myjob \
    --task-id task1 \
    --command-line "/bin/bash -c 'printenv AZ_BATCH_TASK_WORKING_DIR'"

# To add many tasks at once, specify the tasks
# in a JSON file, and pass it to the command. 
# For format, see https://github.com/Azure/azure-docs-cli-python-samples/blob/master/batch/run-job/tasks.json.
az batch task create \
    --job-id myjob \
    --json-file tasks.json

# Update the job so that it is automatically
# marked as completed once all the tasks are finished.
az batch job set \
--job-id myjob \
--on-all-tasks-complete terminateJob

# Monitor the status of the job.
az batch job show --job-id myjob

# Monitor the status of a task.
az batch task show \
    --job-id myjob \
    --task-id task1

```

## <a name="clean-up-deployment"></a>清理部署

运行以下命令以删除资源组及其相关的所有资源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az group create](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az batch account create](https://docs.azure.cn/cli/batch/account?view=azure-cli-latest#az-batch-account-create) | 创建批处理帐户。 |
| [az batch account login](https://docs.azure.cn/cli/batch/account?view=azure-cli-latest#az-batch-account-login) | 针对指定的批处理帐户进行身份验证，以便进一步进行 CLI 交互。  |
| [az batch pool create](https://docs.azure.cn/cli/batch/pool?view=azure-cli-latest#az-batch-pool-create) | 创建计算节点池。  |
| [az batch job create](https://docs.azure.cn/cli/batch/job?view=azure-cli-latest#az-batch-job-create) | 创建批处理作业。  |
| [az batch task create](https://docs.azure.cn/cli/batch/task?view=azure-cli-latest#az-batch-task-create) | 将任务添加到指定的批处理作业。  |
| [az batch job set](https://docs.azure.cn/cli/batch/job?view=azure-cli-latest#az-batch-job-set) | 更新批处理作业的属性。  |
| [az batch job show](https://docs.azure.cn/cli/batch/job?view=azure-cli-latest#az-batch-job-show) | 检索指定批处理作业的详细信息。  |
| [az batch task show](https://docs.azure.cn/cli/batch/task?view=azure-cli-latest#az-batch-task-show) | 从指定的批处理作业中检索任务的详细信息。  |
| [az group delete](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

<!-- Update_Description: update meta properties, wording update, update link -->