---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
title: "改进的详细信息和删除方法 (VB) |Microsoft 文档"
author: Rick-Anderson
description: "本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是一个 ASP.NET MVC Web 应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: c5c14ef0-c128-4dc1-8c01-7f0fdb09e411
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: e057d9f106aaa8afbe521d8185a06dfbf48e46fb
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="improving-the-details-and-delete-methods-vb"></a>改进的详细信息和删除方法 (VB)
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> 本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是 Microsoft Visual Studio 的免费版本的 ASP.NET MVC Web 应用程序的基础知识。 在开始之前，请确保已安装下面列出的先决条件。 你可以通过单击以下链接安装所有这些： [Web 平台安装程序](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，你可以单独安装系统必备组件，使用以下链接：
> 
> - [Visual Studio Web Developer Express SP1 系统必备](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools 更新](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（运行时 + 工具支持）
> 
> 如果你正在使用 Visual Studio 2010，而不 Visual Web Developer 2010，通过单击以下链接安装必备组件： [Visual Studio 2010 系统必备组件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> Visual Web Developer 项目与 VB.NET 的源代码是可以附带本主题。 [下载 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果你愿意 C#，切换到[C# 版本](../cs/improving-the-details-and-delete-methods.md)本教程。


在本教程的此部分中，你将使到自动生成的一些改进`Details`和`Delete`方法。 这些更改并不是必需的但使用只需少量小段代码，你可以轻松地增强应用程序。

## <a name="improving-the-details-and-delete-methods"></a>改进的详细信息和删除方法

当基架`Movie`控制器，ASP.NET MVC 生成的代码起作用的那就太好，但可以进行的更加强大，只需少量的小改动。

打开`Movie`控制器和修改`Details`方法通过返回`HttpNotFound`一部电影时找不到。 您还应修改`Details`方法以设置默认值传递给它的 id。 (类似更改过`Edit`中的方法[第 6 部分](examining-the-edit-methods-and-edit-view.md)本教程。)但是，必须更改的返回类型`Details`方法从`ViewResult`到`ActionResult`，这是因为`HttpNotFound`方法不返回`ViewResult`对象。 下面的示例演示已修改`Details`方法。

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample1.vb)]

代码首先可以轻松地搜索数据使用`Find`方法。 我们内置于方法的重要的安全功能是代码验证`Find`代码尝试使用它执行任何操作之前，已找到一部电影方法。 例如，黑客无法将错误引入到站点通过更改由来自链接的 URL`http://localhost:xxxx/Movies/Details/1`为类似于`http://localhost:xxxx/Movies/Details/12345`（或不表示实际的电影的一些其他值）。 如果没有为 null 的电影的检查，这可能导致数据库错误。

同样地，更改`Delete`和`DeleteConfirmed`方法来指定默认值为 ID 参数并返回`HttpNotFound`一部电影时找不到。 已更新`Delete`中的方法`Movie`控制器如下所示。

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample2.vb)]

请注意，`Delete`方法不会删除数据。 执行删除操作以响应 GET 请求（或者说，执行编辑操作、创建操作或更改数据的任何其他操作）会打开安全漏洞。 有关此的详细信息，请参阅博客条目，Stephen Walther [ASP.NET MVC 提示 #46-不要使用删除的链接，因为他们创建的安全漏洞](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)。

删除数据的 `HttpPost` 方法命名为 `DeleteConfirmed`，以便为 HTTP POST 方法提供一个唯一的签名或名称。 下面显示了两个方法签名：

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample3.vb)]

公共语言运行时 (CLR) 需要具有唯一的签名 （名称相同，不同的参数列表） 的重载的方法。 但是，此处你需要两个删除方法-一个 get-，一个用于 POST，这两需要相同的签名。 （它们都需要接受单个整数作为参数。）

若要对此项进行排序，你可以执行几个事项。 之一是为方法提供不同的名称。 这就是我们所做的他前面示例中。 但是，这会造成一个小问题：ASP.NET 按名称将 URL 段映射到操作方法，如果重命名方法，则路由通常无法找到该方法。 该示例中也提供了解决方案，即向 `DeleteConfirmed` 方法添加 `ActionName("Delete")` 属性。 这有效地执行映射路由的系统，以使 URL，包括*/Delete/*的 post 请求将查找`DeleteConfirmed`方法。

另一种方法，以避免问题具有相同的名称和签名的方法是人为地更改要包括未使用的参数的 POST 方法的签名。 例如，一些开发人员添加的参数类型`FormCollection`传递给 POST 方法中，然后只需不使用参数：

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample4.vb)]

## <a name="wrapping-up"></a>总结

你现在具有一个完整的 ASP.NET MVC 应用程序在 SQL Server Compact 数据库中存储数据。 你可以创建、 读取、 更新、 删除和搜索电影。

![](improving-the-details-and-delete-methods/_static/image1.png)

这一基本教程了你可以开始进行控制器，将其关联与视图，并传递关于硬编码数据。 然后，你将创建并设计数据模型。 Entity Framework Code First 数据模型在运行过程中，从创建数据库和 ASP.NET MVC 基架系统自动生成的操作方法和视图的基本 CRUD 操作。 然后添加搜索表单，以让用户在数据库中搜索。 更改要包括的数据，新列的数据库，然后更新两个页，以创建和显示此新数据时。 通过将标记中的属性与数据模型添加验证`DataAnnotations`命名空间。 客户端和服务器上运行生成验证。

如果你想要部署应用程序，是对第一个测试你的本地 IIS 7 服务器上的应用程序帮助。 你可以使用此[Web 平台安装程序](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;)链接以启用 ASP.NET 应用程序的 IIS 设置。 请参阅下面的部署链接：

- [ASP.NET 部署内容映射](https://msdn.microsoft.com/library/dd394698.aspx)
- [启用 IIS 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Web 应用程序项目部署](https://msdn.microsoft.com/library/dd394698.aspx)

我现在鼓励你转到我们中间级[为 ASP.NET MVC 应用程序创建实体框架数据模型](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)和[MVC 音乐商店](../../mvc-music-store/mvc-music-store-part-1.md)教程，以浏览[ASP.NETMSDN 上的文章](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)，和签出的许多视频和资源在[https://asp.net/mvc](https://asp.net/mvc)若要了解更多有关 ASP.NET MVC ！ [ASP.NET MVC 论坛](https://forums.asp.net/1146.aspx)是询问的问题的好地方。

请尽情体验吧！

-Scott Hanselman ([http://hanselman.com](http://hanselman.com)和[ @shanselman ](http://twitter.com/shanselman)在 Twitter 上) 和 Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

>[!div class="step-by-step"]
[上一篇](adding-validation-to-the-model.md)
