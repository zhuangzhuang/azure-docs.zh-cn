---
title: 利用生存时间使 Azure Cosmos DB 中的数据过期
description: 通过 TTL 功能，Microsoft Azure Cosmos DB 能够在一段时间后将文档自动从系统清除。
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 692e0ec575904ff0a70b8c73268d2df62e776bb6
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978777"
---
# <a name="time-to-live-ttl-in-azure-cosmos-db"></a>Azure Cosmos DB 中的生存时间 (TTL) 

利用**生存时间**（简称 TTL），Azure Cosmos DB 能够在特定一段时间后自动将项从容器中删除。 默认情况下，可以在容器级别设置生存时间，并基于每个项替代该值。 在容器或项级别设置 TTL 后，Azure Cosmos DB 会在一段时间（自上次修改项的时间开始算起）后自动删除这些项。 配置的生存时间值以秒为单位。 配置 TTL 后，系统会基于 TTL 值自动删除已过期的项，不需要客户端应用程序显式发出的 delete 操作。

## <a name="time-to-live-for-containers-and-items"></a>容器和项的生存时间

生存时间值是以秒为单位设置的，解释为自上次修改项的时间的增量。 可以针对容器或容器中的项设置生存时间：

1. **容器的生存时间**（使用 `DefaultTimeToLive` 设置）：

   - 如果缺失（或设置为 null），则项不会自动过期。

   - 如果存在且值设置为"-1"，它等于无穷大，并且项不会到期默认情况下。

   - 如果存在且值设置为某个数字 *"n"* – 项目将会过期 *"n"* 秒后其上次修改时间。

2. **项的生存时间**（使用 `ttl` 设置）：

   - 仅当父容器的 `DefaultTimeToLive` 存在且不是设置为 null 时，此属性才适用。

   - 如果存在，它将替代父容器的 `DefaultTimeToLive` 值。

## <a name="time-to-live-configurations"></a>生存时间配置

* 如果将某个容器的 TTL 设置为“n”，该容器中的项将在 n 秒后过期。  如果同一容器中的项有自身的生存时间且 TTL 设置为 -1（表示不会过期），或者某些项使用不同的数字替代了生存时间设置，则这些项会根据其自己的已配置 TTL 值过期。 

* 如果未针对某个容器设置 TTL，则此容器中的项的生存时间不起作用。 

* 如果某个容器的 TTL 设置为 -1，则此容器中生存时间设置为 n 的项将在 n 秒后过期，剩余的项不会过期。 

基于 TTL 删除项是免费的。 TTL 过期后删除项不会产生额外的费用（即，不会消耗额外的 RU）。

## <a name="next-steps"></a>后续步骤

通过以下文章了解如何配置生存时间：

* [如何配置生存时间](how-to-time-to-live.md)
