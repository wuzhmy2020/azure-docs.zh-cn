---
title: Fsv2 系列 - Azure 虚拟机
description: Fsv2 系列 VM 的规格。
services: virtual-machines
author: brbell
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: f28c6b61aee3c8cbc078db1c2cfb48ed1fba4554
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "78164842"
---
# <a name="fsv2-series"></a>Fsv2 系列

Fsv2 系列基于英特尔® Xeon ®白金 8168 处理器。 它具有持续全芯涡轮时钟速度为 3.4 GHz 和最大单核涡轮频率为 3.7 GHz。 英特尔® AVX-512 指令是英特尔可扩展处理器上的新指令。 这些指令在单精度和双精度浮点操作上为矢量处理工作负载提供高达 2 倍的性能提升。 换句话说，对于任何计算工作负载来说，它们都非常快。

Fsv2 系列 VM 采用英特尔®超线程技术。

ACU：195 - 210

高级存储：支持

高级存储缓存：支持

实时迁移：支持

内存保留更新：受支持

| 大小 | vCPU | 内存：GiB | 临时存储 (SSD) GiB | 最大数据磁盘数 | 最大缓存和临时存储吞吐量：IOPS/MBps（GiB 中的缓存大小） | 最大未缓存磁盘吞吐量：IOPS/MBps | 最大 NIC/预期网络带宽 （Mbps） |
|---|---|---|---|---|---|---|---|
| Standard_F2s_v2  | 2  | 4   | 16  | 4  | 4000/31 (32)       | 3200/47    | 2/875   |
| Standard_F4s_v2  | 4  | 8   | 32  | 8  | 8000/63 (64)       | 6400/95    | 2/1750  |
| Standard_F8s_v2  | 8  | 16  | 64  | 16 | 16000/127 (128)    | 12800/190  | 4/3500  |
| Standard_F16s_v2 | 16 | 32  | 128 | 32 | 32000/255 (256)    | 25600/380  | 4/7000  |
| Standard_F32s_v2 | 32 | 64  | 256 | 32 | 64000/512 (512)    | 51200/750  | 8/14000 |
| Standard_F48s_v2 | 48 | 96  | 384 | 32 | 96000/768 (768)    | 76800/1100 | 8/21000 |
| Standard_F64s_v2 | 64 | 128 | 512 | 32 | 128000/1024 (1024) | 80000/1100 | 8/28000 |
| Standard_F72s_v2<sup>1， 2</sup> | 72 | 144 | 576 | 32 | 144000/1152 (1520) | 80000/1100 | 8/30000 |

<sup>1</sup>使用超过 64 个 vCPU 需要以下支持的来宾操作系统之一：

- Windows 服务器 2016 或更高版本
- Ubuntu 16.04 LTS 或更高版本，使用 Azure 调谐内核（4.15 内核或更高版本）
- SLES 12 SP2 或更高版本
- RHEL 或 CentOS 版本 6.7 到 6.10，安装了 Microsoft 提供的 LIS 软件包 4.3.1（或更高版本）
- RHEL 或 CentOS 版本 7.3，安装了 Microsoft 提供的 LIS 软件包 4.2.1（或更高版本）
- RHEL 或 CentOS 版本 7.6 或更高版本
- 带有 UEK4 或更高版本的 Oracle Linux
- Debian 9 与后端口内核， Debian 10 或更高版本
- 具有 4.14 个或更高版本的 CoreOS

<sup>2</sup> 实例对于专用于单个客户的硬件独立。

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes"></a>其他大小

- [一般用途](sizes-general.md)
- [内存优化](sizes-memory.md)
- [存储优化](sizes-storage.md)
- [GPU 优化](sizes-gpu.md)
- [高性能计算](sizes-hpc.md)
- [前几代](sizes-previous-gen.md)

## <a name="next-steps"></a>后续步骤

了解有关 [Azure 计算单元 (ACU)](acu.md) 如何帮助你跨 Azure SKU 比较计算性能的详细信息。
