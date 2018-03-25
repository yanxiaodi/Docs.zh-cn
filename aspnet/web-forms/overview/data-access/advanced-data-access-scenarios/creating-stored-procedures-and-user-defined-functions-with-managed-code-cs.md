---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
title: "创建存储的过程和用户定义函数与托管代码 (C#) |Microsoft 文档"
author: rick-anderson
description: "Microsoft SQL Server 2005 与.NET 公共语言运行时允许开发人员创建数据库对象通过托管代码集成。 本教程中..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 213eea41-1ab4-4371-8b24-1a1a66c515de
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 6daa0dec764e2e9248cac97ba7d7b6df0da3208f
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-c"></a>创建存储过程和用户定义函数用托管代码 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_CS.zip)或[下载 PDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/datatutorial75cs1.pdf)

> Microsoft SQL Server 2005 与.NET 公共语言运行时允许开发人员创建数据库对象通过托管代码集成。 本教程演示如何创建托管的存储的过程和管理与你的 Visual Basic 或 C# 代码的用户定义函数。 我们还看到这些版本的 Visual Studio 如何使你能够调试此类托管的数据库对象。


## <a name="introduction"></a>介绍

如 Microsoft 的 SQL Server 2005 的数据库使用[Transact-Structured 查询语言 (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL)用于插入、 修改和检索数据。 大多数数据库系统包括用于对一系列然后可作为单个可重用单元执行的 SQL 语句进行分组构造。 存储的过程是一个示例。 另一种是*用户定义函数*(Udf)，我们将在步骤 9 中的更详细地检查的构造。

在其核心而言，SQL 旨在用于处理的数据集。 `SELECT`， `UPDATE`，和`DELETE`语句本质上是应用于相应的表中的所有记录，并仅受其`WHERE`子句。 尚未有许多，旨在用于一次处理一条记录，用于操作标量数据的语言功能。 [`CURSOR`s](http://www.sqlteam.com/item.asp?ItemID=553)允许记录一组要通过一次一个地循环。 字符串操作函数，如`LEFT`， `CHARINDEX`，和`PATINDEX`适用于标量数据。 SQL 还包括控制流语句，如`IF`和`WHILE`。

在 Microsoft SQL Server 2005 中之前, 存储的过程和 Udf 可以仅定义为一个 T-SQL 语句的集合。 SQL Server 2005 中，但是，旨在提供与集成[公共语言运行时 (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx)，这是运行时使用的所有.NET 程序集。 因此，存储的过程和 SQL Server 2005 数据库中的 Udf 可以创建使用托管的代码。 也就是说，你可以创建存储的过程或 UDF 作为 C# 类中的方法。 这样，这些存储的过程和 Udf 利用.NET Framework 中和从您自己的自定义类的功能。

在本教程中我们将讨论如何创建托管存储过程和用户定义函数以及如何将其集成到我们的 Northwind 数据库。 让我们来开始 ！

> [!NOTE]
> 托管的数据库对象提供了对 SQL 的对应的一些好处。 主要优点是语言丰富性和熟悉程度和能够重复使用现有代码和逻辑。 但托管的数据库对象，可能需要使用不涉及多过程逻辑的数据集时效率较低。 有关使用托管的代码而不是 T-SQL 的优势的更全面讨论，请参阅[优点的使用托管代码迁移至创建数据库对象](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx)。


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>步骤 1： 移动外 Northwind 数据库`App_Data`

我们的教程的所有到目前为止已使用 Microsoft SQL Server 2005 Express Edition 数据库文件中的 web 应用程序 s`App_Data`文件夹。 将数据库中的放置`App_Data`简化分发和运行这些教程的所有文件已在一个目录中的位置和所需没有要测试本教程的其他配置步骤。

但是，对于本教程，让 s 移动 Northwind 数据库外的`App_Data`和显式将其注册的 SQL Server 2005 Express Edition 数据库实例。 虽然我们可以执行的步骤对于本教程中使用该数据库`App_Data`文件夹中，多个步骤都要简单得多通过显式注册与 SQL Server 2005 Express Edition 数据库实例的数据库。

本教程中下载具有两个数据库文件-`NORTHWND.MDF`和`NORTHWND_log.LDF`-放置在名为的文件夹`DataFiles`。 如果你以及您自己的实现的教程，则请关闭 Visual Studio 和移动`NORTHWND.MDF`和`NORTHWND_log.LDF`文件从网站的`App_Data`到外部网站的文件夹的文件夹。 一旦数据库文件已被移到另一个文件夹我们需要向 Northwind 数据库的 SQL Server 2005 Express Edition 数据库实例。 这可以从 SQL Server Management Studio 完成。 如果你有非-Express Edition 的 SQL Server 2005 计算机上安装然后你可能已安装的 Management Studio。 如果你仅在计算机中安装 SQL Server 2005 Express Edition，请花些时间下载并安装[Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796)。

启动 SQL Server Management Studio。 如图 1 所示，Management Studio 启动通过要求要连接到的服务器。 Localhost\SQLExpress 输入服务器名称，在身份验证下拉列表中，选择 Windows 身份验证，然后单击连接。


![连接到相应的数据库实例](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image1.png)

**图 1**： 连接到相应的数据库实例


你已连接后，对象资源管理器窗口将列出有关 SQL Server 2005 Express Edition 数据库实例，包括数据库、 安全信息、 管理选项和等的信息。

我们需要附加 Northwind 数据库中的`DataFiles`文件夹中 （或任何位置可能移动了它） 到 SQL Server 2005 Express Edition 数据库实例。 右键单击数据库文件夹，然后从上下文菜单中选择的附加选项。 这将显示附加数据库对话框。 单击添加按钮，深化到相应`NORTHWND.MDF`文件，然后单击确定。 此时你的屏幕应类似于图 2。


[![连接到相应的数据库实例](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image2.png)

**图 2**： 连接到相应的数据库实例 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image4.png))


> [!NOTE]
> 当连接到 SQL Server 2005 Express Edition 实例通过 Management Studio 附加数据库对话框中不允许你向下钻取用户配置文件目录，如我的文档。 因此，请确保将`NORTHWND.MDF`和`NORTHWND_log.LDF`非用户配置文件目录中的文件。


单击确定按钮，附加该数据库。 附加数据库对话框将关闭，并且对象资源管理器现在应列出刚才附加数据库。 很有的 Northwind 数据库具有的名称，例如`9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`。 重命名该数据库，在数据库上右键单击并选择重命名，到 Northwind。


![将数据库重命名为 Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image5.png)

**图 3**： 将数据库重命名为 Northwind


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>步骤 2： 在 Visual Studio 中创建一个新解决方案和 SQL Server 项目

若要在 SQL Server 2005 中创建托管的存储的过程或 Udf 我们将为类中的 C# 代码中编写存储的过程和 UDF 逻辑。 一旦已编写代码，我们将需要将该类编译到程序集 (`.dll`文件) 与 SQL Server 数据库中注册程序集，然后指向中的相应方法的数据库中创建的存储的过程或 UDF 对象程序集。 执行这些步骤可以所有手动。 我们可以在任何文本编辑器创建的代码，从命令行使用 C# 编译器对其进行编译 ([`csc.exe`](https://msdn.microsoft.com/library/ms379563(vs.80).aspx))，数据库使用为其注册[ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/library/ms189524.aspx)命令或从管理Studio 中，并添加的存储的过程或通过类似方式的 UDF 对象。 幸运的是，Visual Studio 的专业人员和团队系统版本包含可以自动执行这些任务的 SQL Server 项目类型。 在本教程中，我们将演练使用 SQL Server 项目类型创建托管的存储的过程和 UDF。

> [!NOTE]
> 如果你使用 Visual Web Developer 或 Visual Studio Standard edition，你将需要改为使用手动方法。 步骤 13 提供手动执行这些步骤的详细的说明。 我建议你在阅读步骤 13，因为这些步骤包括重要必须应用无论使用哪个版本的 Visual Studio 的 SQL Server 配置说明之前读取到 12 的步骤 2。


首先打开 Visual Studio。 从文件菜单中选择新项目以显示新项目对话框框中 （请参见图 4）。 深化到数据库项目类型，然后，从右侧列出的模板，选择要创建新的 SQL Server 项目。 我已选择此项目`ManagedDatabaseConstructs`和放置在名为的解决方案内`Tutorial75`。


[![创建新的 SQL Server 项目](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image6.png)

**图 4**： 创建新的 SQL Server 项目 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image8.png))


单击新建项目对话框中，创建解决方案和 SQL Server 项目中的确定按钮。

SQL Server 项目将绑定到特定数据库。 因此，在创建新的 SQL Server 项目之后我们将立即要求指定此信息。 图 5 所示的新建数据库引用对话框中已填写以指向 Northwind 数据库，我们在步骤 1 中返回的 SQL Server 2005 Express Edition 数据库实例中注册。


![将 SQL Server 项目与 Northwind 数据库相关联](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image9.png)

**图 5**： 将 SQL Server 项目与 Northwind 数据库相关联


若要调试的托管存储的过程和我们将在此项目中创建的 Udf，我们需要启用 SQL/CLR 调试连接的支持。 只要将 SQL Server 项目将与新的数据库相关联 （如我们在图 5 所做的那样），Visual Studio 会要求我们是否我们想要启用 SQL/CLR 调试连接上 （请参阅图 6）。 单击是。


![启用 SQL/CLR 调试](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image10.png)

**图 6**： 启用 SQL/CLR 调试


此时新的 SQL Server 项目具有已添加到解决方案。 它包含名为的文件夹`Test Scripts`与名为的文件`Test.sql`，后者用于调试项目中创建的托管的数据库对象。 我们将了解在步骤 12 中进行调试。

我们现在可以向此项目，添加新的托管存储的过程和 Udf 但 s 我们不要让之前解决方案中的现有 web 应用程序首次包括。 从文件菜单选择添加选项，然后选择现有网站。 浏览到相应的网站文件夹并单击确定。 如图 7 所示，这将更新以包括两个项目的解决方案： 网站和`ManagedDatabaseConstructs`SQL Server 项目。


![解决方案资源管理器现在包括两个项目](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image11.png)

**图 7**： 解决方案资源管理器现在包括两个项目


`NORTHWNDConnectionString`中的值`Web.config`当前引用`NORTHWND.MDF`文件中`App_Data`文件夹。 由于已删除此数据库从`App_Data`并显式注册了此在 SQL Server 2005 Express Edition 数据库实例中，我们需要相应地更新`NORTHWNDConnectionString`值。 打开`Web.config`文件中的网站和更改`NORTHWNDConnectionString`值，以使连接字符串中读取： `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`。 此更改后，你`<connectionStrings>`主题中`Web.config`外观应与以下类似：


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample1.xml)]

> [!NOTE]
> 中所述[前面教程](debugging-stored-procedures-cs.md)，当调试一个 SQL Server 对象从客户端应用程序，例如 ASP.NET 网站，我们需要禁用连接池。 上面显示的连接字符串禁用连接池 ( `Pooling=false` )。 如果你不计划从 ASP.NET 网站中调试托管的存储的过程和 Udf，启用连接池。


## <a name="step-3-creating-a-managed-stored-procedure"></a>步骤 3： 创建托管存储过程

若要将托管的存储的过程添加到 Northwind 数据库我们首先需要创建存储的过程作为 SQL Server 项目中的方法。 从解决方案资源管理器，右键单击`ManagedDatabaseConstructs`项目名称，选择要添加新项。 这将显示添加新项对话框中，列出了可以添加到项目的托管的数据库对象的类型。 如图 8 所示，这包括的其他存储的过程和用户定义函数。

让我们来开始添加只需返回所有已停用的产品的存储的过程。 将新的存储的过程文件命名`GetDiscontinuedProducts.cs`。


[![添加新的存储的过程名为 GetDiscontinuedProducts.cs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image12.png)

**图 8**： 添加新存储过程名为`GetDiscontinuedProducts.cs`([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image14.png))


这将创建一个新的 C# 类文件具有以下内容：


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample2.cs)]

请注意，存储的过程实现为`static`方法内的`partial`名为的类文件`StoredProcedures`。 此外，`GetDiscontinuedProducts`方法用修饰`SqlProcedure attribute`，这会将与存储过程的方法。

下面的代码创建`SqlCommand`对象并设置其`CommandText`到`SELECT`返回所有中的列的查询`Products`的产品表其`Discontinued`字段等于 1。 然后，它将执行该命令并将结果发送回客户端应用程序。 添加此代码与`GetDiscontinuedProducts`方法。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample3.cs)]

所有托管的数据库对象都有权访问[`SqlContext`对象](https://msdn.microsoft.com/library/ms131108.aspx)，它表示调用方的上下文。 `SqlContext`提供对访问[`SqlPipe`对象](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx)通过其[`Pipe`属性](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx)。 这`SqlPipe`对象用于 SQL Server 数据库和调用应用程序之间 ferry 信息。 顾名思义， [ `ExecuteAndSend`方法](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx)执行传入的`SqlCommand`对象，并将结果返回到客户端应用程序。

> [!NOTE]
> 托管的数据库对象是最适合存储的过程和使用过程逻辑，而不是基于集的逻辑的 Udf。 过程逻辑涉及在每行的行上使用的数据集或使用标量数据。 `GetDiscontinuedProducts`方法我们刚刚创建，但是，涉及没有过程的逻辑。 因此，它将理想情况下作为实现 T-SQL 存储过程。 它被实现为托管的存储的过程，以演示用于创建和部署所必需的步骤托管存储的过程。


## <a name="step-4-deploying-the-managed-stored-procedure"></a>步骤 4： 部署的托管存储过程

此代码已完成，我们现在可以将其部署到 Northwind 数据库。 部署 SQL Server 项目将代码编译到程序集、 程序集注册到数据库中，和在数据库中，将它们链接到程序集中的适当方法创建相应的对象。 更确切地说第 13 步拼任务执行的部署选项的准确集合。 右键单击`ManagedDatabaseConstructs`项目在解决方案资源管理器的名称，选择部署选项。 但是，部署将失败并出现以下错误: 外部附近有语法错误。 你可能需要将当前数据库的兼容性级别设置为更高版本的值，以启用此功能。 请参阅为存储过程的帮助`sp_dbcmptlevel`。

尝试与 Northwind 数据库中注册程序集时，将出现此错误消息。 若要向 SQL Server 2005 数据库中注册程序集，数据库 s 兼容性级别必须设置为 90。 默认情况下，新的 SQL Server 2005 数据库具有的兼容级别为 90。 但是，使用 Microsoft SQL Server 2000 创建的数据库具有的默认兼容级别为 80。 由于 Northwind 数据库最初是 Microsoft SQL Server 2000 数据库，其兼容级别将当前设置为 80，因此需要增加为 90 以便注册托管的数据库对象。

若要更新数据库 s 兼容性级别，在 Management Studio 中打开新查询窗口并输入：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample4.sql)]

单击工具栏运行上述查询中的执行图标。


[![更新 Northwind 数据库 s 兼容性级别](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image15.png)

**图 9**： 更新 Northwind 数据库 s 兼容性级别 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image17.png))


在更新后的兼容性级别，重新部署 SQL Server 项目。 这一次部署应完成而未出现错误。

返回到 SQL Server Management Studio，右键单击的 Northwind 数据库在对象资源管理器，然后选择刷新。 接下来，向下钻取可编程性文件夹，然后展开程序集文件夹。 如图 10 显示，Northwind 数据库现在包含生成的程序集`ManagedDatabaseConstructs`项目。


![ManagedDatabaseConstructs 程序集是现在注册与 Northwind 数据库](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image18.png)

**图 10**:`ManagedDatabaseConstructs`程序集是现在注册与 Northwind 数据库


此外展开的存储过程文件夹。 此时您将看到名为的存储的过程`GetDiscontinuedProducts`。 此存储的过程由部署过程并指向`GetDiscontinuedProducts`中的方法`ManagedDatabaseConstructs`程序集。 当`GetDiscontinuedProducts`执行存储的过程，它，反过来，执行`GetDiscontinuedProducts`方法。 由于这是托管的存储的过程不能编辑它通过 Management Studio (因此存储的过程名称旁边的锁定图标)。


![在存储过程文件夹中列出 GetDiscontinuedProducts 存储过程](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image19.png)

**图 11**:`GetDiscontinuedProducts`在存储过程文件夹中列出的存储过程


还有一个更多的障碍我们必须克服之前我们就可以调用托管存储的过程： 数据库被配置为阻止托管代码的执行。 来验证这一点打开新查询窗口并执行`GetDiscontinuedProducts`存储过程。 你将收到以下错误消息： 已禁用的.NET Framework 中的用户代码的执行。 启用 clr 启用配置选项。

若要检查的 Northwind 数据库的配置信息、 输入和执行命令`exec sp_configure`在查询窗口中。 这将显示 clr 启用设置当前设置为 0。


[![启用 clr 设置是当前设置为 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image20.png)

**图 12**： 启用 clr 设置是当前设置为 0 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image22.png))


请注意，在图 12 中的每个配置设置有一起列出的四个值： 所需的最低和最大值的配置和运行的值。 若要更新的 clr 启用设置的配置值，请执行以下命令：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample5.sql)]

如果你重新运行`exec sp_configure`你将看到，则上面的语句更新 clr 启用设置的配置值为 1，但仍将运行的值设置为 0。 对于此配置更改生效我们需要执行[`RECONFIGURE`命令](https://msdn.microsoft.com/library/ms176069.aspx)，它将设置为当前的配置值的运行的值。 只需输入`RECONFIGURE`在查询窗口中，单击工具栏中的执行图标。 如果你运行`exec sp_configure`现在，您应看到值 1 表示启用 clr 设置的配置，运行值。

启用的 clr 配置完成后，我们已准备好运行的托管`GetDiscontinuedProducts`存储过程。 在查询窗口中，输入，并执行命令`exec` `GetDiscontinuedProducts`。 调用存储的过程会导致中相应的托管的代码`GetDiscontinuedProducts`要执行的方法。 此代码会发出`SELECT`查询以返回已停产并将此数据返回到调用应用程序，它是此实例中的 SQL Server Management Studio 的所有产品。 Management Studio 接收这些结果，并将它们显示在结果窗口。


[![存储的过程返回所有 GetDiscontinuedProducts 停止使用的产品](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image23.png)

**图 13**:`GetDiscontinuedProducts`存储过程返回所有停止使用产品 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image25.png))


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>步骤 5： 创建托管存储过程接受输入的参数

许多查询和存储的过程，我们已创建了整个这些教程使用*参数*。 例如，在[创建新的存储过程类型化数据集 s Tableadapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)我们创建了一个名为的存储的过程的教程`GetProductsByCategoryID`接受输入的参数名为`@CategoryID`。 存储的过程然后返回所有产品其`CategoryID`字段与所提供的值匹配`@CategoryID`参数。

若要创建接受输入的参数的托管存储的过程，只需 s 方法定义中指定这些参数。 若要说明这一点，让 s，将添加到另一个托管存储的过程`ManagedDatabaseConstructs`项目名为`GetProductsWithPriceLessThan`。 此托管的存储的过程将接受输入的参数来指定价格，并且将返回所有产品其`UnitPrice`字段小于参数的值。

若要将新的存储的过程添加到项目中，右键单击`ManagedDatabaseConstructs`项目名称，选择要添加新的存储的过程。 为 `GetProductsWithPriceLessThan.cs` 文件命名。 正如我们在步骤 3 中看到的这将创建一个新的 C# 类文件具有一个名为方法`GetProductsWithPriceLessThan`放在`partial`类`StoredProcedures`。

更新`GetProductsWithPriceLessThan`方法的定义以便它可以接受[ `SqlMoney` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx)输入的参数命名为`price`并编写代码来执行并返回查询结果：


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample6.cs)]

`GetProductsWithPriceLessThan`方法的定义和代码十分类似于的定义和代码`GetDiscontinuedProducts`在步骤 3 中创建的方法。 唯一不同之处在于，`GetProductsWithPriceLessThan`方法接受作为输入参数 (`price`)，则`SqlCommand`s 查询包含一个参数 (`@MaxPrice`)，和一个参数添加到`SqlCommand`s`Parameters`集合是和分配的值为`price`变量。

添加此代码后，重新部署 SQL Server 项目。 接下来，返回到 SQL Server Management Studio 并刷新该存储过程文件夹。 你应看到一个新项， `GetProductsWithPriceLessThan`。 从查询窗口中，输入并执行该命令`exec GetProductsWithPriceLessThan 25`，后者将列表小于 25 美元，所有产品，如图 14 所示。


[![显示产品下 25 美元](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image26.png)

**图 14**： 显示产品下 25 美元 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image28.png))


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>步骤 6： 从数据访问层调用托管存储的过程

我们还增加了此时`GetDiscontinuedProducts`和`GetProductsWithPriceLessThan`托管存储的过程来`ManagedDatabaseConstructs`项目，然后注册它们与 Northwind SQL Server 数据库。 我们也会调用 SQL Server Management Studio 从这些托管存储的过程 （请参见图 13 和 14 的 s）。 为了让我们 ASP.NET 应用程序以使用这些托管存储的过程，但是，我们需要将它们添加到数据访问层和业务逻辑层体系结构中。 在此步骤中，我们将添加到两个新方法`ProductsTableAdapter`中`NorthwindWithSprocs`类型化数据集，这在最初创建[创建新的存储过程类型化数据集 s Tableadapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)教程。 在步骤 7 中我们将为 BLL 添加相应的方法。

打开`NorthwindWithSprocs`类型化数据集在 Visual Studio 和通过添加一个新方法开始`ProductsTableAdapter`名为`GetDiscontinuedProducts`。 若要将新方法添加到 TableAdapter，右键单击设计器中的 TableAdapter s 名称，并从上下文菜单中选择添加查询选项。

> [!NOTE]
> 由于我们将从 Northwind 数据库移`App_Data`到 SQL Server 2005 Express Edition 数据库实例的文件夹，则必须在 Web.config 中的相应连接字符串进行更新以反映此更改。 在步骤 2 中，我们讨论更新`NORTHWNDConnectionString`中的值`Web.config`。 如果你忘记进行此更新，你将看到错误消息 Failed 添加查询。 找不到连接`NORTHWNDConnectionString`对象`Web.config`在对话框中时试图将一个新方法添加到 TableAdapter。 若要解决此错误，单击确定，然后进入`Web.config`和更新`NORTHWNDConnectionString`值在步骤 2 中所述。 然后尝试重新将方法添加到 TableAdapter。 这次它应运行正常。


添加新方法将启动 TableAdapter 查询配置向导中，我们已在过去的教程中多次使用。 第一步让我们指定 TableAdapter 访问数据库的方式： 通过临时 SQL 语句或通过新的或现有的存储过程。 由于我们已经创建并注册`GetDiscontinuedProducts`托管的存储的过程，与数据库中，选择使用现有存储过程选项，并按下一步。


[![选择使用现有存储的过程选项](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image29.png)

**图 15**： 选择使用现有存储过程选项 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image31.png))


下一个屏幕提示我们该方法将调用存储过程。 选择`GetDiscontinuedProducts`下拉列表从管理存储的过程，并按下一步。


[![选择 GetDiscontinuedProducts 托管存储的过程](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image32.png)

**图 16**： 选择`GetDiscontinuedProducts`托管存储过程 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image34.png))


我们然后系统要求来指定该存储的过程返回行、 单个值，或执行任何操作。 由于`GetDiscontinuedProducts`返回集的断货的产品行中，选择第一个选项 （表格数据），然后单击下一步。


[![选择表格数据选项](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image35.png)

**图 17**： 选择表格的数据选项 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image37.png))


最后一个向导屏幕可让我们指定使用的数据访问模式和生成的方法的名称。 保留选中的复选框和名称方法`FillByDiscontinued`和`GetDiscontinuedProducts`。 单击完成以完成向导。


[![名称方法 FillByDiscontinued 和 GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image38.png)

**图 18**： 命名方法`FillByDiscontinued`和`GetDiscontinuedProducts`([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image40.png))


重复上述步骤以创建名为的方法`FillByPriceLessThan`和`GetProductsWithPriceLessThan`中`ProductsTableAdapter`为`GetProductsWithPriceLessThan`托管存储的过程。

图 19 显示添加到方法后的数据集设计器的屏幕截图`ProductsTableAdapter`为`GetDiscontinuedProducts`和`GetProductsWithPriceLessThan`托管存储的过程。


[![ProductsTableAdapter 包括在此步骤中添加的新方法](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image41.png)

**图 19**:`ProductsTableAdapter`包括在此步骤中的新方法添加 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image43.png))


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>步骤 7： 将相应的方法添加到业务逻辑层

现在，我们已更新数据访问层，以便包括添加在步骤 4 和 5 中的托管存储的过程中调用方法，我们需要将相应的方法添加到业务逻辑层。 添加到以下两种方法`ProductsBLLWithSprocs`类：


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample7.cs)]

这两种方法是调用相应的 DAL 方法，并返回`ProductsDataTable`实例。 `DataObjectMethodAttribute`上面每个方法的标记会导致在 ObjectDataSource s 配置数据源向导选择选项卡上的下拉列表中包括这些方法。

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>步骤 8： 调用的托管存储过程与表示层

使用业务逻辑和数据访问层扩充用以支持调用`GetDiscontinuedProducts`和`GetProductsWithPriceLessThan`托管存储的过程，我们现在可以显示这些存储过程结果通过 ASP.NET 页。

打开`ManagedFunctionsAndSprocs.aspx`页面`AdvancedDAL`文件夹并从工具箱中，将一个 GridView 拖到设计器。 设置 GridView s`ID`属性`DiscontinuedProducts`和从其智能标记，请将其绑定到名为新 ObjectDataSource `DiscontinuedProductsDataSource`。 配置对象数据源将从其数据拉`ProductsBLLWithSprocs`类的`GetDiscontinuedProducts`方法。


[![配置对象数据源以使用 ProductsBLLWithSprocs 类](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image44.png)

**图 20**： 配置使用 ObjectDataSource`ProductsBLLWithSprocs`类 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image46.png))


[![从下拉列表中选择的选项卡上选择 GetDiscontinuedProducts 方法](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image47.png)

**图 21**： 选择`GetDiscontinuedProducts`从下拉列表中选择的选项卡上的方法 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image49.png))


由于此网格将使用为仅显示产品的信息，在更新中，INSERT、 设置的下拉列表并删除选项卡添加到 （无），然后单击完成。

完成向导之后，Visual Studio 将自动添加 BoundField 或 CheckBoxField 对于每个数据字段中`ProductsDataTable`。 花一些时间来删除所有除这些字段`ProductName`和`Discontinued`速率和点，你 GridView ObjectDataSource s 声明性标记的外观应与以下类似：


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample8.aspx)]

花些时间查看通过浏览器的此页。 访问页时，ObjectDataSource 调用`ProductsBLLWithSprocs`类的`GetDiscontinuedProducts`方法。 正如我们看到在步骤 7 中，此方法调用 DAL s`ProductsDataTable`类 s`GetDiscontinuedProducts`方法，调用`GetDiscontinuedProducts`存储过程。 此存储的过程是托管的存储过程并执行我们在步骤 3 中，返回停产的产品中创建的代码。

托管的存储过程返回的结果被打包成`ProductsDataTable`dal，然后返回到 BLL，然后将它们返回到其中它们是绑定到 GridView，显示表示层。 按预期方式运行网格中列出已停用这些产品。


[![此外，列出停止使用产品](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image50.png)

**图 22**： 列出停止使用的产品 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image52.png))


进一步练习中，向页面添加一个文本框和另一个 GridView。 具有显示产品小于金额输入到文本框中，通过调用此 GridView`ProductsBLLWithSprocs`类的`GetProductsWithPriceLessThan`方法。

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>步骤 9： 创建和调用 T-SQL 的 Udf

用户定义函数或 Udf，是数据库对象的编程语言的函数的语义逼真地模仿。 在 C# 中的函数，如 Udf 可以包括变量的多个输入参数和返回特定类型的值。 UDF 可以返回标量数据-字符串、 整数和等的或表格数据。 让我们快速了解在两种类型的 Udf，开头返回一个标量数据类型的 UDF 的 s。

以下 UDF 计算的特定产品的清单的估计的值。 它会在三个输入参数-拍摄`UnitPrice`， `UnitsInStock`，和`Discontinued`特定产品的值，并返回类型的值`money`。 它通过将乘以计算的清单的估计的值`UnitPrice`通过`UnitsInStock`。 对于不再使用的项，此值被减半。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample9.sql)]

一旦此 UDF 已添加到数据库，它可以通过展开可编程性文件夹，然后函数，然后的标量值函数找到通过 Management Studio。 可在`SELECT`查询如下所示：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample10.sql)]

我已将添加`udf_ComputeInventoryValue`到 Northwind 数据库中; UDF图 23 显示的上述输出`SELECT`查询通过 Management Studio 查看时。 另请注意在对象资源管理器的标量值函数文件夹下列出了 UDF。


[![列出每个产品的清单值](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image53.png)

**图 23**： 列出清单值的每个产品 s ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image55.png))


Udf 还可以返回表格数据。 例如，我们可以创建返回属于特定类别的产品的 UDF:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample11.sql)]

`udf_GetProductsByCategoryID` UDF 接受`@CategoryID`输入的参数和返回指定的结果`SELECT`查询。 创建后，可以在中引用此 UDF `FROM` (或`JOIN`) 子句`SELECT`查询。 下面的示例将返回`ProductID`， `ProductName`，和`CategoryID`饮料的每个值。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample12.sql)]

我已将添加`udf_GetProductsByCategoryID`到 Northwind 数据库中; UDF图 24 显示的上述输出`SELECT`查询通过 Management Studio 查看时。 在对象资源管理器的表值函数文件夹中找不到返回表格格式数据的 Udf。


[![每个饮料列出 ProductID、 产品名称和类别 id](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image56.png)

**图 24**: `ProductID`， `ProductName`，和`CategoryID`为每个饮料列出 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image58.png))


> [!NOTE]
> 有关创建和使用 Udf 的详细信息，请参阅[简介用户定义函数](http://www.sqlteam.com/item.asp?ItemID=1955)。 同时查看[优点和 Drawbacks of User-Defined 函数](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)。


## <a name="step-10-creating-a-managed-udf"></a>步骤 10： 创建托管的 UDF

`udf_ComputeInventoryValue`和`udf_GetProductsByCategoryID`在上述示例中创建的 Udf 是 T-SQL 的数据库对象。 SQL Server 2005 还支持托管的 Udf，可以添加到`ManagedDatabaseConstructs`项目一样的托管存储过程从步骤 3 和 5。 对于此步骤，让我们来实现`udf_ComputeInventoryValue`在托管代码中的 UDF。

若要添加到托管的 UDF`ManagedDatabaseConstructs`项目中，右键单击解决方案资源管理器中的项目名称，选择要添加新项。 从添加新项对话框中选择的用户定义模板并将新的 UDF 文件`udf_ComputeInventoryValue_Managed.cs`。


[![将新的托管的 UDF 添加到 ManagedDatabaseConstructs 项目](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image59.png)

**图 25**： 添加新的托管 UDF 到`ManagedDatabaseConstructs`项目 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image61.png))


用户定义函数模板创建`partial`类名为`UserDefinedFunctions`使用其名称为的类文件的名称相同的方法 (`udf_ComputeInventoryValue_Managed`，在这种情况)。 此方法进行了修饰使用[`SqlFunction`属性](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx)，其标记为托管 UDF 的方法。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample13.cs)]

`udf_ComputeInventoryValue`方法当前返回[`SqlString`对象](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx)并且不接受任何输入参数。 我们需要更新方法定义，使它接受三个输入参数- `UnitPrice`， `UnitsInStock`，和`Discontinued`-并返回`SqlMoney`对象。 计算库存值的逻辑是等同于 T-SQL 的`udf_ComputeInventoryValue`UDF。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample14.cs)]

请注意，UDF 方法 s 输入的参数为其相应的 SQL 类型：`SqlMoney`为`UnitPrice`字段， [ `SqlInt16` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx)为`UnitsInStock`，和[ `SqlBoolean` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx)有关`Discontinued`。 这些数据类型反映中定义的类型`Products`表：`UnitPrice`列的类型是`money`、`UnitsInStock`类型的列`smallint`，和`Discontinued`类型的列`bit`。

代码首先创建一个`SqlMoney`实例名为`inventoryValue`分配值为 0。 `Products`表允许数据库`NULL`中值`UnitsInPrice`和`UnitsInStock`列。 因此，我们需要先检查以查看是否这些值包含`NULL`s，我们通过执行操作`SqlMoney`对象 s [ `IsNull`属性](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx)。 如果这两个`UnitPrice`和`UnitsInStock`包含非`NULL`值，则我们计算`inventoryValue`为这两个产品。 然后，如果`Discontinued`为 true，则我们减半值。

> [!NOTE]
> `SqlMoney`对象仅允许两个`SqlMoney`实例一起相乘。 它不允许`SqlMoney`实例乘以文本的浮点数。 因此，若要减半`inventoryValue`我们由一个新乘`SqlMoney`具有值 0.5 的实例。


## <a name="step-11-deploying-the-managed-udf"></a>步骤 11： 部署托管的 UDF

既然已创建托管的 UDF，我们就可以将其部署到 Northwind 数据库。 正如我们看到在步骤 4 中，通过右键单击解决方案资源管理器中的项目名称，并从上下文菜单中选择部署选项部署 SQL Server 项目中的托管的对象。

一次您的项目中，返回到 SQL Server Management Studio 部署并刷新标量值函数文件夹。 你现在应看到两个条目：

- `dbo.udf_ComputeInventoryValue`-在步骤 9 中创建 T-SQL 的 UDF 和
- `dbo.udf ComputeInventoryValue_Managed`-在刚才部署的步骤 10 中创建托管的 UDF。

若要测试此托管的 UDF，请执行以下查询从 Management Studio 中：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample15.sql)]

此命令使用的托管`udf ComputeInventoryValue_Managed`而不是 T-SQL 的 UDF `udf_ComputeInventoryValue` UDF，但输出是相同。 回头参考图 23，若要查看的 UDF 的输出屏幕快照。

## <a name="step-12-debugging-the-managed-database-objects"></a>步骤 12： 调试托管的数据库对象

在[调试存储过程](debugging-stored-procedures-cs.md)教程，我们讨论了这三个选项用于调试通过 Visual Studio 的 SQL Server： 直接数据库调试、 应用程序调试和从 SQL Server 项目的调试。 管理的数据库对象中不能通过直接数据库调试，对其进行调试，但可以调试，从客户端应用程序并直接从 SQL Server 项目。 为了使调试的正常工作，但是，SQL Server 2005 数据库必须允许 SQL/CLR 调试。 回想一下，我们首次创建时`ManagedDatabaseConstructs`Visual Studio 要求我们是否我们想要启用 SQL/CLR 调试 （在步骤 2 中的看到图 6） 的项目。 通过右键单击服务器资源管理器窗口中的数据库，可以修改此设置。


![确保数据库允许 SQL/CLR 调试](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image62.png)

**图 26**： 确保数据库允许 SQL/CLR 调试


假设我们想要调试`GetProductsWithPriceLessThan`托管存储的过程。 我们将开始设置断点的代码中`GetProductsWithPriceLessThan`方法。


[![GetProductsWithPriceLessThan 方法中设置断点](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image63.png)

**图 27**： 中设置断点`GetProductsWithPriceLessThan`方法 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image65.png))


让我们来首先看一下调试 SQL Server 项目中的托管的数据库对象。 由于我们的解决方案包括两个项目-`ManagedDatabaseConstructs`以及我们的网站-若要调试从 SQL Server 项目，我们需要指示 Visual Studio 启动 SQL Server 项目`ManagedDatabaseConstructs`我们开始调试时的 SQL Server 项目。 右键单击`ManagedDatabaseConstructs`项目在解决方案资源管理器中，作为启动项目选项，从上下文菜单中选择集。

当`ManagedDatabaseConstructs`项目启动从其执行中的 SQL 语句调试器`Test.sql`文件，位于`Test Scripts`文件夹。 例如，若要测试`GetProductsWithPriceLessThan`托管存储的过程，将现有`Test.sql`文件内容的规则与下面的语句时，将调用`GetProductsWithPriceLessThan`管理传入的存储的过程`@CategoryID`14.95 值：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample16.sql)]

你已输入到上述脚本后`Test.sql`、 启动调试通过转到调试菜单并选择开始调试，或通过点击 F5 或绿色工具栏中播放图标。 这将生成解决方案中的项目、 包含到 Northwind 数据库中，部署托管的数据库对象，然后执行`Test.sql`脚本。 此时，将会命中断点，并且我们可以单步执行`GetProductsWithPriceLessThan`方法，检查的输入参数的值，依次类推。


[![点击 GetProductsWithPriceLessThan 方法中的断点](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image66.png)

**图 28**： 中的断点`GetProductsWithPriceLessThan`点击方法 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image68.png))


为了使调试通过客户端应用程序的 SQL 数据库对象，则必须将数据库配置为支持应用程序调试。 右键单击服务器资源管理器中的数据库，并确保选中了应用程序调试选项。 此外，我们需要配置 ASP.NET 应用程序以将与 SQL 调试器集成和禁用连接池。 在步骤 2 中的详细讨论这些步骤已[调试存储过程](debugging-stored-procedures-cs.md)教程。

配置 ASP.NET 应用程序和数据库后，将 ASP.NET 网站设置为启动项目并启动调试。 如果访问调用具有断点的托管对象之一的页时，应用程序，则将暂停，并且控件将被移交给调试器，其中你可以逐句通过代码图 28 中所示。

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>步骤 13： 手动编译和部署托管数据库对象

SQL Server 项目更加轻松地创建、 编译和部署托管的数据库对象。 遗憾的是，SQL Server 项目才专业人员和团队系统版本的 Visual Studio 中可用。 如果你在使用 Visual Web Developer 或标准版的 Visual Studio，并且想要使用托管的数据库对象，你将需要手动创建和部署它们。 这包括四个步骤：

1. 创建一个包含托管的数据库对象的源代码文件
2. 将对象编译为程序集
3. 与 SQL Server 2005 数据库中注册程序集和
4. 在程序集中相应的方法所指向的 SQL Server 中创建的数据库对象。

若要阐释了这些任务，让我们来创建一个新管理返回这些产品的存储的过程其`UnitPrice`大于指定值。 创建名为你计算机上的新文件`GetProductsWithPriceGreaterThan.cs`和在文件中输入下面的代码 （你可以使用 Visual Studio、 记事本或任何文本编辑器来实现此目的）：


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample17.cs)]

此代码在几乎等同于的`GetProductsWithPriceLessThan`步骤 5 中创建的方法。 仅有的差别是方法名称`WHERE`子句，并且在查询中使用的参数名称。 返回`GetProductsWithPriceLessThan`方法，`WHERE`子句读取： `WHERE UnitPrice < @MaxPrice`。 此处，请在`GetProductsWithPriceGreaterThan`，我们使用： `WHERE UnitPrice > @MinPrice` 。

现在，我们需要将该类编译到程序集。 从命令行中，导航到保存你的目录`GetProductsWithPriceGreaterThan.cs`文件中并使用 C# 编译器 (`csc.exe`) 若要将类文件编译到程序集：


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample18.cmd)]

如果此文件夹包含`csc.exe`中不在系统 s `PATH`，你将需要完全引用其路径、 `%WINDOWS%\Microsoft.NET\Framework\version\`，如下所示：


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample19.cmd)]


[![GetProductsWithPriceGreaterThan.cs 编译为程序集](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image69.png)

**图 29**： 编译`GetProductsWithPriceGreaterThan.cs`到程序集 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image71.png))


`/t`标志指定的 C# 类文件，应编译为 DLL （而不是可执行文件）。 `/out`标志指定生成的程序集的名称。

> [!NOTE]
> 而不是编译`GetProductsWithPriceGreaterThan.cs`从命令行或者无法使用的类文件[Visual C# 速成版](https://msdn.microsoft.com/vstudio/express/visualcsharp/)或在 Visual Studio Standard Edition 中创建单独的类库项目。 S ren 异世 Lauritsen 请提供具有代码对于此类的 Visual C# 速成版项目`GetProductsWithPriceGreaterThan`存储的过程和两个托管存储的过程和在步骤 3、 5 和 10 中创建的 UDF。 S ren s 项目还包括将添加相应的数据库对象所需的 T-SQL 命令。


用编译到程序集中的代码，我们已准备好注册 SQL Server 2005 数据库中的程序集。 这可以通过 T-SQL，使用命令执行`CREATE ASSEMBLY`，或通过 SQL Server Management Studio。 允许 s 焦点使用 Management Studio。

从 Management Studio 中，展开 Northwind 数据库中的可编程性文件夹。 其子文件夹之一是程序集。 若要手动将新的程序集添加到数据库中，右键单击程序集文件夹，并从上下文菜单中选择新的程序集。 此新的程序集对话框中 （请参见图 30） 的显示。 单击浏览按钮，选择`ManuallyCreatedDBObjects.dll`我们只需编译，，然后单击确定以向数据库添加程序集的程序集。 你不应看到`ManuallyCreatedDBObjects.dll`在对象资源管理器中的程序集。


[![向数据库添加 ManuallyCreatedDBObjects.dll 程序集](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image72.png)

**图 30**： 添加`ManuallyCreatedDBObjects.dll`到数据库的程序集 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image74.png))


![ManuallyCreatedDBObjects.dll 列出在对象资源管理器](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image75.png)

**图 31**:`ManuallyCreatedDBObjects.dll`列在对象资源管理器


虽然我们具有到 Northwind 数据库中添加程序集，但是我们尚未将关联的存储的过程`GetProductsWithPriceGreaterThan`程序集的方法。 若要完成此操作，打开新查询窗口并执行以下脚本：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample20.sql)]

这将名为 Northwind 数据库中创建新的存储的过程`GetProductsWithPriceGreaterThan`并将它与托管方法关联`GetProductsWithPriceGreaterThan`(即在类中`StoredProcedures`，这是在程序集中`ManuallyCreatedDBObjects`)。

执行以上脚本以后，刷新对象资源管理器中的存储过程文件夹。 你应该会看到新的存储的过程项目- `GetProductsWithPriceGreaterThan` -它具有一个锁状图标旁边。 若要测试此存储的过程，请输入，并在查询窗口中执行以下脚本：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample21.sql)]

如图 32 所示，上面的命令显示有关与这些产品的信息`UnitPrice`大于 $24.95。


[![ManuallyCreatedDBObjects.dll 列出在对象资源管理器](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image76.png)

**图 32**:`ManuallyCreatedDBObjects.dll`列在对象资源管理器 ([单击以查看实际尺寸的图像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image78.png))


## <a name="summary"></a>摘要

Microsoft SQL Server 2005 提供集成使用公共语言运行时 (CLR)，它允许使用托管的代码创建数据库对象。 以前，只能无法使用 T-SQL 的创建这些数据库对象，但现在，我们可以创建这些对象使用的.NET 编程语言，如 C#。 在本教程中我们创建了两个托管存储的过程和托管的用户定义函数。

Visual Studio 的 SQL Server 项目类型便于创建、 编译和部署托管的数据库对象。 此外，它提供了丰富的调试支持。 但是，SQL Server 项目类型才专业人员和团队系统版本的 Visual Studio 中可用。 对于那些使用 Visual Web Developer 或标准版的 Visual Studio、 创建、 编译和部署步骤必须手动执行，正如我们所看到的第 13 步。

尽情享受编程 ！

## <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [优点和缺点的用户定义函数](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [在托管代码中创建 SQL Server 2005 对象](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [创建在 SQL Server 2005 中使用托管的代码的触发器](http://www.15seconds.com/issue/041006.htm)
- [如何： 创建和运行 CLR SQL Server 存储过程](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [如何： 创建和运行 CLR SQL Server 用户定义函数](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [如何： 编辑`Test.sql`脚本运行 SQL 对象](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [简介用户定义的函数](http://www.sqlteam.com/item.asp?ItemID=1955)
- [托管的代码和 SQL Server 2005 （视频）](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [TRANSACT-SQL 参考](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [演练： 在托管代码中创建存储的过程](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 S ren 异世 Lauritsen。 除了查看此文章，S ren 还创建了此文章的下载用于手动编译托管的数据库对象中包含 Visual C# 速成版项目。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](debugging-stored-procedures-cs.md)
[下一页](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
