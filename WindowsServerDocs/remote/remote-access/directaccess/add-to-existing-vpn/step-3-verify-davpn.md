---
title: 步骤3验证部署
description: 本主题是将 DirectAccess 添加到 Windows Server 2016 现有远程访问（VPN）部署的指南的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 43ac612e-2e77-418c-8171-ebb2086b7cb6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 3967f50a354ee6ed31273734f2449d6d337d9b50
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314728"
---
# <a name="step-3-verify-the-deployment"></a>步骤3验证部署

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍如何验证是否已正确配置 DirectAccess 部署。  
  
### <a name="to-verify-access-to-internal-resources-through-directaccess"></a>验证通过 DirectAccess 对内部资源进行的访问  
  
1.  将 DirectAccess 客户端计算机连接到公司网络并获取组策略。  
  
2.  单击通知区域中的“网络连接” 图标以访问 DA 媒体管理器。  
  
3.  单击“DirectAccess 连接”，你会看到状态是“本地连接”。  
  
4.  将客户端计算机连接到外部网络并尝试访问内部资源。  
  
    你应能够访问所有公司资源。  
  


