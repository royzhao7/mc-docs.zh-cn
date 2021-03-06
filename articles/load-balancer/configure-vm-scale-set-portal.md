---
title: 配置包含现有 Azure 负载均衡器的虚拟机规模集 - Azure 门户
description: 了解如何配置包含现有 Azure 负载均衡器的虚拟机规模集。
author: WenJason
ms.author: v-jay
ms.service: load-balancer
ms.topic: article
origin.date: 03/25/2020
ms.date: 04/06/2020
ms.openlocfilehash: ad6c51e111b57f17d27f31e396fab35101457018
ms.sourcegitcommit: 95efd248f5ee3701f671dbd5cfe0aec9c9959a24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82507657"
---
# <a name="configure-a-virtual-machine-scale-set-with-an-existing-azure-load-balancer-using-the-azure-portal"></a>使用 Azure 门户配置包含现有 Azure 负载均衡器的虚拟机规模集

本文介绍如何配置包含现有 Azure 负载均衡器的虚拟机规模集。 

## <a name="prerequisites"></a>先决条件

- Azure 订阅。
- 要将虚拟机规模集部署到的订阅中的现有标准 SKU 负载均衡器。
- 虚拟机规模集的 Azure 虚拟网络。

## <a name="sign-in-to-the-azure-portal"></a>登录到 Azure 门户

在 [https://portal.azure.cn](https://portal.azure.cn) 中登录 Azure 门户。



## <a name="deploy-virtual-machine-scale-set-with-existing-load-balancer"></a>部署包含现有负载均衡器的虚拟机规模集

在本部分，你将在 Azure 门户中创建一个包含现有 Azure 负载均衡器的虚拟机规模集。

> [!NOTE]
> 以下步骤假设已事先部署名为 myVNet 的虚拟网络，以及名为 myLoadBalancer 的 Azure 负载均衡器。  

1. 在屏幕的左上方，单击“创建资源” > “计算” > “虚拟机规模集”，或者在市场搜索中搜索“虚拟机规模集”。    

2. 选择“创建”  。

3. 在“创建虚拟机规模集”中输入 ，或者在“基本信息”选项卡中选择以下信息：  

    | 设置                        | Value                                                                                                 |
    |--------------------------------|-------------------------------------------------------------------------------------------------------|
    | **项目详细信息**            |                                                                                                       |
    | 订阅                   | 选择 Azure 订阅                                                                        |
    | 资源组                 | 选择“新建”，输入 myResourceGroup，然后选择“确定”；或选择现有的资源组。  |
    | **规模集详细信息**          |                                                                                                       |
    | 虚拟机规模集名称 | 输入 myVMSS                                                                                       |
    | 区域                         | 选择“中国东部 2”                                                                                     |
    | **实例详细信息**           |                                                                                                       |
    | 映像                          | 选择“Ubuntu Server 18.04 LTS”                                                                     |
    | 大小                           | 保留默认值                                                                                      |
    | **管理员帐户**      |                                                                                                       |
    | 身份验证类型            | 选择“密码”                                                                                    |
    | 用户名                       | 输入管理员用户名        |
    | 密码                       | 输入管理员密码    |
    | 确认密码               | 重新输入管理员密码 |


    :::image type="content" source="./media/vm-scale-sets/create-vm-scale-set-01.png" alt-text="创建虚拟机规模集。" border="true":::

4. 选择“网络”选项卡。 

5. 在“网络”选项卡中输入或选择以下信息： 

     设置                           | Value                                                    |
    |-----------------------------------|----------------------------------------------------------|
    | **虚拟网络配置** |                                                          |
    | 虚拟网络                   | 选择“myVNet”或现有的虚拟网络。       |
    | **负载均衡**                |                                                          |
    | 使用负载均衡器               | 选择“是”                                            |
    | **负载均衡设置**       |                                                          |
    | 负载均衡选项            | 选择“Azure 负载均衡器”                            |
    | 选择负载均衡器            | 选择“myLoadBalancer”或现有的负载均衡器  |
    | 选择后端池             | 选择“myBackendPool”或现有的后端池。   |

    :::image type="content" source="./media/vm-scale-sets/create-vm-scale-set-02.png" alt-text="创建虚拟机规模集。" border="true":::

6. 选择“管理”选项卡。 

7. 在“管理”选项卡中，将“启动诊断”设置为“关闭”。   

8. 选择蓝色的“查看 + 创建”按钮。 

9. 检查设置，然后选择“创建”按钮。 

## <a name="next-steps"></a>后续步骤

在本文中，你已部署一个包含现有 Azure 负载均衡器的虚拟机规模集。  若要详细了解虚拟机规模集和负载均衡器，请参阅：

- [什么是 Azure 负载均衡器？](load-balancer-overview.md)
- [什么是虚拟机规模集？](../virtual-machine-scale-sets/overview.md)
