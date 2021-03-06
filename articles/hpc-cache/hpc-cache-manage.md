---
title: 管理和更新 Azure HPC 缓存
description: 如何使用 Azure 门户管理和更新 Azure HPC 缓存
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 1/29/2020
ms.author: rohogue
ms.openlocfilehash: da260074fc69fac9e98d3698bb2d40fdf80d7118
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "77252036"
---
# <a name="manage-your-cache-from-the-azure-portal"></a>从 Azure 门户管理缓存

Azure 门户中的缓存概览页显示缓存的项目详细信息、缓存状态和基本统计信息。 它还具有停止或启动缓存、删除缓存、将数据刷新到长期存储以及更新软件的控件。

要打开概览页面，请在 Azure 门户中选择缓存资源。 例如，加载 **"所有资源"** 页并单击缓存名称。

![Azure HPC 缓存实例的概述页面的屏幕截图](media/hpc-cache-overview.png)

页面顶部的按钮可以帮助您管理缓存：

* **启动**和[**停止**](#stop-the-cache)- 挂起缓存操作
* [**刷新**](#flush-cached-data)- 将更改的数据写入存储目标
* [**升级**](#upgrade-cache-software)- 更新缓存软件
* **刷新**- 重新加载概览页面
* [**删除**](#delete-the-cache)- 永久销毁缓存

阅读下面有关这些选项的详细信息。

## <a name="stop-the-cache"></a>停止缓存

您可以停止缓存以在非活动期间降低成本。 在停止缓存时，不会为中断时间付费，但会为缓存分配的磁盘存储付费。 （有关详细信息，请参阅[定价](https://aka.ms/hpc-cache-pricing)页面。

已停止的缓存不响应客户端请求。 在停止缓存之前，应卸载客户端。

"**停止"** 按钮将挂起活动缓存。 当缓存的状态**为"正常**"或 **"已降级"** 时，"**停止"** 按钮可用。

![顶部按钮的屏幕截图，其中突出显示了"停止"，并弹出了一条描述停止操作的弹出消息，并询问"是否要继续？ 带"是"（默认）和"否"按钮](media/stop-cache.png)

单击"是"以确认停止缓存后，缓存会自动将其内容刷新到存储目标。 此过程可能需要一些时间，但它可确保数据一致性。 最后，缓存状态更改为 **"已停止**"。

要重新激活已停止的缓存，请单击"**开始"** 按钮。 无需确认。

![突出显示"开始"的顶部按钮的屏幕截图](media/start-cache.png)

## <a name="flush-cached-data"></a>刷新缓存的数据

概览页上的 **"刷新"** 按钮告诉缓存立即将缓存中存储的所有已更改数据写入后端存储目标。 缓存会定期将数据保存到存储目标，因此，除非您希望确保后端存储系统是最新的，否则无需手动执行此操作。 例如，在拍摄存储快照或检查数据集大小之前，可以使用**Flush。**

> [!NOTE]
> 在刷新过程中，缓存无法为客户端请求提供服务。 缓存访问将挂起并在操作完成后继续。

![顶部按钮的屏幕截图，其中突出显示了 Flush，并弹出了一条描述刷新操作并询问"是否要继续？ 带"是"（默认）和"否"按钮](media/hpc-cache-flush.png)

启动缓存刷新操作时，缓存将停止接受客户端请求，概览页上的缓存状态将更改为**法拉盛**。

缓存中的数据将保存到相应的存储目标。 根据需要刷新的数据量，该过程可能需要几分钟或更长时间。

将所有数据保存到存储目标后，缓存将自动开始再次获取客户端请求。 缓存状态返回到 **"正常**"。

## <a name="upgrade-cache-software"></a>升级缓存软件

如果新软件版本可用，**升级**按钮将变为活动状态。 您还应在页面顶部看到有关更新软件的消息。

![启用了"升级"按钮的按钮顶行屏幕截图](media/hpc-cache-upgrade-button.png)

在软件升级期间，客户端访问不会中断，但缓存性能会降低。 计划在非高峰使用时间或计划维护期间升级软件。

软件更新可能需要几个小时。 配置具有更高吞吐量的缓存比峰值吞吐量值较小的缓存需要更长的时间来升级。

当软件升级可用时，您将有一周左右时间手动应用。 结束日期列在升级消息中。 如果在此期间不升级，Azure 将自动将更新应用于缓存。 自动升级的时间不可配置。 如果您担心缓存性能的影响，则应在时间段到期之前自行升级软件。

如果缓存在结束日期过去时停止，则缓存将在下次启动时自动升级软件。 （更新可能不会立即启动，但将在第一个小时内启动。

单击 **"升级**"按钮开始软件更新。 缓存状态更改为 **"升级**"，直到操作完成。

## <a name="delete-the-cache"></a>删除缓存

"**删除**"按钮将销毁缓存。 删除缓存时，其所有资源都将销毁，不再产生帐户费用。

删除缓存时，用作存储目标的后端存储卷不受影响。 您可以稍后将它们添加到将来的缓存中，也可以将它们分别停用。

> [!NOTE]
> Azure HPC 缓存在删除缓存之前不会自动将更改的数据从缓存写入后端存储系统。
>
> 为了确保缓存中的所有数据已写入长期存储，请[停止缓存](#stop-the-cache)，然后再将其删除。 在单击删除按钮之前，请确保它显示状态 **"已停止**"。
<!--... written to long-term storage, follow this procedure:
>
> 1. [Remove](hpc-cache-edit-storage.md#remove-a-storage-target) each storage target from the Azure HPC Cache by using the delete button on the Storage targets page. The system automatically writes any changed data from the cache to the back-end storage system before removing the target.
> 1. Wait for the storage target to be completely removed. The process can take an hour or longer if there is a lot of data to write from the cache. When it is done, a portal notification says that the delete operation was successful, and the storage target disappears from the list.
> 1. After all affected storage targets have been deleted, it is safe to delete the cache.
>
> Alternatively, you can use the [flush](#flush-cached-data) option to save cached data, but there is a small risk of losing work if a client writes a change to the cache after the flush completes but before the cache instance is destroyed.-->

## <a name="cache-metrics-and-monitoring"></a>缓存指标和监视

概览页显示一些基本缓存统计信息的图形 - 缓存吞吐量、每秒操作和延迟。

![显示上述示例缓存的统计信息的三个线图的屏幕截图](media/hpc-cache-overview-stats.png)

这些图表是 Azure 内置监视和分析工具的一部分。 门户侧边栏中的 **"监视"** 标题下的页面中提供了其他工具和警报。 在[Azure 监视文档](../azure-monitor/insights/monitor-azure-resource.md#monitoring-in-the-azure-portal)的门户部分中了解更多信息。

## <a name="next-steps"></a>后续步骤

<!-- * Learn more about metrics and statistics for hpc cache -->
* 了解有关[Azure 指标和统计信息工具的更多](../azure-monitor/index.yml)
* 获取有关[Azure HPC 缓存的帮助](hpc-cache-support-ticket.md)
