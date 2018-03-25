---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: "MVC 5 Web 应用程序 (12 的 12) 的高级实体框架 6 方案 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 的 ASP.NET MVC 5 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 85276377671b96e65406639c8584d9ebf8d77ff7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="advanced-entity-framework-6-scenarios-for-an-mvc-5-web-application-12-of-12"></a>MVC 5 Web 应用程序 (12 的 12) 的高级的实体框架 6 方案
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下载 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


在以前的教程，你实现的每个层次结构一个表继承。 本教程包括以下引入了几个有用需要注意的时要考虑的因素的基础知识的开发使用 Entity Framework Code First 的 ASP.NET web 应用程序的主题。 分步说明将引导你完成的代码和使用 Visual Studio 的下列主题：

- [执行原始的 SQL 查询](#rawsql)
- [执行无跟踪查询](#notracking)
- [检查 SQL 发送到数据库](#sql)

本教程介绍与跟对资源的详细信息的链接的简要介绍几个主题：

- [存储库和单元的工作模式](#repo)
- [代理类](#proxies)
- [自动更改检测](#changedetection)
- [自动验证](#validation)
- [Visual Studio 的 EF 工具](#tools)
- [实体框架的源代码](#source)

本教程还包括以下各节：

- [摘要](#summary)
- [确认](#acknowledgments)
- [有关 VB 的注意事项](#vb)
- [常见错误，以及解决方案或解决办法它们](#errors)

对于大多数这些主题，你将使用已创建的页。 若要使用原始 SQL 执行批量更新将创建一个新页更新的数据库中的所有课程的信用额度数：

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<a id="rawsql"></a>
## <a name="performing-raw-sql-queries"></a>执行原始 SQL 查询

实体框架代码的第一个 API 包括使您能够 SQL 命令将直接传递到数据库的方法。 有下列选项：

- 使用[DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx)返回实体类型的查询的方法。 返回的对象必须是期望的类型的`DbSet`对象，并且它们是否自动跟踪数据库上下文中的除非关闭跟踪。 (参见下一节[AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx)方法。)
- 使用[Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx)方法用于返回不是实体类型的查询。 返回的数据不跟踪数据库上下文中，即使你使用此方法来检索实体类型。
- 使用[Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx)非查询命令。

使用实体框架的优点之一是它可避免将你过于仔细存储数据的特定方法的代码。 此，它会生成 SQL 查询和命令，其中还使你无需自行编写。 但没有特殊情况下当你需要运行特定的 SQL 查询，你已手动创建的并且这些方法使你可以处理此类异常。

因为时，将始终 true web 应用程序中执行 SQL 命令，你必须采取预防措施来保护你的站点对 SQL 注入式攻击。 若要这样做的一种方法是使用参数化的查询来确保由网页上提交的字符串，不能解释为 SQL 命令。 在本教程中将用户输入集成到查询时，将使用参数化的查询。

### <a name="calling-a-query-that-returns-entities"></a>调用一个查询返回实体

[DbSet&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx)类提供了可用于执行该查询将返回类型的实体的方法`TEntity`。 若要查看你原理将更改中的代码`Details`方法`Department`控制器。

在*DepartmentController.cs*中`Details`方法，替换`db.Departments.FindAsync`方法调用与`db.Departments.SqlQuery`方法调用，如以下突出显示的代码中所示：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

若要验证新代码是否工作正常，请选择**部门**选项卡，然后**详细信息**部门之一。

![部门详细信息](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>调用一个查询返回其他类型的对象

前面你创建的每个注册日期显示学生的数量关于页面的学生统计信息网格。 在执行此代码*HomeController.cs*使用 LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

假设你想要编写代码，以便检索直接在 SQL 中而不使用 LINQ 此数据。 若要执行需要运行该查询将返回实体对象以外，这意味着你需要使用[Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx)方法。

在*HomeController.cs*，替换中的 LINQ 语句`About`方法与 SQL 语句，如以下突出显示的代码中所示：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

运行关于页面。 它显示它以前的相同数据。

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-an-update-query"></a>调用更新查询

假设 Contoso 大学管理员想要能够在数据库中，如更改的每个课程的信用额度数执行大容量更改。 如果该大学具有大量的课程，这会降低效率要检索其所有项作为实体并单独更改。 在本部分中，你将实施一个网页，使用户能够指定要更改用于所有课程的信用额度次数因子和则将进行的更改，通过执行 SQL`UPDATE`语句。 网页外观类似于下图：

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

在*CourseContoller.cs*，添加`UpdateCourseCredits`方法`HttpGet`和`HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

当控制器处理`HttpGet`请求，未返回任何内容中`ViewBag.RowsAffected`变量，并查看显示的空文本框和提交按钮，如在上图中所示。

当**更新**单击按钮时，`HttpPost`调用方法时，和`multiplier`已在文本框中输入的值。 然后执行的代码更新课程，并返回受影响的行数中的视图的 SQL`ViewBag.RowsAffected`变量。 当视图中，获取一个值变量，它显示的更新而不是文本框中的行数并提交按钮，如下面的插图中所示：

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

在*CourseController.cs*，右键单击其中一个`UpdateCourseCredits`方法，，然后单击**添加视图**。

![Add_View_dialog_box_for_Update_Course_Credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

在*Views\Course\UpdateCourseCredits.cshtml*，模板代码替换为以下代码：

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

运行`UpdateCourseCredits`方法通过选择**课程**选项卡上，然后添加"/ UpdateCourseCredits"到末尾的浏览器的地址栏中的 URL (例如： `http://localhost:50205/Course/UpdateCourseCredits`)。 在文本框中输入数字：

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

单击“更新” 。 你看到受影响的行数：

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

单击**回列表**若要查看的课程替换信用额度的修订号的列表。

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

有关原始的 SQL 查询的详细信息，请参阅[原始的 SQL 查询](https://msdn.microsoft.com/data/jj592907)MSDN 上。

<a id="notracking"></a>
## <a name="no-tracking-queries"></a>否跟踪查询

当数据库上下文检索表行，并创建表示它们的实体对象时，默认情况下它将跟踪的内存中的实体是否同步，与数据库中。 内存中的数据充当缓存，并更新实体时使用。 此缓存通常是不必要的 web 应用中由于上下文实例是否通常生存期较短 （新功能之一是创建和释放为每个请求） 和上下文读取再次使用该实体通常释放实体。

你可以通过使用禁用的实体对象在内存中的跟踪[AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx)方法。 在其中你可能想要执行此操作的典型方案包括：

- 查询检索此类大型卷数据的关闭跟踪可能会显著提高性能。
- 你想要将实体附加以便其进行更新，但前面为不同的用途检索相同的实体。 因为该实体已跟踪数据库上下文时，不能将附加你想要更改的实体。 处理这种情况的一种方法是使用`AsNoTracking`与前面的查询的选项。

有关示例，演示如何使用[AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx)方法，请参阅[本教程的早期版本](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md)。 本教程的此版本不修改时间标志实体上设置模型联编程序创建在编辑方法中，因此它不需要`AsNoTracking`。

<a id="sql"></a>
## <a name="examining-sql-sent-to-the-database"></a>检查 SQL 发送到数据库

有时很有用，能够以查看发送到数据库的实际 SQL 查询。 在早期的教程中，您将了解如何执行该操作在侦听器代码;现在，你将看到执行此操作而无需编写拦截器代码的一些方法。 若要试用此项，将查看一个简单查询，并再看会发生什么情况向其添加选项时，此类 eager 加载、 筛选和排序。

在*控制器/CourseController*，替换`Index`方法替换为以下代码，若要暂时停止预先加载：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

现在上设置断点`return`语句 (F9 在该行上光标)。 按 F5 在调试模式下运行该项目并选择课程索引页。 当代码达到断点时，检查`sql`变量。 你看到发送到 SQL Server 查询。 它是一个简单`Select`语句。

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

单击要查看的查询中的放大镜类**文本可视化工具**。

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

现在将向课程索引页添加下拉列表，以便用户可以筛选特定部门。 你将排序课程标题，并且你将指定的预先加载`Department`导航属性。

在*CourseController.cs*，替换`Index`方法替换为以下代码：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

在还原断点`return`语句。

该方法接收的下拉列表中的所选的值`SelectedDepartment`参数。 如果未选择任何内容，此参数将为 null。

A`SelectList`为下拉列表包含所有部门集合传递到视图。 参数传递给`SelectList`构造函数指定的值字段名称、 文本字段名称和选定的项。

有关`Get`方法`Course`存储库，该代码指定筛选表达式、 排序顺序和预先加载的`Department`导航属性。 筛选器表达式始终返回`true`如果则不选择下拉列表中 (即，`SelectedDepartment`为 null)。

在*Views\Course\Index.cshtml*，立即在打开之前`table`标记中，添加以下代码以创建下拉列表和提交按钮：

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

断点仍然组之前，运行课程索引页。 继续代码达到断点的行，第一次，以便在浏览器中显示的页面。 从下拉列表中选择一个部门，然后单击**筛选器**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

这一次部门 query for 下拉列表将第一个断点。 跳过，并查看`query`变量下次代码达到断点才能看到什么`Course`查询现在如下所示。 你将看到类似于以下内容：

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

你可以看到，现在是查询`JOIN`加载的查询`Department`数据以及`Course`数据，并包含`WHERE`子句。

删除`var sql = courses.ToString()`行。

<a id="repo"></a>

## <a name="repository-and-unit-of-work-patterns"></a>存储库和单元的工作模式

许多开发人员编写代码以作为使用实体框架代码的周围的包装器实现的存储库和单元的工作模式。 这些模式用于创建数据访问层和应用程序的业务逻辑层之间的抽象层。 实现这些模式可帮助防止你的应用程序数据存储区中的更改，而且很容易自动的单元测试驱动开发 (TDD)。 但是，编写附加代码以实现这些模式并不总是使用 EF，有几个原因的应用程序的最佳选择：

- EF 上下文类本身使应用商店数据特有的代码中的代码。
- EF 上下文类可以充当数据库的工作单位类更新你不要使用 EF。
- Entity Framework 6 中引入的功能更加轻松地实现 TDD 而无需编写存储库代码。

有关如何实现的存储库和单元的工作模式的详细信息，请参阅[本系列教程的实体框架 5 版本](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)。 有关如何在 Entity Framework 6 中实现 TDD 的信息，请参阅以下资源：

- [如何从 EF6 更轻松地通过 Mocking Dbset 时](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [使用模拟框架进行测试](https://msdn.microsoft.com/data/dn314429)
- [使用你自己的测试替身测试](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>
## <a name="proxy-classes"></a>代理类

当实体框架创建 （例如，在执行查询时） 的实体实例时，它通常创建它们作为动态生成的派生类型，它就像该实体的代理的实例。 有关示例，请参阅以下两个调试器映像。 中的第一个图像，请参阅，`student`变量是预期`Student`键入实例化实体后立即。 在第二个图中，已使用 EF 来从数据库中读取学生实体后你将看到的代理类。

![代理类之前](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![代理类之后](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

此代理类重写插入挂钩，以便在访问属性时自动执行操作的实体的某些虚拟属性。 此机制适用于的一个函数是延迟加载。

大多数情况下不需要要注意的此使用代理服务器，但有例外情况：

- 在某些情况下你可能想要阻止实体框架创建代理实例。 例如，在正在序列化实体时你通常想 POCO 类，不使用代理类。 若要避免序列化问题的一种方法是序列化而不是实体对象的数据传输对象 (Dto) 中所示[使用实体框架使用 Web API](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md)教程。 另一种方法是[禁用代理创建](https://msdn.microsoft.com/data/jj592886.aspx)。
- 当实例化实体类使用`new`运算符，你不会获得代理实例。 这意味着不遇到功能，如延迟加载和自动更改跟踪。 这通常是好了;你通常不需要延迟加载，因为你要创建新的实体不在数据库中，且通常无需更改跟踪如果您显式标记为实体`Added`。 但是，如果你需要延迟加载，并且你需要更改跟踪，你可以创建新实体实例与代理使用[创建](https://msdn.microsoft.com/library/gg679504.aspx)方法`DbSet`类。
- 你可能想要获得的代理类型的实际实体类型。 你可以使用[GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx)方法`ObjectContext`类，以获取代理类型实例的实际实体类型。

有关详细信息，请参阅[使用代理](https://msdn.microsoft.com/data/JJ592886.aspx)MSDN 上。

<a id="changedetection"></a>
## <a name="automatic-change-detection"></a>自动更改检测

实体框架通过比较的实体的当前值与原始值确定更改实体的方式 （并因此需要发送到数据库的更新）。 查询或附加该实体时，存储的原始值。 某些会导致自动更改检测的方法如下所示：

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

如果您跟踪的大量实体，并且你调用这些方法之一多次在循环中，你可能会显著的性能改进，通过暂时关闭自动更改检测使用[AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx)属性。 有关详细信息，请参阅[自动检测更改](https://msdn.microsoft.com/data/jj556205)MSDN 上。

<a id="validation"></a>
## <a name="automatic-validation"></a>自动验证

当调用`SaveChanges`方法，默认情况下，实体框架中的所有更改的实体的所有属性的数据之前验证更新的数据库。 如果你已更新的大量实体，并且你已验证数据，这一工作是不必要和无法使保存的过程所做的更改通过暂时关闭验证花费更少时间。 你可以执行，使用[ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx)属性。 有关详细信息，请参阅[验证](https://msdn.microsoft.com/data/gg193959)MSDN 上。

<a id="tools"></a>
## <a name="entity-framework-power-tools"></a>实体框架 Power 工具

[实体框架 Power 工具](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d)一个 Visual Studio 外接程序，用于创建数据模型关系图中所示这些教程。 这些工具还可以执行其他函数，如生成实体类基于现有数据库中的表，以便你可以使用代码优先使用数据库。 安装工具后，在上下文菜单中显示一些附加选项。 例如，当您右击你上下文类中的**解决方案资源管理器**，获取一个选项以生成关系图。 在你使用 Code First 时无法更改关系图中中的数据模型，但可以来回移动操作，以使其更易于理解。

![上下文菜单中的 EF](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image14.png)

![EF 关系图](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

<a id="source"></a>
## <a name="entity-framework-source-code"></a>实体框架的源代码

Entity Framework 6 的源代码位于[GitHub](https://github.com/aspnet/EntityFramework6)。 你可以报告 bug，并可以提供你自己的 EF 源代码的增强功能。

尽管源代码处于打开状态，实体框架完全支持的 Microsoft 产品。 Microsoft 实体框架团队将控制哪些接受的贡献保持和测试所有代码更改，以确保每个版本的质量。

<a id="summary"></a>
## <a name="summary"></a>摘要

这将完成这一系列的 ASP.NET MVC 应用程序中使用实体框架的教程。 有关如何使用实体框架处理数据的详细信息，请参阅[EF MSDN 上的文档页](https://msdn.microsoft.com/data/ee712907)和[ASP.NET 数据访问的推荐资源](../../../../whitepapers/aspnet-data-access-content-map.md)。

有关如何部署 web 应用程序，你已生成后的详细信息，请参阅[ASP.NET Web 部署的推荐资源](../../../../whitepapers/aspnet-web-deployment-content-map.md)MSDN 库中。

有关与相关的 MVC，例如身份验证和授权，其他主题信息请参阅[ASP.NET MVC 的推荐资源](../recommended-resources-for-mvc.md)。

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>确认

- Tom Dykstra 编写本教程的原始版本，共同撰写了 EF 5 更新，和编写 EF 6 更新。 Tom 是上的 Microsoft Web 平台和工具内容团队的高级编程编写器。
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) 未的大部分更新本教程适用于 EF 5 和 MVC 4 工作，共同创作 EF 6 更新。 Rick 是 Microsoft 将重点放在 Azure 和 MVC 的高级编程编写器。
- [Rowan Miller](http://www.romiller.com)和实体框架团队其他成员的辅助与代码评审，并帮助调试借助迁移所引起时我们已为 EF 5 和 EF 6 更新本教程的许多问题。

<a id="vb"></a>
## <a name="vb"></a>VB

当的 EF 4.1 中最初产生了本教程时，我们将提供已完成的下载项目的 C# 和 VB 的版本。 由于时间限制和其他优先级我们不完成该操作对于此版本。 如果您生成使用这些教程为 VB 项目，并且会愿意与他人共享的请让我们知道。

<a id="errors"></a>
## <a name="common-errors-and-solutions-or-workarounds-for-them"></a>常见错误，以及解决方案或解决办法它们

### <a name="cannot-createshadow-copy"></a>无法创建/卷影副本

错误消息：

> 无法创建/卷影副本&lt;filename&gt;该文件已存在。


解决方案

等待几秒钟，并刷新页面。

### <a name="update-database-not-recognized"></a>更新数据库无法识别

错误消息 (从`Update-Database`PMC 命令):

> 术语更新数据库未被识别为 cmdlet、 函数、 脚本文件或可操作程序的名称。 请检查拼写的名称，或如果包括路径，请验证路径正确，然后重试。


解决方案

退出 Visual Studio。 重新打开项目，然后重试。

### <a name="validation-failed"></a>验证失败

错误消息 (从`Update-Database`PMC 命令):

> 一个或多个实体的验证失败。 请参阅 EntityValidationErrors 属性的更多详细信息。


解决方案

此问题的一个原因是验证错误时`Seed`方法运行。 请参阅[进行种子设定和调试 Entity Framework (EF) 数据库](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)调试的提示和技巧`Seed`方法。

### <a name="http-50019-error"></a>HTTP 500.19 错误

错误消息：

> HTTP 错误 500.19-内部服务器错误  
> 无法访问所请求的页面，因为该页的相关的配置数据无效。


解决方案

可能发生此错误的一种方法是从拥有该解决方案，每个使用相同的端口号的多个副本。 通常可以通过退出的 Visual Studio 中，所有实例，然后重新启动您正在处理的项目来解决此问题。 如果这不起作用，请尝试更改端口号。 右键单击项目文件，然后单击属性。 选择**Web**选项卡上，然后将更改中的端口号**项目 Url**文本框。

### <a name="error-locating-sql-server-instance"></a>错误找到 SQL Server 实例

错误消息：

> 建立与 SQL Server 的连接时发生与网络相关或特定于实例的错误。 未找到或无法访问服务器。 请验证实例名称是否正确，SQL Server 是否已配置为允许远程连接。 (提供程序： SQL 网络接口，错误： 26-错误查找服务器/实例指定)


解决方案

请检查连接字符串。 如果你已手动删除数据库，更改构造字符串中数据库的名称。

>[!div class="step-by-step"]
[上一篇](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
