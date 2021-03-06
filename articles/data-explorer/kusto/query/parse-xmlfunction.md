---
title: parse_xml() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 parse_xml()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 0b62a5825b08038c74bf372b10068978b688e485
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841351"
---
# <a name="parse_xml"></a>parse_xml()

将 `string` 解释为 XML 值，将值转换为 JSON，然后将值作为 `dynamic` 返回。

**语法**

`parse_xml(`*xml*`)`

**参数**

* xml：`string` 类型的表达式，表示 XML 格式的值。

**返回**

一个 [dynamic](./scalar-data-types/dynamic.md) 类型的对象（取决于 xml 的值）；如果 XML 格式无效，则其类型为 null。

使用 [xml2json](https://github.com/Cheedoong/xml2json) 库将 XML 转换为 JSON。

完成转换的方式如下：

XML                                |JSON                                            |Access
-----------------------------------|------------------------------------------------|--------------         
`<e/>`                             | { "e": null }                                  | o.e
`<e>text</e>`                      | { "e": "text" }                                | o.e
`<e name="value" />`               | { "e":{"@name": "value"} }                     | o.e["@name"]
`<e name="value">text</e>`         | { "e": { "@name": "value", "#text": "text" } } | o.e["@name"] o.e["#text"]
`<e> <a>text</a> <b>text</b> </e>` | { "e": { "a": "text", "b": "text" } }          | o.e.a o.e.b
`<e> <a>text</a> <a>text</a> </e>` | { "e": { "a": ["text", "text"] } }             | o.e.a[0] o.e.a[1]
`<e> text <a>text</a> </e>`        | { "e": { "#text": "text", "a": "text" } }      | 1`o.e["#text"] o.e.a

**备注**

* `parse_xml` 的最大输入 `string` 长度为 128 KB。 更长的字符串解释会生成 null 对象 
* 只会转换元素节点、属性和文本节点。 将跳过其他所有内容
 
**示例**

在以下示例中，如果 `context_custom_metrics` 是类似如下的 `string`： 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<duration>
    <value>118.0</value>
    <count>5.0</count>
    <min>100.0</min>
    <max>150.0</max>
    <stdDev>0.0</stdDev>
    <sampledValue>118.0</sampledValue>
    <sum>118.0</sum>
</duration>
```

则下面的 CSL 片段会将 XML 转换为以下 JSON：

```json
{
    "duration": {
        "value": 118.0,
        "count": 5.0,
        "min": 100.0,
        "max": 150.0,
        "stdDev": 0.0,
        "sampledValue": 118.0,
        "sum": 118.0
    }
}
```

此外还会检索对象中 `duration` 槽的值，并从中检索两个槽：`duration.value` 和 `duration.min`（分别为 `118.0` 和 `110.0`）。

```kusto
T
| extend d=parse_xml(context_custom_metrics) 
| extend duration_value=d.duration.value, duration_min=d["duration"]["min"]
```