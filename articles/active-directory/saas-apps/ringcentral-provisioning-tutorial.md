---
title: 教程：使用 Azure 活动目录配置环中心以自动预配用户 |微软文档
description: 了解如何自动预配和取消预配用户帐户从 Azure AD 到 RingCentral。
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 976ac071-fa7e-4f31-aed1-d174942860d4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2019
ms.author: Zhchia
ms.openlocfilehash: 052223f69fc1c1d59ec5f1bcbeb3746ef7122c86
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79087065"
---
# <a name="tutorial-configure-ringcentral-for-automatic-user-provisioning"></a>教程：为自动用户预配配置振铃中心

本教程介绍需要在环中央目录和 Azure 活动目录 （Azure AD） 中执行以配置自动用户预配的步骤。 配置后，Azure AD 使用 Azure AD 预配服务自动预配和取消向[振中央](https://www.ringcentral.com/office/plansandpricing.html)预配用户和组。 有关此服务的功能、工作原理以及常见问题的重要详细信息，请参阅[使用 Azure Active Directory 自动将用户预配到 SaaS 应用程序和取消预配](../manage-apps/user-provisioning.md)。 


## <a name="capabilities-supported"></a>支持功能
> [!div class="checklist"]
> * 在环中心创建用户
> * 当环中央不再需要访问权限时，将用户删除
> * 使用户属性在 Azure AD 和振铃中心之间保持同步
> * [单个登录](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-tutorial)到环中心（推荐）

## <a name="prerequisites"></a>先决条件

本教程中概述的方案假定你已具有以下先决条件：

* [Azure AD 租户](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 中具有配置预配[权限](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)的用户帐户（例如应用程序管理员、云应用程序管理员、应用程序所有者或全局管理员）。 
* [环中心租户](https://www.ringcentral.com/office/plansandpricing.html)
* 具有管理员权限的 RingCentral 中的用户帐户。

## <a name="step-1-plan-your-provisioning-deployment"></a>步骤 1。 规划预配部署
1. 了解[预配服务的工作原理](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)。
2. 确定谁将在[预配范围内](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)。
3. 确定要在[Azure AD 和 环形中心 之间映射](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)哪些数据。 

## <a name="step-2-configure-ringcentral-to-support-provisioning-with-azure-ad"></a>步骤 2. 将振铃中心配置为支持使用 Azure AD 进行预配

1. 登录到您的[振铃管理中心管理控制台](https://login.ringcentral.com/sw.html)。 导航到**工具>目录集成**。

    ![环中央管理控制台](media/ringcentral-provisioning-tutorial/admin.png)

2.  **在选择目录提供程序**下选择**SCIM。** （将来将有一个称为 Azure 活动目录的选项）。 单击**启用 SCIM 服务**。

    ![环中心添加 SCIM](media/ringcentral-provisioning-tutorial/scim.png)

3.  请与matthew.hunt@ringcentral.comRingCentral 支持团队联系，了解**SCIM 身份验证令牌**。 此值将在 Azure 门户中的 RingCentral 应用程序的"预配"选项卡中的"机密令牌"字段中输入。

> [!NOTE]
> 要向用户分配许可证，请参阅[此处](https://support.ringcentral.com/s/article/5-10-Adding-Extensions-via-Web?language)的视频链接。

## <a name="step-3-add-ringcentral-from-the-azure-ad-application-gallery"></a>步骤 3. 从 Azure AD 应用程序库添加振铃中心

从 Azure AD 应用程序库添加振铃中心，以开始管理到环中央的预配。 如果您以前为 SSO 设置了振铃中心，则可以使用相同的应用程序。 但是，建议在最初测试集成时创建单独的应用。 在此处了解有关从库中添加应用程序[的更多详细信息](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)。 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>步骤 4. 定义谁将处于预配范围 

Azure AD 预配服务允许您根据对应用程序的分配以及或基于用户/组的属性来限定谁将预配的范围。 如果选择根据分配将预配到应用的范围，[则可以使用以下步骤将](../manage-apps/assign-user-or-group-access-portal.md)用户和组分配给应用程序。 如果选择仅根据用户或组的属性预配谁的范围，则可以使用[此处](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)所述的范围筛选器。 

* 将用户和组分配给 RingCentral 时，必须选择**默认访问**以外的角色。 具有默认访问权限角色的用户从预配中排除，并且将在预配日志中标记为无效权限。 如果应用程序上唯一可用的角色是默认访问角色，则可以[更新应用程序清单](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps)以添加其他角色。 

* 从小开始。 在向所有人推出之前，先与一小部分用户和组进行测试。 将预配范围设置为分配的用户和组时，可以通过为应用分配一个或两个用户或组来控制这种情况。 当作用域设置为所有用户和组时，可以指定[基于属性的范围筛选器](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)。 


## <a name="step-5-configure-automatic-user-provisioning-to-ringcentral"></a>步骤 5。 将自动用户预配配置为环中心 

本节将指导您完成将 Azure AD 预配服务配置为根据 Azure AD 中的用户和/或组分配在 TestApp 中创建、更新和禁用用户和/或组的步骤。

### <a name="to-configure-automatic-user-provisioning-for-ringcentral-in-azure-ad"></a>要在 Azure AD 中配置环中央的自动用户预配：

1. 登录到 Azure[门户](https://portal.azure.com)。 选择**企业应用程序**，然后选择**所有应用程序**。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

2. 在应用程序列表中，选择“RingCentral”****。

    ![应用程序列表中的“RingCentral”链接](common/all-applications.png)

3. 选择“预配”**** 选项卡。

    ![预配选项卡](common/provisioning.png)

4. 将**预配模式**设置为 **"自动**"。

    ![预配选项卡](common/provisioning-automatic.png)

5. 在 **"管理凭据"** 部分下`https://platform.ringcentral.com/scim/v2`，在**租户 URL**中输入 。 输入在**秘密令牌**中检索到的**SCIM 身份验证令牌**值。 单击 **"测试连接**"以确保 Azure AD 可以连接到环中心。 如果连接失败，请确保您的 RingCentral 帐户具有管理员权限，然后重试。

    ![预配选项卡](./media/ringcentral-provisioning-tutorial/provisioning.png)

6. 在 **"通知电子邮件"** 字段中，输入应接收预配错误通知的个人或组的电子邮件地址，然后选中"**在发生故障时发送电子邮件通知**"复选框。

    ![通知电子邮件](common/provisioning-notification-email.png)

7. 选择“保存”。****

8. 在 **"映射**"部分下，选择**将 Azure 活动目录用户同步到环中心**。

9. 在**属性映射**部分中查看从 Azure AD 同步到环中央的用户属性。 选择为 **"匹配属性"** 的属性用于与 RingCentral 中的用户帐户匹配以进行更新操作。 如果选择更改[匹配的目标属性](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)，则需要确保 RingCentral API 支持基于该属性筛选用户。 选择“保存”按钮以提交任何更改****。

   |特性|类型|
   |---|---|
   |userName|String|
   |externalId|String|
   |活动|Boolean|
   |displayName|String|
   |title|String|
   |emails[type eq "work"].value|String|
   |地址[类型 eq"工作"\国家/地区|String|
   |地址_类型 eq"工作"\区域|String|
   |地址_类型 eq"工作"\局部性|String|
   |addresses[type eq "work"].postalCode|String|
   |addresses[type eq "work"].streetAddress|String|
   |name.givenName|String|
   |name.familyName|String|
   |phoneNumbers[type eq "mobile"].value|String|
   |urn：ietf：参数：scim：架构：扩展：企业：2.0：用户：部门|String|
   |urn：ietf：参数：scim：架构：扩展：企业：2.0：用户：经理|参考|

10. 若要配置范围筛选器，请参阅[范围筛选器教程](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)中提供的以下说明。

11. 要启用环中央的 Azure AD 预配服务，在 **"设置"** 部分将**预配状态**更改为 **"打开**"。

    ![预配状态已打开](common/provisioning-toggle-on.png)

12. 通过在 **"设置"** 部分中选择"**范围"** 中所需的值，定义要预配到 RingCentral 的用户和/或组。

    ![预配范围](common/provisioning-scope.png)

13. 已准备好预配时，单击“保存”****。

    ![保存预配配置](common/provisioning-configuration-save.png)

此操作将开始 **"设置"** 部分中 **"范围**"中定义的所有用户和组的初始同步周期。 初始周期执行的时间比后续周期长，只要 Azure AD 预配服务运行，则大约每 40 分钟执行一次。 

## <a name="step-6-monitor-your-deployment"></a>步骤 6. 监视部署
配置预配后，请使用以下资源监视部署：

1. 使用[预配日志](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs)确定已成功预配或未成功预配哪些用户
2. 检查[进度栏](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user)以查看预配周期的状态以及预配周期与完成有多近
3. 如果预配配置似乎处于不正常状态，则应用程序将进入隔离状态。 在此处了解有关隔离状态的更多[。](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)

## <a name="additional-resources"></a>其他资源

* [管理企业应用的用户帐户预配](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [什么是使用 Azure 活动目录的应用程序访问和单一登录？](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>后续步骤

* [了解如何查看日志并获取有关预配活动的报告](../manage-apps/check-status-user-account-provisioning.md)