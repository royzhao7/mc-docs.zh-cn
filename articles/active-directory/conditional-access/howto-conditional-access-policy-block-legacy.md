---
title: 条件访问 - 阻止旧式身份验证 - Azure Active Directory
description: 创建自定义条件访问策略以阻止旧身份验证协议
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 09/07/2020
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8e1a470bb2ddc500fa0a417dd62c797b635b96e3
ms.sourcegitcommit: 25d542cf9c8c7bee51ec75a25e5077e867a9eb8b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2020
ms.locfileid: "89593688"
---
# <a name="conditional-access-block-legacy-authentication"></a>条件访问：阻止传统身份验证

由于与旧身份验证协议相关的风险增加，Microsoft 建议组织阻止使用这些协议的身份验证请求，并要求使用新式身份验证。

## <a name="create-a-conditional-access-policy"></a>创建条件访问策略

以下步骤将帮助创建条件访问策略以阻止旧身份验证请求。 此策略最初将置于“仅限报告”模式，以便管理员确定其对现有用户产生的影响。 当管理员认为策略按预期方式应用时，可以通过添加特定组并排除其他组来切换到“开”或暂存部署。

1. 以全局管理员、安全管理员或条件访问管理员的身份登录到 Azure 门户。
1. 浏览到“Azure Active Directory” > “安全性” > “条件访问”。
1. 选择“新策略”。
1. 为策略指定一个名称。 建议组织创建一个有意义的策略名称标准。
1. 在“分配”下，选择“用户和组”
   1. 在“包括”下，选择“所有用户”。
   1. 在“排除”下，选择“用户和组”，然后选择必须保留使用旧式身份验证功能的任何帐户。 排除至少一个帐户以防止你被锁定。如果不排除任何帐户，将无法创建此策略。
   1. 选择“完成”。
1. 在“云应用或操作”下，选择“所有云应用”。
   1. 选择“完成”。
1. 在“条件” > “客户端应用”下，将“配置”设置为“是”   。
   1. 仅勾选“Exchange ActiveSync 客户端”和“其他客户端”框。  若要在 Azure 中部署 Exchange ActiveSync 条件访问策略，用户还必须是全局管理员。
   1. 选择“完成”。
1. 在“访问控制” > “授予”下，选择“阻止访问”。
   1. 选择“选择”  。
1. 确认设置并将“启用策略”设置为“仅限报告”。
1. 选择“创建”  ，以便创建启用策略所需的项目。

## <a name="next-steps"></a>后续步骤

[条件访问常见策略](concept-conditional-access-policy-common.md)

[使用条件访问 What If 工具模拟登录行为](troubleshoot-conditional-access-what-if.md)

[如何设置多功能设备或应用程序以使用 Office 365 和 Microsoft 365 发送电子邮件](https://docs.microsoft.com/exchange/mail-flow-best-practices/how-to-set-up-a-multifunction-device-or-application-to-send-email-using-office-3)

