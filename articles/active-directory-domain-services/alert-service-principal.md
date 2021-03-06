---
title: 解决 Azure AD 域服务中的服务主体警报 |微软文档
description: 了解如何对 Azure 活动目录域服务的服务主体配置警报进行故障排除
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: f168870c-b43a-4dd6-a13f-5cfadc5edf2c
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 09/20/2019
ms.author: iainfou
ms.openlocfilehash: 175bfe63176b78c5aeafc7147c46dd5ab1110325
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "71257968"
---
# <a name="known-issues-service-principal-alerts-in-azure-active-directory-domain-services"></a>已知问题：Azure 活动目录域服务中的服务主体警报

[服务主体](../active-directory/develop/app-objects-and-service-principals.md)是 Azure 平台用于管理、更新和维护 Azure AD DS 托管域的应用程序。 如果删除了服务主体，则 Azure AD DS 托管域中的功能将受到影响。

本文可帮助您排除故障并解决与服务主体相关的配置警报。

## <a name="alert-aadds102-service-principal-not-found"></a>警报 AADDS102：未找到服务主体

### <a name="alert-message"></a>警报消息

*Azure AD 域服务正常运行所需的服务主体已从 Azure AD 目录中删除。此配置会影响 Microsoft 监视、管理、修补和同步托管域的能力。*

如果删除了所需的服务主体，Azure 平台无法执行自动化管理任务。 Azure AD DS 托管域可能无法正确应用更新或进行备份。

### <a name="check-for-missing-service-principals"></a>检查是否缺少服务主体

要检查缺少哪些服务主体并需要重新创建，请完成以下步骤：

1. 在 Azure 门户中，从左侧导航菜单中选择**Azure 活动目录**。
1. 选择“企业应用程序”。**** 从 **"应用程序类型"** 下拉菜单中选择*所有应用程序*，然后选择 **"应用**"。
1. 搜索每个应用程序指示。 如果未找到现有应用程序，请按照*解决方法*步骤创建服务主体或重新注册命名空间。

    | 应用程序 ID | 解决方法 |
    | :--- | :--- |
    | 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [重新创建缺少的服务主体](#recreate-a-missing-service-principal) |
    | 443155a6-77f3-45e3-882b-22b3a8d431fb | [重新注册 Microsoft.AAD 命名空间](#re-register-the-microsoft-aad-namespace) |
    | abba844e-bc0e-44b0-947a-dc74e5d09022 | [重新注册 Microsoft.AAD 命名空间](#re-register-the-microsoft-aad-namespace) |
    | d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [重新注册 Microsoft.AAD 命名空间](#re-register-the-microsoft-aad-namespace) |

### <a name="recreate-a-missing-service-principal"></a>重新创建缺少的服务主体

如果 Azure AD 目录中缺少应用程序 ID *2565bd9d-da50-47d4-8b85-4c97f669dc36，* 请使用 Azure AD PowerShell 完成以下步骤。 有关详细信息，请参阅安装[Azure AD 电源外壳](/powershell/azure/active-directory/install-adv2)。

1. 安装 Azure AD PowerShell 模块并按照如下方式导入它：

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```

1. 现在使用[New-AzureAdService 主体][New-AzureAdServicePrincipal]cmdlet 重新创建服务主体：

    ```powershell
    New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
    ```

Azure AD DS 托管域的运行状况会在两小时内自动更新自身并删除警报。

### <a name="re-register-the-microsoft-aad-namespace"></a>重新注册微软 AAD 命名空间

如果申请 ID *443155a6-77f3-45e3-882b-22b3a8d431fb* *，abba844e-bc0e-44b0-94 Azure*AD 目录中缺少*d87dcbc6-a371-462e-88e3-28ad15ec4e64，* 完成以下步骤以重新注册*Microsoft.AAD*资源提供程序：

1. 在 Azure 门户中，搜索并选择 **"订阅**"。
1. 选择与 Azure AD DS 托管域关联的订阅。
1. 从左侧导航中，选择**资源提供程序**。
1. 搜索*微软.AAD*，然后选择**重新注册**。

Azure AD DS 托管域的运行状况会在两小时内自动更新自身并删除警报。

## <a name="alert-aadds105-password-synchronization-application-is-out-of-date"></a>警报 AADDS105：密码同步应用程序已过期

### <a name="alert-message"></a>警报消息

*应用程序 ID"d87dcbc6-a371-462e-88e3-28ad15ec4e64"的服务主体被删除，然后重新创建。娱乐会在 Azure AD 域服务资源上留下不一致的权限，这些资源需要为托管域提供服务。托管域上的密码同步可能会受到影响。*

Azure AD DS 会自动同步来自 Azure AD 的用户帐户和凭据。 如果用于此过程的 Azure AD 应用程序出现问题，则 Azure AD DS 和 Azure AD 之间的凭据同步将失败。

### <a name="resolution"></a>解决方法

要重新创建用于凭据同步的 Azure AD 应用程序，请使用 Azure AD PowerShell 完成以下步骤。 有关详细信息，请参阅安装[Azure AD 电源外壳](/powershell/azure/active-directory/install-adv2)。

1. 安装 Azure AD PowerShell 模块并按照如下方式导入它：

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```

2. 现在使用以下 PowerShell cmdlet 删除旧应用程序和对象：

    ```powershell
    $app = Get-AzureADApplication -Filter "IdentifierUris eq 'https://sync.aaddc.activedirectory.windowsazure.com'"
    Remove-AzureADApplication -ObjectId $app.ObjectId
    $spObject = Get-AzureADServicePrincipal -Filter "DisplayName eq 'Azure AD Domain Services Sync'"
    Remove-AzureADServicePrincipal -ObjectId $app.ObjectId
    ```

删除这两个应用程序后，Azure 平台会自动重新创建它们并尝试恢复密码同步。 Azure AD DS 托管域的运行状况会在两小时内自动更新自身并删除警报。

## <a name="next-steps"></a>后续步骤

如果仍有问题，[请打开 Azure 支持请求][azure-support]以获取其他故障排除帮助。

<!-- INTERNAL LINKS -->
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md

<!-- EXTERNAL LINKS -->
[New-AzureAdServicePrincipal]: /powershell/module/AzureAD/New-AzureADServicePrincipal
