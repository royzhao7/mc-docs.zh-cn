---
title: 显示查询结果缓存 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 .show 查询结果缓存。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: amitof
ms.service: data-explorer
ms.topic: reference
origin.date: 06/16/2020
ms.date: 08/18/2020
ms.openlocfilehash: 9beb84018d5844bc638f7c51d98872f67716f3f3
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88516068"
---
# <a name="show-database-cache-query_results"></a>.show database cache query_results

返回一个表，显示针对上下文数据库的[查询结果缓存](../query/query-results-cache.md)有关的统计信息。

**语法**

`.show database query results cache`

**输出**
 
|输出参数 |类型 |说明 
|---|---|---
|NodeId|`string`|群集节点的标识符。
|命中数  |`long`|缓存命中次数。
|未命中数  |`long`|缓存失误次数。
|CacheCapacityInBytes |`long` |缓存容量（以字节为单位）。
|UsedBytes  |`long` |缓存使用的空间。
|计数  |`long`| 缓存中存储的唯一查询结果的数量。
