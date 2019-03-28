---
title: ASC IoT 预览版的身份验证方法 |Microsoft Docs
description: 使用 ASC 的 IoT 服务时，了解有关可用的不同的身份验证方法。
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 10b38f20-b755-48cc-8a88-69828c17a108
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 23bc4d0df1c8124ec225ac31239c7acb3f1ab546
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541807"
---
# <a name="security-agent-authentication-methods"></a>安全代理身份验证方法 

> [!IMPORTANT]
> Iot ASC 目前处于公共预览状态。
> 此预览版本没有附带服务级别协议提供，不建议用于生产工作负荷。 某些功能可能不受支持或者受限。 有关详细信息，请参阅 [Microsoft Azure 预览版补充使用条款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

本文介绍可用于向 AzureIoTSecurity 代理使用 IoT 中心进行身份验证的不同身份验证方法。

对于每个设备登记到 ASC iot 在 IoT 中心，安全模块需要。 若要对设备进行身份验证，iot ASC 可以使用两种方法之一。 选择最适合于您现有的 IoT 解决方案的方法。 

> [!div class="checklist"]
> * 安全模块选项
> * 设备选项

## <a name="authentication-methods"></a>身份验证方法

AzureIoTSecurity 代理来执行身份验证这两种方法：

 - **模块**身份验证模式<br>
   独立于设备孪生，该模块进行身份验证。
   此类型的身份验证由 Authentication.config 文件中定义所需的信息C#和 c。 LocalConfiguration.json
        
 - **设备**身份验证模式<br>
    在此方法中，安全代理首先进行身份验证针对该设备。 初始身份验证后，ASC 为 IoT 代理执行**Rest**到 IoT 中心使用设备的身份验证数据的 Rest API 调用。 ASC 为 IoT 代理然后请求安全模块身份验证方法和数据从 IoT 中心。 在最后一步中，IoT 代理 ASC IoT 模块执行针对 ASC 的身份验证。    

请参阅[安全代理的安装参数](#security-agent-installation-parameters)若要了解如何配置。
                                
## <a name="authentication-methods-known-limitations"></a>已知限制的身份验证方法

- **模块**身份验证模式仅支持对称密钥身份验证。
- CA 签名的证书不受**设备**身份验证模式。  

## <a name="security-agent-installation-parameters"></a>安全代理的安装参数

当[部署安全代理](select-deploy-agent.md)，必须作为参数提供身份验证详细信息。
下表中记录了这些参数。


|参数|描述|选项|
|---------|---------------|---------------|
|**identity**|身份验证模式| **模块**或**设备**|
|**类型**|身份验证类型|**SymmetricKey**或**SelfSignedCertificate**|
|**filePath**|包含的证书或对称密钥的文件的绝对完整路径| |
|**gatewayHostname**|IoT 中心的 FQDN|示例：ContosoIotHub.azure-devices.net|
|**deviceId**|设备 ID|示例：MyDevice1|
|**certificateLocationKind**|证书存储位置|**本地文件**或**应用商店**|


使用安装安全代理脚本时，自动执行下面的配置。

若要手动编辑安全代理身份验证，请编辑配置文件。 

## <a name="change-authentication-method-after-deployment"></a>在部署后更改身份验证方法

在部署与安装脚本的安全代理时，会自动创建配置文件。

若要在部署后更改身份验证方法，手动编辑配置文件是必需的。


### <a name="c-based-security-agent"></a>C#-基于安全代理

编辑_Authentication.config_使用以下参数：

```xml
<Authentication>
  <add key="deviceId" value=""/>
  <add key="gatewayHostname" value=""/>
  <add key="filePath" value=""/>
  <add key="type" value=""/>
  <add key="identity" value=""/>
  <add key="certificateLocationKind" value="" />
</Authentication>
```

### <a name="c-based-security-agent"></a>基于 C 的安全代理

编辑_LocalConfiguration.json_使用以下参数：

```json
"Authentication" : {
    "Identity" : "",
    "AuthenticationMethod" : "",
    "FilePath" : "",
    "DeviceId" : "",
    "HostName" : ""
}
```

## <a name="see-also"></a>另请参阅
- [安全代理概述](security-agent-architecture.md)
- [部署安全代理](select-deploy-agent.md)
- [访问原始安全数据](how-to-security-data-access.md)