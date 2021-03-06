---
title: 详细了解 Azure 中虚拟机规模集的业务流程模式
description: 详细了解 Azure 中虚拟机规模集的业务流程模式。
author: shandilvarun
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: vashan
ms.openlocfilehash: 4a0be30f181921461ad0bacea6f18ce439d22353
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "76279063"
---
# <a name="orchestration-mode-preview"></a>业务流程模式（预览版）

虚拟机规模集提供平台管理的虚拟机的逻辑分组。 使用规模集可以创建虚拟机配置模型，根据 CPU 或内存负载自动添加或删除其他实例，并自动升级到最新的 OS 版本。 在传统上，规模集允许使用在创建规模集时提供的 VM 配置模型来创建虚拟机，并且规模集只能管理基于配置模型隐式创建的虚拟机。

现在，使用规模集业务流程模式（预览版），可以选择是要让规模集协调在规模集配置模型外部显式创建的虚拟机，还是协调基于配置模型隐式创建的虚拟机。 缩放集业务流程模式还通过在容错域和可用性区域中部署这些 VM，帮助您设计 VM 基础结构，以获得高可用性。


虚拟机规模集支持 2 种不同的业务流程模式：

- ScaleSetVM – 添加到规模集的虚拟机实例基于规模集配置模型。 虚拟机实例生命周期（创建、更新、删除）由规模集管理。
- VM（虚拟机） – 可以在规模集之外创建的虚拟机显式添加到规模集中。 
 

> [!IMPORTANT]
> 业务流程模式是在创建规模集时定义的，以后无法更改或更新。 
> 
> 虚拟机规模集的此项功能目前以公共预览版提供。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。 
> 有关详细信息，请参阅 [Microsoft Azure 预览版补充使用条款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。


## <a name="orchestration-modes"></a>业务流程模式

|                             | "编排模式"："虚拟机"（虚拟机） | "编排模式"："缩放集VM"（虚拟机缩放设置VM） |
|-----------------------------|--------------------------------------------|--------------------------------------------------------------|
| VM 配置模型      | 无                                       | 必选 |
| 将新 VM 添加到规模集  | 创建 VM 时，会显式将 VM 添加到规模集。 | VM 是隐式创建的，将会根据 VM 配置模型、实例计数和自动缩放规则添加到规模集 | |
| 删除 VM                   | 必须单独删除 VM；如果规模集中包含任何 VM，则不会删除规模集。 | 可以单独删除 VM，删除规模集会删除所有 VM 实例。  |
| 附加/分离 VM           | 不支持                              | 不支持 |
| 实例生命周期（创建到删除） | 可以单独管理 VM 及其项目（例如磁盘和 NIC）。 | 实例及其项目（例如磁盘和 NIC）对于创建它们的规模集实例而言是隐式的。 无法在规模集的外部单独对其进行分离或管理 |
| 容错域               | 可以定义容错域。 2 或 3 基于区域支持，5 个用于可用区域。 | 可以定义从 1 到 5 的容错域 |
| 更新域              | 更新域会自动映射到容错域 | 更新域会自动映射到容错域 |
| 可用性区域          | 在一个可用性区域中支持区域部署或 VM | 支持区域部署或多个可用区域;可以定义区域平衡策略 |
| 自动缩放                   | 不支持                              | 支持 |
| OS 升级                  | 不支持                              | 支持 |
| 模型更新               | 不支持                              | 支持 |
| 实例控制            | VM 全面控制。 VM 具有完全限定的 URI，支持全面的 Azure VM 管理功能（例如 Azure Policy、Azure 备份和 Azure Site Recovery） | VM 是规模集的依赖资源。 只能通过规模集访问用于管理的实例。 |
| 实例模型              | Microsoft.Compute/VirtualMachines 模型定义。 | Microsoft.Compute/VirtualMachineScaleSets/VirtualMachines 模型定义。 |
| 容量                    | 可以创建空规模集；最多可将 200 个 VM 添加到规模集 | 可以使用实例计数 0-1000 定义规模集 |
| 移动                        | 支持                                  | 支持 |
| 单个位置组 == false | 不支持                          | 支持 |


## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅[可用性选项 概述](availability.md)。
