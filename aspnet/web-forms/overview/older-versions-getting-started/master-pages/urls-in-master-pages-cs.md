---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
title: "母版页 (C#) 中的 Url |Microsoft 文档"
author: rick-anderson
description: "介绍如何在母版页中的 Url 可以中断由于是在不同的相对目录比内容页的主控页文件。 查看重定基址..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 48b58a18-5ea4-468c-b326-f35331b3e1e9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b01f0ac780121c4e0941df6016220a1cb1ed2d1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="urls-in-master-pages-c"></a>母版页 (C#) 中的 Url
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_CS.zip)或[下载 PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_CS.pdf)

> 介绍如何在母版页中的 Url 可以中断由于是在不同的相对目录比内容页的主控页文件。 重定基址通过 Url 查看 ~ 中以编程方式使用 ResolveUrl 和 ResolveClientUrl 和声明性语法。 （还查看


## <a name="introduction"></a>介绍

在所有示例，我们已了解到目前为止，母版页和内容页具有已位于相同的文件夹 （网站的根文件夹） 中。 但没有的理由为什么母版页和内容页必须位于同一文件夹中。 当然，你可以在子文件夹中创建内容页。 同样，你可能创建`~/MasterPages/`放置你的站点的主控页的文件夹。

在将母版页和内容页放在不同的文件夹中的一个潜在的问题涉及中断的 Url。 如果母版页包含超链接、 图像或其他元素中的相对 Url，该链接将驻留在不同的文件夹中的内容页无效。 在本教程中，我们将检查此问题以及解决方法的源。

## <a name="the-problem-with-relative-urls"></a>使用相对 Url 问题

在网页上的 URL 被认为是*相对 URL*如果它指向该资源的位置是相对于网站的文件夹结构中的网页的位置。 任何不以前导正斜杠开头的 URL (`/`) 或协议 (如`http://`) 是相对的因为在网页上包含的 URL 的位置上基于浏览器会得到解决。

例如，我们的网站具有`~/Images/`具有单个图像文件，文件夹`PoweredByASPNET.gif`。 主控页文件`Site.master`具有`<img>`中的元素`footerContent`区域替换为以下标记：


[!code-html[Main](urls-in-master-pages-cs/samples/sample1.html)]

`src`属性中的值`<img>`元素是相对 URL，因为它不以开头`/`或`http://`。 简单地说，`src`属性值将告诉浏览器中看起来都`Images`名为的文件的子文件夹`PoweredByASPNET.gif`。

当来访内容页时，上述标记被直接发送到浏览器。 花一些时间，请访问`About.aspx`和查看发送到浏览器的 HTML 源文件。 你将找到母版页中的确切相同标记已发送到浏览器。


[!code-html[Main](urls-in-master-pages-cs/samples/sample2.html)]

如果内容页位于根文件夹中 (因为是`About.aspx`) 一切会按预期行为，因为没有方式`Images`相对于根文件夹的子文件夹。 但是，操作将分解内容页是否比主控页的其他文件夹中。 若要说明这一点，创建名为的子`Admin`。 接下来，添加一个名为的内容页`Default.aspx`到`Admin`文件夹，并确保将绑定到的新页`Site.master`母版页。

> [!NOTE]
> 在[*母版页中指定的标题、 Meta 标记和其他 HTML 标头*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)教程中我们创建一个名为的自定义的基本页类`BasePage`，自动设置内容页的标题 (如果它不显式指定了）。 不要忘记将具有新创建的页面的代码隐藏类派生自`BasePage`，以便它可以利用此功能。


创建此内容页后，你的解决方案资源管理器应类似于图 1。


![一个新文件夹和 ASP.NET 网页添加到项目](urls-in-master-pages-cs/_static/image1.png)

**图 01**： 一个新文件夹和 ASP.NET 网页添加到项目


接下来，更新`Web.sitemap`文件以包括新`<siteMapNode>`本课程中的条目。 下面的 XML 演示完整`Web.sitemap`标记，现在包括添加第三个`<siteMapNode>`元素。


[!code-xml[Main](urls-in-master-pages-cs/samples/sample3.xml)]

新创建`Default.aspx`页应具有与在四个 ContentPlaceHolders 相对应的四个内容控件`Site.master`。 将一些文本添加到内容控件引用`MainContent`ContentPlaceHolder，然后访问通过浏览器页面。 如图 2 所示，找不到浏览器`PoweredByASPNET.gif`图像文件。 这是怎么回事？

`~/Admin/Default.aspx`内容页会发送相同 HTML`footerContent`区域，如已`About.aspx`页：


[!code-html[Main](urls-in-master-pages-cs/samples/sample4.html)]

因为`<img>`元素的`src`属性是相对 URL，浏览器会尝试查找`Images`相对于，网页的文件夹位置的文件夹。 换而言之，浏览器查找映像文件`Admin/Images/PoweredByASPNET.gif`。


[![找不到 PoweredByASPNET.gif 图像文件](urls-in-master-pages-cs/_static/image3.png)](urls-in-master-pages-cs/_static/image2.png)

**图 02**:`PoweredByASPNET.gif`映像找不到文件 ([单击以查看实际尺寸的图像](urls-in-master-pages-cs/_static/image4.png))


### <a name="replacing-relative-urls-with-absolute-urls"></a>将替换为绝对 Url 的相对 Url

相对 URL 相反*绝对 URL*，这是一个以正斜杠开头 (`/`) 或如协议`http://`。 绝对 URL 指定从已知的固定点资源的位置，因为相同的绝对 URL 是在任何 web 页面，而不考虑网站的文件夹结构中的网页的位置中有效。

若要解决与图 2 所示的图像无效，我们需要更新`<img>`元素的`src`属性，以便它使用而不是一个相对的绝对 URL。 若要确定正确的绝对 URL，访问你的网站中的 web 页的某个并检查地址栏中。 如图 2 中的地址栏显示，web 应用程序的完全限定的路径是`http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/`。 因此，我们无法更新`<img>`元素的`src`属性设为以下两个绝对 url:

- `/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`

花一些时间来更新`<img>`元素的`src`属性设为使用上面所示的形式之一的绝对 URL，然后访问`~/Admin/Default.aspx`通过浏览器的页。 此时浏览器将正确查找并显示`PoweredByASPNET.gif`图像文件 （请参见图 3）。


[![PoweredByASPNET.gif 映像是现在显示](urls-in-master-pages-cs/_static/image6.png)](urls-in-master-pages-cs/_static/image5.png)

**图 03**:`PoweredByASPNET.gif`映像是现在显示 ([单击以查看实际尺寸的图像](urls-in-master-pages-cs/_static/image7.png))


在中的绝对 URL 进行硬编码，它将紧密到网站的服务器和文件夹位置，这可能会更改标你 HTML。 使用窗体的绝对 URL`http://localhost:3908/...`操作很脆弱，因为端口号前面`localhost`则会自动选择每次启动 Visual Studio 的内置 ASP.NET 开发 Web 服务器。 同样，`http://localhost`一部分才有效，在进行本地测试。 后的代码部署到生产服务器中，URL 基将更改为其他事情，如`http://www.yourserver.com`。 在窗体中的绝对 URL`/ASPNET_MasterPages_Tutorial_04_CS/...`因为此应用程序路径开发和生产服务器之间不同，通常还面临相同易。

好消息是 ASP.NET 提供一个用于生成有效的相对 URL，在运行时方法。

## <a name="usingandresolveclienturl"></a>使用`~`和`ResolveClientUrl`

而是不是绝对 URL 进行硬编码，ASP.NET 将允许页开发人员使用波形符 (`~`) 以指示 web 应用程序的根目录。 例如，在本教程前面我使用了表示法`~/Admin/Default.aspx`文本中指`Default.aspx`页面`Admin`文件夹。 `~`指示`Admin`文件夹是 web 应用程序的根的子文件夹。

`Control`类的[`ResolveClientUrl`方法](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx)采用 URL 和到相应控件所驻留的网页的相对 URL 对其进行修改。 例如，调用`ResolveClientUrl("~/Images/PoweredByASPNET.gif")`从`About.aspx`返回`Images/PoweredByASPNET.gif`。 调用从`~/Admin/Default.aspx`，但是，返回`../Images/PoweredByASPNET.gif`。

> [!NOTE]
> 因为所有 ASP.NET 服务器控件都派生自`Control`类，所有服务器控件都有权访问`ResolveClientUrl`方法。 即使`Page`类派生自`Control`类，这意味着你可以使用此方法直接从 ASP.NET 页的代码隐藏类。


### <a name="usingin-the-declarative-markup"></a>使用`~`中的声明性的标记

多个 ASP.NET Web 控件，包括 URL 相关的属性： 超链接控件具有`NavigateUrl`属性; 图像控件具有`ImageUrl`属性; 依次类推。 呈现时，这些控件将传递到其 URL 相关的属性值`ResolveClientUrl`。 因此，如果这些属性包含`~`若要指示 web 应用程序的根目录，URL 将修改为有效的相对 URL。

请记住，只有 ASP.NET 服务器控件转换`~`在其 URL 相关的属性。 如果`~`显示在静态 HTML 标记中，例如`<img src="~/Images/PoweredByASPNET.gif" />`，ASP.NET 引擎将发送`~`到浏览器的其余部分的 HTML 内容。 浏览器假定`~`是 URL 的一部分。 例如，如果浏览器收到标记`<img src="~/Images/PoweredByASPNET.gif" />`它假定有一个名为子`~`，该子文件夹`Images`包含图像文件`PoweredByASPNET.gif`。

若要解决问题中的图像标记`Site.master`，将现有`<img>`与 ASP.NET 映像 Web 控件的元素。 设置映像 Web 控件的`ID`到`PoweredByImage`，将其`ImageUrl`属性`~/Images/PoweredByASPNET.gif`，并将其`AlternateText`"提供支持的 asp.net ！"的属性


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample5.aspx)]

对母版页进行此更改后, 重新访问`~/Admin/Default.aspx`再次页。 这一次`PoweredByASPNET.gif`的页中出现的图像文件 （请参见图 3）。 当映像 Web 控件是呈现使用`ResolveClientUrl`方法解决其`ImageUrl`属性值。 在`~/Admin/Default.aspx``ImageUrl`将转换为适当的相对 URL，为以下代码段的 HTML 源所示：


[!code-html[Main](urls-in-master-pages-cs/samples/sample6.html)]

> [!NOTE]
> 除了在基于 URL 的 Web 控件属性，在使用`~`还可在调用时`Response.Redirect`和`Server.MapPath`方法，以及其他。 此外，`ResolveClientUrl`可能直接从 ASP.NET 或母版页的声明性标记，调用方法，如果需要请参阅[Fritz 透视](https://www.pluralsight.com/blogs/fritz/)的博客文章[使用`ResolveClientUrl`标记中](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)。


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>修复母版页的剩余相对 Url

除了`<img>`中的元素`footerContent`我们只需固定、 母版页包含需要我们关注的一个更相对 URL。 `topContent`区域包括链接"Master 页教程，"用于指向`Default.aspx`。


[!code-html[Main](urls-in-master-pages-cs/samples/sample7.html)]

由于此 URL 是相对的它将发送到用户`Default.aspx`他们访问的内容页的文件夹中的页。 能够始终指向此链接`Default.aspx`我们需要更换的根文件夹中`<a>`元素与超链接 Web 控件，以便我们可以使用`~`表示法。

删除`<a>`元素标记并在该处添加超链接控件。 设置超链接的`ID`到`lnkHome`，将其`NavigateUrl`属性`~/Default.aspx`，并将其`Text`属性设置为"Master 页教程"。


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample8.aspx)]

就这么简单！ 此时所有母版页和内容页，而不考虑哪些文件夹内容页呈现时，我们母版页中的 Url 正确基于位于中。

### <a name="automatic-url-resolution-in-theheadsection"></a>中的自动 URL 解析`<head>`部分

在[*创建站点范围布局使用 Master 页*](creating-a-site-wide-layout-using-master-pages-cs.md)教程，我们添加`<link>`到`Styles.css`文件中`<head>`区域：


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample9.aspx)]

虽然`<link>`元素的`href`属性是相对的它会自动转换为在运行时正确的路径。 如我们所述[*母版页中指定的标题、 Meta 标记和其他 HTML 标头*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)教程中，`<head>`区域是实际的服务器端控件，从而使其能够修改其内部控件呈现时的内容。

若要验证这一点，重新访问`~/Admin/Default.aspx`页面，查看发送到浏览器的 HTML 源文件。 如下面的代码段所示，`<link>`元素的`href`属性自动修改为适当的相对 URL， `../Styles.css`。


[!code-html[Main](urls-in-master-pages-cs/samples/sample10.html)]

## <a name="summary"></a>摘要

母版页经常包括链接、 图像和其他必须通过 URL 指定的外部资源。 因为主控页和内容页可能不存在的相同文件夹中，务必 abstain 从使用相对 Url。 可以使用硬编码的绝对 Url 时，因此紧密到 web 应用程序中利用的绝对 URL。 如果发生更改的绝对 URL-通常时那样移动或部署 web 应用程序-你将需要记住以返回并更新绝对 Url。

理想的方法是使用波形符 (`~`) 以指示应用程序根目录。 包含与 URL 相关的属性的 ASP.NET Web 控件映射`~`为在运行时应用程序根目录。 在内部，Web 控件使用`Control`类的`ResolveClientUrl`方法来生成有效的相对 URL。 此方法是公用的且可从每个服务器控件 (包括`Page`类)，因此你可以使用它以编程方式从你的代码隐藏类，如果需要。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [在 ASP.NET 母版页](http://www.odetocode.com/Articles/419.aspx)
- [母版页中的 URL 重定基址](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [使用`ResolveClientUrl`标记中](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多个 ASP/ASP.NET 丛书和 4GuysFromRolla.com 创始人，具有已使用 Microsoft Web 技术自 1998 年。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 3.5 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 可以在达到 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

>[!div class="step-by-step"]
[上一页](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
[下一页](control-id-naming-in-content-pages-cs.md)
