---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
title: "包装在一个事务 (VB) 内的数据库修改 |Microsoft 文档"
author: rick-anderson
description: "本教程是 4 的倍数考察更新、 删除和插入的数据批的第一个。 在本教程中我们了解如何允许数据库事务..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 7d821db5-6cbb-4b38-af14-198f9155fc82
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
msc.type: authoredcontent
ms.openlocfilehash: f054445091edbc27263127fb3b7b851776ec617f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="wrapping-database-modifications-within-a-transaction-vb"></a>在事务 (VB) 中的包装数据库修改
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_VB.zip)或[下载 PDF](wrapping-database-modifications-within-a-transaction-vb/_static/datatutorial63vb1.pdf)

> 本教程是 4 的倍数考察更新、 删除和插入的数据批的第一个。 在本教程中我们了解如何数据库事务允许批处理的修改就可以执行以原子操作，这将确保所有步骤都成功或所有步骤都失败。


## <a name="introduction"></a>介绍

正如我们所看到开头[概述的插入、 更新和删除数据](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)教程中，GridView 提供内置支持行级编辑和删除。 只需单击几下鼠标就可以处理程序，但前提是要进行编辑和删除行数基于内容，而无需编写一行代码，创建丰富的数据修改接口。 但是，在某些情况下，这是不足，我们需要先向用户提供编辑或删除的记录批的能力。

例如，最基于 web 的电子邮件客户端使用网格列出每条消息，每个行中包含的电子邮件的 s 信息 （主题、 发件人，等） 以及一个复选框。 此接口允许用户通过检查它们，然后单击删除所选消息按钮删除多个消息。 编辑接口批处理非常适合在用户通常编辑许多不同记录的情况下。 而不强制用户通过单击编辑、 其更改，然后单击更新为需要修改每个记录，编辑接口一批呈现具有其编辑界面每个行。 用户可以快速修改组的需要更改的行，单击全部更新按钮，然后将保存这些更改。 在此系列教程中，我们将查看如何创建用于插入、 编辑和删除数据批的接口。

执行批处理操作时它非常重要，以确定是否应当可以为某些中批处理成功的操作而其他失败。 请考虑一批删除接口-如果成功，删除所选的第一个记录，但第二个失败，例如，由于外键约束冲突，应该发生什么情况？ 应该第一个记录的删除回滚或是否保持已删除的第一个记录可以接受？

如果你想要被视为的批处理操作[原子操作](http://en.wikipedia.org/wiki/Atomic_operation)，一个是其中的所有步骤成功或失败的所有步骤，则数据访问层需要进行扩充以包括对支持[数据库事务](http://en.wikipedia.org/wiki/Database_transaction)。 数据库事务保证的一套的原子性`INSERT`， `UPDATE`，和`DELETE`语句在事务的保护伞下执行，并且是所有的大多数现代数据库系统支持的功能。

在本教程中我们将查看如何扩展 DAL 使用数据库事务。 后续教程将检查批处理插入、 更新和删除接口的实现 web 页。 让我们来开始 ！

> [!NOTE]
> 在修改批事务中的数据时，不始终需要原子性。 在某些情况下，它可能是可以接受的成功一些数据修改和其他人在同一个批处理中失败，例如，当从基于 web 的电子邮件客户端中删除一组的电子邮件。 如果存在 s 数据库错误会中途删除处理，它可能可接受的正常处理这些记录保留已删除的 s。 在这种情况下，不需要修改以支持数据库事务 DAL。 有其他批处理操作方案中，但是，原子性至关重要。 当客户将她资金从一个银行帐户移到另一个中时，必须执行两个操作： 必须从第一个帐户中扣除并随后将添加到第二个中涉及的金额。 银行可能不介意成功的第一步，但第二个步骤失败，其客户可能不难理解会满意。 我建议你完成本教程中，并实现与 DAL 以支持数据库事务，即使你不计划上使用它们在批处理插入、 更新和删除我们将在以下三个教程中生成的接口的增强功能。


## <a name="an-overview-of-transactions"></a>事务的概述

大多数数据库包括对支持*事务*，它实现了多个数据库命令，分组到单个逻辑单元的工作。 保证构成事务的数据库命令都是原子性的这意味着所有命令将都失败，或所有将会成功。

一般情况下，通过使用以下模式的 SQL 语句实现事务：

1. 指示启动事务。
2. 执行构成事务的 SQL 语句。
3. 如果没有在步骤 2，回滚事务中的语句之一时出错。
4. 如果所有步骤 2 中的语句完成而未出现错误，则提交事务。

SQL 语句用于创建、 提交和回滚事务时，可以输入手动编写 SQL 脚本或创建存储过程，或通过编程意味着要使用 ADO.NET 或中的类[ `System.Transactions`命名空间](https://msdn.microsoft.com/library/system.transactions.aspx)。 在本教程中我们将仅检查管理使用 ADO.NET 的事务。 在将来的教程我们将了解如何在数据访问层，在这段时间中，我们将探讨用于创建、 回滚，和提交事务的 SQL 语句中使用存储的过程。 在此期间，请查阅[在 SQL Server 存储过程中管理事务](http://www.4guysfromrolla.com/webtech/080305-1.shtml)有关详细信息。

> [!NOTE]
> [ `TransactionScope`类](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx)中`System.Transactions`命名空间使开发人员能够以编程方式在事务范围内换行一系列语句并包含对，则涉及多个复杂事务支持两个不同的数据库或甚至异类类型的数据存储，如 Microsoft SQL Server 数据库、 Oracle 数据库和 Web 服务等源。 我已确定而不是本教程使用 ADO.NET 事务`TransactionScope`类，因为 ADO.NET 是更具体的数据库事务，并在许多情况下，是小得多占用大量的资源。 此外，在某些情况下`TransactionScope`类使用 Microsoft 分布式事务处理协调器 (MSDTC)。 配置、 实施和性能问题周围 MSDTC 使它而是专用的高级主题和超出范围的这些教程。


在使用 SqlClient 提供程序在 ADO.NET 中，通过调用启动事务[`SqlConnection`类](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx)s [ `BeginTransaction`方法](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx)，它将返回[ `SqlTransaction`对象](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx)。 中放入了构成事务的数据修改语句`try...catch`块。 如果错误发生在中的语句`try`一直阻止，请执行传输至`catch`块其中事务可以回滚通过`SqlTransaction`对象 s [ `Rollback`方法](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx)。 如果所有这些语句成功，完成调用`SqlTransaction`对象 s [ `Commit`方法](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx)末尾`try`块提交事务。 下面的代码段演示此模式。 请参阅[维护与事务的数据库一致性](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)有关额外的语法和示例的 ADO.NET 中使用事务。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample1.vb)]

默认情况下，类型化数据集中 Tableadapter 不使用事务。 若要为事务提供支持，我们需要增加要包括的其他方法，使用上面的模式来执行一系列的范围内的事务数据修改语句的 TableAdapter 类。 在步骤 2 中，我们将了解如何使用分部类来添加这些方法。

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>步骤 1： 使用批处理的数据 Web 页创建工作

我们开始浏览如何增加 DAL 以支持数据库事务之前，让我们来首先花一些时间创建本教程中我们将需要的 ASP.NET web 页面，并且按照的三个。 首先，通过添加一个名为的新文件夹`BatchData`，然后添加以下将与每个页面相关联的 ASP.NET 页`Site.master`母版页。

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![SqlDataSource 相关教程添加的 ASP.NET 页面](wrapping-database-modifications-within-a-transaction-vb/_static/image1.gif)

**图 1**： 添加 ASP.NET 页 SqlDataSource 相关教程


与其他文件夹，`Default.aspx`将使用`SectionLevelTutorialListing.ascx`用户控件，若要列出其部分中的教程。 因此，此用户将控件添加到`Default.aspx`通过从解决方案资源管理器拖到页面上的设计视图拖动。


[![SectionLevelTutorialListing.ascx 用户控件添加到 Default.aspx](wrapping-database-modifications-within-a-transaction-vb/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image1.png)

**图 2**： 添加`SectionLevelTutorialListing.ascx`用户控件`Default.aspx`([单击以查看实际尺寸的图像](wrapping-database-modifications-within-a-transaction-vb/_static/image2.png))


最后，将这些四个页面添加到的条目为`Web.sitemap`文件。 具体而言，自定义后添加以下标记站点图`<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample2.xml)]

在更新后`Web.sitemap`，花些时间查看通过浏览器网站的教程。 在左侧菜单现在包括批处理的数据教程使用的项。


![站点图现在包括批处理的数据教程使用的项](wrapping-database-modifications-within-a-transaction-vb/_static/image3.gif)

**图 3**： 站点图现在包括批处理的数据教程使用的项


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>步骤 2： 更新数据访问层，以便支持数据库事务

返回在第一个教程中，我们讨论[创建数据访问层](../introduction/creating-a-data-access-layer-vb.md)，我们 DAL 中的类型数据集组成数据表和 tableadapter 分离。 数据表保存数据时 Tableadapter 提供数据从数据库中读取到数据表，以更新数据库的到数据表和等的更改的功能。 回想一下 Tableadapter 用于更新数据，我称为批处理更新和 DB 直接提供两种模式。 使用批处理更新模式中，数据集、 数据表或返回数据行的集合传递 TableAdapter。 此数据枚举和每个插入、 修改或删除行， `InsertCommand`， `UpdateCommand`，或`DeleteCommand`执行。 使用数据库直接模式中，TableAdapter 改为传递列插入、 更新或删除单个记录所需的值。 DB 直接模式方法然后使用这些传入的值来执行相应`InsertCommand`， `UpdateCommand`，或`DeleteCommand`语句。

无论所使用的更新模式，自动生成的 Tableadapter 方法不使用事务。 默认情况下每个插入、 更新或删除由 TableAdapter 将被视为单个独立的操作。 例如，假设由 BLL 中某些代码使用 DB 直接模式是将 10 个记录插入数据库。 此代码将调用 TableAdapter 的`Insert`方法十倍。 如果前五个插入操作成功，但第六个一个导致异常，将在数据库中保留的前五个插入的记录。 同样，如果使用批处理更新模式执行插入、 更新和删除到插入修改，或删除数据表中的行，如果第一个多种不同的修改成功，但更高版本时出错，这些较早的修改，完成将保留在数据库中。

在某些情况下，我们想要跨的一系列修改确保原子性。 若要完成此我们必须通过添加新方法的执行手动扩展 TableAdapter `InsertCommand`， `UpdateCommand`，和`DeleteCommand`s 之下的事务。 在[创建数据访问层](../introduction/creating-a-data-access-layer-vb.md)我们看使用[分部类](http://en.wikipedia.org/wiki/Partial_type)来扩展在类型化数据集中数据表的功能。 此方法还可以用于 Tableadapter。

类型化数据集`Northwind.xsd`位于`App_Code`文件夹的`DAL`子文件夹。 创建的子文件夹中`DAL`文件夹名为`TransactionSupport`并添加名为的新类文件`ProductsTableAdapter.TransactionSupport.vb`（请参见图 4）。 此文件将保存的部分实现`ProductsTableAdapter`，包括用于执行使用事务的数据修改的方法。


![添加一个名为 TransactionSupport 文件夹和一个名为 ProductsTableAdapter.TransactionSupport.vb 的类文件](wrapping-database-modifications-within-a-transaction-vb/_static/image4.gif)

**图 4**： 添加名为的文件夹`TransactionSupport`和一个名为的类文件`ProductsTableAdapter.TransactionSupport.vb`


输入下面的代码插入`ProductsTableAdapter.TransactionSupport.vb`文件：


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample3.vb)]

`Partial`向中添加的成员是要添加到编译器的类声明中的关键字指示`ProductsTableAdapter`类`NorthwindTableAdapters`命名空间。 请注意`Imports System.Data.SqlClient`语句文件的顶部。 TableAdapter 配置为使用 SqlClient 提供程序，因为内部它使用[ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx)要向数据库发出其命令对象。 因此，我们需要使用`SqlTransaction`类以开始事务，然后将其提交或回滚它。 如果你使用的 Microsoft SQL Server 之外的数据存储，你将需要使用相应的提供程序。

这些方法提供开始回滚时，所需的构建基块，并提交事务。 它们标记`Public`，使其能够从使用`ProductsTableAdapter`、 DAL 中的另一个类或结构，如 BLL 中的另一层。 `BeginTransaction`将打开 TableAdapter s 内部`SqlConnection`（如果需要），开始事务，并将它分配给`Transaction`属性，并将事务附加到内部`SqlDataAdapter`s`SqlCommand`对象。 `CommitTransaction`和`RollbackTransaction`调用`Transaction`对象 s`Commit`和`Rollback`方法，分别之前关闭内部`Connection`对象。

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>步骤 3： 添加方法以更新和删除数据之下的事务

使用这些方法完成的情况下，我们重新准备好将方法添加到`ProductsDataTable`或 BLL 执行一系列之下的事务的命令。 以下方法使用批处理更新模式来更新`ProductsDataTable`实例使用事务。 它通过调用启动事务`BeginTransaction`方法，然后使用`Try...Catch`用于发出数据修改语句块。 如果调用`Adapter`对象 s`Update`方法会引发异常，则执行会传输到`catch`其中将回滚事务的块和重新引发的异常。 回想一下，`Update`方法通过枚举所提供的行来实现批处理更新模式`ProductsDataTable`并执行所需`InsertCommand`， `UpdateCommand`，和`DeleteCommand`s。 如果任何一个的这些命令将导致错误，事务将回滚撤消以前的事务 s 生命周期内所做的修改。 应`Update`语句完成而未出现错误，则整个提交事务。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample4.vb)]

添加`UpdateWithTransaction`方法`ProductsTableAdapter`类中的分部类通过`ProductsTableAdapter.TransactionSupport.vb`。 或者，此方法无法添加到业务逻辑层的`ProductsBLL`有几个次要的语法更改的类。 也就是说，关键字`Me`中`Me.BeginTransaction()`， `Me.CommitTransaction()`，和`Me.RollbackTransaction()`需要替换为`Adapter`(回想一下，`Adapter`是中的属性的名称`ProductsBLL`类型的`ProductsTableAdapter`)。

`UpdateWithTransaction`方法使用批处理更新模式中，但还可以在事务，如下所示方法范围内使用一系列 DB 直接调用。 `DeleteProductsWithTransaction`方法接受作为输入`List(Of T)`类型的`Integer`，这是`ProductID`s 可删除。 方法启动通过调用事务`BeginTransaction`，然后在`Try`阻止，循环访问所提供的列表，调用 DB 直接模式`Delete`方法为每个`ProductID`值。 如果调用的任何`Delete`失败，将控制传输到`Catch`块其中事务将回滚并重新引发的异常。 如果所有调用`Delete`成功，则会提交事务。 将以下方法添加到`ProductsBLL`类。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample5.vb)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>跨多个 Tableadapter 应用事务

在本教程中检查与事务相关代码允许使用多个语句`ProductsTableAdapter`视为原子操作。 但是，如果多个不同的数据库表进行修改需要以原子方式执行？ 例如，当删除某个类别时，我们可能会首先要重新分配到某些其他类别及其当前产品。 作为一个原子操作，应执行这两个步骤重新分配产品和删除类别。 但是`ProductsTableAdapter`仅包括方法修改`Products`表和`CategoriesTableAdapter`仅包括方法修改`Categories`表。 那么，如何事务可以包含两个 Tableadapter

一种选择是将方法添加到`CategoriesTableAdapter`名为`DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)`然后使该方法调用存储的过程，同时重新分配产品并删除存储过程内定义的事务范围内的类别。 我们将了解如何开始、 提交，以及存储过程中的回滚事务在将来的教程。

另一个选项是在包含 DAL 中创建一个帮助器类`DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)`方法。 此方法将创建的实例`CategoriesTableAdapter`和`ProductsTableAdapter`然后将这些设置两个 Tableadapter`Connection`属性设置为相同`SqlConnection`实例。 在该点，两个 Tableadapter 中的任一将启动的事务，以及调用`BeginTransaction`。 重新分配产品和删除类别的 Tableadapter 方法将调用中`Try...Catch`与事务提交或回滚返回根据需要的块。

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>步骤 4： 添加`UpdateWithTransaction`业务逻辑层的方法

在步骤 3 中，我们将添加`UpdateWithTransaction`方法`ProductsTableAdapter`DAL 中。 我们应将相应的方法添加到 BLL。 表示层可以调用直接下 DAL 调用时`UpdateWithTransaction`方法，这些教程具有努力定义使与表示层 DAL 一个分层体系结构。 因此，它 behooves 我们能够继续这种方法。

打开`ProductsBLL`类文件，并添加一个名为方法`UpdateWithTransaction`到相应的 DAL 方法只需调用。 现在应该在两个新方法`ProductsBLL`: `UpdateWithTransaction`，只需添加和`DeleteProductsWithTransaction`，它被添加在步骤 3 中。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample6.vb)]

> [!NOTE]
> 这些方法不包括`DataObjectMethodAttribute`分配给中的大多数其他方法特性`ProductsBLL`类，因为我们将调用这些方法直接从 ASP.NET 页代码隐藏类。 回想一下，`DataObjectMethodAttribute`用于标记何种方法应出现在 ObjectDataSource 的配置数据源向导和 （选择、 更新、 插入或删除） 哪些选项卡下。 因为 GridView 中没有批处理编辑或删除任何内置支持，我们将需要以编程方式调用这些方法，而不是使用无需编码的声明性方法。


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>步骤 5： 以原子方式更新数据库数据的表示层

若要说明的影响事务更新一批记录时，让 s 创建列出一个 GridView 中的所有产品，并包含按钮 Web 的用户界面控件，单击时，重新分配产品`CategoryID`值。 具体而言，类别重新指派将进度，以便第一个有好几种产品分配一个有效`CategoryID`分配不存在的值而有些则是有意`CategoryID`值。 如果我们尝试使用产品更新数据库其`CategoryID`与现有类别 s 不匹配`CategoryID`，将发生违反外键约束，并且将引发异常。 我们将看到在此示例是，如果使用事务从外键约束冲突引发的异常将导致以前有效`CategoryID`更改将被回滚。 当未使用事务，但是，将保持对初始类别进行修改。

首先打开`Transactions.aspx`页面`BatchData`文件夹，然后拖动一个 GridView 从工具箱中拖动到设计器。 设置其`ID`到`Products`和从其智能标记，请将其绑定到名为新 ObjectDataSource `ProductsDataSource`。 配置对象数据源将从其数据拉`ProductsBLL`类的`GetProducts`方法。 这将是只读的 GridView，因此设置下拉列表在更新中，插入和删除选项卡添加到 （无） 并单击完成。


[![配置对象数据源以使用 ProductsBLL 类的 GetProducts 方法](wrapping-database-modifications-within-a-transaction-vb/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image3.png)

**图 5**： 配置使用 ObjectDataSource`ProductsBLL`类 s`GetProducts`方法 ([单击以查看实际尺寸的图像](wrapping-database-modifications-within-a-transaction-vb/_static/image4.png))


[![在更新中，INSERT、 设置的下拉列表和删除选项卡到 （无）](wrapping-database-modifications-within-a-transaction-vb/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image5.png)

**图 6**： 设置的下拉列表中更新、 插入和删除选项卡为 （无） ([单击以查看实际尺寸的图像](wrapping-database-modifications-within-a-transaction-vb/_static/image6.png))


完成配置数据源向导后，Visual Studio 将创建 BoundFields 和产品数据字段 CheckBoxField。 删除所有除这些字段`ProductID`， `ProductName`， `CategoryID`，和`CategoryName`和重命名`ProductName`和`CategoryName`BoundFields`HeaderText`属性设置为产品和类别，分别。 通过智能标记中，选中启用分页选项。 进行这些修改后, 的 GridView 和 ObjectDataSource s 声明性标记应如下所示：


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample7.aspx)]

接下来，添加上面 GridView 的三个按钮 Web 控件。 设置为刷新网格、 修改类别 （与事务），到第二个 s 和修改类别 （而无需事务） 到第三个一个 s s Text 属性的第一个按钮。


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample8.aspx)]

此时 Visual Studio 中的设计视图应类似于的屏幕快照中图 7 所示。


[![页包含一个 GridView 和三个按钮 Web 控件](wrapping-database-modifications-within-a-transaction-vb/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image7.png)

**图 7**： 的页面包含一个 GridView 和三个按钮 Web 控件 ([单击以查看实际尺寸的图像](wrapping-database-modifications-within-a-transaction-vb/_static/image8.png))


为每个三个按钮 s 创建事件处理程序`Click`事件并使用下面的代码：


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample9.vb)]

刷新按钮 s`Click`事件处理程序仅重新绑定到 GridView 数据，通过调用`Products`GridView 的`DataBind`方法。

第二个事件处理程序重新分配产品`CategoryID`s 并使用新的事务方法从 BLL 执行数据库更新之下的事务。 请注意，每个产品 s`CategoryID`任意设置相同的值为其`ProductID`。 这将不错的第一个几种产品，因为这些产品具有`ProductID`碰巧将映射到有效的值`CategoryID`s。 但一次`ProductID`s 开始变得过大，这种巧合重叠的`ProductID`s 和`CategoryID`s 不再适用。

第三个`Click`事件处理程序更新产品`CategoryID`中相同的方式，但将更新发送到数据库使用`ProductsTableAdapter`s 默认`Update`方法。 这`Update`方法不会包装在事务中的命令序列，以便在第一个遇到外键约束冲突错误之前进行这些更改将保持不变。

若要演示此行为，请访问此页通过浏览器。 最初，在图 8 所示，你应看到数据的第一页。 接下来，单击修改类别 （与事务） 按钮。 这将导致回发，尝试更新的所有产品`CategoryID`值，但将导致违反外键约束 （请参阅图 9）。


[![产品显示在可分页的 GridView](wrapping-database-modifications-within-a-transaction-vb/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image9.png)

**图 8**: 的产品显示在可分页 GridView ([单击以查看实际尺寸的图像](wrapping-database-modifications-within-a-transaction-vb/_static/image10.png))


[![重新分配类别导致违反外键约束](wrapping-database-modifications-within-a-transaction-vb/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image11.png)

**图 9**： 重新分配类别导致违反了外键约束 ([单击以查看实际尺寸的图像](wrapping-database-modifications-within-a-transaction-vb/_static/image12.png))


现在命中你浏览器 s 后退按钮，然后单击刷新网格按钮。 在刷新数据时你应看到完全相同的输出，如图 8 中所示。 也就是说，即使尽管某些产品`CategoryID`s 已更改为合法的值并在数据库中更新，它们时回滚发生外键约束冲突。

现在请尝试单击修改类别 （而无需事务） 按钮。 这将导致相同的外键约束冲突错误 （请参阅图 9），但这次请这些产品其`CategoryID`值已更改为合法值将不会回滚。 命中你浏览器 s 后退按钮，然后刷新网格按钮。 如图 10 显示， `CategoryID` s 前八个产品已被重新分配。 例如，在图 8 中，更改必须`CategoryID`为 1，但在图 10 it s 已重新分配到 2。


[![某些产品 CategoryID 值未更新而其他人已](wrapping-database-modifications-within-a-transaction-vb/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image13.png)

**图 10**： 某些产品`CategoryID`值未更新而其他人已 ([单击以查看实际尺寸的图像](wrapping-database-modifications-within-a-transaction-vb/_static/image14.png))


## <a name="summary"></a>摘要

默认情况下，TableAdapter 的方法在事务范围内未包装的执行的数据库语句，但我们可以轻而易举地添加将创建的方法、 提交和回滚事务。 在本教程中，我们将创建三个此类方法中的`ProductsTableAdapter`类： `BeginTransaction`， `CommitTransaction`，和`RollbackTransaction`。 我们已了解如何使用这些方法以及`Try...Catch`用于进行数据修改语句一系列原子块。 具体而言，我们创建`UpdateWithTransaction`中的方法`ProductsTableAdapter`，它用批处理更新模式来执行必要的修改提供的行`ProductsDataTable`。 我们还添加了`DeleteProductsWithTransaction`方法`ProductsBLL`中 BLL，它接受类`List`的`ProductID`值作为其输入并调用 DB 直接模式方法`Delete`每个`ProductID`。 这两种方法启动： 创建事务，然后执行内的数据修改语句`Try...Catch`块。 如果发生异常，事务将回滚，否则它是已提交。

步骤 5 所示的与批处理更新忽略使用事务的事务性批处理更新的效果。 接下来三个教程中我们将在本教程中的布局的基础之上构建，并创建用于执行批处理更新、 删除和插入的用户界面。

尽情享受编程 ！

## <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [维护与事务的数据库一致性](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [管理 SQL Server 中的事务存储过程](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [事务变得更容易：`System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope 和 Dataadapter](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [在.NET 中使用 Oracle 数据库事务](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Dave Gardner、 希尔顿 Giesenow 和 Teresa 墨。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](batch-inserting-cs.md)
[下一页](batch-updating-vb.md)
