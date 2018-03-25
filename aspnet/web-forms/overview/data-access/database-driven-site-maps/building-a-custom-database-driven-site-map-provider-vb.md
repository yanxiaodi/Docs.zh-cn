---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
title: "生成自定义数据库驱动站点映射提供程序 (VB) |Microsoft 文档"
author: rick-anderson
description: "在 ASP.NET 2.0 中的默认映射提供静态的 XML 文件中检索其数据。 适用于许多小型和中型大小基于 XML 的提供程序时..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: f904cd2c-a408-4484-9324-8b8d7fe33893
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
msc.type: authoredcontent
ms.openlocfilehash: e9b71ed89dce4e973a9096a9e2326d2c16c30673
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="building-a-custom-database-driven-site-map-provider-vb"></a>生成自定义数据库驱动站点映射提供程序 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_VB.zip)或[下载 PDF](building-a-custom-database-driven-site-map-provider-vb/_static/datatutorial62vb1.pdf)

> 在 ASP.NET 2.0 中的默认映射提供静态的 XML 文件中检索其数据。 适用于许多小型和中型 Web 站点的基于 XML 的提供程序时，更大的 Web 应用程序需要更多动态站点映射。 在本教程中我们要生成的自定义站点映射提供程序从业务逻辑层中检索其数据，这又从数据库检索数据。


## <a name="introduction"></a>介绍

ASP.NET 2.0 的站点地图功能使得页开发人员可以如定义在某些持久介质，web 应用程序的站点地图 XML 文件中。 定义后，可以通过以编程方式访问站点地图数据[`SiteMap`类](https://msdn.microsoft.com/library/system.web.sitemap.aspx)中[`System.Web`命名空间](https://msdn.microsoft.com/library/system.web.aspx)或通过各种导航 Web 控件，如SiteMapPath、 菜单和树视图控件。 站点映射系统使用[提供程序模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)，以便可以创建另一个站点映射序列化实现，并将其插入到 web 应用程序。 ASP.NET 2.0 所附带的默认 site 映射提供程序仍然存在站点地图结构中的 XML 文件。 返回[母版页和网站的导航](../introduction/master-pages-and-site-navigation-vb.md)我们创建名为的文件的教程`Web.sitemap`，包含此结构中，并且具有与每个新的教程部分已将更新其 XML。

默认值基于 XML 的站点地图提供商提供如果非常有效站点地图的结构是相当静态，如的这些教程。 在许多情况下，但是，更多动态站点图需要。 请考虑站点地图中图 1 中，其中每个类别和产品显示为网站的结构中的各节所示。 与此站点图，访问对应于根节点的网页可能还会列出所有类别，而访问特定类别 s web 页面将列出该类别的产品和查看某一特定产品 s web 页将显示该产品 s 详细信息。


[![类别和产品构成站点地图的结构](building-a-custom-database-driven-site-map-provider-vb/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image1.png)

**图 1**: 类别和产品构成站点图的结构 ([单击以查看实际尺寸的图像](building-a-custom-database-driven-site-map-provider-vb/_static/image2.png))


虽然此基于类别和产品的结构可能是硬编码到`Web.sitemap`文件，该文件将需要更新每次类别或产品已添加、 删除或重命名。 因此，站点映射维护将极大地简化如果其结构已从数据库或检索，理想情况下，从应用程序 s 体系结构的业务逻辑层。 这样一来，如产品和类别已添加，请重命名或删除，站点图将自动更新以反映这些更改。

因为 ASP.NET 2.0 的站点映射序列化提供程序模型之上构建，我们可以创建捕捉其数据从数据库或体系结构等一个备用数据存储区，我们自己自定义网站地图提供商提供。 在本教程中我们将生成的自定义提供程序从 BLL 检索其数据。 让我们来开始 ！

> [!NOTE]
> 本教程中创建自定义网站地图提供商提供紧密耦合到应用程序 s 体系结构和数据模型。 Jeff Prosise s [SQL Server 中存储站点地图](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/)和[SQL 站点地图提供商提供以前已等待](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)文章检查在 SQL Server 中存储站点地图数据的通用的方法。


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>步骤 1： 创建自定义网站地图提供程序 Web 页

我们开始创建自定义的地图提供程序之前，让我们来首先添加我们将在本教程需要的 ASP.NET 页。 首先，通过添加一个名为的新文件夹`SiteMapProvider`。 接下来，添加到该文件夹，并确保将每个包含网页相关联的以下 ASP.NET 页面`Site.master`母版页：

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

此外添加`CustomProviders`子文件夹`App_Code`文件夹。


![有关站点映射提供程序相关教程添加的 ASP.NET 页面](building-a-custom-database-driven-site-map-provider-vb/_static/image2.gif)

**图 2**： 添加站点的 ASP.NET 页映射提供程序相关的教程


由于没有本节只有一个教程，我们不需要`Default.aspx`列出节的教程。 相反，`Default.aspx`将 GridView 控件中显示的类别。 我们将在步骤 2 中对此进行处理。

接下来，更新`Web.sitemap`以包含对引用`Default.aspx`页。 具体而言，在 Caching 后添加以下标记`<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample1.xml)]

在更新后`Web.sitemap`，花些时间查看通过浏览器网站的教程。 在左侧菜单现在包括唯一站点映射提供程序本教程的项。


![站点图现在包括站点映射提供程序教程中的一个条目](building-a-custom-database-driven-site-map-provider-vb/_static/image3.gif)

**图 3**： 站点图现在包括站点映射提供程序教程中的一个条目


此教程 s 主要重点是用于说明创建自定义的地图提供程序和配置 web 应用程序以使用该提供程序。 具体而言，我们将生成的提供程序返回包含根节点的节点的每个类别和产品，以及站点图，如图 1 中所示。 一般情况下，站点图中的每个节点可能指定的 URL。 对于我们站点地图中，根节点的 URL 将`~/SiteMapProvider/Default.aspx`，这将列出所有数据库中的类别。 站点图中的每个类别节点将有指向一个 URL `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`，这将列出所有中指定的产品*categoryID*。 最后，每个产品站点映射节点将指向`~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`，这将显示特定产品 s 详细信息。

若要启动我们需要创建`Default.aspx`， `ProductsByCategory.aspx`，和`ProductDetails.aspx`页。 分别在步骤 2、 3 和 4，完成这些页面。 由于本教程的过程启用站点地图提供程序，并且自过去教程已介绍创建报告的这些种类的多页主/详细信息，我们将快一些通过步骤 2 至 4。 如果你需要创建跨多个页的主/从报表中使用刷新程序，回头参考[主/详细信息筛选跨两个页](../masterdetail/master-detail-filtering-across-two-pages-vb.md)教程。

## <a name="step-2-displaying-a-list-of-categories"></a>步骤 2： 显示的类别的列表

打开`Default.aspx`页面`SiteMapProvider`文件夹，然后拖动一个 GridView 从工具箱中拖动到设计器中，设置其`ID`到`Categories`。 从 GridView s 智能标记，请将其绑定到名为新 ObjectDataSource`CategoriesDataSource`并将其配置，以便它检索其数据使用`CategoriesBLL`类的`GetCategories`方法。 由于此 GridView 只需显示的类别，并不提供任何数据修改功能，在更新中，INSERT、 设置的下拉列表，删除选项卡添加到 （无）。


[![配置对象数据源以返回使用 GetCategories 方法的类别](building-a-custom-database-driven-site-map-provider-vb/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image3.png)

**图 4**： 配置到返回类别使用 ObjectDataSource`GetCategories`方法 ([单击以查看实际尺寸的图像](building-a-custom-database-driven-site-map-provider-vb/_static/image4.png))


[![在更新中，INSERT、 设置的下拉列表和删除选项卡到 （无）](building-a-custom-database-driven-site-map-provider-vb/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.png)

**图 5**： 设置的下拉列表中更新、 插入和删除选项卡为 （无） ([单击以查看实际尺寸的图像](building-a-custom-database-driven-site-map-provider-vb/_static/image6.png))


完成配置数据源向导后，Visual Studio 将添加为 BoundField `CategoryID`， `CategoryName`， `Description`， `NumberOfProducts`，和`BrochurePath`。 编辑 GridView，使其仅包含`CategoryName`和`Description`BoundFields 和更新`CategoryName`BoundField 的`HeaderText`到类别的属性。

接下来，添加 HyperLinkField 并因此将它 s 最左侧的字段。 设置`DataNavigateUrlFields`属性`CategoryID`和`DataNavigateUrlFormatString`属性`~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`。 设置`Text`查看产品的属性。


![将 HyperLinkField 添加到类别 GridView](building-a-custom-database-driven-site-map-provider-vb/_static/image6.gif)

**图 6**： 添加到 HyperLinkField `Categories` GridView


在创建对象数据源并自定义 GridView 的字段之后, 的两个控件的声明性标记将如下所示：


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample2.aspx)]

图 7 显示了`Default.aspx`通过浏览器查看时。 单击某个类别的查看产品链接将你带到`ProductsByCategory.aspx?CategoryID=categoryID`，我们将在步骤 3 中制作的。


[![每个类别是列出沿与视图产品链接](building-a-custom-database-driven-site-map-provider-vb/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image7.png)

**图 7**： 每个类别是列出沿与视图产品链接 ([单击以查看实际尺寸的图像](building-a-custom-database-driven-site-map-provider-vb/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>步骤 3： 列出所选的类别的产品

打开`ProductsByCategory.aspx`页上，添加一个 GridView，其命名为`ProductsByCategory`。 从其智能标记，将 GridView 绑定到名为新 ObjectDataSource `ProductsByCategoryDataSource`。 配置对象数据源以使用`ProductsBLL`类的`GetProductsByCategoryID(categoryID)`方法和组下拉列表中的更新、 INSERT 和删除选项卡为 (None) 列出。


[![使用 ProductsBLL 类的 GetProductsByCategoryID(categoryID) 方法](building-a-custom-database-driven-site-map-provider-vb/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image9.png)

**图 8**： 使用`ProductsBLL`类 s`GetProductsByCategoryID(categoryID)`方法 ([单击以查看实际尺寸的图像](building-a-custom-database-driven-site-map-provider-vb/_static/image10.png))


在配置数据源向导的最后一步提示输入的参数源*categoryID*。 由于此信息通过查询字符串字段`CategoryID`、 从下拉列表中选择查询字符串和的 QueryStringField 文本框中输入 CategoryID，如图 9 中所示。 单击完成以完成向导。


[![用于 categoryID 参数 CategoryID Querystring 字段](building-a-custom-database-driven-site-map-provider-vb/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image11.png)

**图 9**： 使用`CategoryID`Querystring 字段*categoryID*参数 ([单击以查看实际尺寸的图像](building-a-custom-database-driven-site-map-provider-vb/_static/image12.png))


完成向导后，Visual Studio 将添加相应 BoundFields 和 CheckBoxField 到针对产品数据字段 GridView。 删除所有但`ProductName`， `UnitPrice`，和`SupplierName`BoundFields。 自定义这些三个 BoundFields`HeaderText`属性，以分别读取产品、 价格和供应商。 格式`UnitPrice`BoundField 作为一种货币。

接下来，添加 HyperLinkField 并将其移到的最左边的位置。 设置其`Text`属性查看详细信息，其`DataNavigateUrlFields`属性`ProductID`，并将其`DataNavigateUrlFormatString`属性`~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`。


![添加指向 ProductDetails.aspx 视图详细信息 HyperLinkField](building-a-custom-database-driven-site-map-provider-vb/_static/image10.gif)

**图 10**： 添加查看详细信息指向 HyperLinkField`ProductDetails.aspx`


进行这些自定义项后, 的 GridView 和 ObjectDataSource s 声明性标记应如下所示：


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample3.aspx)]

返回到查看`Default.aspx`通过浏览器并查看产品上的单击链接饮料。 这将需要你`ProductsByCategory.aspx?CategoryID=1`，Northwind 数据库属于饮料类别中显示名称、 价格和供应商的产品 （请参阅图 11）。 请尝试进一步增强此页后，可以包含一个用于返回用户到类别列表页链接 (`Default.aspx`) 和说明如何或 FormView 控件显示的所选的类别的名称和描述。


[![显示饮料名称、 价格和供应商](building-a-custom-database-driven-site-map-provider-vb/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image13.png)

**图 11**： 显示饮料名称、 价格和供应商 ([单击以查看实际尺寸的图像](building-a-custom-database-driven-site-map-provider-vb/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>步骤 4： 显示产品 s 详细信息

最后一页， `ProductDetails.aspx`，显示所选的产品详细信息。 打开`ProductDetails.aspx`拖说明如何从工具箱中拖动到设计器。 设置说明如何 s`ID`属性`ProductInfo`和清除其`Height`和`Width`属性值。 从其智能标记，将说明如何绑定到名为新 ObjectDataSource `ProductDataSource`，配置对象数据源将从其数据拉`ProductsBLL`类的`GetProductByProductID(productID)`方法。 与前面的 web 页在步骤 2 和 3 中创建，在更新中，INSERT、 设置的下拉列表和删除选项卡添加到 （无）。


[![配置对象数据源以使用 GetProductByProductID(productID) 方法](building-a-custom-database-driven-site-map-provider-vb/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.png)

**图 12**： 配置使用 ObjectDataSource`GetProductByProductID(productID)`方法 ([单击以查看实际尺寸的图像](building-a-custom-database-driven-site-map-provider-vb/_static/image16.png))


配置数据源向导的最后一步提示输入的源*productID*参数。 因为这些数据包括通过查询字符串字段`ProductID`，将下拉列表设置为查询字符串和到 ProductID QueryStringField 文本框。 最后，单击完成按钮以完成向导。


[![配置 productID 参数以从 ProductID Querystring 字段提取其值](building-a-custom-database-driven-site-map-provider-vb/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image17.png)

**图 13**： 配置*productID*参数来请求其值从`ProductID`Querystring 字段 ([单击以查看实际尺寸的图像](building-a-custom-database-driven-site-map-provider-vb/_static/image18.png))


完成配置数据源向导后，Visual Studio 将相应 BoundFields 内创建和 CheckBoxField 产品数据字段的说明。 删除`ProductID`， `SupplierID`，和`CategoryID`BoundFields 和根据你的配置其余字段。 后少量美观配置的我说明和 ObjectDataSource s 声明性标记看上去如下所示：


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample4.aspx)]

若要测试此页，请返回到`Default.aspx`然后单击查看产品的饮料类别。 从饮料产品的列表中，单击上的查看详细信息链接的牛奶 Tea。 这将需要你`ProductDetails.aspx?ProductID=1`，它显示牛奶 Tea s （请参阅图 14） 的详细信息。


[![显示牛奶 Tea s 供应商，类别、 价格和其他信息](building-a-custom-database-driven-site-map-provider-vb/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image19.png)

**图 14**： 显示牛奶 Tea s 供应商，类别、 价格和其他信息 ([单击以查看实际尺寸的图像](building-a-custom-database-driven-site-map-provider-vb/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>步骤 5： 了解站点地图提供商提供的内部工作情况

站点图在 web 服务器的内存中表示为一套`SiteMapNode`形成一个层次结构的实例。 必须有一个根，所有非根节点必须具有恰好一个父节点，且所有节点可能都包含任意数目的子级。 每个`SiteMapNode`对象表示网站的结构中的节; 这些部分通常具有相应的网页。 因此， [ `SiteMapNode`类](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx)具有属性，例如`Title`， `Url`，和`Description`，它提供的信息的节信息`SiteMapNode`表示。 此外，还有`Key`属性，用于唯一标识每个`SiteMapNode`在层次结构，以及用于建立此层次结构的属性`ChildNodes`， `ParentNode`， `NextSibling`， `PreviousSibling`，依次类推。

图 15 显示常规站点地图结构来自图 1 中，但具有更细致地在草绘的实现详细信息。


[![每个 SiteMapNode 具有属性如标题、 Url、 密钥和等等](building-a-custom-database-driven-site-map-provider-vb/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.gif)

**图 15**： 每个`SiteMapNode`都有一些属性例如`Title`， `Url`， `Key`，依次类推 ([单击以查看实际尺寸的图像](building-a-custom-database-driven-site-map-provider-vb/_static/image17.gif))


站点图是可通过访问[`SiteMap`类](https://msdn.microsoft.com/library/system.web.sitemap.aspx)中[`System.Web`命名空间](https://msdn.microsoft.com/library/system.web.aspx)。 此类 s`RootNode`属性返回站点映射的根`SiteMapNode`实例;`CurrentNode`返回`SiteMapNode`其`Url`属性与匹配的当前请求的页面的 URL。 ASP.NET 2.0 的导航 Web 控件内部使用此类。

当`SiteMap`类的属性可以访问，它必须从某些持久介质站点地图结构化为内存。 但是，站点映射序列化逻辑不是硬编码到`SiteMap`类。 相反，在运行时`SiteMap`类可确定哪些站点映射*提供程序*用于序列化。 默认情况下， [ `XmlSiteMapProvider`类](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)使用时，从格式正确的 XML 文件读取站点地图的结构。 但是，使用少量的工作我们可以创建自己的自定义网站地图提供商提供。

所有站点映射提供程序必须都派生自[`SiteMapProvider`类](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx)，其中包括基本的方法和所需的站点的属性将映射提供程序，但省略的许多实现细节。 第二个类的[ `StaticSiteMapProvider` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx)，扩展`SiteMapProvider`类并包含所需的功能的更可靠的实现。 在内部，`StaticSiteMapProvider`存储`SiteMapNode`实例的站点中的映射`Hashtable`并提供等方法`AddNode(child, parent)`，`RemoveNode(siteMapNode),`和`Clear()`添加和删除`SiteMapNode`到内部 s `Hashtable`。 `XmlSiteMapProvider` 派生自 `StaticSiteMapProvider`。

当创建自定义网站地图提供商提供扩展`StaticSiteMapProvider`，有两个抽象方法必须重写： [ `BuildSiteMap` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx)和[ `GetRootNodeCore` ](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx)。 `BuildSiteMap`正如其名，负责从持久性存储区加载站点地图结构和构造在内存中。 `GetRootNodeCore`在站点地图中返回的根节点。

之前 web 应用程序可以使用它必须在应用程序的配置注册站点地图提供商提供。 默认情况下，`XmlSiteMapProvider`类使用名称进行注册`AspNetXmlSiteMapProvider`。 若要注册其他站点地图提供程序，添加以下标记到`Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample5.xml)]

*名称*值为指定用户可读名称时提供程序*类型*指定站点地图提供商提供的完全限定的类型名称。 我们将探讨具体值*名称*和*类型*步骤 7 中的值后，我们制作了我们的自定义网站地图提供商提供。

站点映射提供程序类实例化第一次从其访问`SiteMap`类并在 web 应用程序的生存期内的内存中保留。 由于没有可以从多个并发 web 站点访问者调用站点地图提供商提供的一个实例，它是命令性的提供程序的方法，是*线程安全*。

出于性能和可伸缩性原因，它 s 重要我们缓存内存中站点映射结构，并返回此缓存结构，而不是无需重新创建它每次`BuildSiteMap`调用方法。 `BuildSiteMap`可能被调用多次，每个页请求，每个用户，具体取决于在页和站点地图结构的深度导航控件中使用。 在任何情况下，如果我们不会缓存在站点地图结构`BuildSiteMap`然后每次调用它时我们将需要重新检索的体系结构 （这将产生在查询中对数据库） 中的产品和类别信息。 如前面的缓存教程中所述，缓存的数据可能会失效。 为了应对这种情况，我们可以使用时间-或 SQL 缓存依赖项基于满。

> [!NOTE]
> 站点地图提供商提供 （可选） 可能会重写[`Initialize`方法](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx)。 `Initialize`站点地图提供商提供第一次实例化并传递任何自定义特性分配给中的提供程序时调用`Web.config`中`<add>`类似的元素： `<add name="name" type="type" customAttribute="value" />`。 如果你想要允许页开发人员无需修改提供程序的代码中指定各种站点映射提供程序相关设置，它非常有用。 例如，如果我们已读取的分类和产品数据的体系结构，我们 d 通过相对于数据库中直接可能想要让页开发人员指定的数据库连接字符串通过`Web.config`而不是使用硬编码提供程序的代码中的值。 我们生成了第 6 步中的自定义站点映射提供程序不会覆盖这`Initialize`方法。 有关的使用示例`Initialize`方法，请参阅[Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [SQL Server 中存储站点地图](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/)文章。


## <a name="step-6-creating-the-custom-site-map-provider"></a>步骤 6： 创建自定义站点映射提供程序

若要创建的自定义站点映射提供程序生成的类别和 Northwind 数据库中的产品中的站点映射，我们需要创建扩展的类的`StaticSiteMapProvider`。 在步骤 1 中我要求你添加`CustomProviders`文件夹中的`App_Code`的文件夹将新类添加到名为此文件夹`NorthwindSiteMapProvider`。 将下面的代码添加到 `NorthwindSiteMapProvider` 类中:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample6.vb)]

允许 s 开头浏览此类 s`BuildSiteMap`方法，开头[`lock`语句](https://msdn.microsoft.com/library/c5kehkcz.aspx)。 `lock`语句只允许一个线程一次输入，从而序列化到其代码的访问并阻止两个并发线程在另一个 s toe 上单步执行。

类级别`SiteMapNode`变量`root`用于缓存站点地图结构。 站点图构造为第一次，或后已修改基础数据，第一次时`root`将`Nothing`和将构造站点地图结构。 站点映射的根节点分配给`root`的构造过程过程，以便下一次此方法称为，`root`将不会`Nothing`。 因此，只要`root`不`Nothing`站点地图结构将无需重新创建它返回到调用方。

如果根是`Nothing`，站点地图结构创建中的产品和类别信息。 站点图生成通过创建`SiteMapNode`实例，然后组成通过对的调用层次结构`StaticSiteMapProvider`类的`AddNode`方法。 `AddNode`执行内部簿记，存储各种`SiteMapNode`实例`Hashtable`。 我们开始构造层次结构之前，我们首先调用`Clear`方法，从内部元素将清除`Hashtable`。 接下来，`ProductsBLL`类 s`GetProducts`方法以及产生`ProductsDataTable`存储在本地变量。

通过创建根节点并将其分配给站点映射的构造开始`root`。 重载[ `SiteMapNode` s 构造函数](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx)使用此处并在这整个`BuildSiteMap`传递以下信息：

- 对站点地图提供商提供的引用 (`Me`)。
- `SiteMapNode` S `Key`。 这所必需的值必须是唯一的每个`SiteMapNode`。
- `SiteMapNode` S `Url`。 `Url`是可选的但是，如果提供，每个`SiteMapNode`s`Url`值必须唯一。
- `SiteMapNode` S `Title`，这是所必需。

`AddNode(root)`方法调用添加`SiteMapNode``root`到作为根站点映射。 接下来，每个`ProductRow`中`ProductsDataTable`枚举。 如果已存在`SiteMapNode`对于当前产品的类别中，引用它。 否则为新`SiteMapNode`类别已创建，添加作为子项`SiteMapNode``root`通过`AddNode(categoryNode, root)`方法调用。 在相应类别后`SiteMapNode`找到节点或将其创建，`SiteMapNode`为当前产品创建并添加作为类别的子项`SiteMapNode`通过`AddNode(productNode, categoryNode)`。 请注意，类别`SiteMapNode`s`Url`属性值是`~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`而产品`SiteMapNode`s`Url`属性分配`~/SiteMapNode/ProductDetails.aspx?ProductID=productID`。

> [!NOTE]
> 有一个数据库这些产品`NULL`值，则为其`CategoryID`按类别分组`SiteMapNode`其`Title`属性设置为 None，并且其`Url`属性设置为一个空字符串。 我决定设置`Url`为空字符串自`ProductBLL`类 s`GetProductsByCategory(categoryID)`方法当前缺少的功能，以返回与这些产品`NULL``CategoryID`值。 此外，我要演示导航控件的呈现方式`SiteMapNode`缺少的值其`Url`属性。 我建议你扩展本教程，以便无`SiteMapNode`s`Url`属性指向`ProductsByCategory.aspx`，尚未仅显示与产品`NULL``CategoryID`值。


构造站点图后, 的任意对象添加到数据缓存中上使用 SQL 缓存依赖项`Categories`和`Products`表通过`AggregateCacheDependency`对象。 我们探讨了在前面的教程中，使用 SQL 缓存依赖项*使用 SQL 缓存依赖项*。 自定义网站地图提供商提供，但是，使用的数据缓存的重载`Insert`方法，我们已有待浏览。 此重载接受作为其最后一个输入参数从缓存中删除对象时调用的委托。 具体而言，我们传递在新[`CacheItemRemovedCallback`委托](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx)指向`OnSiteMapChanged`方法定义在靠`NorthwindSiteMapProvider`类。

> [!NOTE]
> 站点图的内存中表示缓存通过类级变量`root`。 由于没有只有一个实例的自定义站点映射提供程序类，并且自该实例在 web 应用程序中，所有线程间共享，此类变量用作缓存。 `BuildSiteMap`方法还使用数据缓存中，但仅作为一种方法，以便在基础数据库中的数据时接收通知`Categories`或`Products`表的更改。 请注意，将放入数据缓存的值只是当前日期和时间。 实际站点地图数据*不*将放入数据缓存。


`BuildSiteMap`方法完成通过返回站点图的根节点。

剩余方法都是非常简单。 `GetRootNodeCore`负责返回的根节点。 由于`BuildSiteMap`返回的根，`GetRootNodeCore`只返回`BuildSiteMap`s 返回值。 `OnSiteMapChanged`方法设置`root`回`Nothing`中删除缓存项的时间。 重新设置为根`Nothing`，则下次`BuildSiteMap`是调用，就将重建站点地图结构。 最后，`CachedDate`属性返回的日期和时间值存储在数据缓存中，如果存在这样一个值。 页开发人员可以使用此属性来确定上次缓存站点地图数据。

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>步骤 7： 注册`NorthwindSiteMapProvider`

为了使 web 应用程序以使用`NorthwindSiteMapProvider`站点地图提供商提供在步骤 6 中创建，我们需要将其在注册`<siteMap>`部分`Web.config`。 具体而言，添加以下标记内的`<system.web>`中的元素`Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample7.xml)]

此标记执行两项操作： 首先，它表明内置`AspNetXmlSiteMapProvider`是默认站点映射提供; 其次，注册的用户友好名称 Northwind 使用在步骤 6 中创建自定义网站地图提供商提供。

> [!NOTE]
> 在站点地图提供程序位于应用程序 s`App_Code`文件夹、 的值`type`属性是只需的类名称。 或者，自定义网站地图提供商提供无法创建在单独的类库项目的编译的程序集放置在 web 应用程序的`/Bin`目录。 在这种情况下，`type`属性值将为*Namespace*。*类名*， *AssemblyName* 。


在更新后`Web.config`，花些时间在浏览器中查看教程任何页。 请注意，在左侧的导航接口仍显示各节中定义的教程`Web.sitemap`。 这是因为我们已将保留`AspNetXmlSiteMapProvider`作为默认提供程序。 若要创建使用的导航用户界面元素`NorthwindSiteMapProvider`，我们将需要显式指定应使用 Northwind 站点地图提供商提供。 我们将了解如何实现此目的在步骤 8。

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>步骤 8： 显示站点使用自定义网站地图提供商提供的映射信息

与自定义站点地图提供商提供创建和注册在`Web.config`，我们已准备好添加到导航控件重新`Default.aspx`， `ProductsByCategory.aspx`，和`ProductDetails.aspx`页`SiteMapProvider`文件夹。 首先打开`Default.aspx`页上，并将其拖`SiteMapPath`从工具箱中拖动到设计器。 说明如何位于工具箱的导航部分。


[![向 Default.aspx 添加 SiteMapPath](building-a-custom-database-driven-site-map-provider-vb/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image18.gif)

**图 16**： 添加到 SiteMapPath `Default.aspx` ([单击以查看实际尺寸的图像](building-a-custom-database-driven-site-map-provider-vb/_static/image20.gif))


说明如何显示痕迹导航，，该值指示当前网页的站点映射内的位置。 我们向主控页的顶部添加 SiteMapPath 进来*母版页和网站的导航*教程。

花些时间查看通过浏览器的此页。 在图 16 中添加 SiteMapPath 使用拉取从其数据的默认站点站点地图提供`Web.sitemap`。 因此，则痕迹导航主页显示&gt;自定义站点图，就像在右上角的痕迹一样。


[![痕迹导航使用默认站点地图提供商提供](building-a-custom-database-driven-site-map-provider-vb/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.gif)

**图 17**： 痕迹导航使用默认站点映射提供程序 ([单击以查看实际尺寸的图像](building-a-custom-database-driven-site-map-provider-vb/_static/image23.gif))


如果希望使用我们在步骤 6 中创建自定义网站地图提供商提供 SiteMapPath 图 16 中添加，请设置其[`SiteMapProvider`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx)到 Northwind，名称我们分配给`NorthwindSiteMapProvider`中`Web.config`。 遗憾的是，设计器仍将使用默认网站的映射提供程序，但如果进行此属性更改后访问通过浏览器页面，你将看到痕迹导航现在使用自定义网站地图提供商提供。


[![痕迹导航现在使用自定义站点映射提供程序 NorthwindSiteMapProvider](building-a-custom-database-driven-site-map-provider-vb/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image24.gif)

**图 18**： 痕迹导航现在使用自定义站点地图提供商提供`NorthwindSiteMapProvider`([单击以查看实际尺寸的图像](building-a-custom-database-driven-site-map-provider-vb/_static/image26.gif))


说明如何显示的功能更强的用户界面中`ProductsByCategory.aspx`和`ProductDetails.aspx`页。 将 SiteMapPath 添加到这些页，设置`SiteMapProvider`中这两项 Northwind 属性。 从`Default.aspx`单击饮料，查看产品链接，然后在牛奶 Tea 查看详细信息链接。 如图 19 所示，则痕迹导航包括当前站点映射节 (牛奶 Tea) 和其上级： Beverages 和所有类别。


[![痕迹导航现在使用自定义站点映射提供程序 NorthwindSiteMapProvider](building-a-custom-database-driven-site-map-provider-vb/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.png)

**图 19**： 痕迹导航现在使用自定义站点地图提供商提供`NorthwindSiteMapProvider`([单击以查看实际尺寸的图像](building-a-custom-database-driven-site-map-provider-vb/_static/image22.png))


除 SiteMapPath，如菜单和树视图控件，可以使用其他导航用户界面元素。 `Default.aspx`， `ProductsByCategory.aspx`，和`ProductDetails.aspx`对于本教程，例如，下载中的页都包括菜单控件 （请参阅图 20）。 请参阅[检查 ASP.NET 2.0 的站点导航功能](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)和[使用站点导航控件](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx)部分[ASP.NET 2.0 快速入门](https://quickstarts.asp.net/QuickStartv20/aspnet/)如更深入地了解导航控件和网站映射 ASP.NET 2.0 中的系统。


[![菜单控件列出每个类别和产品](building-a-custom-database-driven-site-map-provider-vb/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image28.gif)

**图 20**: 菜单控件列出每个类别和产品 ([单击以查看实际尺寸的图像](building-a-custom-database-driven-site-map-provider-vb/_static/image30.gif))


本教程中前面所述，可以通过以编程方式访问站点地图结构`SiteMap`类。 下面的代码返回的根`SiteMapNode`的默认提供程序：


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample8.vb)]

由于`AspNetXmlSiteMapProvider`是默认提供有关我们的应用程序，上面的代码将返回中定义的根节点`Web.sitemap`。 若要引用而不是默认站点地图提供商提供，使用`SiteMap`类 s [ `Providers`属性](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx)如下所示：


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample9.vb)]

其中*名称*是自定义网站地图提供商提供 (Northwind，我们的 web 应用程序) 的名称。

若要访问特定于站点地图提供商提供的成员，请使用`SiteMap.Providers["name"]`检索提供程序实例和然后将其强制转换为适当的类型。 例如，若要显示`NorthwindSiteMapProvider`s`CachedDate`属性在 ASP.NET 页中，使用以下代码：


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample10.vb)]

> [!NOTE]
> 请务必测试 SQL 缓存依赖项功能。 之后访问`Default.aspx`， `ProductsByCategory.aspx`，和`ProductDetails.aspx`页，请转到之一中编辑，插入和删除部分的教程和编辑类别或产品的名称。 然后返回到中的页之一`SiteMapProvider`文件夹。 假设的轮询机制，请注意到基础数据库更改为经过足够的时间，应该对站点图会更新以显示新的产品或类别名称。


## <a name="summary"></a>摘要

ASP.NET 2.0 的站点映射功能包括`SiteMap`类，大量的内置导航 Web 控件，并默认站点映射提供程序所需站点映射信息保存到 XML 文件。 若要从如某个其他源数据库、 应用程序 s 体系结构或我们需要创建自定义网站地图提供商提供的远程 Web 服务从使用站点映射信息。 此步骤包括创建直接或间接派生的类从`SiteMapProvider`类。

在本教程中我们已了解如何创建自定义站点映射提供程序站点图基于剔除从应用程序体系结构的产品和类别信息。 我们提供程序扩展`StaticSiteMapProvider`类和引起创建`BuildSiteMap`检索了数据的方法构造站点映射层次结构，并且缓存一个类级变量中的生成结构。 我们用于 SQL 缓存依赖项的回调函数以使其无效的已缓存结构时基础`Categories`或`Products`修改数据。

尽情享受编程 ！

## <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [在 SQL Server 中存储站点地图](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/)和[SQL 站点地图提供商提供以前已等待](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [ASP.NET 2.0 查看 s 提供程序模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [提供程序工具包](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [检查 ASP.NET 2.0 的站点导航功能](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Dave Gardner、 Zack Jones、 Teresa 墨和伯纳黛特 Leigh。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一篇](building-a-custom-database-driven-site-map-provider-cs.md)
