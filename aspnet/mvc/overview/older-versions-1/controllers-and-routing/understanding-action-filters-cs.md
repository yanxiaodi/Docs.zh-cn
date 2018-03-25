---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: "了解操作筛选器 (C#) |Microsoft 文档"
author: microsoft
description: "本教程旨在说明操作筛选器。 操作筛选器是可以应用到的控制器操作-或整个控制器的属性..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: 86d5d429d9900d4c04391804598626705e6c88b4
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/11/2018
---
<a name="understanding-action-filters-c"></a>了解操作筛选器 (C#)
====================
by [Microsoft](https://github.com/microsoft)

[下载 PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> 本教程旨在说明操作筛选器。 操作筛选器是可以应用到的控制器操作-或整个 controller--修改将执行操作的方式的属性。


## <a name="understanding-action-filters"></a>了解操作筛选器

本教程旨在说明操作筛选器。 操作筛选器是可以应用到的控制器操作-或整个 controller--修改将执行操作的方式的属性。 ASP.NET MVC framework 包括多个操作筛选器：

- OutputCache – 此操作筛选器缓存指定的时间量的控制器操作的输出。
- HandleError – 此操作筛选器处理的控制器操作执行时引发的错误。
- 授权 – 此操作筛选器使您可以限制对特定用户或角色的访问。

您还可以创建你自己的自定义操作筛选器。 例如，你可能想要创建自定义操作筛选器以便实现一个自定义身份验证系统。 或者，你可能想要创建操作筛选器修改的控制器操作返回的视图数据。

在本教程中，你将了解如何生成操作筛选器从零开始。 我们创建的日志操作筛选器将在处理操作的不同阶段记录到 Visual Studio 输出窗口。

### <a name="using-an-action-filter"></a>使用操作筛选器

操作筛选器是一个特性。 可以对单个控制器操作或整个控制器来应用大多数操作筛选器。

例如，列表 1 中的数据控制器公开名为操作`Index()`返回当前时间。 此操作用修饰`OutputCache`操作筛选器。 此筛选器会导致要为 10 秒缓存的操作返回的值。

**列表 1 –`Controllers\DataController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

如果反复调用`Index()`操作通过在你的浏览器的地址栏中输入 URL/数据/索引并按刷新按钮多次，则会出现在同一时间为 10 秒。 输出`Index()`操作缓存 （请参见图 1） 的 10 秒钟。


[![缓存的时间](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)

**图 01**： 缓存时间 ([单击以查看实际尺寸的图像](understanding-action-filters-cs/_static/image3.png))


在列出 1 中，单个操作筛选器 –`OutputCache`操作筛选器 – 应用到`Index()`方法。 如果需要可以将多个操作筛选器应用到同样的操作。 例如，你可能想要将同时应用`OutputCache`和`HandleError`到相同的操作的操作筛选器。

列表 1 中`OutputCache`操作筛选器应用于`Index()`操作。 你还可以应用此属性与`DataController`类本身。 在这种情况下，将为 10 秒缓存在控制器公开任何操作返回的结果。

### <a name="the-different-types-of-filters"></a>不同类型的筛选器

ASP.NET MVC framework 支持四种不同类型的筛选器：

1. 授权筛选器 – 实现`IAuthorizationFilter`属性。
2. 操作筛选器 – 实现`IActionFilter`属性。
3. 导致筛选器 – 实现`IResultFilter`属性。
4. 异常筛选器 – 实现`IExceptionFilter`属性。

筛选器执行上面列出的顺序。 例如，授权筛选器始终执行的操作筛选器之前和之后每种其他类型的筛选器始终执行异常筛选器。

授权筛选器用于实现身份验证和授权的控制器操作。 例如，Authorize 筛选器是一种授权筛选器。

操作筛选器包含之前和之后执行控制器操作执行的逻辑。 可用于操作筛选器，例如，修改的控制器操作返回的视图数据。

结果筛选器包含之前和之后执行视图结果执行的逻辑。 例如，你可能想要修改视图结果右键之前视图呈现到浏览器。

异常筛选器是筛选器以运行的最后一个类型。 异常筛选器可用于处理由你的控制器操作或控制器操作结果引发的错误。 此外可以使用异常筛选器以记录错误。

按特定顺序执行每种不同类型的筛选器。 如果你想要控制在其中执行的相同类型的筛选器的顺序，则可以设置筛选器的顺序属性。

所有操作筛选器的基类是`System.Web.Mvc.FilterAttribute`类。 如果你想要实现特定类型的筛选器，则你需要创建一个类继承自的基类的筛选器，实现一个或多个`IAuthorizationFilter`， `IActionFilter`， `IResultFilter`，或`IExceptionFilter`接口。

### <a name="the-base-actionfilterattribute-class"></a>ActionFilterAttribute 基类

ASP.NET MVC framework 以使其更轻松地实现自定义操作筛选器，包括基本`ActionFilterAttribute`类。 此类同时实现`IActionFilter`和`IResultFilter`接口并继承自`Filter`类。

此处的术语不完全一致。 从技术上讲，从 ActionFilterAttribute 类继承的类是一个操作筛选器和结果筛选器。 但是，在松散的意义上，word 操作筛选器用于引用任何类型的 ASP.NET MVC framework 中的筛选器。

基`ActionFilterAttribute`类具有您可以重写以下方法：

- OnActionExecuting – 控制器操作执行之前调用此方法。
- OnActionExecuted – 控制器操作执行后调用此方法。
- OnResultExecuting – 执行控制器操作结果之前调用此方法。
- OnResultExecuted – 控制器操作结果执行后调用此方法。

在下一步的部分中，我们将了解如何实现上述每种不同方法。

### <a name="creating-a-log-action-filter"></a>创建日志操作筛选器

为了说明如何生成自定义操作筛选器，我们将创建自定义操作筛选器来记录处理到 Visual Studio 输出窗口的控制器操作的阶段。 我们`LogActionFilter`中列出 2 包含。

**列出 2 –`ActionFilters\LogActionFilter.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

在列出 2 中， `OnActionExecuting()`， `OnActionExecuted()`， `OnResultExecuting()`，和`OnResultExecuted()`所有方法都调用`Log()`方法。 方法和当前的路由数据的名称传递给`Log()`方法。 `Log()`方法将消息写入 Visual Studio 输出窗口 （请参见图 2）。


[![写入 Visual Studio 输出窗口](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)

**图 02**： 写入 Visual Studio 输出窗口 ([单击以查看实际尺寸的图像](understanding-action-filters-cs/_static/image6.png))


列出 3 中的主页控制器演示了如何将日志操作筛选器应用于整个控制器类。 每当在主页控制器公开的任何的操作调用 – 或者`Index()`方法或`About()`方法 – 处理操作会记录到 Visual Studio 输出窗口的阶段。

**列出 3 –`Controllers\HomeController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a>摘要

在本教程中，已向您介绍 ASP.NET MVC 操作筛选器。 了解有关筛选器的四个不同类型： 授权筛选器、 操作筛选器、 结果筛选器和异常筛选器。 你还了解了有关基`ActionFilterAttribute`类。

最后，您学习了如何实现简单的操作筛选器。 我们创建了日志处理到 Visual Studio 输出窗口的控制器操作的阶段的日志操作筛选器。

>[!div class="step-by-step"]
[上一页](asp-net-mvc-routing-overview-cs.md)
[下一页](improving-performance-with-output-caching-cs.md)
