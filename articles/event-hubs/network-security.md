---
title: Azure 事件中心的网络安全性
description: 本文介绍如何配置来自专用终结点的访问
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: spelluru
ms.openlocfilehash: 46e6a9ecc2ed09aed1076f12c1f61a966485bdad
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2020
ms.locfileid: "80422770"
---
# <a name="network-security-for-azure-event-hubs"></a>Azure 事件中心的网络安全性 
本文介绍如何使用以下安全功能与 Azure 事件中心： 

- 服务标记
- IP 防火墙规则
- 网络服务终结点
- 专用终结点（预览）


## <a name="service-tags"></a>服务标记
服务标记代表给定 Azure 服务中的一组 IP 地址前缀。 Microsoft 管理服务标记包含的地址前缀，并在地址更改时自动更新服务标记，从而最大限度地减少频繁更新网络安全规则的复杂性。 有关服务标记的详细信息，请参阅[服务标记概述](../virtual-network/service-tags-overview.md)。

可以使用服务标记在[网络安全组](../virtual-network/security-overview.md#security-rules) 或 [Azure 防火墙](../firewall/service-tags.md)上定义网络访问控件。 创建安全规则时，使用服务标记代替特定的 IP 地址。 通过在规则的相应*源* 或 *目标* 字段中指定服务标记名称（例如**EventHub），** 可以允许或拒绝相应服务的流量。

| 服务标记 | 目的 | 可以使用入站或出站吗？ | 可以是区域性的吗？ | 是否可在 Azure 防火墙中使用？ |
| --- | -------- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **事件中心** | Azure 事件中心。 | 出站 | 是 | 是 |


## <a name="ip-firewall"></a>IP 防火墙 
默认情况下，只要请求附带有效的身份验证和授权，事件中心命名空间即可从 Internet 访问。 使用 IP 防火墙，您可以进一步将其限制为[CIDR（无类域间路由）](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)表示法中的一组 IPv4 地址或 IPv4 地址范围。

此功能在 Azure 事件中心应仅从某些已知站点访问的情况下非常有用。 防火墙规则使您能够配置规则以接受来自特定 IPv4 地址的流量。 例如，如果将事件中心与 Azure[快速路由](/azure/expressroute/expressroute-faqs#supported-services)一起使用 ，则可以创建**防火墙规则**，仅允许来自本地基础结构 IP 地址的流量。 

IP 防火墙规则在事件中心命名空间级别应用。 因此，这些规则适用于通过任何受支持协议从客户端发出的所有连接。 如果某 IP 地址与事件中心命名空间上的允许 IP 规则不匹配，则将拒绝来自该地址的任何连接尝试并将其标记为“未经授权”。 响应不会提及 IP 规则。 IP 筛选器规则将按顺序应用，与 IP 地址匹配的第一个规则决定了将执行接受操作还是执行拒绝操作。

有关详细信息，请参阅[如何为事件中心配置 IP 防火墙](event-hubs-ip-filtering.md)

## <a name="network-service-endpoints"></a>网络服务终结点
通过将事件中心与[虚拟网络 (VNet) 服务终结点](../virtual-network/virtual-network-service-endpoints-overview.md)相集成，可从绑定到虚拟网络的工作负荷（例如虚拟机）安全地访问消息传递功能，同时在两端保护网络流量路径。

一旦配置为至少绑定到一个虚拟网络子网服务终结点，相应的事件中心命名空间将不再接受来自虚拟网络中的经过授权的子网的流量。 从虚拟网络的角度来看，通过将事件中心命名空间绑定到服务终结点，可配置从虚拟网络子网到消息传递服务的独立网络隧道。 

然后，绑定到子网的工作负荷与相应的事件中心命名空间之间将存在专用和独立的关系，消息传递服务终结点的可观察网络地址位于公共 IP 范围内对此没有影响。 此行为有一个例外。 默认情况下，启用服务终结点可在[IP](event-hubs-ip-filtering.md)防火墙`denyall`中启用与虚拟网络关联的规则。 您可以在 IP 防火墙中添加特定的 IP 地址，以启用对事件中心公共终结点的访问。 

> [!IMPORTANT]
> 事件中心的标准**** 和专用**** 层支持虚拟网络。 **基本**层不支持它。

### <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>通过 VNet 集成启用的高级安全方案 

需要严格和分门别类安全性的解决方案，以及虚拟网络子网在隔离服务之间提供分段时，仍然需要位于这些隔离区中的服务之间的通信路径。

隔离舱之间的任何即时 IP 路由（包括通过 TCP/IP 承载 HTTPS 的）都存在利用网络层漏洞的风险。 消息传递服务提供绝缘通信路径，其中消息甚至在各方之间转换时写入磁盘。 绑定到同一个事件中心实例的两个不同虚拟网络中的工作负荷可通过消息进行高效和可靠的通信，同时保留各自的网络隔离边界完整性。
 
这意味着安全敏感云解决方案不仅可以访问 Azure 行业领先的可靠且可扩展的异步消息传递功能，而且现在可以使用消息传递在安全解决方案隔离舱之间创建通信路径，这些隔离舱本质上比利用任何对等通信模式（包括 HTTPS 和其他 TLS 安全套接字协议）更加安全。

### <a name="bind-event-hubs-to-virtual-networks"></a>将事件中心绑定到虚拟网络

虚拟网络规则**** 是一种防火墙安全功能，用于控制是否允许 Azure 事件中心命名空间接受来自特定虚拟网络子网的连接。

将事件中心命名空间绑定到虚拟网络的过程分为两步。 首先需要在虚拟网络的子网上创建**虚拟网络服务终结点**，并为**Microsoft.EventHub**启用它，如[服务终结点概述](../virtual-network/virtual-network-service-endpoints-overview.md)文章中所述。 添加服务终结点后，使用虚拟网络规则将事件中心命名空间绑定到该终结点****。

虚拟网络规则是事件中心命名空间与虚拟网络子网的关联。 存在此规则时，绑定到子网的所有工作负荷都有权访问事件中心命名空间。 事件中心本身永远不会建立出站连接，不需要访问，因此永远不会通过启用此规则被授予对子网的访问权限。

有关详细信息，请参阅[如何为事件中心配置虚拟网络服务终结点](event-hubs-service-endpoints.md)

## <a name="private-endpoints"></a>专用终结点

[Azure 专用链接服务](../private-link/private-link-overview.md)使您能够通过虚拟网络中的**专用终结点**访问 Azure 服务（例如，Azure 事件中心、Azure 存储和 Azure 宇宙数据库）和 Azure 托管的客户/合作伙伴服务。

专用终结点是一个网络接口，可私下安全地连接到由 Azure 专用链接提供支持的服务。 专用终结点使用 VNet 中的专用 IP 地址将服务有效接入 VNet 中。 发往服务的所有流量都可以通过专用终结点路由，因此不需要网关、NAT 设备、ExpressRoute 或 VPN 连接或公共 IP 地址。 虚拟网络与服务之间的流量将通过 Microsoft 主干网络，因此不会从公共 Internet 泄露。 可以连接到 Azure 资源的实例，从而获得最高级别的访问控制粒度。

> [!NOTE]
> 此功能仅支持**专用**层。 有关专用层的详细信息，请参阅[活动中心专用概述](event-hubs-dedicated-overview.md)。 
>
> 此功能当前处于**预览状态**。 


有关详细信息，请参阅[如何为事件中心配置专用终结点](private-link-service.md)


## <a name="next-steps"></a>后续步骤
请参阅以下文章：

- [如何为事件中心配置 IP 防火墙](event-hubs-ip-filtering.md)
- [如何为事件中心配置虚拟网络服务终结点](event-hubs-service-endpoints.md)
- [如何为事件中心配置专用终结点](private-link-service.md)