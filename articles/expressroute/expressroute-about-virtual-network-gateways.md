---
title: 关于 ExpressRoute 虚拟网络网关 - Azure | Microsoft Docs
description: 了解 ExpressRoute 的虚拟网络网关。 本文包含有关网关 SKU 和类型的信息。
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: 18615cf737eedcd188fd59d2aa98482210b9333a
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991593"
---
# <a name="expressroute-virtual-network-gateway-and-fastpath"></a>ExpressRoute 虚拟网络网关和 FastPath
若要在 Azure 虚拟网络与本地网络通过 ExpressRoute 连接，必须先创建虚拟网络网关。 虚拟网络网关有两个用途： exchange 之间的网络和路由网络流量的 IP 路由。 此文章介绍了网关类型、 网关 Sku 和按 sku 列出的估计的性能。 此文章还介绍了 ExpressRoute[快速](#fastpath)，一项功能，可以使网络流量从本地网络绕过虚拟网络网关，以提高性能。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="gateway-types"></a>网关类型

创建虚拟网络网关时，需要指定几项设置。 其中一个必要设置“-GatewayType”指定是否将网关用于 ExpressRoute 或 VPN 流量。 两种网关类型是：

* **Vpn** - 若要通过公共 Internet 发送加密流量，请使用网关类型“Vpn”。 这也称为 VPN 网关。 站点到站点连接、点到站点连接和 VNet 到 VNet 连接都使用 VPN 网关。

* **ExpressRoute** - 若要在专用连接上发送网络流量，请使用网关类型“ExpressRoute”。 这也称为 ExpressRoute 网关，是配置 ExpressRoute 时使用的网关类型。

对于每种网关类型，每个虚拟网络只能有一个虚拟网络网关。 例如，一个虚拟网络网关使用 -GatewayType Vpn，另一个使用 -GatewayType ExpressRoute。

## <a name="gwsku"></a>网关 SKU
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

如果想要将网关升级为功能更强大的网关 SKU，在大多数情况下，可以使用“Resize-AzVirtualNetworkGateway”PowerShell cmdlet。 此方法适用于升级到 Standard 和 HighPerformance SKU。 但是，若要升级到 UltraPerformance SKU，需要重新创建网关。 重新创建网关会导致停机。

### <a name="aggthroughput"></a>预估性能（按网关 SKU）
下表显示网关类型和估计性能。 此表适用于 Resource Manager 与经典部署模型。

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> 应用程序性能取决于多种因素，例如端到端延迟和应用程序打开的流量数。 表中的数字表示应用程序在理想环境下理论上可达到的上限。
>
>

### <a name="zrgw"></a>区域冗余型网关 SKU

也可以在 Azure 可用性区域中部署 ExpressRoute 网关。 这在物理上和逻辑上将它们分成不同的可用区域，从而保护本地网络与 Azure 的连接免受区域级故障的影响。

![区域冗余型 ExpressRoute 网关](./media/expressroute-about-virtual-network-gateways/zone-redundant.png)

区域冗余型网关使用 ExpressRoute 网关的特定新网关 SKU。

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

新的网关 SKU 还支持其他部署选项，以最好地满足你的需求。 使用新网关 SKU 创建虚拟网络网关时，还可以选择在特定区域中部署网关。 这称为区域网关。 部署区域网关时，网关的所有实例都部署在同一可用性区域中。

## <a name="fastpath"></a>FastPath
ExpressRoute 虚拟网络网关用于交换网络路由和路由网络流量。 快速旨在提高您的本地网络与虚拟网络之间的数据路径性能。 启用时，快速的网络流量将直接发送到虚拟网络中的虚拟机绕过网关。 

上提供了快速[ExpressRoute 直接](expressroute-erdirect-about.md)仅。 换而言之，可以启用此功能仅当你[将虚拟网络连接](expressroute-howto-linkvnet-arm.md)到 ExpressRoute 线路的 ExpressRoute 直接端口上创建。 快速仍需要虚拟网络网关创建用于交换虚拟网络与本地网络之间的路由。 超高性能或 ErGw3AZ 必须是虚拟网络网关。

快速不支持以下功能：
* 网关子网上的 UDR： 如果将 UDR 应用于虚拟网络从你的本地网络的网络流量将继续发送到虚拟网络网关的网关子网。
* VNet 对等互连： 如果你有其他虚拟网络对等互连与连接到 ExpressRoute 从本地网络到其他虚拟网络"（即所谓分支"Vnet) 的网络流量将继续发送到虚拟网络网关。 解决方法是直接连接到 ExpressRoute 线路的所有虚拟网络。

## <a name="resources"></a>REST API 和 PowerShell cmdlet
有关将 REST API 和 PowerShell cmdlet 用于虚拟网络网关配置的其他技术资源和特定语法要求，请参阅以下页面：

| **经典** | **资源管理器** |
| --- | --- |
| [PowerShell](https://docs.microsoft.com/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0#azure) |[PowerShell](https://docs.microsoft.com/powershell/module/az.network#networking) |
| [REST API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>后续步骤
有关可用连接配置的详细信息，请参阅 [ExpressRoute 概述](expressroute-introduction.md)。

有关创建 ExpressRoute 网关的详细信息，请参阅[创建 ExpressRoute 的虚拟网络网关](expressroute-howto-add-gateway-resource-manager.md)。

有关配置区域冗余型网关的详细信息，请参阅[创建区域冗余型虚拟网络网关](../../articles/vpn-gateway/create-zone-redundant-vnet-gateway.md)。

请参阅[链接到 ExpressRoute 的虚拟网络](expressroute-howto-linkvnet-arm.md)有关如何启用快速详细信息。 
