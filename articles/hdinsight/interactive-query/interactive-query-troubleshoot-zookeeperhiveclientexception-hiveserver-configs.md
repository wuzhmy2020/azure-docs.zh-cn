---
title: 阿帕奇·希泽佩林解释器错误 - Azure HDInsight
description: Apache Zeppelin Hive JDBC 解释器指向 Azure HDInsight 中的错误 URL
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: 20309babb9ece0ae20e7442543b0d378f9a51060
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "75895064"
---
# <a name="scenario-apache-hive-zeppelin-interpreter-gives-a-zookeeper-error-in-azure-hdinsight"></a>场景：Apache Hive Zepelin 解释器在 Azure HDInsight 中给出动物园管理员错误

本文介绍在 Azure HDInsight 群集中使用交互式查询组件时出现的问题的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

在 Apache Hive LLAP 群集上，Zeppelin 解释器在尝试执行查询时给出以下错误消息：

```
java.sql.SQLException: org.apache.hive.jdbc.ZooKeeperHiveClientException: Unable to read HiveServer2 configs from ZooKeeper
```

## <a name="cause"></a>原因

Zeppelin Hive JDBC 解释器指向错误的 URL。

## <a name="resolution"></a>解决方法

1. 导航到 Hive 组件摘要并将“Hive JDBC URL”复制到剪贴板。

1. 导航到 `https://clustername.azurehdinsight.net/zeppelin/#/interpreter`

1. 编辑 JDBC 设置：将 hive.url 值更新为步骤 1 中复制的 Hive JDBC URL

1. 保存，然后重试查询

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道之一获取更多支持：

* 通过 [Azure 社区支持](https://azure.microsoft.com/support/community/)获取 Azure 专家的解答。

* 与[@AzureSupport](https://twitter.com/azuresupport)- 正式的 Microsoft Azure 帐户连接 Azure 社区，以将 Azure 社区连接到正确的资源：答案、支持和专家，从而改善客户体验。

* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”****，或打开“帮助 + 支持”**** 中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)。 Microsoft Azure 订阅包含对订阅管理和计费支持的访问权限，并且通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
