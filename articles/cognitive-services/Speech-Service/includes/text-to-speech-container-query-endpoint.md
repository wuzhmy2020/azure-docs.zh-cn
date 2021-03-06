---
title: 查询文本到语音转换容器终结点
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 11/04/2019
ms.author: dapine
ms.openlocfilehash: 8460ddca5cff2b3da540b5fa8cf66e0687892789
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "73491122"
---
该容器提供[基于 REST 的终结点 API。](../rest-text-to-speech.md) 有许多可用于平台、框架和语言变体[的示例源代码项目](https://azure.microsoft.com/resources/samples/cognitive-speech-tts/)。

使用 *"标准文本到语音转换"* 容器时，应依赖于下载的图像标记的区域设置和语音。 例如，如果您下载了标记，`latest`则默认区域设置是`en-US`和`JessaRUS`语音。 然后`{VOICE_NAME}`，论点将是[`en-US-JessaRUS`](../language-support.md#standard-voices)。 请参阅下面的示例 SSML：

```xml
<speak version="1.0" xml:lang="en-US">
    <voice name="en-US-JessaRUS">
        This text will get converted into synthesized speech.
    </voice>
</speak>
```

但是，对于*自定义文本到语音，* 您需要从[自定义语音门户](https://aka.ms/custom-voice-portal)获取**语音/模型**。 自定义模型名称是语音名称的同义词。 导航到 **"训练"** 页面，并复制 **"语音/模型**"以用作`{VOICE_NAME}`参数。
<br><br>
:::image type="content" source="../media/custom-voice/custom-voice-model-voice-name.png" alt-text="自定义语音模型 - 语音名称":::

请参阅下面的示例 SSML：

```xml
<speak version="1.0" xml:lang="en-US">
    <voice name="custom-voice-model">
        This text will get converted into synthesized speech.
    </voice>
</speak>
```

让我们构造一个 HTTP POST 请求，提供几个标头和数据负载。 将`{VOICE_NAME}`占位符替换为您自己的值。

```curl
curl -s -v -X POST http://localhost:5000/speech/synthesize/cognitiveservices/v1 \
 -H 'Accept: audio/*' \
 -H 'Content-Type: application/ssml+xml' \
 -H 'X-Microsoft-OutputFormat: riff-16khz-16bit-mono-pcm' \
 -d '<speak version="1.0" xml:lang="en-US"><voice name="{VOICE_NAME}">This is a test, only a test.</voice></speak>'
```

此命令：

* 为`speech/synthesize/cognitiveservices/v1`终结点构造 HTTP POST 请求。
* 指定`Accept``audio/*`
* 有关详细信息，`Content-Type`请指定`application/ssml+xml`标头，请参阅[请求正文](../rest-text-to-speech.md#request-body)。
* 指定`X-Microsoft-OutputFormat`标头`riff-16khz-16bit-mono-pcm`的更多选项，请参阅[音频输出](../rest-text-to-speech.md#audio-outputs)。
* 向终结点发送语音`{VOICE_NAME}`[合成标记语言 （SSML）](../speech-synthesis-markup.md)请求。