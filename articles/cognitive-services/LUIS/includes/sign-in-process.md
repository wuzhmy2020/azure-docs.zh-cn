---
title: include 文件
description: include 文件
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: include file
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 02/14/2020
ms.topic: include
ms.author: diberry
ms.openlocfilehash: 155c88ec4766391f70701b17038b915c399d8b0c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "77372297"
---
## <a name="sign-in-to-luis-portal"></a>登录到 LUIS 门户

LUIS 的新用户需要执行此过程：

1. 登录到 [LUIS 门户（预览版）](https://preview.luis.ai)，选择你的国家/地区，并同意使用条款。 如果看到“我的应用”  ，则 LUIS 资源已存在，你应该跳过此过程，开始创建应用。

1. 选择“创建 Azure资源”   ，然后选择“创建要将应用迁移到的创作资源”。

    ![选择语言理解创作资源的类型](../media/luis-how-to-azure-subscription/sign-in-create-resource.png)

1. 填写资源的详细信息。

    ![创建创作资源](../media/migrate-authoring-key/choose-authoring-resource-form.png)

    **创建新的创作资源**时，请提供以下信息：

    * **资源名称** - 你选择的自定义名称，用作创作和预测终结点查询的 URL 的一部分。
    * **租户** - 与 Azure 订阅关联的租户。
    * **订阅名称** - 将对资源计费的订阅。
    * **资源组** - 你选择或创建的自定义资源组名称。 使用资源组可将 Azure 资源分组，以便进行访问和管理。
    * **位置** - 位置选项基于**资源组**选择。
    * **定价层** - 定价层确定每秒和每月的最大事务数。

1. 此时将显示要创建的资源的摘要。 选择“**下一页**”。

    ![创建创作资源](../media/sign-in/sign-in-confirm-key-selection.png)

1. 选择“继续”进行确认  。

    ![创建创作资源](../media/sign-in/sign-in-confirm-continue.png)