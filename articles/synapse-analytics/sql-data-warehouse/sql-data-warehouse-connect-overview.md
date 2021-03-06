---
title: 连接到 Synapse SQL 池
description: 连接到 SQL 池。
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 70dd1ae883ee1f44672dccb802c0e770d7676440
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619090"
---
# <a name="connect-to-synapse-sql-pool"></a>连接到 Synapse SQL 池
连接到 SQL 池。

## <a name="find-your-server-name"></a>查找服务器名称
以下示例中的服务器名称sqlpoolservername.database.windows.net。 若要查找完全限定的服务器名称，请执行以下操作：

1. 转到 [Azure 门户](https://portal.azure.com)。
2. 单击**Azure 同步分析**。
3. 单击要连接到的 SQL 池。
4. 找到完整的服务器名称。
   
    ![完整服务器名称](media/sql-data-warehouse-connect-overview/server-connect.PNG)

## <a name="supported-drivers-and-connection-strings"></a>支持的驱动程序和连接字符串
SQL 池[ADO.NET](https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx)支持[ADO.NET、ODBC、PHP](https://msdn.microsoft.com/library/jj730314.aspx)和[JDBC。](https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx) [PHP](https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396) 单击前述的某个驱动程序，查找最新版本和文档。 

若要自动生成通过 Azure 门户使用的驱动程序的连接字符串，请单击前述示例中的“显示数据库连接字符串”****。 下面也是一些示例，说明了每个驱动程序的连接字符串的样式。

> [!NOTE]
> 请考虑将连接超时值设置为 300 秒，以便连接可以经受住短时间内不可用。
> 
> 

### <a name="adonet-connection-string-example"></a>ADO.NET 连接字符串示例
```csharp
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a>ODBC 连接字符串示例
```csharp
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a>PHP 连接字符串示例
```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a>JDBC 连接字符串示例
```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="connection-settings"></a>连接设置
SQL 池在连接和对象创建期间标准化某些设置。 这些设置不能重写，其中包括：

| 数据库设置 | 值 |
|:--- |:--- |
| [ANSI_NULLS](https://msdn.microsoft.com/library/ms188048.aspx) |ON |
| [QUOTED_IDENTIFIERS](https://msdn.microsoft.com/library/ms174393.aspx) |ON |
| [DATEFORMAT](https://msdn.microsoft.com/library/ms189491.aspx) |mdy |
| [DATEFIRST](https://msdn.microsoft.com/library/ms181598.aspx) |7 |

## <a name="next-steps"></a>后续步骤
若要使用 Visual Studio 进行连接和查询，请参阅 [Query with Visual Studio](sql-data-warehouse-query-visual-studio.md)（使用 Visual Studio 进行查询）。 要了解有关身份验证选项的详细信息，请参阅[Azure 同步分析的身份验证](sql-data-warehouse-authentication.md)。

