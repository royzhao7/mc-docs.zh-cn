---
title: Azure 高级存储：Linux VM 的性能设计
description: 使用 Azure 高级 SSD 托管磁盘设计高性能应用程序。 高级存储为 Azure 虚拟机上运行的 I/O 密集型工作负载提供高性能、低延迟的磁盘支持。
author: Johnnytechn
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.date: 06/05/2020
ms.author: v-johya
ms.subservice: disks
ms.openlocfilehash: a75b6ab15a4d44ecb92c9b71ae253ae7fb6b3cbf
ms.sourcegitcommit: 372899a2a21794e631eda1c6a11b4fd5c38751d2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "85852084"
---
# <a name="design-for-performance-on-linux-vms"></a>Linux VM 的性能设计

[!INCLUDE [virtual-machines-common-premium-storage-introduction](../../../includes/virtual-machines-common-premium-storage-introduction.md)]

> [!NOTE]
> 有时，显示为磁盘性能问题的原因实际上是网络瓶颈。 在这些情况下，应优化[网络性能](../../virtual-network/virtual-network-optimize-network-bandwidth.md)。
>
> 如果你希望对磁盘进行基准测试，请参阅我们的关于[磁盘基准测试](disks-benchmarks.md)的文章。
>
> 如果 VM 支持加速网络，则应确保它已启用。 如果未启用，则可以在 [Windows](../../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms) 和 [Linux](../../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms) 上已部署的 VM 上启用它。

如果不熟悉高级存储，请在开始之前先阅读[为 IaaS VM 选择 Azure 磁盘类型](disks-types.md)和[高级页 blob 存储帐户的可伸缩性目标](../../storage/blobs/scalability-targets-premium-page-blobs.md)。


[!INCLUDE [virtual-machines-common-premium-storage-performance.md](../../../includes/virtual-machines-common-premium-storage-performance.md)]

如果你希望对磁盘进行基准测试，请参阅我们的关于[磁盘基准测试](disks-benchmarks.md)的文章。

了解有关可用磁盘类型的详细信息：[选择磁盘类型](disks-types.md)  

SQL Server 用户请阅读有关 SQL Server 性能最佳实践的文章：

* [Azure 虚拟机中 SQL Server 的性能最佳做法](../windows/sql/virtual-machines-windows-sql-performance.md)
* [Azure 高级存储为 Azure VM 中的 SQL Server 提供最高性能](https://cloudblogs.microsoft.com/sqlserver/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm/)

<!-- Update_Description: update meta properties, wording update, update link -->
