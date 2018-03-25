---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: "创建新存储过程的类型化数据集的 Tableadapter (C#) |Microsoft 文档"
author: rick-anderson
description: "在前面的教程中，我们已经在我们的代码中创建 SQL 语句，并且传递给数据库要执行的语句。 另一种方法是使用 s..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 751282ca-5870-4d66-84e4-6cefae23eb4a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: 715f7e3fae89e773b686faa7c49522c587693eec
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>创建新存储过程的类型化数据集的 Tableadapter (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_CS.zip)或[下载 PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial67cs1.pdf)

> 在前面的教程中，我们已经在我们的代码中创建 SQL 语句，并且传递给数据库要执行的语句。 另一种方法是使用存储的过程的 SQL 语句都是预定义在数据库。 在本教程中，我们将了解如何为我们生成新的存储的过程使用 TableAdapter 向导。


## <a name="introduction"></a>介绍

有关这些教程将数据访问层 (DAL) 使用类型化数据集。 中所述[创建数据访问层](../introduction/creating-a-data-access-layer-cs.md)教程中，类型化数据集包含强类型的数据表和 Tableadapter。 数据表表示基础数据库以执行数据访问工作的 Tableadapter 接口时系统中的逻辑实体。 这包括填充数据表的数据、 执行查询，以返回标量数据和插入、 更新和从数据库中删除记录。

执行由 Tableadapter 的 SQL 命令可以是临时的 SQL 语句，如`SELECT columnList FROM TableName`，或存储过程。 在我们的体系结构 Tableadapter 使用临时 SQL 语句。 许多开发人员和数据库管理员，但是，通过临时 SQL 语句的安全性、 可维护性和 updateability 原因希望存储的过程。 有些 ardently 选择其灵活性的临时 SQL 语句。 在我自己的工作我优先通过临时 SQL 语句的存储的过程，但选择了使用临时 SQL 语句来简化早期的教程。

当定义 TableAdapter 或添加新方法，TableAdapter 的向导发出它一样轻松地创建新存储的过程或使用现有存储的过程一样，使用临时 SQL 语句。 在本教程中我们将研究如何使 TableAdapter 的向导自动生成的存储的过程。 在下一教程中我们将了解如何配置以使用现有的或手动创建的存储的过程的 TableAdapter 的方法。

> [!NOTE]
> 请参阅 Rob Howard 博客文章[尚未不使用存储过程？](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/)和[Frans Bouma](https://weblogs.asp.net/fbouma/) s 博客文章[存储过程是错误，M Kay？](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx)上的利与弊生动容许存储的过程和即席 SQL。


## <a name="stored-procedure-basics"></a>存储的过程的基础知识

函数是普遍适用于所有编程语言构造。 函数是调用函数时执行的语句的集合。 函数可以接受输入的参数和 （可选） 可以返回的值。 *[存储过程](http://en.wikipedia.org/wiki/Stored_procedure)*的数据库构造，函数编程语言中与共享许多相似之处。 存储的过程由一组时调用存储的过程执行的 T-SQL 语句组成。 存储的过程可接受 0 到多个输入参数，并可以返回标量值、 输出参数，或，结果通常情况下，设置从`SELECT`查询。

> [!NOTE]
> 存储的过程通常称为 sprocs 或 SPs。


使用创建的存储的过程[ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) T-SQL 的语句。 例如，以下 T-SQL 脚本创建名为的存储的过程`GetProductsByCategoryID`中一个名为的单个参数采用`@CategoryID`并返回`ProductID`， `ProductName`， `UnitPrice`，和`Discontinued`中这些列的字段`Products`具有匹配的表`CategoryID`值：


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

一旦创建此存储的过程后，它可以使用以下语法来调用：


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.sql)]

> [!NOTE]
> 在下一教程中，我们将检查创建通过 Visual Studio IDE 的存储的过程。 对于本教程，但是，我们将让 TableAdapter 向导自动为我们生成的存储的过程。


除了只需返回数据，存储的过程通常用于执行单个事务的作用域内的多个数据库命令。 存储的过程名为`DeleteCategory`，例如，可能需要`@CategoryID`参数和执行两个`DELETE`语句： 第一个，一个要删除相关的产品和第二个删除指定的类别。 存储过程中的多个语句*不*在事务中自动换行。 其他的 T-SQL 命令需要发出以确保存储的过程的多个命令将被视为原子操作的 s。 我们将了解如何在后续教程中的事务的作用域内换行存储的过程的命令。

在体系结构中使用存储的过程，数据访问层的方法将调用特定的存储的过程，而不是发出的临时 SQL 语句。 而不是使它在应用程序 s 体系结构内定义，可实现集中化 （数据库） 上执行的 SQL 语句的位置。 此集中说轻松地查找、 分析和优化查询，并提供有关在何处以及如何将正在使用的数据库更清楚地了解情况。

存储的过程的基础知识的详细信息，请在本教程末尾查阅其他阅读材料部分中的资源。

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>步骤 1： 创建高级的数据访问层方案 Web 页

我们讨论开始上创建使用存储的过程 DAL 之前，让 s 先花点时间网站项目，我们将需要此信息以及下一步的几个教程中创建 ASP.NET 页。 首先，通过添加一个名为的新文件夹`AdvancedDAL`。 接下来，添加到该文件夹，并确保将每个包含网页相关联的以下 ASP.NET 页面`Site.master`母版页：

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![高级的数据访问层方案教程添加的 ASP.NET 页面](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**图 1**： 高级的数据访问层方案教程添加的 ASP.NET 页面


在其他文件夹中，如`Default.aspx`中`AdvancedDAL`文件夹将在其部分中列出这些教程。 回想一下，`SectionLevelTutorialListing.ascx`用户控件提供此功能。 因此，此用户将控件添加到`Default.aspx`通过从解决方案资源管理器拖到页面上的设计视图拖动。


[![SectionLevelTutorialListing.ascx 用户控件添加到 Default.aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)

**图 2**： 添加`SectionLevelTutorialListing.ascx`用户控件`Default.aspx`([单击以查看实际尺寸的图像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png))


最后，将这些页面添加到的条目为`Web.sitemap`文件。 具体而言，在处理批处理数据后添加以下标记`<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.xml)]

在更新后`Web.sitemap`，花些时间查看通过浏览器网站的教程。 现在，在左侧菜单在高级 DAL 方案教程包括项。


![站点图现在包括高级的 DAL 方案教程的条目](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)

**图 3**： 站点图现在包括高级的 DAL 方案教程的条目


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>步骤 2： 配置 TableAdapter 创建新存储过程

为了演示创建数据访问层，而不是临时的 SQL 语句中使用存储的过程，让 s 创建新类型化数据集在`~/App_Code/DAL`文件夹名为`NorthwindWithSprocs.xsd`。 由于我们具有单步执行此过程的详细信息通过在前面的教程中，我们将继续执行快速通过此处的步骤。 如果你遇到困难或需要进一步中创建和配置类型化数据集的分步说明，回头参考[创建数据访问层](../introduction/creating-a-data-access-layer-cs.md)教程。

将新的数据集添加到项目中，通过右键单击`DAL`文件夹中，选择添加新项，并选择数据集模板，如图 4 中所示。


[![将新的类型化数据集添加到名为 NorthwindWithSprocs.xsd 的项目](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png)

**图 4**： 将新的类型化数据集添加到项目的名称为`NorthwindWithSprocs.xsd`([单击以查看实际尺寸的图像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png))


这将创建新类型化数据集、 打开其设计器，创建新的 TableAdapter，并启动 TableAdapter 配置向导。 TableAdapter 配置向导的首先让我们选择要使用的数据库。 到 Northwind 数据库的连接字符串应下拉列表中列出。 选择此选项，然后单击下一步。

从此下一个屏幕中，我们可以选择 TableAdapter 访问数据库的方式。 在前面的教程，我们将选择第一个选项，使用 SQL 语句。 对于本教程，选择第二个选项，创建新的存储的过程，然后单击下一步。


[![指示 TableAdpater 创建新的存储的过程](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png)

**图 5**： 指示创建新存储过程到 TableAdpater ([单击以查看实际尺寸的图像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png))


就像使用临时 SQL 语句，下面步骤中我们会要求提供`SELECT`TableAdapter s 主查询语句。 但而不是使用`SELECT`此处输入直接执行即席查询的语句，TableAdapter 的向导将创建包含此存储的过程`SELECT`查询。

使用以下`SELECT`为此 TableAdapter 查询：


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]


[![输入 SELECT 查询](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png)

**图 6**： 输入`SELECT`查询 ([单击以查看实际尺寸的图像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png))


> [!NOTE]
> 上面的查询与略有不同的主查询`ProductsTableAdapter`中`Northwind`类型化数据集。 回想一下，`ProductsTableAdapter`中`Northwind`类型化数据集包含两个相关的子查询，以使重新的类别名称和每个产品的类别和供应商的公司名称。 在即将到来[使用联接到更新 TableAdapter](updating-the-tableadapter-to-use-joins-cs.md)教程，我们将着眼于添加到此 TableAdapter 相关数据。


花些时间单击高级选项按钮。 从此处，我们可以指定是否向导应还生成 insert、 update 和 delete 语句 TableAdapter，是否使用乐观并发，以及是否应在插入和更新后中刷新数据表。 生成 Insert、 Update 和 Delete 语句选项是默认选中的。 请将其选中。 对于本教程，请取消选中使用开放式并发选项。

当遇到 TableAdapter 向导通过自动创建的存储的过程，它将显示刷新数据的表选项将被忽略。 无论是否选中此复选框，生成 insert 和 update 存储的过程检索只插入或刚更新的记录，正如我们将在步骤 3 中看到。


![保留选中选项生成 Insert、 Update 和 Delete 语句](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png)

**图 7**： 保留选中选项生成 Insert、 Update 和 Delete 语句


> [!NOTE]
> 如果选中使用开放式并发选项，则该向导将添加到的附加条件`WHERE`防止数据在其他字段并无变化如果正在更新的子句。 将回指[实现开放式并发](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md)教程，以使用 TableAdapter s 内置乐观并发控制功能的详细信息。


在输入后`SELECT`查询并确认，生成 Insert、 Update 和 Delete 语句选项处于选中状态，单击下一步。 此下一个屏幕，图 8 所示，提示输入选择、 插入、 更新和删除数据的向导将创建的存储过程的名称。 更改这些存储过程名称传递给`Products_Select`， `Products_Insert`， `Products_Update`，和`Products_Delete`。


[![重命名存储的过程](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**图 8**： 重命名存储过程 ([单击以查看实际尺寸的图像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


若要查看的 T-SQL TableAdapter 向导将使用创建四个存储的过程，请单击预览 SQL 脚本按钮。 从预览 SQL 脚本对话框中可能将脚本保存到文件或将其复制到剪贴板。


![预览使用生成的存储的过程的 SQL 脚本](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**图 9**： 使用 SQL 脚本来生成的存储的过程的预览


命名的存储的过程之后, 单击旁边名称 TableAdapter s 相应方法。 如同时使用的临时 SQL 语句，我们可以创建填充现有数据表或返回一个新的方法。 我们还可以指定 TableAdapter 是否应包含插入、 更新和删除记录的 DB 直接模式。 不要选中，所有的三个复选框，但重命名返回的 DataTable 方法`GetProducts`（如图 10 中所示）。


[![命名方法填充和 GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)

**图 10**： 命名方法`Fill`和`GetProducts`([单击以查看实际尺寸的图像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png))


单击下一步以查看该向导将执行的步骤的摘要。 通过单击完成按钮完成向导。 完成向导后，你将返回到设计器，现在应包含的数据集 s `ProductsDataTable`。


[![数据集 s 设计器将显示新添加的 ProductsDataTable](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)

**图 11**: 数据集 s 设计器显示新添加`ProductsDataTable`([单击以查看实际尺寸的图像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>第 3 步： 检查新创建的存储的过程

TableAdapter 向导自动使用在步骤 2 中创建用于选择、 插入、 更新和删除数据的存储的过程。 这些存储的过程可以查看或修改通过 Visual Studio 通过转到服务器资源管理器，并向下钻取到的数据库 s 的存储过程文件夹。 如图 12 所示，Northwind 数据库包含四个新的存储的过程： `Products_Delete`， `Products_Insert`， `Products_Select`，和`Products_Update`。


![可以在数据库 s 存储的过程文件夹中找到在步骤 2 中创建的四个存储的过程](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)

**图 12**： 可以在数据库 s 存储的过程文件夹中找到在步骤 2 中创建的四个存储的过程


> [!NOTE]
> 如果看不到服务器资源管理器，转到视图菜单，然后选择服务器资源管理器选项。 如果看不到步骤 2 中添加与产品相关的存储的过程，请尝试在存储过程文件夹上右键单击并选择刷新。


若要查看或修改存储的过程，请双击它在服务器资源管理器中的名称或，或者，右键单击该存储过程并选择打开。 图 13 显示`Products_Delete`存储过程中，打开时。


[![可以打开和修改 Visual Studio 中的存储的过程](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png)

**图 13**： 存储过程可以打开和修改在 Visual Studio 的 ([单击以查看实际尺寸的图像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png))


二者的内容`Products_Delete`和`Products_Select`存储的过程是非常简单。 `Products_Insert`和`Products_Update`存储的过程，另一方面，保证仔细它们都执行`SELECT`后的语句其`INSERT`和`UPDATE`语句。 例如，下面的 SQL 组成`Products_Insert`存储过程：


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

存储的过程接受作为输入参数`Products`列由`SELECT`TableAdapter 的向导和这些值中指定的查询中使用`INSERT`语句。 以下`INSERT`语句，`SELECT`查询用于返回`Products`列的值 (包括`ProductID`) 的新添加的记录。 在添加新记录，因为它使用批处理更新模式，自动更新新添加的时间，此刷新功能十分有用`ProductRow`实例`ProductID`具有分配的数据库的自动递增的值的属性。

下面的代码演示此功能。 它包含`ProductsTableAdapter`和`ProductsDataTable`为创建`NorthwindWithSprocs`类型化数据集。 通过创建将新产品添加到数据库`ProductsRow`实例，提供它的值，并调用 TableAdapter s`Update`方法，同时传入`ProductsDataTable`。 在内部，TableAdapter s`Update`方法枚举`ProductsRow`传入的 DataTable 中的实例 （在此示例中都只有一个控件的一个我们刚添加），并执行相应的插入、 更新或删除命令。 在这种情况下，`Products_Insert`执行存储的过程，从而将添加到一条新记录`Products`表并返回新添加记录的详细信息。 `ProductsRow`实例的`ProductID`然后更新值。 后`Update`方法已完成，我们就可以访问的新添加的记录 s`ProductID`值通过`ProductsRow`s`ProductID`属性。


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.cs)]

`Products_Update`存储的过程同样包括`SELECT`后的语句其`UPDATE`语句。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample7.sql)]

请注意，此存储过程包含两个输入的参数`ProductID`:`@Original_ProductID`和`@ProductID`。 此功能允许的方案可能已更改的主键。 例如，在雇员数据库，每个员工记录可能会使用员工的社会安全号作为其主键。 若要更改现有员工的社会安全号码，必须提供新的社会安全号和原始。 有关`Products`无需执行表，此类功能，因为`ProductID`列为`IDENTITY`列，应永远不会更改。 事实上，`UPDATE`中的语句`Products_Update`存储的过程不包括`ProductID`其列列表中的列。 因此，尽管`@Original_ProductID`中使用`UPDATE`语句 s`WHERE`子句，它是多余的`Products`表，无法替换为`@ProductID`参数。 修改存储的过程的参数时，重要使用该存储的过程的 TableAdapter 方法也将更新。

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>步骤 4： 修改存储的过程的参数和更新 TableAdapter

由于`@Original_ProductID`多余参数的值，让 s 将其从删除`Products_Update`完全存储过程。 打开`Products_Update`存储过程中，删除`@Original_ProductID`参数，然后在`WHERE`子句`UPDATE`语句，参数名称中使用的更改`@Original_ProductID`到`@ProductID`。 进行这些更改后，T-SQL 存储过程内应如下所示：


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample8.sql)]

若要将这些更改保存到数据库中，单击工具栏中的保存图标或按 Ctrl + S。 此时，`Products_Update`存储的过程不需要`@Original_ProductID`输入的参数，但 TableAdapter 配置为传递此类参数。 你可以查看 TableAdapter 会将发送到的参数`Products_Update`存储过程在数据集设计器中选择 TableAdapter，转到属性窗口中，然后单击省略号以`UpdateCommand`s`Parameters`集合。 这将显示在图 14 所示的参数集合编辑器对话框。


![参数集合编辑器列表使用的参数传递给 Products_Update 存储过程](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png)

**图 14**： 参数集合编辑器列表使用的参数传递给`Products_Update`存储过程


你可以从此处删除此参数，只需选择`@Original_ProductID`参数从列表中的成员，单击删除按钮。

或者，你可以刷新设计器中的 TableAdapter 上右键单击并选择配置用于所有方法的参数。 此时会弹出 TableAdapter 配置向导列出的存储的过程用于选择、 插入、 更新和删除，以及参数的存储的过程希望接收。 如果你单击更新下拉列表可以看到`Products_Update`存储的过程预期输入的参数，现在不再包括`@Original_ProductID`（请参阅图 15）。 只需单击完成自动更新使用的 TableAdapter 的参数集合。


[![或者可以使用 TableAdapter 的配置向导来刷新其方法的参数集合](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**图 15**： 也可以使用 TableAdapter s 到刷新及其方法的参数集合的配置向导 ([单击以查看实际尺寸的图像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>步骤 5： 添加其他的 TableAdapter 方法

步骤 2 中所示，作为创建新的 TableAdapter 时很容易将互相自动生成的相应存储的过程。 这同样适用向 TableAdapter 添加其他方法。 为了说明这一点，让我们来添加`GetProductByProductID(productID)`方法`ProductsTableAdapter`在步骤 2 中创建。 此方法将作为输入`ProductID`值并返回有关指定的产品的详细信息。

启动的 TableAdapter 上右键单击并从上下文菜单中选择添加查询。


![向 TableAdapter 添加一个新查询](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**图 16**： 向 TableAdapter 添加一个新查询


这将启动 TableAdapter 查询配置向导中，首先将提示输入 TableAdapter 访问数据库的方式。 若要有创建一个新的存储的过程，选择创建新的存储的过程选项，然后单击下一步。


[![选择创建新的存储的过程选项](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)

**图 17**： 选择创建新的存储的过程选项 ([单击以查看实际尺寸的图像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png))


下一个屏幕要求我们能够标识要执行，是否它将返回一组行或单个标量值，或执行查询的类型`UPDATE`， `INSERT`，或`DELETE`语句。 由于`GetProductByProductID(productID)`方法将返回一行，保留选择它返回行选项选择并按下一步。


[![选择它返回行选项](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)

**图 18**： 选择其返回行选项 ([单击以查看实际尺寸的图像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png))


下一个屏幕中显示的 TableAdapter s 主查询，其中只列出了存储过程的名称 (`dbo.Products_Select`)。 存储的过程名称替换为以下`SELECT`语句，它返回所有指定的产品的产品字段：


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample9.sql)]


[![将存储的过程名称替换为 SELECT 查询](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)

**图 19**： 将存储过程名称替换`SELECT`查询 ([单击以查看实际尺寸的图像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png))


后续屏幕要求您将创建的存储的过程的名称。 输入的名称`Products_SelectByProductID`单击下一步。


[![新的存储的过程 Products_SelectByProductID 名称](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**图 20**： 命名新的存储过程`Products_SelectByProductID`([单击以查看实际尺寸的图像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image46.png))


在向导的最后一步，我们可以更改 DataTable 模式的名称生成，以及指示是否使用填充的方法，则返回的 DataTable 模式或两个。 此方法，将保留选中，这两个选项，但重命名的方法`FillByProductID`和`GetProductByProductID`。 单击下一步来查看该向导将执行，然后单击完成以完成向导的步骤的摘要。


[![将 TableAdapter 的方法重命名为 FillByProductID 和 GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image47.png)

**图 21**： 重命名的 TableAdapter 的方法`FillByProductID`和`GetProductByProductID`([单击以查看实际尺寸的图像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image49.png))


完成向导后，TableAdapter 有可用的一个新方法`GetProductByProductID(productID)`，调用时，将执行`Products_SelectByProductID`存储刚创建的过程。 花些时间查看钻取到存储过程文件夹，然后打开此新的存储的过程，从服务器资源管理器`Products_SelectByProductID`（如果未看到它，右键单击存储过程文件夹，然后选择刷新）。

请注意，`SelectByProductID`存储过程采用`@ProductID`作为输入参数并执行`SELECT`我们在向导中输入的语句。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>步骤 6： 创建业务逻辑层类

整个系列教程，我们具有努力维护分层体系结构，表示层在其中进行了所有的业务逻辑层 (BLL) 到其调用。 为了遵守这种设计决策，我们首先需要创建 BLL 类的新类型化数据集，我们可以从表示层访问产品数据之前。

创建名为的新类文件`ProductsBLLWithSprocs.cs`中`~/App_Code/BLL`文件夹并向其中添加以下代码：


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample11.cs)]

此类模仿`ProductsBLL`类语义方面与早期的教程，但使用`ProductsTableAdapter`和`ProductsDataTable`对象从`NorthwindWithSprocs`数据集。 例如，而不是使`using NorthwindTableAdapters`语句开头的类文件作为`ProductsBLL`，`ProductsBLLWithSprocs`类使用`using NorthwindWithSprocsTableAdapters`。 同样，`ProductsDataTable`和`ProductsRow`中此类使用的对象以作为前缀`NorthwindWithSprocs`命名空间。 `ProductsBLLWithSprocs`类提供了两个数据访问方法，`GetProducts`和`GetProductByProductID`，和方法来添加、 更新和删除的单个产品实例。

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>步骤 7： 使用`NorthwindWithSprocs`与表示层的数据集

此时，我们已创建使用存储的过程来访问和修改基础数据库数据 DAL。 我们具有还使用生成的基本 BLL 方法来检索所有产品或特定产品以及方法添加、 更新和删除产品。 要舍入关闭本教程，让 s 创建 ASP.NET 页使用 BLL 的`ProductsBLLWithSprocs`类，用于显示、 更新和删除记录。

打开`NewSprocs.aspx`页面`AdvancedDAL`文件夹，然后拖动一个 GridView 从工具箱中拖动到设计器中，其命名为`Products`。 从 GridView 选择要绑定到名为新 ObjectDataSource s 智能标记`ProductsDataSource`。 配置对象数据源以使用`ProductsBLLWithSprocs`类，如图 22 中所示。


[![配置对象数据源以使用 ProductsBLLWithSprocs 类](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image50.png)

**图 22**： 配置使用 ObjectDataSource`ProductsBLLWithSprocs`类 ([单击以查看实际尺寸的图像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image52.png))


下拉列表中选择的选项卡上有两个选项，`GetProducts`和`GetProductByProductID`。 由于我们想要在 GridView 中显示的所有产品，选择`GetProducts`方法。 更新、 插入和删除选项卡中每个下拉列表中仅有一个方法。 请确保每个下拉列表已选中其相应的方法，然后单击完成。

ObjectDataSource 向导运行完毕后，Visual Studio 将 BoundFields 和添加 CheckBoxField 到针对产品数据字段 GridView。 通过检查启用编辑和智能标记中存在的启用删除选项启用的 GridView s 内置编辑和删除的功能。


[![页包含一个 GridView 进行编辑和删除启用支持](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image53.png)

**图 23**: 页包含使用编辑和删除启用支持 GridView ([单击以查看实际尺寸的图像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image55.png))


正如我们已讨论在上一个教程中，在向导完成后 ObjectDataSource s，Visual Studio 集`OldValuesParameterFormatString`属性设置为原始\_{0}。 这需要还原顺序的数据修改功能工作 {0} 其默认值为正确给定我们 BLL 中的方法所需的参数。 因此，请务必设置`OldValuesParameterFormatString`{0} 属性或删除的属性完全声明性语法。

完成配置数据源向导中，打开编辑和删除 GridView 中的支持以及返回 ObjectDataSource s 后`OldValuesParameterFormatString`为其默认值的属性，你的页面 s 声明性标记应类似于以下：


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample12.aspx)]

此时我们无法整理 GridView 通过自定义编辑界面以包含验证，具有`CategoryID`和`SupplierID`列呈现为 DropDownLists，依次类推。 我们还可以向删除按钮，添加客户端确认和我建议你花时间来实现这些增强功能。 因为这些主题已涉及的前面的教程，但是，我们将不覆盖它们再次此处。

无论是否或不增强 GridView，测试页的核心功能的浏览器中。 如图 24 所示，页将列出一个 GridView，提供每个行编辑和删除功能中的产品。


[![产品可以查看、 编辑和删除从 GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image56.png)

**图 24**: 产品可以查看、 编辑，且已删除从 GridView ([单击以查看实际尺寸的图像](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image58.png))


## <a name="summary"></a>摘要

在类型化数据集 Tableadapter 可访问数据，使用临时 SQL 语句从数据库或通过存储过程。 当使用存储过程时，就可以使用任一现有的存储的过程或可指示 TableAdapter 向导来创建新存储过程基于`SELECT`查询。 在本教程中我们探讨了如何为我们自动创建的存储的过程。

而同时将自动生成有助于节省时间的存储的过程，有某些情况下，向导不 t 所创建的存储的过程与什么我们应已创建我们自己上对齐的位置。 一个示例是`Products_Update`存储过程，应同时`@Original_ProductID`和`@ProductID`即使输入参数`@Original_ProductID`参数已成为多余。

在许多情况下，可能已创建的存储的过程，或者我们可能需要生成这些手动以便具有更大程度上控制存储的过程的命令。 在任一情况下，我们想要指示 TableAdapter 的方法中使用现有存储的过程。 我们应看到如何实现此目的在下一步的教程。

尽情享受编程 ！

## <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [创建和维护存储的过程](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [从存储过程中检索标量数据](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [SQL Server 存储过程的基础知识](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [存储过程： 概述](http://www.sqlteam.com/item.asp?ItemID=563)
- [编写存储的过程](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已希尔顿 Geisenow。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[下一篇](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
