---
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 14abae6f6f72d724fffb1ccaa12f56fb6976f7a1
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/23/2019
ms.locfileid: "66160203"
---
下表列出了可能的 Windows 通用和专用OS 映像的上传和捕获组合。 使用 Y 表示处理不会有任何错误的组合，使用 N 表示会出现错误的组合。下表提供了有关各种错误的原因和解决方法。

| 操作系统 | 上传专用 OS 映像 | 上传通用 OS 映像 | 捕获专用 OS 映像 | 捕获通用 OS 映像 |
| --- | --- | --- | --- | --- |
| Windows 通用 |N<sup>1</sup> |是 |N<sup>3</sup> |是 |
| Windows 专用 |是 |N<sup>2</sup> |是 |N<sup>4</sup> |

