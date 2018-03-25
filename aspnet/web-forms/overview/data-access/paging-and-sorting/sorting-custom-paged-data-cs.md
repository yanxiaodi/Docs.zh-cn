---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
title: "排序自定义分页数据 (C#) |Microsoft 文档"
author: rick-anderson
description: "在以前的教程，我们学习了如何实现自定义分页时 presentating 在网页上的数据。 在本教程中我们将了解如何以扩展前面..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 778baa4e-4af8-4665-947e-7a01d1a4dff2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
msc.type: authoredcontent
ms.openlocfilehash: a71405bc84304bf7c47f400dfa9886208316d223
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="sorting-custom-paged-data-c"></a>排序自定义分页数据 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_CS.exe)或[下载 PDF](sorting-custom-paged-data-cs/_static/datatutorial26cs1.pdf)

> 在以前的教程，我们学习了如何实现自定义分页时 presentating 在网页上的数据。 在本教程中我们将了解如何以扩展前面的示例，用以排序自定义分页的支持。


## <a name="introduction"></a>介绍

与默认分页，请比较自定义分页可以通过几个数量级，使自定义分页事实上分页实现选项，当大量的数据进行分页时提高性能的数据进行分页。 实现自定义分页是更多地涉及而不是实现默认的分页，但是，尤其是在添加到组合排序。 在本教程中，我们将扩展的示例中前一次用以支持排序*和*自定义分页。

> [!NOTE]
> 由于本教程基于前一次，然后开始才一段时间来复制中的声明性语法`<asp:Content>`从前面的教程 s web 页的元素 (`EfficientPaging.aspx`) 并将其之间粘贴`<asp:Content>`中元素`SortParameter.aspx`页。 回头参考的步骤 1 中[将验证控件添加到的编辑和插入接口](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)教程，以复制到另一个一个 ASP.NET 页的功能的更多详细讨论。


## <a name="step-1-reexamining-the-custom-paging-technique"></a>步骤 1： 重新检查的自定义分页技术

若要正常工作的自定义分页，我们必须实现某些技术，它可以有效地获取给定的开始的行索引和最大行数参数的记录的特定子集。 有几种可用来实现此目标的技术。 在前面的教程，我们实现此操作查看使用 Microsoft SQL Server 2005 s 新`ROW_NUMBER()`排名函数。 简单地说，`ROW_NUMBER()`排名函数将按指定的排序顺序排名的查询返回的每一行分配一个行号。 然后通过返回的已编号的结果的特定部分，获取记录的相应的子集。 下面的查询演示如何使用此方法返回时排名结果按字母顺序排列的编号 11 至第 20 这些产品`ProductName`:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample1.sql)]

此方法非常适用于使用特定的排序顺序的分页 (`ProductName`按字母顺序排列，这种情况下)，但查询需要修改可显示按不同的排序表达式的结果。 理想情况下，上述查询可重写用于中的参数`OVER`子句，如下所示：


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample2.sql)]

遗憾的是，参数化`ORDER BY`子句不允许。 相反，我们必须创建存储的过程接受的`@sortExpression`输入的参数，但使用同一种的下列解决方法：

- 为每个可能使用; 的排序表达式编写硬编码的查询然后，使用`IF/ELSE`T-SQL 的语句，以确定要执行的查询。
- 使用`CASE`语句，以提供动态`ORDER BY`表达式基于`@sortExpressio`n 输入参数，请参阅动态排序查询结果部分中使用[的 SQL Power`CASE`语句](http://www.4guysfromrolla.com/webtech/102704-1.shtml)有关详细信息。
- 作为字符串存储过程中创建相应的查询，然后使用[`sp_executesql`系统存储过程](https://msdn.microsoft.com/library/ms188001.aspx)执行动态查询。

每个这些解决方法存在一些缺点。 第一个选项不是作为与其他两个因为它需要你创建的每个可能的排序表达式的查询为容易维护的。 因此，如果以后决定要将新的、 可排序的字段添加到 GridView 你还需要返回并更新存储的过程。 第二种方法都有一些细微部分按非字符串数据库列进行排序时产生性能问题，还会受到与第一个相同的可维护性问题。 和第三个选项，使用动态 SQL，引入了 SQL 注入攻击的风险，如果攻击者能够执行存储的过程在其选择的输入的参数值传递。

虽然这些方法不完美，我认为第三个选项是最佳的三个。 对它的动态 SQL 的使用，它提供其他两个不这样做的灵活性的级别。 此外，仅如果攻击者能够执行存储的过程在其选择的输入参数中传递时，只有可以利用 SQL 注入攻击。 由于 DAL 使用参数化的查询，ADO.NET 将保护发送到数据库中通过体系结构，这些参数，这意味着如果攻击者可以直接执行存储的过程，仅存在 SQL 注入攻击漏洞。

若要实现此功能，在名为 Northwind 数据库中创建新的存储的过程`GetProductsPagedAndSorted`。 此存储的过程应接受三个输入参数： `@sortExpression`，输入的参数的类型`nvarchar(100`)，用于指定如何结果应进行排序和后直接注入`ORDER BY`中文`OVER`子句; 和`@startRowIndex`和`@maximumRows`，从相同的两个整数输入的参数`GetProductsPaged`存储检查在前面的教程的过程。 创建`GetProductsPagedAndSorted`存储过程使用以下脚本：


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample3.sql)]

通过确保的值来启动存储的过程`@sortExpression`指定参数。 如果缺少，结果按顺序排列的`ProductID`。 接下来，将构造动态的 SQL 查询。 请注意动态的 SQL 查询从我们以前用于从 Products 表中检索所有行的查询会稍有不同。 在前面的示例，我们获得每个产品 s 关联的类别使用子查询的 s 和供应商的名称。 返回做出此决定是[创建数据访问层](../introduction/creating-a-data-access-layer-cs.md)教程和已完成而非使用`JOIN`s 因为 TableAdapter 无法自动创建关联的插入、 更新和删除方法对这些产品查询。 `GetProductsPagedAndSorted`存储的过程，但是，必须使用`JOIN`s，结果才会按类别或供应商名称进行排序。

此动态查询通过串联静态查询部分构建和`@sortExpression`， `@startRowIndex`，和`@maximumRows`参数。 由于`@startRowIndex`和`@maximumRows`整数参数，它们必须转换为 nvarchars 为了正确连接。 一旦构造此动态 SQL 查询后，执行通过`sp_executesql`。

花一些时间来测试不同的值与此存储的过程`@sortExpression`， `@startRowIndex`，和`@maximumRows`参数。 从服务器资源管理器，右键单击存储的过程名称并选择执行。 此时会弹出运行存储过程对话框中，可以在其中输入 （请参见图 1） 的输入的参数。 若要按类别名称对结果进行排序，请使用有关 CategoryName`@sortExpression`参数值; 若要按供应商的公司名称进行排序，请使用公司名称。 提供的参数值后, 单击确定。 结果将显示在输出窗口中。 图 2 显示的结果，通过排序时，返回产品排名 11 至第 20 后`UnitPrice`降序排序。


![为存储的过程 s 三个输入参数尝试不同的值](sorting-custom-paged-data-cs/_static/image1.png)

**图 1**： 为存储的过程 s 三个输入参数，请尝试不同的值


[![存储过程的结果将显示在输出窗口](sorting-custom-paged-data-cs/_static/image3.png)](sorting-custom-paged-data-cs/_static/image2.png)

**图 2**: 存储过程的结果将显示在输出窗口中 ([单击以查看实际尺寸的图像](sorting-custom-paged-data-cs/_static/image4.png))


> [!NOTE]
> 当指定排名结果`ORDER BY`中的列`OVER`子句，SQL Server 必须将对结果进行排序。 这是快速操作，如果通过对结果进行由正在排序的列没有聚集的索引，或者如果没有覆盖索引，但会否则成本较高。 若要提高足够大，查询的性能，请考虑添加按对结果排序所依据的列的非聚集索引。 请参阅[排名函数和 SQL Server 2005 中的性能](http://www.sql-server-performance.com/ak_ranking_functions.asp)有关详细信息。


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>步骤 2： 补充数据访问层和业务逻辑层

与`GetProductsPagedAndSorted`创建的存储的过程，我们的下一步是提供一种方式来执行该存储的过程通过我们的应用程序体系结构。 这需要将适当的方法添加到的 DAL 和 BLL。 让我们来开始与 DAL 添加方法。 打开`Northwind.xsd`类型化数据集，右键单击`ProductsTableAdapter`，然后从上下文菜单中选择添加查询选项。 正如我们做在前面的教程，我们想要配置要使用现有存储的过程的此新 DAL 方法`GetProductsPagedAndSorted`，在这种情况下。 首先，该值指示你想要使用现有存储的过程的新的 TableAdapter 方法。


![选择使用现有存储的过程](sorting-custom-paged-data-cs/_static/image5.png)

**图 3**： 选择使用现有存储的过程


若要指定要使用的存储的过程，请选择`GetProductsPagedAndSorted`存储过程从下拉列表中下一个屏幕。


![使用 GetProductsPagedAndSorted 存储过程](sorting-custom-paged-data-cs/_static/image6.png)

**图 4**： 使用 GetProductsPagedAndSorted 存储过程


此存储的过程返回一组记录，因为其结果是，在下一步的屏幕中，指示它返回表格数据。


![指示存储的过程返回表格数据](sorting-custom-paged-data-cs/_static/image7.png)

**图 5**： 指示存储的过程返回表格数据


最后，创建一个数据表的使用这两个填充的 DAL 方法并返回 DataTable 模式，命名方法`FillPagedAndSorted`和`GetProductsPagedAndSorted`分别。


![选择的方法名称](sorting-custom-paged-data-cs/_static/image8.png)

**图 6**： 选择的方法名称


现在我们已扩展 DAL，我们已准备好变为 BLL 重新。 打开`ProductsBLL`类文件，并添加新方法， `GetProductsPagedAndSorted`。 此方法需要接受三个输入参数`sortExpression`， `startRowIndex`，和`maximumRows`只应调用到 DAL 的`GetProductsPagedAndSorted`方法，如下所示：


[!code-csharp[Main](sorting-custom-paged-data-cs/samples/sample4.cs)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>步骤 3： 配置 SortExpression 参数中传递 ObjectDataSource

要包括的方法，利用的 DAL 和 BLL 具有扩充`GetProductsPagedAndSorted`存储的过程，所有保持是配置中的 ObjectDataSource`SortParameter.aspx`页以使用新的 BLL 方法并传入`SortExpression`参数基于用户已请求的结果进行排序的列。

通过更改 ObjectDataSource s 启动`SelectMethod`从`GetProductsPaged`到`GetProductsPagedAndSorted`。 这可以通过配置数据源向导，从属性窗口中，或直接通过声明性语法。 接下来，我们需要提供一个值，ObjectDataSource s [ `SortParameterName`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx)。 如果设置此属性，尝试在 GridView 传递 ObjectDataSource`SortExpression`属性`SelectMethod`。 具体而言，ObjectDataSource 查找的输入参数，其名称是等于的值`SortParameterName`属性。 由于 BLL s`GetProductsPagedAndSorted`方法具有名为的排序表达式输入的参数`sortExpression`，设置 ObjectDataSource 的`SortExpression`sortExpression 属性。

进行这些两个更改后，ObjectDataSource s 声明性语法应类似于以下：


[!code-aspx[Main](sorting-custom-paged-data-cs/samples/sample5.aspx)]

> [!NOTE]
> 如前面的教程中，确保 ObjectDataSource*不*其 SelectParameters 集合中包括的 sortExpression、 值或值的输入的参数。


若要启用在 GridView 中排序，只需选中 GridView s 智能标记，它将设置 GridView s 中的启用排序复选框`AllowSorting`属性`true`，从而导致每个列以将呈现为 LinkButton 的页眉文本。 当最终用户单击其中一个标头 LinkButtons 时，回发时，才会和经过以下步骤：

1. GridView 更新其[`SortExpression`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx)为的值`SortExpression`被单击其标头链接的字段
2. ObjectDataSource 调用 BLL s`GetProductsPagedAndSorted`方法，同时传入 GridView s`SortExpression`方法 s 的值作为属性`sortExpression`输入的参数 (以及相应`startRowIndex`和`maximumRows`输入参数值)
3. BLL 调用 DAL 的`GetProductsPagedAndSorted`方法
4. DAL 执行`GetProductsPagedAndSorted`传递存储过程中，在`@sortExpression`参数 (连同`@startRowIndex`和`@maximumRows`输入参数值)
5. 存储的过程返回相应的数据子集为 BLL，将其返回给 ObjectDataSource;然后绑定到 GridView，转换为 html 代码，并向最终用户向下发送此数据

图 7 显示时按排序的结果的第一页`UnitPrice`升序排序。


[![对结果进行排序单价](sorting-custom-paged-data-cs/_static/image10.png)](sorting-custom-paged-data-cs/_static/image9.png)

**图 7**: 单价的结果进行排序 ([单击以查看实际尺寸的图像](sorting-custom-paged-data-cs/_static/image11.png))


时的当前实现可以正确排序按产品名称、 类别名称、 每个单元和单价数量的结果，请尝试对结果进行排序供应商名称结果在运行时异常 （请参阅图 8）。


![尝试通过以下运行时异常中的供应商结果对结果进行排序](sorting-custom-paged-data-cs/_static/image12.png)

**图 8**： 尝试通过以下运行时异常中的供应商结果对结果进行排序


发生此异常的原因`SortExpression`的 GridView s `SupplierName` BoundField 设置为`SupplierName`。 但是，供应商 s 中的名称`Suppliers`表实际上调用`CompanyName`我们已被使用别名作为此列的列名`SupplierName`。 但是，`OVER`子句使用`ROW_NUMBER()`函数不能使用别名，并且必须使用为实际列名。 因此，更改`SupplierName`BoundField 的`SortExpression`从 CompanyName 到供应商的名称 （请参阅图 9）。 如图 10 显示，此更改后的结果可以进行排序供应商。


![将供应商名称 BoundField 的 SortExpression 更改为 CompanyName](sorting-custom-paged-data-cs/_static/image13.png)

**图 9**： 将供应商名称 BoundField 的 SortExpression 更改为 CompanyName


[![现在可以按供应商排序结果](sorting-custom-paged-data-cs/_static/image15.png)](sorting-custom-paged-data-cs/_static/image14.png)

**图 10**: 结果可以现在按进行排序供应商 ([单击以查看实际尺寸的图像](sorting-custom-paged-data-cs/_static/image16.png))


## <a name="summary"></a>摘要

我们探讨了前面教程中的自定义分页实现所需在设计时指定结果打算进行排序所依据的顺序。 简单地说，这意味着，我们实现的自定义分页实现无法，同时，提供排序功能。 在本教程中我们可以克服此限制了通过扩展存储的过程从第一个包括`@sortExpression`结果无法排序所依据的输入的参数。

创建此存储过程和 DAL 和 BLL 中创建新的方法后，我们只需能够实现一个 GridView，提供这两个排序和自定义分页通过配置对象数据源以传入 GridView s 当前`SortExpression`BLL 属性`SelectMethod`.

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Carlos Santos。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](efficiently-paging-through-large-amounts-of-data-cs.md)
[下一页](creating-a-customized-sorting-user-interface-cs.md)
