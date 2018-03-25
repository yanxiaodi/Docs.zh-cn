---
title: "定位点标记帮助程序"
author: pkellner
description: "了解 ASP.NET Core 定位点标记帮助程序属性以及每个属性在扩展 HTML 定位点标记的行为中所起的作用。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: f3b704174c3287edda12725b7973a2464e485bac
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2018
---
# <a name="anchor-tag-helper"></a>定位点标记帮助程序

作者：[Peter Kellner](http://peterkellner.net) 和 [Scott Addie](https://github.com/scottaddie)

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/tag-helpers/built-in/samples/TagHelpersBuiltInAspNetCore)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

[定位点标记帮助程序](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper)可通过添加新属性来增强标准的 HTML 定位点 (`<a ... ></a>`) 标记。 按照约定，属性名称将使用前缀 `asp-`。 `asp-` 属性的值决定呈现的定位点元素的 `href` 属性值。

本文档中的示例均使用 SpeakerController：

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

`asp-` 属性的清单如下所示。

## <a name="asp-controller"></a>asp-controller

[asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) 属性可分配用于生成 URL 的控制器。 下面的标记列出了所有发言人：

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspController)]

生成的 HTML：

```html
<a href="/Speaker">All Speakers</a>
```

如果指定了 `asp-controller` 属性，而未指定 `asp-action` 属性，则默认的 `asp-action` 值为与当前正在执行的视图关联的控制器操作。 如果前面的标记中省略了 `asp-action`，并在 HomeController 的索引视图 (/Home) 中使用了定位点标记帮助程序，则生成的 HTML 为：

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a>asp-action

[asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) 属性值表示生成的 `href` 属性中包含的控制器操作名称。 下面的标记可将生成的 `href` 属性值设置为发言人评估页：

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspAction)]

生成的 HTML：

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

如果未指定 `asp-controller` 属性，则使用默认控制器，该控制器调用执行当前视图的视图。

如果 `asp-action` 属性值为 `Index`，则不向 URL 追加任何操作，从而导致调用默认的 `Index` 操作。 `asp-controller` 引用的控制器中必须存在指定的（或默认的）操作。

## <a name="asp-route-value"></a>asp-route-{value}

[asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) 属性可实现通配符路由前缀。 占用 `{value}` 占位符的所有值都解释为潜在的路由参数。 如果找不到默认路由，则将此路由前缀作为请求参数和值追加到生成的 `href` 属性。 否则，将在路由模板中替换它。

考虑以下控制器操作：

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

在 Startup.Configure 中定义默认路由模板：

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

MVC 视图使用操作提供的模型，如下所示：

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

默认路由的 `{id?}` 占位符得以匹配。 生成的 HTML：

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

假设路由前缀不属于匹配路由模板的一部分，如下面的 MVC 视图所示：

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker" 
       asp-action="Detail" 
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

生成以下 HTML，因为匹配的路由中未找到 `speakerid`：

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

如果 `asp-controller` 或 `asp-action` 均未指定，则会执行与 `asp-route` 属性中相同的默认处理。

## <a name="asp-route"></a>asp-route

[asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) 属性用于创建直接链接到命名路由的 URL。 使用[路由属性](xref:mvc/controllers/routing#attribute-routing)，路由可以按 `SpeakerController` 中所示进行命名并用于其 `Evaluations` 操作：

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?range=22-24)]

在下列标记中，`asp-route` 属性引用命名路由：

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspRoute)]

定位点标记帮助程序使用 URL /Speaker/Evaluations 生成直接指向该控制器操作的路由。 生成的 HTML：

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

如果除了 `asp-route`，还指定了 `asp-controller` 或 `asp-action`，则可能不会生成预期的路由。 为了避免发生路由冲突，不应将 `asp-route` 与 `asp-controller` 和 `asp-action` 属性结合使用。

## <a name="asp-all-route-data"></a>asp-all-route-data

[asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) 属性支持创建键值对字典。 键是参数名称，值是参数值。

在下面的示例中，将对字典进行初始化并将其传递给 Razor 视图。 或者，也可以使用模型传入数据。

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

前面的代码生成以下 HTML：

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

平展 `asp-all-route-data` 字典，以生成满足重载 `Evaluations` 操作要求的查询字符串：

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?range=26-30)]

如果字典中的任何键匹配路由参数，则将根据需要在路由中替换这些值。 其他不匹配的值作为请求参数生成。

## <a name="asp-fragment"></a>asp-fragment

[asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) 属性可定义要追加到 URL 的 URL 片段。 定位点标记帮助程序添加哈希字符 (#)。 请考虑以下标记：

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspFragment)]

生成的 HTML：

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

生成客户端应用时，哈希标记很有用。 它们可用于在 JavaScript 中轻松地执行标记和搜索等操作。

## <a name="asp-area"></a>asp-area

[asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) 属性可设置用来设置相应路由的区域名称。 以下示例展示了区域属性如何导致重新映射路由。 如果将 `asp-area` 设置为 “Blogs”，则会为此定位点标记的关联控制器和视图的路由添加目录 Areas/Blogs 作为前缀。

* **<项目名称\>**
  * **wwwroot**
  * **区域**
    * **博客**
      * **控制器**
        * HomeController.cs
      * **视图**
        * **主文件夹**
          * *AboutBlog.cshtml*
          * Index.cshtml
        * *_ViewStart.cshtml*
  * **控制器**

鉴于上述目录层次结构，引用 AboutBlog.cshtml 文件的标记是：

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspArea)]

生成的 HTML：

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> 若要使区域在 MVC 应用中正常工作，路由模板必须包含对该区域（如果存在）的引用。 该模板由 Startup.Configure 中 `routes.MapRoute` 方法调用的第二个参数表示：[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a>asp-protocol

[asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) 属性用于在 URL 中指定协议（比如 `https`）。 例如:

[!code-cshtml[samples/TagHelpersBuiltInAspNetCore/Views/Index.cshtml?name=snippet_AspProtocol]]

生成的 HTML：

```html
<a href="https://localhost/Home/About">About</a>
```

示例中的主机名为 localhost，但在生成 URL 时，定位点标记帮助程序会使用网站的公共域。

## <a name="asp-host"></a>asp-host

[asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) 属性用于在 URL 中指定主机名。 例如:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspHost)]

生成的 HTML：

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a>asp-page

[asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) 属性适用于 Razor 页面。 使用它向特定页设置定位点标记的 `href` 属性值。 通过在页面名称前面使用正斜杠 (“/”) 作为前缀，可创建 URL。

下列示例指向与会者 Razor 页面：

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPage)]

生成的 HTML：

```html
<a href="/Attendee">All Attendees</a>
```

`asp-page` 属性与 `asp-route`、`asp-controller` 和 `asp-action` 属性互斥。 但是，`asp-page` 可与 `asp-route-{value}` 结合使用以控制路由，如以下标记所示：

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

生成的 HTML：

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a>asp-page-handler

[asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) 属性适用于 Razor 页面。 它用于链接到特定的页处理程序。

请考虑以下页处理程序：

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

页模型的关联标记链接到 `OnGetProfile` 页处理程序。 注意，`asp-page-handler` 属性值中省略了页处理程序方法名称的 `On<Verb>` 前缀。 如果这是异步方法，还会省略 `Async` 后缀。

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

生成的 HTML：

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a>其他资源

* [区域](xref:mvc/controllers/areas)
* [Razor 页面简介](xref:mvc/razor-pages/index)
