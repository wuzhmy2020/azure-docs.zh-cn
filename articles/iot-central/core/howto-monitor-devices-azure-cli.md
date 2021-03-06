---
title: 使用 Azure IoT Central 资源管理器监视设备连接性
description: 监视设备消息，并通过 IoT Central 资源管理器 CLI 观察设备孪生更改。
author: viv-liu
ms.author: viviali
ms.date: 03/27/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: corywink
ms.openlocfilehash: 09209c21fe1b2b115c1ba6d6e00fcd0ee59a9393
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "80365414"
---
# <a name="monitor-device-connectivity-using-azure-cli"></a>使用 Azure CLI 监视设备连接

本主题适用于构建者和管理员**。

使用 Azure CLI IoT 扩展，了解如何将设备发送到 IoT Central，并观察设备孪生中的更改。 可以使用此工具来调试和观察设备连接性，并诊断设备消息不到达云或者设备不响应孪生更改的问题。

[如需更多详细信息，请访问 Azure CLI 扩展参考](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/central)

## <a name="prerequisites"></a>先决条件

+ Azure CLI 已安装且版本为 2.0.7 或更高。 通过运行 `az --version` 检查 Azure CLI 的版本。 通过 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/install-azure-cli)了解如何进行安装和更新
+ Azure 中的工作或学校帐户，在 IoT Central 应用程序中作为用户添加。

## <a name="install-the-iot-central-extension"></a>安装 IoT Central 扩展

从命令行运行以下命令以安装：

```azurecli
az extension add --name azure-iot
```

通过运行检查扩展的版本：

```azurecli
az --version
```

您应该会看到 azure iot 扩展为 0.8.1 或更高。 如果不是，则运行：

```azurecli
az extension update --name azure-iot
```

## <a name="using-the-extension"></a>使用扩展

以下部分介绍常用的命令和选项，它们可以在你运行 `az iot central` 时使用。 若要查看完整的命令和选项集，请将 `--help` 传递到 `az iot central` 或其任何子命令。

### <a name="login"></a>登录

从登录 Azure CLI 着手。 

```azurecli
az login
```

### <a name="get-the-application-id-of-your-iot-central-app"></a>获取 IoT Central 应用的应用程序 ID
在“管理/应用程序设置”中，复制**应用程序 ID******。 在后面的步骤中使用此值。

### <a name="monitor-messages"></a>监视消息
监视从设备发送到 IoT Central 应用的消息。 输出包括所有标头和注释。

```azurecli
az iot central app monitor-events --app-id <app-id> --properties all
```

### <a name="view-device-properties"></a>查看设备属性
查看某个给定设备当前的读取和读取/写入设备属性。

```azurecli
az iot central device-twin show --app-id <app-id> --device-id <device-id>
```

## <a name="next-steps"></a>后续步骤

现在，你已了解如何使用 IoT Central 资源管理器，建议下一步探索[如何管理设备 IoT Central](howto-manage-devices.md)。
