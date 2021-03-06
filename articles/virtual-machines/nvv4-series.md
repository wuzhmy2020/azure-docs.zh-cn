---
title: NVv4 系列 - Azure 虚拟机
description: NVv4 系列 VM 的规格。
services: virtual-machines
author: vikancha
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 0295ed7d44d64fcc1aeb68e1beaa37987b177edb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273850"
---
# <a name="nvv4-series"></a>NVv4 系列 

NVv4 系列虚拟机由[AMD Radeon 本能 MI25](https://www.amd.com/en/products/professional-graphics/instinct-mi25) GPU 和 AMD EPYC 7V12（罗马）CPU 提供支持。 借助 NVv4 系列 Azure，Azure 正在引入具有部分 GPU 的虚拟机。 选择适合 GPU 加速图形应用程序和虚拟桌面的合适大小的虚拟机，从 GPU 的 1/8 开始，使用 2 个 GiB 帧缓冲区，到具有 16 个 GiB 帧缓冲区的完整 GPU。 NVv4 虚拟机目前仅支持 Windows 来宾操作系统。

<br>

高级存储：支持

高级存储缓存：支持

实时迁移：不支持

内存保留更新：不支持

| 大小 | vCPU | 内存：GiB | 临时存储 (SSD) GiB | GPU | GPU 内存：GiB | 最大数据磁盘数 | 最大 NIC 数 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV4as_v4 |4 |14 |88 | 1/8 | 2 | 4 | 2 |
| Standard_NV8as_v4 |8 |28 |176 | 1/4 | 4 | 8 | 4 |
| Standard_NV16as_v4 |16 |56 |352 | 1/2 | 8 | 16 | 8 |
| Standard_NV32as_v4 |32 |112 |704 | 1 | 16 | 32 | 8 |

<sup>1</sup> NVv4 系列 VM 采用 AMD 同步多线程技术

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>支持的操作系统和驱动程序

要利用运行 Windows 的 Azure NVv4 系列 VM 的 GPU 功能，必须安装 AMD GPU 驱动程序。

要手动安装 AMD GPU 驱动程序，请参阅[适用于 Windows 的 N 系列 AMD GPU 驱动程序设置](./windows/n-series-amd-driver-setup.md)，了解支持的操作系统、驱动程序、安装和验证步骤。

## <a name="other-sizes"></a>其他大小

- [一般用途](sizes-general.md)
- [内存优化](sizes-memory.md)
- [存储优化](sizes-storage.md)
- [GPU 优化](sizes-gpu.md)
- [高性能计算](sizes-hpc.md)
- [前几代](sizes-previous-gen.md)

## <a name="next-steps"></a>后续步骤

了解有关 [Azure 计算单元 (ACU)](acu.md) 如何帮助你跨 Azure SKU 比较计算性能的详细信息。
