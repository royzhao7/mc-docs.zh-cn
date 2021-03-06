---
title: 在 AD FS 中添加 Azure Stack Hub 用户
description: 了解如何为 Active Directory 联合身份验证服务 (AD FS) 部署添加 Azure Stack Hub 用户。
author: WenJason
ms.topic: article
origin.date: 06/03/2019
ms.date: 05/18/2020
ms.author: v-jay
ms.reviewer: unknown
ms.lastreviewed: 06/03/2019
ms.openlocfilehash: 76c06fd0daab2ebf7cab962a93ed1b4a7a4cbfbc
ms.sourcegitcommit: 134afb420381acd8d6ae56b0eea367e376bae3ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83422462"
---
# <a name="add-a-new-azure-stack-hub-user-account-in-active-directory-federation-services-ad-fs"></a>在 Active Directory 联合身份验证服务 (AD FS) 中添加新的 Azure Stack Hub 用户帐户

可以使用“Active Directory 用户和计算机”  管理单元将其他用户添加到 Azure Stack Hub 环境中，并使用 AD FS 作为其标识提供者。

## <a name="add-windows-server-active-directory-users"></a>添加 Windows Server Active Directory 用户

1. 使用允许访问 Windows 管理工具的帐户登录计算机，然后打开新的 Microsoft 管理控制台 (MMC)。
2. 选择“文件”>“添加或删除管理单元”  。

   > [!TIP]
   > 将“directory-domain”  替换为与目录匹配的域。 

3. 选择“Active Directory 用户和计算机”   > “directory-domain”   > “用户”  。
4. 选择“操作”   > “新建”   > “用户”  。
5. 在“新建对象 - 用户”中，提供用户详细信息。 选择“**下一页**”。
6. 提供并确认密码。
7. 选择“下一步”  以完成值。 选择“完成”  以创建用户。


## <a name="next-steps"></a>后续步骤

[创建用于访问 Azure Stack Hub 资源的应用标识](azure-stack-create-service-principals.md)