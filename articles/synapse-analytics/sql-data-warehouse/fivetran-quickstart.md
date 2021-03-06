---
title: 快速入门：Fivetran 和数据仓库
description: Fivetran 和 Azure Synapse Analytics 数据仓库入门。
services: synapse-analytics
author: WenJason
manager: digimobile
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
origin.date: 10/12/2018
ms.date: 07/06/2020
ms.author: v-jay
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 94cd2ba5adf266c396a3462d2a1d59175937839b
ms.sourcegitcommit: 7ea2d04481512e185a60fa3b0f7b0761e3ed7b59
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "85845824"
---
# <a name="quickstart-fivetran-with-data-warehouse"></a>快速入门：Fivetran 和数据仓库 

本快速入门介绍了如何设置新的 Fivetran 用户，以使用通过 SQL 池预配的 Synapse Analytics 数据仓库。 本文假定你已有数据仓库。

## <a name="set-up-a-connection"></a>设置连接

1. 查找用于连接到数据仓库的完全限定的服务器名称和数据库名。
    
    如果需要有关查找此信息的帮助，请参阅[连接到数据仓库](sql-data-warehouse-connect-overview.md)。

2. 在安装向导中，选择是要直接连接数据库还是通过 SSH 隧道进行连接。

   如果选择直接连接数据库，则必须创建防火墙规则，用于允许访问。 此方法是最简单且最安全的方法。

   如果选择通过 SSH 隧道进行连接，Fivetran 会连接到网络中一个单独的服务器。 服务器提供一个连接到数据库的 SSH 隧道。 如果数据库位于虚拟网络上一个无法访问的子网中，则必须使用此方法。

3. 在服务器级别的防火墙中添加 IP 地址“52.0.2.4”，允许传入从 Fivetran 到数据仓库实例的连接。

   有关详细信息，请参阅[创建服务器级防火墙规则](create-data-warehouse-portal.md#create-a-server-level-firewall-rule)。

## <a name="set-up-user-credentials"></a>设置用户凭据

1. 使用 SQL Server Management Studio (SSMS) 或首选工具连接到数据仓库。 以服务器管理员用户身份登录。 然后，运行以下 SQL 命令，为 Fivetran 创建一个用户：

    - 在 master 数据库中： 
    
      ```sql
      CREATE LOGIN fivetran WITH PASSWORD = '<password>'; 
      ```

    - 在数据仓库数据库中：

      ```sql
      CREATE USER fivetran_user_without_login without login;
      CREATE USER fivetran FOR LOGIN fivetran;
      GRANT IMPERSONATE on USER::fivetran_user_without_login to fivetran;
      ```

2. 向 Fivetran 用户授予以下数据仓库权限：

    ```sql
    GRANT CONTROL to fivetran;
    ```

    创建数据库范围的凭据需要 CONTROL 权限，而用户在通过 PolyBase 从 Azure Blob 存储加载文件时会使用该凭据。

3. 向 Fivetran 用户添加适当的资源类。 所用资源类取决于创建列存储索引时需要的内存。 例如，与 Marketo 和 Salesforce 之类的产品集成时由于产品使用的列数较多且数据量较大，需要较高级别的资源类。 较高级别的资源类需要更多内存来创建列存储索引。

    建议使用静态资源类。 可以从 `staticrc20` 资源类着手。 资源类 `staticrc20` 为每个用户分配 200 MB，不考虑你所使用的性能级别。 如果列存储索引无法使用初始资源类级别，请提高资源类的级别。

    ```sql
    EXEC sp_addrolemember '<resource_class_name>', 'fivetran';
    ```

    有关详细信息，请阅读[内存和并发限制](memory-concurrency-limits.md)和[资源类](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md#ways-to-allocate-more-memory)。


## <a name="connect-from-fivetran"></a>从 Fivetran 连接

要从 Fivetran 帐户连接到数据仓库，请输入用于访问数据仓库的凭据： 

* 主机（服务器名称）。
* 端口。
* 数据库。
* 用户（用户名应该为 fivetran\@_server_name_，其中的 server_name 是 Azure 主机 URI _server\_name_.database.chinacloudapi.cn 的一部分）。   
* Password。
