---
title: 快速入门：识别来自麦克风、Objective-C 的语音 - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何在 iOS 上使用语音 SDK 通过 Objective-C 识别语音
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/23/2019
ms.author: chlandsi
ms.openlocfilehash: c1246b19670a18f8dadc0c5e1c64dd5af4c1b210
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "75380773"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-ios-by-using-the-speech-sdk"></a>快速入门：在 iOS 上使用语音 SDK 通过 Objective-C 识别语音

针对[语音合成](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech-langs/objectivec-ios.md)也提供了快速入门。

本文介绍如何使用 Azure 认知服务语音 SDK 在 Objective-C 中创建 iOS 应用，以便将麦克风或包含录制音频的文件中的语音转录为文本。

## <a name="prerequisites"></a>先决条件

准备工作：

* 语音服务的[订阅密钥](~/articles/cognitive-services/Speech-Service/get-started.md)。
* [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) 或更高版本的 macOS 计算机。
* 目标设置为 iOS 9.3 版或更高版本。

## <a name="get-the-speech-sdk-for-ios"></a>获取用于 iOS 的语音 SDK

[!INCLUDE [License notice](~/includes/cognitive-services-speech-service-license-notice.md)]

用于 iOS 的认知服务语音 SDK 目前以 Cocoa 框架形式分发。
它可从[此网站](https://aka.ms/csspeech/iosbinary)下载。 将文件下载到主目录。

## <a name="create-an-xcode-project"></a>创建 Xcode 项目

启动 Xcode，然后通过选择“文件” > “新建” > “项目”来启动新项目。   
在模板选择对话框中，选择“iOS 单一视图应用”模板。 

在随后的对话框中，进行以下选择。

1. 在“项目选项”  对话框中，执行以下操作：
    1. 为快速入门应用输入一个名称，例如 *helloworld*。
    1. 如果已经有 Apple 开发人员帐户，请输入相应的组织名称和组织标识符。 出于测试目的，请使用 *testorg* 之类的名称。 若要对应用进行签名，需要适当的预配配置文件。 有关详细信息，请参阅 [Apple 开发人员站点](https://developer.apple.com/)。
    1. 确保选择 **Objective-C** 作为项目的语言。
    1. 清除所有用于测试和核心数据的复选框。

    ![项目设置](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-project-settings.png)

1. 选择项目目录：
   1. 选择用于放置项目的主目录。 此步骤会在主目录中创建一个 helloworld 目录，其中包含 Xcode 项目的所有文件。
   1. 禁止创建适用于此示例项目的 Git 存储库。
   1. 在项目设置屏幕上调整 SDK 的路径。
      1. 在“常规”选项卡的“嵌入式二进制文件”标头下   ，添加 SDK 库作为框架：选择“添加嵌入式二进制文件”   >   “添加其他”。 转到主目录，然后选择 `MicrosoftCognitiveServicesSpeech.framework` 文件。 此操作会自动将 SDK 库添加到“链接的框架和库”标头。 
         ![添加的框架](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-framework.png)
      1. 转到“生成设置”  选项卡，选择“所有”设置  。
      1. 将目录 $(SRCROOT)/..添加 到“搜索路径”标头下的“框架搜索路径”。  

      ![“框架搜索路径”设置](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-framework-search-paths.png)

## <a name="set-up-the-ui"></a>设置 UI

示例应用有非常简单的 UI。 它有两个用于从文件或麦克风输入启动语音识别的按钮，以及一个用于显示结果的文本标签。 此 UI 在项目的 `Main.storyboard` 部分设置。 打开情节提要的 XML 视图，方法是：右键单击项目树的 `Main.storyboard` 条目，然后选择“打开为”   >   “源代码”。

将自动生成的 XML 替换为以下代码：

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/ios/from-microphone/helloworld/helloworld/Base.lproj/Main.storyboard)]

## <a name="add-the-sample-code"></a>添加示例代码

1. 下载[示例 wav 文件](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-speech-sdk/f9807b1079f3a85f07cbb6d762c6b5449d536027/samples/cpp/windows/console/samples/whatstheweatherlike.wav)，方法是：右键单击链接，然后选择“将目标另存为”。 
   将 wav 文件作为资源添加到项目，方法是将其从 Finder 窗口拖放到“项目”视图的根级别目录中。
   在以下对话框中选择“完成”，不更改设置。 
1. 将自动生成的 `ViewController.m` 文件的内容替换为以下代码：

   [!code-objectivec[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/ios/from-microphone/helloworld/helloworld/ViewController.m#code)]
1. 将字符串 `YourSubscriptionKey` 替换为你的订阅密钥。
1. 将字符串 `YourServiceRegion` 替换为与订阅关联的[区域](~/articles/cognitive-services/Speech-Service/regions.md)。 例如，将 `westus` 用于免费试用版订阅。
1. 添加进行麦克风访问的请求。 右键单击项目树的 `Info.plist` 条目，然后选择“打开为” > “源代码”   。 将以下代码行添加到 `<dict>` 节，然后保存文件。

    ```xml
    <key>NSMicrophoneUsageDescription</key>
    <string>Need microphone access for speech recognition from microphone.</string>
    ```

## <a name="build-and-run-the-sample"></a>生成并运行示例

1. 使调试输出可见，方法是：选择“视图”   > “调试区域”   >   “激活控制台”。
1. 从“产品” > “目标”菜单中的列表中，选择 iOS 模拟器或连接到开发计算机的 iOS 设备作为应用的目标位置   。
1. 在 iOS 模拟器中生成并运行示例代码，方法是在菜单中选择“产品”   >   “运行”。 也可选择“播放”按钮。 
1. 选择应用中的“识别(文件)”按钮以后，应看到音频文件的内容“天气怎么样?”  显示在屏幕下部。

   ![模拟的 iOS 应用](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-simulated-app.png)

1. 选择应用中的“识别(麦克风)”按钮并讲几句话后，应在屏幕下方看到所述文本。 

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [浏览 GitHub 上的 Objective-C 示例](https://aka.ms/csspeech/samples)
