---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: "ASP.NET MVC 4 移动功能 |Microsoft 文档"
author: Rick-Anderson
description: "现在有了与在部署 ASP.NET MVC 5 移动 Web 应用程序 Azure 网站上的代码示例本教程的 MVC 5 版本。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: f4e0e4eb558e0c7b9e94fc83ede986fa4c666739
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-mvc-4-mobile-features"></a>ASP.NET MVC 4 移动功能
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> 现在有了具有代码示例在本教程的 MVC 5 版本[部署 ASP.NET MVC 5 移动 Web 应用程序 Azure 网站上](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/)。


本教程将教您如何使用 ASP.NET MVC 4 Web 应用程序中的移动功能的基础知识。 对于本教程中，你可以使用[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)或 Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer 或 VWD&quot;)。 如果你已有的你可以使用 Visual Studio 的专业版。

在开始之前，请确保已安装下面列出的先决条件。

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) （推荐） 或 Visual Studio Web Developer Express SP1。 Visual Studio 2012 包含 ASP.NET MVC 4。 如果使用的 Visual Web Developer 2010，则必须安装[ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)。

你还需要移动浏览器模拟器。 以下任一起作用：

- [Windows 7 Phone 仿真程序](https://msdn.microsoft.com/library/ff402563(VS.92).aspx)。 （这是本教程中使用大部分屏幕快照中的仿真程序）。
- 更改用户代理字符串以模拟 iPhone。 请参阅[这](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)博客文章。
- [Opera Mobile Emulator](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/)与用户代理设置为 iPhone。 有关如何将 Safari 中的用户代理设置为"iPhone"的说明，请参阅[如何让 Safari 模拟很 IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) David Alison 的博文上。

与 C# 源代码的 visual Studio 项目有本主题可以附带：

- [初学者项目下载](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [已完成项目下载](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>你将生成

本教程中，你将添加移动功能中提供的简单会议列表应用[初学者项目](https://go.microsoft.com/fwlink/?LinkId=228307)。 下面的屏幕截图显示已完成的应用程序的标记页中所示[Windows 7 Phone Emulator](https://msdn.microsoft.com/library/ff402563(VS.92).aspx)。 请参阅[键盘映射为 Windows Phone 仿真程序](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx)来简化键盘输入。

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

你可以使用 Internet Explorer 版本 9 或 10，FireFox 或 Chrome 开发移动应用程序通过设置[用户代理字符串](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)。 下图显示已完成本教程使用 Internet Explorer 模拟 iPhone。 你可以使用 Internet Explorer F-12 开发人员工具和[Fiddler 工具](http://www.fiddler2.com/fiddler2/)来帮助调试你的应用程序。

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>你将学习的技能

下面是你将要掌握的内容：

- ASP.NET MVC 4 模板如何使用 HTML5`viewport`属性和自适应呈现来改善在移动设备上显示。
- 如何创建移动特定视图。
- 如何创建视图切换器移动视图和应用程序的桌面视图之间进行该允许用户切换。

### <a name="getting-started"></a>入门

下载会议列表应用程序初学者项目使用以下链接：[下载](https://go.microsoft.com/fwlink/?LinkId=228307)。 然后在 Windows 资源管理器，右键单击*MvcMobile.zip*文件，然后选择**属性**。 在**MvcMobile.zip 属性**对话框框中，选择**解除阻止**按钮。 (取消阻止一条安全警告，当你尝试使用*.zip*已从 web 下载的文件。)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

右键单击*MvcMobile.zip*文件，然后选择**提取所有**来解压缩该文件。 在 Visual Studio 中，打开*MvcMobile.sln*文件。

按 CTRL + F5 运行应用程序，会将其显示在桌面浏览器中。 启动移动浏览器模拟器，将会议应用程序的 URL 复制到模拟器，，然后单击**按标记浏览**链接。 如果你使用的 Windows Phone 仿真程序，在 URL 栏中单击，然后按 Pause 键来访问键盘。 下面的图像显示*AllTags*视图 (选择**按标记浏览**)。

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

显示为移动设备上一目了然。 选择 ASP.NET 链接。

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

ASP.NET 标记视图显示非常混乱。 例如，**日期**列是很难阅读。 稍后在本教程中，你将创建的版本*AllTags*视图，它专门针对移动浏览器和，这将使显示可读。

注意： 当前中存在一个 bug 移动缓存引擎。 对于生产应用程序，必须安装[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes)有用的功能包。 请参阅[ASP.NET MVC 4 移动缓存 Bug 固定](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)有关此修补程序的详细信息。

## <a name="css-media-queries"></a>CSS 媒体查询

[CSS 媒体查询](http://www.w3.org/TR/css3-mediaqueries/)是一种扩展对 CSS 的媒体类型。 它们允许你创建重写特定浏览器 （用户代理） 的默认 CSS 规则的规则。 面向移动浏览器的 CSS 的通用规则定义最大的屏幕大小。 *Content\Site.css*创建新的 ASP.NET MVC 4 Internet 项目时创建的文件包含以下媒体查询：

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

如果浏览器窗口为 850 像素宽或更小，它将使用在此媒体块中的 CSS 规则。 像这样的 CSS 媒体查询可用于更宽的桌面浏览器显示在比默认 CSS 规则所设计的小 （如移动浏览器） 的浏览器提供 HTML 内容的更好地显示。

## <a name="the-viewport-meta-tag"></a>视区元标记

大多数移动浏览器定义虚拟浏览器窗口宽度 (*视区*) 即远大于移动设备的实际宽度。 这样，移动浏览器以适应整个 web 页面内的虚拟显示器。 用户然后可以放大有趣的内容。 但是，如果你的视区宽度设置为实际设备宽度时，没有缩放是必需的因为内容适合在移动浏览器中。

视区`<meta>`ASP.NET MVC 4 布局文件中的标记将视区设置为设备宽度。 以下行显示视区`<meta>`ASP.NET MVC 4 布局文件中的标记。

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>检查 CSS 媒体查询和视区元标记的执行效果

打开*views/shared\\_Layout.cshtml*文件在编辑器中和注释掉视区`<meta>`标记。 以下标记显示注释掉的行。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

打开*MvcMobile\Content\Site.css*文件在编辑器中，并将媒体查询中的最大宽度更改为零像素。 这将在移动浏览器中使用阻止的 CSS 规则。 以下行显示修改后的媒体查询：

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

保存所做的更改，并浏览到会议应用程序在移动浏览器模拟器。 在下图中的小文本是删除视区的结果`<meta>`标记。 使用没有视区`<meta>`标记中，浏览器时将缩小到默认视区宽度 (850 像素或更大范围对于大多数移动浏览器。)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

撤消所做的更改 — 取消注释视区`<meta>`布局文件中标记并将媒体查询还原到 850 像素*Site.css*文件。 保存所做的更改并刷新移动浏览器中，若要验证已恢复适合移动应用显示。

视区`<meta>`标记和 CSS 媒体查询不特定于 ASP.NET MVC 4，并且您可以利用这些功能在任何 web 应用程序。 但它们现在内置于你创建新的 ASP.NET MVC 4 项目时，将生成的文件。

有关更多信息视区`<meta>`标记中，请参阅[两个视区 A tale-第二部分](http://www.quirksmode.org/mobile/viewports2.html)。

在下一部分中，你将看到如何提供移动浏览器的特定视图。

## <a name="overriding-views-layouts-and-partial-views"></a>重写视图、 布局和分部视图

ASP.NET MVC 4 中的一个重要的新功能是一个简单的机制，你可以重写任何视图 （包括布局和分部视图） 的移动浏览一般情况下，单个移动浏览器，或任何特定浏览器。 若要提供移动特定视图，你可以复制视图文件并添加*。移动*到的文件名称。 例如，若要创建移动*索引*视图中，复制*Views\Home\Index.cshtml*到*Views\Home\Index.Mobile.cshtml*。

在本部分中，你将创建一个移动特定布局文件。

若要开始，将复制*views/shared\\_Layout.cshtml*到*views/shared\\_Layout.Mobile.cshtml*。 打开 *\_Layout.Mobile.cshtml*和更改从标题**MVC4 会议**到**会议 （移动）**。

在每个`Html.ActionLink`调用，删除"浏览者"中每个链接*ActionLink*。 下面的代码演示了移动布局文件的已完成的正文部分。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

复制*Views\Home\AllTags.cshtml*文件为*Views\Home\AllTags.Mobile.cshtml*。 打开新文件并将更改`<h2>`元素从"Tags"到"Tags (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

浏览到标签页使用桌面浏览器，并使用移动浏览器模拟器。 移动浏览器模拟器将显示你所做的两个更改。

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

相比之下，桌面显示并未变化。

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>浏览器特定的视图

除了移动特定和桌面特定视图中，你可以创建单独的浏览器的视图。 例如，你可以创建专门用于 iPhone 浏览器的视图。 在本部分中，你将创建为 iPhone 浏览器和 iPhone 版本的布局*AllTags*视图。

打开*Global.asax*文件并添加以下代码`Application_Start`方法。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

此代码定义名为"iPhone"将与每个传入请求匹配的新显示模式。 如果传入请求与定义 （即，如果用户代理包含字符串"iPhone"） 的条件匹配，ASP.NET MVC 将查找名称中包含"iPhone"后缀的视图。

在代码中，右键单击`DefaultDisplayMode`，选择**解决**，然后选择`using System.Web.WebPages;`。 这将添加到引用`System.Web.WebPages`命名空间，这是 where`DisplayModes`和`DefaultDisplayMode`类型定义。

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

或者，也可以只需手动添加以下行将对`using`文件部分。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

完整内容*Global.asax*文件如下所示。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

保存更改。 复制*MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*文件为*MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*。 打开新文件，然后将更改`h1`标题从`Conference (Mobile)`到`Conference (iPhone)`。

复制*MvcMobile\Views\Home\AllTags.Mobile.cshtml*文件为*MvcMobile\Views\Home\AllTags.iPhone.cshtml*。 在新的文件中，更改`<h2>`元素从"Tags (M)"为"Tags (iPhone)"。

运行该应用程序。 运行移动浏览器模拟器，请确保其用户代理设置为"iPhone"，并浏览到*AllTags*视图。 以下屏幕快照显示*AllTags*视图中呈现[Safari](http://www.apple.com/safari/download/)浏览器。 你可以下载 Safari Windows[此处](https://support.apple.com/kb/DL1531)。

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

在本部分中，我们已了解如何创建移动布局和视图以及如何创建布局和用于特定设备，例如 iPhone 的视图。 在下一部分中，你将看到如何利用多个令人信服的移动视图的 jQuery Mobile。

## <a name="using-jquery-mobile"></a>使用 jQuery Mobile

[JQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html)库提供了适用于所有主要移动浏览器用户界面框架。 jQuery Mobile 适用*渐进增强*的移动浏览器支持 CSS 和 JavaScript。 渐进增强允许所有浏览器来同时允许更强大的浏览器和设备拥有更丰富的显示中显示的网页，基本内容。 JQuery Mobile 中附带了 JavaScript 和 CSS 文件设置的许多元素以适应移动浏览器不需要更改任何标记样式。

在本部分中，你将安装*jQuery.Mobile.MVC* NuGet 包，其中安装 jQuery Mobile 和视图切换器小组件。

若要开始，删除*共享\\_Layout.Mobile.cshtml*和*共享\\_Layout.iPhone.cshtml*前面创建的文件。

重命名*Views\Home\AllTags.Mobile.cshtml*和*Views\Home\AllTags.iPhone.cshtml*文件到*Views\Home\AllTags.iPhone.cshtml.hide*和*将 Views\Home\AllTags.Mobile.cshtml.hide*。 因为文件不再具有*.cshtml*扩展，它们不会用于 ASP.NET MVC 运行时呈现*AllTags*视图。

安装*jQuery.Mobile.MVC*通过执行此操作的 NuGet 包：

1. 从**工具**菜单上，选择**库程序包管理器**，然后选择**程序包管理器控制台**。

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. 在**程序包管理器控制台**，输入`Install-Package jQuery.Mobile.MVC -version 1.0.0`

下图显示了文件添加和更改到 MvcMobile 项目 jQuery.Mobile.MVC NuGet 包。 [添加] 追加的文件名称后添加的文件。 映像不会显示 GIF 和 PNG 文件添加到*Content\images*文件夹。

![](aspnet-mvc-4-mobile-features/_static/image21.png)

JQuery.Mobile.MVC NuGet 程序包将安装以下：

- *应用\_Start\BundleMobileConfig.cs*文件，该引用添加的 jQuery JavaScript 和 CSS 文件所需文件。 您必须按照下面的说明并引用该文件中定义的移动捆绑。
- jQuery Mobile CSS 文件。
- A`ViewSwitcher`控制器小组件 (*Controllers\ViewSwitcherController.cs*)。
- jQuery Mobile JavaScript 文件。
- JQuery Mobile 样式的布局文件 (*views/shared\\_Layout.Mobile.cshtml*)。
- 视图切换器分部视图*(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*)，它提供要从桌面视图，反之亦然，这样和移动视图切换每页顶部的链接。
- 多个*.png*和*.gif*中的图像文件*Content\images*文件夹。

打开*Global.asax*文件并添加以下代码作为的最后一行`Application_Start`方法。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

下面的代码演示完整*Global.asax*文件。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> 如果你使用 Internet Explorer 9 并且看不到`BundleMobileConfig`中黄色突出显示的行上方，单击[兼容性视图按钮](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![（关闭） 兼容性视图按钮的图片](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "（关闭） 兼容性视图按钮的图片")IE 以使更改从一个轮廓的图标中![（关闭） 兼容性视图按钮的图片](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "（关闭） 兼容性视图按钮的图片")为纯色![（上） 的兼容性视图按钮的图片](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "（上） 的兼容性视图按钮的图片")。 另外还可以查看本教程中 FireFox 或 Chrome。


打开*MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*文件并添加以下标记直接后的`Html.Partial`调用：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

完整*MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*文件如下所示：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

生成应用程序，并在移动浏览器模拟器浏览到*AllTags*视图。 你看到以下信息：

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> 你可以调试移动特定代码[设置的用户代理字符串](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)对于 IE 或 Chrome 到 iPhone，然后使用 F-12 开发人员工具。 如果你的移动浏览器中未显示**主页**，**扬声器**，**标记**，和**日期**为按钮的链接、 对 jQuery Mobile 的引用脚本和 CSS 文件可能不是正确的。


除了样式发生改变，你看到**显示移动视图**和链接，让你从移动视图切换到桌面视图。 选择**桌面视图**显示链接和桌面视图。

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

桌面视图不提供直接导航回移动视图的方法。 你将现在修复此问题。 打开*views/shared\\_Layout.cshtml*文件。 页的紧下方`body`元素中，添加以下代码来呈现视图切换器小组件：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

刷新*AllTags*在移动浏览器中的视图。 你现在可以桌面和移动视图之间进行导航。

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> 调试注意： 你可以将以下代码添加到 views/shared 末尾\\_ViewSwitcher.cshtml 来帮助调试视图，当使用浏览器用户代理字符串设置为移动设备。
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  并添加以下将发往的*views/shared\\_Layout.cshtml*文件。  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


浏览到*AllTags*在桌面浏览器中的页。 视图切换器小组件将不显示在桌面浏览器中，因为它仅添加到了移动布局页面。 在教程后面部分中，你将看到如何将视图切换器小组件添加到桌面视图。

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>改进发言人列表

在移动浏览器中，选择**发言人**链接。 因为没有移动视图 (*AllSpeakers.Mobile.cshtml*)，默认发言人显示 (*AllSpeakers.cshtml*) 使用移动布局视图呈现 ( *\_Layout.Mobile.cshtml*)。

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

你可以通过设置全局禁止默认 （非移动） 视图在移动布局内呈现`RequireConsistentDisplayMode`到`true`中*视图\\_ViewStart.cshtml*文件，如下：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

当`RequireConsistentDisplayMode`设置为`true`，移动布局 (*\_Layout.Mobile.cshtml*) 只用于移动视图。 (即，视图文件仅在窗体 ***ViewName**.Mobile.cshtml *。）你可能想要设置`RequireConsistentDisplayMode`到`true`如果你的移动布局不太适合你的非移动视图。 下面显示的屏幕截图如何*发言人*呈现页面时`RequireConsistentDisplayMode`设置为`true`。

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

你可以通过设置来禁用视图中一致的显示模式`RequireConsistentDisplayMode`到`false`视图文件中。 中的以下标记*Views\Home\AllSpeakers.cshtml*文件中设置`RequireConsistentDisplayMode`到`false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>创建移动发言人视图

如你刚才看到*发言人*视图虽然可读，但链接字迹小，不易在移动设备上点击。 在本部分中，你将创建移动特定*发言人*看起来像一个现代移动应用程序的视图-字迹大、 易于点击链接，并包含可快速查找发言人的搜索框。

复制*AllSpeakers.cshtml*到*AllSpeakers.Mobile.cshtml*。 打开*AllSpeakers.Mobile.cshtml*文件并删除`<h2>`标题元素。

在`<ul>`标记中，添加`data-role`属性，并将其值设置为`listview`。 像其他[`data-*`属性](http://html5doctor.com/html5-custom-data-attributes/)，`data-role="listview"`使大型列表项更易于点击。 这是完成的标记如下所示：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

刷新移动浏览器。 更新的视图类似如下所示：

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

尽管移动视图得到了改进，很难导航长的发言人列表。 若要解决此问题，在`<ul>`标记中，添加`data-filter`属性，并将其设置为`true`。 下面的代码显示`ul`标记。

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

下图显示了在得到的结果页顶部的搜索筛选器框`data-filter`属性。

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

在搜索框中键入每个字母时，jQuery Mobile 将筛选显示的列表，如下面的图像中所示。

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>改进标签列表

喜欢默认值*发言人*视图中，*标记*视图虽然可读，但链接字迹小，不易在移动设备上点击。 在此部分中，需要修复*标记*查看你固定的相同方式*发言人*视图。

删除&quot;隐藏&quot;后缀到*Views\Home\AllTags.Mobile.cshtml.hide*文件的名称是因此*Views\Home\AllTags.Mobile.cshtml*。 打开重命名的文件，移除`<h2>`元素。

添加`data-role`和`data-filter`特性以`<ul>`标记，如下所示：

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

下图显示筛选字母的标签页`J`。

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>改进日期列表

你可以提高*日期*查看像改进*发言人*和*标记*视图，以便更轻松地在移动设备上使用。

复制*Views\Home\AllDates.cshtml*文件为*Views\Home\AllDates.Mobile.cshtml*。 打开新文件，移除`<h2>`元素。

添加`data-role="listview"`到`<ul>`标记，如下：

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

下图显示什么**日期**页如下所示使用`data-role`就地的属性。

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png)的内容替换*Views\Home\AllDates.Mobile.cshtml*文件替换为以下代码：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

此代码将所有会话都分组天。 它为每个新的一天，创建一个列表分隔线，并列出下分隔符每一天的所有会话。 下面是在此代码运行如下所示：

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>改进 SessionsTable 视图

在此部分中，你将创建会话一个移动特定的视图。 我们所做的更改将比在我们创建了其他视图中更广泛。

在移动浏览器中，点击**扬声器**按钮，然后输入`Sc`的搜索框中。

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

点击**Scott Hanselman**链接。

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

如你所见，显示很难在移动浏览器上阅读。 日期列难以阅读和标签列也超出了视图。 若要解决此问题，将复制*Views\Home\SessionsTable.cshtml*到*Views\Home\SessionsTable.Mobile.cshtml*，然后将文件的内容替换为以下代码：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

代码移除了 room 和 tag 列，以及垂直，格式标题、 发言人和日期，以便即可在移动浏览器中阅读所有此信息。 下图反映了代码更改。

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>改进 SessionByCode 视图

最后，你将创建的一个移动特定视图*SessionByCode*视图。 在移动浏览器中，点击**扬声器**按钮，然后输入`Sc`的搜索框中。

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

点击**Scott Hanselman**链接。 将显示 Scott Hanselman 的会话。

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

选择**An Overview of MS Web Stack of Love**链接。

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

默认桌面视图虽然不错，但可以改进。

复制*Views\Home\SessionByCode.cshtml*到*Views\Home\SessionByCode.Mobile.cshtml*和的内容替换*Views\Home\SessionByCode.Mobile.cshtml*文件替换为以下标记：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

新的标记使用`data-role`属性以改进视图的布局。

刷新移动浏览器。 下图反映你刚才所做的代码更改：

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>便捷和评审

本教程中引入了新的移动功能的 ASP.NET MVC 4 开发者预览版。 移动功能包括：

- 能够重写布局、 视图和分部视图，在全局以及针对单个视图。
- 控制布局和分部重写强制使用`RequireConsistentDisplayMode`属性。
- 移动视图切换器小组件视图不是也可以在桌面视图中显示。
- 支持特定浏览器，如 iPhone 浏览器的支持。

## <a name="see-also"></a>请参阅

- [jQuery Mobile](http://jquerymobile.com)站点。
- [jQuery Mobile 概述](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [W3C 建议移动 Web 应用程序的最佳做法](http://www.w3.org/TR/mwabp/)
- [用于媒体查询的 W3C 候选建议方案](http://www.w3.org/TR/css3-mediaqueries/)
