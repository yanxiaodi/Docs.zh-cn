---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
title: "分页 DataList 或转发器控件 (C#) 中的报表数据 |Microsoft 文档"
author: rick-anderson
description: "在 DataList 和转发器都不提供自动分页或排序支持时，本教程演示如何将分页支持添加到 DataList 或转发器..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: e8e0809b-25c4-4c3b-8d12-9a17048148ae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 4952adff752ec834b8be5f190181be98a034ccfd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-c"></a>DataList 或转发器控件 (C#) 中的分页报表数据
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_CS.exe)或[下载 PDF](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial44cs1.pdf)

> 虽然 DataList 和转发器都不提供自动分页或排序支持，本教程演示如何将分页支持添加到 DataList 或转发器，允许更灵活的分页和数据的显示接口。


## <a name="introduction"></a>介绍

分页和排序是两个非常常见功能，当在联机应用程序中显示数据。 例如，当搜索时在联机书店 ASP.NET 丛书，可能有数百个此类丛书，但此报表列出搜索结果将列出每页的仅十个匹配项。 此外，可以按标题、 价格、 页计数、 作者姓名等排序结果。 如我们所述[分页和排序报表数据](../paging-and-sorting/paging-and-sorting-report-data-cs.md)教程，都提供内置的分页支持，还可以启用在一个复选框的刻度线的 GridView、 说明如何和 FormView 控件。 GridView 还包括排序支持。

遗憾的是，DataList 和转发器都不提供自动分页或排序支持。 在本教程中我们将查看如何将分页支持添加到 DataList 或转发器。 我们必须手动创建的分页界面、 显示相应的页面的记录，并记住正在跨回发访问过的网页。 尽管这接受更多时间和代码比使用 GridView、 说明或 FormView，DataList 和转发器允许更灵活的分页和数据显示接口。

> [!NOTE]
> 本教程以独占方式重点分页。 在下一教程中，我们将打开我们注意到添加排序功能。


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>步骤 1： 添加分页和排序教程网页

在开始本教程之前，让 s 先花点时间以添加我们需要用于本教程; 下一步的另一个的 ASP.NET 页面。 首先创建一个新文件夹中名为的项目`PagingSortingDataListRepeater`。 接下来，将以下五个 ASP.NET 页添加到此文件夹中，让所有的它们配置为使用母版页`Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![创建 PagingSortingDataListRepeater 文件夹并添加教程 ASP.NET 页](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**图 1**： 创建`PagingSortingDataListRepeater`文件夹并添加教程 ASP.NET 页


接下来，打开`Default.aspx`页上，并将其拖`SectionLevelTutorialListing.ascx`从用户控件`UserControls`到设计图面上的文件夹。 用户控件中，我们在中创建的[母版页和网站的导航](../introduction/master-pages-and-site-navigation-cs.md)教程中，枚举站点图，并在项目符号列表中的当前节中显示这些教程。


[![SectionLevelTutorialListing.ascx 用户控件添加到 Default.aspx](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)

**图 2**： 添加`SectionLevelTutorialListing.ascx`用户控件`Default.aspx`([单击以查看实际尺寸的图像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image4.png))


为了具有显示分页和排序的教程中我们将创建的项目符号列表，我们需要将它们添加到站点映射。 打开`Web.sitemap`文件，并替换为 DataList 站点映射节点标记的编辑和删除之后添加以下标记：


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample1.xml)]


![更新包括新的 ASP.NET 页的站点图](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)

**图 3**： 更新包括新的 ASP.NET 页的站点图


## <a name="a-review-of-paging"></a>分页一项检查

在前面的教程中，我们已了解如何通过 GridView、 说明如何和 FormView 控件中的数据页上。 这三个控件提供一个简单窗体的分页调用*默认分页*，可以通过只需选中启用分页中的控件 s 智能标记选项来实现。 使用默认分页，每次请求一页数据的第一页上访问或当用户导航到另一页数据 GridView，说明如何，或 FormView 控件重新请求*所有*的中的数据对象数据源。 它然后片段出这组特定的记录，以显示给定请求的页索引和要每页显示的记录数。 我们讨论了中的详细信息中的默认分页[分页和排序报表数据](../paging-and-sorting/paging-and-sorting-report-data-cs.md)教程。

由于默认分页重新请求为每个页的所有记录，它是不可行时充分大量的数据进行分页。 例如，假设通过使用页面大小为 10 的 50,000 个记录的分页。 用户移到新的页上，每次必须检索所有 50,000 个记录从数据库中，尽管唯一 10 个，其中会显示。

*自定义分页*通过抓取仅记录在请求的页面上显示的精确子集解决了默认分页的性能问题。 在实现自定义分页时，我们必须编写 SQL 查询将有效地返回只是正确的记录集。 我们已了解如何创建一个查询使用 SQL Server 2005 s 新[`ROW_NUMBER()`关键字](http://www.4guysfromrolla.com/webtech/010406-1.shtml)进来[高效地分页通过大型金额的数据](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md)教程。

若要在 DataList 或转发器控件中实现默认分页，我们可以使用[`PagedDataSource`类](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx)作为周围的包装器`ProductsDataTable`其内容正在通过寻呼发送。 `PagedDataSource`类具有`DataSource`可以分配给任何可枚举对象的属性和[ `PageSize` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx)和[ `CurrentPageIndex` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx)属性用来指示多少条记录到每页的显示和当前的页索引。 已设置这些属性之后,`PagedDataSource`可以用作 Web 控件的任何数据的数据源。 `PagedDataSource`，当枚举时，将只返回其内部的记录的相应子集`DataSource`基于`PageSize`和`CurrentPageIndex`属性。 图 4 展示了的功能`PagedDataSource`类。


![PagedDataSource 包装具有可分页接口的可枚举对象](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image6.png)

**图 4**:`PagedDataSource`包装具有可分页接口的可枚举对象


`PagedDataSource`对象可以是创建和配置直接从业务逻辑层和绑定到 DataList 或转发器通过对象数据源，或可以创建和配置 ASP.NET 页面的代码隐藏类中直接。 如果使用后一种方法，我们必须放弃使用对象数据源，并改为将分页的数据绑定到 DataList 或转发器以编程方式。

`PagedDataSource`对象还具有属性，以支持自定义分页。 但是，我们可以绕过使用`PagedDataSource`的自定义分页，因为我们在已有 BLL 方法`ProductsBLL`用于返回精确的记录，若要显示的自定义分页的类。

在本教程中我们将查看通过添加一个新方法中 DataList 实现默认分页`ProductsBLL`返回进行了适当配置的类`PagedDataSource`对象。 在下一步的教程中，我们将了解如何使用自定义分页。

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>步骤 2： 在业务逻辑层中添加默认分页方法

`ProductsBLL`类当前具有返回所有产品信息的方法`GetProducts()`，另一个用于都返回起始索引处的产品的特定子集`GetProductsPaged(startRowIndex, maximumRows)`。 使用默认分页的 GridView，说明如何和 FormView 控制所有使用`GetProducts()`方法来检索所有产品，但然后使用`PagedDataSource`内部以显示仅记录的正确子集。 若要复制此功能，带有 DataList 和转发器控件，我们可以在模仿此行为 BLL 中创建一个新方法。

将方法添加到`ProductsBLL`类名为`GetProductsAsPagedDataSource`采用在两个整数的输入参数：

- `pageIndex`若要显示，页的索引索引为零，并
- `pageSize`要显示每页的记录数。

`GetProductsAsPagedDataSource`通过检索启动*所有*的从记录`GetProducts()`。 然后，它创建`PagedDataSource`对象，并设置其`CurrentPageIndex`和`PageSize`属性的值传入的`pageIndex`和`pageSize`参数。 该方法最后返回此进行配置`PagedDataSource`:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>步骤 3： 在使用默认分页 DataList 中显示产品信息

与`GetProductsAsPagedDataSource`方法添加到`ProductsBLL`类，我们现在可以创建 DataList 或提供默认分页的转发器。 首先打开`Paging.aspx`页面`PagingSortingDataListRepeater`文件夹，然后从工具箱中拖动到设计器中，设置 DataList 的拖动 DataList`ID`属性`ProductsDefaultPaging`。 从 DataList s 智能标记，创建名为新 ObjectDataSource`ProductsDefaultPagingDataSource`并将其配置，以便它检索数据使用`GetProductsAsPagedDataSource`方法。


[![创建 ObjectDataSource 并将其配置为使用 GetProductsAsPagedDataSource （） 方法](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**图 5**： 创建对象数据源并将它配置为使用`GetProductsAsPagedDataSource``()`方法 ([单击以查看实际尺寸的图像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


在更新中，INSERT、 设置的下拉列表和删除选项卡添加到 （无）。


[![在更新中，INSERT、 设置下拉列表将列出和删除选项卡添加到 （无）](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**图 6**： 在更新中，INSERT、 设置下拉列表将列出和删除选项卡添加到 （无） ([单击以查看实际尺寸的图像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


由于`GetProductsAsPagedDataSource`方法需要两个输入的参数，该向导将提示我们为这些参数值的源。

必须在回发记住的页索引和页大小的值。 它们可以存储在视图状态、 保存到查询字符串、 存储在会话变量或记住使用其他方法。 对于本教程中，我们将使用查询字符串，它具有要将标有书签的数据的特定页的优势。

具体而言，使用的查询字符串字段 pageIndex 和有关的 pageSize`pageIndex`和`pageSize`参数，分别 （请参阅图 7）。 需要一段时间来设置这些参数的默认值，如下图所获胜 t 的查询字符串值时必须存在的用户首次访问此页。 有关`pageIndex`，设置默认值为 0 （这将显示数据的第一页） 和`pageSize`的默认值为 4。


[![使用查询字符串作为源的 pageIndex 和 pageSize 参数](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**图 7**： 查询字符串用作源`pageIndex`和`pageSize`参数 ([单击以查看实际尺寸的图像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image15.png))


在配置对象数据源后，Visual Studio 自动创建`ItemTemplate`DataList 有关。 自定义`ItemTemplate`以便显示产品的名称、 类别和供应商。 此外设置 DataList s`RepeatColumns`属性设置为 2，其`Width`为 100%，并将其`ItemStyle`s`Width`至 50%。 这些宽度设置将提供两个列的间距相等。

进行这些更改后，DataList 和 ObjectDataSource 的标记应类似于以下：


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

> [!NOTE]
> 由于我们并不执行任何更新或删除功能在本教程中，你可以通过禁用 DataList 的视图状态，以减少呈现的页面大小。


在最初两者访问通过浏览器，此页时`pageIndex`也不`pageSize`提供查询字符串参数。 因此，使用默认值为 0 和 4。 如图 8 所示，这将导致显示的前四个产品 DataList。


[![列出了第一个四个产品](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image16.png)

**图 8**： 列出的第一个四个产品 ([单击以查看实际尺寸的图像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image18.png))


没有用户导航到数据的第二页意味着分页接口，那里 s 当前不简单。 我们将在步骤 4 中创建分页接口。 现在，不过，分页仅可通过直接在查询字符串中指定的分页条件。 例如，若要查看的第二页，更改从浏览器的地址栏中的 URL`Paging.aspx`到`Paging.aspx?pageIndex=2`然后按 Enter。 这将导致要显示数据的第二页 （请参阅图 9）。


[![显示第二个数据页](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image19.png)

**图 9**： 显示第二个页面的数据 ([单击以查看实际尺寸的图像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>步骤 4： 创建分页接口

有各种不同的分页接口，可以实现。 GridView、 说明如何和 FormView 控件提供四个不同的接口之间进行选择：

- **下一步、 上一条**用户，到下一个或上一次移动一个页。
- **下一步上, 一步，首先，最后**除了下一步和上一步按钮，该接口包括用于移动到第一个或最后一页的第一个和最后一个按钮。
- **数值**列出中的页码的分页界面，允许用户快速跳转到特定页。
- **数字，首先，最后**数值的页码，除了包括用于移动到第一个或最后一页的按钮。

DataList 和转发器中，为我们负责决定在分页接口和实现它。 这涉及到创建所需的 Web 控件在页和单击特定的分页接口按钮时显示请求的页。 此外，某些分页界面控件可能需要禁用。 例如，查看时使用下一个数据的第一页上, 一步，首先，上次接口，第一个和上一步按钮将被禁用。

对于本教程，让的 s 使用下一步上, 一步，首先，最后的接口。 向页面添加四个按钮 Web 控件并设置其`ID`到`FirstPage`， `PrevPage`， `NextPage`，和`LastPage`。 设置`Text`属性设置为&lt;&lt;第一个，&lt;上一步、 下一步&gt;，和最后一个&gt; &gt; 。


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample4.aspx)]

接下来，创建`Click`为每个这些按钮的事件处理程序。 在稍后我们将添加需显示请求的页的代码。

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>记住正在通过寻呼发送到的记录的总数

无论选择的分页界面，我们需要计算并记住的正在通过寻呼发送到的记录总数。 总行数 （结合使用页面大小） 确定的数据的多少总页数正在通过寻呼发送通过，它确定分页界面控件添加或启用。 在下一步、 上一步、 第一个中，最后一个接口，我们要生成的页计数是两种方式使用：

- 若要确定是否我们正在查看的最后一页，在这种情况下禁用下一步和最后一个按钮。
- 如果用户单击到最后一个 whisk 它们，我们需要最后一个按钮页上，其索引是一种小于页计数。

如总行数的上限除以页大小计算页计数。 例如，如果我们分页 79 记录每页的四个记录，然后页计数为 20 (79 上限 / 4)。 如果我们使用数字分页接口时，此信息通知我们并显示; 多少数字寻呼按钮如果我们分页接口包括下一步或上一次按钮，，用于确定何时要禁用下一步或上一次按钮页计数。

如果分页接口包含最后一个按钮，则命令性，以便当最后一个按钮被单击时我们可以确定最后一个的页索引，在回发记住的正在通过寻呼发送到的记录总数。 若要为此，创建`TotalRowCount`仍然存在其值可查看状态 ASP.NET 页的代码隐藏类中的属性：


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

除了`TotalRowCount`、 花点时间查看创建用于轻松地访问页索引、 页大小的只读的页级别的属性和页计数：


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample6.cs)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>确定正在通过寻呼发送到的记录总数

`PagedDataSource`对象从 ObjectDataSource s 返回`Select()`方法已在其中*所有*条产品记录，即使它们的一个子集显示在 DataList。 `PagedDataSource` S [ `Count`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx)只返回数的项将显示在 DataList; [ `DataSourceCount`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx)返回中的项的总数`PagedDataSource`. 因此，我们需要分配 ASP.NET 页 s`TotalRowCount`属性值的`PagedDataSource`s`DataSourceCount`属性。

若要实现此目的，创建的事件处理程序 ObjectDataSource 的`Selected`事件。 在`Selected`我们有权访问 ObjectDataSource s 的返回值的事件处理程序`Select()`方法在此情况下， `PagedDataSource`。


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

## <a name="displaying-the-requested-page-of-data"></a>显示请求的数据页

当用户单击分页接口中的按钮之一时，我们需要显示请求的数据页。 由于分页参数通过指定查询字符串，以显示数据使用的请求的页面`Response.Redirect(url)`具有用户 s 浏览器重新请求`Paging.aspx`结合适当的分页参数的页。 例如，若要显示的数据的第二页，我们会将用户重定向到`Paging.aspx?pageIndex=1`。

若要为此，创建`RedirectUser(sendUserToPageIndex)`方法，将重定向到用户`Paging.aspx?pageIndex=sendUserToPageIndex`。 然后，从四个按钮调用此方法`Click`事件处理程序。 在`FirstPage``Click`事件处理程序，请调用`RedirectUser(0)`，以将其发送到第一页; 在`PrevPage``Click`事件处理程序中，使用`PageIndex - 1`页索引中; 为，依此类推。


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample8.cs)]

与`Click`事件处理程序完成，DataList 的记录可以通过单击的按钮分页通过。 花一些时间来试试看 ！

## <a name="disabling-paging-interface-controls"></a>禁用分页界面控件

目前，所有四个按钮而不考虑查看的页的启用。 但是，我们想要禁用的第一个和上一步按钮时显示的最后一页时显示的数据，以及下一步和最后一个按钮的第一页。 `PagedDataSource`返回 ObjectDataSource 的对象`Select()`方法具有属性[ `IsFirstPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx)和[ `IsLastPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) ，我们可以检查以确定是否我们正在查看数据的第一个或最后一页。

将以下代码添加到 ObjectDataSource 的`Selected`事件处理程序：


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

添加此元素后，查看第一页中，负载测试时查看的最后一页时，将禁用下一步和最后一个按钮时，将禁用第一个和上一步按钮。

允许 s 完成分页接口通过告知用户哪一页它们重新当前正在查看和存在多少总页数。 向页面添加标签 Web 控件并设置其`ID`属性`CurrentPageNumber`。 设置其`Text`ObjectDataSource s 所选事件处理程序中此类的属性，它包含当前正在查看的页 (`PageIndex + 1`) 和总页数 (`PageCount`)。


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample10.cs)]

图 10 显示`Paging.aspx`时首次访问。 DataList 因为查询字符串为空，默认为显示的前四个产品;第一个和上一步按钮被禁用。 单击下一步显示 （请参阅图 11） 的下一步四个记录;第一个和上一步按钮现已启用。


[![显示第一个数据页](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image22.png)

**图 10**： 第一页的显示数据时 ([单击以查看实际尺寸的图像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image24.png))


[![显示第二个数据页](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image25.png)

**图 11**： 显示第二个页面的数据 ([单击以查看实际尺寸的图像](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image27.png))


> [!NOTE]
> 可以进一步增强分页接口，通过允许用户指定多少页来查看每页。 例如，DropDownList 无法添加列表类似 5、 10、 25，50，和所有的页面大小选项。 在选择的页大小，用户将需要重定向回`Paging.aspx?pageIndex=0&pageSize=selectedPageSize`。 我将保留为读取器为实现此增强功能。


## <a name="using-custom-paging"></a>使用自定义分页

通过使用效率低下的默认分页技术其数据 DataList 页中。 当足够大，大量数据进行分页时, 是命令性，使用自定义分页。 尽管略有不同的实现详细信息，在 DataList 中实现自定义分页背后的概念并使用默认分页相同。 使用自定义分页，使用`ProductBLL`类 s`GetProductsPaged`方法 (而不是`GetProductsAsPagedDataSource`)。 中所述[高效地分页通过大型金额的数据](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md)教程中，`GetProductsPaged`必须传递要返回的行开始行索引和最大值数。 这些参数可以通过查询字符串就像一样维护`pageIndex`和`pageSize`使用默认分页的参数。

由于存在 s 没有`PagedDataSource`使用自定义分页，必须使用其他方法来确定总通过以及是否正在通过寻呼发送的记录数我们重新显示数据的第一个或最后一页。 `TotalNumberOfProducts()`中的方法`ProductsBLL`类返回正在通过寻呼发送到的产品的总数。 若要确定是否在查看数据的第一页，检查开始的行索引如果它是零，则在查看第一页。 如果开始的行索引加上要返回的最大行是大于或等于的记录正在通过寻呼发送通过总数在查看的最后一页。

我们将探讨在下一教程中更详细地实现自定义分页。

## <a name="summary"></a>摘要

DataList 和转发器都不提供外框分页支持位于 GridView，说明如何，和 FormView 控制，可以使用最小的工作量添加这样的功能。 实现默认分页的最简单方法是将中的产品的整个集`PagedDataSource`，然后将绑定`PagedDataSource`到 DataList 或转发器。 在本教程，我们添加`GetProductsAsPagedDataSource`方法`ProductsBLL`类返回`PagedDataSource`。 `ProductsBLL`类已包含所需的自定义分页方法`GetProductsPaged`和`TotalNumberOfProducts`。

检索要显示的自定义分页的记录的精确集，或者中的所有记录以及`PagedDataSource`对于默认分页，我们还需要手动添加分页接口。 对于本教程中，我们将创建下一步上, 一步，首先，上次接口与四个按钮 Web 控件。 此外，还添加显示当前页码和总页数的标签控件。

在下一步的教程，我们将了解如何将排序支持添加到 DataList 和转发器。 我们将了解如何创建 DataList 可以同时分页和排序 （与使用默认和自定义分页的示例）。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已沈 Shulok、 Ken Pespisa 和伯纳黛特 Leigh。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[下一篇](sorting-data-in-a-datalist-or-repeater-control-cs.md)
