---
title: "ASP.NET Core MVC 和 Entity Framework Core - 第 1 个教程，共 10 个教程"
author: tdykstra
description: 
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/intro
ms.openlocfilehash: 7de43a390ee0e11f6eda811b0774343ab330c53b
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-entity-framework-core-using-visual-studio-1-of-10"></a>ASP.NET Core MVC 和 Entity Framework Core 入门 (使用 Visual Studio) (1 / 10)

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

[此处](xref:data/ef-rp/intro) 提供了本教程的 Razor 页版本。 Razor 页面版本更加通俗易懂，并介绍了更多 EF 功能。 我们建议你学习 [本教程的 Razor 页版本](xref:data/ef-rp/intro)。

Contoso 大学示例 web 应用程序演示如何使用 Entity Framework (EF) Core 2.0 和 Visual Studio 2017 创建 ASP.NET Core 2.0 MVC web 应用程序。

示例应用程序供一个虚构的 Contoso 大学网站使用。 它包括诸如学生入学、 课程创建和导师分配等功能。 这是一系列教程中的第一个，这一系列教程主要展示了如何从零开始构建 Contoso 大学示例应用程序。

[下载或查看已完成的应用程序。](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

EF Core 2.0 是 EF 的最新版本，但还没有包括 EF 6.x 的所有功能 。 有关如何在 EF 6.x 和 EF Core 之间选择，请参阅 [ EF Core vs.EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/)。 如果你选择使用 EF 6.x，请参阅[ 本系列教程的上一个版本](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application)。

> [!NOTE]
> * 本教程的 ASP.NET Core 1.1 版本，请参阅 [本教程中 VS 2017 Update 2 版本的 PDF 文档](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf)。
> * 有关本教程的 Visual Studio 2015 版本，请参阅 [ASP.NET Core 文档 VS 2015 版本的 PDF 文档](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf)。

## <a name="prerequisites"></a>系统必备

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a>疑难解答

如果遇到无法解决的问题，可以通过与 [已完成的项目](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)对比代码来查找解决方案。 常见错误以及对应的解决方案，请参阅 [最新教程中的故障排除](advanced.md#common-errors)。 如果没有找到遇到的问题的解决方案，可以将问题发布到StackOverflow.com 的 [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) 或 [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core) 版块。

> [!TIP] 
> 这是一系列一共有十个教程，其中每个都是在前面教程已完成的基础上继续。 请考虑在完成每一个教程后保存项目的副本。 之后如果遇到问题，你可以从保存的副本中开始寻找问题，而不是从头开始。

## <a name="the-contoso-university-web-application"></a>Contoso University Web 应用程序

你将在这些教程中学习构建一个简单的大学网站的应用程序。

用户可以查看和更新学生、 课程和教师信息。 以下是一些你即将创建的页面。

![“学生索引”页](intro/_static/students-index.png)

![学生编辑页](intro/_static/student-edit.png)

本教程主要关注于如何使用 Entity Framework , 所以此站点的UI样式都是直接套用内置的模板。

## <a name="create-an-aspnet-core-mvc-web-application"></a>创建 ASP.NET Core MVC web 应用程序

打开 Visual Studio 并创建一个新 ASP.NET Core C# web 项目名为"ContosoUniversity"。

* 从“文件”菜单中选择“新建”>“项目”。

* 从左窗格中依次选择“已安装”>“Visual C#”>“Web”。

* 选择“ASP.NET Core Web 应用程序”项目模板。

* 输入“ContosoUniversity”作为名称，然后单击“确定”。

  ![“新建项目”对话框](intro/_static/new-project.png)

* 等待 **新 ASP.NET Core Web 应用程序 (.NET Core)** 显示对话框

* 选择 **ASP.NET Core 2.0** 和 **Web 应用程序 （模型-视图-控制器）** 模板。

  **注意：** 本教程需要安装 ASP.NET Core 2.0 和 EF Core 2.0 或更高版本-请确保 **ASP.NET Core 1.1** 未选中。

* 请确保 **身份验证** 设置为  **不进行身份验证**。

* 单击“确定” 

  ![“新建 ASP.NET 项目”对话框](intro/_static/new-aspnet.png)

## <a name="set-up-the-site-style"></a>设置网站样式

通过几个简单的更改设置站点菜单、 布局和主页。

打开 Views/Shared/_Layout.cshtml 并进行以下更改：

* 将文件中的"ContosoUniversity"更改为"Contoso University"。 需要更改三个地方。

* 添加菜单项 **Students**，**Courses**，**Instructors**，和 **Department**，并删除 **Contact**菜单项。

突出显示所作更改。

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,30,36-39,48)]

在 *Views/Home/Index.cshtml*，将文件的内容替换为以下代码以将有关 ASP.NET 和 MVC 的内容替换为有关此应用程序的内容：

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

按 CTRL + F5 来运行该项目或从菜单选择 **调试 > 开始执行(不调试)**。 你会看到首页，以及通过这个教程创建的页对应的选项卡。

![Contoso University 主页](intro/_static/home-page.png)

## <a name="entity-framework-core-nuget-packages"></a>Entity Framework Core NuGet 包

若要为项目添加 EF Core 支持，需要安装相应的数据库驱动包。 本教程使用 SQL Server，相关驱动包[Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)。 该包包含在 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) 包中，因此不需要手动安装。

此包和其依赖项 (`Microsoft.EntityFrameworkCore` 和 `Microsoft.EntityFrameworkCore.Relational`) 一起提供 EF 的运行时支持。 你将在之后的 [迁移](migrations.md) 教程中学习添加工具包。 

有关其他可用于 EF Core 的数据库驱动的信息，请参阅 [数据库驱动](https://docs.microsoft.com/ef/core/providers/)。

## <a name="create-the-data-model"></a>创建数据模型

接下来你将创建 Contoso 大学应用程序的实体类。 你将从以下三个实体类开始。

![Course-Enrollment-Student 数据模型关系图](intro/_static/data-model-diagram.png)

`Student` 和 `Enrollment`实体之间是一对多的关系，`Course` 和`Enrollment` 实体之间也是一个对多的关系。 换而言之，一名学生可以修读任意数量的课程, 并且某一课程可以被任意数量的学生修读。

接下来，你将创建与这些实体对应的类。

### <a name="the-student-entity"></a>Student 实体

![Student 实体关系图](intro/_static/student-entity.png)

在 *Models* 文件夹中，创建一个名为 *Student.cs* 的类文件并且将模板代码替换为以下代码。

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID` 属性将成为对应于此类的数据库表中的主键。 默认情况下，EF 将会将名为 `ID` 或 `classnameID` 的属性解析为主键。

`Enrollments` 属性是导航属性。 导航属性中包含与此实体相关的其他实体。 在这个案例下，`Student entity` 中的 `Enrollments` 属性会保留所有与 `Student` 实体相关的 `Enrollment`。 换而言之，如果在数据库中有两行描述同一个学生的修读情况 （两行的 StudentID 值相同，而且 StudentID 作为外键和某位学生的主键值相同）， `Student` 实体的 `Enrollments` 导航属性将包含那两个 `Enrollment` 实体。

如果导航属性可以具有多个实体 （如多对多或一对多关系），那么导航属性的类型必须是可以添加、 删除和更新条目的容器，如 `ICollection<T>`。 你可以指定 `ICollection<T>` 或实现该接口类型，如 `List<T>` 或 `HashSet<T>`。 如果指定 `ICollection<T>`，EF在默认情况下创建 `HashSet<T>` 集合。

### <a name="the-enrollment-entity"></a>Enrollment 实体

![修读实体关系图](intro/_static/enrollment-entity.png)

在 *Models* 文件夹中，创建 *Enrollment.cs* 并且用以下代码替换现有代码：

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` 属性将被设为主键; 此实体使用 `classnameID` 模式而不是如 `Student` 实体那样直接使用 `ID`。 通常情况下，你选择一个主键模式，并在你的数据模型自始至终使用这种模式。 在这里，使用了两种不同的模式只是为了说明你可以使用任一模式来指定主键。 在 [后面的教程](inheritance.md)，你将了解到使用`ID`这种模式可以更轻松地在数据模型之间实现继承。

`Grade` 属性是 `enum`。 `Grade` 声明类型后的`?`表示 `Grade` 属性可以为 null。 评级为 null 和评级为零是有区别的 --null 意味着评级未知或者尚未分配。

`StudentID` 属性是一个外键，`Student` 是与其且对应的导航属性。 `Enrollment` 实体与一个 `Student` 实体相关联，因此该属性只包含单个 `Student` 实体 (与前面所看到的 `Student.Enrollments` 导航属性不同后，`Student`中可以容纳多个 `Enrollment` 实体)。

`CourseID` 属性是一个外键， `Course` 是与其对应的导航属性。 `Enrollment` 实体与一个 `Course` 实体相关联。

如果一个属性名为 `<navigation property name><primary key property name>`，Entity Framework 就会将这个属性解析为外键属性(例如， `Student` 实体的主键是`ID`， `Student` 是`Enrollment`的导航属性所以`Enrollment`实体中 `StudentID` 会被解析为外键)。 此外还可以将需要解析为外键的属性命名为 `<primary key property name>` (例如，`CourseID` 由于 `Course` 实体的主键所以 `CourseID` 也被解析为外键)。

### <a name="the-course-entity"></a>Course 实体

![Course 实体关系图](intro/_static/course-entity.png)

在 *Models* 文件夹中，创建 *Course.cs* 并且用以下代码替换现有代码：

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments` 属性是导航属性。 一个 `Course` 体可以与任意数量的 `Enrollment` 实体相关。

我们在本系列 [后面的教程](complex-data-model.md) 中会有更多有关 `DatabaseGenerated` 特性的例子。 简单来说，此特性让你能自行指定主键，而不是让数据库自动指定主键。

## <a name="create-the-database-context"></a>创建数据库上下文

使得给定的数据模型与 Entity Framework 功能相协调的主类是数据库上下文类。 可以通过继承 `Microsoft.EntityFrameworkCore.DbContext` 类的方式创建此类。 在该类中你可以指定数据模型中包含哪些实体。 你还可以定义某些 Entity Framework 行为。 在此项目中将数据库上下文类命名为 `SchoolContext`。

在项目文件夹中，创建名为的文件夹 *Data*。

在 *Data* 文件夹创建名为 *SchoolContext.cs* 的类文件，并将模板代码替换为以下代码：

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

此代码将为每个实体集创建 `DbSet` 属性。 在 Entity Framework 中，实体集通常与数据表相对应，具体实体与表中的行相对应。

在这里可以省略 `DbSet<Enrollment>` 和 `DbSet<Course>` 语句，实现的功能没有任何改变。 Entity Framework 会隐式包含这两个实体因为 `Student` 实体引用了 `Enrollment` 实体、`Enrollment` 实体引用了 `Course` 实体。

当数据库创建完成后， EF 创建一系列数据表，表名默认和 `DbSet` 属性名相同。 集合属性的名称一般使用复数形式，但不同的开发人员的命名习惯可能不一样，开发人员根据自己的情况确定是否使用复数形式。 在定义 DbSet 属性的代码之后，添加下面高亮代码，对 DbContext 指定单数的表名来覆盖默认的表名。 此教程在最后一个 DbSet 属性之后，添加以下高亮显示的代码。

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>使用依赖注入注册上下文

ASP.NET Core 默认实现 [依赖注入](../../fundamentals/dependency-injection.md)。 在应用程序启动过程通过依赖注入注册相关服务 （例如 EF 数据库上下文）。 需要这些服务的组件 （如 MVC 控制器） 可以通过向构造函数添加相关参数来获得对应服务。 在本教程后面你将看到控制器构造函数的代码，就是通过上述方式获得上下文实例。

若要将 `SchoolContext` 注册为一种服务，打开 *Startup.cs* ，并将高亮代码添加到 `ConfigureServices` 方法中。

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

通过调用 `DbContextOptionsBuilder` 中的一个方法将数据库连接字符串在配置文件中的名称传递给上下文对象。 进行本地开发时， [ASP.NET Core 配置系统](xref:fundamentals/configuration/index) 在 *appsettings.json* 文件中读取数据库连接字符串。

添加 `using` 语句引用 `ContosoUniversity.Data` 和 `Microsoft.EntityFrameworkCore` 命名空间，然后生成项目。

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

打开 appsettings.json 文件，并如以下示例所示添加连接字符串。

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

数据库连接字符串指定使用 SQL Server LocalDB 数据库。 LocalDB 是 SQL Server Express 数据库引擎的轻量级版本，用于应用程序开发，不在生产环境中使用。 LocalDB 作为按需启动并在用户模式下运行的轻量级数据库没有复杂的配置。 默认情况下， LocalDB 在 `C:/Users/<user>` 目录下创建 *.mdf* 数据库文件。

## <a name="add-code-to-initialize-the-database-with-test-data"></a>添加代码以使用测试数据初始化数据库

Entity Framework 已经为你创建了一个空数据库。 在本部分中，你将编写一个方法用于向数据库填充测试数据，该方法会在数据库创建完成之后执行。

此处将使用 `EnsureCreated` 方法来自动创建数据库。 在 [后面的教程](migrations.md) 你将了解如何通过使用 Code First Migration 来更改而不是删除并重新创建数据库来处理模型更改。

在 *Data* 文件夹中，创建名为的新类文件 *DbInitializer.cs* 并且将模板代码替换为以下代码，使得在需要时能创建数据库并向其填充测试数据。

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

这段代码首先检查是否有学生数据在数据库中，如果没有的话，就可以假定数据库是新建的，然后使用测试数据进行填充。 代码中使用数组存放测试数据而不是使用 `List<T>` 集合是为了优化性能。

在 *Program.cs*，修改 `Main` 方法，使得在应用程序启动时能执行以下操作：

* 从依赖注入容器中获取数据库上下文实例。
* 调用 seed 方法，将上下文传递给它。
* Seed 方法完成此操作时释放上下文。

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

添加 `using` 语句：

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Usings)]

在旧版教程中，你可能会在 *Startup.cs* 中的 `Configure` 方法看到类似的代码。 我们建议你只在为了设置请求管道时使用 `Configure` 方法。 将应用程序启动代码放入 `Main` 方法。

现在首次运行该应用程序，创建数据库并使用测试数据作为种子数据。 每当你更改数据模型时，可以删除数据库、 更新你的 Initialize 方法，然后使用上述方式更新新数据库。 在之后的教程中，你将了解如何在数据模型更改时，只需修改数据库而无需删除重建数据库。

## <a name="create-a-controller-and-views"></a>创建控制器和视图

接下来，将使用 Visual Studio 中的基架引擎添加一个 MVC 控制器，以及使用 EF 来查询和保存数据的视图。

CRUD 操作方法和视图的自动创建被称为基架。 基架与代码生成不同，基架的代码是一个起点，可以修改基架以满足自己需求，而你通常无需修改生成的代码。 当你需要自定义生成代码时，可使用一部分类或需求发生变化时重新生成代码。

* 右键单击 **解决方案资源管理器** 中的 **Controllers** 文件夹选择 **添加 > 新搭建基架的项目**。

如果出现“添加 MVC 依赖项”对话框：

* [将 Visual Studio 更新到最新版本](https://www.visualstudio.com/downloads/)。 15.5 之前的 Visual Studio 版本显示此对话框。
* 如果无法更新，请选择“添加”，然后再次按照添加控制器步骤操作。

* 在“添加基架”对话框中：

  * 选择 **视图使用 Entity Framework 的 MVC 控制器**。

  * 单击 **添加**。

* 在“添加控制器”对话框中：

  * 在 **模型类** 选择 **Student**。

  * 在“数据上下文类”中选择 SchoolContext。

  * 使用 **StudentsController** 作为默认名称。

  * 单击 **添加**。

  ![构架 Student](intro/_static/scaffold-student.png)

  当你单击 **添加** 后，Visual Studio 基架引擎创建 *StudentsController.cs* 文件和一组对应于控制器的视图 (*.cshtml* 文件) 。

（如果你之前手动创建数据库上下文，基架引擎还可以自动创建。 你可以在 **添加控制器** 对话框中单击右侧的加号框 **数据上下文类** 来指定在一个新上下文类。  然后，Visual Studio 将创建你的 `DbContext`，控制器和视图类。）

注意控制器将 `SchoolContext` 作为构造函数参数。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

ASP.NET 依赖注入机制会传递一个 `SchoolContext` 实例到控制器。 在前面的教程中已经通过修改 *Startup.cs* 文件来配置注入规则。

控制器包含 `Index` 操作方法，用于显示数据库中的所有学生。 该方法从学生实体集中获取学生列表，学生实体集则是通过读取数据库上下文实例中的 `Students` 属性获得：

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

本教程后面部分将介绍此代码中的异步编程元素。

*Views/Students/Index.cshtml* 视图使用table标签显示此列表：

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

按 CTRL + F5 来运行该项目，或从菜单选择 **调试 > 开始执行(不调试)**。

单击学生选项卡以查看 `DbInitializer.Initialize` 插入的测试的数据。 你将看到 `Student` 选项卡链接在页的顶部或在单击右上角后的导航图标中，具体显示在哪里取决于浏览器窗口宽度。

![Contoso University 主页宽窄](intro/_static/home-page-narrow.png)

![“学生索引”页](intro/_static/students-index.png)

## <a name="view-the-database"></a>查看数据库

当你启动了应用程序，`DbInitializer.Initialize` 方法调用 `EnsureCreated`。 EF 没有检测到相关数据库，因此自己创建了一个，接着 `Initialize` 方法的其余代码向数据库中填充数据。 你可以使用 Visual Studio 中的 **SQL Server 对象资源管理器** (SSOX) 查看数据库。

关闭浏览器。

如果 SSOX 窗口尚未打开，请从Visual Studio 中的**视图** 菜单中选择。

在 SSOX 中，单击 **(localdb) \MSSQLLocalDB > 数据库**，然后单击和 *appsettings.json* 文件中的连接字符串对应的数据库。

展开“表”节点，查看数据库中的表。

![SSOX 中的表](intro/_static/ssox-tables.png)

右键单击 **Student** 表，然后单击 **查看数据**，即可查看已创建的列和已插入到表的行。

![SSOX 中的 Student 表](intro/_static/ssox-student-table.png)

.mdf 和 .ldf 数据库文件位于 C:\Users\<yourusername> 文件夹中。

因为调用 `EnsureCreated` 的初始化方法在启动应用程序时才运行，所以在这之前你可以更改 `Student` 类、 删除数据库、 再运行一次应用程序，这时候数据库将自动重新创建，以匹配所做的更改。 例如，如果向 `Student` 类添加 `EmailAddress` 属性，重新的创建表中会有 `EmailAddress` 列。

## <a name="conventions"></a>约定

由于 Entity Framwork 有一定的约束条件，你只需要按规则编写很少的代码就能够创建一个完整的数据库。

* `DbSet` 类型的属性用作表名。 如果实体未被 `DbSet` 属性引用，实体类名称用作表名称。

* 使用实体属性名作为列名。

* 以 ID 或 classnameID 命名的实体属性被视为主键属性。

* 如果属性名为 *<navigation property name><primary key property name>*将被解释为外键属性 (例如，`StudentID` 对应 `Student` 导航属性，`Student` 实体的主键是`ID`，所以`StudentID`被解释为外键属性)。 此外也可以将外键属性命名为 *<primary key property name>* (例如，`EnrollmentID`，由于 `Enrollment` 实体的主键是 `EnrollmentID`，因此被解释为外键)。

约定行为可以重写。 例如，本教程前半部分显式指定表名称。 本系列 [后面教程](complex-data-model.md) 则设置列名称并将任何属性设置为主键或外键。

## <a name="asynchronous-code"></a>异步代码

异步编程是 ASP.NET Core 和 EF Core 的默认模式。

Web 服务器的可用线程是有限的，而在高负载情况下的可能所有线程都被占用。 当发生这种情况的时候，服务器就无法处理新请求，直到线程被释放。 使用同步代码时，可能会出现多个线程被占用但不能执行任何操作的情况，因为它们正在等待 I/O 完成。 使用异步代码时，当进程正在等待 I/O 完成，服务器可以将其线程释放用于处理其他请求。 因此，异步代码使得服务器更有效地使用资源，并且该服务器可以无延迟地处理更多流量。

异步代码在运行时，会引入的少量开销，在低流量时对性能的影响可以忽略不计，但在针对高流量情况下潜在的性能提升是可观的。

在以下代码中，`async` 关键字、`Task<T>` 返回值、`await` 关键字和 `ToListAsync` 方法让代码异步执行。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* `async` 关键字用于告知编译器该方法主体将生成回调并自动创建 `Task<IActionResult>` 返回对象。

* 返回类型 `Task<IActionResult>` 表示正在进行的工作返回的结果为 `IActionResult` 类型。

* `await` 关键字会使得编译器将方法拆分为两个部分。 第一部分是以异步方式结束已启动的操作。 第二部分是当操作完成时注入调用回调方法的地方。

* `ToListAsync` 是 `ToList` 方法的的异步扩展版本。

使用 Entity Framework 编写异步代码时的一些注意事项：

* 只有导致查询或发送数据库命令的语句才能以异步方式执行。 包括 `ToListAsync`， `SingleOrDefaultAsync`，和 `SaveChangesAsync`。 不包括只需更改 `IQueryable` 的语句，如 `var students = context.Students.Where(s => s.LastName == "Davolio")`。

* EF 上下文是线程不安全的： 请勿尝试并行执行多个操作。 当调用异步 EF 方法时，始终使用 `await` 关键字。

* 如果你想要利用异步代码的性能优势，请确保你所使用的任何库和包在它们调用导致 Entity Framework 数据库查询方法时也使用异步。

有关在 .NET 异步编程的详细信息，请参阅 [异步概述](https://docs.microsoft.com/dotnet/articles/standard/async)。

## <a name="summary"></a>总结

你现在已创建了一个使用 Entity Framework Core 和 SQL Server Express LocalDB 来存储和显示数据的简单应用程序。 在下一个教程中，你将学习如何执行基本的 CRUD （创建、 读取、 更新、 删除） 操作。

>[!div class="step-by-step"]
[下一篇](crud.md)
