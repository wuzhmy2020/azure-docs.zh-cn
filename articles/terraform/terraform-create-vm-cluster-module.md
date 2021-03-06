---
title: 教程 - 使用模块注册表通过 Terraform 创建 Azure VM 群集
description: 本教程使用 Terraform 模块在 Azure 中创建 Windows 虚拟机群集
keywords: azure devops terraform vm virtual machine cluster module registry
ms.topic: tutorial
ms.date: 03/09/2020
ms.openlocfilehash: 9faeee9bb2f0fb6dc148a3868f6fc0dae3833a2a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "78945293"
---
# <a name="tutorial-create-an-azure-vm-cluster-with-terraform-using-the-module-registry"></a>教程：使用模块注册表通过 Terraform 创建 Azure VM 群集

本文介绍如何使用 Terraform [Azure 计算模块](https://registry.terraform.io/modules/Azure/compute/azurerm/1.0.2)创建小型 VM 群集。 本教程介绍如何执行下列操作： 

> [!div class="checklist"]
> * 使用 Azure 设置身份验证
> * 创建 Terraform 模板
> * 使用 plan 直观显示更改
> * 应用配置以创建 VM 群集

有关 Terraform 的更多信息，请参阅 [Terraform 文档](https://www.terraform.io/docs/index.html)。

## <a name="set-up-authentication-with-azure"></a>使用 Azure 设置身份验证

> [!TIP]
> 如果在 [Azure Cloud Shell](/azure/cloud-shell/overview) 中[使用 Terraform 环境变量](terraform-install-configure.md)或运行此教程，请跳过此步骤。

 查看[安装 Terraform 并配置对 Azure 的访问权限](terraform-install-configure.md)，以创建 Azure 服务主体。 通过此服务主体使用以下代码将新文件 `azureProviderAndCreds.tf` 填充到空目录中：

```hcl
variable subscription_id {}
variable tenant_id {}
variable client_id {}
variable client_secret {}

provider "azurerm" {
    version = "~>1.40"

    subscription_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    tenant_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_secret = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

## <a name="create-the-template"></a>创建模板

使用以下代码创建名为 `main.tf` 的新 Terraform 模板：

```hcl
module mycompute {
    source = "Azure/compute/azurerm"
    resource_group_name = "myResourceGroup"
    location = "East US 2"
    admin_password = "ComplxP@assw0rd!"
    vm_os_simple = "WindowsServer"
    is_windows_image = "true"
    remote_port = "3389"
    nb_instances = 2
    public_ip_dns = ["unique_dns_name"]
    vnet_subnet_id = module.network.vnet_subnets[0]
}

module "network" {
    source = "Azure/network/azurerm"
    location = "East US 2"
    resource_group_name = "myResourceGroup"
}

output "vm_public_name" {
    value = module.mycompute.public_ip_dns_name
}

output "vm_public_ip" {
    value = module.mycompute.public_ip_address
}

output "vm_private_ips" {
    value = module.mycompute.network_interface_private_ip
}
```

在配置目录中运行 `terraform init`。 使用不低于 0.10.6 的 Terraform 版本显示以下输出：

![Terraform Init](media/terraformInitWithModules.png)

## <a name="visualize-the-changes-with-plan"></a>使用 plan 直观显示更改

运行 `terraform plan` 预览由模板创建的虚拟机基础结构。

![Terraform Plan](media/terraform-create-vm-cluster-with-infrastructure/terraform-plan.png)


## <a name="create-the-virtual-machines-with-apply"></a>使用 apply 创建虚拟机

运行 `terraform apply` 在 Azure 上预配 VM。

![Terraform Apply](media/terraform-create-vm-cluster-with-infrastructure/terraform-apply.png)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"] 
> [浏览 Azure Terraform 模块列表](https://registry.terraform.io/modules/Azure)