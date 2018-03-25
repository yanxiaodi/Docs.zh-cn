---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: "处理与实体框架中的 ASP.NET MVC 应用程序 (7 个 10) 的并发 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 87bb08a4d16965a10112a42c4e9318c32f192c04
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>处理与实体框架中的 ASP.NET MVC 应用程序 (7 个 10) 的并发
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 你可以从头开始教程系列或[下载这一章的初学者项目](building-the-ef5-mvc4-chapter-downloads.md)和从这里开始。
> 
> > [!NOTE] 
> > 
> > 如果你遇到无法解决的问题[下载已完成的章](building-the-ef5-mvc4-chapter-downloads.md)并尝试重现你的问题。 你通常可以通过比较你的代码已完成的代码会发现问题的解决方案。 一些常见的错误和如何解决它们，请参阅[错误和解决方法。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


在前面的两个教程中，你处理相关的数据。 本教程演示如何处理并发。 你将创建使用的 web 页`Department`实体，并编辑和删除的页面`Department`实体将处理并发错误。 下图显示的索引和删除页，包括某些并发冲突发生时显示的消息。

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>并发冲突

一个用户才能进行编辑，显示实体的数据，并在第一个用户的更改写入到数据库之前，另一个用户然后更新相同实体的数据时发生并发冲突。 如果不启用此类冲突检测，任何人都更新数据库上次将覆盖其他用户的更改。 在许多应用程序，这种风险很可接受： 如果有很多用户或几个更新，或如果并不真正重要的一些更改将被覆盖，如果并发编程的开销可能超过带来的好处。 在这种情况下，你不必配置此应用程序处理并发冲突。

### <a name="pessimistic-concurrency-locking"></a>保守式并发 （锁定）

如果你的应用程序需要防止并发方案中的意外数据丢失，做到这一点的一种方法是使用数据库锁。 这称为*保守式并发*。 例如，从数据库读取行之前，你请求的锁只读的或更新访问权限。 如果锁定的行更新访问权限时，不允许任何其他用户锁定该行为只读的或更新访问权限，因为它们将获取正在进行更改的数据的副本。 如果锁定用于只读访问的行，其他人可以还将其锁定用于只读访问权限但不是用于更新。

管理锁也有缺点。 它可以是复杂到程序。 它需要大量的数据库管理资源，并且可能导致性能问题的应用程序的用户数会增加 （即，它不能很好）。 出于这些原因，不是所有数据库管理系统都支持保守式并发。 本教程不向你展示如何实现它，并且实体框架提供，没有内置支持。

### <a name="optimistic-concurrency"></a>开放式并发

保守式并发替代方法是*开放式并发*。 开放式并发意味着允许并发冲突发生，然后相应地反应，如果他们这样做。 例如，John 运行部门编辑页上，更改**预算**量从 $350,000.00 0.00 美元到英语的部门。

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

John 单击之前**保存**，Jane 运行相同的页和更改**开始日期**字段从 2007 年 9 月 1 日到 2013 年 8 月 8 日。

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

John 单击**保存**第一个并查看他的更改时浏览器返回到索引页面，然后 Jane 单击**保存**。 接下来如何操作取决于如何处理并发冲突。 某些选项包括：

- 你可以跟踪的用户进行了修改的属性，并更新仅在数据库中的相应列。 在示例方案中，任何数据将不会丢失，因为不同的属性已更新由两个用户。 下一次有人浏览英语部门时，他们将看到 John 的和 Jane 的更改 — 2013 年 8 月 8 日开始日期和零美元的预算。

    这种更新方法可以减少可能导致数据丢失的冲突数，但它无法避免数据丢失，如果实体的同一属性进行竞争的更改。 实体框架是否可以通过这种方式取决于如何实现你更新代码。 它通常是状态的不现实中 web 应用程序，因为它可能需要维护大量，以便跟踪的实体的所有原始属性值以及新值。 因为它需要服务器资源或必须包括 （例如，在隐藏的字段） 的 web 页本身中，保留大量的状态可能会影响应用程序性能。
- 你可以让 Jane 的更改覆盖 John 的更改。 下一次有人浏览英语部门时，他们将看到 2013 年 8 月 8 日和还原的 $350,000.00 值。 这称为*客户端优先*或*在 Wins 中最后一个*方案。 （客户端的值优先于什么是在数据存储。）中所述的简介本部分中，如果您不执行任何并发处理的编码，这将自动发生。
- 你可以防止在数据库中正在更新 Jane 的更改。 通常情况下，将显示一条错误消息、 她显示的数据的当前状态和让她可以如果她仍然想要对它们进行重新应用其更改。 这称为*存储 Wins*方案。 （数据存储值优先于提交的客户端的值。）在本教程中，你将实现存储 Wins 方案。 此方法可确保到发生了什么情况收到通知用户的情况下，任何更改都会被覆盖。

### <a name="detecting-concurrency-conflicts"></a>检测并发冲突

您可以通过处理中解决冲突[OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx)实体框架引发的异常。 为了知道何时引发这些异常，实体框架必须能够检测到冲突。 因此，你必须从数据库和数据模型正确配置。 有关启用冲突检测某些选项包括：

- 在数据库表中，包括可以用于确定当行已更改的跟踪列。 然后，你可以配置实体框架可以包括中的列`Where`的 SQL 子句`Update`或`Delete`命令。

    跟踪列的数据类型通常是[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)。 [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)值是每次更新行时递增序列号。 在`Update`或`Delete`命令，`Where`子句包括跟踪列 （原始行版本） 的原始值。 如果正在更新的行已由其他用户时中的值更改`rowversion`列是不同于原始值，因此`Update`或`Delete`语句找不到要由于更新的行`Where`子句。 在实体框架查找已通过更新了任何行`Update`或`Delete`命令 （即，在受影响的行数为零） 时，它将，解释为并发冲突。
- 配置实体框架可以包括在表中的每个列的原始值`Where`子句`Update`和`Delete`命令。

    如下所示的第一个选项，如果自第一次读取一行，行中的任何内容已更改`Where`子句不会返回的行，若要更新，该实体框架会将解释为并发冲突。 对于有多列的数据库表，这种方法可能会导致非常大`Where`子句，并可能需要维护大量的状态。 如前文所述，保留大量的状态可能会影响应用程序性能因为它需要服务器资源或必须包含在 web 页本身。 因此此方法通常不建议，并且它不在本教程中使用的方法。

    如果您想要实现这种并发的方法，则必须将你想要通过添加跟踪的并发性实体中的所有非主键属性标记[ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx)属性设为它们。 更改使实体框架能够在 SQL 中包括所有列`WHERE`子句`UPDATE`语句。

本教程的其余部分中将添加[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)跟踪属性`Department`实体，创建控制器和视图，并进行测试以验证一切是否正常工作。

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>将开放式并发属性添加到部门实体

在*Models\Department.cs*，添加名为的跟踪属性`RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

[时间戳](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx)属性指定此列将包含在`Where`子句`Update`和`Delete`命令发送到数据库。 该属性称为[时间戳](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx)因为以前版本的 SQL Server 使用 SQL[时间戳](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx)之前 SQL 数据类型[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)替换它。 .Net 类型`rowversion`为字节数组。 如果想要使用 fluent API，则可以使用[IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx)方法，以指定跟踪属性，如下面的示例中所示：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

通过添加一个属性，更改数据库模型，因此你需要执行另一个迁移。 在包管理器控制台 (PMC) 中，输入以下命令：

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>创建部门控制器

创建`Department`控制器和视图相同的方式未其他控制器，使用以下设置：

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

在*Controllers\DepartmentController.cs*，添加`using`语句：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

更改"LastName"到"全名"此文件 （四个匹配项） 中的所有位置，以便部门管理员下拉列表将包含教师的完整名称而不是只是最后一个名称。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

替换现有代码，以`HttpPost``Edit`方法替换为以下代码：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

该视图将存储原始`RowVersion`隐藏字段中的值。 当模型联编程序创建`department`实例，该对象将拥有原始`RowVersion`属性值和其他属性，如由用户在编辑页上输入的新值。 然后创建时实体框架 SQL`UPDATE`命令时，命令将包括`WHERE`查找具有原始行的子句`RowVersion`值。

如果任何行不受`UPDATE`命令 (任何行具有原始`RowVersion`值)，实体框架将引发`DbUpdateConcurrencyException`异常，并且中的代码`catch`块可获取在受影响`Department`的异常中的实体对象。 此实体具有从数据库读取的值和用户输入的新值：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

接下来，代码将添加每个列都具有数据库值不同于在编辑页上输入的用户自定义错误消息：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

较长的错误消息说明发生了什么情况以及如何对它执行的操作：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

最后，代码设置`RowVersion`值`Department`从数据库中检索到的新值的对象。 此新`RowVersion`值将存储在隐藏的字段，将重新显示页，编辑和下一次用户单击时**保存**，因为将捕获的编辑页重新显示发生这种情况的并发错误。

在*Views\Department\Edit.cshtml*，添加隐藏的字段以保存`RowVersion`立即之后的隐藏的字段的属性值`DepartmentID`属性：

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

在*Views\Department\Index.cshtml*，将现有代码替换为以下代码以向左移动行链接和更改页面标题和列标题以显示`FullName`而不是`LastName`中**管理员**列：

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>测试乐观并发处理

运行站点，然后单击**部门**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

右键单击**编辑**Kim Abercrombie 并选择超链接**在新的选项卡中打开**然后单击**编辑**为 Kim Abercrombie 的超链接。 两个窗口显示相同的信息。

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

更改第一个浏览器窗口中的字段，然后单击**保存**。

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

浏览器将显示更改后的值的索引页。

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

更改第二个浏览器窗口中的任何字段，然后单击**保存**。

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

单击**保存**在第二个浏览器窗口。 你看到一条错误消息：

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

单击**保存**试。 在第二个浏览器中输入的值与在第一个浏览器中更改的数据的原始值一起保存。 索引页面显示时，将显示保存的值。

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>更新删除页

对于删除页中，实体框架检测并发冲突某人其他类似的方式编辑部门引起。 当`HttpGet``Delete`方法会显示确认视图，则视图包括原始`RowVersion`隐藏字段中的值。 然后，该值将可供`HttpPost``Delete`用户确认删除时调用的方法。 当实体框架创建 SQL`DELETE`命令时，它包括`WHERE`子句与原始`RowVersion`值。 如果零行中的命令结果的影响 （含义后显示删除确认页，已更改行），则会引发一个并发异常，和`HttpGet Delete`调用方法时，将设置为错误标志`true`以重新显示确认页，并一条错误消息。 还有可能因为行由其他用户时，删除，因此不同的错误消息显示在此情况下影响了零行。

在*DepartmentController.cs*，替换`HttpGet``Delete`方法替换为以下代码：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

该方法将接受可选参数，该值指示是否页正在后将重新显示并发错误。 如果此标志为`true`，一条错误消息发送到视图使用`ViewBag`属性。

中的代码替换`HttpPost``Delete`方法 (名为`DeleteConfirmed`) 替换为以下代码：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

在您刚更换的基架代码，此方法接受仅记录 ID:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

你已更改此参数设为`Department`实体实例创建的模型联编程序。 这使您可以访问`RowVersion`除了的记录密钥的属性值。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

您还已更改的操作方法名称`DeleteConfirmed`到`Delete`。 名为的基架的代码`HttpPost``Delete`方法`DeleteConfirmed`以便`HttpPost`方法一个唯一的签名。 （CLR 需要具有不同的方法参数的重载的方法。）签名是唯一的现在可以坚持使用 MVC 约定，使用相同的名称用于`HttpPost`和`HttpGet`删除方法。

如果捕获到并发错误，则该代码将显示删除确认页，并提供一个标志，指示它应显示并发错误消息。

在*Views\Department\Delete.cshtml*，替换替换为以下代码，使一些格式设置该基架的代码更改，并添加一个错误消息域。 突出显示所做的更改。

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

此代码将添加一条错误消息之间`h2`和`h3`标题：

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

它将替换`LastName`与`FullName`中`Administrator`字段：

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

最后，它将添加隐藏的字段`DepartmentID`和`RowVersion`后的属性`Html.BeginForm`语句：

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

运行部门索引页。 右键单击**删除**英语部门和选择的超链接**在新窗口中打开**然后在第一个窗口中单击**编辑**对于英语的超链接部门。

在第一个窗口中，更改一个值，然后单击**保存**:

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

索引页确认更改。

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

在第二个窗口中，单击**删除**。

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

请参阅并发错误消息，并使用什么是当前数据库中刷新的部门值。

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

如果你单击**删除**同样，在重定向到显示部门已被删除的索引页。

## <a name="summary"></a>摘要

这将完成简介处理并发冲突。 有关其他方法来处理各种并发方案的信息，请参阅[乐观并发模式](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx)和[使用属性值](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx)实体框架团队博客上。 下一教程演示如何实现的每个层次结构一个表继承`Instructor`和`Student`实体。

在找不到其他实体框架资源的链接[ASP.NET 数据访问内容映射](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一页](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[下一页](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
