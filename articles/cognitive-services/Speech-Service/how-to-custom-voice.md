---
title: 使用自定义语音 - 语音服务改进合成
titleSuffix: Azure Cognitive Services
description: 自定义语音是一组在线工具，使用它可为自有品牌创建可识别的独一无二的语音。 只需准备好几个音频文件和关联的听录内容即可完全入门。 请遵循以下链接开始创建自定义语音转文本体验。
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: dapine
ms.openlocfilehash: 786cf687e569af172eabfaa9068ae61ffd6b2988
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220475"
---
# <a name="get-started-with-custom-voice"></a>自定义语音入门

[自定义语音](https://aka.ms/customvoice)是一组在线工具，使用它可为自有品牌创建可识别的独一无二的语音。 只需准备好几个音频文件和关联的听录内容即可完全入门。 请遵循以下链接开始创建自定义文本转语音体验。

## <a name="whats-in-custom-voice"></a>什么是自定义语音？

在开始自定义语音之前，您需要 Azure 帐户和语音服务订阅。 创建帐户后，可以准备数据、训练和测试模型、评估语音质量，并最终部署自定义语音模型。

下图突出显示了使用[自定义语音门户](https://aka.ms/customvoice)创建自定义语音模型的步骤。 请使用链接了解详细信息。

![自定义语音体系结构图](media/custom-voice/custom-voice-diagram.png)

1. [订阅和创建项目](#set-up-your-azure-account)- 创建 Azure 帐户并创建语音服务订阅。 使用此统一订阅可以访问语音转文本、文本转语音、语音翻译和自定义语音门户。 然后，使用语音服务订阅，创建第一个自定义语音项目。

2. [上传数据](how-to-custom-voice-create-voice.md#upload-your-datasets) - 使用自定义语音门户或自定义语音 API 上传数据（音频和文本）。 在门户中，可以调查和评估发音评分以及信噪比。 有关详细信息，请参阅[如何为自定义语音准备数据](how-to-custom-voice-prepare-data.md)。

3. [训练模型](how-to-custom-voice-create-voice.md#build-your-custom-voice-model) – 使用数据创建自定义的“文本转语音”语音模型。 可在不同的语言中训练模型。 训练并测试模型后，如果你对结果感到满意，则可以部署该模型。

4. [部署模型](how-to-custom-voice-create-voice.md#create-and-use-a-custom-voice-endpoint) - 为文本语音模型创建自定义终结点，并使用该终结点在产品、工具和应用程序中进行语音合成。

## <a name="custom-neural-voices"></a>自定义神经语音

神经语音自定义功能当前处于公共预览版中，仅限于选定的客户。 填写此[申请表](https://go.microsoft.com/fwlink/?linkid=2108737)以开始。

> [!NOTE]
> 作为 Microsoft 设计负责任的 AI 承诺的一部分，我们的目的是保护个人和社会的权利，并促进透明的人机交互。 因此，自定义神经语音通常不能对所有客户可用。 只有在您的应用程序经过审查并承诺根据我们的道德原则使用该技术后，您才能访问该技术。 详细了解我们的[应用程序浇注过程](https://aka.ms/custom-neural-gating-overview)。

## <a name="set-up-your-azure-account"></a>设置 Azure 帐户

在使用自定义语音门户创建自定义模型之前，需要语音服务订阅。 按照这些说明在 Azure 中创建语音服务订阅。 如果你没有 Azure 帐户，可以注册一个新帐户。  

创建 Azure 帐户和语音服务订阅后，需要登录到自定义语音门户并连接订阅。

1. 从 Azure 门户获取语音服务订阅密钥。
2. 登录到[自定义语音门户](https://aka.ms/custom-voice)。
3. 选择你的订阅并创建语音项目。
4. 若要切换到另一个语音订阅，请使用顶部导航栏中的齿轮图标。

> [!NOTE]
> 自定义语音服务不支持 30 天免费试用密钥。 必须先在 Azure 中创建 F0 或 S0 密钥才能使用该服务。

## <a name="how-to-create-a-project"></a>如何创建项目

数据、模型、测试和终结点等内容在自定义语音门户中组织成**项目**。 每个项目特定于某个国家/语言，以及要创建的语音的性别。 例如，您可以为呼叫中心在美国使用英语的聊天机器人（en-US）创建女性语音项目。

若要创建第一个项目，请选择“文本转语音/自定义语音”选项卡，然后单击“新建项目”。******** 遵照向导中的说明创建项目。 创建项目后，您将看到四个选项卡：**数据**、**培训**、**测试**和**部署**。 使用[后续步骤](#next-steps)中提供的链接了解如何使用每个选项卡。

> [!IMPORTANT]
> [自定义语音门户](https://aka.ms/custom-voice)最近更新！ 如果在CRIS.ai门户或使用 API 创建了以前的数据、模型、测试和已发布的终结点，则需要在新门户中创建新项目以连接到这些旧实体。

## <a name="next-steps"></a>后续步骤

- [准备自定义语音数据](how-to-custom-voice-prepare-data.md)
- [创建自定义语音](how-to-custom-voice-create-voice.md)
- [指南：录制语音样本](record-custom-voice-samples.md)
