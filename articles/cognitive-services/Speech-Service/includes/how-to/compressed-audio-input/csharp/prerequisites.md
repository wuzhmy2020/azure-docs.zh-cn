---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: dapine
ms.openlocfilehash: 07405ad0da9c9ba280810402a638395a7feb8554
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637294"
---
处理压缩音频是使用 [GStreamer](https://gstreamer.freedesktop.org) 实现的。 出于许可原因，GStreamer 二进制文件未编译并与语音 SDK 链接。 开发人员需要安装多个依赖项和插件，请参阅[在 Windows 上安装](https://gstreamer.freedesktop.org/documentation/installing/on-windows.html?gi-language=c)。 Gstreamer 二进制文件需要位于系统路径中，以便语音 SDK 可以在运行时加载 gstreamer 二进制文件。 如果语音 SDK 能够在运行时找到 libgstreamer-1.0-0.dll，则意味着 gstreamer 二进制文件位于系统路径中。

