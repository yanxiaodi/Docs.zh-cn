---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
title: "使用计算列 (C#) |Microsoft 文档"
author: rick-anderson
description: "在创建数据库表时，Microsoft SQL Server 允许你定义计算的列从表达式计算其值，通常 referen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 57459065-ed7c-4dfe-ac9c-54c093abc261
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 41206f76f9d9ca68971a53d79e84d82349e92333
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="working-with-computed-columns-c"></a>使用计算列 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_CS.zip)或[下载 PDF](working-with-computed-columns-cs/_static/datatutorial71cs1.pdf)

> 在创建数据库表时，Microsoft SQL Server 将允许你定义计算的列从包含通常引用同一数据库记录中的其他值的表达式计算其值。 此类值是在数据库中，这需要特殊的注意事项，使用 Tableadapter 时以只读的。 在本教程中，我们将了解如何迎接计算列所带来的挑战。


## <a name="introduction"></a>介绍

Microsoft SQL Server 允许*[计算列](https://msdn.microsoft.com/library/ms191250.aspx)*，它们通常引用同一个表中的其他列的值的表达式计算其值的列。 例如，跟踪数据模型的时间可能具有名为的表`ServiceLog`与列包括`ServicePerformed`， `EmployeeID`， `Rate`，和`Duration`，以及其他。 而应付款金额每个服务项 （正在乘以持续时间的速率） 无法计算通过某个网页或其他编程接口，它可能是方便起见，可以包括中的列`ServiceLog`表名为`AmountDue`，报告此信息。 此列无法创建为一个正常的列，但它需要随时更新`Rate`或`Duration`更改列的值。 更好的方法是使`AmountDue`列使用表达式的计算列`Rate * Duration`。 这样将导致 SQL Server，自动计算`AmountDue`只要它在查询中引用列的值。

由于计算的列的值由一个表达式，这类列是只读的因此不能具有值分配给它们中`INSERT`或`UPDATE`语句。 但是，当计算的列的主查询的 TableAdapter 使用临时 SQL 语句的一部分时，它们将自动包含在自动生成`INSERT`和`UPDATE`语句。 因此，TableAdapter s`INSERT`和`UPDATE`查询和`InsertCommand`和`UpdateCommand`必须更新属性要删除对任何计算列引用。

挑战之一使用的计算列使用 TableAdapter 使用临时 SQL 语句是 TableAdapter s`INSERT`和`UPDATE`查询自动重新生成 TableAdapter 配置向导完成任何时间。 因此，计算的列中手动删除从`INSERT`和`UPDATE`查询将重新出现，如果重新运行该向导。 尽管使用存储的过程的 Tableadapter 不会从此易受到影响，但它们具有其自己 quirks 我们将在步骤 3 中解决。

在本教程中，我们将添加到计算的列`Suppliers`Northwind 数据库中表，然后创建相应的 TableAdapter，若要使用此表和其计算的列。 我们必须使用存储的过程而不是临时的 SQL 语句，以便我们自定义项不 t 丢失使用 TableAdapter 配置向导时我们 TableAdapter。

让我们来开始 ！

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>步骤 1： 添加计算的列到`Suppliers`表

Northwind 数据库没有任何计算的列，因此我们将需要添加一个自己。 本教程让添加到计算的列的 s`Suppliers`表调用`FullContactName`，采用以下格式返回联系人的名称、 标题和它们适用于公司： `ContactName` (`ContactTitle`， `CompanyName`)。 此计算显示有关供应商的信息时，可能会在报表中使用列。

首先打开`Suppliers`通过右键单击表定义`Suppliers`表在服务器资源管理器中，从上下文菜单中选择打开表定义。 这将显示的列的表和其属性，其数据类型，例如，是否它们允许`NULL`s，依此类推。 若要添加计算的列，启动在表定义中键入列的名称。 接下来，在列属性窗口中的计算所得的列规范部分下 （公式） 的文本框中输入其表达式 （参见图 1）。 命名计算的列`FullContactName`并使用以下表达式：


[!code-sql[Main](working-with-computed-columns-cs/samples/sample1.sql)]

请注意，可以在 SQL 中串联字符串使用`+`运算符。 `CASE`可像在传统的编程语言中的条件使用语句。 在上面的表达式`CASE`语句可以显示为： 如果`ContactTitle`不`NULL`然后输出`ContactTitle`值加上一个逗号，否则为发出执行任何操作。 有关详细信息的有用性`CASE`语句，请参阅[的 SQL Power`CASE`语句](http://www.4guysfromrolla.com/webtech/102704-1.shtml)。

> [!NOTE]
> 而不是使用`CASE`此处语句中，我们本来也可以或者使用`ISNULL(ContactTitle, '')`。 [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx)返回*checkExpression*如果它为非 NULL，否则它将返回*replacementValue*。 而是`ISNULL`或`CASE`起在此情况下，有更复杂的方案其中的灵活性`CASE`语句不能由匹配`ISNULL`。


添加此计算的列后你的屏幕应如下所示的屏幕快照中图 1 中。


[![添加一个名为 FullContactName 到供应商表的计算的列](working-with-computed-columns-cs/_static/image2.png)](working-with-computed-columns-cs/_static/image1.png)

**图 1**： 添加计算列命名为`FullContactName`到`Suppliers`表 ([单击以查看实际尺寸的图像](working-with-computed-columns-cs/_static/image3.png))


在命名计算的列并输入其表达式之后, 将所做的更改保存到表通过单击工具栏中的保存图标，按 Ctrl + S，或通过转到文件菜单并选择保存`Suppliers`。

正在保存表应刷新服务器资源管理器，包括中的只添加列`Suppliers`表的列列表。 此外，在 （公式） 文本框中输入的表达式将自动调整为去除多余的空格、 环绕括号后的列名称的等效表达式 (`[]`)，并包含括号以更为明确地显示操作的顺序：


[!code-sql[Main](working-with-computed-columns-cs/samples/sample2.sql)]

有关 Microsoft SQL Server 中的计算列的详细信息，请参阅[技术文档](https://msdn.microsoft.com/library/ms191250.aspx)。 同时查看[How to: Specify Computed Columns](https://msdn.microsoft.com/library/ms188300.aspx)有关创建计算的列的分步演练。

> [!NOTE]
> 默认情况下，计算的列不以物理方式存储在表，但将改为重新计算某查询引用的每个时间。 通过检查保留复选框，但是，你可以指示 SQL Server 以物理方式在表中存储计算的列。 这样做将使索引可以提高使用中的计算的列值的查询的性能对计算列创建其`WHERE`子句。 请参阅[对计算列创建索引](https://msdn.microsoft.com/library/ms189292.aspx)有关详细信息。


## <a name="step-2-viewing-the-computed-column-s-values"></a>步骤 2： 查看计算的列的值

允许 s 我们在数据访问层上启动工作之前，请花几分钟，即可查看`FullContactName`值。 从服务器资源管理器，右键单击`Suppliers`表名称，然后从上下文菜单中选择新查询。 这将显示一个提示我们选择要包括在查询的表的查询窗口。 添加`Suppliers`表，然后单击关闭。 接下来，检查`CompanyName`， `ContactName`， `ContactTitle`，和`FullContactName`供应商表中的列。 最后，单击执行查询并查看结果工具栏中的红色感叹号图标。

如图 2 所示，其结果将包括`FullContactName`，其中列出`CompanyName`， `ContactName`，和`ContactTitle`列使用格式 l`ContactName`(`ContactTitle`, `CompanyName`) .


[![FullContactName 使用格式 ContactName (公司名称中的联系人职务）](working-with-computed-columns-cs/_static/image5.png)](working-with-computed-columns-cs/_static/image4.png)

**图 2**:`FullContactName`使用格式`ContactName`(`ContactTitle`， `CompanyName`) ([单击以查看实际尺寸的图像](working-with-computed-columns-cs/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>步骤 3： 添加`SuppliersTableAdapter`到数据访问层

若要使用我们的应用程序中的供应商信息，我们需要先在我们 DAL 中创建 TableAdapter 和数据表。 理想情况下，这将使用完成检查前面的教程中的相同简单步骤。 但是，使用计算列引入了几个值得使用讨论的褶皱。

如果你使用的 TableAdapter 使用临时 SQL 语句，则只需可以 TableAdapter s 通过 TableAdapter 配置向导的主查询中包含计算的列。 此操作，但是，将自动生成`INSERT`和`UPDATE`包括计算的列的语句。 如果你尝试执行这些方法中的其中一个`SqlException`与消息列*ColumnName*无法修改，因为它是计算的列，或者它将引发 UNION 运算符的结果。 虽然`INSERT`和`UPDATE`语句可以手动调整到的 TableAdapter`InsertCommand`和`UpdateCommand`属性，这些自定义项将会丢失，只要 TableAdapter 配置向导重新运行。

由于使用临时 SQL 语句的 Tableadapter 脆弱性，建议我们在使用计算列时使用存储的过程。 如果你正在使用现有存储的过程，只需配置 TableAdapter 中所述[使用现有存储过程类型化数据集 s Tableadapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)教程。 如果你有 TableAdapter 向导为你创建的存储的过程，但是，务必最初省略从主查询任何计算的列。 如果在主查询中包含计算的列，TableAdapter 配置向导将通知你，在完成后，它不能创建相应的存储的过程。 简单地说，我们需要最初配置 TableAdapter 使用计算的列释放主查询，然后手动更新对应的存储的过程和 TableAdapter 的`SelectCommand`可包含计算的列。 这种方法是类似于在中使用[更新为使用的 TableAdapter](updating-the-tableadapter-to-use-joins-cs.md)`JOIN`*s*教程。

对于本教程，让我们来添加新的 TableAdapter，并将其自动为我们创建的存储的过程。 因此，我们将需要最初省略`FullContactName`从主查询的计算的列。

首先打开`NorthwindWithSprocs`中的数据集`~/App_Code/DAL`文件夹。 在设计器中右键单击，并从上下文菜单上，选择添加新的 TableAdapter。 这将启动 TableAdapter 配置向导。 查询数据，从指定的数据库 (`NORTHWNDConnectionString`从`Web.config`)，然后单击下一步。 由于我们尚未创建任何可供查询或修改存储的过程`Suppliers`表中，选择的创建新的存储的过程选项，以使该向导将为我们创建它们，并单击下一步。


[![选择创建新存储的过程选项](working-with-computed-columns-cs/_static/image8.png)](working-with-computed-columns-cs/_static/image7.png)

**图 3**： 选择创建新存储的过程选项 ([单击以查看实际尺寸的图像](working-with-computed-columns-cs/_static/image9.png))


后续步骤，提示我们输入主查询。 输入以下查询，返回`SupplierID`， `CompanyName`， `ContactName`，和`ContactTitle`每个供应商的列。 请注意，此查询有意省略了计算的列 (`FullContactName`); 我们将更新要在步骤 4 中包括此列的相应存储的过程。


[!code-sql[Main](working-with-computed-columns-cs/samples/sample3.sql)]

输入主查询并单击下一步后, 该向导将允许我们命名它将生成的四个存储的过程。 命名这些存储的过程`Suppliers_Select`， `Suppliers_Insert`， `Suppliers_Update`，和`Suppliers_Delete`，如图 4 所示。


[![自定义的自动生成的存储过程名称](working-with-computed-columns-cs/_static/image11.png)](working-with-computed-columns-cs/_static/image10.png)

**图 4**： 自定义 Auto-Generated 存储过程的名称 ([单击以查看实际尺寸的图像](working-with-computed-columns-cs/_static/image12.png))


下一步的向导步骤，我们可以命名 TableAdapter 的方法并指定用于访问和更新数据的模式。 不要选中，所有的三个复选框，但重命名`GetData`方法`GetSuppliers`。 单击完成以完成向导。


[![将 GetData 方法重命名为 GetSuppliers](working-with-computed-columns-cs/_static/image14.png)](working-with-computed-columns-cs/_static/image13.png)

**图 5**： 重命名`GetData`方法`GetSuppliers`([单击以查看实际尺寸的图像](working-with-computed-columns-cs/_static/image15.png))


单击时完成，向导将创建四个存储的过程，并将 TableAdapter 和相应的数据表添加到类型化数据集。

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>步骤 4： 在 TableAdapter s 主查询中包含计算的列

现在，我们需要更新 TableAdapter 和在步骤 3 以包括中创建 DataTable`FullContactName`计算的列。 这涉及到两个步骤：

1. 更新`Suppliers_Select`存储过程来返回`FullContactName`计算的列和
2. 更新以包括相应 DataTable`FullContactName`列。

首先，导航到服务器资源管理器和向下钻取，到存储过程文件夹。 打开`Suppliers_Select`存储过程和更新`SELECT`查询以包含`FullContactName`计算的列：


[!code-sql[Main](working-with-computed-columns-cs/samples/sample4.sql)]

将所做的更改保存到存储过程，通过单击工具栏中的保存图标，按 Ctrl + S，或通过选择保存`Suppliers_Select`从文件菜单选项。

接下来，返回到数据集设计器，右键单击`SuppliersTableAdapter`，然后从上下文菜单中选择配置。 请注意，`Suppliers_Select`列现在包括`FullContactName`其的数据列集合中的列。


[![运行 TableAdapter 的配置向导以更新数据表的列](working-with-computed-columns-cs/_static/image17.png)](working-with-computed-columns-cs/_static/image16.png)

**图 6**： 运行配置向导以更新 DataTable 的列中的 TableAdapter s ([单击以查看实际尺寸的图像](working-with-computed-columns-cs/_static/image18.png))


单击完成以完成向导。 这会自动添加到相应的列`SuppliersDataTable`。 TableAdapter 向导是足够智能，可检测`FullContactName`列是计算的列，因此它是只读的。 因此，它将设置列 s`ReadOnly`属性`true`。 若要验证这一点，选择从列`SuppliersDataTable`，然后转到属性窗口 （请参阅图 7）。 请注意，`FullContactName`列 s`DataType`和`MaxLength`属性也会相应地设置。


[![FullContactName 列被标记为只读的](working-with-computed-columns-cs/_static/image20.png)](working-with-computed-columns-cs/_static/image19.png)

**图 7**:`FullContactName`列标记为只读 ([单击以查看实际尺寸的图像](working-with-computed-columns-cs/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>步骤 5： 添加`GetSupplierBySupplierID`到 TableAdapter 的方法

对于本教程中，我们将创建可更新网格中显示供应商的 ASP.NET 页。 在过去的教程我们已更新的业务逻辑层的单个记录通过检索特定记录从作为强类型数据表，更新其属性，然后将发送更新的 DataTable DAL 回 DAL 传播的更改数据库中。 若要完成-检索要更新从 DAL 的记录的此第一步，我们需要先添加`GetSupplierBySupplierID(supplierID)`与 DAL 的方法。

右键单击`SuppliersTableAdapter`数据集设计中，从上下文菜单中选择添加查询选项。 正如我们做在步骤 3 中，让向导为我们生成新的存储的过程，通过选择创建新存储的过程选项 （回头参考图 3 有关此向导步骤的屏幕快照）。 因为此方法会返回多个列的记录，指示我们想要使用 SELECT 返回行的 SQL 查询并单击下一步。


[![选择它返回行选项](working-with-computed-columns-cs/_static/image23.png)](working-with-computed-columns-cs/_static/image22.png)

**图 8**： 选择其返回行选项 ([单击以查看实际尺寸的图像](working-with-computed-columns-cs/_static/image24.png))


后续步骤将提示 us 代表要使用此方法的查询。 输入以下内容，它将返回相同的数据字段作为主查询，但特定供应商。


[!code-sql[Main](working-with-computed-columns-cs/samples/sample5.sql)]

下一个屏幕可让我们名称将自动生成的存储的过程。 命名此存储的过程`Suppliers_SelectBySupplierID`单击下一步。


[![存储的过程 Suppliers_SelectBySupplierID 名称](working-with-computed-columns-cs/_static/image26.png)](working-with-computed-columns-cs/_static/image25.png)

**图 9**： 命名存储过程`Suppliers_SelectBySupplierID`([单击以查看实际尺寸的图像](working-with-computed-columns-cs/_static/image27.png))


最后，向导提示我们的数据访问模式和要用于 TableAdapter 的方法名称。 将选中，这两个复选框保留，但重命名`FillBy`和`GetDataBy`方法`FillBySupplierID`和`GetSupplierBySupplierID`分别。


[![名称 TableAdapter 方法 FillBySupplierID 和 GetSupplierBySupplierID](working-with-computed-columns-cs/_static/image29.png)](working-with-computed-columns-cs/_static/image28.png)

**图 10**： 命名 TableAdapter 方法`FillBySupplierID`和`GetSupplierBySupplierID`([单击以查看实际尺寸的图像](working-with-computed-columns-cs/_static/image30.png))


单击完成以完成向导。

## <a name="step-6-creating-the-business-logic-layer"></a>步骤 6： 创建业务逻辑层

我们创建的 ASP.NET 页的使用步骤 1 中创建计算的列之前，我们首先需要在 BLL 中添加相应的方法。 我们 ASP.NET 页中，我们将创建在步骤 7 中，将允许用户查看和编辑供应商。 因此，我们需要我们 BLL 若要提供，最小，获取的所有供应商和另一个更新的特定供应商提供的方法。

创建名为的新类文件`SuppliersBLLWithSprocs`中`~/App_Code/BLL`文件夹并添加以下代码：


[!code-csharp[Main](working-with-computed-columns-cs/samples/sample6.cs)]

像其他 BLL 类，`SuppliersBLLWithSprocs`具有`protected``Adapter`返回的实例的属性`SuppliersTableAdapter`类，以及两个`public`方法：`GetSuppliers`和`UpdateSupplier`。 `GetSuppliers`方法调用，并返回`SuppliersDataTable`返回的相应`GetSupplier`数据访问层中的方法。 `UpdateSupplier`方法检索有关特定供应商通过 DAL 的调用正在更新的信息`GetSupplierBySupplierID(supplierID)`方法。 然后，它更新`CategoryName`， `ContactName`，和`ContactTitle`属性，然后将这些更改提交到数据库中，通过调用数据访问层 s`Update`方法，并传递在已修改`SuppliersRow`对象。

> [!NOTE]
> 除`SupplierID`和`CompanyName`，供应商表中的所有列都允许`NULL`值。 因此，如果传入的`contactName`或`contactTitle`参数`null`我们需要将设置相应`ContactName`和`ContactTitle`属性设置为`NULL`数据库值使用`SetContactNameNull`和`SetContactTitleNull`方法，分别。


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>步骤 7： 使用的表示层的计算列

与计算列添加到`Suppliers`表的 DAL 和 BLL 相应地更新，我们已准备好生成适用于 ASP.NET 页`FullContactName`计算的列。 首先打开`ComputedColumns.aspx`页面`AdvancedDAL`文件夹，然后拖动一个 GridView 从工具箱中拖动到设计器。 设置 GridView s`ID`属性`Suppliers`和从其智能标记，请将其绑定到名为新 ObjectDataSource `SuppliersDataSource`。 配置对象数据源以使用`SuppliersBLLWithSprocs`类我们添加在步骤 6 后退和单击下一步。


[![配置对象数据源以使用 SuppliersBLLWithSprocs 类](working-with-computed-columns-cs/_static/image32.png)](working-with-computed-columns-cs/_static/image31.png)

**图 11**： 配置使用 ObjectDataSource`SuppliersBLLWithSprocs`类 ([单击以查看实际尺寸的图像](working-with-computed-columns-cs/_static/image33.png))


只有两种方法中定义`SuppliersBLLWithSprocs`类：`GetSuppliers`和`UpdateSupplier`。 请确保这两种方法中选择指定分别更新选项卡上，并且单击完成以完成的对象数据源配置。

在完成数据源配置向导，Visual Studio 将为每个返回的数据字段添加 BoundField。 删除`SupplierID`BoundField 和更改`HeaderText`属性`CompanyName`， `ContactName`， `ContactTitle`，和`FullContactName`BoundFields 到公司、 联系人名称、 标题和完整的联系人姓名，分别。 通过智能标记中，选中启用编辑复选框以开启 GridView s 内置编辑功能。

除了将 BoundFields 添加到 GridView，完成数据源向导也会导致 Visual Studio 设置 ObjectDataSource s`OldValuesParameterFormatString`属性设置为原始\_{0}。 还原此设置改回为其默认值，{0}。

对进行后这些编辑的 GridView 和 ObjectDataSource，其声明性标记应看起来类似以下：


[!code-aspx[Main](working-with-computed-columns-cs/samples/sample7.aspx)]

接下来，请访问此页面通过浏览器。 如图 12 所示，包括的网格中列出每个供应商`FullContactName`列，其值为只需的其他三个列的串联格式化为`ContactName`(`ContactTitle`， `CompanyName`)。


[![在网格中列出每个供应商](working-with-computed-columns-cs/_static/image35.png)](working-with-computed-columns-cs/_static/image34.png)

**图 12**： 网格中列出每个供应商 ([单击以查看实际尺寸的图像](working-with-computed-columns-cs/_static/image36.png))


单击编辑按钮，为特定的供应商导致回发和中呈现该行其编辑接口 （请参阅图 13）。 前三个列呈现在其默认的编辑界面-一个文本框控件`Text`属性设置为数据字段的值。 `FullContactName`列，但是，保持为文本。 当 BoundFields 已添加到在数据源配置向导中，完成 GridView `FullContactName` BoundField s`ReadOnly`属性设置为`true`因为相应`FullContactName`中的列`SuppliersDataTable`具有其`ReadOnly`属性设置为`true`。 在步骤 4 中所述`FullContactName`s`ReadOnly`属性设置为`true`因为 TableAdapter 检测到列是计算的列。


[![FullContactName 列是不可编辑](working-with-computed-columns-cs/_static/image38.png)](working-with-computed-columns-cs/_static/image37.png)

**图 13**:`FullContactName`列是不可编辑 ([单击以查看实际尺寸的图像](working-with-computed-columns-cs/_static/image39.png))


请继续并更新一个或多个可编辑列的值，然后单击更新。 请注意如何`FullContactName`的值将自动更新以反映更改。

> [!NOTE]
> GridView 当前用于 BoundFields 可编辑字段，从而导致是默认的编辑界面。 由于`CompanyName`字段是必需的它应转换为包括 RequiredFieldValidator TemplateField。 我将此作为练习感读取器。 请查阅[将验证控件添加到的编辑和插入接口](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)教程，以转换为 TemplateField BoundField 和添加验证控件的分步说明。


## <a name="summary"></a>摘要

在定义表的架构时，Microsoft SQL Server 允许包含计算列。 下面是通常从同一记录中的其他列引用值的表达式计算其值的列。 自从值计算的列基于一个表达式，它们是只读的并不能分配中的值`INSERT`或`UPDATE`语句。 这将带来挑战将尝试自动生成对应的 TableAdapter 的主查询中使用计算的列时`INSERT`， `UPDATE`，和`DELETE`语句。

在本教程中我们讨论了尝试绕过计算列所带来的挑战的技术。 具体而言，我们使用存储的过程在我们的 TableAdapter 中来克服易使用的临时 SQL 语句的 Tableadapter 中的固有功能。 使用 TableAdapter 向导创建新存储过程时，很重要，我们具有最初省略任何计算的列，因为存在可防止数据修改存储过程生成的主查询。 最初配置 TableAdapter 之后，其`SelectCommand`可以 retooled 存储的过程以包括任何计算的列。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已希尔顿 Geisenow 和 Teresa 墨。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](adding-additional-datatable-columns-cs.md)
[下一页](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
