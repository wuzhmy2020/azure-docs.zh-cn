---
title: Azure 多重身份验证概述
description: 了解 Azure 多重身份验证如何帮助保护对数据和应用程序的访问，同时满足用户对简单登录过程的需求。
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: c50232abd12c8c0390409bd7bf72833b4f153e02
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/05/2020
ms.locfileid: "80667358"
---
# <a name="how-it-works-azure-multi-factor-authentication"></a>工作原理：Azure 多重身份验证

多重身份验证是一种过程。在该过程中，系统会在用户登录时提示其输入其他形式的标识，例如在其手机上输入代码或提供指纹扫描。

如果只使用密码对用户进行身份验证，则会留下不安全的矢量，容易受到攻击。 如果密码弱或者已在其他位置公开，那么如何确定是该用户在使用用户名和密码登录，还是攻击者在登录？ 需要另一种形式的身份验证时，会提高安全性，因为攻击者并不容易获取或复制进行多重身份验证所需的额外内容。

![不同形式的多重身份验证的概念图](./media/concept-mfa-howitworks/methods.png)

Azure 多重身份验证需要以下身份验证方法中的两种或更多种才能运作：

* 你知道的某样东西，通常为密码。
* 你有的某样东西，例如无法轻易复制的可信设备，如电话或硬件密钥。
* 自身的特征 - 生物识别，如指纹或面部扫描。

用户只需执行一个步骤即可自行注册自助式密码重置和 Azure 多重身份验证，这样可以简化加入体验。 管理员可以定义能够使用的辅助身份验证形式。 当用户执行自助式密码重置以进一步保护该过程时，也可能需要 Azure 多重身份验证。

![登录屏幕上使用的身份验证方法](media/concept-authentication-methods/overview-login.png)

Azure 多重身份验证有助于保护对数据和应用程序的访问，同时保持用户的简单性。 它通过要求第二种形式的身份验证提供额外的安全性，并通过一系列简单的[身份验证方法](concept-authentication-methods.md)提供增强式身份验证。 根据管理员制定的配置决策，用户可能会受到 MFA 的质疑，也可能不会受到 MFA 的质疑。

应用程序或服务无需进行任何更改，即使用 Azure 多重身份验证。 验证提示是 Azure AD 登录事件的一部分，该事件在需要时自动请求和处理 MFA 质询。

## <a name="available-verification-methods"></a>可用的验证方法

当用户登录到应用程序或服务并收到 MFA 提示时，他们可以选择其注册的其他验证形式之一。 管理员可能需要注册这些 Azure 多重身份验证验证方法，或者用户可以访问自己的["我的配置文件"](https://myprofile.microsoft.com)来编辑或添加验证方法。

以下其他验证形式可用于 Azure 多重身份验证：

* Microsoft Authenticator 应用
* OATH 硬件令牌
* SMS
* 语音呼叫

## <a name="how-to-enable-and-use-azure-multi-factor-authentication"></a>如何启用和使用 Azure 多重身份验证

可以为 Azure 多重身份验证启用用户和组，以在登录事件期间提示进行其他验证。 所有 Azure AD 租户都可以使用[安全默认值](../fundamentals/concept-fundamentals-security-defaults.md)，以便快速启用所有用户使用 Microsoft 身份验证器应用。

对于更精细的控件，[条件访问](../conditional-access/overview.md)策略可用于定义需要 MFA 的事件或应用程序。 当用户位于公司网络或已注册的设备上时，这些策略可以允许定期登录事件，但在远程或个人设备上提示其他验证因素。

![有关条件访问如何保护登录过程的概览图](media/tutorial-enable-azure-mfa/conditional-access-overview.png)

## <a name="next-steps"></a>后续步骤

要了解许可，请参阅[Azure 多重身份验证的功能和许可证](concept-mfa-licensing.md)。

要查看 MFA 的运行情况，请在以下教程中为一组测试用户启用 Azure 多重身份验证：

> [!div class="nextstepaction"]
> [启用 Azure 多重身份验证](tutorial-mfa-applications.md)
