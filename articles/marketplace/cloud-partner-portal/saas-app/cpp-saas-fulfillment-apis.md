---
title: SaaS 履行 Api |Azure Marketplace
description: 引入了 Api，使您可以将你的 SaaS 集成提供了在 Azure marketplace 中销售的履行的版本。
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: pabutler
ms.openlocfilehash: e7a01af1eba865b0a088b0fa7226273527abd70e
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2019
ms.locfileid: "66257412"
---
# <a name="saas-fulfillment-apis"></a>SaaS 履行 API

> [!IMPORTANT] 
> SaaS 产品/服务的功能已迁移到[Microsoft 合作伙伴中心](https://partner.microsoft.com/dashboard/directory)。  所有新的发布服务器必须使用合作伙伴中心创建新的 SaaS 产品/服务和管理现有产品/服务。  使用 SaaS 产品/服务的当前发布服务器正在 batchwise 迁移从云合作伙伴门户到合作伙伴中心。  云合作伙伴门户将显示状态消息以指示何时已迁移特定的现有产品/服务。
> 有关详细信息，请参阅[创建新的 SaaS 产品/服务](../../partner-center-portal/create-new-saas-offer.md)。

SaaS 履行 Api 使独立软件供应商 (Isv) 可以将其 SaaS 应用程序集成在 Azure marketplace 中销售。 这些 Api 使 ISV 应用程序能够参与所有启用的商务通道： 直接、 合作伙伴主导式 （经销商） 和字段导致。  它们是用于列出在 Azure Marketplace 上的事务 SaaS 产品/服务的要求。

> [!WARNING]
> 此 API 的当前版本为版本 2，这应该用于所有新的 SaaS 提供。  第 1 版 API 已过时，维护以支持现有产品/服务。


## <a name="business-model-support"></a>业务模型支持

此 API 支持以下业务模型功能;您可以：

* 指定产品/服务的多个的计划。 这些计划具有不同的功能，并可能价格不一样。
* 提供有关产品/服务每个站点或每个用户计费模型的基础。
* 提供每月和年 （提前付费） 计费选项。
* 提供专用于基于协商的业务协议客户定价。


## <a name="next-steps"></a>后续步骤

如果尚未这样做，在将 SaaS 应用程序注册[Azure 门户](https://ms.portal.azure.com)中所述[注册一个 Azure AD 应用程序](./cpp-saas-registration.md)。  然后，使用此接口的最新版本进行开发：[SaaS 履行 API 版本 2](./cpp-saas-fulfillment-api-v2.md)。
