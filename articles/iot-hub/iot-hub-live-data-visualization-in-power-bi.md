---
title: Azure IoT 中心的实时传感器数据可视化 - Power BI | Microsoft Docs
description: 使用 Power BI 可视化从传感器收集的温度和湿度数据并将其发送到 Azure IoT 中心。
author: robinsh
keywords: 实时数据可视化, 即时数据可视化, 传感器数据可视化
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 4/11/2018
ms.author: robinsh
ms.openlocfilehash: 7c770aced36e4c90f654de8d31c12d55ad80c8d0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "60779966"
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a>使用 Power BI 可视化 Azure IoT 中心的实时传感器数据

![端到端关系图](./media/iot-hub-live-data-visualization-in-power-bi/1_end-to-end-diagram.png)


[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>学习内容

了解如何可视化 Azure IoT 中心通过 Power BI 接收的实时传感器数据。 若要尝试使用 Web 应用可视化 IoT 中心的数据，请参阅[使用 Azure Web 应用可视化 Azure IoT 中心的实时传感器数据](iot-hub-live-data-visualization-in-web-apps.md)。

## <a name="what-you-do"></a>准备工作

* 添加一个使用者组，让 IoT 中心做好数据访问准备。

* 创建、配置和运行流分析作业，以便将数据从 IoT 中心传输到 Power BI 帐户。

* 创建并发布实现数据可视化的 Power BI 报表。

## <a name="what-you-need"></a>所需条件

* 完成[Raspberry Pi 联机模拟器](iot-hub-raspberry-pi-web-simulator-get-started.md)教程或其中一个设备教程中; 例如， [Raspberry Pi 与 node.js 配合使用](iot-hub-raspberry-pi-kit-node-get-started.md)。 这些涵盖以下要求：
  
  * 一个有效的 Azure 订阅。
  * 已在订阅中创建一个 Azure IoT 中心。
  * 一个可向 Azure IoT 中心发送消息的客户端应用程序。

* 一个 Power BI 帐户。 （[免费试用 Power BI](https://powerbi.microsoft.com/)）

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>创建、配置和运行流分析作业

让我们首先创建一个流分析作业。 创建作业后，可以定义输入、输出和用于检索数据的查询。

### <a name="create-a-stream-analytics-job"></a>创建流分析作业

1. 在 [Azure 门户](https://portal.azure.com)中，单击“创建资源” > “物联网” > “流分析作业”。

2. 为作业输入以下信息。

   **作业名称**：作业的名称。 该名称必须全局唯一。

   **资源组**：使用 IoT 中心所用的同一资源组。

   **位置**：与资源组使用同一位置。

   **固定到仪表板**：选中此选项可以方便地从仪表板访问 IoT 中心。

   ![在 Azure 中创建流分析作业](./media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

3. 单击“创建”。

### <a name="add-an-input-to-the-stream-analytics-job"></a>向流分析作业添加输入

1. 打开流分析作业。

2. 在“作业拓扑”下，单击“输入”。

3. 在“输入”窗格中，单击“添加流输入”，并输入以下信息：

   **输入别名**：输入的唯一别名，并选择下面的“手动提供 IoT 中心设置”。

   **源**：选择“IoT 中心”。
   
   **终结点**：单击“消息传送”。

   **使用者组**：选择刚创建的使用者组。

4. 单击**创建**。

   ![向 Azure 中的流分析作业添加输入](./media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-to-the-stream-analytics-job"></a>向流分析作业添加输出

1. 在“作业拓扑”下，单击“输出”。

2. 在“输出”窗格中，依次单击“添加”和“Power BI”，并输入以下信息：

   **输出别名**：输出的唯一别名。

   **组工作区**：选择目标组工作区。

   **数据集名称**：输入数据集名称。

   **表名称**：输入表名称。

3. 单击“授权”，并登录到 Power BI 帐户。

4. 单击**创建**。

   ![向 Azure 中的流分析作业添加输出](./media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-the-query-of-the-stream-analytics-job"></a>配置流分析作业的查询

1. 在“作业拓扑”下，单击“查询”。

2. 将 `[YourInputAlias]` 替换为作业的输入别名。

3. 将 `[YourOutputAlias]` 替换为作业的输出别名。

4. 单击“ **保存**”。

   ![向 Azure 中的流分析作业添加查询](./media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-the-stream-analytics-job"></a>运行流分析作业

在流分析作业中，单击“启动” > “现在” > “启动”。 成功启动作业以后，作业状态将从“已停止”更改为“正在运行”。

![在 Azure 中运行流分析作业](./media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-to-visualize-the-data"></a>创建并发布实现数据可视化的 Power BI 报表

1. 确保示例应用程序正在设备上运行。 如果没有，请参考[设置设备](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started)中的教程。

2. 登录到 [Power BI](https://powerbi.microsoft.com/en-us/) 帐户。

3. 单击你使用的工作区“我的工作区”。

4. 单击“数据集”。

   此时应当会看到你在为流分析作业创建输出时指定的数据集。

5. 对于你创建的数据集，单击“添加报表”（数据集名称右侧的第一个图标）。

   ![创建 Microsoft Power BI 报表](./media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

6. 创建折线图，显示某段时间的实时温度。

   1. 在报表创建页面上，添加折线图。

   2. 在“字段”窗格中展开一个表，该表是在为流分析作业创建输出时指定的。
   
   3. 将 **EventEnqueuedUtcTime** 拖至“可视化效果”窗格中的“轴”。
   
   4. 将“温度”拖至“值”。

      已创建一个折线图。 X 轴显示 UTC 时区的日期和时间。 Y 轴显示来自传感器的温度。

      ![向 Microsoft Power BI 报表添加温度折线图](./media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

7. 创建另一个折线图，显示某段时间的实时湿度。 为此，请执行上述相同步骤，将 **EventEnqueuedUtcTime** 置于 X 轴，将“湿度”置于 Y 轴。

   ![向 Microsoft Power BI 报表添加湿度折线图](./media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

8. 单击“保存”以保存该报表。

9. 在左侧窗格上单击“报表”，然后单击你刚才创建的报表。

10. 单击“文件” > “发布到 Web”。

11. 单击“创建嵌入代码”，并单击“发布”。

系统会提供一个报表链接，以便将其共享给任何需要进行报表访问的人；同时还会提供一个代码片段，以便将报表集成到博客或网站中。

![发布 Microsoft Power BI 报表](./media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

Microsoft 还提供 [Power BI 移动应用](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/)，用于在移动设备上查看 Power BI 仪表板和报表并进行交互。

## <a name="next-steps"></a>后续步骤

现已成功使用 Power BI 可视化 Azure IoT 中心的实时传感器数据。

还可以通过另一种方法可视化 Azure IoT 中心的数据。 请参阅[使用 Azure Web 应用可视化 Azure IoT 中心的实时传感器数据](iot-hub-live-data-visualization-in-web-apps.md)。

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
