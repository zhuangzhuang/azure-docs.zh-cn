---
title: 有关 Azure Kubernetes 服务 (AKS) 的常见问题解答
description: 查找有关 Azure Kubernetes 服务 (AKS) 的常见问题的解答。
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: iainfou
ms.openlocfilehash: 6bfcd11dd6bfd31583fb2d0cd3f4229d3dd70065
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/20/2019
ms.locfileid: "65887359"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>有关 Azure Kubernetes 服务 (AKS) 的常见问题解答

本文解答有关 Azure Kubernetes 服务 (AKS) 的常见问题。

## <a name="which-azure-regions-currently-provide-aks"></a>哪些 Azure 区域目前提供 AKS？

有关可用区域的完整列表，请参阅[AKS 区域和可用性][aks-regions]。

## <a name="does-aks-support-node-autoscaling"></a>AKS 是否支持节点自动缩放？

是的是可通过自动缩放[Kubernetes autoscaler] [ auto-scaler]截至 Kubernetes 1.10。 有关如何手动配置和使用群集自动缩放程序的信息，请参阅[AKS 群集自动缩放][aks-cluster-autoscale]。

您还可用于内置群集自动缩放程序 （目前以在 AKS 中的预览版） 管理的节点的缩放。 有关详细信息，请参阅[自动缩放以满足在 AKS 中的应用程序需求的群集][aks-cluster-autoscaler]。

## <a name="does-aks-support-kubernetes-rbac"></a>AKS 是否支持 Kubernetes RBAC？

是的 Kubernetes 基于角色的访问控制 (RBAC) 启用默认情况下使用 Azure CLI 创建群集时。 对于使用 Azure 门户或模板创建的群集，可以启用 RBAC。

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>是否可以将 AKS 部署到现有虚拟网络？

是的可以通过使用部署到现有虚拟网络的 AKS 群集[高级网络功能][aks-advanced-networking]。

## <a name="can-i-make-the-kubernetes-api-server-accessible-only-within-my-virtual-network"></a>可以使 Kubernetes API 服务器可访问仅在我的虚拟网络中？

现在不行。 Kubernetes API 服务器公开为公共完全限定的域名 (FQDN)。 通过控制对你的群集访问权限[Kubernetes RBAC 和 Azure Active Directory (Azure AD)][aks-rbac-aad]。

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>安全更新是否可应用于 AKS 代理节点？

Azure 会自动适用于按夜间计划在群集中的 Linux 节点的安全修补程序。 但是，您有责任确保重新启动节点作为这些 Linux 所需。 必须重新启动节点的多个选项：

- 通过 Azure 门户或 Azure CLI 手动执行。
- 通过升级 AKS 群集。 群集升级[cordon 和 drain 节点][ cordon-drain]自动，然后将新节点联机使用的最新的 Ubuntu 映像和新的修补程序版本或次的 Kubernetes 版本。 有关详细信息，请参阅[升级 AKS 群集][aks-upgrade]。
- 通过使用[Kured](https://github.com/weaveworks/kured)，一个适用于 Kubernetes 的开放源代码重启守护程序。 Kured 作为运行[DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)并监视每个节点的指示重新启动是必需的文件是否存在。 在群集范围内操作系统重新启动管理的相同[cordon 和 drain 过程][ cordon-drain]作为群集升级。

有关使用 kured 的详细信息，请参阅[将安全性和内核更新应用于 AKS 中的节点][node-updates-kured]。

### <a name="windows-server-nodes"></a>Windows 服务器节点

Windows Server 中的节点 （当前在 AKS 中的预览版），Windows 更新不会不会自动运行，并应用最新的更新。 有关在 Windows 更新发布周期和验证过程按定期计划，应在 AKS 群集中执行 Windows Server 节点池上的升级。 此升级过程创建节点运行的最新的 Windows Server 映像和修补程序，然后删除旧节点。 此过程的详细信息，请参阅[升级在 AKS 中的节点池][nodepool-upgrade]。

## <a name="why-are-two-resource-groups-created-with-aks"></a>为什么使用 AKS 创建两个资源组？

每个 AKS 部署都跨越两个资源组：

1. 创建第一个资源组。 此组仅包含 Kubernetes 服务资源。 AKS 资源提供程序在部署期间自动创建第二个资源组。 第二个资源组的一个示例是*于 MC_myResourceGroup_myAKSCluster_eastus*。 有关如何指定此第二个资源组的名称的信息，请参阅下一节。
1. 第二个资源组，如*于 MC_myResourceGroup_myAKSCluster_eastus*，包含所有与群集关联的基础结构资源。 这些资源包括 Kubernetes 节点 VM、虚拟网络和存储。 此资源组旨在简化资源清理。

如果您创建的 AKS 群集中使用的资源，例如存储帐户或保留公共 IP 地址，将它们放在自动生成的资源组中。

## <a name="can-i-provide-my-own-name-for-the-aks-infrastructure-resource-group"></a>我是否可为 AKS 基础结构资源组提供自己的名称？

是的。 默认情况下，AKS 资源提供程序会自动创建的辅助资源组 (如*于 MC_myResourceGroup_myAKSCluster_eastus*) 在部署过程。 为了符合企业策略，你可以为此托管群集 (*MC_*) 资源组提供自己的名称。

若要指定资源组名称，请安装 [aks-preview][aks-preview-cli] Azure CLI 扩展版本 *0.3.2* 或更高版本。 当使用创建 AKS 群集[az aks 创建][ az-aks-create]命令，使用 *-节点资源组*参数并指定资源组的名称。 如果您[使用 Azure 资源管理器模板][ aks-rm-template]若要部署 AKS 群集，可以定义的资源组名称使用*nodeResourceGroup*属性。

* 在自己的订阅中的 Azure 资源提供程序会自动创建的辅助资源组。
* 仅当要创建群集时，可以指定自定义资源组名称。

在使用*MC_* 资源组，请记住，您不能：

* 指定现有的资源组*MC_* 组。
* 指定不同的订阅*MC_* 资源组。
* 更改*MC_* 后创建群集资源组名称。
* 指定托管资源内的名称*MC_* 资源组。
* 修改或删除中的托管资源标记*MC_* 资源组。 （请参阅下一节中的其他信息。）

## <a name="can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-mc-resource-group"></a>可以修改标记和其他属性 MC_ 资源组中的 AKS 资源？

如果修改或删除 Azure 创建标记和其他资源属性*MC_* 资源组中，可能会收到意外的结果，例如缩放和升级错误。 AKS，可创建和修改自定义标记。 你可能想要创建或修改自定义标记，例如，若要将业务单元或成本中心分配。 通过修改下的资源*MC_* 在 AKS 群集中断服务级别目标 (SLO)。 有关详细信息，请参阅[没有 AKS 提供的服务级别协议？](#does-aks-offer-a-service-level-agreement)

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-admission-controllers-be-added-or-removed"></a>AKS 支持哪些 Kubernetes 许可控制器？ 是否可以添加或删除许可控制器？

AKS 支持以下[许可控制器][admission-controllers]：

- NamespaceLifecycle
- LimitRanger
- ServiceAccount
- DefaultStorageClass
- DefaultTolerationSeconds
- MutatingAdmissionWebhook
- ValidatingAdmissionWebhook
- ResourceQuota
- DenyEscalatingExec
- AlwaysPullImages

目前，不能修改在 AKS 中的许可控制器的列表。

## <a name="is-azure-key-vault-integrated-with-aks"></a>不是，它没有与 Azure Key Vault 集成。

AKS 没有当前以本机方式集成到 Azure 密钥保管库。 但是， [Kubernetes 项目的 Azure 密钥保管库 FlexVolume] [ keyvault-flexvolume]使你能够直接从 Kubernetes pod 到密钥保管库机密的集成。

## <a name="can-i-run-windows-server-containers-on-aks"></a>是否可以在 AKS 上运行 Windows Server 容器？

是的 Windows Server 容器是在预览版中可用。 若要在 AKS 中运行 Windows Server 容器，你创建运行 Windows Server 作为来宾 OS 的节点池。 Windows Server 容器可以使用仅 Windows Server 2019。 若要开始，请参阅[与 Windows Server 节点池创建 AKS 群集][aks-windows-cli]。

节点池的窗口服务器支持包括属于上游 Kubernetes 项目中的 Windows 服务器的一些限制。 有关这些限制的详细信息，请参阅[AKS 限制中的 Windows Server 容器][aks-windows-limitations]。

## <a name="does-aks-offer-a-service-level-agreement"></a>AKS 是否提供的服务级别协议？

在服务级别协议 (SLA)，该提供程序同意向补偿个服务的费用的客户，如果不符合已发布的服务级别。 AKS 是免费的因为无需付费是可用于向补偿，因此 AKS 会有没有正式的 SLA。 但是，AKS 致力于维护可用性至少达到 99.5%Kubernetes API 服务器。

## <a name="why-cant-i-set-maxpods-below-30"></a>为什么不能设置下面 30 maxPods？

在 AKS，你可以将`maxPods`值时使用 Azure CLI 和 Azure 资源管理器模板创建群集。 但是，Kubenet 和 Azure CNI 需要*最小值*（验证在创建时）：

| 网络 | 最小值 | 最大值 |
| -- | :--: | :--: |
| Azure CNI | 30 | 250 |
| Kubenet | 30 | 110 |

由于 AKS 是一种托管的服务，我们将部署和管理群集的一部分的外接程序和 pod。 在过去，用户可以定义`maxPods`托管的 pod 运行 （例如，30） 所需的值小于的值。 AKS 现在使用以下公式计算的最小 pod 数: ((maxPods 或 (maxPods * vm_count)) > 托管外接程序 pod 最小值。

用户无法重写所需的最低`maxPods`验证。

<!-- LINKS - internal -->

[aks-regions]: ./quotas-skus-regions.md#region-availability
[aks-upgrade]: ./upgrade-cluster.md
[aks-cluster-autoscale]: ./autoscaler.md
[virtual-kubelet]: virtual-kubelet.md
[aks-advanced-networking]: ./configure-azure-cni.md
[aks-rbac-aad]: ./azure-ad-integration.md
[node-updates-kured]: node-updates-kured.md
[aks-preview-cli]: /cli/azure/ext/aks-preview/aks
[az-aks-create]: /cli/azure/aks#az-aks-create
[aks-rm-template]: /rest/api/aks/managedclusters/createorupdate#managedcluster
[aks-cluster-autoscaler]: cluster-autoscaler.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[aks-windows-cli]: windows-container-cli.md
[aks-windows-limitations]: windows-node-limitations.md

<!-- LINKS - external -->

[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[keyvault-flexvolume]: https://github.com/Azure/kubernetes-keyvault-flexvol
