---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: "使用现有存储过程的类型化数据集的 Tableadapter (VB) |Microsoft 文档"
author: rick-anderson
description: "在以前的教程，我们学习了如何使用 TableAdapter 向导来生成新的存储的过程。 在本教程中，我们将了解如何同一个 TableAdapter..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: 5d1be6c30cda5a06087516210a77f48b6a3fe45b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>使用现有存储过程的类型化数据集的 Tableadapter (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip)或[下载 PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> 在以前的教程，我们学习了如何使用 TableAdapter 向导来生成新的存储的过程。 在本教程中我们了解如何相同的 TableAdapter 向导可以使用现有存储过程。 我们还了解了如何将新的存储的过程手动添加到我们的数据库。


## <a name="introduction"></a>介绍

在[前面教程](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)我们已了解如何类型化数据集的 Tableadapter 无法配置为使用存储的过程来访问数据，而不是临时 SQL 语句。 具体而言，我们探讨了如何使 TableAdapter 向导自动创建这些存储的过程。 移植到 ASP.NET 2.0 的旧版应用程序时，或生成现有的数据模型周围的 ASP.NET 2.0 网站时，很有数据库已包含我们需要的存储的过程。 或者，你可能更倾向手动或通过某些工具自动生成的存储的过程的 TableAdapter 向导以外创建你的存储的过程。

在本教程中我们将查看如何配置以使用现有存储的过程的 TableAdapter。 由于 Northwind 数据库只有一小部分的内置存储过程，我们还将在需要手动将新的存储的过程添加到数据库中通过 Visual Studio 环境的步骤。 让我们来开始 ！

> [!NOTE]
> 在[在事务中包装数据库修改](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)教程，我们将方法添加到 TableAdapter 为支持事务 (`BeginTransaction`， `CommitTransaction`，依次类推)。 或者，可以完全在对数据访问层代码无需进行修改的存储过程管理事务。 在本教程中我们将探讨用于执行存储的过程的 s 语句范围内的事务的 T-SQL 命令。


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>步骤 1： 将存储的过程添加到 Northwind 数据库

Visual Studio 便于向数据库添加新的存储的过程。 允许 s 将新的存储的过程添加到 Northwind 数据库返回中的所有列`Products`具有特定的表`CategoryID`值。 从服务器资源管理器窗口中，展开 Northwind 数据库，以显示其文件夹-数据库关系图、 表、 视图和等的。 正如我们看到在前面的教程，该存储过程文件夹将包含的数据库 s 的现有存储的过程。 若要添加新的存储的过程，只需右键单击存储过程文件夹，然后从上下文菜单中选择添加新存储过程选项。


[![右键单击存储的过程文件夹并添加新的存储的过程](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**图 1**： 右键单击存储过程文件夹并添加新的存储过程 ([单击以查看实际尺寸的图像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png))


如图 1 所示，选择添加新存储过程选项将打开脚本窗口在 Visual Studio 中创建存储的过程所需的 SQL 脚本的轮廓。 它是我们的职责充实此脚本并执行它的位置的存储的过程将添加到数据库。

输入以下脚本：


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

此脚本，在执行时，将向名为 Northwind 数据库中添加新的存储的过程`Products_SelectByCategoryID`。 此存储的过程接受一个输入的参数 (`@CategoryID`，类型的`int`) 和它返回所有这些产品以匹配的字段`CategoryID`值。

若要执行此`CREATE PROCEDURE`编写脚本和存储的过程添加到数据库、 单击工具栏中的保存图标或按 Ctrl + S。 这样，存储过程文件夹刷新过程中，做之后显示新创建的存储过程。 此外，在窗口中的脚本将更改从种微妙`CREATE PROCEDURE dbo.Products_SelectProductByCategoryID`到`ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`。 `CREATE PROCEDURE`向数据库中，添加新的存储的过程时`ALTER PROCEDURE`更新现有。 由于脚本开始已更改为`ALTER PROCEDURE`、 更改的存储的过程输入参数或 SQL 语句，并单击保存图标将使用这些更改更新存储的过程。

图 2 显示了 Visual Studio 后`Products_SelectByCategoryID`已保存存储的过程。


[![已添加到数据库中存储的过程 Products_SelectByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**图 2**: 存储过程`Products_SelectByCategoryID`已添加到数据库 ([单击以查看实际尺寸的图像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>步骤 2： 配置要使用现有存储的过程的 TableAdapter

现在，`Products_SelectByCategoryID`存储的过程已添加到数据库，我们可以将配置我们的数据访问层，以便其某个方法调用时使用此存储的过程。 具体而言，我们将添加`GetProducstByCategoryID(<_i22_>categoryID)<!--_i22_-->`方法`ProductsTableAdapter`中`NorthwindWithSprocs`类型化数据集调用`Products_SelectByCategoryID`存储我们刚刚创建的过程。

首先打开`NorthwindWithSprocs`数据集。 右键单击`ProductsTableAdapter`和选择要启动 TableAdapter 查询配置向导中的添加查询。 在[前面教程](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)我们已选择具有为我们创建一个新的存储的过程的 TableAdapter。 对于本教程，但是，我们想要将新的 TableAdapter 方法连接到现有`Products_SelectByCategoryID`存储过程。 因此，从向导 s 第一步中选择使用现有存储的过程选项，然后单击下一步。


[![选择使用现有存储的过程选项](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**图 3**： 选择使用现有存储过程选项 ([单击以查看实际尺寸的图像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))


下面的屏幕提供存储的过程与数据库 s 填充下拉列表。 选择存储的过程列出其在左侧和右侧返回 （如果有） 的数据字段的输入的参数。 选择`Products_SelectByCategoryID`存储过程从列表并单击下一步。


[![选取 Products_SelectByCategoryID 存储过程](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**图 4**： 选取`Products_SelectByCategoryID`存储过程 ([单击以查看实际尺寸的图像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))


下一个屏幕要求我们什么类型的数据返回的存储过程和此处我们答案确定 TableAdapter 的方法返回的类型。 例如，如果我们指示返回表格数据，该方法将返回`ProductsDataTable`使用存储过程返回的记录填充的实例。 与此相反，如果我们指示此存储的过程返回单个值将返回 TableAdapter`Object`分配存储过程返回的第一个记录的第一列中的值。

由于`Products_SelectByCategoryID`存储的过程返回的所有产品的一种特定类别的详细信息，选择第一次答案-表格将数据-然后单击下一步。


[![指示存储的过程返回表格数据](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**图 5**： 指示存储过程返回表格数据 ([单击以查看实际尺寸的图像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))


所有这些剩下是以指示方法模式，以使用这些方法的名称的跟。 保留这两个填充 DataTable 选项检查，但重命名的方法的 DataTable 和返回`FillByCategoryID`和`GetProductsByCategoryID`。 然后单击下一步以查看该向导将执行的任务的摘要。 如果一切看上去正确，请单击完成。


[![名称方法 FillByCategoryID 和 GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**图 6**： 命名方法`FillByCategoryID`和`GetProductsByCategoryID`([单击以查看实际尺寸的图像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


> [!NOTE]
> 我们刚刚创建的 TableAdapter 方法`FillByCategoryID`和`GetProductsByCategoryID`，预期的类型输入的参数`Integer`。 此输入的参数值传递给该存储过程通过其`@CategoryID`参数。 如果你修改`Products_SelectByCategory`存储过程的参数，你将需要也更新这些 TableAdapter 方法的参数。 中所述[以前一教程](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)，这可以在两种方式之一： 通过手动添加或删除参数从参数集合中或通过重新运行 TableAdapter 向导。


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>步骤 3： 添加`GetProductsByCategoryID(categoryID)`BLL 方法

与`GetProductsByCategoryID`DAL 方法完成下, 一步是提供对业务逻辑层中的此方法的访问。 打开`ProductsBLLWithSprocs`类文件并添加以下方法：


[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

此 BLL 方法只返回`ProductsDataTable`从返回`ProductsTableAdapter`s`GetProductsByCategoryID`方法。 `DataObjectMethodAttribute`属性提供 ObjectDataSource s 配置数据源向导所使用的元数据。 具体而言，此方法会在选择选项卡上的下拉列表中。

## <a name="step-4-displaying-products-by-category"></a>步骤 4： 按类别显示产品

若要测试新添加`Products_SelectByCategoryID`存储的过程和相应 DAL 和 BLL 方法，让我们来创建 ASP.NET 页，其中包含的 DropDownList 和 GridView。 尽管 GridView 将显示属于所选类别的产品，下拉列表将列出所有数据库中的类别。

> [!NOTE]
> 我们已创建主/从接口在前面的教程中使用 DropDownLists。 有关在实现主/详细信息报表的更深入探讨，请参阅[主/详细信息筛选与 DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md)教程。


打开`ExistingSprocs.aspx`页面`AdvancedDAL`文件夹，然后拖动 DropDownList 从工具箱中拖动到设计器。 设置的 DropDownList s`ID`属性`Categories`及其`AutoPostBack`属性`True`。 接下来，从其智能标记，将下拉列表绑定到名为新 ObjectDataSource `CategoriesDataSource`。 配置对象数据源，以便它检索从其数据`CategoriesBLL`类的`GetCategories`方法。 在更新中，INSERT、 设置的下拉列表和删除选项卡添加到 （无）。


[![从 CategoriesBLL 类的 GetCategories 方法中检索数据](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**图 7**： 从检索数据`CategoriesBLL`类 s`GetCategories`方法 ([单击以查看实际尺寸的图像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))


[![在更新中，INSERT、 设置的下拉列表和删除选项卡到 （无）](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**图 8**： 设置的下拉列表中更新、 插入和删除选项卡为 （无） ([单击以查看实际尺寸的图像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))


完成对象数据源向导后，配置要显示的 DropDownList`CategoryName`数据字段并使用`CategoryID`字段作为`Value`每个`ListItem`。

此时，应类似于以下的 DropDownList 和 ObjectDataSource s 声明性标记：


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

接下来，将 GridView 拖到设计器中，将其放在下拉列表下方。 设置 GridView s`ID`到`ProductsByCategory`和从其智能标记，请将其绑定到名为新 ObjectDataSource `ProductsByCategoryDataSource`。 配置`ProductsByCategoryDataSource`ObjectDataSource 使用`ProductsBLLWithSprocs`类，让它检索其数据使用`GetProductsByCategoryID(categoryID)`方法。 由于此 GridView 将仅用于显示数据，，您可以在更新中，INSERT、 设置的下拉列表并删除选项卡添加到 （无），然后单击下一步。


[![配置对象数据源以使用 ProductsBLLWithSprocs 类](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**图 9**： 配置使用 ObjectDataSource`ProductsBLLWithSprocs`类 ([单击以查看实际尺寸的图像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))


[![从 GetProductsByCategoryID(categoryID) 方法中检索数据](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**图 10**： 从检索数据`GetProductsByCategoryID(categoryID)`方法 ([单击以查看实际尺寸的图像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))


在选择选项卡中选择的方法需要一个参数，以便向导的最后一步，提示我们为参数的源。 将参数源下拉列表设置为控件，然后选择`Categories`从 ControlID 下拉列表控件。 单击完成以完成向导。


[![类别 DropDownList 用作 categoryID 参数的源](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**图 11**： 使用`Categories`作为源的 DropDownList`categoryID`参数 ([单击以查看实际尺寸的图像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


完成对象数据源向导之后，Visual Studio 将为每个产品数据字段添加 BoundFields 和 CheckBoxField。 随意根据你的自定义这些字段。

请访问通过浏览器页面。 当来访选择饮料类别的页以及在网格中列出的相应产品。 将下拉列表更改为备用的类别中，作为图 12 显示、 导致回发和重新加载网格与新选择的类别的产品。


[![显示生成类别中的产品](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**图 12**： 显示产品的生成类别 ([单击以查看实际尺寸的图像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>步骤 5： 在事务范围内包装存储的过程的语句

在[在事务中包装数据库修改](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md)我们讨论为执行一系列的范围内的事务的数据库修改语句的技术的教程。 回想一下，所有成功要么执行之下的事务的修改或所有发生故障，请确保原子性。 使用事务的技术包括：

- 使用中的类`System.Transactions`命名空间，
- 具有数据访问层使用 ADO.NET 类，如`SqlTransaction`，和
- 添加直接在存储过程内的 T-SQL 的事务命令

*在事务中包装数据库修改*教程在 DAL 中使用的 ADO.NET 类。 本教程的其余部分检查如何管理使用存储过程中的从 T-SQL 命令在某个事务。

用于手动启动、 提交，以及回滚事务密钥的三个 SQL 命令都是`BEGIN TRANSACTION`， `COMMIT TRANSACTION`，和`ROLLBACK TRANSACTION`分别。 使用事务从存储过程，我们需要将应用以下模式中的时，借助 ADO.NET 的方法，如：

1. 指示启动事务。
2. 执行构成事务的 SQL 语句。
3. 如果用任何一种从步骤 2 中，回滚事务的语句错误。
4. 如果所有步骤 2 中的语句完成而未出现错误，则提交事务。

可以使用以下模板的 T-SQL 语法中实现此模式：


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

通过定义该模板启动`TRY...CATCH`一直阻止，请熟悉 SQL Server 2005 的构造。 与类似`Try...Catch`阻止在 Visual Basic 中，SQL`TRY...CATCH`执行中的语句块`TRY`块。 如果任何语句将引发错误，控制立即转移到其中`CATCH`块。

如果没有执行 SQL 语句，该构成事务，错误`COMMIT TRANSACTION`语句提交所做的更改并完成该事务。 如果，但是，一条语句会导致出现错误，`ROLLBACK TRANSACTION`中`CATCH`块将数据库返回到其事务开始前的状态。 存储的过程也会引发错误使用[RAISERROR 命令](https://msdn.microsoft.com/library/ms178592.aspx)，这将导致`SqlException`若要在应用程序中引发。

> [!NOTE]
> 由于`TRY...CATCH`块是新的 SQL Server 2005，如果你使用的较旧版本的 Microsoft SQL Server，则上面的模板将不会起作用。 如果你未使用 SQL Server 2005，请查阅[在 SQL Server 存储过程中管理事务](http://www.4guysfromrolla.com/webtech/080305-1.shtml)会使用其他版本的 SQL Server 的模板。


让我们来看一下的具体示例。 外键约束之间存在`Categories`和`Products`表，这意味着，每个`CategoryID`字段`Products`表必须映射到`CategoryID`中的值`Categories`表。 任何违反此约束，例如，尝试删除具有关联的产品类别的操作导致违反外键约束。 若要验证这一点，重新访问二进制数据部分使用中的更新和删除现有的二进制数据示例 (`~/BinaryData/UpdatingAndDeleting.aspx`)。 此页列出每个类别 （请参阅图 13） 编辑和删除按钮，以及系统中，但如果你尝试删除具有关联的产品的 Beverages-如类别删除失败由于外键约束冲突 （请参阅图 14）。


[![每个类别都显示在具有编辑和删除按钮 GridView](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**图 13**： 每个类别都显示在具有编辑和删除按钮 GridView ([单击以查看实际尺寸的图像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))


[![无法删除具有现有产品的类别](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**图 14**： 无法删除具有现有产品的类别 ([单击以查看实际尺寸的图像](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))


不过，假设我们想要允许删除而不考虑它们是否具有关联的产品的类别。 应删除与产品类别，假设我们想要同时删除其现有的产品 (尽管另一种方法将是只需设置其产品`CategoryID`值复制到`NULL`)。 无法通过外键约束的级联规则实现此功能。 或者，我们无法创建存储的过程接受的`@CategoryID`输入的参数并调用时，显式删除所有关联的产品，然后指定的类别。

此类存储过程我们首次尝试可能如下所示：


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

虽然这肯定将删除关联的产品和类别，它不会之下的事务。 假设上是一些其他外键约束`Categories`，将禁止删除的特定`@CategoryID`值。 问题是，在这种情况下的所有产品之前，将删除我们尝试删除该类别。 最终结果是，针对此类类别中，此存储的过程将删除其所有产品类别保持，因为它仍有相关的一些其他表中的记录。

如果存储的过程已包装在事务范围内但是，到删除`Products`表将在遇到时未能删除上回滚`Categories`。 以下存储的过程脚本使用事务以确保这两者之间的原子性`DELETE`语句：


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

花些时间添加`Categories_Delete`存储到 Northwind 数据库的过程。 请返回到步骤 1 有关将存储的过程添加到数据库的说明。

## <a name="step-6-updating-thecategoriestableadapter"></a>步骤 6： 更新`CategoriesTableAdapter`

尽管我们已添加`Categories_Delete`到数据库，DAL 的存储的过程当前被配置为使用临时 SQL 语句来执行删除。 我们需要更新`CategoriesTableAdapter`并指示它使用`Categories_Delete`改为存储过程。

> [!NOTE]
> 本教程中前面我们已使用`NorthwindWithSprocs`数据集。 但该数据集仅包含单个实体， `ProductsDataTable`，并且我们需要使用类别。 因此，对于时我聊聊数据访问层 I m 引用本教程的其余部分`Northwind`数据集，我们首先创建中的一个[创建数据访问层](../introduction/creating-a-data-access-layer-vb.md)教程。


打开罗斯文数据集，选择`CategoriesTableAdapter`，并转到属性窗口。 属性窗口列表`InsertCommand`， `UpdateCommand`， `DeleteCommand`，和`SelectCommand`使用 TableAdapter，以及其名称和连接信息。 展开`DeleteCommand`属性以查看其详细信息。 如图 15 所示， `DeleteCommand` s`ComamndType`属性设置为文本，指示它发送的文本`CommandText`属性作为临时 SQL 查询。


![在属性窗口中查看其属性设计器中选择 CategoriesTableAdapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**图 15**： 选择`CategoriesTableAdapter`在设计器，以在属性窗口中查看其属性


若要更改这些设置，在属性窗口中选择 (DeleteCommand) 文本，并从下拉列表中选择 （新增功能）。 这将清除出的设置`CommandText`， `CommandType`，和`Parameters`属性。 接下来，设置`CommandType`属性`StoredProcedure`然后键入名称的存储过程`CommandText`(`dbo.Categories_Delete`)。 如果你请务必首先按此顺序中的输入属性`CommandType`然后`CommandText`-Visual Studio 将自动填充的参数集合。 如果按此顺序未输入这些属性，你将需要手动添加参数通过参数集合编辑器。 在任一情况下，它 s 比较明智的做法单击省略号以打开参数集合编辑器以验证是否正确的参数设置已进行更改 （请参阅图 16） 的参数属性。 如果看不到任何参数在对话框中，将添加`@CategoryID`参数手动 (不需要添加`@RETURN_VALUE`参数)。


![确保参数设置正确](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**图 16**： 确保参数设置正确


后已更新 DAL，删除分类自动将删除所有其关联的产品和这样之下的事务。 若要验证这一点，返回到更新和删除现有的二进制数据页，然后单击删除按钮的一个类别。 使用一个单个单击鼠标，将删除该类别和所有其关联的产品。

> [!NOTE]
> 在测试之前`Categories_Delete`存储的过程，这将删除数以及所选类别的产品，它可能比较明智的做法是，以使你的数据库的备份副本。 如果你使用`NORTHWND.MDF`数据库中`App_Data`，只需关闭 Visual Studio 并复制中的 MDF 和 LDF 文件`App_Data`某些其他文件夹。 正在测试的功能之后, 你可以还原数据库关闭 Visual Studio 和替换当前的 MDF 和 LDF 文件中`App_Data`使用备份副本。


## <a name="summary"></a>摘要

在 TableAdapter 的向导将自动为我们生成的存储的过程，可以在的时间时我们可能已有创建此类存储的过程或想改为创建它们手动或使用其他工具。 为了适应这种情况下，TableAdapter 还可以将配置为指向现有存储过程。 在本教程中我们介绍了如何将存储的过程手动添加到数据库中通过 Visual Studio 环境以及如何将 TableAdapter 的方法连接到这些存储过程。 我们还检查的 T-SQL 命令和脚本模式用于启动、 提交，和回滚从存储过程中的事务。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已希尔顿 Geisenow、 S ren 异世 Lauritsen 和 Teresa 墨。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
[下一页](updating-the-tableadapter-to-use-joins-vb.md)
