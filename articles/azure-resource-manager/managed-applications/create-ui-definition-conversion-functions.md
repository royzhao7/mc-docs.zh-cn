---
title: 创建 UI 定义转换函数
description: 介绍在数据类型和编码之间转换值时要使用的函数。
ms.topic: conceptual
origin.date: 07/13/2020
author: rockboyfor
ms.date: 08/24/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: ef65632d84ef29bd762b67eae6a65f58464ebcf0
ms.sourcegitcommit: 2e9b16f155455cd5f0641234cfcb304a568765a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/21/2020
ms.locfileid: "88715639"
---
<!--Verify Successfully-->
# <a name="createuidefinition-conversion-functions"></a>创建 UI 定义转换函数

可以使用这些函数在 JSON 数据类型和编码之间转换值。

## <a name="bool"></a>bool

将参数转换为布尔值。 此函数支持数字、字符串和布尔类型的参数。 与 JavaScript 中的布尔值相似，`0` 或 `'false'` 以外的任何值都返回 `true`。

以下示例返回 `true`：

```json
"[bool(1)]"
```

以下示例返回 `false`：

```json
"[bool(0)]"
```

以下示例返回 `true`：

```json
"[bool(true)]"
```

以下示例返回 `true`：

```json
"[bool('true')]"
```

## <a name="decodebase64"></a>decodeBase64

将参数从 base-64 编码的字符串中解码。 此函数仅支持字符串类型的参数。

以下示例返回 `"Contoso"`：

```json
"[decodeBase64('Q29udG9zbw==')]"
```

## <a name="decodeuricomponent"></a>decodeUriComponent

将参数从 URL 编码的字符串中解码。 此函数仅支持字符串类型的参数。

以下示例返回 `"https://portal.azure.cn/"`：

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="encodebase64"></a>encodeBase64

将参数编码为 base-64 编码的字符串。 此函数仅支持字符串类型的参数。

以下示例返回 `"Q29udG9zbw=="`：

```json
"[encodeBase64('Contoso')]"
```

## <a name="encodeuricomponent"></a>encodeUriComponent

将参数编码为 URL 编码的字符串。 此函数仅支持字符串类型的参数。

以下示例返回 `"https%3A%2F%2Fportal.azure.com%2F"`：

```json
"[encodeUriComponent('https://portal.azure.cn/')]"
```

## <a name="float"></a>float

将参数转换为浮点。 此函数支持数字和字符串类型的参数。

以下示例返回 `1.0`：

```json
"[float('1.0')]"
```

以下示例返回 `2.9`：

```json
"[float(2.9)]"
```

## <a name="int"></a>int

将参数转换为整数。 此函数支持数字和字符串类型的参数。

以下示例返回 `1`：

```json
"[int('1')]"
```

以下示例返回 `2`：

```json
"[int(2.9)]"
```

## <a name="parse"></a>parse

将参数转换为本机类型。 换而言之，此函数是 `string()` 函数的反函数。 此函数仅支持字符串类型的参数。

以下示例返回 `1`：

```json
"[parse('1')]"
```

以下示例返回 `true`：

```json
"[parse('true')]"
```

以下示例返回 `[1,2,3]`：

```json
"[parse('[1,2,3]')]"
```

以下示例返回 `{"type":"webapp"}`：

```json
"[parse('{\"type\":\"webapp\"}')]"
```

## <a name="string"></a>string

将参数转换为字符串。 此函数支持所有 JSON 数据类型的参数。

以下示例返回 `"1"`：

```json
"[string(1)]"
```

以下示例返回 `"2.9"`：

```json
"[string(2.9)]"
```

以下示例返回 `"[1,2,3]"`：

```json
"[string([1,2,3])]"
```

以下示例返回 `"{"type":"webapp"}"`：

```json
"[string({\"type\":\"webapp\"})]"
```

## <a name="next-steps"></a>后续步骤

* 有关 Azure 资源管理器的简介，请参阅 [Azure 资源管理器概述](../management/overview.md)。

<!-- Update_Description: new article about create ui definition conversion functions -->
<!--NEW.date: 08/24/2020-->