---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
title: "创建业务逻辑层 (C#) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将了解如何集中到业务逻辑层 (BLL) 充当一个中介以便 t 之间交换数据的业务规则..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 85554606-47cb-4e4f-9848-eed9da579056
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 7518ddd11a05a9e3d5df85e3cf6ceffa09a25060
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-business-logic-layer-c"></a>创建业务逻辑层 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_2_CS.exe)或[下载 PDF](creating-a-business-logic-layer-cs/_static/datatutorial02cs1.pdf)

> 在本教程中我们将了解如何集中用作表示层和 DAL 之间的数据交换的中介业务逻辑层 (BLL) 到您的业务规则。


## <a name="introduction"></a>介绍

在中创建数据访问层 (DAL)[第一个教程](creating-a-data-access-layer-cs.md)完全离职数据访问逻辑与表示逻辑。 但是，尽管 DAL 能完全分隔与表示层的数据访问的详细信息，它不会强制任何业务规则可能适用。 例如，对于我们的应用程序我们可能希望禁止`CategoryID`或`SupplierID`字段`Products`表要修改时`Discontinued`字段设置为 1，或者我们可能想要强制实施高低规则禁止的情况员工的人受雇后它们进行管理。 另一个常见的方案是对特定角色中的授权可能只有用户可以删除产品，或者可以更改`UnitPrice`值。

在本教程中我们将了解如何集中到业务逻辑层 (BLL) 充当一个中介以便在表示层和 DAL 之间交换数据的这些业务规则。 在实际应用中，应作为一个单独的类库项目; 实现 BLL但是，这些教程我们将实现 BLL 为一系列中的类我们`App_Code`来简化的项目结构的文件夹。 图 1 说明之间的表示层、 BLL 和 DAL 的体系结构关系。


![BLL 将与数据访问层分隔的表示层和实施业务规则](creating-a-business-logic-layer-cs/_static/image1.png)

**图 1**: BLL 将与数据访问层分隔的表示层和实施业务规则


## <a name="step-1-creating-the-bll-classes"></a>步骤 1： 创建 BLL 类

我们 BLL 将组成四个类，一个用于在 DAL; 每个 TableAdapter其中每个 BLL 类将通过方法来检索、 插入、 更新和删除从 DAL，应用适当的业务规则在各自的 TableAdapter。

到更完全分隔与 DAL 和 BLL 相关的类，让我们创建中的两个子文件夹`App_Code`文件夹，`DAL`和`BLL`。 只需右键单击`App_Code`在解决方案资源管理器的文件夹，然后选择新文件夹。 创建以下两个文件夹之后, 移动到第一个教程中创建的类型化的数据集`DAL`子文件夹。

接下来，创建中的四个 BLL 类文件`BLL`子文件夹。 若要完成此操作，右键单击`BLL`子文件夹，选择添加新项，然后选择类模板。 命名的四个类`ProductsBLL`， `CategoriesBLL`， `SuppliersBLL`，和`EmployeesBLL`。


![将四个新类添加到 App_Code 文件夹](creating-a-business-logic-layer-cs/_static/image2.png)

**图 2**： 添加到的四个新类`App_Code`文件夹


接下来，让我们将方法添加到每个类以只包装在第一个教程 Tableadapter 为定义的方法。 现在，这些方法将只是直接调入 DAL;我们会返回更高版本，以添加任何必要的业务逻辑。

> [!NOTE]
> 如果你使用的 Visual Studio Standard Edition 或更高版本 (即你*不*使用 Visual Web Developer)，你可以根据需要设计您以可视方式使用的类[类设计器](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp)。 请参阅[类设计器博客](https://blogs.msdn.com/classdesigner/default.aspx)有关 Visual Studio 中的此新功能的详细信息。


有关`ProductsBLL`我们需要添加总计的七种方法的类：

- `GetProducts()`返回所有产品
- `GetProductByProductID(productID)`返回具有指定的产品 ID 的产品
- `GetProductsByCategoryID(categoryID)`返回从指定的类别的所有产品
- `GetProductsBySupplier(supplierID)`返回指定供应商提供的所有产品
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)`将新产品插入到数据库中使用的值传入的程序;返回`ProductID`新插入的记录的值
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)`更新现有产品在数据库中使用的传入的值;返回`true`精确一行已更新，如果`false`否则为
- `DeleteProduct(productID)`从数据库中删除指定的产品

ProductsBLL.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample1.cs)]

只需返回数据的方法的`GetProducts`， `GetProductByProductID`， `GetProductsByCategoryID`，和`GetProductBySuppliersID`它们只需调用到 DAL 是非常简单。 尽管在某些情况下可能需要实现的业务规则在此级别 （例如基于当前登录的用户或用户所属的角色的授权规则），我们将只是将这些方法作为-是。 有关这些方法，然后，BLL 仅仅是作为服务通过该表示层访问基础数据从数据访问层的代理。

`AddProduct`和`UpdateProduct`方法同时作为参数采用各种产品字段的值和添加新的产品或更新现有，分别。 由于许多`Product`表的列可以接受`NULL`值 (`CategoryID`， `SupplierID`，和`UnitPrice`，仅举几例)，这些输入参数`AddProduct`和`UpdateProduct`映射到此类列使用[可以为 null 类型](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx)。 可以为 null 的类型不熟悉.NET 2.0 和提供一种技术，该值指示是否为值类型应，相反，应由`null`。 在 C# 中可以值类型为 null 的类型进行标记添加`?`后类型 (如`int? x;`)。 请参阅[可以为 Null 类型](https://msdn.microsoft.com/library/1t3y8s4s.aspx)主题中[C# 编程指南](https://msdn.microsoft.com/library/67ef8sbd%28VS.80%29.aspx)有关详细信息。

所有三个方法返回布尔值，该值指示行是否已插入、 更新或删除，因为该操作不可能会导致受影响的行。 例如，如果页开发人员调用`DeleteProduct`传入`ProductID`不存在产品，`DELETE`语句向数据库发出会有任何影响，因此`DeleteProduct`方法将返回`false`。

请注意，当添加新的产品或更新现有我们需要在新的或修改产品的字段值中为列表而不是接受的标量`ProductsRow`实例。 此方法已选择原因`ProductsRow`类派生自 ADO.NET`DataRow`类，该类没有默认的无参数构造函数。 若要创建一个新`ProductsRow`实例，我们必须先创建`ProductsDataTable`实例，然后调用其`NewProductRow()`方法 (中执行的操作`AddProduct`)。 我们对插入和更新产品使用 ObjectDataSource 打开时，这种不足 rears 其头。 简单地说，ObjectDataSource 将尝试创建的输入参数的实例。 如果 BLL 方法需要`ProductsRow`实例，ObjectDataSource 将尝试创建一个，但由于缺少默认的无参数构造函数的问题而失败。 有关此问题的详细信息，请参阅下列两个 ASP.NET 论坛帖子： [Strongly-Typed 数据集更新 ObjectDataSources](https://forums.asp.net/1098630/ShowPost.aspx)，和[问题与 ObjectDataSource 和 Strongly-Typed 数据集](https://forums.asp.net/1048212/ShowPost.aspx).

接下来，在同时`AddProduct`和`UpdateProduct`，该代码创建`ProductsRow`实例并使用刚刚中传递的值填充它。 当将值分配到 Datacolumn DataRow 的可能会发生各种字段级别验证检查。 因此，手动将传入的值返回放入 DataRow 有助于确保传递给 BLL 方法的数据的有效性。 遗憾的是 Visual Studio 生成的强类型 DataRow 类请不要使用可以为 null 的类型。 相反，以指示在 DataRow 特定 DataColumn 应对应于`NULL`数据库的值我们必须使用`SetColumnNameNull()`方法。

在`UpdateProduct`我们首先在要更新使用的产品中加载`GetProductByProductID(productID)`。 虽然这可能看上去到数据库的不必要行程，此额外经历一次将证明值得在将来浏览开放式并发的教程。 乐观并发是一种技术，若要确保两个工作的用户可以同时对同一数据不会意外地覆盖彼此的所做更改。 抓取整个记录还可以更轻松地在仅修改的数据行的列的子集 BLL 中创建更新方法。 当我们探讨`SuppliersBLL`我们将看到此类示例的类。

最后，请注意，`ProductsBLL`类具有[DataObject 属性](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx)应用于它 (`[System.ComponentModel.DataObject]`之前的文件的顶部附近的类语句的语法) 并且方法具有[DataObjectMethodAttribute 属性](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx)。 `DataObject`属性标记为适用于绑定到的对象类[ObjectDataSource 控件](https://msdn.microsoft.com/library/9a4kyhcx.aspx)，而`DataObjectMethodAttribute`指示方法的用途。 正如我们将看到在将来的教程，ASP.NET 2.0 ObjectDataSource 轻松地从类中以声明方式访问数据。 若要帮助筛选可能的类，将绑定到对象数据源的向导中的列表，通过默认标记为这些类`DataObjects`都显示在向导的下拉列表。 `ProductsBLL`类使用将同样效果而无需这些属性，但将其添加可更轻松地使用对象数据源的向导中。

## <a name="adding-the-other-classes"></a>添加其他类

与`ProductsBLL`类完成，我们仍需要添加用于处理类别、 供应商和员工的类。 花一些时间创建的以下类和方法使用从上面的示例中的概念：

- **CategoriesBLL.cs**

    - `GetCategories()`
    - `GetCategoryByCategoryID(categoryID)`
- **SuppliersBLL.cs**

    - `GetSuppliers()`
    - `GetSupplierBySupplierID(supplierID)`
    - `GetSuppliersByCountry(country)`
    - `UpdateSupplierAddress(supplierID, address, city, country)`
- **EmployeesBLL.cs**

    - `GetEmployees()`
    - `GetEmployeeByEmployeeID(employeeID)`
    - `GetEmployeesByManager(managerID)`

值得注意的一个方法是`SuppliersBLL`类的`UpdateSupplierAddress`方法。 此方法提供用于更新只需供应商的地址信息的接口。 此方法在内部，以读取`SupplierDataRow`的指定对象的`supplierID`(使用`GetSupplierBySupplierID`)、 设置其与地址相关的属性，然后调入`SupplierDataTable`的`Update`方法。 `UpdateSupplierAddress`方法如下所示：


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample2.cs)]

请参阅本文中的下载 BLL 类我完整实现。

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>步骤 2： 通过 BLL 类访问类型化数据集

第一个教程中我们已了解以编程方式，直接与类型化数据集处理的示例，但我们 BLL 类添加，表示层应针对工作 BLL 改为。 在`AllProducts.aspx`中第一个教程中，示例`ProductsTableAdapter`用于将产品的列表绑定到 GridView，如下面的代码中所示：


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample3.cs)]

若要使用新 BLL 来说，需要更改的类是第一行代码只需将`ProductsTableAdapter`对象`ProductBLL`对象：


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample4.cs)]

也可以通过使用对象数据源 （如可能是类型化数据集） 以声明方式访问 BLL 类。 我们将会讨论中更详细地 ObjectDataSource 在以下教程。


[![产品列表将显示在一个 GridView](creating-a-business-logic-layer-cs/_static/image4.png)](creating-a-business-logic-layer-cs/_static/image3.png)

**图 3**： 的产品的列表显示在一个 GridView ([单击以查看实际尺寸的图像](creating-a-business-logic-layer-cs/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>步骤 3： 向 DataRow 类添加字段级别验证

字段级别验证是与插入或更新时的业务对象的属性值的检查。 产品的一些字段级验证规则包括：

- `ProductName`字段必须是长度不超过 40 个字符
- `QuantityPerUnit`字段必须是长度不超过 20 个字符
- `ProductID`， `ProductName`，和`Discontinued`字段是必需的但所有其他字段为可选
- `UnitPrice`， `UnitsInStock`， `UnitsOnOrder`，和`ReorderLevel`字段必须为大于或等于零

这些规则可以和应表示在数据库级别。 上的字符数限制`ProductName`和`QuantityPerUnit`中列的数据类型，将捕获字段`Products`表 (`nvarchar(40)`和`nvarchar(20)`分别)。 如果数据库表列允许字段必选和可选所用`NULL`s。 四个[check 约束](https://msdn.microsoft.com/library/ms188258.aspx)存在，确保的唯一值大于或等于零可以建立到`UnitPrice`， `UnitsInStock`， `UnitsOnOrder`，或`ReorderLevel`列。

除了强制实施这些规则应用于数据库它们应也会强制执行的数据集级别。 事实上，字段长度和一个值是必需还是可选已捕获的 Datacolumn 的每个 DataTable 的组。 若要查看现有字段级别验证自动提供，请转到数据集设计器，从一个数据表选择一个字段，然后转到属性窗口。 如图 4 所示，`QuantityPerUnit`中的 DataColumn `ProductsDataTable` 20 个字符的最大长度和允许`NULL`值。 如果我们尝试设置`ProductsDataRow`的`QuantityPerUnit`属性超过 20 个字符的字符串值`ArgumentException`将引发。


[![DataColumn 进行基本的字段级别验证](creating-a-business-logic-layer-cs/_static/image7.png)](creating-a-business-logic-layer-cs/_static/image6.png)

**图 4**: DataColumn 提供基本级别字段验证 ([单击以查看实际尺寸的图像](creating-a-business-logic-layer-cs/_static/image8.png))


遗憾的是，我们不能指定边界检查，如`UnitPrice`值必须大于或等于零，通过属性窗口。 为了提供此类型的字段级别验证我们需要创建 DataTable 的事件处理程序[ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx)事件。 中所述[前面教程](creating-a-data-access-layer-cs.md)，可以通过使用分部类扩展由类型化数据集创建的数据集、 数据表和 DataRow 对象。 使用这种方法，我们可以创建`ColumnChanging`事件处理程序`ProductsDataTable`类。 首先创建中的类`App_Code`文件夹名为`ProductsDataTable.ColumnChanging.cs`。


[![将新类添加到 App_Code 文件夹](creating-a-business-logic-layer-cs/_static/image10.png)](creating-a-business-logic-layer-cs/_static/image9.png)

**图 5**： 添加新类中，以便`App_Code`文件夹 ([单击以查看实际尺寸的图像](creating-a-business-logic-layer-cs/_static/image11.png))


接下来，创建的事件处理程序`ColumnChanging`事件，它可确保`UnitPrice`， `UnitsInStock`， `UnitsOnOrder`，和`ReorderLevel`列的值 (如果不是`NULL`) 大于或等于零。 如果任何此类列超出范围时，引发`ArgumentException`。

ProductsDataTable.ColumnChanging.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample5.cs)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>步骤 4： 将自定义业务规则添加到 BLL 的类

除了字段级别验证可能需要高级自定义业务规则来涉及不同实体或级别的单个列不可表示的概念，如：

- 如果产品已停止使用，其`UnitPrice`无法更新
- 员工的国家/地区居住区域必须是其经理的国家/地区的居住地相同
- 无法停用一种产品，如果它是仅由供应商提供的产品

BLL 类应包含检查，以确保遵守应用程序的业务规则。 这些检查可以直接添加到应用到的方法。

假设我们业务规则规定，产品无法将标记不再使用是否它已从给定的供应商唯一的产品。 也就是说，如果产品*X*是我们从供应商处购买的唯一产品*Y*，我们无法将标记*X*如弃用; 如果有，但是，供应商*Y*我们附带三种产品， *A*， *B*，和*C*，我们无法任何标记，并且所有这些为停止使用。 始终未对齐的奇数的业务规则，但业务规则和常识 ！

若要执行此业务规则在`UpdateProducts`方法，我们将首先检查如果`Discontinued`已设置为`true`并且因此，我们将调用`GetProductsBySupplierID`以确定多少产品我们从购买此产品的供应商。 如果只有一个产品从此供应商处购买的我们引发`ApplicationException`。


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample6.cs)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>响应表示层中的验证错误

从表示层调用 BLL 时我们可以决定是否尝试处理可能会引发或让他们向上冒泡到 ASP.NET 的任何异常 (它将引发`HttpApplication`的`Error`事件)。 若要以编程方式使用 BLL 时处理异常，我们可以使用[try … catch](https://msdn.microsoft.com/library/0yd65esw.aspx)块，如以下示例所示：


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample7.cs)]

我们会在将来的教程、 处理异常向上冒泡从 BLL 使用数据时，Web 控件的插入、 更新，或删除数据可以直接在而不是无需代码包装在一个事件处理程序中处理`try...catch`块。

## <a name="summary"></a>摘要

设计良好的应用程序制作成不同的层，其中每个封装特定角色中。 在本系列文章的第一个教程中，我们将创建使用类型化数据集; 数据访问层在本教程中我们构建业务逻辑层为一系列的类在我们的应用程序`App_Code`调入我们 DAL 的文件夹。 BLL 实现我们的应用程序的字段级别和业务级逻辑。 除了创建单独的 BLL，正如我们在本教程中，做另一个选项是扩展 Tableadapter 的方法，通过使用分部类。 但是，使用此方法不允许我们可以重写现有方法也不它分离我们 DAL 和我们 BLL 完全作为我们已经在本文中的方法。

DAL 和 BLL 完成后，我们已准备好在我们的表示层上启动。 在[下一教程](master-pages-and-site-navigation-cs.md)我们将会从数据访问主题简要 detour，并定义以在整个教程使用一致的页面布局。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已沈 Shulok、 Dennis Patterson、 Carlos Santos 和希尔顿 Giesenow。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](creating-a-data-access-layer-cs.md)
[下一页](master-pages-and-site-navigation-cs.md)
