---
uid: visual-studio/overview/2012/windows-azure-authentication
title: "Windows Azure 身份验证 |Microsoft 文档"
author: Rick-Anderson
description: "Microsoft ASP.NET tools for Windows Azure Active Directory 可以简单，以使托管 Windows Azure 网站上的 web 应用程序的身份验证..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2013
ms.topic: article
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4deb3536699f1ef3025f8858ee71a76a1c2def18
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="windows-azure-authentication"></a>Windows Azure 身份验证
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> Microsoft ASP.NET 工具为 Windows Azure Active Directory 可以很容易地为上托管的 web 应用程序启用身份验证[Windows Azure 网站](https://www.windowsazure.com/home/features/web-sites/)。 可以使用 Windows Azure 身份验证从组织、 从本地 Active Directory 同步的公司帐户或用户在你自己的自定义 Windows Azure Active Directory 域中创建的 Office 365 用户进行身份验证。 启用 Windows Azure 身份验证配置你的应用程序使用单个的用户进行身份验证[Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)租户。
> 
> 对于云服务中的 web 角色不支持 ASP.NET Windows Azure 身份验证工具，但我们计划在将来的版本中这样做。 [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) 支持在 Windows Azure web 角色。
> 
> 有关如何设置您的本地 Active Directory 和 Windows Azure Active Directory 租户之间的同步的详细信息请参阅[使用 AD FS 2.0 实现和管理单一登录](https://technet.microsoft.com/library/jj205462.aspx)。
> 
> Windows Azure Active Directory 是目前只有[免费预览版 service](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。


## <a name="requirements"></a>要求：

- Visual Studio 2012 or [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Web 工具扩展 Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409)或[Web 工具扩展 for Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306)或[Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>使用 Visual Studio 2012 创建的 ASP.NET Web 应用

你可以使用 Visual Studio 2012 创建任何 Web 应用程序，本教程使用 ASP.NET MVC intranet 模板。

1. 创建新的 ASP.NET MVC 4 Intranet 应用程序，接受所有默认值。 (它必须是 In**遍历**net 而不是在**ter** net 项目)。  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>启用窗口 Azure 身份验证 （如果你是原则全局管理员）

如果你没有现有的 Windows Azure Active Directory 租户 （例如，通过现有 Office 365 帐户） 可以通过注册创建新租户[新的 Windows Azure Active Directory 帐户](http://g.microsoftonline.com/0AX00en/5)。

1. 从项目菜单上选择**启用 Windows Azure 身份验证**:  
  
 ![](windows-azure-authentication/_static/image2.png)

2. 为你的 Windows Azure Active Directory 租户 (例如，contoso.onmicrosoft.com) 输入域，然后单击**启用**:

![](windows-azure-authentication/_static/image3.png)

3. Web 身份验证对话框在登录中作为 Windows Azure Active Directory 租户的管理员：  
  
 ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>启用非管理员的原则 Window Azure

如果你没有 Windows Azure Active Directory 租户全局管理员特权，可以取消选中复选框设置应用程序。

![](windows-azure-authentication/_static/image6.png)

该对话框将显示**域**，**应用程序主体 Id**和**答复 URL**其所需的设置与 Azure Active Directory 应用程序原则。 你需要将此信息提供给具有足够的特权来设置应用程序的人员。 请参阅[如何实现与 Windows Azure Active Directory 的 ASP.NET 应用程序的单一登录](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)有关如何使用 cmdlet 来手动创建服务主体的详细信息。  
一旦成功设置应用程序后，你可以单击**继续使用所选设置更新 web.config**。 如果你想要在继续开发应用程序同时等待预配，发生这种情况，你可以单击**接近记住项目文件中的设置**。 下一次调用启用 Windows Azure 身份验证并取消选中设置复选框，你将看到相同的设置，可以单击**继续**，然后单击，**应用这些设置在 web.config**.

1. 为 Windows Azure 身份验证配置你的应用程序并将其设置与 Windows Azure Active Directory 请稍候。
2. Windows Azure 身份验证启用你的应用程序后，单击**关闭：** 

    ![](windows-azure-authentication/_static/image7.png)
3. 按 F5 运行应用程序。 你应自动获取重定向到登录页。 使用目录原则用户凭据登录到应用程序...  

    ![](windows-azure-authentication/_static/image1.jpg)
4. 因为你的应用程序当前使用的自签名的测试证书你将收到一条警告，来自的浏览器的证书不由受信任的证书颁发机构颁发。

    忽略此警告可以安全地在本地开发期间通过单击**继续访问此网站：** 

    ![](windows-azure-authentication/_static/image8.png)
5. 你现在已成功登录到你使用 Windows Azure 身份验证的应用程序 ！

    ![](windows-azure-authentication/_static/image2.jpg)

启用 Windows Azure 身份验证将为你的应用程序做出以下更改：

- 防跨站点请求伪造 ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) 类 (*应用\_Start\AntiXsrfConfig.cs* ) 添加到你的项目。
- NuGet 包`System.IdentityModel.Tokens.ValidatingIssuerNameRegistry`添加到你的项目。
- 你的应用程序中的 Windows Identity Foundation 设置将配置为接受来自 Windows Azure Active Directory 租户的安全令牌。 单击下图所示，若要查看对所做的更改的扩展的视图*Web.config*文件。  
  
     ![](windows-azure-authentication/_static/image9.png)
- 将设置你的 Windows Azure Active Directory 租户中的应用程序服务主体。
- 启用 HTTPS。

## <a name="deploy-the-application-to-windows-azure"></a>部署到 Windows Azure 应用程序

有关完整说明，请参阅[部署 ASP.NET Web 应用程序到 Windows Azure 网站](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)。

若要发布使用 Windows Azure 身份验证到 Azure 网站的应用程序：

1. 右键单击你的应用程序并选择**发布：** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. 从发布 Web 对话框中，下载并导入发布配置文件为你 Azure 网站。

    ![](windows-azure-authentication/_static/image4.jpg)
3. **连接**选项卡上显示**目标 URL** (你的应用程序的公共面向 URL)。 单击**验证连接**来测试连接：

    ![](windows-azure-authentication/_static/image5.jpg)
4. 如果你已将发布到此 Azure 网站之前，请考虑检查**删除目标位置的其他文件**完全发布设置，以确保你的应用程序。 请注意**启用 Windows Azure 身份验证**复选框，则 slected。  

    ![](windows-azure-authentication/_static/image10.png)
5. 可选： 在**预览**选项卡上单击**开始预览**若要查看部署的文件。

    ![](windows-azure-authentication/_static/image6.jpg)
6. 单击**发布。**

    将提示你为目标主机启用 Windows Azure 身份验证。 单击**启用**继续：

    ![](windows-azure-authentication/_static/image11.png)
7. 为你的 Windows Azure Active Directory 租户中输入你的管理员凭据：

    ![](windows-azure-authentication/_static/image7.jpg)
8. 你的应用程序已成功发布后，浏览器将打开到已发布的网站。

    > [!NOTE]
    > 它可能需要应用程序，才能完全与 Windows Azure Active Directory 设置为目标主机中启用 Windows Azure 身份验证后最多 5 分钟 （通常必须更少）。 当你首次运行你的应用程序如果您收到错误 ACS50001： 找不到名称 [领域] 的信赖方，然后等待几分钟，并重试运行一次，应用程序。
9. 出现提示时，请为你的目录中的用户登录：

    ![](windows-azure-authentication/_static/image8.jpg)
10. 您现在已成功登录到你的 Azure 托管应用程序使用 Windows Azure 身份验证。  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>已知问题

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>使用 Windows Azure 身份验证 < o:p >< 时，基于角色的授权失败 / o:p >

Windows Azure 身份验证当前不提供必需的角色声明，以便可以执行基于角色的授权。 经过身份验证的用户的角色必须手动从检索 Windows Azure Active Directory。 < o:p >< / o:p >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>浏览到错误中的 Windows Azure 身份验证结果的应用程序"在已登录用户 (live.com) 的域的不匹配任何 ACS20016 允许此域 STS"< o:p >< / o:p >

如果您已登录到 Microsoft 帐户 （例如 hotmail.com、 live.com、 outlook.com） 并尝试访问的应用程序启用了 Windows Azure 身份验证你可能会收到 400 错误响应，因为你的 Microsoft 帐户的域无法识别 Windows Azure Active Directory。 若要登录到应用程序，从注销你的 Microsoft 帐户第一次。 < o:p >< / o:p >

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a>登录到 Windows Azure 身份验证启用的应用程序和 X509CertificateValidationMode 以外无导致 accounts.accesscontrol.windows.net 证书 < o:p >< 的证书验证错误 / o:p >

证书验证不是必需的并应处于已禁用。 颁发者证书的指纹验证。 WSFederationAuthenticationModule。 < o:p >< / o:p >

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>尝试启用 Windows Azure 身份验证时将 Web 身份验证对话框会显示错误"ACS20016： 在已登录用户 (contoso.onmicrosoft.com) 的域的此 STS 任何允许的域不匹配。"< o:p >< / o:p >

当你之前已成功登录使用相同的 Visual Studio 进程内不同的 Windows Azure Active Directory 帐户时，可能会看到此错误。 从指定的帐户注销或重新启动 Visual Studio。 如果您以前记录在中，选择"使我保持登录中"选项则可能需要清除浏览器 cookie。 < o:p >< / o:p >

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012： 请求不是有效的 WS 联合身份验证协议消息 < o:p >< / o:p >

如果你已登录到 Azure 服务之一的某些其他 Microsoft ID，也可能发生。 使用专用浏览器窗口，例如在 IE 中的 InPrivate 或 Incognito Chrome 中的，或清除所有 cookie。 <o:p></o:p>

## <a name="additional-resources"></a>其他资源

- [Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci
- [Windows Azure 功能： 标识](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory： 为你的组织中开发应用程序](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory： 为多个组织中开发应用程序](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [如何实现与 Windows Azure Active Directory 的单一登录](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [单一登录与 Windows Azure Active Directory： 深入了解如何](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx)– Vittorio Bertocci
- [使用 AD FS 2.0 实现和管理单一登录](https://technet.microsoft.com/library/jj205462.aspx)
