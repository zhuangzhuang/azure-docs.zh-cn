---
title: include 文件
description: include 文件
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 482241deb1081ac8a5265a076eabbdc3fb6d659e
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170865"
---
尽管使用系统路由可以自动加快通信以方便部署，但在某些情况下，需要通过虚拟设备来控制数据包的路由。 为此，可以创建用户定义的路由来指定下一跃点，方便数据包流向特定的子网并转到虚拟设备，并可为作为虚拟设备运行的 VM 启用 IP 转发。

可以使用虚拟设备的一些用例包括：

* 使用入侵检测系统 (IDS) 监视流量
* 使用防火墙控制流量

有关 UDR 和 IP 转发的详细信息，请访问[用户定义的路由和 IP 转发](../articles/virtual-network/virtual-networks-udr-overview.md)。

