---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs
title: "在母版页 (C#) 中指定的标题、 Meta 标记和其他 HTML 标头 |Microsoft 文档"
author: rick-anderson
description: "查找在不同的技术来定义已分类&lt;头&gt;主页面，从内容页中的元素。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 0aa1c84f-c9e2-4699-b009-0e28643ecbc6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 30324c45fd8acbcba43808307512ef7aecffe695
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-c"></a>在母版页 (C#) 中指定的标题、 Meta 标记和其他 HTML 标头
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_CS.zip)或[下载 PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_CS.pdf)

> 查找在不同的技术来定义已分类&lt;头&gt;主页面，从内容页中的元素。


## <a name="introduction"></a>介绍

在 Visual Studio 2008 中创建的新主页面具有，默认情况下，两个 ContentPlaceHolder 控件： 一个名为 head、，位于`<head>`元素，则和一个名为`ContentPlaceHolder1`、 放置在 Web 窗体。 用途`ContentPlaceHolder1`是可以按页基础可自定义的 Web 窗体中定义的区域。 `head` ContentPlaceHolder 启用要添加到自定义内容页`<head>`部分。 （当然，可以修改这些两个 ContentPlaceHolders，或将其删除，并且其他 ContentPlaceHolder 可以添加到母版页。 我们母版页， `Site.master`，当前具有四个 ContentPlaceHolder 控件。)

HTML`<head>`元素充当有关网页文档不是文档本身的信息的存储库。 这包括如网页的标题的信息，通过搜索引擎或内部爬网程序，并链接到外部资源，例如 RSS 源、 JavaScript 和 CSS 文件使用的元信息。 某些此信息可能是相关网站中的所有页。 例如，你可能想要全局导入的相同的 CSS 规则和每个 ASP.NET 页的 JavaScript 文件。 但是，有的某些部分`<head>`是特定于页的元素。 页面标题是一个极好示例。

在本教程中，我们将查看如何定义全局和特定于页的`<head>`部分标记在母版页中和在其内容页中。

## <a name="examining-the-master-pagesheadsection"></a>检查母版页的`<head>`部分

由 Visual Studio 2008 创建的默认主控页文件包含中的以下标记其`<head>`部分：


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample1.aspx)]

请注意，`<head>`元素包含`runat="server"`属性，它指明它是服务器控件 （而非静态 HTML）。 所有 ASP.NET 页面都派生自[`Page`类](https://msdn.microsoft.com/library/system.web.ui.page.aspx)，位于`System.Web.UI`命名空间。 此类包含`Header`属性可访问该页面的`<head>`区域。 使用[`Header`属性](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx)我们可以设置 ASP.NET 页的标题，或将其他标记添加到呈现`<head>`部分。 很有可能，然后，以自定义内容页面的`<head>`通过在页中编写的代码元素`Page_Load`事件处理程序。 我们将研究如何以编程方式在步骤 1 中设置页面的标题。

中所示的标记`<head>`上面的元素还包括一个名为 h e a d 的 ContentPlaceHolder 控件。 由于内容页可以添加到自定义内容，可能不是必需的此 ContentPlaceHolder 控件`<head>`元素以编程方式。 它很有用，但是，在内容页需要添加到的静态标记的情况下`<head>`到相应的内容控件而不是以编程方式可以以声明方式添加为静态标记的元素。

除了`<title>`元素和 head ContentPlaceHolder，母版页的`<head>`元素应包含任何`<head>`-级别普遍适用于所有页面的标记。 在我们的网站，所有页都使用中定义的 CSS 规则`Styles.css`文件。 因此，我们更新`<head>`中的元素[*母版页中使用创建站点范围布局*](creating-a-site-wide-layout-using-master-pages-cs.md)教程，以包括相应`<link>`元素。 我们`Site.master`母版页的当前`<head>`标记如下所示。


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>步骤 1： 设置内容页的标题

通过指定网页的标题`<title>`元素。 请务必设置为适当的值的每一页的标题。 当来访页时，其标题将显示在浏览器的标题栏中。 此外，如果书签页上，浏览器使用页面的标题为书签的建议名称。 此外，许多搜索引擎向页的标题显示搜索结果时显示。

> [!NOTE]
> 默认情况下，Visual Studio 会将设置`<title>`滚动到"未命名页"母版页中的元素。 同样，新的 ASP.NET 页具有其`<title>`太设置为"无标题页上，"。 因为这样做可以很容易忘记设置为适当的值的页面的标题，很多页上没有 Internet 与标题"无标题页"。 网页包含此标题搜索 Google 返回大致 2,460,000 结果。 即使 Microsoft 受到的标题为"无标题页面"发布 web 页。 在撰写本文时，Google 搜索报告 236 Microsoft.com 域中对此类 web 页。


ASP.NET 页可以指定其标题中通过以下方式之一：

- 通过将放置的值在直接`<title>`元素
- 使用`Title`属性中`<%@ Page %>`指令
- 以编程方式设置页面的`Title`属性使用如下代码`Page.Title="title"`或`Page.Header.Title="title"`。

内容页没有`<title>`母版页中定义了元素，因为它。 因此，若要设置内容页的标题既可以使用`<%@ Page %>`指令的`Title`属性，或以编程方式设置。

### <a name="setting-the-pages-title-declaratively"></a>以声明方式设置页的标题

可以设置内容页的标题以声明方式通过`Title`属性[`<%@ Page %>`指令](https://msdn.microsoft.com/library/ydy4x04a.aspx)。 可以通过直接修改设置此属性`<%@ Page %>`指令或通过属性窗口。 让我们看一下这两种方法。

从源视图中，找到`<%@ Page %>`指令，这是在页面的声明性标记的顶部。 `<%@ Page %>`指令`Default.aspx`遵循：


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample3.aspx)]

`<%@ Page %>`指令指定使用的 ASP.NET 引擎时分析和编译页面的特定于页的属性。 这包括其主控页文件、 其代码文件和及其标题，以及其他信息的位置。

默认情况下，当创建新的内容页 Visual Studio 设置`Title`属性设为未命名的页。 更改`Default.aspx`的`Title`属性从"未命名页"到"Master 页教程"，然后查看通过浏览器页面。 图 1 显示浏览器的标题栏中，这反映了新的页面标题。


![浏览器的标题栏现在显示&quot;Master 页教程&quot;而不是&quot;无标题的页面&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image1.png)

**图 01**： 浏览器的标题栏现在显示"Master 页教程"而不是"无标题页"


也可从属性窗口设置页的标题。 从属性窗口中，文档从列表中选择下拉列表负载页级属性，其中包括到`Title`属性。 图 2 显示后的属性窗口`Title`已设置为"Master 页教程"。


![可以将标题从属性窗口中，配置过](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image2.png)

**图 02**： 可能过配置标题从属性窗口


### <a name="setting-the-pages-title-programmatically"></a>以编程方式设置页的标题

主控页`<head runat="server">`标记转换为[`HtmlHead`类](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx)实例由 ASP.NET 引擎呈现页时。 `HtmlHead`类具有[`Title`属性](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx)其值将反映在呈现`<title>`元素。 此属性是可以从 ASP.NET 页的代码隐藏类通过访问`Page.Header.Title`; 这同一个属性还可以访问通过`Page.Title`。

若要练习以编程方式设置页的标题，请导航到`About.aspx`页的代码隐藏类，并创建的事件处理程序已在页面的`Load`事件。 接下来，设置页的标题为"Master 页教程:: 有关::*日期*"，其中*日期*为当前日期。 在添加此代码后你`Page_Load`事件处理程序应类似于以下：


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample4.cs)]

图 3 显示浏览器的标题栏，在访问时`About.aspx`页。


![该页面的标题是以编程方式设置，包括当前日期](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image3.png)

**图 03**: 页面标题是以编程方式设置，包括当前日期


## <a name="step-2-automatically-assigning-a-page-title"></a>步骤 2： 自动分配页标题

正如我们看到在步骤 1 中，可以以声明方式或以编程方式设置的页面标题。 如果你忘记了明确地更改为更具描述性标题，但是，你的页将具有默认标题中，"无标题页"。 理想情况下，该页面的标题会设置自动为我们的事件中我们没有显式指定其值。 例如，如果在运行时的页标题为"无标题页"，我们可能想要自动更新为 ASP.NET 页的文件名相同的标题。 好消息是，使用少量的前期工作，可能能够自动分配的标题。

所有的 ASP.NET web pages 派生自`Page`类`System.Web.UI`命名空间。 `Page`类定义由 ASP.NET 页面所需的最小功能，并揭示基本属性，例如`IsPostBack`， `IsValid`， `Request`，和`Response`，此外还有许多其他。 通常，每一页的 web 应用中需要其他特性或功能。 提供这的常用方法是创建自定义的基本页类。 自定义的基本页类是派生自的类创建`Page`类，包括附加功能。 一旦创建此基类后，你可以从它派生的 ASP.NET 页面 (而不是`Page`类)，从而提供到 ASP.NET 页的扩展的功能。

在此步骤中，我们将创建自动设置到 ASP.NET 页的文件名的页面的标题，如果标题不否则已显式设置基本页。 设置页的标题基于站点图考察步骤 3。

> [!NOTE]
> 创建和使用自定义的基本页类的全面检查不在本系列教程的范围。 详细信息，请阅读[用于你 ASP.NET 页的代码隐藏类的自定义基类](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)。


### <a name="creating-the-base-page-class"></a>创建页基类

我们第一个任务是创建基本页类，它是扩展的类`Page`类。 首先，通过添加`App_Code`到通过右键单击解决方案资源管理器中的项目名称，选择添加 ASP.NET 文件夹，然后选择你的项目的文件夹`App_Code`。 接下来，右键单击`App_Code`文件夹并添加一个名为的新类`BasePage.cs`。 图 4 显示后的解决方案资源管理器`App_Code`文件夹和`BasePage.cs`已添加类。


![添加 App_Code 文件夹和一个名为 BasePage 类](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image4.png)

**图 04**： 添加`App_Code`文件夹和名为的类`BasePage`


> [!NOTE]
> Visual Studio 支持两种项目管理模式： 网站项目和 Web 应用程序项目。 `App_Code`文件夹旨在与网站项目模型一起使用。 如果你正在使用 Web 应用程序项目模型，将放`BasePage.cs`非文件夹中的类`App_Code`，如`Classes`。 有关本主题的详细信息，请参阅[将网站项目迁移到 Web 应用程序项目](http://webproject.scottgu.com/CSharp/Migration2/Migration2.aspx)。


由于自定义的基本页用作 ASP.NET 页的代码隐藏类的基类，它需要扩展`Page`类。


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample5.cs)]

请求 ASP.NET 页时将通过一系列的阶段，最后完成请求的页呈现到 HTML。 我们可以通过重写点击到该阶段`Page`类的`OnEvent`方法。 让我们 base 页自动设置的标题，如果它不显式指定了`LoadComplete`阶段 (后您可能已经猜到，发生的`Load`阶段)。

若要完成此操作，重写`OnLoadComplete`方法，然后输入以下代码：


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample6.cs)]

`OnLoadComplete`方法会启动通过确定如果`Title`属性尚未尚未显式设置。 如果`Title`属性是`null`，空字符串，或具有值"无标题页"，它将分配给请求 ASP.NET 页的文件名。 请求的 ASP.NET 页-的物理路径`C:\MySites\Tutorial03\Login.aspx`，例如-是可通过访问`Request.PhysicalPath`属性。 `Path.GetFileNameWithoutExtension`方法可用于拉出只的文件名部分，并此文件名然后分配给`Page.Title`属性。

> [!NOTE]
> 我恳请你来增强此逻辑，用于改进标题的格式。 例如，如果该页面的文件名是`Company-Products.aspx`，上面的代码将生成的标题"公司-Products"，但理想情况下短划线将替换为一个空格，如下所示"公司 Products"。 此外，考虑大小写更改时添加空间。 也就是说，考虑将代码转换文件名添加`OurBusinessHours.aspx`标题的"我们营业时间"。


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>具有继承的基类的页的内容页

现在，我们需要更新我们派生自定义的基本页的站点中的 ASP.NET 页 (`BasePage`) 而不是`Page`类。 若要完成此，请转到每个代码隐藏类并将更改从类声明：


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample7.cs)]

到:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample8.cs)]

这么做，请访问通过浏览器的站点。 如果您访问其标题显式设置，如页`Default.aspx`或`About.aspx`，使用显式指定的标题。 如果，但是，你可以访问其标题不已从默认值 （"无标题页"） 的页，基本页类将设置到该页面的文件名的标题。

图 5 所示`MultipleContentPlaceHolders.aspx`页上查看通过浏览器时。 请注意标题是精确的页的文件名 （小于扩展），"MultipleContentPlaceHolders"。


[![如果标题是未显式指定，该页面的文件名是自动使用](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image5.png)

**图 05**： 如果标题是未显式指定，该页面的文件名是自动使用 ([单击以查看实际尺寸的图像](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>步骤 3： 将页标题基于站点图

ASP.NET 提供一个可靠的站点映射框架，允许页开发人员可以在 Web 控件用于显示站点地图 （如 SiteMapPath，有关的信息以及外部资源 （例如 XML 文件或数据库表） 中定义分层站点地图菜单上和 TreeView 控件）。

此外可以从 ASP.NET 页的代码隐藏类以编程方式访问站点地图结构。 以这种方式我们自动站点地图中设置的页面标题为其相应的节点内容的标题。 让我们增强`BasePage`，以便它提供此功能在步骤 2 中创建的类。 但首先我们需要创建我们的站点的站点映射。

> [!NOTE]
> 本教程假定读者已熟悉 ASP。NET 的站点映射功能。 使用站点映射的详细信息，请查阅系列我多个部分的文章，[检查 ASP。NET 的站点导航](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)。


### <a name="creating-the-site-map"></a>创建站点图

站点映射系统生成之上[提供程序模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)，这将从序列化站点内存和持久存储区之间的映射信息的逻辑站点地图 API 脱耦。 .NET Framework 附带[`XmlSiteMapProvider`类](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)，这是默认站点地图提供商提供。 顾名思义，`XmlSiteMapProvider`作为其站点映射存储区使用 XML 文件。 让我们可以使用此提供程序定义我们站点映射。

首先在名为的网站的根文件夹中创建站点映射文件`Web.sitemap`。 要实现此目的，右键单击解决方案资源管理器中的网站名称，选择添加新项，然后选择站点图模板。 确保该文件名为`Web.sitemap`并单击添加。


[![添加名为 Web.sitemap 到网站的根文件夹的文件](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image8.png)

**图 06**： 添加文件命名为`Web.sitemap`到网站的根文件夹 ([单击以查看实际尺寸的图像](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image10.png))


添加以下 XML 到`Web.sitemap`文件：


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample9.xml)]

此 XML 定义中图 7 所示分层站点地图结构。


![站点图是当前组合的三个站点映射节点](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image11.png)

**图 07**: 站点图是当前组合的三个站点映射节点


随着我们添加新示例，我们将在将来的教程中更新站点地图结构。

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>更新 Master 页后，可以包括导航 Web 控件

现在，我们已定义站点图，让我们更新母版页，若要包括导航 Web 控件。 具体而言，让我们 ListView 控件添加到左侧列中呈现与在站点地图中定义的每个节点的列表项的未排序的列表的课程部分。

> [!NOTE]
> ListView 控件是为 ASP.NET 3.5 版新增功能。 如果你使用的 ASP.NET 的先前版本，请改为使用转发器控件。 ListView 控件的详细信息，请参阅[使用 ASP.NET 3.5 ListView 和 DataPager 控件](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)。


通过从课程部分删除现有的未经排序的列表标记来启动。 接下来，将 ListView 控件从工具箱拖放下方课程标题。 ListView 位于工具箱中，与其他视图控件的数据部分： GridView、 说明如何和 FormView。 将 ListView 的 ID 属性设置为`LessonsList`。

从数据源配置向导选择要绑定到一个名为的新 SiteMapDataSource 控件的 ListView `LessonsDataSource`。 SiteMapDataSource 控件站点映射系统从返回层次结构。


[![SiteMapDataSource 控件绑定到 LessonsList ListView 控件](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image12.png)

**图 08**: SiteMapDataSource 将控件绑定到`LessonsList`ListView 控件 ([单击以查看实际尺寸的图像](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image14.png))


在创建后 SiteMapDataSource 控件，我们需要定义 ListView 的模板，以便它呈现与由 SiteMapDataSource 控件返回每个节点的列表项的未排序的列表。 这可以实现使用以下模板标记：


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample10.aspx)]

`LayoutTemplate`生成未经排序的列表的标记 (`<ul>...</ul>`) 时`ItemTemplate`呈现作为一个列表项 SiteMapDataSource 所返回每个项 (`<li>`)，其中包含到特定的课程的链接。

在配置的 ListView 的模板之后, 访问的网站。 如图 9 所示，课程部分包含单个的项目符号列表项，主页。 在哪里？ 关于和使用多个 ContentPlaceHolder 控件课程 SiteMapDataSource 旨在返回一组分层的数据，但该 ListView 控件可以仅显示单个级别的层次结构。 因此，将显示仅返回 SiteMapDataSource 站点地图节点的第一个级别。


[![课程部分包含一个列表项](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image15.png)

**图 09**： 课程部分包含单个列表项 ([单击以查看实际尺寸的图像](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image17.png))


若要显示多个级别我们无法嵌套中的多个 Listview `ItemTemplate`。 此方法已在检查[*母版页和网站的导航*教程](../../data-access/introduction/master-pages-and-site-navigation-cs.md)的我[使用数据教程系列](../../data-access/index.md)。 但是，对于本教程系列我们站点图将包含只需两个级别： 主页 （顶级）;和主页是小孩每一课后生成。 而不是编写嵌套的 ListView，我们可以改为指示 SiteMapDataSource 不能返回启动节点，通过设置其[`ShowStartingNode`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx)到`false`。 净效果是 SiteMapDataSource 启动通过返回站点地图节点的第二个层。

进行此更改后，将列表视图显示项目符号供关于并使用多个 ContentPlaceHolder 控件的经验，但为主页省略项目符号项。 若要解决此问题，我们可以显式添加项目符号项为主页中`LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample11.aspx)]

通过配置 SiteMapDataSource 忽略启动节点，并显式添加主页项目符号项，课程部分现在显示预期的输出。


[![课程部分包含的项目符号项主页和每个子节点](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image18.png)

**图 10**： 课程部分主页和每个子节点包含一个项目符号项 ([单击以查看实际尺寸的图像](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>设置基于站点图标题

与就地站点地图中，我们可以更新我们`BasePage`类以使用站点映射中指定的标题。 正如我们做在步骤 2 中，我们仅当想要使用的站点映射节点标题页的标题不已显式设置页面开发人员。 如果正在请求的页面不具有显式设置页标题，然后我们将回退为使用请求的页的文件名 （小于扩展），正如我们做在步骤 2 中站点图中，未找到。 图 11 阐释此决策过程。


![在没有显式设置页标题的情况下，使用相应站点映射的节点的标题](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image21.png)

**图 11**： 在没有显式设置页标题的情况下，使用相应站点映射的节点的标题


更新`BasePage`类的`OnLoadComplete`方法将包含下面的代码：


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample12.cs)]

和前面一样，`OnLoadComplete`方法会启动通过确定是否已显式设置页的标题。 如果`Page.Title`是`null`，空字符串，或已分配的值"无标题页"，然后在代码自动将分配到的值`Page.Title`。

若要确定要使用的标题，代码将启动通过引用[`SiteMap`类](https://msdn.microsoft.com/library/system.web.sitemap.aspx)的[`CurrentNode`属性](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx)。 `CurrentNode`返回[ `SiteMapNode` ](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx)站点地图对应于当前请求的页中的实例。 假设当前请求的页内站点图中，找到`SiteMapNode`的`Title`属性分配给页面的标题。 如果当前请求的页面不在站点图中，`CurrentNode`返回`null`和 （如在步骤 2 中完成），使用请求的页的文件名为标题。

图 12 显示`MultipleContentPlaceHolders.aspx`页上查看通过浏览器时。 因为此页面标题未显式设置，则改为使用其相应的站点映射节点的标题。


![从站点图拉取 MultipleContentPlaceHolders.aspx 页标题](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image22.png)

**图 12**:`MultipleContentPlaceHolders.aspx`页的标题源自站点图


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>步骤 4： 添加到其他特定于页的标记`<head>`部分

步骤 1、 2 和 3 讨论过自定义`<title>`基于按页元素。 除了`<title>`、`<head>`部分可能包含`<meta>`元素和`<link>`元素。 本教程中前面所述`Site.master`的`<head>`部分包括`<link>`元素`Styles.css`。 因为这`<link>`母版页中定义了元素，它包含在`<head>`为所有内容页的部分。 但我们如何有关添加`<meta>`和`<link>`上按页基础元素？

添加到的特定于页的内容的最简单办法`<head>`部分是通过在母版页中创建 ContentPlaceHolder 控件。 我们已经有了此类 ContentPlaceHolder (名为`head`)。 因此，可以添加自定义`<head>`标记，创建相应的内容页中的控件并将标记置于其中。

为了说明添加自定义`<head>`标记到页中，让我们包括`<meta>`description 元素内容页我们当前集。 `<meta>` Description 元素提供了有关 web 页面的简短说明; 大多数搜索引擎将以某种形式的此信息进行了合并，显示搜索结果时。

A `<meta>` description 元素具有以下形式：


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample13.html)]

若要将此标记添加到内容页中，添加到内容控件映射到母版页的 head ContentPlaceHolder 上述文本。 例如，若要定义`<meta>`说明元素`Default.aspx`，添加以下标记：


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample14.aspx)]

因为头 ContentPlaceHolder 不在 HTML 页的正文内，添加到内容控件的标记不会显示在设计视图中。 若要查看`<meta>`说明元素访问`Default.aspx`通过浏览器。 加载页面后，查看源，并且请注意，`<head>`部分包括在内容控件中指定的标记。

花些时间添加`<meta>`说明元素与`About.aspx`， `MultipleContentPlaceHolders.aspx`，和`Login.aspx`。

### <a name="programmatically-adding-markup-to-theheadregion"></a>以编程方式添加到标记`<head>`区域

Head ContentPlaceHolder 使我们能够以声明方式将自定义标记添加到母版页的`<head>`区域。 也可能以编程方式添加自定义标记。 回想一下，`Page`类的`Header`属性返回`HtmlHead`母版页中定义的实例 ( `<head runat="server">`)。

能够以编程方式将内容添加到`<head>`动态要添加的内容时，区域非常有用。 可能根据用户访问的页面;可能是它是正在从数据库中提取。 无论是什么原因，你可以将内容添加到`HtmlHead`通过将控件添加到其控件集合如下所示：


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample15.cs)]

上面的代码将添加`<meta>`关键字元素`<head>`区域，提供以逗号分隔的列表，描述页的关键字。 请注意，若要添加`<meta>`你创建的标记[ `HtmlMeta` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx)实例时，设置其`Name`和`Content`属性，然后将其添加到`Header`的`Controls`集合。 同样，若要以编程方式添加`<link>`元素，创建[ `HtmlLink` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx)对象、 设置其属性，并将其添加到`Header`的`Controls`集合。

> [!NOTE]
> 若要添加任意标记，创建[ `LiteralControl` ](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx)实例时，设置其`Text`属性，然后将其添加到`Header`的`Controls`集合。


## <a name="summary"></a>摘要

在本教程中我们看各种方法来添加`<head>`按页基于区域标记。 母版页应包括`HtmlHead`实例 (`<head runat="server">`) 与 ContentPlaceHolder。 `HtmlHead`实例允许以编程方式访问的内容页`<head>`区域和若要以声明方式和以编程方式将页的标题; ContentPlaceHolder 控件使自定义标记要添加到`<head>`以声明方式通过内容控件的部分。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [动态设置在 ASP.NET 中的页的标题](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [正在检查 ASP。NET 的网站的导航](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [如何使用 HTML 元标记](http://searchenginewatch.com/showPage.html?page=2167931)
- [在 ASP.NET 母版页](http://www.odetocode.com/articles/419.aspx)
- [使用 ASP.NET 3.5 的 ListView 和 DataPager 控件](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [使用适用于 ASP.NET 页的代码隐藏类的自定义的基类](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多个 ASP/ASP.NET 丛书和 4GuysFromRolla.com 创始人，具有已使用 Microsoft Web 技术自 1998 年。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 3.5 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 可以在达到 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Zack Jones 和 Suchi Banerjee。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

>[!div class="step-by-step"]
[上一页](multiple-contentplaceholders-and-default-content-cs.md)
[下一页](urls-in-master-pages-cs.md)
