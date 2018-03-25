---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
title: "对 DataList 或转发器控件 (C#) 中的数据进行排序 |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将研究如何包含排序 DataList 和转发器中的支持，以及如何构造其数据可以 DataList 或转发器..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: f52c302a-1b7c-46fe-8a13-8412c95cbf6d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: cfd0cdb0afe3bf71686715c0b1891adfbbd5019a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="sorting-data-in-a-datalist-or-repeater-control-c"></a>DataList 或转发器控件 (C#) 中对数据进行排序
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_CS.exe)或[下载 PDF](sorting-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial45cs1.pdf)

> 在本教程中我们将查看如何包含排序 DataList 和转发器中的支持，以及如何构造 DataList 或其数据可以分页和排序的转发器。


## <a name="introduction"></a>介绍

在[以前一教程](paging-report-data-in-a-datalist-or-repeater-control-cs.md)，我们探讨了如何向 DataList 添加分页支持。 我们创建中的新方法`ProductsBLL`类 (`GetProductsAsPagedDataSource`) 返回`PagedDataSource`对象。 绑定到 DataList 或转发器时，DataList 或转发器将显示只请求的数据页。 此方法十分类似于什么用于内部由 GridView、 说明如何和 FormView 控件提供其内置的默认分页功能。

除了产品/服务的分页支持，GridView 还包括现成排序支持。 DataList 和转发器都不提供内置的排序功能;但是，进行少量的代码添加排序功能。 在本教程中我们将查看如何包含排序 DataList 和转发器中的支持，以及如何构造 DataList 或其数据可以分页和排序的转发器。

## <a name="a-review-of-sorting"></a>排序检查

正如我们看到在[分页和排序报表数据](../paging-and-sorting/paging-and-sorting-report-data-cs.md)教程中，提供排序支持现成的 GridView 控件。 每个 GridView 字段只能具有一个关联`SortExpression`，表示对数据进行排序所依据的数据字段。 当 GridView s`AllowSorting`属性设置为`true`，具有每个 GridView 字段`SortExpression`属性值已呈现为 LinkButton 其标头。 当用户单击特定的 GridView 字段 s 标头时，产生的回发而的数据排序根据单击字段的`SortExpression`。

GridView 控件具有`SortExpression`属性，它存储`SortExpression`的 GridView 字段的数据按排序。 此外，`SortDirection`属性指示数据是否按升序或降序 （如果用户单击切换特定 GridView 字段 s 标头中的链接，两次连续的排序顺序） 排序。

当 GridView 绑定到其数据源控件时，将传递其`SortExpression`和`SortDirection`属性设置为数据源控件。 数据源控件中检索数据，然后对它进行排序根据所提供`SortExpression`和`SortDirection`属性。 排序后的数据，数据源控件将其返回给 GridView。

若要复制 DataList 或转发器控制此功能，我们必须：

- 创建排序接口
- 请记住要作为排序依据的数据字段，以及是否按升序或降序进行排序
- 指示 ObjectDataSource 按特定的数据字段的数据进行排序

例如，我们将介绍这些步骤 3 和 4 中的三个任务。 接下来，我们将研究如何包括分页和排序中 DataList 或转发器的支持。

## <a name="step-2-displaying-the-products-in-a-repeater"></a>步骤 2： 在中继器中显示的产品

我们担心实现任何与排序相关的功能之前，让我们来开始通过列出的转发器控件中的产品。 首先打开`Sorting.aspx`页面`PagingSortingDataListRepeater`文件夹。 将重复器控件添加到 web 页上，设置其`ID`属性`SortableProducts`。 从转发器 s 智能标记，创建名为新 ObjectDataSource`ProductsDataSource`并将其从中检索数据配置`ProductsBLL`类的`GetProducts()`方法。 （无） 从下拉列表中插入、 更新和删除选项卡中选项的选择。


[![创建 ObjectDataSource 并将其配置为使用 GetProductsAsPagedDataSource() 方法](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**图 1**： 创建对象数据源并将它配置为使用`GetProductsAsPagedDataSource()`方法 ([单击以查看实际尺寸的图像](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image3.png))


[![在更新中，INSERT、 设置下拉列表将列出和删除选项卡添加到 （无）](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image4.png)

**图 2**： 在更新中，INSERT、 设置下拉列表将列出和删除选项卡添加到 （无） ([单击以查看实际尺寸的图像](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image6.png))


与不同 DataList，与 Visual Studio 不会自动创建`ItemTemplate`转发器控件后将其绑定到数据源。 此外，我们必须添加此`ItemTemplate`以声明方式，如中继器控件 s 智能标记缺少 DataList s 中找到编辑模板选项。 允许 s 使用相同`ItemTemplate`从前面的教程，其中显示产品的名称、 供应商和类别。

在添加后`ItemTemplate`，转发器和 ObjectDataSource s 声明性标记的外观应与以下类似：


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample1.aspx)]

图 3 显示此页查看通过浏览器时。


[![显示每个产品的名称、 供应商和类别](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**图 3**： 显示每个产品的名称、 供应商和类别 ([单击以查看实际尺寸的图像](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>步骤 3： 指示 ObjectDataSource 对数据进行排序

若要排序的转发器中显示的数据，我们需要告知对数据进行排序所依据的排序表达式的对象数据源。 对象数据源中检索其数据之前，首先激发其[`Selecting`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx)，这使我们需要指定排序表达式有机会。 `Selecting`事件处理程序传递的类型对象[ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx)，其中有一个名为[ `Arguments` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx)类型的[ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx)。 `DataSourceSelectArguments`类旨在将与数据相关的请求的数据使用者从传递给数据源控件，并且包括[`SortExpression`属性](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx)。

若要将从 ASP.NET 页的排序信息传递给 ObjectDataSource，创建的事件处理程序`Selecting`事件并使用下面的代码：


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

*SortExpression*值应分配要通过 （如 ProductName) 对数据进行排序的数据字段的名称。 没有任何排序方向相关的属性，因此如果你想要按降序对数据进行排序，将字符串追加 DESC 到*sortExpression* （如 ProductName DESC) 的值。

请继续并尝试的一些不同的硬编码值*sortExpression*和浏览器中测试的结果。 如图 4 所示，当使用作为 ProductName DESC *sortExpression*，按其名称按反向字母顺序排序的产品。


[![按其名称按反向字母顺序排序的产品](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**图 4**： 按其名称按反向字母顺序排序的产品 ([单击以查看实际尺寸的图像](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>步骤 4： 创建排序的接口，然后记住的排序表达式和方向

开启排序 GridView 中的支持将每个可排序字段 s 标头文本转换为 LinkButton，单击时，请相应地对数据。 此类的排序接口合理的 GridView，其中其整齐布局数据列中。 对于 DataList 和转发器控件中，但是，不同的排序接口需要。 一个通用的排序界面执行的数据 （而不是数据网格中），列表是提供数据可以排序所依据的字段的下拉列表。 允许 s 出于本教程中实现此类接口。

将上面的 DropDownList Web 控件添加`SortableProducts`中继器并设置其`ID`属性`SortBy`。 从属性窗口中，单击省略号以`Items`属性打开 ListItem 集合编辑器。 添加`ListItem`s 的数据进行排序`ProductName`， `CategoryName`，和`SupplierName`字段。 此外添加`ListItem`要按其名称按反向字母顺序排序的产品。

`ListItem` `Text`属性可以设置为任何值 （如名称），但`Value`属性必须设置为 （如 ProductName) 的数据字段的名称。 若要按降序对结果进行排序，请将字符串 DESC 追加到 ProductName DESC 类似的数据字段名称。


![为每个可排序数据字段添加 ListItem](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**图 5**： 添加`ListItem`为每个可排序数据字段


最后，右侧的下拉列表中添加按钮 Web 控件。 设置其`ID`到`RefreshRepeater`及其`Text`刷新属性。

在创建后`ListItem`s 并添加刷新按钮，DropDownList 和按钮 s 声明性语法外观应与以下类似：


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

与完整的排序 DropDownList，我们接下来需要更新 ObjectDataSource s`Selecting`事件处理程序，使其使用所选`SortBy``ListItem`s`Value`而不是硬编码排序表达式的属性。


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample4.cs)]

在第一次访问页时此点产品将最初按进行排序`ProductName`数据字段，因为它 s `SortBy` `ListItem`默认情况下选择 （请参阅图 6）。 选择不同的排序如类别的选项和单击刷新将导致回发，并重新对数据进行排序的类别名称，如图 7 所示。


[![产品是最初按其名称排序](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)

**图 6**: 产品是最初按其名称 ([单击以查看实际尺寸的图像](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image16.png))


[![产品是现在按类别进行排序](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)

**图 7**: 产品不现在按类别进行排序 ([单击以查看实际尺寸的图像](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image19.png))


> [!NOTE]
> 单击刷新按钮会导致自动重新排序，因为需要已禁用的转发器的视图状态，从而导致转发器重新绑定到其上每次回发的数据源的数据。 如果你已留更改排序的转发器的视图状态启用，下拉列表获胜 t 排序顺序产生任何影响。 若要解决此问题，创建的事件处理程序的刷新按钮 s`Click`事件和重新绑定到其数据源中继器 (通过调用中继器的`DataBind()`方法)。


## <a name="remembering-the-sort-expression-and-direction"></a>记住的排序表达式和方向

在页面上相关联的非排序可能会发生回发，创建可排序 DataList 或转发器时它 s 命令性，跨回发记住的排序表达式和方向。 例如，假设我们在本教程以包括与每个产品的删除按钮中更新转发器。 当用户单击删除按钮 d 我们运行一些代码以删除所选的产品，然后重新绑定数据到转发器。 如果回发不会持续排序详细信息，在屏幕上显示的数据将恢复为原始的排序顺序。

对于本教程，DropDownList 隐式将保存排序表达式和方向在其视图状态为我们。 如果我们已使用不同的排序接口一个，即 LinkButtons 提供各种排序选项 d 我们需要小心之间回发保留排序顺序。 这可通过将排序参数存储在页面的视图状态，方法是包括排序参数，在查询字符串，或通过其他状态持久性方法。

在本教程将来示例探索如何保留页面的视图状态中的排序详细信息。

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>步骤 5： 添加到使用默认分页 DataList 排序支持

在[前面教程](paging-report-data-in-a-datalist-or-repeater-control-cs.md)我们学习了如何实现使用 DataList 默认分页。 让我们来扩展此前面的示例包括分页的数据进行排序的功能。 首先打开`SortingWithDefaultPaging.aspx`和`Paging.aspx`页`PagingSortingDataListRepeater`文件夹。 从`Paging.aspx`页上，单击源按钮以查看页面 s 声明性标记。 复制选定的文本 （请参阅图 8） 将其粘贴到的声明性标记`SortingWithDefaultPaging.aspx`之间`<asp:Content>`标记。


[![复制中的声明性标记&lt;asp： 内容&gt;SortingWithDefaultPaging.aspx 从 Paging.aspx 标记](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)

**图 8**： 复制中的声明性标记`<asp:Content>`标记从`Paging.aspx`到`SortingWithDefaultPaging.aspx`([单击以查看实际尺寸的图像](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image22.png))


复制后的声明性的标记，将复制的方法和属性中的`Paging.aspx`页上的代码隐藏类的代码隐藏类`SortingWithDefaultPaging.aspx`。 接下来，花些时间查看`SortingWithDefaultPaging.aspx`在浏览器中的页。 它应显示相同的功能和外观作为`Paging.aspx`。

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>增强 ProductsBLL 包括默认分页和排序方法

在以前的教程，我们创建了`GetProductsAsPagedDataSource(pageIndex, pageSize)`中的方法`ProductsBLL`类返回`PagedDataSource`对象。 这`PagedDataSource`对象已填入*所有*的产品 (通过 BLL s`GetProducts()`方法)，但对应于指定的那些记录时绑定到 DataList *pageIndex*和*pageSize*显示输入的参数。

我们可以在本教程前面来添加通过指定排序表达式从 ObjectDataSource s 排序支持`Selecting`事件处理程序。 这可以有效地工作 ObjectDataSource 时返回一个对象，可以进行排序，如`ProductsDataTable`返回`GetProducts()`方法。 但是，`PagedDataSource`返回对象`GetProductsAsPagedDataSource`方法不支持其内部数据源的排序。 相反，我们需要从返回对结果进行排序`GetProducts()`方法*之前*我们将其放入`PagedDataSource`。

若要实现此目的，创建中的新方法`ProductsBLL`类， `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`。 若要进行排序`ProductsDataTable`返回`GetProducts()`方法，指定`Sort`其默认属性`DataTableView`:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

`GetProductsSortedAsPagedDataSource`方法只会稍有不同从`GetProductsAsPagedDataSource`在前面的教程中创建的方法。 具体而言，`GetProductsSortedAsPagedDataSource`接受其他输入的参数`sortExpression`并将此值赋给`Sort`属性`ProductDataTable`s `DefaultView`。 几行代码更高版本，`PagedDataSource`对象 s 数据源分配`ProductDataTable`s `DefaultView`。

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>调用 GetProductsSortedAsPagedDataSource 方法并指定 SortExpression 输入参数的值

与`GetProductsSortedAsPagedDataSource`方法完成下, 一步是为此参数提供值。 在 ObjectDataSource`SortingWithDefaultPaging.aspx`当前被配置为调用`GetProductsAsPagedDataSource`方法，并通过其两个的两个输入参数传入`QueryStringParameters`，均`SelectParameters`集合。 这两个`QueryStringParameters`指示的源`GetProductsAsPagedDataSource`方法 s *pageIndex*和*pageSize*参数来自 querystring 字段`pageIndex`和`pageSize`。

更新 ObjectDataSource s`SelectMethod`属性，以便它调用新`GetProductsSortedAsPagedDataSource`方法。 然后，添加一个新`QueryStringParameter`以便*sortExpression*从 querystring 字段访问输入的参数`sortExpression`。 设置`QueryStringParameter`s`DefaultValue`为产品名称。

这些更改后的 ObjectDataSource s 声明性标记应如下所示：


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample6.aspx)]

此时，`SortingWithDefaultPaging.aspx`页将按字母顺序排序其结果，按产品名称 （请参阅图 9）。 这是因为，默认情况下，值为产品名称中作为传递`GetProductsSortedAsPagedDataSource`方法 s *sortExpression*参数。


[![默认情况下，结果按产品名称排序](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)

**图 9**： 通过默认情况下，对结果进行排序的`ProductName`([单击以查看实际尺寸的图像](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image25.png))


如果你手动添加`sortExpression`querystring 字段，如`SortingWithDefaultPaging.aspx?sortExpression=CategoryName`结果将排序由指定`sortExpression`。 但是，这`sortExpression`参数未包含在查询字符串，将移至另一页数据时。 事实上，在下一步或上一次页上单击按钮的采用我们回`Paging.aspx`！ 此外，在该处 s 当前未排序的接口。 用户可以更改分页的数据的排序顺序的唯一方法是通过直接操作查询字符串。

## <a name="creating-the-sorting-interface"></a>创建排序接口

我们首先需要更新`RedirectUser`方法将向用户发送`SortingWithDefaultPaging.aspx`(而不是`Paging.aspx`)，它包括`sortExpression`中查询字符串值。 我们还应添加只读的页级名为`SortExpression`属性。 此属性，类似于`PageIndex`和`PageSize`在前面的教程中, 创建的属性返回的值`sortExpression`querystring 字段如果它存在，并且默认值 （产品名称)，否则。

当前`RedirectUser`方法接受仅单个输入的参数，显示的页的索引。 但是，可能有的时候我们想要将用户重定向到特定页使用查询字符串中指定的新增功能以外的排序表达式的数据。 在稍后我们将创建此页上，将包括一系列按钮 Web 控件的数据进行排序的指定列的排序接口。 单击这些按钮之一时，我们想要传递相应的排序表达式值中将用户重定向。 若要提供此功能，创建两个版本的`RedirectUser`方法。 第一个应接受只页索引显示，而第二个接受的页索引和排序表达式。


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

在本教程中的第一个示例，我们创建使用 DropDownList 排序接口。 对于此示例中，让 s 使用位于上方 DataList 一个排序的三个按钮 Web 控件`ProductName`、 一个为`CategoryName`，和一个用于`SupplierName`。 添加三个按钮 Web 控件，设置其`ID`和`Text`属性相应地：


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample8.aspx)]

接下来，创建`Click`每个事件处理程序。 事件处理程序应调用`RedirectUser`方法，将用户返回到使用适当的排序表达式的第一页。


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

当第一次访问该页面，对数据进行排序按产品名称按字母顺序 （回头参考图 9 中）。 单击下一步按钮以前进到数据的第二个页面，然后单击类别按钮的排序。 这将我们返回到按类别名称排序的数据的第一页 （请参阅图 10）。 同样，供应商按钮单击排序对从数据的第一页开始的供应商的数据进行排序。 因为通过分页的数据，将记住排序选项。 图 11 显示后按类别排序，然后然后转到数据的第 13 个页的页。


[![按类别排序的产品](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)

**图 10**： 按类别排序的产品 ([单击以查看实际尺寸的图像](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image28.png))


[![排序表达式是记住分页通过数据时](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image29.png)

**图 11**: 排序表达式是记住分页通过数据时 ([单击以查看实际尺寸的图像](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>步骤 6： 通过中继器中记录的自定义分页

DataList 示例检查在步骤中，通过使用效率低下的默认分页技术其数据的 5 个页面。 当足够大，大量数据进行分页时, 是命令性，使用自定义分页。 返回[高效地分页通过大型金额的数据](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md)和[排序自定义分页数据](../paging-and-sorting/sorting-custom-paged-data-cs.md)教程，我们将检查中为 BLL 默认和自定义分页和创建的方法的区别利用自定义分页和排序自定义分页的数据。 具体而言，在这些两个前面的教程，我们添加到以下三种方法`ProductsBLL`类：

- `GetProductsPaged(startRowIndex, maximumRows)`返回开始记录的特定子集*值*和不超过*值*。
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)`返回按指定的记录的特定子集*sortExpression*输入的参数。
- `TotalNumberOfProducts()`提供了中的记录的总数`Products`数据库表。

这些方法可以用于有效地页和排序通过使用 DataList 或转发器控件的数据。 为了说明这一点，让我们来开始通过使用自定义分页支持; 创建重复器控件然后，我们将添加排序功能。

打开`SortingWithCustomPaging.aspx`页面`PagingSortingDataListRepeater`文件夹并将转发器添加到页上，设置其`ID`属性`Products`。 从转发器 s 智能标记，创建名为新 ObjectDataSource `ProductsDataSource`。 将其配置为选择从其数据`ProductsBLL`类的`GetProductsPaged`方法。


[![配置对象数据源以使用 ProductsBLL 类的 GetProductsPaged 方法](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image32.png)

**图 12**： 配置使用 ObjectDataSource`ProductsBLL`类 s`GetProductsPaged`方法 ([单击以查看实际尺寸的图像](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image34.png))


在更新中，INSERT、 设置的下拉列表并删除选项卡添加到 （无），然后单击下一步按钮。 配置数据源向导现在将提示提供的源`GetProductsPaged`方法 s*值*和*值*输入参数。 在现实中，将忽略这些输入的参数。 相反，*值*和*值*值将通过传递在`Arguments`中 ObjectDataSource 的属性`Selecting`事件处理程序，就像我们指定的方式*sortExpression*在此教程 s 第一个演示。 因此，将参数源保留在无设置向导中的下拉列表。


[![将参数源设置保留为无](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image35.png)

**图 13**： 将保留为无参数的源设置 ([单击以查看实际尺寸的图像](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image37.png))


> [!NOTE]
> 执行*不*设置 ObjectDataSource s`EnablePaging`属性`true`。 这将导致对象数据源，将自动包含其自己*值*和*值*参数`SelectMethod`s 现有参数列表。 `EnablePaging`绑定自定义分页到 GridView，说明如何或 FormView 控件的数据，因为这些控件需要从 ObjectDataSource s 某些行为时有用属性只能在`EnablePaging`属性是`true`。 由于我们必须手动添加对 DataList 和转发器的分页支持，使此属性设置为`false`（默认值），因为我们将在所需的功能直接在我们的 ASP.NET 页面内烘烤糕点。


最后，可以定义中继器的`ItemTemplate`以便显示产品的名称、 类别和供应商。 这些更改后的转发器和 ObjectDataSource s 声明性语法应看起来类似以下：


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample10.aspx)]

花一些时间，请访问通过浏览器页面，并记下会返回任何记录。 这是因为我们已有待指定*值*和*值*参数值; 因此，为 0 的值正在传递两个。 若要指定这些值，请创建一个事件处理程序 ObjectDataSource 的`Selecting`事件和设置这些参数值以编程方式为硬编码值为 0，5，分别：


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample11.cs)]

进行此更改后，该页面，通过浏览器，查看时显示的前五个产品。


[![显示第一个 5 个记录](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image38.png)

**图 14**： 显示第一个 5 个记录 ([单击以查看实际尺寸的图像](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image40.png))


> [!NOTE]
> 图 14 中列出的产品发生这种情况要按产品名称排序，因为`GetProductsPaged`执行高效的自定义分页查询的存储的过程通过对结果进行排序`ProductName`。


为了允许用户浏览所有页面，我们需要跟踪开始的行索引和最大行数的并跨回发记住这些值。 在默认的分页示例中我们使用，用于查询字符串字段保留这些值;对于本演示，让我们来将此页面的视图状态信息保留。 创建以下两个属性：


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample12.cs)]

接下来，更新选择事件处理程序中的代码，以便它使用`StartRowIndex`和`MaximumRows`而不是 0 和 5 的硬编码值的属性：


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample13.cs)]

此时我们的页面仍将显示的前五个记录为止。 但是，对于就地情况下，这些属性，我们重新已准备好创建我们分页的接口。

## <a name="adding-the-paging-interface"></a>添加分页接口

让的 s 使用相同的第一个、 上一步、 下一步，上次分页接口用于默认分页示例，包括在查看显示哪些页数据的控件标签 Web 和存在多少总页数。 添加四个按钮 Web 控件和转发器下方的标签。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample14.aspx)]

接下来，创建`Click`的四个按钮的事件处理程序。 单击这些按钮之一时，我们需要更新`StartRowIndex`和重新绑定数据到转发器。 第一个、 上一步，和下一步按钮的代码非常简单，但对于最后一个按钮如何执行我们确定数据的最后一页的起始行索引？ 若要计算此索引以及能够确定是否应启用下一步和最后一个按钮我们需要了解总共多少条记录通过正在通过寻呼发送。 我们可以通过调用确定`ProductsBLL`类的`TotalNumberOfProducts()`方法。 允许 s 创建名为的只读的、 页级属性`TotalRowCount`返回的结果`TotalNumberOfProducts()`方法：


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample15.cs)]

使用此属性，我们现在可以确定最后一个页面的开始行索引。 具体而言，它 s 的整数结果的`TotalRowCount`减 1 除以`MaximumRows`，再乘以`MaximumRows`。 现在，我们可以编写`Click`四个分页接口按钮的事件处理程序：


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample16.cs)]

最后，我们需要查看的最后一页时查看数据和下一步和最后一个按钮的第一页时禁用分页接口中的第一个和上一步按钮。 若要完成此操作，请将以下代码添加到 ObjectDataSource 的`Selecting`事件处理程序：


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample17.cs)]

添加这些后`Click`事件处理程序和代码以启用或禁用基于当前的起始行索引，分页界面元素在浏览器中测试页面。 图 15 所示，在第一次访问页第一个时以及上一步按钮将处于禁用状态。 单击下一步显示的数据，第二页，而单击上一次显示的最后一页 （请参阅图 16 和 17）。 查看数据的最后一页时的下一步和最后一个按钮被禁用。


[![当查看第一个产品页时，会禁用上一步和最后一个按钮](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image41.png)

**图 15**： 查看第一个产品页时，将禁用 Previous 和最后一个按钮 ([单击以查看实际尺寸的图像](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image43.png))


[![第二个产品页是全都](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image44.png)

**图 16**： 产品第二个页是全都 ([单击以查看实际尺寸的图像](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image46.png))


[![单击最后一个显示的数据的最后一页](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image47.png)

**图 17**： 单击上一次显示的最后一个数据页 ([单击以查看实际尺寸的图像](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>步骤 7： 包括排序使用的自定义支持分页转发器

现在，已实现自定义分页，我们已准备好包括排序重新支持。 `ProductsBLL`类 s`GetProductsPagedAndSorted`方法具有相同*值*和*值*输入参数作为`GetProductsPaged`，但它允许其他*sortExpression*输入的参数。 若要使用`GetProductsPagedAndSorted`方法从`SortingWithCustomPaging.aspx`，我们需要执行以下步骤：

1. 更改 ObjectDataSource s`SelectMethod`属性从`GetProductsPaged`到`GetProductsPagedAndSorted`。
2. 添加*sortExpression* `Parameter`对象到 ObjectDataSource 的`SelectParameters`集合。
3. 创建专用的页级别`SortExpression`属性保留其值在回发期间通过页面的视图状态。
4. 更新 ObjectDataSource s`Selecting`事件处理程序来分配 ObjectDataSource s *sortExpression*参数页级别的值`SortExpression`属性。
5. 创建排序的接口。

通过更新 ObjectDataSource s 启动`SelectMethod`属性并添加*sortExpression* `Parameter`。 请确保*sortExpression* `Parameter` s`Type`属性设置为`String`。 完成这些前两个任务之后，ObjectDataSource s 声明性标记应如下所示：


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample18.aspx)]

接下来，我们需要页级`SortExpression`其值序列化为视图状态的属性。 如果未不设置任何排序表达式值，将 ProductName 用作默认值：


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample19.cs)]

ObjectDataSource 调用之前`GetProductsPagedAndSorted`方法我们需要将设置*sortExpression* `Parameter`为的值`SortExpression`属性。 在`Selecting`事件处理程序中，添加以下代码行：


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample20.cs)]

所有这些剩下是实现排序接口。 正如我们做最后一个示例中，可让具有排序接口使用允许用户对结果进行排序的三个按钮 Web 控件实现按产品名称、 类别或供应商的 s。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample21.aspx)]

创建`Click`这三个按钮控件的事件处理程序。 在事件处理程序中，重置`StartRowIndex`为 0，设置`SortExpression`到适当的值，并重新绑定到转发器的数据：


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample22.cs)]

该 s 就是它 ！ 时有多个步骤，若要获取自定义分页和排序实现，已针对那些需要默认分页非常相似的步骤。 查看数据时按类别排序的最后一页时，图 18 显示产品。


[![已显示最后一个数据页、 按类别、 Sorted](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image50.png)

**图 18**： 已显示最后一页数据的、 按类别、 Sorted ([单击以查看实际尺寸的图像](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image52.png))


> [!NOTE]
> 在上一示例中，供应商供应商名称用作排序表达式排序时。 但是，对于自定义分页实现中，我们需要使用 CompanyName。 这是因为存储的过程负责实现自定义分页`GetProductsPagedAndSorted`将传递到排序表达式`ROW_NUMBER()`关键字，`ROW_NUMBER()`关键字需要为实际列名，而不是别名。 因此，我们必须使用`CompanyName`(中的列的名称`Suppliers`表) 而不是中使用的别名`SELECT`查询 (`SupplierName`) 的排序表达式。


## <a name="summary"></a>摘要

既不是 DataList 还是中继器提供排序的内置支持，但可以进行少量的代码和自定义排序界面，添加这样的功能。 在实现排序，但不是分页时，可通过指定的排序表达式`DataSourceSelectArguments`对象传递给 ObjectDataSource 的`Select`方法。 这`DataSourceSelectArguments`对象 s`SortExpression`属性可以分配中 ObjectDataSource 的`Selecting`事件处理程序。

若要将排序功能添加到 DataList 或转发器已提供分页支持，最简单的方法是自定义业务逻辑层，以囊括接受排序表达式的方法。 此信息然后中通过 ObjectDataSource s 中的参数传递`SelectParameters`。

本教程中完成分页和排序 DataList 和转发器控件与我们的检查。 我们的下一步和最后一个教程将说明如何将按钮 Web 控件添加到 DataList 和转发器的 s 模板，以便在每个项基础上提供自定义、 用户启动功能。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 David Suru。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](paging-report-data-in-a-datalist-or-repeater-control-cs.md)
[下一页](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
