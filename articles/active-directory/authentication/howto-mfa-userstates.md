---
title: 每用户多重身份验证 - Azure 活动目录
description: 通过更改 Azure 多重身份验证中的用户状态启用 MFA。
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: e955a3d10eea0a23154396a44fe6e1fc552a9ccb
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2020
ms.locfileid: "80653479"
---
# <a name="how-to-require-two-step-verification-for-a-user"></a>如何要求对用户进行双重验证

可以采用两种方法之一要求进行双重验证，这两种方法都需要使用全局管理员帐户。 第一个选项是为每个用户启用 Azure 多重身份验证 (MFA)。 逐个为用户启用此功能后，他们每次登录时都会执行双重验证（有一些例外情况，例如，当他们从受信任的 IP 地址登录时，或者开启了“记忆的设备”__ 功能时）。 第二个选项是设置条件访问策略，该策略需要在特定条件下进行两步验证。

> [!TIP]
> 建议使用条件访问策略启用 Azure 多重身份验证。 不再建议更改用户状态，除非您的许可证不包含条件访问，因为它要求用户在每次登录时执行 MFA。

## <a name="choose-how-to-enable"></a>选择启用方法

**通过更改用户状态启用** - 这是需要进行双重验证的传统方法，本文将对此进行讨论。 它适用于云中的 Azure MFA 以及 Azure MFA 服务器。 使用此方法要求用户**在每次**登录并覆盖条件访问策略时执行两步验证。

**由条件访问策略启用**- 这是为用户启用两步验证的最灵活方法。 启用"使用条件访问"策略仅适用于云中的 Azure MFA，是 Azure AD 的高级功能。 有关此方法的详细信息，请参阅[部署基于云的 Azure 多重身份验证](howto-mfa-getstarted.md)。

通过“Azure AD 标识保护”启用 - 此方法使用“Azure AD 标识保护”风险策略，要求仅基于所有云应用程序的登录风险进行双重验证。**** 此方法需要 Azure Active Directory P2 授权。 有关此方法的详细信息，请参阅[Azure 活动目录标识保护](../identity-protection/howto-sign-in-risk-policy.md)

> [!Note]
> 有关许可和定价的详细信息，请参见 [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/
) 和[多重身份验证](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)定价页。

## <a name="enable-azure-mfa-by-changing-user-state"></a>通过更改用户状态启用 Azure MFA

Azure 多重身份验证中的用户帐户具有以下三种不同状态：

> [!IMPORTANT]
> 通过条件访问策略启用 Azure MFA 不会更改用户的状态。 不要惊慌用户显示为禁用。 条件访问不会更改状态。 **如果用户正在使用条件访问策略，则组织不应启用或强制用户。**

| 状态 | 说明 | 受影响的非浏览器应用 | 受影响的浏览器应用 | 新式身份验证受影响 |
|:---:| --- |:---:|:--:|:--:|
| 已禁用 | 没有在 Azure MFA 中注册某个新用户的默认状态。 | 否 | 否 | 否 |
| 已启用 | 用户已加入 Azure MFA 但尚未注册。 在用户下次登录时会提示他们进行注册。 | 不是。  它们继续工作，直到注册过程完成。 | 是的。 会话过期后，会要求进行 Azure MFA 注册。| 是的。 访问令牌过期后，会要求进行 Azure MFA 注册。 |
| 强制 | 用户已加入，并已完成 Azure MFA 的注册过程。 | 是的。 应用需要应用密码。 | 是的。 在登录时会要求进行 Azure MFA。 | 是的。 在登录时会要求进行 Azure MFA。 |

用户的状态反映管理员是否已在 Azure MFA 中登记用户以及用户是否已完成注册过程。

所有用户的初始状态均为“已禁用”**。 在 Azure MFA 中注册用户后，用户的状态将更改为“已启用”**。 当已启用的用户登录并完成注册过程后，用户的状态将更改为“强制”**。

> [!NOTE]
> 如果在已具有注册详细信息（如电话或电子邮件）的用户对象上重新启用 MFA，则管理员需要让该用户通过 Azure 门户或 PowerShell 重新注册 MFA。 如果用户不重新注册，则其 MFA 状态不会在 MFA 管理 UI 中从 *"已启用"* 转换到*强制*。

### <a name="view-the-status-for-a-user"></a>查看用户状态

使用以下步骤来访问可在其中查看和管理用户状态的页面：

1. 以管理员身份登录到 [Azure 门户](https://portal.azure.com)。
2. 搜索并选择“Azure Active Directory”**。 选择**所有用户** > **所有用户**。
3. 选择**多重身份验证**。 您可能需要向右滚动才能看到此菜单选项。 选择下面的示例屏幕截图以查看完整的 Azure 门户窗口和菜单位置：[![](media/howto-mfa-userstates/selectmfa-cropped.png "从 Azure AD 中的"用户"窗口选择多重身份验证")](media/howto-mfa-userstates/selectmfa.png#lightbox)
4. 此时会打开一个新页面，其中显示了用户状态。
   ![多重身份验证用户状态 - 屏幕截图](./media/howto-mfa-userstates/userstate1.png)

### <a name="change-the-status-for-a-user"></a>更改用户状态

1. 使用前文的步骤访问 Azure 多重身份验证“用户”**** 页面。
2. 找到希望对其启用 Azure MFA 的用户。 可能需要在顶部更改视图。
   ![从“用户”选项卡选择要更改状态的用户](./media/howto-mfa-userstates/enable1.png)
3. 勾选用户名称旁边的框。
4. 在右侧，在“快速步骤”下，选择“启用”或“禁用”************。
   ![通过在快速步骤菜单上单击“启用”来启用所选用户](./media/howto-mfa-userstates/user1.png)

   > [!TIP]
   > “已启用”** 的用户在注册 Azure MFA 后会自动切换到“强制”**。 不应手动将用户状态更改为“强制”。**

5. 在打开的弹出窗口中确认你的选择。

启用用户后，通过电子邮件通知他们。 告诉他们将需要在下次登录时进行注册。 此外，如果你的组织使用不支持新式身份验证的非浏览器应用，则他们需要创建应用密码。 还可以包括指向 [Azure MFA 最终用户指南](../user-help/multi-factor-authentication-end-user.md)的链接，以便帮助他们上手。

### <a name="use-powershell"></a>使用 PowerShell

若要使用 [Azure AD PowerShell](/powershell/azure/overview) 更改用户状态，请更改 `$st.State`。 有三种可能的状态：

* 已启用
* 强制
* 已禁用  

不要直接将用户移动到“强制”** 状态。 如果这样做了，则非基于浏览器的应用将停止工作，因为用户尚未完成 Azure MFA 注册并获得[应用密码](howto-mfa-mfasettings.md#app-passwords)。

先使用以下命令安装模块：

   ```PowerShell
   Install-Module MSOnline
   ```

> [!TIP]
> 不要忘记先使用 **Connect-MsolService** 进行连接

   ```PowerShell
   Connect-MsolService
   ```

此示例 PowerShell 脚本为单个用户启用 MFA：

   ```PowerShell
   Import-Module MSOnline
   Connect-MsolService
   $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
   $st.RelyingParty = "*"
   $st.State = "Enabled"
   $sta = @($st)
   Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta
   ```

当需要批量启用用户时，使用 PowerShell 是一个不错的选择。 例如，以下脚本循环访问用户列表并在其帐户上启用 MFA：

   ```PowerShell
   $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
   foreach ($user in $users)
   {
       $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
       $st.RelyingParty = "*"
       $st.State = "Enabled"
       $sta = @($st)
       Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
   }
   ```

若要禁用 MFA，请使用此脚本：

   ```PowerShell
   Get-MsolUser -UserPrincipalName user@domain.com | Set-MsolUser -StrongAuthenticationRequirements @()
   ```

该脚本还可缩写为：

   ```PowerShell
   Set-MsolUser -UserPrincipalName user@domain.com -StrongAuthenticationRequirements @()
   ```

### <a name="convert-users-from-per-user-mfa-to-conditional-access-based-mfa"></a>将用户从每个用户 MFA 转换为基于条件访问的 MFA

以下 PowerShell 可以帮助您转换为基于条件访问的 Azure 多重身份验证。

在 ISE 窗口中运行此 PowerShell 或另存为 。要在本地运行的 PS1 文件。

```PowerShell
# Sets the MFA requirement state
function Set-MfaState {

    [CmdletBinding()]
    param(
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $ObjectId,
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $UserPrincipalName,
        [ValidateSet("Disabled","Enabled","Enforced")]
        $State
    )

    Process {
        Write-Verbose ("Setting MFA state for user '{0}' to '{1}'." -f $ObjectId, $State)
        $Requirements = @()
        if ($State -ne "Disabled") {
            $Requirement =
                [Microsoft.Online.Administration.StrongAuthenticationRequirement]::new()
            $Requirement.RelyingParty = "*"
            $Requirement.State = $State
            $Requirements += $Requirement
        }

        Set-MsolUser -ObjectId $ObjectId -UserPrincipalName $UserPrincipalName `
                     -StrongAuthenticationRequirements $Requirements
    }
}

# Disable MFA for all users
Get-MsolUser -All | Set-MfaState -State Disabled
```

> [!NOTE]
> 我们最近相应地更改了上述行为和 PowerShell 脚本。 以前，脚本保存了 MFA 方法、禁用 MFA 并还原了这些方法。 现在，禁用的默认行为未清除方法，这不再需要。
>
> 如果在已具有注册详细信息（如电话或电子邮件）的用户对象上重新启用 MFA，则管理员需要让该用户通过 Azure 门户或 PowerShell 重新注册 MFA。 如果用户不重新注册，则其 MFA 状态不会在 MFA 管理 UI 中从 *"已启用"* 转换到*强制*。

## <a name="next-steps"></a>后续步骤

* 为什么会提示或不会提示用户执行 MFA？ 请参阅[“Azure 多重身份验证中的报告”一文中的“Azure AD 登录报告”部分](howto-mfa-reporting.md#azure-ad-sign-ins-report)。
* 若要配置其他设置（例如受信任的 IP、自定义语音消息和欺诈警报），请参阅[配置 Azure 多重身份验证设置](howto-mfa-mfasettings.md)一文
* 有关管理 Azure 多重身份验证的用户设置的信息，请参阅[管理云中 Azure 多重身份验证的用户设置](howto-mfa-userdevicesettings.md)一文
