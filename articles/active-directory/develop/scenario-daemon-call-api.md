---
title: 从守护程序应用调用 Web API - Microsoft 标识平台 | Azure
description: 了解如何构建用于调用 Web API 的守护程序应用
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 338b638d6b33bcbbb5cf377643a96c71b0d314bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "76775193"
---
# <a name="daemon-app-that-calls-web-apis---call-a-web-api-from-the-app"></a>调用 Web API 的守护程序应用 - 从应用调用 Web API

.NET 守护程序应用可以调用一个 Web API。 .NET 守护程序应用还可以调用多个预先批准的 Web API。

## <a name="calling-a-web-api-from-a-daemon-application"></a>从守护程序应用程序调用一个 Web API

下面介绍如何使用令牌来调用一个 API：

# <a name="net"></a>[.NET](#tab/dotnet)

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

# <a name="python"></a>[Python](#tab/python)

```Python
endpoint = "url to the API"
http_headers = {'Authorization': 'Bearer ' + result['access_token'],
                'Accept': 'application/json',
                'Content-Type': 'application/json'}
data = requests.get(endpoint, headers=http_headers, stream=False).json()
```

# <a name="java"></a>[Java](#tab/java)

```Java
HttpURLConnection conn = (HttpURLConnection) url.openConnection();

// Set the appropriate header fields in the request header.
conn.setRequestProperty("Authorization", "Bearer " + accessToken);
conn.setRequestProperty("Accept", "application/json");

String response = HttpClientHelper.getResponseStringFromConn(conn);

int responseCode = conn.getResponseCode();
if(responseCode != HttpURLConnection.HTTP_OK) {
    throw new IOException(response);
}

JSONObject responseObject = HttpClientHelper.processResponse(responseCode, response);
```

---

## <a name="calling-several-apis"></a>调用多个 API

对于守护程序应用，需要预先批准调用的 Web API。 守护程序应用没有增量同意。 （没有用户交互。租户管理员需要提前为应用程序和所有 API 权限提供同意。 如果要调用多个 API，则每次调用 `AcquireTokenForClient` 时都需要为每个资源获取一个令牌。 MSAL 将使用应用程序令牌缓存来避免不必要的服务调用。

## <a name="next-steps"></a>后续步骤

# <a name="net"></a>[.NET](#tab/dotnet)

> [!div class="nextstepaction"]
> [守护程序应用 - 移到生产环境](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-production?tabs=dotnet)

# <a name="python"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [守护程序应用 - 移到生产环境](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-production?tabs=python)

# <a name="java"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [守护程序应用 - 移到生产环境](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-production?tabs=java)

---
