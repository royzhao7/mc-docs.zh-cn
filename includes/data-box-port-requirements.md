---
author: WenJason
ms.service: databox
ms.subservice: pod
ms.topic: include
origin.date: 07/11/2019
ms.date: 07/22/2019
ms.author: v-jay
ms.openlocfilehash: 04e1b85932d599de6c7a1be99e0a48df09e77578
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "68298140"
---
| 端口号。| 入或出 | 端口范围| 必选| 说明 |   |
|--------|-----|-----|-----------|----------|-----------|
| TCP 80 (HTTP)|In|LAN|是|此端口用于通过 HTTP 连接到 Data Box Blog 存储 REST API。 如果未连接到 REST API，则此端口会通过 8443 自动重定向到本地 Web UI。 |
| TCP 443 (HTTPS)|In|LAN|是|此端口用于通过 HTTPS 连接到 Data Box Blog 存储 REST API。 如果未连接到 REST API，则此端口会通过 8443 自动重定向到本地 Web UI。 |
| TCP 8443 (HTTPS-Alt)|In|LAN|是|这是 HTTPS 的替代端口，在连接到本地 Web UI 进行设备管理时使用。 |
| TCP 445 (SMB)|出/入|LAN|某些情况下<br>请参阅说明|仅当通过 SMB 连接时，才需要此端口。 |
| TCP 2049 (NFS)|出/入|LAN|某些情况下<br>请参阅说明|仅当通过 NFS 连接时，才需要此端口。 |
| TCP 111 (NFS)|出/入|LAN|某些情况下<br>请参阅说明|此端口用于 rpcbind/端口映射，仅当通过 NFS 连接时才需要。  |
