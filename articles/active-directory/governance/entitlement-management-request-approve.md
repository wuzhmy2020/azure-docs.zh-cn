---
title: 批准或拒绝访问请求 - Azure AD 授权管理
description: 了解如何使用"我的访问"门户在 Azure 活动目录授权管理中批准或拒绝对访问包的请求。
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: mamtakumar
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/27/2019
ms.author: ajburnle
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: 20ec63efe16f1120ca6e7d07c8917d8ad2b3a0e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261718"
---
# <a name="approve-or-deny-access-requests-in-azure-ad-entitlement-management"></a>在 Azure AD 授权管理中批准或拒绝访问请求

可以使用 Azure AD 权利管理来配置策略，要求对访问包进行审批，并选择一个或多个审批者。 本文介绍指定的审批者如何批准或拒绝针对访问包的请求。

## <a name="open-request"></a>打开请求

若要批准或拒绝访问请求，第一步是找到并打开待审批的访问请求。 可通过两种方式打开访问请求。

**先决条件角色：** 批准人

1. 查找来自 Microsoft Azure 的电子邮件，要求您批准或拒绝请求。 以下是示例电子邮件：

    ![批准对访问包的请求的电子邮件](./media/entitlement-management-shared/approver-request-email.png)

1. 单击“批准或拒绝请求”链接，打开访问请求。****

1. 登录到“我的访问权限”门户。

如果没有电子邮件，可以通过以下步骤来查找待审批的访问请求。

1. 登录到 中的[https://myaccess.microsoft.com](https://myaccess.microsoft.com)"我的访问"门户。  （对于美国政府，"我的访问"门户链接中的域将为`myaccess.microsoft.us`.）

1. 在左侧菜单中，单击“审批”即可看到待审批的访问请求列表。****

1. 在“等待处理”选项卡上找到请求。****

## <a name="approve-or-deny-request"></a>批准或拒绝请求

打开待审批的访问请求以后，即可查看详细信息，这些信息有助于你进行批准或拒绝决策。

**先决条件角色：** 批准人

1. 单击“查看”链接打开“访问请求”窗格。****

1. 单击“详细信息”****，查看有关访问请求的详细信息。

    详细信息包括用户的姓名、组织、访问开始和结束日期（如果已提供）、业务理由、提交请求时间以及请求过期时间。

1. 单击“批准”或“拒绝”。********

1. 必要时输入原因。

    ![我的访问权限门户 - 访问请求](./media/entitlement-management-request-approve/my-access-approve-request.png)

1. 单击“提交”以提交所做的决定。****

    如果为某个策略配置了多个审批者，则只有一个审批者需要进行审批决策。 在某个审批者提交其访问请求决策以后，就完成了该请求，该请求不再可供其他审批者来审批。 其他审批者可以在“我的访问权限”门户中查看请求决策和决策者。 目前仅支持单阶段审批。

    如果配置的审批者都无法审批或拒绝访问请求，则在配置的请求期限过后，请求会到期。 系统会通知用户，告知其访问请求已过期，需重新提交访问请求。

## <a name="next-steps"></a>后续步骤

- [请求访问访问包](entitlement-management-request-access.md)
- [请求过程和电子邮件通知](entitlement-management-process.md)
