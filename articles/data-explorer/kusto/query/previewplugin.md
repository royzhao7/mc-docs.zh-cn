---
title: 预览插件 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍了 Azure 数据资源管理器中的预览插件。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/18/2020
ms.openlocfilehash: 464fb7143f6fff5b58ba26de1c5b77f558669d32
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88516086"
---
# <a name="preview-plugin"></a>preview 插件

返回一个表，其中包含输入记录集中的指定行数以及输入记录集中的记录总数。

```kusto
T | evaluate preview(50)
```

## <a name="syntax"></a>语法

`T` `|` `evaluate` `preview(` *NumberOfRows* `)`

## <a name="returns"></a>返回

`preview` 插件返回两个结果表：
* 最多包含指定行数的表。
  例如，上面的示例查询相当于运行 `T | take 50`。
* 只含有一行/列的表，用于保存输入记录集中的记录数。
  例如，上面的示例查询相当于运行 `T | count`。

> [!TIP]
> 如果 `evaluate` 前面有一个包含复杂筛选器的表格式源，或者引用大多数源表列的筛选器，则最好使用 [`materialize`](materializefunction.md) 函数。 例如：

```kusto
let MaterializedT = materialize(T);
MaterializedT | evaluate preview(50)
```