---
title: PersonName 预生成实体 - LUIS
titleSuffix: Azure Cognitive Services
description: 本文包含了语言理解 (LUIS) 中的 personName 预构建实体信息。
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
origin.date: 05/07/2019
ms.date: 12/04/2019
ms.author: v-lingwu
ms.openlocfilehash: b75311770067f0b663c6eefc9741f4bf11fb55e7
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74884926"
---
# <a name="personname-prebuilt-entity-for-a-luis-app"></a>LUIS 应用的 PersonName 预生成实体
预构建 personName 实体会检测人的姓名。 此实体已定型，因此不需要将包含 personName 的陈述示例添加到应用程序意向中。 英语和中文[区域性](luis-reference-prebuilt-entities.md)支持 personName 实体。

## <a name="resolution-for-personname-entity"></a>解析 personName 实体

查询返回以下实体对象：

`Is Jill Jones in Cairo?`


#### <a name="v3-response"></a>[V3 响应](#tab/V3)


以下 JSON 的 `verbose` 参数设置为 `false`：

```json
"entities": {
    "personName": [
        "Jill Jones"
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 详细响应](#tab/V3-verbose)
以下 JSON 的 `verbose` 参数设置为 `true`：

```json
"entities": {
    "personName": [
        "Jill Jones"
    ],
    "$instance": {
        "personName": [
            {
                "type": "builtin.personName",
                "text": "Jill Jones",
                "startIndex": 3,
                "length": 10,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
    }
}
```
#### <a name="v2-response"></a>[V2 响应](#tab/V2)

以下示例显示了 builtin.personName 实体的解析  。

```json
"entities": [
{
    "entity": "Jill Jones",
    "type": "builtin.personName",
    "startIndex": 3,
    "endIndex": 12
}
]
```

* * * 

## <a name="next-steps"></a>后续步骤

详细了解 [V3 预测终结点](luis-migration-api-v3.md)。

了解[电子邮件](luis-reference-prebuilt-email.md)、[数字](luis-reference-prebuilt-number.md)和[序号](luis-reference-prebuilt-ordinal.md)实体。 
