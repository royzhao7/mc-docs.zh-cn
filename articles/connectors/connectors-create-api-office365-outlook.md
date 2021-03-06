---
title: 连接到 Office 365 Outlook
description: 使用 Azure 逻辑应用自动执行用于管理 Office 365 Outlook 中的电子邮件、联系人和日历的任务和工作流
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
origin.date: 01/08/2020
ms.date: 05/06/2020
ms.author: v-yeche
tags: connectors
ms.openlocfilehash: fda98498e5671eade2c4a9464271394f4a94ff2d
ms.sourcegitcommit: 81241aa44adbcac0764e2b5eb865b96ae56da6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83002047"
---
# <a name="manage-email-contacts-and-calendars-in-office-365-outlook-by-using-azure-logic-apps"></a>使用 Azure 逻辑应用管理 Office 365 Outlook 中的电子邮件、联系人和日历

使用 [Azure 逻辑应用](../logic-apps/logic-apps-overview.md)和 [Office 365 Outlook 连接器](https://docs.microsoft.com/connectors/office365connector/)，可以通过构建逻辑应用来创建用于管理 Office 365 帐户的自动化任务和工作流。 例如，可以自动执行以下任务：

* 获取、发送和回复电子邮件。 
* 在日历上安排会议。
* 添加和编辑联系人。 

可以使用任何触发器来启动工作流，例如，当新电子邮件到达时、当日历项更新时或其他服务（例如 Salesforce）中发生某个事件时。 可以使用对触发器事件做出响应的操作，例如，发送电子邮件或创建新的日历事件。 

> [!NOTE]
> 若要为 @outlook.com 或 @hotmail.com 帐户自动执行任务，请使用 [Outlook.com 连接器](../connectors/connectors-create-api-outlook.md)。

## <a name="prerequisites"></a>先决条件

* 一个 [Office 365 帐户](https://www.office.com/)

* Azure 订阅。 如果没有 Azure 订阅，请[注册一个 Azure 试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。 

* 你要在其中访问 Office 365 Outlook 帐户的逻辑应用。 若要通过 Office 365 Outlook 触发器启动工作流，需要有一个[空白逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)。 若要向工作流中添加 Office 365 Outlook 操作，逻辑应用需要已有一个触发器。

## <a name="add-a-trigger"></a>添加触发器

[触发器](../logic-apps/logic-apps-overview.md#logic-app-concepts)是一个事件，用于启动逻辑应用中的工作流。 此示例逻辑应用使用一个“轮询”触发器，该触发器根据指定的时间间隔和频率检查电子邮件帐户中是否有任何更新的日历事件。

1. 在 [Azure 门户](https://portal.azure.cn)中，在逻辑应用设计器中打开你的空白逻辑应用。

1. 在搜索框中，输入 `office 365 outlook` 作为筛选器。 此示例选择“即将启动将要发生的事件时”  。

    ![选择用于启动逻辑应用的触发器](./media/connectors-create-api-office365-outlook/office365-trigger.png)

1. 如果系统提示你登录，请提供你的 Office 365 凭据，以便逻辑应用可以连接到你的帐户。 或者，如果连接已存在，请提供触发器属性的信息。

    此示例选择供触发器检查的日历，例如：

    ![配置触发器的属性](./media/connectors-create-api-office365-outlook/select-calendar.png)

1. 在触发器中，设置“频率”  和“间隔”  值。 若要添加其他可用的触发器属性（例如“时区”  ），请从“添加新参数”  列表中选择那些属性。

    例如，如果希望触发器每 15 分钟检查一次日历，请将“频率”  设置为“分钟”  ，将“间隔”  设置为 `15`。 

    ![为触发器设置频率和间隔](./media/connectors-create-api-office365-outlook/calendar-settings.png)

1. 在设计器工具栏上选择“保存”。 

现在，添加一个在触发器触发后运行的操作。 例如，可以添加 Twilio“发送消息”  操作，当日历事件将在 15 分钟内启动时，该操作会发送一个文本。

## <a name="add-an-action"></a>添加操作

[操作](../logic-apps/logic-apps-overview.md#logic-app-concepts)是指由逻辑应用中的工作流运行的操作。 此示例逻辑应用在 Office 365 Outlook 中新建一个联系人。 可以使用来自其他触发器或操作的输出创建联系人。 例如，假设你的逻辑应用使用 Dynamics 365 触发器“创建记录时”。  你可以添加 Office 365 Outlook 的“创建联系人”  操作，并使用来自 SalesForce 触发器的输出来新建联系人。

1. 在 [Azure 门户](https://portal.azure.cn)的逻辑应用设计器中打开逻辑应用。

1. 若要将某个操作添加为工作流的最后一步，请选择“新建步骤”  。 

    若要在步骤之间添加操作，请将鼠标指针移到这些步骤之间的箭头上。 选择出现的加号 ( **+** )，然后选择“添加操作”。 

1. 在搜索框中，输入 `office 365 outlook` 作为筛选器。 此示例选择“创建联系人”  。

    ![选择要在逻辑应用中运行的操作](./media/connectors-create-api-office365-outlook/office365-actions.png) 

1. 如果系统提示你登录，请提供你的 Office 365 凭据，以便逻辑应用可以连接到你的帐户。 或者，如果连接已存在，请提供操作属性的信息。

    此示例选择可供操作在其中创建新联系人的联系人文件夹，例如：

    ![配置操作的属性](./media/connectors-create-api-office365-outlook/select-contacts-folder.png)

    若要添加其他可用的操作属性，请从“添加新参数”列表中选择那些属性。 

1. 在设计器工具栏上选择“保存”。 

## <a name="connector-specific-details"></a>特定于连接器的详细信息

有关连接器的 Swagger 文件所述的触发器、操作和限制的技术详细信息，请参阅[连接器的参考页](https://docs.microsoft.com/connectors/office365connector/)。 

## <a name="next-steps"></a>后续步骤

* 了解其他[逻辑应用连接器](../connectors/apis-list.md)

<!-- Update_Description: update meta properties, wording update, update link -->