---
title: Azure Functions 的 Azure 服务总线绑定
description: 了解如何在 Azure Functions 中发送 Azure 服务总线触发器和绑定。
author: craigshoemaker
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.topic: reference
ms.date: 08/11/2020
ms.author: v-junlch
ms.openlocfilehash: e393fc4e5380b61c1754f5fb91c75beb6386bc7a
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88223023"
---
# <a name="azure-service-bus-bindings-for-azure-functions"></a>Azure Functions 的 Azure 服务总线绑定

Azure Functions 通过[触发器和绑定](./functions-triggers-bindings.md)与 [Azure 服务总线](https://www.azure.cn/home/features/service-bus)集成。 与服务总线集成后，你可以构建响应和发送队列或主题消息的函数。

| 操作 | 类型 |
|---------|---------|
| 创建服务总线队列或主题消息时运行函数 | [触发器](./functions-bindings-service-bus-trigger.md) |
| 发送 Azure 服务总线消息 |[输出绑定](./functions-bindings-service-bus-output.md) |

## <a name="add-to-your-functions-app"></a>添加到 Functions 应用

### <a name="functions-2x-and-higher"></a>Functions 2.x 及更高版本

使用触发器和绑定需要引用相应的包。 NuGet 包用于 .NET 类库，而扩展捆绑包用于其他所有应用程序类型。

| 语言                                        | 添加方式...                                   | 备注 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | 安装 [NuGet 包]版本 4.x | |
| C# 脚本、Java、JavaScript、PowerShell | 注册[扩展捆绑包]          | 建议将 [Azure 工具扩展]用于 Visual Studio Code。 |
| C# 脚本（Azure 门户中仅限联机）         | 添加绑定                            | 若要更新现有绑定扩展而不必重新发布函数应用，请参阅[更新扩展]。 |

[NuGet 包]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ServiceBus/
[core tools]: ./functions-run-local.md
[扩展捆绑包]: ./functions-bindings-register.md#extension-bundles
[更新扩展]: ./install-update-binding-extensions-manual.md
[Azure 工具扩展]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>Functions 1.x

Functions 1.x 应用会自动引用 [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet 程序包（版本 2.x）。

## <a name="next-steps"></a>后续步骤

- [创建服务总线队列或主题消息时运行函数（触发器）](./functions-bindings-service-bus-trigger.md)
- [从 Azure Functions 发送 Azure 服务总线消息（输出绑定）](./functions-bindings-service-bus-output.md)

