---
uid: web-api/overview/security/external-authentication-services
title: "与 ASP.NET Web API 的外部身份验证服务 (C#) |Microsoft 文档"
author: rmcmurray
description: "描述如何在 ASP.NET Web API 中使用外部身份验证服务。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: 744396cb0c95d1887f259b1e2e890bd06ef7d049
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="external-authentication-services-with-aspnet-web-api-c"></a>与 ASP.NET Web API 的外部身份验证服务 (C#)
====================
通过[Robert McMurray](https://github.com/rmcmurray)

Visual Studio 2013 和 ASP.NET 4.5.1 展开的安全选项[单页面应用程序](../../../single-page-application/index.md)(SPA) 和[Web API](../../index.md)服务与外部身份验证服务，包括数种集成OAuth/OpenID 和社交媒体身份验证服务： Microsoft 帐户、 Twitter、 Facebook 和 Google。

### <a name="in-this-walkthrough"></a>在本演练中

- [使用外部身份验证服务](#USING)
- [创建示例 Web 应用程序](#SAMPLE)
- [启用 Facebook 身份验证](#FACEBOOK)
- [启用 Google 身份验证](#GOOGLE)
- [启用 Microsoft 身份验证](#MICROSOFT)
- [启用 Twitter 身份验证](#TWITTER)
- [其他信息](#MOREINFO)

    - [组合外部身份验证服务](#COMBINE)
    - [配置 IIS Express 以使用完全限定的域名](#FQDN)
    - [如何获取 Microsoft 身份验证的应用程序设置](#OBTAIN)
    - [可选： 禁用本地注册](#DISABLE)

### <a name="prerequisites"></a>系统必备

若要按照本演练中的示例，你需要具备以下：

- Visual Studio 2013
- 为至少一个以下外部身份验证服务帐户：

    - Google 用户帐户
    - 开发人员帐户使用应用程序标识符和个以下的社交媒体身份验证服务的密钥：

        - Microsoft 帐户 ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
        - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
        - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>使用外部身份验证服务

丰富的 web 开发人员帮助以减少开发当前可用的外部身份验证服务创建新的 web 应用程序的时间。 Web 用户通常具有多个现有帐户用于常用 web 服务和社交媒体网站，因此当身份验证的服务从外部 web 服务或社交媒体网站 web 应用程序实现的情况下，它将保存的开发时间，将所创建的身份验证实现已用。 使用外部身份验证服务将保存最终用户，从无需创建 web 应用程序，另一个帐户以及无需记住另一个用户名和密码。

在过去，开发人员有两个选项： 创建自己的身份验证实现，或了解如何将外部身份验证服务集成到其应用程序。 在最基本级别下, 图展示了简单的请求流正在从配置为使用外部身份验证服务的 web 应用程序请求信息的用户代理 （web 浏览器）：

[![](external-authentication-services/_static/image2.png "单击以展开映像")](external-authentication-services/_static/image1.png)

在前面的图中，情况下，用户代理 （或在此示例中的 web 浏览器） 发出请求的 web 应用程序，将 web 浏览器重定向到外部身份验证服务。 用户代理会将其凭据发送到外部身份验证服务，并重如果用户代理已成功通过身份验证，外部身份验证服务将定向到使用某种形式的原始的 web 应用程序的用户代理标记其用户代理将发送到 web 应用程序。 Web 应用程序将使用该令牌来验证用户代理已成功验证外部身份验证服务和 web 应用程序可能使用该标记来收集有关用户代理的详细信息。 应用程序完成处理用户代理的信息，该 web 应用程序将返回为用户代理基于其授权设置适当的响应。

在此第二个示例中，用户代理与 web 应用程序和外部授权服务器协商和 web 应用程序执行与要检索有关用户的其他信息的外部授权服务器的其他通信代理：

[![](external-authentication-services/_static/image4.png "单击以展开映像")](external-authentication-services/_static/image3.png)

Visual Studio 2013 和 ASP.NET 4.5.1 与外部身份验证服务的集成更轻松地进行开发人员通过提供以下身份验证服务的内置集成：

- Facebook
- google
- Microsoft 帐户 （Windows Live ID 帐户）
- twitter

在本演练中的示例将演示如何通过使用 Visual Studio 2013 附带的新 ASP.NET Web 应用程序模板来配置每个支持的外部身份验证服务。

> [!NOTE]
> 如有必要，你可能需要将你的 FQDN 添加到外部身份验证服务的设置。 此要求基于安全约束，对于某些外部身份验证服务，这需要在应用程序设置要与你的客户端使用的 FQDN 匹配的 FQDN。 （此步骤将会有很大为每个外部身份验证服务; 你将需要参考的文档以查看这是否需要每个外部身份验证服务以及如何配置这些设置。）如果你需要配置 IIS Express 以用于测试此环境中使用 FQDN，请参阅[配置 IIS Express，以使用完全限定的域名](#FQDN)在本演练后面的部分。


<a id="SAMPLE"></a>
## <a name="creating-a-sample-web-application"></a>创建示例 Web 应用程序

以下步骤将引导你完成创建示例应用程序使用 ASP.NET Web 应用程序模板，并将为每个外部身份验证服务稍后在本演练中使用此示例应用程序。

启动 Visual Studio 2013 选择**新项目**从开始页。 或从**文件**菜单上，选择**新建**然后**项目**。

[![](external-authentication-services/_static/image6.png "单击以展开映像")](external-authentication-services/_static/image5.png)

当**新项目**显示对话框中，选择**已安装****模板**展开**Visual C#**。 下**Visual C#**，选择**Web**。 在项目模板列表中，选择**ASP.NET Web 应用程序**。 输入你的项目的名称，然后单击**确定**。

[![](external-authentication-services/_static/image8.png "单击以展开映像")](external-authentication-services/_static/image7.png)

当**新建 ASP.NET 项目**将显示选择**SPA**模板，然后单击**创建项目**。

[![](external-authentication-services/_static/image10.png "单击以展开映像")](external-authentication-services/_static/image9.png)

与 Visual Studio 2013 的等待创建你的项目。

[![](external-authentication-services/_static/image12.png "单击以展开映像")](external-authentication-services/_static/image11.png)

完成后创建您的项目的 Visual Studio 2013，打开*Startup.Auth.cs*文件将位于**应用\_启动**文件夹。

[![](external-authentication-services/_static/image14.png "单击以展开映像")](external-authentication-services/_static/image13.png)

首次创建项目时，没有任何外部身份验证服务中启用*Startup.Auth.cs*文件; 以下展示你的代码可能与的类似，针对在哪里突出显示部分中，您将启用外部身份验证服务和任何相关的设置，才能使用 ASP.NET 应用程序的 Microsoft 帐户、 Twitter、 Facebook 或 Google 身份验证：

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

按 F5 生成和调试 web 应用程序时，它将显示登录屏幕，你将看到已定义没有外部身份验证服务。

[![](external-authentication-services/_static/image16.png "单击以展开映像")](external-authentication-services/_static/image15.png)

在以下部分中，你将学习如何来启用每个使用 Visual Studio 2013 中的 ASP.NET 提供的外部身份验证服务。

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>启用 Facebook 身份验证

使用 Facebook 身份验证要求你创建 Facebook 开发人员帐户，并且你的项目需要的应用程序 ID 和密钥从 Facebook 才能正常。 有关创建 Facebook 开发人员帐户，并获取你的应用程序 ID 和机密密钥的信息，请参阅[https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)。

你一次获取你的应用程序 ID 和机密密钥，使用以下步骤来启用 web 应用程序的 Facebook 身份验证：

1. 在 Visual Studio 2013 中打开你的项目时，打开*Startup.Auth.cs*文件：

    [![](external-authentication-services/_static/image18.png "单击以展开映像")](external-authentication-services/_static/image17.png)
2. 查找代码的突出显示的部分：

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. 删除&quot; // &quot;字符，取消注释的代码中，突出显示的行，然后添加你的应用程序 ID 和机密密钥。 一旦你已添加这些参数，则可以重新编译你的项目：

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. 按 F5 以在 web 浏览器中打开 web 应用程序时，你将看到 Facebook 已被定义为外部身份验证服务：

    [![](external-authentication-services/_static/image20.png "单击以展开映像")](external-authentication-services/_static/image19.png)
5. 当你单击**Facebook**按钮，你的浏览器将重定向到 Facebook 登录页：

    [![](external-authentication-services/_static/image22.png "单击以展开映像")](external-authentication-services/_static/image21.png)
6. 输入你的 Facebook 凭据并单击后**登录**，web 浏览器将重定向回 web 应用程序，这会提示你输入**用户名**你想要将与相关联你Facebook 帐户：

    [![](external-authentication-services/_static/image24.png "单击以展开映像")](external-authentication-services/_static/image23.png)
7. 在输入你的用户名并单击后**注册**按钮，你的 web 应用程序将显示默认值**主页**为你的 Facebook 帐户：

    [![](external-authentication-services/_static/image26.png "单击以展开映像")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>启用 Google 身份验证

Google 是迄今最简单的方法的外部身份验证服务中启用，因为它不需要开发人员帐户，也不要求应用程序 ID 或密钥等其他外部身份验证服务的其他信息需要。

若要启用 web 应用程序的 Google 身份验证，请使用以下步骤：

1. 在 Visual Studio 2013 中打开你的项目时，打开*Startup.Auth.cs*文件：

    [![](external-authentication-services/_static/image28.png "单击以展开映像")](external-authentication-services/_static/image27.png)
2. 查找代码的突出显示的部分：

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. 删除&quot; // &quot;字符来取消注释的代码中，突出显示的行，然后重新编译你的项目：

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. 按 F5 以在 web 浏览器中打开 web 应用程序时，你将看到 Google 已被定义为外部身份验证服务：

    [![](external-authentication-services/_static/image30.png "单击以展开映像")](external-authentication-services/_static/image29.png)
5. 当你单击**Google**按钮，你的浏览器将重定向到 Google 登录页：

    [![](external-authentication-services/_static/image32.png "单击以展开映像")](external-authentication-services/_static/image31.png)
6. 输入你的 Google 凭据并单击后**登录**，Google 将提示你验证你的 web 应用程序有权访问你的 Google 帐户：

    [![](external-authentication-services/_static/image34.png "单击以展开映像")](external-authentication-services/_static/image33.png)
7. 当你单击**接受**，web 浏览器将重定向回 web 应用程序，这会提示你输入**用户名**你想要将与你的 Google 帐户相关联：

    [![](external-authentication-services/_static/image36.png "单击以展开映像")](external-authentication-services/_static/image35.png)
8. 在输入你的用户名并单击后**注册**按钮，你的 web 应用程序将显示默认值**主页**为你的 Google 帐户：

    [![](external-authentication-services/_static/image38.png "单击以展开映像")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>启用 Microsoft 身份验证

Microsoft 身份验证要求你创建开发人员帐户，并且它才能正常需要客户端 ID 和客户端密钥。 有关创建 Microsoft 开发人员帐户，并获取你的客户端 ID 和客户端密钥的信息，请参阅[https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070)。

你一次获取你的使用者密钥和使用者机密，使用以下步骤来启用 web 应用程序的 Microsoft 身份验证：

1. 在 Visual Studio 2013 中打开你的项目时，打开*Startup.Auth.cs*文件：

    [![](external-authentication-services/_static/image40.png "单击以展开映像")](external-authentication-services/_static/image39.png)
2. 查找代码的突出显示的部分：

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. 删除&quot; // &quot;字符，取消注释的代码中，突出显示的行，然后添加你的客户端 ID 和客户端密钥。 一旦你已添加这些参数，则可以重新编译你的项目：

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. 按 F5 以在 web 浏览器中打开 web 应用程序时，你将看到 Microsoft 已被定义为外部身份验证服务：

    [![](external-authentication-services/_static/image42.png "单击以展开映像")](external-authentication-services/_static/image41.png)
5. 当你单击**Microsoft**按钮，你的浏览器将重定向到 Microsoft 登录页：

    [![](external-authentication-services/_static/image44.png "单击以展开映像")](external-authentication-services/_static/image43.png)
6. 输入你的 Microsoft 凭据并单击后**登录**，系统将提示你验证你的 web 应用程序有权访问你的 Microsoft 帐户：

    [![](external-authentication-services/_static/image46.png "单击以展开映像")](external-authentication-services/_static/image45.png)
7. 当你单击**是**，web 浏览器将重定向回 web 应用程序，这会提示你输入**用户名**你想要将与你的 Microsoft 帐户相关联：

    [![](external-authentication-services/_static/image48.png "单击以展开映像")](external-authentication-services/_static/image47.png)
8. 在输入你的用户名并单击后**注册**按钮，你的 web 应用程序将显示默认值**主页**你的 Microsoft 帐户：

    [![](external-authentication-services/_static/image50.png "单击以展开映像")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>启用 Twitter 身份验证

Twitter 身份验证要求你创建开发人员帐户，并且它才能正常需要使用者密钥和使用者机密。 有关创建 Twitter 开发人员帐户和获取使用者密钥和使用者机密的信息，请参阅[https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)。

你一次获取你的使用者密钥和使用者机密，请使用以下步骤启用 web 应用程序的 Twitter 身份验证：

1. 在 Visual Studio 2013 中打开你的项目时，打开*Startup.Auth.cs*文件：

    [![](external-authentication-services/_static/image52.png "单击以展开映像")](external-authentication-services/_static/image51.png)
2. 查找代码的突出显示的部分：

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. 删除&quot; // &quot;字符，取消注释的代码中，突出显示的行，然后添加你的使用者密钥和使用者机密。 一旦你已添加这些参数，则可以重新编译你的项目：

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. 按 F5 以在 web 浏览器中打开 web 应用程序时，你将看到 Twitter 已被定义为外部身份验证服务：

    [![](external-authentication-services/_static/image54.png "单击以展开映像")](external-authentication-services/_static/image53.png)
5. 当你单击**Twitter**按钮，你的浏览器将重定向到 Twitter 登录页：

    [![](external-authentication-services/_static/image56.png "单击以展开映像")](external-authentication-services/_static/image55.png)
6. 输入你的 Twitter 凭据并单击后**Authorize 应用**，web 浏览器将重定向回 web 应用程序，这会提示你输入**用户名**你想要将与相关联你的 Twitter 帐户：

    [![](external-authentication-services/_static/image58.png "单击以展开映像")](external-authentication-services/_static/image57.png)
7. 在输入你的用户名并单击后**注册**按钮，你的 web 应用程序将显示默认值**主页**为你的 Twitter 帐户：

    [![](external-authentication-services/_static/image60.png "单击以展开映像")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>其他信息

有关创建使用 OAuth 和 OpenID 应用程序的其他信息，请参阅以下 Url:

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>组合外部身份验证服务

用于提高灵活性，你可以在同一时间定义多个外部身份验证服务-这允许你的 web 应用程序的用户使用从任何启用的外部身份验证服务的帐户：

[![](external-authentication-services/_static/image62.png "单击以展开映像")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configuring-iis-express-to-use-a-fully-qualified-domain-name"></a>配置 IIS Express 以使用完全限定的域名

某些外部身份验证提供程序不支持使用这样的 HTTP 地址来测试你的应用程序`http://localhost:port/`。 若要解决此问题，可以将完全限定的域名名称 (FQDN) 的静态映射添加到你的主机文件，并在 Visual Studio 2013 中以进行测试/调试使用 FQDN 配置项目的选项。 为此，请使用以下步骤：

- 添加静态 FQDN 映射你的主机文件：

    1. 打开提升的命令提示符窗口中。
    2. 键入以下命令：

        <kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd>
    3. 类似于以下条目添加到主机文件中：

        <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
    4. 保存并关闭你的主机文件。
- 配置 Visual Studio 项目以使用 FQDN:

    1. 在 Visual Studio 2013 中打开你的项目时，单击**项目**菜单，然后选择你的项目的属性。 例如，你可以选择**WebApplication1 属性**。
    2. 选择**Web**选项卡。
    3. 输入你的 FQDN 为**项目 Url**。 例如，将输入<kbd>http://www.wingtiptoys.com</kbd> ，如果已添加到你的主机文件的 FQDN 映射。
- 配置 IIS Express 以使用你的应用程序的 FQDN:

    1. 打开提升的命令提示符窗口中。
    2. 键入以下命令以更改为 IIS Express 文件夹：

        <kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. 键入以下命令以将 FQDN 添加到你的应用程序：

        <kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd>

 其中**WebApplication1**是你的项目的名称和**bindingInformation**包含你想要用于测试的端口号和 FQDN。

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>如何获取 Microsoft 身份验证的应用程序设置

链接到 Windows Live 进行 Microsoft 身份验证的应用程序是一个简单的过程。 如果已未链接到 Windows Live 应用程序，你可以使用以下步骤：

1. 浏览到[https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070)并输入你的 Microsoft 帐户名和密码出现提示时，然后单击**登录**:

    [![](external-authentication-services/_static/image64.png "单击以展开映像")](external-authentication-services/_static/image63.png)
2. 输入的名称和出现提示时，应用程序的语言，然后单击**我接受**:

    [![](external-authentication-services/_static/image66.png "单击以展开映像")](external-authentication-services/_static/image65.png)
3. 上**API 设置**你的应用程序页上，输入你的应用程序和复制的重定向域**客户端 ID**和**客户端机密**为你的项目，然后单击**保存**:

    [![](external-authentication-services/_static/image68.png "单击以展开映像")](external-authentication-services/_static/image67.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>可选： 禁用本地注册

当前的 ASP.NET 本地注册功能不会阻止自动的程序 （机器人） 创建帐户; 的成员例如，通过使用一种 bot 防护和验证的技术，如[CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md)。 因此，应删除的登录页上的本地登录窗体和注册链接。 为此，请打开 *\_Login.cshtml*在项目中，页，然后注释掉的行本地登录面板和注册链接。 则结果页应如下所示下面的代码示例：

[!code-html[Main](external-authentication-services/samples/sample10.html)]

一旦已禁用本地登录面板和注册链接，登录页将仅显示已启用的外部身份验证提供程序：

[![](external-authentication-services/_static/image70.png "单击以展开映像")](external-authentication-services/_static/image69.png)
