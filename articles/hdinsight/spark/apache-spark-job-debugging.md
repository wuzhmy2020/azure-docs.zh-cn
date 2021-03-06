---
title: 调试 Azure HDInsight 中运行的 Apache Spark 作业
description: 使用 YARN UI、Spark UI 和 Spark History Server 来跟踪和调试 Azure HDInsight 中的 Spark 群集上运行的作业
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/29/2019
ms.openlocfilehash: bcf2f97e855126c86dbb1d74cd430704e2af3af1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "75932136"
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a>调试 Azure HDInsight 中运行的 Apache Spark 作业

在本文中，将了解如何使用 [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) UI、Spark UI 和 Spark 历史记录服务器来跟踪和调试 HDInsight 群集上运行的 [Apache Spark](https://spark.apache.org/) 作业。 将使用 Spark 群集中提供的笔记本启动 Spark 作业，相关信息请参阅“机器学习：使用 MLLib 对食物检测数据进行预测分析”****。 也可以执行以下步骤来跟踪使用任何其他方法（例如 spark-submit）提交的应用程序****。

如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>先决条件

* HDInsight 上的 Apache Spark 群集。 有关说明，请参阅[在 Azure HDInsight 中创建 Apache Spark 群集](apache-spark-jupyter-spark-sql.md)。

* 应该已开始运行笔记本，相关信息请参阅 **[Machine learning: Predictive analysis on food inspection data using MLLib](apache-spark-machine-learning-mllib-ipython.md)**（机器学习：使用 MLLib 对食品检测数据进行预测分析）。 有关如何运行此笔记本的说明，请单击以下链接。  

## <a name="track-an-application-in-the-yarn-ui"></a>在 YARN UI 中跟踪应用程序

1. 启动 YARN UI。 在**群集仪表板**下选择**纱线**。

    ![Azure 门户启动 YARN UI](./media/apache-spark-job-debugging/launch-apache-yarn-ui.png)

   > [!TIP]  
   > 或者，也可以从 Ambari UI 启动 YARN UI。 要启动 Ambari UI，请在**群集仪表板**下选择**Ambari 主页**。 从 Ambari UI，导航到**YARN** > **快速链接**>活动资源管理器>**资源管理器 UI**。

2. 由于 Spark 作业是使用 Jupyter 笔记本启动的，因此应用程序的名称为 **remotesparkmagics**（这是从笔记本启动的所有应用程序的名称）。 根据应用程序名称选择应用程序 ID 以获取有关作业的详细信息。 此时会启动应用程序视图。

    ![火花历史记录服务器 查找 Spark 应用程序 ID](./media/apache-spark-job-debugging/find-application-id1.png)

    对于从 Jupyter 笔记本启动的应用程序，在退出笔记本之前，其状态始终是“正在运行”****。

3. 从应用程序视图中，可以进一步深入以找到与应用程序和日志 (stdout/stderr) 关联的容器。 也可以通过单击“跟踪 URL”**** 对应的链接来启动 Spark UI，如下所示。

    ![Spark 历史记录服务器下载容器日志](./media/apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-the-spark-ui"></a>在 Spark UI 中跟踪应用程序

在 Spark UI 中，可以深入到前面启动的应用程序所产生的 Spark 作业。

1. 要启动 Spark UI，请从应用程序视图中选择针对**跟踪 URL**的链接，如上面的屏幕截图所示。 可以看到，应用程序启动的所有 Spark 作业正在 Jupyter 笔记本中运行。

    ![火花历史记录服务器作业选项卡](./media/apache-spark-job-debugging/view-apache-spark-jobs.png)

2. 选择 **"执行器**"选项卡以查看每个执行器的处理和存储信息。 还可以通过选择**线程转储**链接来检索调用堆栈。

    ![火花历史记录服务器执行器选项卡](./media/apache-spark-job-debugging/view-spark-executors.png)

3. 选择 **"阶段"** 选项卡以查看与应用程序关联的阶段。

    ![火花历史记录服务器阶段选项卡](./media/apache-spark-job-debugging/view-apache-spark-stages.png "查看 Spark 阶段")

    每个阶段可能有多个任务，可以查看这些任务的执行统计信息，如下所示。

    ![Spark 历史记录服务器阶段选项卡详细信息](./media/apache-spark-job-debugging/view-spark-stages-details.png "查看 Spark 阶段详细信息")

4. 在阶段详细信息页上，可以启动 DAG 可视化。 展开页面顶部的“DAG 可视化”**** 链接，如下所示。

    ![查看 Spark 阶段 DAG 可视化](./media/apache-spark-job-debugging/view-spark-stages-dag-visualization.png)

    DAG (Direct Aclyic Graph) 呈现了应用程序中的不同阶段。 图形中的每个蓝框表示从应用程序调用的 Spark 操作。

5. 在阶段详细信息页上，还可以启动应用程序时间线视图。 展开页面顶部的“事件时间线”**** 链接，如下所示。

    ![查看 Spark 阶段事件时间线](./media/apache-spark-job-debugging/view-spark-stages-event-timeline.png)

    此时以时间线形式显示 Spark 事件。 时间线视图提供三个级别：跨作业、作业内和阶段内。 上图中捕获了指定阶段的时间线视图。

   > [!TIP]  
   > 如果选中“启用缩放”**** 复选框，则可以在时间线视图中左右滚动。

6. Spark UI 中的其他选项卡也提供了有关 Spark 实例的有用信息。

   * 存储选项卡 - 如果应用程序创建了 RDD，则可以在"存储"选项卡中找到有关这些操作的信息。
   * 环境选项卡 - 此选项卡提供有关 Spark 实例的有用信息，例如：
     * Scala 版本
     * 与群集关联的事件日志目录
     * 应用程序的执行器核心数
     * 等。

## <a name="find-information-about-completed-jobs-using-the-spark-history-server"></a>使用 Spark History Server 查找有关已完成的作业的信息

完成某个作业后，有关该作业的信息将保存在 Spark History Server 中。

1. 要启动 Spark 历史记录服务器，请从 **"概述"** 页中选择 **"群集仪表板**"下的**Spark 历史记录服务器**。

    ![Azure 门户启动 Spark 历史记录服务器](./media/apache-spark-job-debugging/launch-spark-history-server.png "启动 Spark History Server1")

   > [!TIP]  
   > 或者，也可以从 Ambari UI 启动 Spark History Server UI。 要启动 Ambari UI，请从"概述"边栏选项卡中选择"**群集仪表板**"下的**Ambari 主页**。 从 Ambari UI，导航到**Spark2** > **快速链接** > **Spark2 历史记录服务器 UI**。

2. 随后会看到已列出所有已完成的应用程序。 有关详细信息，请选择应用程序 ID 以向下钻取到应用程序中。

    ![Spark 历史记录服务器已完成的应用程序](./media/apache-spark-job-debugging/view-completed-applications.png "启动 Spark History Server2")

## <a name="see-also"></a>请参阅

* [管理 Azure HDInsight 中 Apache Spark 群集的资源](apache-spark-resource-manager.md)
* [使用扩展的 Spark 历史记录服务器调试 Apache Spark 作业](apache-azure-spark-history-server.md)

### <a name="for-data-analysts"></a>适用于数据分析师

* [Apache Spark 和机器学习：使用 HDInsight 中的 Spark 结合 HVAC 数据分析建筑物温度](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark 和机器学习：使用 HDInsight 中的 Spark 预测食品检查结果](apache-spark-machine-learning-mllib-ipython.md)
* [使用 HDInsight 中的 Apache Spark 分析网站日志](apache-spark-custom-library-website-log-analysis.md)
* [使用 HDInsight 中的 Apache Spark 执行 Application Insight 遥测数据分析](apache-spark-analyze-application-insight-logs.md)


### <a name="for-spark-developers"></a>适用于 Spark 开发人员

* [使用 Scala 创建独立的应用程序](apache-spark-create-standalone-application.md)
* [使用 Apache Livy 在 Apache Spark 群集中远程运行作业](apache-spark-livy-rest-interface.md)
* [使用适用于 IntelliJ IDEA 的 HDInsight 工具插件创建和提交 Spark Scala 应用程序](apache-spark-intellij-tool-plugin.md)
* [使用适用于 IntelliJ IDEA 的 HDInsight 工具插件远程调试 Apache Spark 应用程序](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [在 HDInsight 上的 Apache Spark 群集中使用 Apache Zeppelin 笔记本](apache-spark-zeppelin-notebook.md)
* [在 HDInsight 的 Apache Spark 群集中可用于 Jupyter Notebook 的内核](apache-spark-jupyter-notebook-kernels.md)
* [将外部包与 Jupyter 笔记本配合使用](apache-spark-jupyter-notebook-use-external-packages.md)
* [Install Jupyter on your computer and connect to an HDInsight Spark cluster（在计算机上安装 Jupyter 并连接到 HDInsight Spark 群集）](apache-spark-jupyter-notebook-install-locally.md)
