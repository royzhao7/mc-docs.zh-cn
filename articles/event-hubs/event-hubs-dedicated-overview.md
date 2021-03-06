---
title: 专用事件中心概述 - Azure 事件中心 | Microsoft Docs
description: 本文概述专用 Azure 事件中心，它提供事件中心的单租户部署。
ms.topic: article
origin.date: 06/23/2020
ms.date: 08/21/2020
ms.author: v-tawe
ms.openlocfilehash: d95dad40cf112052ead7380d4ee2177efbe934f0
ms.sourcegitcommit: 2e9b16f155455cd5f0641234cfcb304a568765a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/21/2020
ms.locfileid: "88715353"
---
<!-- event hubs dedicate is supported in mooncake, but event hubs cluster not availible -->
<!-- need to contact 21vainet to create dedicate cluster -->

# <a name="overview-of-event-hubs-dedicated"></a>专用事件中心概述

事件中心群集提供单租户部署来满足客户的苛刻流式处理要求。 此单租户套餐提供有保障的 99.99% SLA，只能在专用定价层上使用。 事件中心群集每秒能够引入数百万个事件，且提供有保障的容量和亚秒级的延迟。 在专用群集中创建的命名空间和事件中心不仅包括标准产品/服务的所有功能，而且不存在任何引入限制。 它还免费随附了热门的[事件中心捕获](event-hubs-capture-overview.md)功能，可让你自动批处理数据流并将其记录到 Azure 存储或 Azure Data Lake。 

群集是按容量单位 (CU) 预配和计费的，并预先分配了 CPU 数量和内存资源量。 可为每个群集购买 1、2、4、8、12、16 或 20 个 CU。 每个 CU 可以引入和流式传输的量取决于多种因素，例如生产者和使用者的数量、有效负载的形状、流出量速率（有关更多详细信息，请参阅下面的基准检验结果）。 

> [!NOTE]
> 默认情况下，所有事件中心群集都支持 Kafka，并支持可由现有基于 Kafka 的应用程序使用的 Kafka 终结点。 在群集上启用 Kafka 不影响非 Kafka 用例；没有对应的选项，或者不需要在群集上禁用 Kafka。

## <a name="why-dedicated"></a>为何采用专用形式？

专用事件中心为需要企业级容量的客户提供了三个引人注目的优点：

#### <a name="single-tenancy-guarantees-capacity-for-better-performance"></a>单租户可以保证容量，以提供更好的性能

专用群集可保证容量完全可缩放，并能够凭借完全持久的存储空间和亚秒级的延迟引入高达千兆字节的流式数据，从而能够适应任何流量激增情况。 

#### <a name="inclusive-and-exclusive-access-to-features"></a>对功能的专有访问权限和非专有访问权限 
专用产品/服务包括“捕获”等免费的功能，以及对即将推出的功能（例如“创建自己的密钥”(BYOK)）的专有访问权限。 该服务还为客户管理负载均衡、OS 更新、安全修补程序和分区，因此你可以花更少的时间来维护基础结构，而将更多的时间花在构建客户端功能上。  

#### <a name="cost-savings"></a>节约成本
在高流入量（大于 100 TU）的情况下，群集的每小时成本比在标准产品/服务中购买相当数量的吞吐量单位的成本要少得多。


## <a name="event-hubs-dedicated-quotas-and-limits"></a>事件中心专用层配额和限制

事件中心专用层产品/服务按固定的月度价格计费，至少使用 4 个小时。 专用层提供标准计划的所有功能，但具有企业规模容量和限制，以满足客户的工作负荷需求。 

| 功能 | 标准 | 专用 |
| --- |:---:|:---:|
| 带宽 | 20 TU（最多 40 TU） | 20 CU |
| 命名空间 |  1 | 每个 CU 50 |
| 事件中心 |  每个命名空间 10 | 每个命名空间 1000 |
| 入口事件 | 按每百万个事件支付 | 已含 |
| 消息大小 | 1000000 字节 | 1000000 字节 |
| 分区 | 每个事件中心 32 | 每个事件中心 1024 |
| 使用者组 | 每个事件中心 20 | 每个 CU 无限制，每个事件中心 1000 |
| 中转连接 | 包括 1,000 个，最大 5,000 个 | 包括 100000 个，最大 100000 个 |
| 消息保留期 | 7 天，每个 TU 包含 84 GB | 90 天，每个 CU 包含 10 TB |
| 捕获 | 按每小时支付 | 已含 |

## <a name="how-to-onboard"></a>如何加入

通过添加或删除 CU，可以在一个月内随时扩展或缩小容量，满足自身需求。 专用计划独一无二，它提供了一种亲身实践的加入体验，用户可从事件中心产品团队获得适合自己的灵活部署。 若要加入此 SKU，请[联系支持团队](https://support.azure.cn/support/support-azure/)。

## <a name="faqs"></a>常见问题

#### <a name="what-can-i-achieve-with-a-cluster"></a>可以使用群集来做什么？

对于事件中心群集，可以引入和流式传输的数据量取决于各种因素，例如生成者、使用者、引入和处理速率，等等。 

下表显示了我们在测试期间实现的基准结果：

| 有效负载形状 | 接收方 | 入口带宽| 入口消息 | 出口带宽 | 出口消息 | TU 总数 | 每个 CU 的 TU 数 |
| ------------- | --------- | ---------------- | ------------------ | ----------------- | ------------------- | --------- | ---------- |
| 100x1KB 批 | 2 | 400 MB/秒 | 400k 消息数/秒 | 800 MB/秒 | 800k 消息数/秒 | 400 TU | 100 TU | 
| 10x10KB 批 | 2 | 666 MB/秒 | 66.6k 消息数/秒 | 1.33 GB/秒 | 133k 消息数/秒 | 666 TU | 166 TU |
| 6x32KB 批 | 1 | 1.05 GB/秒 | 34k 消息数/秒 | 1.05 GB/秒 | 34k 消息数/秒 | 1000 TU | 250 TU |

测试中使用了以下条件：

- 一个专用层事件中心群集使用四个容量单位 (CU)。 
- 用于引入的事件中心包含 200 个分区。 
- 引入的数据由从所有分区接收数据的两个接收方应用程序接收。

#### <a name="can-i-scale-updown-my-cluster"></a>是否可以纵向扩展/纵向缩减群集？

创建后，群集将按最少 4 个小时的使用量计费。 你可以向事件中心团队提交[支持请求](https://support.azure.cn)，请求“纵向扩展或纵向缩减专用群集*”以改变群集规模。 完成纵向缩减群集的请求最多可能需要 7 天。 

#### <a name="how-will-geo-dr-work-with-my-cluster"></a>如何将异地灾难恢复应用于群集？

可以将专用层群集下的命名空间与专用层群集下的另一个命名空间进行异地配对。 不鼓励将专用层命名空间与标准产品/服务中的命名空间配对，因为吞吐量限制不兼容，会导致出错。 

#### <a name="can-i-migrate-my-standard-namespaces-to-belong-to-a-dedicated-tier-cluster"></a>是否可以迁移标准命名空间，以使其属于专用层群集？
目前，我们不支持将事件中心数据从标准命名空间迁移到专用命名空间的自动迁移过程。 

## <a name="next-steps"></a>后续步骤

请与 Azure 销售代表或 Azure 支持部门联系，获取有关事件中心专用容量的其他详细信息。 还可访问以下链接，了解有关事件中心定价层的详细信息：

- [专用事件中心定价](https://www.azure.cn/pricing/details/event-hubs/)。 还可以与 Azure 销售代表或 Azure 支持部门联系，获取有关事件中心专用容量的其他详细信息。
- [事件中心常见问题解答](event-hubs-faq.md)中包含了定价信息并解答了一些有关事件中心的常见问题。
