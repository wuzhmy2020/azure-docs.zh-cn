---
title: Azure Cosmos DB 查询语言中的数学函数
description: 了解 Azure Cosmos DB 中的数学函数，以便基于作为参数提供的输入值执行计算，并返回数值。
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: a04867d356aaf2f55dbe900d2e35b42f74206851
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "74873261"
---
# <a name="mathematical-functions-azure-cosmos-db"></a>数学函数 (Azure Cosmos DB)  

每个数学函数均执行一个计算，基于作为参数提供的输出值，并返回数值。

可以运行以下示例所示的查询：

```sql
    SELECT VALUE ABS(-4)
```

结果为：

```json
    [4]
```

## <a name="functions"></a>函数

以下支持的内置数学函数通常基于输入参数执行计算，并返回数值表达式。
  
||||  
|-|-|-|  
|[Abs](sql-query-abs.md)|[ACOS](sql-query-acos.md)|[阿辛](sql-query-asin.md)|  
|[阿坦](sql-query-atan.md)|[ATN2](sql-query-atn2.md)|[CEILING](sql-query-ceiling.md)|  
|[COS](sql-query-cos.md)|[COT](sql-query-cot.md)|[DEGREES](sql-query-degrees.md)|  
|[EXP](sql-query-exp.md)|[FLOOR](sql-query-floor.md)|[LOG](sql-query-log.md)|  
|[日志10](sql-query-log10.md)|[PI](sql-query-pi.md)|[POWER](sql-query-power.md)|  
|[RADIANS](sql-query-radians.md)|[兰德](sql-query-rand.md)|[轮](sql-query-round.md)|
|[SIGN](sql-query-sign.md)|[罪](sql-query-sin.md)|[SQRT](sql-query-sqrt.md)|
|[广场](sql-query-square.md)|[TAN](sql-query-tan.md)|[TRUNC](sql-query-trunc.md)||  
  
除 RAND 以外的所有数学函数都为确定性函数。 这意味着在每次使用特定的输入值集调用这些函数时，它们都将返回相同的结果。

## <a name="next-steps"></a>后续步骤

- [系统功能 Azure 宇宙 DB](sql-query-system-functions.md)
- [Azure 宇宙 DB 简介](introduction.md)
- [用户定义的函数](sql-query-udfs.md)
- [集 料](sql-query-aggregates.md)
