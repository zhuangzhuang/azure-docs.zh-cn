---
title: 教程 - 使用 Azure PowerShell 创建 Azure DNS 专用区域
description: 在本教程中，将在 Azure DNS 中创建并测试专用 DNS 区域和记录。 这是有关使用 Azure PowerShell 创建和管理第一个专用 DNS 区域和记录的分步指南。
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 3/11/2019
ms.author: victorh
ms.openlocfilehash: b4d75c7a6db89b19d88cddcc564fd4e6a9ad0f49
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "65916868"
---
# <a name="tutorial-create-an-azure-dns-private-zone-using-azure-powershell"></a>教程：使用 Azure PowerShell 创建 Azure DNS 专用区域

本教程将引导你完成使用 Azure PowerShell 创建你的第一个专用 DNS 区域和记录的步骤。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

DNS 区域用来托管某个特定域的 DNS 记录。 若要开始在 Azure DNS 中托管域，需要为该域名创建 DNS 区域。 随后会在此 DNS 区域内为每个 DNS 记录创建域。 若要向虚拟网络发布专用 DNS 区域，请指定一个列表，其中包含允许在区域中解析记录的虚拟网络。  这些虚拟网络称为“解析虚拟网络”。 也可指定一个虚拟网络，让 Azure DNS 在创建 VM、更改 IP 或删除 VM 时为其保留主机名记录。  这称为“注册虚拟网络”。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 创建 DNS 专用区域
> * 创建测试虚拟机
> * 创建额外的 DNS 记录
> * 测试专用区域

如果还没有 Azure 订阅，可以在开始前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

如果需要，也可以使用 [Azure CLI](private-dns-getstarted-cli.md) 完成本教程中的步骤。

<!--- ## Get the Preview PowerShell modules
These instructions assume you have already installed and signed in to Azure PowerShell, including ensuring you have the required modules for the Private Zone feature. -->

<!---[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)] -->

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

## <a name="create-the-resource-group"></a>创建资源组

首先，创建一个资源组，使之包含 DNS 区域： 

```azurepowershell
New-AzResourceGroup -name MyAzureResourceGroup -location "eastus"
```

## <a name="create-a-dns-private-zone"></a>创建 DNS 专用区域

结合 **ZoneType** 参数的 *Private* 值使用 `New-AzDnsZone` cmdlet 创建 DNS 区域。 以下示例在名为 **MyAzureResourceGroup** 的资源组中创建名为 **private.contoso.com** 的 DNS 区域，并将 DNS 区域提供给名为 **MyAzureVnet** 的虚拟网络。

如果省略 **ZoneType** 参数，则会将区域创建为公共区域。因此，创建专用区域时必须使用此参数。 

```azurepowershell
$backendSubnet = New-AzVirtualNetworkSubnetConfig -Name backendSubnet -AddressPrefix "10.2.0.0/24"
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName MyAzureResourceGroup `
  -Location eastus `
  -Name myAzureVNet `
  -AddressPrefix 10.2.0.0/16 `
  -Subnet $backendSubnet

New-AzDnsZone -Name private.contoso.com -ResourceGroupName MyAzureResourceGroup `
   -ZoneType Private `
   -RegistrationVirtualNetworkId @($vnet.Id)
```

如果希望创建仅用于名称解析的区域（不创建自动主机名），则可使用 *ResolutionVirtualNetworkId* 参数而非 *RegistrationVirtualNetworkId* 参数。

> [!NOTE]
> 你将无法看到自动创建的主机名记录。 但稍后你将进行测试来确保它们存在。

### <a name="list-dns-private-zones"></a>列出 Azure DNS 专用区域

通过省略 `Get-AzDnsZone` 中的区域名称，可以枚举资源组中的所有区域。 此操作将返回区域对象的数组。

```azurepowershell
Get-AzDnsZone -ResourceGroupName MyAzureResourceGroup
```

通过省略 `Get-AzDnsZone` 的区域名和资源组名，可枚举 Azure 订阅中的所有区域。

```azurepowershell
Get-AzDnsZone
```

## <a name="create-the-test-virtual-machines"></a>创建测试虚拟机

现在，创建两台虚拟机，以便可以测试专用 DNS 区域：

```azurepowershell
New-AzVm `
    -ResourceGroupName "myAzureResourceGroup" `
    -Name "myVM01" `
    -Location "East US" `
    -subnetname backendSubnet `
    -VirtualNetworkName "myAzureVnet" `
    -addressprefix 10.2.0.0/24 `
    -OpenPorts 3389

New-AzVm `
    -ResourceGroupName "myAzureResourceGroup" `
    -Name "myVM02" `
    -Location "East US" `
    -subnetname backendSubnet `
    -VirtualNetworkName "myAzureVnet" `
    -addressprefix 10.2.0.0/24 `
    -OpenPorts 3389
```

完成此设置可能需要几分钟时间。

## <a name="create-an-additional-dns-record"></a>创建额外的 DNS 记录

可以使用 `New-AzDnsRecordSet` cmdlet 创建记录集。 下面的示例在 DNS 区域 **private.contoso.com** 的资源组 **MyAzureResourceGroup** 中创建相对名称为 **db** 的一个记录。 记录集的完全限定名称为 **db.private.contoso.com**。 记录类型为“A”，IP 地址为“10.2.0.4”，TTL 为 3600 秒。

```azurepowershell
New-AzDnsRecordSet -Name db -RecordType A -ZoneName private.contoso.com `
   -ResourceGroupName MyAzureResourceGroup -Ttl 3600 `
   -DnsRecords (New-AzDnsRecordConfig -IPv4Address "10.2.0.4")
```

### <a name="view-dns-records"></a>查看 DNS 记录

若要列出区域中的 DNS 记录，请运行：

```azurepowershell
Get-AzDnsRecordSet -ZoneName private.contoso.com -ResourceGroupName MyAzureResourceGroup
```
请记住，你将不会看到为两台测试虚拟机自动创建的 A 记录。

## <a name="test-the-private-zone"></a>测试专用区域

现在，可以测试 **private.contoso.com** 专用区域的名称解析。

### <a name="configure-vms-to-allow-inbound-icmp"></a>将 VM 配置为允许入站 ICMP

可以使用 ping 命令来测试名称解析。 因此，在两台虚拟机上都将防火墙配置为允许入站 ICMP 数据包。

1. 连接到 myVM01，使用管理员权限打开 Windows PowerShell 窗口。
2. 运行以下命令：

   ```powershell
   New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
   ```

针对 myVM02 重复上述操作。

### <a name="ping-the-vms-by-name"></a>按名称对 VM 执行 ping 命令

1. 从 myVM02 Windows PowerShell 命令提示符下，使用自动注册的主机名对 myVM01 执行 ping 命令：
   ```
   ping myVM01.private.contoso.com
   ```
   应当会看到与以下内容类似的输出：
   ```
   PS C:\> ping myvm01.private.contoso.com

   Pinging myvm01.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time=1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 1ms, Average = 0ms
   PS C:\>
   ```
2. 现在，对之前创建的 **db** 名称执行 ping 命令：
   ```
   ping db.private.contoso.com
   ```
   应当会看到与以下内容类似的输出：
   ```
   PS C:\> ping db.private.contoso.com

   Pinging db.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 0ms, Average = 0ms
   PS C:\>
   ```

## <a name="delete-all-resources"></a>删除所有资源

不再需要时，可以通过删除 **MyAzureResourceGroup** 资源组来删除在本教程中创建的资源。

```azurepowershell
Remove-AzResourceGroup -Name MyAzureResourceGroup
```

## <a name="next-steps"></a>后续步骤

在本教程中，你部署了一个专用 DNS 区域，创建了一条 DNS 记录，并测试了该区域。
接下来，可以详细了解专用 DNS 区域。

> [!div class="nextstepaction"]
> [将 Azure DNS 用于专用域](private-dns-overview.md)




