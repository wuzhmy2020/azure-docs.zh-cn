---
title: 教程：使用 Power BI 分析 Azure HDInsight Apache Spark 数据
description: 教程 - 使用 Microsoft Power BI 直观显示存储在 HDInsight 群集中的 Apache Spark 数据
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.custom: hdinsightactive,mvc
ms.date: 03/02/2020
ms.openlocfilehash: d7330225ecbdc6715847821a47c140a3c2b8d1b9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "78251954"
---
# <a name="tutorial-analyze-apache-spark-data-using-power-bi-in-hdinsight"></a>教程：使用 Power BI 在 HDInsight 中分析 Apache Spark 数据

在本教程中，了解如何使用 [Microsoft Power BI](https://powerbi.microsoft.com/) 在 Azure HDInsight 中可视化 [Apache Spark](https://azure.microsoft.com/services/hdinsight/) 群集中的数据。

在本教程中，你将了解如何执行以下操作：
> [!div class="checklist"]
> * 使用 Power BI 可视化 Spark 数据

如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>先决条件

* 完成文章[教程：在 Azure HDInsight 中的 Apache Spark 群集上加载数据并运行查询](./apache-spark-load-data-run-query.md)。

* [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/)。

* 可选：[Power BI 试用版订阅](https://app.powerbi.com/signupredirect?pbi_source=web)。

## <a name="verify-the-data"></a>验证数据

[上一教程](apache-spark-load-data-run-query.md)中创建的 [Jupyter Notebook](https://jupyter.org/) 包含用于创建 `hvac` 表的代码。 此表基于在所有 HDInsight Spark 群集上均可用的 CSV 文件（位于 `\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv`）。 使用以下过程验证数据。

1. 从 Jupyter 笔记本中，粘贴以下代码，然后按 Shift+Enter  。 该代码验证表是否存在。

    ```PySpark
    %%sql
    SHOW TABLES
    ```

    输出如下所示：

    ![在 Spark 中显示表](./media/apache-spark-use-bi-tools/apache-spark-show-tables.png)

    如果在开始本教程之前关闭笔记本，这会清除 `hvactemptable`，使其不包含在输出中。  只有元存储中存储的 Hive 表（由“isTemporary”列下的“False”表示）可从 BI 工具中进行访问   。 在本教程中，将连接到创建的 hvac 表  。

2. 将以下代码粘贴到空白单元格中，然后按 Shift+Enter  。 该代码验证表中的数据。

    ```PySpark
    %%sql
    SELECT * FROM hvac LIMIT 10
    ```

    输出如下所示：

    ![在 Spark 中显示 hvac 表中的行](./media/apache-spark-use-bi-tools/apache-spark-select-limit.png)

3. 请在 Notebook 的“文件”菜单中选择“关闭并停止”   。 关闭笔记本以释放资源。

## <a name="visualize-the-data"></a>可视化数据

在本部分，将使用 Power BI 从 Spark 群集数据创建可视化效果、报表和仪表板。

### <a name="create-a-report-in-power-bi-desktop"></a>在 Power BI Desktop 中创建报表

使用 Spark 的前几步是连接到 Power BI Desktop 中的集群，从集群中加载数据，并根据此数据创建基本的可视化效果。

> [!NOTE]  
> 本文中所示的连接器当前处于预览状态。 通过 [Power BI 社区](https://community.powerbi.com/)站点或 [Power BI Ideas](https://ideas.powerbi.com/forums/265200-power-bi-ideas)（Power BI 想法）提供反馈。

1. 打开 Power BI Desktop。 关闭启动初始屏幕（如果已打开）。

2. 从“主页”  选项卡，导航到“获取数据”   > “更多...”  。

    ![从 HDInsight Apache Spark 将数据导入 Power BI Desktop](./media/apache-spark-use-bi-tools/hdinsight-spark-power-bi-desktop-get-data.png "从 Apache Spark BI 将数据导入 Power BI")

3. 在搜索框中输入 `Spark`，选择 Azure HDInsight Spark，然后选择“连接”   。

    ![从 Apache Spark BI 将数据导入 Power BI](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "从 Apache Spark BI 将数据导入 Power BI")

4. 在“服务器”  文本框中输入群集 URL（形式为 `mysparkcluster.azurehdinsight.net`）。

5. 在“数据连接模式”  下，择“DirectQuery”  。 然后选择“确定”。 

    Spark 可使用任一数据连接模式。 若使用 DirectQuery，则报告中会显示更改，但不刷新整个数据集。 如果导入数据，则必须刷新数据集以查看更改。 若要深入了解如何以及何时使用 DirectQuery，请参阅[在 Power BI 中使用 DirectQuery](https://powerbi.microsoft.com/documentation/powerbi-desktop-directquery-about/)。

6. 输入 HDInsight 登录帐户信息，然后选择“连接”  。 默认帐户名为 admin  。

7. 选择 `hvac` 表，稍后即可看到数据预览，然后选择“加载”  。

    ![Spark 群集用户名和密码](./media/apache-spark-use-bi-tools/apache-spark-bi-select-table.png "Spark 群集用户名和密码")

    Power BI Desktop 拥有连接到 Spark 群集和从 `hvac` 表中加载数据所需的信息。 该表及表中各列显示在“字段”窗格中  。

8. 可视化每栋建筑物的目标温度与实际温度之间的差异：

    1. 在“可视化”窗格中，选择“分区图”   。

    2. 将“BuildingID”字段拖到“轴”，并将“ActualTemp”和“TargetTemp”字段拖至“值”      。

        ![添加值列](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "添加值列")

        关系图如下所示：

        ![面积图总和](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph-sum.png "面积图总和")

        默认情况下，视觉效果会显示 **ActualTemp** 和 **TargetTemp** 的总和。 选择“可视化效果”窗格中 ActualTemp 和 TragetTemp 旁边的向下键，可看到已选中“总和”    。

    3. 选择“可视化效果”窗格中 ActualTemp 和 TragetTemp 旁边的向下键，选择“平均值”以获得每栋建筑物的实际温度与目标温度的平均值    。

        ![值的平均值](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "值的平均值")

        数据可视化效果应与屏幕截图中类似。 在视觉效果上移动光标可获取相关数据的工具提示。

        ![面积图](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "面积图")

9. 导航到“文件”   > “保存”  ，为文件输入名称 `BuildingTemperature`，然后选择“保存”  。

### <a name="publish-the-report-to-the-power-bi-service-optional"></a>将报表发布到 Power BI 服务（可选）

Power BI 服务允许在整个组织中共享报表和仪表板。 在本部分，首先发布数据集和报表。 然后，将报表固定到仪表板。 仪表板通常用于关注报表中的数据子集。 报表中只有一个可视化效果，但完成这些步骤仍然很有用。

1. 打开 Power BI Desktop。

1. 在“主页”选项卡上，选择“发布”   。

    ![从 Power BI Desktop 发布](./media/apache-spark-use-bi-tools/apache-spark-bi-publish.png "从 Power BI Desktop 发布")

1. 选择要将数据集和报表发布到的工作区，然后选择“选择”  。 在下图中，默认选择“我的工作区”  。

    ![选择要将数据集和报表发布到的工作区](./media/apache-spark-use-bi-tools/apache-spark-bi-select-workspace.png "选择要将数据集和报表发布到的工作区")

1. 发布成功后，选择“在 Power BI 中打开 BuildingTemperature.pbix”  。

    ![发布成功，请单击以输入凭据](./media/apache-spark-use-bi-tools/apache-spark-bi-publish-success.png "发布成功，请单击以输入凭据")

1. 在 Power BI 服务中，选择“输入凭据”  。

    ![在 Power BI 服务中输入凭据](./media/apache-spark-use-bi-tools/apache-spark-bi-enter-credentials.png "在 Power BI 服务中输入凭据")

1. 选择“编辑凭据”  。

    ![在 Power BI 服务中编辑凭据](./media/apache-spark-use-bi-tools/apache-spark-bi-edit-credentials.png "在 Power BI 服务中编辑凭据")

1. 输入 HDInsight 登录帐户信息，然后选择“登录”  。 默认帐户名为 admin  。

    ![登录到 Spark 群集](./media/apache-spark-use-bi-tools/apache-spark-bi-sign-in.png "登录到 Spark 群集")

1. 在左侧窗格中，转到“工作区” > “我的工作区” > “报表”，然后选择“BuildingTemperature”     。

    ![左侧窗格中报表下列出的报表](./media/apache-spark-use-bi-tools/apache-spark-bi-service-left-pane.png "左侧窗格中报表下列出的报表")

    还可在左窗格中的“数据集”下找到 BuildingTemperature   。

    现可在 Power BI 服务中使用 Power BI Desktop 中创建的视觉对象。

1. 将光标悬停在可视化效果上，然后选择右上角的“固定”图标。

    ![Power BI 服务中的报表](./media/apache-spark-use-bi-tools/apache-spark-bi-service-report.png "Power BI 服务中的报表")

1. 选择“新建仪表板”，输入名称 `Building temperature`，再选择“固定”  。

    ![固定到新仪表板](./media/apache-spark-use-bi-tools/apache-spark-bi-pin-dashboard.png "固定到新仪表板")

1. 在报表中，选择“转到仪表板”  。

视觉对象已固定到仪表板 - 可在报表中添加其他视觉对象，并将其固定在同一仪表板上。 有关报表和仪表板的详细信息，请参阅 [Power BI 中的报表](https://powerbi.microsoft.com/documentation/powerbi-service-reports/)和 [Power BI 中的仪表板](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/)。

## <a name="clean-up-resources"></a>清理资源

完成本教程后，可以删除群集。 有了 HDInsight，便可以将数据存储在 Azure 存储中，因此可以在群集不用时安全地删除群集。 此外，还需要为 HDInsight 群集付费，即使不用也是如此。 由于群集费用数倍于存储空间费用，因此在群集不用时删除群集可以节省费用。

若要删除群集，请参阅[使用浏览器、PowerShell 或 Azure CLI 删除 HDInsight 群集](../hdinsight-delete-cluster.md)。

## <a name="next-steps"></a>后续步骤

在本教程中，了解了如何使用 [Microsoft Power BI](https://powerbi.microsoft.com/) 在 Azure HDInsight 中可视化 [Apache Spark](https://azure.microsoft.com/services/hdinsight/) 群集中的数据。 请继续学习下一篇文章，试着创建一个机器学习应用程序。

> [!div class="nextstepaction"]
> [创建机器学习应用程序](./apache-spark-ipython-notebook-machine-learning.md)