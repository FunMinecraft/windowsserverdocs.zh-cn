---
title: nslookup lserver
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 347ad6e380f8d8163c4954771c9e985271b2d549
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373076"
---
# <a name="nslookup-lserver"></a>nslookup lserver

>适用于：Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

将默认服务器更改为指定的域名系统（DNS）域。
## <a name="syntax"></a>语法
```
lserver <DNSDomain> 
```
## <a name="parameters"></a>参数

|    参数    |                      描述                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | 为默认服务器指定新的 DNS 域。  |
| {help &#124; ？} | 显示**nslookup**子命令的简短摘要。 |

## <a name="remarks"></a>备注
- **Lserver**命令使用初始服务器来查找有关指定 DNS 域的信息。 这与**服务器**命令不同，后者使用当前的默认服务器。
  ## <a name="additional-references"></a>其他参考
  [命令行语法键](command-line-syntax-key.md)
  [nslookup 服务器](nslookup-server.md)
