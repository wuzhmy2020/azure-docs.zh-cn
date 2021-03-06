---
title: 更改 Azure AD 授权管理中访问包的资源角色 - Azure 活动目录
description: 了解如何在 Azure 活动目录授权管理中更改现有访问包的资源角色。
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/15/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 102bbfbd1c02c93830f5c7fce89fe95d7fde54c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261887"
---
# <a name="change-resource-roles-for-an-access-package-in-azure-ad-entitlement-management"></a>更改 Azure AD 授权管理中访问包的资源角色

访问包管理者随时可以更改访问包中的资源，而无需考虑如何预配用户对新资源的访问权限，或者从以前的资源中删除用户的访问权限。 本文介绍如何更改现有访问包的资源角色。

本视频概述了如何更改访问包。

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE3LD4Z]

## <a name="check-catalog-for-resources"></a>检查目录资源

如果需要将资源添加到访问包，则应检查目录中所需的资源是否可用。 如果您是访问包管理器，则无法将资源添加到目录，即使您拥有这些资源也是如此。 您仅限于使用目录中可用的资源。

**先决条件角色：** 全局管理员、用户管理员、目录所有者或访问包管理器

1. 在 Azure 门户中，依次单击“Azure Active Directory”、“标识监管”。********

1. 在左侧菜单中，单击 **"目录"，** 然后打开目录。

1. 在左侧菜单中，单击 **"资源"** 以查看此目录中的资源列表。

    ![目录中的资源列表](./media/entitlement-management-access-package-resources/catalog-resources.png)

1. 如果您是访问包管理器，并且需要向目录添加资源，则可以要求目录所有者添加这些资源。

## <a name="add-resource-roles"></a>添加资源角色

资源角色是与资源关联的权限集合。 为用户提供可请求的资源的方式是将资源角色添加到访问包。 您可以为组、团队、应用程序和 SharePoint 网站添加资源角色。

**先决条件角色：** 全局管理员、用户管理员、目录所有者或访问包管理器

1. 在 Azure 门户中，依次单击“Azure Active Directory”、“标识监管”。********

1. 在左侧菜单中单击“访问包”，然后打开访问包。****

1. 在左侧菜单中单击“资源角色”。****

1. 单击“添加资源角色”打开“将资源角色添加到访问包”页。****

    ![访问包 - 添加资源角色](./media/entitlement-management-access-package-resources/resource-roles-add.png)

1. 根据是否要添加组、团队、应用程序或 SharePoint 站点，请执行以下资源角色部分之一中的步骤。

## <a name="add-a-group-or-team-resource-role"></a>添加组或团队资源角色

当用户被分配访问包时，您可以让授权管理自动将用户添加到 Microsoft Teams 中的组或团队中。 

- 当组或团队是访问包的一部分，并且用户被分配到该访问包时，用户将添加到该组或团队中（如果尚未存在）。
- 当用户的访问包分配到期时，他们将从组或团队中删除，除非他们当前已分配到包含同一组或团队的另一个访问包。

您可以选择任何[Azure AD 安全组或 Office 365 组](../fundamentals/active-directory-groups-create-azure-portal.md)。 管理员可将任何组添加到目录；如果目录所有者是组的所有者，则他们可将任何组添加到该目录中。 选择组时，请记住以下 Azure AD 约束：

- 当用户（包括来宾）作为成员添加到组或团队时，他们可以看到该组或团队的所有其他成员。
- Azure AD 无法更改使用 Azure AD Connect 从 Windows Server Active Directory 同步的组的成员身份，或在 Exchange Online 中作为通讯组创建的组的成员身份。  
- 无法通过添加或删除成员来更新动态组的成员身份，因此，不适合在权利管理中使用动态组成员身份。

有关详细信息，请参阅[比较组](https://docs.microsoft.com/office365/admin/create-groups/compare-groups)和[Office 365 组和 Microsoft 团队](https://docs.microsoft.com/microsoftteams/office-365-groups)。

1. 在"**添加资源角色以访问包**"页上，单击 **"组和团队**"以打开"选择组"窗格。

1. 选择要包含在访问包中的组和团队。

    ![访问包 - 添加资源角色 - 选择组](./media/entitlement-management-access-package-resources/group-select.png)

1. 单击“选择”。

    选择组或团队后，"**子类型"** 列将列出以下子类型之一：

    |  |  |
    | --- | --- |
    | 安全性 | 用于授予对资源的访问。 |
    | 分发 | 用于向一组人员发送通知。 |
    | O365 | 未启用团队的 Office 365 组。 用于公司内外用户之间的协作。 |
    | 团队 | 启用团队的 Office 365 组。 用于公司内外用户之间的协作。 |

1. 在“角色”列表中，选择“所有者”或“成员”。************

    我们通常会选择“成员”角色。 如果选择“所有者”角色，则允许用户添加或删除其他成员或所有者。

    ![访问包 - 为组或团队添加资源角色](./media/entitlement-management-access-package-resources/group-role.png)

1. 单击 **“添加”**。

    对访问包具有现有分配的任何用户在添加访问包时将自动成为此组或团队的成员。

## <a name="add-an-application-resource-role"></a>添加应用程序资源角色

为用户分配访问包后，可让 Azure AD 自动为用户分配对 Azure AD 企业应用程序（包括 SaaS 应用程序，以及联合到 Azure AD 的组织应用程序）的访问权限。 对于通过联合单一登录与 Azure AD 集成的应用程序，Azure AD 将为分配给应用程序的用户颁发联合令牌。

应用程序可以有多个角色。 将某个应用程序添加到访问包时，如果该应用程序有多个角色，则你需要为这些用户指定相应的角色。 如果您正在开发应用程序，则可以在["如何：配置企业应用程序的 SAML 令牌中颁发的角色声明](../develop/active-directory-enterprise-app-role-management.md)"中详细了解如何将这些角色添加到应用程序中。

一旦应用程序角色成为访问包的一部分：

- 为某个用户分配该访问包时，该用户将添加到该应用程序角色（如果该用户没有该角色）。
- 当用户的访问包分配过期时，他们的访问权限将从应用程序中删除，除非他们已分配到包含该应用程序角色的其他访问包。

选择应用程序时，请注意以下事项：

- 应用程序还可能已将组分配到其角色。  可以选择添加组来取代访问包中的应用程序角色，但是，用户在“我的访问权限”门户中看不到作为访问包的一部分的应用程序。

1. 在“将资源角色添加到访问包”页上，单击“应用程序”打开“选择应用程序”窗格。********

1. 选择要包含在访问包中的应用程序。

    ![访问包 - 添加资源角色 - 选择应用程序](./media/entitlement-management-access-package-resources/application-select.png)

1. 单击“选择”。

1. 在“角色”列表中，选择应用程序角色。****

    ![访问包 - 为应用程序添加资源角色](./media/entitlement-management-access-package-resources/application-role.png)

1. 单击 **“添加”**。

    添加此应用程序时，在访问包中具有现有分配的任何用户将自动获得此应用程序的访问权限。

## <a name="add-a-sharepoint-site-resource-role"></a>添加 SharePoint 站点资源角色

为用户分配访问包时，Azure AD 可自动为用户分配对 SharePoint Online 站点或 SharePoint Online 站点集合的访问权限。

1. 在“将资源角色添加到访问包”页上，单击“SharePoint 站点”打开“选择 SharePoint Online 站点”窗格。********

1. 选择要包含在访问包中的 SharePoint Online 站点。

    ![访问包 - 添加资源角色 - 选择 SharePoint Online 站点](./media/entitlement-management-access-package-resources/sharepoint-site-select.png)

1. 单击“选择”。

1. 在“角色”列表中，选择 SharePoint Online 站点角色。****

    ![访问包 - 为 SharePoint Online 站点添加资源角色](./media/entitlement-management-access-package-resources/sharepoint-site-role.png)

1. 单击 **“添加”**。

    添加此 SharePoint Online 站点时，在访问包中具有现有分配的任何用户将自动获得此 SharePoint Online 站点的访问权限。

## <a name="remove-resource-roles"></a>删除资源角色

**先决条件角色：** 全局管理员、用户管理员、目录所有者或访问包管理器

1. 在 Azure 门户中，依次单击“Azure Active Directory”、“标识监管”。********

1. 在左侧菜单中单击“访问包”，然后打开访问包。****

1. 在左侧菜单中单击“资源角色”。****

1. 在资源角色列表中，找到要删除的资源角色。

1. 依次单击省略号 (**...**)、“删除资源角色”。****

    删除此资源角色后，在访问包中具有现有分配的任何用户对此资源角色的访问权限将自动吊销。

## <a name="when-changes-are-applied"></a>应用更改时

在权利管理中，Azure AD 每天会多次处理访问包中的分配和资源的批量更改。 因此，如果你做了分配或更改了访问包的资源角色，可能需要在长达 24 小时之后，才会在 Azure AD 中做出该项更改，此外，还需要花费一段时间将这些更改传播到其他 Microsoft Online Services 或连接的 SaaS 应用程序。 如果所做的更改只影响几个对象，则更改可能在几分钟后即可在 Azure AD 中应用，之后，其他 Azure AD 组件会检测到该更改并更新 SaaS 应用程序。 如果所做的更改会影响数千个对象，则应用更改需要花费较长的时间。 例如，如果某个访问包具有 2 个应用程序分配和 100 个用户分配，而你决定将一个 SharePoint 站点角色添加到该访问包，则在将所有用户加入 SharePoint 站点角色之前可能会出现延迟。 可以通过 Azure AD 审核日志、Azure AD 预配日志和 SharePoint 站点审核日志来监视进度。

删除团队成员时，也会将其从 Office 365 组中删除。 从团队的聊天功能中删除可能会延迟。 有关详细信息，请参阅[组成员身份](https://docs.microsoft.com/microsoftteams/office-365-groups#group-membership)。

## <a name="next-steps"></a>后续步骤

- [使用 Azure Active Directory 创建基本组并添成员](../fundamentals/active-directory-groups-create-azure-portal.md)
- [如何：为企业应用程序配置 SAML 令牌中颁发的角色声明](../develop/active-directory-enterprise-app-role-management.md)
- [在线分享点简介](/sharepoint/introduction)