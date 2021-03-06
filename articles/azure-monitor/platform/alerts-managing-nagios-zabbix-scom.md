---
title: 在 Azure Monitor 中管理来自 System Center Operations Manager、Zabbix 和 Nagios 的警报
description: 在 Azure Monitor 中管理来自 System Center Operations Manager、Zabbix 和 Nagios 的警报
ms.topic: conceptual
ms.date: 09/24/2018
ms.subservice: alerts
ms.openlocfilehash: 5ef03f522fb91302bfdec985f1e3c5cf8138ae97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "77667442"
---
# <a name="manage-alerts-from-system-center-operations-manager-zabbix-and-nagios-in-azure-monitor"></a>在 Azure Monitor 中管理来自 System Center Operations Manager、Zabbix 和 Nagios 的警报

现在，可以在 [Azure Monitor](https://aka.ms/azure-alerts-overview) 中查看来自 Nagios、Zabbix 和 System Center Operations Manager 的警报。 这些警报来自 Log Analytics 与 Nagios/Zabbix 服务器或 System Center Operations Manager 的集成。 

## <a name="prerequisites"></a>先决条件
Log Analytics 存储库中类型为 Alert 的任何记录都将导入到 Azure Monitor 中，因此，你必须执行收集这些记录所需的配置。
1. 对于**Nagios**和**Zabbix**警报，[将这些服务器配置为](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents)向日志分析[发送警报](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-alerts-nagios-zabbix?toc=%2Fazure%2Fazure-monitor%2Ftoc.json)。
1. 对于 **System Center Operations Manager** 警报，[将 Operations Manager 管理组连接到 Log Analytics 工作区](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents)。 此后，部署来自 Azure 解决方案市场的[警报管理](https://docs.microsoft.com/azure/azure-monitor/platform/alert-management-solution)解决方案。 部署完成后，在 System Center Operations Manager 中创建的任何警报都将导入到 Log Analytics 中。

## <a name="view-your-alert-instances"></a>查看警报实例
在配置了到 Log Analytics 中的导入后，可以开始在 [Azure Monitor](https://aka.ms/azure-alerts-overview) 中查看来自这些监视服务的警报实例。 这些警报实例出现在 Azure Monitor 中后，你可以[管理警报实例](https://aka.ms/managing-alert-instances)、[管理基于这些警报创建的智能组](https://aka.ms/managing-smart-groups)以及[更改警报和智能组的状态](https://aka.ms/managing-alert-smart-group-states)。

> [!NOTE]
>  1. 此解决方案仅允许在 Azure Monitor 中查看 System Center Operations Manager/Zabbix/Nagios 触发的警报实例。 警报规则配置只能在相应的监视工具中查看/编辑。 
>  1. 所有已触发的警报实例都将在 Azure Monitor 和 Azure Log Analytics 中提供。 目前，无法在两者之间进行选择，也无法仅引入已触发的特定警报。
>  1. 来自 System Center Operations Manager、Zabbix 和 Nagios 的所有警报的信号类型都是“未知”，因为基础遥测类型不可用。
>  1. Nagios 警报不具有状态 - 例如，警报的[监视条件](https://aka.ms/azure-alerts-overview)将不会从“已触发”变为“已解决”。 而是，“已触发”和“已解决”都显示为单独的警报实例。 

