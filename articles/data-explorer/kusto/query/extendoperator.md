---
title: extend 运算符 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 extend 运算符。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 25cea725bbeb4851f3b93bb94ecf820d2c6c5a6e
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841771"
---
# <a name="extend-operator"></a>extend 运算符

创建计算列并将其追加到结果集中。

```kusto
T | extend duration = endTime - startTime
```

**语法**

*T* `| extend` [*ColumnName* | `(`*ColumnName*[`,` ...]`)` `=`] *Expression* [`,` ...]

**参数**

* *T*：输入表格结果集。
* ColumnName：可选。 要添加或更新的列的名称。 如果省略，则会生成名称。 如果 Expression 返回多个列，则可在括号中指定列名列表。 在这种情况下，会将指定的名称赋予 Expression 的输出列，并会删除其余的输出列（如果有）。 如果未指定列名列表，则 Expression 的所有具有所生成名称的输出列都将添加到输出中。
* *表达式：* 对输入的列进行计算。

**返回**

输入表格结果集的副本，使得：
1. 已存在于输入中的使用 `extend` 标记的列名会被删除，然后作为新的计算值进行追加。
2. 输入中不存在的使用 `extend` 标记的列名将作为其新的计算值追加。

**提示**

* `extend` 运算符会将新列添加到输入结果集中，该列没有索引。 在大多数情况下，如果将新列设置为与具有索引的现有表列完全相同，则 Kusto 可以自动使用现有索引。 但在某些复杂情况下，此传播并未完成。 在此类情况下，如果目标是重命名列，请改用 [`project-rename` 运算符](projectrenameoperator.md)。

**示例**

```kusto
Logs
| extend
    Duration = CreatedOn - CompletedOn
    , Age = now() - CreatedOn
    , IsSevere = Level == "Critical" or Level == "Error"
```

可以使用 [series_stats](series-statsfunction.md) 函数返回多个列。