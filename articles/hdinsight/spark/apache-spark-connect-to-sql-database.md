---
title: 使用 Apache Spark 将数据读取和写入 Azure SQL 数据库
description: 了解如何在 HDInsight Spark 群集和 Azure SQL 数据库之间建立连接，以便将数据读取、写入数据和将数据流式传输到 SQL 数据库中
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/05/2020
ms.openlocfilehash: 4e0c1626582297aa7d80cbbd4241b6f81e314f8f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "78927457"
---
# <a name="use-hdinsight-spark-cluster-to-read-and-write-data-to-azure-sql-database"></a>使用 HDInsight Spark 群集将数据读取和写入 Azure SQL 数据库

了解如何将 Azure HDInsight 中的 Apache Spark 群集与 Azure SQL 数据库连接，然后读取、写入和流式传输数据到 SQL 数据库中。 本文中的说明使用[Jupyter 笔记本](https://jupyter.org/)运行 Scala 代码段。 但是，可以在 Scala 或 Python 中创建独立的应用程序，然后执行相同的任务。

## <a name="prerequisites"></a>先决条件

* Azure HDInsight Spark 群集。  遵照[在 HDInsight 中创建 Apache Spark 群集](apache-spark-jupyter-spark-sql.md)中的说明。

* Azure SQL 数据库。 按照创建 Azure [SQL 数据库](../../sql-database/sql-database-get-started-portal.md)的说明进行操作。 确保使用示例 **AdventureWorksLT** 架构和数据创建数据库。 另外，请确保创建服务器级防火墙规则，以允许客户端的 IP 地址访问服务器上的 SQL 数据库。 同一篇文章中提供了有关添加防火墙规则的说明。 创建 Azure SQL 数据库后，请确保保持以下值方便。 从 Spark 群集连接到数据库时需要这些值。

    * 托管 Azure SQL 数据库的服务器名称。
    * Azure SQL 数据库名称。
    * Azure SQL 数据库管理员用户名/密码。

* SQL Server Management Studio (SSMS)。 遵照[使用 SSMS 连接和查询数据](../../sql-database/sql-database-connect-query-ssms.md)中的说明。

## <a name="create-a-jupyter-notebook"></a>创建 Jupyter Notebook

首先创建与 Spark 群集关联的[犹太笔记本](https://jupyter.org/)。 到时要使用此 Notebook 来运行本文中所用的代码片段。

1. 从 [Azure 门户网站](https://portal.azure.com/)打开群集。
1. 选择右侧**群集仪表板**下方的 **Jupyter notebook**。  如果没有看到“群集仪表板”****，请从左侧菜单中选择“概述”****。 出现提示时，请输入群集的管理员凭据。

    ![阿帕奇火花上的朱派笔记本](./media/apache-spark-connect-to-sql-database/hdinsight-spark-cluster-dashboard-jupyter-notebook.png "Spark 上的 Jupyter Notebook")

   > [!NOTE]  
   > 也可以在浏览器中打开以下 URL 来访问 Spark 群集中的 Jupyter Notebook。 将**CLUSTERNAME**替换为群集的名称：
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

1. 在 Jupyter Notebook 的右上角，依次单击“新建”、“Spark”创建 Scala 笔记本。******** HDInsight Spark 群集上的 Jupyter Notebook 还提供适用于 Python2 应用程序的 **PySpark** 内核，以及适用于 Python3 应用程序的 **PySpark3** 内核。 本文将会创建 Scala 笔记本。

    ![Spark 上适用于 Jupyter Notebook 的内核](./media/apache-spark-connect-to-sql-database/kernel-jupyter-notebook-on-spark.png "Spark 上适用于 Jupyter Notebook 的内核")

    有关内核的详细信息，请参阅[将 Jupyter Notebook 内核与 HDInsight 中的 Apache Spark 群集配合使用](apache-spark-jupyter-notebook-kernels.md)。

   > [!NOTE]  
   > 本文使用 Spark (Scala) 内核，因为目前只有 Scala 和 Java 才支持将数据从 Spark 流式传输到 SQL 数据库。 尽管可以使用 Python 在 SQL中 读取和写入数据，但为了保持一致，本文将使用 Scala 执行所有三个操作。

1. 这会打开默认名称为“无标题”的新笔记本。**** 单击笔记本名称，然后输入所选的名称。

    ![提供笔记本的名称](./media/apache-spark-connect-to-sql-database/hdinsight-spark-jupyter-notebook-name.png "提供笔记本的名称")

现在可以开始创建应用程序。

## <a name="read-data-from-azure-sql-database"></a>从 Azure SQL 数据库读取数据

在本部分，我们要从 AdventureWorks 数据库中的某个表（例如 **SalesLT.Address**）读取数据。

1. 在新的 Jupyter 笔记本中，在代码单元格中粘贴以下代码段，并将占位符值替换为 Azure SQL 数据库的值。

       // Declare the values for your Azure SQL database

       val jdbcUsername = "<SQL DB ADMIN USER>"
       val jdbcPassword = "<SQL DB ADMIN PWD>"
       val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
       val jdbcPort = 1433
       val jdbcDatabase ="<AZURE SQL DB NAME>"

    按 **SHIFT + ENTER** 运行代码单元。  

1. 使用以下代码片段生成可传递给 Spark 数据帧 API 的 JDBC URL。 此代码创建一个 `Properties` 对象来保存参数。 粘贴代码单元中的代码片段，然后按 **SHIFT + ENTER** 运行。

       import java.util.Properties

       val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=60;"
       val connectionProperties = new Properties()
       connectionProperties.put("user", s"${jdbcUsername}")
       connectionProperties.put("password", s"${jdbcPassword}")         

1. 使用下面的代码段创建具有 Azure SQL 数据库中表中的数据框。 在此代码段中，我们使用作为`SalesLT.Address` **AdventureWorksLT**数据库一部分提供的表。 粘贴代码单元中的代码片段，然后按 **SHIFT + ENTER** 运行。

       val sqlTableDF = spark.read.jdbc(jdbc_url, "SalesLT.Address", connectionProperties)

1. 现在，可以针对数据帧执行操作，例如，获取数据架构：

       sqlTableDF.printSchema

    会显示如下所示的输出：

    ![架构输出](./media/apache-spark-connect-to-sql-database/read-from-sql-schema-output.png "架构输出")

1. 还可以执行其他操作，例如检索前 10 行。

       sqlTableDF.show(10)

1. 或者，从数据集检索特定的列。

       sqlTableDF.select("AddressLine1", "City").show(10)

## <a name="write-data-into-azure-sql-database"></a>将数据写入 Azure SQL 数据库

在本节中，我们使用群集上可用的示例 CSV 文件在 Azure SQL 数据库中创建表，并使用数据填充表。 该示例 CSV 文件 (**HVAC.csv**) 已在所有 HDInsight 群集上提供，路径为 `HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv`。

1. 在新的 Jupyter 笔记本中，在代码单元格中粘贴以下代码段，并将占位符值替换为 Azure SQL 数据库的值。

       // Declare the values for your Azure SQL database

       val jdbcUsername = "<SQL DB ADMIN USER>"
       val jdbcPassword = "<SQL DB ADMIN PWD>"
       val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
       val jdbcPort = 1433
       val jdbcDatabase ="<AZURE SQL DB NAME>"

    按 **SHIFT + ENTER** 运行代码单元。  

1. 以下代码片段生成可传递给 Spark 数据帧 API 的 JDBC URL。 此代码创建一个 `Properties` 对象来保存参数。 粘贴代码单元中的代码片段，然后按 **SHIFT + ENTER** 运行。

       import java.util.Properties

       val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=60;"
       val connectionProperties = new Properties()
       connectionProperties.put("user", s"${jdbcUsername}")
       connectionProperties.put("password", s"${jdbcPassword}")

1. 使用以下代码片段提取 HVAC.csv 中的数据架构，并使用该架构将 CSV 中的数据载入数据帧 `readDf`。 粘贴代码单元中的代码片段，然后按 **SHIFT + ENTER** 运行。

       val userSchema = spark.read.option("header", "true").csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv").schema
       val readDf = spark.read.format("csv").schema(userSchema).load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

1. 使用 `readDf` 数据帧创建临时表 `temphvactable`。 然后使用该临时表创建 Hive 表 `hvactable_hive`。

       readDf.createOrReplaceTempView("temphvactable")
       spark.sql("create table hvactable_hive as select * from temphvactable")

1. 最后，使用配置单元表在 Azure SQL 数据库中创建表。 以下代码段在`hvactable`Azure SQL 数据库中创建。

       spark.table("hvactable_hive").write.jdbc(jdbc_url, "hvactable", connectionProperties)

1. 使用 SSMS 连接到 Azure SQL 数据库，并验证`dbo.hvactable`您是否在那里看到。

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 通过提供连接详细信息（如下图所示）启动 SSMS 并连接到 Azure SQL 数据库。

    ![使用 SSMS1 连接到 SQL 数据库](./media/apache-spark-connect-to-sql-database/connect-to-sql-db-ssms.png "使用 SSMS1 连接到 SQL 数据库")

    b.保留“数据库类型”设置，即设置为“共享”。 从**对象资源管理器**中，展开 Azure SQL 数据库和表节点以查看创建的**dbo.hvac。"**

    ![使用 SSMS2 连接到 SQL 数据库](./media/apache-spark-connect-to-sql-database/connect-to-sql-db-ssms-locate-table.png "使用 SSMS2 连接到 SQL 数据库")

1. 在 SSMS 中运行查询以查看表中的列。

    ```sql
    SELECT * from hvactable
    ```

## <a name="stream-data-into-azure-sql-database"></a>将数据流式传输到 Azure SQL 数据库

在本节中，我们将数据流式传输到`hvactable`上一节中已在 Azure SQL 数据库中创建的。

1. 作为第一步，请确保 中没有 记录。 `hvactable` 使用 SSMS 针对该表运行以下查询。

    ```sql
    TRUNCATE TABLE [dbo].[hvactable]
    ```

1. 在 HDInsight Spark 群集上创建一个新的 Jupyter Notebook。 在代码单元中粘贴以下代码片段，然后按 **SHIFT + ENTER**：

       import org.apache.spark.sql._
       import org.apache.spark.sql.types._
       import org.apache.spark.sql.functions._
       import org.apache.spark.sql.streaming._
       import java.sql.{Connection,DriverManager,ResultSet}

1. 我们将数据从**HVAC.csv**流式传输到`hvactable`。 HVAC.csv 文件在 群集上`/HdiSamples/HdiSamples/SensorSampleData/HVAC/`可用。 在以下代码片段中，我们先获取要流式传输的数据的架构。 然后，使用该架构创建流数据帧。 粘贴代码单元中的代码片段，然后按 **SHIFT + ENTER** 运行。

       val userSchema = spark.read.option("header", "true").csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv").schema
       val readStreamDf = spark.readStream.schema(userSchema).csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/") 
       readStreamDf.printSchema

1. 输出显示 **HVAC.csv** 的架构。 也有`hvactable`相同的架构。 输出列出表中的列。

    ![hdinsight 阿帕奇火花架构表](./media/apache-spark-connect-to-sql-database/hdinsight-schema-table.png "表的架构")

1. 最后，使用以下代码段从 HVAC.csv 读取数据并将其流式传输到`hvactable`Azure SQL 数据库中。 将代码段粘贴到代码单元格中，将占位符值替换为 Azure SQL 数据库的值，然后按**SHIFT + ENTER**运行。

       val WriteToSQLQuery  = readStreamDf.writeStream.foreach(new ForeachWriter[Row] {
          var connection:java.sql.Connection = _
          var statement:java.sql.Statement = _
          
          val jdbcUsername = "<SQL DB ADMIN USER>"
          val jdbcPassword = "<SQL DB ADMIN PWD>"
          val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
          val jdbcPort = 1433
          val jdbcDatabase ="<AZURE SQL DB NAME>"
          val driver = "com.microsoft.sqlserver.jdbc.SQLServerDriver"
          val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
  
         def open(partitionId: Long, version: Long):Boolean = {
           Class.forName(driver)
           connection = DriverManager.getConnection(jdbc_url, jdbcUsername, jdbcPassword)
           statement = connection.createStatement
           true
         }
  
         def process(value: Row): Unit = {
           val Date  = value(0)
           val Time = value(1)
           val TargetTemp = value(2)
           val ActualTemp = value(3)
           val System = value(4)
           val SystemAge = value(5)
           val BuildingID = value(6)  
    
           val valueStr = "'" + Date + "'," + "'" + Time + "'," + "'" + TargetTemp + "'," + "'" + ActualTemp + "'," + "'" + System + "'," + "'" + SystemAge + "'," + "'" + BuildingID + "'"
           statement.execute("INSERT INTO " + "dbo.hvactable" + " VALUES (" + valueStr + ")")   
           }

         def close(errorOrNull: Throwable): Unit = {
            connection.close
          }
         })
        
         var streamingQuery = WriteToSQLQuery.start()

1. 通过在 SQL 服务器管理工作室 （SSMS） 中运行以下查询，验证数据是否流式传输到 。 `hvactable` 每次运行该查询时，它都会显示表中的行数已递增。

    ```sql
    SELECT COUNT(*) FROM hvactable
    ```

## <a name="next-steps"></a>后续步骤

* [使用 HDInsight Spark 群集分析 Data Lake Storage 中的数据](apache-spark-use-with-data-lake-store.md)
* [使用事件中心处理结构化流事件](apache-spark-eventhub-structured-streaming.md)
* [将 Apache Spark 结构化流式处理与 Apache Kafka on HDInsight 配合使用](../hdinsight-apache-kafka-spark-structured-streaming.md)
