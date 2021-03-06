---
title: 快速入门：适用于 C# 的文本分析客户端库 | Microsoft Docs
description: 适用于 C# 的文本分析客户端库入门
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 03/17/2020
ms.author: aahi
ms.reviewer: assafi
ms.openlocfilehash: 64eb19e43223c1953a7244f8fd29c48d085f1e96
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "80116954"
---
<a name="HOLTop"></a>

#### <a name="version-30-preview"></a>[版本 3.0-preview](#tab/version-3)

[v3 参考文档](https://aka.ms/azsdk-net-textanalytics-ref-docs) | [v3 库源代码](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics) | [v3 包(NuGet)](https://www.nuget.org/packages/Azure.AI.TextAnalytics) | [v3 示例](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics/samples)

#### <a name="version-21"></a>[版本 2.1](#tab/version-2)

[v2 参考文档](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet-preview) | [v2 库源代码](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.TextAnalytics) | [v2 包(NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics/) | [v2 示例](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples)

---

## <a name="prerequisites"></a>先决条件

* Azure 订阅 - [免费创建订阅](https://azure.microsoft.com/free/)
* [Visual Studio IDE](https://visualstudio.microsoft.com/vs/)
* 你有了 Azure 订阅后，<a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="创建文本分析资源"  target="_blank">将在 Azure 门户中创建文本分析资源 <span class="docon docon-navigate-external x-hidden-focus"></span></a>，以获取你的密钥和终结点。  部署后，单击“转到资源”  。
    * 你需要从创建的资源获取密钥和终结点，以便将应用程序连接到文本分析 API。 你稍后会在快速入门中将密钥和终结点粘贴到下方的代码中。
    * 可以使用免费定价层 (`F0`) 试用该服务，然后再升级到付费层进行生产。

## <a name="setting-up"></a>设置

### <a name="create-a-new-net-core-application"></a>创建新的 .NET Core 应用程序

使用 Visual Studio IDE 创建新的 .NET Core 控制台应用。 这会创建包含单个 C# 源文件的“Hello World”项目：*program.cs*。

#### <a name="version-30-preview"></a>[版本 3.0-preview](#tab/version-3)

右键单击**解决方案资源管理器**中的解决方案，然后选择“管理 NuGet 包”  ，以便安装客户端库。 在打开的包管理器中，选择“浏览”  ，选中“包括预发行版”  并搜索 `Azure.AI.TextAnalytics`。 选择版本 `1.0.0-preview.3`，然后选择“安装”  。 也可使用[包管理器控制台](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package)。

> [!TIP]
> 想要立即查看整个快速入门代码文件？ 可以[在 GitHub 上](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/TextAnalytics/program.cs)找到它，其中包含此快速入门中的代码示例。 

#### <a name="version-21"></a>[版本 2.1](#tab/version-2)

右键单击**解决方案资源管理器**中的解决方案，然后选择“管理 NuGet 包”  ，以便安装客户端库。 在打开的包管理器中选择“浏览”  ，搜索 `Microsoft.Azure.CognitiveServices.Language.TextAnalytics`。 单击它，然后进行**安装**。 也可使用[包管理器控制台](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package)。

> [!TIP]
> 想要立即查看整个快速入门代码文件？ 可以[在 GitHub 上](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/samples/TextAnalytics/synchronous/Program.cs)找到它，其中包含此快速入门中的代码示例。 

---

#### <a name="version-30-preview"></a>[版本 3.0-preview](#tab/version-3)

打开 *program.cs* 文件并添加以下 `using` 指令：

```csharp
using System;
using System.Globalization;
using Azure.AI.TextAnalytics;
```

在应用程序的 `Program` 类中，为资源的密钥和终结点创建变量。

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly TextAnalyticsApiKeyCredential credentials = new TextAnalyticsApiKeyCredential("<replace-with-your-text-analytics-key-here>");
private static readonly Uri endpoint = new Uri("<replace-with-your-text-analytics-endpoint-here>");
```

替换应用程序的 `Main` 方法。 稍后将定义此处调用的方法。

```csharp
static void Main(string[] args)
{
    var client = new TextAnalyticsClient(endpoint, credentials);
    // You will implement these methods later in the quickstart.
    SentimentAnalysisExample(client);
    LanguageDetectionExample(client);
    EntityRecognitionExample(client);
    EntityPIIExample(client);
    EntityLinkingExample(client);
    KeyPhraseExtractionExample(client);

    Console.Write("Press any key to exit.");
    Console.ReadKey();
}
```

#### <a name="version-21"></a>[版本 2.1](#tab/version-2)

打开 *program.cs* 文件并添加以下 `using` 指令：

[!code-csharp[Import directives](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=imports)]

在应用程序的 `Program` 类中，为资源的密钥和终结点创建变量。 

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly string key = "<replace-with-your-text-analytics-key-here>";
private static readonly string endpoint = "<replace-with-your-text-analytics-endpoint-here>";
```

替换应用程序的 `Main` 方法。 稍后将定义此处调用的方法。

[!code-csharp[main method](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=main)]

---

## <a name="object-model"></a>对象模型

文本分析客户端是一个 `TextAnalyticsClient` 对象，该对象使用你的密钥在 Azure 中进行身份验证，并提供用于接受文本（单个字符串或批）的函数。 可以同步方式或异步方式将文本发送到 API。 响应对象包含发送的每个文档的分析信息。 

如果使用的是版本 `3.0-preview`，则可以使用可选的 `TextAnalyticsClientOptions` 实例，通过各种默认设置（例如默认语言或国家/地区提示）来初始化客户端。 还可以使用 Azure Active Directory 令牌进行身份验证。 

## <a name="code-examples"></a>代码示例

* [情绪分析](#sentiment-analysis)
* [语言检测](#language-detection)
* [命名实体识别](#named-entity-recognition-ner)
* [检测个人信息](#detect-personal-information)
* [实体链接](#entity-linking)
* [关键短语提取](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>验证客户端

#### <a name="version-30-preview"></a>[版本 3.0-preview](#tab/version-3)

请确保先前的 main 方法使用终结点和凭据创建新的客户端对象。

```csharp
var client = new TextAnalyticsClient(endpoint, credentials);
```

#### <a name="version-21"></a>[版本 2.1](#tab/version-2)

创建用于存储凭据的新 `ApiKeyServiceClientCredentials` 类，并将凭据发送到客户端的请求。 在该类中创建 `ProcessHttpRequestAsync()` 的重写，用于将密钥添加到 `Ocp-Apim-Subscription-Key` 标头。

[!code-csharp[Client class](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=clientClass)]

创建一个方法，以实例化包含你的终结点的 [TextAnalyticsClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-dotnet) 对象和包含你的密钥的 `ApiKeyServiceClientCredentials` 对象。

[!code-csharp[Client authentication](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=authentication)]

---

## <a name="sentiment-analysis"></a>情绪分析

#### <a name="version-30-preview"></a>[版本 3.0-preview](#tab/version-3)

创建一个名为 `SentimentAnalysisExample()` 的新函数，该函数接受你之前创建的客户端，并调用其 `AnalyzeSentiment()` 函数。 如果成功，则返回的 `Response<DocumentSentiment>` 对象将包含整个输入文档的情绪标签和分数，以及每个句子的情绪分析。 如果发生错误，则会引发 `RequestFailedException`。

```csharp
static void SentimentAnalysisExample(TextAnalyticsClient client)
{
    string inputText = "I had the best day of my life. I wish you were there with me.";
    DocumentSentiment documentSentiment = client.AnalyzeSentiment(inputText);
    Console.WriteLine($"Document sentiment: {documentSentiment.Sentiment}\n");

    var si = new StringInfo(inputText);
    foreach (var sentence in documentSentiment.Sentences)
    {
        Console.WriteLine($"\tSentence [length {sentence.GraphemeLength}]");
        Console.WriteLine($"\tText: \"{si.SubstringByTextElements(sentence.GraphemeOffset, sentence.GraphemeLength)}\"");
        Console.WriteLine($"\tSentence sentiment: {sentence.Sentiment}");
        Console.WriteLine($"\tPositive score: {sentence.ConfidenceScores.Positive:0.00}");
        Console.WriteLine($"\tNegative score: {sentence.ConfidenceScores.Negative:0.00}");
        Console.WriteLine($"\tNeutral score: {sentence.ConfidenceScores.Neutral:0.00}\n");
    }
}
```

### <a name="output"></a>输出

```console
Document sentiment: Positive

        Sentence [length 30]
        Text: "I had the best day of my life."
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Sentence [length 30]
        Text: "I wish you were there with me."
        Sentence sentiment: Neutral
        Positive score: 0.21
        Negative score: 0.02
        Neutral score: 0.77
```

#### <a name="version-21"></a>[版本 2.1](#tab/version-2)

创建名为 `SentimentAnalysisExample()` 的新函数，用于提取前面创建的客户端，并调用该客户端的 [Sentiment()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.sentiment?view=azure-dotnet) 函数。 如果成功，返回的 [SentimentResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.sentimentresult?view=azure-dotnet) 对象将包含情绪 `Score`，否则包含 `errorMessage`。 

评分接近 0 表示消极情绪，评分接近 1 表示积极情绪。

[!code-csharp[Sentiment analysis](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=sentiment)]

```console
Sentiment Score: 0.87
```

---

## <a name="language-detection"></a>语言检测

#### <a name="version-30-preview"></a>[版本 3.0-preview](#tab/version-3)


创建一个名为 `LanguageDetectionExample()` 的新函数，该函数接受你之前创建的客户端并调用其 `DetectLanguage()` 函数。 返回的 `Response<DetectedLanguage>` 对象会包含检测到的语言及其名称和 ISO-6391 代码。 如果发生错误，则会引发 `RequestFailedException`。

> [!Tip]
> 在某些情况下，可能很难根据输入区分语言。 可以使用 `countryHint` 参数指定 2 个字母的国家/地区代码。 默认情况下，API 使用“US”作为默认的 countryHint，要删除此行为，可以通过将此值设置为空字符串 `countryHint = ""` 来重置此参数。 若要设置不同的默认值，请设置 `TextAnalyticsClientOptions.DefaultCountryHint` 属性，然后在客户端初始化期间传递它。

```csharp
static void LanguageDetectionExample(TextAnalyticsClient client)
{
    DetectedLanguage detectedLanguage = client.DetectLanguage("Ce document est rédigé en Français.");
    Console.WriteLine("Language:");
    Console.WriteLine($"\t{detectedLanguage.Name},\tISO-6391: {detectedLanguage.Iso6391Name}\n");
}
```

### <a name="output"></a>输出

```console
Language:
        French, ISO-6391: fr
```

#### <a name="version-21"></a>[版本 2.1](#tab/version-2)

创建名为 `languageDetectionExample()` 的新函数，用于提取前面创建的客户端，并调用该客户端的 [DetectLanguage()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.detectlanguage?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_DetectLanguage_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) 函数。 如果成功，返回的 [LanguageResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.languageresult?view=azure-dotnet) 对象将包含 `DetectedLanguages` 中检测到的语言列表，否则包含 `errorMessage`。 输出返回的第一种语言。

> [!Tip]
> 在某些情况下，可能很难根据输入区分语言。 可以使用 `countryHint` 参数指定 2 个字母的国家/地区代码。 默认情况下，API 使用“US”作为默认的 countryHint，要删除此行为，可以通过将此值设置为空字符串 `countryHint = ""` 来重置此参数。

[!code-csharp[Language Detection example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=languageDetection)]

### <a name="output"></a>输出

```console
Language: English
```

---

## <a name="named-entity-recognition-ner"></a>命名实体识别 (NER)

#### <a name="version-30-preview"></a>[版本 3.0-preview](#tab/version-3)


> [!NOTE]
> 版本 `3.0-preview` 中的新增功能：
> * 实体识别现在包括检测文本中的个人信息的功能。
> * 实体关联现在独立于实体识别。


创建一个名为 `EntityRecognitionExample()` 的新函数，该函数接受你之前创建的客户端，调用其 `RecognizeEntities()` 函数并循环访问结果。 返回的 `Response<IReadOnlyCollection<CategorizedEntity>>` 对象将包含检测到的实体的列表。 如果发生错误，则会引发 `RequestFailedException`。

```csharp
static void EntityRecognitionExample(TextAnalyticsClient client)
{
    var response = client.RecognizeEntities("I had a wonderful trip to Seattle last week.");
    Console.WriteLine("Named Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tText: {entity.Text},\tCategory: {entity.Category},\tSub-Category: {entity.SubCategory}");
        Console.WriteLine($"\t\tLength: {entity.GraphemeLength},\tScore: {entity.ConfidenceScore:F2}\n");
    }
}
```

### <a name="output"></a>输出

```console
Named Entities:
        Text: Seattle,  Category: Location,     Sub-Category: GPE
                Length: 7,      Score: 0.92

        Text: last week,        Category: DateTime,     Sub-Category: DateRange
                Length: 9,      Score: 0.80
```

## <a name="detect-personal-information"></a>检测个人信息

创建一个名为 `EntityPIIExample()` 的新函数，该函数接受你之前创建的客户端，调用其 `RecognizePiiEntities()` 函数并循环访问结果。 与前面的函数类似，返回的 `Response<IReadOnlyCollection<CategorizedEntity>>` 对象将包含检测到的实体的列表。 如果发生错误，则会引发 `RequestFailedException`。

```csharp
static void EntityPIIExample(TextAnalyticsClient client)
{
    string inputText = "Insurance policy for SSN on file 123-12-1234 is here by approved.";
    var response = client.RecognizePiiEntities(inputText);
    Console.WriteLine("Personally Identifiable Information Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tText: {entity.Text},\tCategory: {entity.Category},\tSub-Category: {entity.SubCategory}");
        Console.WriteLine($"\t\tLength: {entity.GraphemeLength},\tScore: {entity.ConfidenceScore:F2}\n");
    }
}
```

### <a name="output"></a>输出

```console
Personally Identifiable Information Entities:
        Text: 123-12-1234,      Category: U.S. Social Security Number (SSN),    Sub-Category:
                Length: 11,     Score: 0.85
```


## <a name="entity-linking"></a>实体链接

创建一个名为 `EntityLinkingExample()` 的新函数，该函数接受你之前创建的客户端，调用其 `RecognizeLinkedEntities()` 函数并循环访问结果。 返回的 `Response<IReadOnlyCollection<LinkedEntity>>` 表示检测到的实体的列表。 如果发生错误，则会引发 `RequestFailedException`。 由于链接实体是唯一标识的，因此同一实体的实例将以分组形式出现在 `LinkedEntity` 对象下，显示为 `LinkedEntityMatch` 对象的列表。

```csharp
static void EntityLinkingExample(TextAnalyticsClient client)
{
    var response = client.RecognizeLinkedEntities(
        "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, " +
        "to develop and sell BASIC interpreters for the Altair 8800. " +
        "During his career at Microsoft, Gates held the positions of chairman, " +
        "chief executive officer, president and chief software architect, " +
        "while also being the largest individual shareholder until May 2014.");
    Console.WriteLine("Linked Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tName: {entity.Name},\tID: {entity.DataSourceEntityId},\tURL: {entity.Url}\tData Source: {entity.DataSource}");
        Console.WriteLine("\tMatches:");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tText: {match.Text}");
            Console.WriteLine($"\t\tLength: {match.GraphemeLength},\tScore: {match.ConfidenceScore:F2}\n");
        }
    }
}
```

### <a name="output"></a>输出

```console
Linked Entities:
        Name: Altair 8800,      ID: Altair 8800,        URL: https://en.wikipedia.org/wiki/Altair_8800  Data Source: Wikipedia
        Matches:
                Text: Altair 8800
                Length: 11,     Score: 0.78

        Name: Bill Gates,       ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates   Data Source: Wikipedia
        Matches:
                Text: Bill Gates
                Length: 10,     Score: 0.55

                Text: Gates
                Length: 5,      Score: 0.55

        Name: Paul Allen,       ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen   Data Source: Wikipedia
        Matches:
                Text: Paul Allen
                Length: 10,     Score: 0.53

        Name: Microsoft,        ID: Microsoft,  URL: https://en.wikipedia.org/wiki/Microsoft    Data Source: Wikipedia
        Matches:
                Text: Microsoft
                Length: 9,      Score: 0.47

                Text: Microsoft
                Length: 9,      Score: 0.47

        Name: April 4,  ID: April 4,    URL: https://en.wikipedia.org/wiki/April_4      Data Source: Wikipedia
        Matches:
                Text: April 4
                Length: 7,      Score: 0.25

        Name: BASIC,    ID: BASIC,      URL: https://en.wikipedia.org/wiki/BASIC        Data Source: Wikipedia
        Matches:
                Text: BASIC
                Length: 5,      Score: 0.28
```

#### <a name="version-21"></a>[版本 2.1](#tab/version-2)

> [!NOTE]
> 在版本 2.1 中，实体链接包含在 NER 响应中。

创建名为 `RecognizeEntitiesExample()` 的新函数，用于提取前面创建的客户端，并调用该客户端的 [Entities()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.entities?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_Entities_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) 函数。 循环访问这些结果。 如果成功，返回的 [EntitiesResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.entitiesresult?view=azure-dotnet) 对象将包含 `Entities` 中检测到的实体列表，否则包含 `errorMessage`。 对于每个检测到的实体，输出其类型、子类型和维基百科名称（如果存在），以及其在原始文本中的位置。

[!code-csharp[Entity Recognition example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=entityRecognition)]

--- 


## <a name="key-phrase-extraction"></a>关键短语提取

#### <a name="version-30-preview"></a>[版本 3.0-preview](#tab/version-3)

创建一个名为 `KeyPhraseExtractionExample()` 的新函数，该函数接受你之前创建的客户端，并调用其 `ExtractKeyPhrases()` 函数。 返回的 `<Response<IReadOnlyCollection<string>>` 对象将包含检测到的关键短语的列表。 如果发生错误，则会引发 `RequestFailedException`。

```csharp
static void KeyPhraseExtractionExample(TextAnalyticsClient client)
{
    var response = client.ExtractKeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in response.Value)
    {
        Console.WriteLine($"\t{keyphrase}");
    }
}
```

### <a name="output"></a>输出

```console
Key phrases:
    cat
    veterinarian
```

#### <a name="version-21"></a>[版本 2.1](#tab/version-2)

创建名为 `KeyPhraseExtractionExample()` 的新函数，用于提取前面创建的客户端，并调用该客户端的 [KeyPhrases()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.keyphrases?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_KeyPhrases_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) 函数。 如果成功，结果将包含 `KeyPhrases` 中检测到的关键短语列表，如果失败，则将包含 `errorMessage`。 输出任何检测到的关键短语。

[!code-csharp[Key phrase extraction example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=keyPhraseExtraction)]


### <a name="output"></a>输出

```console
Key phrases:
    cat
    veterinarian
```

---
