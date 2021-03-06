---
title: 为 B2B 企业集成方案交换消息
description: 使用企业集成包在 Azure 逻辑应用中的贸易伙伴之间接收和发送 B2B 消息
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 02/10/2020
ms.openlocfilehash: 01b2bd464db51e255930fe83a3f4321687322275
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "77151069"
---
# <a name="receive-and-send-b2b-messages-by-using-azure-logic-apps-and-enterprise-integration-pack"></a>使用 Azure 逻辑应用和企业集成包接收和发送 B2B 消息

当您拥有定义贸易伙伴和协议的集成帐户时，您可以创建一个自动业务到企业 （B2B） 工作流，该工作流通过使用[Azure 逻辑应用](../logic-apps/logic-apps-overview.md)与企业[集成包](../logic-apps/logic-apps-enterprise-integration-overview.md)一起在贸易合作伙伴之间交换消息。 Azure 逻辑应用适用于支持 AS2、X12、EDIFACT 和罗塞塔网行业标准协议的连接器。 您还可以将这些连接器与[逻辑应用中提供的其他连接器](../connectors/apis-list.md)（例如 Salesforce 和 Office 365 Outlook）进行组合。

本文演示如何使用请求触发器创建接收 HTTP 请求的逻辑应用，使用 AS2 和 X12 操作解码消息内容，然后使用响应操作返回响应。

## <a name="prerequisites"></a>先决条件

* Azure 订阅。 如果您还没有订阅，[请注册一个免费的 Azure 帐户](https://azure.microsoft.com/free/)。

* 一个空白逻辑应用，以便您可以通过使用["请求](../connectors/connectors-native-reqres.md)触发器"创建 B2B 工作流，随后执行以下操作：

  * [AS2 解码](../logic-apps/logic-apps-enterprise-integration-as2.md)

  * [条件](../logic-apps/logic-apps-control-flow-conditional-statement.md)， 根据 AS2 解码操作成功还是失败发送[响应](../connectors/connectors-native-reqres.md)

  * [解码 X12 消息](../logic-apps/logic-apps-enterprise-integration-x12.md) 

  如果您对逻辑应用是新加入的，请查看[什么是 Azure 逻辑应用？](../logic-apps/logic-apps-overview.md)和["快速入门：创建第一个逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)"。

* 与 Azure 订阅关联并链接到逻辑应用的[集成帐户](../logic-apps/logic-apps-enterprise-integration-accounts.md)。 逻辑应用和集成帐户必须存在于同一位置或 Azure 区域中。

* 您已在集成帐户中定义的至少两个[贸易伙伴](../logic-apps/logic-apps-enterprise-integration-partners.md)以及针对这些合作伙伴的[AS2 和 X12 协议](logic-apps-enterprise-integration-agreements.md)。

## <a name="add-request-trigger"></a>添加请求触发器

此示例在 Azure 门户中使用逻辑应用设计器，但可以在可视化工作室中按照逻辑应用设计器的类似步骤操作。

1. 在[Azure 门户](https://portal.azure.com)中，在逻辑应用设计器中打开空白逻辑应用。

1. 在搜索框中，输入`when a http request`，然后选择 **"何时收到 HTTP 请求**以用作触发器"。

   ![选择"请求触发器"以启动逻辑应用工作流](./media/logic-apps-enterprise-integration-b2b/select-http-request-trigger.png)

1. 将**请求正文 JSON 架构**框留空，因为 X12 消息是平面文件。

   ![将"请求正文 JSON 架构"留空](./media/logic-apps-enterprise-integration-b2b/receive-trigger-message-body-json-schema.png)

1. 完成后，请在设计器工具栏上选择“保存”****。

   此步骤将生成**HTTP POST URL，** 用于发送触发逻辑应用的请求。 若要复制此 URL，请选择 URL 旁边的复制图标。

   ![为请求触发器为接收呼叫而生成的 URL](./media/logic-apps-enterprise-integration-b2b/generated-url-request-trigger.png)

## <a name="add-as2-decode-action"></a>添加 AS2 解码操作

现在添加要使用的 B2B 操作。 此示例使用 AS2 和 X12 操作。

1. 在触发器下，选择“新建步骤”。**** 要隐藏触发器详细信息，请单击触发器的标题栏。

   ![向逻辑应用工作流添加另一个步骤](./media/logic-apps-enterprise-integration-b2b/add-new-action-under-trigger.png)

1. 在搜索框中**选择操作**，输入`as2 decode`，然后选择**AS2 解码 （v2）**。

   ![查找并选择"AS2 解码 （v2）"](./media/logic-apps-enterprise-integration-b2b/add-as2-decode-action.png)

1. 对于 **"消息"解码**属性，请输入您希望 AS2 操作解码的输入，即 HTTP 请求触发器`body`接收的内容。 您可以通过多种方式将此内容指定为输入，从动态内容列表中指定或作为表达式：

   * 要从显示可用触发器输出的列表中选择，请单击"**消息"以解码**框。 显示动态内容列表后，在 **"何时收到 HTTP 请求"** 下，选择 **"正文**属性值"，例如：

     ![从触发器中选择"正文"值](./media/logic-apps-enterprise-integration-b2b/select-body-content-from-trigger.png)

   * 要输入引用触发器输出的`body`表达式，请单击"**消息"以解码**框。 显示动态内容列表后，选择 **"表达式**"。 在表达式编辑器中，在此处输入表达式，然后选择 **"确定**" ：

     `triggerOutputs()['body']`

     或者，在 **"消息解码"** 框中，直接输入此表达式：

     `@triggerBody()`

     表达式解析为**正文**令牌。

     ![从触发器解析的正文输出](./media/logic-apps-enterprise-integration-b2b/resolved-trigger-outputs-body-expression.png)

1. 对于**消息标头**属性，输入 AS2 操作所需的任何标头，由 HTTP 请求触发器接收`headers`的内容描述。

   要输入引用触发器输出的`headers`表达式，请单击 **"消息标题"** 框中。 显示动态内容列表后，选择 **"表达式**"。 在表达式编辑器中，在此处输入表达式，然后选择 **"确定**" ：

   `triggerOutputs()['Headers']`

   要使此表达式作为此令牌解析，请切换设计器和代码视图，例如：

   ![已解决的标头从触发器输出](./media/logic-apps-enterprise-integration-b2b/resolved-trigger-outputs-headers-expression.png)

## <a name="add-response-action-for-message-receipt-notification"></a>添加邮件接收通知的响应操作

要通知贸易伙伴收到该消息，可以使用响应操作返回包含 AS2 消息处置通知 （MDN） 的**响应**。 通过在**AS2 解码**操作后立即添加此操作，如果该操作失败，逻辑应用不会继续处理。

1. 在**AS2 解码**操作下，选择 **"新建步骤**"。

1. 在 **"在**搜索框下选择操作"下，选择 **"内置**"。 在搜索框中输入 `condition`。 从 **"操作"** 列表中，选择 **"条件**"。

   ![添加"条件"操作](./media/logic-apps-enterprise-integration-b2b/add-condition-action.png)

   现在将显示条件形状，包括条件是否满足的路径。

   ![具有决策路径的条件形状](./media/logic-apps-enterprise-integration-b2b/added-condition-action.png)

1. 现在指定要评估的条件。 在"**选择值**"框中，输入此表达式：

   `@body('AS2_Decode')?['AS2Message']?['MdnExpected']`

   在中间框中，确保比较操作设置为`is equal to`。 在右侧框中，输入值`Expected`。 要使表达式作为此令牌解析，请切换设计器和代码视图。

   ![具有决策路径的条件形状](./media/logic-apps-enterprise-integration-b2b/expression-for-evaluating-condition.png)

1. 现在指定要返回的响应，**以便 AS2 解码**操作是否成功。

   1. 对于**AS2 解码**操作成功时的情况，在 **"如果为真"** 形状中，选择 **"添加操作**"。 在"在搜索框中**选择操作**"，输入`response`，然后选择 **"响应**"。

      ![查找并选择"响应"操作](./media/logic-apps-enterprise-integration-b2b/select-http-response-action.png)

   1. 要从**AS2 解码**操作的输出访问 AS2 MDN，请指定以下表达式：

      * 在**响应**操作的 **"标头"** 属性中，输入此表达式：

        `@body('AS2_Decode')?['OutgoingMdn']?['OutboundHeaders']`

      * 在 **"响应**"操作的 **"正文"** 属性中，输入以下表达式：

        `@body('AS2_Decode')?['OutgoingMdn']?['Content']`

   1. 要将表达式作为令牌解析，请切换设计器和代码视图：

      ![用于访问 AS2 MDN 的已解析表达式](./media/logic-apps-enterprise-integration-b2b/response-action-success-resolved-expression.png)

   1. 对于**AS2 解码**操作失败的情况，在 **"如果假"** 形状中，选择 **"添加操作**"。 在"在搜索框中**选择操作**"，输入`response`，然后选择 **"响应**"。 设置**响应**操作以返回所需的状态和错误。

1. 保存逻辑应用。

## <a name="add-decode-x12-message-action"></a>添加解码 X12 消息操作

1. 现在添加**解码 X12 消息**操作。 在 **"响应**"操作下，选择 **"添加操作**"。

1. 在搜索框中**选择操作**，输入`x12 decode`，然后选择 **"解码 X12"消息**。

   ![查找并选择"解码 X12 消息"操作](./media/logic-apps-enterprise-integration-b2b/add-x12-decode-action.png)

1. 如果 X12 操作提示您输入连接信息，请提供连接的名称，选择要使用的集成帐户，然后选择 **"创建**"。

   ![创建与集成帐户的 X12 连接](./media/logic-apps-enterprise-integration-b2b/create-x12-integration-account-connection.png)

1. 现在指定 X12 操作的输入。 此示例使用 AS2 操作的输出，该操作是消息内容，但请注意，此内容采用 JSON 对象格式，并且已对 base64 进行编码。 因此，您必须将此内容转换为字符串。

   在**X12 平面文件消息中解码**框中，输入此表达式以转换 AS2 输出：

   `@base64ToString(body('AS2_Decode')?['AS2Message']?['Content'])`

    要使表达式作为此令牌解析，请切换设计器和代码视图。

    ![将基本64编码的内容转换为字符串](./media/logic-apps-enterprise-integration-b2b/x12-decode-message-content.png)

1. 保存逻辑应用。

   如果需要此逻辑应用的其他步骤，例如，以 JSON 对象格式解码消息内容和输出该内容，请继续构建逻辑应用。

现在，您已经设置完 B2B 逻辑应用。 在真实应用中，您可能希望将解码的 X12 数据存储在业务线 （LOB） 应用或数据存储中。 例如，请参阅以下文章：

* [从 Azure 逻辑应用连接到 SAP 系统](../logic-apps/logic-apps-using-sap-connector.md)
* [使用 SSH 和 Azure 逻辑应用监视、创建和管理 SFTP 文件](../connectors/connectors-sftp-ssh.md)

要连接自己的 LOB 应用并在逻辑应用中使用这些 API，可以添加更多操作或[编写自定义 API。](../logic-apps/logic-apps-create-api-app.md)

## <a name="next-steps"></a>后续步骤

* [接收和响应传入的 HTTPS 呼叫](../connectors/connectors-native-reqres.md)
* [为 B2B 企业集成交换 AS2 消息](../logic-apps/logic-apps-enterprise-integration-as2.md)
* [为 B2B 企业集成交换 X12 消息](../logic-apps/logic-apps-enterprise-integration-x12.md)
* 了解有关[企业集成包的更多](../logic-apps/logic-apps-enterprise-integration-overview.md)