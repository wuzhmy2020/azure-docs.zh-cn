---
title: 将托管磁盘复制到订阅 - PowerShell 示例
description: Azure PowerShell 脚本示例 - 将托管磁盘复制（或移动）到同一或不同订阅
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/06/2017
ms.author: ramankum
ms.openlocfilehash: 9e655ce33c8952acab7228f4caae81f09c2d56ba
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "75375893"
---
# <a name="copy-managed-disks-in-the-same-subscription-or-different-subscription-with-powershell"></a>使用 PowerShell 将托管磁盘复制到同一订阅或不同订阅

此脚本在同一订阅或不同订阅中创建现有托管磁盘的副本。 将在与父托管磁盘相同的区域中创建新磁盘。

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>示例脚本

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.ps1 "Copy managed disk")]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令并使用源托管磁盘的 Id 在目标订阅中创建新托管磁盘。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [New-AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/New-AzDiskConfig) | 创建用于创建磁盘的磁盘配置。 它包括父磁盘的资源 Id 以及与父磁盘的位置相同的位置。  |
| [New-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/New-AzDisk) | 使用作为参数传递的磁盘配置、磁盘名称和资源组名称创建磁盘。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](/powershell/azure/overview)。

可以在 [Azure Linux VM 文档](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机 PowerShell 脚本示例。