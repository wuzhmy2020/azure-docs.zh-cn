---
title: 在 Azure 数据湖存储 Gen2（预览）中，对文件& ACL 使用 Azure CLI
description: 使用 Azure CLI 管理具有分层命名空间的存储帐户中的目录、文件和目录访问控制列表 （ACL）。
services: storage
author: normesta
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.topic: conceptual
ms.date: 11/24/2019
ms.author: normesta
ms.reviewer: prishet
ms.openlocfilehash: ce2b4200496938e6cffb935207df8c7027eaf37a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "77486128"
---
# <a name="use-azure-cli-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2-preview"></a>使用 Azure CLI 管理 Azure 数据湖存储 Gen2 中的目录、文件和 ACL（预览版）

本文介绍如何使用[Azure 命令行接口 （CLI）](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)创建和管理具有分层命名空间的存储帐户中的目录、文件和权限。 

> [!IMPORTANT]
> 本文`storage-preview`中介绍的扩展当前处于公共预览版中。

[示例](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview#adls-gen2-support) | [第 1 代到第 2 代映射](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview#mapping-from-adls-gen1-to-adls-gen2) | [提供反馈](https://github.com/Azure/azure-cli-extensions/issues)
## <a name="prerequisites"></a>先决条件

> [!div class="checklist"]
> * Azure 订阅。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。
> * 一个已启用分层命名空间 (HNS) 的存储帐户。 按[这些](data-lake-storage-quickstart-create-account.md)说明创建一个。
> * Azure CLI`2.0.67`版本或更高版本。

## <a name="install-the-storage-cli-extension"></a>安装存储 CLI 扩展

1. 打开[Azure 云外壳](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest)，或者如果您在本地[安装了](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)Azure CLI，请打开命令控制台应用程序，如 Windows PowerShell。

2. 使用以下命令验证已安装的 Azure CLI`2.0.67`版本是或更高版本。

   ```azurecli
    az --version
   ```
   如果版本的 Azure CLI 低于`2.0.67`，则安装更高版本。 请参阅[安装 Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)。

3. 安装 `storage-preview` 扩展。

   ```azurecli
   az extension add -n storage-preview
   ```

## <a name="connect-to-the-account"></a>连接到帐户

1. 如果您在本地使用 Azure CLI，则运行登录命令。

   ```azurecli
   az login
   ```

   如果 CLI 可以打开默认浏览器，它将这样做并加载 Azure 登录页。

   否则，请在 打开 浏览器[https://aka.ms/devicelogin](https://aka.ms/devicelogin)页面并输入终端中显示的授权代码。 然后，在浏览器中使用您的帐户凭据登录。

   若要详细了解不同的身份验证方法，请参阅使用 Azure CLI 登录。

2. 如果您的身份与多个订阅相关联，则将活动订阅设置为将承载静态网站的存储帐户的订阅。

   ```azurecli
   az account set --subscription <subscription-id>
   ```

   将`<subscription-id>`占位符值替换为订阅的 ID。

## <a name="create-a-file-system"></a>创建文件系统

文件系统充当文件的容器。 可以使用 命令创建一个`az storage container create`。 

此示例创建名为 `my-file-system` 的文件系统。

```azurecli
az storage container create --name my-file-system --account-name mystorageaccount
```

## <a name="create-a-directory"></a>创建目录

使用`az storage blob directory create`命令创建目录引用。 

本示例将名为 的`my-directory`目录添加到名为 的`my-file-system`文件系统中，该文件系统位于名为`mystorageaccount`的帐户中。

```azurecli
az storage blob directory create -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="show-directory-properties"></a>显示目录属性

可以使用 命令将目录的属性打印到控制台`az storage blob show`。

```azurecli
az storage blob directory show -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="rename-or-move-a-directory"></a>重命名或移动目录

使用`az storage blob directory move`命令重命名或移动目录。

此示例将目录的名称 `my-directory` 重命名为 `my-new-directory`。

```azurecli
az storage blob directory move -c my-file-system -d my-new-directory -s my-directory --account-name mystorageaccount
```

## <a name="delete-a-directory"></a>删除目录

使用`az storage blob directory delete`命令删除目录。

此示例删除名为 `my-directory` 的目录。 

```azurecli
az storage blob directory delete -c my-file-system -d my-directory --account-name mystorageaccount 
```

## <a name="check-if-a-directory-exists"></a>检查是否存在目录

使用`az storage blob directory exist`命令确定文件系统中是否存在特定目录。

此示例显示`my-directory``my-file-system`文件系统中是否存在名为的目录。 

```azurecli
az storage blob directory exists -c my-file-system -d my-directory --account-name mystorageaccount 
```

## <a name="download-from-a-directory"></a>从目录下载

使用`az storage blob directory download`命令从目录中下载文件。

此示例从名为 `my-directory` 的目录中下载名为 `upload.txt` 的文件。 

```azurecli
az storage blob directory download -c my-file-system --account-name mystorageaccount -s "my-directory/upload.txt" -d "C:\mylocalfolder\download.txt"
```

此示例下载整个目录。

```azurecli
az storage blob directory download -c my-file-system --account-name mystorageaccount -s "my-directory/" -d "C:\mylocalfolder" --recursive
```

## <a name="list-directory-contents"></a>列出目录内容

使用`az storage blob directory list`命令列出目录的内容。

此示例列出位于名为`my-directory``my-file-system``mystorageaccount`的存储帐户的文件系统中名为 的目录的内容。 

```azurecli
az storage blob directory list -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="upload-a-file-to-a-directory"></a>将文件上传到目录

使用`az storage blob directory upload`命令将文件上载到目录。

此示例将名为 `upload.txt` 的文件上传到名为 `my-directory` 的目录。 

```azurecli
az storage blob directory upload -c my-file-system --account-name mystorageaccount -s "C:\mylocaldirectory\upload.txt" -d my-directory
```

此示例上载整个目录。

```azurecli
az storage blob directory upload -c my-file-system --account-name mystorageaccount -s "C:\mylocaldirectory\" -d my-directory --recursive 
```

## <a name="show-file-properties"></a>显示文件属性

可以使用 命令将文件的属性打印到控制台`az storage blob show`。

```azurecli
az storage blob show -c my-file-system -b my-file.txt --account-name mystorageaccount
```

## <a name="rename-or-move-a-file"></a>重命名或移动文件

使用`az storage blob move`命令重命名或移动文件。

本示例将文件从名称`my-file.txt`重命名为 名称。 `my-file-renamed.txt`

```azurecli
az storage blob move -c my-file-system -d my-file-renamed.txt -s my-file.txt --account-name mystorageaccount
```

## <a name="delete-a-file"></a>删除文件

使用`az storage blob delete`命令删除文件。

此示例删除名为`my-file.txt`

```azurecli
az storage blob delete -c my-file-system -b my-file.txt --account-name mystorageaccount 
```

## <a name="manage-permissions"></a>管理权限

可以获取、设置和更新目录与文件的访问权限。

> [!NOTE]
> 若要使用 Azure Active Directory (Azure AD) 为命令授权，请确保已为安全主体分配了[存储 Blob 数据所有者角色](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)。 若要详细了解如何应用 ACL 权限以及更改它们所带来的影响，请参阅 [Azure Data Lake Storage Gen2 中的访问控制](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)。

### <a name="get-directory-and-file-permissions"></a>获取目录和文件权限

使用`az storage blob directory access show`命令获取**目录**的 ACL。

此示例获取某个目录的 ACL，然后将 ACL 输出到控制台。

```azurecli
az storage blob directory access show -d my-directory -c my-file-system --account-name mystorageaccount
```

使用`az storage blob access show`命令获取**文件的**访问权限。 

此示例获取某个文件的 ACL，然后将 ACL 输出到控制台。

```azurecli
az storage blob access show -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```

下图显示了获取目录 ACL 后的输出。

![获取 ACL 输出](./media/data-lake-storage-directory-file-acl-cli/get-acl.png)

在此示例中，所有者用户拥有读取、写入和执行权限。 所有者组仅拥有读取和执行权限。 有关这些访问控制列表的详细信息，请参阅 [Azure Data Lake Storage Gen2 中的访问控制](data-lake-storage-access-control.md)。

### <a name="set-directory-and-file-permissions"></a>设置目录和文件权限

使用`az storage blob directory access set`命令设置**目录**的 ACL。 

此示例针对所有者用户、所有者组或其他用户的目录设置 ACL，然后将 ACL 输出到控制台。

```azurecli
az storage blob directory access set -a "user::rw-,group::rw-,other::-wx" -d my-directory -c my-file-system --account-name mystorageaccount
```

本示例为拥有的用户、拥有组或其他用户在目录中设置*默认*ACL，然后将 ACL 打印到控制台。

```azurecli
az storage blob directory access set -a "default:user::rw-,group::rw-,other::-wx" -d my-directory -c my-file-system --account-name mystorageaccount
```

使用`az storage blob access set`命令设置**文件的**acl。 

此示例针对所有者用户、所有者组或其他用户的文件设置 ACL，然后将 ACL 输出到控制台。

```azurecli
az storage blob access set -a "user::rw-,group::rw-,other::-wx" -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```
下图显示了设置文件 ACL 后的输出。

![获取 ACL 输出](./media/data-lake-storage-directory-file-acl-cli/set-acl-file.png)

在此示例中，所有者用户和所有者组只拥有读取和写入权限。 所有其他用户拥有写入和执行权限。 有关这些访问控制列表的详细信息，请参阅 [Azure Data Lake Storage Gen2 中的访问控制](data-lake-storage-access-control.md)。

### <a name="update-directory-and-file-permissions"></a>更新目录和文件权限

设置此权限的另一种方法是使用 或`az storage blob directory access update``az storage blob access update`命令。 

通过将参数设置为 ACL 的短形式来`-permissions`更新目录或文件的 ACL。

此示例更新**目录**的 ACL。

```azurecli
az storage blob directory access update --permissions "rwxrwxrwx" -d my-directory -c my-file-system --account-name mystorageaccount
```

此示例更新**文件的**ACL。

```azurecli
az storage blob access update --permissions "rwxrwxrwx" -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```

还可以通过将`--owner`或`group`参数设置为用户的实体 ID 或用户主体名称 （UPN） 来更新目录或文件的拥有用户和组。 

此示例更改目录的所有者。 

```azurecli
az storage blob directory access update --owner xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -d my-directory -c my-file-system --account-name mystorageaccount
```

此示例更改文件的所有者。 

```azurecli
az storage blob access update --owner xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```
## <a name="manage-user-defined-metadata"></a>管理用户定义的元数据

通过将命令与一个或多个名称值对一起使用，`az storage blob directory metadata update`可以将用户定义的元数据添加到文件或目录。

本示例为名为`my-directory`目录的目录添加用户定义的元数据。

```azurecli
az storage blob directory metadata update --metadata tag1=value1 tag2=value2 -c my-file-system -d my-directory --account-name mystorageaccount
```

此示例显示名为 的`my-directory`目录的所有用户定义的元数据。

```azurecli
az storage blob directory metadata show -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="see-also"></a>请参阅

* [示例](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview)
* [第 1 代到第 2 代映射](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview#mapping-from-adls-gen1-to-adls-gen2)
* [提供反馈](https://github.com/Azure/azure-cli-extensions/issues)
* [已知问题](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [源代码](https://github.com/Azure/azure-cli-extensions/tree/master/src)

