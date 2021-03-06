---
title: include 文件
description: include 文件
services: virtual-network
author: rockboyfor
ms.service: virtual-network
ms.topic: include
origin.date: 04/13/2018
ms.date: 01/20/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 84affd242dd8b0b2483e30e2d49e621b8372b6cc
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "76530879"
---
## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>如何在 Azure 门户中创建经典 VNet
若要基于上述方案创建经典 VNet，请执行以下步骤。

1. 在浏览器中导航到 https://portal.azure.cn ，并根据需要使用 Azure 帐户登录。
2. 单击“创建资源” > “网络” > “虚拟网络”。 请注意，“选择部署模型”  列表已显示了“经典”  。 3. 单击“创建”  ，如下图所示。

    ![在 Azure 门户中创建 VNet](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)
4. 在“虚拟网络”窗格中，键入 VNet 的**名称**，并单击“地址空间”。 为 VNet 及其第一个子网配置地址空间设置，并单击“确定”。  下图显示了我们的方案的 CIDR 块设置。

    ![“地址空间”窗格](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)
5. 单击“资源组”并选择要将 VNet 添加到的资源组，或者单击“新建资源组”将 VNet 添加到新资源组。   下图显示了名为 **TestRG** 的新资源组的资源组设置。 有关资源组的详细信息，请访问 [Azure 资源管理器概述](../articles/azure-resource-manager/management/overview.md#resource-groups)。

    ![“创建资源组”窗格](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)
6. 如有必要，更改 VNet 的“订阅”和“位置”设置   。 
7. 如果不想看到该 VNet 作为**启动板**中的磁贴，请禁用“固定到启动板”  。 
8. 单击“创建”，注意名为“创建虚拟网络”的磁贴，如下图所示   。

    ![在门户中创建 VNet](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)
9. 等待 VNet 完成创建，当看到下面的磁贴时，单击它以添加更多子网。

    ![在门户中创建 VNet](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)
10. 你应该会看到你的 VNet 的**配置**，如下所示。 

    ![在门户中创建 VNet](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)
11. 单击“子网” > “添加”，为子网键入**名称**并指定“地址范围(CIDR 块)”，然后单击“确定”。 下图显示了当前方案的设置。

    ![在 Azure 门户中创建 VNet](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)

<!-- Update_Description: update meta properties, wording update, update link -->