---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
title: "查询数据与 SqlDataSource 控件 (VB) |Microsoft 文档"
author: rick-anderson
description: "前面的教程中我们用于 ObjectDataSource 控件完全分隔的表示层从数据访问层。 与此老师正在启动..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: b12f752d-3502-40a4-b695-fc7b7d08cfd3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
msc.type: authoredcontent
ms.openlocfilehash: a3832bd9847ec8e789b71d13b30a673c8779f4ac
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="querying-data-with-the-sqldatasource-control-vb"></a>查询与 SqlDataSource 控件 (VB) 的数据
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_VB.exe)或[下载 PDF](querying-data-with-the-sqldatasource-control-vb/_static/datatutorial47vb1.pdf)

> 前面的教程中我们用于 ObjectDataSource 控件完全分隔的表示层从数据访问层。 我们从开始本教程，了解如何为不需要此类严格的演示文稿和数据访问分离的简单应用程序使用 SqlDataSource 控件。


## <a name="introduction"></a>介绍

所有这些教程我们已检查到目前为止已使用的演示文稿、 业务逻辑和数据访问层组成的分层体系结构。 数据访问层 (DAL) 已在第一个教程制作 ([创建数据访问层](../introduction/creating-a-data-access-layer-vb.md)) 和业务逻辑层中第二个 ([创建业务逻辑层](../introduction/creating-a-business-logic-layer-vb.md))。 从开始[将数据显示与 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md)教程，我们已了解如何使用 ASP.NET 2.0 s 新 ObjectDataSource 控件以声明方式与从演示文稿层体系结构建立连接。

虽然所有教程到目前为止已使用体系结构来处理数据，还有可能访问、 插入、 更新和删除数据库数据直接从 ASP.NET 页中，跳过体系结构。 直接在 web 页中，这样会将特定数据库查询和业务逻辑。 对于充分大型或复杂的应用程序，设计、 实施和使用分层体系结构至关重要的成功、 updatability，与应用程序的可维护性。 开发可靠的体系结构，但是，可能没有必要时创建极其简单、 一次性应用程序。

ASP.NET 2.0 提供了五个内置的数据源控件[SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx)， [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx)， [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx)， [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx)，和[SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx)。 SqlDataSource 可用来访问和修改直接来自关系数据库中，包括 Microsoft SQL Server、 Microsoft Access、 Oracle、 MySQL 和其他数据。 在本教程的下一步的三个，我们将查看如何使用 SqlDataSource 控件，探索如何查询和筛选器数据库数据，以及如何使用到 SqlDataSource 插入、 更新和删除数据。


![ASP.NET 2.0 包含五个内置的数据源控件](querying-data-with-the-sqldatasource-control-vb/_static/image1.gif)

**图 1**: ASP.NET 2.0 包含五个内置的数据源控件


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>比较的对象数据源和 SqlDataSource

从概念上讲，ObjectDataSource 和 SqlDataSource 控件是只需对数据的代理。 中所述[将数据显示与 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md)教程中，ObjectDataSource 具有指示提供的数据和方法来调用要选择，请插入、 更新和删除数据的对象类型的属性从基础的对象类型。 ObjectDataSource 的属性具有配置后，数据如 GridView、 说明或 DataList Web 控件可以绑定到控件，使用 ObjectDataSource s `Select()`， `Insert()`， `Delete()`，和`Update()`方法与基础体系结构进行交互。

SqlDataSource 提供相同的功能，但操作针对关系数据库，而不是对象库。 使用 SqlDataSource，我们必须指定数据库连接字符串和临时 SQL 查询或存储过程执行插入、 更新、 删除和检索数据。 SqlDataSource s `Select()`， `Insert()`， `Update()`，和`Delete()`方法调用时，连接到指定的数据库和发出相应的 SQL 查询。 如下面的关系图所示，这些方法执行连接到数据库、 发出查询，并返回结果的 grunt 工作。


![SqlDataSource 充当到数据库的代理](querying-data-with-the-sqldatasource-control-vb/_static/image2.gif)

**图 2**: SqlDataSource 充当到数据库的代理


> [!NOTE]
> 在本教程中我们将重点从数据库检索数据。 在[插入、 更新和删除数据与 SqlDataSource 控件](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)教程，我们将了解如何配置 SqlDataSource 以支持插入、 更新和删除。


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>SqlDataSource 和 AccessDataSource 控件

除了 SqlDataSource 控件中，ASP.NET 2.0 还包括 AccessDataSource 控件。 这两个不同的控件会导致许多开发人员熟悉 ASP.NET 2.0 怀疑 AccessDataSource 控件用于处理以独占方式使用 Microsoft Access SqlDataSource 控件设计为可以以独占方式使用 Microsoft SQL Server。 SqlDataSource 控件时 AccessDataSource 设计为专门用于 Microsoft Access，适用于*任何*可以通过.NET 访问的关系数据库。 这包括任何符合 OleDb 或 ODBC 的数据存储，如 Microsoft SQL Server、 Microsoft Access、 Oracle、 Informix、 MySQL 和 PostgreSQL，此外还有许多其他。

AccessDataSource 和 SqlDataSource 控件之间的唯一区别是指定的数据库连接信息的方式。 AccessDataSource 控件需要仅的 Access 数据库文件的文件路径。 SqlDataSource，另一方面，需要完整的连接字符串。

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>步骤 1： 创建 SqlDataSource Web 页

我们开始浏览如何直接使用数据库数据使用 SqlDataSource 控件之前，让我们来首先花一些时间，我们需要以及本教程的下一步的三个网站项目中创建 ASP.NET 页。 首先，通过添加一个名为的新文件夹`SqlDataSource`。 接下来，添加到该文件夹，并确保将每个包含网页相关联的以下 ASP.NET 页面`Site.master`母版页：

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![SqlDataSource 相关教程添加的 ASP.NET 页面](querying-data-with-the-sqldatasource-control-vb/_static/image3.gif)

**图 3**： 添加 ASP.NET 页 SqlDataSource 相关教程


在其他文件夹中，如`Default.aspx`中`SqlDataSource`文件夹将在其部分中列出这些教程。 回想一下，`SectionLevelTutorialListing.ascx`用户控件提供此功能。 因此，此用户将控件添加到`Default.aspx`通过从解决方案资源管理器拖到页面上的设计视图拖动。


[![SectionLevelTutorialListing.ascx 用户控件添加到 Default.aspx](querying-data-with-the-sqldatasource-control-vb/_static/image5.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image4.gif)

**图 4**： 添加`SectionLevelTutorialListing.ascx`用户控件`Default.aspx`([单击以查看实际尺寸的图像](querying-data-with-the-sqldatasource-control-vb/_static/image6.gif))


最后，将这些四个页面添加到的条目为`Web.sitemap`文件。 具体而言，将以下标记之后添加自定义按钮添加到 DataList 和中继器`<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample1.sql)]

在更新后`Web.sitemap`，花些时间查看通过浏览器网站的教程。 在左侧菜单现在包括用于编辑、 插入和删除教程的项。


![站点图现在包括 SqlDataSource 教程的条目](querying-data-with-the-sqldatasource-control-vb/_static/image7.gif)

**图 5**： 站点图现在包括 SqlDataSource 教程的条目


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>步骤 2： 添加和配置 SqlDataSource 控件

首先打开`Querying.aspx`页面`SqlDataSource`文件夹，然后切换到设计视图。 将一个 SqlDataSource 控件从工具箱中拖动到设计器和组其`ID`到`ProductsDataSource`。 与 ObjectDataSource，SqlDataSource 不生成任何呈现的输出，因此会显示为灰色的框，在设计图面上。 若要配置 SqlDataSource，单击从 SqlDataSource s 智能标记的配置数据源链接。


![单击配置从 SqlDataSource s 智能标记的数据源链接](querying-data-with-the-sqldatasource-control-vb/_static/image8.gif)

**图 6**： 单击配置从 SqlDataSource s 智能标记的数据源链接


这会打开 SqlDataSource 控件 s 配置数据源向导。 尽管向导的步骤不同从 ObjectDataSource 控件 s，最终目标是提供有关如何检索、 插入、 更新和删除通过数据源的数据的详细信息相同。 对于 SqlDataSource 这必然指定要使用的基础数据库和提供的临时 SQL 语句或存储的过程。

向导首先提示我们数据库。 下拉列表包含在 web 应用程序 s 中找到这些数据库`App_Data`文件夹和那些已添加到服务器资源管理器中的数据连接节点。 因为我们已添加的连接字符串`NORTHWIND.MDF`数据库中`App_Data`到我们的项目的文件夹`Web.config`文件，下拉列表包含该连接字符串，对引用`NORTHWINDConnectionString`。 从下拉列表中选择此项目，然后单击下一步。


![从下拉列表中选择 NORTHWINDConnectionString](querying-data-with-the-sqldatasource-control-vb/_static/image9.gif)

**图 7**： 选择`NORTHWINDConnectionString`从下拉列表


选择数据库之后, 向导将要求提供查询返回数据。 我们可以指定表或视图以返回或可以输入自定义 SQL 语句或指定存储的过程的列。 您可以自定义 SQL 语句或存储的过程和表中的指定列的情况下通过指定此选项之间切换，或查看单选按钮。

> [!NOTE]
> 对于此第一个示例，让我们来使用从表或视图选项指定的列。 我们在本教程后面返回到向导，浏览指定的自定义 SQL 语句或存储的过程选项。


图 8 显示配置 Select 语句屏幕时选择表或视图的单选按钮中的指定列。 下拉列表包含表和视图在 Northwind 数据库中，与所选的表或视图的列下面的复选框列表中显示的组。 对于本例，请让 s 返回`ProductID`， `ProductName`，和`UnitPrice`中的列`Products`表。 如图 8 所示之后进行这些选择向导显示生成的 SQL 语句, `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`。


![从产品表中返回数据](querying-data-with-the-sqldatasource-control-vb/_static/image10.gif)

**图 8**： 返回数据自`Products`表


在配置向导以返回`ProductID`， `ProductName`，和`UnitPrice`中的列`Products`表中，单击下一步按钮。 此最终屏幕提供了机会检查从前面的步骤配置该查询的结果。 单击测试查询按钮执行配置`SELECT`语句并显示在网格中的结果。


![单击测试查询按钮以查看你选择的查询](querying-data-with-the-sqldatasource-control-vb/_static/image11.gif)

**图 9**： 单击测试查询按钮以接受审核你`SELECT`查询


若要完成向导，请单击完成。

如与 ObjectDataSource，SqlDataSource 的向导只是将值分配给控件的属性，即[ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx)和[ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx)属性。 完成向导后，你 SqlDataSource 控件 s 声明性标记应类似于以下：


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample2.aspx)]

`ConnectionString`属性提供有关如何连接到数据库的信息。 此属性可以将分配一个完整的硬编码的连接字符串值，或可以指向中的连接字符串`Web.config`。 若要引用 Web.config 中的连接字符串值，使用语法`<%$ expressionPrefix:expressionValue %>`。 通常情况下， *expressionPrefix*是 ConnectionStrings 和*expressionValue*是中的连接字符串的名称`Web.config` [ `<connectionStrings>`部分](https://msdn.microsoft.com/library/bf7sd233.aspx)。 但是，可以使用语法来引用`<appSettings>`元素或从资源文件的内容。 请参阅[ASP.NET 表达式概述](https://msdn.microsoft.com/library/d5bd1tad.aspx)有关此语法的详细信息。

`SelectCommand`属性指定的临时 SQL 语句或存储的过程以执行以返回数据。

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>步骤 3： 添加数据 Web 控件，并将其绑定到 SqlDataSource

SqlDataSource 已配置后，它可以绑定到数据 Web 控件，如 GridView 或说明。 对于本教程，让我们来显示一个 GridView 中的数据。 从工具箱中，将一个 GridView 拖到该页面上，并将其绑定到`ProductsDataSource`SqlDataSource 通过从 GridView s 智能标记中的下拉列表中选择数据源。


[![添加一个 GridView，并将其绑定到 SqlDataSource 控件](querying-data-with-the-sqldatasource-control-vb/_static/image13.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image12.gif)

**图 10**： 添加一个 GridView，并将其绑定到 SqlDataSource 控件 ([单击以查看实际尺寸的图像](querying-data-with-the-sqldatasource-control-vb/_static/image14.gif))


一旦你已从 GridView s 智能标记中的下拉列表中选择 SqlDataSource 控件，Visual Studio 将自动添加 BoundField 或 CheckBoxField 到 GridView 为每个返回的数据源控件的列。 由于 SqlDataSource 返回三个数据库列`ProductID`， `ProductName`，和`UnitPrice`GridView 中有三个字段。

花一些时间来配置 GridView 的三 BoundFields。 更改`ProductName`字段 s`HeaderText`产品名称的属性和`UnitPrice`字段 s 到价格。 此外设置格式`UnitPrice`字段作为一种货币。 进行这些修改之后, 你 GridView s 声明性标记应如下类似如下：


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample3.aspx)]

访问此页面通过浏览器。 图 11 显示，如 GridView 列出每个产品 s `ProductID`， `ProductName`，和`UnitPrice`值。


[![GridView 显示每个产品的 ProductID、 产品名称和单价值](querying-data-with-the-sqldatasource-control-vb/_static/image16.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image15.gif)

**图 11**: GridView 显示每个产品 s `ProductID`， `ProductName`，和`UnitPrice`值 ([单击以查看实际尺寸的图像](querying-data-with-the-sqldatasource-control-vb/_static/image17.gif))


GridView 访问页时调用其数据源控件的`Select()`方法。 如果我们已使用 ObjectDataSource 控件，这称为`ProductsBLL`类的`GetProducts()`方法。 使用 SqlDataSource，但是，`Select()`方法建立到指定的数据库和问题的连接`SelectCommand`(`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`，在此示例中)。 SqlDataSource 返回其结果它 GridView 然后枚举，在每个数据库记录返回 GridView 中创建行。

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>内置数据 Web 控制功能和 SqlDataSource 控件

通常，所固有的 Web 控件分页、 排序、 编辑，数据的功能删除、 插入和等等特定于数据 Web 控件，并不依赖于使用的数据源控件。 也就是说，GridView 可以利用其内置分页、 排序、 编辑和删除是否绑定到 ObjectDataSource 或 SqlDataSource。 但是，某些数据 Web 控件功能是敏感到正在使用的数据源控件或数据源控件的配置的。

例如，在[高效地分页通过大型金额的数据](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md)教程我们讨论如何，默认情况下，Web 的数据的分页逻辑控制 naively 返回*所有*从基础的记录数据源，然后显示在给定的当前页索引和要每页显示的记录数的记录的仅适当的子集。 当足够大的结果集进行分页时，此模型是效率非常低。 幸运的是，ObjectDataSource 可以配置为支持自定义分页，它返回要显示的记录只有精确的子集。 SqlDataSource 控件，但是，缺少实现自定义分页的属性。

使用分页和排序的另一种微妙产生与 SqlDataSource。 默认情况下，可以分页或排序通过 GridView 从 SqlDataSource 返回的数据。 若要进行此演示，请检查 GridView s 中的智能标记中的启用分页和启用排序选项`Querying.aspx`和验证这是否按预期方式工作。

排序和分页合作，因为 SqlDataSource 到松散类型化数据集检索数据库数据。 从数据集，可以查明的查询所返回的一个重要方面实现分页的记录总数。 此外，可以通过 DataView 进行排序的数据集的结果。 GridView 请求分页或排序数据时，SqlDataSource 自动将使用这些功能。

SqlDataSource 可以将配置为通过更改返回而不是数据集 DataReader 其[`DataSourceMode`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx)从`DataSet`（默认值） 到`DataReader`。 可能的情况下首选使用 DataReader，当将 SqlDataSource 的结果传递给需要 DataReader 的现有代码。 此外，由于 DataReaders 相当简单的对象，而不是数据集，它们提供更好的性能。 如果进行此更改，但是，数据 Web 控件可以既不进行排序，也页由于 SqlDataSource 无法确定由查询，返回多少条记录，也不 DataReader 提供任何方法的返回的数据进行排序。

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>步骤 4： 使用自定义 SQL 语句或存储过程

时配置 SqlDataSource 控件，则为自定义 SQL 语句或存储的过程，或从现有的表或视图的列，两种方法之一可以指定用于返回数据的查询。 在步骤 2 中，我们探讨了选择中的列`Products`表。 让我们来看如何使用自定义 SQL 语句。

添加到另一个 GridView 控件`Querying.aspx`页上，选择创建新的数据源从下拉列表中的智能标记。 接下来，指示，数据将会从数据库中提取这将创建一个新的 SqlDataSource 控件。 命名该控件`ProductsWithCategoryInfoDataSource`。


![创建一个名为 ProductsWithCategoryInfoDataSource 的新 SqlDataSource 控件](querying-data-with-the-sqldatasource-control-vb/_static/image18.gif)

**图 12**： 创建一个名为的新 SqlDataSource 控件`ProductsWithCategoryInfoDataSource`


下一个屏幕要求我们以指定的数据库。 正如我们做回图 7 中，选择`NORTHWINDConnectionString`从下拉列表，然后单击下一步。 在配置 Select 语句屏幕中，选择指定一个自定义 SQL 语句或存储的过程单选按钮，然后单击下一步。 此时会弹出定义自定义语句或存储过程屏幕，其中提供标记为选择、 更新、 插入和删除的选项卡。 在每个选项卡可以在文本框中输入的自定义的 SQL 语句，或从下拉列表中选择存储的过程。 在本教程中我们将考察输入自定义的 SQL 语句;下一教程包括的示例，使用存储的过程。


![输入自定义 SQL 语句或选取的存储的过程](querying-data-with-the-sqldatasource-control-vb/_static/image19.gif)

**图 13**： 输入自定义 SQL 语句或选取的存储的过程


自定义 SQL 语句可以在文本框手动输入，或者可以通过单击查询生成器按钮以图形方式构造。 从查询生成器或文本框中，使用以下查询返回`ProductID`和`ProductName`字段从`Products`表使用`JOIN`以检索产品 s`CategoryName`从`Categories`表：


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample4.sql)]


![你可以以图形方式构造查询使用查询生成器](querying-data-with-the-sqldatasource-control-vb/_static/image20.gif)

**图 14**： 你可以以图形方式构造查询使用查询生成器


指定的查询之后, 单击下一步转到测试查询屏幕。 单击完成以完成 SqlDataSource 向导。

完成向导后，GridView 将具有三个 BoundFields 添加到它显示`ProductID`， `ProductName`，和`CategoryName`列从查询中返回，这导致以下声明性标记：


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample5.aspx)]


[![GridView 显示每个产品的 ID、 名称和关联的类别名称](querying-data-with-the-sqldatasource-control-vb/_static/image22.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image21.gif)

**图 15**: GridView 显示每个产品的 ID、 名称和关联的类别名称 ([单击以查看实际尺寸的图像](querying-data-with-the-sqldatasource-control-vb/_static/image23.gif))


## <a name="summary"></a>摘要

在本教程中我们已了解如何查询并显示使用 SqlDataSource 控件的数据。 如 ObjectDataSource，SqlDataSource 用作代理，提供一种用于访问数据的声明性方式。 其属性指定要连接到的数据库和 SQL`SELECT`查询执行; 通过属性窗口或通过使用配置数据源向导可以指定它们。

`SELECT`从指定的查询的查询示例，我们在本教程中检查返回的所有记录。 SqlDataSource 控件，但是，可以包括`WHERE`子句和参数，其值以编程方式分配，或自动请求从指定的源。 我们将研究如何创建并在下一教程中使用参数化的查询 ！

尽情享受编程 ！

## <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [访问关系数据库数据](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [SqlDataSource 控件概述](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [SqlDataSource 控件的 ASP.NET 快速入门教程：](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Web.config`<connectionStrings>`元素](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [数据库连接字符串引用](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Susan Connery、 伯纳黛特 Leigh 和 David Suru。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
[下一页](using-parameterized-queries-with-the-sqldatasource-vb.md)
