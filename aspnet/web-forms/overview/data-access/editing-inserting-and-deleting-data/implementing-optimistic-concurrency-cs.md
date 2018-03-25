---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
title: "实现开放式并发 (C#) |Microsoft 文档"
author: rick-anderson
description: "Web 应用程序允许多个用户编辑数据，对于没有在同一时间可能两个用户编辑相同的数据的风险。 在此 tutori..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 56e15b33-93b8-43ad-8e19-44c6647ea05c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
msc.type: authoredcontent
ms.openlocfilehash: a19e6c320838849e10d2aa397a23a0ee906bac22
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="implementing-optimistic-concurrency-c"></a>实现开放式并发 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_CS.exe)或[下载 PDF](implementing-optimistic-concurrency-cs/_static/datatutorial21cs1.pdf)

> Web 应用程序允许多个用户编辑数据，对于没有在同一时间可能两个用户编辑相同的数据的风险。 在本教程中我们将实现乐观并发控制，以处理此风险。


## <a name="introduction"></a>介绍

对于 web 应用程序仅允许用户查看数据，或者对于包含仅可以修改数据的单个用户的那些，意外覆盖彼此的所做更改的两个并发用户没有威胁。 对于 web 应用程序允许多个用户更新或删除数据，但是，没有一个用户修改，以与其他并发用户的冲突的可能性。 而无需部署任何并发策略，在两个用户同时编辑单个记录时将提交她更改的用户上次将重写第一次所做的更改。

例如，想象一下这样，两个用户，Jisun 和 Sam，同时也已访问我们允许访问者更新和删除通过 GridView 控件的产品的应用程序中的页。 同时单击时间大致相同 GridView 中的编辑按钮。 Jisun 产品名称更改为"牛奶 Tea"，并单击更新按钮。 最终结果是`UPDATE`语句发送到数据库，设置*所有*的产品的可更新字段 (即使 Jisun 只更新一个字段， `ProductName`)。 在此时间点，数据库将具有值"牛奶 Tea，"饮料，尖端液体等用于此特定产品供应商的类别。 但是，在 Sam 的屏幕上 GridView 仍将显示产品名称可编辑的 GridView 行中作为"牛奶"。 几秒后 Jisun 的更改已提交，Sam 到调味品更新类别并单击更新。 这会导致`UPDATE`语句发送到数据库设置为"牛奶，"产品名称`CategoryID`到相应的 Beverages 类别 ID，依次类推。 产品名称的 Jisun 的更改已被覆盖。 图 1 以图形方式描述了这一系列的事件。


[![当两个用户同时更新的记录存在一个用户 s 可能发生变化以覆盖其他 s](implementing-optimistic-concurrency-cs/_static/image2.png)](implementing-optimistic-concurrency-cs/_static/image1.png)

**图 1**： 当两个用户同时更新记录存在 s 可能一个用户 s 将更改为覆盖其他 s ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image3.png))


同样，当两个用户正在访问页面，一个用户可能是中途时就会删除其他用户更新记录。 或者，当用户加载页，在单击删除按钮时，另一个用户可能已修改该记录的内容。

有三个[并发控制](http://en.wikipedia.org/wiki/Concurrency_control)提供策略：

- **不执行任何操作**-如果并发用户正在修改同一记录，让 win （默认行为） 的最后一个提交
- [**开放式并发**](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -假定，尽管可能存在并发冲突不时地，大多数情况下不会产生此类冲突; 因此，如果确实会发生冲突，只需通知用户，其更改无法保存，因为另一个用户修改了相同的数据
- **保守式并发**-假定并发冲突是司空见惯并且用户不会允许正在告知其更改未保存由于另一个用户的并发活动; 因此，当一个用户启动更新记录时，锁定它从而阻止任何其他用户编辑或删除该记录，直到用户提交其修改

我们的教程的所有到目前为止已使用的默认并发解析策略-也就是说，我们已让 win 进行的上次写入。 在本教程中我们将研究如何实现乐观并发控制。

> [!NOTE]
> 我们不会查看本教程系列中的保守式并发示例。 保守式并发很少使用，如锁定，因为如果未正确释放，可以防止其他用户更新数据。 例如，如果用户锁定以进行编辑的记录，然后离开前对其解除锁定一天，没有其他用户将能够更新该记录，直到原始用户返回并完成其更新。 因此，在其中使用保守式并发的情况下，没有通常的超时，如果达到，取消锁定。 票证销售网站，锁定特定座位位置段短的时间，用户完成订单过程时，是一种保守式并发控制。


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>步骤 1： 查看如何乐观并发被实现

通过确保正在更新或删除的记录具有相同的值，像更新或删除进程启动时适用乐观并发控制。 例如时单击编辑按钮可编辑的 GridView 中，记录的值是从数据库读取，并且显示在文本框中和其他 Web 控件。 GridView 保存这些原始值。 更高版本，用户进行她更改，并单击更新按钮后，原始值加上的新值会发送到业务逻辑层，然后给数据访问层。 数据访问层必须发出 SQL 语句，如果用户已开始编辑的原始值都是仍在数据库中的值相同，只会更新该记录。 图 2 显示了这一序列的事件。


[![若要成功执行 Update 或 Delete，对于原始值必须等于当前的数据库值](implementing-optimistic-concurrency-cs/_static/image5.png)](implementing-optimistic-concurrency-cs/_static/image4.png)

**图 2**: For Update 或 Delete succeed，原始值必须是等于当前的数据库值 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image6.png))


各种方法来实现开放式并发 (请参阅[Peter A.Bromberg](http://peterbromberg.net/)的[Optmistic 并发更新逻辑](http://www.eggheadcafe.com/articles/20050719.asp)简要查看多个选项)。 ADO.NET 类型化数据集提供可配置为只需一个复选框的刻度线的一个实现。 为类型化数据集中 TableAdapter 增强了 TableAdapter 的启用乐观并发`UPDATE`和`DELETE`语句以包括所有中的原始值的比较`WHERE`子句。 以下`UPDATE`语句，例如，更新的名称和产品价格只有当前数据库值与最初检索到更新 GridView 中的记录时的值相等。 `@ProductName`和`@UnitPrice`参数包含由用户输入的新值，而`@original_ProductName`和`@original_UnitPrice`包含最初加载到了 GridView 时单击编辑按钮的值：


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample1.sql)]

> [!NOTE]
> 这`UPDATE`语句已简化以便于阅读。 在实践中，`UnitPrice`签入`WHERE`子句将更多地涉及自`UnitPrice`可以包含`NULL`s 和正在检查`NULL = NULL`始终返回 False (而是必须使用`IS NULL`)。


除了使用不同的基础`UPDATE`语句，配置要使用乐观并发还会修改其数据库的签名 TableAdapter 直接的方法。 回想一下我们第一个教程中，从[*创建数据访问层*](../introduction/creating-a-data-access-layer-cs.md)，DB 直接方法是接受的标量列表的那些值作为输入的参数 (而不是强类型 DataRow 或DataTable 实例）。 当使用乐观并发，直接 DB`Update()`和`Delete()`方法包括原始值的输入的参数。 此外，有关使用批处理 BLL 中的代码更新模式 (`Update()`接受 Datarow 和数据表，而不是标量值的方法重载) 也必须更改。

而是不是扩展了现有 DAL 的 Tableadapter，让我们使用开放式并发 （这将需要更改以适应 BLL），而是创建新类型化数据集名为`NorthwindOptimisticConcurrency`，我们将添加到`Products`TableAdapter，使用开放式并发。 接下来，我们将创建`ProductsOptimisticConcurrencyBLL`具有适当的修改，支持乐观并发 DAL 的业务逻辑层类。 已放置此奠定基础，我们就会准备好创建 ASP.NET 页。

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>步骤 2： 创建数据访问层，支持乐观并发

若要创建新的类型化数据集，右键单击`DAL`文件夹内的`App_Code`文件夹并添加名为的新数据集`NorthwindOptimisticConcurrency`。 正如我们看到在第一个教程，这样做将新的 TableAdapter 到类型化数据集，自动启动 TableAdapter 配置向导。 在第一个屏幕中，我们系统会提示指定要连接到-连接到相同的 Northwind 数据库使用的数据库`NORTHWNDConnectionString`设置从`Web.config`。


[![连接到相同的 Northwind 数据库](implementing-optimistic-concurrency-cs/_static/image8.png)](implementing-optimistic-concurrency-cs/_static/image7.png)

**图 3**： 连接到相同的 Northwind 数据库 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image9.png))


接下来，我们系统将提示如何查询数据： 通过临时 SQL 语句，一个新的存储过程，或一个现有存储过程。 由于我们在我们原始 DAL 中使用临时 SQL 查询，使用此选项此处以及。


[![指定要使用的临时 SQL 语句检索的数据](implementing-optimistic-concurrency-cs/_static/image11.png)](implementing-optimistic-concurrency-cs/_static/image10.png)

**图 4**： 指定要使用的临时 SQL 语句检索的数据 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image12.png))


在以下屏幕上，输入要用于检索产品信息的 SQL 查询。 我们将使用用于完全相同的 SQL 查询`Products`从我们原始 DAL，返回的所有的 TableAdapter`Product`以及该产品的供应商和类别名称的列：


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample2.sql)]


[![在原始 DAL 中使用相同的 SQL 查询从产品 TableAdapter](implementing-optimistic-concurrency-cs/_static/image14.png)](implementing-optimistic-concurrency-cs/_static/image13.png)

**图 5**： 使用相同的 SQL 查询从`Products`TableAdapter 中原始 DAL ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image15.png))


然后再移至下一个屏幕中，单击高级选项按钮。 若要让此 TableAdapter 为利用乐观并发控制，只需选中"使用开放式并发"复选框。


[![启用乐观并发控制的支票&quot;使用乐观并发&quot;复选框](implementing-optimistic-concurrency-cs/_static/image17.png)](implementing-optimistic-concurrency-cs/_static/image16.png)

**图 6**： 通过检查"使用开放式并发"复选框来启用乐观并发控制 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image18.png))


最后，指示 TableAdapter 应使用的数据访问模式，同时填充 DataTable 并返回 DataTable;此外指示应创建的数据库的直接方法。 更改方法的名称返回 DataTable 模式从 GetData getproducts，以便镜像我们在我们原始 DAL 中使用的命名约定。


[![具有利用所有的数据访问模式的 TableAdapter](implementing-optimistic-concurrency-cs/_static/image20.png)](implementing-optimistic-concurrency-cs/_static/image19.png)

**图 7**： 具有 TableAdapter 利用所有数据访问模式 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image21.png))


完成向导后，，数据集设计器将包括一个强类型`Products`DataTable 和 TableAdapter。 花一些时间来重命名从 DataTable`Products`到`ProductsOptimisticConcurrency`，则你可以通过右键单击 DataTable 的标题栏并从上下文菜单中选择重命名。


[![DataTable 和 TableAdapter 已添加到类型化数据集](implementing-optimistic-concurrency-cs/_static/image23.png)](implementing-optimistic-concurrency-cs/_static/image22.png)

**图 8**: DataTable 和 TableAdapter 已添加到类型化数据集 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image24.png))


若要查看的区别`UPDATE`和`DELETE`间查询`ProductsOptimisticConcurrency`TableAdapter （它使用开放式并发） 和产品 TableAdapter （这不），单击 TableAdapter，然后转到属性窗口。 在`DeleteCommand`和`UpdateCommand`属性的`CommandText`子属性，你可以看到 DAL 的更新或删除相关方法调用时发送到数据库的实际 SQL 语法。 有关`ProductsOptimisticConcurrency`TableAdapter`DELETE`使用语句是：


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample3.sql)]

而`DELETE`用于产品 TableAdapter 中我们原始 DAL 语句是要简单得多：


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample4.sql)]

如你所见，`WHERE`中的子句`DELETE`使用开放式并发的 TableAdapter 的语句包括每个之间的比较`Product`表的现有的列的值和原始值时上次填充 GridView （或说明或 FormView）。 由于以外的其他所有字段`ProductID`， `ProductName`，和`Discontinued`可以`NULL`其他参数和检查的值是包含在内，以正确地比较`NULL`中值`WHERE`子句。

随着，我们不会添加任何其他数据表到开放式并发启用数据集为本教程中，我们 ASP.NET 页面将仅向提供更新和删除产品信息。 但是，我们仍需要添加`GetProductByProductID(productID)`方法`ProductsOptimisticConcurrency`TableAdapter。

若要完成此操作，右键单击 TableAdapter 的标题栏 (上面的区域权限`Fill`和`GetProducts`方法名称)，然后从上下文菜单中选择添加查询。 这将启动 TableAdapter 查询配置向导。 因为我们 TableAdapter 的初始配置后，选择创建`GetProductByProductID(productID)`使用临时 SQL 语句的方法 （请参见图 4）。 由于`GetProductByProductID(productID)`方法返回有关特定产品的信息，指示此查询是`SELECT`查询返回行的类型。


[![查询将类型标记为&quot;返回行的选择&quot;](implementing-optimistic-concurrency-cs/_static/image26.png)](implementing-optimistic-concurrency-cs/_static/image25.png)

**图 9**: 查询将类型标记为"`SELECT`它返回行"([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image27.png))


在下一个屏幕上，我们系统提示输入要使用与预先加载的 TableAdapter 的默认查询的 SQL 查询。 增加现有查询，以包含子句`WHERE ProductID = @ProductID`，如图 10 中所示。


[![添加 WHERE 子句为预先加载查询，以返回特定产品记录](implementing-optimistic-concurrency-cs/_static/image29.png)](implementing-optimistic-concurrency-cs/_static/image28.png)

**图 10**： 添加`WHERE`Pre-Loaded 查询返回特定的产品记录的子句 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image30.png))


最后，更改到生成的方法名`FillByProductID`和`GetProductByProductID`。


[![FillByProductID 和 GetProductByProductID 重命名方法](implementing-optimistic-concurrency-cs/_static/image32.png)](implementing-optimistic-concurrency-cs/_static/image31.png)

**图 11**： 重命名的方法`FillByProductID`和`GetProductByProductID`([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image33.png))


完成此向导中，与 TableAdapter 现在包含用于检索数据的两个方法： `GetProducts()`，它将返回*所有*产品; 和`GetProductByProductID(productID)`，这将返回指定的产品。

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>步骤 3： 为乐观并发启用 DAL 创建业务逻辑层

我们现有`ProductsBLL`类具有使用的批量更新和 DB 直接模式的示例。 `AddProduct`方法和`UpdateProduct`重载这两个使用批处理更新模式中，传入`ProductRow`到 TableAdapter 的更新方法的实例。 `DeleteProduct`方法，另一方面，使用 DB 直接模式，调用 TableAdapter 的`Delete(productID)`方法。

与新`ProductsOptimisticConcurrency`TableAdapter，数据库直接方法现在要求还中传递的原始值。 例如，`Delete`方法现在需要 10 个输入参数： 原始`ProductID`， `ProductName`， `SupplierID`， `CategoryID`， `QuantityPerUnit`， `UnitPrice`， `UnitsInStock`， `UnitsOnOrder`， `ReorderLevel`，和`Discontinued`。 它将使用这些附加输入的参数的值中`WHERE`子句`DELETE`语句发送到数据库，仅删除指定的记录，如果数据库的当前值映射到原始的。

尽管为 TableAdapter 的方法签名`Update`批处理更新模式中使用的方法未发生更改，记录的原始列值和新值所需的代码。 因此，而不是尝试与我们现有使用乐观并发启用 DAL`ProductsBLL`类中，让我们创建一个新的业务逻辑层类，用于使用我们新 DAL。

添加一个名为类`ProductsOptimisticConcurrencyBLL`到`BLL`文件夹内的`App_Code`文件夹。


![将 ProductsOptimisticConcurrencyBLL 类添加到 BLL 文件夹](implementing-optimistic-concurrency-cs/_static/image34.png)

**图 12**： 添加`ProductsOptimisticConcurrencyBLL`BLL 文件夹类


接下来，添加以下代码`ProductsOptimisticConcurrencyBLL`类：


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample5.cs)]

请注意使用`NorthwindOptimisticConcurrencyTableAdapters`开始上面的类声明的语句。 `NorthwindOptimisticConcurrencyTableAdapters`命名空间包含`ProductsOptimisticConcurrencyTableAdapter`类，该类提供 DAL 的方法。 此外在类声明之前你将找到`System.ComponentModel.DataObject`属性，指示 Visual Studio 以在对象数据源向导的下拉列表中包括此类。

`ProductsOptimisticConcurrencyBLL`的`Adapter`属性提供快速访问权限的实例`ProductsOptimisticConcurrencyTableAdapter`类，并遵循在我们原始 BLL 类中使用的模式 (`ProductsBLL`， `CategoriesBLL`，依次类推)。 最后，`GetProducts()`方法只需调用到 DAL 的`GetProducts()`方法并返回`ProductsOptimisticConcurrencyDataTable`对象填充`ProductsOptimisticConcurrencyRow`数据库中每个产品记录的实例。

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>删除产品 DB 直接模式使用开放式并发

在使用针对使用开放式并发 DAL DB 直接模式时，则必须将方法传递了新值和原始值。 用于删除，没有新值，因此需要在传递仅的原始值。 在我们 BLL，然后，我们必须接受所有原始参数作为输入参数。 让我们来看`DeleteProduct`中的方法`ProductsOptimisticConcurrencyBLL`类使用 DB 直接方法。 这意味着此方法需要采用作为输入参数，所有 10 个产品数据字段中并将这些值交给 DAL，如下面的代码中所示：


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample6.cs)]

如果当用户单击删除按钮，原始值-上次已加载到的 GridView （或说明或 FormView） 这些值的不同数据库中的值从`WHERE`子句不会与相匹配的任何数据库记录和任何记录将受影响。 因此，TableAdapter 的`Delete`方法将返回`0`和 BLL`DeleteProduct`方法将返回`false`。

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>更新产品的批处理更新模式使用开放式并发

如前文所述，TableAdapter 的`Update`批处理更新模式的方法具有相同的方法签名，而不考虑是否使用开放式并发。 也就是说，`Update`方法需要 DataRow，Datarow、 数据表或类型化数据集的数组。 没有其他输入的参数来指定原始值。 这可能是因为 DataTable 将跟踪有关其 DataRow(s) 的原始值和修改值。 当 DAL 发出其`UPDATE`语句，`@original_ColumnName`参数填入 DataRow 的原始值，而`@ColumnName`参数填入 DataRow 的修改后的值。

在`ProductsBLL`（它使用我们的原始、 非乐观并发 DAL） 的类，在使用批处理更新模式时以更新我们的代码执行下列事件顺序的产品信息：

1. 将当前的数据库产品信息读入`ProductRow`实例使用 TableAdapter 的`GetProductByProductID(productID)`方法
2. 分配到的新值`ProductRow`步骤 1 中的实例
3. 调用 TableAdapter 的`Update`方法，同时传入`ProductRow`实例

这一序列的步骤，但是，不会正确支持乐观并发，因为`ProductRow`填充在步骤 1 填充直接从数据库中，这意味着使用 DataRow 的原始值指那些中当前存在数据库，并不是那些已绑定到 GridView 编辑过程的开头。 相反，当使用开放式并发的启用 DAL，我们需要 alter`UpdateProduct`方法重载，可使用以下步骤：

1. 将当前的数据库产品信息读入`ProductsOptimisticConcurrencyRow`实例使用 TableAdapter 的`GetProductByProductID(productID)`方法
2. 分配*原始*值复制到`ProductsOptimisticConcurrencyRow`步骤 1 中的实例
3. 调用`ProductsOptimisticConcurrencyRow`实例的`AcceptChanges()`方法，指示其当前值为"原始"的 DataRow
4. 分配*新*值复制到`ProductsOptimisticConcurrencyRow`实例
5. 调用 TableAdapter 的`Update`方法，同时传入`ProductsOptimisticConcurrencyRow`实例

步骤 1 中的所有当前的数据库值的读取复制为指定的产品记录。 此步骤是在多余`UpdateProduct`更新的重载*所有*的产品列 （为这些值将覆盖在步骤 2 中），但对于其中的列的值的一个子集作为传递这些重载至关重要输入的参数。 一旦原始值已分配给`ProductsOptimisticConcurrencyRow`实例，`AcceptChanges()`调用方法，其中将当前的 DataRow 值标记为要在中使用的原始值`@original_ColumnName`中的参数`UPDATE`语句。 接下来，将新的参数值分配给`ProductsOptimisticConcurrencyRow`和最后，`Update`调用方法时，在数据行中传递。

下面的代码演示`UpdateProduct`重载接受产品的所有数据的字段作为输入的参数。 而不显示在这里，`ProductsOptimisticConcurrencyBLL`本教程还包含下载中包含的类`UpdateProduct`接受只是该产品的名称和价格作为输入参数的重载。


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample7.cs)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>步骤 4： 将原始值和新值从该 ASP.NET 页传递给 BLL 方法

DAL 和 BLL 完成，那么保持是创建 ASP.NET 页，可以利用内置于系统的开放式并发逻辑。 具体而言，Web 控件 （GridView，说明如何或 FormView） 的数据必须记住其原始值并且 ObjectDataSource 必须将这两组值传递到业务逻辑层。 此外，必须配置 ASP.NET 页用于正常处理并发冲突。

首先打开`OptimisticConcurrency.aspx`页面`EditInsertDelete`文件夹并将一个 GridView 添加到设计器中，设置其`ID`属性`ProductsGrid`。 从 GridView 的智能标记，选择创建名为新 ObjectDataSource `ProductsOptimisticConcurrencyDataSource`。 由于我们希望此对象数据源，用于支持乐观并发 DAL，将其配置为使用`ProductsOptimisticConcurrencyBLL`对象。


[![具有 ObjectDataSource 使用 ProductsOptimisticConcurrencyBLL 对象](implementing-optimistic-concurrency-cs/_static/image36.png)](implementing-optimistic-concurrency-cs/_static/image35.png)

**图 13**: ObjectDataSource 使用`ProductsOptimisticConcurrencyBLL`对象 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image37.png))


选择`GetProducts`， `UpdateProduct`，和`DeleteProduct`向导中的下拉列表中的方法。 对于 UpdateProduct 方法，请使用接受所有产品的数据字段的重载。

## <a name="configuring-the-objectdatasource-controls-properties"></a>配置 ObjectDataSource 控件的属性

完成向导后，ObjectDataSource 的声明性标记应如下所示：


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample8.aspx)]

如你所见，`DeleteParameters`集合包含`Parameter`十个输入参数中的每个实例`ProductsOptimisticConcurrencyBLL`类的`DeleteProduct`方法。 同样，`UpdateParameters`集合包含`Parameter`实例中的输入参数的每个`UpdateProduct`。

对于涉及数据修改这些前面的教程，我们将删除 ObjectDataSource`OldValuesParameterFormatString`此时，由于此属性指示 BLL 方法需要在中传递的旧 （或原始） 的值，以及新值的属性。 此外，此属性的值指示用于原始值的输入的参数名称。 由于我们正在到 BLL 传入的原始值，请执行*不*删除此属性。

> [!NOTE]
> 值`OldValuesParameterFormatString`属性必须映射到 BLL 中的预期的原始值的输入的参数名称。 因为我们已命名为这些参数`original_productName`， `original_supplierID`，依次类推中，你可以将`OldValuesParameterFormatString`属性值作为`original_{0}`。 如果，但是，这些 BLL 方法的输入的参数具有类似名称`old_productName`， `old_supplierID`，依次类推中，你将需要更新`OldValuesParameterFormatString`属性`old_{0}`。


还有一个不需要顺序 ObjectDataSource 正确将原始值传递给 BLL 方法进行的最后一个属性设置。 ObjectDataSource 具有[ConflictDetection 属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx)，可分配给[两个值之一](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges`默认值;不向这些 BLL 方法的原始输入参数中发送的原始值
- `CompareAllValues`-未将原始值发送到 BLL 方法; 这些方法如果使用乐观并发，请选择此选项

花些时间设置`ConflictDetection`属性`CompareAllValues`。

## <a name="configuring-the-gridviews-properties-and-fields"></a>配置 GridView 的属性和字段

具有正确配置对象数据源的属性，我们将注意力转向设置 GridView。 首先，由于我们希望 GridView 以支持编辑和删除，则单击从 GridView 的智能标记的启用编辑和启用删除复选框。 这将添加 CommandField 其`ShowEditButton`和`ShowDeleteButton`都设置为`true`。

当绑定到`ProductsOptimisticConcurrencyDataSource`ObjectDataSource，GridView 包含字段，为每个产品的数据字段。 虽然可以编辑此类 GridView，用户体验是算作接受的。 `CategoryID`和`SupplierID`BoundFields 将呈现为文本框中，需要用户输入的相应的类别和供应商作为 ID 号。 将数值字段和任何验证控件，以确保已提供产品的名称并确保单价，库存量、 顺序和重新排序级别值的单位是这两个正确的数字值并大于或等于尚无格式设置为零。

如我们所述*将验证控件添加到的编辑和插入接口*和*自定义的数据修改界面*教程中，用户界面可进行自定义将替换 TemplateFields BoundFields。 我已通过以下方式修改此 GridView 和其编辑界面：

- 删除`ProductID`， `SupplierName`，和`CategoryName`BoundFields
- 转换`ProductName`到 TemplateField BoundField 和添加 RequiredFieldValidation 控件。
- 转换`CategoryID`和`SupplierID`BoundFields 到 TemplateFields，并调整编辑界面使用 DropDownLists 而不是文本框。 在这些 TemplateFields 的`ItemTemplates`、`CategoryName`和`SupplierName`数据字段的显示。
- 转换`UnitPrice`， `UnitsInStock`， `UnitsOnOrder`，和`ReorderLevel`BoundFields 到 TemplateFields 和添加的 CompareValidator 控件。

由于我们已经学习了如何完成这些任务在前面的教程中，我只需将还会列出最终声明性语法此处并将作为做法的实现。


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample9.aspx)]

我们非常接近具有完全工作示例。 但是，有一些细微部分，将会发生一些设置，并且导致我们问题。 此外，我们仍需要某些提醒用户并发冲突发生时的接口。

> [!NOTE]
> 为了使数据 Web 控件以正确将原始值传递给 ObjectDataSource （这将传递到 BLL），这至关重要的 GridView`EnableViewState`属性设置为`true`（默认值）。 如果禁用了视图状态，会在回发时丢失的原始值。


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>将正确的原始值传递到 ObjectDataSource

有几个问题 GridView 已配置的方式。 如果对象数据源的`ConflictDetection`属性设置为`CompareAllValues`（按原样我们的工具），当 ObjectDataSource`Update()`或`Delete()`通过 GridView （或说明或 FormView） 调用方法，ObjectDataSource 尝试复制到其相应的 GridView 的原始值`Parameter`实例。 回头参考图 2 此过程的图形表示形式。

具体而言，GridView 的原始值分配每次将数据绑定到 GridView 的双向数据绑定语句中的值。 因此，这至关重要，所有所需的原始值将捕获通过双向数据绑定和中转换格式提供了它们。

若要查看为何这很重要，需要一段时间来访问我们的页面的浏览器中。 按预期方式运行 GridView 列出每个产品具有最左边的列中的编辑和删除按钮。


[![在一个 GridView，列出的产品](implementing-optimistic-concurrency-cs/_static/image39.png)](implementing-optimistic-concurrency-cs/_static/image38.png)

**图 14**: GridView 中列出的产品 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image40.png))


如果单击删除按钮对任何产品`FormatException`引发。


[![尝试删除 FormatException 中的任何产品结果](implementing-optimistic-concurrency-cs/_static/image42.png)](implementing-optimistic-concurrency-cs/_static/image41.png)

**图 15**： 尝试中删除任何产品结果`FormatException`([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image43.png))


`FormatException` ObjectDataSource 尝试在原始读取时，将引发`UnitPrice`值。 由于`ItemTemplate`具有`UnitPrice`格式设为货币 (`<%# Bind("UnitPrice", "{0:C}") %>`)，它包括货币符号，如 19.95 美元。 `FormatException`出现 ObjectDataSource 尝试将此字符串转换`decimal`。 若要避免此问题，我们具有多个选项：

- 删除货币格式从`ItemTemplate`。 也就是说，而不是使用`<%# Bind("UnitPrice", "{0:C}") %>`，只需使用`<%# Bind("UnitPrice") %>`。 此的缺点是，已不再格式化价格。
- 显示`UnitPrice`格式设为在货币`ItemTemplate`，但使用`Eval`关键字来实现此目的。 回想一下，`Eval`执行单向数据绑定。 我们仍需要提供`UnitPrice`值的原始值，因此我们仍需要中的双向数据绑定语句`ItemTemplate`，但这可以放置标签 Web 控件中其`Visible`属性设置为`false`。 我们无法在 ItemTemplate 中使用以下标记：


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample10.aspx)]

- 删除货币格式从`ItemTemplate`，使用`<%# Bind("UnitPrice") %>`。 在 GridView 的`RowDataBound`其中事件处理程序中，以编程方式访问控制标签 Web`UnitPrice`值已显示并设置其`Text`到格式化的版本属性。
- 保留`UnitPrice`格式设为货币。 在 GridView 的`RowDeleting`事件处理程序替换现有的原始`UnitPrice`值 ($19.95) 与实际的十进制值使用`Decimal.Parse`。 我们已了解如何完成类似中`RowUpdating`中的事件处理程序[*处理 BLL-和 ASP.NET 页中的 DAL 级别异常*](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)教程。

为我的示例中所选的要与第二种方法，添加隐藏的标签 Web 控件`Text`属性是双向数据绑定到未格式化`UnitPrice`值。

后解决此问题，请尝试再次单击删除按钮对任何产品。 此时你将获得`InvalidOperationException`ObjectDataSource 尝试调用 BLL`UpdateProduct`方法。


[![ObjectDataSource 找不到具有它想要发送的输入参数的方法](implementing-optimistic-concurrency-cs/_static/image45.png)](implementing-optimistic-concurrency-cs/_static/image44.png)

**图 16**: ObjectDataSource 找不到具有它想要发送的输入参数的方法 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image46.png))


查看异常的消息，它是清除 ObjectDataSource 想要调用 BLL`DeleteProduct`包含方法中，`original_CategoryName`和`original_SupplierName`输入参数。 这是因为`ItemTemplate`s 针对`CategoryID`和`SupplierID`TemplateFields 当前包含使用双向绑定语句`CategoryName`和`SupplierName`数据字段。 相反，我们需要包括`Bind`具有语句`CategoryID`和`SupplierID`数据字段。 若要完成此操作，将使用的现有绑定语句`Eval`语句，然后添加隐藏的标签控制其`Text`属性绑定到`CategoryID`和`SupplierID`使用双向数据绑定，如所示的数据字段下面：


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample11.aspx)]

进行这些更改后，现在我们已能够成功删除和编辑产品信息 ！ 在步骤 5 中，我们将查看如何验证检测到并发冲突。 但现在，需要几分钟才能稍后尝试更新和删除几个记录，以确保该更新和删除适用于单个用户按预期方式工作。

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>步骤 5： 测试的乐观并发支持

为了验证并发冲突正在检测到 （而不是导致盲目地覆盖的数据），我们需要打开此页的两个浏览器窗口。 在浏览器这两种情况下，单击编辑按钮牛奶。 然后，在只需浏览器之一，将名称更改为"牛奶 Tea"，然后单击更新。 更新应成功并返回到其预编辑状态，使用"牛奶 Tea"作为新的产品名称的 GridView。

在其他浏览器窗口实例中，但是，产品名称文本框中仍将显示"牛奶"。 在此第二个浏览器窗口中，更新`UnitPrice`到`25.00`。 而不乐观并发支持，单击第二个浏览器实例中的更新会更改产品名称返回到"牛奶"，从而覆盖第一个浏览器实例所做的更改。 采用乐观并发，但是，单击第二个浏览器实例中的更新按钮都会导致[DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx)。


[![检测到并发冲突时，引发 DBConcurrencyException](implementing-optimistic-concurrency-cs/_static/image48.png)](implementing-optimistic-concurrency-cs/_static/image47.png)

**图 17**： 检测到时的并发冲突，则`DBConcurrencyException`引发 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image49.png))


`DBConcurrencyException`利用 DAL 的批处理更新模式时才会引发。 DB 直接模式不会引发异常，它只是表示不影响了任何行。 若要说明这一点，返回到其预编辑状态的两个浏览器实例的 GridView。 接下来，在第一个浏览器实例中，单击编辑按钮并更改回"牛奶"从"牛奶 Tea"产品名称，然后单击更新。 在第二个浏览器窗口中，牛奶中单击删除按钮。

单击删除，在页回发，GridView 调用 ObjectDataSource`Delete()`方法，并 ObjectDataSource 调用到`ProductsOptimisticConcurrencyBLL`类的`DeleteProduct`方法，传递的原始值。 原始`ProductName`值第二个浏览器实例为"牛奶 Tea"，这与当前不匹配`ProductName`数据库中的值。 因此`DELETE`向数据库发出语句会影响零行，因为在数据库中没有记录，`WHERE`子句满足。 `DeleteProduct`方法返回`false`和对象数据源的数据重新绑定到 GridView。

从最终用户的角度来看，为牛奶 Tea 第二个浏览器窗口中单击删除按钮上导致屏幕 flash 且后回来，, 该产品是仍然存在，但现在它列出为"牛奶"（产品名称所做的更改第一个浏览器实例）。 如果用户再次单击删除按钮，将成功删除，如 GridView 的原始`ProductName`值 （"牛奶"） 现在与数据库中的值匹配。

在这两种情况下，用户体验并不理想之选。 我们清楚地不需要向用户显示的基本细节`DBConcurrencyException`异常时使用的批处理更新模式。 和使用数据库直接模式时的行为是有些令人费解的用户的命令失败，但没有精确的原因的迹象。

若要更正这两个问题，我们可以创建提供到说明更新或删除操作失败的原因的页面上的标签 Web 控件。 对于批处理更新模式中，我们可以确定是否`DBConcurrencyException`异常发生在 GridView 的后级别事件处理程序中，根据需要显示的警告标签。 有关数据库直接方法，我们可以检查 BLL 方法的返回值 (即`true`一行是否受到影响，如果`false`否则为) 并根据需要显示一条信息性消息。

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>步骤 6： 添加信息性消息，并显示它们在遇到时并发冲突

当发生并发冲突时，表现出的行为取决于是否使用 DAL 的批处理更新或 DB 直接模式的方式。 我们的教程中使用这两种模式，与所用的更新和用于删除的数据库直接模式的批处理更新模式。 若要开始，让我们向我们说明试图删除或更新数据时发生了并发冲突的页面中添加两个标签 Web 控件。 设置标签控件的`Visible`和`EnableViewState`属性设置为`false`; 这将导致它们只为那些特定页访问的位置为在每个页，请访问上隐藏其`Visible`属性以编程方式设置为`true`。


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample12.aspx)]

除了设置其`Visible`， `EnabledViewState`，和`Text`属性，我已将设置`CssClass`属性`Warning`，这将导致产生标签的要显示在大型、 红色、 斜体、 加粗字体。 此 CSS`Warning`已定义类并且将其添加到 Styles.css 进来*检查与插入、 更新和删除的事件相关联*教程。

在添加后这些标签，Visual Studio 中的设计器应类似于图 18。


[![两个标签控件已添加到页面](implementing-optimistic-concurrency-cs/_static/image51.png)](implementing-optimistic-concurrency-cs/_static/image50.png)

**图 18**： 两个标签控件已添加到页面 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image52.png))


与就地这些标签 Web 控件，我们已准备好检查如何确定当发生并发冲突，在该点的相应标签`Visible`属性可以设置为`true`，显示的信息性消息。

## <a name="handling-concurrency-violations-when-updating"></a>在更新时处理并发冲突

让我们首先看一下如何使用批处理更新模式时处理并发冲突。 由于此类冲突与批处理更新模式原因`DBConcurrencyException`异常抛出，我们需要将代码添加到我们 ASP.NET 页后，可以确定是否`DBConcurrencyException`在更新过程中出现异常。 如果这样，我们应显示其更改未保存，因为另一个用户已修改相同的数据之间，启动编辑记录时向用户说明发送消息和它们单击更新按钮时。

正如我们看到在*处理 BLL-和 ASP.NET 页中的 DAL 级别异常*教程，则可以检测到，在该数据 Web 控件后级别事件处理程序中禁止显示此类异常。 因此，我们需要创建 GridView 的事件处理程序`RowUpdated`事件检查是否`DBConcurrencyException`引发异常。 此事件处理程序将传递对更新过程中发生任何异常的引用，如事件处理程序下面的代码中所示：


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample13.cs)]

中`DBConcurrencyException`异常，此事件处理程序显示`UpdateConflictMessage`标签控件，并指示已处理该异常。 使用此代码中的位置，当更新记录，就会发生并发冲突时用户的更改都将丢失，因为它们可能已覆盖另一个用户修改在同一时间。 具体而言，GridView 是返回到其预编辑状态，并绑定到当前的数据库数据。 这将更新 GridView 行有其他用户的更改，这是以前不可见。 此外，`UpdateConflictMessage`标签控件将向用户解释发生。 图 19 中详细介绍了这一序列的事件。


[![用户 s 在并发冲突的表面会丢失更新](implementing-optimistic-concurrency-cs/_static/image54.png)](implementing-optimistic-concurrency-cs/_static/image53.png)

**图 19**： 用户 s 在并发冲突的表面会丢失更新 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image55.png))


> [!NOTE]
> 或者，而不是返回到预先编辑状态 GridView，我们可能会使 GridView 在其编辑状态通过设置`KeepInEditMode`传入的属性`GridViewUpdatedEventArgs`为 true 的对象。 如果采用此方法，但是，请务必重新绑定到 GridView 数据 (通过调用其`DataBind()`方法)，以便其他用户的值加载到编辑界面。 可用于本教程使用下载的代码具有这两行中的代码`RowUpdated`注释掉的事件处理程序; 只需取消注释这些代码具有 GridView 行在并发冲突后继续处于编辑模式。


## <a name="responding-to-concurrency-violations-when-deleting"></a>删除时响应并发冲突

使用数据库直接模式中，没有在遇到并发冲突时引发异常。 相反，database 语句只会影响任何记录，作为 WHERE 子句不与任何记录不匹配。 这样，它们返回一个布尔值，该值指示它们受到影响精确一条记录已设计所有 BLL 中创建的数据修改方法。 因此，若要确定在删除一条记录时，是否发生了并发冲突，我们可以检查返回值的 BLL`DeleteProduct`方法。

可以通过对象数据源的后级别事件处理程序中检查 BLL 方法的返回值`ReturnValue`属性`ObjectDataSourceStatusEventArgs`对象传递到事件处理程序。 由于我们感兴趣的返回值确定`DeleteProduct`我们需要创建对象数据源的事件处理程序方法，`Deleted`事件。 `ReturnValue`属性属于类型`object`和可以是`null`如果引发了异常，并且该方法已中断，它无法返回值。 因此，我们应首先确保`ReturnValue`属性不是`null`和是一个布尔值。 假定此检查通过，我们显示`DeleteConflictMessage`标签控件，如果`ReturnValue`是`false`。 可以使用下面的代码完成此操作：


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample14.cs)]

在遇到时并发冲突，将取消用户的删除请求。 刷新 GridView，显示所发生的时间之间该记录用户的更改加载页，但他单击删除按钮时。 当这种冲突调查时，`DeleteConflictMessage`显示标签，则说明发生 （请参阅图 20）。


[![S 删除当用户取消在遇到时并发冲突](implementing-optimistic-concurrency-cs/_static/image57.png)](implementing-optimistic-concurrency-cs/_static/image56.png)

**图 20**： 删除取消在遇到时并发冲突的用户 s ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-cs/_static/image58.png))


## <a name="summary"></a>摘要

允许多个，每个应用程序中存在并发冲突的机会并发用户更新或删除数据。 如果此类冲突的两个用户同时更新相同的数据在最后一个写入"wins"中获取的任何人员都，覆盖其他用户的更改的更改不计算在内。 或者，开发人员可以实现任一乐观和悲观并发控制。 乐观并发控制假定并发冲突不太频繁，以及只是不允许更新或删除命令将构成并发冲突。 保守式并发控制假定是不可接受的并发冲突频发，并且只需拒绝一个用户的更新或删除命令。 使用保守式并发控制更新记录涉及到锁定，从而阻止任何其他用户修改或删除记录锁定时的数据。

.NET 中的类型数据集提供了有关支持乐观并发控制的功能。 具体而言，`UPDATE`和`DELETE`语句向数据库发出包括所有表的列，从而确保 update 或 delete 会仅发生如果用户与原始数据匹配记录的当前数据具有执行其 update 或 delete。 DAL 配置支持乐观并发后, BLL 方法需要更新。 此外，以便 ObjectDataSource 从其数据的 Web 控件中检索原始值，并将它们向下传递到 BLL，则必须配置调入 BLL ASP.NET 页。

正如我们看到在本教程中，在 ASP.NET web 应用程序中实现乐观并发控制涉及更新 DAL 和 BLL 和 ASP.NET 页中添加支持。 此添加的工作是明智的做法投入时间和工作量的取决于你的应用程序。 如果你不常有并发用户更新数据，或者它们将更新的数据是从另一个不同，然后并发控制不是一个关键问题。 如果，但是，你定期有多个用户在你使用相同的数据的站点上，并发控制可帮助防止意外覆盖其他人一个用户的更新或删除。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一页](customizing-the-data-modification-interface-cs.md)
[下一页](adding-client-side-confirmation-when-deleting-cs.md)
