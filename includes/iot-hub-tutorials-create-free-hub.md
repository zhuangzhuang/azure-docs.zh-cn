---
title: include 文件
description: include 文件
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 04/19/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: a7a86c6a2661a8a1f30491391fc76f4dc5d71f54
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/23/2019
ms.locfileid: "66163541"
---
若要使用 Azure 门户创建 IoT 中心，请执行以下操作：

1. 登录到 [Azure 门户](http://portal.azure.com)。

1. 选择“创建资源” > “物联网” > “IoT 中心”。

    ![选择安装 IoT 中心](media/iot-hub-tutorials-create-free-hub/selectiothub.png)

1. 若要创建免费层 IoT 中心，请使用以下表中的值：

    | 设置 | 值 |
    | ------- | ----- |
    | 订阅 | 在下拉列表中选择自己的 Azure 订阅。 |
    | 资源组 | 新建。 本教程使用名称 **tutorials-iot-hub-rg**。 |
    | 区域 | 本教程使用“美国西部”。 可以选择离你最近的区域。 |
    | 名称 | 以下屏幕截图使用名称 **tutorials-iot-hub**。 创建中心时，必须选择自己的唯一名称。 |

    ![中心设置 1](media/iot-hub-tutorials-create-free-hub/hubdefinition-1.png)

    | 设置 | 值 |
    | ------- | ----- |
    | 定价和缩放层 | F1 免费。 你只能在订阅中拥有一个免费层中心。 |
    | IoT 中心单位 | 1 |

    ![中心设置 2](media/iot-hub-tutorials-create-free-hub/hubdefinition-2.png)

1. 单击“创建”。 创建中心可能需要几分钟。

    ![中心设置 3](media/iot-hub-tutorials-create-free-hub/hubdefinition-3.png)

1. 记下所选的 IoT 中心名称。 本教程稍后会用到此值。