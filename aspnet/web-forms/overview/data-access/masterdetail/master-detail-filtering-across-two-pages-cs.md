---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: "跨两个页 (C#) 筛选主/详细信息 |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将使用一个 GridView 若要列出数据库中的供应商来实现此模式。 每个供应商中行 GridView 将包含 Vie..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 3411272896dee0da4d5f89aa2bdda0999d660423
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="masterdetail-filtering-across-two-pages-c"></a>大纲/细节筛选跨两个页 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe)或[下载 PDF](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> 在本教程中我们将使用一个 GridView 若要列出数据库中的供应商来实现此模式。 每个供应商中行 GridView 将包含，当单击，会使用户进入单独页时，它为所选供应商列出这些产品的查看产品链接。


## <a name="introduction"></a>介绍

在前面的两个教程中，我们将了解如何[主/详细信息报表显示在单个 web 页面使用 DropDownLists](master-detail-filtering-with-a-dropdownlist-cs.md)到[显示的"主"的记录和 GridView 或说明控件](master-detail-filtering-with-two-dropdownlists-cs.md)以显示"详细信息。" 用于主/详细信息报表的另一种常见模式是具有一个 web 页面，在另一台显示的详细信息上的主记录。 论坛网站、 like [ASP.NET 论坛](https://forums.asp.net/)，是在实践中此模式的出色示例。 ASP.NET 论坛是组成各种论坛开始使用，Web 窗体数据 Presentation 控件，等等。 每个论坛组成多个线程和每个线程组成大量的文章。 在 ASP.NET 论坛主页上，列出了论坛。 在论坛上单击 whisks 你`ShowForum.aspx`页面，其中列出该论坛的线程。 同样，在线程上单击带你访问`ShowPost.aspx`，它会显示的线程的被单击的文章。

在本教程中我们将使用一个 GridView 若要列出数据库中的供应商来实现此模式。 每个供应商中行 GridView 将包含，当单击，会使用户进入单独页时，它为所选供应商列出这些产品的查看产品链接。

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>步骤 1： 添加`SupplierListMaster.aspx`和`ProductsForSupplierDetails.aspx`到页`Filtering`文件夹

在第三个教程中定义页面布局时我们添加了大量中的"起始"页`BasicReporting`， `Filtering`，和`CustomFormatting`文件夹。 但是，我们未添加起始页出于本教程在该时间，因此需要一段时间来添加到两个新页面`Filtering`文件夹：`SupplierListMaster.aspx`和`ProductsForSupplierDetails.aspx`。 `SupplierListMaster.aspx`将列出时的"主"记录 （供应商）`ProductsForSupplierDetails.aspx`将显示有关所选供应商的产品。

当创建这些两个新页面请务必将其与关联`Site.master`母版页。


![将 SupplierListMaster.aspx 和 ProductsForSupplierDetails.aspx 页添加到筛选文件夹](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**图 1**： 添加`SupplierListMaster.aspx`和`ProductsForSupplierDetails.aspx`到页`Filtering`文件夹


此外，当将新页面添加到项目中，务必要更新站点映射文件， `Web.sitemap`，相应地。 对于本教程只需添加`SupplierListMaster.aspx`作为子级的筛选报表中使用以下 XML 内容的网站映射到页`<siteMapNode>`元素：


[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> 你可以帮助自动更新站点映射文件时添加新的 ASP.NET 页使用的过程[k。 Scott Allen](http://odetocode.com/Blogs/scott/)Visual Studio 的免费[站点映射宏](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx)。


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>步骤 2： 显示中的供应商列表`SupplierListMaster.aspx`

与`SupplierListMaster.aspx`和`ProductsForSupplierDetails.aspx`创建的页面，我们的下一步是创建的供应商的 GridView `SupplierListMaster.aspx`。 向页面添加一个 GridView，并将其绑定到新对象数据源。 应使用此 ObjectDataSource`SuppliersBLL`类的`GetSuppliers()`方法以返回所有供应商。


[![选择 SuppliersBLL 类](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**图 2**： 选择`SuppliersBLL`类 ([单击以查看实际尺寸的图像](master-detail-filtering-across-two-pages-cs/_static/image4.png))


[![配置对象数据源以使用 GetSuppliers() 方法](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**图 3**： 配置使用 ObjectDataSource`GetSuppliers()`方法 ([单击以查看实际尺寸的图像](master-detail-filtering-across-two-pages-cs/_static/image7.png))


我们需要包含一个链接，标题为查看产品每 GridView 行中，单击时，将用户带到`ProductsForSupplierDetails.aspx`选定行中传递`SupplierID`通过查询字符串的值。 例如，如果用户单击东京 Traders 供应商查看产品链接 (具有`SupplierID`值为 4)，应将它们发送到`ProductsForSupplierDetails.aspx?SupplierID=4`。

若要完成此操作，将添加[HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx)到 GridView，从而将超链接添加到每个 GridView 行。 通过单击从 GridView 的智能标记的编辑列链接启动。 接下来，从左上方列表中选择 HyperLinkField 并单击添加在 GridView 的字段列表中包括 HyperLinkField。


[![将 HyperLinkField 添加到 GridView](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**图 4**: HyperLinkField 添加 GridView ([单击以查看实际尺寸的图像](master-detail-filtering-across-two-pages-cs/_static/image10.png))


HyperLinkField 可以配置为使用相同的文本或 URL 值在每个 GridView 行中，链接，也可以将这些值基于绑定到每个特定行的数据值。 若要指定静态跨所有行的值使用 HyperLinkField`Text`或`NavigateUrl`属性。 由于我们想要的所有行是相同的链接文本，设置 HyperLinkField`Text`查看产品的属性。


[![将 HyperLinkField 的文本属性设置为视图产品](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**图 5**： 设置 HyperLinkField`Text`查看产品属性 ([单击以查看实际尺寸的图像](master-detail-filtering-across-two-pages-cs/_static/image13.png))


若要设置的文本或 URL 值根据基础数据绑定到 GridView 行，指定数据字段的文本或应从在请求 URL 值`DataTextField`或`DataNavigateUrlFields`属性。 `DataTextField`只能设置为的单个数据字段;`DataNavigateUrlFields`，但是，可以将设置为以逗号分隔列表的数据字段。 我们经常需要基于文本或在当前行的数据字段值和一些静态标记的组合的 URL。 在本教程中，例如，我们希望 HyperLinkField 的链接的 URL 为`ProductsForSupplierDetails.aspx?SupplierID=supplierID`，其中 *`supplierID`* 是每个 GridView 行`SupplierID`值。 请注意，我们需要这两个静态数据驱动此处值：`ProductsForSupplierDetails.aspx?SupplierID=`链接的 URL 部分是静态的而 *`supplierID`* 部分是数据驱动由于其值是每个行自己的`SupplierID`值。

若要指示静态和数据驱动的值的组合，使用`DataTextFormatString`和`DataNavigateUrlFormatString`属性。 根据需要则这些属性中输入的静态标记，然后使用标记`{0}`想中指定的字段的值`DataTextField`或`DataNavigateUrlFields`属性显示。 如果`DataNavigateUrlFields`属性具有多个字段指定的使用`{0}`插入的第一个字段值的位置`{1}`第二个字段值，等等。

将此应用于我们的教程，我们需要将设置`DataNavigateUrlFields`属性`SupplierID`，因为它是数据字段我们需要根据每个行，自定义其值与`DataNavigateUrlFormatString`属性`ProductsForSupplierDetails.aspx?SupplierID={0}`。


[![配置 HyperLinkField 以包括基于供应商 Id 正确链接 URL](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**图 6**： 配置 HyperLinkField 正确链接 URL 基于时包括`SupplierID`([单击以查看实际尺寸的图像](master-detail-filtering-across-two-pages-cs/_static/image16.png))


在添加后 HyperLinkField，随意自定义和 GridView 的字段重新排序。 以下标记显示 GridView 后所做的一些小的字段级自定义。


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

花些时间查看`SupplierListMaster.aspx`通过浏览器的页。 如图 7 所示，页当前列出所有供应商包括查看产品链接。 查看产品上单击链接将你转到`ProductsForSupplierDetails.aspx`，并传递沿供应商的`SupplierID`中查询字符串。


[![每个供应商行包含视图产品链接](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**图 7**： 每个供应商行包含视图产品链接 ([单击以查看实际尺寸的图像](master-detail-filtering-across-two-pages-cs/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>步骤 3： 列出中的供应商的产品`ProductsForSupplierDetails.aspx`

此时`SupplierListMaster.aspx`页发送到的用户`ProductsForSupplierDetails.aspx`，传递所选供应商的`SupplierID`中查询字符串。 本教程的最后一个步骤是在 GridView 中显示的产品`ProductsForSupplierDetails.aspx`其`SupplierID`等于`SupplierID`通过查询字符串。 若要通过添加到 GridView 完成本入门`ProductsForSupplierDetails.aspx`页上，使用一个名为的新 ObjectDataSource 控件`ProductsBySupplierDataSource`，它调用`GetProductsBySupplierID(supplierID)`方法从`ProductsBLL`类。


[![添加名为 ProductsBySupplierDataSource 新对象数据源](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**图 8**： 添加新对象数据源名为`ProductsBySupplierDataSource`([单击以查看实际尺寸的图像](master-detail-filtering-across-two-pages-cs/_static/image22.png))


[![选择 ProductsBLL 类](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**图 9**： 选择`ProductsBLL`类 ([单击以查看实际尺寸的图像](master-detail-filtering-across-two-pages-cs/_static/image25.png))


[![已调用 GetProductsBySupplierID(supplierID) 方法 ObjectDataSource](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**图 10**： 具有 ObjectDataSource 调用`GetProductsBySupplierID(supplierID)`方法 ([单击以查看实际尺寸的图像](master-detail-filtering-across-two-pages-cs/_static/image28.png))


配置数据源向导的最后步骤要求我们提供的源`GetProductsBySupplierID(supplierID)`方法的 *`supplierID`* 参数。 若要使用的查询字符串值，将参数源设置为查询字符串，并输入要使用的 QueryStringField 文本框中的查询字符串值的名称 (`SupplierID`)。


[![填充供应商 Id 从供应商 Id 查询字符串值的参数值](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**图 11**： 填充 *`supplierID`* 参数值从`SupplierID`查询字符串值 ([单击以查看实际尺寸的图像](master-detail-filtering-across-two-pages-cs/_static/image31.png))


这就是所有到它 ！ 图 12 显示`ProductsForSupplierDetails.aspx`页上通过单击东京 Traders 链接进行访问时`SupplierListMaster.aspx`。


[![通过为全产品提供显示](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**图 12**: 产品提供的东京 Traders 显示 ([单击以查看实际尺寸的图像](master-detail-filtering-across-two-pages-cs/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>显示中的供应商信息`ProductsForSupplierDetails.aspx`

如图 12 所示，`ProductsForSupplierDetails.aspx`页只是列出由提供的产品`SupplierID`查询字符串中指定。 有人直接发送到此页上，但是，将不知道图 12 显示了为全产品。 若要纠正此我们可以在此页还显示供应商信息。

首先，通过添加上面产品 GridView FormView。 创建一个名为的新 ObjectDataSource 控件`SuppliersDataSource`，它调用`SuppliersBLL`类的`GetSupplierBySupplierID(supplierID)`方法。


[![选择 SuppliersBLL 类](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**图 13**： 选择`SuppliersBLL`类 ([单击以查看实际尺寸的图像](master-detail-filtering-across-two-pages-cs/_static/image37.png))


[![已调用 GetSupplierBySupplierID(supplierID) 方法 ObjectDataSource](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**图 14**： 具有 ObjectDataSource 调用`GetSupplierBySupplierID(supplierID)`方法 ([单击以查看实际尺寸的图像](master-detail-filtering-across-two-pages-cs/_static/image40.png))


与`ProductsBySupplierDataSource`，具有 *`supplierID`* 参数分配的值为`SupplierID`查询字符串值。


[![填充供应商 Id 从供应商 Id 查询字符串值的参数值](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**图 15**： 填充 *`supplierID`* 参数值从`SupplierID`查询字符串值 ([单击以查看实际尺寸的图像](master-detail-filtering-across-two-pages-cs/_static/image43.png))


在绑定到设计视图中 ObjectDataSource FormView，Visual Studio 会自动创建 FormView `ItemTemplate`， `InsertItemTemplate`，和`EditItemTemplate`与每个返回的数据字段的标签和文本框 Web 控件对象数据源。 由于我们只想要显示供应商信息随意删除`InsertItemTemplate`和`EditItemTemplate`。 接下来，编辑 ItemTemplate，使其显示中的供应商的公司名称`<h3>`元素的地址、 城市、 国家/地区和公司名称下的电话号码。 或者，你可以手动设置 FormView`DataSourceID`并创建`ItemTemplate`标记中，正如我们做返回"[将数据显示与 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)"教程。

后这些编辑 FormView 的声明性标记应看起来类似于以下：


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

图 16 显示的屏幕截图`ProductsForSupplierDetails.aspx`后上面详细说明的供应商信息已包含页上。


[![产品的列表包括有关供应商的摘要](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**图 16**： 产品的列表包括摘要有关供应商 ([单击以查看实际尺寸的图像](master-detail-filtering-across-two-pages-cs/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>有关应用最终接触`ProductsForSupplierDetails.aspx`UI

以改进用户体验有此报表是几个我们应该是以对进行的添加件`ProductsForSupplierDetails.aspx`页。 当前用户可以从的唯一方法`ProductsForSupplierDetails.aspx`页面回发到供应商提供的列表是单击其浏览器后退按钮。 让我们添加超链接控件与`ProductsForSupplierDetails.aspx`页链接回`SupplierListMaster.aspx`，提供要返回到主列表的用户的另一种方法。


[![添加用于将用户返回至 SupplierListMaster.aspx 的超链接控件](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**图 17**： 添加超链接控件以将用户返回到`SupplierListMaster.aspx`([单击以查看实际尺寸的图像](master-detail-filtering-across-two-pages-cs/_static/image49.png))


如果用户单击不具有任何产品的供应商提供的查看产品链接`ProductsBySupplierDataSource`中的 ObjectDataSource`ProductsForSupplierDetails.aspx`不会返回任何结果。 绑定到 ObjectDataSource GridView 不会呈现导致用户的浏览器中的页上的空白区域中的任何标记。 若要更清楚地了解告知用户不是否存在与所选供应商相关的任何产品我们可以设置 GridView`EmptyDataText`到消息时，会发生这种情况，我们需要显示的属性。 我已设置该属性为"没有此供应商提供的产品"

默认情况下，罗斯文数据库中的所有供应商提供至少一个产品。 但是，对于本教程我手动修改`Products`表以便供应商 Escargots Nouveaux 不再与任何产品相关联。 图 18 显示的详细信息页 Escargots Nouveaux 在后进行此更改。


[![用户将收到通知供应商不提供任何产品](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**图 18**： 供应商不提供任何产品通知用户 ([单击以查看实际尺寸的图像](master-detail-filtering-across-two-pages-cs/_static/image52.png))


## <a name="summary"></a>摘要

尽管主/详细信息报表可以显示 master 和详细信息记录在一页，在很多网站中它们被分隔跨两个 web 页。 在本教程中我们介绍了如何通过将列在"主"网页中 GridView 供应商和"详细信息"页中列出的关联的产品实现主/详细信息报表。 每个供应商中行母板网页包含指向传递的行的详细信息页面的`SupplierID`值。 可以使用 GridView HyperLinkField 轻松地添加此类特定行的链接。

在详细信息页中指定的提供程序检索这些产品已进行调用来完成`ProductsBLL`类的`GetProductsBySupplierID(supplierID)`方法。 *`supplierID`* 作为参数源使用查询字符串以声明方式指定参数值。 我们还了解了如何在使用 FormView 的详细信息页中显示的供应商详细信息。

我们[下一教程](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)是主/详细信息报表上的最后一个。 我们将了解如何在每行都有一个选择按钮 GridView 中显示的产品列表。 单击选择按钮将同一页上的说明控件中显示该产品的详细信息。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已希尔顿 Giesenow。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](master-detail-filtering-with-two-dropdownlists-cs.md)
[下一页](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
