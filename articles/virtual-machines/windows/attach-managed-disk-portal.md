---
title: 将托管数据磁盘附加到 Windows VM-Azure
description: 如何使用 Azure 门户将托管数据磁盘附加到 Windows VM。
author: roygara
ms.service: virtual-machines-windows
ms.topic: how-to
ms.date: 02/06/2020
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: b100676cd8cfbffd273ea6ad50b63545f2d6037a
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87836218"
---
# <a name="attach-a-managed-data-disk-to-a-windows-vm-by-using-the-azure-portal"></a>使用 Azure 门户将托管数据磁盘附加到 Windows VM

本文介绍如何使用 Azure 门户将新的托管数据磁盘附加到 Windows 虚拟机 (VM)。 VM 的大小决定了可以附加的数据磁盘数量。 有关详细信息，请参阅[虚拟机的大小](../sizes.md)。


## <a name="add-a-data-disk"></a>添加数据磁盘

1. 转到 [Azure 门户](https://portal.azure.com)以添加数据磁盘。 搜索并选择“虚拟机”。 
2. 从列表中选择虚拟机。
3. 在“虚拟机”页上选择“磁盘”   。
4. 在“磁盘”页上选择“添加数据磁盘”   。
5. 在新磁盘的下拉列表中，选择“创建磁盘”。 
6. 在“创建托管磁盘”页中，键入磁盘名称，并根据需要调整其他设置。  完成操作后，选择“创建”  。
7. 在“磁盘”页中，选择“保存”，以保存 VM 的新磁盘配置   。
8. 在 Azure 创建磁盘并将磁盘附加到虚拟机之后，新磁盘将出现在“数据磁盘”下的虚拟机磁盘设置中。 


## <a name="initialize-a-new-data-disk"></a>初始化新的数据磁盘

1. 连接到 VM。
1. 在正在运行的 VM 中选择 Windows“开始”菜单，然后在搜索框中输入 **diskmgmt.msc**  。 此时会打开“磁盘管理”控制台  。
2. “磁盘管理”识别出新的未初始化磁盘，并显示“初始化磁盘”窗口  。
3. 请确保选择新磁盘，然后选择“确定”对其进行初始化  。
4. 新的磁盘显示为“未分配”  。 右键单击磁盘上任意位置，选择“新建简单卷”。  此时会打开“新建简单卷向导”窗口  。
5. 完成向导中的每一步，保留所有默认值，完成后，选择“完成”  。
6. 关闭“磁盘管理”  。
7. 此时出现一个弹出窗口，通知你需要先格式化新磁盘才能使用新磁盘。 选择“格式化磁盘”  。
8. 在“格式化新磁盘”窗口中，检查设置，然后选择“启动”   。
9. 此时出现一条警告，通知你格式化磁盘会擦除所有数据。 选择“确定”  。
10. 完成格式化后，选择“确定”  。

## <a name="next-steps"></a>后续步骤

- 还可以[使用 PowerShell 附加数据磁盘](attach-disk-ps.md)。
- 如果应用程序需要使用 *D:* 盘存储数据，可以[更改 Windows 临时磁盘的驱动器号](change-drive-letter.md)。