---
title: logman delete
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8360a4955a5ebed3eb25fda77acf587c56fbf5d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374435"
---
# <a name="logman-delete"></a>logman delete

>适用于：Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

删除现有的数据收集器。  

## <a name="syntax"></a>语法  
```  
logman delete <[-n] <name>> [options]  
```  
## <a name="parameters"></a>参数  

|        参数        |                                                                               描述                                                                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /?            |                                                                    显示区分上下文的帮助。                                                                     |
|   -s <computer name>    |                                                          在指定的远程计算机上执行命令。                                                          |
|     -config <value>     |                                                         指定包含命令选项的设置文件。                                                         |
|       [-n] <name>       |                                                                   目标数据收集器的名称。                                                                    |
|          -ets           |                                              直接将命令发送到事件跟踪会话，无需保存或计划。                                               |
| -[-] u < user [password] > | 指定要以其身份运行的用户。 输入密码 \* 会生成密码提示。 在密码提示符下键入密码时，不会显示密码。 |

## <a name="BKMK_examples"></a>示例  
以下命令删除数据收集器 perf_log。  
```  
logman delete perf_log  
```  
#### <a name="additional-references"></a>其他参考  
[logman](logman.md)  
