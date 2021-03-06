---
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 03/09/2020
ms.date: 06/01/2020
ms.author: v-jay
ms.openlocfilehash: ab1c96074d2061b2647199417059e16ca1af3d91
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84199682"
---
下表描述了表存储的容量、可伸缩性和性能目标。

| 资源 | 目标 |
|----------|---------------|
| Azure 存储帐户中表的个数 | 仅受存储帐户的容量限制 |
| 表中的分区个数 | 仅受存储帐户的容量限制 |
| 分区中实体的个数 | 仅受存储帐户的容量限制 |
| 单个表的最大大小 | 500 TiB |
| 单个实体的最大大小，包括所有属性值 | 1 MiB |
| 表实体中属性的最大数目 | 255（包括 3 个系统属性：**PartitionKey**、**RowKey** 和 **Timestamp**） |
| 实体中单个属性的最大总大小 | 因属性类型而异。 有关详细信息，请参阅[了解表服务数据模型](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model)中的**属性类型**。 |
| **PartitionKey** 的大小 | 大小最大为 1 KiB 的字符串 |
| **RowKey** 的大小 | 大小最大为 1 KiB 的字符串 |
| 实体组事务的大小 | 一个事务最多可包含 100 个实体，并且有效负载大小必须小于 4 MiB。 实体组事务只能包含对实体的更新一次。 |
| 每个表存储的访问策略的最大数目 | 5 |
| 每个存储帐户的最大请求速率 | 20,000 事务/秒，假定实体大小为 1-KiB |
| 单个表分区的目标吞吐量（1 KiB 实体） | 每秒最多 2,000 个实体 |
