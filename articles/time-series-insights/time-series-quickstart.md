---
title: 快速入门：Azure 时序见解资源管理器 | Microsoft Docs
description: 此快速入门说明如何在 Web 浏览器中开始使用 Azure 时序见解资源管理器，使大量 IoT 数据可视化。 在演示环境中浏览关键功能。
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 04/22/2019
ms.openlocfilehash: be663520c9c1afd11ace57cd9dcb8ffb8219b86b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2019
ms.locfileid: "64696959"
---
# <a name="quickstart-explore-azure-time-series-insights"></a>快速入门：探索 Azure 时序见解

提供了快速入门资源管理器，使用户可在免费演示环境中体验 Azure 时序见解。 通过它，你将了解如何使用 Web 浏览器将大量 IoT 数据可视化，并浏览当前正式推出的重要功能。

Azure 时序见解是一种完全托管的分析、存储和可视化效果服务，可简化如何同时浏览和分析数十亿个 IoT 事件的过程。 它提供数据的全局视图，从而可以快速验证 IoT 解决方案并避免任务关键型设备出现代价高昂的故障时间。 通过 Azure 时序见解，可以近乎实时地发现隐藏的趋势、发现异常情况并对其进行根本原因分析。

要更加灵活地使用 Azure 时序见解，可以通过其功能强大的 [REST API](./time-series-insights-update-tsq.md) 和[客户端 SDK](./tutorial-create-tsi-sample-spa.md) 将其添加到预先存在的应用程序中。 通过 API，可以在所选择的客户端应用程序中存储、查询时序数据以及使用时序数据。 还可以选择使用客户端 SDK 将 UI 组件添加到现有应用程序中。

时序见解资源管理器是介绍当前正式版中功能的导览器。

## <a name="prepare-the-demo-environment"></a>准备演示环境

1. 如果尚未创建帐户，请创建一个[免费的 Azure 帐户](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)。

1. 在浏览器中，导航到[正式发布演示](https://insights.timeseries.azure.com/demo)。

1. 根据系统的提示使用 Azure 帐户凭据登录到时序见解资源管理器。

1. 将显示时序见解快速教程页面。 单击“下一步”  开始快速教程。

   [![单击“下一步”](media/quickstart/quickstart1.png)](media/quickstart/quickstart1.png#lightbox)

## <a name="explore-the-demo-environment"></a>浏览演示环境

1. 显示“时间选择面板”  。 使用该面板选择期限，进行可视化。

   [![时间选项面板](media/quickstart/quickstart2.png)](media/quickstart/quickstart2.png#lightbox)

1. 在此区域中单击并拖动，然后单击“搜索”按钮  。

   [![选择期限](media/quickstart/quickstart3.png)](media/quickstart/quickstart3.png#lightbox)

   时序见解显示指定期限的图表可视化效果。 可以在折线图中执行各种操作，例如筛选、固定、排序和堆叠。

   要返回到“时间选择面板”，请单击如下所示的向下键  ：

   [![图表](media/quickstart/quickstart4.png)](media/quickstart/quickstart4.png#lightbox)

1. 在“术语面板”中，单击“添加”，添加新的搜索词   。

   [![添加项](media/quickstart/quickstart5.png)](media/quickstart/quickstart5.png#lightbox)

1. 在此图表中，可以选择一个区域，右键单击该区域，然后选择“浏览事件”  。

   [![浏览事件](media/quickstart/quickstart6.png)](media/quickstart/quickstart6.png#lightbox)

   正在探索的区域将显示原始数据的网格：

   [![网格视图](media/quickstart/quickstart7.png)](media/quickstart/quickstart7.png#lightbox)

## <a name="select-and-filter-data"></a>选择和筛选数据

1. 编辑术语以更改图表中的值，并添加另一个术语来交叉关联不同类型的值：

   [![添加术语](media/quickstart/quickstart8.png)](media/quickstart/quickstart8.png#lightbox)

1. 在“筛选器系列...”对话框中输入筛选器术语，进行临时系列筛选  。 对于本快速入门，输入“Station5”交叉关联此站的温度和压力  。

   [![筛选器系列](media/quickstart/quickstart9.png)](media/quickstart/quickstart9.png#lightbox)

完成快速入门后，可尝试用示例数据集创建不同的可视化效果。

## <a name="next-steps"></a>后续步骤

已准备好创建自己的时序见解环境：
> [!div class="nextstepaction"]
> [计划时序见解环境](time-series-insights-environment-planning.md)
