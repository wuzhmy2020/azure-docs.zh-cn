---
title: 创建诊断设置以收集 Azure 中的日志和指标
description: 创建诊断设置，以将 Azure 平台日志转发到 Azure Monitor 日志、Azure 存储或 Azure 事件中心。
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 6ff9e62ed145b7892d2997193dae8e171ae49c98
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2020
ms.locfileid: "80585383"
---
# <a name="create-diagnostic-setting-to-collect-platform-logs-and-metrics-in-azure"></a>创建诊断设置以收集 Azure 中的平台日志和指标
Azure 中的[平台日志](platform-logs-overview.md)（包括 Azure 活动日志和资源日志）提供 Azure 资源及其所依赖的 Azure 平台的详细诊断和审核信息。 本文详细介绍如何创建和配置诊断设置，以将平台日志发送到不同的目标。

> [!IMPORTANT]
> 应先禁用任何旧的配置，然后再创建用于收集活动日志的诊断设置。 有关详细信息，请参阅[通过旧版设置收集 Azure 活动日志](diagnostic-settings-legacy.md)。

每个 Azure 资源都需有自身的诊断设置，其设置定义了以下内容：

- 发送到设置中所定义目标的日志和指标数据的类别。 不同资源类型的可用类别各不相同。
- 要将日志发送到的一个或多个目标。 当前目标包括 Log Analytics 工作区、事件中心和 Azure 存储。
 
一个诊断设置只能为每个目标定义一种类型。 若要将数据发送到多个特定的目标类型（例如，两个不同的 Log Analytics 工作区），请创建多个设置。 每个资源最多可以有 5 个诊断设置。


> [!NOTE]
> [平台指标](metrics-supported.md)自动收集到 [Azure Monitor 指标](data-platform-metrics.md)中。 使用诊断设置可将特定 Azure 服务的指标收集到 Azure Monitor 日志中，以使用[日志查询](../log-query/log-query-overview.md)结合其他监视数据进行分析。

## <a name="destinations"></a>Destinations 
平台日志可以发送到下表中列出的目标。 每个目标的配置是使用本文所述的创建诊断设置的相同过程执行的。 有关将数据发送到该目标的详细信息，请参阅下表中的每个链接。

| 目标 | 描述 |
|:---|:---|
| [日志分析工作区](resource-logs-collect-workspace.md) | 将日志收集到 Log Analytics 工作区可以使用强大的日志查询结合 Azure Monitor 收集的其他监视数据对其进行分析，并利用其他 Azure Monitor 功能，例如警报和可视化。 |
| [事件中心](resource-logs-stream-event-hubs.md) | 向事件中心发送日志可将数据流式传输到外部系统，例如第三方 SIEM 和其他日志分析解决方案。 |
| [Azure 存储帐户](resource-logs-collect-storage.md) | 将日志存档到 Azure 存储帐户有助于审核、静态分析或备份。 |

## <a name="create-diagnostic-settings-in-azure-portal"></a>在 Azure 门户中创建诊断设置
可以在 Azure 门户中通过“Azure Monitor”菜单或资源菜单配置诊断设置。

1. 在 Azure 门户中配置诊断设置的位置取决于资源。

   - 对于单项资源，在资源菜单中的“监视器”下，单击“诊断设置”。********

        ![诊断设置](media/diagnostic-settings/menu-resource.png)

    - 对于一项或多项资源，在 Azure Monitor 菜单中，单击“设置”下的“诊断设置”，然后单击相应资源。********
    
        ![诊断设置](media/diagnostic-settings/menu-monitor.png)

    - 对于活动日志，在“Azure Monitor”菜单中，单击“活动日志”，然后单击“诊断设置”。************ 请确保禁用活动日志的任何旧配置。 有关详细信息，请参阅[禁用现有设置](/azure/azure-monitor/platform/activity-log-collect#collecting-activity-log)。

        ![诊断设置](media/diagnostic-settings/menu-activity-log.png)

2. 如果选定的资源上不存在任何设置，系统会提示创建设置。 单击“添加诊断设置”****。

   ![添加诊断设置 - 没有现有的设置](media/diagnostic-settings/add-setting.png)

   如果资源上有现有的设置，则会看到已配置的设置列表。 单击“添加诊断设置”以添加新设置，或单击“编辑设置”以编辑现有设置。******** 每个设置最多只能包含一个目标类型。

   ![添加诊断设置 - 现有的设置](media/diagnostic-settings/edit-setting.png)

3. 为设置指定名称（如果未指定）。
4. 选中要将日志发送到的每个目标对应的框。 单击“配置”并根据下表中所述指定其设置。****

    | 设置 | 描述 |
    |:---|:---|
    | Log Analytics 工作区 | 工作区的名称。 |
    | 存储帐户 | 存储帐户的名称。 |
    | 事件中心命名空间 | 要在其中创建事件中心的命名空间（如果这是首次流式传输日志）或要将日志流式传输到的命名空间（如果已有资源将该日志类别流式传输到此命名空间）。
    | 事件中心名称 | （可选）在设置中指定要将所有数据发送到的事件中心名称。 如果未指定名称，将为每个日志类别创建一个事件中心。 如果发送多个类别，可能需要指定一个名称来限制创建的事件中心数。 有关详细信息，请参阅 [Azure 事件中心配额和限制](../../event-hubs/event-hubs-quotas.md)。 |
    | 事件中心策略名称 | 定义流式传输机制拥有的权限。 |

    ![添加诊断设置 - 现有的设置](media/diagnostic-settings/setting-details.png)

5. 选中要发送到指定目标的每个数据类别对应的框。 每种 Azure 服务的类别列表有所不同。

   > [!NOTE]
   > 当前不支持通过诊断设置发送多维指标。 多维指标将按平展后的单维指标导出，并跨维值聚合。
   >
   > ** 例如：可以基于每个队列级别浏览和绘制事件中心上的“传入消息”指标。 但是，当通过诊断设置导出时，该指标将表示为事件中心的所有队列中的所有传入消息。

6. 单击“保存”  。

片刻之后，新设置会显示在此资源的设置列表中，生成新的事件数据后，日志会立即流式传输到指定的目标。 请注意，发出事件后可能需要最多 15 分钟的时间该事件才会[出现在 Log Analytics 工作区中](data-ingestion-time.md)。



## <a name="create-diagnostic-settings-using-powershell"></a>使用 PowerShell 创建诊断设置
在 [Azure PowerShell](powershell-quickstart-samples.md) 中使用 [Set-AzDiagnosticSetting](https://docs.microsoft.com/powershell/module/az.monitor/set-azdiagnosticsetting) cmdlet 创建诊断设置。 有关参数说明，请参阅此 cmdlet 的文档。

> [!IMPORTANT]
> 不能将此方法用于 Azure 活动日志。 请改为利用[使用资源管理器模板在 Azure Monitor 中创建诊断设置](diagnostic-settings-template.md)，创建资源管理器模板并使用 PowerShell 进行部署。

以下示例 PowerShell cmdlet 使用所有三个目标创建诊断设置。


```powershell
Set-AzDiagnosticSetting -Name KeyVault-Diagnostics -ResourceId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault -Category AuditEvent -MetricCategory AllMetrics -Enabled $true -StorageAccountId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount -WorkspaceId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/myworkspace  -EventHubAuthorizationRuleId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.EventHub/namespaces/myeventhub/authorizationrules/RootManageSharedAccessKey
```


## <a name="create-diagnostic-settings-using-azure-cli"></a>使用 Azure CLI 创建诊断设置
在 [Azure CLI](https://docs.microsoft.com/cli/azure/monitor?view=azure-cli-latest) 中使用 [az monitor diagnostic-settings create](https://docs.microsoft.com/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az-monitor-diagnostic-settings-create) 命令创建诊断设置。 有关参数说明，请参阅此命令的文档。

> [!IMPORTANT]
> 不能将此方法用于 Azure 活动日志。 相反，[请使用使用资源管理器模板在 Azure 监视器中创建诊断设置](diagnostic-settings-template.md)来创建资源管理器模板，并将其与 CLI 一起部署。

以下示例 CLI 命令使用所有三个目标创建诊断设置。



```azurecli
az monitor diagnostic-settings create  \
--name KeyVault-Diagnostics \
--resource /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault \
--logs    '[{"category": "AuditEvent","enabled": true}]' \
--metrics '[{"category": "AllMetrics","enabled": true}]' \
--storage-account /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount \
--workspace /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/myworkspace \
--event-hub-rule /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.EventHub/namespaces/myeventhub/authorizationrules/RootManageSharedAccessKey
```

### <a name="configure-diagnostic-settings-using-rest-api"></a>使用 REST API 配置诊断设置
若要使用 [Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/) 创建或更新诊断设置，请参阅[诊断设置](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)。


### <a name="configure-diagnostic-settings-using-resource-manager-template"></a>使用资源管理器模板配置诊断设置
若要使用资源管理器模板创建或更新诊断设置，请参阅[使用资源管理器模板在 Azure Monitor 中创建诊断设置](diagnostic-settings-template.md)。

## <a name="next-steps"></a>后续步骤

* [详细了解 Azure 平台日志](platform-logs-overview.md)
