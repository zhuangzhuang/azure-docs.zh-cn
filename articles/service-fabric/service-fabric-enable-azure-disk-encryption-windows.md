---
title: 适用于 Azure Service Fabric Windows 群集启用磁盘加密 |Microsoft Docs
description: 本文介绍如何使用 Azure 资源管理器 Azure Key Vault 为 Azure 中的 Service Fabric 群集节点启用磁盘加密。
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: navya
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/22/2019
ms.author: aljo
ms.openlocfilehash: 2e9c41409c1f528947e3bef281e9a3c34da39e9b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "66119159"
---
# <a name="enable-disk-encryption-for-service-fabric-windows-cluster-nodes"></a>为 Service Fabric Windows 群集节点启用磁盘加密 
> [!div class="op_single_selector"]
> * [适用于 Windows 的磁盘加密](service-fabric-enable-azure-disk-encryption-windows.md)
> * [适用于 Linux 的磁盘加密](service-fabric-enable-azure-disk-encryption-linux.md)
>
>

按照以下步骤在 Service Fabric Windows 群集节点上启用磁盘加密。 将需要为每个节点类型/虚拟机规模集执行这些步骤。 为了对节点加密，我们将利用虚拟机规模集上的 Azure 磁盘加密功能。

该指南包含下列步骤：

* 在 Service Fabric Windows 群集虚拟机规模集上启用磁盘加密时需要注意的关键概念。
* 在 Service Fabric Windows 群集虚拟机规模集上启用磁盘加密之前必须遵循的先决条件步骤。
* 在 Service Fabric Windows 群集虚拟机规模集上启用磁盘加密时要遵循的步骤。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>必备组件
* **自行注册**：若要使用虚拟机规模集磁盘加密预览版，需自行注册
* 可通过运行以下步骤自行注册订阅： 
```powershell
Register-AzProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```
* 等待大约 10 分钟，直到状态为“已注册”。 可通过运行以下命令来检查状态： 
```powershell
Get-AzProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
```
* **Azure Key Vault**：在与规模集相同的订阅和区域中创建 KeyVault，并在 KeyVault 上使用其 PS cmdlet 设置访问策略“EnabledForDiskEncryption”。 也可以使用 Azure 门户中的 KeyVault UI 设置该策略： 
```powershell
Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
```
* 安装最新[Azure CLI](/cli/azure/install-azure-cli) ，其中包含新的加密命令。
* 从 [Azure PowerShell](https://github.com/Azure/azure-powershell/releases) 发行版安装最新版本的 Azure SDK。 以下是在规模集实例上启用 ([Set](/powershell/module/az.compute/set-azvmssdiskencryptionextension)) 加密、检索 ([Get](/powershell/module/az.compute/get-azvmssvmdiskencryption)) 加密状态以及删除 ([disable](/powershell/module/az.compute/disable-azvmssdiskencryption)) 加密所需的虚拟机规模集 ADE cmdlet。

| 命令 | Version |  源  |
| ------------- |-------------| ------------|
| Get-AzVmssDiskEncryptionStatus   | 1.0.0 或更高版本 | Az.Compute |
| Get-AzVmssVMDiskEncryptionStatus   | 1.0.0 或更高版本 | Az.Compute |
| Disable-AzVmssDiskEncryption   | 1.0.0 或更高版本 | Az.Compute |
| Get-AzVmssDiskEncryption   | 1.0.0 或更高版本 | Az.Compute |
| Get-AzVmssVMDiskEncryption   | 1.0.0 或更高版本 | Az.Compute |
| Set-AzVmssDiskEncryptionExtension   | 1.0.0 或更高版本 | Az.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>受支持的磁盘加密方案
* 只有使用托管磁盘创建的规模集才支持虚拟机规模集加密，本机（或非托管）磁盘规模集则不支持。
* 虚拟机规模集加密支持的 Windows 虚拟机规模集的 OS 和数据卷。 Windows 规模集的 OS 和数据卷支持禁用加密。
* 虚拟机规模集 VM 重置映像和当前预览版中不支持的升级操作。


### <a name="create-new-cluster-and-enable-disk-encryption"></a>新建群集并启用磁盘加密

使用以下命令创建群集并启用磁盘加密使用 Azure 资源管理器模板和自签名的证书。

### <a name="sign-in-to-azure"></a>登录 Azure 

```powershell
Login-AzAccount
Set-AzContext -SubscriptionId <guid>

```

```azurecli

azure login
az account set --subscription $subscriptionId

```

#### <a name="use-the-custom-template-that-you-already-have"></a>使用现有的自定义模板 

如果需要根据需要编写自定义模板，我们强烈建议从 [Azure Service Fabric 模板示例](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master)中提供的模板之一着手。 请遵照下面[自定义群集模板][customize-your-cluster-template]部分中的指导和说明。

如果已有一个自定义模板，请确保模板中的所有三个证书相关参数以及参数文件已按如下所示命名并使用 null 值。

```Json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```


```powershell
$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\templates\templateparam.json"
$templateFilePath="c:\templates\template.json"


New-AzServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

```


```azurecli

declare certPassword=""
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare certSubjectName="mylinuxsecure.westus.cloudapp.azure.com"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"
declare certOutputFolder="c:\certificates"


az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certPassword  \
    --certificate-subject-name $certSubjectName \
    --template-file $templateFilePath --parameter-file $parametersFilePath

```

#### <a name="deploy-application-to-windows-service-fabric-cluster"></a>将应用程序部署到 Windows Service Fabric 群集
遵照相关步骤和指南[将应用程序部署到群集](service-fabric-deploy-remove-applications.md)


#### <a name="enable-disk-encryption-for-service-fabric-cluster-virtual-machine-scale-set-created-above"></a>为上面创建的 Service Fabric 群集虚拟机规模集启用磁盘加密
 
```powershell

$VmssName = "nt1vm"
$vaultName = "mykeyvault"
$resourceGroupName = "mycluster"
$KeyVault = Get-AzKeyVault -VaultName $vaultName -ResourceGroupName $rgName
$DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
$KeyVaultResourceId = $KeyVault.ResourceId

Set-AzVmssDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType All

```

```azurecli

az vmss encryption enable -g <resourceGroupName> -n <VMSS name> --disk-encryption-keyvault <KeyVaultResourceId>

```


#### <a name="validate-if-disk-encryption-enabled-for-windows-virtual-machine-scale-set"></a>验证是否为 Windows 虚拟机规模集启用了磁盘加密。
获取整个虚拟机规模集或规模集中任意实例的状态。 请参阅下面的命令。
此外用户可以登录到 VM 规模集中，并确保驱动器加密

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```azurecli

az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```


#### <a name="disable-disk-encryption-for-service-fabric-cluster-virtual-machine-scale-set"></a>为 Service Fabric 群集虚拟机规模集禁用磁盘加密 
禁用磁盘加密适用于整个虚拟机规模集，而不适用于单个实例 

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```CLI

az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>后续步骤
此时，通过了解如何为 Service Fabric 群集虚拟机规模集启用/禁用磁盘加密，你已经拥有一个安全的群集。 接下来，[适用于 Linux 的磁盘加密](service-fabric-enable-azure-disk-encryption-linux.md) 

[customize-your-cluster-template]: https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure#creating-a-custom-arm-template
