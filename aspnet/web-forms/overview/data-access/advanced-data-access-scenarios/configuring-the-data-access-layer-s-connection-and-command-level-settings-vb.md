---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
title: "配置的数据访问层连接和命令级别设置 (VB) |Microsoft 文档"
author: rick-anderson
description: "在类型化数据集中 Tableadapter 自动负责连接到数据库、 发布命令，并填充数据表中使用的结果..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: d57dfa2b-d627-45cb-b5b1-abbf3159d770
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
msc.type: authoredcontent
ms.openlocfilehash: ab392f2a7d9b6cf97da920f899aea23379209f96
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-vb"></a>配置的数据访问层连接和命令级别设置 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_VB.zip)或[下载 PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/datatutorial72vb1.pdf)

> 在类型化数据集中 Tableadapter 自动负责连接到数据库、 发布命令，并填充数据表中使用的结果。 但是，如果我们要注意的自行，这些详细信息和在本教程中我们将了解如何访问 TableAdapter 中的数据库连接和命令级别设置有情况。


## <a name="introduction"></a>介绍

在整个教程系列我们使用了类型化数据集来实现数据访问层和业务对象我们分层体系结构。 中所述[第一个教程](../introduction/creating-a-data-access-layer-vb.md)，数据表用作存储库的数据而 Tableadapter 充当包装以与要检索和修改基础数据的数据库通信的类型化数据集 s。 Tableadapter 封装在使用数据库中所涉及的复杂性并使我们不必编写代码以连接到数据库、 发出命令，或填充到数据表的结果。

有的时间，但是，当我们需要 burrow 到 TableAdapter 的深度和编写代码直接使用的 ADO.NET 对象。 在[在事务中包装数据库修改](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md)教程中，例如，我们添加方法到开头、 提交，和回滚 ADO.NET 事务 TableAdapter。 这些方法用于内部，手动创建`SqlTransaction`对象分配给 TableAdapter 的`SqlCommand`对象。

在本教程中我们将查看如何访问 TableAdapter 中的数据库连接和命令级别设置。 具体而言，我们将添加到功能`ProductsTableAdapter`启用访问权限的基础的连接字符串和命令超时设置。

## <a name="working-with-data-using-adonet"></a>使用使用 ADO.NET 的数据

Microsoft.NET Framework 包含大量的专门用于处理数据的类。 这些类，在中找到[`System.Data`命名空间](https://msdn.microsoft.com/library/system.data.aspx)，统称为*ADO.NET*类。 某些 ADO.NET 涵盖的类绑定到特定*数据提供程序*。 可以将数据提供程序视为一个信道，使信息 ADO.NET 类和基础数据存储区之间流动。 有等 OleDb 和 ODBC，以及提供程序专门设计用于特定的数据库系统通用的提供程序。 例如，可以将连接到使用 OleDb 访问接口的 Microsoft SQL Server 数据库时，SqlClient 提供程序是高效得多如设计和优化专门为 SQL Server。

当以编程方式访问数据时，通常使用以下模式：

1. 建立到数据库的连接。
2. 发出命令。
3. 有关`SELECT`查询，适用于生成的记录。

有单独的 ADO.NET 类，用于执行每个步骤。 若要连接到使用 SqlClient 提供程序的数据库，例如，使用[`SqlConnection`类](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx)。 颁发`INSERT`， `UPDATE`， `DELETE`，或`SELECT`到数据库，使用命令[`SqlCommand`类](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx)。

除[在事务中包装数据库修改](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md)教程中，我们没有编写任何低级别的 ADO.NET 代码自行因为 Tableadapter 自动生成的代码包括到所需的功能连接到数据库、 发出命令、 检索数据，和该数据填充到数据表。 但是，可能有当我们需要自定义这些低级别的设置的时间。 通过以下几个步骤中，我们将查看如何发挥供内部使用 Tableadapter 的 ADO.NET 对象。

## <a name="step-1-examining-with-the-connection-property"></a>第 1 步： 检查与连接属性

每个 TableAdapter 类具有`Connection`指定数据库连接信息的属性。 此属性的数据类型和`ConnectionString`值由 TableAdapter 配置向导中所做的选择。 回想一下，我们首先将 TableAdapter 添加到类型化数据集时此向导将询问你我们数据库源 （请参见图 1）。 此第一步中的下拉列表包括这些配置文件，以及在服务器资源管理器数据连接的任何其他数据库中指定的数据库。 如果下拉列表中，我们要使用的数据库不存在，则可以通过单击新建连接按钮并提供所需的连接信息指定新的数据库连接。


[![TableAdapter 配置向导第一步](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image1.png)

**图 1**: TableAdapter 配置向导的第一步 ([单击以查看实际尺寸的图像](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image3.png))


允许 s 花一些时间来检查 TableAdapter s 代码`Connection`属性。 中所述[创建数据访问层](../introduction/creating-a-data-access-layer-vb.md)教程中，我们可以通过转到类视图窗口中，向下合适的类，钻取，然后双击成员名称来查看自动生成的 TableAdapter 代码。

通过转到视图菜单并选择类视图 （或通过键入 Ctrl + Shift + C），请导航到类视图窗口。 从类视图窗口的上半，深化至`NorthwindTableAdapters`命名空间，然后选择`ProductsTableAdapter`类。 这将显示`ProductsTableAdapter`底部中的 s 成员一半类视图，如图 2 中所示。 双击`Connection`属性以查看其代码。


![双击要查看其自动生成的代码的类视图中的连接属性](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image4.png)

**图 2**： 双击类视图以查看其自动生成的代码中的连接属性


TableAdapter 的`Connection`属性和其他与连接相关的代码如下所示：


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample1.vb)]

当实例化的 TableAdapter 类时，成员变量`_connection`等同于`Nothing`。 当`Connection`访问属性时，它首先检查是否`_connection`成员变量已实例化。 如果没有，`InitConnection`调用方法时，该实例化`_connection`并设置其`ConnectionString`TableAdapter 配置向导 s 第一个步骤中指定的连接字符串值的属性。

`Connection`属性还可以分配给`SqlConnection`对象。 这样将相关联的新`SqlConnection`对象与每个 TableAdapter 的`SqlCommand`对象。

## <a name="step-2-exposing-connection-level-settings"></a>步骤 2： 公开连接级别设置

连接信息应保持 TableAdapter 中封装并无法访问应用程序体系结构中的其他层。 但是，可能有方案的 TableAdapter 的连接级别信息需要访问或可自定义的查询、 用户或 ASP.NET 页时。

让我们来扩展`ProductsTableAdapter`中`Northwind`要包括的数据集`ConnectionString`业务逻辑层可以使用用于读取或更改所用的 TableAdapter 的连接字符串的属性。

> [!NOTE]
> A*连接字符串*是一个字符串，指定数据库连接信息，如要使用的数据库、 身份验证凭据和其他数据库相关的设置的位置的提供程序。 有关使用各种数据存储区和提供程序的连接字符串模式的列表，请参阅[ConnectionStrings.com](http://www.connectionstrings.com/)。


中所述[创建数据访问层](../introduction/creating-a-data-access-layer-vb.md)教程中，可以通过使用分部类扩展的类型化数据集的自动生成类。 首先，在名为的项目中创建新的子文件夹`ConnectionAndCommandSettings`下方`~/App_Code/DAL`文件夹。


![添加名为 ConnectionAndCommandSettings 的子文件夹](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image5.png)

**图 3**： 添加名为的子文件夹`ConnectionAndCommandSettings`


添加名为的新类文件`ProductsTableAdapter.ConnectionAndCommandSettings.vb`，然后输入以下代码：


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample2.vb)]

此部分的类添加`Public`属性名为`ConnectionString`到`ProductsTableAdapter`类，该类允许任何层以读取或更新基础连接的 TableAdapter s 的连接字符串。

与此分部类创建 （并保存），打开`ProductsBLL`类。 转到现有的方法之一，在键入`Adapter`然后按句点键来打开智能感知。 你应该会看到新`ConnectionString`IntelliSense，这意味着您可以以编程方式读取或调整此值从 BLL 中可用的属性。

## <a name="exposing-the-entire-connection-object"></a>公开整个连接对象

此部分的类公开基础的连接对象的属性只有一个： `ConnectionString`。 如果你想要使整个连接对象的 TableAdapter 界限更高版本，或者可以更改`Connection`属性的保护级别。 自动生成的代码在步骤 1 中，我们探讨表明，TableAdapter s`Connection`属性被标记为`Friend`，这意味着，它仅可以由同一程序集中的类。 可进行更改，但是，通过 TableAdapter 的`ConnectionModifier`属性。

打开`Northwind`数据集，单击`ProductsTableAdatper`在设计器中，并导航到属性窗口。 此时您将看到`ConnectionModifier`设置为其默认值， `Assembly`。 若要使`Connection`属性仅在类型化数据集 s 程序集更改`ConnectionModifier`属性`Public`。


[![可以通过 ConnectionModifier 属性配置连接属性 s 可访问性级别](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image6.png)

**图 4**:`Connection`属性 s 可访问性的情况下可以配置级别通过`ConnectionModifier`属性 ([单击以查看实际尺寸的图像](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image8.png))


保存的数据集，然后返回到`ProductsBLL`类。 如之前，请转到现有的方法之一，键入`Adapter`然后按句点键来打开智能感知。 此列表应包括`Connection`属性，这意味着，你现在可以以编程方式读取或从 BLL 分配任何连接级别设置。

## <a name="step-3-examining-the-command-related-properties"></a>第 3 步： 检查与命令相关的属性

TableAdapter 包含，默认情况下，具有自动生成的主查询`INSERT`， `UPDATE`，和`DELETE`语句。 此主查询 s `INSERT`， `UPDATE`，和`DELETE`语句在 TableAdapter 的代码中实现 ADO.NET 数据适配器对象通过`Adapter`属性。 与类似其`Connection`属性，`Adapter`属性的数据类型由使用的数据提供程序。 因为这些教程使用 SqlClient 提供程序，`Adapter`属性属于类型[ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx)。

TableAdapter s`Adapter`属性具有三个属性的类型`SqlCommand`，它使用到问题`INSERT`， `UPDATE`，和`DELETE`语句：

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

A`SqlCommand`对象负责将特定查询发送到数据库，并具有属性，例如： [ `CommandText` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx)，其中包含的临时 SQL 语句或存储的过程以执行; 和[ `Parameters`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx)，这是一套`SqlParameter`对象。 正如我们所看到的进来[创建数据访问层](../introduction/creating-a-data-access-layer-vb.md)教程中，这些命令可以通过属性窗口自定义对象。

除了其主查询 TableAdapter 可以包含可变数量的方法，调用时，调度到数据库指定的命令。 主查询的命令对象和所有其他方法的命令对象存储在 TableAdapter 的`CommandCollection`属性。

允许 s 花些时间查看生成的代码`ProductsTableAdapter`中`Northwind`这两个属性及其支持的成员变量和帮助器方法的数据集：


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample3.vb)]

代码`Adapter`和`CommandCollection`属性类似于的`Connection`属性。 有保留的属性所使用的对象的成员变量。 属性`Get`访问器首先检查以查看相应的成员变量是否`Nothing`。 如果是这样，在其中创建成员变量的实例并为核心分配与命令相关的属性调用初始化方法。

## <a name="step-4-exposing-command-level-settings"></a>步骤 4： 公开命令级别设置

理想情况下，命令级别信息应保留在数据访问层内封装。 应体系结构的其他层中需要此信息，但是，它可以通过公开一个分部类，就像使用连接级别设置。

由于 TableAdapter 只有单个`Connection`属性，公开连接级别设置的代码将非常简单。 因为 TableAdapter 可以有多个命令对象的修改命令级别设置时，情况就更复杂一点`InsertCommand`， `UpdateCommand`，和`DeleteCommand`，变量命令中的对象数以及`CommandCollection`属性。 在更新命令级别设置，这些设置将需要传播到所有的命令对象。

例如，假设中包括了时间特别长执行 TableAdapter 的某些查询。 当使用 TableAdapter 执行这些查询之一，我们可能需要增加命令对象 s [ `CommandTimeout`属性](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx)。 此属性指定的要等待的时间要执行的命令的秒数，默认值为 30。

若要允许`CommandTimeout`属性要按其调整 BLL，添加以下`Public`方法`ProductsDataTable`使用分部类文件在步骤 2 中创建 (`ProductsTableAdapter.ConnectionAndCommandSettings.vb`):


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample4.vb)]

无法从要设置的所有命令问题在命令超时的 BLL 或表示层调用此方法，该 TableAdapter 实例。

> [!NOTE]
> `Adapter`和`CommandCollection`属性标记为`Private`，这意味着仅从 TableAdapter 中的代码进行访问。 与不同`Connection`属性，这些访问修饰符是不可配置。 因此，如果你需要公开命令级别到体系结构中的其他层的属性必须使用提供前面讨论的分部类方法`Public`方法或属性，可读取或写入`Private`命令对象。


## <a name="summary"></a>摘要

在类型化数据集中 Tableadapter 用来封装数据访问的详细信息和复杂性。 使用 Tableadapter，我们不需要担心编写 ADO.NET 代码，以连接到数据库、 发出命令，或填充到数据表的结果。 它是所有自动处理为我们。

但是，可能有当我们需要自定义的低级别的 ADO.NET 细节，例如更改连接字符串或默认连接或命令超时值的时间。 TableAdapter 具有自动生成`Connection`， `Adapter`，和`CommandCollection`属性，但这些都是`Friend`或`Private`，默认情况下。 可以通过扩展 TableAdapter 使用分部类包含公开此内部信息`Public`方法或属性。 或者，TableAdapter s`Connection`属性访问修饰符可通过 TableAdapter s 配置`ConnectionModifier`属性。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Burnadette Leigh，S ren 异世 Lauritsen Teresa 墨和希尔顿 Geisenow。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](working-with-computed-columns-vb.md)
[下一页](protecting-connection-strings-and-other-configuration-information-vb.md)
