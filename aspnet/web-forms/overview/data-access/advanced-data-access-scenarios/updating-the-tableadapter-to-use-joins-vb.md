---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
title: "使用更新 TableAdapter 联接 (VB) |Microsoft 文档"
author: rick-anderson
description: "使用的数据库时，共有分布在多个表的请求数据。 若要从两个不同的表中检索数据我们可以使用..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: e624a3e0-061b-4efc-8b0e-5877f9ff6714
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
msc.type: authoredcontent
ms.openlocfilehash: 2e0698269c0a29c234f03dc56f7b63e7bc83d032
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="updating-the-tableadapter-to-use-joins-vb"></a>使用更新 TableAdapter 联接 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_VB.zip)或[下载 PDF](updating-the-tableadapter-to-use-joins-vb/_static/datatutorial69vb1.pdf)

> 使用的数据库时，共有分布在多个表的请求数据。 若要从两个不同的表中检索数据我们可以使用相关子查询或联接操作。 在本教程中我们比较相关子查询和联接语法着眼于如何创建在其主查询包含联接的 TableAdapter 之前。


## <a name="introduction"></a>介绍

与关系数据库我们感兴趣使用的数据通常跨越多个表格。 例如，显示产品信息时我们可能想要列出每个产品 s 相应的类别和供应商 s 的名称。 `Products`表具有`CategoryID`和`SupplierID`值，但实际的类别和供应商名称位于`Categories`和`Suppliers`表，分别。

若要从另一个，相关表中检索信息，我们可以使用*相关子查询*或`JOIN` *s*。 相关子查询是嵌套`SELECT`引用外部查询中的列的查询。 例如，在[创建数据访问层](../introduction/creating-a-data-access-layer-vb.md)教程，我们使用了中的两个相关子查询`ProductsTableAdapter`s 主查询以返回每个产品类别和供应商的名称。 A`JOIN`是一个 SQL 构造，会将两个不同的表中的相关的行合并。 我们使用`JOIN`中[查询数据与 SqlDataSource 控件](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb.md)教程，以显示与每个产品类别信息。

我们已从使用 abstained 的原因`JOIN`由于 TableAdapter 的向导自动生成的相应中的限制是与 Tableadapter `INSERT`， `UPDATE`，和`DELETE`语句。 更具体地说，如果 TableAdapter s 主查询包含任何`JOIN`s，TableAdapter 不能自动创建的临时 SQL 语句或存储的过程，以其`InsertCommand`， `UpdateCommand`，和`DeleteCommand`属性。

在此教程中我们将简要比较和对比相关子查询和`JOIN`之前探索如何创建包括的 TableAdapter 的 s`JOIN`中其主查询。

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>比较和对比相关子查询和`JOIN`s

回想一下，`ProductsTableAdapter`中的第一个教程中创建`Northwind`数据集使用的相关子查询以使重新每个产品 s 相应类别和供应商名称。 `ProductsTableAdapter` S 主查询如下所示。


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample1.sql)]

这两个相关子查询-`(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)`和`(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)`-是`SELECT`作为中外部的其他列返回单个值的每个产品的查询`SELECT`语句的列列表。

或者，`JOIN`可以用于返回每个产品 s 供应商和类别名称。 以下查询返回与上述相同的输出，但使用`JOIN`取代子查询的 s:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample2.sql)]

A`JOIN`合并从一个表具有基于某些条件的另一个表中记录的记录。 在上面的查询中，例如，`LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID`指示 SQL Server 合并每个类别的产品记录记录其`CategoryID`值与产品 s 匹配`CategoryID`值。 合并的结果，我们可以使用相应的类别字段中的每个产品 (如`CategoryName`)。

> [!NOTE]
> `JOIN`s 常用查询从关系数据库的数据时。 如果你不熟悉如何`JOIN`语法或需要有点画笔有关其用法，我建议[SQL Join 教程](http://www.w3schools.com/sql/sql_join.asp)在[W3 学校](http://www.w3schools.com/)。 此外值得读取是[`JOIN`基础知识](https://msdn.microsoft.com/library/ms191517.aspx)和[子查询基础知识](https://msdn.microsoft.com/library/ms189575.aspx)的部分[SQL 联机丛书](https://msdn.microsoft.com/library/ms130214.aspx)。


由于`JOIN`s 和相关子查询可以同时用于从其他表中检索相关的数据，许多开发人员左划伤着头和想知道要使用哪种方法。 所有 SQL 专家我已讨论以说过大致的相同，它不真正有意义在性能如 SQL Server 将产生大致相同的执行计划。 然后，其建议中，是使用你和你的团队在最熟悉的技术。 它值得注意的，保证此建议，而且传达后这些专家立即会 express 其首选项的`JOIN`随着相关子查询。

在生成数据访问层使用类型化数据集时，工具更好地工作，使用子查询时。 具体而言，TableAdapter 的向导将不自动生成对应`INSERT`， `UPDATE`，和`DELETE`语句，如果主查询包含任何`JOIN`s，但将自动生成这些语句时相关子查询使用。

若要浏览此缺点，创建临时类型化数据集在`~/App_Code/DAL`文件夹。 在 TableAdapter 配置向导的过程中选择要使用的临时 SQL 语句，然后输入以下`SELECT`查询 （请参见图 1）：


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample3.sql)]


[![输入主查询包含联接](updating-the-tableadapter-to-use-joins-vb/_static/image2.png)](updating-the-tableadapter-to-use-joins-vb/_static/image1.png)

**图 1**： 输入包含的主查询`JOIN`s ([单击以查看实际尺寸的图像](updating-the-tableadapter-to-use-joins-vb/_static/image3.png))


默认情况下，将自动创建 TableAdapter `INSERT`， `UPDATE`，和`DELETE`语句基于主查询。 如果单击高级按钮，可以看到此功能已启用。 尽管此设置，将不能够创建 TableAdapter `INSERT`， `UPDATE`，和`DELETE`语句因为主查询包含`JOIN`。


![输入主查询包含联接](updating-the-tableadapter-to-use-joins-vb/_static/image4.png)

**图 2**： 输入包含主查询`JOIN`s


单击完成以完成向导。 此时你的数据集设计器将包括与列与 DataTable 单个 TableAdapter 中返回的字段的每个`SELECT`查询的列列表。 这包括`CategoryName`和`SupplierName`，如图 3 所示。


![DataTable 包含用于在列列表中返回每个字段的列](updating-the-tableadapter-to-use-joins-vb/_static/image5.png)

**图 3**: DataTable 包括一个列，为每个字段中的列列表返回


TableAdapter 而 DataTable 有相应的列，缺少的值其`InsertCommand`， `UpdateCommand`，和`DeleteCommand`属性。 若要确认这一点，在设计器中的 TableAdapter 上单击，然后转到属性窗口。 此时您将看到， `InsertCommand`， `UpdateCommand`，和`DeleteCommand`属性设置为 （无）。


[![InsertCommand、 限于 UpdateCommand 和 DeleteCommand 属性设置为 （无）](updating-the-tableadapter-to-use-joins-vb/_static/image7.png)](updating-the-tableadapter-to-use-joins-vb/_static/image6.png)

**图 4**: `InsertCommand`， `UpdateCommand`，和`DeleteCommand`属性设置为 （无） ([单击以查看实际尺寸的图像](updating-the-tableadapter-to-use-joins-vb/_static/image8.png))


若要解决这种不足，我们可以手动提供的 SQL 语句和参数`InsertCommand`， `UpdateCommand`，和`DeleteCommand`通过属性窗口的属性。 或者，我们无法通过配置到 TableAdapter s 主查询启动*不*包括任何`JOIN`s。 这将允许`INSERT`， `UPDATE`，和`DELETE`要为我们自动生成语句。 完成向导后，我们无法然后手动更新 TableAdapter s`SelectCommand`从属性窗口使其包含`JOIN`语法。

在这种方法，它是非常脆弱重新配置向导中，自动生成通过使用临时 SQL 查询，因为任何时候，只要 TableAdapter s 主查询时`INSERT`， `UPDATE`，和`DELETE`语句会重新创建。 这意味着我们的 TableAdapter 上右击，从上下文菜单中，选择配置并再次完成向导时可能丢失的所有更高版本所做的自定义。

自动生成的 TableAdapter s 脆弱性`INSERT`， `UPDATE`，和`DELETE`语句，幸运的是，限制为临时 SQL 语句。 如果你 TableAdapter 使用存储的过程，你可以自定义`SelectCommand`， `InsertCommand`， `UpdateCommand`，或`DeleteCommand`存储过程和重新运行而无需担心将存储的过程的 TableAdapter 配置向导修改。

在接下来几个步骤，我们将创建 TableAdapter，最初，使用主查询的省略了任何`JOIN`s，以便相应插入、 更新和删除存储过程将自动生成。 然后，我们将更新`SelectCommand`因此，它使用`JOIN`从相关表返回其他列。 最后，我们将创建相应的业务逻辑层类，并演示如何使用 ASP.NET web 页中的 TableAdapter。

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>步骤 1： 创建使用简化的主查询的 TableAdapter

对于本教程中我们将添加 TableAdapter 和为强类型 DataTable`Employees`表中`NorthwindWithSprocs`数据集。 `Employees`表包含`ReportsTo`字段指定`EmployeeID`员工 s 管理器。 例如，员工 Anne 刘天妮具有`ReportTo`值为 5，即`EmployeeID`Steven 林丹。 因此，Anne 向 Steven，她的经理报告。 以及报告每个员工的`ReportsTo`值，我们可能还想要检索其管理器的名称。 这可以实现使用`JOIN`。 但使用`JOIN`时最初创建 TableAdapter，不能向导自动生成相应的插入、 更新和删除功能。 因此，我们将首先创建的 TableAdapter 的主查询不包含任何`JOIN`s。 然后，在步骤 2 中，我们将更新的主查询存储过程检索通过 manager s 名称`JOIN`。

首先打开`NorthwindWithSprocs`中的数据集`~/App_Code/DAL`文件夹。 右键单击设计器上，从上下文菜单中，选择添加选项并选择 TableAdapter 菜单项。 这将启动 TableAdapter 配置向导。 如图 5 所示，有向导创建新的存储的过程，并单击下一步。 在创建新使用刷新程序存储从 TableAdapter 的向导的过程，请查阅[创建新的存储过程类型化数据集 s Tableadapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)教程。


[![选择创建新存储的过程选项](updating-the-tableadapter-to-use-joins-vb/_static/image10.png)](updating-the-tableadapter-to-use-joins-vb/_static/image9.png)

**图 5**： 选择创建新的存储过程选项 ([单击以查看实际尺寸的图像](updating-the-tableadapter-to-use-joins-vb/_static/image11.png))


使用以下`SELECT`TableAdapter s 主查询语句：


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample4.sql)]

由于此查询不包含任何`JOIN`s，TableAdapter 向导自动将创建具有相应的存储的过程`INSERT`， `UPDATE`，和`DELETE`语句，以及用于执行主存储的过程查询。

以下步骤，我们可以命名 TableAdapter s 存储过程。 使用名称`Employees_Select`， `Employees_Insert`， `Employees_Update`，和`Employees_Delete`，如图 6 中所示。


[![名称的 TableAdapter s 存储过程](updating-the-tableadapter-to-use-joins-vb/_static/image13.png)](updating-the-tableadapter-to-use-joins-vb/_static/image12.png)

**图 6**： 命名 TableAdapter s 存储过程 ([单击以查看实际尺寸的图像](updating-the-tableadapter-to-use-joins-vb/_static/image14.png))


最后一步，提示我们命名 TableAdapter 的方法。 使用`Fill`和`GetEmployees`作为方法名称。 此外请确保选中要更新将直接发送到数据库 (GenerateDBDirectMethods) 复选框已选中的创建方法。


[![名称的 TableAdapter 的方法填充和 GetEmployees](updating-the-tableadapter-to-use-joins-vb/_static/image16.png)](updating-the-tableadapter-to-use-joins-vb/_static/image15.png)

**图 7**： 命名 TableAdapter 的方法`Fill`和`GetEmployees`([单击以查看实际尺寸的图像](updating-the-tableadapter-to-use-joins-vb/_static/image17.png))


完成向导后，需要一段时间来检查数据库中的存储的过程。 你应该会看到四个新的： `Employees_Select`， `Employees_Insert`， `Employees_Update`，和`Employees_Delete`。 接下来，检查`EmployeesDataTable`和`EmployeesTableAdapter`刚刚创建。 数据表中的主查询返回的每个字段的列。 单击 TableAdapter，然后转到属性窗口。 此时您将看到， `InsertCommand`， `UpdateCommand`，和`DeleteCommand`属性是否正确配置调用相应的存储的过程。


[![TableAdapter 包括插入、 更新和删除功能](updating-the-tableadapter-to-use-joins-vb/_static/image19.png)](updating-the-tableadapter-to-use-joins-vb/_static/image18.png)

**图 8**: TableAdapter 包括插入、 更新和删除功能 ([单击以查看实际尺寸的图像](updating-the-tableadapter-to-use-joins-vb/_static/image20.png))


使用插入、 更新和删除自动创建的存储的过程和`InsertCommand`， `UpdateCommand`，和`DeleteCommand`正确配置的属性，我们已准备好自定义`SelectCommand`s 存储过程来返回其他每个员工 s manager 有关的信息。 具体而言，我们需要更新`Employees_Select`存储过程来使用`JOIN`并返回 manager s`FirstName`和`LastName`值。 已更新存储的过程后，我们将需要更新 DataTable，以使其包括这些额外的列。 我们将介绍这两个任务在步骤 2 和 3。

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>步骤 2： 自定义要包括的存储的过程`JOIN`

通过转到服务器资源管理器、 向下钻取，到 Northwind 数据库 s 存储过程文件夹，并打开开始`Employees_Select`存储过程。 如果看不到此存储的过程中，右键单击存储过程文件夹然后选择刷新。 更新存储的过程，以便它使用`LEFT JOIN`先返回 manager s 和姓氏：


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample5.sql)]

在更新后`SELECT`语句，通过转到文件菜单并选择保存的更改保存`Employees_Select`。 或者，你可以单击工具栏中的保存图标或按 Ctrl + S。 保存你更改后，右键单击`Employees_Select`在服务器资源管理器存储过程并选择执行。 这将运行存储的过程并在输出窗口中显示其结果 （请参阅图 9）。


[![存储过程结果将显示在输出窗口](updating-the-tableadapter-to-use-joins-vb/_static/image22.png)](updating-the-tableadapter-to-use-joins-vb/_static/image21.png)

**图 9**: 存储过程结果将显示在输出窗口 ([单击以查看实际尺寸的图像](updating-the-tableadapter-to-use-joins-vb/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>步骤 3： 更新数据表的列

此时，`Employees_Select`存储过程返回`ManagerFirstName`和`ManagerLastName`值，但`EmployeesDataTable`缺少这些列。 可以将这些缺少的列添加到在两种方式之一 DataTable:

- **手动**-DataTable 数据集设计器中右键单击并从添加菜单中，选择列。 然后可以命名的列，并相应地设置其属性。
- **自动**-TableAdapter 配置向导将更新以反映返回的字段的 DataTable 的列`SelectCommand`存储过程。 在使用临时 SQL 语句时，向导还将删除`InsertCommand`， `UpdateCommand`，和`DeleteCommand`属性，因为`SelectCommand`现在包含`JOIN`。 但在使用存储的过程时，这些命令属性保持不变。

我们已经学习了如何手动添加数据表列在前面的教程，包括[母版/详细介绍 Master 记录项目符号列表使用详细信息 DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)和[上载文件](../working-with-binary-files/uploading-files-vb.md)，并且我们将在我们的下一教程中查看此过程再次的更多详细信息。 对于本教程，但是，让我们来使用自动方法通过 TableAdapter 配置向导。

通过右键单击启动`EmployeesTableAdapter`并从上下文菜单中选择配置。 这将打开 TableAdapter 配置向导中，其中列出了用于选择、 插入、 更新和删除，以及它们的返回值和参数 （如果有） 的存储的过程。 图 10 显示此向导。 在这里我们可以看到`Employees_Select`存储过程现在返回`ManagerFirstName`和`ManagerLastName`字段。


[![此向导显示 Employees_Select 的更新的列列表的存储过程](updating-the-tableadapter-to-use-joins-vb/_static/image25.png)](updating-the-tableadapter-to-use-joins-vb/_static/image24.png)

**图 10**： 向导显示的更新列列表`Employees_Select`存储过程 ([单击以查看实际尺寸的图像](updating-the-tableadapter-to-use-joins-vb/_static/image26.png))


通过单击完成完成向导。 在数据集设计器中，返回时`EmployeesDataTable`包括两个其他列：`ManagerFirstName`和`ManagerLastName`。


[![EmployeesDataTable 包含两个新列](updating-the-tableadapter-to-use-joins-vb/_static/image28.png)](updating-the-tableadapter-to-use-joins-vb/_static/image27.png)

**图 11**:`EmployeesDataTable`包含两个新列 ([单击以查看实际尺寸的图像](updating-the-tableadapter-to-use-joins-vb/_static/image29.png))


为了说明这一点已更新`Employees_Select`存储的过程是否生效，并且插入、 更新和删除的 TableAdapter 的功能仍然正常工作时，让我们来创建允许用户查看和删除员工的网页。 我们创建这样的页之前，但是，我们需要先使用从员工的业务逻辑层中创建一个新类`NorthwindWithSprocs`数据集。 在步骤 4 中，我们将创建`EmployeesBLLWithSprocs`类。 在步骤 5 中，我们将使用此类从 ASP.NET 页。

## <a name="step-4-implementing-the-business-logic-layer"></a>步骤 4： 实现业务逻辑层

创建新的类文件中`~/App_Code/BLL`文件夹名为`EmployeesBLLWithSprocs.vb`。 此类模仿现有的语义`EmployeesBLL`类，仅此新一个提供较少的方法和使用`NorthwindWithSprocs`数据集 (而不是`Northwind`数据集)。 向 `EmployeesBLLWithSprocs` 类添加下面的代码。


[!code-vb[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample6.vb)]

`EmployeesBLLWithSprocs`类 s`Adapter`属性返回的一个实例`NorthwindWithSprocs`数据集的`EmployeesTableAdapter`。 这由类 s`GetEmployees`和`DeleteEmployee`方法。 `GetEmployees`方法调用`EmployeesTableAdapter`s 对应`GetEmployees`方法，调用`Employees_Select`存储过程并使用在其结果来填充`EmployeeDataTable`。 `DeleteEmployee`方法同样调用`EmployeesTableAdapter`s`Delete`方法，调用`Employees_Delete`存储过程。

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>步骤 5： 使用表示层中的数据

与`EmployeesBLLWithSprocs`类完成后，我们重新已准备好处理通过 ASP.NET 页的员工数据。 打开`JOINs.aspx`页面`AdvancedDAL`文件夹，然后拖动一个 GridView 从工具箱中拖动到设计器中，设置其`ID`属性`Employees`。 接下来，从 GridView s 智能标记，将网格绑定到一个名为的新 ObjectDataSource 控件`EmployeesDataSource`。

配置对象数据源以使用`EmployeesBLLWithSprocs`类，并从选择和删除选项卡，确保`GetEmployees`和`DeleteEmployee`方法从下拉列表中处于选中状态。 单击完成以完成 ObjectDataSource 的配置。


[![配置对象数据源以使用 EmployeesBLLWithSprocs 类](updating-the-tableadapter-to-use-joins-vb/_static/image31.png)](updating-the-tableadapter-to-use-joins-vb/_static/image30.png)

**图 12**： 配置使用 ObjectDataSource`EmployeesBLLWithSprocs`类 ([单击以查看实际尺寸的图像](updating-the-tableadapter-to-use-joins-vb/_static/image32.png))


[![具有 ObjectDataSource 使用 GetEmployees 和 DeleteEmployee 方法](updating-the-tableadapter-to-use-joins-vb/_static/image34.png)](updating-the-tableadapter-to-use-joins-vb/_static/image33.png)

**图 13**: ObjectDataSource 使用`GetEmployees`和`DeleteEmployee`方法 ([单击以查看实际尺寸的图像](updating-the-tableadapter-to-use-joins-vb/_static/image35.png))


Visual Studio 将为每个到 GridView 添加 BoundField`EmployeesDataTable`的列。 删除所有除这些 BoundFields `Title`， `LastName`， `FirstName`， `ManagerFirstName`，和`ManagerLastName`和重命名`HeaderText`姓氏、 名字、 Manager s 第一个名称，最后四 BoundFields 属性和Manager s 姓氏、 分别。

若要允许用户从此页删除员工，我们需要执行两项操作。 首先，指示 GridView 删除功能提供通过检查从其智能标记启用删除选项。 其次，更改 ObjectDataSource s`OldValuesParameterFormatString`由 ObjectDataSource 向导设置的值的属性 (`original_{0}`) 为其默认值 (`{0}`)。 进行这些更改后，你 GridView 和 ObjectDataSource s 声明性标记应类似于以下：


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample7.aspx)]

通过访问通过浏览器测试页。 如图 14 所示，此页将列出每个员工和他或她 manager s 名称 （假定他们具有一个）。


[![联接中 Employees_Select 存储过程返回的管理器的名称](updating-the-tableadapter-to-use-joins-vb/_static/image37.png)](updating-the-tableadapter-to-use-joins-vb/_static/image36.png)

**图 14**:`JOIN`中`Employees_Select`存储过程返回的管理器名称 ([单击以查看实际尺寸的图像](updating-the-tableadapter-to-use-joins-vb/_static/image38.png))


单击删除按钮启动落在执行的删除工作流`Employees_Delete`存储过程。 但是，尝试`DELETE`由于外键约束冲突而失败的存储过程中的语句 （请参阅图 15）。 具体来说，每个员工都必须一个或多个记录`Orders`表，导致无法删除。


[![删除员工在违反了外键约束中具有对应订单结果](updating-the-tableadapter-to-use-joins-vb/_static/image40.png)](updating-the-tableadapter-to-use-joins-vb/_static/image39.png)

**图 15**： 删除员工在违反了外键约束中具有对应订单结果 ([单击以查看实际尺寸的图像](updating-the-tableadapter-to-use-joins-vb/_static/image41.png))


若要允许员工要删除你可以：

- 更新级联删除的外键约束
- 手动删除从记录`Orders`名员工想要删除的表或
- 更新`Employees_Delete`存储过程首先删除从相关的记录`Orders`表，然后删除`Employees`记录。 我们讨论了中的此技术[使用现有存储过程类型化数据集 s Tableadapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)教程。

我将此作为练习为读取器。

## <a name="summary"></a>摘要

使用关系数据库时, 很常见的查询以提取其数据从多个相关表。 相关子查询和`JOIN`s 提供数据访问在查询中的相关表中的两个不同的技术。 在前面的教程，我们通常情况下进行使用的相关子查询，因为 TableAdapter 无法自动生成`INSERT`， `UPDATE`，和`DELETE`语句的查询涉及`JOIN`s。 尽管使用 TableAdapter 配置向导完成时，将覆盖任何自定义项的临时 SQL 语句时，可以手动提供这些值。

幸运的是，使用存储的过程创建的 Tableadapter 不会遇到与使用临时 SQL 语句创建相同的易。 因此，它是可以创建其主查询使用的 TableAdapter`JOIN`时使用存储的过程。 在本教程中我们已了解如何创建此类的 TableAdapter。 我们通过使用启动`JOIN`-较少`SELECT`TableAdapter s 主查询，以便相应的插入、 更新和删除存储过程将自动创建查询。 TableAdapter s 初始完成配置后，我们扩充`SelectCommand`存储过程来使用`JOIN`并重新运行 TableAdapter 配置向导以更新`EmployeesDataTable`的列。

重新运行 TableAdapter 配置向导自动更新`EmployeesDataTable`列以反映返回的数据字段`Employees_Select`存储过程。 或者，我们无法具有这些列手动添加到数据表。 我们将在下一教程中浏览到 DataTable 手动添加的列。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已希尔顿 Geisenow、 David Suru 和 Teresa 墨。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
[下一页](adding-additional-datatable-columns-vb.md)
