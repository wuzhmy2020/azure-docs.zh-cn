---
title: 使用视频索引器 API 自定义品牌模型
titleSuffix: Azure Media Services
description: 了解如何使用视频索引器 API 自定义品牌模型。
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 01/14/2020
ms.author: anzaman
ms.openlocfilehash: 79c3a7934e9152a4908f895c20ee6fbdc0f360cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "80127994"
---
# <a name="customize-a-brands-model-with-the-video-indexer-api"></a>使用视频索引器 API 自定义品牌模型

在对视频和音频内容进行索引和重新索引的过程中，可以使用视频索引器从语音和视觉文本中检测品牌。 品牌检测功能可以确定内容中是否提到必应品牌数据库建议的产品、服务和公司。 例如，如果视频或音频内容中提到 Microsoft，或者在视频中以可视文本显示，视频索引器会将其检测为内容中的品牌。 使用自定义品牌模型时，可以排除某些品牌，不让其检测到；还可以添加一些品牌，这些品牌应该在你的模型中，但可能不在必应的品牌数据库中。

如需详细的概述，请参阅[概述](customize-brands-model-overview.md)。

可以使用视频索引器 API 来创建、使用和编辑视频中检测到的自定义品牌模型，如本主题所述。 也可使用视频索引器网站，如[使用视频索引器网站自定义品牌模型](customize-brands-model-with-api.md)中所述。

## <a name="create-a-brand"></a>创建品牌

[创建品牌](https://api-portal.videoindexer.ai/docs/services/operations/operations/Create-Brand)API 创建新的自定义品牌，并将其添加到指定帐户的自定义品牌模型中。

> [!NOTE]
> 将`enabled`（在正文中）设置为 true 将品牌置于"*包含*"列表中，以便视频索引器进行检测。 设置为`enabled`false 将品牌置于 *"排除"* 列表中，因此视频索引器无法检测到它。

可以在正文中设置的一些其他参数：

* 该`referenceUrl`值可以是该品牌的任何参考网站，例如指向其维基百科页面的链接。
* 该`tags`值是品牌的标签列表。 此标记显示在视频索引器网站上的品牌*类别*字段中。 例如，品牌“Azure”可以标记或归类为“云”。

### <a name="response"></a>响应

响应介绍刚刚按照以下示例的格式创建的品牌。

```json
{
  "referenceUrl": "https://en.wikipedia.org/wiki/Example",
  "id": 97974,
  "name": "Example",
  "accountId": "SampleAccountId",
  "lastModifierUserName": "SampleUserName",
  "created": "2018-04-25T14:59:52.7433333",
  "lastModified": "2018-04-25T14:59:52.7433333",
  "enabled": true,
  "description": "This is an example",
  "tags": [
    "Tag1",
    "Tag2"
  ]
}
```

## <a name="delete-a-brand"></a>删除品牌

[删除品牌](https://api-portal.videoindexer.ai/docs/services/operations/operations/Delete-Brand?)API 会从指定帐户的自定义品牌模型中删除品牌。 该帐户在`accountId`参数中指定。 成功调用以后，品牌将不再位于 *Include* 或 *Exclude* 品牌列表中。

### <a name="response"></a>响应

成功删除品牌时，不会返回任何内容。

## <a name="get-a-specific-brand"></a>获取特定的品牌

[获取品牌](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Brand?)API 允许您使用品牌 ID 搜索自定义品牌模型中品牌的详细信息。

### <a name="response"></a>响应

响应介绍按照以下示例的格式搜索（使用品牌 ID）的品牌。

```json
{
  "referenceUrl": "https://en.wikipedia.org/wiki/Example",
  "id": 128846,
  "name": "Example",
  "accountId": "SampleAccountId",
  "lastModifierUserName": "SampleUserName",
  "created": "2018-01-06T13:51:38.3666667",
  "lastModified": "2018-01-11T13:51:38.3666667",
  "enabled": true,
  "description": "This is an example",
  "tags": [
    "Tag1",
    "Tag2"
  ]
}
```

> [!NOTE]
> `enabled`设置为`true`表示品牌位于"视频索引器要检测的*包含"* 列表中，并且`enabled`false 表示品牌位于 *"排除"* 列表中，因此视频索引器不会检测到它。

## <a name="update-a-specific-brand"></a>更新特定品牌

[通过更新品牌](https://api-portal.videoindexer.ai/docs/services/operations/operations/Update-Brand?)API，您可以使用品牌 ID 搜索自定义品牌模型中品牌的详细信息。

### <a name="response"></a>响应

响应按照以下示例的格式针对更新的品牌提供更新的信息。

```json
{
  "referenceUrl": null,
  "id": 97974,
  "name": "Example",
  "accountId": "SampleAccountId",
  "lastModifierUserName": "SampleUserName",
  "Created": "2018-04-25T14:59:52.7433333",
  "lastModified": "2018-04-25T15:37:50.67",
  "enabled": false,
  "description": "This is an update example",
  "tags": [
    "Tag1",
    "NewTag2"
  ]
}
```

## <a name="get-all-of-the-brands"></a>获取所有品牌

[获取所有品牌](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Brands?)API 返回指定帐户的自定义品牌模型中的所有品牌，无论该品牌是要在 *"包含"* 还是 *"排除*品牌"列表中。

### <a name="response"></a>响应

响应提供一个列表，其中包含帐户中的所有品牌，以及遵循以下示例格式的每个品牌的详细信息。

```json
[
    {
        "ReferenceUrl": null,
        "id": 97974,
        "name": "Example",
        "accountId": "AccountId",
        "lastModifierUserName": "UserName",
        "Created": "2018-04-25T14:59:52.7433333",
        "LastModified": "2018-04-25T14:59:52.7433333",
        "enabled": true,
        "description": "This is an example",
        "tags": ["Tag1", "Tag2"]
    },
    {
        "ReferenceUrl": null,
        "id": 97975,
        "name": "Example2",
        "accountId": "AccountId",
        "lastModifierUserName": "UserName",
        "Created": "2018-04-26T14:59:52.7433333",
        "LastModified": "2018-04-26T14:59:52.7433333",
        "enabled": false,
        "description": "This is another example",
        "tags": ["Tag1", "Tag2"]
    },
]
```

> [!NOTE]
> 名为 *"示例"* 的品牌位于"视频索引器要检测的*包含"* 列表中，而名为*示例 2*的品牌位于 *"排除"* 列表中，因此视频索引器不会检测到它。

## <a name="get-brands-model-settings"></a>获取品牌模型设置

[获取品牌设置](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Brands)API 返回指定帐户中的品牌模型设置。 品牌模型设置表示是否启用从必应品牌数据库进行检测的功能。 如果未启用必应品牌，视频索引器将仅检测指定帐户的自定义品牌模型中的品牌。

### <a name="response"></a>响应

响应显示是否已按照以下示例的格式启用必应品牌。

```json
{
  "state": true,
  "useBuiltIn": true
}
```

> [!NOTE]
> `useBuiltIn`设置为 true 表示必应品牌已启用。 如果`useBuiltin`为 false，则禁用必应品牌。 可以`state`忽略该值，因为它已被弃用。

## <a name="update-brands-model-settings"></a>更新品牌模型设置

[更新品牌](https://api-portal.videoindexer.ai/docs/services/operations/operations/Update-Brands-Model-Settings?)API 更新指定帐户中的品牌模型设置。 品牌模型设置表示是否启用从必应品牌数据库进行检测的功能。 如果未启用必应品牌，视频索引器将仅检测指定帐户的自定义品牌模型中的品牌。

设置为`useBuiltIn`true 的标志表示必应品牌已启用。 如果`useBuiltin`为 false，则禁用必应品牌。

### <a name="response"></a>响应

成功更新品牌模型设置时，没有返回的内容。

## <a name="next-steps"></a>后续步骤

[使用网站自定义品牌模型](customize-brands-model-with-website.md)
