---
title: "ASP.NET Core MVC 和 EF Core 教程 - 创建、读取、更新和删除 (2/10)"
author: tdykstra
description: 
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/crud
ms.openlocfilehash: a7e0d4ff3d57e42dd7e33ffb5f26f2143520be87
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2018
---
# <a name="create-read-update-and-delete---ef-core-with-aspnet-core-mvc-tutorial-2-of-10"></a>创建、读取、更新和删除 - EF Core 和 ASP.NET Core MVC 教程（第 2 个，共 10 个）

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University 示例 Web 应用程序演示如何使用 Entity Framework Core 和 Visual Studio 创建 ASP.NET Core MVC Web 应用程序。 若要了解教程系列，请参阅[本系列中的第一个教程](intro.md)。

在上一个教程中，创建了一个使用 Entity Framework 和 SQL Server LocalDB 来存储和显示数据的 MVC 应用程序。 在本教程中，将评审和自定义 MVC 基架在控制器和视图中自动创建的 CRUD （创建、读取、更新、删除）代码。

> [!NOTE] 
> 为了在控制器和数据访问层之间创建一个抽象层，常见的做法是实现存储库模式。 为了保持这些教程内容简单并重点介绍如何使用 Entity Framework 本身，它们不使用存储库。 有关存储库和 EF 的信息，请参阅[本系列中的最后一个教程](advanced.md)。

在本教程中，将使用以下网页：

![学生详细信息页](crud/_static/student-details.png)

![学生创建页](crud/_static/student-create.png)

![学生编辑页](crud/_static/student-edit.png)

![学生删除页](crud/_static/student-delete.png)

## <a name="customize-the-details-page"></a>自定义“详细信息”页

学生索引页的基架代码省略了 `Enrollments` 属性，因为该属性包含一个集合。 在“详细信息”页上，将以 HTML 表形式显示集合的内容。

在 Controllers/StudentsController.cs 中，“详细信息”视图的操作方法使用 `SingleOrDefaultAsync` 方法检索单个 `Student` 实体。 添加调用 `Include` 的代码。 `ThenInclude` 和 `AsNoTracking` 方法，如以下突出显示的代码所示。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]

`Include` 和 `ThenInclude` 方法使上下文加载 `Student.Enrollments` 导航属性，并在每个注册中加载 `Enrollment.Course` 导航属性。  有关这些方法的详细信息，请参阅[读取相关数据](read-related-data.md)教程。

对于返回的实体未在当前上下文生存期中更新的情况，`AsNoTracking` 方法将会提升性能。 本教程末尾将介绍有关 `AsNoTracking` 的详细信息。

### <a name="route-data"></a>路由数据

传递到 `Details` 方法的键值来自路由数据。 路由数据是模型绑定器在 URL 的段中找到的数据。 例如，默认路由指定控制器、操作和 ID 段：

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]

在下面的 URL 中，默认路由将 Instructor 映射作为控制器、Index 作为操作，1 作为 ID；这些都是路由数据值。

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

URL 的最后部分 ("?courseID=2021") 是一个查询字符串。 如果将 `id` 作为查询字符串值传递，模型绑定器也会将 ID 值作为参数传递给 `Details` 方法：

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

在索引页中，超链接 URL 由 Razor 视图中的标记帮助器语句创建。 在以下 Razor 代码中，`id` 参数与默认路由相匹配，因此 `id` 将添加到路由数据。

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

`item.ID` 为 6 时，会生成以下 HTML：

```html
<a href="/Students/Edit/6">Edit</a>
```

在以下 Razor 代码中，`studentID` 与默认路由中的参数不匹配，所以将它作为查询字符串添加。

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

`item.ID` 为 6 时，会生成以下 HTML：

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

有关标记帮助器的详细信息，请参阅 [ASP.NET Core 中的标记帮助器](xref:mvc/views/tag-helpers/intro)。

### <a name="add-enrollments-to-the-details-view"></a>将注册添加到“详细信息”视图

打开 *Views/Students/Details.cshtml*。 每个字段都使用 `DisplayNameFor` 和 `DisplayFor` 帮助器来显示，如下面的示例中所示：

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

在最后一个字段之后和 `</dl>` 闭合标记之前，添加以下代码以显示注册列表：

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

如果代码缩进在粘贴代码后出现错误，请按 CTRL+K+D 进行更正。

此代码循环通过 `Enrollments` 导航属性中的实体。 它将针对每个注册显示课程标题和成绩。 课程标题从 Course 实体中检索，该实体存储在 Enrollments 实体的 `Course` 导航属性中。

运行应用，选择“学生”选项卡，然后单击学生的“详细信息”链接。 将看到所选学生的课程和年级列表：

![学生详细信息页](crud/_static/student-details.png)

## <a name="update-the-create-page"></a>更新“创建”页

在 *StudentsController.cs* 中修改 HttpPost `Create` 方法，在 `Bind` 特性中添加 try catch 块并删除 ID 值。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]

此代码将 ASP.NET MVC 模型绑定器创建的 Student 实体添加到 Students 实体集，然后将更改保存到数据库。 （模型绑定器指的是 ASP.NET MVC 功能，用户可利用它来轻松处理使用表单提交的数据；模型绑定器将已发布的表单值转换为 CLR 类型，并将其传递给操作方法的参数。 在本例中，模型绑定器将使用 Form 集合的属性值实例化 Student 实体。）

已从 `Bind` 特性删除 `ID`，因为 ID 是插入行时 SQL Server 将自动设置的主键值。 来自用户的输入不会设置 ID 值。

除了 `Bind` 特性，try-catch 块是对基架代码所做的唯一更改。 如果保存更改时捕获到来自 `DbUpdateException` 的异常，则会显示一般错误消息。 有时 `DbUpdateException` 异常是由应用程序外部的某些内容而非编程错误引起的，因此建议用户再次尝试。 尽管在本示例中未实现，但生产质量应用程序会记录异常。 有关详细信息，请参阅[监视和遥测（使用 Azure 构建真实世界云应用）](https://docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry)中的“见解记录”部分。

`ValidateAntiForgeryToken` 特性帮助抵御跨网站请求伪造 (CSRF) 攻击。 令牌通过 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) 自动注入到视图中，并在用户提交表单时包含该令牌。 令牌由 `ValidateAntiForgeryToken` 特性验证。 有关 CSRF 的详细信息，请参阅[反请求伪造](../../security/anti-request-forgery.md)。

<a id="overpost"></a>
### <a name="security-note-about-overposting"></a>有关过多发布的安全说明

基架代码包含在 `Create` 方法中的 `Bind` 特性是防止在创建方案中过多发布的一种方法。 例如，假设 Student 实体包含不希望此网页设置的 `Secret` 属性。

```csharp
public class Student
{
    public int ID { get; set; }
    public string LastName { get; set; }
    public string FirstMidName { get; set; }
    public DateTime EnrollmentDate { get; set; }
    public string Secret { get; set; }
}
```

即使网页上没有 `Secret` 字段，黑客也可以使用 Fiddler 之类的工具，或者编写一些 JavaScript 来发布 `Secret` 表单值。 创建 Student 实例时，如果不利用 `Bind` 特性来限制模型绑定器使用的字段，模型绑定器会选取该 `Secret` 表单值并使用它来创建 Student 实体实例。 然后将在数据库中更新黑客为 `Secret` 表单字段指定的任意值。 下图显示 Fiddler 工具正在将 `Secret` 字段（值为“OverPost”）添加到已发布的表单值。

![Fiddler 添加 Secret 字段](crud/_static/fiddler.png)

然后值“OverPost”将成功添加到插入行的 `Secret` 属性，尽管你从未打算网页可设置该属性。

可以防止在编辑方案中过多发布，方法是首先从数据库读取实体，然后调用 `TryUpdateModel` 并在显式允许的属性列表中传递。 这些教程中使用的也是这种方法。

许多开发者首选的防止过多发布的另一种方法是使用视图模型，而不是包含模型绑定的实体类。 仅包含想要在视图模型中更新的属性。 完成 MVC 模型绑定器后，根据需要使用 AutoMapper 之类的工具将视图模型属性复制到实体实例。 使用实体实例上的 `_context.Entry` 将其状态设置为 `Unchanged`，然后在视图模型中包含的每个实体属性上将 `Property("PropertyName").IsModified` 设置为 true。 此方法同时适用于编辑和创建方案。

### <a name="test-the-create-page"></a>测试创建页

Views/Students/Create.cshtml 中的代码对每个字段使用 `label`、`input` 和 `span`（适用于验证消息）标记帮助器。

运行应用，选择“学生”选项卡，并单击“新建”。

输入姓名和日期。 如果浏览器允许输入无效日期，请尝试输入。 （某些浏览器强制要求使用日期选取器。）然后单击“创建”，查看错误消息。

![日期验证错误](crud/_static/date-error.png)

这是默认获取的服务器端验证；在下一个教程中，还将介绍如何添加生成客户端验证代码的特性。 以下突出显示的代码显示 `Create` 方法中的模型验证检查。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]

将日期更改为有效值，并单击“创建”，查看“索引”页中显示的新学生。

## <a name="update-the-edit-page"></a>更新“编辑”页

在 StudentController.cs 中，HttpGet `Edit` 方法（不具有 `HttpPost` 特性）使用 `SingleOrDefaultAsync` 方法检索所选的 Student 实体，如 `Details` 方法中所示。 不需要更改此方法。

### <a name="recommended-httppost-edit-code-read-and-update"></a>建议的 HttpPost 编辑代码：读取和更新

使用以下代码替换 HttpPost Edit 操作方法。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]

这些更改实现安全最佳做法，防止过多发布。 基架生成了 `Bind` 特性，并将模型绑定器创建的实体添加到具有 `Modified` 标记的实体集。 不建议将该代码用于多个方案，因为 `Bind` 特性将清除未在 `Include` 参数中列出的字段中的任何以前存在的数据。

新代码读取现有实体并调用 `TryUpdateModel`，以[基于已发布表单数据中的用户输入](xref:mvc/models/model-binding#how-model-binding-works)更新已检索实体中的字段。 Entity Framework 的自动更改跟踪在由表单输入更改的字段上设置 `Modified` 标记。 调用 `SaveChanges` 方法时，Entity Framework 会创建 SQL 语句，以更新数据库行。 忽略并发冲突，并且仅在数据库中更新由用户更新的表列。 （下一个教程将介绍如何处理并发冲突。）

作为防止过多发布的最佳做法，请将希望通过“编辑”页更新的字段列入 `TryUpdateModel` 参数。 （参数列表中字段列表之前的空字符串用于与表单字段名称一起使用的前缀。）目前没有要保护的额外字段，但是列出希望模型绑定器绑定的字段可确保以后将字段添加到数据模型时，它们将自动受到保护，直到明确将其添加到此处为止。

这些更改会导致 HttpPost `Edit` 方法与 HttpGet `Edit` 方法的方法签名相同，因此已重命名 `EditPost` 方法。

### <a name="alternative-httppost-edit-code-create-and-attach"></a>可选 HttpPost 编辑代码：创建和附加

建议的 HttpPost 编辑代码确保只更新已更改的列，并保留不希望包含在模型绑定内的属性中的数据。 但是，读取优先的方法需要额外的数据库读取，并可能产生处理并发冲突的更复杂代码。 另一种方法是将模型绑定器创建的实体附加到 EF 上下文，并将其标记为已修改。 （请勿使用此代码更新项目，它只是显示一种可选的方法。） 

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]

网页 UI 包含实体中的所有字段并能更新其中任意字段时，可以使用此方法。

基架代码使用创建和附加方法，但仅捕获 `DbUpdateConcurrencyException` 异常并返回 404 错误代码。  显示的示例捕获任意数据库更新异常并显示错误消息。

### <a name="entity-states"></a>实体状态

数据库上下文跟踪内存中的实体是否与数据库中相应的行同步，并且此信息确定调用 `SaveChanges` 方法时会发生的情况。 例如，将新实体传递到 `Add` 方法时，该实体的状态会设置为 `Added`。 然后调用 `SaveChanges` 方法时，数据库上下文发出 SQL INSERT 命令。

实体可能处于以下状态之一：

* `Added`。 数据库中尚不存在实体。 `SaveChanges` 方法发出 INSERT 语句。

* `Unchanged`。 不需要通过 `SaveChanges` 方法对此实体执行操作。 从数据库读取实体时，实体将从此状态开始。

* `Modified`。 已修改实体的部分或全部属性值。 `SaveChanges` 方法发出 UPDATE 语句。

* `Deleted`。 已标记该实体进行删除。 `SaveChanges` 方法发出 DELETE 语句。

* `Detached`。 数据库上下文未跟踪该实体。

在桌面应用程序中，通常会自动设置状态更改。 读取一个实体并对其某些属性值做出更改。 这将使其实体状态自动更改为 `Modified`。 然后调用 `SaveChanges` 时，Entity Framework 生成 SQL UPDATE 语句，该语句仅更新已更改的实际属性。

在 Web 应用中，最初读取实体并显示其要编辑的数据的 `DbContext` 将在页面呈现后进行处理。 调用 HttpPost `Edit` 操作方法时，将发出新的 Web 请求并且具有 `DbContext` 的新实例。 如果在新的上下文中重新读取实体，则将模拟桌面处理。

但如果不希望进行额外的读取操作，则必须使用模型绑定器创建的实体对象。  执行此操作最简单的方法是将实体状态设置为“已修改”，就像在之前所示的替代 HttpPost 编辑代码中完成的一样。 然后调用 `SaveChanges` 时，Entity Framework 会更新数据库行的所有列，因为上下文无法知道已更改的属性。

如果想避免读取优先的方法，但还希望 SQL UPDATE 语句只更新用户实际更改的字段，则代码将更复杂。 调用 HttpPost `Edit` 方法时，必须以某种方式保存初始值（如使用隐藏字段），以便初始值可用。 然后可以使用初始值创建 Student 实体、调用具有初始实体版本的 `Attach` 方法、将实体的值更新为新值，再调用 `SaveChanges`。

### <a name="test-the-edit-page"></a>测试编辑页

运行应用，选择“学生”选项卡，然后单击“编辑”超链接。

![学生编辑页](crud/_static/student-edit.png)

更改某些数据并单击“保存”。 将打开“索引”页，将看到已更改的数据。

## <a name="update-the-delete-page"></a>更新“删除”页

在 StudentController.cs 中，HttpGet `Delete` 方法的模板代码使用 `SingleOrDefaultAsync` 方法来检索所选的 Student 实体，如 Details 和 Edit 方法中所示。 但是，若要在调用 `SaveChanges` 失败时实现自定义错误消息，请将部分功能添加到此方法及其相应的视图中。

正如所看到的更新和创建操作，删除操作需要两个操作方法。 为响应 GET 请求而调用的方法将显示一个视图，使用户有机会批准或取消操作。 如果用户批准，则创建 POST 请求。 发生此情况时，将调用 HttpPost `Delete` 方法，然后该方法实际执行删除操作。

将 try-catch 块添加到 HttpPost `Delete` 方法，以处理更新数据库时可能出现的任何错误。 如果发生错误，HttpPost Delete 方法会调用 HttpGet Delete 方法，并向其传递一个指示发生错误的参数。 然后 HttpGet Delete 方法重新显示确认页以及错误消息，向用户提供取消或重试的机会。

使用以下管理错误报告的代码替换 HttpGet `Delete` 操作方法。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]

此代码接受可选参数，指示保存更改失败后是否调用此方法。 没有失败的情况下调用 HttpGet `Delete` 方法时，此参数为 false。 由 HttpPost `Delete` 方法调用以响应数据库更新错误时，此参数为 true，并且将错误消息传递到视图。

### <a name="the-read-first-approach-to-httppost-delete"></a>HttpPost Delete 的读取优先方法

使用以下执行实际删除操作并捕获任何数据库更新错误的代码替换 HttpPost `Delete` 操作方法（名为 `DeleteConfirmed`）。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6,8-11,13-14,18-23)]

此代码检索所选的实体，然后调用 `Remove` 方法以将实体的状态设置为 `Deleted`。 调用 `SaveChanges` 时生成 SQL DELETE 命令。

### <a name="the-create-and-attach-approach-to-httppost-delete"></a>HttpPost Delete 的创建和附加方法

如果在大容量应用程序中提高性能是优先事项，则可以通过只使用主键值实例化 Student 实体，然后将实体状态设置为 `Deleted` 来避免不必要的 SQL 查询。 这是 Entity Framework 删除实体需要执行的所有操作。 （请勿将此代码放在项目中；这里只是为了说明替代方法。）

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]

如果实体包含还应删除的相关数据，请确保在数据库中配置了级联删除。 若使用此方法删除实体，EF 可能不知道有需要删除的相关实体。

### <a name="update-the-delete-view"></a>更新“删除”视图

在 Views/Student/Delete.cshtml 中，在 H2 标题和 H3 标题之间添加错误消息，如以下示例所示：

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

运行应用，选择“学生”选项卡，并单击“删除”超链接：

![删除确认页](crud/_static/student-delete.png)

单击“删除”。 将显示不含已删除学生的索引页。 （将看到并发教程中错误处理代码的效果示例。）

## <a name="closing-database-connections"></a>关闭数据库连接

若要释放数据库连接包含的资源，完成此操作时必须尽快处理上下文实例。 ASP.NET Core 内置[依赖关系注入](../../fundamentals/dependency-injection.md)会完成此任务。

在 Startup.cs 中，调用 [AddDbContext 扩展方法](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs)来预配 ASP.NET DI 容器的 `DbContext` 类。 默认情况下，该方法将服务生存期设置为 `Scoped`。 `Scoped` 表示上下文对象生存期与 Web 请求生存期一致，并在 Web 请求结束时将自动调用 `Dispose` 方法。

## <a name="handling-transactions"></a>处理事务

默认情况下，Entity Framework 隐式实现事务。 在对多个行或表进行更改并调用 `SaveChanges` 的情况下，Entity Framework 自动确保所有更改都成功或全部失败。 如果完成某些更改后发生错误，这些更改会自动回退。 如果需要更多控制操作（例如，如果想要在事务中包含在 Entity Framework 外部完成的操作），请参阅[事务](https://docs.microsoft.com/ef/core/saving/transactions)。

## <a name="no-tracking-queries"></a>非跟踪查询

当数据库上下文检索表行并创建表示它们的实体对象时，默认情况下，它会跟踪内存中的实体是否与数据库中的内容同步。 更新实体时，内存中的数据充当缓存并使用该数据。 在 Web 应用程序中，此缓存通常是不必要的，因为上下文实例通常生存期较短（创建新的实例并用于处理每个请求），并且通常在再次使用该实体之前处理读取实体的上下文。

可以通过调用 `AsNoTracking` 方法禁用对内存中的实体对象的跟踪。 可能想要执行的典型方案包括以下操作：

* 在上下文生存期内，不需要更新任何实体，并且不需要 EF [自动加载具有由单独的查询检索的实体的导航属性](read-related-data.md)。 在控制器的 HttpGet 操作方法中经常遇到这些情况。

* 正在运行检索大量数据的查询，将只更新一小部分返回的数据。 关闭对大型查询的跟踪可能更有效，稍后为少数需要更新的实体运行查询。

* 想要附加一个实体来更新它，但之前为了其他目的，已检索了相同的实体。 由于数据库上下文已跟踪了该实体，因此无法附加要更改的实体。 处理这种情况的一种方法是在早前的查询上调用 `AsNoTracking`。

有关详细信息，请参阅[跟踪与非跟踪](https://docs.microsoft.com/ef/core/querying/tracking)。

## <a name="summary"></a>摘要

现在拥有一组完整的页面，可对 Student 实体执行简单的 CRUD 操作。 在下一个教程中，将通过添加排序、筛选和分页来扩展“索引”页。

>[!div class="step-by-step"]
[上一页](intro.md)
[下一页](sort-filter-page.md)  
