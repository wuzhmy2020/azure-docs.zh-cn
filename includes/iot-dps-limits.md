---
author: rothja
ms.service: cost-management-billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 2b8d60da507aa1eb312d1a8505f756ff337085ff
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "77592404"
---
下表列出了适用于 Azure IoT 中心设备预配服务资源的限制。

| 资源 | 限制 |
| --- | --- |
| 每个 Azure 订阅的最大设备预配服务数 | 10 |
| 最大登记数 | 1,000,000 |
| 最大注册数 | 1,000,000 |
| 最大登记组数 | 100 |
| 最大 CA 数 | 25 |
| 链接的 IoT 中心的最大数量 | 50 |
| 消息的最大大小 | 96 KB|

> [!NOTE]
> 若要增加预配服务上的登记和注册数量，请联系 [Microsoft 支持部门](https://azure.microsoft.com/support/options/)。

> [!NOTE]
> 增加 CA 的最大数目不受支持。

超过以下配额时，设备预配服务将限制请求。

| 限制 | 每单位值 |
| --- | --- |
| 操作 | 200/分钟/服务 |
| 设备注册数 | 200/分钟/服务 |
| 设备轮询操作 | 5/10 秒/设备 |
