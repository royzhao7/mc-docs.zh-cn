---
title: 将 Windows Admin Center 注册到 Azure
description: 如何将 Windows Admin Center 注册到 Azure。
author: WenJason
ms.author: v-jay
ms.topic: how-to
origin.date: 07/21/2020
ms.date: 08/31/2020
ms.openlocfilehash: 4596ee2a4c33a797204616a292b351a9415246a4
ms.sourcegitcommit: 4e2d781466e54e228fd1dbb3c0b80a1564c2bf7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/26/2020
ms.locfileid: "88871696"
---
# <a name="register-windows-admin-center-with-azure"></a>将 Windows Admin Center 注册到 Azure

> 适用于 Azure Stack HCI v20H2；Windows Server 2019

若要结合使用 Azure 服务和 Windows Admin Center，必须首先在管理 PC 上安装 Windows Admin Center 并完成一次性注册。

## <a name="complete-the-registration-process-in-windows-admin-center"></a>在 Windows Admin Center 完成注册过程

1. 启动 Windows Admin Center，单击右上角的“设置”齿轮图标，随即会转到“帐户”页。 然后，在左侧的“网关”菜单中，选择“Azure”，然后单击“注册”  。
1. 系统会向你提供一个唯一代码。 单击代码右侧的“复制”按钮。
1. 单击“输入代码”，这将打开另一个浏览器窗口，你可以在其中粘贴应用或设备上显示的代码。
1. 粘贴代码后，系统提示即将登录到远程设备或服务上的 Windows Admin Center。 
1. 输入电子邮件或电话号码。 如果你的设备为托管设备，你将转到组织的登录页面进行身份验证。 按照说明进行操作并输入相应的凭据。
1. 应会看到以下消息：“你已登录到你设备上的 Windows Admin Center 应用程序。” 关闭浏览器窗口以返回原始注册页。
1. 通过提供 Azure Active Directory（租户）ID 连接到 Azure Active Directory。 如果你执行了之前的步骤，则系统会预填充 ID 字段。 如果你的组织未向你提供现有 ID，请将“Azure Active Directory 应用程序”设置为“新建” 。 如果你已有 ID，请单击“使用现有项”，系统将显示一个空字段，供你输入管理员提供的 ID。输入 ID 后，Windows Admin Center 将确认找到具有该 ID 的帐户。 如果你有现有 ID 但不知道它是什么，请按照[这些步骤](/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) 检索此 ID。
1. 单击“连接”按钮连接到 Azure。 应会看到一条确认消息，指示现在已经连接到 Azure AD。
1. 转到 Azure 门户中的“应用权限”，在 Azure 中授予权限。 在“授予许可”下，选择“授予管理员许可”。 
1. 关闭窗口，并通过 Azure 帐户登录到 Windows Admin Center。

## <a name="next-steps"></a>后续步骤

现在可以执行以下操作：

- [在 Windows 管理中心使用 Azure Stack HCI](../get-started.md)
- [连接到 Azure 混合服务](https://docs.microsoft.com/windows-server/manage/windows-admin-center/azure/)