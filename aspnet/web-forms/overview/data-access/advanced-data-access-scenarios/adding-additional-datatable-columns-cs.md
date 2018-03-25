---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
title: "添加其他数据表列 (C#) |Microsoft 文档"
author: rick-anderson
description: "使用 TableAdapter 向导时创建的类型化数据集，相应的数据表中的主数据库查询返回的列。 但存在..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 615f3361-f21f-4338-8bc1-fce8ae071de9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 2982af897b433706889cb4eda79dcb4e76baea62
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="adding-additional-datatable-columns-c"></a>添加其他数据表列 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_CS.zip)或[下载 PDF](adding-additional-datatable-columns-cs/_static/datatutorial70cs1.pdf)

> 使用 TableAdapter 向导时创建的类型化数据集，相应的数据表中的主数据库查询返回的列。 但有时当 DataTable 需要包含额外的列。 在本教程中我们了解为什么存储的过程时，建议使用我们需要其他数据表列。


## <a name="introduction"></a>介绍

当为类型化数据集添加 TableAdapter，相应的 DataTable 的架构由 TableAdapter s 主查询决定。 例如，如果主查询返回数据字段*A*， *B*，和*C*，DataTable 将具有三个名为的相应列*A*，*B*，和*C*。除了其主查询 TableAdapter 可以包括其他可能返回基于某些参数的数据子集的查询。 例如，于`ProductsTableAdapter`s 主查询，返回有关所有产品的信息，它还包含等方法`GetProductsByCategoryID(categoryID)`和`GetProductByProductID(productID)`，其返回基于提供的参数的特定产品信息。

也在所有 TableAdapter 的方法返回相同或比主查询中指定的更少数据字段的工作将 DataTable 的架构反映 TableAdapter s 主查询的模型。 如果需要返回其他数据字段的 TableAdapter 方法，然后我们应 DataTable 的架构也相应扩展。 在[母版/详细介绍 Master 记录项目符号列表使用详细信息 DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)我们添加了一种对方法的教程`CategoriesTableAdapter`返回`CategoryID`， `CategoryName`，和`Description`中定义的数据字段加号主查询`NumberOfProducts`，报告的每个类别关联的产品数目的其他数据字段。 我们手动添加到新列`CategoriesDataTable`目的是捕获`NumberOfProducts`数据字段从这一新方法的值。

中所述[上载文件](../working-with-binary-files/uploading-files-cs.md)教程，但是必须格外小心使用 Tableadapter 使用临时 SQL 语句，并且让其数据字段不精确匹配主查询的方法。 如果重新运行 TableAdapter 配置向导，它将更新所有 TableAdapter 的方法，使其数据字段列表与匹配的主查询。 因此，任何的自定义的列列表的方法将还原到的主查询的列列表并不返回预期的数据。 使用存储的过程时，就不会发生此问题。

在本教程中我们将查看如何扩展 DataTable 的架构以包括其他列。 由于脆弱性 TableAdapter 使用临时 SQL 语句时，在本教程中我们将使用存储的过程。 请参阅[创建新的存储过程类型化数据集 s Tableadapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)和[使用现有存储过程类型化数据集 s Tableadapter](https://data-access/tutorials/using-existing-stored-procedures-for-the-typed-dataset-amp-rsquo-s-tableadapters-cs)教程有关详细信息配置 TableAdapter 以使用存储的过程。

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>步骤 1： 添加`PriceQuartile`列`ProductsDataTable`

在*创建新的存储过程类型化数据集 s Tableadapter*我们创建名为类型化数据集的教程`NorthwindWithSprocs`。 此数据集当前包含两个数据表：`ProductsDataTable`和`EmployeesDataTable`。 `ProductsTableAdapter`有以下三个方法：

- `GetProducts`-在主查询，返回中的所有记录`Products`表
- `GetProductsByCategoryID(categoryID)`-返回使用指定的所有产品*categoryID*。
- `GetProductByProductID(productID)`-返回具有指定的特定产品*productID*。

主查询和两个附加方法将返回相同的数据字段，即所有中的列集`Products`表。 有无相关子查询或`JOIN`拉取来自的相关的数据的 s`Categories`或`Suppliers`表。 因此，`ProductsDataTable`对应的列中每个字段`Products`表。

对于本教程，让 s 将方法添加到`ProductsTableAdapter`名为`GetProductsWithPriceQuartile`，返回所有产品。 除了标准产品数据字段，`GetProductsWithPriceQuartile`还将包括`PriceQuartile`指示产品的价格处于下哪些四分位数的数据字段。 例如，其价格均为中成本最高的 25%这些产品将具有`PriceQuartile`值为 1，尽管其价格归入底部 25%的那些都将有的值为 4。 我们担心如何创建存储的过程来返回此信息之前，但是，我们首先需要更新`ProductsDataTable`可包含的列来保存`PriceQuartile`结果时`GetProductsWithPriceQuartile`使用方法。

打开`NorthwindWithSprocs`数据集，然后右键单击`ProductsDataTable`。 从上下文菜单中选择添加，然后挑选列。


[![将新列添加到 ProductsDataTable](adding-additional-datatable-columns-cs/_static/image2.png)](adding-additional-datatable-columns-cs/_static/image1.png)

**图 1**： 添加到一个新列`ProductsDataTable`([单击以查看实际尺寸的图像](adding-additional-datatable-columns-cs/_static/image3.png))


这会将新列添加到名为的类型的 Column1 DataTable `System.String`。 我们需要更新此列名的 PriceQuartile 和其类型设置为`System.Int32`因为它将用于保存介于 1 和 4 之间的数字。 选择中的新添加列`ProductsDataTable`并从属性窗口中，设置`Name`PriceQuartile 属性和`DataType`属性`System.Int32`。


[![设置新的列的名称和数据类型属性](adding-additional-datatable-columns-cs/_static/image5.png)](adding-additional-datatable-columns-cs/_static/image4.png)

**图 2**： 设置新列 s`Name`和`DataType`属性 ([单击以查看实际尺寸的图像](adding-additional-datatable-columns-cs/_static/image6.png))


如图 2 所示，有其他属性，可以设置，如是否列中的值必须是唯一的如果列是自动递增列，是否数据库`NULL`值允许，依次类推。 将保留这些值设置为各自的默认值。

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>步骤 2： 创建`GetProductsWithPriceQuartile`方法

现在，`ProductsDataTable`已更新，包括`PriceQuartile`列中，我们已准备好创建`GetProductsWithPriceQuartile`方法。 启动的 TableAdapter 上右键单击并从上下文菜单中选择添加查询。 这将打开 TableAdapter 查询配置向导中，首先让我们是否我们要使用的临时 SQL 语句或新的或现有的存储的过程。 由于我们不尚未有一个存储的过程返回价格四分位数数据，让我们来允许为我们创建此存储的过程的 TableAdapter。 选择创建新存储的过程选项，然后单击下一步。


[![指示 TableAdapter 向导以创建为我们的存储的过程](adding-additional-datatable-columns-cs/_static/image8.png)](adding-additional-datatable-columns-cs/_static/image7.png)

**图 3**： 指示 TableAdapter 向导以创建存储过程为我们 ([单击以查看实际尺寸的图像](adding-additional-datatable-columns-cs/_static/image9.png))


在后续屏幕中，显示在图 4 中，向导将要求我们要添加查询的类型。 由于`GetProductsWithPriceQuartile`方法将返回所有列和记录从`Products`表，请选中它返回行选项，然后单击下一步。


[![我们的查询将该返回多个行的 SELECT 语句](adding-additional-datatable-columns-cs/_static/image11.png)](adding-additional-datatable-columns-cs/_static/image10.png)

**图 4**： 我们的查询将会`SELECT`语句该返回多个行 ([单击以查看实际尺寸的图像](adding-additional-datatable-columns-cs/_static/image12.png))


接下来我们将提示您输入`SELECT`查询。 在向导中输入以下查询：


[!code-sql[Main](adding-additional-datatable-columns-cs/samples/sample1.sql)]

上面的查询使用 SQL Server 2005 s 新[`NTILE`函数](https://msdn.microsoft.com/library/ms175126.aspx)若要将结果划分为四个组组由`UnitPrice`按降序排序的值。

遗憾的是，查询生成器不知道如何分析`OVER`关键字和分析上述查询时，将显示错误。 因此，上面的查询在向导中的文本框中直接输入而无需使用查询生成器。

> [!NOTE]
> 有关详细信息 NTILE 和 SQL Server 2005 s 其他排名函数，请参阅[Microsoft SQL Server 2005 返回排名结果](http://www.4guysfromrolla.com/webtech/010406-1.shtml)和[排名函数部分](https://msdn.microsoft.com/library/ms189798.aspx)从[SQLServer 2005 联机丛书](https://msdn.microsoft.com/library/ms189798.aspx)。


在输入后`SELECT`查询并单击下一步，则向导会询问我们提供它将创建的存储过程的名称。 命名新的存储的过程`Products_SelectWithPriceQuartile`单击下一步。


[![存储的过程 Products_SelectWithPriceQuartile 名称](adding-additional-datatable-columns-cs/_static/image14.png)](adding-additional-datatable-columns-cs/_static/image13.png)

**图 5**： 命名存储过程`Products_SelectWithPriceQuartile`([单击以查看实际尺寸的图像](adding-additional-datatable-columns-cs/_static/image15.png))


最后，我们会提示名称的 TableAdapter 方法。 保留这两个填充 DataTable 并返回 DataTable 选中的复选框和名称方法`FillWithPriceQuartile`和`GetProductsWithPriceQuartile`。


[![名称的 TableAdapter s 方法，然后单击完成](adding-additional-datatable-columns-cs/_static/image17.png)](adding-additional-datatable-columns-cs/_static/image16.png)

**图 6**： 名称，TableAdapter 的方法并单击完成 ([单击以查看实际尺寸的图像](adding-additional-datatable-columns-cs/_static/image18.png))


与`SELECT`指定查询和存储的过程和 TableAdapter 方法名为，单击完成以完成向导。 此时可能会收到警告还是说，该向导中的两个`OVER`不支持 SQL 构造或语句。 可以忽略这些警告。

完成向导后，应包括 TableAdapter`FillWithPriceQuartile`和`GetProductsWithPriceQuartile`应包括名为的存储的过程的方法和数据库`Products_SelectWithPriceQuartile`。 花一些时间来验证 TableAdapter，确实包含此新方法和存储的过程已正确添加到数据库。 当检查数据库中，如果你看不到存储的过程尝试右键单击的存储过程文件夹并选择刷新。


![验证已将新方法添加到 TableAdapter](adding-additional-datatable-columns-cs/_static/image19.png)

**图 7**： 验证已将新方法添加到 TableAdapter


[![确保数据库包含 Products_SelectWithPriceQuartile 存储过程](adding-additional-datatable-columns-cs/_static/image21.png)](adding-additional-datatable-columns-cs/_static/image20.png)

**图 8**： 确保数据库包含`Products_SelectWithPriceQuartile`存储过程 ([单击以查看实际尺寸的图像](adding-additional-datatable-columns-cs/_static/image22.png))


> [!NOTE]
> 而不临时的 SQL 语句中使用存储的过程的好处之一是，重新运行 TableAdapter 配置向导将不会修改存储的过程列列表。 此方法右键单击 TableAdapter，从上下文菜单以启动向导，选择配置选项，然后单击完成来完成此进行验证。 接下来，请转到该数据库并查看`Products_SelectWithPriceQuartile`存储过程。 请注意尚未修改其列列表。 以前我们一直在使用临时 SQL 语句，重新运行 TableAdapter 配置向导将恢复此查询的列列表以匹配主查询列列表中，从而从使用的查询中删除 NTILE 语句`GetProductsWithPriceQuartile`方法。


当数据访问层 s`GetProductsWithPriceQuartile`调用方法、 TableAdapter 执行`Products_SelectWithPriceQuartile`存储过程，并将添加到行`ProductsDataTable`为每个返回的记录。 存储过程返回的数据字段映射到`ProductsDataTable`的列。 由于没有`PriceQuartile`数据字段从存储的过程中，返回其值分配给`ProductsDataTable`s`PriceQuartile`列。

对于其查询不会返回这些 TableAdapter 方法`PriceQuartile`数据字段，`PriceQuartile`列的值是由指定的值其`DefaultValue`属性。 如图 2 所示，此值设置为`DBNull`，默认值。 如果您希望使用不同的默认值，只需设置`DefaultValue`属性相应地。 请确保`DefaultValue`值有效，并在给定列 s `DataType` (即，`System.Int32`为`PriceQuartile`列)。

此时我们已向数据表添加一个额外的列执行所需的步骤。 若要验证此附加列按预期方式工作，让我们来创建 ASP.NET 页显示每个产品的名称、 价格和价格四分位数。 我们在此之前，不过，我们首先需要更新业务逻辑层，若要包括到 DAL s 调用方法`GetProductsWithPriceQuartile`方法。 我们将在步骤 3 中，接下来，更新 BLL，然后在步骤 4 中创建 ASP.NET 页。

## <a name="step-3-augmenting-the-business-logic-layer"></a>步骤 3： 补充业务逻辑层

我们使用新之前`GetProductsWithPriceQuartile`方法与表示层中，我们首先应添加相应的方法为 BLL。 打开`ProductsBLLWithSprocs`类文件并添加以下代码：


[!code-csharp[Main](adding-additional-datatable-columns-cs/samples/sample2.cs)]

与中的其他数据检索方法类似`ProductsBLLWithSprocs`、`GetProductsWithPriceQuartile`方法只需调用 DAL s 对应`GetProductsWithPriceQuartile`方法并返回其结果。

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>步骤 4： 在 ASP.NET 网页中显示的价格四分位数信息

加上 BLL 重新已准备好创建显示每个产品的价格四分位数的 ASP.NET 页完成我们。 打开`AddingColumns.aspx`页面`AdvancedDAL`文件夹，然后拖动一个 GridView 从工具箱中拖动到设计器中，设置其`ID`属性`Products`。 从 GridView s 智能标记，请将其绑定到名为新 ObjectDataSource `ProductsDataSource`。 配置对象数据源以使用`ProductsBLLWithSprocs`类的`GetProductsWithPriceQuartile`方法。 因为这将为只读的网格，在更新中，INSERT、 设置的下拉列表，并删除选项卡添加到 （无）。


[![配置对象数据源以使用 ProductsBLLWithSprocs 类](adding-additional-datatable-columns-cs/_static/image24.png)](adding-additional-datatable-columns-cs/_static/image23.png)

**图 9**： 配置使用 ObjectDataSource`ProductsBLLWithSprocs`类 ([单击以查看实际尺寸的图像](adding-additional-datatable-columns-cs/_static/image25.png))


[![从 GetProductsWithPriceQuartile 方法中检索产品信息](adding-additional-datatable-columns-cs/_static/image27.png)](adding-additional-datatable-columns-cs/_static/image26.png)

**图 10**： 中检索产品信息`GetProductsWithPriceQuartile`方法 ([单击以查看实际尺寸的图像](adding-additional-datatable-columns-cs/_static/image28.png))


完成配置数据源向导后，Visual Studio 将自动添加 BoundField 或 CheckBoxField 到 GridView 为每个方法返回的数据字段。 这些数据字段之一是`PriceQuartile`，这是我们将添加到列`ProductsDataTable`在步骤 1 中。

编辑删除的 GridView 的字段以外的所有`ProductName`， `UnitPrice`，和`PriceQuartile`BoundFields。 配置`UnitPrice`BoundField 设置其值作为货币的格式，并让`UnitPrice`和`PriceQuartile`BoundFields 右和中心-对齐，分别。 最后，更新剩余 BoundFields`HeaderText`属性添加到产品、 价格和价格四分位数，分别。 另外，请检查从 GridView s 智能标记启用排序复选框。

这些修改后的 GridView 和 ObjectDataSource s 声明性标记应如下所示：


[!code-aspx[Main](adding-additional-datatable-columns-cs/samples/sample3.aspx)]

图 11 显示时通过浏览器访问此页。 请注意，最初，产品按排序与每个分配适当的产品的降序其价格`PriceQuartile`值。 这是当然的此数据可以进行分类按其他条件仍反映价格方面的产品的排名的价格四分位数列值 （请参阅图 12）。


[![通过其价格订购的产品](adding-additional-datatable-columns-cs/_static/image30.png)](adding-additional-datatable-columns-cs/_static/image29.png)

**图 11**： 按其价格排序的产品 ([单击以查看实际尺寸的图像](adding-additional-datatable-columns-cs/_static/image31.png))


[![产品按其名称进行排序](adding-additional-datatable-columns-cs/_static/image33.png)](adding-additional-datatable-columns-cs/_static/image32.png)

**图 12**： 按其名称进行排序的产品 ([单击以查看实际尺寸的图像](adding-additional-datatable-columns-cs/_static/image34.png))


> [!NOTE]
> 使用几行代码我们无法增加 GridView，以便它着色根据产品行其`PriceQuartile`值。 我们可能颜色这些产品的第一个四分位数浅绿色，第二个四分位数浅黄色中的那些，等等。 我建议你花一些时间来添加此功能。 如果你需要在格式设置 GridView 刷新程序，请查阅[自定义格式设置基于时数据](../custom-formatting/custom-formatting-based-upon-data-cs.md)教程。


## <a name="an-alternative-approach---creating-another-tableadapter"></a>一种方法-创建另一个 TableAdapter

如我们看到在本教程中，当主查询将方法添加到返回之外的数据字段的 TableAdapter 拼的不同而不同，我们可以添加 DataTable 对应的列。 这种方法，但是，工作得只有少量的 TableAdapter 中返回不同的数据字段的方法和从主查询太多，这些备用数据字段不发生更改。

而不是将列添加到数据表中，可以改为将另一个 TableAdapter 添加到包含从第一个 TableAdapter 的方法的返回不同的数据字段的数据集。 对于本教程，而不是添加`PriceQuartile`列`ProductsDataTable`(其中仅使用由`GetProductsWithPriceQuartile`方法)，我们无法向名为数据集添加其他 TableAdapter`ProductsWithPriceQuartileTableAdapter`使用`Products_SelectWithPriceQuartile`存储为其主查询的过程。 获取产品信息价格四分位数，所需的 ASP.NET 页将使用`ProductsWithPriceQuartileTableAdapter`，而那些没有无法继续使用`ProductsTableAdapter`。

通过添加新的 TableAdapter，数据表保持 untarnished 镜像，并且其列精确由其 TableAdapter 的 s 方法返回的数据字段。 但是，其他 Tableadapter 会导致重复任务和功能。 例如，如果这些 ASP.NET 页显示`PriceQuartile`列还需要提供插入、 更新和删除的支持，`ProductsWithPriceQuartileTableAdapter`将需要其`InsertCommand`， `UpdateCommand`，和`DeleteCommand`属性正确配置。 虽然这些属性将镜像`ProductsTableAdapter`s，此配置引入了一个额外步骤。 此外，有两种方法，若要更新，删除，或通过向数据库-添加产品现在`ProductsTableAdapter`和`ProductsWithPriceQuartileTableAdapter`类。

本教程中下载内容还包括`ProductsWithPriceQuartileTableAdapter`类`NorthwindWithSprocs`演示了此备用方法的数据集。

## <a name="summary"></a>摘要

在大多数情况下，所有 TableAdapter 中的方法将返回组相同的数据字段，但有特定的方法或两个可能需要时要返回的其他字段的时间。 例如，在[母版/详细介绍 Master 记录项目符号列表使用详细信息 DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)我们添加了一种对方法的教程`CategoriesTableAdapter`，除了返回的主查询的数据字段`NumberOfProducts`字段，报告的每个类别关联的产品数目。 在本教程中我们查看添加中的方法`ProductsTableAdapter`返回`PriceQuartile`除了主查询的数据字段的字段。 若要捕获更多数据字段由方法返回的 TableAdapter s 我们需要将对应的列添加到数据表。

如果你打算手动将列添加到 DataTable，建议 TableAdapter 使用存储的过程。 如果 TableAdapter 使用临时 SQL 语句，任何时候 TableAdapter 配置向导将运行所有数据字段列表恢复到主查询返回的数据字段的方法。 此问题不会扩展到存储过程，这就是为什么建议并已在本教程中使用它们。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已徐 Schmidt、 Jacky Goor、 伯纳黛特 Leigh 和希尔顿 Giesenow。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](updating-the-tableadapter-to-use-joins-cs.md)
[下一页](working-with-computed-columns-cs.md)
