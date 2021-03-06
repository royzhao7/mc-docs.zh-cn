---
title: 将 Internet 域指向流量管理器 - Azure 流量管理器
description: 本文将帮助将公司域名指向流量管理器域名。
services: traffic-manager
author: rockboyfor
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 10/11/2016
ms.date: 02/24/2020
ms.author: v-yeche
ms.openlocfilehash: a01df20cf19bfa4e006a0fc0c9542af162848003
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77653205"
---
# <a name="point-a-company-internet-domain-to-an-azure-traffic-manager-domain"></a>将公司 Internet 域指向 Azure 流量管理器域

创建流量管理器配置文件时，Azure 会自动为该配置文件分配一个 DNS 名称。 若要使用 DNS 区域中的某个名称，请创建一条映射到流量管理器配置文件域名的 CNAME DNS 记录。 可以在流量管理器配置文件的“配置”页上的“常规”  部分中找到流量管理器域名。

例如，若要将名称 `www.contoso.com` 指向流量管理器 DNS 名称 `contoso.trafficmanager.cn`，请创建以下 DNS 资源记录：

    www.contoso.com IN CNAME contoso.trafficmanager.cn

对 www\.contoso.com 发出的所有流量请求都会定向到 contoso.trafficmanager.cn。

> [!IMPORTANT]
> 无法将第二级域（例如 *contoso.com*）指向流量管理器域。 DNS 协议标准不允许对二级域名使用 CNAME 记录。

## <a name="next-steps"></a>后续步骤

* [流量管理器路由方法](traffic-manager-routing-methods.md)
* [流量管理器 - 禁用、启用或删除配置文件](disable-enable-or-delete-a-profile.md)
* [流量管理器 - 禁用或启用终结点](disable-or-enable-an-endpoint.md)

<!--Update_Description: update meta properties-->