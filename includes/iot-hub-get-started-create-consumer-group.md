---
author: robinsh
manager: philmea
ms.author: robinsh
ms.topic: include
ms.date: 05/20/2019
ms.openlocfilehash: c164433efc6a34a3a06676a3145feb18d3de80b9
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/27/2019
ms.locfileid: "66248854"
---
## <a name="add-a-consumer-group-to-your-iot-hub"></a>将使用者组添加到 IoT 中心

[使用者组](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#event-consumers)到事件流，使应用和 Azure 服务单独使用同一个事件中心终结点中的数据提供独立的视图。 在本部分中，添加一个使用者组到更高版本中使用本教程将数据提取从终结点的 IoT 中心的内置终结点。

要将使用者组添加到 IoT 中心，请执行以下步骤：

1. 在 [Azure 门户](https://portal.azure.com/)中打开 IoT 中心。

2. 在左窗格中，选择**内置终结点**，选择**事件**右窗格中，在下面输入名称并**使用者组**。 选择“保存”。 

   ![在 IoT 中心创建使用者组](./media/iot-hub-get-started-create-consumer-group/iot-hub-create-consumer-group-azure.png)
