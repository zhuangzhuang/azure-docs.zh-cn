---
title: 什么是 Azure Active Directory 条件访问中的服务依赖项？ | Microsoft Docs
description: 了解如何在 Azure Active Directory 条件访问中使用条件来触发策略。
services: active-directory
keywords: 对应用的条件性访问, 使用 Azure AD 进行条件性访问, 保护对公司资源的访问, 条件性访问策略
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/18/2019
ms.author: joflore
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: f727fc7133ebc9ee124e63253e8a266862b0d908
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "60354345"
---
# <a name="what-are-service-dependencies-in-azure-active-directory-conditional-access"></a>什么是 Azure Active Directory 条件访问中的服务依赖项？ 


使用条件性访问策略，可以指定到网站和服务的访问要求。 例如，您的访问要求可以包括需要多重身份验证 (MFA) 或[托管设备](require-managed-devices.md)。 


直接访问的网站或服务，相关策略的影响时，通常容易评估。 例如，如果您有一个策略，要求适用于 SharePoint Online 配置 MFA，MFA 将强制为每次登录到 SharePoint web 门户。 但是，它并不总是直接以评估策略的影响，因为云应用程序的其他云应用的依赖项。 例如，Microsoft Teams 利用了 SharePoint online。 因此，在 Microsoft Teams 访问在我们当前的方案时，您还将受 SharePoint MFA 策略约束。   


## <a name="policy-enforcement"></a>策略强制执行 

如果已配置的服务依赖项，可能会使用早期绑定或后期绑定强制应用的策略。 

**早期绑定策略实施**意味着用户在访问调用的应用之前必须满足依赖的服务策略。 例如，用户必须登录到 MS Teams 之前满足 SharePoint 策略。 

**后期绑定策略实施**到调用应用程序在用户登录后发生。 调用下游服务的令牌的应用请求时，强制将推迟到。 示例包括 MS Teams 访问 Planner 和 Office.com 访问 SharePoint。 

下图显示了 MS Teams 服务依赖项。 规划器指示后期绑定强制，实线箭头指示早期绑定强制的虚线箭头。 



![MS Teams 服务依赖项](./media/service-dependencies/01.png)



  

作为最佳做法，应在相关的应用和服务尽可能设置通用的策略。 具有一致的安全状况提供最佳用户体验。 例如，在 Exchange Online 设置通用策略，SharePoint Online、 MS Teams 和 Skype 的业务显著减少了意外从不同的策略应用于下游服务可能会出现的提示。 

下表列出了其他服务依赖关系，其中客户端应用程序必须满足  

| 客户端应用         | 下游服务                          | 强制 |
| :--                 | :--                                         | ---         | 
| Azure Data Lake     | Microsoft Azure 管理 （门户和 API） | 早期绑定 |
| Microsoft 教室 | Exchange                                    | 早期绑定 |
|                     | SharePoint                                  | 早期绑定  |
| Microsoft Teams     | Exchange                                    | 早期绑定 |
|                     | MS 规划器                                  | 后期绑定  |
|                     | SharePoint                                  | 早期绑定 |
|                     | Skype for Business Online                   | 早期绑定 |
| Office 门户       | Exchange                                    | 后期绑定  |
|                     | SharePoint                                  | 后期绑定  |
| Outlook 组      | Exchange                                    | 早期绑定 |
|                     | SharePoint                                  | 早期绑定 |
| PowerApps           | Microsoft Azure 管理 （门户和 API） | 早期绑定 |
|                     | Microsoft Azure Active Directory              | 早期绑定 |
| Project             | Dynamics CRM                                | 早期绑定 |
| Skype for Business  | Exchange                                    | 早期绑定 |
| Visual Studio       | Microsoft Azure 管理 （门户和 API） | 早期绑定 |



## <a name="next-steps"></a>后续步骤

若要了解如何在环境中实现条件访问，请参阅[在 Azure Active Directory 中规划条件访问部署](plan-conditional-access.md)。
