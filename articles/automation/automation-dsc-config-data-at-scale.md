---
title: 为 Azure Automation State Configuration 大规模配置数据
description: 本文介绍了如何为 Azure Automation State Configuration 大规模配置数据。
keywords: dsc,powershell,配置,安装程序
services: automation
ms.service: automation
ms.subservice: dsc
author: WenJason
ms.author: v-jay
origin.date: 08/08/2019
ms.date: 08/10/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: b1ca916ecb3a1d874e57752ee8e82c0277ce8307
ms.sourcegitcommit: e6b216b180734783219378410e13192e314a4497
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/05/2020
ms.locfileid: "87790074"
---
# <a name="configure-data-at-scale-for-azure-automation-state-configuration"></a>为 Azure Automation State Configuration 大规模配置数据

> 适用于：Windows PowerShell 5.1

管理数百甚至数千个服务器可能很有难度。
客户反馈表明，最困难的方面实际上是管理[配置数据](https://docs.microsoft.com/powershell/scripting/dsc/configurations/configdata)。
跨逻辑构造（如位置、类型和环境）组织信息。

> [!NOTE]
> 本文引用了由开源社区维护的一个解决方案。
> 支持仅以 GitHub 协作的形式提供，而不是由 Azure 提供。

## <a name="community-project-datum"></a>社区项目：Datum

为了解决此难题，已创建了一个名为 [Datum](https://github.com/gaelcolas/Datum) 的由社区维护的解决方案。
Datum 是基于其他配置管理平台中的极佳思路构建的，实现的解决方案类型与 PowerShell DSC 相同。
信息将基于逻辑思路[组织到文本文件中](https://github.com/gaelcolas/Datum#3-intended-usage)。
示例有：

- 应该全局应用的设置
- 应该应用于某个位置中所有服务器的设置
- 应该应用于所有数据库服务器的设置
- 各个服务器设置

此信息按照你偏好的文件格式（JSON、Yaml 或 PSD1）进行组织。
然后提供 cmdlet，以通过将每个文件中的[信息合并](https://github.com/gaelcolas/Datum#datum-tree)到服务器或服务器角色的单一视图，来生成配置数据文件。

生成数据文件后，可将其与 [DSC 配置脚本](https://docs.microsoft.com/powershell/scripting/dsc/configurations/write-compile-apply-configuration)配合使用，以生成 MOF 文件，并[将 MOF 文件上传到 Azure 自动化](./tutorial-configure-servers-desired-state.md#create-and-upload-a-configuration-to-azure-automation)。
然后，在[本地](./automation-dsc-onboarding.md#enable-physicalvirtual-linux-machines)或[在 Azure 中](./automation-dsc-onboarding.md#enable-azure-vms)注册服务器以拉取配置。

若要试用 Datum，请访问 [PowerShell 库](https://www.powershellgallery.com/packages/datum/)并下载该解决方案，或者单击“项目站点”以查看[文档](https://github.com/gaelcolas/Datum#2-getting-started--concepts)。

## <a name="next-steps"></a>后续步骤

- 若要了解 PowerShell DSC，请参阅 [Windows PowerShell Desired State Configuration 概述](https://docs.microsoft.com/powershell/scripting/dsc/overview/overview)。
- 在 [DSC 资源](https://docs.microsoft.com/powershell/scripting/dsc/resources/resources)中了解 PowerShell DSC 资源。
- 有关本地 Configuration Manager 配置的详细信息，请参阅[配置本地 Configuration Manager](https://docs.microsoft.com/powershell/scripting/dsc/managing-nodes/metaconfig)。
