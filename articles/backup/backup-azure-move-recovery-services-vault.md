---
title: 如何移动 Azure 备份恢复服务保管库
description: 有关如何在 Azure 订阅和资源组之间移动恢复服务保管库的说明。
ms.topic: conceptual
ms.date: 04/08/2019
ms.custom: references_regions
ms.openlocfilehash: 69021131f12b57aedcd531997029858b0722933f
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89181504"
---
# <a name="move-a-recovery-services-vault-across-azure-subscriptions-and-resource-groups"></a>跨 Azure 订阅和资源组移动恢复服务保管库

本文介绍如何跨 Azure 订阅移动针对 Azure 备份配置的恢复服务保管库，或者将其移到同一订阅中的另一个资源组。 可以使用 Azure 门户或 PowerShell 移动恢复服务保管库。

## <a name="supported-regions"></a>支持的区域

澳大利亚东部支持恢复服务保管库的资源移动，澳大利亚东南部、加拿大中部、加拿大东部、南东亚、东亚、美国中南部、美国中北部、美国东部、美国东部2、美国中南部、美国东部、美国西部、美国中南部、美国西部、美国西部2、巴西南部、印度中部、韩国南部、日本东部、日本西部、韩国中部、韩国南部、北欧、西欧、南非北部、南非西部、英国南部和英国西部。

## <a name="unsupported-regions"></a>不受支持的区域

法国中部、法国南部、德国东北部、德国中部、US Gov 爱荷华州、中国北部、中国 North2、中国东部、中国东部2

## <a name="prerequisites-for-moving-recovery-services-vault"></a>移动恢复服务保管库的先决条件

- 在跨资源组的保管库移动期间，源和目标资源组都会被锁定，从而阻止了写入和删除操作。 有关详细信息，请参阅此[文章](../azure-resource-manager/management/move-resource-group-and-subscription.md)。
- 只有订阅管理员有权移动保管库。
- 对于跨订阅移动保管库，目标订阅必须位于与源订阅相同的租户中，并且应启用其状态。
- 必须有权对目标资源组执行写入操作。
- 移动保管库只会更改资源组。 恢复服务保管库将位于同一位置，并且无法更改。
- 一次只能在每个区域移动一个恢复服务保管库。
- 如果 VM 不会跨订阅移动到恢复服务保管库或新资源组，则当前 VM 恢复点将在保管库中保持不变，直到它们过期。
- 不管 VM 是否连同保管库一起移动，都始终可以从保管库中保留的备份历史记录还原该 VM。
- Azure 磁盘加密要求密钥保管库和 VM 位于同一 Azure 区域和订阅中。
- 若要移动包含托管磁盘的虚拟机，请参阅[此文](https://azure.microsoft.com/blog/move-managed-disks-and-vms-now-available/)。
- 用于移动通过经典模型部署的资源的选项会有所不同，这取决于你是要将资源移到订阅中，还是移动到新的订阅。 有关详细信息，请参阅此[文章](../azure-resource-manager/management/move-resource-group-and-subscription.md)。
- 跨订阅移动保管库或将其移到新资源组后，为保管库定义的备份策略将会保留。
- 只能移动包含以下任何类型的备份项的保管库。 在移动保管库之前，需要停止下面未列出的任何类型的备份项目，并将数据永久删除。
  - Azure 虚拟机
  - Microsoft Azure 恢复服务 (MARS) 代理
  - Microsoft Azure 备份 Server (MABS) 
  - Data Protection Manager (DPM)
- 如果跨订阅移动包含 VM 备份数据的保管库，则必须将 VM 移到同一订阅，并使用同一目标 VM 资源组名称（与旧订阅中的名称相同）来继续备份。

> [!NOTE]
> 不支持跨 Azure 区域移动 Azure 备份的恢复服务保管库。<br><br>
> 如果已配置任何 Vm (Azure IaaS、Hyper-v、VMware) 或物理计算机使用 **Azure Site Recovery**进行灾难恢复，则移动操作将被阻止。 如果要为 Azure Site Recovery 移动保管库，请参阅 [此文](../site-recovery/move-vaults-across-regions.md) ，了解如何手动移动保管库。

## <a name="use-azure-portal-to-move-recovery-services-vault-to-different-resource-group"></a>使用 Azure 门户将恢复服务保管库移到不同的资源组

若要将恢复服务保管库及其关联的资源移到不同的资源组，请执行以下操作：

1. 登录 [Azure 门户](https://portal.azure.com/)。
2. 打开“恢复服务保管库”列表，并选择要移动的保管库。**** 当保管库仪表板打开时，其外观如下图所示。

   ![打开恢复服务保管库](./media/backup-azure-move-recovery-services/open-recover-service-vault.png)

   如果看不到保管库的 **概要** 信息，请选择下拉图标。 现在，应会看到保管库的“概要”信息。

   ![“概要”信息选项卡](./media/backup-azure-move-recovery-services/essentials-information-tab.png)

3. 在保管库概述菜单中，选择**资源组**旁边的 "**更改**" 以打开 "**移动资源**" 窗格。

   ![更改资源组](./media/backup-azure-move-recovery-services/change-resource-group.png)

4. 在 " **移动资源** " 窗格中，对于所选的保管库，建议通过选中相应的复选框来移动可选的相关资源，如下图所示。

   ![移动订阅](./media/backup-azure-move-recovery-services/move-resource.png)

5. 要添加目标资源组，请在 " **资源组** " 下拉列表中选择现有资源组，或选择 " **创建新组** " 选项。

   ![创建资源](./media/backup-azure-move-recovery-services/create-a-new-resource.png)

6. 添加资源组后，确认 **我了解到与移动的资源关联的工具和脚本在更新为使用新的资源 id 选项后，将无法工作** ，然后选择 **"确定"** 完成移动保管库。

   ![确认消息](./media/backup-azure-move-recovery-services/confirmation-message.png)

## <a name="use-azure-portal-to-move-recovery-services-vault-to-a-different-subscription"></a>使用 Azure 门户将恢复服务保管库移到不同的订阅

可将恢复服务保管库及其关联的资源移到不同的订阅

1. 登录 [Azure 门户](https://portal.azure.com/)。
2. 打开“恢复服务保管库”列表，并选择要移动的保管库。 当保管库仪表板打开时，其外观如下图所示。

    ![打开恢复服务保管库](./media/backup-azure-move-recovery-services/open-recover-service-vault.png)

    如果看不到保管库的 **概要** 信息，请选择下拉图标。 现在，应会看到保管库的“概要”信息。

    ![“概要”信息选项卡](./media/backup-azure-move-recovery-services/essentials-information-tab.png)

3. 在保管库概述菜单中，选择 "**订阅**" 旁边的 "**更改**"，以打开 "**移动资源**" 窗格。

   ![更改订阅](./media/backup-azure-move-recovery-services/change-resource-subscription.png)

4. 选择要移动的资源。此处我们建议使用“全选”选项来选择列出的所有可选资源。****

   ![移动资源](./media/backup-azure-move-recovery-services/move-resource-source-subscription.png)

5. 在“订阅”下拉列表中选择目标订阅，保管库将移到该订阅。****
6. 要添加目标资源组，请在 " **资源组** " 下拉列表中选择现有资源组，或选择 " **创建新组** " 选项。

   ![添加订阅](./media/backup-azure-move-recovery-services/add-subscription.png)

7. 选择**我了解，在将其更新为使用新的资源 id 选项进行确认，然后选择 "确定" 之前，与移动资源关联的工具和脚本将不起作用**。 **OK**

> [!NOTE]
> 跨订阅备份 (RS vault 和受保护的 Vm 在不同的订阅中) 不是受支持的方案。 此外，还不能在保管库移动操作过程中修改本地冗余存储 (LRS) 到全局冗余存储的存储冗余选项 (GRS) ，反之亦然。
>
>

## <a name="use-powershell-to-move-recovery-services-vault"></a>使用 PowerShell 移动恢复服务保管库

若要将恢复服务保管库移到另一个资源组，请使用 `Move-AzureRMResource` cmdlet。 `Move-AzureRMResource` 需要资源名称和资源类型。 可以通过 `Get-AzureRmRecoveryServicesVault` cmdlet 获取这两项信息。

```powershell
$destinationRG = "<destinationResourceGroupName>"
$vault = Get-AzureRmRecoveryServicesVault -Name <vaultname> -ResourceGroupName <vaultRGname>
Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vault.ID
```

若要将资源移到不同的订阅，请包含 `-DestinationSubscriptionId` 参数。

```powershell
Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vault.ID
```

执行上述 cmdlet 之后，系统会要求确认是否要移动指定的资源。 键入 **Y** 确认。 验证成功后，资源将会移动。

## <a name="use-cli-to-move-recovery-services-vault"></a>使用 CLI 移动恢复服务保管库

若要将恢复服务保管库移到另一个资源组，请使用以下 cmdlet：

```azurecli
az resource move --destination-group <destinationResourceGroupName> --ids <VaultResourceID>
```

若要移到新订阅，请提供 `--destination-subscription-id` 参数。

## <a name="post-migration"></a>迁移之后

1. 设置/验证资源组的访问控制。  
2. 移动完成后，需要再次为保管库配置备份报告和监视功能。 在移动操作期间，以前的配置将会丢失。

## <a name="next-steps"></a>后续步骤

可以在资源组和订阅之间移动许多不同类型的资源。

有关详细信息，请参阅[将资源移到新资源组或订阅](../azure-resource-manager/management/move-resource-group-and-subscription.md)。
