---
title: 如何使用 Twilio 实现语音和 SMS (Java) | Microsoft Docs
description: 了解如何在 Azure 中使用 Twilio API 服务发起电话呼叫和发送短信。 用 Java 编写的代码示例。
services: ''
documentationcenter: java
author: georgewallace
ms.assetid: f3508965-5527-4255-9d51-5d5f926f4d43
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 11/25/2014
ms.author: gwallace
ms.openlocfilehash: 18e93ce18ed746612996399dc1aeb258abd26165
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "69637213"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a>如何通过 Java 使用 Twilio 实现语音和短信功能
本指南演示如何在 Azure 中使用 Twilio API 服务执行常见编程任务。 所涉及的任务包括发起电话呼叫和发送短信服务 (SMS) 消息。 有关 Twilio 以及应用程序中使用语音和 SMS 的详细信息，请参阅[后续步骤](#NextSteps)部分。

## <a name="what-is-twilio"></a><a id="WhatIs"></a>什么是 Twilio？
Twilio 是一种电话 Web 服务 API，可方便用户利用现有 Web 语言和技能生成语音和短信应用程序。 Twilio 属于第三方服务（而非 Azure 功能和 Microsoft 产品）。

利用 **Twilio 语音**，应用程序可以发起和接收电话呼叫。 应用程序可以使用 **Twilio SMS** 发出和接收 SMS 消息。 应用程序可以通过 **Twilio 客户端**使用现有 Internet 连接（包括移动连接）启用语音通信。

## <a name="twilio-pricing-and-special-offers"></a><a id="Pricing"></a>Twilio 定价和特惠套餐
[Twilio 定价][twilio_pricing]中提供了有关 Twilio 定价的信息。 Azure 客户可享受[特惠套餐][special_offer]：1000 条信息的免费信用额度或 1000 分钟的入站。 要注册此优惠或获取更多信息，请访问[https://ahoy.twilio.com/azure][special_offer]。

## <a name="concepts"></a><a id="Concepts"></a>概念
Twilio API 是一个为应用程序提供语音和 SMS 功能的 RESTful API。 提供了多种语言版本的客户端库；有关列表，请参阅 [Twilio API 库][twilio_libraries]。

Twilio API 的关键方面是 Twilio 谓词和 Twilio 标记语言 (TwiML)。

### <a name="twilio-verbs"></a><a id="Verbs"></a>Twilio 谓词
API 使用 Twilio 动词;例如**&lt;，"Say"&gt;** 谓词指示 Twilio 在呼叫中发出消息。

下面是 Twilio 谓词的列表。

* 拨号 ：将呼叫者连接到另一个电话。 ** &lt;&gt;**
* 收集：收集电话键盘上输入的数字数字。 ** &lt;&gt;**
* **挂断&gt;电话：结束&lt;** 通话。
* 播放 ：播放音频文件。 ** &lt;&gt;**
* 队列 ：将 添加到调用方队列。 ** &lt;&gt;**
* 暂停：静默等待指定秒数。 ** &lt;&gt;**
* 录制 ：记录调用方的语音并返回包含录制的文件的 URL。 ** &lt;&gt;**
* 重定向：以不同的 URL 将呼叫或短信的控制权转移到 TwiML。 ** &lt;&gt;**
* 拒绝 ：拒绝您的 Twilio 号码的来电，而不向您计费。 ** &lt;&gt;**
* 说：将文本转换为在呼叫时所做的语音。 ** &lt;&gt;**
* 短信 ：发送短信。 ** &lt;&gt;**

### <a name="twiml"></a><a id="TwiML"></a>TwiML
TwiML 是一组基于 XML 的指令，这些指令以用于指示 Twilio 如何处理呼叫或 SMS 的 Twilio 谓词为基础。

例如，以下 TwiML 将文本 **Hello World!** 转换为 语音。

```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World!</Say>
    </Response>
```

当应用程序调用 Twilio API 时，某个 API 参数将为返回 TwiML 响应的 URL。 在开发过程中，可以使用 Twilio 提供的 URL 来提供应用程序所使用的 TwiML 响应。 还可以托管自己的 URL 来生成 TwiML 响应，也可以选择使用 **TwiMLResponse** 对象。

有关 Twilio 谓词、其属性和 TwiML 的详细信息，请参阅 [TwiML][twiml]。 有关 Twilio API 的其他信息，请参阅 [Twilio API][twilio_api]。

## <a name="create-a-twilio-account"></a><a id="CreateAccount"></a>创建 Twilio 帐户
准备好获取 Twilio 帐户后，请在[试用 Twilio][try_twilio] 上注册。 可以先使用免费帐户，以后再升级帐户。

注册 Twilio 帐户时，将收到帐户 ID 和身份验证令牌。 需要二者才能发起 Twilio API 呼叫。 为了防止对帐户进行未经授权的访问，请保护身份验证令牌。 帐户 ID 和身份验证令牌会分别显示在 [Twilio 控制台][twilio_console]上标记为“帐户 SID”**** 和“身份验证令牌”**** 的字段中。

## <a name="create-a-java-application"></a><a id="create_app"></a>创建 Java 应用程序
1. 获取 Twilio JAR 并将其添加到 Java 生成路径和 WAR 部署程序集。 在[https://github.com/twilio/twilio-java][twilio_java]，您可以下载 GitHub 源并创建自己的 JAR，或下载预构建的 JAR（无论是否具有依赖项）。
2. 确保 JDK 的 **cacerts** 密钥库包含带有 MD5 指纹 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4（序列号为 35:DE:F4:CF，SHA1 指纹为 D2:32:09:AD:23:D3:14:23:21:74:E4:0D:7F:9D:62:13:97:86:63:3A）的 Equifax 安全证书颁发机构证书。 这是[https://api.twilio.com][twilio_api_service]服务的证书颁发机构 （CA） 证书，当您使用 Twilio API 时调用该证书。 有关如何确保 JDK 的 **cacerts** 密钥库包含正确 CA 证书的信息，请参阅[将证书添加到 Java CA 证书存储][add_ca_cert]。

[如何在 Azure 上的 Java 应用程序中使用 Twilio 发起电话呼叫][howto_phonecall_java]中提供了有关使用适用于 Java 的 Twilio 客户端库的详细说明。

## <a name="configure-your-application-to-use-twilio-libraries"></a><a id="configure_app"></a>将应用程序配置为使用 Twilio 库
在代码中，可以在要在应用程序中使用的 Twilio 包或类的源文件的顶部添加 **import** 语句。

对于 Java 源文件：

```java
    import com.twilio.*;
    import com.twilio.rest.api.*;
    import com.twilio.type.*;
    import com.twilio.twiml.*;
```

对于 Java Server Page (JSP) 源文件：

```java
    import="com.twilio.*"
    import="com.twilio.rest.api.*"
    import="com.twilio.type.*"
    import="com.twilio.twiml.*"
 ```
 
根据要使用的 Twilio 包或类，**import** 语句可能有差别。

## <a name="how-to-make-an-outgoing-call"></a><a id="howto_make_call"></a>如何拨打传出呼叫
以下代码演示了如何使用 **Call** 类发起传出呼叫。 此代码还使用 Twilio 提供的网站返回 Twilio 标记语言 (TwiML) 响应。 将值替换为**从**电话号码和**电话号码**，并确保在运行代码之前验证 Twilio 帐户的 **"从**电话号码"。

```java
    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize the Twilio client.
    Twilio.init(accountSID, authToken);

    // Use the Twilio-provided site for the TwiML response.
    URI uri = new URI("https://twimlets.com/message" +
            "?Message%5B0%5D=Hello%20World%21");

    // Declare To and From numbers
    PhoneNumber to = new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");

    // Create a Call creator passing From, To and URL values
    // then make the call by executing the create() method
    Call.creator(to, from, uri).create();
```

有关传入 **Call.creator** 方法的参数的详细信息，请参阅 [https://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls]。

如前所述，此代码使用 Twilio 提供的网站返回 TwiML 响应。 可以改用自己的网站来提供 TwiML 响应；有关详细信息，请参阅[如何在 Azure 上的 Java 应用程序中提供 TwiML 响应](#howto_provide_twiml_responses)。

## <a name="how-to-send-an-sms-message"></a><a id="howto_send_sms"></a>如何发送短信
以下代码演示了如何使用 **Message** 类发送短信。 号码**from** **4155992671，** 由 Twilio 提供，用于试用帐户发送短信。 在运行代码前，必须为 Twilio 帐户验证“被呼叫方”号码。****

```java
    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize the Twilio client.
    Twilio.init(accountSID, authToken);

    // Declare To and From numbers and the Body of the SMS message
    PhoneNumber to = new PhoneNumber("+14159352345"); // Replace with a valid phone number for your account.
    PhoneNumber from = new PhoneNumber("+14158141829"); // Replace with a valid phone number for your account.
    String body = "Where's Wallace?";

    // Create a Message creator passing From, To and Body values
    // then send the SMS message by calling the create() method
    Message sms = Message.creator(to, from, body).create();
```

有关传入 **Message.creator** 方法的参数的详细信息，请参阅 [https://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms]。

## <a name="how-to-provide-twiml-responses-from-your-own-website"></a><a id="howto_provide_twiml_responses"></a>如何从自己的网站提供 TwiML 响应
当应用程序启动对 Twilio API 的调用时（例如通过 **CallCreator.create** 方法），Twilio 会将请求发送到应该返回 TwiML 响应的 URL。 上面的示例使用 Twilio 提供的 URL [https://twimlets.com/message][twimlet_message_url]。 （虽然 TwiML 专供 Web 服务使用，但可以在浏览器中查看 TwiML。 例如，单击以查看[https://twimlets.com/message][twimlet_message_url]空响应元素;单击以查看"**&lt;响应"&gt;** 元素。作为另一[https://twimlets.com/message?Message%5B0%5D=Hello%20World%21][twimlet_message_url_hello_world]**&lt;&gt;** 个示例，单击以查看包含**&lt;Say&gt;** 元素的响应元素。

可以创建自己的返回 HTTP 响应的 URL 网站，而不用依赖 Twilio 提供的 URL。 可以用任何语言创建返回 HTTP 响应的网站；本主题假定在 JSP 页面中承载 URL。

以下 JSP 页面将生成在呼叫时念出 **Hello World!** 的 TwiML 响应。

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello World!</Say>
    </Response>
```

以下 JSP 页面将生成一个 TwiML 响应，念出一些文字，中间停顿几次，然后念出有关 Twilio API 版本和 Azure 角色名称的信息。

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello from Azure!</Say>
        <Pause></Pause>
        <Say>The Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>The Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>
```

**ApiVersion** 参数以 Twilio 语音请求（非 SMS 请求）的形式提供。 若要查看 Twilio 语音和 SMS 请求的可用请求参数，请分别参阅 <https://www.twilio.com/docs/api/twiml/twilio_request> 和 <https://www.twilio.com/docs/api/twiml/sms/twilio_request>。 **RoleName** 环境变量作为 Azure 部署的一部分提供。 （如果要添加自定义环境变量以便能够从 **System.getenv** 中选取它们，请参阅[其他角色配置设置][misc_role_config_settings]中的环境变量部分。）

将 JSP 页面设置为提供 TwiML 响应后，请使用 JSP 页面的 URL 作为传入到 **Call.creator** 方法中的 URL。 例如，如果将名为 MyTwiML 的 Web 应用程序部署到 Azure 托管服务，且 JSP 页面的名称为 mytwiml.jsp，则可将 URL 传递到 **Call.creator**，如下所示：

```java
    // Declare To and From numbers and the URL of your JSP page
    PhoneNumber to = new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");
    URI uri = new URI("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    // Create a Call creator passing From, To and URL values
    // then make the call by executing the create() method
    Call.creator(to, from, uri).create();
```

使用 TwiML 进行响应的另一个选项是通过 **VoiceResponse** 类，可在 **com.twilio.twiml** 包中找到它。

有关将 Azure 中的 Twilio 与 Java 一起使用的更多信息，请参阅[如何在 Azure 上的 Java 应用程序中使用 Twilio 发起电话呼叫][howto_phonecall_java]。

## <a name="how-to-use-additional-twilio-services"></a><a id="AdditionalServices"></a>如何使用其他 Twilio 服务
除了此处所示的示例之外，Twilio 还提供了基于 Web 的 API，可通过这些 API 从 Azure 应用程序中使用其他 Twilio 功能。 有关完整详细信息，请参阅 [Twilio API 文档][twilio_api_documentation]。

## <a name="next-steps"></a><a id="NextSteps"></a>后续步骤
现在，已了解 Twilio 服务的基础知识，单击下面的链接可以了解详细信息：

* [Twilio 安全准则][twilio_security_guidelines]
* [Twilio How 和示例代码][twilio_howtos]
* [Twilio 快速入门教程][twilio_quickstarts]
* [GitHub 上的 Twilio][twilio_on_github]
* [与 Twilio 技术支持交流][twilio_support]

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: https://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: https://twimlets.com/message
[twimlet_message_url_hello_world]: https://twimlets.com/message?Message%5B0%5D=Hello%20World%21
[twilio_rest_making_calls]: https://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: https://www.twilio.com/docs/api/rest/sending-sms
[twilio_pricing]: https://www.twilio.com/pricing
[special_offer]: https://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api]: https://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_console]:  https://www.twilio.com/console
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified#
[twilio_api_documentation]: https://www.twilio.com/docs
[twilio_security_guidelines]: https://www.twilio.com/docs/security
[twilio_howtos]: https://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: https://www.twilio.com/help/contact
[twilio_quickstarts]: https://www.twilio.com/docs/quickstart
