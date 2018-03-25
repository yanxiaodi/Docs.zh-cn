---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
title: "主/详细信息使用详细信息 DataList (C#) 的主机记录的项目符号列表 |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将压缩以前一教程两页主/详细信息的报表到单个页中，t 上显示的类别名称的项目符号列表..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2006
ms.topic: article
ms.assetid: c727bb73-7b59-41a1-8dc3-623c6d69e7c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: cb943941ea4dbfbdc9230df4598ad406d4dee0b6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-c"></a>主/从 Master 记录的项目符号列表中使用的详细信息 DataList (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_CS.exe)或[下载 PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/datatutorial35cs1.pdf)

> 在本教程中我们将压缩以前一教程两页主/详细信息的报表到单个页中，屏幕和屏幕右侧的所选的类别的产品的左侧显示的类别名称的项目符号列表。


## <a name="introduction"></a>介绍

在[前面教程](master-detail-filtering-acess-two-pages-datalist-cs.md)我们介绍了如何在两个页上分隔母版/详细信息报告。 在母版页中我们将使用重复器控件来呈现类别的项目符号列表。 每个类别名称为超链接，单击时，将采用指向详细信息页，其中两个列 DataList 介绍了这些产品用户属于所选类别。

在本教程中我们将压缩两页教程到单个页中，呈现为 LinkButton 每个类别名称显示在屏幕左侧的类别名称的项目符号列表。 单击某个类别名称 LinkButtons 引发回发，并绑定到屏幕的右侧两列 DataList 的所选的类别的产品。 除了显示每个类别的名称，在左侧中继器显示有多少总产品适用于给定类别 （请参见图 1）。


[![在左侧显示的类别名称和产品的总数量](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image1.png)

**图 1**： 左侧显示类别的名称和产品的总数量 ([单击以查看实际尺寸的图像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>步骤 1： 在屏幕的左部分中显示中继器

对于本教程，我们需要将显示所选的类别的产品的左侧的类别的项目符号列表。 可以使用标准的 HTML 元素段落标记，非换行空格定位网页中的内容`<table>`s，依次类推或通过级联样式表 (CSS) 技术。 所有的我们的教程到目前为止已用于 CSS 技术定位。 当我们在我们母版页中构建导航用户界面[母版页和网站的导航](../introduction/master-pages-and-site-navigation-cs.md)教程，我们使用了*绝对定位*，，该值指示导航的精确像素偏移量列表和主要内容。 或者，使用 CSS 来定位到另一个通过左右的一个元素*浮点*。 我们可以具有类别表中所选的类别的产品的左侧出现由浮点 DataList 左侧转发器的项目符号列表

打开`CategoriesAndProducts.aspx`来自页`DataListRepeaterFiltering`文件夹和中继器和 DataList 向页面添加。 设置中继器 s`ID`到`Categories`和 DataList s 到`CategoryProducts`。 转到源视图并将其自身的转发器和 DataList 控件放`<div>`元素。 也就是说，将括在转发器`<div>`元素第一个，然后在自己 DataList`<div>`中继器之后紧邻的元素。 你标记此时应类似于以下：


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample1.aspx)]

若要 float DataList 左侧转发器，我们需要使用`float`CSS 样式属性，如下所示：


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample2.html)]

`float: left;`浮动第一个`<div>`左侧的第二个元素。 `width`和`padding-right`设置指示第一个`<div>`s`width`和之间添加填充量`<div>`元素的内容和其右边距。 有关详细信息中 CSS 的浮点元素签出[Floatutorial](http://css.maxdesign.com.au/floatutorial/)。

而不是指定的样式设置直接通过第一个`<p>`元素 s`style`属性，但让改为创建新的 CSS 类中的 s`Styles.css`名为`FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample3.css)]

然后我们可以将`<div>`与`<div class="FloatLeft">`。

在添加 CSS 类和配置中的标记后`CategoriesAndProducts.aspx`页上，转到设计器。 你应看到浮点 DataList 左侧 （尽管右现在这两项仅出现如以来我们遇到有待配置其数据源或模板灰框） 转发器。


[![DataList 左侧浮动转发器](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image4.png)

**图 2**: 中继器浮动 DataList 左侧 ([单击以查看实际尺寸的图像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>步骤 2： 确定每个类别的产品数

使用转发器和 DataList s 周围标记完成，我们已准备好将类别数据绑定到中继器重新控制。 但是，如图 1 中的类别的项目符号列表所示，除了每个类别的名称我们还需要显示的与类别关联的产品数目。 若要访问此信息，我们可以：

- **确定此信息从 ASP.NET 页的代码隐藏类。** 给定特定 *`categoryID`* 我们可以通过调用确定关联的产品数`ProductsBLL`类的`GetProductsByCategoryID(categoryID)`方法。 此方法返回`ProductsDataTable`对象，其`Count`属性指示多少`ProductsRow`s 存在，即指定的产品数 *`categoryID`* 。 我们可以创建`ItemDataBound`事件处理程序，对于绑定到中继器，每个类别调用中继器`ProductsBLL`类的`GetProductsByCategoryID(categoryID)`方法并在输出中包含其计数。
- **更新`CategoriesDataTable`要包括的类型化数据集中`NumberOfProducts`列。** 然后，我们可以更新`GetCategories()`中的方法`CategoriesDataTable`若要包括此信息或，或者，将保留`GetCategories()`作为-并创建一个新`CategoriesDataTable`调用方法`GetCategoriesAndNumberOfProducts()`。

让我们来浏览这两种技术。 第一种方法会更易于实现的因为我们不需要更新数据访问层;但是，它需要与数据库的多个通信。 调用`ProductsBLL`类 s`GetProductsByCategoryID(categoryID)`中的方法`ItemDataBound`事件处理程序添加的额外数据库调用的转发器中显示每个类别。 凭借此技术有*N* + 1 个数据库调用其中*N*是显示在转发器的类别的数目。 使用第二个方法中，产品计数返回有关从每个类别的信息`CategoriesBLL`类 s `GetCategories()` (或`GetCategoriesAndNumberOfProducts()`) 方法，从而导致检索只需一次到数据库。

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>确定 ItemDataBound 事件处理程序中的产品数

确定转发器 s 中每种类别的产品数`ItemDataBound`事件处理程序不需要对我们现有的数据访问层进行任何修改。 可直接在内进行所有修改`CategoriesAndProducts.aspx`页。 首先，通过添加名为新 ObjectDataSource`CategoriesDataSource`通过中继器 s 智能标记。 接下来，配置`CategoriesDataSource`ObjectDataSource 以便它检索从其数据`CategoriesBLL`类的`GetCategories()`方法。


[![配置对象数据源以使用 CategoriesBLL 类的 GetCategories() 方法](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image7.png)

**图 3**： 配置使用 ObjectDataSource`CategoriesBLL`类 s`GetCategories()`方法 ([单击以查看实际尺寸的图像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image9.png))


在每个项`Categories`中继器需要能可单击，，单击时，会导致`CategoryProducts`DataList 以显示所选类别这些产品。 这可以通过使每个类别的超链接，链接回此相同的页 (`CategoriesAndProducts.aspx`)，但传入`CategoryID`通过查询字符串，我们在前面的教程中看到一样。 此方法的优点是可以标有书签并由搜索引擎编制索引页，其中显示特定类别的产品。

或者，我们可以让每个类别 LinkButton，这是我们将使用本教程中的方法。 LinkButton 呈现为超链接用户 s 浏览器中，但单击时，引发的回发;在回发时，DataList 的 ObjectDataSource 需要刷新以显示属于所选类别这些产品。 对于本教程，使用超链接更有意义比使用 LinkButton;但是，可能有其他场景中，使用 LinkButton 是更有利。 超链接的方法是将适合于此示例的而让改为尝试使用 LinkButton 的 s。 如我们所见，使用 LinkButton 引入了一些难题，否则不会出现与超链接。 因此，在本教程使用 LinkButton 将突出显示这些难题并帮助提供有关我们可能想要使用而不是超链接 LinkButton 这些方案的解决方案。

> [!NOTE]
> 建议重复本教程中使用超链接控件或`<a>`LinkButton 替代的元素。


以下标记显示中继器和 ObjectDataSource 声明性语法。 请注意中继器的模板呈现 LinkButton 作为每个项的项目符号列表：


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample4.aspx)]

> [!NOTE]
> 对于本教程的转发器必须具有启用其视图状态 (请注意省略`EnableViewState="False"`从转发器 s 声明性语法)。 在步骤 3 中我们将创建一个事件处理程序为转发器 s`ItemCommand`事件在其中我们将更新 DataList 的 ObjectDataSource 的`SelectParameters`集合。 转发器的`ItemCommand`，但是，如果禁用了视图状态获胜 t 激发。 请参阅[ASP.NET 问题的问题贴](http://scottonwriting.net/sowblog/posts/1263.aspx)和[其解决方案](http://scottonwriting.net/sowBlog/posts/1268.aspx)原因的详细信息。 必须为中继器 s 启用视图状态`ItemCommand`事件激发。


与 LinkButton`ID`属性值`ViewCategory`不具有其`Text`属性集。 如果我们必须只是想要显示的类别名称，我们将具有文本属性以声明方式，通过设置数据绑定语法，如下所示：


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample5.aspx)]

但是，我们希望能够显示这两个类别的名称*和*属于该类别的产品数。 可以从转发器 s 检索此信息`ItemDataBound`通过到调用的事件处理程序`ProductBLL`类 s`GetCategoriesByProductID(categoryID)`方法，并确定多少条记录返回在产生`ProductsDataTable`，如以下代码演示：


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample6.cs)]

首先，我们确保我们重新使用数据项 (一个其`ItemType`是`Item`或`AlternatingItem`)，然后引用`CategoriesRow`刚刚已绑定到当前的实例`RepeaterItem`。 接下来，我们通过创建的实例确定此类别的产品数`ProductsBLL`类，调用其`GetCategoriesByProductID(categoryID)`方法，并确定使用返回的记录数`Count`属性。 最后，`ViewCategory`中 ItemTemplate LinkButton 是引用并将其`Text`属性设置为*CategoryName* (*NumberOfProductsInCategory*)，其中*NumberOfProductsInCategory*设置为带有零个小数位数的数字的格式。

> [!NOTE]
> 或者，我们无法添加了*格式设置函数*到 ASP.NET 页的代码隐藏类接受类别 s 的`CategoryName`和`CategoryID`值，并返回`CategoryName`加上的数字类别中的产品 (通过调用确定`GetCategoriesByProductID(categoryID)`方法)。 此类格式设置的函数的结果无法以声明方式分配给 LinkButton 的文本属性将需`ItemDataBound`事件处理程序。 请参阅[GridView 控件中使用 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)或[格式设置的 DataList 和数据后转发器基于](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md)使用格式设置的函数的详细信息的教程。


添加此事件处理程序后, 需要一段时间来测试通过浏览器页面。 请注意如何在项目符号列表中，显示类别的名称和与类别关联的产品数列出每个类别 （请参见图 4）。


[![显示每个类别名称和产品数](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image10.png)

**图 4**： 显示每个类别的名称和数量的产品 ([单击以查看实际尺寸的图像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>更新`CategoriesDataTable`和`CategoriesTableAdapter`以针对每个类别包括的产品数目

而不是确定它与每个类别的产品数目 s 绑定到转发器，我们可以通过调整简化此过程`CategoriesDataTable`和`CategoriesTableAdapter`数据访问层，将此信息包括本机中。 若要实现此目的，我们必须添加到新列`CategoriesDataTable`以容纳的相关联的产品数。 若要将新列添加到数据表中，打开类型化数据集 (`App_Code\DAL\Northwind.xsd`)，右键单击 DataTable 若要修改，然后选择添加 / 列。 添加到一个新列`CategoriesDataTable`（请参见图 5）。


[![将新列添加到 CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image13.png)

**图 5**： 添加到一个新列`CategoriesDataSource`([单击以查看实际尺寸的图像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image15.png))


这将添加一个名为的新列`Column1`，这可以更改通过只需键入其他名称。 重命名此新列`NumberOfProducts`。 接下来，我们需要配置此列的属性。 单击新列，转到属性窗口。 更改列 s`DataType`属性从`System.String`到`System.Int32`并设置`ReadOnly`属性`True`，如图 6 中所示。


![设置数据类型和新的列的 ReadOnly 属性](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image16.png)

**图 6**： 设置`DataType`和`ReadOnly`新列的属性


虽然`CategoriesDataTable`现在具有`NumberOfProducts`列中，按任何相应的 TableAdapter 的查询未设置其值。 我们可以更新`GetCategories()`方法来返回此信息，如果我们想要此类信息返回每次检索类别信息。 如果，但是，我们只需要获取关联产品的极少数情况下 （例如，仅对于本教程中） 中的类别数，则我们可以保留`GetCategories()`作为-并创建新的方法，返回此信息。 允许 s 使用此后一种方法，创建一个名为的新方法`GetCategoriesAndNumberOfProducts()`。

若要添加此新`GetCategoriesAndNumberOfProducts()`方法中，右键单击`CategoriesTableAdapter`，然后选择新查询。 这将显示的向上 TableAdapter 查询配置向导中，哪些我们已在前面的教程中多次使用。 对于此方法，通过指示，此查询使用返回行的临时 SQL 语句中启动向导。


[![创建使用临时 SQL 语句的方法](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image17.png)

**图 7**： 创建方法使用临时 SQL 语句 ([单击以查看实际尺寸的图像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image19.png))


[![SQL 语句返回的行](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image20.png)

**图 8**: SQL 语句返回行 ([单击以查看实际尺寸的图像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image22.png))


下一步的向导屏幕提示 us 代表要使用的查询。 若要返回每个类别 s `CategoryID`， `CategoryName`，和`Description`字段，以及与该类别关联的产品数目使用以下`SELECT`语句：


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample7.sql)]


[![指定使用的查询](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image23.png)

**图 9**： 指定使用的查询 ([单击以查看实际尺寸的图像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image25.png))


请注意，子查询计算与类别关联的产品数斐波那是别名为`NumberOfProducts`。 此命名的匹配项将导致返回此子查询，以与关联的值`CategoriesDataTable`s`NumberOfProducts`列。

在输入此查询后，最后一步是选择新的方法的名称。 使用`FillWithNumberOfProducts`和`GetCategoriesAndNumberOfProducts`填充 DataTable 并返回数据表模式，分别。


[![新的 TableAdapter 的方法 FillWithNumberOfProducts 名称和 GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image26.png)

**图 10**： 命名新的 TableAdapter s 方法`FillWithNumberOfProducts`和`GetCategoriesAndNumberOfProducts`([单击以查看实际尺寸的图像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image28.png))


此时数据访问层已扩展为包含的每个类别的产品数目。 由于所有我们表示层将路由到通过单独的业务逻辑层 DAL 的所有调用我们需要添加相应`GetCategoriesAndNumberOfProducts`方法`CategoriesBLL`类：


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample8.cs)]

DAL 和 BLL 完成后，我们重新准备好将绑定到此数据`Categories`中的转发器`CategoriesAndProducts.aspx`！ 如果以前已为从确定转发器中创建对象数据源中的产品数`ItemDataBound`事件处理程序部分中，删除此对象数据源并删除转发器的`DataSourceID`属性设置; 还无线免缠绕转发器 s`ItemDataBound`通过删除的事件处理的事件`Handles Categories.OnItemDataBound`ASP.NET 代码隐藏类中的语法。

返回在其原始状态中继器，添加名为新 ObjectDataSource`CategoriesDataSource`通过中继器 s 智能标记。 配置对象数据源以使用`CategoriesBLL`类，但而不是让它使用`GetCategories()`方法，具有它使用`GetCategoriesAndNumberOfProducts()`改为 （请参阅图 11）。


[![配置对象数据源以使用 GetCategoriesAndNumberOfProducts 方法](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image29.png)

**图 11**： 配置使用 ObjectDataSource`GetCategoriesAndNumberOfProducts`方法 ([单击以查看实际尺寸的图像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image31.png))


接下来，更新`ItemTemplate`以便 LinkButton s`Text`属性以声明方式分配使用数据绑定的语法，并且同时包含`CategoryName`和`NumberOfProducts`数据字段。 转发器的完整声明性标记和`CategoriesDataSource`ObjectDataSource 按照：


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample9.aspx)]

通过更新以包括 DAL 呈现的输出`NumberOfProducts`列是不同于使用`ItemDataBound`事件处理程序方法 (回头参考图 4，以查看屏幕截图显示的类别名称和产品数转发器)。

## <a name="step-3-displaying-the-selected-category-s-products"></a>步骤 3： 显示所选的类别的产品

现在我们有`Categories`以及数量的产品类别列表中显示每个类别中的转发器。 转发器的每个类别，单击时，会回发时，在该点我们使用 LinkButton 需要显示在所选类别这些产品`CategoryProducts`DataList。

我们面临的挑战之一是： 如何让 DataList 显示只需为所选类别这些产品。 在[母版/详细介绍与详细信息说明如何使用可选择的主 GridView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)可以选择的教程，我们已了解如何生成一个 GridView 其行、 与所选行 s 的详细信息显示在同一页面上的说明。 GridView 的 ObjectDataSource 返回有关使用的所有产品的信息`ProductsBLL`s`GetProducts()`方法，同时说明的 ObjectDataSource 检索有关所选的产品使用信息`GetProductsByProductID(productID)`方法。 *`productID`* 参数值以声明方式由 GridView s 的值与关联`SelectedValue`属性。 遗憾的是，中继器没有`SelectedValue`属性和不能用作参数源。

> [!NOTE]
> 这是显示在中继器中使用 LinkButton 时这些挑战之一。 我们已使用超链接传入`CategoryID`通过查询字符串相反，我们无法将该查询字符串字段用作源参数的值。


我们担心在缺乏之前`SelectedValue`属性中继器，不过，让我们来首先将 DataList 绑定到对象数据源，并指定其`ItemTemplate`。

从 DataList s 智能标记中，选择要添加名为新 ObjectDataSource`CategoryProductsDataSource`和将其配置为使用`ProductsBLL`类的`GetProductsByCategoryID(categoryID)`方法。 由于在本教程中 DataList 提供了一个只读接口，随意中插入、 更新、 设置的下拉列表，删除选项卡添加到 （无）。


[![配置对象数据源以使用 ProductsBLL 类的 GetProductsByCategoryID(categoryID) 方法](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image32.png)

**图 12**： 配置使用 ObjectDataSource`ProductsBLL`类 s`GetProductsByCategoryID(categoryID)`方法 ([单击以查看实际尺寸的图像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image34.png))


由于`GetProductsByCategoryID(categoryID)`方法需要输入的参数 (*`categoryID`*)，配置数据源向导允许我们需要指定参数的源。 已在一个 GridView 或 DataList 预置类别，我们 d 将参数源下拉列表设置为控件和到 ControlID `ID` Web 控件的数据。 但是，由于中继器缺少`SelectedValue`不能用作参数源属性。 如果选中，你将找到 ControlID 下拉列表仅包含一个控件`ID``CategoryProducts`、 `ID` DataList。

现在，将参数源下拉列表设置为 None。 我们将得到以编程方式指定此参数值时在转发器中单击 LinkButton 的类别。


[![执行不指定 categoryID 参数参数源](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image35.png)

**图 13**： 未指定参数源 *`categoryID`* 参数 ([单击以查看实际尺寸的图像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image37.png))


完成配置数据源向导后，Visual Studio 自动生成 DataList 的`ItemTemplate`。 替换此默认设置`ItemTemplate`该模板后，我们前面教程中使用; 此外，还要设置 DataList 的`RepeatColumns`属性设置为 2。 进行这些更改后你 DataList 和其关联的对象数据源的声明性标记应如下所示：


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample10.aspx)]

目前， `CategoryProductsDataSource` ObjectDataSource s  *`categoryID`* 永远不会设置参数，以便查看网页时，将显示安装的产品。 我们需要做什么是将此参数值设置基于`CategoryID`的转发器中的被单击类别。 它引入了两个难题： 首先，如何执行我们确定何时在转发器的 LinkButton`ItemTemplate`已单击; 和第二个，我们可以如何确定`CategoryID`其 LinkButton 被单击相应的类别的？

与按钮和 ImageButton 控件一样 LinkButton 具有`Click`事件和一个[`Command`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx)。 `Click`事件旨在已单击 LinkButton 则只需注意。 有时，但是，除了指出已单击 LinkButton 我们还需要将一些额外信息传递给事件处理程序。 如果出现这种情况，LinkButton s [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx)和[ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx)属性可以分配此额外信息。 然后，当单击 LinkButton 后，其`Command`事件将触发 (而不是其`Click`事件) 和事件处理程序传递的值`CommandName`和`CommandArgument`属性。

当`Command`中继器中继器 s 中的模板内从引发事件[`ItemCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx)触发并传递`CommandName`和`CommandArgument`单击 LinkButton 值 (或按钮或ImageButton)。 因此，若要确定类别中中继器 LinkButton 单击时，我们需要执行以下操作：

1. 设置`CommandName`属性在转发器的 LinkButton`ItemTemplate`为某个值 (我以前使用 ListProducts)。 通过将此值设置`CommandName`值，LinkButton 的`Command`LinkButton 单击时，事件将激发。
2. 设置 LinkButton s`CommandArgument`属性的当前项的值`CategoryID`。
3. 为转发器 s 创建事件处理程序`ItemCommand`事件。 在事件处理程序中，集中`CategoryProductsDataSource`ObjectDataSource s`CategoryID`为传入的值的参数`CommandArgument`。

以下`ItemTemplate`标记类别中继器实现步骤 1 和 2。 请注意如何`CommandArgument`值分配数据项的`CategoryID`使用数据绑定的语法：


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample11.aspx)]

每当创建`ItemCommand`是明智始终首先检查传入的事件处理程序`CommandName`值因为*任何*`Command`引发事件*任何*按钮、 LinkButton，或将导致在转发器 ImageButton`ItemCommand`事件激发。 虽然我们当前仅有一个此类 LinkButton 现在，在将来我们 （或我们的团队的其他开发人员） 可能其他按钮 Web 将控件添加到转发器，单击时，引发相同`ItemCommand`事件处理程序。 因此，它 s 最好请始终确保你检查`CommandName`属性然后如果到预期的值的它匹配仅进行编程逻辑。

后确保传入的`CommandName`值等于 ListProducts，事件处理程序然后将分配`CategoryProductsDataSource`ObjectDataSource s`CategoryID`为传入的值的参数`CommandArgument`。 此修改 ObjectDataSource 的`SelectParameters`会自动导致 DataList 重新本身绑定到数据源，显示新选择的类别的产品。


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample12.cs)]

其中这些添加的内容，我们的教程中已完成 ！ 需要一段时间来在浏览器中对它进行测试。 图 14 首先访问页时显示的屏幕。 由于某一类别中尚未选择，则会不显示任何产品。 单击类别，如生成，显示这些产品的产品类别中的两列视图 （请参阅图 15）。


[![要安装的产品是显示在第一个中访问的页面](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image38.png)

**图 14**： 否产品不显示在第一个中访问的页面 ([单击以查看实际尺寸的图像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image40.png))


[![单击右侧的匹配产品的生成类别列表](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image41.png)

**图 15**： 单击生成类别列出右侧的匹配产品 ([单击以查看实际尺寸的图像](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image43.png))


## <a name="summary"></a>摘要

正如我们看到在本教程和前一次，主/详细信息报表可以分散在两个页，或其中一个上合并。 在单个页面上显示母版/详细信息报告，但是，引入了一些难题如何最布局 master 和此页上的详细信息记录到。 在*母版/详细介绍与详细信息说明如何使用可选择的主 GridView*教程，我们必须出现上方的主记录的详细信息记录; 在本教程中我们用于 CSS 技术具有到的主记录 float左侧的详细信息。

以及显示主/详细信息报表，我们还具有我们来探讨如何检索与每个类别以及如何以 LinkButton （或按钮或 ImageButton） 时执行服务器端逻辑单击内的产品数转发器。

本教程中完成主/详细信息报表的 DataList 和转发器与我们的检查。 我们接下来的教程将说明如何添加编辑和删除 DataList 控件的功能。

尽情享受编程 ！

## <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/)浮点 CSS 元素使用 CSS 的教程
- [CSS 定位](http://www.brainjar.com/css/positioning/)定位使用 CSS 的元素的详细信息
- [对出内容替换 HTML](http://www.w3schools.com/html/html_layout.asp)使用`<table>`s 的和其他 HTML 元素定位

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Zack Jones。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](master-detail-filtering-acess-two-pages-datalist-cs.md)
[下一页](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
