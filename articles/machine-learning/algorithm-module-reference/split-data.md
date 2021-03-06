---
title: 拆分数据：模块引用
titleSuffix: Azure Machine Learning
description: 了解如何在 Azure 机器学习中使用“拆分数据”模块将一个数据集拆分为两个非重复集。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: 9eba6f2c47629b708dde4a5a2888b76dbd24b4e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79455887"
---
# <a name="split-data-module"></a>“拆分数据”模块

本文介绍 Azure 机器学习设计器（预览版）中的一个模块。

使用拆分数据模块将数据集划分为两个不同的集。

当您需要将数据分离到定型和测试集中时，此模块非常有用。 您还可以自定义数据的划分方式。 某些选项支持数据随机化。 其他类型是针对特定数据类型或型号定制的。

## <a name="configure-the-module"></a>配置模块

> [!TIP]
> 在选择拆分模式之前，请阅读所有选项以确定所需的拆分类型。
> 如果更改拆分模式，可能会重置所有其他选项。

1. 在设计器中将“拆分数据”模块添加到管道****。 你可在“示例和拆分”类别中的“数据转换”下找到此模块********。

1. **拆分模式**：根据您拥有的数据类型和如何划分数据，选择以下模式之一。 每个拆分模式都有不同的选项。

   - **拆分行**：如果只想将数据分成两部分，请使用此选项。 您可以指定要放入每个拆分中的数据百分比。 默认情况下，数据被除以 50/50。

     还可随机化每组中的行选定内容，并使用分层采样。 在分层采样中，必须选择希望在两个结果数据集中平等分配值的单个数据列。  

   - **正则表达式拆分**：如果要通过测试值的单个列来划分数据集，请选择此选项。

     例如，如果要分析情绪，可以检查文本字段中是否存在特定产品名称。 然后，可以将数据集划分为具有目标产品名称的行和行，而不带目标产品名称。

   - **相对表达式拆分**：每当要将条件应用于数字列时，请使用此选项。 该数字可以是日期/时间字段、包含年龄或美元金额的列，甚至一个百分比。 例如，您可能希望根据项目的成本、按年龄范围对人员进行分组或按日历日期划分数据来划分数据集。

### <a name="split-rows"></a>拆分行

1. 将[拆分数据](./split-data.md)模块添加到设计器中的管道中，并连接要拆分的数据集。
  
1. 对于**拆分模式**，选择**拆分行**。 

1. **第一个输出数据集中的行数：** 使用此选项确定将进入第一个（左侧）输出中的行数。 所有其他行将进入第二个（右侧）输出。

   比率表示发送到第一个输出数据集的行的百分比，因此必须输入介于 0 和 1 之间的小数。
     
   例如，如果输入**0.75**作为值，数据集将拆分为 75/25。 在此拆分中，75% 的行将发送到第一个输出数据集。 剩余的 25% 将发送到第二个输出数据集。
  
1. 如果要将数据选定内容随机化为两个组，请选择“随机化拆分”选项****。 在创建训练和测试数据集时，这是首选选项。

1. **随机种子**：输入非负整数值以启动要使用的实例的伪随机序列。 此默认种子用于生成随机数字的所有模块。 

   指定种子使结果可重现。 如果需要重复执行拆分操作的结果，则应指定随机数生成器的种子。 否则，默认情况下随机种子设置为**0**，这意味着初始种子值是从系统时钟获取的。 因此，每次执行拆分时，数据的分布都会略有不同。 

1. **分层拆分**：将此选项设置为**True，** 以确保两个输出数据集包含*地层列*或*分层键列*中值的代表性样本。 

   使用分层采样，将对数据进行划分，以便每个输出数据集大约获取每个目标值的相同百分比。 例如，您可能希望确保培训和测试集在结果或其他列（如性别）方面大致平衡。

1. 提交管道。


## <a name="select-a-regular-expression"></a>选择正则表达式

1. 将[拆分数据](./split-data.md)模块添加到管道中，并将其作为输入连接到要拆分的数据集。  
  
1. 对于“拆分模式”，请选择“正则表达式拆分”********。

1. 在 **"正则表达式"** 框中，输入有效的正则表达式。 
  
   正则表达式应遵循正则表达式的 Python 语法。

1. 提交管道。

   基于提供的正则表达式，数据集分为两组行：具有与表达式匹配的值的行和所有剩余行。 

以下示例演示如何使用**正则表达式**选项划分数据集。 

### <a name="single-whole-word"></a>单个整字 

本示例将包含列`Gryphon``Text`中文本的所有行放入第一个数据集。 它将其他行放入**拆分数据**的第二个输出中。

```text
    \"Text" Gryphon  
```

### <a name="substring"></a>Substring

本示例查找数据集第二列中的任何位置的指定字符串。 此处的索引值 1 表示该位置。 匹配区分大小写。

```text
(\1) ^[a-f]
```

第一个结果数据集包含索引列以以下字符之一`a`开头的所有行： `b`、 `c`、 `d`、 `e` `f`、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 所有其他行都定向到第二个输出。

## <a name="select-a-relative-expression"></a>选择相对表达式

1. 将[拆分数据](./split-data.md)模块添加到管道中，并将其作为输入连接到要拆分的数据集。
  
1. 对于**拆分模式**，选择**相对表达式**。
  
1. 在 **"关系"表达式**框中，输入对单个列执行比较操作的表达式。

   对于**数字列**：
   - 该列包含任何数字数据类型的数字，包括日期和时间数据类型。
   - 表达式最多可以引用一个列名称。
   - 使用 ampersand 字符`&`， FOR 操作。 将管道字符 、`|`用于 OR 操作。
   - 支持以下运算符： `<`、 `>` `<=`、 `>=`、 `==` `!=`、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、
   - 不能通过使用 对`(``)`操作进行分组。
   
   对于**字符串列**：
   - 支持以下运算符： `==`. `!=`

1. 提交管道。

   表达式将数据集分成两组行：值满足条件的行和所有剩余行。

以下示例演示如何使用**拆分数据**模块中的 **"相对表达式"** 选项来划分数据集。  

### <a name="calendar-year"></a>日历年

常见情形是按年份分割数据集。 以下表达式选择列`Year`中的值大于`2010`的所有行。

```text
\"Year" > 2010
```

日期表达式必须考虑数据列中包含的所有日期部分。 数据列中的日期格式必须一致。 

例如，在使用格式`mmddyyyy`的日期列中，表达式应如下所示：

```text
\"Date" > 1/1/2010
```

### <a name="column-index"></a>列索引

以下表达式演示如何使用列索引选择数据集的第一列中值小于或等于 30、但不等于 20 的所有行。

```text
(\0)<=30 & !=20
```


## <a name="next-steps"></a>后续步骤

参阅 Azure 机器学习[可用的模块集](module-reference.md)。 
