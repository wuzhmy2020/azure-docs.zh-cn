---
title: 使用 Azure 地图搜索服务搜索位置 |微软 Azure 地图
description: 在本文中，您将学习如何使用 Microsoft Azure 地图搜索服务搜索位置进行地理编码和反向地理编码。
author: philmea
ms.author: philmea
ms.date: 01/15/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: cf0e5267885df1ace51271c53bb2d68ee5002f00
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335434"
---
# <a name="search-for-a-location-using-azure-maps-search-services"></a>使用 Azure 地图搜索服务搜索位置

Azure 地图[搜索服务](https://docs.microsoft.com/rest/api/maps/search)是一组 RESTful API，旨在帮助开发人员搜索地址、地点、按名称或类别分类的业务列表以及其他地理信息。 除了支持传统的地理编码外，服务还可以根据纬度和经度反转地理编码地址和交叉街道。 搜索返回的纬度和经度值可用作其他 Azure 地图服务（如[路由](https://docs.microsoft.com/rest/api/maps/route)和[天气](https://docs.microsoft.com/rest/api/maps/weather)服务）中的参数。

在本文中，您将学习如何：

* 使用[搜索地址 API]( https://docs.microsoft.com/rest/api/maps/search/getsearchaddress)请求地址（地理编码地址位置）的纬度和经度坐标
* 使用[模糊搜索 API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)搜索地址或兴趣点 （POI）
* 搜索地址以及属性和坐标
* 进行[反向地址搜索](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)，将坐标位置转换为街道地址
* 使用[搜索地址反向跨街 API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreversecrossstreet)搜索十字路口

## <a name="prerequisites"></a>先决条件

要完成本文中的步骤，需要首先创建 Azure 地图帐户并获取映射帐户订阅密钥。 按照[创建帐户](quick-demo-map-app.md#create-an-account-with-azure-maps)中的说明创建 Azure 地图帐户订阅，然后按照[获取主键](quick-demo-map-app.md#get-the-primary-key-for-your-account)中的步骤获取帐户的主密钥。 有关 Azure Maps 中身份验证的详细信息，请参阅[在 Azure Maps 中管理身份验证](./how-to-manage-authentication.md)。

本文使用 [Postman 应用](https://www.getpostman.com/apps)来构建 REST 调用。 可以使用你喜欢的任何 API 开发环境。

## <a name="request-latitude-and-longitude-for-an-address-geocoding"></a>请求地址的纬度和经度（地理编码）

在此示例中，我们使用 Azure 地图[获取搜索地址 API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress)将街道地址转换为纬度和经度坐标。 您可以将完整或部分街道地址传递给 API，并接收包含详细地址属性（如街道、邮政编码和国家/地区）以及纬度和经度位置值的响应。

如果您有一组要地理编码的地址，则可以使用["后搜索地址批处理 API"](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressbatch)在单个 API 调用中发送一批查询。

1. 在邮递员中，单击 **"新请求** | **GET 请求**"并命名为 **"地址搜索**"。

2. 在“生成器”选项卡上，选择“GET”HTTP 方法，输入 API 终结点的请求 URL，并选择授权协议（如果有）。****

![地址搜索](./media/how-to-search-for-address/address_search_url.png)

| 参数 | 建议的值 |
|---------------|------------------------------------------------| 
| HTTP 方法 | GET |
| 请求 URL | [https://atlas.microsoft.com/search/address/json?](https://atlas.microsoft.com/search/address/json?) | 
| 授权 | 无身份验证 |

3. 单击“参数”，输入以下键值对用作请求 URL 中的查询或路径参数：**** 

![地址搜索](./media/how-to-search-for-address/address_search_params.png) 

| 键 | “值” | 
|------------------|-------------------------| 
| api-version | 1.0 | 
| subscription-key | \<你的 Azure Maps 密钥\> | 
| query | 400 Broad St, Seattle, WA 98109 | 

4. 单击“发送”并查看响应正文。**** 

本例中指定了完整地址查询，并在响应正文中收到了一条结果。 

5. 在“参数”中，将查询字符串编辑为以下值： 

    ```plaintext 
        400 Broad, Seattle 
    ``` 

6. 将以下键/值对添加至 Params 部分****，并单击“发送”****： 

| 键 | “值” | 
|-----|------------| 
| typeahead | true | 

**typeahead** 标志告知地址搜索 API 要将查询视为部分输入，并返回预测值的数组。

## <a name="using-fuzzy-search-api"></a>使用模糊搜索 API

当您不知道搜索查询的用户输入是什么时，建议使用 Azure 映射[模糊搜索 API。](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) API 将兴趣点 （POI） 搜索和地理编码合并为规范的"单行搜索"。 例如，该 API 可以处理任何地址或 POI 令牌组合的输入。 还可以使用完全受坐标和半径约束的上下文位置 （纬度/经度对）为该 API 加权，或者在不使用任何地理偏置定位点的情况下，以更泛的方式执行该 API。

大多数搜索查询默认指定 `maxFuzzyLevel=1`，以提高性能并减少不正常的结果。 可以根据需要，通过传入查询参数 `maxFuzzyLevel=2` 或 `3`，按请求重写此默认值。

### <a name="search-for-an-address-using-fuzzy-search"></a>使用模糊搜索搜索地址

1. 打开 Postman 应用，单击“新建”>“新建”，选择“GET 请求”。**** 输入“模糊搜索”作为请求名称，选择用于保存该请求的集合或文件夹，单击“保存”。********

2. 在“生成器”选项卡上，选择“GET”HTTP 方法，输入 API 终结点的请求 URL。****

    ![模糊搜索](./media/how-to-search-for-address/fuzzy_search_url.png)

    | 参数 | 建议的值 |
    |---------------|------------------------------------------------|
    | HTTP 方法 | GET |
    | 请求 URL | [https://atlas.microsoft.com/search/fuzzy/json?](https://atlas.microsoft.com/search/fuzzy/json?) |
    | 授权 | 无身份验证 |

    URL 路径中的 **json** 属性确定响应格式。 本文使用 json，便于使用和可读性。 可以在 [Maps 功能 API 参考](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)的 **Get 模糊搜索**定义中找到可用的响应格式。

3. 单击“参数”，输入以下键值对用作请求 URL 中的查询或路径参数：****

    ![模糊搜索](./media/how-to-search-for-address/fuzzy_search_params.png)

    | 键 | “值” |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | subscription-key | \<你的 Azure Maps 密钥\> |
    | query | pizza |

4. 单击“发送”并查看响应正文。****

    "比萨饼"的不明确查询字符串返回"比萨饼"和"餐厅"类别中的 10[个兴趣点结果](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi#searchpoiresponse)（POI）。 每个结果返回位置的街道地址、纬度和经度值、视图端口和入口点。
  
    此查询的结果会有变化，并不与任何特定的参照位置紧密相关。 您可以使用**国家/地区设置**参数仅指定应用程序需要覆盖的国家/地区。 默认行为是搜索整个世界，可能会返回不必要的结果。

5. 将以下键/值对添加至 Params 部分****，并单击“发送”****：

    | 键 | “值” |
    |------------------|-------------------------|
    | countrySet | 美国 |
  
    结果现在受限为国家/地区代码，查询返回了美国境内的比萨餐馆。
  
    要为某个位置提供结果，可以查询兴趣点，并在模糊搜索服务的调用中使用返回的纬度和经度值。 本例使用搜索服务返回了西雅图太空针塔 (Seattle Space Needle) 的位置，并使用了纬度 /经度 值来定位搜索。
  
6. 在“参数”中，输入以下键值对并单击“发送”：****

    ![模糊搜索](./media/how-to-search-for-address/fuzzy_search_latlon.png)
  
    | 键 | “值” |
    |-----|------------|
    | lat | 47.620525 |
    | lon | -122.349274 |


## <a name="search-for-a-street-address-using-reverse-address-search"></a>使用反向地址搜索搜索街道地址

Azure 地图[获取搜索地址反向 API]( https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)有助于将坐标（例如：37.786505，-122.3862）转换为人类可以理解的街道地址。 通常，在跟踪应用程序时，您需要从设备或资产接收 GPS 源，并想知道坐标所在的地址。
如果您有一组坐标位置来反转地理编码，则可以使用["后搜索地址反向批处理 API"](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressreversebatch)在单个 API 调用中发送一批查询。


1. 在邮递员中，单击 **"新请求** | **GET 请求**"并命名为 **"反向地址搜索**"。

2. 在“生成器”选项卡上，选择“GET”HTTP 方法，输入 API 终结点的请求 URL。****
  
    ![反向地址搜索 URL](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | 参数 | 建议的值 |
    |---------------|------------------------------------------------|
    | HTTP 方法 | GET |
    | 请求 URL | [https://atlas.microsoft.com/search/address/reverse/json?](https://atlas.microsoft.com/search/address/reverse/json?) |
    | 授权 | 无身份验证 |
  
3. 单击“参数”，输入以下键值对用作请求 URL 中的查询或路径参数：****
  
    ![反向地址搜索参数](./media/how-to-search-for-address/reverse_address_search_params.png)
  
    | 键 | “值” |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | subscription-key | \<你的 Azure Maps 密钥\> |
    | query | 47.591180,-122.332700 |
  
4. 单击“发送”并查看响应正文。****

    该响应包括有关 Safeco Field 的关键地址信息。
  
5. 将以下键/值对添加至 Params 部分****，并单击“发送”****：

    | 键 | “值” |
    |-----|------------|
    | 数字 | true |

    如果与请求一起发送[数字](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)查询参数，响应可能包括街道的一侧（左侧或右侧）以及该号码的偏移位置。
  
6. 将以下键/值对添加至 Params 部分****，并单击“发送”****：

    | 键 | “值” |
    |-----|------------|
    | returnSpeedLimit | true |
  
    设置[ReturnSpeedLimit](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)查询参数时，响应将返回发布的速度限制。

7. 将以下键/值对添加至 Params 部分****，并单击“发送”****：

    | 键 | “值” |
    |-----|------------|
    | returnRoadUse | true |

    如果设置了 [returnRoadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) 查询参数，响应将返回街道级别的反向地理编码的道路用途数组。

8. 将以下键/值对添加至 Params 部分****，并单击“发送”****：

    | 键 | “值” |
    |-----|------------|
    | roadUse | true |

    您可以使用[道路使用](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)查询参数将反向地理编码查询限制为特定类型的道路。
  
## <a name="search-for-cross-street-using-reverse-address-cross-street-search"></a>使用反向地址跨街搜索搜索交叉街道

1. 在邮递员中，单击 **"新请求** | **GET 请求**"并命名为 **"反向地址跨街搜索**"。

2. 在“生成器”选项卡上，选择“GET”HTTP 方法，输入 API 终结点的请求 URL。****
  
    ![反向地址十字路口搜索](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | 参数 | 建议的值 |
    |---------------|------------------------------------------------|
    | HTTP 方法 | GET |
    | 请求 URL | [https://atlas.microsoft.com/search/address/reverse/crossstreet/json?](https://atlas.microsoft.com/search/address/reverse/crossstreet/json?) |
    | 授权 | 无身份验证 |
  
3. 单击“参数”，输入以下键值对用作请求 URL 中的查询或路径参数：****
  
    | 键 | “值” |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | subscription-key | \<你的 Azure Maps 密钥\> |
    | query | 47.591180,-122.332700 |
  
4. 单击“发送”并查看响应正文。****

## <a name="next-steps"></a>后续步骤

- 浏览[Azure 地图搜索服务 REST API 文档](https://docs.microsoft.com/rest/api/maps/search)。
- 了解[Azure 地图搜索服务最佳实践](https://docs.microsoft.com/azure/azure-maps/how-to-use-best-practices-for-search)以及如何优化查询。
