---
title: "从 ASP.NET MVC 迁移到 ASP.NET 核心 MVC"
author: ardalis
description: "了解如何开始迁移到 ASP.NET 核心 MVC ASP.NET MVC 项目。"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: c9c9f63cd635f364d9b2e081dc051a46a44d3e4f
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a>从 ASP.NET MVC 迁移到 ASP.NET 核心 MVC

通过[Rick Anderson](https://twitter.com/RickAndMSFT)， [Daniel Roth](https://github.com/danroth27)， [Steve Smith](https://ardalis.com/)，和[Scott Addie](https://scottaddie.com)

这篇文章演示如何开始迁移到 ASP.NET MVC 项目[ASP.NET 核心 MVC](../mvc/overview.md)。 在过程中，会突出显示许多已更改利用 ASP.NET MVC 的内容。 从 ASP.NET MVC 迁移是一个多步骤过程，本文涵盖初始安装程序、 基本控制器和视图、 静态内容和客户端依赖关系。 其他文章涵盖迁移配置和标识代码在许多 ASP.NET MVC 项目中找到。

> [!NOTE]
> 这些示例中的版本号可能不是最新。 你可能需要相应地更新你的项目。

## <a name="create-the-starter-aspnet-mvc-project"></a>创建初学者 ASP.NET MVC 项目

为了演示升级，我们将开始通过创建 ASP.NET MVC 应用程序。 创建同名*WebApp1*使命名空间将与我们在下一步中创建 ASP.NET Core 项目相匹配。

![Visual Studio 的新项目对话框](mvc/_static/new-project.png)

![新建 Web 应用程序对话框： 在 ASP.NET 模板面板中选择 MVC 项目模板](mvc/_static/new-project-select-mvc-template.png)

*可选：*更改从解决方案的名称*WebApp1*到*Mvc5*。 Visual Studio 将显示新的解决方案名称 (*Mvc5*)，这会使它更轻松地判断此项目从下一步的项目。

## <a name="create-the-aspnet-core-project"></a>创建 ASP.NET Core 项目

创建一个新*空*ASP.NET Core 与以前的项目同名的 web 应用 (*WebApp1*) 以便将两个项目中的命名空间匹配。 使用相同的命名空间，可更轻松地复制两个项目之间的代码。 你将需要在比以前的项目以使用相同的名称不同的目录中创建此项目。

![“新建项目”对话框](mvc/_static/new_core.png)

![新建 ASP.NET Web 应用程序对话框： 在 ASP.NET 核心模板面板中选择的空项目模板](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *可选：*创建新的 ASP.NET Core 应用使用*Web 应用程序*项目模板。 将项目*WebApp1*，然后选择的一个身份验证选项**单个用户帐户**。 重命名此应用程序到*FullAspNetCore*。 在转换过程中创建此项目将保存您的时间。 你可以查看若要查看的最终结果或将代码复制到转换项目模板生成的代码。 它也是很有帮助时遇到困难在转换步骤中，要与模板生成项目进行比较。

## <a name="configure-the-site-to-use-mvc"></a>配置站点以使用 MVC

* 安装`Microsoft.AspNetCore.Mvc`和`Microsoft.AspNetCore.StaticFiles`NuGet 包。

  `Microsoft.AspNetCore.Mvc` 是 ASP.NET 核心 MVC 框架。 `Microsoft.AspNetCore.StaticFiles` 是静态文件处理程序。 是一个模块化，ASP.NET 运行时，你必须显式选择中提供静态文件 (请参阅[使用静态文件](../fundamentals/static-files.md))。

* 打开*.csproj*文件 (右键单击中的项目**解决方案资源管理器**和选择**编辑 WebApp1.csproj**) 并添加`PrepareForPublish`目标：

  [!code-xml[](mvc/sample/WebApp1.csproj?range=21-23)]

  `PrepareForPublish`目标所需的获取通过 Bower 的客户端库。 我们将讨论的更高版本。

* 打开*Startup.cs*文件并将更改代码以匹配以下内容：

  [!code-csharp[](mvc/sample/Startup.cs?highlight=14,27-34)]

  `UseStaticFiles`扩展方法将添加静态文件处理程序。 如前所述，ASP.NET 运行时是一个模块化，和中，你必须显式选择要为静态文件服务。 `UseMvc`扩展方法将添加路由。 有关详细信息，请参阅[应用程序启动](../fundamentals/startup.md)和[路由](../fundamentals/routing.md)。

## <a name="add-a-controller-and-view"></a>添加控制器和视图

在本部分中，你将添加一个最小控制器和视图，以用作占位符的 ASP.NET MVC 控制器和视图将在下一部分中迁移。

* 添加*控制器*文件夹。

* 添加**MVC 控制器类**同名*HomeController.cs*到*控制器*文件夹。

![“添加新项”对话框](mvc/_static/add_mvc_ctl.png)

* 添加*视图*文件夹。

* 添加*视图/主页*文件夹。

* 添加*Index.cshtml*到 MVC 视图页*视图/主页*文件夹。

![“添加新项”对话框](mvc/_static/view.png)

项目结构如下所示：

![显示文件和文件夹的 WebApp1 的解决方案资源管理器](mvc/_static/project-structure-controller-view.png)

内容替换*Views/Home/Index.cshtml*替换为以下文件：

```html
<h1>Hello world!</h1>
```

运行应用。

![Web 应用程序在 Microsoft Edge 中打开](mvc/_static/hello-world.png)

请参阅[控制器](xref:mvc/controllers/actions)和[视图](xref:mvc/views/overview)有关详细信息。

现在，我们已最小的工作 ASP.NET Core 项目，我们可以开始从 ASP.NET MVC 项目迁移功能。 我们将需要将以下：

* 客户端内容 （CSS、 字体和脚本）

* 控制器

* 视图

* 模型

* 绑定

* 筛选器

* 输入/输出的日志，标识 （这将在执行下一步的教程。）

## <a name="controllers-and-views"></a>控制器和视图

* 将每个方法复制利用 ASP.NET MVC`HomeController`对新`HomeController`。 请注意，在 ASP.NET MVC 内置模板的控制器操作方法的返回类型[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); 在 ASP.NET 核心 MVC，操作方法返回`IActionResult`相反。 `ActionResult` 实现`IActionResult`，因此无需更改你的操作方法的返回类型。

* 复制*About.cshtml*， *Contact.cshtml*，和*Index.cshtml* Razor 视图文件从 ASP.NET MVC 项目添加到 ASP.NET 核心项目。

* 运行 ASP.NET Core 应用和测试每个方法。 我们尚未尚未进行迁移的布局文件或样式，因此呈现的视图将仅包含视图文件中的内容。 不会有的布局生成文件链接`About`和`Contact`视图，因此你将需要从浏览器中调用它们 (替换**4492**与你的项目中使用的端口号)。

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![联系人页面](mvc/_static/contact-page.png)

请注意在缺乏样式和菜单项。 此问题将在下一部分得以解决。

## <a name="static-content"></a>静态内容

在以前版本的 ASP.NET MVC，静态内容已承载 web 项目的根目录中，已使用服务器端文件混合。 在 ASP.NET Core 静态内容承载于*wwwroot*文件夹。 你将想要复制的静态内容从旧 ASP.NET MVC 应用程序到*wwwroot* ASP.NET Core 项目文件夹中的。 在此示例转换：

* 复制*favicon.ico*文件从旧的 MVC 项目到*wwwroot* ASP.NET Core 项目文件夹中的。

旧 ASP.NET MVC 项目使用[Bootstrap](http://getbootstrap.com/)其 Bootstrap 文件中的样式和存储*内容*和*脚本*文件夹。 该模板后，生成旧的 ASP.NET MVC 项目，该引用布局文件中的 Bootstrap (*Views/Shared/_Layout.cshtml*)。 你也可以将复制*bootstrap.js*和*bootstrap.css*文件从 ASP.NET MVC 项目合并为*wwwroot*文件夹中新的项目中，但这种方法不使用用于管理 ASP.NET Core 中的客户端依赖项的改进的机制。

在新的项目中，我们将添加对 Bootstrap （和其他客户端库），它使用支持[Bower](https://bower.io/):

* 添加[Bower](https://bower.io/)名为配置文件*bower.json*与项目根目录 (在项目中，右键单击，然后**添加 > 新项 > Bower 配置文件**)。 添加[Bootstrap](http://getbootstrap.com/)和[jQuery](https://jquery.com/)文件 （请参阅下面突出显示的行）。

  [!code-json[](mvc/sample/bower.json?highlight=5-6)]

在保存文件，Bower 将自动下载到的依赖关系*wwwroot/lib*文件夹。 你可以使用**搜索解决方案资源管理器**框查找资产的路径：

![解决方案资源管理器搜索结果中显示的 jquery 资产](mvc/_static/search.png)

请参阅[Bower 与管理客户端包](../client-side/bower.md)有关详细信息。

<a name="migrate-layout-file"></a>

## <a name="migrate-the-layout-file"></a>迁移布局文件

* 复制*_ViewStart.cshtml*文件从旧的 ASP.NET MVC 项目*视图*文件夹导入到 ASP.NET 核心项目*视图*文件夹。 *_ViewStart.cshtml*文件未更改 ASP.NET 核心 mvc。

* 创建*视图/共享*文件夹。

* *可选：*复制*_ViewImports.cshtml*从*FullAspNetCore* MVC 项目*视图*文件夹导入到 ASP.NET 核心项目*视图*文件夹。 删除中的任何命名空间声明*_ViewImports.cshtml*文件。 *_ViewImports.cshtml*文件对于视图的所有文件提供命名空间，并使[标记帮助程序](xref:mvc/views/tag-helpers/intro)。 新的布局文件中使用标记帮助程序。 *_ViewImports.cshtml*文件是用于 ASP.NET 核心新功能。

* 复制*_Layout.cshtml*文件从旧的 ASP.NET MVC 项目*视图/共享*文件夹导入到 ASP.NET 核心项目*视图/共享*文件夹。

打开*_Layout.cshtml*文件并进行以下更改 （已完成的代码下面显示）：

   * 替换`@Styles.Render("~/Content/css")`与`<link>`元素加载*bootstrap.css* （见下文）。

   * 删除 `@Scripts.Render("~/bundles/modernizr")`。

   * 注释掉`@Html.Partial("_LoginPartial")`行 (括在一行时与`@*...*@`)。 在将来的教程，我们会返回到它。

   * 替换`@Scripts.Render("~/bundles/jquery")`与`<script>`元素 （见下文）。

   * 替换`@Scripts.Render("~/bundles/bootstrap")`与`<script>`元素 （见下文）...

替换 CSS 链接：

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

替换脚本标记：

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

已更新*_Layout.cshtml*文件如下所示：

[!code-html[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

在浏览器中查看站点。 它应现在正确加载，以就地预期的样式。

* *可选：*可能想要尝试使用新的布局文件。 对于此项目中，你可以复制中的布局文件*FullAspNetCore*项目。 新的布局文件使用[标记帮助程序](xref:mvc/views/tag-helpers/intro)并且具有其他的改进功能。

## <a name="configure-bundling-and-minification"></a>配置绑定和缩减

有关如何配置绑定和缩减的信息，请参阅[捆绑和缩减](../client-side/bundling-and-minification.md)。

## <a name="solving-http-500-errors"></a>解决 HTTP 500 错误

有许多问题的包含没有关于此问题的源的信息可能会导致 HTTP 500 错误消息。 例如，如果*Views/_ViewImports.cshtml*文件包含在你的项目中不存在的命名空间，你将收到 HTTP 500 错误。 若要获取详细的错误消息，添加以下代码：

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

请参阅**使用开发人员异常页**中[错误处理](../fundamentals/error-handling.md)有关详细信息。

## <a name="additional-resources"></a>其他资源

* [客户端开发](xref:client-side/index)
* [标记帮助程序](xref:mvc/views/tag-helpers/intro)
