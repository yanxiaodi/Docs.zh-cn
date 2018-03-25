---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
title: "使用 SQL 缓存依赖项 (VB) |Microsoft 文档"
author: rick-anderson
description: "最简单的缓存策略是时间的允许缓存的数据在指定段后过期。 但是，此简单的方法意味着，缓存的数据 maintai..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: bd347d93-4251-4532-801c-a36f2dfa7f96
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
msc.type: authoredcontent
ms.openlocfilehash: af302d67b009fc25e38fb33a5e2a623f7200bcd5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="using-sql-cache-dependencies-vb"></a>使用 SQL 缓存依赖项 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_VB.zip)或[下载 PDF](using-sql-cache-dependencies-vb/_static/datatutorial61vb1.pdf)

> 最简单的缓存策略是时间的允许缓存的数据在指定段后过期。 但是，此简单的方法意味着，缓存的数据要维护对其基础的数据源，从而产生陈旧数据所占用太长或较短时间已过期的当前数据没有关联。 更好的方法是使用 SqlCacheDependency 类，以便数据会保留缓存直到其基础数据已修改 SQL 数据库中。 此教程演示如何。


## <a name="introduction"></a>介绍

在检查的缓存技术[ObjectDataSource 与缓存数据](caching-data-with-the-objectdatasource-vb.md)和[体系结构中缓存数据](caching-data-in-the-architecture-vb.md)教程使用基于时间的到期后指定逐出缓存中的数据段。 这种方法是平衡的缓存数据失效针对性能增益的最简单方法。 通过选择的时间到期*x* ，秒页开发人员 concedes 以享受仅缓存的性能优势*x*秒，但可以 rest 变得简单，她的数据都不会过时超过最大值*x*秒。 当然，对于静态数据， *x*可以扩展到 web 应用的生存期中，在已检查作为[缓存数据在应用程序启动](caching-data-at-application-startup-vb.md)教程。

当缓存数据库的数据，基于时间的到期通常选择为其易于使用，但通常是不足的解决方案。 理想情况下，数据库数据将保持缓存之前已修改基础数据中的站点数据库。只有在那时将逐出缓存。 这种方法最大化缓存的性能优势，并最大程度减少过时的数据的持续时间。 但是，若要享受这些优势有必须知道基础数据库数据的已修改，且逐出缓存中的相应项目的位置中的某些系统。 在 ASP.NET 2.0 中之前, 页开发人员负责实施此系统。

ASP.NET 2.0 提供[`SqlCacheDependency`类](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx)和确定当发生了更改数据库中，以便相应缓存项的必要基础结构可以被逐出。 有两种技术用于确定何时基础数据发生变化： 通知和轮询。 在讨论后通知和轮询之间的差异，我们将创建基础结构支持轮询，然后研究一下如何使用所需`SqlCacheDependency`类中声明性和以编程方式方案。

## <a name="understanding-notification-and-polling"></a>了解通知和轮询

有两种技术可以用于确定数据库中的数据修改时： 通知和轮询。 通知，当自上次执行查询以来已更改特定查询的结果后，数据库将自动警报 ASP.NET 运行时，逐出的哪个点与查询关联的缓存的项。 使用轮询，数据库服务器会维护有关上次更新时间特定的表的信息。 ASP.NET 运行时定期轮询数据库以检查表已发生的更改因为它们已输入到缓存。 这些修改其数据的表具有逐出及其关联的缓存项。

通知选项要求比轮询小于安装程序中，并且由于它跟踪更改在查询级别而不是在表级别更精细。 遗憾的是，通知只是完整版本的 Microsoft SQL Server 2005 （即，不是 Express 版本） 中提供的。 但是，轮询选项可以用于从 7.0 的 Microsoft SQL Server 2005 的所有版本。 由于这些教程使用 SQL Server 2005 Express edition，我们将重点设置和使用轮询选项。 在本教程针对进一步上 SQL Server 2005 的通知功能的资源的末尾，请查阅进一步读取部分。

使用轮询，必须将数据库配置为包括名为的表`AspNet_SqlCacheTablesForChangeNotification`具有三列- `tableName`， `notificationCreated`，和`changeId`。 此表包含每个表都有可能需要在 web 应用程序中的 SQL 缓存依赖项中使用的数据行。 `tableName`列指定的名称时表`notificationCreated`指示的日期和时间的行已添加到表。 `changeId`列的类型是`int`和具有初始值为 0。 与每一次修改表，其值即会递增。

除了`AspNet_SqlCacheTablesForChangeNotification`表，数据库还需要在每个可能出现的表上的触发器在中包括 SQL 缓存依赖项。 这些触发器执行，而无论插入、 更新或删除行和递增表 s`changeId`中的值`AspNet_SqlCacheTablesForChangeNotification`。

ASP.NET 运行时跟踪当前`changeId`表缓存数据使用时`SqlCacheDependency`对象。 定期检查数据库和任何`SqlCacheDependency`对象其`changeId`不同于数据库中的值不同以来逐出`changeId`值指示已对表的更改数据被缓存后。

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>步骤 1： 浏览`aspnet_regsql.exe`命令行程序

数据库必须安装程序以包含上面所述的基础结构使用轮询方法： 预定义的表 (`AspNet_SqlCacheTablesForChangeNotification`)，少量的存储的过程和触发器上每个可能在 web 中的 SQL 缓存依赖关系中使用的表。应用程序。 可以通过命令行程序创建这些表、 存储的过程和触发器`aspnet_regsql.exe`，该文件位于`$WINDOWS$\Microsoft.NET\Framework\version`文件夹。 若要创建`AspNet_SqlCacheTablesForChangeNotification`表和关联的存储的过程，运行以下命令从命令行：


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample1.cmd)]

> [!NOTE]
> 若要执行指定的数据库登录名必须是在这些命令[ `db_securityadmin` ](https://msdn.microsoft.com/library/ms188685.aspx)和[ `db_ddladmin` ](https://msdn.microsoft.com/library/ms190667.aspx)角色。 若要检查发送到的数据库 T-SQL`aspnet_regsql.exe`命令行程序，请参阅[此博客文章](http://scottonwriting.net/sowblog/posts/10709.aspx)。


例如，若要向 Microsoft SQL Server 数据库添加轮询的基础结构名为`pubs`名为数据库服务器上`ScottsServer`使用 Windows 身份验证，导航到相应的目录并从命令行中，输入：


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample2.cmd)]

已添加的数据库级基础结构后，我们需要将触发器添加到将 SQL 缓存依赖关系中使用这些表。 使用`aspnet_regsql.exe`命令行程序再次，但指定表名称使用`-t`切换和而不是使用`-ed`切换使用`-et`，如下所示：


[!code-html[Main](using-sql-cache-dependencies-vb/samples/sample3.html)]

若要添加到触发器`authors`和`titles`表上`pubs`上的数据库`ScottsServer`，使用：


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample4.cmd)]

对于本教程将添加到触发器`Products`， `Categories`，和`Suppliers`表。 我们将查看步骤 3 中的特定命令行语法。

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>步骤 2： 引用中的 Microsoft SQL Server 2005 Express Edition 数据库`App_Data`

`aspnet_regsql.exe`命令行程序需要的数据库和服务器名称，以便添加必要的轮询基础结构。 什么是 Microsoft SQL Server 2005 Express 数据库中驻留的数据库和服务器名称，但`App_Data`文件夹？ 而不是无需发现的数据库和服务器名称是什么，我已发现的最简单方法是附加到该数据库`localhost\SQLExpress`数据库实例和重命名数据使用[SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx)。 如果你有一个在计算机上安装的 SQL Server 2005 的完整版本，然后你很可能已具有在计算机上安装的 SQL Server Management Studio。 如果你只有在 Express 版本，你可以下载免费[Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796)。

通过关闭 Visual Studio 启动。 接下来，打开 SQL Server Management Studio 并选择连接到`localhost\SQLExpress`服务器使用 Windows 身份验证。


![将附加到 localhost\SQLExpress 服务器](using-sql-cache-dependencies-vb/_static/image1.gif)

**图 1**： 将附加到`localhost\SQLExpress`服务器


连接到服务器后，Management Studio 将显示服务器并使数据库、 安全性和等的子文件夹。 右键单击数据库文件夹并选择附加选项。 此时将显示附加数据库对话框框中 （请参见图 2）。 单击添加按钮，然后选择`NORTHWND.MDF`数据库文件夹中你的 web 应用程序 s`App_Data`文件夹。


[![将附加 northwnd 不。MDF App_Data 文件夹中的数据库](using-sql-cache-dependencies-vb/_static/image2.gif)](using-sql-cache-dependencies-vb/_static/image1.png)

**图 2**： 附加`NORTHWND.MDF`数据库从`App_Data`文件夹 ([单击以查看实际尺寸的图像](using-sql-cache-dependencies-vb/_static/image2.png))


这会将数据库添加到数据库文件夹。 数据库名称可能是数据库文件的完整路径或完整路径前面带有[GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)。 为了避免不得不时使用 aspnet 在此时间较长的数据库名称中键入\_regsql.exe 命令行工具、 重命名附加为多用户友好名称，请仅在数据库上右键单击该数据库，然后选择重命名。 我已重我的数据库命名为 DataTutorials。


![将附加的数据库重命名为多用户友好名称](using-sql-cache-dependencies-vb/_static/image3.gif)

**图 3**： 将附加的数据库重命名为多用户友好名称


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>步骤 3： 将轮询基础结构添加到 Northwind 数据库

现在，我们已附加`NORTHWND.MDF`数据库从`App_Data`文件夹中，我们重新已准备好添加轮询基础结构。 假设你已重命名为 DataTutorials 的数据库，运行以下四个命令：


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample5.cmd)]

在运行这些四个命令后, 右键单击 Management Studio 中的数据库名称，转到任务子菜单，然后选择分离。 然后关闭 Management Studio 并重新打开 Visual Studio。

后已重新打开 Visual Studio，钻取到通过服务器资源管理器数据库。 请注意新的表 (`AspNet_SqlCacheTablesForChangeNotification`)，新存储过程和触发器在`Products`， `Categories`，和`Suppliers`表。


![数据库现在包括必要的轮询基础结构](using-sql-cache-dependencies-vb/_static/image4.gif)

**图 4**： 数据库现在包括必要的轮询基础结构


## <a name="step-4-configuring-the-polling-service"></a>步骤 4： 配置轮询服务

在数据库中创建所需的表、 触发器和存储的过程后, 的最后一步是配置轮询服务，通过完成`Web.config`通过指定以毫秒为单位的数据库迁移到使用和轮询频率。 以下标记轮询 Northwind 数据库一次每隔一秒。


[!code-xml[Main](using-sql-cache-dependencies-vb/samples/sample6.xml)]

`name`中的值`<add>`与一个特定的数据库元素 (NorthwindDB) 关联的用户可读名称。 在使用 SQL 缓存依赖项，我们将需要参考此处定义以及缓存的数据为基础的表的数据库名称。 我们将了解如何使用`SqlCacheDependency`类以编程方式将与 SQL 缓存依赖项相关联缓存在步骤 6 中的数据。

轮询系统建立 SQL 缓存依赖项后, 将连接到数据库中定义`<databases>`元素每个`pollTime`毫秒并执行`AspNet_SqlCachePollingStoredProcedure`存储过程。 在步骤 3 中使用此存储的过程-已添加回`aspnet_regsql.exe`命令行工具-返回`tableName`和`changeId`中每个记录的值`AspNet_SqlCacheTablesForChangeNotification`。 过时的 SQL 缓存依赖项是从缓存中逐出。

`pollTime`设置引入了性能和数据已过期时间之间的权衡。 一个较小`pollTime`值会增加到数据库，请求数，但更多快速逐出缓存中的过时数据。 更大`pollTime`值将减少数据库请求数，但会增加后端数据的更改时和时逐出相关的缓存项之间的延迟。 幸运的，数据库请求正在执行简单的存储的过程从简单的轻型表返回只需几行该 s。 但执行试验不同`pollTime`值来查找之间找到一个理想的平衡点数据库应用程序的访问和数据过期时间。 最小`pollTime`允许的值为 500。

> [!NOTE]
> 上面的示例中提供单个`pollTime`中的值`<sqlCacheDependency>`元素，但是你可以选择指定`pollTime`中的值`<add>`元素。 如果你有多个指定的数据库并想要自定义每个数据库的轮询频率，这非常有用。


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>步骤 5： 以声明方式使用 SQL 缓存依赖项

在步骤 1 至 4 我们介绍了如何设置必要的数据库基础结构和配置轮询系统。 与此基础结构，我们现在可以添加项到数据缓存与使用编程或声明性技术关联 SQL 缓存依赖项。 在此步骤中，我们将查看如何以声明方式使用 SQL 缓存依赖项。 在步骤 6 中，我们将看编程的方式。

[缓存数据与 ObjectDataSource](caching-data-with-the-objectdatasource-vb.md)教程介绍了 ObjectDataSource 的声明性缓存功能。 通过只需设置`EnableCaching`属性`True`和`CacheDuration`到某些时间间隔的属性，ObjectDataSource 自动将缓存达到指定间隔从其基础对象返回的数据。 ObjectDataSource 还可以使用一个或多个 SQL 缓存依赖项。

若要演示如何以声明方式使用 SQL 缓存依赖项，打开`SqlCacheDependencies.aspx`页面`Caching`文件夹，然后拖动一个 GridView 从工具箱中拖动到设计器。 设置 GridView s`ID`到`ProductsDeclarative`和从其智能标记上，选择要绑定到名为新 ObjectDataSource `ProductsDataSourceDeclarative`。


[![创建名为 ProductsDataSourceDeclarative 新对象数据源](using-sql-cache-dependencies-vb/_static/image5.gif)](using-sql-cache-dependencies-vb/_static/image3.png)

**图 5**： 创建新对象数据源命名`ProductsDataSourceDeclarative`([单击以查看实际尺寸的图像](using-sql-cache-dependencies-vb/_static/image4.png))


配置对象数据源以使用`ProductsBLL`类，然后在选择选项卡中设置下拉列表`GetProducts()`。 在更新选项卡，选择`UpdateProduct`带有三个输入参数的重载`productName`， `unitPrice`，和`productID`。 在 INSERT 和 DELETE 选项卡中设置下拉列表中的，以便 （无）。


[![使用带有三个输入参数的 UpdateProduct 重载](using-sql-cache-dependencies-vb/_static/image6.gif)](using-sql-cache-dependencies-vb/_static/image5.png)

**图 6**： 带有三个输入参数使用 UpdateProduct 重载 ([单击以查看实际尺寸的图像](using-sql-cache-dependencies-vb/_static/image6.png))


[![设置为 (None) 对插入和删除选项卡的下拉列表](using-sql-cache-dependencies-vb/_static/image7.gif)](using-sql-cache-dependencies-vb/_static/image7.png)

**图 7**： 对插入和删除选项卡设置为 (None) 的下拉列表 ([单击以查看实际尺寸的图像](using-sql-cache-dependencies-vb/_static/image8.png))


完成配置数据源向导后，Visual Studio 将 BoundFields 内创建和 CheckBoxFields GridView 为每个数据字段。 删除所有字段，但`ProductName`， `CategoryName`，和`UnitPrice`，并根据你的设置这些字段的格式。 从 GridView s 智能标记，选中启用分页、 启用排序和启用编辑复选框。 Visual Studio 将设置 ObjectDataSource s`OldValuesParameterFormatString`属性`original_{0}`。 或者可以使 GridView 的编辑功能，使其正常工作，来移除完全从的声明性语法或集回其默认值，此属性`{0}`。

最后，添加到的 GridView 和集上方标签 Web 控件其`ID`属性`ODSEvents`及其`EnableViewState`属性`False`。 进行这些更改后，你页面 s 声明性标记应类似于以下。 请注意，已进行了一些的审美自定义项，则不需要为了演示 SQL 缓存依赖项功能的 GridView 字段。


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample7.aspx)]

接下来，创建的事件处理程序 ObjectDataSource 的`Selecting`事件并在它中添加以下代码：


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample8.vb)]

回想一下，ObjectDataSource 的`Selecting`仅从其基础对象检索数据时，事件将激发。 如果 ObjectDataSource 从其自己的缓存访问数据，不激发此事件。

现在，请访问此页面通过浏览器。 因为我们遇到有待实现任何缓存，每次页上，进行排序，或编辑的网格页应显示文本，选择事件激发，如图 8 所示。


[![ObjectDataSource 的选择事件激发每个时间分页 GridView，编辑或 Sorted](using-sql-cache-dependencies-vb/_static/image8.gif)](using-sql-cache-dependencies-vb/_static/image9.png)

**图 8**: ObjectDataSource s`Selecting`事件将触发每个时间分页 GridView、 编辑或 Sorted ([单击以查看实际尺寸的图像](using-sql-cache-dependencies-vb/_static/image10.png))


正如我们看到在[ObjectDataSource 与缓存数据](caching-data-with-the-objectdatasource-vb.md)教程中，设置`EnableCaching`属性`True`ObjectDataSource 由指定的持续时间的缓存其数据将导致其`CacheDuration`属性。 ObjectDataSource 还有[`SqlCacheDependency`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx)，从而将一个或多个 SQL 缓存依赖项添加到缓存的数据使用模式：


[!code-css[Main](using-sql-cache-dependencies-vb/samples/sample9.css)]

其中*databaseName*是作为中指定的数据库的名称`name`属性`<add>`中的元素`Web.config`，和*tableName*是数据库表的名称。 例如，若要创建无限期地缓存数据 ObjectDataSource 基于 SQL 缓存依赖项针对 Northwind s`Products`表中，设置 ObjectDataSource s`EnableCaching`属性`True`及其`SqlCacheDependency`属性NorthwindDB:Products。

> [!NOTE]
> 你可以使用 SQL 缓存依赖项*和*通过设置基于时间的到期`EnableCaching`到`True`，`CacheDuration`为时间间隔中，和`SqlCacheDependency`对数据库和表名。 当达到基于时间的到期时或当轮询系统会注意，基础数据库数据已更改，以先发生者为准，ObjectDataSource 将逐出其数据。


在 GridView`SqlCacheDependencies.aspx`显示从两个表的数据`Products`和`Categories`(产品 s`CategoryName`字段检索通过`JOIN`上`Categories`)。 因此，我们想要指定两个 SQL 缓存依赖项： NorthwindDB:Products;NorthwindDB:Categories。


[![配置对象数据源以支持缓存产品和类别上使用 SQL 缓存依赖项](using-sql-cache-dependencies-vb/_static/image9.gif)](using-sql-cache-dependencies-vb/_static/image11.png)

**图 9**： 上配置支持缓存使用 SQL 缓存依赖项 ObjectDataSource`Products`和`Categories`([单击以查看实际尺寸的图像](using-sql-cache-dependencies-vb/_static/image12.png))


在配置对象数据源以支持缓存之后, 重新访问通过浏览器页面。 同样，激发的文本选择事件应显示在第一次的页访问，但应消失时分页、 排序，或单击编辑或取消按钮。 这是因为数据加载到 ObjectDataSource 的缓存之后，它将一直存在直到`Products`或`Categories`修改表或数据通过 GridView 进行了更新。

沿网格分页，并记下在缺乏选择事件触发后文本，打开新的浏览器窗口并导航到编辑，插入和删除部分中的基础知识教程 (`~/EditInsertDelete/Basics.aspx`)。 更新的名称或产品价格。 然后，从第一个浏览器窗口中，查看不同的页面的数据，排序网格中，或单击行的编辑按钮。 这一次，选择事件激发应重新出现，如基础数据库数据已被修改 （请参阅图 10）。 如果文本不出现，请稍等片刻，然后重试。 请记住，轮询服务正在检查的更改`Products`表每个`pollTime`毫秒，而没有更新基础数据时和时逐出缓存的数据之间的延迟。


[![修改 Products 表逐出缓存的产品数据](using-sql-cache-dependencies-vb/_static/image10.gif)](using-sql-cache-dependencies-vb/_static/image13.png)

**图 10**： 修改产品表逐出缓存产品数据 ([单击以查看实际尺寸的图像](using-sql-cache-dependencies-vb/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>步骤 6： 以编程方式使用`SqlCacheDependency`类

[体系结构中缓存数据](caching-data-in-the-architecture-vb.md)教程看而不是紧密耦合 ObjectDataSource 缓存的体系结构中使用单独的缓存层的好处。 在该教程中，我们将创建`ProductsCL`类，以演示如何以编程方式使用数据缓存。 若要利用缓存层中的 SQL 缓存依赖项，使用`SqlCacheDependency`类。

使用轮询系统`SqlCacheDependency`对象必须与一个特定的数据库和表对相关联。 下面的代码，例如，创建`SqlCacheDependency`对象基于 Northwind 数据库的`Products`表：


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample10.vb)]

两个输入参数为`SqlCacheDependency`s 构造函数将分别为数据库和表名称。 与 ObjectDataSource s 类似`SqlCacheDependency`属性，使用的数据库名称是中指定的值相同`name`属性`<add>`中的元素`Web.config`。 表名是数据库表的实际名称。

若要将关联`SqlCacheDependency`与添加到数据缓存的项，请使用之一`Insert`接受依赖关系的方法重载。 下面的代码添加*值*到数据缓存中用于无限期的持续时间，但将其关联与`SqlCacheDependency`上`Products`表。 简单地说，*值*将保留在缓存中，直到逐出由于内存约束，或者由于轮询系统已检测到`Products`表已更改自缓存。


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample11.vb)]

缓存层 s`ProductsCL`类当前缓存中的数据`Products`表使用 60 秒的基于时间的到期。 让我们来更新此类，以便它改为使用 SQL 缓存依赖项。 `ProductsCL`类的`AddCacheItem`方法，负责将数据添加到缓存，当前包含以下代码：


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample12.vb)]

更新此代码以使用`SqlCacheDependency`对象而不是`MasterCacheKeyArray`缓存依赖项：


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample13.vb)]

若要测试此功能，向下现有页面添加一个 GridView `ProductsDeclarative` GridView。 设置此新的 GridView s`ID`到`ProductsProgrammatic`并通过其智能标记，请将其绑定到名为新 ObjectDataSource `ProductsDataSourceProgrammatic`。 配置对象数据源以使用`ProductsCL`类，设置的下拉列表中选择和更新选项卡添加到`GetProducts`和`UpdateProduct`分别。


[![配置对象数据源以使用 ProductsCL 类](using-sql-cache-dependencies-vb/_static/image11.gif)](using-sql-cache-dependencies-vb/_static/image15.png)

**图 11**： 配置使用 ObjectDataSource`ProductsCL`类 ([单击以查看实际尺寸的图像](using-sql-cache-dependencies-vb/_static/image16.png))


[![从选择选项卡上的下拉列表中选择 GetProducts 方法](using-sql-cache-dependencies-vb/_static/image12.gif)](using-sql-cache-dependencies-vb/_static/image17.png)

**图 12**： 选择`GetProducts`从选择的选项卡 s 下拉列表的方法 ([单击以查看实际尺寸的图像](using-sql-cache-dependencies-vb/_static/image18.png))


[![从更新选项卡的下拉列表中选择 UpdateProduct 方法](using-sql-cache-dependencies-vb/_static/image13.gif)](using-sql-cache-dependencies-vb/_static/image19.png)

**图 13**： 从更新选项卡的下拉列表中选择 UpdateProduct 方法 ([单击以查看实际尺寸的图像](using-sql-cache-dependencies-vb/_static/image20.png))


完成配置数据源向导后，Visual Studio 将 BoundFields 内创建和 CheckBoxFields GridView 为每个数据字段。 如与第一个 GridView 添加到此页上，删除所有字段，但`ProductName`， `CategoryName`，和`UnitPrice`，并根据你的设置这些字段的格式。 从 GridView s 智能标记，选中启用分页、 启用排序和启用编辑复选框。 与`ProductsDataSourceDeclarative`ObjectDataSource，Visual Studio 将设置`ProductsDataSourceProgrammatic`ObjectDataSource s`OldValuesParameterFormatString`属性`original_{0}`。 为了使 GridView 的编辑功能，若要正常工作，请将此属性设置回`{0}`（或完全声明性语法中删除属性赋值）。

完成这些任务之后，生成的 GridView 和 ObjectDataSource 声明性标记应如下所示：


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample14.aspx)]

若要测试 SQL 缓存层中的缓存依赖项中设置断点`ProductCL`类的`AddCacheItem`方法，然后开始调试。 当你首次访问`SqlCacheDependencies.aspx`，应会命中断点，作为第一次请求数据并将其放入缓存。 接下来，将移动到 GridView 的另一页或排序的列之一。 这将导致 GridView 类型重新查询其数据，但应以来在缓存中找到数据`Products`尚未修改数据库表。 如果反复不在缓存中找到的数据，请确保有足够的内存可用在你的计算机上，然后重试。

后到的 GridView 的几个页面的分页，打开第二个浏览器窗口并导航到编辑，插入和删除部分中的基础知识教程 (`~/EditInsertDelete/Basics.aspx`)。 更新来自产品表的记录，然后，从第一个浏览器窗口中，查看新的页或单击其中一个排序的标头。

在这种情况下你将看到以下两项操作之一： 任一断点将被命中，，该值指示缓存的数据已被逐出由于数据库; 中更改或者，将不会命中断点，这意味着，`SqlCacheDependencies.aspx`现在显示过时的数据。 如果不命中断点，则很可能因为数据的更改轮询服务不尚未激发。 请记住，轮询服务正在检查的更改`Products`表每个`pollTime`毫秒，而没有更新基础数据时和时逐出缓存的数据之间的延迟。

> [!NOTE]
> 此延迟是更有可能出现在编辑通过在 GridView 的产品之一时`SqlCacheDependencies.aspx`。 在[体系结构中缓存数据](caching-data-in-the-architecture-vb.md)教程，我们添加`MasterCacheKeyArray`缓存依赖关系，以确保通过正在编辑的数据`ProductsCL`类的`UpdateProduct`方法已从缓存中逐出。 但是，我们替换为此缓存依赖项时修改`AddCacheItem`之前在此步骤中的方法，因此`ProductsCL`类将继续显示缓存的数据，直到轮询系统会注意到更改`Products`表。 我们将了解如何将重新引入`MasterCacheKeyArray`缓存在步骤 7 中的依赖项。


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>步骤 7： 将多个依赖项与缓存的项相关联

回想一下，`MasterCacheKeyArray`缓存依赖项用于确保*所有*更新中其关联任何单个项时，与产品相关的数据从缓存中逐出。 例如，`GetProductsByCategoryID(categoryID)`方法缓存`ProductsDataTables`实例为每个唯一*categoryID*值。 如果逐出这些对象之一，`MasterCacheKeyArray`缓存依赖项可确保其他还会删除。 而无需此缓存依赖项，修改缓存的数据时可能性是存在的其他缓存的产品数据可能会过期。 因此，它 s 重要我们维护`MasterCacheKeyArray`缓存时使用 SQL 缓存依赖项的依赖项。 但是，数据缓存 s`Insert`方法仅允许单个的依赖对象。

此外，使用 SQL 缓存依赖项时我们可能需要将关联作为依赖关系的多个数据库表。 例如，`ProductsDataTable`缓存在中`ProductsCL`类包含每个产品类别和供应商名称但`AddCacheItem`方法仅使用一个依赖项上`Products`。 在此情况下，如果用户更新的类别或供应商的名称缓存的产品数据将保留在缓存中并已过期。 因此，我们想要依赖于缓存的产品数据不仅`Products`表，但在`Categories`和`Suppliers`以及表。

[ `AggregateCacheDependency`类](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx)提供了一种将多个依赖项与缓存项相关联。 首先创建`AggregateCacheDependency`实例。 接下来，添加一组的依赖关系使用`AggregateCacheDependency`s`Add`方法。 如果将项插入数据缓存之后，将传递中`AggregateCacheDependency`实例。 当*任何*的`AggregateCacheDependency`实例 s 依赖项更改，将逐出缓存的项。

下面显示的更新的代码`ProductsCL`类的`AddCacheItem`方法。 该方法将创建`MasterCacheKeyArray`缓存依赖项以及`SqlCacheDependency`对象`Products`， `Categories`，和`Suppliers`表。 这些所有合并成一个`AggregateCacheDependency`对象名为`aggregateDependencies`，后者再传递到`Insert`方法。


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample15.vb)]

测试出此新代码。现在将更改为`Products`， `Categories`，或`Suppliers`表导致被逐出缓存的数据。 此外，`ProductsCL`类 s`UpdateProduct`方法，它在编辑通过 GridView 产品时调用，逐出`MasterCacheKeyArray`缓存依赖项，这会导致缓存`ProductsDataTable`逐出和要在下次重新检索的数据请求。

> [!NOTE]
> SQL 缓存依赖项也可以用于[输出缓存](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx)。 此功能的演示，请参阅：[使用与 SQL Server ASP.NET 输出缓存](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx)。


## <a name="summary"></a>摘要

除非在数据库中修改，否则，当缓存数据库的数据，数据将理想情况下会保留在缓存中。 使用 ASP.NET 2.0 中，可以创建和声明性和编程方案中使用 SQL 缓存依赖项。 使用此方法时面临的挑战之一是在发现时修改的数据。 Microsoft SQL Server 2005 的完整版本提供当查询结果已更改时，可以通知应用程序的通知功能。 对于 Express 版本的 SQL Server 2005 和 SQL server 的较旧版本，必须使用一种轮询系统。 幸运的是，设置必要的轮询基础结构将非常简单。

尽情享受编程 ！

## <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [在 Microsoft SQL Server 2005 中使用查询通知](https://msdn.microsoft.com/library/ms175110.aspx)
- [创建查询通知](https://msdn.microsoft.com/library/ms188669.aspx)
- [在缓存中使用的 ASP.NET`SqlCacheDependency`类](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [ASP.NET SQL 服务器注册工具 (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [概述`SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Marko Rangel、 Teresa 墨和希尔顿 Giesenow。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一篇](caching-data-at-application-startup-vb.md)
