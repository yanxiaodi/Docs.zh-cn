---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: "引入的 ASP.NET Web Pages-创建一致的布局 |Microsoft 文档"
author: tfitzmac
description: "本教程演示如何使用布局在使用 ASP.NET Web 页的站点上创建一致的外观的页。 它假定你已完成..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 692adc5a03892f27c91fe8868c8eab6ce08f49cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>引入了 ASP.NET Web 页-创建一致的布局
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本教程演示如何使用*布局*在使用 ASP.NET Web 页的站点上创建一致的外观的页。 它假定你已完成通过系列[删除数据库数据中的 ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584)。
> 
> 你将学习：
> 
> - 布局页。
> - 如何将布局页与动态内容结合起来。
> - 如何将值传递给布局页。


## <a name="about-layouts"></a>有关布局

你到目前为止已创建的页具有所有已完成，独立页。 它们都属于相同的站点中，但它们不具有任何通用元素或标准的外观。

大多数站点都有一致的外观和布局。 例如，如果你转到[Microsoft.com/web](https://www.microsoft.com/web/)站点和检查，您了解页面都遵循到总体布局和可视主题：

![显示标头、 导航区域、 内容区域和页脚的布局的 Microsoft.com/web 站点页](layouts/_static/image1.png)

*低效*方式创建此布局可以是定义标头、 导航栏中和页脚单独在每个页面上。 你将会将复制的相同标记每个时间。 如果你想要更改的某些内容 （例如，更新页脚），你将必须单独更改每一页。

这正是*布局页*进入。 在 ASP.NET Web 页中，你可以定义你的站点上的页面中提供的总体容器布局页。 例如，标头、 导航区域中和页脚可以包含布局页。 布局页包含主要内容的位置的占位符。

然后，你可以定义包含标记和仅该页面的代码的各个内容页。 内容页无需是完整的 HTML 页面; 例如：他们甚至不需要具有`<body>`元素。 它们还具有你想要显示中的内容布局页面是告诉 ASP.NET 的代码行。 此处是大致介绍了此关系的工作原理的图片：

![显示两个内容页和它们容纳布局页的概念图](layouts/_static/image2.png)

这种交互很容易理解何时在操作中查看它。 在本教程中，你将更改电影页面要使用的布局。

## <a name="adding-a-layout-page"></a>添加布局页

你首先创建定义一种典型的页面布局有页眉、 页脚和主要内容区域的布局页。 在 WebPagesMovies 站点中，添加一个名为的 CSHTML 页 *\_Layout.cshtml*。

前导下划线 ( `_` ) 字符非常重要。 如果页面的名称以下划线开头，则 ASP.NET 不会直接向浏览器发送该页面。 此约定，可以定义所需的站点，但该用户不应能够直接请求的页。

中页面的内容替换为以下代码：

[!code-html[Main](layouts/samples/sample1.html)]

如你所见，此标记是只需使用的 HTML`<div>`元素多页以及一个定义三个部分`<div>`元素来保存的三个部分。 页脚包含 Razor 代码执行少量： `@DateTime.Now.Year`，这将在该位置在页中呈现当前年份。

请注意，没有名为的样式表的链接*Movies.css*。 样式表是可在其中定义元素的物理布局的详细信息。 你将在稍后创建的。

在此唯一不同之功能 *\_Layout.cshtml*页`@Render.Body()`行。 这是与另一页合并此布局时内容的目的地的占位符。

## <a name="adding-a-css-file"></a>添加.css 文件

页上定义的元素的实际排列方式 （即，外观） 的首选的方式是使用级联样式表 (CSS) 规则。 因此，你将创建*.css*具有用于将新布局规则的文件。

在 WebMatrix 中，选择你的站点的根目录。 然后在**文件**选项卡的功能区中，单击下的箭头**新建**按钮，然后单击**新文件夹**。

![在新建下，功能区中的新建文件夹选项。](layouts/_static/image3.png)

将新文件夹命名*样式*。

![命名新文件夹样式](layouts/_static/image4.png)

在新*样式*文件夹中，创建名为的文件*Movies.css*。

![创建新的 Movies.css 文件](layouts/_static/image5.png)

新的内容替换*.css*替换为以下文件：

[!code-css[Main](layouts/samples/sample2.css)]

我们不会说过多这些 CSS 规则，只是想说明以下两项操作。 一个是除了设置字体和大小，规则使用绝对定位来建立的页眉、 页脚和主要内容区域的位置。 如果你熟悉定位在 CSS 中，你可以阅读[CSS 定位](http://www.w3schools.com/css/css_positioning.asp)教程在 W3Schools 站点。

其他需要注意的事项，在底部，我们已复制最初的样式规则定义的单独在*Movies.cshtml*文件。 在中使用这些规则[简介显示数据通过使用 ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580)教程，以使`WebGrid`添加到表的条带化的标记帮助器呈现。 (如果你要使用*.css*文件有关样式定义，你也可能给整个站点的样式规则中。)

## <a name="updating-the-movies-file-to-use-the-layout"></a>更新要使用的布局的电影文件

现在你可以更新你的站点以使用新的布局中的现有文件。 打开*Movies.cshtml*文件。 在顶部，作为第一个代码行，添加以下代码：

[!code-csharp[Main](layouts/samples/sample3.cs)]

该页面现在开始方式如下：

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

此一行代码将告诉 ASP.NET，当*电影*页运行时，它应与合并 *\_Layout.cshtml*文件。

由于*Movies.cshtml*文件现在使用布局页，你可以删除从标记*Movies.cshtml*已处理的页 *\_Layout.cshtml*文件。 带`<!DOCTYPE>`， `<html>`，和`<body>`开始和结束标记。 带整个`<head>`元素和其内容，其中包括网格中的样式规则，因为您现在已经获得这些规则*.css*文件。 当处于它时，更改现有`<h1>`元素`<h2>`元素; 你具有`<h1>`已布局页中的元素。 更改`<h2>`"列表电影"的文本。

通常，你不需要在内容页中进行这些种类的更改。 当你的站点开始与布局页中时，你首先创建内容页，而所有这些元素不。 在这种情况下，不过，您要转换的独立页到使用一种布局，因此清理的位。

如果已完成， *Movies.cshtml*页面将如下所示：

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>测试布局

现在，你可以看到布局如下所示。 在 WebMatrix 中，右键单击*Movies.cshtml*页，选择**在浏览器中启动**。 当浏览器显示页时，它类似于此页：

![使用一种布局呈现电影页](layouts/_static/image6.png)

ASP.NET 所合并到 Movies.cshtml 页面的内容 *\_Layout.cshtml*页上右`RenderBody`方法。 和当然 *\_Layout.cshtml*的页引用*.css*定义查找范围页的文件。

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>更新 AddMovie 页后，可以使用布局

布局实际好处是，你可以使用它们将所有页在你的网站。 打开*AddMovie.cshtml*页。

你可能会记住*AddMovie.cshtml*页最初在其中以定义查找范围的验证错误消息中发生一些 CSS 规则。 由于你有*.css*文件中为你的网站现在，你可以将移动到这些规则*.css*文件。 从其中移除这些*AddMovie.cshtml*文件并将它们添加到底部*Movies.css*文件。 要移动的以下规则：

[!code-css[Main](layouts/samples/sample6.css)]

现在，进行中的更改相同排序*AddMovie.cshtml*针对*Movies.cshtml* -添加`Layout="~/_Layout.cshtml;`和删除现在是多余的 HTML 标记。 更改`<h1>`元素`<h2>`。 完成后，页面将如下所示此示例：

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

运行页面。 现在看起来像此图中：

![使用一种布局呈现的添加电影页](layouts/_static/image7.png)

你想要对站点中的页面进行类似更改 — *EditMovie.cshtml*和*DeleteMovie.cshtml*。 但是，在执行之前，你可以另一项更改使其变得更加灵活的布局。

## <a name="passing-title-information-to-the-layout-page"></a>传递到布局页的标题信息

 *\_Layout.cshtml*你创建的页具有`<title>`设置为"我的电影站点"的元素。 大多数浏览器显示为选项卡上的文本的此元素的内容：

![该页面的&lt;标题&gt;浏览器选项卡中显示的元素](layouts/_static/image8.png)

此标题信息才是泛型。 假设你想要进行更具体到当前页的标题文本。 （标题文本也用于按搜索引擎确定你的页面即将。）可以将信息从如下的内容页传递*Movies.cshtml*或*AddMovie.cshtml*到布局页，然后使用该信息以自定义布局页呈现。

打开*Movies.cshtml*再次页。 在顶部代码中，添加以下行：

[!code-csharp[Main](layouts/samples/sample8.cs)]

`Page`上所有可用对象，则*.cshtml*页，即适用于此目的，一个页，并且其布局之间共享信息。

打开*\_Layout.cshtml*页。 更改`<title>`使它类似于此标记的元素：

[!code-html[Main](layouts/samples/sample9.html)]

此代码将呈现任何处于`Page.Title`页中的该位置在右侧的属性。

运行*Movies.cshtml*页。 这一次该浏览器选项卡显示你所传递的值作为`Page.Title`:

![浏览器选项卡显示在可动态创建的标题](layouts/_static/image9.png)

如果您希望在浏览器中查看页面源文件。 你可以看到，`<title>`元素呈现为`<title>List Movies</title>`。

> [!TIP] 
> 
> **Page 对象**
> 
> 一个有用的功能`Page`是动态对象-`Title`属性不是固定或保留的名称。 你可以使用*任何*名称的值为`Page`对象。 例如，你无法轻易传递标题使用名为的属性`Page.CurrentName`或`Page.MyPage`。 唯一限制是，名称必须遵循的哪些属性可以命名的一般规则。 （例如，名称不能包含空格。）
> 
> 你可以通过传递任意数目的值`Page`对象。 如果你想要将影片信息传递给布局页，您无法将值传递通过使用类似`Page.MovieTitle`和`Page.Genre`和`Page.MovieYear`。 （或是为了存储信息的任何其他名称。）唯一的要求-这是可能明显 — 是你需要使用内容页和布局页中的相同名称。
> 
> 使用传递的信息`Page`对象已不再局限于只是要在布局页上显示的文本。 可以将值传递给布局页中，然后布局页中的代码可以使用的值来确定是否要显示的页上，部分什么*.css*文件以使用，依次类推。 在中传递的值`Page`对象像任何其他值在代码中使用。 它只是值源自于在内容页中，并传递给布局页。


打开*AddMovie.cshtml*页上，并将行添加到提供的标题的代码，顶部*AddMovie.cshtml*页：

[!code-csharp[Main](layouts/samples/sample10.cs)]

运行*AddMovie.cshtml*页。 你看到的新标题：

![浏览器选项卡显示在可动态创建的添加电影标题](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>更新要使用布局的剩余页面

现在可以在你的站点中完成剩余的页，以便它们使用新的布局。 打开*EditMovie.cshtml*和*DeleteMovie.cshtml*接下来并在每个进行相同的更改。

添加链接到布局页的代码的行：

[!code-csharp[Main](layouts/samples/sample11.cs)]

添加行以设置页的标题：

[!code-csharp[Main](layouts/samples/sample12.cs)]

或：

[!code-csharp[Main](layouts/samples/sample13.cs)]

删除所有多余的 HTML 标记-基本上，保留内位`<body>`元素 （加上在顶部的代码块）。

更改`<h1>`元素`<h2>`元素。

已完成这些更改，测试每个，并确保它正确显示，并且标题正确无误。

## <a name="parting-thoughts-about-layout-pages"></a>最后，建议有关布局页

在本教程中，你将创建 *\_Layout.cshtml*页上，使用`RenderBody`方法合并从另一页的内容。 这是在 Web 页中使用布局的基本模式。

布局页具有我们未在此处介绍的附加功能。 例如，可以嵌套布局页-一个布局页又可以引用另一个。 嵌套的布局可以很有用，如果你正在使用的站点需要不同的布局的小节。 你还可以使用其他方法 (例如， `RenderSection`) 若要设置名为布局页中的部分。

布局页的组合和*.css*文件是功能强大。 正如你将在下一步的系列教程中看到的在 WebMatrix 中你可以创建基于站点*模板*，它会为你提供了的站点中的预构建的功能。 模板，可以很好的布局页和 CSS 代码即可创建的网站的外观和具有菜单等功能的用途。 下面是主页的从基于模板，显示布局页和 CSS 使用的功能的站点的屏幕快照：

![显示标头、 导航区域、 内容区域、 可选部分和登录链接的 WebMatrix 网站模板创建的布局](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>完成列表影片页 （更新为使用布局页）

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>完成页列出了添加影片页 （对于布局更新）

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>完成删除影片页 （对于布局更新） 的页列表

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>完成编辑影片页 （对于布局更新） 的页列表

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>接下来

在下一步的教程中，你将了解如何将你的站点发布到 Internet，以便每个人都可以看到它。

## <a name="additional-resources"></a>其他资源

- [创建一致的查看](https://go.microsoft.com/fwlink/?LinkID=202891)-提供了更详细地使用布局的项目。 它还描述如何将值传递到的布局页面中显示或隐藏的某些内容。
- [嵌套具有 Razor 布局页](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor)-Mike Brind 博客举例说明如何嵌套布局页。 （包括的下载页。）

>[!div class="step-by-step"]
[上一页](deleting-data.md)
[下一页](publishing.md)
