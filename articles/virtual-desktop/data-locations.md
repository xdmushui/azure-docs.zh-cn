---
title: Windows 虚拟桌面的数据位置-Azure
description: 对 Windows 虚拟机的数据和元数据的存储位置的简要概述。
author: Heidilohr
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: e626b7e729e394b1012848904f5ce12279c3ef24
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "88010066"
---
# <a name="data-and-metadata-locations-for-windows-virtual-desktop"></a>Windows 虚拟桌面的数据和元数据位置

>[!IMPORTANT]
>本教程的内容适用于包含 Azure 资源管理器 Windows 虚拟桌面对象的 Windows 虚拟桌面。 如果你使用的是不包含 Azure 资源管理器对象的 Windows 虚拟桌面（经典），请参阅[此文](./virtual-desktop-fall-2019/data-locations-2019.md)。

Windows 虚拟桌面目前可用于所有地理位置。 管理员可以在创建主机池虚拟机和相关服务（如文件服务器）时选择存储用户数据的位置。 在[azure 数据中心地图](https://azuredatacentermap.azurewebsites.net/)上详细了解 azure 地理位置。

>[!NOTE]
>Microsoft 不会控制或限制你或你的用户可以访问你的用户和特定于应用程序的数据的区域。

>[!IMPORTANT]
>Windows 虚拟桌面存储数据中心内的全局元数据信息，如租户名称、主机池名称、应用组名称和用户主体名称。 每次客户创建服务对象时，都必须输入服务对象的位置。 它们所输入的位置确定将存储对象的元数据的位置。 客户将选择一个 Azure 区域，并且元数据将存储在相关地理位置。 有关所有 Azure 区域和相关地理位置的列表，请参阅[azure 地理](https://azure.microsoft.com/global-infrastructure/geographies/)位置。

目前，我们仅支持在美国中存储元数据 (我们) Azure 地域。 存储的元数据是静态加密的，异地冗余镜像将保留在地理位置。 所有客户数据（如应用设置和用户数据）驻留在客户选择的位置，并且不受服务管理。 随着服务的增长，更多的地理位置将变为可用。

出于灾难恢复目的，在 Azure 地域内复制服务元数据。