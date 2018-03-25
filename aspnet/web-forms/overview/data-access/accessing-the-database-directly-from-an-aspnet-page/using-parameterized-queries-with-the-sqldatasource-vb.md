---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
title: "参数化的查询使用 SqlDataSource (VB) |Microsoft 文档"
author: rick-anderson
description: "在本教程中，我们将继续我们看 SqlDataSource 控件，并了解如何定义参数化的查询。 可以指定参数这两个 decla..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: e322f34c-83b7-41ea-ab65-ab1e0bdcc609
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: b1cda18620a970c45b05039dd380c393e3854889
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="using-parameterized-queries-with-the-sqldatasource-vb"></a>使用 SqlDataSource (VB) 的参数化的查询
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_VB.exe)或[下载 PDF](using-parameterized-queries-with-the-sqldatasource-vb/_static/datatutorial48vb1.pdf)

> 在本教程中，我们将继续我们看 SqlDataSource 控件，并了解如何定义参数化的查询。 参数声明的方式和以编程方式，可以指定，可以从大量位置，例如，查询字符串、 会话状态，其他控件和的详细信息的请求。


## <a name="introduction"></a>介绍

在以前的教程，我们已了解如何使用 SqlDataSource 控件直接从数据库检索数据。 使用配置数据源向导，我们可以选择数据库，然后将其： 选取要从表或视图; 返回的列输入自定义的 SQL 语句;或使用存储的过程。 从表或视图选择列或输入自定义 SQL 语句，SqlDataSource 控件是否 s`SelectCommand`属性分配产生的临时 SQL`SELECT`语句也是如此这`SELECT`语句时执行SqlDataSource 的`Select()`调用方法 (以编程方式或自动从数据 Web 控件)。

SQL`SELECT`中以前的教程的演示不具备使用语句`WHERE`子句。 在`SELECT`语句，`WHERE`子句可以用于限制返回的结果。 例如，若要显示的价格多个 $50.00 的产品的名称，我们可以使用以下查询：


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample1.sql)]

通常情况下中, 使用的值`WHERE`子句确定被某些外部源，例如查询字符串值、 会话变量或从 Web 控件在页上的用户输入。 理想情况下，将这类输入指定使用*参数*。 使用 Microsoft SQL Server，使用表示参数`@parameterName`，如下所示：


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample2.sql)]

SqlDataSource 支持参数化的查询，同时包括这两者`SELECT`语句和`INSERT`， `UPDATE`，和`DELETE`语句。 此外，参数值可以自动从请求不同的源查询字符串，会话状态，在页上，控件和其他操作或可以以编程方式分配。 在本教程中，我们将了解如何定义参数化的查询以及如何指定参数值同时以声明方式和以编程方式。

> [!NOTE]
> 在以前的教程，我们将比较 ObjectDataSource 我们所选的工具已通过使用 SqlDataSource，注意其概念的相似之处的第一次 46 教程。 这些相似之处还将扩展到参数。 ObjectDataSource 的参数映射到在业务逻辑层的方法的输入参数。 使用 SqlDataSource，直接在 SQL 查询中定义参数。 这两个控件具有的参数集合及其`Select()`， `Insert()`， `Update()`，和`Delete()`方法，而且可以都是从预定义源 （查询字符串值、 会话变量中，依次类推填充这些参数值) 或以编程方式分配。


## <a name="creating-a-parameterized-query"></a>创建参数化查询

SqlDataSource 控件 s 配置数据源向导提供了用于定义要执行若要检索数据库记录的命令的三个途径：

- 通过选取从现有的表或视图中，列
- 通过输入自定义 SQL 语句，或
- 通过选择存储的过程

选择现有表或视图的参数中的列时`WHERE`子句必须指定通过添加`WHERE`子句对话框。 在创建自定义 SQL 语句，但是，可以输入参数直接转换`WHERE`子句 (使用`@parameterName`来表示每个参数)。 A[存储过程](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)包含一个或多个 SQL 语句和这些语句可以参数化。 SQL 语句中使用的参数但是，必须在作为输入参数传递到存储过程。

因为创建参数化的查询取决于如何 SqlDataSource 的`SelectCommand`是指定，但仍使 s 一下在所有三种方法。 若要开始，打开`ParameterizedQueries.aspx`页面`SqlDataSource`文件夹中，从工具箱中拖动到设计器中，拖动 SqlDataSource 控件并设置其`ID`到`Products25BucksAndUnderDataSource`。 接下来，单击从控件 s 智能标记的配置数据源链接。 选择要使用的数据库 (`NORTHWINDConnectionString`)，然后单击下一步。

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>步骤 1： 添加`WHERE`子句从表或视图选择列时

在选择要返回数据库中具有 SqlDataSource 控件的数据时，配置数据源向导允许我们只需选择要从现有表中返回或查看 （请参见图 1） 的列。 会自动执行生成 sql`SELECT`语句，它就发送到数据库时 SqlDataSource 的`Select()`调用方法。 正如我们做上一教程中，从下拉列表中选择产品表，并检查`ProductID`， `ProductName`，和`UnitPrice`列。


[![选取要从表或视图中返回的列](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.png)

**图 1**： 到返回从表或视图选择列 ([单击以查看实际尺寸的图像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.png))


若要包括`WHERE`中的子句`SELECT`语句中，单击`WHERE`按钮，随后会显示添加`WHERE`子句对话框中 （请参见图 2）。 若要添加参数以限制返回的结果`SELECT`查询，首先选择要筛选的数据的列。 接下来，选择要用于筛选的运算符 (=、 &lt;， &lt;=、 &gt;，依次类推)。 最后，例如选择从查询字符串或会话状态参数的值的源。 在配置参数后，单击添加按钮以将其包含在`SELECT`查询。

对于本例，请让 s 只返回这些结果其中`UnitPrice`值是否小于或等于 $25.00。 因此，选取`UnitPrice`从列下拉列表和&lt;= 从运算符下拉列表。 使用硬编码的参数值 （如 $25.00) 时或参数值是以编程方式指定，则选择无从源下拉列表。 接下来，25.00 中的值文本框中输入的硬编码的参数值，并通过单击添加按钮完成该过程。


[![限制从返回的结果添加 WHERE 子句对话框](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.png)

**图 2**： 限制从添加结果返回`WHERE`子句对话框中 ([单击以查看实际尺寸的图像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.png))


添加参数后, 单击确定以返回到配置数据源向导。 `SELECT`底部的向导的语句现在应包含`WHERE`与名为的参数的子句`@UnitPrice`:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample3.sql)]

> [!NOTE]
> 如果指定多个条件中的`WHERE`子句添加从`WHERE`子句对话框中，向导将它们联接起来与`AND`运算符。 如果你需要包括`OR`中`WHERE`子句 (如`WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`) 则需要生成`SELECT`通过自定义 SQL 语句屏幕的语句。


完成配置 SqlDataSource （单击下一步，然后完成），然后检查 SqlDataSource s 声明性标记。 标记现在包含`<SelectParameters>`集合，这明确指出中的参数的源`SelectCommand`。


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample4.aspx)]

当 SqlDataSource s`Select()`调用方法时，`UnitPrice`参数值 (25.00) 应用于`@UnitPrice`中的参数`SelectCommand`之前发送到数据库。 最终结果是仅返回小于或等于 $25.00 而返回这些产品`Products`表。 若要确认此操作，请向页面添加一个 GridView，将其绑定到此数据源，然后查看通过浏览器页面。 你应该只会看到列出的、 小于或等于 $25.00，如下图 3 确认这些产品。


[![显示只有那些产品小于或等于 $25.00](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.png)

**图 3**： 显示仅那些产品小于或等于 $25.00 ([单击以查看实际尺寸的图像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.png))


## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>步骤 2： 将参数添加到自定义 SQL 语句

添加自定义 SQL 语句时可以输入`WHERE`子句显式或指定以查询生成器的筛选器单元格的值。 为了说明这点，让其价格均为某个特定阈值小于 GridView 中显示仅这些产品的 s。 首先，通过添加到文本框中`ParameterizedQueries.aspx`页后，可以从用户处收集此阈值。 设置文本框中 s`ID`属性`MaxPrice`。 添加按钮 Web 控件并设置其`Text`属性显示匹配的产品。

接下来，将一个 GridView 拖到该页面上并从其智能标记选择创建名为新 SqlDataSource `ProductsFilteredByPriceDataSource`。 从配置数据源向导中，转到指定的自定义 SQL 语句或存储的过程屏幕 （请参见图 4），然后输入以下查询：


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample5.sql)]

在输入查询 （手动或通过查询生成器） 后，单击下一步。


[![只返回那些小于或等于参数值](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.png)

**图 4**： 返回仅那些产品小于或等于参数值 ([单击以查看实际尺寸的图像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.png))


由于该查询包含参数，该向导的下一个屏幕将提示我们提供的参数值的源。 从参数源下拉列表中选择控件和`MaxPrice`(TextBox 控件的`ID`值) 从 ControlID 下拉列表。 你也可以输入要在用户未输入任何文本到其中的情况下使用的可选的默认值`MaxPrice`文本框。 同时，不要输入默认值。


[![MaxPrice 文本框中的文本属性用作参数源](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.png)

**图 5**:`MaxPrice`文本框中 s`Text`属性用作参数源 ([单击以查看实际尺寸的图像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.png))


单击下一步，以完成配置数据源向导，然后完成。 GridView、 文本框、 按钮和 SqlDataSource 的声明性标记如下所示：


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample6.aspx)]

请注意，SqlDataSource s 中的参数`<SelectParameters>`部分`ControlParameter`，其中包括其他属性，例如`ControlID`和`PropertyName`。 当 SqlDataSource s`Select()`调用方法时，`ControlParameter`捕捉从指定的 Web 控件属性的值并将其分配到中的相应参数`SelectCommand`。 在此示例中， `MaxPrice` s 文本属性用作`@MaxPrice`参数值。

花一些时间才能查看此页通过浏览器。 在第一次访问页时或每次`MaxPrice`文本框中缺少记录不会显示在 GridView 的值。


[![没有记录是显示时 MaxPrice 文本框中为空](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.png)

**图 6**： 无记录时会显示`MaxPrice`文本框是空 ([单击以查看实际尺寸的图像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.png))


显示要安装的产品的原因是因为，默认情况下，参数值为空字符串转换为数据库`NULL`值。 由于比较`[UnitPrice] <= NULL`始终评估为 False，返回任何结果。

在文本框中，如 5.00，输入一个值，然后单击显示匹配产品按钮。 在回发时，SqlDataSource 通知 GridView 其参数源一个已更改。 因此，GridView 绑定到 SqlDataSource，显示这些产品小于或等于为 $5.00。


[![显示产品小于或等于 5.00 美元](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.png)

**图 7**： 显示产品小于或等于 5.00 美元 ([单击以查看实际尺寸的图像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.png))


## <a name="initially-displaying-all-products"></a>最初显示的所有产品

而不是首次加载页时显示的产品，我们可能想要显示*所有*产品。 列出所有产品的一种方法每当`MaxPrice`文本框是空是将参数的默认值设置为某些 insanely 高的值，如 1000000，因为它正 s，Northwind Traders 将不断已列出清单单价不太可能超过 1000000。 但是，此方法是 shortsighted，并在其他情况下可能无法工作。

在前面的教程-[声明性参数](../basic-reporting/declarative-parameters-vb.md)和[主/详细信息筛选与 DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md)我们已面对类似的问题。 我们的解决方案有已放置在业务逻辑层中的此逻辑。 具体而言，BLL 检查传入的值，如果它是`NULL`或某些保留值，调用路由到返回所有记录的 DAL 方法。 如果传入的值为一个正常的筛选值，对执行使用参数化 SQL 语句 DAL 方法进行调用`WHERE`子句与提供的值。

遗憾的是，我们绕过体系结构使用 SqlDataSource 时。 相反，我们需要自定义 SQL 语句以智能方式获取的所有记录，如果`@MaximumPrice`参数是`NULL`或某些保留的值。 对于本练习中，让 s 具有它因此，如果`@MaximumPrice`参数等于`-1.0`，然后*所有*的记录是要返回 (`-1.0`发挥保留值，因为没有任何产品可以有一个负数`UnitPrice`值)。 若要实现此目的，我们可以使用以下 SQL 语句：


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample7.sql)]

这`WHERE`子句将返回*所有*记录如果`@MaximumPrice`参数等于`-1.0`。 如果参数值不是`-1.0`，只有这些产品其`UnitPrice`小于或等于`@MaximumPrice`返回参数值。 通过设置的默认值`@MaximumPrice`参数`-1.0`，在第一个页面加载 (或每当`MaxPrice`文本框中为空)，`@MaximumPrice`将具有的值`-1.0`和所有产品都将都显示。


[![现在所有产品都都显示时 MaxPrice 文本框是空](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.png)

**图 8**： 现在所有产品都都显示时`MaxPrice`文本框是空 ([单击以查看实际尺寸的图像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image16.png))


有几点需要注意的这种方法。 首先，请注意参数的数据类型由它推断 s SQL 查询中的使用情况。 如果你更改`WHERE`子句从`@MaximumPrice = -1.0`到`@MaximumPrice = -1`，运行时将其参数视为一个整数。 如果你然后尝试分配`MaxPrice`错误会发生 （如 5.00) 的十进制值的文本框中，因为它不能将 5.00 转换为整数。 若要解决此问题，请确保你使用`@MaximumPrice = -1.0`中`WHERE`子句或更高，设置`ControlParameter`对象的`Type`为十进制数的属性。

其次，通过添加`OR @MaximumPrice = -1.0`到`WHERE`子句，查询引擎无法使用索引，在`UnitPrice`（假设存在），从而导致表扫描。 如果有大量足够大中的记录，这可能会影响性能`Products`表。 更好的方法是将该逻辑移到存储过程其中`IF`语句将执行`SELECT`查询从`Products`表而无需`WHERE`子句时需要返回所有记录或一个其`WHERE`子句只包含`UnitPrice`条件，以便可以使用索引。

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>步骤 3： 创建和使用参数化存储的过程

存储的过程可以定义存储过程内的 SQL 语句中包括一套然后，可以使用的输入参数。 在配置 SqlDataSource 用于接受输入的参数的存储的过程时，可以指定这些参数值，与临时 SQL 语句中使用的相同技术。

若要说明在 SqlDataSource 中使用存储的过程，让 s 创建新的存储的过程名为 Northwind 数据库中`GetProductsByCategory`，这样便可以接受一个名为参数`@CategoryID`并返回产品的列的所有其`CategoryID`列匹配的`@CategoryID`。 若要创建存储的过程，请转到服务器资源管理器中，并向下钻取`NORTHWND.MDF`数据库。 （如果 don t 看到服务器资源管理器，显示它通过转到视图菜单并选择服务器资源管理器选项。）

从`NORTHWND.MDF`数据库、 右键单击存储过程文件夹、 选择添加新存储过程和输入以下语法：


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample8.sql)]

单击保存图标 （或 Ctrl + S） 若要保存存储的过程。 你可以通过从存储过程文件夹，右键单击该并选择执行测试存储的过程。 这将提示你输入该存储的过程的参数 (`@CategoryID`，在这种情况) 之后该结果将显示在输出窗口中。


[![GetProductsByCategory 存储过程执行时，@CategoryID为 1](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image17.png)

**图 9**:`GetProductsByCategory`时的情况下执行存储过程`@CategoryID`的 1 ([单击以查看实际尺寸的图像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image18.png))


让我们来使用此存储的过程中一个 GridView 饮料类别中显示的所有产品。 向页面添加一个新的 GridView 并将其绑定到名为新 SqlDataSource `BeverageProductsDataSource`。 继续到指定的自定义 SQL 语句或存储的过程屏幕，选择存储过程单选按钮，并选择`GetProductsByCategory`从下拉列表的存储过程。


[![选择 GetProductsByCategory 从下拉列表的存储过程](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image19.png)

**图 10**： 选择`GetProductsByCategory`从下拉列表中的存储过程 ([单击以查看实际尺寸的图像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image20.png))


因为存储的过程接受一个输入的参数 (`@CategoryID`)，单击下一步提示我们需要指定此参数的值的源。 饮料`CategoryID`为 1，因此在无保持参数源下拉列表，然后在默认值文本框中输入 1。


[![使用硬编码值为 1 以返回饮料类别中的产品](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image21.png)

**图 11**： 使用 Hard-Coded 值为 1 以返回饮料类别中的产品 ([单击以查看实际尺寸的图像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image22.png))


如以下声明性标记所示，当使用存储的过程，SqlDataSource s`SelectCommand`属性设置为存储过程的名称和[`SelectCommandType`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx)设置为`StoredProcedure`，以指示`SelectCommand`是存储的过程而不是临时 SQL 语句的名称。


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample9.aspx)]

测试浏览器中的页。 尽管显示仅属于饮料类别这些产品*所有*产品的字段显示自`GetProductsByCategory`存储的过程返回中的列的所有`Products`表。 当然，我们无法限制或自定义 GridView s 编辑列对话框中从 GridView 中显示的字段。


[![将显示所有饮料](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image23.png)

**图 12**： 将显示所有饮料 ([单击以查看实际尺寸的图像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image24.png))


## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>步骤 4： 以编程方式调用 SqlDataSource 的`Select()`语句

示例我们到目前为止看到本教程以前一教程中的已有 SqlDataSource 控件直接绑定到 GridView。 SqlDataSource 控件的数据，但是，可以以编程方式访问和在代码中枚举。 当你需要查询数据对其进行检查，但不要需要显示它，则可能特别有用。 而不是无需编写所有样本 ADO.NET 代码，以连接到数据库、 指定的命令和检索结果，你可以让处理此单调代码 SqlDataSource。

为了说明使用 SqlDataSource 的数据以编程方式，我们假设，您的老板已接近你与创建显示随机选择的类别和其关联的产品的名称的网页的请求。 也就是说，当用户访问此页时，我们希望随机选择一个类别从`Categories`表、 显示的类别名称，并列出属于该类别的产品。

若要实现此目的，我们需要两个 SqlDataSource 控件一个获取从随机类别`Categories`表，另一个用于获取 s 产品的类别。 我们生成了检索此步骤; 中的随机类别记录 SqlDataSource编写检索类别的产品 SqlDataSource 考察步骤 5。

首先，通过添加到 SqlDataSource`ParameterizedQueries.aspx`并设置其`ID`到`RandomCategoryDataSource`。 对其进行配置以便它使用以下 SQL 查询：


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample10.sql)]

`ORDER BY NEWID()`返回按随机顺序进行排序的记录 (请参阅[使用`NEWID()`随机排序记录](http://www.sqlteam.com/item.asp?ItemID=8747))。 `SELECT TOP 1`返回从结果集中的第一条记录。 总之，此查询返回`CategoryID`和`CategoryName`列从单一的随机选择的类别的值。

若要显示的类别 s`CategoryName`值、 向页面添加标签 Web 控件，请将其`ID`属性`CategoryNameLabel`，并将清空其`Text`属性。 若要以编程方式从 SqlDataSource 控件中检索数据，我们需要调用其`Select()`方法。 [ `Select()`方法](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx)需要一个输入的参数的类型[ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx)，它指定应如何在返回前消息数据。 这可以包括排序和筛选的数据的说明，使用 Web 控件进行排序，或从 SqlDataSource 控件数据进行分页的数据。 对于我们的示例，不过，我们不需要数据在返回前修改，因此将将传递中`DataSourceSelectArguments.Empty`对象。

`Select()`方法返回实现的对象`IEnumerable`。 依赖于 SqlDataSource 控件 s 的值，则返回的精确类型[`DataSourceMode`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx)。 以前一教程中所述，可以将此属性设置为值`DataSet`或`DataReader`。 如果设置为`DataSet`、`Select()`方法返回[DataView](https://msdn.microsoft.com/library/01s96x0z.aspx)对象; 如果设置为`DataReader`，它将返回实现的对象[ `IDataReader` ](https://msdn.microsoft.com/library/system.data.idatareader.aspx)。 由于`RandomCategoryDataSource`SqlDataSource 具有其`DataSourceMode`属性设置为`DataSet`（默认值），我们将使用 DataView 对象。

下面的代码演示如何检索从记录`RandomCategoryDataSource`作为 DataView SqlDataSource 以及如何读取`CategoryName`从第一个 DataView 行的列值：


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample11.vb)]

`randomCategoryView(0)`返回第一个`DataRowView`处于数据视图。 `randomCategoryView(0)("CategoryName")`返回的值`CategoryName`在此第一行中的列。 请注意，数据视图是松散类型化。 若要引用特定的列的值我们需要将作为字符串 (在此情况下，CategoryName) 传递列的名称。 图 13 显示中显示的消息`CategoryNameLabel`查看网页时。 当然，显示的实际类别名称随机选择通过`RandomCategoryDataSource`SqlDataSource 每个访问 （包括回发） 页。


[![名称显示随机选择的类别 s](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image25.png)

**图 13**： 显示名称的随机选择的类别 s ([单击以查看实际尺寸的图像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image26.png))


> [!NOTE]
> 如果 SqlDataSource 控制 s`DataSourceMode`属性必须设置为`DataReader`中的返回值`Select()`方法需要强制转换为`IDataReader`。 若要读取`CategoryName`来自第一行中，我们 d 列值使用如下代码：


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample12.vb)]

使用 SqlDataSource 随机选择一个类别，我们重新已准备好添加列出类别的产品 GridView。

> [!NOTE]
> 而不是使用标签 Web 控件来显示类别的名称，我们无法添加 FormView 或说明如何到页中，将其绑定到 SqlDataSource。 使用标签，但是，让我们研究一下如何以编程方式调用 SqlDataSource 的`Select()`语句和其生成的数据，在代码中使用。


## <a name="step-5-assigning-parameter-values-programmatically"></a>步骤 5： 以编程方式分配参数值

所有这些示例我们遇到为止看到在本教程中使用的是硬编码参数值或执行从预定义的参数源 （页上，依次类推 Web 控件中的查询字符串值） 之一。 但是，SqlDataSource 控件的参数还可以设置以编程方式。 若要完成当前在本例中，我们需要返回属于指定类别的产品的所有 SqlDataSource。 将具有此 SqlDataSource`CategoryID`参数需要设置其值基于`CategoryID`返回列值`RandomCategoryDataSource`中的 SqlDataSource`Page_Load`事件处理程序。

首先，通过添加到页面的一个 GridView，并将其绑定到名为新 SqlDataSource `ProductsByCategoryDataSource`。 像我们未在步骤 3 中，以便它将调用配置 SqlDataSource`GetProductsByCategory`存储过程。 将参数源下拉列表设置为 None，但不要输入默认值，因为我们将以编程方式设置此默认值。


[![未指定参数源或默认值](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image27.png)

**图 14**： 未指定参数源或默认值 ([单击以查看实际尺寸的图像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image28.png))


完成 SqlDataSource 向导后，生成的声明性标记应类似于以下：


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample13.aspx)]

我们可以分配`DefaultValue`的`CategoryID`参数以编程方式在`Page_Load`事件处理程序：


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample14.vb)]

添加此元素后，该页面包括一个 GridView，显示与随机选择的类别关联的产品。


[![未指定参数源或默认值](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image29.png)

**图 15**： 未指定参数源或默认值 ([单击以查看实际尺寸的图像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image30.png))


## <a name="summary"></a>摘要

SqlDataSource 使页开发人员可以定义其参数值可以为硬编码、 从预定义的参数源中提取或以编程方式分配参数化的查询。 在本教程中我们已了解如何创建参数化的查询从临时 SQL 查询和存储的过程的配置数据源向导。 我们还了解了使用硬编码参数源，Web 控件作为参数源，并以编程方式指定参数值。

如与 ObjectDataSource，SqlDataSource 还提供功能来修改其基础数据。 在下一教程中我们将考察如何定义`INSERT`， `UPDATE`，和`DELETE`使用 SqlDataSource 的语句。 添加这些语句后，我们可以利用内置插入、 编辑和删除固有 GridView，说明如何和 FormView 控件功能。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Scott Clyde Randell Schmidt，Ken Pespisa。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](querying-data-with-the-sqldatasource-control-vb.md)
[下一页](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
