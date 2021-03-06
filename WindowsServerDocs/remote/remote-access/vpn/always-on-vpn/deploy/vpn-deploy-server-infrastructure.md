---
title: 配置服务器基础结构
description: 在此步骤中，将安装和配置支持 VPN 所需的服务器端组件。 服务器端组件包括配置 PKI 以分发用户、VPN 服务器和 NPS 服务器使用的证书。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: lizross
author: eross-msft
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: a7d1c451989d69f45f02571de4854b0f0f4e12f5
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80307829"
---
# <a name="step-2-configure-the-server-infrastructure"></a>步骤 2： 配置服务器基础结构

>适用于： Windows Server （半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows 10

- [**上一个：** 步骤1。规划 Always On 的 VPN 部署](always-on-vpn-deploy-planning.md)
- [**下一步：** 步骤3。为 Always On VPN 配置远程访问服务器](vpn-deploy-ras.md)

在此步骤中，你将安装和配置支持 VPN 所需的服务器端组件。 服务器端组件包括配置 PKI 以分发用户、VPN 服务器和 NPS 服务器使用的证书。  你还可以配置 RRAS 以支持 IKEv2 连接，并配置 NPS 服务器以执行 VPN 连接授权。

## <a name="configure-certificate-autoenrollment-in-group-policy"></a>在组策略中配置证书自动注册
在此过程中，你将在域控制器上配置组策略，以便域成员自动请求用户和计算机证书。 这样，VPN 用户便可以请求和检索自动对 VPN 连接进行身份验证的用户证书。 同样，此策略允许 NPS 服务器自动请求服务器身份验证证书。 

在 VPN 服务器上手动注册证书。

>[!TIP]
>对于未域的已加入计算机，请参阅[未加入域的计算机的 CA 配置](#ca-configuration-for-non-domain-joined-computers)。 由于 RRAS 服务器未加入域，因此无法使用自动注册来注册 VPN 网关证书。  因此，请使用脱机证书请求过程。

1. 在域控制器上，打开组策略管理。

2. 在导航窗格中，右键单击你的域（例如，corp.contoso.com），然后选择 "**在此域中创建 GPO 并在此处链接"** 。

3. 在 "新建 GPO" 对话框中，输入 "**自动注册策略**"，然后选择 **"确定"** 。

4. 在导航窗格中，右键单击 "**自动注册策略**"，然后选择 "**编辑**"。

5. 在组策略管理编辑器中，完成以下步骤以配置计算机证书自动注册：

    1. 在导航窗格中，在 "Windows > 设置 **" > ** **策略**" > **Windows 设置** **" > "** **公钥策略**"。

    2. 在详细信息窗格中，右键单击 "**证书服务客户端–自动注册**"，然后选择 "**属性**"。

    3. 在 "证书服务客户端–自动注册属性" 对话框的 "**配置模型**" 中，选择 "**已启用**"。

    4. 选中 **“续订过期证书、更新未决证书并删除吊销的证书”** 以及 **“使用证书模板的更新证书”** 。

    5. 选择“确定”。

6. 在组策略管理编辑器中，完成以下步骤以配置用户证书自动注册：

    1. 在导航窗格中， > **Windows > 设置**"中的"**用户配置**" > **策略** **" > ** **公钥策略**"。

    2. 在详细信息窗格中，右键单击**证书服务客户端 - 自动注册**，然后选择**属性**。

    3. 在 "证书服务客户端–自动注册属性" 对话框的 "**配置模型**" 中，选择 "**已启用**"。

    4. 选中 **“续订过期证书、更新未决证书并删除吊销的证书”** 以及 **“使用证书模板的更新证书”** 。

    5. 选择“确定”。

    6. 关闭“组策略管理编辑器”。

7. 关闭“组策略管理”。

### <a name="ca-configuration-for-non-domain-joined-computers"></a>未加入域的计算机的 CA 配置

由于 RRAS 服务器未加入域，因此无法使用自动注册来注册 VPN 网关证书。  因此，请使用脱机证书请求过程。

1. 在 RRAS 服务器上，根据附录 A 中提供的示例证书策略请求（第0部分）生成名为**VPNGateway**的文件，并自定义以下条目：

   - 在 [NewRequest] 节中，将使用者名称的 vpn.contoso.com 替换为所选的 [_Customer_] vpn 终结点 FQDN。

   - 在 [Extensions] 部分，使用所选的 [_Customer_] vpn 终结点 FQDN 替换用于使用者备用名称的 vpn.contoso.com。

2. 将**VPNGateway**文件保存或复制到所选位置。

3. 在提升的命令提示符下，导航到包含**VPNGateway**文件的文件夹，然后键入：

   ```
   certreq -new VPNGateway.inf VPNGateway.req
   ```

4. 将新创建的**VPNGateway**输出文件复制到证书颁发机构服务器或特权访问工作站（PAW）。

5. 将**VPNGateway**文件保存或复制到证书颁发机构服务器或特权访问工作站（PAW）上的所选位置。

6. 在提升的命令提示符下，导航到包含在上一步中创建的 VPNGateway 文件的文件夹，然后键入：

   ```
   certreq -attrib “CertificateTemplate:[Customer]VPNGateway” -submit VPNgateway.req VPNgateway.cer
   ```

7. 如果 "证书颁发机构列表" 窗口出现提示，请选择相应的企业 CA 来为证书请求提供服务。

8. 将新创建的**VPNGateway**输出文件复制到 RRAS 服务器。 

9. 将**VPNGateway**文件保存或复制到 RRAS 服务器上的所选位置。

10. 在提升的命令提示符下，导航到包含在上一步中创建的 VPNGateway 文件的文件夹，然后键入：

    ```
    certreq -accept VPNGateway.cer
    ```

11. 按[此处](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in)所述运行 "证书" mmc 管理单元，选择 "**计算机帐户**" 选项。

12. 请确保具有以下属性的 RRAS 服务器存在有效的证书：

    - **预期目的：** 服务器身份验证，IP 安全 IKE 中间 

    - **证书模板：** [_Customer_] VPN 服务器

#### <a name="example-vpngatewayinf-script"></a>示例： VPNGateway 脚本

在这里，你可以看到证书请求策略的示例脚本，该策略用于使用带外进程来请求 VPN 网关证书。

>[!TIP]
>可以在 "证书请求策略" 文件夹下的 "VPN 产品 IP 工具包" 中找到 VPNGateway 脚本的副本。 仅使用客户特定的值更新 "Subject" 和 "\_continue\_"。

```
[Version] 

Signature="$Windows NT$"

[NewRequest]
Subject = "CN=vpn.contoso.com"
Exportable = FALSE   
KeyLength = 2048     
KeySpec = 1          
KeyUsage = 0xA0      
MachineKeySet = True
ProviderName = "Microsoft RSA SChannel Cryptographic Provider"
RequestType = PKCS10 

[Extensions]
2.5.29.17 = "{text}"
_continue_ = "dns=vpn.contoso.com&"
```

## <a name="create-the-vpn-users-vpn-servers-and-nps-servers-groups"></a>创建 VPN 用户、VPN 服务器和 NPS 服务器组

在此过程中，你可以添加新的 Active Directory （AD）组，其中包含允许使用 VPN 连接到组织网络的用户。

此组有两个用途：

- 它定义了允许哪些用户自动注册 VPN 要求的用户证书。

- 它定义 NPS 授权哪些用户进行 VPN 访问。

通过使用自定义组，如果你想要撤消用户的 VPN 访问权限，则可以从组中删除该用户。

还会添加包含 VPN 服务器的组和包含 NPS 服务器的其他组。 你使用这些组将证书请求限制为其成员。

>[!NOTE]
>建议位于 DMA/外围网络中的 VPN 服务器未加入域。 但是，如果想要将 VPN 服务器加入域以获得更好的可管理性（组策略、备份/监视代理、无本地用户管理等），则将 AD 组添加到 VPN 服务器证书模板。

### <a name="configure-the-vpn-users-group"></a>配置 VPN 用户组

1. 在域控制器上，打开 Active Directory 用户和计算机 "。

2. 右键单击容器或组织单位，选择 "**新建**"，然后选择 "**组**"。

3. 在 "**组名**" 中，输入**VPN 用户**，然后选择 **"确定"** 。

4. 右键单击 " **VPN 用户**"，然后选择 "**属性**"。

5. 在 "VPN 用户属性" 对话框的 "**成员**" 选项卡上，选择 "**添加**"。

6. 在 "选择用户" 对话框中，添加需要 VPN 访问权限的所有用户，然后选择 **"确定"** 。

7. 关闭“Active Directory 用户和计算机”。

### <a name="configure-the-vpn-servers-and-nps-servers-groups"></a>配置 VPN 服务器和 NPS 服务器组

1. 在域控制器上，打开 Active Directory 用户和计算机 "。

2. 右键单击容器或组织单位，选择 "**新建**"，然后选择 "**组**"。

3. 在 "**组名**" 中，输入**VPN 服务器**，然后选择 **"确定"** 。

4. 右键单击 " **VPN 服务器**"，然后选择 "**属性**"。

5. 在 "VPN 服务器属性" 对话框的 "**成员**" 选项卡上，选择 "**添加**"。

6. 选择 "**对象类型**"，选中 "**计算机**" 复选框，然后选择 **"确定"** 。

7. 在 "**输入要选择的对象名称**" 中，输入 VPN 服务器的名称，然后选择 **"确定"** 。

8. 选择 **"确定"** 以关闭 "VPN 服务器属性" 对话框。

9. 为 NPS 服务器组重复前面的步骤。

10. 关闭“Active Directory 用户和计算机”。

## <a name="create-the-user-authentication-template"></a>创建用户身份验证模板

在此过程中，您将配置自定义客户端-服务器身份验证模板。 此模板是必需的，因为你希望通过选择 "升级兼容性级别" 并选择 "Microsoft 平台加密提供程序" 来提高证书的整体安全性。 这最后一次更改使你可以在客户端计算机上使用 TPM 来保护证书。 有关 TPM 的概述，请参阅[受信任的平台模块技术概述](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview)。

>[!IMPORTANT] 
>Microsoft 平台加密提供程序 "需要 TPM 芯片，在这种情况下，如果你运行的是 VM，并且收到以下错误：" 尝试手动注册证书时，无法在本地计算机中找到有效的 CSP "。提供程序 "，并在证书属性的" 加密 "选项卡中按顺序按顺序进行。

**方法**

1. 在 CA 上，打开 "证书颁发机构"。

2. 在导航窗格中，右键单击 "**证书模板**"，然后选择 "**管理**"。

3. 在 "证书模板" 控制台中，右键单击 "**用户**"，然后选择 "**复制模板**"。

   >[!WARNING]
   >请不要在步骤10之前的任何时间选择 "**应用** **" 或 "确定"** 。  如果在输入所有参数之前选择了这些按钮，则很多选择都是固定的，不再可编辑。 例如，在 "**加密**" 选项卡上，如果_旧的加密存储提供程序_在 "提供程序类别" 字段中显示，则该提供程序将被禁用，从而阻止进一步的更改。 唯一的替代方法是删除并重新创建模板。  

4. 在 "新模板的属性" 对话框的 "**常规**" 选项卡上，完成以下步骤：

   1. 在 "**模板显示名称**" 中，键入**VPN 用户身份验证**。

   2. 清除 "**在 Active Directory 中发布证书**" 复选框。

5. 在 "**安全**" 选项卡上，完成以下步骤：

   1. 选择“添加”。

   2. 在 "选择用户、计算机、服务帐户或组" 对话框中，输入**VPN 用户**，然后选择 **"确定"** 。

   3. 在 "**组或用户名**" 中，选择 " **VPN 用户**"。

   4. 在 " **VPN 用户的权限**" 中，选中 "**允许**" 列中的 "**注册**" 和 "**自动注册**" 复选框。

      >[!TIP]
      >请确保选中 "读取" 复选框。 换句话说，你需要注册的读取权限。 

   5. 在 "**组或用户名**" 中，选择 "**域用户**"，然后选择 "**删除**"。

6. 在 "**兼容性**" 选项卡上，完成以下步骤：

   1. 在 "**证书颁发机构**" 中，选择**Windows Server 2012 R2**。

   2. 在 "**产生的更改**" 对话框中，选择 **"确定"** 。

   3. 在 "**证书收件人**" 中，选择**Windows 8.1/Windows Server 2012 R2**。

   4. 在 "**产生的更改**" 对话框中，选择 **"确定"** 。

7. 在 "**请求处理**" 选项卡上，清除 "**允许导出私钥**" 复选框。

8. 在 "**加密**" 选项卡上，完成以下步骤：

   1. 在 "**提供程序" 类别**中，选择 "**密钥存储提供程序**"。

   2. 选择 **"请求必须使用以下提供程序之一"** 。

   3. 选中 " **Microsoft 平台加密提供程序**" 复选框。

9. 在 "**使用者名称**" 选项卡上，如果没有在所有用户帐户上列出的电子邮件地址，请清除 "在使用者名称和**电子邮件名称** **中包含电子邮件名称**" 复选框。

10. 选择 **"确定"** 以保存 VPN 用户身份验证证书模板。

11. 关闭证书模板控制台。

12. 在 "证书颁发机构" 管理单元的导航窗格中，右键单击 "**证书模板**"，选择 "**新建**"，然后选择 "**要颁发的证书模板**"。

13. 选择 " **VPN 用户身份验证**"，然后选择 **"确定"** 。

14. 关闭 "证书颁发机构" 管理单元。

## <a name="create-the-vpn-server-authentication-template"></a>创建 VPN 服务器身份验证模板

在此过程中，你可以为 VPN 服务器配置新的服务器身份验证模板。 如果有多个证书可用于服务器身份验证扩展密钥用法，则添加 IP 安全（IPsec） IKE 中间应用程序策略允许服务器筛选证书。

>[!IMPORTANT]
>由于 VPN 客户端从公共 Internet 访问此服务器，因此使用者和备用名称与内部服务器名称不同。 因此，你无法在 VPN 服务器上自动注册此证书。

**先决条件：**

已加入域的 VPN 服务器

**方法**

1. 在 CA 上，打开 "证书颁发机构"。

2. 在导航窗格中，右键单击 "**证书模板**"，然后选择 "**管理**"。

3. 在 "证书模板" 控制台中，右键单击 " **RAS 和 IAS 服务器**"，然后选择 "**复制模板**"。

4. 在 "新模板的属性" 对话框的 "**常规**" 选项卡上的 "**模板显示名称**" 中，输入 vpn 服务器的描述性名称，例如 " **vpn 服务器身份验证**" 或 " **RADIUS 服务器**"。

5. 在 "**扩展**" 选项卡上，完成以下步骤：

    1. 选择 "**应用程序策略**"，然后选择 "**编辑**"。

    2. 在 "**编辑应用程序策略扩展**" 对话框中，选择 "**添加**"。

    3. 在 "**添加应用程序策略**" 对话框中，选择 " **IP 安全 IKE 中级**"，然后选择 **"确定"** 。
   
        如果 VPN 服务器上存在多个服务器身份验证证书，则将 IP 安全 IKE 中级添加到 EKU 可帮助。 如果存在 IP 安全 IKE 中级，则 IPSec 只使用具有两个 EKU 选项的证书。 如果不这样做，IKEv2 身份验证可能会失败，并出现错误13801： IKE 身份验证凭据不可接受。

    4. 选择 **"确定"** 以返回到 "**新模板的属性**" 对话框。

6. 在 "**安全**" 选项卡上，完成以下步骤：

    1. 选择“添加”。

    2. 在 "**选择用户、计算机、服务帐户或组**" 对话框中，输入**VPN 服务器**，然后选择 **"确定"** 。

    3. 在 "**组或用户名**" 中，选择 " **VPN 服务器**"。

    4. 在 " **VPN 服务器的权限**" 中，选中 "**允许**" 列中的 "**注册**" 复选框。

    5. 在 "**组或用户名**" 中，选择 " **RAS 和 IAS 服务器**"，然后选择 "**删除**"。

7. 在 "**使用者名称**" 选项卡上，完成以下步骤：

    1. 选择 **"在请求中提供"** 。

    2. 在 "**证书模板**警告" 对话框中，选择 **"确定"** 。

8. 可有可无如果要配置 VPN 连接的条件性访问，请选择 "**请求处理**" 选项卡，然后选择 "**允许导出私钥**"。

9. 选择 **"确定"** 以保存 VPN 服务器证书模板。

10. 关闭证书模板控制台。

11. 在 "证书颁发机构" 管理单元的导航窗格中，右键单击 "**证书模板**"，单击 "**新建**"，然后单击 "**要颁发的证书模板**"。

12. 重新启动证书颁发机构服务。(*)

13. 在 "证书颁发机构" 管理单元的导航窗格中，右键单击 "**证书模板**"，选择 "**新建**"，然后选择 "**要颁发的证书模板**"。

14. 选择在上面的步骤4中选择的名称，然后单击 **"确定"** 。

15. 关闭 "证书颁发机构" 管理单元。

* **可以通过在 CMD 中运行以下命令来停止/启动 CA 服务：**

```
Net Stop "certsvc"
Net Start "certsvc"
```

## <a name="create-the-nps-server-authentication-template"></a>创建 NPS 服务器身份验证模板

要创建的第三个和最后一个证书模板是 NPS 服务器身份验证模板。 NPS 服务器身份验证模板是在本部分前面创建的 NPS 服务器组中受保护的 RAS 和 IAS 服务器模板的简单副本。

你将为自动注册配置此证书。

**方法**

1. 在 CA 上，打开 "证书颁发机构"。

2. 在导航窗格中，右键单击 "**证书模板**"，然后选择 "**管理**"。

3. 在 "证书模板" 控制台中，右键单击 " **RAS 和 IAS 服务器**"，然后选择 "**复制模板**"。

4. 在 "新模板的属性" 对话框的 "**常规**" 选项卡上，在 "**模板显示名称**" 中键入**NPS 服务器身份验证**。

5. 在 "**安全**" 选项卡上，完成以下步骤：

    1. 选择“添加”。

    2. 在 "**选择用户、计算机、服务帐户或组**" 对话框中，输入 " **NPS 服务器**"，然后选择 **"确定"** 。

    3. 在 "**组或用户名**" 中，选择 " **NPS 服务器**"。

    4. 在 " **NPS 服务器的权限**" 中，选中 "**允许**" 列中的 "**注册**" 和 "**自动注册**" 复选框。

    5. 在 "**组或用户名**" 中，选择 " **RAS 和 IAS 服务器**"，然后选择 "**删除**"。

6. 选择 **"确定"** 以保存 NPS 服务器证书模板。

7. 关闭证书模板控制台。

8. 在 "证书颁发机构" 管理单元的导航窗格中，右键单击 "**证书模板**"，选择 "**新建**"，然后选择 "**要颁发的证书模板**"。

9. 选择 " **NPS 服务器身份验证**"，然后选择 **"确定"** 。

10. 关闭 "证书颁发机构" 管理单元。

## <a name="enroll-and-validate-the-user-certificate"></a>注册并验证用户证书

由于你正在使用组策略来自动注册用户证书，因此你只需更新策略，Windows 10 将自动注册用户帐户以获取正确的证书。 然后，你可以在证书控制台中验证证书。

**方法**

1. 以**VPN 用户**组成员的身份登录到已加入域的客户端计算机。

2. 按 Windows 键 + R，键入**gpupdate/force**，然后按 enter。

3. 在 "开始" 菜单上，键入**certmgr.msc**，然后按 enter。

4. 在 "证书" 管理单元的 "**个人**" 下，选择 "**证书**"。 证书将出现在详细信息窗格中。

5. 右键单击具有当前域用户名的证书，然后选择 "**打开**"。

6. 在 "**常规**" 选项卡上，确认 "**有效期**" 下列出的日期为今天的日期。 否则，可能是因为选择了错误的证书。

7. 选择 **"确定"** ，然后关闭 "证书" 管理单元。

## <a name="enroll-and-validate-the-server-certificates"></a>注册并验证服务器证书

与用户证书不同，你必须手动注册 VPN 服务器的证书。 注册后，请使用用于用户证书的相同过程进行验证。 与用户证书一样，NPS 服务器会自动注册其身份验证证书，因此您只需对其进行验证即可。

>[!NOTE]
>你可能需要重新启动 VPN 和 NPS 服务器，使其能够更新其组成员身份，然后才能完成这些步骤。

### <a name="enroll-and-validate-the-vpn-server-certificate"></a>注册并验证 VPN 服务器证书

1. 在 VPN 服务器的 "开始" 菜单上，键入**certlm.msc**，然后按 enter。

2. 右键单击 "**个人**"，选择 "**所有任务**"，然后选择 "**申请新证书**" 以启动证书注册向导。

3. 在 "开始之前" 页上，选择 "**下一步**"。

4. 在 "选择证书注册策略" 页上，选择 "**下一步**"。

5. 在 "申请证书" 页上，选中 VPN 服务器旁边的复选框以将其选中。

6. 在 "VPN 服务器" 复选框下，选择 "**需要详细信息**" 以打开 "证书属性" 对话框并完成以下步骤：

    1. 选择 "**使用者**" 选项卡，在 "**使用者名称** **" 下选择**"**类型**"。

    2. 在 "**使用者名称**"**下，输入**用于连接到 VPN 的外部域客户端的名称，例如 vpn.contoso.com，然后选择 "**添加**"。

    3. 在 "**备用名称**" 下的 "**类型**" 中，选择**DNS**。

    4. 在 "**备用名称**" 下的 "**值**" 中，输入客户端用来连接到 VPN 的所有服务器名称，例如，vpn.contoso.com、VPN、132.64.86.2。

    5. 输入每个名称后，选择 "**添加**"。

    6. 完成后选择 **"确定"** 。

7. 选择 "**注册**"。

8. 选择“完成”。

9. 在 "证书" 管理单元的 "**个人**" 下，选择 "**证书**"。
    
    列出的证书将显示在详细信息窗格中。

10. 右键单击具有您的 VPN 服务器名称的证书，然后选择 "**打开**"。

11. 在 "**常规**" 选项卡上，确认 "**有效期**" 下列出的日期为今天的日期。 否则，可能是因为选择的证书不正确。

12. 在 "**详细信息**" 选项卡上，选择 "**增强型密钥用法**"，并验证 " **IP 安全 IKE 中间**和**服务器身份验证**" 是否显示在列表中。

13. 选择 **"确定"** 以关闭证书。

14. 关闭 "证书" 管理单元。

### <a name="validate-the-nps-server-certificate"></a>验证 NPS 服务器证书

1. 重新启动 NPS 服务器。

2. 在 NPS 服务器的 "开始" 菜单上，键入**certlm.msc**，然后按 enter。

3. 在 "证书" 管理单元的 "**个人**" 下，选择 "**证书**"。

    列出的证书将显示在详细信息窗格中。

4. 右键单击包含 NPS 服务器名称的证书，然后选择 "**打开**"。

5. 在 "**常规**" 选项卡上，确认 "**有效期**" 下列出的日期为今天的日期。 否则，可能是因为选择的证书不正确。

6. 选择 **"确定"** 以关闭证书。

7. 关闭 "证书" 管理单元。

## <a name="next-steps"></a>后续步骤

[步骤3。为 Always On VPN 配置远程访问服务器](vpn-deploy-ras.md)：在此步骤中，你将配置远程访问 VPN 以允许 IKEV2 VPN 连接，拒绝来自其他 VPN 协议的连接，并分配静态 ip 地址池，用于颁发 IP 地址，以便连接授权的 VPN 客户端。
