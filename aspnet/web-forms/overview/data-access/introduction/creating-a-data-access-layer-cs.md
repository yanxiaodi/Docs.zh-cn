---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
title: 创建数据访问层 (C#) |Microsoft 文档
author: rick-anderson
description: 在本教程中我们将从一开始启动，并创建数据访问层 (DAL)，使用类型化数据集，以访问数据库中的信息。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/05/2010
ms.topic: article
ms.assetid: cfe2a6a0-1e56-4dc8-9537-c8ec76ba96a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 927b2490b5c539a79bb9939b88942499b23cc464
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="creating-a-data-access-layer-c"></a>创建数据访问层 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_1_CS.exe)或[下载 PDF](creating-a-data-access-layer-cs/_static/datatutorial01cs1.pdf)

> 在本教程中我们将从一开始启动，并创建数据访问层 (DAL)，使用类型化数据集，以访问数据库中的信息。


## <a name="introduction"></a>介绍

为 web 开发人员，我们生活围绕使用数据。 我们将创建数据库来存储数据，来检索和修改它，和 web 页来收集和汇总它的代码。 这是将浏览技术用于在 ASP.NET 2.0 实现这些常见的模式一冗长系列中的第一个教程。 我们将开始创建[软件体系结构](http://en.wikipedia.org/wiki/Software_architecture)组合使用类型化数据集，业务逻辑层 (BLL) 数据访问层 (DAL)，它强制自定义业务规则和表示层组成 ASP.NET 的页共享通用的页面布局。 一旦此后端基础工作已排列，我们来看看到报告，显示如何显示，汇总、 收集，和验证从 web 应用程序的数据。 这些教程旨在是简洁并提供引导你完成此过程以可视方式并提供充足的屏幕截图的分步说明。 每个教程是在 C# 和 Visual Basic 版本中可用，包括使用的完整代码下载。 （此第一个教程是非常长，但其余部分将显示更多可以理解的小区块中。）

我们将放置在 Northwind 数据库的 Microsoft SQL Server 2005 Express Edition 版本使用在本系列教程**应用\_数据**目录。 除了数据库文件中，**应用\_数据**文件夹还包含用于创建数据库的 SQL 脚本，以防你想要使用不同的数据库版本。 这些脚本可能也会[直接从 Microsoft 下载](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en)，如果您希望使用。 如果你使用 Northwind 数据库的不同 SQL Server 版本，你将需要更新**NORTHWNDConnectionString**中应用程序的设置**Web.config**文件。 Web 应用程序是为文件系统基于网站项目中使用 Visual Studio 2005 专业版生成的。 但是，所有这些教程将起作用同样有效的免费版本的 Visual Studio 2005 中， [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/)。  
  
在本教程中我们将从一开始启动，并创建数据访问层 (DAL) 后, 跟在第二个教程中，创建业务逻辑层 (BLL) 和在页面布局和第三个中的导航正常工作。 在前三个中的布局后第三个将生成的基础之上的教程。 我们提供了许多介绍在此第一个教程，因此启动 Visual Studio 并让我们开始吧 ！

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>步骤 1： 创建 Web 项目，并连接到数据库

我们可以创建我们的数据访问层 (DAL) 之前，我们首先需要创建网站并设置我们的数据库。 首先，创建新文件系统基于 ASP.NET 网站。 若要完成此操作，请转到文件菜单并选择新的网站，显示新建网站对话框。 选择 ASP.NET 网站模板，将位置下拉列表设置为文件系统、 选择一个文件夹，以将 web 站点，并将语言设置为 C#。


[![创建新的文件系统基于 Web 站点](creating-a-data-access-layer-cs/_static/image2.png)](creating-a-data-access-layer-cs/_static/image1.png)

**图 1**： 创建 New File System-Based 网站 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image3.png))


这将创建新的网站与**Default.aspx** ASP.NET 页和**应用\_数据**文件夹。

与创建网站后下, 一步是在 Visual Studio 服务器资源管理器中添加对数据库的引用。 通过将数据库添加到服务器资源管理器中，你可以添加表、 存储的过程、 视图和从 Visual Studio 中的等所有。 你还可以查看表数据，或通过查询生成器手动或以图形方式创建您自己的查询。 此外，我们为 DAL 生成类型化数据集时我们将需要点 Visual Studio 连接到可从其构造类型化数据集的数据库。 虽然我们可以提供此连接信息在该点的时间，Visual Studio 将自动填充已在服务器资源管理器中注册的数据库的下拉列表。

将 Northwind 数据库添加到服务器资源管理器的步骤取决于是否想要使用中的 SQL Server 2005 Express Edition 数据库**应用\_数据**文件夹或者如果您使用 Microsoft SQL Server 2000 或 2005年你想要改为使用的数据库服务器设置。

## <a name="using-a-database-in-theappdatafolder"></a>使用数据库中 theApp\_DataFolder

如果你没有要连接到的 SQL Server 2000 或 2005年数据库服务器，或者你只想要避免无需将数据库添加到数据库服务器，则可以使用位于下载 websit Northwind 数据库的 SQL Server 2005 Express Edition 版本e's**应用\_数据**文件夹 (**northwnd 不。MDF**)。

数据库置于**应用\_数据**文件夹会自动添加到服务器资源管理器。 假设你有 SQL Server 2005 Express Edition 安装您的计算机上，你应该看到名为 northwnd 不的节点。MDF 在服务器资源管理器，你可以展开和浏览其表、 视图、 存储的过程等 （请参见图 2）。

**应用\_数据**文件夹还可以包含 Microsoft Access **.mdb**文件，如 SQL Server 的对应，会自动添加到服务器资源管理器。 如果你不想要使用的任何 SQL Server 选项，则可以始终[下载 Northwind 数据库文件的 Microsoft Access 版本](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN)拖放到**应用\_数据**目录。 请记住，但是，Access 数据库不作为功能丰富作为 SQL Server，并不旨在在网站方案中使用。 此外，几个 35 + 教程将使用不受访问某些数据库级功能。

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>连接到 Microsoft SQL Server 2000 或 2005年数据库服务器中数据库

或者，你可能会连接到 Northwind 数据库的数据库服务器上安装。 如果数据库服务器已没有安装了 Northwind 数据库，你首先必须将其添加到数据库服务器通过运行安装脚本包含在本教程的下载或通过[下载的 SQL Server 2000 版的 Northwind安装脚本](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en)直接从 Microsoft 网站上找到。

一旦安装了数据库，请转到 Visual Studio 的服务器资源管理器，右键单击数据连接节点中，然后选择添加连接。 如果看不到服务器资源管理器转到视图 / 服务器资源管理器或命中的 Ctrl + Alt + S。 这将显示添加连接对话框中，你可以在其中指定要连接到的服务器，身份验证信息，且数据库名称。 一旦你已成功配置的数据库连接信息并单击确定按钮，数据库将添加作为节点下面的数据连接节点的节点。 你可以展开数据库节点来浏览其表、 视图、 存储的过程和等等。


![添加到你的数据库服务器的 Northwind 数据库的连接](creating-a-data-access-layer-cs/_static/image4.png)

**图 2**： 添加到你的数据库服务器的 Northwind 数据库的连接


## <a name="step-2-creating-the-data-access-layer"></a>步骤 2： 创建数据访问层

在使用数据的一个选项是特定于数据的逻辑将直接嵌入到表示层 （在 web 应用中，ASP.NET 页构成了表示层）。 这可能需要编写 ASP.NET 页的代码部分中的 ADO.NET 代码或使用 SqlDataSource 控件标记部分中的形式。 在任一情况下，此方法紧密标数据访问逻辑与表示层。 但是，建议的方法，是分开的表示层的数据访问逻辑。 此单独的层称为数据访问层，简称，DAL 并通常实现为一个单独的类库项目。 也记录此分层体系结构的好处 （在本教程末尾的有关这些优点的信息请参阅"进一步读数"部分） 且在本系列中，我们将采取的方法。

所有代码都是特定于基础数据源创建连接到数据库，如颁发**选择**，**插入**，**更新**，和**删除**命令，依次类推应位于 DAL。 表示层不应包含对此类数据访问代码的任何引用，但应进行的所有数据请求 DAL 调用。 数据访问层通常包含用于访问基础数据库数据的方法。 Northwind 数据库中，例如，具有**产品**和**类别**记录用于销售和它们所属的类别的产品的表。 在我们 DAL 我们将具有类似的方法：

- **GetCategories()，**这将返回有关所有类别信息
- **GetProducts()**，这将返回有关所有产品信息
- **GetProductsByCategoryID (*categoryID*)**，这样就会返回属于指定类别的所有产品
- **GetProductByProductID (*productID*)**，这样就会返回有关特定产品的信息

这些方法，调用时，将连接到数据库、 发出相应的查询，并返回结果。 我们返回这些结果的方式非常重要。 这些方法可能只需返回的数据集或 DataReader 填充数据库查询，但理想情况下这些结果应返回使用*强类型对象*。 强类型的对象是一个在编译时，严格定义其架构而相反，松散类型化的对象，是一个直到运行时才知道其架构。

例如，DataReader 和 （默认） 数据集是松散类型化对象，因为它们的架构定义的用于填充其数据库查询返回的列。 若要从松散类型化的 DataTable 我们需要使用类似的语法访问特定的列: ***DataTable*。行[*索引*]["*columnName*"]**。 DataTable 的松散类型在此示例中表现出事实，我们需要访问使用字符串或序号索引的列名称。 强类型的数据表，另一方面，将具有每个列实现为属性，从而导致如下所示的代码： ***DataTable*。行[*索引*]。*columnName***\*。

若要返回强类型对象，开发人员可以创建自己的自定义业务对象或使用类型化数据集。 其属性通常反映基础数据库表的业务对象的列的类表示，开发人员实现业务对象。 类型化数据集是由基于数据库架构和其成员都是根据此架构强类型的 Visual Studio 为你生成一个类。 类型化数据集本身扩展 ADO.NET 数据集、 数据表和 DataRow 类的类组成。 除了强类型的数据表，类型化数据集现在还包括 Tableadapter，是使用的填充数据集的数据表和传播回数据库数据表中的修改的方法的类。

> [!NOTE]
> 有关的优点和缺点的自定义业务对象与使用类型化数据集的详细信息，请参阅[设计数据层组件和层间传递数据](https://msdn.microsoft.com/library/ms978496.aspx)。


我们将使用这些教程的体系结构相符的强类型化数据集。 图 3 说明了使用类型化数据集的应用程序的不同层之间的工作流。


[![所有数据访问代码都降级到 DAL](creating-a-data-access-layer-cs/_static/image6.png)](creating-a-data-access-layer-cs/_static/image5.png)

**图 3**： 所有数据访问代码都降级到 DAL ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>创建类型化数据集和表适配器

若要开始创建我们 DAL，我们首先将类型化数据集添加到我们的项目。 若要完成此操作，右键单击解决方案资源管理器中的项目节点并选择添加新项。 从模板列表中选择数据集选项并将其命名**Northwind.xsd**。


[![选择将新的数据集添加到你的项目](creating-a-data-access-layer-cs/_static/image9.png)](creating-a-data-access-layer-cs/_static/image8.png)

**图 4**： 选择将新的数据集添加到您的项目 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image10.png))


单击添加，当系统提示添加到的数据集后**应用\_代码**文件夹中，选择是。 将显示为类型化数据集设计器，并将启动 TableAdapter 配置向导，让你可以添加到类型化数据集的第一个 TableAdapter。

类型化数据集用作强类型集合的数据;它由构成的强类型 DataTable 实例，其中每个反过来组成强类型 DataRow 实例。 我们将为每个我们需要使用在本教程系列中的基础数据库表创建强类型的 DataTable。 让我们开始创建用于数据表**产品**表。

请记住，强类型数据表未包括上如何访问其基础数据库表中的数据的任何信息。 为了检索要填充 DataTable 的数据，我们使用一个 TableAdapter 类，该类用作我们的数据访问层。 有关我们**产品**数据表，TableAdapter 将包含方法**GetProducts()**， **GetProductByCategoryID (*categoryID*)**，依次类推我们将从表示层中调用。 DataTable 的角色是充当用于在各层之间传递数据的强类型对象。

TableAdapter 配置向导首先提示你选择要使用的数据库。 下拉列表显示在服务器资源管理器中这些数据库。 如果您不未向服务器资源管理器添加 Northwind 数据库，可以在此时若要这样做单击新建连接按钮。


[![从下拉列表中选择 Northwind 数据库](creating-a-data-access-layer-cs/_static/image12.png)](creating-a-data-access-layer-cs/_static/image11.png)

**图 5**： 从下拉列表中选择 Northwind 数据库 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image13.png))


选择数据库并单击下一步后, 你系统将要求你想要保存的连接字符串中**Web.config**文件。 通过保存连接字符串中，你可以避免具有硬编码在 TableAdapter 类中，可简化操作，如果连接字符串信息在将来发生更改。 如果选择将连接字符串保存在配置文件中将被放置在**&lt;connectionStrings&gt;**部分中，可以是[（可选） 加密](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)以改进安全或更高版本通过新的 ASP.NET 2.0 属性页内 IIS GUI 管理工具的说明进行操作，这是更适合管理员已修改。


[![将连接字符串保存到 Web.config](creating-a-data-access-layer-cs/_static/image15.png)](creating-a-data-access-layer-cs/_static/image14.png)

**图 6**： 将连接字符串保存到**Web.config** ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image16.png))


接下来，我们需要第一个的强类型 datatable 中定义的架构，并提供为我们 TableAdapter 填充强类型化数据集时要使用第一种方法。 通过创建查询返回的列从表，我们希望在我们 DataTable 中反映同时完成这两个步骤。 在向导末尾我们将对此查询提供的方法名称。 一旦完成的工作，则可以从我们表示层调用此方法。 该方法将执行定义的查询并填充强类型的 DataTable。

若要开始定义我们首先必须指示我们希望如何可以发出查询的 TableAdapter 的 SQL 查询。 我们可以使用临时 SQL 语句，创建一个新的存储的过程，或使用现有存储的过程。 对于这些教程中，我们将使用临时 SQL 语句。 请参阅[Brian Noyes](http://briannoyes.net/)的文章，[生成数据访问层与 Visual Studio 2005 数据集设计器](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)有关使用存储的过程的示例。


[![查询使用的临时 SQL 语句的数据](creating-a-data-access-layer-cs/_static/image18.png)](creating-a-data-access-layer-cs/_static/image17.png)

**图 7**： 查询使用临时 SQL 语句的数据 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image19.png))


此时我们可以键入 SQL 查询中手动。 当在 TableAdapter 中创建第一种方法通常想要让查询返回需要在相应的 DataTable 中表示这些列。 我们可以通过创建的查询返回所有列和中的所有行来完成此**产品**表：


[![在文本框中输入 SQL 查询](creating-a-data-access-layer-cs/_static/image21.png)](creating-a-data-access-layer-cs/_static/image20.png)

**图 8**： 输入 SQL 查询到文本框中 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image22.png))


或者，使用查询生成器，并以图形方式构造查询，如图 9 中所示。


[![以图形方式，通过查询编辑器中创建查询](creating-a-data-access-layer-cs/_static/image24.png)](creating-a-data-access-layer-cs/_static/image23.png)

**图 9**: 查询以图形方式，通过查询编辑器中创建 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image25.png))


创建查询之后, 再移到下一个屏幕上，单击高级选项按钮。 在网站项目中，"生成 Insert、 Update 和 Delete 语句"是唯一一种高级选项选择默认设置。如果你从类库或 Windows 项目运行此向导也将选择"使用开放式并发"选项。 现在请保留"使用开放式并发"选项未选中状态。 在将来的教程中，我们将检查开放式并发。


[![选择仅生成 Insert、 Update 和 Delete 语句选项](creating-a-data-access-layer-cs/_static/image27.png)](creating-a-data-access-layer-cs/_static/image26.png)

**图 10**： 选择仅生成 Insert、 Update 和 Delete 语句选项 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image28.png))


在验证之后的高级的选项，单击下一步转到最后一个屏幕。 此处我们将要求选择要添加到 TableAdapter 的方法。 有两种模式填充数据：

- **填充 DataTable**使用此方法创建一个方法采用作为参数数据表并填充它基于查询的结果。 ADO.NET DataAdapter 类，例如，实现此模式与其**Fill()**方法。
- **返回 DataTable**使用此方法时该方法将创建和为你填充 DataTable 并返回它作为方法返回值。

你可以实现一个或这两种模式的 TableAdapter。 你还可以重命名此处提供的方法。 即使我们将仅使用这些教程在后一种模式，让我们将检查，这两个复选框保留。 此外，让我们重命名而不是泛型**GetData**方法**GetProducts**。

如果选中，则最终的复选框，"GenerateDBDirectMethods，"创建**Insert()**， **update （)**，和**delete （)** TableAdapter 的方法。 如果选中此选项，所有更新将都需要通过 TableAdapter 的唯一**update （)**方法，后者采用在类型化数据集、 数据表、 单个数据行或返回数据行的数组。 (如果你已经图 9 中的高级属性中此复选框未选中的"生成 Insert、 Update 和 Delete 语句"选项设置不起作用。)让我们保留选中此复选框。


[![GetData 方法名称更改为 GetProducts](creating-a-data-access-layer-cs/_static/image30.png)](creating-a-data-access-layer-cs/_static/image29.png)

**图 11**： 将方法名称由**GetData**到**GetProducts** ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image31.png))


通过单击完成完成向导。 在向导关闭后我们将返回到显示 DataTable 我们刚刚创建的数据集设计器。 您可以看到中的列列表**产品**DataTable (**ProductID**， **ProductName**，依次类推)，以及的方法**ProductsTableAdapter** (**Fill()**和**GetProducts()**)。


[![已添加到类型化数据集的产品 DataTable 和 ProductsTableAdapter](creating-a-data-access-layer-cs/_static/image33.png)](creating-a-data-access-layer-cs/_static/image32.png)

**图 12**:**产品**DataTable 和**ProductsTableAdapter**已添加到类型化数据集 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image34.png))


我们现在有单个数据表的数据集类型 (**Northwind.Products**) 和一个强类型 DataAdapter 类 (**NorthwindTableAdapters.ProductsTableAdapter**) 与**GetProducts()**方法。 这些对象可以用于从类似的代码访问的所有产品的列表：

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample1.html)]

此代码不需要我们要写入的数据访问特定代码的一位。 我们不一定非要实例化任何 ADO.NET 类，我们没有来指代的任意连接字符串，SQL 查询或存储过程。 相反，TableAdapter 为我们提供低级别的数据访问代码。

此示例中使用每个对象也是强类型，允许 Visual Studio 以提供 IntelliSense 和编译时类型检查。 并且返回的 TableAdapter 的所有数据表的最佳可以绑定到 ASP.NET Web 控件，如 GridView、 说明、 DropDownList、 如何，以及多个其他数据。 下面的示例演示绑定返回 DataTable **GetProducts()**方法只需获得的三个行中的代码中一个 GridView**页\_负载**事件处理程序。

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample2.aspx)]

AllProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample3.cs)]


[![产品列表将显示在一个 GridView](creating-a-data-access-layer-cs/_static/image36.png)](creating-a-data-access-layer-cs/_static/image35.png)

**图 13**： 的产品的列表显示在一个 GridView ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image37.png))


尽管此示例要求这样做我们在我们的 ASP.NET 页中编写的代码的三个行**页\_负载**事件处理程序中，在将来我们将查看如何使用对象数据源以声明方式检索中的数据的教程DAL。 与 ObjectDataSource 我们将不需要编写任何代码，并将获取以及分页和排序支持 ！

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>步骤 3： 添加参数化的数据访问层的方法

此时我们**ProductsTableAdapter**类具有一个方法，但**GetProducts()**，它返回数据库中的所有产品。 在能够使用的所有产品是肯定有用，可以在我们将需要检索有关特定产品或属于特定类别的所有产品的信息的时间。 若要将此类功能添加到我们的数据访问层我们可以向 TableAdapter 添加参数化的方法。

让我们添加**GetProductsByCategoryID (*categoryID*)**方法。 要将新方法添加到 DAL，返回到数据集设计器中，右键单击**ProductsTableAdapter**部分，然后选择添加查询。


![右键单击 TableAdapter，然后选择添加查询](creating-a-data-access-layer-cs/_static/image38.png)

**图 14**： 的 TableAdapter 上右键单击，然后选择添加查询


我们首先提示您有关是否我们想要使用的临时 SQL 语句或新的或现有的存储的过程访问数据库。 让我们选择使用临时 SQL 语句试。 接下来，我们会要求我们要使用哪种类型的 SQL 查询。 由于我们想要返回属于指定类别的所有产品，我们希望编写**选择**语句语句可返回行。


[![选择创建可返回行的 SELECT 语句](creating-a-data-access-layer-cs/_static/image40.png)](creating-a-data-access-layer-cs/_static/image39.png)

**图 15**： 选择创建**选择**语句其返回行 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image41.png))


下一步是定义用于访问数据的 SQL 查询。 由于我们想要返回属于特定类别的产品，我将使用相同**选择**语句从**GetProducts()**，但将添加以下**其中**子句：**其中 CategoryID = @CategoryID** 。 **@CategoryID**参数到 TableAdapter 向导指示我们正在创建的方法，将需要输入的参数的相应类型 （也就是说，可以为 null 的整数）。


[![输入查询以仅返回在指定类别中的产品](creating-a-data-access-layer-cs/_static/image43.png)](creating-a-data-access-layer-cs/_static/image42.png)

**图 16**： 输入以仅返回产品指定类别中的查询 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image44.png))


最后一步中我们可以选择数据访问模式，来使用，以及自定义所生成的方法的名称。 填充模式中，让我们将名称更改为**FillByCategoryID**并返回数据表返回模式 (**获取 * X*** 方法)，让我们使用**GetProductsByCategoryID**.


[![选择的 TableAdapter 方法的名称](creating-a-data-access-layer-cs/_static/image46.png)](creating-a-data-access-layer-cs/_static/image45.png)

**图 17**： 选择 TableAdapter 方法的名称 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image47.png))


完成向导后，数据集设计器包含新的 TableAdapter 方法。


![产品可以现在查询按类别](creating-a-data-access-layer-cs/_static/image48.png)

**图 18**: 产品可以现在查询按类别


花些时间添加**GetProductByProductID (*productID*)**方法使用相同的技术。

这些参数化的查询可以直接从数据集设计器进行测试。 右键单击 TableAdapter 中的方法，然后选择预览数据。 接下来，输入要使用的参数并单击预览的值。


[![显示这些产品隶属于饮料类别](creating-a-data-access-layer-cs/_static/image50.png)](creating-a-data-access-layer-cs/_static/image49.png)

**图 19**： 显示那些产品属于饮料类别 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image51.png))


与**GetProductsByCategoryID (*categoryID*)**方法中我们 DAL，我们现在可以创建仅显示那些产品指定类别中的 ASP.NET 页。 下面的示例显示的所有产品中饮料类别中，其具有**CategoryID**为 1。

Beverages.asp

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample4.aspx)]

Beverages.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample5.cs)]


[![显示饮料类别中的这些产品](creating-a-data-access-layer-cs/_static/image53.png)](creating-a-data-access-layer-cs/_static/image52.png)

**图 20**： 显示饮料类别中的那些产品 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>步骤 4： 插入、 更新和删除数据

有两种模式通常用于插入、 更新和删除数据。 第一个模式，我将调用数据库直接模式，涉及到创建方法，调用时，问题**插入**，**更新**，或**删除**命令对一条数据库记录的数据库。 若要插入、 更新或删除的值中，此类方法中的一系列对应的标量值 （整数、 字符串、 布尔值、 Datetime，等） 通常传递。 例如，对于此模式**产品**delete 方法将采用一个整数参数的表，该值指示**ProductID** insert 方法将需要时删除的记录字符串**ProductName**即为十进制**UnitPrice**，整数**UnitsOnStock**，依次类推。


[![每个插入、 更新和删除请求发送到数据库立即](creating-a-data-access-layer-cs/_static/image56.png)](creating-a-data-access-layer-cs/_static/image55.png)

**图 21**： 每个插入、 更新和删除请求发送到数据库立即 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image57.png))


其他模式，我将引用以作为批处理更新模式，是更新整个数据集、 数据表或在一个方法调用中返回数据行的集合。 此模式与开发人员删除、 插入和修改数据表中的 Datarow，然后将这些数据行或 DataTable 传递给的更新方法。 此方法然后枚举传入 Datarow，确定是否它们已被修改、 添加或删除 (通过 DataRow 的[RowState 属性](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx)值)，因此会发出每个记录的相应数据库请求。


[![调用更新方法时，所有更改将与数据库都同步](creating-a-data-access-layer-cs/_static/image59.png)](creating-a-data-access-layer-cs/_static/image58.png)

**图 22**： 调用更新方法时，所有更改将与数据库都同步 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image60.png))


TableAdapter 默认情况下，使用批处理更新模式，但也支持 DB 直接模式。 由于我们从高级属性选择"生成 Insert、 Update 和 Delete 语句"选项，创建我们 TableAdapter 时**ProductsTableAdapter**包含**update （)**方法，它实现批处理更新模式。 具体而言，TableAdapter 包含**update （)**类型化数据集、 强类型的数据表或一个或多个数据行可传递的方法。 如果保留"GenerateDBDirectMethods"复选框选中时首次创建 TableAdapter 的 DB 直接模式将还可通过实现**Insert()**， **update （)**，和**delete （)**方法。

这两种数据修改模式使用 TableAdapter 的**InsertCommand**， **UpdateCommand**，和**DeleteCommand**属性来颁发其**插入**，**更新**，和**删除**到数据库的命令。 你可以检查和修改**InsertCommand**， **UpdateCommand**，和**DeleteCommand**由数据集设计器中的 TableAdapter 上单击，然后将属性到属性窗口中。 (请确保选择了 TableAdapter，且**ProductsTableAdapter**对象是在属性窗口中的下拉列表中选择。)


[![TableAdapter 具有 InsertCommand、 限于 UpdateCommand 和 DeleteCommand 属性](creating-a-data-access-layer-cs/_static/image62.png)](creating-a-data-access-layer-cs/_static/image61.png)

**图 23**: TableAdapter 具有**InsertCommand**， **UpdateCommand**，和**DeleteCommand**属性 ([单击以查看全屏映像](creating-a-data-access-layer-cs/_static/image63.png))


若要检查或修改任何这些数据库命令属性，请单击**CommandText**子属性，将显示查询生成器。


[![配置查询生成器中的 INSERT、 UPDATE 和 DELETE 语句](creating-a-data-access-layer-cs/_static/image65.png)](creating-a-data-access-layer-cs/_static/image64.png)

**图 24**： 配置**插入**，**更新**，和**删除**查询生成器中的语句 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image66.png))


下面的代码示例演示如何使用批处理更新模式为双精度，所有产品的数量和具有 25 个单元库存量或更低的价格：

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample6.cs)]

下面的代码演示如何使用 DB 直接模式以编程方式删除特定的产品，然后更新一个，并将一个新：

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample7.cs)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>创建自定义插入、 更新和删除方法

**Insert()**， **update （)**，和**delete （)**由数据库直接方法创建的方法可能会有点麻烦，尤其是对于包含许多列的表。 查看前面的代码示例中，没有 IntelliSense 的帮助不特别清楚什么**产品**表列将映射到每个输入参数为**update （)**和**Insert()**方法。 可能有些时候当我们只想要更新的单个列或两个，或者想自定义**Insert()**将可能的方法返回的值的新插入的记录**标识**（自动递增）字段。

若要创建这样的自定义方法，请返回到数据集设计器。 右键单击 TableAdapter，然后选择添加的查询，返回到 TableAdapter 向导。 在第二个屏幕上我们可以指示查询创建的类型。 让我们创建一个方法来添加新的产品，然后返回新添加记录的值**ProductID**。 因此，选择创建**插入**查询。


[![创建一个方法来将新行添加到产品表](creating-a-data-access-layer-cs/_static/image68.png)](creating-a-data-access-layer-cs/_static/image67.png)

**图 25**： 创建一个要向其中添加新建行方法**产品**表 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image69.png))


在下一个屏幕**InsertCommand**的**CommandText**显示。 通过添加增强此查询**选择作用域\_IDENTITY()**在查询结束时，这将返回插入到的最后一个标识值**标识**同一范围内的列。 (请参阅[技术文档](https://msdn.microsoft.com/library/ms190315.aspx)有关详细信息**作用域\_IDENTITY()**以及为何可能愿意[使用范围\_IDENTITY() 替代的 @@IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).)请确保你最终**插入**用分号的语句才能添加**选择**语句。


[![增加查询返回 scope_identity （） 值](creating-a-data-access-layer-cs/_static/image71.png)](creating-a-data-access-layer-cs/_static/image70.png)

**图 26**： 增加到返回查询**作用域\_IDENTITY()**值 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image72.png))


最后，将该新方法**InsertProduct**。


[![将新的方法名称设置为 InsertProduct](creating-a-data-access-layer-cs/_static/image74.png)](creating-a-data-access-layer-cs/_static/image73.png)

**图 27**： 将新的方法名称设置为**InsertProduct** ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image75.png))


当您返回到数据集设计器你会看到**ProductsTableAdapter**包含一个新方法， **InsertProduct**。 如果此新方法不具有每个列中的参数**产品**表，很有可能你忘记终止**插入**用分号的语句。 配置**InsertProduct**方法，并确保你有以分号分隔**插入**和**选择**语句。

默认情况下，会插入方法问题非查询方法，这意味着它们返回受影响的行数。 但是，我们希望**InsertProduct**方法以返回由查询返回，不受影响的行数的值。 若要完成此操作，调整**InsertProduct**方法的**ExecuteMode**属性**标量**。


[![将 ExecuteMode 属性更改为标量](creating-a-data-access-layer-cs/_static/image77.png)](creating-a-data-access-layer-cs/_static/image76.png)

**图 28**： 更改**ExecuteMode**属性**标量**([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image78.png))


下面的代码演示此新**InsertProduct**中操作的方法：

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample8.cs)]

## <a name="step-5-completing-the-data-access-layer"></a>步骤 5： 完成数据访问层

请注意， **ProductsTableAdapters**类返回**CategoryID**和**供应商 Id**值从**产品**表，但不包括**CategoryName**列从**类别**表或**CompanyName**列从**供应商**表，尽管这些可能是我们想要在其中显示了产品信息时显示的列。 我们可以增加 TableAdapter 的初始方法**GetProducts()**、 同时包含这两者**CategoryName**和**CompanyName**列的值，这将更新若要包括这些新的列的强类型的 DataTable。

这可以显示问题，但是，为 TableAdapter 的方法插入、 更新和删除数据基于此初始的方法。 幸运的是，自动生成的方法插入、 更新和删除将不可受中的子查询**选择**子句。 注意，要添加到我们查询通过**类别**和**供应商**为子查询，而不是**加入**s，我们将避免不得不返工这些修改数据的方法。 右键单击**GetProducts()**中的方法**ProductsTableAdapter**并选择配置。 然后，调整**选择**子句，以便其所示类似：

[!code-sql[Main](creating-a-data-access-layer-cs/samples/sample9.sql)]


[![更新的 SELECT 语句 GetProducts() 方法](creating-a-data-access-layer-cs/_static/image80.png)](creating-a-data-access-layer-cs/_static/image79.png)

**图 29**： 更新**选择**语句**GetProducts()**方法 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image81.png))


在更新后**GetProducts()**要使用此 DataTable 将包括两个新列的新查询方法： **CategoryName**和**供应商名称**。


![产品 DataTable 具有两个新列](creating-a-data-access-layer-cs/_static/image82.png)

**图 30**:**产品**DataTable 具有两个新列


花一些时间来更新**选择**中的子句**GetProductsByCategoryID (*categoryID*)**以及方法。

如果你更新**GetProducts()** **选择**使用**加入**语法数据集设计器将无法自动生成用于插入、 更新和删除方法使用数据库直接模式的数据库数据。 相反，你将需要手动创建它们更像与我们**InsertProduct**本教程中前面的方法。 此外，手动将需要提供**InsertCommand**， **UpdateCommand**，和**DeleteCommand**属性值，则如果你想要使用批更新模式。

## <a name="adding-the-remaining-tableadapters"></a>添加剩余的 Tableadapter

直到现在，我们只介绍了使用单个 TableAdapter 的单个数据库表。 但是，Northwind 数据库包含我们将需要使用我们的 web 应用程序中的多个相关的表。 类型化数据集可以包含多个相关数据表。 因此，若要完成我们 DAL，我们需要将数据表添加我们将在这些教程中使用的其他表。 若要将新的 TableAdapter 添加到类型化数据集，打开数据集设计器，在设计器中，右键单击并选择添加 / TableAdapter。 这将创建新的 DataTable 和 TableAdapter，并引导你完成本教程中前面部分中，我们探讨向导。

需要几分钟才能创建以下 Tableadapter 和使用下面的查询的方法。 请注意，中的查询**ProductsTableAdapter**包括子查询中获取每个产品类别和供应商名称。 此外，如果你已已之后，你已添加**ProductsTableAdapter**类的**GetProducts()**和**GetProductsByCategoryID (*categoryID*)**方法。

- **ProductsTableAdapter**

    - **GetProducts**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample10.sql)]
    - **GetProductsByCategoryID**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample11.sql)]
    - **GetProductsBySupplierID**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample12.sql)]
    - **GetProductByProductID**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample13.sql)]
- **CategoriesTableAdapter**

    - **GetCategories**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample14.sql)]
    - **GetCategoryByCategoryID**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample15.sql)]
- **SuppliersTableAdapter**

    - **GetSuppliers**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample16.sql)]
    - **GetSuppliersByCountry**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample17.sql)]
    - **GetSupplierBySupplierID**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample18.sql)]
- **EmployeesTableAdapter**

    - **GetEmployees**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample19.sql)]
    - **GetEmployeesByManager**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample20.sql)]
    - **GetEmployeeByEmployeeID**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample21.sql)]


[![数据集设计器后已添加四个 Tableadapter](creating-a-data-access-layer-cs/_static/image84.png)](creating-a-data-access-layer-cs/_static/image83.png)

**图 31**: 数据集设计器后四个 Tableadapter 已添加 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>将自定义代码添加到 DAL

将 Tableadapter 和数据表添加到类型化数据集表示为 XML 架构定义文件 (**Northwind.xsd**)。 你可以通过右键单击查看此架构信息**Northwind.xsd**文件位于解决方案资源管理器，并选择查看代码。


[![罗斯文的 XML 架构定义 (XSD) 文件类型化数据集](creating-a-data-access-layer-cs/_static/image87.png)](creating-a-data-access-layer-cs/_static/image86.png)

**图 32**： 罗斯文类型化数据集的 XML 架构定义 (XSD) 文件 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image88.png))


此架构信息将转换为 C# 或 Visual Basic 代码在设计时编译时或在运行时 （如果需要），此时你可以单步执行它使用调试程序。 若要查看此自动生成的代码转到的类视图和钻取到 TableAdapter 或类型化数据集类。 如果看不到类视图屏幕上，转到视图菜单，选择在这里，或按 Ctrl + Shift + C。 从类视图中，你可以看到属性、 方法和事件的类型化数据集和 TableAdapter 类。 若要查看特定方法的代码，双击类视图中的方法名称或右键单击它并选择转到定义。


![通过选择转到定义从类视图中检查自动生成的代码](creating-a-data-access-layer-cs/_static/image89.png)

**图 33**： 通过选择转到定义从类视图中检查自动生成的代码


虽然自动生成的代码可节省了很好的时间，该代码通常是非常泛型，并需要进行自定义以满足应用程序的独特需求。 扩展自动生成的代码的风险，是生成代码的工具可能会决定是时候"重新生成"，并覆盖你的自定义。 使用.NET 2.0 的新的分部类概念，很容易将类拆分到多个文件。 这使我们能够向自动生成的类中添加我们自己的方法、 属性和事件，而无需担心覆盖我们自定义项的 Visual Studio。

为了演示如何自定义 DAL，让我们添加**GetProducts()**方法**SuppliersRow**类。 **SuppliersRow**类表示中的单个记录**供应商**表; 每个供应商可以提供程序零到许多产品，因此**GetProducts()**将返回那些指定的供应商的产品。 若要完成此创建中的新类文件**应用\_代码**文件夹名为**SuppliersRow.cs**并添加以下代码：

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample22.cs)]

此分部类指示编译器，当生成**Northwind.SuppliersRow**类，以包含**GetProducts()**我们刚才定义的方法。 如果你生成你的项目，然后返回到类视图你将看到**GetProducts()**现在作为一种列出**Northwind.SuppliersRow**。


![GetProducts() 方法属于现在 Northwind.SuppliersRow 类](creating-a-data-access-layer-cs/_static/image90.png)

**图 34**: **GetProducts()**方法是现在的一部分**Northwind.SuppliersRow**类


**GetProducts()**方法现在可以用于枚举的一套产品特定供应商，如以下代码所示：

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample23.html)]

此数据也可以显示在任何 ASP。NET 的数据 Web 控件。 以下页面 GridView 控件使用两个字段：

- 显示每个供应商的名称 BoundField 和
- 包含如何控件绑定到返回的结果为 TemplateField **GetProducts()**每个供应商的方法。

我们将查看如何在将来的教程中显示此类主-详细信息报表。 现在，此示例旨在演示如何使用自定义的方法添加到**Northwind.SuppliersRow**类。

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample24.aspx)]

SuppliersAndProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample25.cs)]


[![在左侧列中，他们的产品的右侧列出供应商的公司名称](creating-a-data-access-layer-cs/_static/image92.png)](creating-a-data-access-layer-cs/_static/image91.png)

**图 35**: 供应商的公司名称列出在左侧列中，他们的产品的右侧 ([单击以查看实际尺寸的图像](creating-a-data-access-layer-cs/_static/image93.png))


## <a name="summary"></a>摘要

当生成 web 应用程序创建 DAL 应该是一个你的第一个步骤，发生之前你开始创建表示层。 使用 Visual Studio，创建类型化数据集所基于的 DAL 是完成的任务，可以在 10-15 分钟内无需编写一行代码。 下一步的教程将相互依赖此 DAL。 在[下一教程](creating-a-business-logic-layer-cs.md)我们定义大量的业务规则，请参阅如何在单独的业务逻辑层中实现它们。

尽情享受编程 ！

## <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [生成使用强类型化的 Tableadapter 和 VS 2005 和 ASP.NET 2.0 中的数据表 DAL](https://weblogs.asp.net/scottgu/435498)
- [设计数据层组件和层间传递数据](https://msdn.microsoft.com/library/ms978496.aspx)
- [生成与 Visual Studio 2005 数据集设计器的数据访问层](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [加密 ASP.NET 2.0 中的配置信息的应用程序](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [TableAdapter 概述](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [使用类型化数据集](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [使用 Visual Studio 2005 和 ASP.NET 2.0 中的强类型数据访问](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [如何扩展 TableAdapter 方法](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [从存储过程中检索标量数据](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>在本教程中包含的主题的视频培训

- [ASP.NET 应用程序中的数据访问层](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [如何手动将数据集绑定到数据网格](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [如何使用数据集和筛选器从 ASP 应用程序](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Ron 绿色、 希尔顿 Giesenow、 Dennis Patterson、 沈 Shulok、 Abel Gomez 和 Carlos Santos。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[下一篇](creating-a-business-logic-layer-cs.md)
