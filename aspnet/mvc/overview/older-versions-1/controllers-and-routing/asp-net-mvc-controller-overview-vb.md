---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: "ASP.NET MVC 控制器概述 (VB) |Microsoft 文档"
author: StephenWalther
description: "在本教程中，Stephen Walther 向你介绍 ASP.NET MVC 控制器。 了解如何创建新的控制器，并返回不同类型的操作 res..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 452e2cf771e8b1f298ce9693ec2a707e7c1d4dd1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-controller-overview-vb"></a>ASP.NET MVC 控制器概述 (VB)
====================
通过[Stephen Walther](https://github.com/StephenWalther)

> 在本教程中，Stephen Walther 向你介绍 ASP.NET MVC 控制器。 了解如何创建新的控制器，并返回不同类型的操作结果。


本教程介绍了 ASP.NET MVC 控制器、 控制器操作和操作结果的主题。 完成本教程后，你将了解如何使用控制器来控制访问者与 ASP.NET MVC 网站交互的方式。

## <a name="understanding-controllers"></a>了解控制器

MVC 控制器负责对针对 ASP.NET MVC 网站发出的请求的响应。 每个浏览器请求映射到特定的控制器。 例如，假设你到你的浏览器的地址栏中输入以下 URL:

`http://localhost/Product/Index/3`

在这种情况下，将调用名为 ProductController 的控制器。 ProductController 负责生成对浏览器请求的响应。 例如，在控制器可能会返回到浏览器返回的特定视图或控制器可能会将用户重定向到另一个控制器。

列表 1 包含名为 ProductController 的简单控制器。

**Listing1-Controllers\ProductController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

正如您可以看到从清单 1，控制器将是只是一个类 （Visual Basic.NET 或 C# 类）。 控制器是从 System.Web.Mvc.Controller 基类派生的类。 控制器由于控制器继承自这个基类，免费继承几个有用的方法 （我们将讨论在稍后这些方法）。

## <a name="understanding-controller-actions"></a>了解控制器操作

控制器公开控制器操作。 操作是在浏览器地址栏中输入特定 URL 时被调用的控制器上的方法。 例如，假设以下 url 发出请求：

`http://localhost/Product/Index/3`

在这种情况下，ProductController 类上调用 index （） 方法。 Index （） 方法是控制器操作的示例。

控制器操作必须是了控制器类的公共方法。 Visual Basic.NET 方法，默认情况下，都是公共方法。 请注意添加到控制器类中任何公共方法自动公开为控制器操作 （你务必要小心这由于控制器操作可以调用该领域的任何人都只需通过浏览器地址栏中键入正确的 URL）。

没有控制器操作必须满足一些附加要求。 用作控制器操作的方法不能重载。 此外，控制器操作不能为静态方法。 除此之外，你可以使用的控制器操作几乎任何方法。

## <a name="understanding-action-results"></a>了解操作结果

控制器操作返回称为*操作结果*。 操作结果是对浏览器请求响应中返回内容的控制器操作。

ASP.NET MVC framework 支持几种类型的操作的结果，包括：

1. ViewResult-表示 HTML 和标记。
2. EmptyResult-表示任何结果。
3. RedirectResult-表示重定向到新 URL。
4. JsonResult-表示可在 AJAX 应用程序的 JavaScript 对象表示法结果。
5. JavaScriptResult-表示 JavaScript 脚本。
6. ContentResult-表示文本结果。
7. FileContentResult-表示一个可下载文件 （使用二进制内容）。
8. FilePathResult-表示 （包括路径） 的可下载文件。
9. FileStreamResult-表示一个可下载文件 （使用文件流）。

所有这些操作结果从 ActionResult 基类继承。

在大多数情况下，控制器操作返回 ViewResult。 例如，清单 2 中的索引控制器操作返回 ViewResult。

**列出 2-Controllers\BookController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

当操作返回 ViewResult 时，HTML 返回到浏览器。 中列出的 2 的 index （） 方法返回到浏览器命名索引的视图。

请注意中列出的 2 的 index （） 操作不返回 ViewResult()。 相反，控制器基类的 View() 方法被调用。 通常情况下，你不会返回操作结果直接。 相反，你调用控制器基类的以下方法之一：

1. 查看-返回 ViewResult 操作结果。
2. 重定向-返回 RedirectResult 操作结果。
3. RedirectToAction-返回 RedirectToRouteResult 操作结果。
4. RedirectToRoute-返回 RedirectToRouteResult 操作结果。
5. Json-返回 JsonResult 操作结果。
6. JavaScriptResult-返回 JavaScriptResult。
7. 内容-返回 ContentResult 操作结果。
8. 文件-返回 FileContentResult、 FilePathResult 或 FileStreamResult 具体取决于参数传递给方法。

因此，如果你想要返回到浏览器的视图，则调用 View() 方法。 如果你想要重定向到另一个控制器操作的用户，则调用 RedirectToAction() 方法。 例如，清单 3 中的 Details() 操作显示的视图或将用户重定向到的 index （） 操作，具体取决于是否 Id 参数具有一个值。

**列出 3-CustomerController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

特殊的 ContentResult 操作结果。 你可以使用 ContentResult 操作结果返回为纯文本操作结果。 例如，清单 4 中的 index （） 方法返回一条消息，以纯文本，而不是 HTML。

**列出 4-Controllers\StatusController.vb**

> StatusController


> System.Web.Mvc.Controller


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

当调用 StatusController.Index() 操作时，则不会返回一个视图。 相反，原始文本"Hello World ！" 返回到浏览器。

如果控制器操作返回的结果是不操作结果-例如，日期或整数-然后结果都包装在 ContentResult 自动。 例如，在列出 5 WorkController index （） 操作调用时，日期是作为返回 ContentResult 自动。

**列出 5-WorkController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

列出 5 中的 index （） 操作返回的 DateTime 对象。 ASP.NET MVC 框架将 DateTime 对象转换为字符串，并自动在 ContentResult 包装的日期时间值。 浏览器接收的日期和时间作为纯文本。

## <a name="summary"></a>摘要

本教程的目的是为了向你介绍 ASP.NET MVC 控制器、 控制器操作和控制器操作结果的概念。 在第一个部分中，您学习了如何将新的控制器添加到 ASP.NET MVC 项目。 接下来，您学习了如何公共方法的一个控制器到 universe 公开为控制器操作。 最后，我们讨论了不同类型的操作可从控制器操作返回的结果。 具体而言，我们讨论了如何返回 ViewResult、 RedirectToActionResult 和 ContentResult 的控制器操作。

>[!div class="step-by-step"]
[上一页](creating-a-custom-route-constraint-cs.md)
[下一页](creating-custom-routes-vb.md)
