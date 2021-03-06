---
title: 教程 - 为 Terraform 配置 Azure Cloud Shell
description: 本教程介绍如何将 Terraform 与 Azure Cloud Shell 配合使用以简化身份验证和模板配置。
keywords: azure devops terraform cloud shell
ms.topic: tutorial
ms.date: 03/09/2020
ms.openlocfilehash: 3a9db1143ba07b549a271d53d610e0a4853467c6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "78945337"
---
# <a name="tutorial-configure-azure-cloud-shell-for-terraform"></a>教程：为 Terraform 配置 Azure Cloud Shell

Terraform 适用于 macOS、Windows 或 Linux 中的 Bash 命令行。 在 [Azure Cloud Shell](/azure/cloud-shell/overview) 的 Bash 体验中运行 Terraform 配置具有一些独特优势。 本教程演示如何编写使用 Cloud Shell 部署到 Azure 的 Terraform 脚本。

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>自动凭据配置

Terraform 已安装并可立即在 Cloud Shell 中使用。 Terraform 脚本在登录 Cloud Shell 时通过 Azure 进行身份验证以管理基础结构，而无需任何其他配置。 自动身份验证会绕过两个手动过程：
- 创建 Active Directory 服务主体
- 配置 Azure Terraform 提供程序变量


## <a name="use-modules-and-providers"></a>使用模块和提供程序

Azure Terraform 模块需要凭据才能访问和修改 Azure 资源。 若要在 Cloud Shell 中使用 Terraform 模块，请添加以下代码：


```hcl
# Configure the Microsoft Azure Provider
provider "azurerm" {
    # The "feature" block is required for AzureRM provider 2.x. 
    # If you are using version 1.x, the "features" block is not allowed.
    version = "~>2.0"
    features {}
}
```

使用任何 `terraform` CLI 命令时，Cloud Shell 会通过环境变量传递 `azurerm` 提供程序所需的值。

## <a name="other-cloud-shell-developer-tools"></a>其他 Cloud Shell 开发人员工具

文件和 shell 状态保留在 Cloud Shell 会话间的 Azure 存储中。 使用 [Azure 存储资源管理器](/azure/vs-azure-tools-storage-manage-with-storage-explorer)将文件从本地计算机复制和上传到 Cloud Shell。

Azure CLI 可在 Cloud Shell 中使用，是一款在 `terraform apply` 或 `terraform destroy` 完成后测试配置和检查工作的出色工具。


## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用模块注册表创建小型 VM 群集](terraform-create-vm-cluster-module.md)