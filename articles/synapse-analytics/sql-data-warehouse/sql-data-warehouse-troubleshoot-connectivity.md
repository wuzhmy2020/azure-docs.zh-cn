---
title: 连接性疑难解答
description: 在 Synapse SQL 池中排除连接故障。
services: synapse-analytics
author: anumjs
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/27/2019
ms.author: anjangsh
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 08fb0a6675d18370482abe9b1d7b9a0d9ee5c364
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633007"
---
# <a name="troubleshooting-connectivity-issues"></a>排查连接问题

本文列出了有关连接到 SQL 分析数据库的常见故障排除技术。

- [检查服务可用性](sql-data-warehouse-troubleshoot-connectivity.md#check-service-availability)
- [检查已暂停的操作或正在缩放的操作](sql-data-warehouse-troubleshoot-connectivity.md#check-for-paused-or-scaling-operation)
- [检查防火墙设置](sql-data-warehouse-troubleshoot-connectivity.md#check-your-firewall-settings)
- [检查 VNet/服务终结点设置](sql-data-warehouse-troubleshoot-connectivity.md#check-your-vnetservice-endpoint-settings)
- [检查最新的驱动程序](sql-data-warehouse-troubleshoot-connectivity.md#check-for-the-latest-drivers)
- [检查连接字符串](sql-data-warehouse-troubleshoot-connectivity.md#check-your-connection-string)
- [间歇性连接问题](sql-data-warehouse-troubleshoot-connectivity.md#intermittent-connection-issues)
- [常见错误消息](sql-data-warehouse-troubleshoot-connectivity.md#common-error-messages)

## <a name="check-service-availability"></a>检查服务可用性

查看服务是否可用。 在 Azure 门户中，转到尝试连接的 Synapse SQL 池。 在左侧 TOC 面板中，单击“诊断并解决问题”。****

![选择资源运行状况](./media/sql-data-warehouse-troubleshoot-connectivity/diagnostics-link.png)

此处将显示 Synapse SQL 池的状态。 如果服务未显示为“可用”，**** 请查看更多的步骤。

![服务可用](./media/sql-data-warehouse-troubleshoot-connectivity/resource-health.png)

如果"资源"运行状况显示 Synapse SQL 池实例已暂停或缩放，请按照该指南继续实例。

![服务暂停](./media/sql-data-warehouse-troubleshoot-connectivity/resource-health-pausing.png) 可以在此处找到有关资源运行状况的其他信息。

## <a name="check-for-paused-or-scaling-operation"></a>检查暂停的或正在缩放的操作

检查门户以查看 Synapse SQL 池实例是否暂停或缩放。

![服务已暂停](./media/sql-data-warehouse-troubleshoot-connectivity/overview-paused.png)

如果看到服务已暂停或正在缩放，请查看一下这是否是你的维护计划。 在 Synapse SQL 池*概述*的门户上，您将看到当选的维护计划。

![维护计划概览](./media/sql-data-warehouse-troubleshoot-connectivity/overview-maintance-schedule.png)

否则，请与 IT 管理员联系，验证该维护是否为计划的事件。 要恢复 SQL 分析实例，请按照[以下步骤操作](pause-and-resume-compute-portal.md)。

## <a name="check-your-firewall-settings"></a>检查防火墙设置

SQL 分析数据库通过端口 1433 进行通信。如果尝试从企业网络内部进行连接，则该网络的防火墙可能不允许经端口 1433 的出站流量。 在这种情况下，你无法连接到 Azure SQL 数据库服务器，除非 IT 部门打开了端口 1433。 可从[此处](../../sql-database/sql-database-firewall-configure.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#create-and-manage-ip-firewall-rules)找到有关防火墙配置的更多信息。

## <a name="check-your-vnetservice-endpoint-settings"></a>检查 VNet/服务终结点设置

如果收到 40914 和 40615 错误，请参阅[此处的错误说明和解决方法](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#errors-40914-and-40615)。

## <a name="check-for-the-latest-drivers"></a>检查最新驱动程序

### <a name="software"></a>软件

检查以确保您使用最新的工具连接到 Synapse SQL 池：

- SSMS
- Azure Data Studio
- SQL Server Data Tools (Visual Studio)

### <a name="drivers"></a>驱动程序

请进行检查，确保使用最新的驱动程序版本。使用旧版驱动程序可能会导致意外行为，因为旧驱动程序可能不支持新功能。

- [ODBC](/sql/connect/odbc/download-odbc-driver-for-sql-server)
- [JDBC](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server)
- [OLE DB](/sql/connect/oledb/download-oledb-driver-for-sql-server)
- [PHP](/sql/connect/php/download-drivers-php-sql-server)

## <a name="check-your-connection-string"></a>检查连接字符串

进行检查以确保正确设置了连接字符串。  下面是一些示例。  可在[此处找到有关连接字符串](/sql-data-warehouse/sql-data-warehouse-connection-strings.md)的更多信息。

ADO.NET 连接字符串

```csharp
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

ODBC 连接字符串

```csharp
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

PHP 连接字符串

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

JDBC 连接字符串

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="intermittent-connection-issues"></a>间歇性连接问题

查看是否遇到服务器上负载过重、有大量排队请求的情况。 您可能需要扩展 Synapse SQL 池以寻找其他资源。

## <a name="common-error-messages"></a>常见错误消息

对于 40914 和 40615 错误，请参阅[此处的错误说明和解决方法](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#errors-40914-and-40615)。

## <a name="still-having-connectivity-issues"></a>仍有连接问题？

创建[支持票证](/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket.md)，以便工程团队可以为您提供支持。
