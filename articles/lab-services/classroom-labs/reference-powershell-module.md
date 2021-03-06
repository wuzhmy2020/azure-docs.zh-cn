---
title: 用于 Azure 实验室服务的 PowerShell 模块 |微软文档
description: 本文提供有关 PowerShell 模块的信息，该模块可帮助管理 Azure 实验室服务中的项目。
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2019
ms.author: spelluru
ms.openlocfilehash: cc82355ee43f9fee4f9c2e1bb1bcc0481e4dcea3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "73609393"
---
# <a name="azlabservices-powershell-module-preview"></a>Az.LabServices PowerShell 模块（预览版）
Az.LabServices 是一个 PowerShell 模块，可简化 Azure 实验室服务的管理。 它提供了可组合功能来创建、查询、更新和删除实验室帐户、实验室、VM 和图像。 有关此模块的详细信息，请参阅[GitHub 上的 Az.LabServices 主页](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Modules/Library)。

> [!NOTE]
> 此模块处于**预览版**。 

## <a name="example-command"></a>示例命令
下面是使用 PowerShell 命令停止所有实验室中所有正在运行的 VM 的示例。

```powershell
Get-AzLabAccount | Get-AzLab | Get-AzLabVm -Status Running | Stop-AzLabVm
```

## <a name="get-started"></a>入门
1. 如果[计算机上不存在 Azure PowerShell，](https://docs.microsoft.com/powershell/azure/overview)请安装它。 
2. 将[Az.LabServices.psm1](https://github.com/Azure/azure-devtestlab/blob/master/samples/ClassroomLabs/Modules/Library/Az.LabServices.psm1)下载到您的计算机。
3. 导入模块：

    ```powershell
    Import-Module .\Az.LabServices.psm1
    ```
4. 运行以下命令以列出订阅中的所有实验室。

    ```powershell
    Get-AzLabAccount | Get-AzLab
    ```

## <a name="next-steps"></a>后续步骤
请参阅[GitHub 上的 Az.LabServices 主页](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Modules/Library)。
