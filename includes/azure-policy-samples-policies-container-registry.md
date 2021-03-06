---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/02/2020
ms.author: dacoulte
ms.openlocfilehash: 888f8954ede7b733b89a4c3215a889eae6825e4d
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2020
ms.locfileid: "80624184"
---
|名称 |说明 |效果 |版本 |GitHub |
|---|---|---|---|---|
|[容器注册表应使用客户托管密钥 (CMK) 进行加密](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5b9159ae-1701-4a6f-9a7a-aa9c8ddd0580) |审核未通过客户托管密钥 (CMK) 启用加密的容器注册表。 若要详细了解 CMK 加密，请访问 https://aka.ms/acr/CMK 。 |Audit, 已禁用 |1.0.0-preview |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_CMKEncryptionEnabled_Audit.json)
|[容器注册表不得允许无限制的网络访问](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd0793b48-0edc-4296-a390-4c75d1bdfd71) |审核容器注册表，这些注册表默认情况下未配置任何网络（IP 或 VNET）规则，因此允许所有网络访问。 如果容器注册表至少有一个 IP/防火墙规则或配置了虚拟网络，则会将其视为合规。 有关容器注册表网络规则的详细信息，请访问： https://aka.ms/acr/vnet 。 |Audit, 已禁用 |1.0.0-preview |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_NetworkRulesExist_Audit.json)
