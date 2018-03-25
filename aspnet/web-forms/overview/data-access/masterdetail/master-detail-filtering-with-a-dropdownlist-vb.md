---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
title: "使用 DropDownList (VB) 进行筛选主/详细信息 |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将了解如何在 DropDownList 控件和一个 GridView 中的选定的列表项的详细信息中显示的主记录。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: ea44717e-ab2e-46cd-a692-e4a9c0de194c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 3f66e133b2bc8cdececf55b4d92508e6ff157d95
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>主/从使用 DropDownList (VB) 进行筛选
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_7_VB.exe)或[下载 PDF](master-detail-filtering-with-a-dropdownlist-vb/_static/datatutorial07vb1.pdf)

> 在本教程中我们将了解如何在 DropDownList 控件和一个 GridView 中的选定的列表项的详细信息中显示的主记录。


## <a name="introduction"></a>介绍

常用的报表类型是*主/详细信息报表*中，报表从此处开始通过显示的"主"的记录集。 用户可以然后向下钻取一个主机记录，从而查看该主记录的"详细信息。" 主/详细信息报告是用于可视化一个对多关系，如报表的理想选择显示的所有类别，然后允许用户选择一种特定类别，然后显示其关联的产品。 此外，主/详细信息报表可用于显示特别"宽"表 （一种是有大量的列） 中的详细的信息。 例如，主/详细信息报表的"主"级别可能会在数据库中，显示只包括产品名称和单位价格的产品和向下钻取到特定的产品将显示额外的产品字段 (分类、 供应商、 每个单元，数量和等等）。

有多种方法可以与其实现主/详细信息报表。 通过这和接下来三个教程中，我们将查看在不同的主/详细信息报表。 在本教程中，我们将了解如何显示中的主记录[DropDownList 控件](https://msdn.microsoft.com/library/dtx91y0z.aspx)和 GridView 中的选定的列表项的详细信息。 具体而言，本教程的主/从报表将列出类别和产品信息。

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>步骤 1： 在 DropDownList 中显示的类别

主/从报表将与显示的选定的列表项的产品列出的 DropDownList 中的类别在 GridView 的页中进一步向下。 然后，早 us，第一个任务是具有 DropDownList 中显示的类别。 打开`FilterByDropDownList.aspx`页面`Filtering`文件夹中，将在 DropDownList 上从工具箱中拖动到页面的设计器，并设置其`ID`属性`Categories`。 接下来，单击下拉列表的智能标记中的选择数据源链接。 这将显示数据源配置向导。


[![指定的 DropDownList 数据源](master-detail-filtering-with-a-dropdownlist-vb/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image1.png)

**图 1**： 指定下拉列表的数据源 ([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-vb/_static/image3.png))


选择要添加名为新 ObjectDataSource `CategoriesDataSource` ，它调用`CategoriesBLL`类的`GetCategories()`方法。


[![添加名为 CategoriesDataSource 新对象数据源](master-detail-filtering-with-a-dropdownlist-vb/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image4.png)

**图 2**： 添加新对象数据源名为`CategoriesDataSource`([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-vb/_static/image6.png))


[![选择使用 CategoriesBLL 类](master-detail-filtering-with-a-dropdownlist-vb/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image7.png)

**图 3**： 选择使用`CategoriesBLL`类 ([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-vb/_static/image9.png))


[![配置对象数据源以使用 GetCategories() 方法](master-detail-filtering-with-a-dropdownlist-vb/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image10.png)

**图 4**： 配置使用 ObjectDataSource`GetCategories()`方法 ([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-vb/_static/image12.png))


配置对象数据源，我们仍需要来指定哪些数据源字段应显示在 DropDownList 和后一个应关联为列表项的值。 具有`CategoryName`字段作为显示和`CategoryID`作为每个列表项的值。


[![具有 DropDownList 显示 CategoryName 字段和使用 CategoryID 值](master-detail-filtering-with-a-dropdownlist-vb/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image13.png)

**图 5**： 具有 DropDownList 显示`CategoryName`字段并使用`CategoryID`作为值 ([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-vb/_static/image15.png))


此时我们具有从记录将填充一个 DropDownList 控件`Categories`表 （所有在大约六个秒钟内完成）。 通过浏览器查看时为止图 6 显示我们的进度。


[![下拉列表将列出当前类别](master-detail-filtering-with-a-dropdownlist-vb/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image16.png)

**图 6**: 下拉列表将列出当前类别 ([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-vb/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>步骤 2： 添加产品 GridView

主/详细信息报表中的最后一个步骤是列出与所选类别关联的产品。 若要完成此操作，向页面添加一个 GridView，并创建名为新 ObjectDataSource `productsDataSource`。 具有`productsDataSource`控件精选从其数据`ProductsBLL`类的`GetProductsByCategoryID(categoryID)`方法。


[![选择 GetProductsByCategoryID(categoryID) 方法](master-detail-filtering-with-a-dropdownlist-vb/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image19.png)

**图 7**： 选择`GetProductsByCategoryID(categoryID)`方法 ([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-vb/_static/image21.png))


在选择此方法之后, ObjectDataSource 向导提示我们值的方法的 *`categoryID`* 参数。 若要使用的所选值`categories`DropDownList 项设置为控件和到 ControlID 参数源`Categories`。


[![CategoryID 参数值设置为类别下拉列表](master-detail-filtering-with-a-dropdownlist-vb/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image22.png)

**图 8**： 设置 *`categoryID`* 参数的值`Categories`DropDownList ([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-vb/_static/image24.png))


需要一段时间来签出我们的浏览器中的进度。 当第一次访问该页面，这些产品属于所选类别 （如图 9 中所示），则会显示 （饮料），但更改 DropDownList 不更新的数据。 这是因为 GridView 更新必须进行回发。 为了实现此目的进行 （都不需要编写任何代码） 的两个选项：

- **设置的类别的 DropDownList 的**[有些属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**为 True。** （你可以完成此操作通过检查 DropDownList 的智能标记中的启用有些选项。）这将触发回发，每当 DropDownList 的选定项更改的用户。 因此，当用户从下拉列表中选择新类别回发将随之发生，并且将使用新选择的类别的产品更新 GridView。 （这是我已在本教程中使用的方法）。
- **添加的 DropDownList 旁边的按钮 Web 控件。** 设置其`Text`到刷新的属性或其他类似的。 使用此方法时，用户将需要选择新类别，然后单击按钮。 单击的按钮，将导致回发，并更新 GridView 列出所选类别的这些产品。

图 9 和 10 说明了操作中的主/从报表。


[![第一次访问该页面，将显示饮料产品](master-detail-filtering-with-a-dropdownlist-vb/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image25.png)

**图 9**： 第一次访问该页面，将显示饮料产品 ([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-vb/_static/image27.png))


[![自动选择新的产品 （生成） 将导致回发时，更新 GridView](master-detail-filtering-with-a-dropdownlist-vb/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image28.png)

**图 10**： 自动选择新的产品 （生成） 将导致回发时，更新 GridView ([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-vb/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>添加"-选择一个类别-"列表项

在第一次访问时`FilterByDropDownList.aspx`页上的 DropDownList 的第一个列表项 （饮料） 选择默认情况下，在 GridView 显示饮料产品的类别。 而不是显示第一个类别的产品，我们可能想要改为让 DropDownList 项选择该列表将显示为类似，"-选择一个类别-"。

若要将新的列表项添加到下拉列表中，转到属性窗口，然后单击中的省略号`Items`属性。 添加新列表项与`Text`"-选择一个类别-"和`Value` `-1`。


[![添加 a — 选择列表项的类别-](master-detail-filtering-with-a-dropdownlist-vb/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image31.png)

**图 11**： 添加-选择列表项的类别-([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-vb/_static/image33.png))


或者，你可以通过将以下标记添加到下拉列表添加列表项：


[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample1.aspx)]

此外，我们需要将设置的 DropDownList 控件的`AppendDataBoundItems`设为 True，因为当类别所绑定到的下拉列表从 ObjectDataSource 时它们将覆盖任何手动添加列表项，如果`AppendDataBoundItems`不 True。


![AppendDataBoundItems 属性设置为 True](master-detail-filtering-with-a-dropdownlist-vb/_static/image34.png)

**图 12**： 设置`AppendDataBoundItems`属性为 True


之后这些更改，当第一次访问的页面的"-选择一个类别-"选项，并且不显示任何产品时。


[![在初始页面加载显示要安装的产品](master-detail-filtering-with-a-dropdownlist-vb/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image35.png)

**图 13**： 显示在初始页负载否产品 ([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-vb/_static/image37.png))


因为"-选择一个类别-"列表项被选中时显示任何产品的原因是因为其值是`-1`和与数据库中有任何产品`CategoryID`的`-1`。 如果这是你想要然后完此时的行为 ！ 如果，但是，你想要显示*所有*类别中的选中"-选择一个类别-"列表项时，返回到`ProductsBLL`类和自定义`GetProductsByCategoryID(categoryID)`方法，以便它时，将调用`GetProducts()`方法如果传入中 *`categoryID`* 参数小于零：


[!code-vb[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample2.vb)]

此处使用的方法是通过类似于我们用来显示所有供应商的方法返回[声明性参数](../basic-reporting/declarative-parameters-cs.md)教程，虽然此示例中，我们将使用值为`-1`以指示应为所有记录相对于检索`Nothing`。 这是因为 *`categoryID`* 参数`GetProductsByCategoryID(categoryID)`方法要求为整数值通过在中，而声明性的参数教程中我们已传递的字符串输入参数中。

图 14 显示的屏幕截图`FilterByDropDownList.aspx`如果选择"-选择一个类别-"选项。 在这里，默认情况下，将显示所有产品，用户可以通过选择特定类别，缩小显示。


[![所有产品都现在列出默认情况下](master-detail-filtering-with-a-dropdownlist-vb/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image38.png)

**图 14**： 所有产品都现在列出默认情况下 ([单击以查看实际尺寸的图像](master-detail-filtering-with-a-dropdownlist-vb/_static/image40.png))


## <a name="summary"></a>摘要

显示按层次结构相关的数据时，它通常有助于呈现使用主/详细信息报表，用户可以从中启动浏览的层次结构中按自上而下的数据以及向下钻取详细信息数据。 在本教程中，我们探讨生成一个简单的主/详细信息报告，显示所选的类别的产品。 这是使用完成的 DropDownList 的类别和一个 GridView 属于所选类别的产品的列表。

在[下一教程](master-detail-filtering-with-two-dropdownlists-vb.md)我们将进一步的 DropDownList 接口一个步骤，使用两个 DropDownLists。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一页](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
[下一页](master-detail-filtering-with-two-dropdownlists-vb.md)
