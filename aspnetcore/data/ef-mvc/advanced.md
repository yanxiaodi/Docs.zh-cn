---
title: "使用 EF Core 创建 ASP.NET Core MVC - 高级 - 第 10 个教程（共 10 个）"
author: tdykstra
description: "本教程介绍了除开发使用 Entity Framework Core 的 ASP.NET Web 应用程序的基本知识以外的几个主题，了解这些主题大有益处。"
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/advanced
ms.openlocfilehash: 458f2dc8a67f8c706d043f0d9d7cb7ce962e52ce
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2018
---
# <a name="advanced-topics---ef-core-with-aspnet-core-mvc-tutorial-10-of-10"></a>高级主题 - 使用 EF Core 创建 ASP.NET Core MVC 教程（第 10 个教程，共 10 个）

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University 示例 Web 应用程序演示如何使用 Entity Framework Core 和 Visual Studio 创建 ASP.NET Core MVC Web 应用程序。 若要了解系列教程，请参阅[本系列中的第一个教程](intro.md)。

之前的教程中，已经以每个类一张表的方式实现了继承。 本教程介绍了几个主题，如果使用 Entity Framework Core 开发 ASP.NET Core Web 应用程序时，要使用比基本知识更高阶的知识，这些主题大有益处。

## <a name="raw-sql-queries"></a>原始 SQL 查询

使用 Entity Framework 的优点之一是，它可以避免将代码与存储数据的特定方法过于紧密地绑定在一起。 它通过生成 SQL 查询和命令来实现这一点，这样也可让你免于亲自编写这些内容。 但需要运行手动创建的特定 SQL 查询时，会有例外。 对于这些情况，Entity Framework Code First API 包含了可用于将 SQL 命令直接传递到数据库的方法。 EF Core 1.0 中提供以下选项：

* 对返回实体类型的查询使用 `DbSet.FromSql` 方法。 返回对象的类型必须是符合 `DbSet` 对象需要的类型，并且数据库上下文会自动跟踪返回对象 [关闭跟踪](crud.md#no-tracking-queries)。

* 对非查询命令使用 `Database.ExecuteSqlCommand`。

如果需要运行返回非实体类型的查询，可将 ADO.NET 与 EF 提供的数据库连接一起使用。 即便使用此方法来检索实体类型，数据库上下文也不会跟踪返回的数据。

在 Web 应用程序中执行 SQL 命令时，请务必采取预防措施来保护站点免受 SQL 注入攻击。 一种方法是使用参数化查询，确保不会将网页提交的字符串视为 SQL 命令。 在本教程中，将用户输入集成到查询中时会使用参数化查询。

## <a name="call-a-query-that-returns-entities"></a>调用返回实体的查询

`DbSet<TEntity>` 类提供了一种方法，可用于执行返回 `TEntity` 类型的实体的查询。 若想查看其作用方式，需更改“院系”控制器的 `Details` 方法中的代码。

在 DepartmentsController.cs 的 `Details` 方法中，将检索院系的代码替换为 `FromSql` 方法调用，如以下突出显示状态的代码所示：

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

如需验证新代码是否正常工作，请选择“院系”选项卡，然后选择某一院系的“详细信息”。

![院系详细信息](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a>调用返回其他类型的查询

“关于”页面中显示了每个注册日期的学生数量，之前已为该页面创建了一个学生统计数据网格。 从 Students 实体集 (`_context.Students`) 获取数据，并使用 LINQ 将该结果投影到 `EnrollmentDateGroup` 视图模型对象的列表中。 假设你想编写 SQL 本身，而不是使用 LINQ。 为此，需运行返回非实体对象的 SQL 查询。 在 EF Core 1.0 中，实现此操作的一种方法是编写 ADO.NET 代码并从 EF 获取数据库连接。

在 HomeController.cs 中，将 `About` 方法替换为以下代码：

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

添加 using 语句：

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

运行应用并转到“关于”页。 页面中显示的数据与之前相同。

![“关于”页面](advanced/_static/about.png)

## <a name="call-an-update-query"></a>调用更新查询

假设 Contoso University 管理员想要在数据库中执行全局更改，例如更改每门课程的学分数。 如果该大学拥有大量课程，将课程全部当作实体进行检索并逐一更改效率很低。 本部分中将实现一个可让用户指定因素（用于更改所有课程的学分数）的网页，并通过执行 SQL UPDATE 语句进行更改。 该网页如下图所示：

![“更新课程学分”页面](advanced/_static/update-credits.png)

在 CoursesContoller.cs 中，为 HttpGet 和 HttpPost 添加 UpdateCourseCredits 方法：

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

控制器处理 HttpGet 请求时，`ViewData["RowsAffected"]` 中不会返回任何内容，并且视图中会显示一个空文本框和一个提交按钮，如上图所示。

单击“更新”按钮后，系统会调用 HttpPost 方法，并为乘数赋予在文本框中输入的值。 然后，代码执行更新课程的 SQL，并将受影响的行数返回给 `ViewData` 中的视图。 视图获得 `RowsAffected` 值时，即显示更新后的行数。

在“解决方案资源管理器”中，右键单击“Views/Courses”文件夹，然后依次单击“添加”和“新建项”。

在“添加新项”对话框中，单击左窗格中“已安装”下的“ASP.NET”，单击“MVC 视图页面”并将新视图命名为“UpdateCourseCredits.cshtml”。

在 Views/Courses/UpdateCourseCredits.cshtml 中，将模板代码替换为以下代码：

[!code-html[Main](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

通过选择“课程”选项卡运行 `UpdateCourseCredits` 方法，然后将“/UpdateCourseCredits”添加到浏览器地址栏中 URL 的末尾（例如 `http://localhost:5813/Courses/UpdateCourseCredits`）。 在文本框中输入数字：

![“更新课程学分”页面](advanced/_static/update-credits.png)

单击“更新” 。 随即显示受影响的行数：

![“更新课程学分”页面中受影响的行](advanced/_static/update-credits-rows-affected.png)

单击“返回列表”，查看具有修订后的学分数的课程列表。

请注意，生产代码可确保更新操作始终能生成有效数据。 此处显示的简化代码可将学分数放大到足够大，以生成 5 以上的数字。 （`Credits` 属性具有 `[Range(0, 5)]` 特性。）更新查询可以工作，但无效数据可能会导致系统中假设学分数小于或等于 5 的其他部分出现异常结果。

有关原始 SQL 查询的详细信息，请参阅[原始 SQL 查询](https://docs.microsoft.com/ef/core/querying/raw-sql)。

## <a name="examine-sql-sent-to-the-database"></a>检查发送到数据库的 SQL

有时，能够查看发送到数据库的实际 SQL 查询会有所帮助。 EF Core 会自动使用 ASP.NET Core 内置的日志记录功能来编写包含用于查询和更新的 SQL 的日志。 本部分中将介绍 SQL 日志记录的一些示例。

打开 StudentsController.cs，并在 `Details` 方法的 `if (student == null)` 语句上设置断点。

在调试模式下运行应用，并转到某位学生的“详细信息”页面。

转到显示调试输出的“输出”窗口，可看到以下查询：

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

可看到这里可能有意外内容：SQL 从 Person 表中选择的行数多达两行（`TOP(2)`。 `SingleOrDefaultAsync` 方法未解析为服务器上的一行。 原因如下：

* 如果查询返回多行，则该方法返回 NULL。
* 若要确定查询是否返回多行，EF 必须检查它是否至少返回两行。

请注意，无需使用调试模式，只需在断点处停止即可获取“输出”窗口中的日志记录输出。 这种方法非常便捷，只需在想查看输出时停止日志记录即可。 如果不进行此操作，程序将继续进行日志记录，要查看感兴趣的部分则必须向后滚动。

## <a name="repository-and-unit-of-work-patterns"></a>存储库和和工作单元模式

许多开发人员通过编写代码将存储库和工作单元模式实现为代码周围的包装器，与 Entity Framework 一起使用。 这些模式被用于在应用程序的数据访问层和业务逻辑层之间创建一个抽象层。 实现这些模式有助于使应用程序免受数据存储中所作更改的影响，而且有助于进行自动单元测试或测试驱动开发 (TDD)。 但是对使用 EF 的应用程序而言，通过编写附加代码来实现这些模式并不总是最佳选择，原因如下：

* EF 上下文类本身会将代码与数据存储特定的代码隔离开来。

* 对于使用 EF 进行的数据库更新，EF 上下文类可充当工作单元类。

* EF 包含了一些无需编写存储库代码即可实现 TDD 的功能。

若要了解如何实现存储库和工作单元模式，请参阅[本系列教程的 Entity Framework 5 版本](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)。

Entity Framework Core 实现可用于测试的内存中数据库提供程序。 有关详细信息，请参阅[使用 InMemory 进行测试](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory)。

## <a name="automatic-change-detection"></a>自动更改检测

Entity Framework 通过比较实体的当前值与原始值来确定实体发生的更改，从而确定需将哪些更改发送给数据库。 查询或附加实体时，将存储原始值。 导致自动更改检测的部分方法如下所示：

* DbContext.SaveChanges

* DbContext.Entry

* ChangeTracker.Entries

如果跟踪的实体数量巨大，且在循环中多次调用其中某种方法，则使用 `ChangeTracker.AutoDetectChangesEnabled` 属性临时关闭自动更改检测即可显著改善性能。 例如：

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a>Entity Framework Core 源代码和开发计划

Entity Framework Core 源代码位于 [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore)。 EF Core 储存库中包含夜间生成、问题跟踪、功能规范、设计会议备注和[后续开发的路线图](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap)。 你可以归档或查找 bug 并进行更改。

尽管源代码开源，但是 Entity Framework Core 也被视为 Microsoft 产品受到完全支持。 Microsoft Entity Framework 团队持续控制要接受的更改，并对所有代码更改进行测试来确保每个版本的质量。

## <a name="reverse-engineer-from-existing-database"></a>从现有数据库进行反向工程

若要从现有数据库对数据模型（如实体类）进行反向工程，请使用 [scaffold-dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) 命令。 请参阅[入门教程](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db)。

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a>使用动态 LINQ 简化排序选择代码

[本系列中的第三个教程](sort-filter-page.md)介绍了如何按照 `switch` 语句中的硬编码列名称编写 LINQ 代码。 如果只有两列可供选择，这种方法可行，但是如果拥有许多列，代码可能会变得冗长。 要解决该问题，可使用 `EF.Property` 方法将属性名称指定为一个字符串。 要尝试此方法，请将 `StudentsController` 中的 `Index` 方法替换为以下代码。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a>后续步骤

至此，在 ASP.NET MVC 应用程序中使用 Entity Framework Core 的系列教程已告一段落。

有关 EF Core 的详细信息，请参阅 [Entity Framework Core 文档](https://docs.microsoft.com/ef/core)。 另外也可参阅 [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action)（实际运用 Entity Framework Core）一书。

若要了解如何部署 Web 应用，请参阅[托管和部署](xref:host-and-deploy/index)。

有关 ASP.NET Core MVC 涉及的其他主题，例如身份验证和授权，请参阅 [ASP.NET Core 文档](https://docs.microsoft.com/aspnet/core/)。

## <a name="acknowledgments"></a>致谢

感谢 Tom Dykstra 和 Rick Anderson（twitter @RickAndMSFT）编写本教程。 感谢 Rowan Miller、Diego Vega 和 Entity Framework 小组的其他成员帮助进行代码评审，并帮助调试在编写本教程代码时出现的问题。

## <a name="common-errors"></a>常见错误  

### <a name="contosouniversitydll-used-by-another-process"></a>其他进程在使用 ContosoUniversity.dll

错误消息：

> 无法打开“...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' for writing -- ”进程无法访问文件“...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll”，因为它正在由其他进程使用。

解决方案：

在 IIS Express 中停止站点。 转到 Windows 系统任务栏，找到 IIS Express 并右键单击其图表，选择 Contoso University 网站，然后单击“停止站点”。

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a>Up 和 Down 方法中存在没有代码搭建基架的迁移

可能的原因：

EF CLI 命令不自动关闭和保存代码文件。 如果在运行 `migrations add` 命令时有尚未保存的代码，EF 将找不到该更改。

解决方案：

运行 `migrations remove` 命令，保存代码更改，然后重新运行 `migrations add` 命令。

### <a name="errors-while-running-database-update"></a>运行数据库更新时出错

在包含现有数据的数据库中更改架构时，可能会发生其他错误。 如果出现无法解决的迁移错误，可以在连接字符串中更改数据库名或者删除数据库。 如果是新数据库，则没有要迁移的数据，因此在完成更新数据库命令时很可能不会出错。

最简单的方法是重命名 appsettings.json 中的数据库。 下次运行 `database update` 时将创建一个新的数据库。

若要在 SSOX 中删除数据库，请右键单击该数据库，单击“删除”，然后在“删除数据库”对话框中选择“关闭现有连接”，然后单击“确定”。

若要使用 CLI 删除数据库，请运行 `database drop` CLI 命令：

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a>定位 SQL Server 实例时出错

错误消息：

> 建立到 SQL Server 的连接时出现与网络相关或特定于实例的错误。 未找到或无法访问服务器。 请验证实例名称是否正确，SQL Server 是否已配置为允许远程连接。 （提供程序：SQL 网络接口，错误：26 - 定位指定的服务器/实例时出错）

解决方案：

检查连接字符串。 如果已手动删除数据库文件，请更改构造字符串中数据库的名称，以使用新数据库重新开始。

>[!div class="step-by-step"]
[上一篇](inheritance.md)
