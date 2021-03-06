---
title: 排查工作区问题
titleSuffix: ML Studio (classic) - Azure
description: 本指南介绍了设置 Azure 机器学习工作室（经典版）工作区时一些常见难题的解决方案。
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/20/2017
ms.openlocfilehash: 58ccd63e16382aca8e16eb67efba951a055eb254
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79217841"
---
# <a name="troubleshooting-guide-create-and-connect-to-an-azure-machine-learning-studio-classic-workspace"></a>故障排除指南：创建并连接到 Azure 机器学习工作室（经典）工作区

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

本指南介绍了设置 Azure 机器学习工作室（经典版）工作区时一些常见难题的解决方案。

## <a name="workspace-owner"></a>工作区所有者
若要在机器学习工作室（经典版）中打开工作区，则必须登录到用于创建工作区的 Microsoft 帐户，或者需要收到所有者发出的邀请以加入工作区。 在 Azure 门户中可以管理工作区，包括配置访问权限。

有关管理工作区的详细信息，请参阅[管理 Azure 机器学习工作室（经典版）工作区]。

[管理 Azure 机器学习工作室（经典版）工作区]: manage-workspace.md

## <a name="allowed-regions"></a>允许的区域
机器学习当前只允许有限数量的区域。 如果您的订阅不包括这些区域之一，您可能会看到错误消息"在允许的区域中没有订阅"。

要请求将某区域添加到订阅，可从 Azure 门户创建新的 Microsoft 支持请求，将“计费”**** 选为问题类型，然后按照提示提交请求。

## <a name="storage-account"></a>存储帐户
机器学习服务需要存储帐户以存储数据。 可以使用现有存储帐户，或者可以在创建新的机器学习工作室（经典版）工作区时创建新的存储帐户（如果有创建新的存储帐户的配额）。

创建新的机器学习工作室（经典版）工作区之后，可以使用用于创建工作区的 Microsoft 帐户登录到机器学习工作室（经典版）。 如果您遇到错误消息"找不到工作区"（类似于以下屏幕截图），请使用以下步骤删除您的浏览器 Cookie。

![未找到工作区](media/troubleshooting-creating-ml-workspace/screen3.png)

**删除浏览器 cookie**

1. 如果使用 Internet Explorer，请单击右上角的“工具”**** 按钮，并选择“Internet 选项”****。  

   ![Internet 选项](media/troubleshooting-creating-ml-workspace/screen4.png)

2. 在“常规”**** 选项卡下，单击“删除...”****

   ![“常规”选项卡](media/troubleshooting-creating-ml-workspace/screen5.png)

3. 在“删除浏览历史记录”**** 对话框中，确保已选中“Cookie 和网站数据”****，并单击“删除”****。

   ![删除 Cookie](media/troubleshooting-creating-ml-workspace/screen6.png)

删除 Cookie 之后，重启浏览器，并转到 [Microsoft Azure 机器学习工作室（经典版）](https://studio.azureml.net)页。 如果系统提示输入用户名和密码，请输入用于创建工作区的同一个 Microsoft 帐户。

## <a name="comments"></a>注释

目标是使机器学习体验尽可能流畅。 请在 [Azure 机器学习论坛](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning)上任意发表评论和问题，帮助我们改进服务。
