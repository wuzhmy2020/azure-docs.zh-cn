---
title: Azure 媒体服务中的 LiveEvent 状态和计费 | Microsoft Docs
description: 本主题概述了 Azure 媒体服务 LiveEvent 状态和计费。
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 10/24/2019
ms.author: juliako
ms.openlocfilehash: e4eee3b9f3f97bf7cd7a7b61425ec5c9a3a198ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "76543069"
---
# <a name="live-event-states-and-billing"></a>直播活动状态和计费

在 Azure 媒体服务中，一旦直播活动的状态转换为**正在运行**，就会开始计费。 即使没有视频流经服务，您也会被计费。 若要停止对直播活动的计费，必须停止直播活动。 实时转录的计费方式与实时事件相同。

将[直播活动](https://docs.microsoft.com/rest/api/media/liveevents)上的 **LiveEventEncodingType** 设置为 Standard 或 Premium1080p 后，媒体服务就会自动关闭在输入源丢失 12 小时后仍处于**正在运行**状态但却没有**实时输出**运行的直播活动。 但是，在直播活动处于**正在运行**状态的时间段内，仍会进行计费。

> [!NOTE]
> 直通直播活动不会自动关闭，必须通过 API 显式停止，以避免过多计费。 

## <a name="states"></a>States

直播活动可能会处于以下任一状态。

|状态|描述|
|---|---|
|**停止**| 这是创建后实时事件的初始状态（除非自动启动设置为 true。此状态下不发生计费。 在此状态下，可以更新直播活动属性，但不允许进行流式处理。|
|**开始**| 正在启动直播活动并分配资源。 此状态下不会发生计费。 此状态下不允许进行更新或流式处理。 如果发生错误，则直播活动会返回到“已停止”状态。|
|**正在运行**| 已分配了直播活动资源，已生成了引入和预览 URL，并且能够接收实时流。 此时，计费处于活动状态。 必须显式对直播活动资源调用停止操作才能停止进一步计费。|
|**正在停止**| 正在停止直播活动并解除预配资源。 此暂时性状态下不会发生计费。 此状态下不允许进行更新或流式处理。|
|**删除**| 正在删除直播活动。 此暂时性状态下不会发生计费。 此状态下不允许进行更新或流式处理。|

您可以选择在创建实时事件时启用实时转录。 如果这样做，每当实时事件处于 **"正在运行"** 状态时，您都会为实时转录收费。 请注意，即使没有音频流经实时事件，您也会被计费。

## <a name="next-steps"></a>后续步骤

- [实时传送视频流概述](live-streaming-overview.md)
- [实时传送视频流教程](stream-live-tutorial-with-api.md)
