---
title: 标识监管 - Azure Active Directory | Microsoft Docs
description: 使用 Azure Active Directory 标识监管可以在组织的安全性和员工工作效率与适当的流程和可见性需求之间实现平衡。
services: active-directory
documentationcenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/24/2019
ms.author: ajburnle
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd319dd6a83a392f6df26d07a58be22a9c8bdb61
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063673"
---
# <a name="what-is-azure-ad-identity-governance"></a>什么是 Azure AD Identity Governance？

使用 Azure Active Directory (Azure AD) 标识监管可以在组织的安全性和员工工作效率与适当的流程和可见性需求之间实现平衡。 它为您提供了确保合适的人员能够正确访问正确的资源的能力。 这些相关的 Azure AD 和企业移动性 + 安全功能允许您通过保护、监视和审核对关键资产的访问来降低访问风险，同时确保员工和业务合作伙伴的工作效率。  

身份治理使组织能够跨员工、业务合作伙伴和供应商以及跨本地和云中的服务和应用程序执行以下任务：

- 监管标识生命周期
- 监管访问生命周期
- 保护用于管理的特权访问

具体而言，标识监管旨在帮助组织解决以下四个关键问题：

- 哪些用户应该有权访问哪些资源？
- 这些用户使用该访问权限做些什么？
- 是否存在有效的组织控制措施可用于管理访问权限？
- 审核员是否可以验证控制措施是否正常实施？

## <a name="identity-lifecycle"></a>标识生命周期

身份治理可帮助组织在*工作效率*之间取得平衡 - 一个人访问所需资源的速度有多快，例如当他们加入我的组织时？ 安全性 - 用户的访问权限会不断发生怎样的变化（例如，由于该用户的雇佣状态发生变化）？**  标识生命周期管理是标识监管的基石，大规模的有效监管需要将应用程序的标识生命周期管理基础结构现代化。

![标识生命周期](./media/identity-governance-overview/identity-lifecycle.png)

对于许多组织而言，员工的标识生命周期与该用户在 HCM（人力资本管理）系统中的表示形式密切相关。  Azure AD 高级版会自动维护活动目录和 Azure 活动目录中工作日中代表的人员的用户身份，如[工作日入站预配教程](../saas-apps/workday-inbound-tutorial.md)中所述。  Azure AD Premium 还包含可从 SAP、Oracle eBusiness 和 Oracle PeopleSoft 等本地 HCM 系统导入记录的 [Microsoft Identity Manager](/microsoft-identity-manager/)。

越来越多的方案需要与组织外部的人员协作。 使用 [Azure AD B2B](/azure/active-directory/b2b/) 协作可以安全地将组织的应用程序和服务与来自任何组织的来宾用户和外部合作伙伴共享，同时保持对自己公司数据的控制。  [Azure AD 授权管理](entitlement-management-overview.md)使您能够选择允许哪个组织的用户请求访问，并将其添加为 B2B 来宾到组织的目录，并确保在不再需要访问权限时删除这些来宾。

## <a name="access-lifecycle"></a>访问生命周期

组织需要使用一个流程来管理最初在创建用户标识时未为用户预配的访问权限。  此外，企业组织需要能够有效缩放，以便持续制定和实施访问策略与控制措施。

![访问生命周期](./media/identity-governance-overview/access-lifecycle.png)

通常，IT 部门会将访问权限审批决策委托给业务决策人。  此外，IT 部门可能涉及到用户本身的事务。  例如，访问公司位于欧洲的市场营销应用程序中的机密客户数据的用户需要知道该公司的策略。 来宾用户可能不知道他们受邀访问的组织的数据处理要求。

组织可以通过[动态组](../users-groups-roles/groups-dynamic-membership.md)等技术，并结合 [SaaS 应用](../saas-apps/tutorial-list.md)的用户预配或者[与 SCIM集成的应用](../app-provisioning/use-scim-to-provision-users-and-groups.md)，将访问生命周期过程自动化。  组织还可以控制哪些[来宾用户有权访问本地应用程序](../b2b/hybrid-cloud-to-on-premises.md)。  然后，可以使用 [Azure AD 访问评审](access-reviews-overview.md)定期评审这些访问权限。   [Azure AD 授权管理](entitlement-management-overview.md)还使您能够定义用户如何跨组和团队成员身份、应用程序角色和 SharePoint Online 角色的包请求访问。

当用户尝试访问应用程序时，Azure AD 将强制实施[条件访问](/azure/active-directory/conditional-access/)策略。 例如，条件访问策略可以包括显示[使用条款](../conditional-access/terms-of-use.md)，并确保用户在能够访问应用程序之前[同意这些条款](../conditional-access/require-tou.md)。

## <a name="privileged-access-lifecycle"></a>特权访问权限生命周期

一直以来，其他供应商将特权访问权限描述为标识监管中的一项独立功能。 但是，Microsoft 将特权访问权限的监管视为标识监管的重要组成部分 - 特别是考虑到与这些管理员权限相关的滥用可能会对组织造成的影响。 拥有管理权限的员工、供应商与合同工需要接受监管。

![特权访问权限生命周期](./media/identity-governance-overview/privileged-access-lifecycle.png)

[Azure AD 特权标识管理 （PIM）](../privileged-identity-management/pim-configure.md)提供了其他控件，用于保护 Azure AD、Azure 和其他 Microsoft 在线服务中资源的访问权限。  Azure AD PIM 提供的实时访问和角色更改警报功能，以及多重身份验证和条件访问，共同提供了一套综合性的监管控制措施，可帮助保护公司的资源（目录、Office 365 和 Azure 资源角色）。 与处理其他形式的访问权限一样，组织可以使用访问评审来针对充当管理员角色的所有用户配置定期的访问权限重新认证。

## <a name="getting-started"></a>入门

查看 Azure 门户中的 **"开始使用标识治理**"选项卡，开始使用授权管理、访问审核、特权标识管理和使用条款。

![标识监管入门](./media/identity-governance-overview/getting-started.png)


如果你对标识治理功能有任何反馈，请在 Azure 门户中单击“获得反馈?”**** 以提交反馈。 团队定期审查反馈。

虽然没有针对每个客户的完美解决方案或建议，但以下配置指南还提供 Microsoft 建议您遵循的基准策略，以确保员工队伍更安全、更高效。

- [标识和设备访问权限配置](/microsoft-365/enterprise/microsoft-365-policies-configurations)
- [保护特权访问](../users-groups-roles/directory-admin-roles-secure.md)

## <a name="appendix---least-privileged-roles-for-managing-in-identity-governance-features"></a>附录 - 身份治理功能中管理的特权最低角色

在标识治理中使用最小特权角色来执行管理任务是一种最佳做法。 我们建议你使用 Azure AD PIM 根据需要激活角色以执行这些任务。 以下是配置标识治理功能所需的最小特权目录角色：

| Feature | 最小特权角色 |
| ------- | --------------------- |
| 权利管理 | 用户管理员（将 SharePoint Online 站点添加到目录中除外，这需要全局管理员） |
| 访问评审 | 用户管理员（Azure 或 Azure AD 角色的访问评审除外，这需要特权角色管理员） |
|Privileged Identity Management | 特权角色管理员 |
| 使用条款 | 安全管理员或条件访问管理员 |

## <a name="next-steps"></a>后续步骤

- [Azure AD 权利管理是什么？](entitlement-management-overview.md)
- [什么是 Azure AD 访问审核？](access-reviews-overview.md)
- [什么是 Azure AD Privileged Identity Management？](../privileged-identity-management/pim-configure.md)
- [“使用条款”可以用来做什么？](active-directory-tou.md)


