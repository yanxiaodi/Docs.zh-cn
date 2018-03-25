---
title: ASP.NET Core MVC 和 EF Core - 并发 - 第 8 个教程（共 10 个）
author: tdykstra
description: 本教程介绍如何处理多个用户同时更新同一实体时出现的冲突。
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/concurrency
ms.openlocfilehash: c271488d4da72ba340f3617ac20c7b6da2574c69
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2018
---
# <a name="handling-concurrency-conflicts---ef-core-with-aspnet-core-mvc-tutorial-8-of-10"></a>处理并发冲突 - EF Core 和 ASP.NET Core MVC 教程，第 8 个教程（共 10 个）

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University 示例 Web 应用程序演示如何使用 Entity Framework Core 和 Visual Studio 创建 ASP.NET Core MVC Web 应用程序。 若要了解教程系列，请参阅[本系列中的第一个教程](intro.md)。

在之前的教程中，你学习了如何更新数据。 本教程介绍如何处理多个用户同时更新同一实体时出现的冲突。

你将创建可处理 Department 实体的 Web 页面并处理并发错误。 下图显示了“编辑”和“删除”页面，包括发生并发冲突时显示的一些消息。

![“院系编辑”页](concurrency/_static/edit-error.png)

![“院系删除”页](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a>并发冲突

当某用户显示实体数据以对其进行编辑，而另一用户在上一用户的更改写入数据库之前更新同一实体的数据时，会发生并发冲突。 如果不启用此类冲突的检测，则最后更新数据库的人员将覆盖其他用户的更改。 在许多应用程序中，此风险是可接受的：如果用户很少或更新很少，或者一些更改被覆盖并不重要，则并发编程可能弊大于利。 在此情况下，不必配置应用程序来处理并发冲突。

### <a name="pessimistic-concurrency-locking"></a>悲观并发（锁定）

如果应用程序确实需要防止并发情况下出现意外数据丢失，一种方法是使用数据库锁定。 这称为悲观并发。 例如，在从数据库读取一行内容之前，请求锁定为只读或更新访问。 如果将一行锁定为更新访问，则其他用户无法将该行锁定为只读或更新访问，因为他们得到的是正在更改的数据的副本。 如果将一行锁定为只读访问，则其他人也可将其锁定为只读访问，但不能进行更新。

管理锁定有缺点。 编程可能很复杂。 它需要大量的数据库管理资源，且随着应用程序用户数量的增加，可能会导致性能问题。 由于这些原因，并不是所有的数据库管理系统都支持悲观并发。 Entity Framework Core 未提供对它的内置支持，并且本教程不展示其实现方式。

### <a name="optimistic-concurrency"></a>悲观并发

悲观并发的替代方法是乐观并发。 悲观并发是指允许发生并发冲突，并在并发冲突发生时作出正确反应。 例如，Jane 访问“院系编辑”页面，并将英语系的预算从 350,000.00 美元更改为 0.00 美元。

![将预算更改为零](concurrency/_static/change-budget.png)

在 Jane 单击“保存”之前，John 访问了相同页面，并将开始日期字段从 2007/1/9 更改为 2013/1/9。

![将开始日期更改为 2013](concurrency/_static/change-date.png)

Jane 先单击“保存”，并在浏览器返回索引页时看到她的更改。

![预算已更改为零](concurrency/_static/budget-zero.png)

然后 John 单击“编辑”页面上的“保存”，但页面上预算仍显示为 350,000.00 美元。 接下来的情况取决于并发冲突的处理方式。

其中一些选项包括：

* 可以跟踪用户已修改的属性，并仅更新数据库中相应的列。

     在示例方案中，不会有数据丢失，因为是由两个用户更新不同的属性。 下次有人浏览英语系时，将看到 Jane 和 John 两个人的更改 - 开始日期为 9/1/2013，预算为零美元。 这种更新方法可减少可能导致数据丢失的冲突次数，但是如果对实体的同一属性进行竞争性更改，则数据难免会丢失。 Entity Framework 是否以这种方式工作取决于更新代码的实现方式。 通常不适合在 Web 应用程序中使用，因为它要求保持大量的状态，以便跟踪实体的所有原始属性值以及新值。 维护大量的状态可能会影响应用程序的性能，因为它需要服务器资源或必须包含在网页本身（例如隐藏字段）或 Cookie 中。

* 可让 John 的更改覆盖 Jane 的更改。

     下次有人浏览英语系时，将看到 2013/1/9 和还原的值 350,000.00 美元。 这称为“客户端优先”或“最后一个优先”。 （客户端的所有值优先于数据存储的值。）正如本部分的介绍所述，如果不为并发处理编写任何代码，则自动执行此操作。

* 可以阻止在数据库中更新 John 的更改。

     通常，将显示一条错误消息，向他显示数据的当前状态，并允许他重新应用其更改（若他仍想更改）。 这称为“存储优先”方案。 （数据存储值优先于客户端提交的值。）本教程将执行“存储优先”方案。 此方法可确保用户在未收到具体发生内容的警报时，不会覆盖任何更改。

### <a name="detecting-concurrency-conflicts"></a>检测并发冲突

你可以通过处理 Entity Framework 引发的 `DbConcurrencyException` 异常来解决冲突。 为了知道何时引发这些异常，Entity Framework 必须能够检测到冲突。 因此，你必须正确配置数据库和数据模型。 启用冲突检测的某些选项包括：

* 数据库表中包含一个可用于确定某行更改时间的跟踪列。 然后可配置 Entity Framework，将该列包含在 SQL Update 或 Delete 命令的 Where 子句中。

     跟踪列的数据类型通常是 `rowversion`。 `rowversion` 值是一个序列号，该编号随着每次行的更新递增。 在 Update 或 Delete 命令中，Where 子句包含跟踪列的原始值（原始行版本）。 如果正在更新的行已被其他用户更改，则 `rowversion` 列中的值与原始值不同，这导致 Update 或 Delete 语句由于 Where 子句而找不到要更新的行。 当 Entity Framework 发现 Update 或 Delete 命令没有更新行（即受影响的行数为零）时，便将其解释为并发冲突。

* 配置 Entity Framework，在 Update 或 Delete 命令的 Where 子句中包含表中每个列的原始值。

     与第一个选项一样，如果行自第一次读取后发生更改，Where 子句将不返回要更新的行，Entity Framework 会将其解释为并发冲突。 对于包含许多列的数据库表，此方法可能导致非常多的 Where 子句，并且可能需要维持大量的状态。 如前所述，维持大量的状态会影响应用程序的性能。 因此通常不建议使用此方法，并且它也不是本教程中使用的方法。

     如果确实想要实现此并发方法，必须通过向要跟踪并发的实体添加 `ConcurrencyCheck` 属性来标记它们的所有非主键属性。 所作更改使 Entity Framework 能够将所有列包含在 Update 和 Delete 语句的 SQL Where 子句中。

在本教程的其余部分，将向 Department 实体添加 `rowversion` 跟踪属性，创建控制器和视图，并进行测试以验证是否一切正常工作。

## <a name="add-a-tracking-property-to-the-department-entity"></a>向 Department 实体添加跟踪属性

在 Models/Department.cs 中，添加名为 RowVersion 的跟踪属性：

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

`Timestamp` 属性指定此列将包含在发送到数据库的 Update 和 Delete 命令的 Where 子句中。 该属性称为 `Timestamp`，因为 SQL Server 的之前版本在 SQL `rowversion` 类型将其替换之前使用 SQL `timestamp` 数据类型。 用于 `rowversion` 的 .NET 类型为字节数组。

如果更愿意使用 Fluent API，可使用 `IsConcurrencyToken` 方法（路径为 Data/SchoolContext.cs）指定跟踪属性，如下例所示：

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

通过添加属性，更改了数据库模型，因此需要再执行一次迁移。

保存更改并生成项目，然后在命令窗口中输入以下命令：

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a>创建“院系”控制器和视图

像之前在学生、课程和讲师教程中操作的那样，为“院系”控制器和视图创建基架。

![“为院系”创建基架](concurrency/_static/add-departments-controller.png)

在 DepartmentsController.cs 文件中，将出现的 4 次“FirstMidName”更改为“FullName”，以便院系管理员下拉列表将包含讲师的全名，而不仅仅是姓氏。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a>更新“院系索引”视图

基架引擎在索引视图中创建 RowVersion 列，但不应显示该字段。

将 Views/Departments/Index.cshtml 中的代码替换为以下代码。

[!code-html[Main](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

这会将标题更改为“院系”，删除 RowVersion 列，并显示全名（而非管理员的名字）。

## <a name="update-the-edit-methods-in-the-departments-controller"></a>更新“院系”控制器中的编辑方法

在 HttpGet `Edit` 方法和 `Details` 方法中，添加 `AsNoTracking`。 在 HttpGet `Edit` 方法中，为管理员添加预先加载。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

将 HttpPost `Edit` 方法的现有代码替换为以下代码：

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

代码先尝试读取要更新的院系。 如果 `SingleOrDefaultAsync` 方法返回 NULL，则该院系已被另一用户删除。 此情况下，代码将使用已发布的表单值创建院系实体，以便“编辑”页重新显示错误消息。 或者，如果仅显示错误消息而未重新显示院系字段，则不必重新创建 Department 实体。

该视图将原始 `RowVersion` 值存储在隐藏字段中，且此方法在 `rowVersion` 参数中接收该值。 在调用 `SaveChanges` 之前，必须将该原始 `RowVersion` 属性值置于实体的 `OriginalValues` 集合中。

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

然后，当 Entity Framework 创建 SQL UPDATE 命令时，该命令将包含一个 WHERE 子句，用于查找具有原始 `RowVersion` 值的行。 如果没有行受到 UPDATE 命令影响（没有行具有原始 `RowVersion` 值），则 Entity Framework 会引发 `DbUpdateConcurrencyException` 异常。

该异常的 catch 块中的代码获取受影响的 Department 实体，该实体具有来自异常对象上的 `Entries` 属性的更新值。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

`Entries` 集合将仅包含一个 `EntityEntry` 对象。  该对象可用于获取用户输入的新值和当前的数据库值。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

对于所含的数据库值与用户在“编辑”页上输入的值不同的每一列，该代码均为其添加一个自定义错误消息（为简洁起见，此处仅显示一个字段）。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

最后，该代码将 `departmentToUpdate` 的 `RowVersion` 值设置为从数据库中检索到的新值。 重新显示“编辑”页时，这个新的 `RowVersion` 值将存储在隐藏字段中，当用户下次单击“保存”时，将只捕获自“编辑”页重新显示起发生的并发错误。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

`ModelState` 具有旧的 `RowVersion` 值，因此需使用 `ModelState.Remove` 语句。 在此视图中，当两者都存在时，字段的 `ModelState` 值优于模型属性值。

## <a name="update-the-department-edit-view"></a>更新“院系编辑”视图

在 Views/Departments/Edit.cshtml 中，进行以下更改：

* 添加隐藏字段以保存 `RowVersion` 属性值，紧跟在 `DepartmentID` 属性的隐藏字段后面。

* 向下拉列表添加“选择管理员”选项。

[!code-html[Main](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a>测试“编辑”页中的并发冲突

运行应用并转到“院系索引”页。 右键单击英语系的“编辑”超链接，并选择“在新选项卡中打开”，然后单击英语系的“编辑”超链接。 现在，两个浏览器选项卡显示相同的信息。

在第一个浏览器选项卡中更改一个字段，然后单击“保存”。

![更改后的“院系编辑”页 1](concurrency/_static/edit-after-change-1.png)

浏览器显示具有更改值的索引页。

在第二个浏览器选项卡中更改一个字段。

![更改后的“院系编辑”页 2](concurrency/_static/edit-after-change-2.png)

单击“保存” 。 看见一条错误消息：

![“院系编辑”页错误消息](concurrency/_static/edit-error.png)

再次单击“保存”。 保存在第二个浏览器选项卡中输入的值。 在索引页中出现时，可以看到已保存的值。

## <a name="update-the-delete-page"></a>更新“删除”页

对于“删除”页，Entity Framework 以类似方式检测其他人编辑院系所引起的并发冲突。 当 HttpGet `Delete` 方法显示确认视图时，该视图包含隐藏字段中的原始 `RowVersion` 值。 然后，该值可用于在用户确认删除时进行调用的 HttpPost `Delete` 方法。 当 Entity Framework 创建 SQL DELETE 命令时，它包含具有原始 `RowVersion` 值的 WHERE 子句。 如果该命令不影响任何行（即在显示“删除”确认页面后更改了行），则引发并发异常，调用 HttpGet `Delete` 方法，并将错误标志设置为 true，以重新显示具有一条错误消息的确认页面。 任意行均未受影响的原因也可能是，该行已被其他用户删除，因此在此情况下不显示错误消息。

### <a name="update-the-delete-methods-in-the-departments-controller"></a>更新“院系”控制器中的删除方法

在 DepartmentsController.cs 中，将 HttpGet `Delete` 方法替换为以下代码：

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

该方法接受可选参数，该参数指示是否在并发错误之后重新显示页面。 如果此标志为 true 且指定的院系不复存在，则它已被其他用户删除。 此情况下，代码将重定向到索引页。  如果此标志为 true 且该院系确实存在，则它已被其他用户更改。 此情况下，代码将使用 `ViewData` 向视图发送一条错误消息。  

将 HttpPost `Delete` 方法（名为 `DeleteConfirmed`）中的代码替换为以下代码：

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

在刚替换的基架代码中，此方法仅接受记录 ID：


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

已将此参数更改为由模型绑定器创建的 Department 实体实例。 这样，EF 就可访问除记录键之外的 RowVersion 属性值。

```csharp
public async Task<IActionResult> Delete(Department department)
```

你还将操作方法名称从 `DeleteConfirmed` 更改为了 `Delete`。 基架代码使用 `DeleteConfirmed` 名称来为 HttpPost 方法提供唯一签名。 （CLR 要求重载方法具有不同的方法参数。）既然签名是唯一的，就可继续使用 MVC 约定，并为 HttpPost 和 HttpGet 删除方法使用同一名称。

如果已删除院系，则 `AnyAsync` 方法返回 false，应用程序仅返回到 Index 方法。

如果捕获到并发错误，代码将重新显示“删除”确认页，并提供一个指示它应显示并发错误消息的标志。

### <a name="update-the-delete-view"></a>更新“删除”视图

在 Views/Departments/Delete.cshtml 中，将基架代码替换为添加 DepartmentID 和 RowVersion 属性的错误消息字段和隐藏字段的以下代码。 突出显示所作更改。

[!code-html[Main](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

这将进行以下更改：

* 在 `h2` 和 `h3` 标题之间添加错误消息。

* 将“管理员”字段中的 FirstMidName 替换为 FullName。

* 删除 RowVersion 字段。

* 添加 `RowVersion` 属性的隐藏字段。

运行应用并转到“院系索引”页。 右键单击英语系的“删除”超链接，并选择“在新选项卡中打开”，然后在第一个选项卡中单击英语系的“编辑”超链接。

在第一个窗口中，更改其中一个值，然后单击“保存”：

![删除之前显示的更改后的“院系删除”页面](concurrency/_static/edit-after-change-for-delete.png)

在第二个选项卡中，单击“删除”。 你将看到并发错误消息，且已使用数据库中的当前内容刷新了“院系”值。

![显示有并发错误的“院系删除”确认页](concurrency/_static/delete-error.png)

如果再次单击“删除”，会重定向到已删除显示院系的索引页。

## <a name="update-details-and-create-views"></a>更新“详细信息”和“创建”视图

可选择性地清理“详细信息”和“创建”视图中的基架代码。

替换 Views/Departments/Details.cshtml 中的代码，以删除 RowVersion 列并显示管理员的全名。

[!code-html[Main](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

替换 Views/Departments/Create.cshtml 中的代码，向下拉列表添加“选择”选项。

[!code-html[Main](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a>摘要

处理并发冲突已介绍完毕。 要深入了解如何处理 EF Core 中的并发，请参阅[并发冲突](https://docs.microsoft.com/ef/core/saving/concurrency)。 下一个教程将介绍如何为 Instructor 和 Students 实体实现“每个层次结构一个表”继承。

>[!div class="step-by-step"]
[上一页](update-related-data.md)
[下一页](inheritance.md)  
