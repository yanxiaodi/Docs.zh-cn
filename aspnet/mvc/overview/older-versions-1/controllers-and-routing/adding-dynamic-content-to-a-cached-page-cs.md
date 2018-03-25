---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: "将动态内容添加到缓存的页面 (C#) |Microsoft 文档"
author: microsoft
description: "了解如何混合同一页中的动态和缓存内容。 缓存后替换使您能够显示动态内容，如横幅播发 o..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: bee7e17ee16d75419c215558b1deb7d6f0d79448
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a>将动态内容添加到缓存的页面 (C#)
====================
通过[Microsoft](https://github.com/microsoft)

> 了解如何混合同一页中的动态和缓存内容。 缓存后替换，你可以显示动态内容，如横幅播发或用于缓存已输出的页中的新闻的项。


通过利用的输出缓存，你可以显著地提高 ASP.NET MVC 应用程序的性能。 而不是重新生成一个页面请求该页的每个时间，可以一次生成页，并在多个用户的内存中缓存中。

但没有问题。 如果你需要在页中显示动态内容？ 例如，假设你想要在页中显示横幅播发。 你不希望横幅播发将缓存，以便每个用户将看到非常相同的播发。 你不会进行任何 money 这样 ！

幸运的是，这里有一个简单的解决方案。 你可以利用的 ASP.NET 框架调用一项功能*缓存后替换*。 缓存后替换使你可以用在页中已缓存在内存中的动态内容。


通常情况下，当你使用 [OutputCache] 特性的输出缓存页面，页面将缓存服务器和客户端 （web 浏览器） 上。 当使用缓存后替换时，页面将缓存仅在服务器上。


#### <a name="using-post-cache-substitution"></a>使用缓存后替换

使用缓存后替换需要两个步骤。 首先，你需要定义返回一个字符串，表示你想要在缓存的页中显示的动态内容的方法。 接下来，你调用 HttpResponse.WriteSubstitution() 方法将动态内容插入页。

例如，假设你想要随机中缓存的页面中显示不同新闻项。 列表 1 中的类公开一个名为 RenderNews()，随机从列表中的三个新闻项返回一个新闻项的单个方法。

**列表 1 – Models\News.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

若要充分利用缓存后替换，你可以调用 HttpResponse.WriteSubstitution() 方法。 WriteSubstitution() 方法设置代码，来替换动态内容的缓存的页面区域。 WriteSubstitution() 方法用于列出 2 中的视图中显示的随机新闻项。

**列出 2 – Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

RenderNews 方法传递给 WriteSubstitution() 方法。 请注意，不调用 RenderNews 方法 （有没有括号）。 而是对方法的引用传递给 WriteSubstitution()。

索引视图将被缓存。 该视图会返回列出 3 中的控制器。 请注意，使用会导致索引视图要为 60 秒缓存的 [OutputCache] 特性修饰的 index （） 操作。

**列出 3 – Controllers\HomeController.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

即使缓存的索引视图，在请求索引页时，会显示不同的随机新闻项。 在请求索引页时，显示的页面的时间将不会更改 （请参见图 1） 的 60 秒。 时间不会更改的事实证明会缓存该页面。 但是，内容所插入 WriteSubstitution() 方法-随机新闻项目-更改与每个请求。

**图 1 – 将注入动态新闻项中缓存的页面**

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>生成的帮助程序方法中使用缓存后替换

更简单的方法以利用缓存后替换是封装对自定义帮助程序方法内 WriteSubstitution() 方法的调用。 通过列出 4 中的帮助器方法，此方法进行了阐释。

**列出 4 – AdHelper.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

列出 4 包含公开两个方法的静态类： RenderBanner() 和 RenderBannerInternal()。 RenderBanner() 方法表示实际的帮助器方法。 此方法，以便你可以在视图中就像任何其他帮助器方法一样调用 Html.RenderBanner() 扩展标准的 ASP.NET MVC HtmlHelper 类。

RenderBanner() 方法调用将 RenderBannerInternal() 方法传递给 WriteSubsitution() 方法 HttpResponse.WriteSubstitution() 方法。

RenderBannerInternal() 方法是私有方法。 此方法不会公开为一个帮助器方法。 RenderBannerInternal() 方法随机从三个横幅播发映像的列表中返回一个横幅播发图像。

已修改的索引视图中列出 5 演示了如何使用 RenderBanner() 帮助器方法。 请注意，其他&lt;%@ 导入 %&gt;指令是包含要导入 MvcApplication1.Helpers 命名空间的视图的顶部。 如果你忘记导入此命名空间，然后 RenderBanner() 方法不会出现作为 Html 属性的方法。

**列出 5 – Views\Home\Index.aspx （使用 RenderBanner() 方法）**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

不同横幅播发时请求列出 5 中的视图由呈现页时，会显示每个请求 （请参见图 2）。 对页进行缓存，但横幅播发动态注入 RenderBanner() 帮助器方法。

**图 2 – 显示随机横幅播发的索引视图**

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a>摘要

本教程介绍如何动态更新中缓存的页面内容。 您学习了如何使用 HttpResponse.WriteSubstitution() 方法来使动态内容能够注入到缓存的页面。 你还了解了如何封装对 HTML 帮助程序方法内 WriteSubstitution() 方法的调用。

充分利用缓存尽可能 – 它可以对 web 应用程序的性能产生极大影响。 在本教程中所述，你可以利用的缓存甚至当你需要在页面中显示动态内容时。

## 

## 

>[!div class="step-by-step"]
[上一页](improving-performance-with-output-caching-cs.md)
[下一页](creating-a-controller-cs.md)
