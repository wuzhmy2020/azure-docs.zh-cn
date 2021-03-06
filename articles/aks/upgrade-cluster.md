---
title: 升级 Azure Kubernetes 服务 (AKS) 群集
description: 了解如何升级 Azure 库伯奈斯服务 （AKS） 群集以获取最新功能和安全更新。
services: container-service
ms.topic: article
ms.date: 05/31/2019
ms.openlocfilehash: 183e0a85f65d24dc7133307391931bea754a456d
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632611"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>升级 Azure Kubernetes 服务 (AKS) 群集

在 AKS 群集的生命周期中，经常需要升级到最新的 Kubernetes 版本。 必须应用最新的 Kubernetes 安全版本，或者通过升级来获取最新功能。 本文演示如何在 AKS 群集中升级主组件或单个默认的节点池。

对于使用多个节点池或 Windows 服务器节点（当前在 AKS 中预览）的 AKS 群集，请参阅[在 AKS 中升级节点池][nodepool-upgrade]。

## <a name="before-you-begin"></a>在开始之前

本文要求运行 Azure CLI 2.0.65 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI][azure-cli-install]。

> [!WARNING]
> AKS 群集升级会触发节点的隔离和排空。 如果可用计算配额较低，则升级可能会失败。 有关详细信息，请参阅[增加配额](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request)。
> 如果您正在运行自己的群集自动缩放器部署，请在升级期间禁用它（您可以将其缩放为零副本），因为它可能会干扰升级过程。 托管自动缩放程序会自动处理此问题。 

## <a name="check-for-available-aks-cluster-upgrades"></a>检查是否有可用的 AKS 群集升级

若要检查哪些 Kubernetes 版本可用于群集，请使用 [az aks get-upgrades][az-aks-get-upgrades] 命令。 以下示例在名为 *myResourceGroup* 的资源组中检查是否有可供名为 *myAKSCluster* 的群集使用的升级：

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

> [!NOTE]
> 升级 AKS 群集时，不能跳过 Kubernetes 次要版本。 例如，允许在*1.12.x* -> *1.13.x*或*1.13.x* -> *1.14.x*之间进行升级，但不允许升级*1.12.x* -> *1.14.x。*
>
> 要升级，从*1.12.x* -> *1.14.x*，首先从*1.12.x* -> *1.13.x*升级，然后从*1.13.x* -> *1.14.x*升级。

以下示例输出表明，群集可以升级到版本 1.13.9** 和 1.13.10**：

```console
Name     ResourceGroup     MasterVersion    NodePoolVersion    Upgrades
-------  ----------------  ---------------  -----------------  ---------------
default  myResourceGroup   1.12.8           1.12.8             1.13.9, 1.13.10
```
如果没有可用的升级，你将获得：
```console
ERROR: Table output unavailable. Use the --query option to specify an appropriate query. Use --debug for more info.
```

## <a name="upgrade-an-aks-cluster"></a>升级 AKS 群集

如果有一系列适用于 AKS 群集的版本，则可使用 [az aks upgrade][az-aks-upgrade] 命令进行升级。 在升级过程中，AKS 将向运行指定 Kubernetes 版本的群集添加一个新节点，然后仔细地一次[隔离并清空][kubernetes-drain]一个旧节点，将对正在运行的应用程序造成的中断情况降到最低。 确认新节点运行应用程序 Pod 以后，就会删除旧节点。 此过程会重复进行，直至群集中的所有节点都已升级完毕。

以下示例将群集升级到版本 1.13.10**：

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.13.10
```

升级群集需要几分钟时间，具体取决于有多少节点。 

> [!NOTE]
> 允许群集升级完成的总时间。 此时间是通过取 `10 minutes * total number of nodes in the cluster` 的乘积来计算的。 例如，在 20 节点群集中，升级操作必须在 200 分钟内成功，否则 AKS 将使操作失败，以避免出现无法恢复的群集状态。 若要在升级失败时恢复，请在达到超时值后重试升级操作。

若要确认升级是否成功，请使用 [az aks show][az-aks-show] 命令：

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

以下示例输出表明群集现在运行 1.13.10**：

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ---------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.13.10               Succeeded            myaksclust-myresourcegroup-19da35-90efab95.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>后续步骤

本文演示了如何升级现有的 AKS 群集。 若要详细了解如何部署和管理 AKS 群集，请参阅相关教程系列。

> [!div class="nextstepaction"]
> [AKS 教程][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[az-aks-show]: /cli/azure/aks#az-aks-show
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
