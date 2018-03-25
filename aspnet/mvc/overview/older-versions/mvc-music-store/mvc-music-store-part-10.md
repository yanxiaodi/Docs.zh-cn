---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: "第 10 部分： 导航和站点设计，结束的最终更新 |Microsoft 文档"
author: jongalloway
description: "本系列教程详细介绍所有生成 ASP.NET MVC 音乐商店示例应用程序所采取的步骤。 第 10 部分介绍对导航和 s。 最后更新..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: 2a65e4b793b615c45cdf31166e0a000ae72ee534
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>第 10 部分： 导航和站点设计，结束的最终更新
====================
通过[Jon Galloway](https://github.com/jongalloway)

> MVC 音乐商店是，从而引入并说明如何使用 ASP.NET MVC 和 Visual Studio 进行 web 开发分步教程应用程序。  
>   
> MVC 音乐商店是一个轻型示例存储区实现，该类销售音乐集联机，并实现基本的站点管理、 用户登录，和购物车功能。  
>   
> 本系列教程详细介绍所有生成 ASP.NET MVC 音乐商店示例应用程序所采取的步骤。 第 10 部分介绍如何对导航和站点设计，结束的最终更新。


对于我们的站点，完成所有的主要功能，但我们仍有一些功能添加到站点导航、 主页上，和应用商店浏览页。

## <a name="creating-the-shopping-cart-summary-partial-view"></a>创建摘要部分购物车视图

我们想要公开整个站点内的用户的购物车中的项的数目。

![](mvc-music-store-part-10/_static/image1.png)

我们可以轻松地实现这通过创建其添加到我们 Site.master 了分部视图。

如前面所示，购物车控制器包括返回了分部视图的 CartSummary 操作方法：

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

若要创建 CartSummary 分部视图，右键单击视图/ShoppingCart 文件夹，然后选择添加视图。 将视图 CartSummary 命名并检查"创建了分部视图"复选框，如下所示。

![](mvc-music-store-part-10/_static/image2.png)

CartSummary 分部视图的过程非常简单-只需购物车中的显示项的数目的购物车索引视图的链接。 CartSummary.cshtml 的完整代码如下所示：

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

我们可以在站点上，使用 Html.RenderAction 方法包括站点母版上，任何页中包含了分部视图。 RenderAction 要求我们指定操作名称 ("CartSummary") 和控制器名称 ("ShoppingCart") 作为下面。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

之前将此添加到站点布局，我们还将创建流派菜单以便我们可以将所有我们 Site.master 更新一次。

## <a name="creating-the-genre-menu-partial-view"></a>创建流派菜单分部视图

我们可以让它为我们的用户通过添加一个流派菜单，其中列出了所有风格可用在我们的存储中存储导航容易得多。

![](mvc-music-store-part-10/_static/image3.png)

我们将遵循相同的步骤还创建了 GenreMenu 分部视图，然后我们可以将这两个添加到站点 master。 首先，将以下 GenreMenu 控制器操作添加到 StoreController:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

此操作返回的列表将显示的部分视图中，我们将在下一步创建的风格。

*注意： 我们具有 [ChildActionOnly] 特性添加到此控制器操作，这表示我们只想要从分部视图使用此操作。此属性将阻止从正在执行通过浏览到 /Store/GenreMenu 的控制器操作。这不是必需的分部视图，但它是很好的做法，由于我们想要确保按我们期望的那样使用我们的控制器操作。我们还会返回 PartialView 而不是视图中，这样就知道，它不应为此视图中，使用布局，因为它正在包括在其他视图的视图引擎。*

右键单击 GenreMenu 控制器操作，然后创建名为强类型使用流派视图数据类，如下所示的 GenreMenu 了分部视图。

![](mvc-music-store-part-10/_static/image4.png)

更新 GenreMenu 分部视图以显示，如下所示使用未经排序的列表项的视图代码。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>更新站点布局，以显示我们分部视图

我们可以将我们分部视图添加到站点布局 (/视图/共享/\_Layout.cshtml) 通过调用 Html.RenderAction()。 我们将添加这两个在中，以及一些其他的标记来显示它们，如下所示：

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

现在当我们运行应用程序，我们将看到在左侧的导航区域中 Genre 和顶部购物车摘要。

## <a name="update-to-the-store-browse-page"></a>更新到应用商店浏览页

应用商店浏览页上正常运行，但看上去很好。 我们可以更新页后，可以更好的布局中显示专辑，通过更新的视图代码 （/Views/Store/Browse.cshtml 中找到），如下所示：

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

此处我们正在使用的 Url.Action 而不是次 Html.ActionLink，以便我们可以应用到该链接可以包括唱片集图稿特殊格式设置。

*注意： 我们均会显示这些专辑泛型唱片集封面。此信息存储在数据库，并通过存储管理器可编辑。你是欢迎添加你自己的图片。*

现在当我们浏览到一种风格，我们将看到与唱片集图稿网格中所示唱片集。

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>更新主页上显示顶部销售专辑

我们想要功能我们最畅销的专辑在主页上，来提高销量。 对我们 HomeController 来处理，并添加一些其他的图形中，我们将进行一些更新。

首先，我们将添加一个导航属性到唱片集类，以便 EntityFramework 知道它们相关联。 最后几行我们**唱片集**类现在应如下所示：

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*注意： 这将需要添加 using 语句，以便为 System.Collections.Generic 命名空间中。*

首先，我们将添加 storeDB 字段和 MvcMusicStore.Models using 语句，如下所示我们其他控制器。 接下来，我们将向它查询我们的数据库以查找根据 OrderDetails 顶部销售专辑 HomeController 中添加以下方法。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

这是一个私有方法，因为我们不想将其提供为控制器操作。 我们会将其包括在为简单起见，HomeController 但建议将你的业务逻辑移到适当的单独的服务类。

完成此操作后，我们可以更新要查询前 5 销售唱片集并将它们返回到视图的索引控制器操作。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

更新 HomeController 的完整代码所示。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

最后，我们将需要更新我们主页索引视图，以便它可以通过更新模型类型，并将唱片集列表添加到底部显示的唱片集的列表。 我们将利用此机会来还向页面添加一个标题和促销部分。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

现在我们运行应用程序时，我们将看到使用顶部销售专辑和我们的促销消息我们更新的主页。

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>结束语

我们已了解，ASP.NET MVC 便于对来创建复杂的网站和数据库访问，成员身份，AJAX 等。 非常快速。 希望本教程已授予所需若要开始构建您自己的 ASP.NET MVC 应用程序工具 ！


>[!div class="step-by-step"]
[上一篇](mvc-music-store-part-9.md)
