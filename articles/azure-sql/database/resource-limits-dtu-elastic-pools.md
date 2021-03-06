---
title: DTU 资源限制弹性池
description: 本页介绍了 Azure SQL 数据库中弹性池的一些常见的 DTU 资源限制。
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: seo-lt-2019 sqldbrb=1 references_regions
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 07/28/2020
ms.date: 09/14/2020
ms.openlocfilehash: fe47b84fb28443d51edcc704cdc2ccd34c75c80d
ms.sourcegitcommit: d5cdaec8050631bb59419508d0470cb44868be1a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/11/2020
ms.locfileid: "90014192"
---
# <a name="resources-limits-for-elastic-pools-using-the-dtu-purchasing-model"></a>使用 DTU 购买模型的弹性池的资源限制
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

本文详细介绍了位于使用 DTU 购买模型的弹性池中的 Azure SQL 数据库的资源限制。

* 有关 Azure SQL 数据库的 DTU 购买模型资源限制，请参阅 [DTU 资源限制 - Azure SQL 数据库](resource-limits-dtu-single-databases.md)。
* 有关 vCore 资源限制，请参阅 [vCore 资源限制 - Azure SQL 数据库](resource-limits-vcore-single-databases.md)和 [vCore 资源限制 - 弹性池](resource-limits-vcore-elastic-pools.md)。

## <a name="elastic-pool-storage-sizes-and-compute-sizes"></a>弹性池：存储大小和计算大小

对于 Azure SQL 数据库弹性池，下表显示了每个服务层级可用的资源和计算大小。 可以通过以下方式设置服务层、计算大小和存储量：

* [Azure 门户](elastic-pool-manage.md#azure-portal)
* [PowerShell](elastic-pool-manage.md#powershell)
* [Azure CLI](elastic-pool-manage.md#azure-cli)
* [REST API](elastic-pool-manage.md#rest-api)。

> [!IMPORTANT]
> 有关缩放指南和注意事项，请参阅[缩放弹性池](elastic-pool-scale.md)

弹性池中各个数据库的资源限制通常与池外部基于 DTU 和服务层级的各个数据库相同。 例如，S2 数据库的最大并发辅助进程数为 120 个。 因此，如果池中每个数据库的最大 DTU 是 50 个 DTU（这等效于 S2），则标准池中数据库的最大并发辅助进程数也是 120 个辅助进程。
 
对于相同数量的 DTU，提供给弹性池的资源可能会超过提供给弹性池外部的单个数据库的资源。 这意味着根据工作负荷模式，弹性池的 eDTU 利用率可能低于池中各个数据库的 DTU 利用率之和。 例如，在弹性池中只有一个数据库的极端情况下，如果数据库 DTU 利用率为 100%，则对于某些工作负荷模式，池 eDTU 利用率可能为 50%。 即使每个数据库的最大 DTU 保持在给定池大小支持的最大值，也可能发生这种情况。

> [!NOTE]
> 以下每个表中每个池资源的存储限制不包括 tempdb 和日志存储。

### <a name="basic-elastic-pool-limits"></a>基本弹性池限制

| 每个池的 eDTU 数 | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| 每个池包含的存储 (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| 每个池的最大存储空间 (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| 每个池的最大内存中 OLTP 存储 (GB) | 空值 | 空值 | 空值 | 空值 | 空值 | 空值 | 空值 | 空值 |
| 每个池的最大数据库数 | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| 每个池的最大并发辅助角色数（请求数）<sup>1</sup> | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| 每个池的最大并发会话数 <sup>1</sup> | 30000 | 30000 | 30000 | 30000 |30000 | 30000 | 30000 | 30000 |
| 每个数据库的最小 DTU 选项 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 |
| 每个数据库的最大 DTU 选项 | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| 每个数据库的最大存储空间 (GB) | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |
||||||||

<sup>1</sup> 如需了解任何单个数据库的最大并发辅助角色数（请求数），请参阅[单一数据库资源限制](resource-limits-vcore-single-databases.md)。 例如，如果弹性池使用 Gen5 且每个数据库的最大 vCore 数设置为 2，则最大并发辅助角色数目值为 200。  如果每个数据库的最大 vCore 数设置为 0.5，则最大并发辅助角色数目值为 50，因为 Gen5 上每个 vCore 的最大并发辅助角色数为 100。 对于每个数据库的最大 vCore 设置小于 1 个 vCore 或更少的其他情况，最大并发辅助角色数会相应重新缩放。

### <a name="standard-elastic-pool-limits"></a>标准弹性池限制

| 每个池的 eDTU 数 | **50** | **100** | **200** | **300** | **400** | **800**|
|:---|---:|---:|---:| ---: | ---: | ---: |
| 每个池包含的存储 (GB) <sup>1</sup> | 50 | 100 | 200 | 300 | 400 | 800 |
| 每个池的最大存储空间 (GB) | 500 | 750 | 1024 | 1280 | 1536 | 2048 |
| 每个池的最大内存中 OLTP 存储 (GB) | 空值 | 空值 | 空值 | 空值 | 空值 | 空值 |
| 每个池的最大数据库数 | 100 | 200 | 500 | 500 | 500 | 500 |
| 每个池的最大并发辅助角色数（请求数）<sup>2</sup> | 100 | 200 | 400 | 600 | 800 | 1600 |
| 每个池的最大并发会话数 <sup>2</sup> | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
| 每个数据库的最小 DTU 选项 | 0, 10, 20, 50 | 0, 10, 20, 50, 100 | 0, 10, 20, 50, 100, 200 | 0, 10, 20, 50, 100, 200, 300 | 0, 10, 20, 50, 100, 200, 300, 400 | 0, 10, 20, 50, 100, 200, 300, 400, 800 |
| 每个数据库的最大 DTU 选项 | 10, 20, 50 | 10, 20, 50, 100 | 10, 20, 50, 100, 200 | 10, 20, 50, 100, 200, 300 | 10, 20, 50, 100, 200, 300, 400 | 10, 20, 50, 100, 200, 300, 400, 800 |
| 每个数据库的最大存储空间 (GB) | 500 | 750 | 1024 | 1024 | 1024 | 1024 |
||||||||

<sup>1</sup> 请参阅 [SQL 数据库定价选项](https://azure.cn/pricing/details/sql-database/)，详细了解由于任何预配的额外存储导致的额外成本。

<sup>2</sup> 如需了解任何单个数据库的最大并发辅助角色数（请求数），请参阅[单一数据库资源限制](resource-limits-vcore-single-databases.md)。 例如，如果弹性池使用 Gen5 且每个数据库的最大 vCore 数设置为 2，则最大并发辅助角色数目值为 200。  如果每个数据库的最大 vCore 数设置为 0.5，则最大并发辅助角色数目值为 50，因为 Gen5 上每个 vCore 的最大并发辅助角色数为 100。 对于每个数据库的最大 vCore 设置小于 1 个 vCore 或更少的其他情况，最大并发辅助角色数会相应重新缩放。

### <a name="standard-elastic-pool-limits-continued"></a>标准弹性池限制（续）

| 每个池的 eDTU 数 | **1200** | **1600** | **2000** | **2500** | **3000** |
|:---|---:|---:|---:| ---: | ---: |
| 每个池包含的存储 (GB) <sup>1</sup> | 1200 | 1600 | 2000 | 2500 | 3000 |
| 每个池的最大存储空间 (GB) | 2560 | 3072 | 3584 | 4096 | 4096 |
| 每个池的最大内存中 OLTP 存储 (GB) | 空值 | 空值 | 空值 | 空值 | 空值 |
| 每个池的最大数据库数 | 500 | 500 | 500 | 500 | 500 |
| 每个池的最大并发辅助角色数（请求数）<sup>2</sup> | 2400 | 3200 | 4000 | 5000 | 6000 |
| 每个池的最大并发会话数 <sup>2</sup> | 30000 | 30000 | 30000 | 30000 | 30000 |
| 每个数据库的最小 DTU 选项 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| 每个数据库的最大 DTU 选项 | 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| 每个数据库的最大存储空间 (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
|||||||

<sup>1</sup> 请参阅 [SQL 数据库定价选项](https://azure.cn/pricing/details/sql-database/)，详细了解由于任何预配的额外存储导致的额外成本。

<sup>2</sup> 如需了解任何单个数据库的最大并发辅助角色数（请求数），请参阅[单一数据库资源限制](resource-limits-vcore-single-databases.md)。 例如，如果弹性池使用 Gen5 且每个数据库的最大 vCore 数设置为 2，则最大并发辅助角色数目值为 200。  如果每个数据库的最大 vCore 数设置为 0.5，则最大并发辅助角色数目值为 50，因为 Gen5 上每个 vCore 的最大并发辅助角色数为 100。 对于每个数据库的最大 vCore 设置小于 1 个 vCore 或更少的其他情况，最大并发辅助角色数会相应重新缩放。

### <a name="premium-elastic-pool-limits"></a>高级弹性池限制

| 每个池的 eDTU 数 | **125** | **250** | **500** | **1000** | **1500**|
|:---|---:|---:|---:| ---: | ---: |
| 每个池包含的存储 (GB) <sup>1</sup> | 250 | 500 | 750 | 1024 | 1536 |
| 每个池的最大存储空间 (GB) | 1024 | 1024 | 1024 | 1024 | 1536 |
| 每个池的最大内存中 OLTP 存储 (GB) | 1 | 2 | 4 | 10 | 12 |
| 每个池的最大数据库数 | 50 | 100 | 100 | 100 | 100 |
| 每个池的最大并发工作线程数（请求数）<sup>2</sup> | 200 | 400 | 800 | 1600 | 2400 |
| 每个池的最大并发会话数 <sup>2</sup> | 30000 | 30000 | 30000 | 30000 | 30000 |
| 每个数据库的最小 eDTU 数 | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 | 0, 25, 50, 75, 125, 250, 500, 1000|
| 每个数据库的最大 eDTU 数 | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 25, 50, 75, 125, 250, 500, 1000|
| 每个数据库的最大存储空间 (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
|||||||

<sup>1</sup> 请参阅 [SQL 数据库定价选项](https://azure.microsoft.com/pricing/details/sql-database/elastic/)，详细了解由于任何预配的额外存储导致的额外成本。
<sup>1</sup> 如需了解任何单个数据库的最大并发辅助角色数（请求数），请参阅[单一数据库资源限制](resource-limits-vcore-single-databases.md)。 例如，如果弹性池使用 Gen5 且每个数据库的最大 vCore 数设置为 2，则最大并发辅助角色数目值为 200。  如果每个数据库的最大 vCore 数设置为 0.5，则最大并发辅助角色数目值为 50，因为 Gen5 上每个 vCore 的最大并发辅助角色数为 100。 对于每个数据库的最大 vCore 设置小于 1 个 vCore 或更少的其他情况，最大并发辅助角色数会相应重新缩放。

### <a name="premium-elastic-pool-limits-continued"></a>高级弹性池限制（续）

| 每个池的 eDTU 数 | **2000** | **2500** | **3000** | **3500** | **4000**|
|:---|---:|---:|---:| ---: | ---: |
| 每个池包含的存储 (GB) <sup>1</sup> | 2048 | 2560 | 3072 | 3548 | 4096 |
| 每个池的最大存储空间 (GB) | 2048 | 2560 | 3072 | 3548 | 4096|
| 每个池的最大内存中 OLTP 存储 (GB) | 16 | 20 | 24 | 28 | 32 |
| 每个池的最大数据库数 | 100 | 100 | 100 | 100 | 100 |
| 每个池的最大并发辅助角色数（请求数）<sup>2</sup> | 3200 | 4000 | 4800 | 5600 | 6400 |
| 每个池的最大并发会话数 <sup>2</sup> | 30000 | 30000 | 30000 | 30000 | 30000 |
| 每个数据库的最小 DTU 选项 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 |
| 每个数据库的最大 DTU 选项 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 |
| 每个数据库的最大存储空间 (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
|||||||

<sup>1</sup> 请参阅 [SQL 数据库定价选项](https://azure.cn/pricing/details/sql-database/)，详细了解由于任何预配的额外存储导致的额外成本。

<sup>2</sup> 如需了解任何单个数据库的最大并发辅助角色数（请求数），请参阅[单一数据库资源限制](resource-limits-vcore-single-databases.md)。 例如，如果弹性池使用 Gen5 且每个数据库的最大 vCore 数设置为 2，则最大并发辅助角色数目值为 200。  如果每个数据库的最大 vCore 数设置为 0.5，则最大并发辅助角色数目值为 50，因为 Gen5 上每个 vCore 的最大并发辅助角色数为 100。 对于每个数据库的最大 vCore 设置小于 1 个 vCore 或更少的其他情况，最大并发辅助角色数会相应重新缩放。

如果使用了弹性池的所有 DTU，那么池中的每个数据库会接收相同数量的资源来处理查询。 SQL 数据库服务通过确保相等的计算时间片段，在数据库之间提供资源共享的公平性。 弹性池资源共享公平性是在将每个数据库的 DTU 最小值设为非零值时，对另外为每个数据库保证的任意资源量的补充。

> [!NOTE]
> 有关 `tempdb` 限制，请参阅 [tempdb 限制](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database?view=sql-server-2017#tempdb-database-in-sql-database)。

### <a name="database-properties-for-pooled-databases"></a>共用数据库的数据库属性

下表介绍了共用数据库的属性。

| 属性 | 说明 |
|:--- |:--- |
| 每个数据库的最大 eDTU 数 |根据池中其他数据库的 eDTU 使用率，池中任何数据库可以使用的 eDTU 的最大数目。 每个数据库的 eDTU 上限并不是数据库的资源保障。 这是应用于池中所有数据库的全局设置。 将每个数据库的最大 eDTU 数设置得足够高，以处理数据库使用高峰情况。 因为池通常会假定数据库存在热使用模式和冷使用模式，在这些模式中并非所有数据库同时处于高峰使用状态，所以预期会存在某种程度的过量使用情况。 例如，假设每个数据库的高峰使用量为 20 个 eDTU，并且池中 100 个数据库仅有 20% 同时处于高峰使用中。 如果将每个数据库的 eDTU 最大值设为 20 个 eDTU，则可以认为超量 5 倍使用该池是合理的，并且将每个池的 eDTU 数设为 400。 |
| 每个数据库的最小 eDTU 数 |池中任何数据库可以保证的 eDTU 最小数目。 这是应用于池中所有数据库的全局设置。 每个数据库的最小 eDTU 可能设为 0，这也是默认值。 此属性值可以设置为介于 0 和每个数据库的平均 eDTU 使用量之间的任意值。 池中数据库的数目和每个数据库的 eDTU 下限的积不能超过每个池的 eDTU 数。 例如，如果一个池有 20 个数据库，每个数据库的 eDTU 最小值设为 10 个 eDTU，则池的 eDTU 数目必须大于或等于 200 个 eDTU。 |
| 每个数据库的最大存储 |用户为池中的数据库设置的最大数据库大小。 但是，共用数据库共享已分配的池存储。 即使每个数据库的总存储空间上限设置为大于池的可用总存储空间，所有数据库实际使用的总空间也不能超出可用的池限制。 最大数据库大小是指数据文件的最大大小，不包括日志文件使用的空间。 |
|||

## <a name="next-steps"></a>后续步骤

* 有关单一数据库的 vCore 资源限制，请参阅[使用 vCore 购买模型的单一数据库的资源限制](resource-limits-vcore-single-databases.md)
* 有关单一数据库的 DTU 资源限制，请参阅[使用 DTU 购买模型的单一数据库的资源限制](resource-limits-dtu-single-databases.md)
* 有关弹性池的 vCore 资源限制，请参阅[使用 vCore 购买模型的弹性池的资源限制](resource-limits-vcore-elastic-pools.md)
* 有关 Azure SQL 托管实例中的托管实例的资源限制，请参阅 [SQL 托管实例资源限制](../managed-instance/resource-limits.md)。
* 有关常规 Azure 限制的相关信息，请参阅 [Azure 订阅和服务限制、配额和约束](../../azure-resource-manager/management/azure-subscription-service-limits.md)。
* 有关逻辑 SQL 服务器上的资源限制的信息，请参阅[逻辑 SQL 服务器资源限制概述](resource-limits-logical-server.md)了解有关服务器级别和订阅级别限制的信息。
