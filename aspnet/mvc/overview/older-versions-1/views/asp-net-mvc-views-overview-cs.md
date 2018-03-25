---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: "ASP.NET MVC 视图概述 (C#) |Microsoft 文档"
author: StephenWalther
description: "有何它区别从 HTML 页和 ASP.NET MVC 视图是什么？ 在本教程中，Stephen Walther 向您介绍视图，并演示如何 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 9de095b0621af3b6166a2e1cbcb1c63c26a88aa2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-views-overview-c"></a>ASP.NET MVC 视图概述 (C#)
====================
通过[Stephen Walther](https://github.com/StephenWalther)

> 有何它区别从 HTML 页和 ASP.NET MVC 视图是什么？ 在本教程中，Stephen Walther 视图向你介绍并演示了如何，你可以充分利用查看数据和 HTML 的视图中的帮助器。


本教程的目的是为你提供对 ASP.NET MVC 视图、 查看数据和 HTML 帮助器的简短介绍。 本教程结束时，应了解如何创建新视图、 将数据从控制器传递了一个视图，和使用 HTML 帮助器以生成在视图中的内容。

## <a name="understanding-views"></a>了解视图

对于 ASP.NET 或 Active Server Pages，ASP.NET MVC 不包括直接对应于页的任何内容。 在 ASP.NET MVC 应用程序，没有一个页面对应于你的浏览器的地址栏中键入 URL 中的路径的磁盘上。 在 ASP.NET MVC 应用程序页的最佳捷径是调用*视图*。

ASP.NET MVC 应用程序、 传入的浏览器请求映射到控制器操作。 控制器操作可能会返回一个视图。 但是，控制器操作可能会执行一些其他类型的操作时，例如将你重定向到另一个控制器操作。

列表 1 包含名为 HomeController 的简单控制器。 HomeController 公开名为 index （） 和 Details() 的两个控制器操作。

**列表 1-HomeController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

你可以调用的第一个操作，index （） 操作中，通过在浏览器地址栏中键入以下 URL:

/ Home/Index

你可以调用第二个操作，Details() 操作中，通过将此地址键入你的浏览器：

/ 主页/详细信息

Index （） 操作返回的视图。 你创建的大多数操作将返回视图。 但是，操作可以返回其他类型的操作结果。 例如，Details() 操作返回传入的请求重定向到 index （） 操作 RedirectToActionResult。

Index （） 操作包含以下的单个代码行：

View();

以下代码行将返回必须位于你的 web 服务器上的以下路径的视图：

\Views\Home\Index.aspx

该视图的路径可以推断出的控制器名称和控制器操作的名称。

如果你愿意，你可以为显式有关视图。 以下代码行返回名为 Fred 的视图：

视图 (Fred);

当执行此代码行时，视图将返回从以下路径：

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> 如果你打算创建 ASP.NET MVC 应用程序的单元测试然后它是一个好办法应明确视图名称。 这样一来，你可以创建单元测试验证预期的视图已由控制器操作。


## <a name="adding-content-to-a-view"></a>将内容添加到视图

视图是一种标准 (X) 可以包含脚本的 HTML 文档。 您可以使用脚本将动态内容添加到视图。

例如，清单 2 中的视图显示当前日期和时间。

**列出 2-\Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

请注意，列出 2 中的 HTML 页面的正文包含以下脚本：

&lt;%Response.write(datetime.now); %&gt;

使用脚本分隔符&lt;%和 %&gt;来标记的开头和末尾脚本。 此脚本是用 C# 编写的。 它通过调用 Response.Write() 方法，以呈现到浏览器的内容显示了当前日期和时间。 脚本分隔符&lt;%和 %&gt;可以用于执行一个或多个语句。

由于如此频繁调用 Response.Write()，Microsoft 为你提供一种快捷方式为调用 Response.Write() 方法。 列出 3 中的视图使用分隔符&lt;%= 和 %&gt;作为调用 Response.Write() 的快捷方式。

**列出 3-Views\Home\Index2.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

可以使用任何.NET 语言生成动态内容视图中。 通常情况下，你将使用 Visual Basic.NET 或 C# 编写你的控制器和视图。

## <a name="using-html-helpers-to-generate-view-content"></a>使用 HTML 帮助器生成查看内容

若要更加轻松地将内容添加到视图，你可以利用称为*的 HTML 帮助器*。 HTML 帮助器，通常情况下，是生成的字符串的方法。 HTML 帮助器可用于生成如文本框、 链接、 下拉列表和列表框的标准 HTML 元素。

例如，三个 HTML 帮助器-列出 4 利用中的视图的 BeginForm()、 TextBox() 和 Password() 帮助器-生成登录名窗体 （请参见图 1）。

**列出 4--\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]


[![新项目对话框](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)

**图 01**： 一个标准的登录窗体 ([单击以查看实际尺寸的图像](asp-net-mvc-views-overview-cs/_static/image2.png))


在视图的 Html 属性上调用所有 HTML 帮助器方法。 例如，通过调用 Html.TextBox() 方法呈现文本框。

请注意，使用脚本分隔符&lt;%= 和 %&gt;时调用的 Html.TextBox() 和 Html.Password() 帮助器。 这些帮助器只返回一个字符串。 你需要调用 Response.Write() 以呈现到浏览器的字符串。

使用 HTML 帮助器方法是可选的。 它们使你更轻松通过减少的 HTML 和你需要编写的脚本。 列出 5 中的视图将列出 4 中的视图与完全相同的方式呈现而无需使用 HTML 帮助器。

**列出 5-\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

此外可以创建你自己的 HTML 帮助器。 例如，你可以创建一个 HTML 表中自动显示一组数据库记录的 GridView() 帮助器方法。 我们在本教程中浏览本主题**创建自定义 HTML 帮助器**。

## <a name="using-view-data-to-pass-data-to-a-view"></a>使用视图数据来将数据传递到视图

使用视图数据将从控制器的数据传递到视图。 将像通过邮件发送的包的视图数据。 必须使用此包发送从控制器中传递给视图的所有数据。 例如，列出 6 中的控制器中添加消息来查看数据。

**列出 6-ProductController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

控制器 ViewData 属性表示名称和值对的集合。 在列出 6 中，index （） 方法将项添加到名为 Hello World 的值的消息视图数据收集 ！。 当视图返回由 index （） 方法时，查看数据被自动传递到视图中。

列出 7 中的视图从查看数据中检索消息，并呈现到浏览器的消息。

**列出 7-\Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

请注意视图充分利用的 Html.Encode() HTML 帮助器方法，在呈现消息时。 Html.Encode() HTML 帮助器特殊字符进行编码如&lt;和&gt;转换是安全网页中显示的字符。 每当你呈现用户提交到网站的内容，应该对要防止 JavaScript 注入式攻击的内容进行编码。

(因为我们在创建的消息自行 ProductController，我们不真正需要对消息进行编码。 但是，它是一个好习惯时从查看数据的视图中显示内容检索始终调用 Html.Encode() 方法。）

在列出 7 中，我们利用要从控制器的一个简单的字符串消息传递到视图的视图数据。 此外可以使用视图数据将其他类型的数据，如数据库记录，从到视图控制器的集合。 例如，如果你想要的产品数据库表的内容显示在视图中，然后将数据库的集合视图中记录的数据。

此外可以将从控制器的强类型化的视图数据传递到视图。 我们在本教程中浏览本主题**了解强类型化视图数据和视图**。

## <a name="summary"></a>摘要

本教程提供对 ASP.NET MVC 视图、 查看数据和 HTML 帮助器的简短介绍。 在第一个部分中，您学习了如何将新视图添加到你的项目。 你已了解，你必须添加视图到正确的文件夹以从特定控制器调用它。 接下来，我们讨论 HTML 帮助的主题。 你已了解如何 HTML 帮助器使您能够轻松地生成标准 HTML 内容。 最后，您学习了如何充分利用查看数据以将数据从控制器传递到视图。

>[!div class="step-by-step"]
[下一篇](creating-custom-html-helpers-cs.md)
