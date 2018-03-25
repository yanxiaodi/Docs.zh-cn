---
title: "Razor 页面和 Entity Framework Core - 第 1 个教程（共 8 个）"
author: rick-anderson
description: "介绍了如何使用 Entity Framework Core 创建 Razor 页面应用"
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/intro
ms.openlocfilehash: 091f34da347d52ba8e3e87779ddc4aeb790c2800
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2018
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a>Razor 页面和 Entity Framework Core 入门（使用 Visual Studio）（第 1 个教程，共 8 个）

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University 示例 Web 应用演示了如何使用 Entity Framework (EF) Core 2.0 和 Visual Studio 2017 创建 ASP.NET Core 2.0 MVC Web 应用程序。

该示例应用是一个虚构的 Contoso University 的网站。 其中包括学生录取、课程创建和讲师分配等功能。 本页是介绍如何构建 Contoso University 示例应用系列教程中的第一部分。

[下载或查看已完成的应用。](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [下载说明](xref:tutorials/index#how-to-download-a-sample)。

## <a name="prerequisites"></a>系统必备

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

熟悉 [Razor 页面](xref:mvc/razor-pages/index)。 新程序员在开始学习本系列之前，应先完成 [Razor 页面入门](xref:tutorials/razor-pages/razor-pages-start)。

## <a name="troubleshooting"></a>疑难解答

如果遇到无法解决的问题，通常可以通过将自己的代码与[已完成阶段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots)或[已完成项目](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final)进行比较来寻找解决方案。 有关常见错误列表及其解决办法，请参阅[本系列最后一个教程的疑难解答部分](xref:data/ef-mvc/advanced#common-errors)。 如果在该处找不到所需内容，可以在 [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) 上发表有关 [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) 或 [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core) 的问题。

> [!TIP]
> 本系列教程以之前教程中已完成的工作为基础。 每次成功完成教程后，请考虑保存项目副本。 如果遇到问题，便可以从上一教程重新开始，而无需从头开始。 或者可以下载[已完成阶段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots)并使用已完成阶段重新开始。

## <a name="the-contoso-university-web-app"></a>Contoso University Web 应用

这些教程中所构建的应用是一个基本的大学网站。

用户可以查看和更新学生、课程和讲师信息。 以下是在本教程中创建的几个屏幕。

![“学生索引”页](intro/_static/students-index.png)

![学生编辑页](intro/_static/student-edit.png)

此网站的 UI 样式与内置模板生成的 UI 样式类似。 教程的重点是 EF Core 和 Razor 页面，而非 UI。

## <a name="create-a-razor-pages-web-app"></a>创建 Razor 页面 Web 应用

* 从 Visual Studio“文件”菜单中选择“新建” > “项目”。
* 创建新的 ASP.NET Core Web 应用程序。 将该项目命名为 ContosoUniversity 。 务必将该项目命名为 ContosoUniversity，以便复制/粘贴代码时命名空间相匹配。
 ![新建 ASP.NET Core Web 应用程序](intro/_static/np.png)
* 在下拉列表中选择“ASP.NET Core 2.0”，然后选择“Web 应用程序”。
 ![Web 应用程序（Razor 页面）](../../mvc/razor-pages/index/_static/np2.png)

按 F5 在调试模式下运行应用，或按 Ctrl-F5 在运行（不附加调试器）

## <a name="set-up-the-site-style"></a>设置网站样式

设置网站菜单、布局和主页时需作少量更改。

打开 Pages/_Layout.cshtml 并进行以下更改：

* 将“ContosoUniversity”的每个匹配项都改为“Contoso University”。 共有三个匹配项。

* 添加 Students、Courses、Instructors 和 Departments 菜单项，并删除“联系人”菜单项。

突出显示所作更改。 （所有标记均不显示。）

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

在 Pages/Index.cshtml 中，将文件内容替换为以下代码，以将有关 ASP.NET 和 MVC 的文本替换为有关本应用的文本：

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

按 Ctrl+F5 运行项目。 将显示主页以及后续教程中创建的标签：

![Contoso University 主页](intro/_static/home-page.png)

## <a name="create-the-data-model"></a>创建数据模型

创建 Contoso University 应用的实体类。 从以下三个实体开始：

![Course-Enrollment-Student 数据模型关系图](intro/_static/data-model-diagram.png)

`Student` 和 `Enrollment` 实体之间存在一对多关系。 `Course` 和 `Enrollment` 实体之间存在一对多关系。 一名学生可以报名参加任意数量的课程。 一门课程中可以包含任意数量的学生。

以下部分将为这几个实体中的每一个实体创建一个类。

### <a name="the-student-entity"></a>Student 实体

![Student 实体关系图](intro/_static/student-entity.png)

创建 Models 文件夹。 在 Models 文件夹中，使用以下代码创建一个名为 Student.cs 的类文件：

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID` 属性成为此类对应的数据库 (DB) 表的主键列。 默认情况下，EF Core 将名为 `ID` 或 `classnameID` 的属性视为主键。

`Enrollments` 属性是导航属性。 导航属性链接到与此实体相关的其他实体。 在这种情况下，`Student entity` 的 `Enrollments` 属性包含与该 `Student` 相关的所有 `Enrollment` 实体。 例如，如果数据库中的 Student 行有两个相关的 Enrollment 行，则 `Enrollments` 导航属性包含这两个 `Enrollment` 实体。 相关的 `Enrollment` 行是 `StudentID` 列中包含该学生的主键值的行。 例如，假设 ID=1 的学生在 `Enrollment` 表中有两行。 `Enrollment` 表中有两行的 `StudentID` = 1。 `StudentID` 是 `Enrollment` 表中的外键，用于指定 `Student` 表中的学生。

如果导航属性包含多个实体，则导航属性必须是列表类型，例如 `ICollection<T>`。 可以指定 `ICollection<T>` 或诸如 `List<T>` 或 `HashSet<T>` 的类型。 使用 `ICollection<T>` 时，EF Core 会默认创建 `HashSet<T>` 集合。 包含多个实体的导航属性来自于多对多和一对多关系。

### <a name="the-enrollment-entity"></a>Enrollment 实体

![Enrollment 实体关系图](intro/_static/enrollment-entity.png)

在 Models 文件夹中，使用以下代码创建 Enrollment.cs：

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` 属性为主键。 `Student` 实体使用的是 `ID` 模式，而本实体使用的是 `classnameID` 模式。 通常情况下，开发者会选择一种模式并在整个数据模型中都使用该模式。 下一个教程将介绍如何使用不带类名的 ID，以便更轻松地在数据模型中实现集成。

`Grade` 属性为 `enum`。 `Grade` 类型声明后的问号表明 `Grade` 属性可为 NULL。 为 NULL 的年级与零年级不同，NULL 意味着该年级未知或尚未分配。

`StudentID` 属性是外键，其对应的导航属性为 `Student`。 `Enrollment` 实体与一个 `Student` 实体相关联，因此该属性只包含一个 `Student` 实体。 `Student` 实体与 `Student.Enrollments` 导航属性不同，后者包含多个 `Enrollment` 实体。

`CourseID` 属性是外键，其对应的导航属性为 `Course`。 `Enrollment` 实体与一个 `Course` 实体相关联。

如果属性命名为 `<navigation property name><primary key property name>`，EF Core 会将其视为外键。 例如 `Student` 导航属性的 `StudentID`，因为 `Student` 实体的主键为 `ID`。 还可以将外键属性命名为 `<primary key property name>`。 例如 `CourseID`，因为 `Course` 实体的主键为 `CourseID`。

### <a name="the-course-entity"></a>Course 实体

![Course 实体关系图](intro/_static/course-entity.png)

在 Models 文件夹中，使用以下代码创建 Course.cs：

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments` 属性是导航属性。 `Course` 实体可与任意数量的 `Enrollment` 实体相关。

应用可以通过 `DatabaseGenerated` 特性指定主键，而无需靠数据库生成。

## <a name="create-the-schoolcontext-db-context"></a>创建 SchoolContext 数据库上下文

数据库上下文类是为给定数据模型协调 EF Core 功能的主类。 数据上下文派生自 `Microsoft.EntityFrameworkCore.DbContext`。 数据上下文指定数据模型中包含哪些实体。 在此项目中，类命名为 `SchoolContext`。

在项目文件夹中，创建一个名为 Data 的文件夹。

在 Data 文件夹中，使用以下代码创建 SchoolContext.cs：

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

此代码会为每个实体集创建一个 `DbSet` 属性。 在 EF Core 术语中：

* 实体集通常对应一个数据库表。
* 实体对应表中的行。

`DbSet<Enrollment>` 和 `DbSet<Course>` 可以省略。 EF Core 隐式包含了它们，因为 `Student` 实体引用 `Enrollment` 实体，而 `Enrollment` 实体引用 `Course` 实体。 在本教程中，将 `DbSet<Enrollment>` 和 `DbSet<Course>` 保留在 `SchoolContext` 中。

创建数据库时，EF Core 会创建名称与 `DbSet` 属性名相同的表。 集合的属性名通常采用复数形式（使用 Students，而不使用 Student）。 开发者对表名称是否应为复数形式意见不一。 在这些教程中，在 DbContext 中指定单数形式的表名称会覆盖默认行为。 若要指定单数形式的表名称，请添加以下突出显示的代码：

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>通过依赖关系注入注册上下文

ASP.NET Core 包含[依赖关系注入](xref:fundamentals/dependency-injection)。 服务（例如 EF Core 数据库上下文）在应用程序启动期间通过依赖关系注入进行注册。 需要这些服务（如 Razor 页面）的组件通过构造函数提供相应服务。 本教程的后续部分介绍了用于获取数据库上下文实例的构造函数代码。

要将 `SchoolContext` 注册为服务，请打开 Startup.cs，并将突出显示的行添加到 `ConfigureServices` 方法。

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

通过调用 `DbContextOptionsBuilder` 对象上的方法，可将连接字符串的名称传递给上下文。 进行本地开发时，[ASP.NET Core 配置系统](xref:fundamentals/configuration/index)会从 appsettings.json 文件读取连接字符串。

为 `ContosoUniversity.Data` 和 `Microsoft.EntityFrameworkCore` 命名空间添加 `using` 语句。 生成项目。

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

打开 appsettings.json 文件，并按以下代码所示添加连接字符串：

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

上述连接字符串使用 `ConnectRetryCount=0` 来防止 [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) 挂起。

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

连接字符串指定 SQL Server LocalDB 数据库。 LocalDB 是轻型版本 SQL Server Express 数据库引擎，专门针对应用开发，而非生产使用。 LocalDB 按需启动并在用户模式下运行，因此没有复杂的配置。 默认情况下，LocalDB 会在 `C:/Users/<user>` 目录中创建 .mdf 数据库文件。

## <a name="add-code-to-initialize-the-db-with-test-data"></a>添加代码，以使用测试数据初始化该数据库

EF Core 会创建一个空的数据库。 本部分中编写了 Seed 方法来使用测试数据填充该数据库。

在 Data 文件夹中，新建一个名为 DbInitializer.cs 的类文件，并添加以下代码：

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

该代码会检查数据库中是否存在任何学生。 如果数据库中没有任何学生，则会填充测试数据。 它会将测试数据加载到数组中，而不是 `List<T>` 集合中，以便优化性能。

`EnsureCreated` 方法自动为数据库上下文创建数据库。 如果数据库已存在，则返回 `EnsureCreated`，并且不修改数据库。

在 Program.cs 中，修改 `Main` 方法以执行以下操作：

* 从依赖关系注入容器获取数据库上下文实例。
* 调用 seed 方法，并将上下文传递给它。
* Seed 方法完成时释放上下文。

下面的代码显示更新后的 Program.cs 文件。

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

第一次运行该应用时，会使用测试数据创建并填充数据库。 更新数据模型时：
* 删除数据库。
* 更新 seed 方法。
* 运行该应用，并创建新的种子数据库。 

后续教程中，在更改数据模型时会更新该数据库，而不会删除并重新创建该数据库。

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a>添加基架工具

本部分中将使用包管理器控制台 (PMC) 来添加 Visual Studio Web 代码生成包。 必须添加此包才能运行基架引擎。

从“工具”菜单中，选择“NuGet 包管理器” > “包管理器控制台”。

在包管理器控制台 (PMC) 中输入以下命令：

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

前一个命令将 NuGet 包添加到 *.csproj 文件中：

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a>构架模型

* 打开项目目录（包含 Program.cs、Startup.cs 和 .csproj 文件的目录）中的命令窗口。
* 运行以下命令：


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
如果生成以下错误：

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

再次运行该命令，并在页面底部添加批注。

如果收到错误：
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

打开项目目录（包含 Program.cs、Startup.cs 和 .csproj 文件的目录）中的命令窗口。


生成项目。 此版本生成如下错误：

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 将 `_context.Student` 全局更改为 `_context.Students`（即向 `Student` 添加一个“s”）。 找到并更新 7 个匹配项。 我们计划在下一版本中修复[此 bug](https://github.com/aspnet/Scaffolding/issues/633)。

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a>测试应用

运行应用并选择“学生”链接。 “学生”链接将显示在页面顶部，具体取决于浏览器宽度。 如果看不到“学生”链接，请单击右上角的导航图标。

![Contoso University 主页宽窄](intro/_static/home-page-narrow.png)

测试“创建”，“编辑”和“详细信息”链接。

## <a name="view-the-db"></a>查看数据库

启动应用时，`DbInitializer.Initialize` 会调用 `EnsureCreated`。 `EnsureCreated` 检测数据库是否存在，并在必要时创建一个数据库。 如果数据库中没有学生，`Initialize` 方法将添加学生。

从 Visual Studio 中的“视图”菜单打开 SQL Server 对象资源管理器 (SSOX)。
在 SSOX 中，单击“(localdb)\MSSQLLocalDB”>“数据库”>“ContosoUniversity1”。

展开“表”节点。

右键单击 Student 表，然后单击“查看数据”，以查看创建的列和插入到表中的行。

.mdf 和 .ldf 数据库文件位于 C:\Users\\<yourusername> 文件夹中。

启动应用时会调用 `EnsureCreated`，以进行以下工作流：

* 删除数据库。
* 更改数据库架构（例如添加一个 `EmailAddress` 字段）。
* 运行应用。

`EnsureCreated` 创建一个带有 `EmailAddress` 列的数据库。

## <a name="conventions"></a>约定

因为使用了约定或 EF Core 所做的假设，为使 EF Core 创建完整数据库而编写的代码量是最小的。

* 使用 `DbSet` 属性的名称作为表名。 若是未被 `DbSet` 属性引用的实体，则使用实体类名作为表名。

* 使用实体属性名作为列名。

* 以 ID 或 classnameID 命名的实体属性被视为主键属性。

* 如果属性命名为 <navigation property name><primary key property name>，将视为外键属性（例如 `Student` 导航属性的 `StudentID`，因为 `Student` 实体的主键为 `ID`）。 可将外键属性命名为 <primary key property name>（例如 `EnrollmentID`，因为 `Enrollment` 实体的主键为 `EnrollmentID`）。

可以重写常规行为。 例如，可以显式指定表名，如本教程中前面部分所示。 可以显式设置列名。 可以显式设置主键和外键。

## <a name="asynchronous-code"></a>异步代码

异步编程是 ASP.NET Core 和 EF Core 的默认模式。

Web 服务器可用的线程数量有限，在高负载情况下，所有可用的线程可能都正在使用。 在这种情况下，线程释放之前，服务器无法处理新的请求。 若使用同步代码，许多线程在等待 I/O 完成时实际上并未执行任何操作，但仍被占用。 若使用异步代码，进程在等待 I/O 完成时其线程会被释放，以便服务器用于处理其他请求。 因此，使用异步代码可以更有效地利用服务器资源，并且可以让服务器在没有延迟的情况下处理更多流量。

异步代码会在运行时引入少量开销。 流量较低时，对性能的影响可以忽略不计，但流量较高时，潜在的性能改善非常显著。

在以下代码中，`async` 关键字、`Task<T>` 返回值、`await` 关键字和 `ToListAsync` 方法让代码异步执行。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* `async` 关键字让编译器执行以下操作：

  * 为方法主体的各部分生成回调。
  * 自动创建返回的 [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) 对象。 有关详细信息，请参阅[任务返回类型](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType)。

* 隐式返回类型 `Task` 表示正在进行的工作。

* `await` 关键字让编译器将该方法拆分为两个部分。 第一部分以异步方式启动的操作结束。 第二部分被放入一个回调方法中，当操作完成时调用。

* `ToListAsync` 是 `ToList` 扩展方法的异步版本。

编写使用 EF Core 的异步代码时需要注意的一些事项：

* 只会异步执行导致查询或命令被发送到数据库的语句。 这包括 `ToListAsync`、`SingleOrDefaultAsync`、`FirstOrDefaultAsync` 和 `SaveChangesAsync`。 不包括只会更改 `IQueryable` 的语句，例如 `var students = context.Students.Where(s => s.LastName == "Davolio")`。

* EF Core 上下文并非线程安全：请勿尝试并行执行多个操作。 

* 若要利用异步代码的性能优势，请验证在调用向数据库发送查询的 EF Core 方法时，库程序包（如用于分页）是否使用异步。

有关在 .NET 中进行异步编程的详细信息，请参阅[异步概述](https://docs.microsoft.com/dotnet/articles/standard/async)。

下一个教程将介绍基本的 CRUD（创建、读取、更新、删除）操作。

>[!div class="step-by-step"]
[下一篇](xref:data/ef-rp/crud)
