---
title: 设置或自定义服务器备份
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 441c2d6c-435a-42cb-90f2-6d680d279d34
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 06d8381e622cdb6c437b0f5aa4d1cd9293ef779a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310589"
---
# <a name="set-up-or-customize-server-backup"></a>设置或自定义服务器备份

>适用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials
  
 在安装期间不会自动配置服务器备份。 你应通过计划每日备份来自动保护你的服务器及其数据。 建议维护每日备份计划，因为大多数组织都不能承受丢失在几天内创建的数据的后果。  
  
 请参阅以下部分来设置或自定义服务器备份：  
  
-   [设置或更改服务器备份设置](Set-up-or-customize-server-backup.md#BKMK_1)  
  
-   [服务器备份计划](Set-up-or-customize-server-backup.md#BKMK_2)  
  
-   [备份目标驱动器](Set-up-or-customize-server-backup.md#BKMK_Target)  
  
-   [要备份的项](Set-up-or-customize-server-backup.md#BKMK_4)  
  
##  <a name="set-up-or-change-server-backup-settings"></a><a name="BKMK_1"></a>设置或更改服务器备份设置  
  
#### <a name="to-set-up-or-change-server-backup-settings"></a>设置或更改服务器备份设置  
  
1.  打开“仪表板”，然后单击“设备”选项卡。  
  
2.  在“列表”视图中，单击服务器以将其选中。  
  
3.  在“任务”窗格中，单击“为服务器设置备份”。  
  
    > [!NOTE]
    >  如果想要更改现有的备份设置，请单击“为服务器自定义备份”。  
  
4.  按向导中的说明进行操作。  
  
    > [!NOTE]
    >  如果在将外部硬盘驱动器连接到服务器之前启动向导，则在连接硬盘驱动器后，在“选择备份目标”页上单击“刷新列表”。  
  
> [!NOTE]
>  在 Windows Server Essentials 的默认安装中，服务器配置为每周自动执行一次碎片整理。 如果你使用非 Microsoft 映像软件，这将导致生成的备份比正常备份大。 如果无需定期对服务器进行碎片整理，你可以遵循以下步骤关闭碎片整理计划：  
> 
> 1. 按 Windows 键 + W 以打开“搜索”。  
>    2. 在“搜索”文本框中，键入 **Defragment**。  
>    3. 在结果部分中，单击“碎片整理和优化驱动器”。  
>    4. 在“优化驱动器”页面上，选择一个驱动器，然后单击“更改设置”。  
>    5. 在“优化计划”窗口中，清除“按计划运行(推荐)”复选框，然后单击“确定”以保存更改。  
  
##  <a name="server-backup-schedule"></a><a name="BKMK_2"></a>服务器备份计划  
 当你使用设置服务器备份向导或自定义服务器备份向导时，你可以选择在一天内多次备份服务器数据。 因为向导将计划基于增量的备份，因此备份将快速运行，并且不会显著影响服务器性能。 默认情况下，向导将计划一个要在每天 12:00 PM 和 11:00 PM 运行的备份。 但是，你可以根据组织需求调整备份计划。 你应该不定期评估备份计划的有效性，并根据需要更改计划。  
  
##  <a name="backup-target-drive"></a><a name="BKMK_Target"></a>备份目标驱动器  
 你可以使用多个外部存储驱动器进行备份，也可以在现场和场外存储位置之间旋转驱动器。 这样在现场硬件出现物理损坏时，可以帮助恢复数据，从而改善您的灾难防备计划。  
  
 为你的服务器备份选择存储驱动器时，请考虑以下方面：  
  
-   选择包含足够空间的驱动器来存储数据。 你的存储驱动器所含的容量应至少是要备份数据的存储容量的 2.5 倍。 驱动器还应足够大，以适应以后增长的服务器数据。  
  
-   当使用外部存储驱动器时，请确保该驱动器为空，或仅包含不需要的数据。  
  
    > [!CAUTION]
    >  设置服务器备份向导将在配置存储驱动器以进行备份时格式化存储驱动器。  
  
-   如果备份目标驱动器包含脱机驱动器，备份配置将不会成功。 若要完成配置，请在选择备份目标时，清除复选框以排除处于脱机状态的驱动器。  
  
-   如果你将包含以前的备份的驱动器选择为备份目标，则该向导允许你选择是否要保留以前的备份。 如果保留这些备份，则该向导不会格式化驱动器。  
  
-   你应访问外部存储驱动器制造商的网站，以确保你的备份驱动器在运行 Windows Server Essentials 的计算机上受支持。  
  
-   驱动器不能包含可扩展固件接口 (EFI) 系统分区。 如果 USB 驱动器上存在 EFI 分区，则假定该磁盘是启动磁盘。 如果确定不需要磁盘上的数据，则可以重新格式化磁盘并将其用于备份。  
  
    > [!CAUTION]
    >  在重新格式化磁盘时，将删除所有数据。  
  
    #### <a name="to-remove-an-efi-system-partition-from-a-usb-disk"></a>从 USB 磁盘删除 EFI 系统分区  
  
    1.  在控制面板中，打开“系统和安全”。  
  
    2.  在“管理工具”下，单击“创建和格式化硬盘分区”。  
  
    3.  删除 USB 磁盘上的所有卷，或只删除 EFI 分区。 设置服务器备份向导将删除所有卷。  
  
-   驱动器不能包含任何共享的服务器文件夹。 在可以将磁盘用作备份目标驱动器之前，你必须停止任何共享服务器文件夹上的共享。 可以从仪表板中或在文件资源管理器中停止共享。  
  
    #### <a name="to-stop-sharing-on-a-server-folder-from-the-dashboard"></a>从仪表板停止服务器文件夹上的共享。  
  
    1.  在仪表板上，单击“存储”，然后单击“服务器文件夹”。  
  
    2.  选择想要停止共享的文件夹，然后在任务窗格上，单击“停止”。  
  
> [!NOTE]
>  如果由于备份驱动器空间不足而导致备份失败，则会从 Windows Server Essentials 数据库中删除备份目标驱动器的驱动器号，并且仪表板不会显示该驱动器。 如果想要在将来备份时使用该驱动器，则必须使用本机工具重新分配驱动器号。  
> 
>  **为现有卷重新分配驱动器号**  
> 
> 1. 在控制面板中，打开“系统和安全”。  
>    2. 在“管理工具”下，单击“创建和格式化硬盘分区”。  
>    3. 右键单击该驱动器，然后单击“更改驱动器号和路径”。  
>    4. 单击 **“添加”** 。  
>    5. 在“添加驱动器号或路径”对话框中，选择要分配的驱动器号。 （可以重新分配相同的驱动器号。）然后单击 **"确定"** 。  
> 
>    该驱动器将立即显示在仪表板上。  
  
##  <a name="items-to-be-backed-up"></a><a name="BKMK_4"></a>要备份的项  
 你可以选择备份服务器上的所有驱动器、文件和文件夹，或仅选择单独的硬盘、文件或文件夹进行备份。  
  
 在添加或删除驱动器时，或者添加或删除共享文件和文件夹时，你应该重新访问服务器备份配置，以确保将这些项添加到备份配置中或从其中删除它们。 若要添加或删除要备份的项，请执行以下操作之一：  
  
- 若要在服务器备份中包含数据驱动器，请选择相邻的复选框  
  
- 若要从服务器备份中排除数据驱动器，请清除相邻的复选框  
  
  > [!NOTE]
  >  如果你想要从备份中排除“操作系统”项，必须首先清除“系统备份(推荐)”复选框。  
  
  若要最大程度地减少服务器备份所使用的服务器存储量，你可能想要排除包含你认为没有价值或并非特别重要的文件的所有文件夹。  
  
  例如，你可能有一个包含录制的电视节目的文件夹（使用大量硬盘驱动器空间）。 你可以选择不备份这些文件，因为通常无论如何你都会在查看这些文件后删除它们。 或者，你可能有一个包含不想要保留的临时文件的文件夹。  
  
## <a name="see-also"></a>另请参阅  
  
-   [管理服务器备份](Manage-Server-Backup-in-Windows-Server-Essentials.md)  
  
-   [管理备份和还原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
