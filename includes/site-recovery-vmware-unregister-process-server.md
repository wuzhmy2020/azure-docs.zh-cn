---
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: include
ms.date: 04/28/2019
ms.author: ramamill
ms.openlocfilehash: 00b0c1b1a40ad16db177916c57dba6e9d5a187a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "67173478"
---
根据你的具体情况执行以下步骤。

### <a name="unregister-a-connected-process-server"></a>注销已连接的进程服务器

1. 以管理员身份与进程服务器建立远程连接。
2. 在“控制面板”**** 中，打开“程序”>“卸载程序”****。
3. 卸载程序微软**Azure 站点恢复移动服务/主目标服务器**。
4. 卸载程序微软**Azure 站点恢复配置/进程服务器**。
5. 卸载步骤 3 和步骤 4 中的程序后，卸载**Microsoft Azure 站点恢复配置/进程服务器依赖项**。

### <a name="unregister-a-disconnected-process-server"></a>注销已断开连接的进程服务器

仅当无法恢复安装了进程服务器的计算机时，才使用这些步骤。

1. 以管理员身份登录配置服务器。
2. 打开管理命令提示符，并浏览到 `%ProgramData%\ASR\home\svsystems\bin`。
3. 运行此命令以获取一个或多个进程服务器的列表。

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
    - S. 号：进程服务器序列号。
    - IP/名称：运行进程服务器的计算机的 IP 地址和名称。
    - 检测信号：来自进程服务器计算机的最后一个检测信号。
    ![Unregister-cmd](media/site-recovery-vmware-unregister-process-server/Unregister-cmd.PNG)

4. 指定要注销的进程服务器的序列号。
5. 取消注册进程服务器从系统中删除其所有详细信息，并显示消息：**成功未注册的服务器名称>（服务器 IP 地址）**

