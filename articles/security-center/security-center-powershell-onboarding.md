---
title: 使用 PowerShell 连接到 Azure 安全中心
description: 本文档将指导你完成通过 PowerShell cmdlet 开始使用 Azure 安全中心的过程。
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: e400fcbf-f0a8-4e10-b571-5a0d0c3d0c67
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/02/2018
ms.author: memildin
ms.openlocfilehash: b471fbb62862cd48ebbb239d65b563aa109ef629
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2020
ms.locfileid: "80435478"
---
# <a name="automate-onboarding-of-azure-security-center-using-powershell"></a>使用 PowerShell 自动载入 Azure 安全中心

通过 Azure 安全中心 PowerShell 模块，能够以编程方式保护 Azure 工作负载的安全。
借助 PowerShell，可以自动执行任务，并可避免手动任务中固有的人为错误。 对于涉及几十个资源且具有数百乃至数千种资源的大规模部署，这一方法尤为有用 – 所有这些必须从一开始就受到保护。

通过 PowerShell 使用 Azure 安全中心，使你能够以编程方式自动开始使用 Azure 资源并对其进行管理，以及添加必要的安全控件。

本文提供了一个示例 PowerShell 脚本，可在你的环境中进行修改和使用，以跨订阅推出安全中心。 

在此示例中，通过实施安全中心的标准层，提供高级威胁防护和检测功能，我们将对 ID 为 d07c0080-170c-4c24-861d-9c817742786c 的订阅启用安全中心，并应用提供高级别保护的建议设置：

1. 设置[安全中心标准保护级别](https://azure.microsoft.com/pricing/details/security-center/)。 
 
2. 设置日志分析工作区，日志分析代理将在与订阅关联的 VM 上发送其收集的数据 - 在此示例中，现有用户定义的工作区（my工作区）。

3. 激活安全中心的自动代理预配，部署[日志分析代理](security-center-enable-data-collection.md#auto-provision-mma)。

5. 将组织的[CISO 设置为安全中心警报和显著事件的安全联系人](security-center-provide-security-contact-details.md)。

6. 分配安全中心的[默认安全策略](tutorial-security-policy.md)。

## <a name="prerequisites"></a>先决条件

这些步骤应在运行安全中心 cmdlet 前执行：

1.  以管理员身份运行 PowerShell。
2.  在 PowerShell 中运行以下命令：
      
        Set-ExecutionPolicy -ExecutionPolicy AllSigned
        Install-Module -Name Az.Security -Force

## <a name="onboard-security-center-using-powershell"></a>通过 PowerShell 开始使用安全中心

1.  将你的订阅注册到安全中心资源提供程序：

        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
        Register-AzResourceProvider -ProviderNamespace 'Microsoft.Security' 

2.  可选：设置订阅的覆盖范围级别（定价层）（如未定义，则定价层被设置为“免费”）：

        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
        Set-AzSecurityPricing -Name "default" -PricingTier "Standard"

3.  配置代理将报告的 Log Analytics 工作区。 必须具有一个已创建的 Log Analytics 工作区，订阅的虚拟机将向其报告。 你可以定义向同一工作区报告的多个订阅。 如未定义，则将使用默认工作区。

        Set-AzSecurityWorkspaceSetting -Name "default" -Scope
        "/subscriptions/d07c0080-170c-4c24-861d-9c817742786c" -WorkspaceId"/subscriptions/d07c0080-170c-4c24-861d-9c817742786c/resourceGroups/myRg/providers/Microsoft.OperationalInsights/workspaces/myWorkspace"

4.  在 Azure VM 上自动预配日志分析代理：
    
        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
    
        Set-AzSecurityAutoProvisioningSetting -Name "default" -EnableAutoProvision

    > [!NOTE]
    > 建议启用自动预配，以确保 Azure 虚拟机自动受到 Azure 安全中心的保护。
    >

5.  可选：强烈建议为你使用的订阅定义安全联系人详细信息，该信息将被用作接收安全中心所生成警报和通知的收件人：

        Set-AzSecurityContact -Name "default1" -Email "CISO@my-org.com" -Phone "2142754038" -AlertAdmin -NotifyOnAlert 

6.  分配默认安全中心策略计划：

        Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
        $Policy = Get-AzPolicySetDefinition | where {$_.Properties.displayName -EQ '[Preview]: Enable Monitoring in Azure Security Center'}
        New-AzPolicyAssignment -Name 'ASC Default <d07c0080-170c-4c24-861d-9c817742786c>' -DisplayName 'Security Center Default <subscription ID>' -PolicySetDefinition $Policy -Scope '/subscriptions/d07c0080-170c-4c24-861d-9c817742786c'

至此，你已通过 PowerShell 成功载入 Azure 安全中心！

现在可以将这些 PowerShell cmdlet 与自动化脚本结合使用，从而以编程方式循环访问订阅和资源。 这可节省时间并减少人为错误的可能性。 你可以使用此[示例脚本](https://github.com/Microsoft/Azure-Security-Center/blob/master/quickstarts/ASC-Samples.ps1)作为参考。






## <a name="see-also"></a>请参阅
若要详细了解如何通过 PowerShell 来自动开始使用安全中心，请参阅以下文章：

* [Az.Security](https://docs.microsoft.com/powershell/module/az.security)。

若要详细了解安全中心，请参阅以下文章：

* [在 Azure 安全中心设置安全策略](tutorial-security-policy.md)-- 了解如何为 Azure 订阅和资源组配置安全策略。
* [管理和响应 Azure 安全中心中的安全警报](security-center-managing-and-responding-alerts.md)-- 了解如何管理和响应安全警报。