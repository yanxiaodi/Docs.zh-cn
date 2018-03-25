---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: "开发使用 Azure Active Directory 的 ASP.NET 应用程序 |Microsoft 文档"
author: Rick-Anderson
description: "Microsoft ASP.NET tools for Azure Active Directory 可以方便地为在 Azure 上托管的 web 应用程序启用身份验证。 你也可以使用 Azure 身份验证..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2014
ms.topic: article
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 1ef0468d5f5c17480b23ac88983f30fe6f4979c0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a>开发使用 Azure Active Directory 的 ASP.NET 应用程序
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> Microsoft ASP.NET tools 为 Azure Active Directory 可以很容易地为上托管的 web 应用程序启用身份验证[Azure](https://www.windowsazure.com/home/features/web-sites/)。 可以使用 Azure 身份验证从组织、 从本地 Active Directory 同步的公司帐户或用户在你自己的自定义 Azure Active Directory 域中创建的 Office 365 用户进行身份验证。 启用 Windows Azure 身份验证配置你的应用程序使用单个的用户进行身份验证[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)租户。
> 
>  本教程编写由 Rick Anderson[@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)


本教程将演示如何创建的 ASP.NET 应用程序配置为使用登录[Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD)。 你还将了解如何调用 Graph API，以获取有关当前登录的用户的信息以及如何部署应用程序到 Azure。

## <a name="prerequisites"></a>系统必备

1. [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express)或[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)。
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) -3 或更高版本的更新是必需的。
3. 一个 Azure 帐户。 [单击此处](https://azure.microsoft.com/pricing/free-trial/)免费试用版，如果你还没有帐户。

## <a name="add-a-global-administrator-to-your-active-directory"></a>将全局管理员添加到你的 Active Directory

1. 登录到[Azure 管理门户](https://manage.windowsazure.com/)。
2. 所有 Azure 帐户包含**默认目录**-单击它，然后单击**用户**页顶部的选项卡 （请参阅下图所示）。
3. 单击添加用户。  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. 创建的新用户**全局管理员**角色。 单击**用户**从顶部菜单中，然后单击**添加用户**命令栏上的按钮。
5. 在**添加用户**对话框中，输入新用户的名称，然后单击右箭头。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. 输入的用户名和角色设置为**全局管理员**。 全局管理员的密码恢复目的需要备用电子邮件地址。 完成后后，单击右箭头。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. 在对话框的下一页上，单击**创建**。 将为新用户创建和对话框中显示临时密码。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)  
  
 保存密码，你将需要在首次登录后更改的密码。 下图显示了新的管理员帐户。 你必须使用 Azure Active Directory 登录到你的应用程序，不也显示此页上的 Microsoft 帐户。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>创建 ASP.NET 应用程序

以下步骤使用[Visual Studio Express 2013 for Web](https://www.microsoft.com/download/details.aspx?id=40747)，并且需要[Visual Studio 2013 Update 3](https://www.microsoft.com/download/details.aspx?id=43721)。

1. 在 Visual Studio 中，单击**文件**然后**新项目**。 上**新项目**对话框中，选择 Visual C# Web 项目从左侧菜单，然后单击**确定**。 你可能还想取消选中**向项目添加 Application Insights**如果你不希望你的应用程序的功能。
2. 在**新建 ASP.NET 项目**对话框中，选择**MVC**，然后单击**更改身份验证**。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. 上**更改身份验证**对话框中，选择**组织帐户**。 这些选项可以用于自动向 Azure AD 中注册你的应用程序，以及自动配置你的应用程序与 Azure AD 集成。 无需使用**更改身份验证**对话框来注册和配置你的应用程序，但它可以更轻松。 如果正在使用 Visual Studio 2012，例如所示，可以仍手动在 Azure 管理门户中注册应用程序和更新其配置以与 Azure AD 集成。  
 在下拉列表菜单中，选择**云-单个组织**和**单一登录，读取目录数据**。 输入你的 Azure AD 目录，例如 （在下面的映像） 的域*aricka0yahoo.onmicrosoft.com*，然后单击**确定**。 可以在 azure 门户的默认目录从域选项卡中获取的域名 （向下，请参阅下一步的映像）。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)  
  
 下图显示在 Azure 门户中的域名。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)  

    > [!NOTE]
    > 你可以选择配置通过单击将在 Azure AD 中注册应用程序 ID URI**更多选项**。 应用程序 ID URI 是应用程序，这是在 Azure AD 中注册并且应用程序用于与 Azure AD 通信时自身标识的唯一标识符。 有关应用程序 ID URI 和已注册应用程序的其他属性的详细信息，请参阅[本主题](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering)。 通过单击应用程序 ID URI 字段的下方的复选框，你还可以选择在使用相同的应用程序 ID URI 的 Azure AD 中覆盖现有注册。
4. 单击后**确定**，将显示一个登录对话框，并且将需要使用全局管理员帐户 （不与你的订阅关联的 Microsoft 帐户） 登录。 如果你之前创建新的管理员帐户，你将需要更改密码并再次使用新密码，然后登录。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. 你已成功通过身份验证后，**新建 ASP.NET 项目**对话框将显示你的身份验证选择 (**组织**) 和新的应用程序将在其中的目录注册 (*aricka0yahoo.onmicrosoft.com*图所示)。 此信息，下面选择标记为复选框**在云中托管**。 如果选择此复选框，则项目将设置为 Azure web 应用和将启用用于轻松发布更高版本。 单击 **“确定”**。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. **配置 Azure 网站**对话框将出现，并使用自动生成站点名称和区域。 另请注意你当前登录对话框中的帐户。 你想要确保此帐户是一个你的 Azure 订阅连接到，通常的 Microsoft 帐户。

    > [!NOTE]
    > 此项目需要一个数据库。 你需要选择一个现有的数据库，或创建一个新。 数据库是必需的因为该项目已使用本地数据库文件存储少量的身份验证配置数据。 在部署到 Azure 网站应用程序时，此数据库未附带部署，因此你需要选择一个可访问位于云中。 单击 **“确定”**。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. 将创建该项目，并且你的身份验证选项和 web 应用程序选项将自动配置与项目。 此过程完成后，通过按以本地方式运行该项目**^ F5**。 你将需要使用你的组织帐户进行登录。 为前面创建的帐户提供的用户名和密码，单击**登录**。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. 在成功登录之后, 将显示 ASP.NET 站点，您已通过在页面右上角中显示用户名身份验证。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)  
  
 如果收到错误：  
 值不能为 null 或为空。 参数名称： linkText   
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)  
  
 请参阅[调试](#dbg)在本教程末尾的部分。

## <a name="basics-of-the-graph-api"></a>Graph API 基础知识

[Graph API](https://msdn.microsoft.com/library/azure/hh974476.aspx)是用于在你的 Azure AD 目录中执行 CRUD 和其他操作在对象上的编程接口。 如果你选择用于身份验证的组织帐户选项，在 Visual Studio 2013 中创建新项目时，已将配置应用程序以调用 Graph API。 本部分简要说明 Graph API 的工作原理。

1. 在你运行的应用程序，单击顶部的登录用户名称页面右上角。 这将转到用户配置文件页上，这是在主页控制器上的操作。 你会注意到表中包含你前面创建的管理员帐户有关的用户信息。 此信息存储在你的目录，并调用图形 API 来加载页面时检索此信息。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. 返回到 Visual Studio 并展开**控制器**文件夹，然后打开**HomeController.cs**文件。 你将看到**UserProfile()**包含代码，以检索令牌，然后调用 Graph API 的操作。 下面被复制此代码： 

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    若要调用 Graph API，首先需要检索令牌。 检索令牌时，必须在对 Graph API 的所有后续请求的 Authorization 标头中附加其字符串值。 大部分上面的代码处理对 Azure AD，以获取令牌进行身份验证，使用令牌调用 Graph API，以及然后转换响应，以便它可以出现在视图中的详细信息。

    有关讨论最相关的部分是以下突出显示的行将： `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`。 这条线表示的用户，这就已反序列化 JSON 响应中，会显示在视图中的名称。

    您可以调用 Graph API 使用 HttpClient 自己并处理原始数据，但更简单的方法是使用[Graph 客户端库，该库可通过 NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/)。 客户端库处理原始 HTTP 请求和返回的数据为你的转换，并使其可以更轻松地使用 Graph API 在.NET 环境中工作。 请参阅上相关的 Graph API 代码示例[GitHub。](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>部署到 Azure 应用程序

以下步骤将演示如何部署到 Azure 应用程序。 在前面的步骤中，你将新项目连接与 web 应用程序在 Azure 上，以便随时可在几个步骤中发布。

1. 在 Visual Studio 中，右键单击项目并选择**发布**。 **发布 Web**对话框将显示与每个已配置的设置。 单击**下一步**按钮转到**设置**页。 可能会提示你进行身份验证;请确保你进行身份验证使用你的 Azure 订阅帐户 （通常是 Microsoft 帐户） 而不是前面创建的组织帐户。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. 检查**启用组织身份验证**选项。 在**域**字段中，输入你的目录的域。 从**访问级别**下拉列表中，选择**单一登录，读取目录数据**。 你会注意到你使用的上一个数据库在已填充**数据库**部分。 单击“发布” 。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio 将会开始部署你的网站，，然后将出现一个新的浏览器窗口。 你可能会提示重新进行身份验证对你的目录一次。 一旦你已通过身份验证，你将重定向到新发布的网站在 Azure 上。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>调试应用程序

如果你收到以下错误：   
 值不能为 null 或为空。 参数名称： linkText   
   
![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)  
  

中的代码替换*views/shared\\_LoginPartial.cshtml*替换为以下文件：

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

后运行该应用程序，如果在已登录的用户显示"Null 用户"，请注销，然后重新登录之前创建的 Active Directory 帐户。

要遵循的绝佳教程是 Rick Rainey[深入了解： Azure 网站和组织使用 Azure AD 的身份验证](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)。

## <a name="more-information"></a>详细信息

- [深入探讨： 在 Azure 网站和组织使用 Azure AD 的身份验证](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Azure AD Graph API 概述](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Azure AD 中的身份验证方案](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [GitHub 上的 azure AD 代码示例](https://github.com/AzureADSamples)
