---
title: ipv4_is_match() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 ipv4_is_match()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/24/2020
ms.date: 08/18/2020
ms.openlocfilehash: 13012dca196f4fd99e33fbd12de4e9e33028fcf0
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88516046"
---
# <a name="ipv4_is_match"></a>ipv4_is_match()

匹配两个 IPv4 字符串。 分析并比较两个 IPv4 字符串，同时考虑根据参数前缀和可选的 `PrefixMask` 参数计算出的组合 IP 前缀掩码。

```kusto
ipv4_is_match("127.0.0.1", "127.0.0.1") == true
ipv4_is_match('192.168.1.1', '192.168.1.255') == false
ipv4_is_match('192.168.1.1/24', '192.168.1.255/24') == true
ipv4_is_match('192.168.1.1', '192.168.1.255', 24) == true
```

## <a name="syntax"></a>语法

`ipv4_is_match(`*Expr1*`, `*Expr2*`[ ,`*PrefixMask*`])`

## <a name="arguments"></a>参数

* Expr1、Expr2：表示 IPv4 地址的字符串表达式。 可以使用 [IP 前缀表示法](#ip-prefix-notation)对 IPv4 字符串进行掩码操作。
* PrefixMask：从 0 到 32 的整数，表示所考虑的最有效位的数目。

## <a name="ip-prefix-notation"></a>IP 前缀表示法

IP 地址可通过使用斜杠 (`/`) 字符的 `IP-prefix notation` 进行定义。 斜杠 (`/`) 左边的 IP 地址是基本 IP 地址。 斜杠 (`/`) 右边的数字（1 到 32）是网络掩码中连续位的数目。 

例如，192.168.2.0/24 将具有关联的网络/子网掩码，其中包含 24 个连续位或点分十进制格式的 255.255.255.0。

## <a name="returns"></a>返回

* `true`：如果第一个 IPv4 字符串参数的长表示形式等于第二个 IPv4 字符串参数。
*  `false`：其他情况。
* `null`：如果两个 IPv4 字符串之一转换不成功。

## <a name="examples"></a>示例

### <a name="ipv4-comparison-equality---ip-prefix-notation-specified-inside-the-ipv4-strings"></a>IPv4 比较相等 - IPv4 字符串中指定的 IP 前缀表示法

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
datatable(ip1_string:string, ip2_string:string)
[
 '192.168.1.0',    '192.168.1.0',       // Equal IPs
 '192.168.1.1/24', '192.168.1.255',     // 24 bit IP-prefix is used for comparison
 '192.168.1.1',    '192.168.1.255/24',  // 24 bit IP-prefix is used for comparison
 '192.168.1.1/30', '192.168.1.255/24',  // 24 bit IP-prefix is used for comparison
]
| extend result = ipv4_is_match(ip1_string, ip2_string)
```

|ip1_string|ip2_string|result|
|---|---|---|
|192.168.1.0|192.168.1.0|1|
|192.168.1.1/24|192.168.1.255|1|
|192.168.1.1|192.168.1.255/24|1|
|192.168.1.1/30|192.168.1.255/24|1|

### <a name="ipv4-comparison-equality---ip-prefix-notation-specified-inside-the-ipv4-strings-and-an-additional-argument-of-the-ipv4_is_match-function"></a>IPv4 比较相等 - IP 前缀表示法在 IPv4 字符串中指定，作为 `ipv4_is_match()` 函数的附加参数

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
datatable(ip1_string:string, ip2_string:string, prefix:long)
[
 '192.168.1.1',    '192.168.1.0',   31, // 31 bit IP-prefix is used for comparison
 '192.168.1.1/24', '192.168.1.255', 31, // 24 bit IP-prefix is used for comparison
 '192.168.1.1',    '192.168.1.255', 24, // 24 bit IP-prefix is used for comparison
]
| extend result = ipv4_is_match(ip1_string, ip2_string, prefix)
```

|ip1_string|ip2_string|前缀|result|
|---|---|---|---|
|192.168.1.1|192.168.1.0|31|1|
|192.168.1.1/24|192.168.1.255|31|1|
|192.168.1.1|192.168.1.255|24|1|
