---
title: 安全控件
titleSuffix: Azure Storage
description: 用于评估 Azure 存储的安全控制清单。
services: storage
author: WenJason
ms.author: v-jay
ms.service: storage
ms.subservice: common
ms.topic: conceptual
origin.date: 03/11/2020
ms.date: 07/20/2020
ms.openlocfilehash: 84c7bf8d64ce7250e91facc2699f776210f269e3
ms.sourcegitcommit: 31da682a32dbb41c2da3afb80d39c69b9f9c1bc6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2020
ms.locfileid: "86414575"
---
# <a name="security-controls-for-azure-storage"></a>Azure 存储的安全控制

本文介绍 Azure 存储中内置的安全控制。

[!INCLUDE [Security controls Header](../../../includes/security-controls-header.md)]

## <a name="data-protection"></a>数据保护

| 安全控制 | Yes/No | 注释 |
|---|---|--|
| 服务器端静态加密：Microsoft 管理的密钥 | 是 |  |
| 服务器端静态加密：客户管理的密钥 (BYOK) | 是 | 请参阅[在 Azure Key Vault 中使用客户托管密钥进行存储服务加密](storage-service-encryption-customer-managed-keys.md?toc=%2fstorage%2fblobs%2ftoc.json)。|
| 列级加密（Azure 数据服务）| 空值 |  |
| 传输中加密（例如 ExpressRoute 加密、VNet 中加密，以及 VNet-VNet 加密）| 是 | 支持标准的 HTTPS/TLS 机制。  用户也可以先加密数据，然后再将其传输到服务。 |
| 加密的 API 调用| 是 |  |

## <a name="network"></a>网络

| 安全控制 | Yes/No | 注释 |
|---|---|--|
| 服务终结点支持| 是 |  |
| 服务标记支持| 是 | 有关 Azure 存储支持的服务标记的详细信息，请参阅 [Azure 服务标记概述](../../virtual-network/service-tags-overview.md)。 |
| VNet 注入支持| 空值 |  |
| 网络隔离和防火墙支持| 是 | |
| 强制隧道支持| 空值 |  |

## <a name="monitoring--logging"></a>监视和日志记录

| 安全控制 | Yes/No | 注释|
|---|---|--|
| Azure 监视支持（Log Analytics、App Insights 等）| 是 | Azure Monitor 指标|
| 控制和管理平面日志记录和审核 | 是 | Azure 活动日志 |
| 数据平面日志记录和审核| 是 | Azure Monitor 资源日志 |

## <a name="identity"></a>标识

| 安全控制 | Yes/No | 注释|
|---|---|--|
| 身份验证| 是 | Azure Active Directory、共享密钥、共享访问令牌。 |
| 授权| 是 | 支持通过 RBAC、POSIX ACL 和 SAS 令牌进行授权 |

## <a name="configuration-management"></a>配置管理

| 安全控制 | Yes/No | 注释|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 是 | 支持通过 Azure 资源管理器 API 进行资源提供程序版本控制 |

## <a name="next-steps"></a>后续步骤

- 详细了解[跨 Azure 服务的内置安全控制](../../security/fundamentals/security-controls.md)。