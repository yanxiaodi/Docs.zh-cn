---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
title: "创建自定义排序的用户界面 (VB) |Microsoft 文档"
author: rick-anderson
description: "显示较长的列表已排序数据，它可以是非常有帮助向通过引入分隔符行组相关的数据。 在本教程中我们将了解如何凭据..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: f3897a74-cc6a-4032-8f68-465f155e296a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 527bfe62a7293d0aa2d6b5b83119bb922906120b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-customized-sorting-user-interface-vb"></a>创建自定义排序的用户界面 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_VB.exe)或[下载 PDF](creating-a-customized-sorting-user-interface-vb/_static/datatutorial27vb1.pdf)

> 显示较长的列表已排序数据，它可以是非常有帮助向通过引入分隔符行组相关的数据。 在本教程中我们将了解如何创建此类的排序的用户界面。


## <a name="introduction"></a>介绍

显示较长的列表排序时数据中只有少量的已排序的列中的不同值，最终用户可能很难识别，完全匹配，差异边界发生。 例如，有 81 产品的数据库，但只有九个不同的类别选项 (八个唯一类别加上`NULL`选项)。 请考虑有兴趣检查数据项类别下的产品的用户。 从页，其中列出*所有*的单个 GridView 中的产品，用户可能会决定她的最佳方法是对结果进行排序按类别、 将组合在一起的所有数据项产品组合在一起。 按类别进行排序后, 用户然后需要查寻列表中，通过查找数据项分组产品开始和结束的位置。 由于对结果进行的按字母顺序排序，因此查找数据项产品的类别名称并不困难，但它仍需要密切扫描中网格项的列表。

为了帮助突出显示排序组之间的边界，很多网站，请采用一个用户界面，将添加此类组之间的分隔符。 如图 1 所示的分隔符使用户能够更快地找到特定的组和标识其边界，以及确定哪些不同的组数据中存在。


[![每个类别组都清楚地标识](creating-a-customized-sorting-user-interface-vb/_static/image2.png)](creating-a-customized-sorting-user-interface-vb/_static/image1.png)

**图 1**： 每个类别组是明确标识 ([单击以查看实际尺寸的图像](creating-a-customized-sorting-user-interface-vb/_static/image3.png))


在本教程中我们将了解如何创建此类的排序的用户界面。

## <a name="step-1-creating-a-standard-sortable-gridview"></a>步骤 1： 创建一个标准的、 可排序的 GridView

我们探讨如何增加 GridView 提供增强的排序界面之前，让我们来首先创建一个标准的、 可排序的 GridView，列出的产品。 首先打开`CustomSortingUI.aspx`页面`PagingAndSorting`文件夹。 向页面添加一个 GridView，设置其`ID`属性`ProductList`，并将其绑定到新对象数据源。 配置对象数据源以使用`ProductsBLL`类的`GetProducts()`用于选择记录的方法。

接下来，配置 GridView，以便它仅包含`ProductName`， `CategoryName`， `SupplierName`，和`UnitPrice`BoundFields 和停止使用 CheckBoxField。 最后，配置 GridView 支持排序检查 GridView s 智能标记中的启用排序复选框 (或通过设置其`AllowSorting`属性`true`)。 建立到这些添加后`CustomSortingUI.aspx`页上，声明性标记应类似于以下：


[!code-aspx[Main](creating-a-customized-sorting-user-interface-vb/samples/sample1.aspx)]

需要一段时间来在浏览器为止查看我们的进度。 图 2 显示可排序的 GridView 时其数据按类别按字母顺序进行排序。


[![可排序 GridView s 按类别排列数据](creating-a-customized-sorting-user-interface-vb/_static/image5.png)](creating-a-customized-sorting-user-interface-vb/_static/image4.png)

**图 2**： 按类别排列数据的可排序 GridView s ([单击以查看实际尺寸的图像](creating-a-customized-sorting-user-interface-vb/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>步骤 2： 添加分隔符行浏览技术

通用的可排序 GridView 完成，所有这些剩下将与能够在每个唯一的已排序组之前 GridView 中添加分隔符行。 但是，如何可以这样的行被注入到 GridView？ 从根本上来说，我们需要以循环访问 GridView 的行，确定在已排序的列中，值之间出现差异的位置，然后添加适当的分隔符行。 在考虑此问题，它看起来自然解决方案某处在于 GridView 的`RowDataBound`事件处理程序。 如我们所述[自定义格式设置基于时数据](../custom-formatting/custom-formatting-based-upon-data-vb.md)教程，则当应用行级格式基于行的数据，通常使用此事件处理程序。 但是，`RowDataBound`事件处理程序不在这里，该解决方案，因为行不能添加到 GridView 以编程方式从此事件处理程序。 GridView 的`Rows`集合，事实上，是只读的。

若要将其他行添加到 GridView 我们具有三个选项：

- 将这些元数据分隔符行添加到绑定到 GridView 的实际数据
- GridView 已绑定到数据后，将添加其他`TableRow`实例到 GridView s 控制集合
- 创建的自定义服务器控件扩展 GridView 控件，重写这些方法负责构造 GridView 的结构

如果在许多 web 页或多个网站中需要此功能，则创建自定义服务器控件将是最好的方法。 但是，它需要大量的代码和 GridView s 内部工作情况的全面浏览到深度。 因此，我们将不考虑该选项对于本教程。

另外两种选项添加分隔符行的实际数据被绑定到 GridView 和处理 GridView 的控件集合后其已绑定-以不同方式攻击问题并需要讨论。

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>将行添加到数据绑定到 GridView

当 GridView 绑定到数据源时，它将创建`GridViewRow`为数据源返回每个记录。 因此，我们可以通过添加分隔符记录到数据源绑定到 GridView 之前插入所需的分隔符行。 图 3 说明了这一概念。


![一种方法涉及将分隔符行添加到数据源](creating-a-customized-sorting-user-interface-vb/_static/image7.png)

**图 3**： 一种方法涉及将分隔符行添加到数据源


我在引号中使用的术语分隔符记录，因为没有特殊分隔符记录;相反，我们必须以某种方式标记的数据源中的特定记录用作分隔符，而不是正常的数据行。 有关我们的示例，我们重新绑定`ProductsDataTable`实例到 GridView，组成`ProductRows`。 我们可能会通过设置一条记录标记为分隔符行其`CategoryID`属性`-1`（因为通常存在此类值无法 t）。

若要利用此技术 d 我们需要执行以下步骤：

1. 以编程方式检索数据绑定到 GridView (`ProductsDataTable`实例)
2. 基于 GridView s 对数据进行排序`SortExpression`和`SortDirection`属性
3. 循环访问`ProductsRows`中`ProductsDataTable`，他们寻求哪里排序列中的差异
4. 在每个组边界，插入分隔符记录`ProductsRow`实例到数据表，即具有它 s`CategoryID`设置为`-1`（或任何代码已决定将标记作为分隔符记录的记录）
5. 之后将注入分隔符行，以编程方式将数据绑定到 GridView

除了这些五个步骤，d 我们还需要提供事件处理程序 GridView s`RowDataBound`事件。 在这里，我们 d 检查每个`DataRow`并确定它是否分隔符第一行，其`CategoryID`设置以前`-1`。 如果是这样，我们 d 可能想要调整其格式设置或单元格中显示的文本。

使用这种方法，根据需要还 GridView s 提供事件处理程序，以将注入排序组边界需要多做一些工作不是上述，`Sorting`的跟踪事件并保留`SortExpression`和`SortDirection`值。

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>操作 GridView s s 控制在它后面的集合被数据绑定

而不是之前将其绑定到 GridView 邮件数据，我们可以添加分隔符行*后*数据绑定到 GridView。 数据绑定的过程建立 GridView 的控制层次结构，这实际上是只需`Table`实例组成的行，其中每个由个单元格的集合组成的集合。 具体而言，GridView 的控件集合包含`Table`对象在其根`GridViewRow`(派生自`TableRow`类) 为在每个记录`DataSource`绑定到 GridView，和一个`TableCell`中每个对象`GridViewRow`实例中每个数据字段`DataSource`。

若要添加每个排序组之间的分隔符行，我们可以直接操作此控件层次结构后已创建。 我们可以确信已为呈现页面的时间的最后一个时间创建 GridView 的控制层次结构。 因此，这种方法都会重写`Page`类的`Render`方法，此时 GridView s 最终控件层次结构更新以包含所需的分隔符的行。 图 4 说明了此过程。


[![另一种方法操作 GridView 的控制层次结构](creating-a-customized-sorting-user-interface-vb/_static/image9.png)](creating-a-customized-sorting-user-interface-vb/_static/image8.png)

**图 4**： 另一种方法操作控制层次结构中的 GridView s ([单击以查看实际尺寸的图像](creating-a-customized-sorting-user-interface-vb/_static/image10.png))


对于本教程中，我们将使用此后一种方法以自定义排序的用户体验。

> [!NOTE]
> 代码我 m 提供在本教程基于提供的示例[Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) s 博客文章[播放与 GridView 排序分组位](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx)。


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>步骤 3： 将分隔符行添加到的 GridView 的控件层次结构

由于我们只想要将分隔符的行添加到的 GridView 的控件层次结构，被创建并在该页面，请访问最后一次创建其控制层次结构后，我们想要执行结束时此添加的页面生命周期，但在实际的 GridView c 之前到 HTML 呈现管理下层次结构。 此时我们可以实现此目的的最新可能点是`Page`类的`Render`事件，我们可以在我们使用以下方法签名的代码隐藏类中重写：


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample2.vb)]

当`Page`类 s 原始`Render`调用方法`base.Render(writer)`每个页中的控件将呈现，生成基于其控件层次结构的标记。 因此，它是命令性我们这两个调用`base.Render(writer)`，以便在呈现页面，和我们操作 GridView s 控制层次结构之前调用`base.Render(writer)`，以便分隔符行已添加到之前的 GridView 的控件层次结构s 已呈现。

若要插入的排序组标头我们首先需要确保用户已请求对数据进行排序。 默认情况下，未排序的 GridView 的内容，并因此我们不需要输入排序标头的任何组。

> [!NOTE]
> 如果你想 GridView 要按特定列排序，首次加载页时，调用 GridView 的`Sort`方法上第一次的页访问 （但不是能在后续回发）。 若要实现此目的，添加此调用在`Page_Load`内的事件处理程序`if (!Page.IsPostBack)`条件。 将回指[分页和排序报表数据](paging-and-sorting-report-data-vb.md)教程信息有关的详细信息`Sort`方法。


假设已排序数据，我们的下一个任务是确定哪些列的数据已按排序，且然后来扫描寻找该列 s 中的差异的行值。 以下代码确保数据已排序和查找数据排序所依据的列：


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample3.vb)]

如果 GridView 具有尚未为排序，GridView 的`SortExpression`属性将不设置。 因此，我们只想添加分隔符行，如果此属性具有一些值。 如果是这样，我们接下来需要确定数据已排序所依据的列的索引。 这通过循环访问 GridView s 实现`Columns`集合，其搜索列`SortExpression`属性等于 GridView 的`SortExpression`属性。 除了列的索引中，我们还抓住`HeaderText`属性，用于显示分隔符行时使用。

对数据进行排序所依据的索引，最后一步是列的要枚举的 GridView 的行。 为每个行，我们需要以确定是否已排序的列值 s 与不同于以前的行排序的 s 列的值。 如果这样，我们需要插入新`GridViewRow`到控件的层次结构的实例。 替换为以下代码完成此操作：


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample4.vb)]

此代码启动通过以编程方式引用`Table`对象位于 GridView 的控件层次结构的根，并创建一个名为的字符串变量`lastValue`。 `lastValue`用于当前行 s 排序列值与以前的行的值进行比较。 接下来，GridView s`Rows`枚举的集合，并为每个行排序的列的值存储在`currentValue`变量。

> [!NOTE]
> 若要确定特定行 s 排序列的值我使用的单元格的`Text`属性。 这非常适用于 BoundFields，但将不按预期方式工作的 TemplateFields，CheckBoxFields，依此类推。 我们将探讨如何很快帐户备用 GridView 字段。


`currentValue`和`lastValue`变量然后进行比较。 如果它们不同，我们需要将新的分隔符行添加到控件层次结构。 这通过确定的索引来实现`GridViewRow`中`Table`对象 s`Rows`集合，创建新`GridViewRow`和`TableCell`实例，并添加`TableCell`和`GridViewRow`到控制层次结构。

请注意分隔符行 s 唯一`TableCell`的格式设置为该视图所跨越的 GridView 的整个宽度格式都是使用`SortHeaderRowStyle`CSS 类，并且具有其`Text`属性如，其中显示排序组名称 （如类别） 和组的值 （如饮料）。 最后，`lastValue`更新的值为`currentValue`。

用于格式化排序的组头行的 CSS 类`SortHeaderRowStyle`需要指定在`Styles.css`文件。 可随意使用任何样式设置欢迎你;我使用了以下程序：


[!code-css[Main](creating-a-customized-sorting-user-interface-vb/samples/sample5.css)]

替换为当前的代码，排序接口添加排序组标头，通过任何 BoundField 排序时 （请参见图 5： 显示屏幕快照按供应商进行排序时）。 但是，如果按任何其他字段类型 （如 CheckBoxField 或 TemplateField） 进行排序，排序组标头是却找不到 （请参阅图 6）。


[![通过 BoundFields 排序时，排序接口包含排序组标头](creating-a-customized-sorting-user-interface-vb/_static/image12.png)](creating-a-customized-sorting-user-interface-vb/_static/image11.png)

**图 5**: 排序接口包括排序组标头时按排序 BoundFields ([单击以查看实际尺寸的图像](creating-a-customized-sorting-user-interface-vb/_static/image13.png))


[![排序组标头是缺少时排序 CheckBoxField](creating-a-customized-sorting-user-interface-vb/_static/image15.png)](creating-a-customized-sorting-user-interface-vb/_static/image14.png)

**图 6**: 排序组标头是缺少时排序 CheckBoxField ([单击以查看实际尺寸的图像](creating-a-customized-sorting-user-interface-vb/_static/image16.png))


按 CheckBoxField 进行排序时缺少排序组标头的原因是因为该代码当前只需使用`TableCell`s`Text`属性来确定每个行排序的列的值。 有关 CheckBoxFields， `TableCell` s`Text`属性为空字符串; 相反，值是可通过位于内的复选框 Web 控件`TableCell`s`Controls`集合。

若要处理除 BoundFields 以外的字段类型，我们需要扩充代码其中`currentValue`分配变量以检查是否存在的中的复选框`TableCell`s`Controls`集合。 而不是使用`currentValue = gvr.Cells(sortColumnIndex).Text`，此代码替换为以下代码：


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample6.vb)]

此代码检查排的序列`TableCell`确定是否有任何控件中的当前行`Controls`集合。 如果有，并且第一个控件是一个复选框，`currentValue`变量设置为是或否，具体取决于复选框的`Checked`属性。 否则，值取自`TableCell`s`Text`属性。 可以复制此逻辑以处理对 GridView 中可能存在任何 TemplateFields 进行排序。

上面的代码添加，排序组标头且现在显示在排序通过停止使用 CheckBoxField 时 （请参阅图 7）。


[![排序组标头是现在存在时排序 CheckBoxField](creating-a-customized-sorting-user-interface-vb/_static/image18.png)](creating-a-customized-sorting-user-interface-vb/_static/image17.png)

**图 7**: 排序组标头是现在存在时排序 CheckBoxField ([单击以查看实际尺寸的图像](creating-a-customized-sorting-user-interface-vb/_static/image19.png))


> [!NOTE]
> 如果你有与产品`NULL`数据库值`CategoryID`， `SupplierID`，或`UnitPrice`字段，这些值将显示为 GridView 中的空字符串默认情况下，这意味着为与这些产品的分隔符行的文本`NULL`值将如下类别: (即，在该处 s 类别后的没有名称： 喜欢个类别： Beverages)。 如果你想此处显示的一个值你可以设置 BoundFields [ `NullDisplayText`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx)文本到您要显示或分配时，可以在 Render 方法中添加条件语句`currentValue`为分隔符行的`Text`属性。


## <a name="summary"></a>摘要

GridView 不包括用于自定义排序的接口的许多内置选项。 但是，进行少量的低级别的代码，它可以调整 GridView 的控件层次结构，以创建更多自定义的接口的 s。 在本教程中，我们将了解如何将排序组分隔符行添加一个可排序的 GridView，更轻松地标识不同的组和这些组边界。 有关自定义排序接口的其他示例，请参阅[Scott Guthrie](https://weblogs.asp.net/scottgu/) s[几 ASP.NET 2.0 GridView 排序提示和技巧](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx)博客文章。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一篇](sorting-custom-paged-data-vb.md)
