---
title: Azure 平台日志概述 | Microsoft Docs
description: Azure Monitor 中的日志概述提供了有关 Azure 资源操作的丰富、频繁的数据。
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/19/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 89de6b3737c8a1e91832aba8f749078806b64e90
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "77659314"
---
# <a name="overview-of-azure-platform-logs"></a>Azure 平台日志概述
平台日志提供 Azure 资源及其所依赖的 Azure 平台的详细诊断和审核信息。 它们是自动生成的，虽然你需要配置某些平台日志，以便将其转发到一个或多个目标进行保留。 本文概述了平台日志，其中包括它们提供什么信息，以及如何配置它们以方便收集和分析。

## <a name="types-of-platform-logs"></a>平台日志的类型
下表列出了在不同 Azure 层提供的特定的平台日志。

| 日志 | 层 | 描述 |
|:---|:---|:---|
| 资源日志 | Azure 资源 | 深入了解在 Azure 资源（数据平面）内执行的操作，例如，从 Key Vault 获取机密，或向数据库发出请求。** 资源日志的内容因 Azure 服务和资源类型而异。<br><br>资源日志以前称为诊断日志。**  |
| 活动日志 | Azure 订阅 | 了解从外部（管理平台）** 对订阅中的每个 Azure 资源执行的操作，以及对服务运行状况事件进行的更新。 通过活动日志，可确定订阅中资源上进行的任何写入操作 (PUT, POST, DELETE) 的“什么操作、谁操作和操作时间”等信息。______ 还可以了解该操作和其他相关属性的状态。  每个 Azure 订阅都有一个活动日志。 |
| Azure Active Directory 日志 | Azure 租户 |  包含特定租户的 Azure Active Directory 中的登录活动和更改审核日志的历史记录。 请参阅[什么是 Azure Active Directory 报表？](../../active-directory/reports-monitoring/overview-reports.md)，获取 Azure Active Directory 日志的完整说明。   |

> [!NOTE]
> Azure 活动日志主要用于在 Azure 资源管理器中发生的活动。 它不跟踪使用经典/RDFE 模型的资源。 某些经典资源类型在 Azure 资源管理器中具有代理资源提供程序（例如 Microsoft.ClassicCompute）。 如果通过 Azure 资源管理器使用这些代理资源提供程序与经典资源类型进行交互，则操作会显示在活动日志中。 如果在 Azure 资源管理器代理外部与经典资源类型进行交互，则操作只会记录在操作日志中。 可以在门户的一个单独部分中浏览操作日志。

![平台日志概述](media/platform-logs-overview/logs-overview.png)




## <a name="viewing-platform-logs"></a>查看平台日志
可以通过不同的选项查看和分析不同的 Azure 平台日志。

- 查看 Azure 门户中的活动日志，并通过 PowerShell 和 CLI 访问事件。 有关详细信息，请参阅[查看和检索 Azure 活动日志事件](activity-log-view.md)。 
- 在 Azure 门户中查看 Azure Active Directory 的安全和活动报表。 请参阅[什么是 Azure Active Directory 报告？](../../active-directory/reports-monitoring/overview-reports.md)  以获取详细信息。
- 资源日志由受支持的 Azure 资源自动生成，但这些资源不能查看，除非你将它们发送到[目标](#destinations)。 

## <a name="destinations"></a>Destinations
可以将平台日志发送到下表中的一个或多个目标，具体取决于监视要求。 通过[创建诊断设置](diagnostic-settings.md)为平台日志配置目标。

| 目标 | 方案 | 参考 |
|:---|:---|:---|:---|
| Log Analytics 工作区 | 借助其他监视数据对日志进行分析，并利用 Azure Monitor 功能（例如日志查询和警报）。 | [活动日志和资源日志](resource-logs-collect-workspace.md)<br>[Azure 活动目录日志](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md) |
| Azure 存储 | 将日志存档供审核、静态分析或备份。 |[活动日志和资源日志](archive-diagnostic-logs.md)<br>[Azure 活动目录日志](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md) |
| 事件中心 | 将日志流式传输到第三方日志记录和遥测系统。  |[活动日志和资源日志](resource-logs-stream-event-hubs.md)<br>[Azure 活动目录日志](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md) |



## <a name="next-steps"></a>后续步骤

* [查看不同类别的活动日志架构](activity-log-schema.md)
* [查看不同 Azure 服务的资源日志架构](diagnostic-logs-schema.md)
