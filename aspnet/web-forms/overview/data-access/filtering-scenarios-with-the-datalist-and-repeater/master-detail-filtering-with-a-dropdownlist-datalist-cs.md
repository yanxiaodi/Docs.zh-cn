---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: "使用 DropDownList (C#) 进行筛选主/详细信息 |Microsoft 文档"
author: rick-anderson
description: "在本教程中，我们将了解如何使用 DropDownLists 显示用于显示 'master' 记录和 DataList 单个 web 页中显示主/详细信息报表..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: c2199f0957f4cbe1d35dd971744087da9af1abce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>主/从使用 DropDownList (C#) 进行筛选
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe)或[下载 PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> 在本教程中，我们将了解如何使用 DropDownLists 以显示"主"的记录和显示的"详细信息"DataList 单个 web 页中显示主/详细信息报表。


## <a name="introduction"></a>介绍

主/详细信息报表，我们首先创建在早期版本中使用一个 GridView[主/详细信息筛选与 DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)教程中，首先显示的"主"的记录集。 用户可以然后向下钻取一个主机记录，从而查看该主记录的"详细信息。" 主/详细信息报表是可视化一个对多关系以及如何显示特别"宽"的表 （一种是有大量的列） 中的详细的信息的理想选择。 我们已介绍了如何实现使用在前面的教程中的 GridView 和说明控件的主/详细信息报表。 在本教程和后面的两个，我们将重新检查这些概念，但重点在于使用 DataList 和转发器而是控制。

在本教程中，我们将查看使用说明包含的"主"的记录，DataList 中显示的"详细信息"的记录。

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>步骤 1： 添加主/从教程 Web 页

让我们开始本教程之前，首先我们一段时间来添加的文件夹和我们需要以及本教程后面的两个处理使用 DataList 和转发器控件的主/详细信息报表的 ASP.NET 页。 首先创建一个新文件夹中名为的项目`DataListRepeaterFiltering`。 接下来，将以下五个 ASP.NET 页添加到此文件夹中，让所有的它们配置为使用母版页`Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![创建 DataListRepeaterFiltering 文件夹并添加教程 ASP.NET 页](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**图 1**： 创建`DataListRepeaterFiltering`文件夹并添加教程 ASP.NET 页


接下来，打开`Default.aspx`页上，并将其拖`SectionLevelTutorialListing.ascx`从用户控件`UserControls`到设计图面上的文件夹。 用户控件中，我们在中创建的[母版页和网站的导航](../introduction/master-pages-and-site-navigation-cs.md)教程中，枚举站点图，并会显示项目符号列表中的当前节中的教程。


[![SectionLevelTutorialListing.ascx 用户控件添加到 Default.aspx](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**图 2**： 添加`SectionLevelTutorialListing.ascx`用户控件`Default.aspx`([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))


为了使项目符号列表显示的主/从教程，我们将创建，我们需要将它们添加到站点图。 打开`Web.sitemap`文件，并在"显示数据与 DataList 和转发器"站点映射节点标记后添加以下标记：

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]


![更新包括新的 ASP.NET 页的站点图](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**图 3**： 更新包括新的 ASP.NET 页的站点图


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>步骤 2： 在 DropDownList 中显示的类别

主/从报表将与显示的选定的列表项的产品列出的 DropDownList 中的类别在 DataList 的页中进一步向下。 然后，早 us，第一个任务是具有 DropDownList 中显示的类别。 首先打开`FilterByDropDownList.aspx`页面`DataListRepeaterFiltering`文件夹，然后拖动 DropDownList 从工具箱拖到页面的设计器。 接下来，设置的 DropDownList`ID`属性`Categories`。 单击下拉列表的智能标记中的选择数据源链接并创建名为新 ObjectDataSource `CategoriesDataSource`。


[![添加名为 CategoriesDataSource 新对象数据源](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**图 4**： 添加新对象数据源名为`CategoriesDataSource`([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))


配置新对象数据源，以便它将调用`CategoriesBLL`类的`GetCategories()`方法。 配置对象数据源，我们仍需要来指定哪些数据源字段应显示在下拉列表和后一个应为每个列表项的值关联。 具有`CategoryName`字段作为显示和`CategoryID`作为每个列表项的值。


[![具有 DropDownList 显示 CategoryName 字段和使用 CategoryID 值](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**图 5**： 具有 DropDownList 显示`CategoryName`字段并使用`CategoryID`作为值 ([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))


此时我们具有从记录将填充一个 DropDownList 控件`Categories`表 （所有在大约六个秒钟内完成）。 通过浏览器查看时为止图 6 显示我们的进度。


[![下拉列表将列出当前类别](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**图 6**: 下拉列表将列出当前类别 ([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>步骤 2： 添加产品 DataList

主/详细信息报表的最后一步是列出与所选类别关联的产品。 若要完成此操作，向页面添加 DataList 并创建名为新 ObjectDataSource `ProductsByCategoryDataSource`。 具有`ProductsByCategoryDataSource`控件检索从其数据`ProductsBLL`类的`GetProductsByCategoryID(categoryID)`方法。 由于此主/详细信息报表是只读的则选择 (None) 选项在插入、 更新和删除选项卡中。


[![选择 GetProductsByCategoryID(categoryID) 方法](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**图 7**： 选择`GetProductsByCategoryID(categoryID)`方法 ([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))


单击下一步后, ObjectDataSource 向导将提示我们提供的值的源`GetProductsByCategoryID(categoryID)`方法的 *`categoryID`* 参数。 若要使用的所选值`categories`DropDownList 项设置为控件和到 ControlID 参数源`Categories`。


[![CategoryID 参数值设置为类别下拉列表](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**图 8**： 设置 *`categoryID`* 参数的值`Categories`DropDownList ([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))


Visual Studio 将自动生成完成之后配置数据源向导，`ItemTemplate`的显示名称和值的每个数据字段 DataList。 让我们增强 DataList 若要改用`ItemTemplate`显示只需产品的名称、 类别、 供应商，每个单元，并连同价格数量`SeparatorTemplate`，插入`<hr>`每个项之间的元素。 我要使用`ItemTemplate`部分中的示例从[带有 DataList 和转发器控件中显示数据](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md)教程中，但随意使用查找最引人注目的任何模板标记。

进行这些更改后，你 DataList 和其 ObjectDataSource 标记应类似于以下：

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

需要一段时间来签出我们的浏览器中的进度。 第一次访问该页面，属于所选的类别 （饮料） 这些产品将显示 （如图 9 中所示），但更改 DropDownList 不更新的数据。 这是因为 DataList 更新必须进行回发。 若要完成此我们可以设置的 DropDownList`AutoPostBack`属性`true`或向页面添加按钮 Web 控件。 对于本教程，我已选择要设置的 DropDownList`AutoPostBack`属性`true`。

图 9 和 10 说明了操作中的主/从报表。


[![第一次访问该页面，将显示饮料产品](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**图 9**： 第一次访问该页面，将显示饮料产品 ([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))


[![自动选择新的产品 （生成） 将导致回发时，更新 DataList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**图 10**： 自动选择新的产品 （生成） 将导致回发时，更新 DataList ([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>添加"-选择一个类别-"列表项

在第一次访问时`FilterByDropDownList.aspx`页上的 DropDownList 的第一个列表项 （饮料） 选择默认情况下，在 DataList 显示饮料产品的类别。 在*主/详细信息筛选与 DropDownList*教程，我们将"-选择一个类别-"选项添加到下拉列表，已选择默认情况下，如果选中，显示*所有*的在数据库中的产品。 这种方法时可管理列出一个 GridView 中的产品时为每个产品行占用了少量的屏幕的实际空间。 与 DataList，但是，每个产品的信息将占用更大块的屏幕。 我们仍将添加"-选择一个类别-"选项和让它在默认情况下，选择但而不是让它显示所有产品当选中时，让我们将其配置，使之显示要安装的产品。

若要将新的列表项添加到下拉列表中，转到属性窗口，然后单击中的省略号`Items`属性。 添加新列表项与`Text`"-选择一个类别-"和`Value` `0`。


![添加](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**图 11**： 添加"-选择一个类别-"列表项


或者，你可以通过将以下标记添加到下拉列表添加列表项：

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

此外，我们需要将设置的 DropDownList 控件的`AppendDataBoundItems`到`true`因为如果设置为`false`（默认值），当从 ObjectDataSource 类别绑定的 DropDownList 到时它们将会覆盖任何手动添加的列表项。


![AppendDataBoundItems 属性设置为 True](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**图 12**： 设置`AppendDataBoundItems`属性为 True


我们选择值的原因`0`对于"-选择一个类别-"列表项都是因为值为系统中不有任何类别`0`，选择"-选择一个类别-"列表项目时将因此返回任何产品记录。 若要确认这一点，请一段时间来访问通过浏览器页面。 如图 13 所示，当最初查看页面选定了"-选择一个类别-"列表项，并显示安装的产品。


[![当](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**图 13**： 选择"-选择一个类别-"列表项目时，将显示否产品 ([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))


如果您而是将显示*所有*产品选中"-选择一个类别-"选项后，使用值`-1`相反。 敏锐读取器将能在该重新中回调*主/详细信息筛选与 DropDownList*教程我们更新了`ProductsBLL`类的`GetProductsByCategoryID(categoryID)`方法，以便如果 *`categoryID`* 值`-1`在未返回记录的所有产品中传递。

## <a name="summary"></a>摘要

显示按层次结构相关的数据时，它通常有助于呈现使用主/详细信息报表，用户可以从中启动浏览的层次结构中按自上而下的数据以及向下钻取详细信息数据。 在本教程中，我们探讨生成一个简单的主/详细信息报告，显示所选的类别的产品。 这通过 DropDownList 用于类别列表，属于所选类别的产品 DataList 完成。

在下一教程中我们将查看跨两个页分隔 master 和详细信息记录。 在第一页中，"主"的记录的列表将显示，包含要查看的详细信息的链接。 单击链接将 whisk 用户第二个页上，将显示为所选的主记录的详细信息。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢...

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已徐 Schmidt。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[下一篇](master-detail-filtering-acess-two-pages-datalist-cs.md)
