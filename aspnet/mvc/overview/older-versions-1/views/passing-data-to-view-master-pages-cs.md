---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
title: "将数据传递到视图母版页 (C#) |Microsoft 文档"
author: microsoft
description: "本教程旨在说明如何为视图的母版页，从控制器传递数据。 我们检查用于将数据传递到视图 m 的两种策略..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: 5fee879b-8bde-42a9-a434-60ba6b1cf747
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: b8bc8ce0690d2e45877be75011d8883facbc74a7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="passing-data-to-view-master-pages-c"></a>将数据传递给视图母版页 (C#)
====================
通过[Microsoft](https://github.com/microsoft)

[下载 PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_CS.pdf)

> 本教程旨在说明如何为视图的母版页，从控制器传递数据。 我们检查用于将数据传递到视图的母版页的两种策略。 首先，我们讨论的简单解决方案，它会导致难以维护应用程序。 接下来，我们检查需要稍有更多的初始工作，但在更容易维护的应用程序中的结果的更好的解决方案。


## <a name="passing-data-to-view-master-pages"></a>将数据传递给视图母版页

本教程旨在说明如何为视图的母版页，从控制器传递数据。 我们检查用于将数据传递到视图的母版页的两种策略。 首先，我们讨论的简单解决方案，它会导致难以维护应用程序。 接下来，我们检查需要稍有更多的初始工作，但在更容易维护的应用程序中的结果的更好的解决方案。

### <a name="the-problem"></a>问题

假设你正在构建电影数据库应用程序，并且你想要在你的应用程序中每页上显示电影类别列表中的 （请参见图 1）。 此外，假设的电影类别列表存储在数据库表。 在这种情况下，它会有一定意义从数据库检索类别并呈现电影类别视图的母版页中的列表。


[![在视图的母版页中显示电影类别](passing-data-to-view-master-pages-cs/_static/image2.png)](passing-data-to-view-master-pages-cs/_static/image1.png)

**图 01**： 电影类别显示在视图的母版页 ([单击以查看实际尺寸的图像](passing-data-to-view-master-pages-cs/_static/image3.png))


下面是问题。 如何检索在母版页中的电影类别列表中？ 它很容易在母版页中直接调用的模型类的方法。 换而言之，它很容易包括用于从母版页中的数据库权限检索数据的代码。 但是，绕过你的 MVC 控制器来访问数据库将违反是构建一个 MVC 应用程序的主要优点之一完全分离关注点。

在 MVC 应用程序，你希望 MVC 视图和 MVC 模型均由你的 MVC 控制器之间的所有交互。 这种问题将会导致更易于维护、 自适应，和可测试的应用程序。

在 MVC 应用程序，传递到 – 包括视图的母版页 – 视图的所有数据应通过的控制器操作都传递到视图。 此外，数据应传递通过利用查看数据。 在本教程的其余部分中，我将检查两种方法可以将视图数据传递到视图的母版页。

### <a name="the-simple-solution"></a>简单的解决方案

让我们开始使用将从控制器的视图数据传递到视图的母版页的简单解决方案。 最简单的解决方案是在每个控制器操作中传递主控页查看数据。

请考虑列出 1 中的控制器。 它公开两个操作名为`Index()`和`Details()`。 `Index()`操作方法会返回每个电影的电影数据库表中。 `Details()`操作方法会返回特定电影类别中每个影片。

**列表 1 –`Controllers\HomeController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample1.cs)]

请注意，index （） 和 Details() 操作添加两项以查看数据。 Index （） 操作将添加两个密钥： 类别和影片。 类别键表示的电影类别显示视图的母版页的列表。 电影键表示索引视图页中显示的影片的列表。

Details() 操作还将添加名为类别和影片的两个密钥。 类别键，同样，表示显示视图的母版页的电影类别的列表。 电影键表示的详细信息视图页显示的特定类别中的影片列表 （请参见图 2）。


[![详细信息视图](passing-data-to-view-master-pages-cs/_static/image5.png)](passing-data-to-view-master-pages-cs/_static/image4.png)

**图 02**: 的详细信息视图 ([单击以查看实际尺寸的图像](passing-data-to-view-master-pages-cs/_static/image6.png))


索引视图包含在清单 2。 它只需循环访问由中查看数据的电影项表示的影片列表。

**列出 2 –`Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample2.aspx)]

视图的母版页包含在清单 3。 视图的母版页循环，并呈现所有影片类别由类别项表示从查看数据。

**列出 3 –`Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample3.aspx)]

通过查看数据情况下，所有数据都传递给视图和视图的母版页。 这是将数据传递给母版页的正确方法。

因此，这是为什么使用此解决方案？ 问题是该解决方案违反了模拟 （不重复自己） 原则。 每个控制器操作必须添加非常相同电影类别以查看数据的列表。 在你的应用程序中有重复的代码使你的应用程序更难维护、 改编，和修改。

### <a name="the-good-solution"></a>很好的解决方案

在此部分中，我们将检查到将数据从控制器操作传递到视图的母版页的替代，并更好地，解决方案。 而不是在每个控制器操作中添加母版页的电影类别，我们将添加影片类别到查看数据一次。 应用程序控制器中添加了使用的视图的母版页的所有视图数据。

ApplicationController 类包含在清单 4。

**列出 4 –`Controllers\ApplicationController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample4.cs)]

有三个您应注意到了清单 4 中的应用程序控制器的操作。 首先，请注意此类从 System.Web.Mvc.Controller 基类继承。 应用程序控制器是一个控制器类。

其次，请注意应用程序控制器类是一个抽象类。 一个抽象类是具体的类必须实现的类。 因为应用程序控制器是一个抽象类，你不能调用的类中直接定义任何方法。 如果你尝试直接调用应用程序类，然后你将获得找不到资源的错误消息。

第三个，请注意，应用程序控制器包含将影片类别以查看数据的列表添加一个构造函数。 每个控制器类都继承自应用程序控制器将自动调用应用程序控制器构造函数。 每当从应用程序控制器继承任何控制器上调用任何操作，电影类别是自动包含在视图的数据。

从应用程序控制器继承中列出 5 的电影控制器。

**列出 5-`Controllers\MoviesController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample5.cs)]

电影控制器上，就像在上一节中讨论的主页控制器公开名为的两个操作方法`Index()`和`Details()`。 请注意，电影类别显示视图的母版页由该列表不是添加到视图中的数据`Index()`或`Details()`方法。 由于从应用程序控制器继承电影控制器，则将添加电影类别列表中的自动查看数据。

请注意将视图的母版页的视图数据添加到此解决方案不违反干 （不重复自己） 原则。 将添加影片类别来查看数据列表中的代码包含仅在一个位置： 应用程序控制器的构造函数。

### <a name="summary"></a>摘要

在本教程中，我们讨论了两种方法来将从控制器的视图数据传递到视图的母版页。 首先，我们探讨了一个简单，但难以维护方法。 在第一个部分中，我们讨论了如何添加查看的视图的母版页数据中每个每个控制器操作应用程序中。 我们的结论，这是错误的方法，因为它违反了模拟 （不重复自己） 原则。

接下来，我们探讨了用于添加数据视图的母版页需查看数据的多较好策略。 而不是在每个控制器操作中添加的视图数据，我们添加了视图数据一次在应用程序控制器内。 这样一来，你就可以将数据传递到视图母版页中的 ASP.NET MVC 应用程序时避免重复代码。

>[!div class="step-by-step"]
[上一页](creating-page-layouts-with-view-master-pages-cs.md)
[下一页](asp-net-mvc-views-overview-vb.md)
