---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
title: "插入、 更新和删除数据使用 SqlDataSource (VB) |Microsoft 文档"
author: rick-anderson
description: "在前面的教程中我们学习了如何 ObjectDataSource 控件允许插入、 更新和删除的数据。 SqlDataSource 控件支持 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 9673bef3-892c-45ba-a7d8-0da3d6f48ec5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 4664e15ca6afa14f072e0840354c7f5a48d97ddf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-vb"></a>插入、 更新和删除数据使用 SqlDataSource (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_VB.exe)或[下载 PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/datatutorial49vb1.pdf)

> 在前面的教程中我们学习了如何 ObjectDataSource 控件允许插入、 更新和删除的数据。 SqlDataSource 控件支持相同的运算，但方法不同，并且本教程演示如何配置 SqlDataSource 插入、 更新和删除数据。


## <a name="introduction"></a>介绍

中所述[概述的插入、 更新和删除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)、 GridView 控件提供内置的更新和删除的功能，而说明和 FormView 控件包括插入支持连同编辑和删除功能。 这些数据修改功能直接插入没有无需编写的代码行的数据源控件。 [概述的插入、 更新和删除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)检查使用 ObjectDataSource 来支持插入、 更新和删除与 GridView、 说明如何和 FormView 控件。 或者，SqlDataSource 可用来代替对象数据源。

回想一下，以支持插入、 更新和删除与 ObjectDataSource 我们需要指定的对象层方法调用来执行插入、 更新或删除操作。 使用 SqlDataSource，我们需要提供`INSERT`， `UPDATE`，和`DELETE`SQL 语句 （或存储的过程） 来执行。 正如我们将在本教程中看到的这些语句可以手动创建，也可以由 SqlDataSource s 配置数据源向导自动生成。

> [!NOTE]
> 因为我们已已经讨论了插入、 编辑和删除功能的 GridView，说明如何，并 FormView 控制，本教程将重点介绍如何配置 SqlDataSource 控件以支持这些操作。 如果你需要作好准备实现这些功能在 GridView、 说明如何和 FormView，返回到编辑、 插入、 和删除数据教程中的，从开始安装[概述的插入、 更新和删除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)。


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>步骤 1： 指定`INSERT`，`UPDATE`，和`DELETE`语句

正如我们看到在过去的两个教程中，我们需要 SqlDataSource 控件从检索数据的已设置两个属性：

1. `ConnectionString`它指定什么数据库发送到，查询和
2. `SelectCommand`指定的临时 SQL 语句或存储的过程名称以执行，以返回结果。

有关`SelectCommand`带参数的值、 SqlDataSource s 通过指定值的参数`SelectParameters`集合并可以包含硬编码值，通用参数的源值 (查询字符串字段，会话变量 Web 控件值和等等），或可以以编程方式分配。 当 SqlDataSource 控制 s`Select()`方法从数据 Web 控件调用以编程方式或自动建立到数据库的连接，参数值分配给查询，而命令关闭往返到数据库。 然后返回结果为数据集或 DataReader，具体取决于控件 s 的值`DataSourceMode`属性。

选择数据，以及 SqlDataSource 控件可用来插入、 更新和删除数据，通过提供`INSERT`， `UPDATE`，和`DELETE`基本相同的方法中的 SQL 语句。 只需将分配`InsertCommand`， `UpdateCommand`，和`DeleteCommand`属性`INSERT`， `UPDATE`，和`DELETE`要执行的 SQL 语句。 如果语句具有参数 （如最始终将它们），将它们包含在`InsertParameters`， `UpdateParameters`，和`DeleteParameters`集合。

一次`InsertCommand`， `UpdateCommand`，或`DeleteCommand`指定值，相应的数据 Web 控件 s 智能标记中的启用插入、 启用编辑或启用删除选项将会变得可用。 为了说明这一点，让 s 举个例子从`Querying.aspx`我们在中创建的页[查询数据与 SqlDataSource 控件](querying-data-with-the-sqldatasource-control-vb.md)教程和增加使之包括删除功能。

首先打开`InsertUpdateDelete.aspx`和`Querying.aspx`页从`SqlDataSource`文件夹。 从设计器上`Querying.aspx`页面上，选择第一个示例从 SqlDataSource GridView (`ProductsDataSource`和`GridView1`控件)。 选择两个控件后, 转到编辑菜单并选择复制 （或只需按 Ctrl + C）。 接下来，转到该设计器的`InsertUpdateDelete.aspx`并粘贴在控件中。 你具有到移到两个控件后`InsertUpdateDelete.aspx`，浏览器中测试。 你应看到的值`ProductID`， `ProductName`，和`UnitPrice`列中的记录的所有`Products`数据库表。


[![所有产品列出，请按 ProductID 排序](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.png)

**图 1**： 所有产品列出，请按排序`ProductID`([单击以查看实际尺寸的图像](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>添加 SqlDataSource s`DeleteCommand`和`DeleteParameters`属性

此时我们可以只是返回的所有记录从 SqlDataSource`Products`表和一个 GridView，呈现此数据。 我们的目标是将此示例允许用户删除通过 GridView 的产品进行扩展。 若要完成此我们需要指定 SqlDataSource 控件 s 的值`DeleteCommand`和`DeleteParameters`属性，然后配置以支持删除 GridView。

`DeleteCommand`和`DeleteParameters`可以多种方式指定属性：

- 通过声明性语法
- 从设计器中的属性窗口
- 从指定的自定义 SQL 语句或存储的过程中配置数据源向导的屏幕
- 通过高级按钮从一个表中查看屏幕在配置数据源向导中指定列中，这将实际自动生成`DELETE`中使用的 SQL 语句和参数集合`DeleteCommand`和`DeleteParameters`属性

我们将研究如何自动具有`DELETE`在步骤 2 中创建的语句。 现在，让我们来使用设计器中中的属性窗口尽管同样效果配置数据源向导或声明性语法选项。

中的设计器从`InsertUpdateDelete.aspx`，单击`ProductsDataSource`SqlDataSource 然后调出属性窗口 （从视图菜单中，选择属性窗口中，或只需按 F4）。 选择省略号一组将显示的 DeleteQuery 属性。


![从属性窗口中选择 DeleteQuery 属性](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.gif)

**图 2**： 从属性窗口中选择 DeleteQuery 属性


> [!NOTE]
> SqlDataSource 不具有 DeleteQuery 属性。 相反，DeleteQuery 是的组合`DeleteCommand`和`DeleteParameters`属性和查看通过设计器窗口时仅列出在属性窗口中。 如果你在源视图中的属性窗口中查看，你将找到`DeleteCommand`属性改为。


单击省略号以 DeleteQuery 属性，以打开命令和参数编辑器对话框框中 （请参见图 3）。 通过此对话框可以指定`DELETE`SQL 语句并指定参数。 输入以下查询到`DELETE`命令文本框中 (或者手动或使用查询生成器中，如果需要):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample1.sql)]

接下来，单击刷新参数按钮以添加`@ProductID`参数的以下参数的列表。


[![从属性窗口中选择 DeleteQuery 属性](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.png)

**图 3**： 从属性窗口中选择 DeleteQuery 属性 ([单击以查看实际尺寸的图像](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.png))


执行*不*为此参数 （不定义其参数源位于 None） 提供一个值。 后将删除的支持添加到 GridView，GridView 将自动提供此参数值，使用的值其`DataKeys`其删除按钮被单击的行集合。

> [!NOTE]
> 中使用的参数名称`DELETE`查询*必须*的名称相同`DataKeyNames`GridView，说明如何或 FormView 中的值。 也就是说中的参数`DELETE`语句特意名为`@ProductID`(而不是，即`@ID`)，因为产品表 （和 GridView 中的将字段名值） 中的主键列名称是`ProductID`。


如果参数名称和`DataKeyNames`值不匹配，GridView 不能自动赋予参数值从`DataKeys`集合。

在命令和参数编辑器对话框中输入与删除相关的信息后单击确定并转到源视图以检查生成的声明性标记：

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample2.aspx)]

请注意添加`DeleteCommand`属性以及`<DeleteParameters>`部分和名为的参数对象`productID`。

## <a name="configuring-the-gridview-for-deleting"></a>配置用于删除 GridView

与`DeleteCommand`属性添加，GridView s 智能标记现在包含启用删除选项。 请继续并选中此复选框。 中所述[概述的插入、 更新和删除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)，这将导致 GridView 添加与 CommandField 其`ShowDeleteButton`属性设置为`True`。 如图 4 所示，通过浏览器访问页时是包含删除按钮。 通过删除某些产品测试出此页。


[![每个 GridView 行现在包含一个删除按钮](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.png)

**图 4**： 每个 GridView 行现在包含一个删除按钮 ([单击以查看实际尺寸的图像](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.png))


如果单击删除按钮，产生的回发，GridView 将分配`ProductID`参数值的`DataKeys`其删除按钮被单击，并调用 SqlDataSource s 的行的集合值`Delete()`方法。 SqlDataSource 控件，然后连接到数据库并执行`DELETE`语句。 GridView 然后将 SqlDataSource，取回，显示当前的产品集 （其中不再包括只删除记录） 重新绑定。

> [!NOTE]
> 由于 GridView 使用其`DataKeys`集合以填充 SqlDataSource 参数，它 s 重要的 GridView s`DataKeyNames`属性设置为的列构成的主键，SqlDataSource 的`SelectCommand`返回这些列。 此外，它非常重要的参数名在 SqlDataSource`DeleteCommand`设置为`@ProductID`。 如果`DataKeyNames`未设置属性或未命名参数`@ProductsID`，单击删除按钮将导致回发，但实际上获胜的不会删除任何记录。


图 5 以图形方式描述了这种交互。 将回指[检查与插入、 更新和删除的事件相关联](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)教程，以插入、 更新和删除数据 Web 控件与关联的事件链上的更多详细讨论。


![单击删除按钮在 GridView 调用 SqlDataSource s delete （） 方法](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.gif)

**图 5**： 单击删除按钮在 GridView 调用 SqlDataSource 的`Delete()`方法


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>步骤 2： 自动生成`INSERT`，`UPDATE`，和`DELETE`语句

为步骤 1，将检查`INSERT`， `UPDATE`，和`DELETE`可通过属性窗口或控件 s 声明性语法指定 SQL 语句。 但是，此方法要求，我们手动写出的 SQL 语句手动，这可以是单调并且容易出错。 幸运的是，配置数据源向导提供一个选项用于具有`INSERT`， `UPDATE`，和`DELETE`使用指定的列从一个表中查看屏幕时自动生成的语句。

让我们来了解此自动生成选项。 将说明如何添加到中的设计器`InsertUpdateDelete.aspx`并设置其`ID`属性`ManageProducts`。 接下来，从说明 s 智能标记中，选择创建新的数据源并创建名为 SqlDataSource `ManageProductsDataSource`。


[![创建名为 ManageProductsDataSource 新 SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.png)

**图 6**： 创建新的 SqlDataSource 名为`ManageProductsDataSource`([单击以查看实际尺寸的图像](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.png))


从配置数据源向导中，选择使用`NORTHWINDConnectionString`连接字符串，并单击下一步。 从配置 Select 语句屏幕中，将指定的列从表或视图的单选按钮处于选中状态，然后选择`Products`从下拉列表的表。 选择`ProductID`， `ProductName`， `UnitPrice`，和`Discontinued`复选框列表中的列。


[![使用产品表，返回 ProductID、 ProductName、 UnitPrice 和已停止使用的列](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.png)

**图 7**： 使用`Products`表，则返回`ProductID`， `ProductName`， `UnitPrice`，和`Discontinued`列 ([单击以查看实际尺寸的图像](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image10.png))


若要自动生成`INSERT`， `UPDATE`，和`DELETE`语句基于所选的表和列，单击高级按钮，然后检查生成`INSERT`， `UPDATE`，和`DELETE`语句复选框。


![检查生成 INSERT、 UPDATE 和 DELETE 语句复选框](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.gif)

**图 8**： 检查生成`INSERT`， `UPDATE`，和`DELETE`语句复选框


生成`INSERT`， `UPDATE`，和`DELETE`语句复选框仅将是可选如果选定的表具有主键，并且在返回的列的列表中包含的主键列 （或多个列）。 使用开放式并发复选框，则该命令将可选择一次生成`INSERT`， `UPDATE`，和`DELETE`语句复选框已选中，将增加`WHERE`在随后出现的子句`UPDATE`和`DELETE`语句，以提供乐观并发控制。 现在，将此复选框保留为未选中状态;在下一教程中，我们将检查与 SqlDataSource 控件的开放式并发。

在检查生成后`INSERT`， `UPDATE`，和`DELETE`语句复选框，单击确定以返回到配置 Select 语句屏幕中，然后单击下一步，并完成，以完成配置数据源向导。 完成向导之后，Visual Studio 会将 BoundFields 添加到用于说明如何`ProductID`， `ProductName`，和`UnitPrice`列和为 CheckBoxField`Discontinued`列。 从说明 s 智能标记，以便用户访问此页可以逐步执行产品检查启用分页选项。 说明如何 s 还清除`Width`和`Height`属性。

请注意，智能标记的启用插入、 启用编辑和启用删除选项。 这是因为 SqlDataSource 包含值其`InsertCommand`， `UpdateCommand`，和`DeleteCommand`，如下面的声明性语法所示：

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample3.aspx)]

请注意 SqlDataSource 控件已自动设置的值其`InsertCommand`， `UpdateCommand`，和`DeleteCommand`属性。 中引用的列的一套`InsertCommand`和`UpdateCommand`属性基于这些中`SELECT`语句。 也就是说，而不是使*每个*产品中的列`InsertCommand`和`UpdateCommand`，有中指定这些列`SelectCommand`(较少`ProductID`，这被省略，因为它 s [`IDENTITY`列](http://www.sqlteam.com/item.asp?ItemID=102)、 编辑时，不能更改其值和插入时自动分配)。 此外，每个参数中`InsertCommand`， `UpdateCommand`，和`DeleteCommand`有中相应的参数的属性`InsertParameters`， `UpdateParameters`，和`DeleteParameters`集合。

若要打开说明的数据修改功能，请检查启用插入、 启用编辑和启用删除其智能标记中的选项。 这将添加与 CommandField 其`ShowInsertButton`， `ShowEditButton`，和`ShowDeleteButton`属性设置为`True`。

访问在浏览器页面，并记下编辑、 删除和说明中包含的新按钮。 单击编辑按钮将说明如何变为编辑模式，其中显示了每个 BoundField 其`ReadOnly`属性设置为`False`（默认值） 作为文本框中，并作为一个复选框 CheckBoxField。


[![说明如何 s 默认的编辑界面](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image11.png)

**图 9**: 说明如何 s 默认编辑接口 ([单击以查看实际尺寸的图像](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image12.png))


同样，你可以删除当前所选的产品或向系统添加新产品。 由于`InsertCommand`语句仅适用于`ProductName`， `UnitPrice`，和`Discontinued`列，其他列具有或者`NULL`或在插入时的数据库分配其默认值。 如果使用对象数据源，就像`InsertCommand`缺少列不允许任何数据库表`NULL`s 和不要有默认值，将发生 SQL 错误时尝试执行`INSERT`语句。

> [!NOTE]
> 说明如何 s 插入和编辑接口缺少任何种类的自定义项或验证。 若要添加验证控件或自定义接口，你需要将 BoundFields 转换为 TemplateFields。 请参阅[将验证控件添加到的编辑和插入接口](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)和[自定义的数据修改界面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)有关详细信息的教程。


此外，请记住情况下，更新和删除，说明如何使用当前产品 s`DataKey`值，该值时才存在`DataKeyNames`属性配置。 如果编辑或删除似乎不起作用，确保`DataKeyNames`属性设置。

## <a name="limitations-of-automatically-generating-sql-statements"></a>自动生成 SQL 语句的限制

由于生成`INSERT`， `UPDATE`，和`DELETE`语句选项才可用，当选择从表中的列，为变得更加复杂的查询将需要编写自己`INSERT`， `UPDATE`，和`DELETE`像我们在步骤 1 中的语句。 通常情况下，SQL`SELECT`语句使用`JOIN`s 以使重新为的一个或多个查找表中的数据显示目的 (例如使重新`Categories`表的`CategoryName`字段显示产品信息时)。 同时，我们可能想要允许用户编辑、 更新或将数据插入核心表 (`Products`，在这种情况下)。

虽然`INSERT`， `UPDATE`，和`DELETE`语句可以手动输入，请考虑以下时间保存提示。 最初，以便它返回将仅从数据设置 SqlDataSource`Products`表。 使用表或视图的屏幕中的配置数据源向导 s 指定列，以便你可以自动生成`INSERT`， `UPDATE`，和`DELETE`语句。 然后，完成向导后，选择要配置从属性窗口 SelectQuery （或者，或者，请返回到的配置数据源向导，但使用指定的自定义 SQL 语句或存储的过程选项）。 然后更新`SELECT`语句以包含`JOIN`语法。 此方法有自动生成的 SQL 语句的时间保存优点，并允许更多自定义`SELECT`语句。

自动生成的另一个局限性`INSERT`， `UPDATE`，和`DELETE`语句是中的列`INSERT`和`UPDATE`语句基于返回的列`SELECT`语句。 我们可能需要更新或插入更多或更少的字段，但是。 例如，在步骤 2 中的示例中，可能是我们需要保留`UnitPrice`BoundField 是只读的。 在这种情况下，它不应 t 出现在`UpdateCommand`。 或者，我们可能想要在 GridView 中设置不会出现表字段的值。 例如，当添加新记录我们可能想`QuantityPerUnit`值设置为 TODO。

如果此类自定义项是必需的你需要手动，使它们通过属性窗口中，指定自定义 SQL 语句或存储的过程选项在向导中，或通过声明性语法。

> [!NOTE]
> 当添加参数不具有对数据中的相应字段 Web 控件时，请记住，这些参数值将需要被分配以某种方式的值。 这些值可以是： 硬编码直接在`InsertCommand`或`UpdateCommand`; 可以来自某些预定义源 （查询字符串、 会话状态、 Web 控件在页上，依次类推）; 也可以以编程方式，分配，如我们在前面的教程中看到。


## <a name="summary"></a>摘要

为了使数据 Web 一些控制以利用其内置插入、 编辑和删除功能，它们所绑定到数据源控件必须提供此类的功能。 对于 SqlDataSource，这意味着`INSERT`， `UPDATE`，和`DELETE`SQL 语句必须分配给`InsertCommand`， `UpdateCommand`，和`DeleteCommand`属性。 这些属性和相应的参数集合中，可以手动添加或配置数据源向导通过自动生成。 在本教程中，我们将探讨这两种技术。

我们探讨了与对象的数据源中使用开放式并发[实现开放式并发](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md)教程。 SqlDataSource 控件还提供乐观并发支持。 中所述步骤 2 中，自动生成时`INSERT`， `UPDATE`，和`DELETE`语句，该向导提供了一个使用开放式并发选项。 我们将在下一教程中看到的开放式并发使用 SqlDataSource 修改`WHERE`中的子句`UPDATE`和`DELETE`语句，以确保其他列的值没有更改自数据一次的 t页面上显示。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一页](using-parameterized-queries-with-the-sqldatasource-vb.md)
[下一页](implementing-optimistic-concurrency-with-the-sqldatasource-vb.md)
