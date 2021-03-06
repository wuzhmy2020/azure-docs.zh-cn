---
title: 快速入门：使用 Python 调用必应自定义搜索终结点 | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: 按照此快速入门开始使用 Python 从必应自定义搜索实例请求搜索结果
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: a601b309d18e489f6b631cb26e5f3e13ef790b42
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "80238831"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-python"></a>快速入门：使用 Python 调用必应自定义搜索终结点

参考本快速入门开始从必应自定义搜索实例请求搜索结果。 虽然此应用程序是使用 Python 编写的，但必应自定义搜索 API 是一种 RESTful Web 服务，与大多数编程语言兼容。 该示例的源代码可在 [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingCustomSearchv7.py) 上获得。

## <a name="prerequisites"></a>先决条件

- 必应自定义搜索实例。 请参阅[快速入门：创建第一个必应自定义搜索实例](quick-start.md)，了解详细信息。
- [Python](https://www.python.org/) 2.x 或 3.x

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>创建并初始化应用程序

1. 在你喜欢使用的 IDE 或编辑器中创建新的 Python 文件，然后添加以下 import 语句。 为订阅密钥、自定义配置 ID 和搜索词创建变量。 

    ```python
    import json
    import requests
    
    subscriptionKey = "YOUR-SUBSCRIPTION-KEY"
    customConfigId = "YOUR-CUSTOM-CONFIG-ID"
    searchTerm = "microsoft"
    ```

## <a name="send-and-receive-a-search-request"></a>发送和接收搜索请求 

1. 通过将搜索词追加​​到 `q=` 查询参数后面，并将搜索实例的自定义配置 ID 追加​​到 `customconfig=` 后面来构造请求 URL。 使用 `&` 字符分隔参数。 可以使用下面的全局终结点，也可以使用资源的 Azure 门户中显示的[自定义子域](../../cognitive-services/cognitive-services-custom-subdomains.md)终结点。

    ```python
    url = 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 'q=' + searchTerm + '&' + 'customconfig=' + customConfigId
    ```

2. 将请求发送到必应自定义搜索实例，并打印出返回的搜索结果。  

    ```python
    r = requests.get(url, headers={'Ocp-Apim-Subscription-Key': subscriptionKey})
    print(r.text)
    ```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [生成自定义搜索 Web 应用](./tutorials/custom-search-web-page.md)
