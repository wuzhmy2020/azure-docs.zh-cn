---
title: 使用 Azure Migrate 准备 Hyper-V VM 以进行评估/迁移
description: 了解如何使用 Azure Migrate 准备评估/迁移 Hyper-V VM。
ms.topic: tutorial
ms.date: 01/01/2020
ms.custom: mvc
ms.openlocfilehash: 1d327f558806e0205540c183c56b92ba31e33cb7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "77031214"
---
# <a name="prepare-for-assessment-and-migration-of-hyper-v-vms-to-azure"></a>准备评估 Hyper-V VM 并将其迁移到 Azure

本文介绍如何使用 [Azure Migrate](migrate-services-overview.md) 准备好要评估并迁移到 Azure 的本地 Hyper-V VM。

[Azure Migrate](migrate-overview.md) 在一个中心位置提供多种工具，帮助你发现、评估应用、基础结构和工作负荷并将其迁移到 Microsoft Azure。 该中心包含 Azure Migrate 工具，以及第三方独立软件供应商 (ISV) 的产品/服务。

本教程是介绍如何评估 Hyper-V VM 以及将其迁移到 Azure 的教程系列中的第一篇文章。 在本教程中，你将了解如何执行以下操作：

> [!div class="checklist"]
> * 准备 Azure。 设置要与 Azure Migrate 配合使用的 Azure 帐户和资源的权限。
> * 准备本地 Hyper-V 主机和 VM 以进行服务器评估。 可以使用配置脚本准备，也可以手动准备。
> * 准备部署 Azure Migrate 设备。 该设备用于发现和评估本地 VM。
> * 准备本地 Hyper-V 主机和 VM 以进行服务器迁移。


> [!NOTE]
> 教程中演示了方案的最简单部署路径，使你能够快速设置概念证明。 教程尽可能使用默认选项，不会演示所有可能的设置和路径。 有关详细说明，请查看 Hyper-V 评估和迁移的操作指南。


如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/pricing/free-trial/)。


## <a name="prepare-azure"></a>准备 Azure

### <a name="azure-permissions"></a>Azure 权限

需要为 Azure Migrate 部署设置权限。

**任务** | **权限**
--- | ---
**创建 Azure Migrate 项目** | Azure 帐户需要创建项目的权限。
**注册 Azure Migrate 设备** | Azure Migrate 使用轻型 Azure Migrate 设备通过 Azure Migrate 服务器评估来发现并评估 Hyper-V VM。 此设备发现 VM，并将 VM 元数据和性能数据发送到 Azure Migrate。<br/><br/>在设备注册过程中，以下资源提供程序将注册到在设备中选择的订阅：Microsoft.OffAzure、Microsoft.Migrate 和 Microsoft.KeyVault。 通过注册资源提供程序来配置订阅，以供资源提供程序使用。 需要订阅的“参与者”或“所有者”角色才能注册资源提供程序。<br/><br/> 在载入过程中，Azure Migrate 将创建一个 Azure Active Directory (Azure AD) 应用：<br/> AAD 应用用于在代理（在设备上运行）与其各自在 Azure 上运行的服务之间通信（身份验证和授权）。 此应用无权对任何资源进行 ARM 调用，也没有 RBAC 访问权限。



### <a name="assign-permissions-to-create-project"></a>分配创建项目的权限

请检查你是否有权创建 Azure Migrate 项目。

1. 在 Azure 门户中打开订阅，然后选择“访问控制(IAM)”。 
2. 在“检查访问权限”中找到相关的帐户，然后单击它以查看权限。 
3. 你应该拥有“参与者”或“所有者”权限。  
    - 如果你刚刚创建了免费的 Azure 帐户，那么你就是订阅的所有者。
    - 如果你不是订阅所有者，请让所有者分配该角色。


### <a name="assign-permissions-to-register-the-appliance"></a>分配注册设备的权限

可以使用以下方法之一为 Azure Migrate 分配权限，以便在注册设备期间创建 Azure AD 应用：

- 租户/全局管理员可为租户中的用户授予创建和注册 Azure AD 应用的权限。
- 租户/全局管理员可将“应用程序开发人员”角色（拥有权限）分配到帐户。

> [!NOTE]
> - 除上述权限外，应用对订阅没有任何其他访问权限。
> - 只有在注册新的设备时，你才需要这些权限。 设置设备后可以删除这些权限。


#### <a name="grant-account-permissions"></a>授予帐户权限

租户/全局管理员可按如下所述授予权限：

1. 在 Azure AD 中，租户/全局管理员应导航到“Azure Active Directory” **“用户”** “用户设置” >    >   。
2. 管理员应将“应用注册”设置为“是”   。

    ![Azure AD 权限](./media/tutorial-prepare-hyper-v/aad.png)

> [!NOTE]
> 这是不受影响的默认设置。 [了解详细信息](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance)。



#### <a name="assign-application-developer-role"></a>分配“应用程序开发人员”角色

租户/全局管理员可将“应用程序开发人员”角色分配到帐户。 [了解详细信息](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal)。


## <a name="prepare-hyper-v-for-assessment"></a>准备 Hyper-V 以进行评估

可以手动或使用配置脚本为 VM 评估准备 Hyper-V。 下面是需要使用脚本或[手动](#prepare-hyper-v-manually)准备的内容。

- [验证](migrate-support-matrix-hyper-v.md#hyper-v-host-requirements) Hyper-V 主机设置，并确保在 Hyper-V 主机上打开了[所需的端口](migrate-support-matrix-hyper-v.md#port-access)。
- 在每台主机上设置 PowerShell 远程控制，使 Azure Migrate 设备能够通过 WinRM 连接在主机上运行 PowerShell 命令。
- 如果 VM 磁盘位于远程 SMB 共享上，则委派凭据。
- 在 Hyper-V 主机上设置供设备用来发现 VM 的帐户。
- 在要发现和评估的每个 VM 上，启用 Hyper-V Integration Services。



## <a name="prepare-with-a-script"></a>使用脚本准备

此脚本执行以下任务：

- 检查你是否在受支持的 PowerShell 版本中运行该脚本。
- 验证你（运行脚本的用户）是否对 Hyper-V 主机拥有管理特权。
- 允许你创建一个本地用户帐户（非管理员），供 Azure Migrate 服务用来与 Hyper-V 主机通信。 此用户帐户将添加到主机上的以下组：
    - 远程管理用户
    - Hyper-V 管理员
    - 性能监视器用户
- 检查主机是否正在运行受支持的 Hyper-V 版本，并检查 Hyper-V 角色。
- 启用 WinRM 服务，并在主机上打开端口 5985 (HTTP) 和 5986 (HTTPS)（收集元数据时需要使用这些端口）。
- 在主机上启用 PowerShell 远程控制。
- 检查主机管理的所有 VM 上是否已启用 Hyper-V 集成服务。
- 根据需要在主机上启用 CredSSP。

按如下所示运行脚本：

1. 确保在 Hyper-V 主机上安装了 PowerShell 4.0 或更高版本。
2. 从 [Microsoft 下载中心](https://aka.ms/migrate/script/hyperv)下载脚本。 该脚本已由 Microsoft 加密签名。
3. 使用 MD5 或 SHA256 哈希文件验证脚本完整性。 井号标签值如下。 运行以下命令生成脚本的哈希：
    ```
    C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]
    ```
    用法示例：
    ```
    C:\>CertUtil -HashFile C:\Users\Administrators\Desktop\ MicrosoftAzureMigrate-Hyper-V.ps1
    SHA256
    ```

4.  验证脚本完整性后，在每台 Hyper-V 主机上结合以下 PowerShell 命令运行该脚本：
    ```
    PS C:\Users\Administrators\Desktop> MicrosoftAzureMigrate-Hyper-V.ps1
    ```

### <a name="hashtag-values"></a>井号标签值

哈希值为:

| **哈希** | **值** |
| --- | --- |
| **MD5** | 0ef418f31915d01f896ac42a80dc414e |
| **SHA256** | 0ad60e7299925eff4d1ae9f1c7db485dc9316ef45b0964148a3c07c80761ade2 |


## <a name="prepare-hyper-v-manually"></a>手动准备 Hyper-V

按照本部分中的过程手动准备 Hyper-V，而不是使用脚本。

### <a name="verify-powershell-version"></a>验证 PowerShell 版本

确保在 Hyper-V 主机上安装了 PowerShell 4.0 或更高版本。



### <a name="set-up-an-account-for-vm-discovery"></a>设置用于 VM 发现的帐户

Azure Migrate 需要拥有发现本地 VM 的权限。

- 在 Hyper-V 主机/群集上设置拥有管理员权限的域或本地用户帐户。

    - 对于要包含在发现中的所有主机和群集，都需要设置一个帐户。
    - 该帐户可以是本地帐户或域帐户。 我们建议为该帐户分配 Hyper-V 主机或群集的管理员权限。
    - 或者，如果你不想要分配管理员权限，则需要分配以下权限：
        - 远程管理用户
        - Hyper-V 管理员
        - 性能监视器用户

### <a name="verify-hyper-v-host-settings"></a>验证 Hyper-V 主机设置

1. 验证服务器评估的 [Hyper-V 主机要求](migrate-support-matrix-hyper-v.md#hyper-v-host-requirements)。
2. 确保 Hyper-V 主机上已打开[所需的端口](migrate-support-matrix-hyper-v.md#port-access)。

### <a name="enable-powershell-remoting-on-hosts"></a>在主机上启用 PowerShell 远程处理

按如下所述在每台主机上设置 PowerShell 远程处理：

1. 在每台主机上，以管理员身份打开 PowerShell 控制台。
2. 运行以下命令：

    ```
    Enable-PSRemoting -force
    ```
### <a name="enable-integration-services-on-vms"></a>在 VM 上启用 Integration Services

应在每个 VM 上启用 Integration Services，使 Azure Migrate 能够捕获 VM 上的操作系统信息。

在要发现和评估的每个 VM 上，启用 [Hyper-V Integration Services](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services)。


### <a name="enable-credssp-on-hosts"></a>在主机上启用 CredSSP

如果主机上的 VM 包含位于 SMB 共享上的磁盘，请在主机上完成此步骤。

- 可在所有 Hyper-v 主机上远程运行此命令。
- 如果在群集中添加了新的主机节点，则会自动添加这些节点以供发现，但需要根据情况在新节点上手动启用 CredSSP。

按如下所示启用 CredSSP：

1. 识别运行包含 SMB 共享中的磁盘的 Hyper-V VM 的 Hyper-V 主机。
2. 在识别到的每台 Hyper-V 主机上运行以下命令：

    ```
    Enable-WSManCredSSP -Role Server -Force
    ```

设置设备时，[在设备上启用 CredSSP](tutorial-assess-hyper-v.md#delegate-credentials-for-smb-vhds) 即可完成 CredSSP 的设置。 本教程系列的下一篇文章对此做了介绍。


## <a name="prepare-for-appliance-deployment"></a>准备进行设备部署

在设置 Azure Migrate 设备并在下一篇教程中开始评估之前，需要准备好设备部署。

1. [验证](migrate-appliance.md#appliance---hyper-v)设备要求。
2. [查看](migrate-appliance.md#url-access)设备需要访问的 Azure URL。
3. 查看设备在发现和评估期间要收集的数据。
4. [注意](migrate-appliance.md#collected-data---hyper-v)设备的端口访问要求。


## <a name="prepare-for-hyper-v-migration"></a>准备 Hyper-V 迁移

1. [查看](migrate-support-matrix-hyper-v-migration.md#hyper-v-hosts) Hyper-V 主机迁移要求，以及迁移 VM 时 Hyper-V 主机和群集需要访问的 Azure URL。
2. [查看](migrate-support-matrix-hyper-v-migration.md#hyper-v-vms)要迁移到 Azure 的 Hyper-V VM 要求。


## <a name="next-steps"></a>后续步骤

本教程介绍以下操作：

> [!div class="checklist"]
> * 设置 Azure 帐户权限。
> * 准备要评估和迁移的 Hyper-V 主机与 VM。
> * 准备了 Azure Migrate 设备部署。

继续阅读下一篇教程，以创建 Azure Migrate 项目、部署设备，并发现和评估要迁移到 Azure 的 Hyper-V VM。

> [!div class="nextstepaction"]
> [评估 Hyper-V VM](./tutorial-assess-hyper-v.md)
