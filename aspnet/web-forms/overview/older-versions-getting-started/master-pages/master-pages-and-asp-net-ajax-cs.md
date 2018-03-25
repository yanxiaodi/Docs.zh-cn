---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
title: "主控页和 ASP.NET AJAX (C#) |Microsoft 文档"
author: rick-anderson
description: "讨论使用 ASP.NET AJAX 和母版页的选项。 查看使用 ScriptManagerProxy 类;讨论如何 dependi 加载这些各种 JS 文件..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 0c55eb66-ba44-4d49-98e8-5c87fd9b1111
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
msc.type: authoredcontent
ms.openlocfilehash: 6e09951be5483ed098b8cab6517335f9962a5d95
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="master-pages-and-aspnet-ajax-c"></a>主控页和 ASP.NET AJAX (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_CS.zip)或[下载 PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_CS.pdf)

> 讨论使用 ASP.NET AJAX 和母版页的选项。 查看使用 ScriptManagerProxy 类;讨论各种 JS 文件的加载方式具体取决于是否在 Master 中使用 ScriptManager 页或内容页。


## <a name="introduction"></a>介绍

过去的几年里，越来越多的开发人员构建了一种[AJAX](http://en.wikipedia.org/wiki/Ajax_(programming))-已启用 web 应用程序。 启用了 AJAX 的网站使用大量的相关的 web 技术提供更快地响应的用户体验。 创建启用了 AJAX 的 ASP.NET 应用程序是向 Microsoft 的功能极为轻松谢谢[ASP.NET AJAX 框架](../../../../ajax/index.md)。 ASP.NET AJAX 内置到 ASP.NET 3.5 和 Visual Studio 2008;它也会作为单独的下载 ASP.NET 2.0 应用程序提供。

在生成时启用了 AJAX 的网页添加与 ASP.NET AJAX 框架，则必须添加一个精确[ScriptManager 控件](https://msdn.microsoft.com/library/bb398863.aspx)到使用框架的每个页。 顾名思义，ScriptManager 管理启用了 AJAX 的网页中使用的客户端脚本。 至少，ScriptManager 发出 HTML，指示浏览器下载该构成 ASP.NET AJAX 客户端库的 JavaScript 文件。 此外可以用于注册自定义 JavaScript 文件、 脚本启用 web 服务和自定义应用程序服务功能。

如果你的站点使用主页面 （如应），则不一定需要 ScriptManager 控件添加到每个内容单页;相反，你可以将 ScriptManager 控件添加到母版页。 本教程演示如何将 ScriptManager 控件添加到母版页。 它还讨论如何使用 ScriptManagerProxy 控制在特定的内容页中注册自定义脚本和脚本服务。

> [!NOTE]
> 本教程不了解设计或构建使用 ASP.NET AJAX framework 的启用了 AJAX 的 web 应用程序。 有关使用 AJAX 的详细信息，请查阅[ASP.NET AJAX 视频](../../../videos/aspnet-ajax/index.md)和[教程](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)以及为本教程结尾处列出的其他阅读材料部分中这些资源。


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>检查发出 ScriptManager 控件的标记

ScriptManager 控件便会发出该构成 ASP.NET AJAX 客户端库会指示浏览器下载 JavaScript 文件的标记。 它还会初始化此库的页面添加内联 JavaScript 位。 以下标记显示添加到包括 ScriptManager 控件的页面的呈现输出的内容：


[!code-html[Main](master-pages-and-asp-net-ajax-cs/samples/sample1.html)]

`<script src="url"></script>`标记指示浏览器可以下载并执行的 JavaScript 文件*url*。 ScriptManager 发出三个此类标记，则一个引用的文件`WebResource.axd`，而其他两个引用文件`ScriptResource.axd`。 这些文件实际上不存在为你的网站中的文件。 相反，当这些文件中的任一的请求到达 web 服务器，则 ASP.NET 引擎检查查询字符串，然后返回合适的 JavaScript 内容。 这三个外部 JavaScript 文件所提供的脚本构成 ASP.NET AJAX 框架的客户端库。 其他`<script>`标记发出 ScriptManager 包括初始化此库的内联脚本。

对于使用 ASP.NET AJAX 框架中，但不是需要不使用框架的页的页面至关重要的外部脚本引用和由 ScriptManager 发出的内联脚本。 因此，可能原因它非常适合仅将 ScriptManager 添加到使用 ASP.NET AJAX 框架这些页面。 和，这样已足够，但如果你有很多页中使用框架添加到的所有页面的重复任务，至少可以说 ScriptManager 控件将显示。 或者，你可以向主页上，然后将此必要的脚本注入到所有内容页添加 ScriptManager。 使用此方法时，你不需要记住将 ScriptManager 添加到使用 ASP.NET AJAX 框架，因为它已包含母版页的一个新页。 步骤 1 添加到母版页 ScriptManager 的演练。

> [!NOTE]
> 如果你计划包括你母版页的用户界面中的 AJAX 功能，然后你别无选择事务中的-必须在母版页中包括 ScriptManager。


将 ScriptManager 添加到母版页的一个缺点是，在发出上述脚本*每个*页上，而不管是否其需要。 这清楚地将导致这些页面，不要使用 ASP.NET AJAX framework 的任何功能中尚未包含 （通过母版页） 包含 ScriptManager 浪费带宽。 但是，只需多少浪费带宽？

- 发出的 （如上所示） ScriptManager 的实际内容进行合计稍有超过 1 KB。
- 所引用的三个外部脚本文件`<script>`元素，但是，构成大致 450KB 的未压缩的数据; 使用 gzip 压缩的网站，在此总带宽可以减少附近 100 KB。 但是，这些脚本文件为一年，这意味着它们只需一次下载缓存由浏览器，则可以重复使用站点上的其他页中。

在最佳情况下，然后，缓存的脚本文件时, 总成本为 1 KB，即可以忽略不计。 在最坏的情况下，但是-这当脚本文件具有尚未下载和 web 服务器未使用任何形式的压缩 — 带宽命中为大约 450 KB，可以将其添加任何位置从第二个或两个通过宽带连接到最多为一分钟 通过拨号调制解调器的用户。 好消息是，因为外部脚本文件将缓存由浏览器，此最坏的情况很少发生。

> [!NOTE]
> 如果您仍觉得不妥 ScriptManager 控件置于主控页，请考虑 Web 窗体 (`<form runat="server">`母版页中的标记)。 每个 ASP.NET 页使用的回发的模型必须包含精确一个 Web 窗体。 添加 Web 窗体添加更多的内容： 一个数字的隐藏的表单字段`<form>`标记本身，并且，如有必要，JavaScript 函数适用于启动来自脚本的回发。 此标记不需要不回发的页。 无法通过从母版页删除 Web 窗体并手动将其添加到每个需求的内容页面消除此冗余的标记。 但是，Web 窗体母版页中的好处能弥补免于它不必要地添加到某些内容页不足。


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>步骤 1： 将 ScriptManager 控件添加到 Master 页

每个网页使用 ASP.NET AJAX 框架必须包含精确一个 ScriptManager 控件。 鉴于此要求，它通常有意义将单个 ScriptManager 控件在主页上，以便所有内容页具有 ScriptManager 控件自动包含。 此外，ScriptManager 必须用前面的任何 ASP.NET AJAX 服务器控件，如 UpdatePanel 和 UpdateProgress 控件。 因此，最好将 Web 窗体中任何 ContentPlaceHolder 所有控件之前 ScriptManager。

打开`Site.master`母版页和之前将 ScriptManager 控件添加到在 Web 窗体，页`<div id="topContent">`元素 （请参见图 1）。 如果你使用 Visual Web Developer 2008 或 Visual Studio 2008，ScriptManager 控件位于 AJAX Extensions 选项卡中的工具箱中。如果你使用的 Visual Studio 2005，你将需要首先安装 ASP.NET AJAX framework 并将控件添加到工具箱。 请访问[ASP.NET AJAX Wiki](https://github.com/DevExpress/AjaxControlToolkit/wiki)若要获取适用于 ASP.NET 2.0 的框架。

添加到页面的 ScriptManager 后, 更改其`ID`从`ScriptManager1`到`MyManager`。


[![将 ScriptManager 添加到 Master 页](master-pages-and-asp-net-ajax-cs/_static/image2.png)](master-pages-and-asp-net-ajax-cs/_static/image1.png)

**图 01**： 将 ScriptManager 添加到母版页 ([单击以查看实际尺寸的图像](master-pages-and-asp-net-ajax-cs/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>步骤 2： 使用内容页中的 ASP.NET AJAX 框架

与 ScriptManager 控件添加到母版页我们现在可以向任何内容页添加 ASP.NET AJAX framework 功能。 让我们创建新的 ASP.NET 页显示 Northwind 数据库中的随机选择的产品。 我们将使用 ASP.NET AJAX 框架的计时器控件来更新此显示每隔 15 秒钟，显示新产品。

通过在名为的根目录中创建一个新页启动`ShowRandomProduct.aspx`。 不要忘记将绑定到此新页`Site.master`母版页。


[![将新的 ASP.NET 页添加到网站](master-pages-and-asp-net-ajax-cs/_static/image5.png)](master-pages-and-asp-net-ajax-cs/_static/image4.png)

**图 02**： 将新的 ASP.NET 页添加到网站 ([单击以查看实际尺寸的图像](master-pages-and-asp-net-ajax-cs/_static/image6.png))


回想一下，在[*母版页中指定的标题、 Meta 标记和其他 HTML 标头*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)教程中我们创建一个名为的自定义的基本页类`BasePage`如果它已生成了该页面的标题未显式设置。 转到`ShowRandomProduct.aspx`页的代码隐藏类，并将其派生自`BasePage`(而不是从`System.Web.UI.Page`)。

最后，更新`Web.sitemap`文件以包含本课程的适当的项。 添加以下标记下的`<siteMapNode>`到内容页交互课程主机：


[!code-xml[Main](master-pages-and-asp-net-ajax-cs/samples/sample2.xml)]

此添加`<siteMapNode>`元素将反映在这些课程列表 （请参见图 5）。

### <a name="displaying-a-randomly-selected-product"></a>显示随机选择的产品

返回到`ShowRandomProduct.aspx`。 从设计器中，UpdatePanel 控件从工具箱中拖动到`MainContent`内容控件并设置其`ID`属性`ProductPanel`。 UpdatePanel 表示可以通过分部页回发异步更新的屏幕上的区域。

我们第一项任务是显示有关 UpdatePanel 中随机选择产品的信息。 通过将说明如何控件拖动到 UpdatePanel 启动。 设置说明如何控制`ID`属性`ProductInfo`和清除其`Height`和`Width`属性。 展开说明的智能标记，并从选择数据源下拉列表，选择将说明如何绑定到一个名为的新 SqlDataSource 控件`RandomProductDataSource`。


[![将说明如何绑定到新的 SqlDataSource 控件](master-pages-and-asp-net-ajax-cs/_static/image8.png)](master-pages-and-asp-net-ajax-cs/_static/image7.png)

**图 03**： 将说明如何绑定到新的 SqlDataSource 控件 ([单击以查看实际尺寸的图像](master-pages-and-asp-net-ajax-cs/_static/image9.png))


配置要连接到通过 Northwind 数据库的 SqlDataSource 控件`NorthwindConnectionString`(我们在中创建的[*与从内容页母版页交互*](interacting-with-the-content-page-from-the-master-page-cs.md)教程)。 当配置 select 语句选择指定的自定义 SQL 语句，然后输入以下查询：


[!code-sql[Main](master-pages-and-asp-net-ajax-cs/samples/sample3.sql)]

`TOP 1`中的关键字`SELECT`子句将返回仅由查询返回的第一条记录。 [ `NEWID()`函数](https://msdn.microsoft.com/library/ms190348.aspx)生成一个新[全局唯一标识符值 (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)并可以使用`ORDER BY`子句按随机顺序返回表的记录。


[![配置 SqlDataSource 以返回单个、 随机选择记录](master-pages-and-asp-net-ajax-cs/_static/image11.png)](master-pages-and-asp-net-ajax-cs/_static/image10.png)

**图 04**： 配置 SqlDataSource 以返回一个随机选择记录 ([单击以查看实际尺寸的图像](master-pages-and-asp-net-ajax-cs/_static/image12.png))


完成向导后，Visual Studio 创建上面的查询返回的两个列 BoundField。 此时页面的声明性标记应类似以下：


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample4.aspx)]

图 5 所示`ShowRandomProduct.aspx`页上查看通过浏览器时。 单击浏览器的刷新按钮以重新加载页;你应该会看到`ProductName`和`UnitPrice`针对新的随机选择记录的值。


[![显示随机产品的名称和价格](master-pages-and-asp-net-ajax-cs/_static/image14.png)](master-pages-and-asp-net-ajax-cs/_static/image13.png)

**图 05**： 显示随机产品的名称和价格 ([单击以查看实际尺寸的图像](master-pages-and-asp-net-ajax-cs/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>自动显示新产品每 15 秒钟

ASP.NET AJAX 框架包含在指定的时间; 将执行回发的计时器控件在回发计时器的`Tick`引发事件。 如果该计时器控件放置在 UpdatePanel 内就会引发分部页回发，在此期间，我们可以重新绑定数据到说明如何以显示新的随机选择的产品。

若要实现此目的，计时器从工具箱拖放到 UpdatePanel。 更改计时器的`ID`从`Timer1`到`ProductTimer`及其`Interval`从 60000 到 15000 之间的属性。 `Interval`属性指示的回发之间的毫秒数; 将其设置到 15000 之间会导致计时器以触发分部页回发每隔 15 秒。 此时计时器的声明性标记应类似以下：


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample5.aspx)]

为计时器的创建事件处理程序`Tick`事件。 我们需要此事件处理程序中重新说明如何将数据绑定通过调用说明的`DataBind`方法。 这样指示说明如何重新检索从其数据源控件，数据以选择并显示一个新的随机选择记录 （如时重新加载该页面，通过单击浏览器的刷新按钮）。


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample6.cs)]

这就是所有到它 ！ 重新访问通过浏览器页面。 最初，显示随机产品的信息。 如果您耐心观看屏幕你会注意到，15 秒后，新产品有关的信息神奇会替换现有的显示。

若要更好地查看这里会发生什么，让我们将添加到显示显示上次更新时间 UpdatePanel 的标签控件。 添加内 UpdatePanel 标签 Web 控件，设置其`ID`到`LastUpdateTime`，然后清除其`Text`属性。 接下来，为 UpdatePanel 创建事件处理程序`Load`事件和显示标签中的当前时间。 (UpdatePanel`Load`在每个完整或部分页回发时触发事件。)


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample7.cs)]

进行此更改完成后，该页面包括加载当前显示的产品的时间。 图 6 显示的页时首次访问。 图 7 显示的页更高版本 15 秒后计时器控件具有"勾选了"和 UpdatePanel 尚未刷新，以显示有关新产品信息。


[![随机选择的产品显示在页面加载](master-pages-and-asp-net-ajax-cs/_static/image17.png)](master-pages-and-asp-net-ajax-cs/_static/image16.png)

**图 06**: 随机选择的产品显示在页面加载 ([单击以查看实际尺寸的图像](master-pages-and-asp-net-ajax-cs/_static/image18.png))


[![显示新随机选择产品每隔 15 秒](master-pages-and-asp-net-ajax-cs/_static/image20.png)](master-pages-and-asp-net-ajax-cs/_static/image19.png)

**图 07**： 每隔 15 秒为单位显示新随机选择产品 ([单击以查看实际尺寸的图像](master-pages-and-asp-net-ajax-cs/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>步骤 3： 使用 ScriptManagerProxy 控件

包括 ASP.NET AJAX framework 客户端库所需的脚本，以及 ScriptManager 还可以注册自定义 JavaScript 文件，对脚本启用 Web 服务和自定义身份验证、 授权和配置文件服务的引用。 通常，此类自定义项是特定于特定页面。 但是，如果自定义脚本文件、 Web 服务引用或身份验证、 授权或配置文件服务引用在母版页中 ScriptManager 中，则将它们包括在*所有*网站的网页中。

若要添加 ScriptManager 相关自定义项添加基于按页，请使用 ScriptManagerProxy 控件。 你可以将 ScriptManagerProxy 添加到内容的页，然后注册自定义 JavaScript 文件、 Web 服务引用或身份验证、 授权或从 ScriptManagerProxy; 的配置文件服务此操作将注册特定的内容页这些服务。

> [!NOTE]
> ASP.NET 页只能存在一个不超过 ScriptManager 控件。 因此，无法将 ScriptManager 控件添加到内容页中，如果在母版页中已定义 ScriptManager 控件。 ScriptManagerProxy 的唯一目的是提供一种方法，开发人员可以在主页上，定义 ScriptManager，但仍具有基于按页添加 ScriptManager 自定义项的能力。


若要查看操作中的 ScriptManagerProxy 控件，让我们增加中的 UpdatePanel`ShowRandomProduct.aspx`以包括使用客户端脚本来暂停或恢复计时器控件的按钮。 Timer 控件具有三个客户端方法，我们可以使用来实现此所需的功能：

- `_startTimer()`-启动计时器控件
- `_raiseTick()`-发布后和引发从而导致"变化而变化，"计时器控件其`Tick`的服务器上事件
- `_stopTimer()`-停止计时器控件

让我们使用一个名为变量创建一个 JavaScript 文件`timerEnabled`和一个名为函数`ToggleTimer`。 `timerEnabled`变量指示计时器控件当前是启用还是禁用; 它将默认为 true。 `ToggleTimer`函数接受两个输入参数: 暂停/继续按钮和客户端的参考`id`计时器控件的值。 此函数在切换的值`timerEnabled`、 获取对计时器控件的引用、 启动或停止计时器 (具体取决于值`timerEnabled`)，并更新到"暂停"或"恢复"按钮的显示文本。 每次单击暂停/继续按钮时，将调用此函数。

首先创建一个新文件夹中名为的网站`Scripts`。 接下来，将新文件添加到名为的脚本文件夹`TimerScript.js`的 JScript 文件类型。


[![将新的 JavaScript 文件添加到脚本文件夹](master-pages-and-asp-net-ajax-cs/_static/image23.png)](master-pages-and-asp-net-ajax-cs/_static/image22.png)

**图 08**： 添加新的 JavaScript 文件到`Scripts`文件夹 ([单击以查看实际尺寸的图像](master-pages-and-asp-net-ajax-cs/_static/image24.png))


[![新的 JavaScript 文件已添加到网站](master-pages-and-asp-net-ajax-cs/_static/image26.png)](master-pages-and-asp-net-ajax-cs/_static/image25.png)

**图 09**: 新的 JavaScript 文件已添加到网站 ([单击以查看实际尺寸的图像](master-pages-and-asp-net-ajax-cs/_static/image27.png))


接下来，将以下脚本添加到 TimerScript.js 文件：


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample8.cs)]

现在，我们需要注册此自定义 JavaScript 文件`ShowRandomProduct.aspx`。 返回到`ShowRandomProduct.aspx`和 ScriptManagerProxy 控件添加到页; 设置其`ID`到`MyManagerProxy`。 若要注册自定义 JavaScript 文件在设计器中选择 ScriptManagerProxy 控件，然后转到属性窗口。 属性之一是标题为脚本。 选择此属性将显示图 10 中所示 ScriptReference 集合编辑器。 单击添加按钮，以包括新的脚本引用，然后输入的路径属性中的脚本文件路径： `~/Scripts/TimerScript.js`。


[![添加对 ScriptManagerProxy 控件的脚本引用](master-pages-and-asp-net-ajax-cs/_static/image29.png)](master-pages-and-asp-net-ajax-cs/_static/image28.png)

**图 10**： 添加对 ScriptManagerProxy 控件的脚本引用 ([单击以查看实际尺寸的图像](master-pages-and-asp-net-ajax-cs/_static/image30.png))


添加脚本引用 ScriptManagerProxy 控制的声明性完标记将更新以包括`<Scripts>`与单个集合`ScriptReference`条目，如下面的代码段的标记所阐释：


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample9.aspx)]

`ScriptReference`条目指示 ScriptManagerProxy 要包含在其呈现标记中的 JavaScript 文件的引用。 也就是说，通过注册自定义脚本中 ScriptManagerProxy`ShowRandomProduct.aspx`页面的呈现的输出现在包括另一个`<script src="url"></script>`标记： `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`。

现在，我们可以调用`ToggleTimer`中定义的函数`TimerScript.js`中的客户端脚本从`ShowRandomProduct.aspx`页。 添加 UpdatePanel 中的以下 HTML:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample10.aspx)]

此时将显示一个具有文本"暂停"的按钮。 每当它单击后，JavaScript 函数`ToggleTimer`称为传递中对按钮和计时器控件的 id 值的引用 (`ProductTimer`)。 请注意用于获取语法`id`计时器控件的值。 `<%=ProductTimer.ClientID%>`发出的值`ProductTimer`计时器控件`ClientID`属性。 在[*在内容页中的控件 ID 命名*](control-id-naming-in-content-pages-cs.md)教程，我们讨论了服务器端之间的差异`ID`值和生成的客户端`id`值，以及如何`ClientID`返回客户端`id`。

图 11 显示时首次通过浏览器访问此页。 计时器当前正在运行，以及更新显示的产品信息每隔 15 秒。 图 12 后单击暂停按钮后显示的屏幕。 单击暂停按钮停止计时器，并更新到"恢复"按钮的文本。 产品信息将刷新 （和继续刷新每 15 秒钟） 在用户点击恢复后。


[![单击暂停按钮以停止计时器控件](master-pages-and-asp-net-ajax-cs/_static/image32.png)](master-pages-and-asp-net-ajax-cs/_static/image31.png)

**图 11**： 单击暂停按钮以停止计时器控件 ([单击以查看实际尺寸的图像](master-pages-and-asp-net-ajax-cs/_static/image33.png))


[![单击恢复按钮以重新启动计时器](master-pages-and-asp-net-ajax-cs/_static/image35.png)](master-pages-and-asp-net-ajax-cs/_static/image34.png)

**图 12**： 单击恢复按钮以重新启动计时器 ([单击以查看实际尺寸的图像](master-pages-and-asp-net-ajax-cs/_static/image36.png))


## <a name="summary"></a>摘要

构建启用了 AJAX 的 web 应用程序使用 ASP.NET AJAX 框架时，命令性的每个启用了 AJAX 的网页，包括 ScriptManager 控件。 若要简化此过程，我们可以将 ScriptManager 添加到母版页，而无需记住将 ScriptManager 添加到每个内容页。 介绍了如何将 ScriptManager 添加主控页时查看在内容页中实现 AJAX 功能的步骤 2 到步骤 1。

如果你需要添加自定义脚本，对脚本启用 Web 服务的引用或自定义身份验证、 授权或配置文件服务添加到特定的内容页中，将 ScriptManagerProxy 控件添加到内容页，并将自定义项存在。 步骤 3 学习了如何使用 ScriptManagerProxy 在特定的内容页中注册的自定义的 JavaScript 文件。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [ASP.NET AJAX 框架](../../../../ajax/index.md)
- [ASP.NET AJAX 教程](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [ASP.NET AJAX 视频](../../../videos/aspnet-ajax/index.md)
- [使用 Microsoft ASP.NET AJAX 构建交互式用户界面](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [使用随机排序记录 NEWID](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [使用计时器控件](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多个 ASP/ASP.NET 丛书和 4GuysFromRolla.com 创始人，具有已使用 Microsoft Web 技术自 1998 年。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 3.5 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)。 可以在达到 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](interacting-with-the-content-page-from-the-master-page-cs.md)
[下一页](specifying-the-master-page-programmatically-cs.md)
