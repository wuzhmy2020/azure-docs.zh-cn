---
title: include 文件
description: include 文件
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 08/07/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 3386cb51a8a728576f6615002d6154d89ca662c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "68883730"
---
<!-- This tells how to get the connection string for the registryReadWrite shared access policy of your IoT hub -->

若要获取 **registryReadWrite** 策略的 IoT 中心连接字符串，请执行以下步骤：

1. 在[Azure 门户](https://portal.azure.com)中，选择**资源组**。 选择中心所在的资源组，然后从资源列表中选择中心。

2. 在中心的左侧窗格中，选择“共享访问策略”****。

3. 从策略列表中选择“registryReadWrite”**** 策略。

4. 在“共享访问密钥”**** 下，选择“连接字符串 - 主密钥”**** 所对应的“复制”图标并保存该值。

    ![显示如何检索连接字符串](./media/iot-hub-include-find-registryrw-connection-string/iot-hub-get-connection-vs2019.png)

有关 IoT 中心共享访问策略和权限的详细信息，请参阅[访问控制和权限](../articles/iot-hub/iot-hub-devguide-security.md#access-control-and-permissions)。
