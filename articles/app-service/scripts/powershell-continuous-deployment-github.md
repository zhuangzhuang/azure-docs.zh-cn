---
title: Azure PowerShell 脚本示例 - 使用 GitHub 持续部署创建应用 | Microsoft Docs
description: Azure PowerShell 脚本示例 - 从 GitHub 使用连续部署创建 Web 应用
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: 42f901f8-02f7-4869-b22d-d99ef59f874c
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: d60fc1afbd129471ba56dddb6e2d5beba5ca2330
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "66136484"
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a>从 GitHub 使用连续部署创建 Web 应用

此示例脚本使用其相关资源，在应用服务中创建 Web 应用，并在 Git 存储库中设置连续部署。 有关不进行连续部署的 GitHub 部署，请参阅[从 GitHub 创建 Web 应用并部署代码](powershell-deploy-github.md)。

必要时，请使用 [Azure PowerShell 指南](/powershell/azure/overview)中的说明安装 Azure PowerShell，并运行 `Connect-AzAccount` 创建与 Azure 的连接。 同时，请确保：

- 已使用 `az login` 命令创建与 Azure 的连接。
- 应用程序代码在拥有的公共或专用 GitHub 存储库中。
- 已[在 GitHub 帐户中创建访问令牌](https://help.github.com/articles/creating-an-access-token-for-command-line-use/)。

## <a name="sample-script"></a>示例脚本

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/deploy-github-continuous/deploy-github-continuous.ps1?highlight=1-2 "Create a web app with continuous deployment from GitHub")]

## <a name="clean-up-deployment"></a>清理部署 

运行脚本示例后，可以使用以下命令删除资源组、Web 应用以及所有相关资源。

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) | 创建应用服务计划。 |
| [New-AzWebApp](/powershell/module/az.websites/new-azwebapp) | 创建 Web 应用。 |
| [Set-AzResource](/powershell/module/az.resources/set-azresource) | 修改资源组中的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](/powershell/azure/overview)。

可以在 [Azure PowerShell 示例](../samples-powershell.md)中找到 Azure 应用服务 Web 应用的其他 Azure Powershell 示例。
