---
title: Azure Stack HCI 计费和付款
description: Azure Stack HCI 中计费和付款功能的原理。
author: WenJason
ms.author: v-jay
ms.topic: conceptual
origin.date: 07/21/2020
ms.date: 08/31/2020
ms.openlocfilehash: 5a595734f7a77072030696b95cca0390de15fcfa
ms.sourcegitcommit: 4e2d781466e54e228fd1dbb3c0b80a1564c2bf7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/26/2020
ms.locfileid: "88871686"
---
# <a name="azure-stack-hci-billing-and-payment"></a>Azure Stack HCI 计费和付款

> 适用于：Azure Stack HCI，版本 20H2

Azure Stack HCI 是一项 Azure 服务，与其他 Azure 服务一样，要进行 Azure 订阅计费。 不需要传统的本地软件许可证，尽管来宾虚拟机 (VM) 可能要求执行单独的操作系统许可操作。 货币和折扣由 Azure Commerce 计费平台集中处理，客户会在月底获得一个综合的明细账单。

## <a name="what-does-azure-stack-hci-charge-for"></a>Azure Stack HCI 对什么收费？

对于已在使用 Azure 或任何其他云服务的客户来说，Azure Stack HCI 的云式计费模型并不陌生，是具有一致性和易于使用的模型。 在公共预览期间不收取任何费用。 预览使用期结束后，将基于统一费率按 Azure Stack HCI 群集中的每个物理处理器内核进行计费（如果还使用了其他 Azure 服务，会收取额外使用费）。

定价模型不同于基于使用情况或基于消耗量的计费（基于 VM 数量）。 虽然虚拟处理器内核 (VCPU) 数可能会在一个月内发生变化，但它不会影响你为 Azure Stack HCI 支付的价格：你仍按群集中存在的物理内核数付费。

## <a name="advantages-of-the-azure-stack-hci-billing-model"></a>Azure Stack HCI 计费模型的优点

- 该计费模型很简单：不设有关内存、存储或网络流入量/流出量的最小值、最大值以及任何数学计算。
- 虚拟化基础架构运行效率越高、虚拟到物理密度越高，通过该模型受益的程度就越高。
- 很容易计算本地 Azure Stack HCI 部署的成本，并且从网络边缘到数据中心，成本的改变是可以预测的。

## <a name="how-the-number-of-processor-cores-is-assessed"></a>如何估算处理器核心数

为确定群集中存在多少内核，Azure Stack HCI 会定期检查物理内核的数量，并报告给 Azure。 如果只是偶尔进行连接或发生连接中断，请不要担心；它始终会在稍后重试。 可以一次上传几天或几周的核心数据。 客户必须至少每 30 天连接到 Azure 一次，这样系统才能完成计费。

若要手动将核心数据上传到 Azure，请使用 `Sync-AzureStackHCI` cmdlet。

## <a name="faq"></a>FAQ

- 如果我已经有了一个 Azure 订阅，是否可以将它用于 Azure Stack HCI？ **是**
- 如果我的组织的财务部门已批准了有关 Azure 的费用，该费用是否涵盖 Azure Stack HCI？ **是**
- 如果我有 Azure 承诺使用费，是否可以将其用于 Azure Stack HCI？ **是**
- 如果我有 Azure 额度（例如面向学生的或作为奖励授予的），是否可以将其用于 Azure Stack HCI？ **是**
- 如果我的组织获得了企业协议折扣，这是否适用于 Azure Stack HCI？ **是**
- Azure 门户成本管理工具是否适用于 Azure Stack HCI？ **是**
- 使用 Azure 计费 API 构建的第三方或自定义工具是否适用于 Azure Stack HCI？ **是**

## <a name="next-steps"></a>后续步骤

如需相关信息，另请参阅：

- [定价概述 - Azure 定价原理](https://azure.cn/pricing/)
