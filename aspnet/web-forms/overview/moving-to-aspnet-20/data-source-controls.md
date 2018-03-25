---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: "数据源控件 |Microsoft 文档"
author: microsoft
description: "DataGrid 控件在 ASP.NET 1.x 标记为 Web 应用程序中的数据访问中的重大改进。 但是，它不是因为它可能是用户友好..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: b1ac7fb62767d61c97fe00338bc0f5087f4863b5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="data-source-controls"></a>数据源控件
====================
by [Microsoft](https://github.com/microsoft)

> DataGrid 控件在 ASP.NET 1.x 标记为 Web 应用程序中的数据访问中的重大改进。 但是，它不是因为它可能是用户友好。 它仍然需要相当长的代码可以从中获得多有用的功能。 在所有数据访问工作中中 1.x 模型就是这样。


DataGrid 控件在 ASP.NET 1.x 标记为 Web 应用程序中的数据访问中的重大改进。 但是，它不是因为它可能是用户友好。 它仍然需要相当长的代码可以从中获得多有用的功能。 在所有数据访问工作中中 1.x 模型就是这样。

ASP.NET 2.0 解决此问题与部分与数据源控件。 ASP.NET 2.0 中的数据源控件提供开发人员提供用于检索数据，显示数据，和编辑数据的声明性模型。 数据源控件的用途是提供一致数据的表示形式而不考虑这些数据源的数据绑定控件。 ASP.NET 2.0 中的数据源控件的核心是 DataSourceControl 抽象类。 DataSourceControl 类提供基实现的 IDataSource 接口和 IListSource 接口，后者允许你要分配为数据绑定控件 （通过新的 DataSourceId 属性的数据源的数据源控件讨论更高版本） 和公开这样的其中作为列表的数据。 每个列表的数据从数据源控件公开为 DataSourceView 对象。 由 IDataSource 接口提供对 DataSourceView 实例的访问。 例如，GetViewNames 方法返回与特定数据源控件，使您能够枚举 DataSourceViews ICollection 关联和 GetView 方法允许你按名称访问特定的 DataSourceView 实例。

数据源控件具有没有用户界面。 作为服务器控件，以便它们可以支持声明性语法，并且它们到页面状态具有访问权限，如果需要实现它们。 数据源控件向客户端不呈现任何 HTML 标记。

> [!NOTE]
> 正如您将看到更高版本，那里还缓存通过使用数据源控件获得的好处。


## <a name="storing-connection-strings"></a>存储连接字符串

我们深入探讨着眼于如何配置数据源控件之前，我们应涵盖有关连接字符串的 ASP.NET 2.0 中的新功能。 ASP.NET 2.0 引入了新的节，您可以轻松地存储连接字符串可在运行时动态读取配置文件中。 &lt;ConnectionStrings&gt;部分可以轻松地存储连接字符串。

下面的代码段添加一个新的连接字符串。

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> 正如使用&lt;appSettings&gt;部分中， &lt;connectionStrings&gt;部分会出现之外&lt;system.web&gt;配置文件中的部分。


若要使用此连接字符串，可以使用以下语法设置服务器控件的 ConnectionString 属性时。

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

&lt;ConnectionStrings&gt;部分也被加密，因此不公开敏感信息。 这项功能将在更高版本的模块中。

## <a name="caching-data-sources"></a>缓存的数据源

每个 DataSourceControl 提供四个属性用于配置缓存;EnableCaching、 CacheDuration、 CacheExpirationPolicy 和 CacheKeyDependency。

## <a name="enablecaching"></a>EnableCaching

EnableCaching 是布尔值属性，它确定缓存启用数据源控件。

## <a name="cacheduration-property"></a>CacheDuration 属性

CacheDuration 属性设置缓存就保持有效的秒的数。 此属性设置为**0**使得保持有效之前显式失效的缓存。

## <a name="cacheexpirationpolicy-property"></a>CacheExpirationPolicy 属性

CacheExpirationPolicy 属性可以设置为**绝对**或**可调**。 将其设置为绝对意味着，将缓存数据的时间的最大量是 CacheDuration 属性指定的秒数。 通过将它设置为可调，执行每个操作时重置的过期时间。

## <a name="cachekeydependency-property"></a>CacheKeyDependency 属性

如果为 CacheKeyDependency 属性指定的字符串值，ASP.NET 将设置该字符串上基于新的缓存依赖项。 这允许您显式使缓存失效的通过只需更改或删除 CacheKeyDependency。

**重要**： 如果模拟已启用和访问数据源和/或数据的内容取决于客户端标识，则我们建议，缓存不可用将 EnableCaching 设置为 False。 如果在此方案中启用了缓存，并最初请求数据的用户以外的用户发出的请求，授权与数据源不会强制执行。 只需将从缓存中提供数据。

## <a name="the-sqldatasource-control"></a>SqlDataSource 控件

SqlDataSource 控件允许开发人员为访问存储在支持 ADO.NET 任何关系数据库中的数据。 它可以使用 System.Data.SqlClient 提供程序访问 SQL Server 数据库、 System.Data.OleDb 提供程序、 System.Data.Odbc 提供程序或要访问 Oracle 的 System.Data.OracleClient 提供程序。 因此，SqlDataSource 肯定不仅用于访问 SQL Server 数据库中的数据。

若要使用 SqlDataSource，你只需为 ConnectionString 属性提供值和指定的 SQL 命令或存储过程。 SqlDataSource 控件负责使用的基础 ADO.NET 体系结构。 它将打开的连接、 查询数据源或执行存储的过程，返回的数据，然后关闭你的连接。

> [!NOTE]
> 因为 DataSourceControl 类会自动关闭你的连接，它应减少客户调用生成的泄露数据库连接数。


以下代码段将 DropDownList 控件绑定到 SqlDataSource 控件使用如上所示的配置文件中存储的连接字符串。

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

如上图所示，SqlDataSource DataSourceMode 属性指定数据源的模式。 在上面的示例中，DataSourceMode 设置为 DataReader。 在这种情况下，SqlDataSource 将返回使用和只进只读游标的 IDataReader 对象。 返回对象的指定的类型的提供程序使用由控制。 在这种情况下，我正在使用 System.Data.SqlClient 提供程序中指定&lt;connectionStrings&gt; web.config 文件的部分。 因此，返回的对象将是类型 SqlDataReader。 通过指定 DataSourceMode 值的数据集，数据可以存储在服务器上的数据集。 此模式可用于添加功能，例如排序、 分页，等等。如果我已经数据绑定到 GridView 控件 SqlDataSource，我将选择的数据集模式。 但是，对于 DropDownList DataReader 模式是正确的选择。

> [!NOTE]
> 当缓存 SqlDataSource 或 AccessDataSource，DataSourceMode 属性必须设置为数据集。 如果你启用与 DataSourceMode DataReader 缓存，则会发生异常。


## <a name="sqldatasource-properties"></a>SqlDataSource 属性

以下是一些 SqlDataSource 控件的属性。

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

一个布尔值，指定是否已取消选择命令，是否其中一个参数为 null。 默认情况下，则为 true。

### <a name="conflictdetection"></a>ConflictDetection

其中多个用户可能正在更新数据源在同一时间的情况下，ConflictDetection 属性确定 SqlDataSource 控件的行为。 此属性的计算结果为 ConflictOptions 枚举值之一。 这些值为**CompareAllValues**和**OverwriteChanges**。 如果设置为 OverwriteChanges，若要将数据写入到数据源的最后一个人将覆盖任何以前的更改。 但是，如果 ConflictDetection 属性设置为 CompareAllValues，SelectCommand 返回的列创建参数，参数还会创建以每个允许到 SqlDataSource 这些列中保存的原始值确定由于 SelectCommand 已执行，因此，发生了变化的值。

### <a name="deletecommand"></a>DeleteCommand

设置或获取从数据库中删除行时使用的 SQL 字符串。 这可以是 SQL 查询或存储的过程名称。

### <a name="deletecommandtype"></a>DeleteCommandType

设置或 SQL 查询 （文本） 或存储的过程 (StoredProcedure) 或者获取 delete 命令的类型。

### <a name="deleteparameters"></a>DeleteParameters

返回与 SqlDataSource 控件关联的 SqlDataSourceView 对象 DeleteCommand 使用的参数。

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

此属性用于指定在其中 ConflictDetection 属性设置为 CompareAllValues 的情况下的原始值参数的格式。 默认值为 {0} 这意味着原始值参数将采用与原始参数同名。 换而言之，如果字段名称为雇员 id，原始值参数将为@EmployeeID。

### <a name="selectcommand"></a>SelectCommand

设置或获取用于从数据库检索数据的 SQL 字符串。 这可以是 SQL 查询或存储的过程名称。

### <a name="selectcommandtype"></a>SelectCommandType

设置或 SQL 查询 （文本） 或存储的过程 (StoredProcedure) 或者获取 select 命令的类型。

### <a name="selectparameters"></a>SelectParameters

返回与 SqlDataSource 控件关联的 SqlDataSourceView 对象 SelectCommand 使用的参数。

### <a name="sortparametername"></a>SortParameterName

获取或设置当使用时会使用对数据进行排序检索数据源控件存储的过程参数的名称。 仅在 SelectCommandType 设置为 StoredProcedure 有效。

### <a name="sqlcachedependency"></a>SqlCacheDependency

指定的数据库和表中的 SQL Server 缓存依赖项使用的以分号分隔字符串。 （SQL 缓存依赖项将讨论在更高版本的模块中。）

### <a name="updatecommand"></a>UpdateCommand

设置或获取在更新数据库中的数据时使用的 SQL 字符串。 这可以是 SQL 查询或存储的过程名称。

### <a name="updatecommandtype"></a>UpdateCommandType

设置或 SQL 查询 （文本） 或存储的过程 (StoredProcedure) 或者获取的更新命令的类型。

### <a name="updateparameters"></a>UpdateParameters

返回与 SqlDataSource 控件关联的 SqlDataSourceView 对象 UpdateCommand 使用的参数。

## <a name="the-accessdatasource-control"></a>AccessDataSource 控件

AccessDataSource 控件派生自 SqlDataSource 类，用于数据绑定到 Microsoft Access 数据库。 AccessDataSource 控件的 ConnectionString 属性是只读的属性。 而不是使用 ConnectionString 属性，用于指向 Access 数据库，如下所示的数据文件属性。

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

AccessDataSource 将始终设置为 System.Data.OleDb 基 SqlDataSource ProviderName 并连接到使用 Microsoft.Jet.OLEDB.4.0 OLE DB 提供程序的数据库。 AccessDataSource 控件不能用于连接到受密码保护的 Access 数据库。 如果你必须连接到受密码保护数据库，则应使用 SqlDataSource 控件。

> [!NOTE]
> 在网站内存储的 access 数据库应放置在应用程序\_数据目录。 ASP.NET 不允许在要浏览此目录中的文件。 你将需要授予对应用程序的读取和写入权限的进程帐户\_数据目录时使用 Access 数据库。


## <a name="the-xmldatasource-control"></a>XmlDataSource 控件

XmlDataSource 用于数据绑定 XML 数据，数据绑定控件。 你可以将它们绑定到 XML 文件使用数据文件属性或可以绑定到使用数据属性的 XML 字符串。 XmlDataSource 作为可绑定字段公开 XML 特性。 在你需要将绑定到不表示为属性的值的情况下，你将需要使用 XSL 转换。 你还可以使用筛选 XML 数据的 XPath 表达式。

请考虑下面的 XML 文件：

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

请注意，XmlDataSource 使用的 XPath 属性*人员/人员*以便筛选仅&lt;人员&gt;节点。 DropDownList 然后数据-将绑定到 LastName 属性使用 DataTextField 属性。

虽然 XmlDataSource 控件主要用于数据绑定到只读的 XML 数据，则可以编辑 XML 数据文件。 请注意，在这种情况下，自动插入、 更新和删除的 XML 文件中的信息不会自动与其他数据源控件一样。 相反，你将需要编写代码来手动编辑使用下列方法之一 XmlDataSource 控件的数据。

### <a name="getxmldocument"></a>GetXmlDocument

检索一个 XmlDocument 对象，包含由 XmlDataSource 检索的 XML 代码。

### <a name="save"></a>保存

将内存中 XmlDocument 保存回数据源。

请务必认识到 Save 方法满足以下两个条件时才会起作用：

1. XmlDataSource 使用数据文件属性绑定到 XML 文件而不是数据属性以将绑定到内存中 XML 数据。
2. 通过转换或 TransformFile 属性不指定任何转换。

另请注意 Save 方法可以产生意外的结果时并行调用多个用户。

## <a name="the-objectdatasource-control"></a>ObjectDataSource 控件

我们已介绍到目前为止的数据源控件是两个层应用程序的极佳选择数据源控件直接与数据存储通信的位置。 但是，许多实际应用程序是多层应用程序可能需要告知这，反过来，与数据层进行通信的业务对象数据源控件。 在这些情况下，ObjectDataSource 精美填充帐单。 ObjectDataSource 结合使用与源对象。 ObjectDataSource 控件将创建的源对象，调用指定方法，单个请求的范围内的所有对象实例的释放的实例，如果你的对象已而不是静态方法 （在 Visual Basic 中为共享） 的实例方法。 因此，你的对象必须是无状态。 也就是说，你的对象应获取和发布在单个请求的范围内的所有所需的资源。 你可以控制如何通过处理 ObjectDataSource 控件 ObjectCreating 事件创建的源对象。 你可以创建的源对象实例和将 ObjectDataSourceEventArgs 类的 ObjectInstance 属性设置为该实例。 ObjectDataSource 控件将使用而不是自行创建实例的 ObjectCreating 事件中创建的实例。

如果 ObjectDataSource 控件源对象公开的公共静态方法 （在 Visual Basic 中为共享） 可以调用来检索和修改数据，ObjectDataSource 控件将直接调用这些方法。 如果 ObjectDataSource 控件必须创建源对象的实例，以便进行方法调用，该对象必须包含不带参数的公共构造函数。 创建源对象的新实例时，ObjectDataSource 控件将调用此构造函数。

如果源对象不包含不带参数的公共构造函数，你可以创建将由 ObjectCreating 事件中的 ObjectDataSource 控件源对象的实例。

## <a name="specifying-object-methods"></a>指定对象的方法

ObjectDataSource 控件源对象可以包含任意数量的方法，用于选择、 插入、 更新或删除数据。 由 ObjectDataSource 控件基于名称的方法，调用这些方法，如通过 ObjectDataSource 控件 SelectMethod、 InsertMethod、 UpdateMethod 或 delete 方法属性标识。 源对象还可以包括可选的 SelectCount 方法，由使用的 SelectCountMethod 属性，返回的数据源中的对象总数的计数的 ObjectDataSource 控件标识。 ObjectDataSource 控件将调用 SelectCount 方法之后调用选择方法以当分页时检索的使用数据源处的记录总数。

## <a name="lab-using-data-source-controls"></a>实验室中使用数据源控件

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>练习 1-SqlDataSource 控件与显示数据

下面的练习使用 SqlDataSource 控件来连接到 Northwind 数据库。 它假定您的 SQL Server 2000 实例上有包含到 Northwind 数据库的访问。

1. 创建一个新的 ASP.NET 网站。
2. 添加新的 web.config 文件。

    1. 右键单击解决方案资源管理器中的项目，然后单击添加新项。
    2. 从模板列表中选择 Web 配置文件并单击添加。
3. 编辑&lt;connectionStrings&gt;部分，如下所示： 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. 切换到代码视图，并添加 ConnectionString 属性和 SelectCommand 属性与&lt;asp: SqlDataSource&gt;控制，如下所示： 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. 从设计视图中，添加一个新的 GridView 控件。
6. 从 GridView 任务菜单的选择数据源下拉列表中，选择 SqlDataSource1。
7. 右键单击 Default.aspx，然后在浏览器中从菜单中选择视图。 当系统提示保存，请单击是。
8. GridView 显示从 Products 表的数据。

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>练习 2-编辑数据与 SqlDataSource 控件

下面的练习演示如何将数据绑定的 DropDownList 控制是通过使用声明性语法和允许你编辑的 DropDownList 控件中显示的数据。

1. 在设计视图中，从 Default.aspx 中删除 GridView 控件。 

    **重要**： 离开页面上的 SqlDataSource 控件。
2. 向 Default.aspx 中添加的 DropDownList 控件。
3. 切换到源视图。
4. 添加到的 DataSourceId、 DataTextField 和 DataValueField 特性&lt;asp: DropDownList&gt;控制，如下所示： 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. 保存 Default.aspx，并在浏览器中查看它。 请注意，下拉列表包含所有来自 Northwind 数据库的产品。
6. 关闭浏览器。
7. 在源视图中的 Default.aspx，添加一个新的文本框中控件的 DropDownList 控件下方。 将文本框中的 ID 属性更改为 txtProductName。
8. 在文本框控件中，添加一个新的按钮控件。 将按钮的 ID 属性更改为 btnUpdate 和将文本属性设为**更新产品名称**。
9. 在 Default.aspx 的源视图中，将添加 UpdateCommand 属性和两个新 UpdateParameters 到 SqlDataSource 标记，如下所示： 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > 请注意，有两个更新此代码中添加的参数 （ProductName 和 ProductID）。 这些参数映射到 txtProductName 文本框中的文本属性和 ddlProducts DropDownList SelectedValue 属性。
10. 切换到设计视图，并双击该按钮控件添加事件处理程序。
11. 将以下代码添加到 btnUpdate\_单击代码： 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. 在 Default.aspx 上右键单击并选择在浏览器中查看它。 当系统提示保存所有更改，请单击是。
13. ASP.NET 2.0 分部类允许在运行时编译。 不需要生成应用程序，以便查看代码更改生效。
14. 从下拉列表中选择一个产品。
15. 在文本框中输入所选产品的新名称，然后单击更新按钮。
16. 在数据库中更新的产品名称。

## <a name="exercise-3-using-the-objectdatasource-control"></a>练习 3 使用 ObjectDataSource 控件

本练习将演示如何使用 ObjectDataSource 控件和源对象进行交互与 Northwind 数据库。

1. 右键单击解决方案资源管理器中的项目，然后单击添加新项。
2. 在模板列表中选择 Web 窗体。 将名称更改为 object.aspx 并单击添加。
3. 右键单击解决方案资源管理器中的项目，然后单击添加新项。
4. 在模板列表中选择类。 将类的名称更改为 NorthwindData.cs 并单击添加。
5. 单击是将类添加到应用程序出现提示时\_代码文件夹。
6. 将以下代码添加到 NorthwindData.cs 文件： 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. 将以下代码添加到 object.aspx 的源视图： 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. 保存所有文件，并浏览 object.aspx。
9. 通过查看详细信息、 编辑员工、 添加员工，和删除员工与界面进行交互。
