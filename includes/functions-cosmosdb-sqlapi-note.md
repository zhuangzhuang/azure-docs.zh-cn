---
title: include 文件
description: include 文件
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 08/22/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: c54aa861a47b11756f05e003e9b944df6c5b0e28
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132443"
---
Azure Cosmos DB 绑定只能与 SQL API 配合使用。 对于所有其他的 Azure Cosmos DB API，应使用适用于 API 的静态客户端通过函数来访问数据库。API 包括 [Azure Cosmos DB 的 API for MongoDB](../articles/cosmos-db/mongodb-introduction.md)、[Cassandra API](../articles/cosmos-db/cassandra-introduction.md)、[Gremlin API](../articles/cosmos-db/graph-introduction.md) 和[表 API](../articles/cosmos-db/table-introduction.md)。
