---
title: 常见问题
description: 提供有关 Azure VMware 解决方案的一些常见问题的解答。
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: dikamath
ms.openlocfilehash: cffa31bb66adfde2af24ab2542322479639ed9dd
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2020
ms.locfileid: "88752180"
---
# <a name="frequently-asked-questions-about-azure-vmware-solution-preview"></a>有关 Azure VMware 解决方案预览版的常见问题

有关 Azure VMware 解决方案的常见问题的解答。

## <a name="general"></a>常规

**什么是 Azure VMware 解决方案？**

随着企业采用 IT 现代化策略来提高业务敏捷性、降低成本和加速创新，混合云平台已成为客户数字化转型的关键推动力。 Azure VMware 解决方案将 VMware 的软件定义数据中心 (SDDC) 软件与 Microsoft Azure 全局云服务生态系统相结合。 Azure VMware 解决方案经过管理，以满足性能、可用性、安全性和合规性要求。

## <a name="azure-vmware-solution-service"></a>Azure VMware 解决方案服务

**Azure VMware 解决方案目前可用在何处？**

正在将该服务连续添加到新区域，因此请查看 [最新的服务可用性信息](https://azure.microsoft.com/global-infrastructure/services/?products=azure-vmware) 以了解更多详细信息。 

**Azure VMware 解决方案实例中运行的工作负荷是使用还是与 Azure 服务集成？**

所有 Azure 服务都将可供 Azure VMware 解决方案客户使用。 需要逐一解决特定服务的性能和可用性限制。

**我是否使用与现在相同的工具来管理私有云资源？**

是的。 Azure 门户用于部署和大量管理操作。 vCenter 和 NSX Manager 用于管理 vSphere 和 NSX-T 资源。

**可以使用我的本地 vCenter 来管理私有云吗？**

在启动时，Azure VMware 解决方案不支持跨本地和私有云环境的单一管理体验。 将通过私有云的本地 vCenter 和 NSX Manager 来管理私有云群集。

**可以使用在本地运行的 vRealize Suite 吗？** 

可以根据具体情况评估特定集成和用例。

**能否将 vSphere Vm 从本地环境迁移到 Azure VMware 解决方案私有云？**

是。 如果满足标准跨 vCenter [vMotion 要求](https://kb.vmware.com/s/article/210695) ，则可以使用 VM 迁移和 VMotion 将 vm 移到私有云。

**本地环境中是否需要特定版本的 vSphere？**

由于所有云环境都附带 HCX，因此应在本地环境中使用 vSphere 5.5 或更高版本才可使用 vMotion。

**变更控制过程是怎样的？**

对服务本身进行的更新将遵循 Microsoft Azure 的标准变更管理过程。 所有工作负载管理任务和关联的变更管理过程均由客户负责。

**这与 Azure VMware Solution by CloudSimple 有何不同？**

对于新的 Azure VMware 解决方案，Microsoft 与 VMware 具有云服务提供商直接合作伙伴关系。 新解决方案完全是由 Microsoft 设计、构建和支持的，并由 VMware 认可。 两种解决方案的体系结构是一致的，VMware 技术堆栈在 Azure 专用基础结构上运行。

**如果我已是 Azure VMware 解决方案客户，此预览版对我来说意味着什么？**

Azure VMware Solution by CloudSimple 没有任何更改。 我们继续支持 Azure 上的解决方案。 Azure VMware 解决方案由我们的服务级别协议 [(SLA)](https://aka.ms/CSVMwareSLA) 提供支持。 客户应继续将服务用于生产工作负载；这是一款受 [Microsoft 服务条款](https://azure.microsoft.com/support/legal/)约束的可用解决方案。

**可以从 Azure VMware Solution by CloudSimple 迁移到此新的解决方案吗？**

可以，Azure VMware 解决方案支持使用熟悉的 VMware 工具（如 HCX）进行迁移。 对于对迁移到新解决方案感兴趣的客户，请与你的 Microsoft 帐户团队合作来探索选项和可用支持。



## <a name="compute-network-and-storage"></a>计算、网络和存储

**是否有多种类型的主机可用？**

只有一种类型的主机可用。

**每种主机类型的 CPU 规格是什么？**

服务器具有双 18 核 2.3 GHz Intel CPU。

**每个主机中有多少内存？**

服务器的 RAM 为 576 GB。

**每个主机的存储容量是多少？**

每个 ESXi 主机都有两个 vSAN diskgroups，容量层为 15.2 TB，每个 diskgroup) 有 3.2 TB 个 NVMe 缓存层 (1.6 TB。

**每个 ESXi 主机中提供多少网络带宽？**

每个 ESXi 主机都是配置了 4 25 Gbps Nic 的 Azure VMware 解决方案，为 ESXi 系统流量预配了两个 Nic，为工作负荷流量预配了两个 nic。 

**VSAN 数据存储上存储的数据是否静态加密？**

是的，默认情况下，使用存储在 Azure Key Vault 中的密钥加密所有 vSAN 数据。

## <a name="hosts-clusters-and-private-clouds"></a>主机、群集和私有云

**是否共享了底层基础结构？**

没有，私有云主机和群集是专用的，在使用前后都会安全擦除。

**每个群集的最小和最大主机数是多少？**

群集可以在 3 到 16 个 ESXi 主机之间缩放。 试用群集限制为 3 个主机。

**可以缩放私有云群集吗？**

可以，群集可在最小和最大 ESXi 主机数之间缩放。 试用群集限制为 3 个主机。

**什么是试用群集？**

试用群集是三个主机群集，用于一个月评估 Azure VMware 解决方案私有云。

**可以为试用群集使用高端主机吗？**

不是。 高端 ESXi 主机保留用于生产群集。

## <a name="azure-vmware-solution-and-vmware-software"></a>Azure VMware 解决方案和 VMware 软件

**私有云中使用的是哪些版本的 VMware 软件？**

私有云使用 vSphere 6.7、vSAN 6.7、HCX 和 2.5 版本的 NSX-T。  

**私有云是否使用 VMware NSX？**

是的，NSX-T 2.5 用于 Azure VMware 解决方案私有云中软件定义的网络。

**可以在私有云中使用 VMware NSX-V 吗？**

不是。 NSX-T 是唯一支持的 NSX 版本。

**本地环境或连接到私有云的网络中是否需要 NSX？**

不需要，不需要在本地使用 NSX。

**在私有云中，VMware 软件的升级和更新计划是什么？**

私有云软件捆绑升级的目的是将软件保存到 VMware 的软件捆绑版本的最新版本中。 私有云软件版本可能不同于各个软件组件的最新版本， (ESXi，，NSX，vCenter，vSAN) 。

**私有云软件堆栈的更新频率是多少？**

私有云软件按计划升级，该计划将跟踪 VMware 发布的软件捆绑包。 私有云无需停机即可升级。

## <a name="connectivity"></a>连接

**将私有云与本地环境合并需要什么网络 IP 地址计划？**

部署 Azure VMware 解决方案私有云需要专用网络/22 地址空间。 此专用地址空间不应与订阅中的其他虚拟网络或本地网络重叠。
 
**如何实现从本地环境连接到 Azure VMware 解决方案私有云？**

可以通过以下两种方法之一连接到服务： 

- 使用 Azure 虚拟网络上部署的 VM 或应用程序网关，通过 ExpressRoute 对等互连到私有云。
- 通过 ExpressRoute Global Reach 从本地数据中心连接到 Azure ExpressRoute 线路。

**如何将工作负载 VM 连接到 Internet 或 Azure 服务终结点？**

在 Azure 门户中，为私有云启用 Internet 连接。 使用 NSX-T 管理器，创建一个 NSX-T T1 路由器和逻辑交换机。 然后，使用 vCenter 在逻辑交换机定义的网络段上部署 VM。 该 VM 将具有对 Internet 和 Azure 服务的网络访问权限。

**是否需要限制从 Internet 访问私有云中逻辑网络上的 VM？**

不是。 不允许直接将网络流量从 Internet 传入私有云。

**是否需要限制从逻辑网络上的 VM 到 Internet 的 Internet 访问？**

是的。 需要使用 NSX-T 管理器创建防火墙，限制 VM 对 Internet 的访问。

## <a name="accounts-and-privileges"></a>帐户和特权

**我的新的 Azure VMware 解决方案私有云会获得哪些帐户和特权？**

你可获得 vCenter 中 cloudadmin 用户的凭据和 NSX-T 管理器的管理员访问权限。 还可通过一个 CloudAdmin 组合并 Azure Active Directory。 有关详细信息，请参阅[访问权限和标识的概念](concepts-identity.md)。

**是否可以拥有 ESXi 主机的管理员访问权限？**

不可以，对 ESXi 的管理员访问权限受到限制，以满足解决方案的安全要求。

**我可在 vCenter 中拥有哪些特权和权限？**

你将拥有 CloudAdmin 组特权。 有关详细信息，请参阅[访问权限和标识的概念](concepts-identity.md)。

**我可以拥有对 NSX-T 管理器的哪些特权和权限？**

你可拥有 NSX 的完全管理员特权，并且可以管理基于角色的访问控制，就像在本地 NSX-T 数据中心一样。 有关详细信息，请参阅[访问权限和标识的概念](concepts-identity.md)。

> [!NOTE]
> 创建了 T0 路由器，并将其配置为私有云部署的一部分。 对该逻辑路由器或 NSX-T 边缘节点 VM 的任何修改都可能会影响与私有云的连接。

## <a name="billing-and-support"></a>计费和支持

**Azure VMware 解决方案预览期间将如何计费**

预览期间的 Azure VMware 解决方案的计费是按现用现付的方式。 在正式发布后，将提供其他选项。

**在 Azure VMware 解决方案的预览过程中，定价如何构造？**

有关定价的一般问题，请参阅 Azure VMware 解决方案[定价](https://azure.microsoft.com/pricing/details/azure-vmware)页面。 预览版定价适用于请求，请联系你的帐户团队，或单击定价页面上的链接以联系销售人员。

**谁支持 Azure VMware 解决方案？**

Microsoft 提供对 Azure VMware 解决方案的支持。 请注意，根据我们的预览准则，我们将在周一至周五的9到 5 pm PST 营业时间内提供支持。 可通过[此链接](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)提交支持票证

**需要哪些帐户才能创建 Azure VMware 解决方案私有云？**

需要 Azure 订阅中的 Azure 帐户。

<a name="how-to-request-a-quota-increase-for-avs"></a>**如何实现请求 Azure VMware 解决方案的主机配额增加？**

随时都可通过[提交支持请求](..\azure-portal\supportability\how-to-create-azure-support-request.md)来请求增加配额。 配额管理团队会在三个工作日内评估和批准请求。  

> [!IMPORTANT]
> 在请求增加配额之前，请确保在 Azure 门户中注册 Microsoft AVS 资源提供程序。  
> ```azurecli-interactive
> az provider register -n Microsoft.AVS --subscription <your subscription ID>
> ```
> 有关注册资源提供程序的其他方式，请参阅 [Azure 资源提供程序和类型](../azure-resource-manager/management/resource-providers-and-types.md)。

1. 在 Azure 门户中的“帮助 + 支持”下，创建“新支持请求”并为票证提供以下信息 ：
   - **问题类型：** 技术方面
   - **订阅：** 订阅 ID
   - **服务：** Azure VMware 解决方案 
   - **摘要：** 配额增加
   - **问题类型：** 容量管理问题
   - **问题子类型：** 客户请求额外的主机配额/容量

1. 在“详细信息”选项卡上，在支持票证的描述中提供：
   - 附加节点数   
   - 节点 SKU
   - 区域

   > [!NOTE] 
   > 默认情况下，将授予至少四个节点。

1. 单击“预览 + 创建”提交请求。

<!-- LINKS - external -->
[kb2106952]: https://kb.vmware.com/s/article/2106952

<!-- LINKS - internal -->
[Access and Identity Concepts]: concepts-identity.md
