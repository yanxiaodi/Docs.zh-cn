---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
title: "母版页和网站的导航 (VB) |Microsoft 文档"
author: rick-anderson
description: "用户友好的一个常见特性是网站的它们具有一致的站点范围的页面布局和导航方案。 本教程介绍如何 y..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 022801d8-a327-4d0c-8780-6094c9cee00d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
msc.type: authoredcontent
ms.openlocfilehash: 7bb8a9bccbd9dfbbf983618ecd32588fd8f69c64
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="master-pages-and-site-navigation-vb"></a>母版页和网站的导航 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_3_VB.exe)或[下载 PDF](master-pages-and-site-navigation-vb/_static/datatutorial03vb1.pdf)

> 用户友好的一个常见特性是网站的它们具有一致的站点范围的页面布局和导航方案。 本教程介绍如何可以轻松地更新的所有页创建一致的外观和感觉。


## <a name="introduction"></a>介绍

用户友好的一个常见特性是网站的它们具有一致的站点范围的页面布局和导航方案。 ASP.NET 2.0 引入了两个新功能，从而大大简化实现站点范围页面布局和导航方案： 主页和站点导航。 母版页使开发人员可以使用指定的可编辑区域创建一个覆盖整个站点模板。 然后，此模板都可以应用于站点中的 ASP.NET 页。 为母版页的指定的可编辑区域，此类 ASP.NET 页仅需提供内容使用母版页的所有 ASP.NET 页面都是相同的母版页中的所有其他标记。 此模型允许开发人员定义并集中化站点范围页面布局，从而使其更轻松地跨可以方便地进行更新的所有页创建一致的外观和感觉。

[站点导航系统](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)提供了一种机制供网页开发人员定义站点图和一个 API，使该站点图要以编程方式查询。 新导航 Web 控件的菜单、 树视图和 SiteMapPath 更方便地呈现全部或部分的常见导航用户界面元素中的站点映射。 我们将使用默认网站的导航提供程序，这意味着我们站点图，将 XML 格式文件中定义。

若要说明这些概念，并让我们教程的网站更易于使用，让我们花本课程中定义一个站点范围的页面布局、 实现站点地图中，并将添加的导航用户界面。 本教程结束时，我们必须生成我们教程的网页的精美的网站设计。


[![本教程的最终结果](master-pages-and-site-navigation-vb/_static/image2.png)](master-pages-and-site-navigation-vb/_static/image1.png)

**图 1**: 最终结果的本教程 ([单击以查看实际尺寸的图像](master-pages-and-site-navigation-vb/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>步骤 1： 创建母版页

第一步是创建网站的主页面。 现在，我们的网站包含仅的类型化数据集 (`Northwind.xsd`中`App_Code`文件夹)，BLL 类 (`ProductsBLL.vb`， `CategoriesBLL.vb`，依此类推，所有在`App_Code`文件夹)，数据库 (`NORTHWND.MDF`中`App_Data`文件夹中），配置文件 (`Web.config`)，和 CSS 样式表文件 (`Styles.css`)。 我清理这些页和演示由于我们将在将来的教程中重新中更详细介绍这些示例使用前两个教程的 DAL 和 BLL 的文件。


![我们的项目中的文件](master-pages-and-site-navigation-vb/_static/image4.png)

**图 2**： 我们的项目中的文件


若要创建主页，右键单击解决方案资源管理器中的项目名称并选择添加新项。 然后从模板列表中选择母版页类型并将其命名`Site.master`。


[![向网站添加新的主页面](master-pages-and-site-navigation-vb/_static/image6.png)](master-pages-and-site-navigation-vb/_static/image5.png)

**图 3**： 向网站添加新的主页面 ([单击以查看实际尺寸的图像](master-pages-and-site-navigation-vb/_static/image7.png))


定义在母版页中的站点范围页面布局此处。 你可以使用设计视图，然后添加需要任何布局或 Web 控件或您可以通过在源视图中手动手动添加标记。 在我母版页我使用[级联样式表](http://www.w3schools.com/css/default.asp)用于定位和外部文件中定义的 CSS 设置样式`Style.css`。 你不能从如下所示的标记，而定义的 CSS 规则，以便导航`<div>`的内容进行了绝对定位，以便它显示在左侧，具有固定的宽度的 200 像素。

Site.master


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample1.aspx)]

母版页定义静态页面布局和使用母版页的 ASP.NET 页可以编辑的区域。 指示由 ContentPlaceHolder 控件，可以在内容中看到这些内容的可编辑区域`<div>`。 我们母版页具有单个 ContentPlaceHolder (`MainContent`)，但主页可能具有多个 ContentPlaceHolders。

上面输入的标记，切换到设计视图显示主控页的布局。 使用此母版页任何 ASP.NET 页将具有此统一的布局，与指定的标记的功能`MainContent`区域。


[![Master 页上，通过设计视图查看](master-pages-and-site-navigation-vb/_static/image9.png)](master-pages-and-site-navigation-vb/_static/image8.png)

**图 4**： 在母版页、 时查看通过设计视图 ([单击以查看实际尺寸的图像](master-pages-and-site-navigation-vb/_static/image10.png))


## <a name="step-2-adding-a-homepage-to-the-website"></a>步骤 2： 添加到网站的主页

与定义母版页，我们已准备好添加网站的 ASP.NET 页。 让我们首先，通过添加`Default.aspx`，我们网站的主页。 右键单击解决方案资源管理器中的项目名称，然后选择添加新项。 选择从模板列表和名称的 Web 窗体选项文件`Default.aspx`。 另外，请检查"选择母版页"复选框。


[![添加新的 Web 窗体，检查选择的主控页复选框](master-pages-and-site-navigation-vb/_static/image12.png)](master-pages-and-site-navigation-vb/_static/image11.png)

**图 5**： 添加新的 Web 窗体，检查选择的主控页复选框 ([单击以查看实际尺寸的图像](master-pages-and-site-navigation-vb/_static/image13.png))


单击确定按钮后，我们要求选择主页面应使用此新的 ASP.NET 页。 虽然你的项目中，可以有多个母版页，我们有一个。


[![选择应使用此 ASP.NET 页的母版页](master-pages-and-site-navigation-vb/_static/image15.png)](master-pages-and-site-navigation-vb/_static/image14.png)

**图 6**： 选择此 ASP.NET 页应使用的母版页 ([单击以查看实际尺寸的图像](master-pages-and-site-navigation-vb/_static/image16.png))


主控页后，新的 ASP.NET 页将包含以下标记：

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample2.aspx)]

在`@Page`存在指令是对使用的主控页文件的引用 (`MasterPageFile="~/Site.master"`)，且 ASP.NET 页面的标记包含的每个 ContentPlaceHolder 控件在主页上，与该控件中定义的内容控件`ContentPlaceHolderID`将内容映射到特定 ContentPlaceHolder 控制。 内容控件是所在的位置标记希望在相应 ContentPlaceHolder 中显示。 设置`@Page`指令的`Title`属性到主页，并将一些欢迎内容添加到内容控件：

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample3.aspx)]

`Title`属性中`@Page`指令使我们可以设置页面的标题从 ASP.NET 页中，即使`<title>`母版页中定义了元素。 我们还可以设置标题，以编程方式使用`Page.Title`。 另请注意，对样式表的主控页的引用 (如`Style.css`) 会自动更新，使它们在任何 ASP.NET 页中，而无论在 ASP.NET 页与母版页哪些目录如何工作。

切换到我们可以看到了我们的页面的外观的浏览器中的设计视图。 请注意，在设计中的内容可编辑区域是可编辑 ASP.NET 页查看在母版页中定义的非 ContentPlaceHolder 标记将灰显。


[![为 ASP.NET 页的设计视图显示可编辑和非可编辑区域](master-pages-and-site-navigation-vb/_static/image18.png)](master-pages-and-site-navigation-vb/_static/image17.png)

**图 7**： 为 ASP.NET 页显示两个可编辑的设计视图和非可编辑区域 ([单击以查看实际尺寸的图像](master-pages-and-site-navigation-vb/_static/image19.png))


当`Default.aspx`网页访问由的浏览器，该页面的母版页内容和 ASP ASP.NET 引擎会自动将合并。NET 的内容，并为向下发送到请求的浏览器的最后一个 HTML 呈现合并的内容。 当更新母版页的内容时，使用此母版页的所有 ASP.NET 页将都具有其重新合并的新的主页面内容下次他们请求其内容。 简单地说，母版页模型允许为单个页面布局模板是定义 （母版页） 的更改将立即反映在整个网站。

## <a name="adding-additional-aspnet-pages-to-the-website"></a>向网站添加其他 ASP.NET 页面

让我们看一段时间来将其他 ASP.NET 页存根 （stub） 添加到最终将保存各种报表演示站点。 将有多个不是，总共 35 演示因此而不是让我们创建的所有存根 （stub） 页刚才创建的第一个的几个。 由于还会有许多类别的演示，更好地管理演示添加类别的文件夹。 现在添加以下三个文件夹：

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

最后，添加新文件，如图 8 中的解决方案资源管理器中所示。 在添加每个文件，请记住要检查的"选择母版页"复选框。


![添加以下文件](master-pages-and-site-navigation-vb/_static/image20.png)

**图 8**： 添加的以下文件


## <a name="step-2-creating-a-site-map"></a>步骤 2： 创建站点图

管理网站的多个少量的页面组成的挑战之一提供为访客站点中导航的简单方式。 开始时，必须定义站点的导航结构。 接下来，此结构必须转换为可导航用户界面元素，例如菜单或痕迹导航。 最后，此整个过程需要需要维护和随着新页面添加到站点和删除现有的更新。 在 ASP.NET 2.0 中之前, 开发人员已为自己创建站点的导航结构，维护它，并将其转换到可导航用户界面元素。 使用 ASP.NET 2.0 中，但是，开发人员可以利用非常灵活站点导航系统中生成。

ASP.NET 2.0 站点导航系统提供一种是开发人员用来定义站点图，然后通过编程的 API 来访问此信息。 ASP.NET 附带的站点映射提供程序需要站点地图数据存储在 XML 文件格式设置特定方式。 但由于站点导航系统基于[提供程序模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)可以扩展以支持的其他方法的序列化站点映射信息。 Jeff Prosise 文章[SQL 站点映射提供程序你 've 已等待](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)演示如何创建站点映射提供程序，将网站映射存储在 SQL Server 数据库; 另一个选项是创建[站点地图提供商提供基于文件系统结构](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)。

对于本教程中，但是，让我们使用附带的默认 site 映射提供程序使用 ASP.NET 2.0。 若要创建站点图，只需右键单击解决方案资源管理器中的项目名称，选择添加新项，然后选择站点图选项。 保留的名称作为`Web.sitemap`，然后单击添加按钮。


[![将站点图添加到你的项目](master-pages-and-site-navigation-vb/_static/image22.png)](master-pages-and-site-navigation-vb/_static/image21.png)

**图 9**： 将站点图添加到你的项目 ([单击以查看实际尺寸的图像](master-pages-and-site-navigation-vb/_static/image23.png))


站点映射文件是一个 XML 文件。 请注意，Visual Studio 提供了 IntelliSense 站点地图结构。 站点映射文件必须具有`<siteMap>`节点作为其根节点，这必须包含精确一个`<siteMapNode>`子元素。 该第一个`<siteMapNode>`元素然后可以包含任意数目的子代`<siteMapNode>`元素。

定义要模拟的文件系统结构的站点映射。 也就是说，添加`<siteMapNode>`为每个三个文件夹和子元素`<siteMapNode>`这些文件夹中的 ASP.NET 页的每个元素如下所示：

Web.sitemap


[!code-xml[Main](master-pages-and-site-navigation-vb/samples/sample4.xml)]

站点图定义网站的导航结构，即描述站点的各个部分的层次结构。 每个`<siteMapNode>`中的元素`Web.sitemap`表示站点的导航结构中的节。


[![站点图表示分层导航结构](master-pages-and-site-navigation-vb/_static/image25.png)](master-pages-and-site-navigation-vb/_static/image24.png)

**图 10**： 站点图表示分层导航结构 ([单击以查看实际尺寸的图像](master-pages-and-site-navigation-vb/_static/image26.png))


ASP.NET 公开站点图结构通过.NET Framework 的[站点地图类](https://msdn.microsoft.com/library/system.web.sitemap.aspx)。 此类具有`CurrentNode`属性，它返回有关的信息部分用户当前访问;`RootNode`属性返回的站点图根 （家庭中我们站点图）。 同时`CurrentNode`和`RootNode`属性返回[SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx)实例，具有属性如`ParentNode`， `ChildNodes`， `NextSibling`， `PreviousSibling`，依此类推，允许站点图要遍历的层次结构。

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>步骤 3： 显示基于站点图菜单

访问 ASP.NET 2.0 中的数据可以是以编程方式完成，如在 ASP.NET 中 1.x，或以声明方式，通过新[数据源控件](https://msdn.microsoft.com/library/ms227679.aspx)。 有多个内置的数据源控件，如 SqlDataSource 控件，用于访问关系数据库数据、 ObjectDataSource 控件，以便从类和其他人访问数据。 你甚至可以创建你自己[自定义数据源控件](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp)。

数据源控件充当 ASP.NET 页和基础数据之间的代理。 为了显示一个数据源控件检索到的数据，我们通常将向页面添加另一个 Web 控件，并将其绑定到数据源控件。 若要将 Web 控件绑定到数据源控件，只需设置 Web 控件的`DataSourceID`属性的数据源控件的值`ID`属性。

若要帮助中使用站点图的数据，ASP.NET 包括 SiteMapDataSource 控件，使我们可以将针对我们网站的站点映射 Web 控件绑定。 两个 Web 控件的树视图和菜单通常用于提供导航用户界面。 若要将站点地图数据绑定到这两个控件之一，只需将 SiteMapDataSource 添加到 TreeView 以及页或菜单控件`DataSourceID`相应地设置属性。 例如，我们无法将菜单控件添加到母版页使用以下标记：


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample5.aspx)]

对于更精细的控制发出 HTML 度，我们可以将 SiteMapDataSource 控件绑定到该转发器控件，如下所示：


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample6.aspx)]

SiteMapDataSource 控制权将返回站点地图层次结构一个级别一次启动与根站点映射节点 （家庭中我们站点图），然后下一个级别 （基本报告、 筛选报表和自定义格式设置），依次类推。 当绑定到中继器 SiteMapDataSource，它枚举第一个级别返回并实例化`ItemTemplate`每个`SiteMapNode`级集中的第一个实例。 若要访问的特定属性`SiteMapNode`，我们可以使用`Eval(propertyName)`，这是我们如何获取每个`SiteMapNode`的`Url`和`Title`超链接控件的属性。

上面的转发器示例中将呈现以下标记：


[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample7.html)]

这些站点映射节点 （基本报告、 筛选报表和自定义格式设置） 构成*第二个*呈现，不的第一个站点映射的级别。 这是因为 SiteMapDataSource`ShowStartingNode`属性设置为 False，导致 SiteMapDataSource 跳过根站点映射节点并改为启动通过站点地图层次结构中返回的第二个级别。

若要显示有关基本报告、 筛选报表和自定义格式设置的子级`SiteMapNode`s，我们可以将另一个转发器添加到初始中继器`ItemTemplate`。 此第二个转发器将绑定到`SiteMapNode`实例的`ChildNodes`属性，如下所示：


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample8.aspx)]

这些两个重复字符导致 （一些标记已删除为简便起见） 的以下标记：


[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample9.html)]

使用 CSS 样式选择从[Rachel Andrew](http://www.rachelandrew.co.uk/)的预订[CSS Anthology: 101 要点，技巧&amp;Hacks](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance)、`<ul>`和`<li>`元素的风格以便标记生成以下 visual 输出：


![由两个中继器和某些 CSS 菜单](master-pages-and-site-navigation-vb/_static/image27.png)

**图 11**： 菜单由两个中继器和某些 CSS 组成


该菜单主控页中，并且绑定到站点地图中定义`Web.sitemap`，这意味着对站点图的任何更改将立即反映在所有页使用`Site.master`母版页。

## <a name="disabling-viewstate"></a>禁用视图状态

所有 ASP.NET 控件可以根据需要都保留其状态为[查看状态](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/)，其序列化为呈现的 HTML 中的隐藏的表单字段。 视图状态用于由控件回发，之间保留其以编程方式更改状态如数据绑定到数据 Web 控件。 视图状态当允许时要记住跨回发的信息，它会增加必须发送到客户端，并可能导致严重的页膨胀如果没有密切监视的标记的大小。 数据 Web 控件尤其 GridView 是标记的特别付出向页面添加多个额外千字节为单位。 尽管这种增加可能可以忽略不计为宽带或 intranet 的用户，则视图状态可以向拨号用户往返行程添加几秒钟时间。

若要查看视图状态，请访问浏览器中的页，然后查看发送的网页的源的影响 （在 Internet Explorer 中，转到视图菜单，然后选择源选项）。 你还可以启用[页跟踪](https://msdn.microsoft.com/library/sfbfw58f.aspx)若要查看使用的每个页上的控件的视图状态分配。 视图状态信息在名为的隐藏的表单字段中序列化`__VIEWSTATE`为位于`<div>`紧跟在打开之后元素`<form>`标记。 正在使用; Web 窗体时，仅保存视图状态如果你的 ASP.NET 页不包含`<form runat="server">`中不会有其声明性语法`__VIEWSTATE`呈现标记中的隐藏的表单字段。

`__VIEWSTATE`母版页由生成的窗体字段将大致 1800 字节添加到页面的生成的标记。 此额外膨胀主要是因为重复器控件，如 SiteMapDataSource 控件的内容保存到视图状态。 虽然额外的 1800 字节似乎不太多工作要具有许多字段和记录使用 GridView 时，获取有关，很高兴的视图状态可以轻松地膨胀按 10 或更多的系数。

可以通过设置页或控件级别禁用视图状态`EnableViewState`属性`False`，从而减小呈现标记的大小。 自数据 Web 控件保持在回发绑定到数据 Web 控件的数据的视图状态，当禁用数据的视图状态 Web 控件数据必须绑定上每个回发。 在 ASP.NET 版本 1.x 这一职责孙的肩膀上，页开发人员;使用 ASP.NET 2.0 中，但是，数据 Web 控件将重新绑定到其每次回发的数据源控件必要。

若要让我们减少页面的视图状态将设置转发器控件的`EnableViewState`属性`False`。 这可以通过属性窗口，在设计器中或以声明方式在源视图中完成。 进行此更改后转发器的声明性标记应如下所示：


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample10.aspx)]

此更改后，页上的呈现视图状态大小收缩到单纯后 52 字节，97%节省视图中状态大小 ！ 在这一系列整个教程中我们将禁用默认情况下的 Web 控件的数据的视图状态，以减少呈现标记的大小。 中的大部分示例`EnableViewState`属性将设置为`False`和执行此操作没有规定。 时，将讨论状态的视图是在其中必须在数据的顺序中启用它的情况下才对 Web 进行控制，以提供其预期的功能。

## <a name="step-4-adding-breadcrumb-navigation"></a>步骤 4： 添加痕迹导航

若要完成母版页，让我们将痕迹导航 UI 元素添加到每个页面。 痕迹导航快速向用户显示其站点层次结构中的当前位置。 添加 ASP.NET 2.0 中的痕迹只轻松 SiteMapPath 控件添加到页;不需要任何代码。

对于我们的站点，将此控件添加到标头`<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample11.aspx)]

痕迹导航显示当前页用户访问在站点地图层次结构，以及该站点映射节点"上级，"上，一直到的根 （家庭中我们站点图）。


![痕迹导航显示当前页，并在站点中的其上级映射层次结构](master-pages-and-site-navigation-vb/_static/image28.png)

**图 12**： 痕迹导航显示当前页，并在站点中的其上级映射层次结构


## <a name="step-5-adding-the-default-page-for-each-section"></a>步骤 5： 添加每个部分的默认页

我们的站点中的教程将分解为不同类别基本报告 Filtering、 自定义格式设置，与每个类别以及作为该文件夹中的 ASP.NET 页的相应教程的文件夹，依此类推。 此外，每个文件夹包含`Default.aspx`页。 此默认页上，让我们显示全部教程： 当前的部分。 也就是说，对于`Default.aspx`中`BasicReporting`文件夹我们将具有指向`SimpleDisplay.aspx`， `DeclarativeParams.aspx`，和`ProgrammaticParams.aspx`。 在这里，同样，我们可以使用`SiteMap`中定义的类和数据 Web 控件来显示此信息基于站点图`Web.sitemap`。

让我们显示同样，但这次我们将显示标题和说明的教程使用中继器未经排序的列表。 由于标记和代码来完成此项将需要为每个重复`Default.aspx`页上，我们可以封装在此 UI 逻辑[用户控件](https://msdn.microsoft.com/library/y6wb1a0e.aspx)。 调用的网站中创建一个文件夹`UserControls`和将类型 Web 用户控件名为的新项添加到该`SectionLevelTutorialListing.ascx`，并添加以下标记：


[![将新的 Web 用户控件添加到用户控件文件夹](master-pages-and-site-navigation-vb/_static/image30.png)](master-pages-and-site-navigation-vb/_static/image29.png)

**图 13**： 添加到新的 Web 用户控件`UserControls`文件夹 ([单击以查看实际尺寸的图像](master-pages-and-site-navigation-vb/_static/image31.png))


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.vb


[!code-vb[Main](master-pages-and-site-navigation-vb/samples/sample13.vb)]

在前面的转发器示例中，我们将绑定`SiteMap`转发器的数据以声明方式;`SectionLevelTutorialListing`用户控件，但是，这样做以编程方式。 在`Page_Load`事件处理程序，将进行检查以确保此页 s URL 映射到站点地图中的节点。 如果在不具有对应的页面中使用此用户控件`<siteMapNode>`条目，`SiteMap.CurrentNode`将返回`Nothing`和任何数据将绑定到转发器。 假设我们有`CurrentNode`，我们将绑定其`ChildNodes`转发器的集合。 由于我们站点图设置以便`Default.aspx`每个部分中的页面的该部分中的教程的所有父节点，此代码将显示的链接和说明的所有部分的教程，如以下屏幕截图中所示。

一旦创建此转发器后，打开`Default.aspx`页在每个文件夹，请转到设计视图中，并只需将用户控件拖动到设计图面上的解决方案资源管理器从想要显示的教程列表。


[![用户在控件有已添加到 Default.aspx](master-pages-and-site-navigation-vb/_static/image33.png)](master-pages-and-site-navigation-vb/_static/image32.png)

**图 14**： 用户控件具有已添加到`Default.aspx`([单击以查看实际尺寸的图像](master-pages-and-site-navigation-vb/_static/image34.png))


[![列出 Reporting 入门教程](master-pages-and-site-navigation-vb/_static/image36.png)](master-pages-and-site-navigation-vb/_static/image35.png)

**图 15**： 列出基本 Reporting 教程 ([单击以查看实际尺寸的图像](master-pages-and-site-navigation-vb/_static/image37.png))


## <a name="summary"></a>摘要

使用定义的站点映射和母版页完成，我们现在具有一致的页面布局和导航方案为我们与数据相关的教程。 我们将添加到我们的站点的多少页面，无论更新站点范围页的布局或站点导航信息是由于正在集中式此信息的快速而简单的过程。 具体而言，在母版页中定义的页布局信息`Site.master`和站点中的映射`Web.sitemap`。 我们不需要编写*任何*代码来实现此站点范围页面布局和导航机制，和我们保留在 Visual Studio 中的完整所见即所得设计器支持。

已完成的数据访问层和业务逻辑层和具有定义一致的页面布局和站点导航，我们已准备好开始浏览报表的常见模式。 接下来三个教程中我们将查看显示从 BLL GridView、 说明如何和 FormView 控件中检索的数据的基本报表任务。

尽情享受编程 ！

## <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [ASP.NET Master 页概述](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [在 ASP.NET 2.0 中的母版页](http://odetocode.com/Articles/419.aspx)
- [ASP.NET 2.0 设计模板](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [ASP.NET 站点导航概述](https://msdn.microsoft.com/library/e468hxky.aspx)
- [检查 ASP.NET 2.0 的网站的导航](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [ASP.NET 2.0 站点导航功能](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [了解 ASP.NET 视图状态](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [如何： 启用 ASP.NET 页跟踪](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [ASP.NET 用户控件](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已沈 Shulok、 Dennis Patterson 和希尔顿 Giesenow。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一篇](creating-a-business-logic-layer-vb.md)
