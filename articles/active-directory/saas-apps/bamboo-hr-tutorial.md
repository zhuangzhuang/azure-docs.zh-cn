---
title: 教程：Azure Active Directory 与 BambooHR 集成 | Microsoft 文档
description: 了解如何在 Azure Active Directory 和 BambooHR 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: f826b5d2-9c64-47df-bbbf-0adf9eb0fa71
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9a054274da0c83fa5f079e05a1c648c1fc693fd7
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/20/2019
ms.locfileid: "65900328"
---
# <a name="tutorial-azure-active-directory-integration-with-bamboohr"></a>教程：Azure Active Directory 与 BambooHR 集成

在本教程中，了解如何将 BambooHR 与 Azure Active Directory (Azure AD) 进行集成。
将 BambooHR 与 Azure AD 集成可提供以下优势：

* 可在 Azure AD 中控制谁有权限访问 BambooHR。
* 可让用户使用其 Azure AD 帐户自动登录到 BambooHR（单一登录）。
* 可在中心位置（即 Azure 门户）管理帐户。

如果要了解有关 SaaS 应用与 Azure AD 集成的更多详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。
如果还没有 Azure 订阅，可以在开始前[创建一个免费帐户](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 BambooHR 的集成，需要以下项：

* 一个 Azure AD 订阅。 如果你没有 Azure AD 环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。
* 已启用 BambooHR 单一登录的订阅

## <a name="scenario-description"></a>方案描述

本教程会在测试环境中配置和测试 Azure AD 单一登录。

* BambooHR 支持“SP”发起的 SSO

## <a name="adding-bamboohr-from-the-gallery"></a>从库中添加 BambooHR

若要配置 BambooHR 与 Azure AD 的集成，需要从库中将 BambooHR 添加到托管 SaaS 应用列表。

若要从库中添加 BambooHR，请执行以下步骤：

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。

    ![“Azure Active Directory”按钮](common/select-azuread.png)

2. 转到“企业应用”，并选择“所有应用”选项。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

3. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮](common/add-new-app.png)

4. 在搜索框中，键入“BambooHR”，在结果面板中选择“BambooHR”，然后单击“添加”按钮添加该应用程序。

     ![结果列表中的 BambooHR](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，将基于名为“Britta Simon”的测试用户配置和测试 BambooHR 的 Azure AD 单一登录。
若要运行单一登录，需要在 Azure AD 用户与 BambooHR 相关用户之间建立链接关系。

若要配置和测试 BambooHR 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
2. **[配置 BambooHR 单一登录](#configure-bamboohr-single-sign-on)** - 在应用程序端配置单一登录。
3. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
4. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
5. [创建 BambooHR 测试用户](#create-bamboohr-test-user) - 在 BambooHR 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
6. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录。

若要配置 BambooHR 的 Azure AD 单一登录，请执行以下步骤：

1. 在 [Azure 门户](https://portal.azure.com/)的“BambooHR”应用程序集成页上，选择“单一登录”。

    ![配置单一登录链接](common/select-sso.png)

2. 在**选择单一登录方法**对话框中，选择 **SAML/WS-Fed**模式以启用单一登录。

    ![单一登录选择模式](common/select-saml-option.png)

3. 在“使用 SAML 设置单一登录”页上，单击“编辑”图标以打开“基本 SAML 配置”对话框。

    ![编辑基本 SAML 配置](common/edit-urls.png)

4. 在“基本 SAML 配置”部分中，按照以下步骤操作：

    ![BambooHR 域和 URL 单一登录信息](common/sp-identifier.png)

    a. 在“登录 URL”文本框中，使用以下模式键入 URL：`https://<company>.bamboohr.com`

    b. 在“标识符(实体 ID)”文本框中，使用以下模式键入 URL：`BambooHR-SAML`

    > [!NOTE]
    > “登录 URL”值不是实际值。 请使用实际登录 URL 更新此值。 若要获取此值，请与 [BambooHR 客户端支持团队](https://www.bamboohr.com/contact.php)联系。 还可以参考 Azure 门户中的“基本 SAML 配置”部分中显示的模式。

4. 在“使用 SAML 设置单一登录”页上，在“SAML 签名证书”部分中，单击“下载”以根据要求从给定的选项下载**证书(Base64)** 并将其保存在计算机上。

    ![证书下载链接](common/certificatebase64.png)

6. 在“设置 BambooHR”部分，根据要求复制相应 URL。

    ![复制配置 URL](common/copy-configuration-urls.png)

    a. 登录 URL

    b. Azure AD 标识符

    c. 注销 URL

### <a name="configure-bamboohr-single-sign-on"></a>配置 BambooHR 单一登录

1. 在一个新窗口中，以管理员身份登录到 BambooHR 公司站点。

2. 在主页上执行以下操作：
   
    ![BambooHR 单一登录页面](./media/bamboo-hr-tutorial/ic796691.png "单一登录")   

    a. 选择“应用”。
   
    b. 在“应用”窗格中，选择“单一登录”。
   
    c. 选择“SAML 单一登录”。

3. 在“SAML 单一登录”窗格中，执行以下操作：
   
    ![“SAML 单一登录”窗格](./media/bamboo-hr-tutorial/IC796692.png "SAML 单一登录")
   
    a. 在“SSO 登录 URL”框中，粘贴在步骤 6 中从 Azure 门户复制的“登录 URL”。
      
    b. 在记事本中，打开从 Azure 门户下载的 base-64 编码证书，复制其内容，然后将其粘贴到“X.509 证书”框中。
   
    c. 选择“保存”。

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户 

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”。

    ![“用户和组”以及“所有用户”链接](common/users.png)

2. 选择屏幕顶部的“新建用户”。

    ![“新建用户”按钮](common/new-user.png)

3. 在“用户属性”中，按照以下步骤操作。

    ![“用户”对话框](common/user-properties.png)

    a. 在“名称”字段中，输入 BrittaSimon。
  
    b. 在“用户名”字段中，键入 brittasimon\@yourcompanydomain.extension  
    例如： BrittaSimon@contoso.com

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 单击“创建”。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 BambooHR 的权限，以支持其使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”和“BambooHR”。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

2. 在应用程序列表中，键入并选择“BambooHR”。

    ![应用程序列表中的 BambooHR 链接](common/all-applications.png)

3. 在左侧菜单中，选择“用户和组”。

    ![“用户和组”链接](common/users-groups-blade.png)

4. 单击“添加用户”按钮，然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格](common/add-assign-user.png)

5. 在“用户和组”对话框中，选择“用户”列表中的 Britta Simon，然后单击屏幕底部的“选择”按钮。

6. 如果你在 SAML 断言中需要任何角色值，请在“选择角色”对话框中从列表中为用户选择合适的角色，然后单击屏幕底部的“选择”按钮。

7. 在“添加分配”对话框中，单击“分配”按钮。

### <a name="create-bamboohr-test-user"></a>创建 BambooHR 测试用户

若要使 Azure AD 用户能够登录到 BambooHR，请执行以下操作，在 BambooHR 中手动设置它们：

1. 以管理员身份登录到 **BambooHR** 站点。

2. 在顶部的工具栏中，选择“设置”。
   
    ![“设置”按钮](./media/bamboo-hr-tutorial/IC796694.png "设置")

3. 选择“概述”。

4. 在左窗格中，选择“安全” > “用户”。

5. 键入要设置的有效 Azure AD 帐户的用户名、密码和电子邮件地址。

6. 选择“保存”。
        
>[!NOTE]
>若要设置 Azure AD 用户帐户，还可以使用 BambooHR 用户帐户创建工具或 API。

### <a name="test-single-sign-on"></a>测试单一登录 

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的 BambooHR 磁贴时，应会自动登录到为其设置了 SSO 的 BambooHR。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

- [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory 的应用程序访问与单一登录是什么？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什么是 Azure Active Directory 中的条件访问？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

