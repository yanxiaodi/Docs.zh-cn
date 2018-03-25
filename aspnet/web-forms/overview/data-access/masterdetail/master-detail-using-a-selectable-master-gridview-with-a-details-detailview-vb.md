---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
title: "使用详细信息 DetailView (VB) 可选择的主 GridView 主/详细信息 |Microsoft 文档"
author: rick-anderson
description: "本教程将具有其行中包含的名称和选择按钮以及每个产品的价格 GridView。 单击选择按钮为 particu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 1d1a7c93-971d-4690-9c5e-dac0e5014a09
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
msc.type: authoredcontent
ms.openlocfilehash: eae9c07eff7780aab18346815ca410d687789d17
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-vb"></a>使用详细信息 DetailView (VB) 可选择的主 GridView 主/详细信息
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_10_VB.exe)或[下载 PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/datatutorial10vb1.pdf)

> 本教程将具有其行中包含的名称和选择按钮以及每个产品的价格 GridView。 单击选择按钮特定产品将导致在同一页上的说明控件中显示其完整的详细信息。


## <a name="introduction"></a>介绍

在[以前一教程](master-detail-filtering-across-two-pages-vb.md)我们已了解如何创建主/详细信息报表使用两个网页："主"的 web 页面，从中我们显示供应商提供; 列表，列出所选提供这些产品"详细信息"网页供应商。 此两个页的报表格式可以压缩到一个页面。 本教程将具有其行中包含的名称和选择按钮以及每个产品的价格 GridView。 单击选择按钮特定产品将导致在同一页上的说明控件中显示其完整的详细信息。


[![单击选择按钮显示产品的详细信息](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image1.png)

**图 1**： 单击选择按钮显示产品的详细信息 ([单击以查看实际尺寸的图像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image3.png))


## <a name="step-1-creating-a-selectable-gridview"></a>步骤 1： 创建一个可选择的 GridView

回想一下，在两页主/详细信息报告每个主记录包含超链接，单击时，用户就会发送到传递已单击的行的详细信息页`SupplierID`中查询字符串值。 这样的超链接已添加到使用 HyperLinkField 每 GridView 一行。 为了使单页主/详细信息报表，我们将需要一个按钮对每个 GridView 行，单击时，显示的详细信息。 GridView 控件可以配置为包括有关导致回发，并将该行标记为 GridView 的每个行的选择按钮[SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx)。

首先，通过添加到的 GridView 控件`DetailsBySelecting.aspx`页面`Filtering`文件夹中，设置其`ID`属性`ProductsGrid`。 接下来，添加名为新 ObjectDataSource `AllProductsDataSource` ，它调用`ProductsBLL`类的`GetProducts()`方法。


[![创建名为 AllProductsDataSource ObjectDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image4.png)

**图 2**： 创建 ObjectDataSource 命名`AllProductsDataSource`([单击以查看实际尺寸的图像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image6.png))


[![使用 ProductsBLL 类](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image7.png)

**图 3**： 使用`ProductsBLL`类 ([单击以查看实际尺寸的图像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image9.png))


[![配置对象数据源来调用 GetProducts() 方法](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image10.png)

**图 4**： 配置对 Invoke ObjectDataSource`GetProducts()`方法 ([单击以查看实际尺寸的图像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image12.png))


编辑删除的 GridView 的字段以外的所有`ProductName`和`UnitPrice`BoundFields。 此外，随意根据需要如格式设置自定义这些 BoundFields`UnitPrice`作为一种货币 BoundField 和更改`HeaderText`BoundFields 的属性。 通过单击编辑列链接进行 GridView 的智能标记，或手动配置的声明性语法，可以以图形方式，完成这些步骤。


[![删除多余的产品名称和单价 BoundFields](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image13.png)

**图 5**： 但所有删除`ProductName`和`UnitPrice`BoundFields ([单击以查看实际尺寸的图像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image15.png))


Gridview 最终的标记是：


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample1.aspx)]

接下来，我们需要将标记为可选择，这样会将选择按钮添加到每个行 GridView。 若要完成此操作，只需选中 GridView 的智能标记中的启用选定内容复选框。


[![确保 GridView 的行可选择](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image16.png)

**图 6**： 使 GridView 的行可选 ([单击以查看实际尺寸的图像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image18.png))


选中启用选择选项将添加到 CommandField`ProductsGrid`与 GridView 其`ShowSelectButton`属性设置为 True。 这会导致选择按钮的 GridView，每一行如图 6 所示。 默认情况下，选择按钮呈现为 LinkButtons，但你可以使用按钮或 ImageButtons 改为通过 CommandField`ButtonType`属性。


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample2.aspx)]

单击 GridView 行的选择按钮的回发时，才会和 GridView`SelectedRow`更新属性。 除了`SelectedRow`属性，GridView 提供[SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx)， [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx)，和[SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx)属性。 `SelectedIndex`属性返回所选行的索引，而`SelectedValue`和`SelectedDataKey`属性返回值基于 GridView[将字段名](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx)。

`DataKeyNames`属性用于关联一个或多个数据字段值的每一行，并通常用于属性从基础数据与每个 GridView 行的唯一标识信息。 `SelectedValue`属性返回的第一个值`DataKeyNames`所选行的数据字段作为 where`SelectedDataKey`属性返回的所选的行`DataKey`对象，其中包含所有指定的数据的键字段的值该行。

`DataKeyNames`属性自动设置为唯一标识数据个字段，当将数据源绑定到 GridView，说明如何或通过设计器的说明。 尽管此属性具有已设置为我们自动前面教程中，示例会起作用而无需`DataKeyNames`指定属性。 但是，对于在此教程中，可选择 GridView 以及将来的教程，我们将会检查插入、 更新和删除，`DataKeyNames`属性必须正确设置。 花一些时间，以确保你 GridView`DataKeyNames`属性设置为`ProductID`。

让我们查看一下我们到目前为止通过浏览器的进度。 请注意 GridView 列出的名称和所有产品以及选择 LinkButton 的价格。 单击选择按钮会导致回发。 在步骤 2 中，我们将了解如何通过显示所选产品的详细信息中具有对此回发说明如何响应。


[![每个产品行包含选择 LinkButton](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image19.png)

**图 7**： 每个产品行包含选择 LinkButton ([单击以查看实际尺寸的图像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image21.png))


## <a name="highlighting-the-selected-row"></a>突出显示所选的行

`ProductsGrid` GridView 具有`SelectedRowStyle`属性，可以用于指示所选行的视觉样式。 正确使用，这可以通过多个清楚地显示当前选定的 GridView 哪一行来提高用户的体验。 对于本教程中，让我们必须包含一个黄色背景突出显示所选的行。

与我们前面的教程，让我们努力保持定义为 CSS 类与美观相关的设置。 因此，创建新的 CSS 类中`Styles.css`名为`SelectedRowStyle`。


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample3.css)]

要应用到此 CSS 类`SelectedRowStyle`属性*所有*GridViews 在我们教程系列中，编辑`GridView.skin`外观中`DataWebControls`要包括主题`SelectedRowStyle`设置如下所示：


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample4.aspx)]

添加此元素后，所选的 GridView 行现在已突出显示黄色背景颜色。


[![自定义的所选的行的外观，可以使用 GridView SelectedRowStyle 属性](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image22.png)

**图 8**： 使用进行自定义所选行的外观 GridView`SelectedRowStyle`属性 ([单击以查看实际尺寸的图像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image24.png))


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>步骤 2： 在说明中显示所选的产品的详细信息

与`ProductsGrid`GridView 完成，所有保持是添加显示选择特定产品信息的说明。 添加 GridView 上方的一个说明如何控件并创建名为新 ObjectDataSource `ProductDetailsDataSource`。 由于我们希望此说明如何以显示有关所选产品的特定信息，配置`ProductDetailsDataSource`使用`ProductsBLL`类的`GetProductByProductID(productID)`方法。


[![调用 ProductsBLL 类 GetProductByProductID(productID) 方法](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image25.png)

**图 9**： 调用`ProductsBLL`类的`GetProductByProductID(productID)`方法 ([单击以查看实际尺寸的图像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image27.png))


具有 *`productID`* 参数的值从 GridView 控件获得`SelectedValue`属性。 如前面所述，GridView`SelectedValue`属性返回的第一个数据所选行的键值。 因此，它是命令性的 GridView`DataKeyNames`属性设置为`ProductID`，以便所选的行`ProductID`值由返回`SelectedValue`。


[![将产品 id 参数设置为 GridView SelectedValue 属性](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image28.png)

**图 10**： 设置 *`productID`* 参数 GridView`SelectedValue`属性 ([单击以查看实际尺寸的图像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image30.png))


一次`productDetailsDataSource`ObjectDataSource 有已正确配置并绑定到说明如何，本教程已完成 ！ 当首次访问页时未选择行，因此 GridView 的`SelectedValue`属性返回`Nothing`。 由于没有与产品`NULL``ProductID`值，通过返回任何记录`GetProductByProductID(productID)`方法，这意味着未显示说明 （请参阅图 11）。 如果单击 GridView 行的选择按钮，回发时，才会和刷新的说明。 这次请 GridView`SelectedValue`属性返回`ProductID`所选行的`GetProductByProductID(productID)`方法返回`ProductsDataTable`提供有关该特定的产品，并说明如何显示这些详细信息 （请参阅图 12）。


[![显示第一个访问，仅 GridView 时](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image31.png)

**图 11**： 当首次访问，将显示仅 GridView ([单击以查看实际尺寸的图像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image33.png))


[![在选择某一行，将显示产品的详细信息](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image34.png)

**图 12**： 时选择的行，将显示产品的详细信息 ([单击以查看实际尺寸的图像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image36.png))


## <a name="summary"></a>摘要

在此环境及前面的三个教程，我们已了解大量显示主/详细信息报表的技术。 在本教程中，我们探讨了使用可选择 GridView 容纳的主记录和说明如何在同一页上显示有关所选的主记录的详细信息。 前面的教程中我们介绍了如何显示母版/详细报表使用 DropDownLists 并在上一个网页和详细信息记录在另一台显示主记录。

本教程到结束我们检查主/详细信息报表。 从下一步的教程，我们将首先自定义格式设置的 GridView，说明如何和 FormView 与我们浏览。 我们将看到如何自定义绑定到它们的数据基于这些控件的外观、 如何汇总在 GridView 的页脚中的数据以及如何使用模板以获取更大程度上控制布局。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已希尔顿 Giesenow。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一篇](master-detail-filtering-across-two-pages-vb.md)
