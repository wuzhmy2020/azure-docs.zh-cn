---
title: 部署 Azure 文件同步 | Microsoft Docs
description: 了解如何从头到尾部署 Azure 文件同步。
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 07/19/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: f2c4e762ebf10a5ca2120c13a52750a7781d60b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268062"
---
# <a name="deploy-azure-file-sync"></a>部署 Azure 文件同步
使用 Azure 文件同步，即可将组织的文件共享集中在 Azure 文件中，同时又不失本地文件服务器的灵活性、性能和兼容性。 Azure 文件同步可将 Windows Server 转换为 Azure 文件共享的快速缓存。 可以使用 Windows Server 上可用的任意协议本地访问数据，包括 SMB、NFS 和 FTPS。 并且可以根据需要在世界各地具有多个缓存。

强烈建议先阅读[规划 Azure 文件部署](storage-files-planning.md)和[规划 Azure 文件同步部署](storage-sync-files-planning.md)，再按照本文中的步骤进行操作。

## <a name="prerequisites"></a>先决条件
* 要部署 Azure 文件同步的同一区域中的 Azure 文件共享。有关详细信息，请参阅：
    - Azure 文件同步的[适用地区](storage-sync-files-planning.md#azure-file-sync-region-availability)。
    - [创建文件共享](storage-how-to-create-file-share.md)，了解创建文件共享的分步说明。
* 至少有一个受支持的 Windows 服务器或 Windows 服务器群集实例，以便与 Azure 文件同步同步。有关受支持版本的 Windows 服务器的详细信息，请参阅[与 Windows 服务器的互操作性](storage-sync-files-planning.md#windows-file-server-considerations)。
* Az PowerShell 模块可与 PowerShell 5.1 或 PowerShell 6+ 一起使用。 您可以在任何受支持的系统上（包括非 Windows 系统）上使用 Az PowerShell 模块进行 Azure 文件同步，但服务器注册 cmdlet 必须始终在正在注册的 Windows Server 实例上运行（这可以直接或通过 PowerShell 完成）远程）。 在 Windows 服务器 2012 R2 上，您可以验证是否至少正在运行 PowerShell 5.1。\*通过查看 **$PSVersionTable**对象的**PSVersion**属性的值：

    ```powershell
    $PSVersionTable.PSVersion
    ```

    与 Windows Server 2012 R2 的全新安装一样，如果 PSVersion 值低于 5.1.\*，可通过下载并安装 [Windows Management Framework (WMF) 5.1](https://www.microsoft.com/download/details.aspx?id=54616) 轻松升级。 要下载和安装 Windows Server 2012 R2 的相应软件包是**Win8.1AndW2K12R2-KB\*\*\*\*\*\*\*-x64.msu**。 

    PowerShell 6+ 可与任何受支持的系统一起使用，并且可以通过其[GitHub 页面](https://github.com/PowerShell/PowerShell#get-powershell)下载。 

    > [!Important]  
    > 如果您计划使用服务器注册 UI，而不是直接从 PowerShell 注册，则必须使用 PowerShell 5.1。

* 如果您已选择使用 PowerShell 5.1，请确保至少安装了 .NET 4.7.2。 详细了解[.NET 框架版本和系统上的依赖项](https://docs.microsoft.com/dotnet/framework/migration-guide/versions-and-dependencies)。

    > [!Important]  
    > 如果要在 Windows 服务器核心上安装 .NET 4.7.2+，则必须使用`quiet`和`norestart`标志进行安装，否则安装将失败。 例如，如果安装 .NET 4.8，该命令将如下所示：
    > ```PowerShell
    > Start-Process -FilePath "ndp48-x86-x64-allos-enu.exe" -ArgumentList "/q /norestart" -Wait
    > ```

* Az PowerShell 模块，可通过此处的说明进行安装：[安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-Az-ps)。
     
    > [!Note]  
    > 安装 Az PowerShell 模块时，现在会自动安装 Az.StorageSync 模块。

## <a name="prepare-windows-server-to-use-with-azure-file-sync"></a>准备 Windows Server，用于 Azure 文件同步
对于要与 Azure 文件同步配合使用的每个服务器（包括故障转移群集中的服务器节点），请禁用“Internet Explorer 增强的安全性配置”。**** 只需在最初注册服务器时禁用。 可在注册服务器后重新启用。

# <a name="portal"></a>[门户](#tab/azure-portal)
> [!Note]  
> 如果要在 Windows 服务器核心上部署 Azure 文件同步，则可以跳过此步骤。

1. 打开服务器管理器。
2. 单击“本地服务器”****：  
    ![服务器管理器 UI 左侧的“本地服务器”](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
3. 在“属性”子窗格上，选择“IE 增强的安全性配置”的链接********。  
    ![服务器管理器 UI 中的“IE 增强的安全性配置”窗格](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
4. 在**Internet 资源管理器增强安全配置**对话框中，为**管理员**和**用户**选择 **"关闭**"：  
    ![选定“关”的“Internet Explorer 增强的安全性配置”弹出窗口](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

# <a name="powershell"></a>[电源外壳](#tab/azure-powershell)
若要禁用“Internet Explorer 增强的安全性配置”，请在权限提升的 PowerShell 会话中执行以下命令：

```powershell
$installType = (Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\").InstallationType

# This step is not required for Server Core
if ($installType -ne "Server Core") {
    # Disable Internet Explorer Enhanced Security Configuration 
    # for Administrators
    Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0 -Force
    
    # Disable Internet Explorer Enhanced Security Configuration 
    # for Users
    Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A8-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0 -Force
    
    # Force Internet Explorer closed, if open. This is required to fully apply the setting.
    # Save any work you have open in the IE browser. This will not affect other browsers,
    # including Microsoft Edge.
    Stop-Process -Name iexplore -ErrorAction SilentlyContinue
}
``` 

---

## <a name="deploy-the-storage-sync-service"></a>部署存储同步服务 
Azure 文件同步的部署过程首先会将一个“存储同步服务”资源放入所选订阅的资源组中。**** 我们建议尽量少预配这些资源。 将在服务器与此资源之间创建信任关系，一个服务器只能注册到一个存储同步服务。 因此，我们建议根据需要部署尽量多的存储同步服务，以隔离服务器组。 请记住，不同存储同步服务中的服务器不能彼此同步。

> [!Note]
> 存储同步服务从已部署到的订阅和资源组中继承访问权限。 我们建议仔细检查谁有权访问该服务。 具有写访问权限的实体可以开始从已注册到此存储同步服务的服务器同步新的文件集，使数据流向这些实体可以访问的 Azure 存储。

# <a name="portal"></a>[门户](#tab/azure-portal)
要部署存储同步服务，请转到[Azure 门户](https://portal.azure.com/)，单击"*创建资源*"，然后搜索 Azure 文件同步。在搜索结果中，选择**Azure 文件同步**，然后选择 **"创建**"以打开 **"部署存储同步**"选项卡。

在打开的窗格中，输入以下信息：

- 名称：存储同步服务的唯一名称（按订阅）****。
- 订阅：需要在其中创建存储同步服务的订阅****。 根据组织的配置策略，可能有权访问一个或多个订阅。 Azure 订阅是对每项云服务（如 Azure 文件）计费的最基本容器。
- **资源组**：资源组是 Azure 资源的逻辑组，如存储帐户或存储同步服务。 您可以创建新的资源组或使用现有资源组进行 Azure 文件同步。（我们建议将资源组用作容器，以逻辑方式隔离组织的资源，例如对特定项目的人力资源或资源进行分组。
- **位置**：要在其中部署 Azure 文件同步的区域。此列表中只有支持的区域可用。

完成后，选择“创建”部署存储同步服务****。

# <a name="powershell"></a>[电源外壳](#tab/azure-powershell)
使用`<Az_Region>`自己的`<RG_Name>`值替换`<my_storage_sync_service>`和 ，然后使用以下命令创建和部署存储同步服务：

```powershell
$hostType = (Get-Host).Name

if ($installType -eq "Server Core" -or $hostType -eq "ServerRemoteHost") {
    Connect-AzAccount -UseDeviceAuthentication
}
else {
    Connect-AzAccount
}

# this variable holds the Azure region you want to deploy 
# Azure File Sync into
$region = '<Az_Region>'

# Check to ensure Azure File Sync is available in the selected Azure
# region.
$regions = @()
Get-AzLocation | ForEach-Object { 
    if ($_.Providers -contains "Microsoft.StorageSync") { 
        $regions += $_.Location 
    } 
}

if ($regions -notcontains $region) {
    throw [System.Exception]::new("Azure File Sync is either not available in the selected Azure Region or the region is mistyped.")
}

# the resource group to deploy the Storage Sync Service into
$resourceGroup = '<RG_Name>'

# Check to ensure resource group exists and create it if doesn't
$resourceGroups = @()
Get-AzResourceGroup | ForEach-Object { 
    $resourceGroups += $_.ResourceGroupName 
}

if ($resourceGroups -notcontains $resourceGroup) {
    New-AzResourceGroup -Name $resourceGroup -Location $region
}

$storageSyncName = "<my_storage_sync_service>"
$storageSync = New-AzStorageSyncService -ResourceGroupName $resourceGroup -Name $storageSyncName -Location $region
```

---

## <a name="install-the-azure-file-sync-agent"></a>安装 Azure 文件同步代理
Azure 文件同步代理是一个可下载包，可实现 Windows 服务器与 Azure 文件共享的同步。 

# <a name="portal"></a>[门户](#tab/azure-portal)
可从 [Microsoft 下载中心](https://go.microsoft.com/fwlink/?linkid=858257)下载代理。 下载完成后，双击 MSI 包，开始安装 Azure 文件同步代理。

> [!Important]  
> 如果要对故障转移群集使用 Azure 文件同步，则 必须在群集中的每个节点上安装 Azure 文件同步代理。 必须注册群集中的每个节点才能使用 Azure 文件同步。

建议执行以下操作：
- 保留默认安装路径(C:\Program Files\Azure\StorageSyncAgent)，以简化故障排除和服务器维护。
- 启用 Microsoft 更新，使 Azure 文件同步保持最新。 Azure 文件同步代理的所有更新（包括功能更新和修补程序）都可从 Microsoft 更新进行。 我们建议将最新的更新安装到 Azure 文件同步。有关详细信息，请参阅[Azure 文件同步更新策略](storage-sync-files-planning.md#azure-file-sync-agent-update-policy)。

Azure 文件同步代理安装完成后，服务器注册 UI 自动打开。 在注册之前，必须创建存储同步服务；请参阅下一部分了解如何创建存储同步服务。

# <a name="powershell"></a>[电源外壳](#tab/azure-powershell)
执行以下 PowerShell 代码，以下载适用于所用 OS 的 Azure 文件同步代理版本，并将其安装在系统上。

> [!Important]  
> 如果要对故障转移群集使用 Azure 文件同步，则 必须在群集中的每个节点上安装 Azure 文件同步代理。 必须注册群集中的每个节点才能使用 Azure 文件同步。

```powershell
# Gather the OS version
$osver = [System.Environment]::OSVersion.Version

# Download the appropriate version of the Azure File Sync agent for your OS.
if ($osver.Equals([System.Version]::new(10, 0, 17763, 0))) {
    Invoke-WebRequest `
        -Uri https://aka.ms/afs/agent/Server2019 `
        -OutFile "StorageSyncAgent.msi" 
} elseif ($osver.Equals([System.Version]::new(10, 0, 14393, 0))) {
    Invoke-WebRequest `
        -Uri https://aka.ms/afs/agent/Server2016 `
        -OutFile "StorageSyncAgent.msi" 
} elseif ($osver.Equals([System.Version]::new(6, 3, 9600, 0))) {
    Invoke-WebRequest `
        -Uri https://aka.ms/afs/agent/Server2012R2 `
        -OutFile "StorageSyncAgent.msi" 
} else {
    throw [System.PlatformNotSupportedException]::new("Azure File Sync is only supported on Windows Server 2012 R2, Windows Server 2016, and Windows Server 2019")
}

# Install the MSI. Start-Process is used to PowerShell blocks until the operation is complete.
# Note that the installer currently forces all PowerShell sessions closed - this is a known issue.
Start-Process -FilePath "StorageSyncAgent.msi" -ArgumentList "/quiet" -Wait

# Note that this cmdlet will need to be run in a new session based on the above comment.
# You may remove the temp folder containing the MSI and the EXE installer
Remove-Item -Path ".\StorageSyncAgent.msi" -Recurse -Force
```

---

## <a name="register-windows-server-with-storage-sync-service"></a>向存储同步服务注册 Windows Server
向存储同步服务注册 Windows Server 可在服务器（或群集）与存储同步服务之间建立信任关系。 一个服务器只能注册到一个存储同步服务，并可与同一存储同步服务关联的其他服务器和 Azure 文件共享同步。

> [!Note]
> 服务器注册使用你的 Azure 凭据在存储同步服务与 Windows Server 之间创建信任关系，但是，服务器随后会创建并使用自身有效的标识，前提是该服务器保持已注册状态，并且当前的共享访问签名令牌（存储 SAS）有效。 取消注册服务器后，无法将新的 SAS 令牌颁发给服务器，因此，服务器无法访问 Azure 文件共享，并停止任何同步。

# <a name="portal"></a>[门户](#tab/azure-portal)
服务器注册 UI 应在 Azure 文件同步代理安装后自动打开。 如果没有打开，可以手动从其文件位置 C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe 打开。 服务器注册 UI 打开时，请选择“登录”开始操作****。

登录后，系统会提示输入以下信息：

![服务器注册 UI 的屏幕快照](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- Azure 订阅：包含存储同步服务的订阅（请参阅[部署存储同步服务](#deploy-the-storage-sync-service)）****。 
- 资源组：包含存储同步服务的资源组****。
- 存储同步服务：想要向其注册的存储同步服务的名称****。

选择相应的信息之后，选择“注册”完成服务器注册****。 在注册过程中，系统会提示进行其他登录。

# <a name="powershell"></a>[电源外壳](#tab/azure-powershell)
```powershell
$registeredServer = Register-AzStorageSyncServer -ParentObject $storageSync
```

---

## <a name="create-a-sync-group-and-a-cloud-endpoint"></a>创建同步组和云终结点
同步组定义一组文件的同步拓扑。 同步组中的终结点保持彼此同步。 同步组中必须包含一个表示 Azure 文件共享的云终结点，以及一个或多个服务器终结点。 服务器终结点表示已注册服务器上的路径。 服务器可以包含多个同步组中的服务器终结点。 可以创建任意数量的同步组，以适当地描述所需的同步拓扑。

云终结点是指向 Azure 文件共享的指针。 所有服务器终结点将与某个云终结点同步，使该云终结点成为中心。 Azure 文件共享的存储帐户必须位于存储同步服务所在的同一个区域。 将同步整个 Azure 文件共享，但存在一种例外情况：将预配一个特殊的文件夹，它相当于 NTFS 卷上的“System Volume Information”隐藏文件夹。 此目录名为“.SystemShareInformation”。 其中包含不会同步到其他终结点的重要同步元数据。 请不要使用或删除它！

> [!Important]  
> 可对同步组中的任何云终结点或服务器终结点进行更改，并将文件同步到同步组中的其他终结点。 如果直接对云终结点（Azure 文件分享）进行更改，首先需要通过 Azure 文件同步更改检测作业来发现更改。 每 24 小时仅针对云终结点启动一次更改检测作业。 有关详细信息，请参阅 [Azure 文件常见问题解答](storage-files-faq.md#afs-change-detection)。

# <a name="portal"></a>[门户](#tab/azure-portal)
要创建同步组，请在[Azure 门户](https://portal.azure.com/)中转到存储同步服务，然后选择 "**同步组**"

![在 Azure 门户中创建新的同步组](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

在打开的窗格中输入以下信息，创建具有云终结点的同步组：

- **同步组名称**：要创建的同步组的名称。 此名称在存储同步服务内必须是唯一的，但可以是符合逻辑的任何名称。
- 订阅：在[部署存储同步服务](#deploy-the-storage-sync-service)中用于部署存储同步服务的订阅****。
- 存储帐户：如果选择“选择存储账户”，另一个窗格随即出现，可在其中选择包含要同步的 Azure 文件共享的存储帐户********。
- **Azure 文件共享**：要与其同步的 Azure 文件共享的名称。

# <a name="powershell"></a>[电源外壳](#tab/azure-powershell)
若要创建同步组，请执行以下 PowerShell。 请记得将 `<my-sync-group>` 替换为同步组的所需名称。

```powershell
$syncGroupName = "<my-sync-group>"
$syncGroup = New-AzStorageSyncGroup -ParentObject $storageSync -Name $syncGroupName
```

成功创建同步组后，可以创建云终结点。 请务必将 `<my-storage-account>` 和 `<my-file-share>` 替换为预期的值。

```powershell
# Get or create a storage account with desired name
$storageAccountName = "<my-storage-account>"
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroup | Where-Object {
    $_.StorageAccountName -eq $storageAccountName
}

if ($storageAccount -eq $null) {
    $storageAccount = New-AzStorageAccount `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup `
        -Location $region `
        -SkuName Standard_LRS `
        -Kind StorageV2 `
        -EnableHttpsTrafficOnly:$true
}

# Get or create an Azure file share within the desired storage account
$fileShareName = "<my-file-share>"
$fileShare = Get-AzStorageShare -Context $storageAccount.Context | Where-Object {
    $_.Name -eq $fileShareName -and $_.IsSnapshot -eq $false
}

if ($fileShare -eq $null) {
    $fileShare = New-AzStorageShare -Context $storageAccount.Context -Name $fileShareName
}

# Create the cloud endpoint
New-AzStorageSyncCloudEndpoint `
    -Name $fileShare.Name `
    -ParentObject $syncGroup `
    -StorageAccountResourceId $storageAccount.Id `
    -AzureFileShareName $fileShare.Name
```

---

## <a name="create-a-server-endpoint"></a>创建服务器终结点
服务器终结点代表已注册服务器上的特定位置，例如服务器卷中的文件夹。 服务器终结点必须是已注册的服务器（而不是装载的共享）上的路径；若要使用云分层，该路径必须在非系统卷上。 不支持网络附加存储 (NAS)。

# <a name="portal"></a>[门户](#tab/azure-portal)
要添加服务器终结点，请转到新创建的同步组，然后选择 **"添加服务器终结点**"。

![在“同步组”窗格中添加一个新的服务器终结点](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

在 **"添加服务器终结点**"窗格中，输入以下信息以创建服务器终结点：

- **已注册服务器**：要创建服务器终结点的服务器或群集的名称。
- **路径**：要作为同步组的一部分同步的 Windows 服务器路径。
- 云分层：启用或禁用云分层的开关****。 通过云分层可以将不常使用或访问的文件分层到 Azure 文件。
- **卷可用空间**：服务器终结点所在的卷上要保留的可用空间量。 例如，如果有一个服务器终结点的卷上的卷可用空间设置为 50%，则约有一半数据会分层为 Azure 文件。 不管是否启用云分层，Azure 文件共享在同步组中始终具有完整的数据副本。

要添加服务器终结点，请选择"**创建**"。 现在，文件在 Azure 文件共享和 Windows Server 之间保持保存。 

# <a name="powershell"></a>[电源外壳](#tab/azure-powershell)
执行以下 PowerShell 命令创建服务器终结点（请务必将 `<your-server-endpoint-path>` 和 `<your-volume-free-space>` 替换为所需值）。

```powershell
$serverEndpointPath = "<your-server-endpoint-path>"
$cloudTieringDesired = $true
$volumeFreeSpacePercentage = <your-volume-free-space>

if ($cloudTieringDesired) {
    # Ensure endpoint path is not the system volume
    $directoryRoot = [System.IO.Directory]::GetDirectoryRoot($serverEndpointPath)
    $osVolume = "$($env:SystemDrive)\"
    if ($directoryRoot -eq $osVolume) {
        throw [System.Exception]::new("Cloud tiering cannot be enabled on the system volume")
    }

    # Create server endpoint
    New-AzStorageSyncServerEndpoint `
        -Name $registeredServer.FriendlyName `
        -SyncGroup $syncGroup `
        -ServerResourceId $registeredServer.ResourceId `
        -ServerLocalPath $serverEndpointPath `
        -CloudTiering `
        -VolumeFreeSpacePercent $volumeFreeSpacePercentage
} else {
    # Create server endpoint
    New-AzStorageSyncServerEndpoint `
        -Name $registeredServer.FriendlyName `
        -SyncGroup $syncGroup `
        -ServerResourceId $registeredServer.ResourceId `
        -ServerLocalPath $serverEndpointPath 
}
```

---

## <a name="configure-firewall-and-virtual-network-settings"></a>配置防火墙和虚拟网络设置

### <a name="portal"></a>门户
如果要将 Azure 文件同步配置为使用防火墙和虚拟网络设置，请执行以下操作：

1. 从 Azure 门户导航到要保护的存储帐户。
1. 选择左侧**菜单上的防火墙和虚拟网络**按钮。
1. 选择"**允许从 进行访问****"下的选定网络**。
1. 确保您的服务器 IP 或虚拟网络列在相应的部分下。
1. 确保**选中允许受信任的 Microsoft 服务访问此存储帐户**。
1. 选择 **"保存"** 以保存设置。

![配置防火墙和虚拟网络设置以配合 Azure 文件同步](media/storage-sync-files-deployment-guide/firewall-and-vnet.png)

## <a name="onboarding-with-azure-file-sync"></a>使用 Azure 文件同步进行载入
若要通过 Azure 文件同步在不停机的情况下首次进行载入，同时保持完整的文件保真度和访问控制列表 (ACL)，则建议采用的步骤如下所述：
 
1. 部署存储同步服务。
2. 创建一个同步组。
3. 在包含完整数据集的服务器上安装 Azure 文件同步代理。
4. 注册该服务器并在共享中创建一个服务器终结点。 
5. 让同步服务执行到 Azure 文件共享（云终结点）的完整上传。  
6. 在初始上传完成后，在剩余的每台服务器上安装 Azure 文件同步代理。
7. 在剩余的每台服务器上创建新的文件共享。
8. 使用云分层策略在新的文件共享中创建服务器终结点（如果需要）。 （此步骤要求有额外的存储可供初始设置使用。）
9. 让 Azure 文件同步代理快速还原完整命名空间，而无需实际数据传输。 在完成完整的命名空间同步后，同步引擎将根据服务器终结点的云分层策略填充本地磁盘空间。 
10. 确保同步完成，并根据需要测试拓扑。 
11. 将用户和应用程序重定向到此新共享。
12. 还可以选择删除服务器上任何重复的共享。
 
如果没有可用于初始载入的额外存储空间，并且希望附加到现有的共享，则可以在 Azure 文件共享中预先播种数据。 当且仅当可以接受停机并且绝对可以保证在初始载入过程中服务器共享上不会发生数据更改时，才建议使用此方法。 
 
1. 确保任何服务器上的数据在载入过程中无法更改。
2. 使用 SMB 上的任何数据传输工具预占 Azure 文件与服务器数据共享，例如 Robocopy、直接 SMB 复制。 由于 AzCopy 不通过 SMB 上传数据，因此不能使用它进行预先播种。
3. 使用所需的指向现有共享的服务器终结点创建 Azure 文件同步拓扑。
4. 让同步服务在所有终结点上完成对帐过程。 
5. 在对帐完成后，你可以打开共享进行更改。
 
目前，种子预设定方法有一些局限性： 
- 不能保持文件的完全保真度。 例如，文件会丢失 ACL 和时间戳。
- 在同步拓扑完全启动并运行之前更改服务器上的数据可能会导致各个服务器终结点上发生冲突。  
- 创建云终结点后，Azure 文件同步运行一个过程，用于在开始初始同步之前检测云中的文件。完成此过程所需要的时间因网络速度、可用带宽以及文件和文件夹数量等各种因素而异。 对于预览版，粗略估计，检测流程以大约每秒 10 个文件的速度运行。因此，当在云中预先播种数据时，即使预先播种运行速度很快，获得完全运行的系统所需的总体时间也会更长。

## <a name="self-service-restore-through-previous-versions-and-vss-volume-shadow-copy-service"></a>通过早期版本和 VSS 进行自助服务恢复（卷卷卷影影复制服务）

> [!IMPORTANT]
> 以下信息只能与存储同步代理的版本 9（或以上）一起使用。 低于 9 的版本将没有存储同步自助服务 cmdlet。

早期版本是一种 Windows 功能，允许您利用卷的服务器端 VSS 快照向 SMB 客户端显示文件的可恢复版本。
这支持一个强大的方案，通常称为自助服务还原，直接针对信息工作者，而不是依赖于从 IT 管理员的还原。

VSS 快照和早期版本独立于 Azure 文件同步工作。但是，必须将云分层设置为兼容模式。 许多 Azure 文件同步服务器终结点可以存在于同一卷上。 您必须对每个卷进行以下 PowerShell 调用，该卷甚至有一个服务器终结点，您计划或正在其中使用云分层。

```powershell
Import-Module ‘<SyncAgentInstallPath>\StorageSync.Management.ServerCmdlets.dll’
Enable-StorageSyncSelfServiceRestore [-DriveLetter] <string> [[-Force]] 
```

VSS 快照是整个卷的快照。 默认情况下，给定卷最多可存在 64 个快照，并授予足够的空间来存储快照。 VSS 会自动处理此问题。 默认快照计划每天（星期一到星期五）拍摄两个快照。 该计划可通过 Windows 计划任务进行配置。 上面的 PowerShell cmdlet 执行两件事：
1. 它将 Azure 文件同步云分层的指定卷与以前的版本兼容，并保证可以从以前的版本还原文件，即使它分层到服务器上的云。 
2. 它启用默认 VSS 计划。 然后，您可以决定稍后对其进行修改。 

> [!Note]  
> 此处需要注意两个要点：
>- 如果使用 -Force 参数，并且当前启用了 VSS，则它将覆盖当前的 VSS 快照计划并将其替换为默认计划。 请确保在运行 cmdlet 之前保存自定义配置。
> - 如果在群集节点上使用此 cmdlet，还必须在群集中的所有其他节点上运行它！ 

为了查看是否启用了自助服务还原兼容性，可以运行以下 cmdlet。

```powershell
    Get-StorageSyncSelfServiceRestore [[-Driveletter] <string>]
```

它将列出服务器上的所有卷以及每个卷的云分层兼容天数。 此数字根据每个卷的最大可能的快照和默认快照计划自动计算。 因此，默认情况下，向信息工作者显示的所有早期版本都可用于从中还原。 如果更改默认计划以拍摄更多快照，则情况也是如此。
但是，如果更改计划的方式将导致卷上的可用快照早于兼容天数值，则用户将无法使用此较旧的快照（以前的版本）从中还原。

> [!Note]
> 启用自助服务还原可能会影响 Azure 存储消耗和账单。 此影响仅限于当前在服务器上分层的文件。 启用此功能可确保云中有一个可以通过早期版本 （VSS 快照） 条目引用的文件版本。
>
> 如果禁用该功能，Azure 存储消耗量将缓慢下降，直到兼容的天数窗口过去。 没有办法加快速度。 

每个卷（64）的默认最大 VSS 快照数以及获取快照的默认计划，会导致信息工作者最多可以还原 45 天的以前版本，具体取决于可在卷上存储多少 VSS 快照。

如果最大值每个卷的 64 个 VSS 快照不是正确的设置，您可以通过[注册表项更改该值](https://docs.microsoft.com/windows/win32/backup/registry-keys-for-backup-and-restore#maxshadowcopies)。
要使新限制生效，您需要重新运行 cmdlet，以便在以前启用的每个卷上启用以前的版本兼容性，并且使用 -Force 标志来考虑每个卷的新最大 VSS 快照数。 这将导致新计算的兼容天数。 请注意，此更改将仅对新分层文件生效，并覆盖您可能所做的 VSS 计划上的任何自定义项。

## <a name="migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync"></a>将 DFS 复制 (DFS-R) 部署迁移至 Azure 文件同步
若要将 DFS-R 部署迁移至 Azure 文件同步，请执行以下操作：

1. 创建一个同步组以表示要替换的 DFS-R 拓扑。
2. 从 DFS-R 拓扑具有完整数据集的服务器开始迁移。 在该服务器上安装 Azure 文件同步。
3. 注册该服务器，并为要迁移的第一个服务器创建服务器终结点。 请勿启用云分层。
4. 让所有数据同步至你的 Azure 文件共享（云终结点）。
5. 在剩余的每个 DFS-R 服务器上安装并注册 Azure 文件同步代理。
6. 禁用 DFS-R。 
7. 在每个 DFS-R 服务器上创建服务器终结点。 请勿启用云分层。
8. 确保同步完成，并根据需要测试拓扑。
9. 注销 DFS-R。
10. 现在应该可以根据需要在任何一个服务器终结点上启用云分层。

有关详细信息，请参阅 [Azure 文件同步与分布式文件系统 (DFS) 的互操作](storage-sync-files-planning.md#distributed-file-system-dfs)。

## <a name="next-steps"></a>后续步骤
- [添加或删除 Azure 文件同步服务器终结点](storage-sync-files-server-endpoint.md)
- [向 Azure 文件同步注册或注销服务器](storage-sync-files-server-registration.md)
- [监视 Azure 文件同步](storage-sync-files-monitoring.md)
