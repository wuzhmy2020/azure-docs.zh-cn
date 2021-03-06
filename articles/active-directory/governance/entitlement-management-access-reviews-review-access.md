---
title: 查看 Azure AD 授权管理中访问包的访问
description: 了解如何在 Azure 活动目录访问审核（预览）中完成对授权管理访问包的访问审查。
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
ms.date: 11/01/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 99de022b7259b33baab3aa825673a8f85e932bff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "78968750"
---
# <a name="review-access-of-an-access-package-in-azure-ad-entitlement-management"></a>查看 Azure AD 授权管理中访问包的访问

Azure AD 授权管理简化了企业管理对组、应用程序和 SharePoint 站点的访问的方式。 本文介绍如何为作为指定审阅者分配到访问包的其他用户执行访问审核。

## <a name="prerequisites"></a>先决条件

要查看用户的活动访问包分配，必须满足执行访问审核的先决条件：
- Azure AD Premium P2
- 全局管理员
- 指定用户管理员、目录所有者或访问包管理器

有关详细信息，请参阅[许可证要求](entitlement-management-overview.md#license-requirements)。


## <a name="open-the-access-review"></a>打开访问评审

使用以下步骤查找和打开访问审核：

1. 您可能会收到来自 Microsoft 的电子邮件，要求您查看访问权限。 找到电子邮件以打开访问审核。 下面是一个用于查看访问权限的示例电子邮件：
    
    ![访问审阅审阅者电子邮件](./media/entitlement-management-access-reviews-review-access/review-access-reviewer-email.png)

1. 单击 **"查看用户访问**"链接以打开访问审核。 

1. 如果您没有该电子邮件，则可以通过直接导航到https://myaccess.microsoft.com找到挂起的访问评论。  （对于美国政府，`https://myaccess.microsoft.us`请使用。

1. 单击左侧导航栏上的 **"访问审阅**"以查看分配给您挂起的访问审核的列表。
    
    ![在"我的访问"上选择访问评论](./media/entitlement-management-access-reviews-review-access/review-access-myaccess-select-access-review.png)

1. 单击要开始的评论。
    
    ![选择访问审核](./media/entitlement-management-access-reviews-review-access/review-access-select-access-review.png)

## <a name="perform-the-access-review"></a>执行访问评审

打开访问审核后，您将看到需要查看的用户名称。 可通过两种方式批准或拒绝访问权限：
- 您可以手动批准或拒绝一个或多个用户的访问权限
- 您可以接受系统建议

### <a name="manually-approve-or-deny-access-for-one-or-more-users"></a>手动批准或拒绝一个或多个用户的访问权限
1. 查看用户列表并确定需要哪些用户继续具有访问权限。

    ![要查看的用户列表](./media/entitlement-management-access-reviews-review-access/review-access-list-of-users.png)

1. 要批准或拒绝访问权限，请选择用户名左侧的单选按钮。

1. 在用户名上方的栏中选择 **"批准**"或 **"拒绝**"。

    ![选择用户](./media/entitlement-management-access-reviews-review-access/review-access-select-users.png)

1. 如果您不确定，可以单击"**不知道"** 按钮。

    如果进行此选择，则用户维护访问权限，并且此选择将位于审核日志中。 该日志显示您仍已完成审阅的任何其他审阅者。

1. 您可能需要提供决策的理由。 键入原因并单击"**提交**"。

    ![批准或拒绝访问权限](./media/entitlement-management-access-reviews-review-access/review-access-decision-approve.png)

1. 您可以在审核结束前的任何时间更改您的决定。 为此，请从列表中选择用户并更改决策。 例如，您可以批准以前被拒绝的用户的访问权限。

如果有多个评审者，将记录最后提交的响应。 考虑一个管理员指定两个审阅者的示例 - Alice 和 Bob。 Alice 首先打开审核并批准访问。 在审核结束之前，Bob 将打开审核并拒绝访问。 在这种情况下，将记录最后一个拒绝访问决定。

>[!NOTE]
>如果用户被拒绝访问，则不会立即从访问包中删除他们。 审核结束时，用户将从访问包中删除，或者管理员结束审核。

### <a name="approve-or-deny-access-using-the-system-generated-recommendations"></a>使用系统生成的建议批准或拒绝访问

要更快地查看多个用户的访问权限，可以使用系统生成的建议，只需单击一下即可接受建议。 建议是根据用户的登录活动生成的。

1.  在页面顶部的栏中，单击"**接受建议**"。
    
    ![选择"接受建议"](./media/entitlement-management-access-reviews-review-access/review-access-use-recommendations.png)
    
    您将看到建议操作的摘要。

1.  单击"**提交**"以接受建议。

## <a name="next-steps"></a>后续步骤

- [访问包的自我审查](entitlement-management-access-reviews-self-review.md)