---
title: 配置 Always-On VPN 用户隧道
titleSuffix: Azure VPN Gateway
description: 本文介绍如何为 VPN 网关配置 Always On VPN 用户隧道
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/12/2020
ms.author: cherylmc
ms.openlocfilehash: 56934dd13661d8f623e673e2817e87618675c7ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79502272"
---
# <a name="configure-an-always-on-vpn-user-tunnel"></a>配置 Always On VPN 用户隧道

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="configure-the-gateway"></a>配置网关

 使用["配置点对点 VPN 连接"一](vpn-gateway-howto-point-to-site-resource-manager-portal.md)文中的说明将 VPN 网关配置为使用 IKEv2 和基于证书的身份验证。

## <a name="configure-a-user-tunnel"></a>配置用户隧道

[!INCLUDE [user configuration](../../includes/vpn-gateway-vwan-always-on-user.md)]

## <a name="to-remove-a-profile"></a>删除配置文件

要删除配置文件，请使用以下步骤：

1. 运行以下命令：

   ```powershell
   C:\> Remove-VpnConnection UserTest  
   ```

1. 断开连接，清除“自动连接”复选框。****

   ![清理](./media/vpn-gateway-howto-always-on-user-tunnel/disconnect.jpg)

## <a name="next-steps"></a>后续步骤

若要排查可能会发生的任何连接问题，请参阅 [Azure 点到站点连接问题](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md)。
