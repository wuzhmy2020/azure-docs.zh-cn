---
title: 安全控件
description: 用于评估 Azure 资源管理器服务的内置安全控制的清单。
ms.topic: conceptual
ms.date: 09/04/2019
ms.openlocfilehash: d0a0625153e428a0d261e52d40b31ef5142eddfd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "75485619"
---
# <a name="security-controls-for-azure-resource-manager"></a>Azure 资源管理器的安全控制

本文介绍 Azure 资源管理器中内置的安全控制。

[!INCLUDE [Security controls Header](../../../includes/security-controls-header.md)]

## <a name="data-protection"></a>数据保护

| 安全控制 | Yes/No | 说明 |
|---|---|--|
| 服务器端静态加密：微软管理的密钥 | 是 |  |
| 传输中加密（例如 ExpressRoute 加密、VNet 中加密，以及 VNet-VNet 加密）| 是 | HTTPS/TLS。 |
| 服务器端静态加密：客户管理的密钥 (BYOK) | 空值 | Azure 资源管理器不存储客户内容，仅存储控制数据。 |
| 列级加密（Azure 数据服务）| 是 | |
| 加密的 API 调用| 是 | |

## <a name="network"></a>网络

| 安全控制 | Yes/No | 说明 |
|---|---|--|
| 服务终结点支持| 否 | |
| VNet 注入支持| 是 | |
| 网络隔离和防火墙支持| 否 |  |
| 强制隧道支持| 否 |  |

## <a name="monitoring--logging"></a>监视和日志记录

| 安全控制 | Yes/No | 说明|
|---|---|--|
| Azure 监视支持（Log Analytics、App Insights 等）| 否 | |
| 控制和管理平面日志记录和审核| 是 | 活动日志公开对资源执行的所有写入操作（PUT、POST、DELETE）；请参阅[查看活动日志来审核对资源的操作](view-activity-logs.md)。 |
| 数据平面日志记录和审核| 空值 | |

## <a name="identity"></a>标识

| 安全控制 | Yes/No | 说明|
|---|---|--|
| 身份验证| 是 | 基于 [Azure Active Directory](/azure/active-directory)。|
| 授权| 是 | |

## <a name="configuration-management"></a>配置管理

| 安全控制 | Yes/No | 说明|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 是 |  |

## <a name="next-steps"></a>后续步骤

- 详细了解 Azure[服务中的内置安全控件](../../security/fundamentals/security-controls.md)。