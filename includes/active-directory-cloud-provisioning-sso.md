---
author: billmath
ms.service: active-directory
ms.subservice: cloud-provisioning
ms.topic: include
ms.date: 10/16/2019
ms.author: billmath
ms.openlocfilehash: 3aa1571b46938b03f556fa124d3f0a2a70f2c5c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "79504321"
---
## <a name="steps-to-enable-single-sign-on"></a>启用单一登录的步骤
云预配适用于单一登录。  当前，在安装代理时没有启用 SSO 的选项，但您可以使用以下步骤启用 SSO 并使用它。 

### <a name="step-1-download-and-extract-azure-ad-connect-files"></a>第 1 步：下载并提取 Azure AD 连接文件
1.  首先，下载最新版本的[Azure AD 连接](https://www.microsoft.com/download/details.aspx?id=47594)
2.  使用管理权限打开命令提示符，然后导航到刚刚下载的 msi。
3.  运行以下内容：`msiexec /a C:\filepath\AzureADConnect.msi /qb TARGETDIR=C:\filepath\extractfolder`
4. 更改文件路径和提取文件夹以匹配文件路径和提取文件夹的名称。  内容现在应位于提取文件夹中。

### <a name="step-2-import-the-seamless-sso-powershell-module"></a>第 2 步：导入无缝 SSO 电源外壳模块

1. 下载并安装[Azure AD 电源外壳](https://docs.microsoft.com/powershell/azure/active-directory/overview)。
2. 浏览到 `%programfiles%\Microsoft Azure Active Directory Connect` 文件夹。
3. 使用以下命令导入无缝 SSO PowerShell 模块：`Import-Module .\AzureADSSO.psd1`。

### <a name="step-3-get-the-list-of-active-directory-forests-on-which-seamless-sso-has-been-enabled"></a>第 3 步：获取已启用无缝 SSO 的活动目录林列表

1. 以管理员身份运行 PowerShell。 在 PowerShell 中，调用 `New-AzureADSSOAuthenticationContext`。 出现提示时，输入租户的全局管理员凭据。
2. 调用 `Get-AzureADSSOStatus`。 此命令可提供已在其中启用了此功能的 Active Directory 林列表（请查看“域”列表）。

### <a name="step-4-enable-seamless-sso-for-each-active-directory-forest"></a>步骤 4：为每个 Active Directory 林启用无缝 SSO

1. 调用 `Enable-AzureADSSOForest`。 出现提示时，输入目标 Active Directory 林的域管理员凭据。

   > [!NOTE]
   >域管理员凭据用户名必须以 SAM 帐户名称格式（contoso_johndoe 或 contoso.com_johndoe）输入。 我们使用用户名的域部分来查找使用 DNS 的域管理员的域控制器。

   >[!NOTE]
   >使用的域管理员帐户不能是受保护用户组的成员。 如果是这样，操作将失败。

2. 为你要在其中设置该功能的每个 Active Directory 林重复上述步骤。

### <a name="step-5-enable-the-feature-on-your-tenant"></a>步骤 5。 在租户上启用此功能

若要在租户上启用此功能，请调用 `Enable-AzureADSSO -Enable $true`。
