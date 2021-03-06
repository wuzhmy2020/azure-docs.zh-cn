---
title: 使用 Azure CLI 管理 Azure 区块链服务
description: 如何使用 Azure CLI 管理 Azure 区块链服务
ms.date: 11/22/2019
ms.topic: article
ms.reviewer: janders
ms.openlocfilehash: ac75be644877905c1517395c1c789b1ea16fd49c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "74455585"
---
# <a name="manage-azure-blockchain-service-using-azure-cli"></a>使用 Azure CLI 管理 Azure 区块链服务

除了 Azure 门户之外，您还可以使用 Azure CLI 管理 Azure 区块链服务的区块链成员和事务节点。

请确保已安装最新的[Azure CLI，](https://docs.microsoft.com/cli/azure/install-azure-cli)并登录到 中的`az login`Azure 帐户。

在以下示例中，将示例`<parameter names>`替换为您自己的值。

## <a name="create-blockchain-member"></a>创建区块链成员

示例在 Azure 区块链服务中创建一个区块链成员，该成员在新联合体中运行 Quorum 分类帐协议。

```azurecli
az resource create \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName> \
                     --resource-type Microsoft.Blockchain/blockchainMembers \
                     --is-full-object \
                     --properties '{ "location":"<myBlockchainLocation>", "properties": {"password":"<myStrongPassword>", "protocol":"Quorum","consortium":"<myConsortiumName>", "consortiumManagementAccountPassword":"<myConsortiumManagementAccountPassword>", "firewallRules":[{"ruleName":"<myRuleName>","startIpAddress":"<myStartIpAddress>", "endIpAddress":"<myEndIpAddress>"}]}, "sku":{"name":"<skuName>"}}'
```

| 参数 | 描述 |
|---------|-------------|
| **resource-group** | 在其中创建 Azure 区块链服务资源的资源组名称。 |
| **name** | 用于标识 Azure 区块链服务区块链成员的唯一名称。 此名称用于公共终结点地址。 例如，`myblockchainmember.blockchain.azure.com` 。 |
| **位置** | 在其中创建区块链成员的 Azure 区域。 例如，`eastus` 。 选择最靠近用户或其他 Azure 应用程序的位置。 |
| **密码** | 成员帐户密码。 成员帐户密码用来使用基本身份验证向区块链成员的公共终结点进行身份验证。 密码必须满足以下四个要求中的三个：长度必须在 12 & 72 个字符、1 个小写字符、1 个大写字符、1 个数字和 1 个特殊字符之间，不是数字符号（*）、百分比（）、逗号（、星\`形）、回引号（）、双引号（）、单引号（）、破折号（-）和半列（;)|
| **协议** | 公共预览支持仲裁。 |
| **财团** | 要加入或创建的联盟的名称。 |
| **consortiumManagementAccountPassword** | 联盟管理密码。 密码用于加入联合体。 |
| **规则名称** | 将 IP 地址范围列入白名单的规则名称。 防火墙规则的可选参数。|
| **启动 Ip 地址** | 开始用于白名单的 IP 地址范围。 防火墙规则的可选参数。 |
| **端Ip地址** | 用于白名单的 IP 地址范围的末尾。 防火墙规则的可选参数。 |
| **skuName** | 层类型。 对于标准层，请使用 S0，对于基本层，请使用 B0。 |

## <a name="change-blockchain-member-password"></a>更改区块链成员密码

更改区块链成员密码的示例。

```azurecli
az resource update \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName> \
                     --resource-type Microsoft.Blockchain/blockchainMembers \
                     --set properties.password='<myStrongPassword>' \
                     --remove properties.consortiumManagementAccountAddress
```

| 参数 | 描述 |
|---------|-------------|
| **resource-group** | 在其中创建 Azure 区块链服务资源的资源组名称。 |
| **name** | 标识 Azure 区块链服务成员的名称。 |
| **密码** | 成员帐户密码。 密码必须满足以下四个要求中的三个：长度必须在 12 & 72 个字符、1 个小写字符、1 个大写字符、1 个数字和 1 个特殊字符之间，这些字符不是数字符号（*）、百分比（）、逗号（、星形）、回引号（）、\`双引号（）、单引号（）、单引号（'）、破折号（-）和分号（;)。 |

## <a name="create-transaction-node"></a>创建事务节点

在现有区块链成员内部创建交易节点。 通过添加事务节点，可以增加安全隔离和分配负载。 例如，您可以为不同的客户端应用程序提供事务节点终结点。

```azurecli
az resource create \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName>/transactionNodes/<myTransactionNode> \
                     --resource-type Microsoft.Blockchain/blockchainMembers \
                     --is-full-object \
                     --properties '{"location":"<myRegion>", "properties":{"password":"<myStrongPassword>", "firewallRules":[{"ruleName":"<myRuleName>", "startIpAddress":"<myStartIpAddress>", "endIpAddress":"<myEndIpAddress>"}]}}'
```

| 参数 | 描述 |
|---------|-------------|
| **resource-group** | 在其中创建 Azure 区块链服务资源的资源组名称。 |
| **name** | Azure 区块链服务区块链成员的名称，该成员还包括新的交易节点名称。 |
| **位置** | 在其中创建区块链成员的 Azure 区域。 例如，`eastus` 。 选择最靠近用户或其他 Azure 应用程序的位置。 |
| **密码** | 事务节点密码。 密码必须满足以下四个要求中的三个：长度必须在 12 & 72 个字符、1 个小写字符、1 个大写字符、1 个数字和 1 个特殊字符之间，这些字符不是数字符号（*）、百分比（）、逗号（、星形）、回引号（）、\`双引号（）、单引号（）、单引号（'）、破折号（-）和分号（;)。 |
| **规则名称** | 将 IP 地址范围列入白名单的规则名称。 防火墙规则的可选参数。 |
| **启动 Ip 地址** | 开始用于白名单的 IP 地址范围。 防火墙规则的可选参数。 |
| **端Ip地址** | 用于白名单的 IP 地址范围的末尾。 防火墙规则的可选参数。|

## <a name="change-transaction-node-password"></a>更改事务节点密码

示例更改事务节点密码。

```azurecli
az resource update \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName>/transactionNodes/<myTransactionNode> \
                     --resource-type Microsoft.Blockchain/blockchainMembers \
                     --set properties.password='<myStrongPassword>'
```

| 参数 | 描述 |
|---------|-------------|
| **resource-group** | Azure 区块链服务资源所在的资源组名称。 |
| **name** | Azure 区块链服务区块链成员的名称，该成员还包括新的交易节点名称。 |
| **密码** | 事务节点密码。 密码必须满足以下四个要求中的三个：长度必须在 12 & 72 个字符、1 个小写字符、1 个大写字符、1 个数字和 1 个特殊字符之间，这些字符不是数字符号（*）、百分比（）、逗号（、星形）、回引号（）、\`双引号（）、单引号（）、单引号（'）、破折号（-）和分号（;)。 |

## <a name="change-consortium-management-account-password"></a>更改联合体管理帐户密码

联营管理帐户用于联营成员管理。 每个成员都由联合体管理帐户唯一标识，您可以使用以下命令更改此帐户的密码。

```azurecli
az resource update \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName> \
                     --resource-type Microsoft.Blockchain/blockchainMembers \
                     --set properties.consortiumManagementAccountPassword='<myConsortiumManagementAccountPassword>' \
                     --remove properties.consortiumManagementAccountAddress
```

| 参数 | 描述 |
|---------|-------------|
| **resource-group** | 在其中创建 Azure 区块链服务资源的资源组名称。 |
| **name** | 标识 Azure 区块链服务成员的名称。 |
| **consortiumManagementAccountPassword** | 联营管理帐户密码。 密码必须满足以下四个要求中的三个：长度必须在 12 & 72 个字符、1 个小写字符、1 个大写字符、1 个数字和 1 个特殊字符之间，这些字符不是数字符号（*）、百分比（）、逗号（、星形）、回引号（）、\`双引号（）、单引号（）、单引号（'）、破折号（-）和分号（;)。 |
  
## <a name="update-firewall-rules"></a>更新防火墙规则

```azurecli
az resource update \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName> \
                     --resource-type Microsoft.Blockchain/blockchainMembers \
                     --set properties.firewallRules='[{"ruleName":"<myRuleName>", "startIpAddress":"<myStartIpAddress>", "endIpAddress":"<myEndIpAddress>"}]' \
                     --remove properties.consortiumManagementAccountAddress
```

| 参数 | 描述 |
|---------|-------------|
| **resource-group** | Azure 区块链服务资源所在的资源组名称。 |
| **name** | Azure 区块链服务区块链成员的名称。 |
| **规则名称** | 将 IP 地址范围列入白名单的规则名称。 防火墙规则的可选参数。|
| **启动 Ip 地址** | 开始用于白名单的 IP 地址范围。 防火墙规则的可选参数。|
| **端Ip地址** | 用于白名单的 IP 地址范围的末尾。 防火墙规则的可选参数。|

## <a name="list-api-keys"></a>列出 API 密钥

API 密钥可用于类似于用户名和密码的节点访问。 有两个 API 密钥支持密钥轮换。 使用以下命令列出 API 密钥。

```azurecli
az resource invoke-action \
                            --resource-group <myResourceGroup> \
                            --name <myMemberName>/transactionNodes/<myTransactionNode> \
                            --action "listApiKeys" \
                            --resource-type Microsoft.Blockchain/blockchainMembers
```

| 参数 | 描述 |
|---------|-------------|
| **resource-group** | Azure 区块链服务资源所在的资源组名称。 |
| **name** | Azure 区块链服务区块链成员的名称，该成员还包括新的交易节点名称。 |

## <a name="regenerate-api-keys"></a>重新生成 API 密钥

使用以下命令重新生成 API 密钥。

```azurecli
az resource invoke-action \
                            --resource-group <myResourceGroup> \
                            --name <myMemberName>/transactionNodes/<myTransactionNode> \
                            --action "regenerateApiKeys" \
                            --resource-type Microsoft.Blockchain/blockchainMembers \
                            --request-body '{"keyName":"<keyValue>"}'
```

| 参数 | 描述 |
|---------|-------------|
| **resource-group** | Azure 区块链服务资源所在的资源组名称。 |
| **name** | Azure 区块链服务区块链成员的名称，该成员还包括新的交易节点名称。 |
| **键名称** | 将\<键值\>替换为键 1 或键 2。 |

## <a name="delete-a-transaction-node"></a>删除事务节点

示例删除区块链成员交易节点。

```azurecli
az resource delete \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName>/transactionNodes/<myTransactionNode> \
                     --resource-type Microsoft.Blockchain/blockchainMembers
```

| 参数 | 描述 |
|---------|-------------|
| **resource-group** | Azure 区块链服务资源所在的资源组名称。 |
| **name** | Azure 区块链服务区块链成员的名称，该成员还包括要删除的交易节点名称。 |

## <a name="delete-a-blockchain-member"></a>删除区块链成员

示例删除区块链成员。

```azurecli
az resource delete \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName> \
                     --resource-type Microsoft.Blockchain/blockchainMembers
```

| 参数 | 描述 |
|---------|-------------|
| **resource-group** | Azure 区块链服务资源所在的资源组名称。 |
| **name** | 要删除的 Azure 区块链服务区块链成员的名称。 |

## <a name="azure-active-directory"></a>Azure Active Directory

### <a name="grant-access-for-azure-ad-user"></a>授予 Azure AD 用户的权限

```azurecli
az role assignment create \
                            --role <role> \
                            --assignee <assignee> \
                            --scope /subscriptions/<subId>/resourceGroups/<groupName>/providers/Microsoft.Blockchain/blockchainMembers/<myMemberName>
```

| 参数 | 描述 |
|---------|-------------|
| **作用** | Azure AD 角色的名称。 |
| **assignee** | Azure AD 用户 ID。 例如： `user@contoso.com` |
| **scope** | 角色分配的范围。 可以是区块链成员或交易节点。 |

**例子：**

将 Azure AD 用户节点访问权限授予区块链**成员**：

```azurecli
az role assignment create \
                            --role 'myRole' \
                            --assignee user@contoso.com \
                            --scope /subscriptions/mySubscriptionId/resourceGroups/contosoResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/contosoMember1
```

**例子：**

授予 Azure AD 用户对区块链**事务节点**的节点访问权限：

```azurecli
az role assignment create \
                            --role 'MyRole' \
                            --assignee user@contoso.com \
                            --scope /subscriptions/mySubscriptionId/resourceGroups/contosoResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/contosoMember1/transactionNodes/contosoTransactionNode1
```

### <a name="grant-node-access-for-azure-ad-group-or-application-role"></a>授予 Azure AD 组或应用程序角色的节点访问权限

```azurecli
az role assignment create \
                            --role <role> \
                            --assignee-object-id <assignee_object_id>
```

| 参数 | 描述 |
|---------|-------------|
| **作用** | Azure AD 角色的名称。 |
| **受让人-对象-id** | Azure AD 组 ID 或应用程序 ID。 |
| **scope** | 角色分配的范围。 可以是区块链成员或交易节点。 |

**例子：**

授予**应用程序角色的**节点访问权限

```azurecli
az role assignment create \
                            --role 'myRole' \
                            --assignee-object-id 22222222-2222-2222-2222-222222222222 \
                            --scope /subscriptions/mySubscriptionId/resourceGroups/contosoResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/contosoMember1
```

### <a name="remove-azure-ad-node-access"></a>删除 Azure AD 节点访问

```azurecli
az role assignment delete \
                            --role <myRole> \
                            --assignee <assignee> \
                            --scope /subscriptions/mySubscriptionId/resourceGroups/<myResourceGroup>/providers/Microsoft.Blockchain/blockchainMembers/<myMemberName>/transactionNodes/<myTransactionNode>
```

| 参数 | 描述 |
|---------|-------------|
| **作用** | Azure AD 角色的名称。 |
| **assignee** | Azure AD 用户 ID。 例如： `user@contoso.com` |
| **scope** | 角色分配的范围。 可以是区块链成员或交易节点。 |

## <a name="next-steps"></a>后续步骤

了解如何使用[Azure 门户配置 Azure 区块链服务事务节点](configure-transaction-nodes.md)。
