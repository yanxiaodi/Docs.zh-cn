---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: "异步和与实体框架中的 ASP.NET MVC 应用程序的存储的过程 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 的 ASP.NET MVC 5 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 7412b32ac29179dfa319544781d4c7165c58196b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a>异步和与实体框架中的 ASP.NET MVC 应用程序的存储的过程
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下载 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


在早期教程中，您学习了如何读取和更新使用的同步的编程模型的数据。 在本教程中，你将了解如何实现异步编程模型。 异步代码可帮助更好地执行，因为它使更好地利用服务器资源的应用程序。

在本教程还将了解如何存储的过程用于插入、 更新和删除实体上的操作。

最后，你将重新部署到 Azure，以及已实现后首次你部署的数据库更改的所有应用程序。

下图显示一些您将使用的页。

![部门页](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![创建部门](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a>为什么涉及异步代码

Web 服务器具有有限的数量的线程可用，并且在高负载情况下的所有可用线程可能正在使用。 在这种情况，服务器无法处理新请求，直到线程会被释放。 与同步代码，多个线程可能会占用时它们实际上不执行任何操作，因为它们正在等待 I/O 完成。 与异步代码时进程正在等待 I/O 完成，将其线程被释放服务器用于处理其他请求。 因此，异步代码允许服务器资源更有效地使用且已启用服务器以处理更多流量不会延迟。

在早期版本的.NET 中，编写和测试异步代码较为复杂，容易，并且难以调试的错误。 .NET 4.5 中编写、 测试和调试异步代码是，应通常编写异步代码，除非你有其他原因不变得更加方便。 异步代码会引入的少量开销，但潜在的性能改善的性能影响可以忽略不计，时针对的高流量情况低流量情况下，较高。

有关异步编程的详细信息，请参阅[使用.NET 4.5 的异步支持，以避免阻塞调用](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async)。

## <a name="create-the-department-controller"></a>创建部门控制器

创建相同的方式更早版本的控制器，只不过这次选择部门控制器**使用异步控制器**操作复选框。

![部门控制器基架](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

以下突出显示内容已添加到的同步代码`Index`方法来执行异步：

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

应用了四个更改，以启用实体框架数据库查询，以异步方式执行：

- 该方法将标有`async`关键字，它告诉编译器生成的方法主体的部件的回调并自动创建`Task<ActionResult>`返回的对象。
- 返回类型已从更改`ActionResult`到`Task<ActionResult>`。 `Task<T>`类型表示正在进行的工作类型的结果`T`。
- `await`关键字应用于 web 服务调用。 当编译器发现此关键字时，在幕后它拆分方法为两个部分。 第一部分结尾以异步方式启动该操作。 第二部分被放入操作完成时调用的回调方法。
- 异步版本`ToList`调用扩展方法。

为什么是`departments.ToList`的语句，但不是修改`departments = db.Departments`语句？ 原因是以异步方式执行导致查询或命令发送到数据库的语句。 `departments = db.Departments`语句将设置查询，但是直到不执行查询`ToList`调用方法。 因此，仅`ToList`以异步方式执行方法。

在`Details`方法和`HttpGet``Edit`和`Delete`方法，`Find`方法是，则查询发送到数据库，因此是以异步方式执行该方法的一个：

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

在`Create`， `HttpPost Edit`，和`DeleteConfirmed`方法，它是`SaveChanges`导致要执行的命令的方法调用、 不等的语句`db.Departments.Add(department)`这仅在内存中修改导致实体。

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

打开*Views\Department\Index.cshtml*，并将模板代码替换为以下代码：

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

此代码会从索引标题更改为部门，将管理员名称移动到右侧，并提供管理员的完整名称。

在创建中，删除详细信息，并编辑视图，则更改标题`InstructorID`为"Administrator"相同的方式的课程视图中的部门名称字段更改为"部门"字段。

在创建和编辑视图使用下面的代码：

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

在删除和详细信息视图中使用以下代码：

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

运行该应用程序，并单击**部门**选项卡。

![部门页](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

一切会与其他控制器中的相同，但在此控制器中的 SQL 查询的所有正在执行以异步方式。

请注意当您正在使用实体框架的异步编程的一些事项：

- 异步代码不是线程安全。 换而言之，换而言之，不要尝试执行中使用同一上下文实例并行的多个操作。
- 如果你想要利用异步代码的性能优势，请确保任何库包，你仅使用 （例如，用于分页），还使用异步，如果它们调用任何导致查询发送到数据库的实体框架方法。

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a>存储的过程用于插入、 更新和删除

一些开发人员和 Dba 更愿意使用存储的过程来访问数据库。 在早期版本的实体框架中，您可以检索数据使用通过存储的过程[执行原始的 SQL 查询](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)，但你不能指示 EF 存储的过程用于更新操作。 EF 6 中它很容易配置 Code First 以使用存储的过程。

1. 在*DAL\SchoolContext.cs*，添加到突出显示的代码`OnModelCreating`方法。

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    此代码指示使用实体框架与使用存储过程为插入、 更新和删除操作上`Department`实体。
2. 在包管理控制台中，输入以下命令：

    `add-migration DepartmentSP`

    打开*迁移\&lt; 时间戳&gt;\_DepartmentSP.cs*若要查看中的代码`Up`创建的方法插入、 更新和删除存储过程：

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
- 在包管理控制台中，输入以下命令：

    `update-database`
- 在调试模式下运行应用程序，请单击**部门**选项卡上，并依次**新建**。
- 新部门，输入数据，然后单击**创建**。

    ![创建部门](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
- 在 Visual Studio 中，查看日志中**输出**窗口来查看存储的过程已用于插入新的部门行。

    ![部门插入 SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

代码首先创建默认存储过程名称。 如果使用现有数据库，你可能需要自定义为了使用已在数据库中定义的存储的过程的存储的过程名称。 有关如何执行该操作的信息，请参阅[实体框架代码第一个插入/更新/删除存储过程](https://msdn.microsoft.com/data/dn468673)。

如果你想要自定义内容生成存储的过程执行，则可以编辑迁移的基架的代码`Up`创建存储的过程的方法。 通过这种方式所做的更改将反映每当迁移运行，并且将应用到你的生产数据库中，当迁移后部署在生产中自动运行。

如果你想要更改现有存储的过程中的上一个迁移创建，你可以使用 Add-migration 命令生成的空白的迁移，然后手动编写调用的代码[AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx)方法.

## <a name="deploy-to-azure"></a>将部署到 Azure

此部分要求你已完成可选**将应用部署到 Azure**主题中[迁移和部署](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)的这一系列教程。 如果必须解决通过删除本地项目中的数据库的迁移错误，跳过此部分。

1. 在 Visual Studio 中，右键单击中的项目**解决方案资源管理器**和选择**发布**从上下文菜单。
2. 单击“发布” 。

    Visual Studio 将部署到 Azure，应用程序，并在默认浏览器中，在 Azure 中运行中打开应用程序。
3. 测试应用程序以验证它是否正常工作。

    第一次运行某个页面访问数据库，实体框架将运行所有迁移`Up`使数据库保持最新的当前数据模型所需的方法。 你现在可以使用所有自上次部署，包括在本教程中添加的部门页面以来添加的网页。

## <a name="summary"></a>摘要

在本教程中，您了解如何通过编写代码以异步方式执行，提高服务器的效率以及如何使用存储的过程，以插入、 更新和删除操作。 在下一步的教程中，你将了解如何在多个用户尝试同时编辑同一个记录时防止数据丢失。

在找不到其他实体框架资源的链接[ASP.NET 数据访问的推荐资源](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一页](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[下一页](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
