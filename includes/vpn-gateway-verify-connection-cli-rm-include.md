---
title: include 文件
description: include 文件
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 0e009354e66ab13cdb9fbc3cf9e4b37e904bdfd1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "67172780"
---
可使用 [az network vpn-connection show](/cli/azure/network/vpn-connection) 命令来验证连接是否成功。 在此示例中，“ --Name”是指要测试的连接的名称。 当连接处于建立过程中时，连接状态会显示“正在连接”。 建立连接后，状态更改为“已连接”。

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```
