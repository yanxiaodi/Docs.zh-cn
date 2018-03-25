---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
title: "在 GridView 的页脚 (C#) 中显示的摘要信息 |Microsoft 文档"
author: rick-anderson
description: "摘要信息通常显示在摘要行中的报表的底部。 GridView 控件可以包含为其单元中，我们可以 pr 的页脚行..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d50edc31-9286-4c6a-8635-be09e72752a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: 0d3df976181a4641dbfffe77875989c77ece059d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="displaying-summary-information-in-the-gridviews-footer-c"></a>在 GridView 的页脚 (C#) 中显示的摘要信息
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_15_CS.exe)或[下载 PDF](displaying-summary-information-in-the-gridview-s-footer-cs/_static/datatutorial15cs1.pdf)

> 摘要信息通常显示在摘要行中的报表的底部。 GridView 控件可以包含为其单元格我们可以以编程方式插入聚合数据的页脚行。 在本教程中我们将了解如何在页脚此行中显示聚合数据。


## <a name="introduction"></a>介绍

除了上顺序和重新排序级别中看到每个产品的价格、 库存量，单位，用户可能还会对感兴趣聚合的信息，例如平均价格，库存，量的总数，依此类推。 此类的摘要信息通常显示在摘要行中的报表的底部。 GridView 控件可以包含为其单元格我们可以以编程方式插入聚合数据的页脚行。

此任务将向我们提供三个难题：

1. 配置 GridView 若要显示其页脚行
2. 确定摘要数据;即如何我们计算平均价格或库存量的单元总数？
3. 将摘要数据注入到相应的单元格中的页脚行

在本教程中我们将了解如何克服这些难题。 具体而言，我们将创建的页面与一个 GridView 中显示的所选的类别的产品列出下拉列表中的类别。 GridView 将包括显示了平均价格和总单位数，库存量和在该类别中的产品的订单的页脚行。


[![摘要信息显示在 GridView 的页脚行中](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image1.png)

**图 1**： 摘要信息显示在 GridView 的页脚行中 ([单击以查看实际尺寸的图像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image3.png))


本教程中的，产品主/从接口，其类别均基于在早期版本中介绍的概念[主/详细信息筛选与 DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)教程。 如果你尚未过完成前面的教程，请这样做之前上继续进行此。

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>步骤 1： 添加的类别的 DropDownList 和产品 GridView

在以前有关自行处理将摘要信息添加到 GridView 的页脚，让我们首先只需生成主/详细信息报表。 我们已完成此第一步，我们将查看如何包含摘要数据。

首先打开`SummaryDataInFooter.aspx`页面`CustomFormatting`文件夹。 添加的 DropDownList 控件并设置其`ID`到`Categories`。 接下来，单击从下拉列表的智能标记的选择数据源链接，然后选择要添加名为新 ObjectDataSource `CategoriesDataSource` ，它调用`CategoriesBLL`类的`GetCategories()`方法。


[![添加名为 CategoriesDataSource 新对象数据源](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image4.png)

**图 2**： 添加新对象数据源名为`CategoriesDataSource`([单击以查看实际尺寸的图像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image6.png))


[![已调用 CategoriesBLL 类 GetCategories() 方法 ObjectDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image7.png)

**图 3**： 具有 ObjectDataSource 调用`CategoriesBLL`类的`GetCategories()`方法 ([单击以查看实际尺寸的图像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image9.png))


配置对象数据源之后, 该向导将返回我们应显示的向导我们需要指定哪些数据字段值的下拉列表的数据源配置和哪一个应对应于的下拉列表的值`ListItem` s。 具有`CategoryName`显示的字段并使用`CategoryID`作为值。


[![作为文本和值 ListItems，分别使用类别名称和类别 id 字段](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image10.png)

**图 4**： 使用`CategoryName`和`CategoryID`字段为`Text`和`Value`为`ListItem`s，分别 ([单击以查看实际尺寸的图像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image12.png))


我们现在有 DropDownList (`Categories`)，它列出系统中的类别。 现在，我们需要添加一个 GridView，列出属于所选类别这些产品。 我们执行，但是之前, 需要一段时间来检查 DropDownList 的智能标记中的启用有些复选框。 中所述*主/详细信息筛选与 DropDownList*教程，通过设置的 DropDownList`AutoPostBack`属性`true`将回发每次更改时的 DropDownList 值页。 这将导致 GridView 进行刷新，显示为新选择的类别这些产品。 如果`AutoPostBack`属性设置为`false`（默认值）、 更改类别不会导致回发并因此不会更新列出的产品。


[![检查 DropDownList 的智能标记中的启用有些复选框](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image13.png)

**图 5**： 检查启用有些中的复选框的 DropDownList 智能标记 ([单击以查看实际尺寸的图像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image15.png))


将 GridView 控件添加到页面中，为了显示所选类别的产品。 设置 GridView`ID`到`ProductsInCategory`并将其绑定到名为新 ObjectDataSource `ProductsInCategoryDataSource`。


[![添加名为 ProductsInCategoryDataSource 新对象数据源](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image16.png)

**图 6**： 添加新对象数据源名为`ProductsInCategoryDataSource`([单击以查看实际尺寸的图像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image18.png))


配置对象数据源，以便它将调用`ProductsBLL`类的`GetProductsByCategoryID(categoryID)`方法。


[![已调用 GetProductsByCategoryID(categoryID) 方法 ObjectDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image19.png)

**图 7**： 具有 ObjectDataSource 调用`GetProductsByCategoryID(categoryID)`方法 ([单击以查看实际尺寸的图像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image21.png))


由于`GetProductsByCategoryID(categoryID)`方法采用一个输入参数，在向导的最后一步中，我们可以指定参数值的源。 为了显示从所选类别这些产品，则用参数从拉取`Categories`DropDownList。


[![从选择的类别 DropDownList 获取 categoryID 参数值](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image22.png)

**图 8**： 获取 *`categoryID`* 从所选类别下拉列表的参数值 ([单击以查看实际尺寸的图像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image24.png))


完成向导后 GridView 都将有 BoundField 的每个产品的属性。 让我们来清理这些 BoundFields 以便仅`ProductName`， `UnitPrice`， `UnitsInStock`，和`UnitsOnOrder`BoundFields 会显示。 请尝试将任何字段级别设置添加到剩余 BoundFields (如格式`UnitPrice`作为一种货币)。 进行这些更改后，GridView 的声明性标记应类似于以下：


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample1.aspx)]

此时我们具有完全正常运行的主/详细信息报表的订购件属于所选类别这些产品显示名称、 单价、 库存，量和单位。


[![从选择的类别 DropDownList 获取 categoryID 参数值](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image25.png)

**图 9**： 获取 *`categoryID`* 从所选类别下拉列表的参数值 ([单击以查看实际尺寸的图像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image27.png))


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>步骤 2： 在 GridView 中显示页脚

GridView 控件可以显示页眉和页脚行。 这些行显示的值决定`ShowHeader`和`ShowFooter`属性，分别与`ShowHeader`默认为`true`和`ShowFooter`到`false`。 若要只需包含在 GridView 的页脚设置其`ShowFooter`属性`true`。


[![GridView ShowFooter 属性设置为 true](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image28.png)

**图 10**： 设置 GridView`ShowFooter`属性`true`([单击以查看实际尺寸的图像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image30.png))


页脚行具有 GridView; 中定义的字段的每个单元格但是，这些单元格是默认情况下为空。 需要一段时间来在浏览器中查看我们的进度。 与`ShowFooter`属性现在设置为`true`，GridView 包括空的页脚行。


[![GridView 现在包括页脚行](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image31.png)

**图 11**: GridView 现在包括尾行 ([单击以查看实际尺寸的图像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image33.png))


图 11 中的页脚行不会突出显示，因为它具有白色背景。 让我们创建`FooterStyle`CSS 类`Styles.css`，指定深红色背景，然后配置`GridView.skin`中的外观文件`DataWebControls`主题向 GridView 的分配此 CSS 类`FooterStyle`的`CssClass`属性。 如果你需要提升外观和主题，回头参考[将数据显示与 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)教程。

首先，通过添加以下 CSS 类`Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample2.css)]

`FooterStyle` CSS 类中是类似的样式应用到`HeaderStyle`类，尽管`HeaderStyle`的背景色是很微妙更深的地方，其文本将以粗体显示。 此外，页脚中的文本为右对齐; 而居中标头的文本。

接下来，若要将此 CSS 类与每个 GridView 页脚相关联，请打开`GridView.skin`文件中`DataWebControls`主题和集`FooterStyle`的`CssClass`属性。 在此添加后文件的标记应如下所示：


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample3.aspx)]

在下面所示的屏幕快照，此更改作出页脚清晰地突显出来的详细信息。


[![GridView 的页脚行现在具有红色背景色](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image34.png)

**图 12**: GridView 页脚行现在具有红色背景色 ([单击以查看实际尺寸的图像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image36.png))


## <a name="step-3-computing-the-summary-data"></a>步骤 3： 计算摘要数据

使用显示的 GridView 的页脚，面向我们的下一个挑战是如何计算摘要数据。 有两种方法来计算此聚合的信息：

1. 通过 SQL 查询我们无法向数据库来计算特定类别的摘要数据发出的其他查询。 SQL 包括大量的聚合函数以及`GROUP BY`子句以指定应通过该汇总数据的数据。 以下 SQL 查询将恢复所需的信息：  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample4.sql)]

    当然，您不希望发出直接从此查询`SummaryDataInFooter.aspx`页上，但会通过创建中的方法`ProductsTableAdapter`和`ProductsBLL`。
2. 计算此信息，因为它将被添加到 GridView 中, 所述[自定义格式设置基于时数据](custom-formatting-based-upon-data-cs.md)教程，GridView`RowDataBound`事件处理程序引发一次每个行添加到 GridView 后其已数据绑定。 通过创建此事件的事件处理程序，我们可以将是正在运行的总我们期待的值的聚合。 上次数据行已绑定到 GridView 我们具有总计和所需计算平均值的信息。

我通常采用第二种方法，它将行程保存到数据库和数据访问层和业务逻辑层中实现的摘要的功能所需的工作量，但是这两种方法便已足够。 对于本教程让我们使用第二个选项，并跟踪正在运行的总使用`RowDataBound`事件处理程序。

创建`RowDataBound`事件处理程序，方法是通过在设计器中选择 GridView，单击闪电形图标从属性窗口中，双击 GridView`RowDataBound`事件。 这将创建名为一个新的事件处理程序`ProductsInCategory_RowDataBound`中`SummaryDataInFooter.aspx`页的代码隐藏类。


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample5.cs)]

为了维护运行总和我们需要定义事件处理程序的范围之外的变量。 创建以下四个页面级别变量：

- `_totalUnitPrice`的类型`decimal`
- `_totalNonNullUnitPriceCount`的类型`int`
- `_totalUnitsInStock`的类型`int`
- `_totalUnitsOnOrder`的类型`int`

接下来，编写代码以递增这三个变量，每个数据行中遇到`RowDataBound`事件处理程序。


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample6.cs)]

`RowDataBound`通过确保我们正在处理 DataRow 启动事件处理程序。 一旦已建立的`Northwind.ProductsRow`只绑定到的实例`GridViewRow`对象在`e.Row`变量中存储`product`。 下一步，它运行的总变量就会递增当前产品的对应值 1 (前提是它们不包含数据库`NULL`值)。 我们跟踪的两个运行`UnitPrice`总计和数非`NULL``UnitPrice`记录因为平均价格为这两个数字的商。

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>步骤 4： 在页脚中显示的摘要数据

服务器总计在一起的摘要数据，最后一步是将其显示在 GridView 的页脚行。 此任务中，也可以以编程方式通过`RowDataBound`事件处理程序。 回想一下，`RowDataBound`个事件处理程序都会激发*每个*绑定到 GridView，包括页脚行的行。 因此，我们可以增加我们的事件处理程序，以显示页脚行中使用下面的代码中的数据：


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample7.cs)]

由于页脚行添加到 GridView 后已添加的所有数据行，我们可以确信，按时间我们已准备好运行的总计算将已完成的脚注中显示的摘要数据。 最后一步中，然后，是在表尾的单元格设置这些值。

若要在特定的页脚单元格中显示文本，请使用`e.Row.Cells[index].Text = value`，其中`Cells`索引从 0 处开始。 下面的代码计算平均价格 （除以产品数的总价格），以及总单位数，库存顺序的 GridView 相应页脚单元格上单元中显示内容。


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample8.cs)]

图 13 显示报表后已添加此代码。 请注意如何`ToString("c")`格式为一种货币将平均价格摘要信息。


[![GridView 的页脚行现在具有红色背景色](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image37.png)

**图 13**: GridView 页脚行现在具有红色背景色 ([单击以查看实际尺寸的图像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image39.png))


## <a name="summary"></a>摘要

显示摘要数据是常见的报表要求，并 GridView 控件可以轻松地在其页脚行中包括此类信息。 显示页脚行时 GridView`ShowFooter`属性设置为`true`中以编程方式通过设置其单元格可以包含文本和`RowDataBound`事件处理程序。 计算摘要数据或者可以通过重新查询数据库或通过使用代码在 ASP.NET 页的代码隐藏类以编程方式计算摘要数据。

本教程到结束 GridView、 说明如何和 FormView 控件使用的自定义格式设置我们的检查。 我们的下一步教程中启动时，我们浏览的插入、 更新和删除使用这些相同的控件的数据。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一页](using-the-formview-s-templates-cs.md)
[下一页](custom-formatting-based-upon-data-vb.md)
