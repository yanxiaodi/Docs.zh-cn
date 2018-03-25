---
title: "迁移的身份验证和标识到 ASP.NET 核心"
author: ardalis
description: "了解如何将身份验证和标识从 ASP.NET MVC 项目迁移到 ASP.NET 核心 MVC 项目。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/identity
ms.openlocfilehash: bf452ad3969863f8f058b29a31f19af13cb2fc6b
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="migrating-authentication-and-identity-to-aspnet-core"></a>迁移的身份验证和标识到 ASP.NET 核心

<a name="migration-identity"></a>

作者：[Steve Smith](https://ardalis.com/)

在前面文章我们[配置从 ASP.NET MVC 项目迁移到 ASP.NET 核心 MVC](configuration.md)。 在本文中，我们将迁移的注册、 登录名和用户管理功能。

## <a name="configure-identity-and-membership"></a>配置的标识和成员身份

在 ASP.NET MVC 中，使用 ASP.NET 标识 Startup.Auth.cs 和 IdentityConfig.cs，位于 App_Start 文件夹中配置身份验证和标识功能。 在 ASP.NET 核心 MVC，这些功能在中配置*Startup.cs*。

安装`Microsoft.AspNetCore.Identity.EntityFrameworkCore`和`Microsoft.AspNetCore.Authentication.Cookies`NuGet 包。

然后，打开 Startup.cs 并更新`ConfigureServices()`要使用实体框架和标识服务的方法：

```csharp
public void ConfigureServices(IServiceCollection services)
{
  // Add EF services to the services container.
  services.AddEntityFramework(Configuration)
    .AddSqlServer()
    .AddDbContext<ApplicationDbContext>();

  // Add Identity services to the services container.
  services.AddIdentity<ApplicationUser, IdentityRole>(Configuration)
    .AddEntityFrameworkStores<ApplicationDbContext>();

  services.AddMvc();
}
```

在这种情况下，有两种在上面的代码中，我们尚未尚未迁移从 ASP.NET MVC 项目中引用的类型：`ApplicationDbContext`和`ApplicationUser`。 创建一个新*模型*文件夹中 ASP.NET Core 项目，并将两个类添加到它对应于这些类型。 你将找到 ASP.NET MVC 的这些类中的版本`/Models/IdentityModels.cs`，但我们将使用每个已迁移的项目中的类的一个文件，因为它是更清晰。

ApplicationUser.cs:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

ApplicationDbContext.cs:

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvc6Project.Models
{
  public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
  {
    public ApplicationDbContext()
    {
      Database.EnsureCreated();
    }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
      options.UseSqlServer();
    }
  }
}
```

ASP.NET 核心 MVC 初学者 Web 项目不包括的用户，多自定义项或 ApplicationDbContext。 当迁移实际的应用程序时，将需要迁移的所有自定义属性和方法的应用程序的用户和 DbContext 类，以及任何其他应用程序使用 （例如，如果你 DbContext 具有 DbSet的模型类<Album>，当然你将需要迁移唱片集类)。

使用就地这些文件，Startup.cs 文件可用于通过更新其使用编译语句：

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

我们的应用程序现已准备好支持身份验证和标识服务-它只需要向用户公开这些功能。

## <a name="migrate-registration-and-login-logic"></a>迁移注册和登录名逻辑

标识配置的服务应用程序和数据访问使用 Entity Framework 和 SQL Server 配置，我们现在就将支持注册和登录名添加到应用程序。 回想一下，[前面的迁移过程](mvc.md#migrate-layout-file)我们注释掉对 _LoginPartial _Layout.cshtml 中的引用。 现在，它是返回到该代码中，取消注释它，并在必要的控制器视图和视图，以支持登录功能将添加的时间。

更新 _Layout.cshtml;取消注释@Html.Partial行：

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

现在，添加新的 MVC 视图页调用 _LoginPartial 到视图/共享文件夹：

替换为以下代码更新 _LoginPartial.cshtml （替换其内容的所有）：

```cshtml
@inject SignInManager<User> SignInManager
@inject UserManager<User> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="LogOff" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log off</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

此时，你应能够刷新浏览器中的站点。

## <a name="summary"></a>摘要

ASP.NET 核心引入 ASP.NET 标识功能更改。 在本文中，你看到了如何将 ASP.NET 标识的身份验证和用户管理功能迁移到 ASP.NET 核心。
