---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
title: "使用两个 DropDownLists (VB) 进行筛选主/详细信息 |Microsoft 文档"
author: rick-anderson
description: "本教程将扩展的主/从关系添加第三个层，使用两个 DropDownList 控件来选择所需的父和祖父 recor..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 11ae4f64-01ba-4823-95f4-a2fe1f84f7d7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
msc.type: authoredcontent
ms.openlocfilehash: c345fbfe5df4d8ce06695c4dd4b88cc099ad7836
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-with-two-dropdownlists-vb"></a>主/从使用两个 DropDownLists (VB) 进行筛选
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_8_VB.exe)或[下载 PDF](master-detail-filtering-with-two-dropdownlists-vb/_static/datatutorial08vb1.pdf)

> 本教程将展开要添加第三个层，使用两个 DropDownList 控件来选择所需的父和祖父记录的主/从关系。


## <a name="introduction"></a>介绍

在[以前一教程](master-detail-filtering-with-a-dropdownlist-vb.md)，我们探讨了如何显示使用填入类别和显示属于所选类别这些产品 GridView 单个 DropDownList 简单母版/详细报表。 此报表模式适用于显示有一个对多关系，并且可以轻松扩展使其适用于的方案，包括多个一个对多关系的记录。 例如，订单输入系统将具有对应客户、 订单和订单行项的表。 给定的客户可能必须与包含多个项的每个订单的多个订单。 可以通过两个 DropDownLists 和 GridView 向用户显示此类数据。 第一个下拉列表将具有与第二个数据库中每个客户的列表项的内容正在所选客户的订单。 GridView 将列出所选订单中的行项。

Northwind 数据库包含中的规范客户/顺序/订单详细信息信息其`Customers`， `Orders`，和`Order Details`表，这些表不在我们的体系结构中捕获。 但是，我们仍可以说明使用两个依赖 DropDownLists。 第一个下拉列表将列出类别和第二个属于所选类别的产品。 说明如何将列出所选产品的详细信息。

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>步骤 1： 创建并填充类别下拉列表

我们的第一个目标是添加列出的类别的 DropDownList。 这些步骤已在前面的教程中，检查在详细信息，但出于完整性考虑摘要如下。

打开`MasterDetailsDetails.aspx`页面`Filtering`文件夹中，将 DropDownList 添加到页上，设置其`ID`属性`Categories`，然后单击其智能标记中的配置数据源链接。 从数据源配置向导选择要添加新的数据源。


[![为下拉列表中添加新的数据源](master-detail-filtering-with-two-dropdownlists-vb/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image1.png)

**图 1**： 为下拉列表中添加新的数据源 ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image3.png))


新的数据源当然，应对象数据源。 命名此新 ObjectDataSource `CategoriesDataSource` ，并将其调用`CategoriesBLL`对象的`GetCategories()`方法。


[![选择使用 CategoriesBLL 类](master-detail-filtering-with-two-dropdownlists-vb/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image4.png)

**图 2**： 选择使用`CategoriesBLL`类 ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image6.png))


[![配置对象数据源以使用 GetCategories() 方法](master-detail-filtering-with-two-dropdownlists-vb/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image7.png)

**图 3**： 配置使用 ObjectDataSource`GetCategories()`方法 ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image9.png))


在配置对象数据源之后我们仍需要指定哪一数据源字段应显示在`Categories`DropDownList 和哪一个应配置为列表项的值。 设置`CategoryName`字段作为显示和`CategoryID`作为每个列表项的值。


[![具有 DropDownList 显示 CategoryName 字段和使用 CategoryID 值](master-detail-filtering-with-two-dropdownlists-vb/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image10.png)

**图 4**： 具有 DropDownList 显示`CategoryName`字段并使用`CategoryID`作为值 ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image12.png))


此时我们具有一个 DropDownList 控件 (`Categories`) 填充从记录`Categories`表。 当用户从下拉列表中选择新类别我们需要回发发生以便刷新产品我们将在步骤 2 中创建的 DropDownList。 因此，应检查从启用有些选项`categories`DropDownList 的智能标记。


[![为类别 DropDownList 启用有些](master-detail-filtering-with-two-dropdownlists-vb/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image13.png)

**图 5**： 为启用有些`Categories`DropDownList ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>步骤 2： 在第二个 DropDownList 中显示所选的类别的产品

与`Categories`DropDownList 完成后，我们的下一步是显示属于所选类别的产品的说明。 若要完成此操作，请将另一个 DropDownList 添加到名为的页`ProductsByCategory`。 与`Categories`DropDownList，创建的新 ObjectDataSource`ProductsByCategory`名为的 DropDownList `ProductsByCategoryDataSource`。


[![为 ProductsByCategory 下拉列表中添加新的数据源](master-detail-filtering-with-two-dropdownlists-vb/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image16.png)

**图 6**： 添加新的数据源`ProductsByCategory`DropDownList ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image18.png))


[![创建名为 ProductsByCategoryDataSource 新对象数据源](master-detail-filtering-with-two-dropdownlists-vb/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image19.png)

**图 7**： 创建新对象数据源命名`ProductsByCategoryDataSource`([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image21.png))


由于`ProductsByCategory`DropDownList 需要显示属于所选的类别中，仅对这些产品具有调用 ObjectDataSource`GetProductsByCategoryID(categoryID)`方法从`ProductsBLL`对象。


[![选择使用 ProductsBLL 类](master-detail-filtering-with-two-dropdownlists-vb/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image22.png)

**图 8**： 选择使用`ProductsBLL`类 ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image24.png))


[![配置对象数据源以使用 GetProductsByCategoryID(categoryID) 方法](master-detail-filtering-with-two-dropdownlists-vb/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image25.png)

**图 9**： 配置使用 ObjectDataSource`GetProductsByCategoryID(categoryID)`方法 ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image27.png))


我们需要在向导的最后一步中指定的值 *`categoryID`* 参数。 将此参数分配给的选定项`Categories`DropDownList。


[![从类别 DropDownList 拉取 categoryID 参数值](master-detail-filtering-with-two-dropdownlists-vb/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image28.png)

**图 10**： 拉取 *`categoryID`* 参数值从`Categories`DropDownList ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image30.png))


使用配置对象数据源，所有就是指定哪些数据源字段用于显示和值的下拉列表的项。 显示`ProductName`字段并使用`ProductID`字段作为值。


[![指定用于的 DropDownList ListItems 文本和值属性的数据源字段](master-detail-filtering-with-two-dropdownlists-vb/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image31.png)

**图 11**： 指定使用的数据源字段的 DropDownList `ListItem` s`Text`和`Value`属性 ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image33.png))


使用对象数据源和`ProductsByCategory`DropDownList 配置我们的页面将显示两个 DropDownLists： 第一个将在第二个将列出这些产品属于所选类别时列出的所有类别。 当用户从第一个下拉列表中选择新类别时，回发将随之发生和第二个下拉列表将被重新绑定，显示属于新选择的类别这些产品。 图 12 和 13 显示`MasterDetailsDetails.aspx`中通过浏览器查看时的操作。


[![当第一次访问该页面，选中饮料类别](master-detail-filtering-with-two-dropdownlists-vb/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image34.png)

**图 12**： 当第一次访问该页面，选中饮料类别 ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image36.png))


[![选择一个不同的类别显示新类别的产品](master-detail-filtering-with-two-dropdownlists-vb/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image37.png)

**图 13**： 选择不同的类别显示新类别的产品 ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image39.png))


当前`productsByCategory`DropDownList，更改时，未*不*导致回发。 但是，我们需要回发发生添加说明如何以显示所选的产品的详细信息 (步骤 3) 后。 因此，选中从启用有些复选框`productsByCategory`DropDownList 的智能标记。


[![启用 productsByCategory DropDownList 有些功能](master-detail-filtering-with-two-dropdownlists-vb/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image40.png)

**图 14**： 启用的有些功能`productsByCategory`DropDownList ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>步骤 3： 使用说明如何以显示所选产品的详细信息

最后一步是要在说明中显示所选产品的详细信息。 若要实现此目的，将说明如何添加到页中，设置其`ID`属性`ProductDetails`，并为它创建新对象数据源。 配置此对象数据源，将从其数据拉`ProductsBLL`类的`GetProductByProductID(productID)`方法使用的所选的值`ProductsByCategory`的值的 DropDownList  *`productID`* 参数。


[![选择使用 ProductsBLL 类](master-detail-filtering-with-two-dropdownlists-vb/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image43.png)

**图 15**： 选择使用`ProductsBLL`类 ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image45.png))


[![配置对象数据源以使用 GetProductByProductID(productID) 方法](master-detail-filtering-with-two-dropdownlists-vb/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image46.png)

**图 16**： 配置使用 ObjectDataSource`GetProductByProductID(productID)`方法 ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image48.png))


[![从 ProductsByCategory DropDownList 拉取 productID 参数值](master-detail-filtering-with-two-dropdownlists-vb/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image49.png)

**图 17**： 拉取 *`productID`* 参数值从`ProductsByCategory`DropDownList ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image51.png))


您可以选择显示可用字段中的任何`ProductDetails`说明。 我已选择删除`ProductID`， `SupplierID`，和`CategoryID`字段和重新排序，格式其余字段。 此外，我已清理说明的`Height`和`Width`属性，允许说明如何扩展到最佳显示所需的宽度，其数据，而不是让限于指定的大小。 完整的标记如下所示：


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample1.aspx)]

花一些时间来试用`MasterDetailsDetails.aspx`在浏览器中的页。 乍一看起来一切正常工作根据需要，但没有细微问题。 当你选择的新类别`ProductsByCategory`DropDownList 更新以包括所选的类别，这些产品但`ProductDetails`说明如何继续执行，以显示以前的产品信息。 选择一个不同的产品进行所选类别时，将更新说明。 此外，如果足够彻底测试，你将找到，如果你不断地选择新类别 (如选择从饮料`Categories`DropDownList，然后调味品，然后糖果) 每个其他类别选择导致`ProductDetails`要刷新的说明。

若要帮助 concretize 此问题，我们来看具体示例的信息。 当你首次访问页时选择饮料类别以及相关的产品中加载`ProductsByCategory`DropDownList。 牛奶是所选的产品，其详细信息显示在`ProductDetails`说明，如图 18 中所示。


[![选择产品的细节将显示在说明](master-detail-filtering-with-two-dropdownlists-vb/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image52.png)

**图 18**: 选择产品详细信息显示在说明 ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image54.png))


如果将类别选择从饮料更改为调味品，产生的回发和`ProductsByCategory`DropDownList 更新相应地，但说明仍牛奶显示详细信息。


[![仍显示了以前选择产品的详细信息](master-detail-filtering-with-two-dropdownlists-vb/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image55.png)

**图 19**： 仍显示了以前选择产品的详细信息 ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image57.png))


选取列表中的新产品刷新说明如何按预期方式。 如果您更改产品后选择新类别，说明如何再次不会刷新。 但是，如果而不是选择新产品选择新类别，说明如何将刷新。 世界上的现状此处？

问题是该页面的生命周期中的计时问题。 每当将经历大量步骤作为其呈现请求页。 在以下步骤之一 ObjectDataSource 控制检查以查看是否有其`SelectParameters`值已更改。 因此，数据 Web 控件绑定到 ObjectDataSource 知道它需要刷新其显示。 例如，选择新类别时， `ProductsByCategoryDataSource` ObjectDataSource 检测到其参数值已更改和`ProductsByCategory`DropDownList 重新绑定次数本身，获取有关所选类别的产品。

在此情况下，会发生的问题在于： ObjectDataSources 检查已更改的参数的页面生命周期中的点发生*之前*的 Web 控件关联的数据绑定。 因此，选择新类别时`ProductsByCategoryDataSource`ObjectDataSource 检测到更改其参数的值。 使用 ObjectDataSource`ProductDetails`说明，但是，不注意任何此类更改，因为`ProductsByCategory`DropDownList 尚未重新绑定。 更高版本的生命周期中`ProductsByCategory`DropDownList 绑定到其 ObjectDataSource，抓取新选择的类别的产品。 虽然`ProductsByCategory`DropDownList 的值已更改，`ProductDetails`说明的 ObjectDataSource 已执行其参数值检查; 因此，说明如何显示其以前的结果。 图 20 描述了这种交互。


[![ProductsByCategory DropDownList 值更改后 ProductDetails 说明 ObjectDataSource 检查更改](master-detail-filtering-with-two-dropdownlists-vb/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image58.png)

**图 20**: `ProductsByCategory` DropDownList 值更改后`ProductDetails`说明的 ObjectDataSource 检查更改 ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image60.png))


若要纠正此我们需要显式重新绑定`ProductDetails`后的说明`ProductsByCategory`已绑定的 DropDownList。 我们可以完成此操作通过调用`ProductDetails`说明的`DataBind()`方法时`ProductsByCategory`DropDownList 的`DataBound`事件激发。 以下事件处理程序将代码添加到`MasterDetailsDetails.aspx`页的代码隐藏类 (请参阅"[以编程方式设置对象数据源的参数值](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)"有关如何添加事件处理程序的讨论):


[!code-vb[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample2.vb)]

此显式调用后`ProductDetails`说明的`DataBind()`方法也已添加，本教程按预期方式工作。 图 21 突出显示了如何更改此时消失了我们更早版本的问题。


[![ProductDetails 说明如何为显式刷新时 ProductsByCategory DropDownList 的数据绑定事件激发](master-detail-filtering-with-two-dropdownlists-vb/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image61.png)

**图 21**:`ProductDetails`说明时，可以显式刷新`ProductsByCategory`DropDownList 的`DataBound`事件将触发 ([单击以查看实际尺寸的图像](master-detail-filtering-with-two-dropdownlists-vb/_static/image63.png))


## <a name="summary"></a>摘要

DropDownList 充当主/详细信息报表的理想的用户界面元素在 master 和详细信息记录之间存在一个对多关系的情况。 在前面的教程中，我们已了解如何使用单个 DropDownList 来筛选显示的所选类别的产品。 在本教程中我们的产品 GridView 替换 DropDownList，说明用于显示所选产品的详细信息。 在本教程中所述的概念可以轻松地扩展到涉及多个一对多的关系，如客户、 订单和订单项的数据模型。 一般情况下，你始终可以为每一个对多关系中的"one"实体添加 DropDownList。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已希尔顿 Giesenow。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](master-detail-filtering-with-a-dropdownlist-vb.md)
[下一页](master-detail-filtering-across-two-pages-vb.md)
