---
title: Azure PowerShell 脚本示例 - 将应用程序证书添加到群集 | Microsoft Docs
description: Azure PowerShell 脚本示例 - 将应用程序证书添加到 Service Fabric 群集。
services: service-fabric
documentationcenter: ''
author: aljo-microsoft
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/18/2018
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 313a1b7e4f0109a0a5cb94214918d97c25b5cf5d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "66153851"
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>将应用程序证书添加到 Service Fabric 群集

此示例脚本在指定的 Azure Key Vault 中创建一个自签名证书，并将它安装到 Service Fabric 群集的所有节点上。 该证书还会下载到本地文件夹。 已下载证书的名称与 Key Vault 中的证书的名称相同。 根据需要自定义参数。

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

必要时，请使用 [Azure PowerShell 指南](/powershell/azure/overview)中的说明安装 Azure PowerShell，并运行 `Connect-AzAccount` 创建与 Azure 的连接。 

## <a name="sample-script"></a>示例脚本

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "Add an application certificate to a cluster")]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令：表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [Add-AzServiceFabricApplicationCertificate](/powershell/module/az.servicefabric/Add-azServiceFabricApplicationCertificate) | 将新的应用程序证书添加到构成群集的虚拟机规模集。  |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](/powershell/azure/overview)。

可以在 [Azure PowerShell 示例](../service-fabric-powershell-samples.md)中找到 Azure Service Fabric 的其他 Azure Powershell 示例。
