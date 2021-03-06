---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
origin.date: 09/10/2020
ms.date: 09/15/2020
ms.author: v-tawe
ms.custom: generated
ms.openlocfilehash: 375320fc7d251576a84c34f75286109c131b87ff
ms.sourcegitcommit: f5d53d42d58c76bb41da4ea1ff71e204e92ab1a7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90524030"
---
|名称<br /><sub>（Azure 门户）</sub> |说明 |效果 |版本<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[允许的存储帐户 SKU](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7433c107-6db4-4ad1-b57a-a76dce0154a1) |此策略可用于指定组织可部署的一组存储帐户 SKU。 |拒绝 |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/AllowedStorageSkus_Deny.json) |
|[应为存储帐户启用异地冗余存储](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fbf045164-79ba-4215-8f95-f8048dc1780b) |此策略将审核未启用异地冗余存储的任何存储帐户。 |Audit、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/GeoRedundant_StorageAccounts_Audit.json) |
|[应启用安全传输到存储帐户](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F404c3081-a854-4457-ae30-26a93ef643f9) |审核存储帐户中安全传输的要求。 安全传输选项会强制存储帐户仅接受来自安全连接 (HTTPS) 的请求。 使用 HTTPS 可确保服务器和服务之间的身份验证并保护传输中的数据免受中间人攻击、窃听和会话劫持等网络层攻击 |Audit、Deny、Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_AuditForHTTPSEnabled_Audit.json) |
|[存储帐户应使用专用链接连接](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F6edd7eda-6dd8-40f7-810d-67160c639cd9) |专用链接通过与存储帐户建立专用连接来强制实施安全通信 |AuditIfNotExists、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/StorageAccountPrivateEndpointEnabled_Audit.json) |
|[存储帐户应使用客户管理的密钥进行加密](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F6fac406b-40ca-413b-bf8e-0bf964659c25) |使用客户管理的密钥 (CMK) 更灵活地保护存储帐户。 指定 CMK 时，该密钥会用于保护和控制对加密数据的密钥的访问。 使用 CMK 可提供附加功能来控制密钥加密密钥的轮换或以加密方式擦除数据。 |Audit、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/StorageAccountCustomerManagedKeyEnabled_Audit.json) |
|[存储帐户应允许从受信任的 Microsoft 服务进行访问](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc9d007d0-c057-4772-b18c-01e546713bcd) |某些与存储帐户交互的 Microsoft 服务在网络上运行，但这些网络无法通过网络规则获得访问权限。 若要帮助此类服务按预期方式工作，请允许受信任的 Microsoft 服务集绕过网络规则。 这些服务随后会使用强身份验证访问存储帐户。 |Audit、Deny、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/StorageAccess_TrustedMicrosoftServices_Audit.json) |
|[存储帐户应迁移到新的 Azure 资源管理器资源](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F37e0d2fe-28a5-43d6-a273-67d37d1f5606) |使用新的 Azure 资源管理器为存储帐户提供安全增强功能，例如：更强大的访问控制 (RBAC)、更好的审核、基于 Azure 资源管理器的部署和监管、对托管标识的访问权限、访问密钥保管库以获取机密、基于 Azure AD 的身份验证以及对标记和资源组的支持，以简化安全管理 |Audit、Deny、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Classic_AuditForClassicStorages_Audit.json) |
|[应限制对存储帐户的网络访问](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F34c877ad-507e-4c82-993e-3452a6e0ad3c) |应限制对存储帐户的网络访问。 配置网络规则，以便只允许来自允许的网络的应用程序访问存储帐户。 若要允许来自特定 Internet 或本地客户端的连接，可以向来自特定 Azure 虚拟网络或到公共 Internet IP 地址范围的流量授予访问权限 |Audit、Deny、Disabled |[1.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_NetworkAcls_Audit.json) |
|[存储帐户应使用虚拟网络规则来限制网络访问](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F2a1a9cdf-e04d-429a-8416-3bfb72a1b26f) |使用虚拟网络规则作为首选方法（而不使用基于 IP 的筛选），保护存储帐户免受潜在威胁危害。 禁止基于 IP 的筛选可以阻止公共 IP 访问你的存储帐户。 |Audit、Deny、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/StorageAccountOnlyVnetRulesEnabled_Audit.json) |
