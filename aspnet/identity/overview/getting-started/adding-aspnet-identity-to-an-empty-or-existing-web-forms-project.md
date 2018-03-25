---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: "添加 ASP.NET 标识设置为一个空的或现有的 Web 窗体项目 |Microsoft 文档"
author: raquelsa
description: "本教程演示如何将 ASP.NET 标识 （ASP.NET 的新成员资格系统） 添加到 ASP.NET 应用程序。 当你创建新的 Web 窗体或 MVC..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/23/2013
ms.topic: article
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 3ab67b93a32106c0b79f9e8d739d47835391edb5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>添加 ASP.NET 标识设置为一个空的或现有的 Web 窗体项目
====================
通过[Raquel Soares De Almeida](https://github.com/raquelsa)

> 本教程演示如何将添加[ASP.NET 标识](introduction-to-aspnet-identity.md)（ASP.NET 的新成员资格系统） 向 ASP.NET 应用程序。
> 
> 当你在 Visual Studio 2013 RTM 与单个帐户中创建新的 Web 窗体或 MVC 项目时，Visual Studio 将安装所需的所有包，并为您添加所有必需的类。 本教程将说明将 ASP.NET 身份验证支持添加到现有的 Web 窗体项目或新的空项目的步骤。 我将概述了所有 NuGet 包，你需要安装，以及你需要添加的类。 注册新用户和登录时突出显示的用户管理和身份验证的所有主入口点 Api，我将会通过示例 Web 窗体。 此示例将用于 SQL 数据存储在实体框架上生成 ASP.NET 标识默认实现。 本教程中，我们将使用 LocalDB 的 SQL 数据库。
> 
> 本教程已写入 Raquel Soares De Almeida 和 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。


## <a name="getting-started-aspnet-identity"></a>入门 ASP.NET 标识

1. 首先，安装并运行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。
2. 单击**新项目**从一开始页上，您可以使用菜单并选择**文件**，，然后**新项目**。
3. 选择**Visual C# i** n 左窗格中，然后**Web** ，然后选择**ASP.NET Web 应用程序**。 将你的项目"WebFormsIdentity"，然后单击**确定**。   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image1.png)
4. 在**新建 ASP.NET 项目**对话框中，选择**空**模板。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
 请注意**更改身份验证**按钮处于禁用状态和无身份验证支持提供此模板中。 Web 窗体、 MVC 和 Web API 模板允许您选择的身份验证方法。 有关详细信息，请参阅[的身份验证概述](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth)。

## <a name="adding-identity-packages-to-your-app"></a>将标识包添加到你的应用程序

在解决方案资源管理器，右键单击你的项目，然后选择**管理 NuGet 包**。 在搜索文本框对话框中，键入"*Identity.E*"。 单击安装此包。   
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image3.png)  
  
请注意，此包将安装依赖项包： EntityFramework 和 Microsoft ASP.NET Identity Core。

## <a name="adding-web-forms-to-register-users"></a>添加 Web 窗体来注册用户

1. 在**解决方案资源管理器**，右键单击你的项目，然后单击**添加**，，然后**Web 窗体**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. 在**指定项名称**对话框中，命名新的 web 窗体**注册**，然后单击**确定**
3. 将在生成标记*Register.aspx*文件替换为以下代码。 突出显示代码更改。   

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > 这是仅的简化的版本*Register.aspx*创建一个新的 ASP.NET Web 窗体项目时创建的文件。 上面的标记添加窗体字段和一个按钮以注册新的用户。
4. 打开*Register.aspx.cs*文件并将文件的内容替换为以下代码：

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. 上面的代码是的简化的版本*Register.aspx.cs*创建一个新的 ASP.NET Web 窗体项目时创建的文件。
    > 2. *IdentityUser*类是默认 EntityFramework 实现*IUser*接口。 *IUser*接口是 ASP.NET Identity Core 上的用户的最小接口。
    > 3. *UserStore*类是用户存储的默认 EntityFramework 实现。 此类实现 ASP.NET Identity Core 的最小接口： *IUserStore*， *IUserLoginStore*， *IUserClaimStore*和*IUserRoleStore*.
    > 4. *UserManager*与类公开用户相关的 Api 将自动将其保存到的更改*UserStore*。
    > 5. *IdentityResult*类表示标识操作的结果。
5. 在**解决方案资源管理器**，右键单击你的项目，然后单击**添加**，**添加 ASP.NET 文件夹**然后**应用\_数据**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. 打开*Web.config*文件并添加我们将用来存储用户信息的数据库的连接字符串条目。 数据库将在运行时通过 EntityFramework 为创建标识实体。 连接字符串是类似于在创建新的 Web 窗体项目时，为你创建的。 突出显示的代码演示了应添加的标记：

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > 有关 Visual Studio 2015 或更高版本，则替换`(localdb)\v11.0`与`(localdb)\MSSQLLocalDB`连接字符串中。
    
7. 右键单击文件*Register.aspx*中你的项目并选择**设为起始页**。 按 Ctrl + F5 生成并运行 web 应用程序。 输入新的用户名和密码，然后单击**注册**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET 标识具有对验证的支持，在此示例中，你可以验证用户和密码的默认行为来自标识核心包的验证程序。 用户的默认验证 (`UserValidator`) 都有一个属性`AllowOnlyAlphanumericUserNames`，其默认值设置为`true`。 密码的默认验证 (`MinimumLengthValidator`) 可确保该密码具有至少 6 个字符。 这些验证程序属性位于`UserManager`，如果你想要自定义验证，可以重写

## <a name="verifying-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>验证 LocalDb 标识数据库和表由实体框架生成

1. 在**视图**菜单上，单击**服务器资源管理器**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. 展开**DefaultConnection (WebFormsIdentity)**，展开**表**，右键单击**AspNetUsers**单击**显示表数据**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configuring-the-application-for-owin-authentication"></a>OWIN 身份验证的应用程序配置

此时我们仅添加了支持用于创建用户。 现在，我们将演示，我们可以如何添加身份验证登录用户。 ASP.NET 标识用于表单身份验证使用 Microsoft OWIN 身份验证中间件。 OWIN Cookie 身份验证是 cookie 和声明可由任何框架上托管的基于身份验证机制[OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx)或 IIS。 与此模型中，可以跨多个框架，包括 ASP.NET MVC 和 Web 窗体使用相同的身份验证包。 有关详细信息项目 Katana 以及如何运行主机不可知，请参阅中的中间件[Getting Started with Katana 项目](https://msdn.microsoft.com/magazine/dn451439.aspx)。

## <a name="installing-authentication-packages-to-your-application"></a>将身份验证程序包安装到你的应用程序

1. 在解决方案资源管理器，右键单击你的项目，然后选择**管理 NuGet 包**。 在搜索文本框对话框中，键入"*Identity.Owin*"。 单击安装此包。   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image10.png)
2. 搜索包***Microsoft.Owin.Host.SystemWeb***并将其安装。   

    > [!NOTE]
    > **Microsoft.Aspnet.Identity.Owin**包包含 OWIN 扩展类，用于管理和配置 OWIN 身份验证中间件将使用 ASP.NET 标识核心包的一组。  
    > **Microsoft.Owin.Host.SystemWeb**包包含使基于 OWIN 的应用程序可以在使用 ASP.NET 请求管道的 IIS 上运行的 OWIN 服务器。 有关详细信息请参阅[在 IIS 中的 OWIN 中间件集成管道](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md)。

## <a name="adding-owin-startup-and-authentication-configuration-classes"></a>添加 OWIN 启动和身份验证配置类

1. 在**解决方案资源管理器**，右键单击你的项目，单击**添加**，，然后**添加新项**。 在搜索文本框对话框中，键入"*owin*"。 将类"*启动*"，然后单击**添加**。   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. 在 Startup.cs 文件中，添加突出显示的代码下面显示配置 OWIN cookie 身份验证。

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > 此类包含`OwinStartup`指定 OWIN 启动类的属性。 每个 OWIN 应用程序具有一个 startup 类，其中您可以指定应用程序管道的组件。 请参阅[OWIN 启动类检测](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md)有关此模型的详细信息。

## <a name="adding-web-forms-for-registering-and-logging-in-users"></a>添加用于注册和登录用户的 Web 窗体

1. 打开*Register.cs*文件并添加以下代码以将时的登录用户注册成功。 下面突出显示所做的更改。

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - ASP.NET 标识和 OWIN Cookie 身份验证是基于声明的系统事件，因为框架需要应用程序开发人员生成[ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx)用户。 ClaimsIdentity 具有的用户属于哪些角色如用户的所有声明的信息。 在此阶段，你还可以添加用户的多个的声明。
    > - 你可以在登录用户通过使用从 OWIN 和调用 AuthenticationManager`SignIn`并传入 ClaimsIdentity 如上所示。 此代码将在用户登录，并生成一个 cookie 以及。 此调用是类似于[FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx)由[FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)模块。
2. 在**解决方案资源管理器**，右键单击项目单击**添加**，，然后**Web 窗体**。 将 web 窗体**登录**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. 内容替换*Login.aspx*文件替换为以下代码：  

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. 内容替换*Login.aspx.cs*替换为以下文件：  

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - `Page_Load`现在检查当前用户的状态并可采取措施基于其`Context.User.Identity.IsAuthenticated`状态。  
    >     **在用户名中显示已登录**： 的 Microsoft ASP.NET Identity Framework 已添加的扩展方法上, [System.Security.Principal.IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) ，使你能获取`UserName`和`UserId`为在已登录的用户。 这些扩展方法定义中`Microsoft.AspNet.Identity.Core`程序集。 这些扩展方法是为替换[HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) 。
    > - 登录方法：   
    >     `This`方法取代了以前`CreateUser_Click`在此示例和现在登录已成功创建用户之后用户的方法。   
    >  Microsoft OWIN Framework 已添加的扩展方法上, `System.Web.HttpContext` ，你能够获取对引用`IOwinContext`。 这些扩展方法定义中`Microsoft.Owin.Host.SystemWeb`程序集。 `OwinContext`类会公开`IAuthenticationManager`表示可对当前请求的身份验证中间件功能的属性。  
    >  可以通过使用来在用户登录`AuthenticationManager`从 OWIN 和调用`SignIn`和传入`ClaimsIdentity`如上所示。   
    >  因为 ASP.NET 标识和 OWIN Cookie 身份验证是基于声明的系统，框架需要应用程序生成`ClaimsIdentity`用户。   
    >  `ClaimsIdentity`具有的用户，如用户所属的角色的所有声明的信息。 你还可以在此阶段添加用户的多个的声明  
    >  此代码将在用户登录，并生成一个 cookie 以及。 此调用是类似于[FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx)由[FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)模块。
    > - `SignOut`方法：   
    >  获取对`AuthenticationManager`从 OWIN 和调用`SignOut`。 这是类似于[FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx)方法，由[FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)模块。
5. 按**Ctrl + F5**生成并运行 web 应用程序。 输入新的用户名和密码，然后单击**注册**。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
 注意： 此时，新用户创建并登录。
6. 单击**注销**按钮。你将重定向到窗体中的日志。
7. 输入无效的用户名称或密码，然后单击上**登录**按钮。   
 `UserManager.Find`方法将返回 null 和错误消息:"*无效的用户名或密码*"将显示。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
