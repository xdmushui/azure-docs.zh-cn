---
title: Azure 防火墙策略中的 IP 组
description: IP 组允许对 Azure 防火墙规则的 IP 地址进行分组和管理。
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: conceptual
ms.date: 07/30/2020
ms.author: victorh
ms.openlocfilehash: 5192ecb31c71364bdf1301b13da0b0742625d44f
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2020
ms.locfileid: "87460127"
---
# <a name="ip-groups-in-azure-firewall-policy"></a>Azure 防火墙策略中的 IP 组

IP 组允许通过以下方式对 Azure 防火墙策略的 IP 地址进行分组和管理：

- 作为 DNAT 规则中的源类型
- 作为网络规则中的源或目标类型
- 作为应用程序规则中的源类型


IP 组可以有单个 IP 地址、多个 IP 地址或一个或多个 IP 地址范围。

对于 Azure 中的区域和订阅，可以在 Azure 防火墙 DNAT、网络和应用程序规则中重复使用 IP 组。 组名称必须是唯一的。 可以在 Azure 门户、Azure CLI 或 REST API 中配置 IP 组。 提供了一个示例模板，帮助你入门。

## <a name="sample-format"></a>示例格式

以下 IPv4 地址格式示例有效，可在 IP 组中使用：

- 单个地址：10.0.0。0
- CIDR 表示法： 10.1.0.0/32
- 地址范围： 10.2.0.0-10.2.0.31

## <a name="create-an-ip-group"></a>创建 IP 组

可以使用 Azure 门户、Azure CLI 或 REST API 创建 IP 组。 有关详细信息，请参阅[创建 IP 组](../firewall/create-ip-group.md)。

## <a name="browse-ip-groups"></a>浏览 IP 组
1. 在 Azure 门户搜索栏中，键入 " **IP 组**" 并将其选中。 你可以看到 IP 组的列表，或者可以选择 "**添加**" 以创建新的 ip 组。
2. 选择 IP 组以打开 "概述" 页。 可以编辑、添加或删除 IP 地址或 IP 组。

   ![IP 组概述](media/ip-groups/overview.png)

## <a name="manage-an-ip-group"></a>管理 IP 组

你可以查看 IP 组中的所有 IP 地址以及与其关联的规则或资源。 若要删除 IP 组，必须先将该 IP 组与使用该 ip 组的资源取消关联。

1. 若要查看或编辑 IP 地址，请在左窗格中的 "**设置**" 下选择 " **ip 地址**"。
2. 若要添加单个或多个 IP 地址，请选择 "**添加 Ip 地址**"。 这会打开用于上传的**拖动或浏览**页，也可以手动输入该地址。
3.    选择右侧的省略号（**...**）可编辑或删除 IP 地址。 若要编辑或删除多个 IP 地址，请选中相应的框，然后选择顶部的 "**编辑**" 或 "**删除**"。
4. 最后，可以导出 CSV 文件格式的文件。

> [!NOTE]
> 如果删除 IP 组中的所有 IP 地址，但在规则中仍在使用它，则会跳过该规则。


## <a name="use-an-ip-group"></a>使用 IP 组

你现在可以选择**Ip 组**作为 ip 地址的**源类型**或**目标类型**（如果你使用 DNAT、应用程序或网络规则创建策略）。

:::image type="content" source="media/ip-groups/firewall-ip-group.png" alt-text="防火墙中的 IP 组":::

## <a name="ip-address-limits"></a>IP 地址限制

每个防火墙最多可以有100个 IP 组，每个 IP 组最多可包含5000个单独 IP 地址或 IP 前缀。

## <a name="related-azure-powershell-cmdlets"></a>相关 Azure PowerShell cmdlet

以下 Azure PowerShell cmdlet 可用于创建和管理 IP 组：

- [New-AzIpGroup](https://docs.microsoft.com/powershell/module/az.network/new-azipgroup?view=azps-3.4.0)
- [AzIPGroup](https://docs.microsoft.com/powershell/module/az.network/remove-azipgroup?view=azps-3.4.0)
- [Get-AzIpGroup](https://docs.microsoft.com/powershell/module/az.network/get-azipgroup?view=azps-3.4.0)
- [Set-AzIpGroup](https://docs.microsoft.com/powershell/module/az.network/set-azipgroup?view=azps-3.4.0)
- [新-AzFirewallPolicyNetworkRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallpolicynetworkrule?view=azps-3.4.0)
- [新-AzFirewallPolicyApplicationRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallpolicyapplicationrule?view=azps-3.4.0)
- [新-AzFirewallPolicyNatRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallpolicynatrule?view=azps-3.4.0)

## <a name="next-steps"></a>后续步骤

- [教程：使用 Azure 防火墙管理器保护虚拟 WAN](secure-cloud-network.md)