---
title: 创建的组或应用程序-Azure Active Directory 访问评审 |Microsoft Docs
description: 了解如何在 Azure Active Directory 访问评审中创建访问评审的组成员或应用程序访问。
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/21/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6bafa4614e40bb1796ec90e07ecf5b9286a8acb9
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/23/2019
ms.locfileid: "66113507"
---
# <a name="create-an-access-review-of-groups-or-applications-in-azure-ad-access-reviews"></a>创建访问评审的组或 Azure AD 中的应用程序访问评审

员工和来宾对组和应用程序的访问权限会不断变化。 为了降低与过期访问权限分配相关的风险，管理员可以使用 Azure Active Directory (Azure AD) 针对组成员或应用程序访问权限创建访问评审。 如果需要定期评审访问权限，则还可以创建定期访问评审。 有关这些方案的详细信息，请参阅[管理用户访问权限](manage-user-access-with-access-reviews.md)和[管理来宾访问权限](manage-guest-access-with-access-reviews.md)。

本文介绍如何创建一个或多个访问评审的组成员或应用程序访问。

## <a name="prerequisites"></a>必备组件

- Azure AD Premium P2
- [访问评审已启用](access-reviews-overview.md)
- 全局管理员或用户管理员

有关详细信息，请参阅[哪些用户必须具有许可证？](access-reviews-overview.md#which-users-must-have-licenses)。

## <a name="create-one-or-more-access-reviews"></a>创建一个或多个访问评审

1. 登录到 Azure 门户，然后打开[访问评审页](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/)。

1. 在左侧菜单中，单击**访问评审**。

1. 单击“新建访问评审”创建新的访问评审。

    ![访问评审 - 控制](./media/create-access-review/access-reviews.png)

1. 命名访问评审。 可选择为评审提供说明。 名称和说明向评审者显示。

    ![创建访问评审 - 评审名称和说明](./media/create-access-review/name-description.png)

1. 设置“开始日期”。 默认情况下，访问评审只进行一次，从创建的时候开始，在一个月内结束。 可以更改开始和结束日期，使访问评审在将来的时间开始，并持续所需的天数。

    ![创建访问评审 - 开始和结束日期](./media/create-access-review/start-end-dates.png)

1. 若要使访问评审再次发生，请更改**频率**设置从**一次**到**每周**，**每月**， **每季度**或**每年一次**。 使用**持续时间**滑块或文本框中，可以定义多长时间内每段评论各定期系列的将是打开的审阅者的输入。 例如，每月评审的最长持续时间可以设置为 27 天，以免评审时间重叠。

1. 使用“结束”设置指定如何结束定期访问评审系列。 系列的结束方式有三种：持续运行，无限期地开始评审；运行至指定日期；运行至已完成定义的评审数目。 另一个用户管理员或另一个全局管理员可以停止序列创建之后通过更改中的日期**设置**，以便它在该日期结束。

1. 在中**用户**部分中，指定访问评审适用于的用户。 访问评审的对象可以是组成员，或者是已分配到应用程序的用户。 可将访问评审的范围进一步限定为仅评审属于成员（或已分配到应用程序）的来宾用户，而不是评审属于成员或有权访问应用程序的所有用户。

    ![创建访问评审 - 用户](./media/create-access-review/users.png)

1. 在中**组**部分中，选择想要查看的成员身份的一个或多个组。

    > [!NOTE]
    > 选择多个组，将创建多个访问评审。 例如，选择五个组将创建五个单独的访问评审。
    
    ![创建访问评审的所选组](./media/create-access-review/select-group.png)

1. 在中**应用程序**部分 (如果选择了**分配给应用程序**在步骤 8 中)，选择你想要评审对访问权限的应用程序。

    > [!NOTE]
    > 选择多个应用程序将创建多个访问评审。 例如，选择五个应用程序将创建五个单独的访问评审。
    
    ![创建访问评审-选择应用程序](./media/create-access-review/select-application.png)

1. 在“评审者”部分选择一人或多人来评审范围内的所有用户。 也可以选择让成员评审自己的访问权限。 如果资源是一个组，可以要求组的所有者进行评审。 还可以要求评审者在审批访问权限时提供原因。

    ![创建访问评审 - 评审者](./media/create-access-review/reviewers.png)

1. 在“计划”部分选择要使用的计划。 “默认计划”将始终存在。

    ![创建访问评审 - 计划](./media/create-access-review/programs.png)

    可以将访问评审组织为计划，以便针对不同目的简化跟踪和收集访问评审的方式。 可以将每个访问评审链接到一个计划。 然后，在为审核员准备报告时，可以将重点放在特定计划范围内的访问评审。 程序和访问评审结果是对全局管理员、 用户管理员、 安全管理员或安全读取者角色中的用户可见。

    若要查看程序的列表，请转到访问评审页，然后选择**程序**。 如果你是全局管理员或用户管理员角色中，您可以创建其他计划。 例如，可以选择针对每个符合性措施或业务目标创建一个计划。 如果不再需要某个计划，并且没有任何链接到它的控件，则可以将其删除。

### <a name="upon-completion-settings"></a>完成设置后

1. 若要指定评审完成后发生的情况，请展开“完成后的设置”部分。

    ![完成后的设置](./media/create-access-review/upon-completion-settings.png)

1. 若要自动删除被拒绝用户的访问权限，请将“将结果自动应用到资源”设置为“启用”。 若要在评审完成后手动应用结果，请将开关设置为“禁用”。

1. 使用“如果审阅者未答复”列表指定对于审阅者在评审期限内未评审的用户要执行的操作。 此设置不影响审阅者已手动评审的用户。 如果最终的审阅者决策是“拒绝”，则会删除用户的访问权限。

    - **不更改** - 将用户访问权限保持不变
    - **删除访问权限** - 删除用户的访问权限
    - **批准访问权限** - 批准用户的访问权限
    - **采用建议** - 根据系统的建议拒绝或批准用户的持续访问权限

### <a name="advanced-settings"></a>高级设置

1. 若要指定其他设置，请展开“高级设置”部分。

    ![高级设置](./media/create-access-review/advanced-settings.png)

1. 将“显示建议”设置为“启用”，以基于用户的访问权限信息向评审者显示系统建议。

1. 将“需要提供审批原因”设置为“启用”，以要求审阅者提供批准原因。

1. 将“邮件通知”设置为“启用”，以便在访问评审开始时让 Azure AD 向评审者发送电子邮件通知，并在评审完成时向管理员发送电子邮件通知。

1. 将“提醒”设置为“启用”，让 Azure AD 向尚未完成其审阅的审阅者发送访问评审正在进行的提醒。

    默认情况下，Azure AD 自动在中途向还未作出回复的审阅者发送结束日期提醒。

## <a name="start-the-access-review"></a>启动访问评审

指定访问评审的设置后，单击“启动”。 访问评审会在其状态指示符列表中显示。

![访问评审列表](./media/create-access-review/access-reviews-list.png)

默认情况下，在评审开始后不久，Azure AD 会向评审者发送一封电子邮件。 如果选择不让 Azure AD 发送电子邮件，请务必通知评审者有一个访问评审任务等待他们完成。 说明如何显示[评审为组或应用程序的访问权限](perform-access-review.md)。 如果评审工作是让来宾评审他们自己的访问权限，显示它们如何的说明[评审自己的访问权限，对组或应用程序](review-your-access.md)。

如果已分配为审阅者的来宾和他们未接受邀请，它们不会收到一封电子邮件来自访问评审因为它们必须先接受邀请之前查看。

## <a name="create-reviews-via-apis"></a>通过 API 创建评审

也可以使用 API 创建访问评审。 在 Azure 门户中管理组和应用程序用户的访问评审的方法也可以使用 Microsoft Graph API 来实现。 有关详细信息，请参阅[API 参考的 Azure AD 访问评审](https://docs.microsoft.com/graph/api/resources/accessreviews-root?view=graph-rest-beta)。 代码示例，请参阅[检索 Azure AD 访问的示例通过 Microsoft Graph 评审](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/m-p/236096)。

## <a name="next-steps"></a>后续步骤

- [评审访问权限给组或应用程序](perform-access-review.md)
- [评审自己的访问权限，对组或应用程序](review-your-access.md)
- [完成访问评审的组或应用程序](complete-access-review.md)
