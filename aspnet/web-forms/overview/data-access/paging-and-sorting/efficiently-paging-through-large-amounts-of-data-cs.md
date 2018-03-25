---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
title: "有效地分页大量的数据 (C#) |Microsoft 文档"
author: rick-anderson
description: "使用大量的数据，作为其基础数据源控件 retriev 时，数据的显示控件的默认分页选项不合适，则..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 59c01998-9326-4ecb-9392-cb9615962140
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
msc.type: authoredcontent
ms.openlocfilehash: ac16cc79f2ed6b62bf676553a374180cd0165632
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="efficiently-paging-through-large-amounts-of-data-c"></a>有效地分页大量的数据 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_CS.exe)或[下载 PDF](efficiently-paging-through-large-amounts-of-data-cs/_static/datatutorial25cs1.pdf)

> 数据呈现控件的默认分页选项不合适，则在使用大量的数据，因为其基础数据源控件检索所有记录，即使显示数据的一个子集。 在这种情况下，我们必须打开的自定义分页。


## <a name="introduction"></a>介绍

如我们在前面的教程所述，可以采用两种方式之一实现分页：

- **默认分页**可以通过只需选中启用分页选项中的数据的 Web 控件 s 智能标记; 但是，只要查看一页数据，ObjectDataSource 检索*所有*的记录，即使但在页中显示它们的子集
- **自定义分页**提高了默认的性能通过从需要为用户; 请求的数据的特定页显示的数据库中检索只有这些记录分页但是，自定义分页涉及有点更多工作来实现比默认分页

因为实现只检查一个复选框，并重新易于完成 ！ 默认分页是一个不错的选择。 其 na 遇到做法中检索的所有记录，不过，使 implausible 选择时与许多并发用户分页的数据或站点足够大的量。 在这种情况下，我们必须打开的自定义分页以便提供响应系统。

自定义分页的挑战在于能够编写返回精确的所需的数据的特定页的记录集的查询。 幸运的是，Microsoft SQL Server 2005 提供一个新的关键字为排名结果，这使我们能够编写可以高效地检索记录的正确子集的查询。 在本教程中我们将了解如何使用此新的 SQL Server 2005 关键字 GridView 控件中实现自定义分页。 自定义分页的用户界面时同样默认分页，请单步执行从一页下一步使用适用于自定义分页可以比默认分页是几个数量级。

> [!NOTE]
> 通过自定义分页表现出确切的性能提升取决于记录正在通过寻呼发送到和放置在数据库服务器上的负载的总数。 在本教程末尾中，我们将查看展示通过自定义分页获得的性能好处一些粗略度量值。


## <a name="step-1-understanding-the-custom-paging-process"></a>步骤 1： 了解自定义分页过程

当数据进行分页时, 在页上显示的精确记录取决于所请求的数据的页和每页显示的记录数。 例如，假设我们想要通过 81 产品页上显示每页包含 10 种产品。 查看时的第一页，d 我们希望产品 1 到 10;查看的第二页时 d 我们感兴趣产品 11 到 20，依次类推。

有规定需要检索的记录和分页接口的呈现方式的三个变量：

- **启动行索引**要显示的数据页中的第一行的索引; 通过页索引乘以每页显示的记录并添加一个计算此索引可以。 例如，如果通过记录 10 一次 （其页索引为 0） 的第一页的分页时, 启动行索引为 0 \* 10 + 1 或 1 个; 第二页 （其页索引为 1），启动行索引为 1 \* 10 + 1或 11。
- **最大行数**最大每页显示的记录数。 此变量称为最大行数，因为最后页有可能是较少的页大小比返回的记录。 例如，在查看 81 产品 10 记录每页的分页，第九个和最后一个页面将必须仅一条记录。 不过，任何页，将不显示更多记录比最大行数的值。
- **总计记录计数**总正在通过寻呼发送到的记录数。 确定要为给定页检索的记录所需的此变量不是 t，尽管它未规定分页接口。 例如，如果有正在通过寻呼发送通过 81 产品，分页接口知道在分页用户界面中显示九个页号。

使用默认分页启动行索引将计算为产品的页索引和页面大小加 1，而最大行数是只需的页大小。 由于默认分页检索所有从记录为已知呈现任何数据页中，每一行的索引时的数据库，从而使移动到开始的行索引行一项重要任务。 此外，总计记录计数是易于使用，因为它 s 只需在 DataTable （或任何对象用于保存数据库结果） 的记录数。

给定的开始的行索引和最大行数变量，自定义分页实现必须仅返回后，启动启动行索引处和最多的记录的最大行数的记录的精确的子集。 自定义分页提供两个难题：

- 我们必须能够高效地将行索引正在通过寻呼发送通过，以便我们可以开始返回指定的开始的行索引处的记录的整个数据中的每个行与相关联
- 我们需要提供的记录正在通过寻呼发送通过总数

在随后两个步骤中，我们将检查这些两个质询响应所需的 SQL 脚本。 除了 SQL 脚本中，我们还需要在 DAL 和 BLL 实现方法。

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>步骤 2： 返回记录正在通过寻呼发送通过的总数

我们将研究如何检索正在显示的页面的记录的精确子集之前，让我们来首先看一下如何返回的记录正在通过寻呼发送通过总数。 为了正确配置的分页用户界面需要这些信息。 使用可以获取的特定 SQL 查询返回的记录总数[`COUNT`聚合函数](https://msdn.microsoft.com/library/ms175997.aspx)。 例如，若要确定中的记录总数`Products`表，我们可以使用以下查询：


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample1.sql)]

让我们来将方法添加到我们 DAL 返回此信息。 具体而言，我们将创建名为 DAL 方法`TotalNumberOfProducts()`执行`SELECT`上面所示语句。

首先打开`Northwind.xsd`中的类型化数据集文件`App_Code/DAL`文件夹。 接下来，右键单击`ProductsTableAdapter`设计器中，选择添加查询。 正如我们在前面的教程中看到的遇到这将使我们能够与 DAL 添加新方法，调用时，将执行特定的 SQL 语句或存储的过程。 与前面的教程中我们 TableAdapter 方法，在此选择使用的临时 SQL 语句。


![使用临时 SQL 语句](efficiently-paging-through-large-amounts-of-data-cs/_static/image1.png)

**图 1**： 使用临时 SQL 语句


在下一个屏幕中，我们可以指定哪种类型的查询以创建。 因为此查询将返回单个标量值中的记录总数`Products`表选择`SELECT`它返回单个值选项。


![配置要使用返回单个值的 SELECT 语句的查询](efficiently-paging-through-large-amounts-of-data-cs/_static/image2.png)

**图 2**： 配置要使用返回单个值的 SELECT 语句的查询


后，该值指示要使用的查询的类型，我们接下来必须指定查询。


![使用从产品查询选择 COUNT(*)](efficiently-paging-through-large-amounts-of-data-cs/_static/image3.png)

**图 3**： 使用选择的计数 (\*) FROM 产品查询


最后，指定方法的名称。 为前面提到，但仍使 s 使用`TotalNumberOfProducts`。


![命名为 DAL 方法 TotalNumberOfProducts](efficiently-paging-through-large-amounts-of-data-cs/_static/image4.png)

**图 4**： 命名 DAL 方法 TotalNumberOfProducts


单击完成，后，该向导将添加`TotalNumberOfProducts`与 DAL 的方法。 DAL 中的标量返回方法返回可以为 null 的类型，如果 SQL 查询的结果是`NULL`。 我们`COUNT`查询，但是，将始终返回非`NULL`值; 而不考虑，DAL 方法将返回为 null 的整数。

除了 DAL 方法中，我们还需要 BLL 中的方法。 打开`ProductsBLL`类文件，并添加`TotalNumberOfProducts`方法只需调用 DAL s`TotalNumberOfProducts`方法：


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample2.cs)]

DAL s`TotalNumberOfProducts`方法返回为 null 的整数; 但是，我们已创建`ProductsBLL`类的`TotalNumberOfProducts`方法，以便它返回一个标准的整数。 因此，我们需要将`ProductsBLL`类 s`TotalNumberOfProducts`方法返回为 null 返回 DAL s 的整数的值部分`TotalNumberOfProducts`方法。 调用`GetValueOrDefault()`返回的值可以为 null 的整数，如果它存在，则可以为 null 的整数是否`null`，但是，它将返回默认整数值为 0。

## <a name="step-3-returning-the-precise-subset-of-records"></a>步骤 3： 返回记录的精确的子集

我们的下一个任务是在 DAL 和接受启动行索引的 BLL 创建方法和最大行数变量前面所述，并返回相应的记录。 我们在此之前，让 s 先来看一下所需的 SQL 脚本。 我们面临的挑战是，我们必须要高效地将索引分配到整个结果正在通过寻呼发送通过，以便我们可以返回启动启动行索引处 （和最多的记录的最大记录数） 仅对这些记录中的每个行。

如果已存在列用作行索引的数据库表中，这不是一个难题。 第一次看到我们可能会认为`Products`表 s`ProductID`字段即可满足要求，因为第一个产品具有`ProductID`为 1，2，第二个，依此类推。 但是，删除产品会使序列，使这种方法中的存在间隔。

有两种常规技术用于有效地将行索引相关联的数据，能够分页浏览，从而支持记录要检索的精确子集：

- **使用 SQL Server 2005 s`ROW_NUMBER()`关键字**到 SQL Server 2005 中，新`ROW_NUMBER()`关键字将排名的某些排序基于每个返回的记录与相关联。 此排名可以用作每个行的行索引。
- **使用表变量和`SET ROWCOUNT`**  SQL Server s [ `SET ROWCOUNT`语句](https://msdn.microsoft.com/library/ms188774.aspx)可以用于指定查询应终止; 之前处理的总记录数[表变量](http://www.sqlteam.com/item.asp?ItemID=9454)是本地的 T-SQL 的变量，可以容纳 akin 到表格数据、[临时表](http://www.sqlteam.com/item.asp?ItemID=2029)。 此方法适用于 Microsoft SQL Server 2005 和 SQL Server 2000 (而`ROW_NUMBER()`方法仅适用于 SQL Server 2005)。  
  
 本指南旨在创建具有的表变量`IDENTITY`列和列的主键的表的数据通过正在通过寻呼发送。 接下来，将其数据通过正在通过寻呼发送表的内容转储到表变量中，从而将连续的行索引相关联 (通过`IDENTITY`列) 为表中每个记录。 已填充的表变量之后,`SELECT`表变量中，语句与基础表联接，可以将执行拉出特定的记录。 `SET ROWCOUNT`语句用于智能地限制需要转储到表变量的记录数。  
  
 此方法的效率取决于所请求的页号作为`SET ROWCOUNT`值分配的值开始的行索引以及最大行数。 当通过： 编号较低的页面，例如第一个分页的数据的几个页面时这种方法是非常高效。 但是，它展示默认分页类似的性能，检索在其结尾附近的页时。

本教程通过实现自定义分页使用`ROW_NUMBER()`关键字。 有关使用表变量的详细信息和`SET ROWCOUNT`技术，请参阅[分页通过大型结果集的多个有效方法](http://www.4guysfromrolla.com/webtech/042606-1.shtml)。

`ROW_NUMBER()`关键字与通过使用以下语法执行特定排序返回每个记录关联排名：


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample3.sql)]

`ROW_NUMBER()`返回一个数字值，指定每个记录方面指示排序的秩。 例如，若要查看每个产品按从最顺序排列的秩最少的成本较低我们无法使用以下查询：


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample4.sql)]

图 5 显示此查询的结果通过 Visual Studio 中的查询窗口运行时。 请注意，按价格，以及每个行的价格排名订购的产品。


![价格级别所包含的每个返回的记录](efficiently-paging-through-large-amounts-of-data-cs/_static/image5.png)

**图 5**: 价格级别所包含的每个返回的记录


> [!NOTE]
> `ROW_NUMBER()`只是其中之一的许多新排名函数是在 SQL Server 2005 中可用。 有关的更全面讨论`ROW_NUMBER()`，以及其他排名函数，如读取[Microsoft SQL Server 2005 返回排名结果](http://www.4guysfromrolla.com/webtech/010406-1.shtml)。


当指定排名结果`ORDER BY`中的列`OVER`子句 (`UnitPrice`，在上面的示例)，SQL Server 必须将对结果进行排序。 这是快速操作，如果通过对结果进行，正在排序的列没有聚集的索引，或者如果没有覆盖索引，但会否则成本较高。 帮助改进足够大，查询的性能，请考虑添加按对结果排序所依据的列的非聚集索引。 请参阅[排名函数和 SQL Server 2005 中的性能](http://www.sql-server-performance.com/ak_ranking_functions.asp)了解性能注意事项的详细信息。

返回的排名信息`ROW_NUMBER()`中不能直接`WHERE`子句。 但是，派生的表可以用于返回`ROW_NUMBER()`结果，然后可以出现在`WHERE`子句。 例如，以下查询使用派生的表一起返回 ProductName 和 UnitPrice 列中，`ROW_NUMBER()`结果，然后使用`WHERE`子句，以便只返回这些产品价格排名为 11 到 20 之间：


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample5.sql)]

扩展此得更深入的概念，我们可以利用这种方法来检索给定的所需的开始的行索引和最大行数值数据的某个特定页：


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample6.html)]

> [!NOTE]
> 在本教程中，我们将会看到在更高版本上 *`StartRowIndex`* 提供 ObjectDataSource 编制了索引从零开始，启动而`ROW_NUMBER()`SQL Server 2005 返回值索引从 1 开始。 因此，`WHERE`子句将返回这些记录其中`PriceRank`完全大于 *`StartRowIndex`* 且小于或等于 *`StartRowIndex`*   + *`MaximumRows`*.


现在我们已讨论如何`ROW_NUMBER()`可以是用于检索给定的开始的行索引和最大行数值数据的特定页，我们现在需要实现此逻辑作为方法的 DAL 和 BLL。

创建我们必须决定排序此查询时所依据的结果将进行排名;让我们来按其名称按字母顺序排序的产品。 这意味着，如果使用自定义分页实现在本教程中我们将不是能够创建自定义分页的报表，不是还可以进行排序。 在下一步的教程中，不过，我们将了解如何可以提供此类功能。

上一节中我们创建 DAL 方法作为临时 SQL 语句。 遗憾的是，使用 TableAdapter 向导不等的 Visual Studio 中的 T-SQL 的分析器`OVER`语法使用`ROW_NUMBER()`函数。 因此，我们必须为存储过程来创建此 DAL 方法。 选择从视图菜单 （或命中的 Ctrl + Alt + S） 的服务器资源管理器，然后展开`NORTHWND.MDF`节点。 若要添加新的存储的过程，右键单击存储过程节点，并选择添加新的存储过程 （请参阅图 6）。


![将新的存储的过程添加到产品分页](efficiently-paging-through-large-amounts-of-data-cs/_static/image6.png)

**图 6**： 将新的存储的过程添加到产品分页


此存储的过程应接受两个整数输入的参数-`@startRowIndex`和`@maximumRows`并用`ROW_NUMBER()`函数按排序`ProductName`字段，只返回这些行大于指定`@startRowIndex`和小于或等于`@startRowIndex`  +  `@maximumRow` s。 为新的存储过程中输入以下脚本，然后单击保存图标，将该存储的过程添加到数据库。


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample7.sql)]

在创建后的存储的过程，花些时间对它进行测试。右键单击`GetProductsPaged`存储的过程中服务器资源管理器的名称并选择执行选项。 Visual Studio 随后将提示您为输入参数，`@startRowIndex`和`@maximumRow`s （请参阅图 7）。 请尝试不同的值，并检查结果。


![输入一个值@startRowIndex和@maximumRows参数](efficiently-paging-through-large-amounts-of-data-cs/_static/image7.png)

**图 7**： 输入一个值@startRowIndex和@maximumRows参数


后选择这些输入参数值，输出窗口中会显示结果。 图 8 显示结果时两个传入 10`@startRowIndex`和`@maximumRows`参数。


[![返回记录，将出现在第二个数据页](efficiently-paging-through-large-amounts-of-data-cs/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image8.png)

**图 8**: 记录，将出现在第二个数据页中返回 ([单击以查看实际尺寸的图像](efficiently-paging-through-large-amounts-of-data-cs/_static/image10.png))


与此存储过程创建，我们已准备好创建 re`ProductsTableAdapter`方法。 打开`Northwind.xsd`类型化数据集，右键单击在`ProductsTableAdapter`，然后选择添加查询选项。 而不是创建使用临时 SQL 语句中的查询创建使用现有存储的过程。


![创建使用现有存储的过程的 DAL 方法](efficiently-paging-through-large-amounts-of-data-cs/_static/image11.png)

**图 9**： 创建使用现有存储的过程的 DAL 方法


接下来，我们将提示您选择要调用的存储的过程。 选取`GetProductsPaged`从下拉列表的存储过程。


![选择 GetProductsPaged 从下拉列表的存储过程](efficiently-paging-through-large-amounts-of-data-cs/_static/image12.png)

**图 10**： 选择 GetProductsPaged 从下拉列表的存储过程


下一个屏幕然后询问什么类型的数据返回的存储过程： 表格数据、 单个值或没有值。 由于`GetProductsPaged`存储的过程可以返回多个记录，指示它返回表格数据。


![指示存储的过程返回表格数据](efficiently-paging-through-large-amounts-of-data-cs/_static/image13.png)

**图 11**： 指示存储的过程返回表格数据


最后，指示你想要创建的方法的名称。 与我们前面的教程，请继续并创建方法使用这两个填充 DataTable 返回 DataTable。 将第一个方法`FillPaged`和第二个`GetProductsPaged`。


![名称方法 FillPaged 和 GetProductsPaged](efficiently-paging-through-large-amounts-of-data-cs/_static/image14.png)

**图 12**： 名称方法 FillPaged 和 GetProductsPaged


除了创建 DAL 方法返回产品的特定页，我们还需要提供 BLL 中的此类功能。 DAL 像方法一样，BLL 的 GetProductsPaged 方法必须接受用于指定开始的行索引和最大行数的两个整数输入，并且必须返回只在指定范围内的记录。 创建这样的 BLL 方法，只需调用下 DAL 的 GetProductsPaged 方法，如下所示 ProductsBLL 类中：


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample8.cs)]

你可以使用任何名称为 BLL 方法 s 输入参数，但是，我们将会看到很快，选择使用`startRowIndex`和`maximumRows`保存我们从一个额外的配置对象数据源以使用此方法时的工作的位。

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>步骤 4： 配置对象数据源以使用自定义分页

使用用于访问完整的记录的特定子集的 BLL 和 DAL 方法，我们已准备好创建 GridView 重新控制通过使用自定义分页其基础记录该页面。 首先打开`EfficientPaging.aspx`页面`PagingAndSorting`文件夹中，将一个 GridView 添加到页上，并将其配置为使用新的 ObjectDataSource 控件。 在过去的教程中，我们通常必须配置为使用 ObjectDataSource`ProductsBLL`类的`GetProducts`方法。 这一次，但是，我们想要使用`GetProductsPaged`方法相反，因为`GetProducts`方法返回*所有*的数据库中的产品而`GetProductsPaged`返回只记录的特定子集。


![配置对象数据源以使用 ProductsBLL 类的 GetProductsPaged 方法](efficiently-paging-through-large-amounts-of-data-cs/_static/image15.png)

**图 13**： 配置对象数据源以使用 ProductsBLL 类的 GetProductsPaged 方法


自从我们重新创建一个只读的 GridView，花些时间在 INSERT、 UPDATE、 中设置的方法下拉列表并删除选项卡添加到 （无）。

接下来，该对象数据源向导会提示我们的源`GetProductsPaged`方法 s`startRowIndex`和`maximumRows`输入参数值。 这些输入的参数将实际的 GridView 自动设置，因此只需将源设置保留为无并单击完成。


![将保留为 None 的输入的参数源](efficiently-paging-through-large-amounts-of-data-cs/_static/image16.png)

**图 14**： 保留为 None 的输入的参数源


完成对象数据源向导后，命令将 BoundField 或 CheckBoxField 为每个产品数据字段包含 GridView。 随意认为适合定制 GridView 的外观。 我已选择仅显示`ProductName`， `CategoryName`， `SupplierName`， `QuantityPerUnit`，和`UnitPrice`BoundFields。 此外，配置 GridView 支持分页，通过检查其智能标记中的分页启用复选框。 这些更改后的 GridView 和对象数据源的声明性标记应看起来类似以下：


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample9.aspx)]

如果访问通过浏览器页，但是，GridView 是没有任何地方要查找。


![GridView 是不显示](efficiently-paging-through-large-amounts-of-data-cs/_static/image17.png)

**图 15**: GridView 是不显示


缺少 GridView，因为对象数据源当前使用 0 的值为这两个`GetProductsPaged``startRowIndex`和`maximumRows`输入参数。 因此，生成的 SQL 查询返回的任何记录，因此 GridView 不会显示。

若要解决此问题，我们需要配置对象数据源以使用自定义分页。 这可以通过以下步骤完成：

1. **设置 ObjectDataSource s`EnablePaging`属性`true`**这将指示必须将传递给 ObjectDataSource`SelectMethod`两个其他参数： 一个用于指定开始的行索引 ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx))，，另一个用于指定最大行数 ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx))。
2. **设置 ObjectDataSource s`StartRowIndexParameterName`和`MaximumRowsParameterName`属性相应地**`StartRowIndexParameterName`和`MaximumRowsParameterName`属性指示传入的输入参数的名称`SelectMethod`用于自定义分页用途. 默认情况下，这些参数名称是`startIndexRow`和`maximumRows`，这是正因如此，在创建时`GetProductsPaged`方法在 BLL，我使用了这些值为输入参数。 如果选择了使用不同的参数名称 BLL s`GetProductsPaged`如方法`startIndex`和`maxRows`，对于你需要的示例设置 ObjectDataSource s`StartRowIndexParameterName`和`MaximumRowsParameterName`属性相应地 （例如为 startIndex`StartRowIndexParameterName`和最大行数为`MaximumRowsParameterName`)。
3. **设置 ObjectDataSource s [ `SelectCountMethod`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx)总数量的记录正在分页通过返回的方法的名称 (`TotalNumberOfProducts`)**回想一下，`ProductsBLL`类的`TotalNumberOfProducts`方法返回的记录正在通过寻呼发送通过使用执行 DAL 方法总数`SELECT COUNT(*) FROM Products`查询。 为正确呈现的分页界面情况下，此信息所需的对象数据源。
4. **删除`startRowIndex`和`maximumRows``<asp:Parameter>`从 ObjectDataSource s 声明性标记元素**配置时通过向导 ObjectDataSource，Visual Studio 自动添加两个`<asp:Parameter>`元素`GetProductsPaged`方法 s 输入参数。 通过设置`EnablePaging`到`true`，将自动传递这些参数; 如果他们还出现在声明性语法，ObjectDataSource 将尝试传递*四个*参数`GetProductsPaged`方法和两个参数`TotalNumberOfProducts`方法。 如果你忘记了以删除这些`<asp:Parameter>`元素，在访问通过浏览器，您将收到错误消息的页时： *ObjectDataSource ObjectDataSource1 找不到非泛型方法 TotalNumberOfProducts 具有参数： 值，值*。

进行这些更改后，ObjectDataSource s 声明性语法应如下所示：


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample10.aspx)]

请注意，`EnablePaging`和`SelectCountMethod`已经设置了属性和`<asp:Parameter>`已删除元素。 进行这些更改之后，图 16 将显示属性窗口的屏幕截图。


![若要使用自定义分页，配置 ObjectDataSource 控件](efficiently-paging-through-large-amounts-of-data-cs/_static/image18.png)

**图 16**： 若要使用自定义分页，配置 ObjectDataSource 控件


进行这些更改后，请访问此页通过浏览器。 你应看到列出，10 种产品按字母顺序排列。 花一些时间来单步执行一页数据一次。 因为它仅检索需要显示为给定页的记录，而不没有默认分页和自定义分页之间任何 visual 区别从最终用户 s 角度来看，自定义分页更有效地页面通过大量的数据。


[![数据、 Ordered 按产品名称，是分页使用自定义分页](efficiently-paging-through-large-amounts-of-data-cs/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image19.png)

**图 17**: Data、 Ordered 按产品名称，是分页使用自定义分页 ([单击以查看实际尺寸的图像](efficiently-paging-through-large-amounts-of-data-cs/_static/image21.png))


> [!NOTE]
> 使用自定义分页页计数值返回 ObjectDataSource 的`SelectCountMethod`GridView 的视图状态中存储。 其他 GridView 变量`PageIndex`， `EditIndex`， `SelectedIndex`，`DataKeys`集合中，依次类推存储在*控制状态*，这保留无论 GridView s 的值如何`EnableViewState`属性。 由于`PageCount`值保存在回发期间使用的视图状态，当使用分页接口，其中包含链接，使你转到的最后一页时，它是命令性 GridView 的视图状态为已启用。 （如果你分页的接口不包括的直接链接到最后一页上，然后你可以禁用视图状态。）


单击最后一页链接导致回发，并指示 GridView 以更新其`PageIndex`属性。 如果单击最后一页链接时，赋给变量 GridView 其`PageIndex`属性之一的值小于其`PageCount`属性。 与视图状态已禁用，`PageCount`值丢失在回发期间和`PageIndex`改为分配的最大整数值。 接下来，尝试确定的起始行索引乘以 GridView`PageSize`和`PageCount`属性。 这会导致`OverflowException`由于产品超出了允许的最大整数大小。

## <a name="implement-custom-paging-and-sorting"></a>实现自定义分页和排序

我们当前的自定义分页实现需要在创建时，静态指定通过调数据时所依据的顺序`GetProductsPaged`存储过程。 但是，你可能记录 GridView s 智能标记包含除了启用分页选项启用排序复选框。 遗憾的是，将排序支持添加到使用了当前的自定义分页实现 GridView 仅将排序数据的当前查看页上的记录。 例如，如果你配置 GridView 还支持分页，并按产品名称的降序顺序查看数据，第一页时的排序然后，它将于第 1 页反向产品的顺序。 如图 18 所示，如墨鱼时显示为第一个产品按反向字母顺序，将忽略的 71 其他产品按字母顺序; 晚墨鱼，排序只有第一页上的那些记录中计入的排序。


[![仅数据显示在当前页面上进行排序](efficiently-paging-through-large-amounts-of-data-cs/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image22.png)

**图 18**： 仅显示数据在当前页面上进行排序 ([单击以查看实际尺寸的图像](efficiently-paging-through-large-amounts-of-data-cs/_static/image24.png))


排序仅适用于数据的当前页因为后已从 BLL s 检索的数据排序发生`GetProductsPaged`，因此该方法仅返回的特定页中的记录。 若要实现正确进行排序，我们需要传递到排序表达式`GetProductsPaged`方法，以便可以在返回的数据的特定页之前适当排名数据。 我们将了解如何实现此目的在我们的下一步教程。

## <a name="implementing-custom-paging-and-deleting"></a>实现自定义分页和删除

如果你启用删除功能中使用自定义分页技术，您会发现从的最后一页中删除最后一条记录时分页其数据 GridView，GridView 消失而不是适当递减 GridView 的`PageIndex`. 若要重现此 bug，启用删除可能导致，本教程只是我们刚刚创建。 转到的最后一页 （页 9），其中你应看到一个产品，因为我们分页 81 产品，一次 10 种产品。 删除此产品。

一旦删除最后一个产品，GridView*应*自动转到第八个页上，并使用默认分页展现这样的功能。 使用自定义分页，但是，删除最后一页中，于最后一个产品后 GridView 只需从屏幕上消失完全。 确切原因*为什么*发生这种情况均超出了本教程的范围的一个位; 请参阅[从使用自定义分页 GridView 删除最后一页上的最后一个记录](http://scottonwriting.net/sowblog/posts/7326.aspx)并与来源的数据的低级别的详细信息此问题。 在摘要它由于下面的 GridView 时单击删除按钮执行的步骤序列的 s:

1. 删除的记录
2. 获取相应的记录，若要显示有关指定`PageIndex`和`PageSize`
3. 检查以确保`PageIndex`不超过数据源; 中的数据的页面数，如果它存在，自动递减 GridView 的`PageIndex`属性
4. 将合适的页面的数据绑定到 GridView 使用在步骤 2 中获取的记录

此问题，是因为该在步骤 2`PageIndex`时仍抓取要显示的记录是使用`PageIndex`只需删除其唯一记录的最后一页。 因此，在步骤 2 中，*没有*返回记录的因为数据的最后一页不再包含任何记录。 然后，在步骤 3 中，GridView 认识到其`PageIndex`属性大于数据源中的页的总数 （自我们已删除的最后一页中的最后一个记录），因此递减其`PageIndex`属性。 在步骤 4 中 GridView 尝试将本身绑定到在步骤 2; 中检索数据但是，在步骤 2 中未返回任何记录，因此导致空 GridView。 使用默认分页，此问题没有 t 外围因为在步骤 2 中*所有*从数据源中检索记录。

若要解决此问题，我们具有两个选项。 第一种是创建的事件处理程序 GridView 的`RowDeleted`确定只需删除页中显示多少条记录的事件处理程序。 如果存在只有一条记录，则只需删除的记录必须已被最后一个，并且我们需要递减 GridView 的`PageIndex`。 当然，我们只想更新`PageIndex`如果删除操作实际上已成功时，这就可以确定通过确保`e.Exception`属性是`null`。

此方法很有效，因为它会更新`PageIndex`步骤 1 之后但在步骤 2 之前。 因此，在步骤 2 中，相应的记录集将返回。 若要实现此目的，使用代码如下所示：


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample11.cs)]

备用解决方法是创建的事件处理程序 ObjectDataSource s`RowDeleted`事件并设置`AffectedRows`属性的值为 1。 删除在步骤 1 中 （但之前重新检索步骤 2 中的数据） 的记录后, GridView 更新其`PageIndex`属性，如果一个或多个行受该操作。 但是， `AffectedRows` ObjectDataSource 未设置属性，并且因此忽略了此步骤。 一种方法具有执行此步骤是手动设置`AffectedRows`属性，如果删除操作成功完成。 这可以实现使用代码如下所示：


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample12.cs)]

这两个这些事件处理程序的代码可在代码隐藏类的`EfficientPaging.aspx`示例。

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>比较默认和自定义分页的性能

由于自定义分页仅检索所需的记录，而默认分页返回*所有*的查看，每一页的记录它 s 清除自定义分页是比默认分页更高效。 但是仅多少更高效是自定义分页？ 可以看到什么样的性能提升，将从默认分页移动到自定义分页？

遗憾的是，在该处 s 没有一个适合大小所有此处回答。 性能提升取决于许多因素影响，最卓越两个正在数量的记录正在通过寻呼发送通过负载置于 web 服务器和数据库服务器之间的数据库服务器和通信通道。 对于只需少量几十个记录的小型表，性能差异可能可以忽略不计。 不过，对于具有数千到成千上万的行的大型表的性能差异是很严重的。

项目的挖掘， [ASP.NET 2.0 与 SQL Server 2005 中的自定义分页](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)，包含所运行的用于展示之间这两项分页技术分页通过与数据库表时的性能差异某些性能测试50,000 个记录。 这些测试在我检查这两个执行 SQL Server 级别的查询的时间 (使用[SQL 事件探查器](https://msdn.microsoft.com/library/ms173757.aspx)) 和 ASP.NET 页使用[ASP.NET 的跟踪功能](https://msdn.microsoft.com/library/y13fw6we.aspx)。 请记住，这些测试已在具有单个活动用户，我开发框上运行，并因此是科学并不模拟典型的网站的负载模式。 无论如何，结果说明中的默认实例和自定义分页的执行时间的相对差异时使用的数据量非常大。


|  | **Avg.持续时间 （秒）** | **读取** |
| --- | --- | --- |
| **默认分页 SQL 事件探查器** | 1.411 | 383 |
| **自定义分页 SQL 事件探查器** | 0.002 | 29 |
| **默认分页 ASP.NET 跟踪** | 2.379 | *不适用* |
| **自定义分页 ASP.NET 跟踪** | 0.029 | *不适用* |


如你所见，检索数据的特定页平均需要小于读取 354 和很短的时间内完成。 在 ASP.NET 页上，自定义的页面是能够在呈现接近于 1/100<sup>th</sup>它所花时间的使用默认分页时。 请参阅[我文章](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)对于这些结果以及代码和数据库的详细信息，可以下载重现这些测试在你自己的环境中。

## <a name="summary"></a>摘要

默认分页很容易做到以实现数据 Web 控件 s 智能标记只检查启用分页的复选框，但这种简单的代价是性能。 使用默认分页，当用户请求数据的任何页*所有*返回记录，即使仅有极小一部分其中可能会显示。 为了应对这项性能开销，ObjectDataSource 提供替代的分页选项自定义分页。

自定义分页改进了默认通过检索需要要显示的那些记录分页 s 性能问题时它更为复杂，若要实现自定义分页的 s。 首先，必须编写查询，用于正确 （和有效地） 访问请求的记录的特定子集。 这可以实现多种方式;在本教程中，我们探讨的一个是使用 SQL Server 2005 s 新`ROW_NUMBER()`为级别函数结果，然后返回不仅仅是导致其排名是否在指定的范围内。 此外，我们需要添加一种方式来确定总正在通过寻呼发送到的记录数。 在创建后这些 DAL 和 BLL 方法，我们还需要配置对象数据源，以便它可以确定总记录数通过正在通过寻呼发送，并可以正确传递给 BLL 启动行索引和最大行数的值。

实现自定义分页需要大量的步骤并不几乎与默认分页一样简单，而自定义分页时，需要这么足够大，大量数据进行分页。 结果检查作为显示、 自定义分页可以舍弃从 ASP.NET 页呈现时间 （秒），并可通过一个或多个数量级的淡化数据库服务器上的负载。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一页](paging-and-sorting-report-data-cs.md)
[下一页](sorting-custom-paged-data-cs.md)
