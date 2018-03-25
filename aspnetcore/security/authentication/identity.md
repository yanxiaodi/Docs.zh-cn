---
title: "在 ASP.NET Core 上的标识简介"
author: rick-anderson
description: "与 ASP.NET Core 应用中使用的标识。 包括，设置密码要求 （RequireDigit、 RequiredLength、 RequiredUniqueChars 和的详细信息）。"
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity
ms.openlocfilehash: a84c5f1d4cf802ee0c4116d2a02bdbfbab9aa72b
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a>在 ASP.NET Core 上的标识简介

通过[Pranav Rastogi](https://github.com/rustd)， [Rick Anderson](https://twitter.com/RickAndMSFT)， [Tom Dykstra](https://github.com/tdykstra)，Jon Galloway[艾力克 Reitan](https://github.com/Erikre)，和[Steve Smith](https://ardalis.com/)

ASP.NET 核心标识是允许你向你的应用程序添加登录功能的成员身份系统。 用户可以创建帐户和登录名使用的用户名和密码或它们可以使用如 Facebook、 Google、 Microsoft 帐户、 Twitter 或其他外部登录提供程序。

你可以配置 ASP.NET 核心标识来使用 SQL Server 数据库来存储用户名、 密码和配置文件数据。 或者，你可以使用你自己的持久存储区，例如，Azure 表存储。 本文档包含用于 Visual Studio 以及有关使用 CLI 的说明。

[查看或下载的示例代码。](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [（如何下载）](https://docs.microsoft.com/aspnet/core/tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a>标识的概述

在本主题中，你将了解如何使用 ASP.NET 核心标识来添加功能，用于注册、 登录，并注销用户。 有关创建使用 ASP.NET 核心标识应用的更多详细说明，请参阅本文末尾的后续步骤部分。

1.  使用单个用户帐户创建一个 ASP.NET 核心 Web 应用程序项目。

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    在 Visual Studio 中，选择**文件** > **新建** > **项目**。 选择**ASP.NET 核心 Web 应用程序**单击**确定**。

    ![“新建项目”对话框](identity/_static/01-new-project.png)

    选择 ASP.NET Core **Web 应用程序 （模型-视图-控制器）** asp.net 核心 2.x，然后选择**更改身份验证**。

    ![“新建项目”对话框](identity/_static/02-new-project.png)

    出现一个对话框，产品/服务身份验证选项。 选择**单个用户帐户**单击**确定**以返回到上一个对话框。

    ![“新建项目”对话框](identity/_static/03-new-project-auth.png)

    选择**单个用户帐户**指示 Visual Studio 来创建模型、 Viewmodel、 视图、 控制器和其他资产所需的身份验证作为项目模板的一部分。

    # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

    如果使用.NET 核心 CLI，创建新的项目使用``dotnet new mvc --auth Individual``。 此命令创建一个新的项目与 Visual Studio 将创建相同的标识模板代码。

    创建的项目包含`Microsoft.AspNetCore.Identity.EntityFrameworkCore`包，其中的标识数据和 SQL Server 使用的架构仍然存在[实体框架核心](https://docs.microsoft.com/ef/)。

    ---

2.  配置标识服务，并添加中的中间件`Startup`。

    标识服务添加到中的应用程序`ConfigureServices`中的方法`Startup`类：

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]
    
    这些服务都提供给应用程序通过[依赖关系注入](xref:fundamentals/dependency-injection)。
    
    通过调用情况下，启用应用程序的标识`UseAuthentication`中`Configure`方法。 `UseAuthentication` 添加了身份验证[中间件](xref:fundamentals/middleware/index)向请求管道。
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]
    
    这些服务都提供给应用程序通过[依赖关系注入](xref:fundamentals/dependency-injection)。
    
    通过调用情况下，启用应用程序的标识`UseIdentity`中`Configure`方法。 `UseIdentity` 添加了基于 cookie 的身份验证[中间件](xref:fundamentals/middleware/index)向请求管道。
        
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    有关进程的应用程序启动的详细信息，请参阅[应用程序启动](xref:fundamentals/startup)。

3.  创建一个用户。

    启动应用程序，然后单击**注册**链接。

    如果这是你要执行此操作的第一个时间，你可能需要运行迁移。 应用程序会提示您**应用迁移**。 如果需要请刷新页面。
    
    ![将应用迁移网页](identity/_static/apply-migrations.png)
    
    或者，你可以测试与你的应用不持久的数据库的情况下通过使用内存中数据库的 ASP.NET 核心标识。 若要使用的内存中数据库，添加``Microsoft.EntityFrameworkCore.InMemory``包到你的应用程序和修改您的应用程序调用``AddDbContext``中``ConfigureServices``，如下所示：

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    当用户单击**注册**链接，``Register``上调用操作``AccountController``。 ``Register``操作通过调用创建用户`CreateAsync`上`_userManager`对象 (提供给``AccountController``通过依赖关系注入):
 
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    如果已成功创建用户，用户记录通过调用``_signInManager.SignInAsync``。

    **注意：**请参阅[帐户确认](xref:security/authentication/accconfirm#prevent-login-at-registration)有关步骤，以防止在注册的即时登录名。
 
4.  登录。
 
    用户可以通过单击登录**登录**链接顶部的站点，或可能的登录页导航它们，如果用户尝试访问要求获得授权的站点的一部分。 当用户提交的登录页中上, 窗体``AccountController````Login``调用操作。

    ``Login``操作调用``PasswordSignInAsync``上``_signInManager``对象 (提供给``AccountController``通过依赖关系注入)。

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    基``Controller``类会公开``User``你可以从控制器方法访问的属性。 例如，可以枚举`User.Claims`并做出授权决策。 有关详细信息，请参阅[授权](xref:security/authorization/index)。
 
5.  注销。
 
    单击**注销**链接调用`LogOut`操作。
 
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    前面的代码调用上面`_signInManager.SignOutAsync`方法。 `SignOutAsync`方法清除在 cookie 中存储的用户的声明。
 
<a name="pw"></a>
6.  配置。

    标识具有一些可以在应用程序的 startup 类中重写的默认行为。 `IdentityOptions` 无需使用的默认行为时配置。 下面的代码设置多个密码强度选项：

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

    ---
    
    有关如何配置标识的详细信息，请参阅[配置标识](xref:security/authentication/identity-configuration)。
    
    你还可以配置的数据类型的主键，请参阅[配置标识主键数据类型](xref:security/authentication/identity-primary-key-configuration)。
 
7.  查看数据库。

    如果你的应用使用 SQL Server 数据库 （默认在 Windows 上以及为 Visual Studio 用户），你可以查看数据库中创建的应用。 你可以使用**SQL Server Management Studio**。 或者，从 Visual Studio 中，选择**视图** > **SQL Server 对象资源管理器**。 连接到**(localdb) \MSSQLLocalDB**。 名称匹配的数据库**aspnet-<*的你的项目名称*>-<*日期字符串*>** 显示。

    ![在 AspNetUsers 数据库表的上下文菜单](identity/_static/04-db.png)
    
    展开的数据库并将其**表**，然后右键单击**dbo。AspNetUsers**表，然后选择**查看数据**。

8. 确认标识在有效运行

    默认值*ASP.NET 核心 Web 应用程序*项目模板，用户可以访问应用程序中的任何操作，而无到登录名。 若要验证 ASP.NET 标识工作原理，添加`[Authorize]`属性设为`About`操作`Home`控制器。
 
    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```
    
    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    运行项目使用**Ctrl** + **F5**并导航到**有关**页。 仅经过身份验证的用户可以访问**有关**现在，页，以便 ASP.NET 将你重定向到登录页来登录或注册。

    # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

    打开命令窗口并导航到项目的根目录包含`.csproj`文件。 运行[dotnet 运行](/dotnet/core/tools/dotnet-run)命令以运行应用程序：

    ```cs
    dotnet run 
    ```

    浏览的输出中指定的 URL [dotnet 运行](/dotnet/core/tools/dotnet-run)命令。 URL 应指向`localhost`与生成的端口号。 导航到**有关**页。 仅经过身份验证的用户可以访问**有关**现在，页，以便 ASP.NET 将你重定向到登录页来登录或注册。

    ---

## <a name="identity-components"></a>标识组件

标识系统的主引用程序集是`Microsoft.AspNetCore.Identity`。 此程序包包含 ASP.NET 核心标识接口的核心集，包括`Microsoft.AspNetCore.Identity.EntityFrameworkCore`。

这些依赖关系需要 ASP.NET Core 应用程序中使用的标识系统：

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore` -包含要用于实体框架核心标识所需的类型。

* `Microsoft.EntityFrameworkCore.SqlServer` 实体框架的核心是用于类似于 SQL Server 关系数据库的 Microsoft 的推荐使用此数据访问技术。 对于测试，你可以使用`Microsoft.EntityFrameworkCore.InMemory`。

* `Microsoft.AspNetCore.Authentication.Cookies` -使应用可以使用基于 cookie 的身份验证的中间。

## <a name="migrating-to-aspnet-core-identity"></a>迁移到 ASP.NET 核心标识

有关其他信息和指南迁移你现有的标识存储，请参阅[迁移身份验证和标识](xref:migration/identity)。

## <a name="setting-password-strength"></a>设置密码强度

请参阅[配置](#pw)有关设置的最小密码要求的示例。

## <a name="next-steps"></a>后续步骤

* [迁移身份验证和标识](xref:migration/identity)
* [帐户确认和密码恢复](xref:security/authentication/accconfirm)
* [使用 SMS 设置双因素身份验证](xref:security/authentication/2fa)
* [Facebook、 Google、 和外部提供程序身份验证](xref:security/authentication/social/index)
