---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: "MVC Web 应用程序 (10/10) 的高级实体框架方案 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 148a1aa33f6f713ae471ab14c7180f6c08a8679a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>MVC Web 应用程序 (10/10) 的高级的实体框架方案
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 你可以从头开始教程系列或[下载这一章的初学者项目](building-the-ef5-mvc4-chapter-downloads.md)和从这里开始。
> 
> > [!NOTE] 
> > 
> > 如果你遇到无法解决的问题[下载已完成的章](building-the-ef5-mvc4-chapter-downloads.md)并尝试重现你的问题。 你通常可以通过比较你的代码已完成的代码会发现问题的解决方案。 一些常见的错误和如何解决它们，请参阅[错误和解决方法。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


在以前的教程，你可以实现的存储库和单元的工作模式。 本教程涵盖以下主题：

- 执行原始的 SQL 查询。
- 执行无跟踪查询。
- 检查查询发送到数据库。
- 使用代理类。
- 禁用自动检测的更改。
- 禁用验证，在保存时更改。
- [错误和解决方法](#errors)

对于大多数这些主题，你将使用已创建的页。 若要使用原始 SQL 执行批量更新将创建一个新页更新的数据库中的所有课程的信用额度数：

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

并使用否跟踪查询来查询你将新的验证逻辑添加到部门编辑页：

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>执行原始 SQL 查询

实体框架代码的第一个 API 包括使您能够 SQL 命令将直接传递到数据库的方法。 有下列选项：

- 使用`DbSet.SqlQuery`返回实体类型的查询的方法。 返回的对象必须是期望的类型的`DbSet`对象，并且它们是否自动跟踪数据库上下文中的除非关闭跟踪。 (参见下一节`AsNoTracking`方法。)
- 使用`Database.SqlQuery`方法用于返回不是实体类型的查询。 返回的数据不跟踪数据库上下文中，即使你使用此方法来检索实体类型。
- 使用[Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx)非查询命令。

使用实体框架的优点之一是它可避免将你过于仔细存储数据的特定方法的代码。 此，它会生成 SQL 查询和命令，其中还使你无需自行编写。 但没有特殊情况下当你需要运行特定的 SQL 查询，你已手动创建的并且这些方法使你可以处理此类异常。

因为时，将始终 true web 应用程序中执行 SQL 命令，你必须采取预防措施来保护你的站点对 SQL 注入式攻击。 若要这样做的一种方法是使用参数化的查询来确保由网页上提交的字符串，不能解释为 SQL 命令。 在本教程中将用户输入集成到查询时，将使用参数化的查询。

### <a name="calling-a-query-that-returns-entities"></a>调用一个查询返回实体

假设你想`GenericRepository`类以提供附加的筛选和排序灵活性而无需使用其他方法创建一个派生的类。 实现此目的的一种方法将添加接受 SQL 查询的方法。 你可以指定任何类型的筛选或排序你想在控制器中，如`Where`取决于联接或子查询的子句。 在本部分中，你将看到如何实现此类方法。

创建`GetWithRawSql`通过添加以下代码的方法*GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

在*CourseController.cs*，调用中的新方法`Details`方法，如下面的示例中所示：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

你无法在此情况下使用`GetByID`方法，但你正在使用`GetWithRawSql`方法来验证是否`GetWithRawSQL`方法的用法。

运行要验证的选择查询工作原理的详细信息页 (选择**课程**选项卡，然后**详细信息**一个课程)。

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>调用一个查询返回其他类型的对象

前面你创建的每个注册日期显示学生的数量关于页面的学生统计信息网格。 在执行此代码*HomeController.cs*使用 LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

假设你想要编写代码，以便检索直接在 SQL 中而不使用 LINQ 此数据。 若要执行需要运行该查询将返回实体对象以外，这意味着你需要使用`Database.SqlQuery`方法。

在*HomeController.cs*，替换中的 LINQ 语句`About`方法替换为以下代码：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

运行关于页面。 它显示它以前的相同数据。

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>调用更新查询

假设 Contoso 大学管理员想要能够在数据库中，如更改的每个课程的信用额度数执行大容量更改。 如果该大学具有大量的课程，这会降低效率要检索其所有项作为实体并单独更改。 在本部分中，你将实施一个网页，允许用户指定用来更改用于所有课程的信用额度数因子和则将进行的更改，通过执行 SQL`UPDATE`语句。 网页外观类似于下图：

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

在以前的教程，你用于泛型存储库读取和更新`Course`中的实体`Course`控制器。 对于此大容量更新操作，你需要创建一个新的存储库方法不属于泛型存储库。 若要做到这一点，你将创建专用`CourseRepository`派生自的类`GenericRepository`类。

在*DAL*文件夹中，创建*CourseRepository.cs*和替换为以下代码替换现有代码：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

在*UnitOfWork.cs*，更改`Course`从存储库类型`GenericRepository<Course>`到`CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

在*CourseContoller.cs*，添加`UpdateCourseCredits`方法：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

此方法将为两者使用`HttpGet`和`HttpPost`。 当`HttpGet``UpdateCourseCredits`方法运行`multiplier`变量将为 null，并且视图将显示的空文本框和提交按钮，如在上图中所示。

当**更新**单击按钮和`HttpPost`方法运行`multiplier`将具有在文本框中输入的值。 然后，代码调用的存储库`UpdateCourseCredits`方法，返回的数目影响的行，而该值将存储在`ViewBag`对象。 当视图收到受影响行数`ViewBag`对象，它显示该数字，而非文本框中，将提交按钮下, 图中所示：

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

中创建视图*Views\Course*更新课程信用额度页的文件夹：

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

在*Views\Course\UpdateCourseCredits.cshtml*，模板代码替换为以下代码：

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

运行`UpdateCourseCredits`方法通过选择**课程**选项卡上，然后添加"/ UpdateCourseCredits"到末尾的浏览器的地址栏中的 URL (例如： `http://localhost:50205/Course/UpdateCourseCredits`)。 在文本框中输入数字：

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

单击“更新” 。 你看到受影响的行数：

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

单击**回列表**若要查看的课程替换信用额度的修订号的列表。

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

有关原始的 SQL 查询的详细信息，请参阅[原始的 SQL 查询](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx)实体框架团队博客上。

## <a name="no-tracking-queries"></a>否跟踪查询

当数据库上下文检索数据库行，并创建表示它们的实体对象时，默认情况下它将跟踪的内存中的实体是否同步，与数据库中。 内存中的数据充当缓存，并更新实体时使用。 此缓存通常是不必要的 web 应用中由于上下文实例是否通常生存期较短 （新功能之一是创建和释放为每个请求） 和上下文读取再次使用该实体通常释放实体。

你可以指定上下文是否通过使用跟踪查询的实体对象`AsNoTracking`方法。 在其中你可能想要执行此操作的典型方案包括：

- 该查询将检索此类大型卷数据的关闭跟踪可能会显著提高性能。
- 你想要将实体附加以便其进行更新，但前面为不同的用途检索相同的实体。 因为该实体已跟踪数据库上下文时，不能将附加你想要更改的实体。 要防止此类情况发生的一种方法是使用`AsNoTracking`与前面的查询的选项。

在本部分中，你将实施阐释了这些方案中的第二个的业务逻辑。 具体而言，你将强制实施业务规则指出一个教师不能为多个部门的管理员。

在*DepartmentController.cs*，添加新方法，可以从调用`Edit`和`Create`方法以确保没有两个部门具有相同的管理员：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

中添加代码`try`块`HttpPost``Edit`方法调用此新方法，如果没有任何验证错误。 `try`块现在看起来像下面的示例：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

运行部门编辑页面，尝试更改到已经是另一个部门的管理员的教师部门的管理员。 您收到预期的错误消息：

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

现在运行部门编辑页再次和此时间更改**预算**量。 当你单击**保存**，你看到一个错误页面：

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

异常错误消息为"`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`"这是由于以下事件序列：

- `Edit`方法调用`ValidateOneAdministratorAssignmentPerInstructor`方法，检索所有具有 Kim Abercrombie 以其管理员身份的部门。 从而导致要读取的英语部门。 因为这是正在编辑的部门，不会报告错误。 由于此读取操作，但是，已从数据库读取的英语的部门实体现在正在跟踪的数据库上下文。
- `Edit`方法尝试设置`Modified`能在英语版的标志部门实体创建的 MVC 模型联编程序，但该失败，因为上下文已跟踪英语的部门实体。

此问题的一种解决方案是防止上下文跟踪验证查询检索到的内存中部门实体。 这样做，因为你不会更新此实体或从其缓存在内存中受益的方式再次读取它没有缺点。

在*DepartmentController.cs*中`ValidateOneAdministratorAssignmentPerInstructor`方法，不指定任何跟踪，如在下面的示例所示：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

重复你尝试编辑**预算**某个部门的量。 这一次操作成功，并预计将于部门索引页上，显示修订的预算值返回站点。

## <a name="examining-queries-sent-to-the-database"></a>检查查询发送到数据库

有时很有用，能够以查看发送到数据库的实际 SQL 查询。 若要执行此操作，你可以检查在调试器中的查询变量，或调用查询的`ToString`方法。 若要试用此项，将查看一个简单查询，并再看会发生什么情况向其添加选项时，此类 eager 加载、 筛选和排序。

在*控制器/CourseController*，替换`Index`方法替换为以下代码：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

现在中设置断点*GenericRepository.cs*上`return query.ToList();`和`return orderBy(query).ToList();`的语句`Get`方法。 在调试模式下运行该项目并选择课程索引页。 当代码达到断点时，检查`query`变量。 你看到发送到 SQL Server 查询。 它是一个简单`Select`语句：

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

查询会很长时间才能在 Visual Studio 中的调试窗口中显示。 若要查看整个查询，可以复制变量的值，并将其粘贴到文本编辑器：

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

现在将向课程索引页添加下拉列表，以便用户可以筛选特定部门。 你将排序课程标题，并且你将指定的预先加载`Department`导航属性。 在*CourseController.cs*，替换`Index`方法替换为以下代码：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

该方法接收的下拉列表中的所选的值`SelectedDepartment`参数。 如果未选择任何内容，此参数将为 null。

A`SelectList`为下拉列表包含所有部门集合传递到视图。 参数传递给`SelectList`构造函数指定的值字段名称、 文本字段名称和选定的项。

有关`Get`方法`Course`存储库，该代码指定筛选表达式、 排序顺序和预先加载的`Department`导航属性。 筛选器表达式始终返回`true`如果则不选择下拉列表中 (即，`SelectedDepartment`为 null)。

在*Views\Course\Index.cshtml*，立即在打开之前`table`标记中，添加以下代码以创建下拉列表和提交按钮：

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

仍中设置的断点与`GenericRepository`类，运行课程索引页。 继续代码达到断点、 的前两个时间，以便在浏览器中显示的页面。 从下拉列表中选择一个部门，然后单击**筛选器**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

这一次部门 query for 下拉列表将第一个断点。 跳过，并查看`query`变量下次代码达到断点才能看到什么`Course`查询现在如下所示。 你将看到类似于以下内容：

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

你可以看到，现在是查询`JOIN`加载的查询`Department`数据以及`Course`数据，并包含`WHERE`子句。

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>使用代理类

当实体框架创建 （例如，在执行查询时） 的实体实例时，它通常创建它们作为动态生成的派生类型，它就像该实体的代理的实例。 此代理将重写插入挂钩，以便在访问属性时自动执行操作的实体的某些虚拟属性。 例如，此机制用于支持延迟加载的关系。

大多数情况下不需要要注意的此使用代理服务器，但有例外情况：

- 在某些情况下你可能想要阻止实体框架创建代理实例。 例如，序列化非代理实例可能比序列化代理实例更高效。
- 当实例化实体类使用`new`运算符，你不会获得代理实例。 这意味着不遇到功能，如延迟加载和自动更改跟踪。 这通常是好了;你通常不需要延迟加载，因为你要创建新的实体不在数据库中，且通常无需更改跟踪如果您显式标记为实体`Added`。 但是，如果你需要延迟加载，并且你需要更改跟踪，你可以创建新实体实例与代理使用`Create`方法`DbSet`类。
- 你可能想要获得的代理类型的实际实体类型。 你可以使用`GetObjectType`方法`ObjectContext`类，以获取代理类型实例的实际实体类型。

有关详细信息，请参阅[使用代理](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx)实体框架团队博客上。

## <a name="disabling-automatic-detection-of-changes"></a>禁用自动检测更改

实体框架通过比较的实体的当前值与原始值确定更改实体的方式 （并因此需要发送到数据库的更新）。 如果该实体已查询或附加，存储的原始值。 某些会导致自动更改检测的方法如下所示：

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

如果您跟踪的大量实体，并且你调用这些方法之一多次在循环中，你可能会显著的性能改进，通过暂时关闭自动更改检测使用[AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx)属性。 有关详细信息，请参阅[自动检测更改](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx)。

## <a name="disabling-validation-when-saving-changes"></a>禁用验证，在保存时更改

当调用`SaveChanges`方法，默认情况下，实体框架中的所有更改的实体的所有属性的数据之前验证更新的数据库。 如果你已更新的大量实体，并且你已验证数据，这一工作是不必要和无法使保存的过程所做的更改通过暂时关闭验证花费更少时间。 你可以执行，使用[ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx)属性。 有关详细信息，请参阅[验证](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx)。

## <a name="summary"></a>摘要

这将完成这一系列的 ASP.NET MVC 应用程序中使用实体框架的教程。 在找不到其他实体框架资源的链接[ASP.NET 数据访问内容映射](../../../../whitepapers/aspnet-data-access-content-map.md)。

有关如何部署 web 应用程序，你已生成后的详细信息，请参阅[ASP.NET 部署内容映射](https://msdn.microsoft.com/library/bb386521.aspx)MSDN 库中。

有关与相关的 MVC，例如身份验证和授权，其他主题信息请参阅[MVC 的推荐资源](../../getting-started/recommended-resources-for-mvc.md)。

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>确认

- Tom Dykstra 编写本教程的原始版本和是上的 Microsoft Web 平台和工具内容团队高级编程的编写器。
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) 共同撰写本教程并确实执行操作的大部分工作的 EF 5 和 MVC 4 对它进行更新。 Rick 是 Microsoft 将重点放在 Azure 和 MVC 的高级编程编写器。
- [Rowan Miller](http://www.romiller.com)和实体框架团队其他成员的辅助与代码评审，并帮助调试借助迁移所引起时我们已 EF 5 更新本教程的许多问题。

## <a name="vb"></a>VB

当最初生成本教程时，我们将提供已完成的下载项目的 C# 和 VB 的版本。 我们利用此更新提供每个章节，以使其更轻松地开始任意位置在系列中，但由于时间限制和我们未不执行该操作 VB.其他优先级的 C# 可下载项目 如果您生成使用这些教程为 VB 项目，并且会愿意与他人共享的请让我们知道。

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>错误和解决方法

### <a name="cannot-createshadow-copy"></a>无法创建/卷影副本

错误消息：

*无法创建/卷影副本 DotNetOpenAuth.OpenId 该文件已存在时。*

解决方案：

等待几秒钟，并刷新页面。

### <a name="update-database-not-recognized"></a>更新数据库无法识别

错误消息：

*术语更新数据库未被识别为 cmdlet、 函数、 脚本文件或可操作程序的名称。请检查拼写的名称，或如果包括路径，请验证路径正确，然后重试。*(从 *`Update-Database`*  PMC 命令。)

解决方案：

退出 Visual Studio。 重新打开项目，然后重试。

### <a name="validation-failed"></a>验证失败

错误消息：

*一个或多个实体的验证失败。请参阅 EntityValidationErrors 属性的更多详细信息。* (从 *`Update-Database`*  PMC 命令。)

解决方案：

此问题的一个原因是验证错误时`Seed`方法运行。 请参阅[进行种子设定和调试 Entity Framework (EF) 数据库](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)调试的提示和技巧`Seed`方法。

### <a name="http-50019-error"></a>HTTP 500.19 错误

错误消息：

*无法访问 HTTP 错误 500.19-内部服务器错误的请求页面，因为该页的相关的配置数据无效。*

解决方案：

可能发生此错误的一种方法是从拥有该解决方案，每个使用相同的端口号的多个副本。 通常可以通过退出的 Visual Studio 中，所有实例，然后重新项目启动你正在致力于解决此问题。 如果这不起作用，请尝试更改端口号。 右键单击项目文件，然后单击属性。 选择**Web**选项卡上，然后将更改中的端口号**项目 Url**文本框。

### <a name="error-locating-sql-server-instance"></a>错误找到 SQL Server 实例

错误消息：

*建立与 SQL Server 的连接时发生与网络相关或特定于实例的错误。未找到或无法访问服务器。请验证实例名称正确以及 SQL Server 配置为允许远程连接。(提供程序： SQL 网络接口，错误： 26-错误查找服务器/实例指定)*

解决方案：

请检查连接字符串。 如果你已手动删除数据库，更改构造字符串中数据库的名称。

>[!div class="step-by-step"]
[上一页](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
[下一页](building-the-ef5-mvc4-chapter-downloads.md)
