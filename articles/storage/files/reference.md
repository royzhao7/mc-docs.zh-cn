---
title: Azure 文件存储参考
description: 查找 Azure 存储 API 参考、自述文件和客户端库包。
author: WenJason
ms.author: v-jay
origin.date: 07/14/2020
ms.date: 08/24/2020
ms.service: storage
ms.topic: conceptual
ms.reviewer: ripohane
ms.openlocfilehash: 57945ea08ac8d9dcbb591fc595e0c28826d969c9
ms.sourcegitcommit: ecd6bf9cfec695c4e8d47befade8c462b1917cf0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2020
ms.locfileid: "88753577"
---
# <a name="azure-files-reference"></a>Azure 文件存储参考

查找 Azure 文件 API 参考、库包、自述文件和入门文章。

## <a name="net-client-libraries"></a>.NET 客户端库

下表列出了 Azure 文件 .NET API 的参考文档和示例文档。

|  版本  | 参考文档 | 程序包 | 快速入门 |
| :-------: | ----------------------- | ------- | ---------- |
| 12.x | [适用于 .NET 的 Azure 文件客户端库 v12](https://docs.microsoft.com/dotnet/api/overview/azure/storage.files.shares-readme) | [包 (NuGet)](https://www.nuget.org/packages/Azure.Storage.Files/) | &nbsp; |
| 11.x | [Microsoft.Azure.Storage.File 命名空间](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.file) | [包 (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Storage.File/) | [使用 .NET 针对 Azure 文件进行开发](/storage/files/storage-dotnet-how-to-use-files) |

### <a name="storage-management"></a>存储管理

下表列出了 Azure 存储管理 .NET API 的参考文档。

|  版本  | 参考文档 | 程序包 |
| :-------: | ----------------------- | ------- |
| 16.x | [Microsoft.Azure.Management.Storage](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.storage) | [包 (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/) |

### <a name="data-movement"></a>数据移动

下表列出了 Azure 存储数据移动 .NET API 的参考文档。

|  版本  | 参考文档 | 程序包 |
| :-------: | ----------------------- | ------- |
| 1.x | [数据移动](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.datamovement) | [包 (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/) |

## <a name="java-client-libraries"></a>Java 客户端库

下表列出了 Azure 文件 Java API 的参考文档和示例文档。

|  版本  | 参考文档 | 程序包 | 快速入门 |
| :-------: | ----------------------- | ------- | ---------- |
| 12.x | [适用于 Java 的 Azure 文件客户端库](https://docs.microsoft.com/java/api/overview/azure/storage-file-share-readme) | [包 (Maven)](https://mvnrepository.com/artifact/com.azure/azure-storage-file-share) | &nbsp; |
| 8.x | [com.microsoft.azure.storage.file](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.file) | [包 (Maven)](https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) | [使用 Java 针对 Azure 文件进行开发](/storage/files/storage-java-how-to-use-file-storage) |

### <a name="storage-management"></a>存储管理

下表列出了 Azure 存储管理 Java API 的参考文档。

|  版本  | 参考文档 | 程序包 |
| :-------: | ----------------------- | ------- |
| 0.9.x | [com.microsoft.azure.management.storage](https://docs.microsoft.com/java/api/overview/azure/storage/management) | [包 (Maven)](https://mvnrepository.com/artifact/com.microsoft.azure/azure-svc-mgmt-storage) |

## <a name="python-client-libraries"></a>Python 客户端库

下表列出了 Azure 文件 Python API 的参考文档和示例文档。

|  版本  | 参考文档 | 程序包 | 快速入门 |
| :-------: | ----------------------- | ------- | ---------- |
| 12.x | [适用于 Python 的 Azure 存储客户端库 v12](https://docs.microsoft.com/azure/developer/python/sdk/storage/overview?view=storage-py-v12) | [包 (PyPI)](https://pypi.org/project/azure-storage-file/12.0.0b4/) | [示例](https://docs.microsoft.com/python/api/overview/azure/storage-file-share-readme#examples) |
| 2.x | [适用于 Python 的 Azure 存储客户端库 v2](https://docs.microsoft.com/azure/developer/python/sdk/storage/overview?view=storage-py-v2) | [包 (PyPI)](https://pypi.org/project/azure-storage-file/2.1.0/) | [使用 Python 针对 Azure 文件进行开发](/storage/files/storage-python-how-to-use-file-storage) |

## <a name="javascript-client-libraries"></a>JavaScript 客户端库

下表列出了 Azure 文件 JavaScript API 的参考文档和示例文档。

|  版本  | 参考文档 | 程序包 | 快速入门 |
| :-------: | ----------------------- | ------- | ---------- |
| 12.x | [适用于 JavaScript 的 Azure 文件客户端库](https://docs.microsoft.com/javascript/api/overview/azure/storage-file-share-readme) | [包 (npm)](https://www.npmjs.com/package/@azure/storage-file-share) | [示例](https://docs.microsoft.com/javascript/api/overview/azure/storage-file-share-readme#examples) |
| 10.x | [@azure/storage-file](https://docs.microsoft.com/javascript/api/@azure/storage-file) | [包 (npm)](https://www.npmjs.com/package/@azure/storage-file) | &nbsp; |

## <a name="rest-apis"></a>REST API

下表列出了 Azure 文件 REST API 的参考文档和示例文档。

| 参考文档 | 概述 |
| ----------------------- | -------- |
| [文件服务 REST API](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api) | [文件服务概念](https://docs.microsoft.com/rest/api/storageservices/file-service-concepts) |

### <a name="other-rest-reference"></a>其他 REST 参考

- [Azure 存储导入和导出 REST API](https://docs.microsoft.com/rest/api/storageimportexport/) 可帮助你管理导入/导出作业，这些作业将数据传输到 Blob 存储或从 Blob 存储传输数据。

## <a name="other-languages-and-platforms"></a>其他语言和平台

下面的列表包含一些链接，这些链接指向其他编程语言和平台的库。

- [C++](https://azure.github.io/azure-storage-cpp)
- [Ruby](https://azure.github.io/azure-storage-ruby)
- [PHP](https://azure.github.io/azure-storage-php/)
- [iOS](https://azure.github.io/azure-storage-ios/)
- [Android](https://azure.github.io/azure-storage-android)

## <a name="powershell"></a>PowerShell

下表包含指向参考内容的最新版本的链接。

| 版本 | 平台 |
| ------- | -------- |
|  3.x  | [PowerShell](https://docs.microsoft.com/powershell/module/az.storage/?view=azps-3.8.0) |
|  2.x  | [PowerShell](https://docs.microsoft.com/powershell/module/az.storage/?view=azps-2.8.0) |

## <a name="azure-cli"></a>Azure CLI

- [Azure CLI](/cli/storage)
