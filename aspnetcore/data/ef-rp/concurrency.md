---
title: "Razor 页面和 EF Core - 并发 - 第 8 个教程（共 8 个）"
author: rick-anderson
description: "本教程介绍如何处理多个用户同时更新同一实体时出现的冲突。"
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/concurrency
ms.openlocfilehash: 1c6cdefa1410839606711d7460a8f4d0f1d6c72b
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2018
---
zh-cn/

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a>处理并发冲突 - EF Core 和 Razor 页面（第 8 个教程，共 8 个）

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Tom Dykstra](https://github.com/tdykstra) 和 [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

本教程介绍如何处理多个用户并发更新同一实体（同时）时出现的冲突。 如果遇到无法解决的问题，请下载[本阶段的已完成应用](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8)。

## <a name="concurrency-conflicts"></a>并发冲突

在以下情况下，会发生并发冲突：

* 用户导航到实体的编辑页面。
* 第一个用户的更改还未写入数据库之前，另一用户更新同一实体。

如果未启用并发检测，当发生并发更新时：

* 最后一个更新优先。 也就是最后一个更新的值保存至数据库。
* 第一个并发更新将会丢失。

### <a name="optimistic-concurrency"></a>乐观并发

乐观并发允许发生并发冲突，并在并发冲突发生时作出正确反应。 例如，Jane 访问院系编辑页面，将英语系的预算从 350,000.00 美元更改为 0.00 美元。

![将预算更改为零](concurrency/_static/change-budget.png)

在 Jane 单击“保存”之前，John 访问了相同页面，并将开始日期字段从 2007/1/9 更改为 2013/1/9。

![将开始日期更改为 2013](concurrency/_static/change-date.png)

Jane 先单击“保存”，并在浏览器显示索引页时看到她的更改。

![预算已更改为零](concurrency/_static/budget-zero.png)

John 单击“编辑”页面上的“保存”，但页面的预算仍显示为 350,000.00 美元。 接下来的情况取决于并发冲突的处理方式。

乐观并发包括以下选项：

* 可以跟踪用户已修改的属性，并仅更新数据库中相应的列。

 在这种情况下，数据不会丢失。 两个用户更新了不同的属性。 下次有人浏览英语系时，将看到 Jane 和 John 两个人的更改。 这种更新方法可以减少导致数据丢失的冲突数。 本方法：*如果对同一属性进行竞争性更改，则无法避免数据丢失。
        *通常不适用于 Web 应用。 它需要维持重要状态，以便跟踪所有提取值和新值。 维持大量状态可能影响应用性能。
        *相对于实体上的并发检测，可能增加应用复杂性。

* 可让 John 的更改覆盖 Jane 的更改。

 下次有人浏览英语系时，将看到 2013/9/1 和提取的值 350,000.00 美元。 这种方法称为“客户端优先”或“最后一个优先”方案。 （客户端的所有值优先于数据存储的值。）如果不对并发处理进行任何编码，则自动执行“客户端优先”。

* 可以阻止在数据库中更新 John 的更改。 通常，应用将：*显示错误消息。
        *显示数据的当前状态。
        *允许用户重新应用更改。

 这称为“存储优先”方案。 （数据存储值优先于客户端提交的值。）本教程实施“存储优先”方案。 此方法可确保用户在未收到警报时不会覆盖任何更改。

## <a name="handling-concurrency"></a>处理并发 

当属性配置为[并发令牌](https://docs.microsoft.com/ef/core/modeling/concurrency)时：

* EF Core 验证提取属性后是否未更改属性。 调用 [SaveChanges](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) 或 [SaveChangesAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) 时会执行此检查。
* 如果提取属性后更改了属性，将引发 [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0)。 

数据库和数据模型必须配置为支持引发 `DbUpdateConcurrencyException`。

### <a name="detecting-concurrency-conflicts-on-a-property"></a>检测属性的并发冲突

可使用 [ConcurrencyCheck](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) 特性在属性级别检测并发冲突。 该特性可应用于模型上的多个属性。 有关详细信息，请参阅[数据注释 - ConcurrencyCheck](https://docs.microsoft.com/ef/core/modeling/concurrency#data-annotations)。

本教程中不使用 `[ConcurrencyCheck]` 特性。

### <a name="detecting-concurrency-conflicts-on-a-row"></a>检测行的并发冲突

要检测并发冲突，请将 [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) 跟踪列添加到模型。  `rowversion`：

* 是 SQL Server 特定的。 其他数据库可能无法提供类似功能。
* 用于确定从数据库提取实体后未更改实体。 

数据库生成 `rowversion` 序号，该数字随着每次行的更新递增。 在 `Update` 或 `Delete` 命令中，`Where` 子句包括 `rowversion` 的提取值。 如果要更新的行已更改：

 * `rowversion` 不匹配提取值。
 * `Update` 或 `Delete` 命令不能找到行，因为 `Where` 子句包含提取的 `rowversion`。
 * 引发一个 `DbUpdateConcurrencyException`。

在 EF Core 中，如果未通过 `Update` 或 `Delete` 命令更新行，则引发并发异常。

### <a name="add-a-tracking-property-to-the-department-entity"></a>向 Department 实体添加跟踪属性

在 Models/Department.cs 中，添加名为 RowVersion 的跟踪属性：

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

[Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) 特性指定此列包含在 `Update` 和 `Delete` 命令的 `Where` 子句中。 该特性称为 `Timestamp`，因为之前版本的 SQL Server 在 SQL `rowversion` 类型将其替换之前使用 SQL `timestamp` 数据类型。

Fluent API 还可指定跟踪属性：

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

以下代码显示更新 Department 名称时由 EF Core 生成的部分 T-SQL：

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

前面突出显示的代码显示包含 `RowVersion` 的 `WHERE` 子句。 如果数据库 `RowVersion` 不等于 `RowVersion` 参数（`@p2`），则不更新行。

以下突出显示的代码显示验证更新哪一行的 T-SQL：

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

[@@ROWCOUNT](https://docs.microsoft.com/sql/t-sql/functions/rowcount-transact-sql) 返回受上一语句影响的行数。 在没有行更新的情况下，EF Core 引发 `DbUpdateConcurrencyException`。

在 Visual Studio 的输出窗口中可看见 EF Core 生成的 T-SQL。

### <a name="update-the-db"></a>更新数据库

添加 `RowVersion` 属性可更改数据库模型，这需要迁移。

生成项目。 在命令窗口中输入以下命令：

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

前面的命令：

* 添加 Migrations/{time stamp}_RowVersion.cs 迁移文件。
* 更新 Migrations/SchoolContextModelSnapshot.cs 文件。 此次更新将以下突出显示的代码添加到 `BuildModel` 方法：

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* 运行迁移以更新数据库。

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a>构架院系模型

* 退出 Visual Studio。
* 打开项目目录（包含 Program.cs、Startup.cs 和 .csproj 文件的目录）中的命令窗口。
* 运行下面的命令：

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

上述命令为 `Department` 模型创建基架。 在 Visual Studio 中打开项目。

生成项目。 此版本生成如下错误：

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 将 `_context.Department` 全局更改为 `_context.Departments`（即向 `Department` 添加一个“s”）。 找到并更新 7 个匹配项。

### <a name="update-the-departments-index-page"></a>更新院系索引页

基架引擎为索引页创建 `RowVersion` 列，但不应显示该字段。 本教程中显示 `RowVersion` 的最后一个字节，以帮助理解并发。 不能保证最后一个字节是唯一的。 实际应用不会显示 `RowVersion` 或 `RowVersion` 的最后一个字节。

更新索引页：

* 用院系替换索引。
* 将包含 `RowVersion` 的标记替换为 `RowVersion` 的最后一个字节。
* 将 FirstMidName 替换为 FullName。

以下标记显示更新后的页面：

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>更新编辑页模型

使用以下代码更新 pages\departments\edit.cshtml.cs：

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

要检测并发问题，请使用来自所提取实体的 `rowVersion` 值更新 [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue)。 EF Core 使用包含原始 `RowVersion` 值的 WHERE 子句生成 SQL UPDATE 命令。 如果没有行受到 UPDATE 命令影响（没有行具有原始 `RowVersion` 值），将引发 `DbUpdateConcurrencyException` 异常。

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

在前面的代码中，`Department.RowVersion` 为实体提取后的值。 使用此方法调用 `FirstOrDefaultAsync` 时，`OriginalValue` 为数据库中的值。

以下代码获取客户端值（向此方法发布的值）和数据库值：

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

以下代码为每列添加自定义错误消息，这些列中的数据库值与发布到 `OnPostAsync` 的值不同：

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

以下突出显示的代码将 `RowVersion` 值设置为从数据库检索的新值。 用户下次单击“保存”时，将仅捕获最后一次显示编辑页后发生的并发错误。

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

`ModelState` 具有旧的 `RowVersion` 值，因此需使用 `ModelState.Remove` 语句。 在 Razor 页面中，当两者都存在时，字段的 `ModelState` 值优于模型属性值。

## <a name="update-the-edit-page"></a>更新“编辑”页

使用以下标记更新 Pages/Departments/Edit.cshtml：

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

前面的标记：

* 将 `page` 指令从 `@page` 更新为 `@page "{id:int}"`。
* 添加隐藏的行版本。 必须添加 `RowVersion`，以便回发绑定值。
* 显示 `RowVersion` 的最后一个字节以进行调试。
* 将 `ViewData` 替换为强类型 `InstructorNameSL`。

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>使用编辑页测试并发冲突

在英语系打开编辑的两个浏览器实例：

* 运行应用，然后选择“院系”。
* 右键单击英语系的“编辑”超链接，然后选择“在新选项卡中打开”。
* 在第一个选项卡中，单击英语系的“编辑”超链接。

两个浏览器选项卡显示相同信息。

在第一个浏览器选项卡中更改名称，然后单击“保存”。

![更改后的“院系编辑”页 1](concurrency/_static/edit-after-change-1.png)

浏览器显示更改值并更新 rowVersion 标记后的索引页。 请注意更新后的 rowVersion 标记，它在其他选项卡的第二回发中显示。

在第二个浏览器选项卡中更改不同字段。

![更改后的“院系编辑”页 2](concurrency/_static/edit-after-change-2.png)

单击“保存” 。 可看见所有不匹配数据库值的字段的错误消息：

![“院系编辑”页错误消息](concurrency/_static/edit-error.png)

此浏览器窗口将不会更改名称字段。 将当前值（语言）复制并粘贴到名称字段。 退出选项卡。客户端验证将删除错误消息。

![“院系编辑”页错误消息](concurrency/_static/cv.png)

再次单击“保存”。 保存在第二个浏览器选项卡中输入的值。 在索引页中可以看到保存的值。

## <a name="update-the-delete-page"></a>更新“删除”页

使用以下代码更新“删除”页模型：

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

删除页检测提取实体并更改时的并发冲突。 提取实体后，`Department.RowVersion` 为行版本。 EF Core 创建 SQL DELETE 命令时，它包括具有 `RowVersion` 的 WHERE 子句。 如果 SQL DELETE 命令导致零行受影响：

* SQL DELETE 命令中的 `RowVersion` 与数据库中的 `RowVersion` 不匹配。
* 引发 DbUpdateConcurrencyException 异常。
* 使用 `concurrencyError` 调用 `OnGetAsync`。

### <a name="update-the-delete-page"></a>更新“删除”页

使用以下代码更新 Pages/Departments/Delete.cshtml：

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


上述标记进行以下更改：

* 将 `page` 指令从 `@page` 更新为 `@page "{id:int}"`。
* 添加错误消息。
* 将“管理员”字段中的 FirstMidName 替换为 FullName。
* 更改 `RowVersion` 以显示最后一个字节。
* 添加隐藏的行版本。 必须添加 `RowVersion`，以便回发绑定值。

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>使用删除页测试并发冲突

创建测试系。

在测试系打开删除的两个浏览器实例：

* 运行应用，然后选择“院系”。
* 右键单击测试系的“删除”超链接，然后选择“在新选项卡中打开”。
* 单击测试系的“编辑”超链接。

两个浏览器选项卡显示相同信息。

在第一个浏览器选项卡中更改预算，然后单击“保存”。

浏览器显示更改值并更新 rowVersion 标记后的索引页。 请注意更新后的 rowVersion 标记，它在其他选项卡的第二回发中显示。

从第二个选项卡中删除测试部门。并发错误显示来自数据库的当前值。 单击“删除”将删除实体，除非 `RowVersion` 已更新，院系已删除。

请参阅[继承](xref:data/ef-mvc/inheritance)了解如何继承数据模型。

### <a name="additional-resources"></a>其他资源

* [EF Core 中的并发令牌](https://docs.microsoft.com/ef/core/modeling/concurrency)
* [EF Core 中的并发处理](https://docs.microsoft.com/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[上一篇](xref:data/ef-rp/update-related-data)
