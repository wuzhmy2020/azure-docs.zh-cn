---
title: 我在 Azure 自动化中的第一个 PowerShell 工作流 Runbook
description: 本教程指导使用 PowerShell 工作流创建、测试和发布一个简单的文本 Runbook。
keywords: powershell 工作流, powershell 工作流示例, 工作流 powershell
services: automation
ms.subservice: process-automation
ms.date: 09/24/2018
ms.topic: conceptual
ms.openlocfilehash: a5a1cad3179063f75a5d9a19567624180b5793a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79367255"
---
# <a name="my-first-powershell-workflow-runbook"></a>我的第一个 PowerShell 工作流 Runbook

> [!div class="op_single_selector"]
> * [图形](automation-first-runbook-graphical.md)
> * [电源外壳](automation-first-runbook-textual-powershell.md)
> * [PowerShell 工作流](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)

本教程指导在 Azure 自动化中创建 [PowerShell 工作流 Runbook](automation-runbook-types.md#powershell-workflow-runbooks)。 从您测试和发布的简单 Runbook 开始，同时了解如何跟踪 Runbook 作业的状态。 然后修改 Runbook 以实际管理 Azure 资源，通过启动 Azure 虚拟机进行说明。 最后，通过添加 Runbook 参数使 Runbook 更加健壮。

>[!NOTE]
>本文进行了更新，以便使用新的 Azure PowerShell Az 模块。 你仍然可以使用 AzureRM 模块，至少在 2020 年 12 月之前，它将继续接收 bug 修补程序。 若要详细了解新的 Az 模块和 AzureRM 兼容性，请参阅[新 Azure Powershell Az 模块简介](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0)。 有关混合 Runbook 辅助角色上的 Az 模块安装说明，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)。 对于自动化帐户，可以使用["如何更新 Azure 自动化 中的 Azure PowerShell"模块](automation-update-azure-modules.md)将模块更新到最新版本。

## <a name="prerequisites"></a>先决条件

要完成本教程，需要：

* Azure 订阅。 如果您还没有，您可以[激活您的 MSDN 订阅者权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或注册[一个免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。
* [自动化帐户](automation-offering-get-started.md) ，用来保存 Runbook 以及向 Azure 资源进行身份验证。 此帐户必须有权启动和停止虚拟机。
* Azure 虚拟机。 由于您停止并启动此计算机，因此它不应是生产 VM。

## <a name="step-1---create-new-runbook"></a>步骤 1 - 创建新的 Runbook

首先创建一个简单的运行手册，输出文本`Hello World`。

1. 在 Azure 门户中，打开自动化帐户。

   通过自动化帐户页面可快速查看此帐户中的资源。 应该已拥有某些资产。 这些资产大多数是自动包含在新自动化帐户中的模块。 您还应将凭据资产与您的订阅相关联。
 
1. 在 **"流程自动化**"下选择**Runbook**以打开 Runbook 列表。
1. 通过选择 **"创建运行簿"创建新的 Runbook。**
1. 将该 Runbook 命名为 **MyFirstRunbook-Workflow**。
1. 在这种情况下，您将创建一个[PowerShell 工作流运行簿](automation-runbook-types.md#powershell-workflow-runbooks)。 为**Runbook 类型**选择**PowerShell 工作流**。
1. 单击“创建”以创建 Runbook 并打开文本编辑器 **** 。

## <a name="step-2---add-code-to-the-runbook"></a>步骤 2 - 将代码添加到 Runbook

您可以直接在 Runbook 中键入代码，也可以从库控件中选择 cmdlet、Runbook 和资产，并将它们添加到具有任何相关参数的 Runbook 中。 在本教程中，您将直接在 Runbook 中键入代码。

1. Runbook 当前为空，只有必需`Workflow`的关键字、Runbook 的名称以及包含整个工作流的大括号。

   ```powershell-interactive
   Workflow MyFirstRunbook-Workflow
   {
   }
   ```

1. 在`Write-Output "Hello World"`大括号之间键入。

   ```powershell-interactive
   Workflow MyFirstRunbook-Workflow
   {
   Write-Output "Hello World"
   }
   ```

1. 通过单击“保存” **** 保存 Runbook。

## <a name="step-3---test-the-runbook"></a>步骤 3 - 测试 Runbook

在发布 Runbook 使其在生产中可用之前，应对其进行测试以确保其正常工作。 测试 Runbook 会运行其草稿版本，并允许您以交互方式查看其输出。

1. 选择 **"测试"窗格**以打开"测试"窗格。
1. 单击"**开始"** 以启动测试，并测试唯一启用的选项。
1. 请注意，将创建[Runbook 作业](automation-runbook-execution.md)，其状态将显示在窗格中。

   作业状态以 开始`Queued`，指示作业正在等待云中的 Runbook 工作人员变为可用。 状态更改为`Starting`工作人员声明作业时的状态。 最后，当 Runbook 实际开始运行时，状态变为`Running`该状态。

1. 运行簿作业完成后，"测试"窗格将显示其输出。 在这种情况下，您将看到`Hello World`。

   ![Hello World](media/automation-first-runbook-textual/test-output-hello-world.png)

1. 关闭测试窗格以返回到画布。

## <a name="step-4---publish-and-start-the-runbook"></a>步骤 4 - 发布和启动 Runbook

您创建的 Runbook 仍处于草稿模式。 必须首先发布此 Runbook，然后才能在生产中运行它。 当发布 Runbook 时，可以用草稿版本覆盖现有的已发布版本。 在本例中，还没有已发布版本，因为刚刚创建 Runbook。

1. 单击“发布”以发布该 Runbook，并在出现提示时单击“是”********。
1. 向左滚动以查看 Runbook 页中的**Runbook，** 并注意"**创作状态"** 字段设置为 **"已发布**"。
1. 向后滚动到右侧以查看**MyFirstRunbook-工作流的**页面。

   顶部的选项允许您立即启动 Runbook、安排将来的开始时间或创建[Webhook，](automation-webhooks.md)以便通过 HTTP 调用启动 Runbook。

1. 选择“启动”，并在出现提示时选择“是”******** 启动 runbook。

   ![启动 Runbook](media/automation-first-runbook-textual/automation-runbook-controls-start.png)

1. 为已创建的 Runbook 作业打开作业窗格。 在这种情况下，请保持窗格打开状态，以便可以监视作业的进度。

1. 请注意，作业状态显示在**作业摘要**中。 此状态与您在测试 Runbook 时看到的状态匹配。

   ![作业摘要](media/automation-first-runbook-textual/job-pane-status-blade-jobsummary.png)

1. 运行簿状态显示`Completed`后，单击"**输出**"。 将打开"输出"页，您可以在其中查看`Hello World`消息。

   ![作业摘要](media/automation-first-runbook-textual/job-pane-status-blade-outputtile.png)

1. 关闭“输出”页。

1. 单击“所有日志”打开 Runbook 作业的“流”窗格****。 您应该只在输出`Hello World`流中看到。 请注意，如果 Runbook 写入 runbook，则"流"窗格可以显示 Runbook 作业的其他流，例如详细和错误流。

   ![作业摘要](media/automation-first-runbook-textual/job-pane-status-blade-alllogstile.png)

1. 关闭"流"窗格和"作业"窗格以返回到 **"MyFirstRunbook"** 页。
1. 单击 **"资源**下的**作业**"以打开此 Runbook 的作业页面。 此页列出 Runbook 创建的所有作业。 您应该只看到列出的一个作业，因为您只运行该作业一次。

   ![作业](media/automation-first-runbook-textual/runbook-control-job-tile.png)

1. 单击作业名称可打开启动 Runbook 时查看的相同作业窗格。 使用此窗格可以查看为 Runbook 创建的任何作业的详细信息。

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>步骤 5 - 添加身份验证来管理 Azure 资源

已经测试并发布 Runbook，但到目前为止它不执行任何有用的操作。 需要让其管理 Azure 资源。 除非使用订阅的凭据进行身份验证，否则它无法执行此操作。 身份验证使用`Connect-AzAccount`cmdlet。

>[!NOTE]
>对于 PowerShell 运行`Add-AzAccount`簿`Add-AzureRMAccount`，并且是`Connect-AzAccount`的别名。 您可以使用这些 cmdlet，也可以将自动化帐户中的[模块更新](automation-update-azure-modules.md)到最新版本。 即使您刚刚创建了新的自动化帐户，您也可能需要更新模块。

1. 导航到**MyFirstRunbook-工作流**页面，然后单击 **"编辑**"打开文本编辑器。
2. 删除行`Write-Output`。
3. 将光标放在大括号之间的空白行上。
4. 键入或复制并粘贴以下代码，这些代码处理具有"自动运行为帐户"的身份验证。

   ```powershell-interactive
   # Ensures you do not inherit an AzContext in your runbook
   Disable-AzContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $AzureContext = Select-AzSubscription -SubscriptionId $Conn.SubscriptionID
   ```

1. 单击 **"测试"窗格**，以便可以测试 Runbook。
1. 单击“启动”以启动测试 **** 。 完成后，您应该会看到类似于以下内容的输出，显示帐户中的基本信息。 此操作是对凭据有效的确认。

   ![Authenticate](media/automation-first-runbook-textual/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>步骤 6 – 添加用于启动虚拟机的代码

现在 Runbook 正在对 Azure 订阅进行身份验证，可以管理资源。 让我们添加一个命令来启动虚拟机。 您可以在 Azure 订阅中选择任何 VM，现在您将在 Runbook 中硬编码该名称。 如果要跨多个订阅管理资源，则需要将`AzContext`参数与[Get-AzContext](/powershell/module/az.accounts/get-azcontext) cmdlet 一起使用。

1. 提供 VM 的名称和资源组名称，以便首先输入对[启动-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Start-AzVM?view=azps-3.5.0
) cmdlet 的调用，如下所示。 

   ```powershell-interactive
   workflow MyFirstRunbook-Workflow
   {
   # Ensures that you do not inherit an AzContext in your runbook
   Disable-AzContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $AzureContext = Get-AzSubscription -SubscriptionId $Conn.SubscriptionID

   Start-AzVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName' -AzContext $AzureContext
   }
   ```

1. 保存 Runbook，并单击“测试”窗格，以便可以测试 Runbook****。
1. 单击“启动”以启动测试 **** 。 完成后，检查 VM 已启动。

## <a name="step-7---add-an-input-parameter-to-the-runbook"></a>步骤 7 - 将输入参数添加到 Runbook

Runbook 当前启动在 Runbook 中硬编码的 VM。 如果可以在启动 Runbook 时指定 VM，则该 VM 将更有用。 让我们向 Runbook 添加输入参数以提供该功能。

1. 将`VMName`和`ResourceGroupName`参数的变量添加到 Runbook，并将变量与`Start-AzVM`cmdlet 一起使用，如下所示。

   ```powershell-interactive
   workflow MyFirstRunbook-Workflow
   {
    Param(
     [string]$VMName,
     [string]$ResourceGroupName
    )
   # Ensures you do not inherit an AzContext in your runbook
   Disable-AzContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzVM -Name $VMName -ResourceGroupName $ResourceGroupName
   }
   ```

2. 保存 Runbook 并打开“测试”窗格。 现在可以为测试中使用的两个输入变量提供值。
3. 关闭“测试”窗格。
4. 单击“发布”以发布 Runbook 的新版本 **** 。
5. 停止已启动的 VM。
6. 单击“启动”以启动 Runbook **ResourceGroupName** 。 
7. 键入要启动的 VM 的**VMNAME**和**RESOURCEGROUPNAME**的值。

   ![启动 Runbook](media/automation-first-runbook-textual/automation-pass-params.png)

8. 运行簿完成后，验证 VM 是否已启动。

## <a name="next-steps"></a>后续步骤

* 有关 PowerShell 的详细信息（包括语言参考和学习模块），请参阅 [PowerShell 文档](https://docs.microsoft.com/powershell/scripting/overview)。
* 要开始使用图形运行簿，请参阅[我的第一个图形运行簿](automation-first-runbook-graphical.md)。
* 要开始使用 PowerShell 运行簿，请参阅[我的第一个 PowerShell 运行簿](automation-first-runbook-textual-powershell.md)。
* 要了解有关 Runbook 类型及其优点和限制的更多信息，请参阅[Azure 自动化 Runbook 类型](automation-runbook-types.md)。
* 有关 PowerShell 脚本支持功能的详细信息，请参阅[Azure 自动化 中的本机 PowerShell 脚本支持](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)。
