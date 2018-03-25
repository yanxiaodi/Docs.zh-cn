---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: "在 ASP.NET 网页中创建可读的 Url 页 (Razor) 站点 |Microsoft 文档"
author: tfitzmac
description: "本指南介绍了一个 ASP.NET Web 页 (Razor) 的网站，以及如何这样便可以使用更具可读性且更适合 SEO 的 Url 中的路由。 你的将..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 7858b7cbd6dccafb2867ed9a1d102561e211e435
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>在 ASP.NET Web 页 (Razor) 站点中创建可读的 Url
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本指南介绍了一个 ASP.NET Web 页 (Razor) 的网站，以及如何这样便可以使用更具可读性且更适合 SEO 的 Url 中的路由。
> 
> 你将学习：
> 
> - 如何 ASP.NET 使用路由来允许你使用更具可读性且更可搜索的 Url。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET 网页 (Razor) 3
>   
> 
> 本教程还适用于 ASP.NET Web Pages 2。


## <a name="about-routing"></a>有关路由

在你的站点中的页面的 Url 可以影响以及站点的工作原理。 URL 的&quot;友好&quot;可以更加轻松让人有机会使用站点。 它还可帮助搜索引擎搜索引擎优化 (SEO) 站点。 ASP.NET 网站包括能够自动使用友好的 Url。

ASP.NET，可以创建有意义描述用户的操作，而不是仅指向服务器上的文件的 Url。 请考虑这些 Url 的虚构博客：

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

比较以下到这些 Url:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

在第一个对，用户必须知道使用显示博客*blog.cshtml*页，然后将必须构造查询字符串，可获取正确的类别或日期范围。 更易于理解和创建的第二个示例集。

第一个示例的 Url 也点直接到特定的文件 (*blog.cshtml*)。 如果出于某种原因博客移到另一个文件夹的服务器上，或者如果博客已重新，改为使用不同的页，则链接将是错误的。 第二个组 Url 未指向特定页，因此，即使的博客实现或位置发生更改，Url 将仍将有效。

在 ASP.NET Web 页中，您可以创建类似于上面的示例中的更友好 Url 因为 ASP.NET 使用*路由*。 路由可以完成请求指向的页 （或页） 的 URL 从创建逻辑映射。 因为映射被逻辑 （不是物理，到特定文件），路由提供极其灵活地为您的网站定义 Url 的方式。

## <a name="how-routing-works"></a>路由的工作原理

当 ASP.NET 处理的请求时，它会读取来确定如何将其路由的 URL。 ASP.NET 尝试匹配三个部分，将从左到右，磁盘上的文件的 URL。 如果没有匹配项，则将在 URL 中剩余的任何内容传递给页作为*路径信息*。

假设有人进行使用此 URL 的请求：

`http://www.contoso.com/a/b/c`

搜索如下所示：

1. 是否存在的文件的路径和名称*/a/b/c.cshtml*？ 如果是这样，运行该页面，并向其传递任何信息。 否则为...
2. 是否存在的文件的路径和名称*/a/b.cshtml*？ 如果因此，运行该页面，并将值传递`c`到它。 否则为...
3. 是否存在的文件的路径和名称*/a.cshtml*？ 如果因此，运行该页面，并将值传递`b/c`到它。

如果搜索已找到不精确的匹配项*.cshtml*其指定文件夹中的文件，ASP.NET 将继续进行反过来查找这些文件：

1. */a/b/c/default.cshtml* （任何路径信息）。
2. */a/b/c/index.cshtml* （任何路径信息）。

> [!NOTE]
> 为了清楚起见，特定页的请求 (即，包括请求*.cshtml*文件扩展名) 就像你预期一样工作。 请求喜欢`http://www.contoso.com/a/b.cshtml`将运行页面*b.cshtml*正常。


在页上，你可以通过该页面的路径信息`UrlData`属性，它是一个字典。 假设你有一个名为文件*ViewCustomers.cshtml*和你的站点获取此请求：

`http://mysite.com/myWebSite/ViewCustomers/1000`

如上面的规则中所述，请求将转到你的页面。 在页上，可以使用类似如下的代码，获取和显示的路径信息 (在此情况下，值&quot;1000年&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> 由于路由不涉及完整的文件名，可能存在多义性如果必须具有相同的页名称但不同的文件名扩展 (例如， *MyPage.cshtml*和*MyPage.html*). 为了避免出现路由问题，最好是若要确保你不包含其名称仅在其扩展名不同的站点中的页。


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他资源

[WebMatrix-Url、 UrlData 和路由 SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO)。 由 Mike Brind 此博客文章提供有关路由的工作方式中的 ASP.NET Web Pages 某些其他详细信息。
