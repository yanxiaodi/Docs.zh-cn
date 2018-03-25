---
title: "区域"
author: rick-anderson
description: "演示如何使用区域。"
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/areas
ms.openlocfilehash: 1ade49de3f6c58edc4ea7b06bc593b3db797081c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="areas"></a>区域

作者：[Dhananjay Kumar](https://twitter.com/debug_mode) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

区域是 ASP.NET MVC 功能，用于将相关功能以单独的名称空间（用于路由）和文件夹结构（用于视图）的形式组织到一个组中。 使用区域，将通过为 `controller` 和 `action` 添加另一个路由参数 `area`，创建用于路由目的的层次结构。

区域提供了将大型 ASP.NET Core MVC Web 应用分区为较小功能分组的方法。 区域实际上是应用程序内的一个 MVC 结构。 在 MVC 项目中，模型、控制器和视图等逻辑组件保存在不同的文件夹中，MVC 使用命名约定来创建这些组件之间的关系。 对于大型应用，将应用分区为独立的高级功能区域可能更有利。 例如，具有多个业务单位（如结账、计费、搜索等）的电子商务应用。每个单位都有自己的逻辑组件视图、控制器和模型。 在此方案中，可使用区域对同一项目中的业务组件进行物理分区。

区域可以说是 ASP.NET Core MVC 项目中的较小功能单位，它具有自己的一组控制器、视图和模型。

以下情况，请考虑在 MVC 项目中使用区域：

* 应用程序由应进行逻辑区分的多个高级功能组件组成

* 想对 MVC 项目进行分区，使每个功能区域可以独立工作

区域特性：

* 一个 ASP.NET Core MVC 应用可以有任意数量的区域

* 每个区域都有自己的控制器、模型和视图

* 可用于将大型 MVC 项目组织为可以独立工作的多个高级组件

* 支持具有相同名称的多个控制器 - 只要它们具有不同的区域

我们来看看演示如何创建和使用区域的示例。 假设你有一个商店应用，它有两组不同的控制器和视图：产品和服务。 使用 MVC 区域的典型文件夹结构如下所示：

* 项目名称

  * 区域

    * 产品

      * Controllers

        * HomeController.cs

        * ManageController.cs

      * 视图

        * 主页

          * Index.cshtml

        * 管理

          * Index.cshtml

    * 服务

      * Controllers

        * HomeController.cs

      * 视图

        * 主页

          * Index.cshtml

当 MVC 尝试呈现 Area 中的视图时，它将默认尝试检查以下位置：

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

以上是默认位置，可以通过 `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions` 上的 `AreaViewLocationFormats` 进行更改。

例如，在下面的代码中，文件夹名称不是“Area”，而是改为了“Categories”。

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

值得注意的一点是，这里唯独看重的是 Views 文件夹的结构，而 Controllers 和 Models 等其他文件夹则无关紧要。 例如，完全可以不设 Controllers 和 Models 文件夹。 这是可行的，因为 Controllers 和 Models 的内容只是请求相应视图时才会编译到 .dll 中的代码，而 Views 的内容则不是。

定义文件夹层次结构后，需告知 MVC 每个控制器与一个区域相关联。 可使用 `[Area]` 属性修饰控制器名称，来完成此操作。

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

设置适用于新创建区域的路由定义。 [路由到控制器操作](routing.md)文章详细介绍了如何创建路由定义，包括使用常规路由和属性路由。 在此示例中，我们将使用常规路由。 为此，请打开 Startup.cs 文件，并通过添加以下名为 `areaRoute` 的路由定义修改文件。

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

浏览到 `http://<yourApp>/products`，将调用 `Products` 区域中 `HomeController` 的 `Index` 操作方法。

## <a name="link-generation"></a>链接生成

* 生成从基于区域的控制器内的动作到同一控制器内另一个动作的链接。

  假设当前的请求路径类似于 `/Products/Home/Create`

  HtmlHelper 语法：`@Html.ActionLink("Go to Product's Home Page", "Index")`

  TagHelper 语法：`<a asp-action="Index">Go to Product's Home Page</a>`

  请注意，我们不需要提供“area”和“controller”值，因为它们在当前请求的上下文中已经可用。 此类值称为 `ambient` 值。

* 生成从基于区域的控制器内的动作到不同控制器上另一个动作的链接

  假设当前的请求路径类似于 `/Products/Home/Create`

  HtmlHelper 语法：`@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`

  TagHelper 语法：`<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  请注意，此处使用“area”的环境值，但上面显式指定了“controller”值。

* 生成从基于区域的控制器内的动作到不同区域中不同控制器上另一个动作的链接。

  假设当前的请求路径类似于 `/Products/Home/Create`

  HtmlHelper 语法：`@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`

  TagHelper 语法：`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`

  请注意，此处不使用环境值。

* 生成从基于区域的控制器内的动作到不在区域中的不同控制器上另一个动作的链接。

  HtmlHelper 语法：`@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`

  TagHelper 语法：`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  由于我们要生成到基于非区域的控制器操作的链接，因此在此清空“area”的环境值。

## <a name="publishing-areas"></a>发布区域

当 .csproj 文件中包含 `<Project Sdk="Microsoft.NET.Sdk.Web">` 时，所有 `*.cshtml` 和 `wwwroot/**` 文件将发布到输出。
