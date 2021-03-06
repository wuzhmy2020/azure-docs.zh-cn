---
title: 定义数据类型
description: 在 Synapse SQL 池中定义表数据类型的建议。
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
ms.openlocfilehash: 5b3d24232c26bb4d483d360eb593bc361190ccfe
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631235"
---
# <a name="table-data-types-in-synapse-sql-pool"></a>Synapse SQL 池中的表数据类型
本文中包括用于在 SQL 池中定义表数据类型的建议。 

## <a name="supported-data-types"></a>支持的数据类型

SQL 池支持最常用的数据类型。 有关受支持数据类型的列表，请参阅 CREATE TABLE 语句中的[数据类型](/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes)。 

## <a name="minimize-row-length"></a>最大限度缩短行长度
最大限度减小数据类型大小可缩短行长度，获得更好的查询性能。 使用适用于数据的最小数据类型。 

- 避免使用较大默认长度定义字符列。 例如，如果最长的值是 25 个字符，则将列定义为 VARCHAR(25)。 
- 避免在仅需要 VARCHAR 时使用 [NVARCHAR](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql)。
- 尽可能使用 NVARCHAR(4000) 或 VARCHAR(8000)，而非 NVARCHAR(MAX) 或 VARCHAR(MAX)。

如果使用 PolyBase 外部表加载表，则表行的定义长度不能超过 1 MB。 当数据长度可变的行超过 1 MB 时，可使用 BCP 而不是 PolyBase 加载行。

## <a name="identify-unsupported-data-types"></a>识别不支持的数据类型
如果要从另一个 SQL 数据库迁移数据库，可能会发现 SQL 池中不支持的数据类型。 使用以下查询在现有 SQL 架构中发现不支持的数据类型：

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','xml')
 AND  y.[is_user_defined] = 1;
```


## <a name="workarounds-for-unsupported-data-types"></a><a name="unsupported-data-types"></a>对不受支持的数据类型的解决方法

下面的列表显示了 SQL 池不支持的数据类型，并为不支持的数据类型提供了有用的替代方法。

| 不支持的数据类型 | 解决方法 |
| --- | --- |
| [几何](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [地理](/sql/t-sql/spatial-geography/spatial-types-geography) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [层次结构](/sql/t-sql/data-types/hierarchyid-data-type-method-reference) |[恩瓦尔查尔](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql)（4000） |
| [image](/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [text](/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql) |
| [ntext](/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql) |
| [sql_variant](/sql/t-sql/data-types/sql-variant-transact-sql) |将列拆分成多个强类型化列。 |
| [表](/sql/t-sql/data-types/table-transact-sql) |转换成暂时表。 |
| [timestamp](/sql/t-sql/data-types/date-and-time-types) |重写代码来使用 [datetime2](/sql/t-sql/data-types/datetime2-transact-sql) 和 [CURRENT_TIMESTAMP](/sql/t-sql/functions/current-timestamp-transact-sql) 函数。 只有常量作为默认值支持，因此current_timestamp不能定义为默认约束。 如果需要从时间戳类型列迁移行版本值，请使用[BINARY](/sql/t-sql/data-types/binary-and-varbinary-transact-sql)（8） 或[VARBINARY](/sql/t-sql/data-types/binary-and-varbinary-transact-sql)（8） 来表示"无 NULL"或 NULL 行版本值。 |
| [xml](/sql/t-sql/xml/xml-transact-sql) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql) |
| [用户定义类型 (user-defined type)](/sql/relational-databases/native-client/features/using-user-defined-types) |尽可能转换回本机数据类型。 |
| 默认值 | 默认值仅支持文本和常量。 |


## <a name="next-steps"></a>后续步骤

有关开发表的详细信息，请参阅[表概述](sql-data-warehouse-tables-overview.md)。
