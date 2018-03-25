---
title: "Razor 页面和 EF Core - 排序、筛选、分页 - （第 3 个教程，共 8 个）"
author: rick-anderson
description: "本教程将使用 ASP.NET Core 和 Entity Framework Core 向页面添加排序、筛选和分页功能。"
ms.author: riande
ms.date: 10/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/sort-filter-page
ms.openlocfilehash: 9c1ee6f8c00f3cd501ea86fbf73f51ae540a010a
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-razor-pages-3-of-8"></a>排序、筛选、分页和分组 - EF Core 和 Razor 页面（第 3 个教程，共 8 个）

作者：[Tom Dykstra](https://github.com/tdykstra)、[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

本教程将添加排序、筛选、分组和分页功能。

下图显示完整的页面。 列标题是可单击的链接，可用于对列进行排序。 重复单击列标题可在升降和降序排序顺序之间切换。

![“学生索引”页](sort-filter-page/_static/paging.png)

如果遇到无法解决的问题，请下载[本阶段的已完成应用](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting)。

## <a name="add-sorting-to-the-index-page"></a>向索引页添加排序

向 Students/Index.cshtml.cs `PageModel` 添加字符串，使其包含排序参数：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet1&highlight=10-13)]


用以下代码更新 Students/Index.cshtml.cs `OnGetAsync`：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly)]

上述代码接收来自 URL 中的查询字符串的 `sortOrder` 参数。 该 URL（包括查询字符串）由[定位点标记帮助器](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper
)生成

`sortOrder` 参数为“名称”或“日期”。 `sortOrder` 参数后面可跟“_desc”以指定降序（可选）。 默认排序顺序为升序。

如果通过“学生”链接对“索引”页发起请求，则不会有任何查询字符串。 学生按姓氏升序显示。 按姓氏升序是 `switch` 语句中的默认顺序 (fall-through case)。 用户单击列标题链接时，查询字符串值中会提供相应的 `sortOrder` 值。

Razor 页面使用 `NameSort` 和 `DateSort` 为列标题超链接配置相应的查询字符串值：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=3-4)]

以下代码包含 C# [?: 运算符](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/conditional-operator)：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_Ternary)]

第一行指定当 `sortOrder` 为 NULL 或为空时，`NameSort` 设置为“name_desc”。 如果 `sortOrder` 不为 NULL 或不为空，则 `NameSort` 设置为空字符串。

`?: operator` 也称为三元运算符。

通过这两个语句，视图可如下设置列标题超链接：

| 当前排序顺序 | 姓氏超链接 | 日期超链接 |
|:--------------------:|:-------------------:|:--------------:|
| 姓氏升序 | descending        | ascending      |
| 姓氏降序 | ascending           | ascending      |
| 日期升序       | ascending           | descending     |
| 日期降序      | ascending           | ascending      |

该方法使用 LINQ to Entities 指定要作为排序依据的列。 此代码会初始化 switch 语句前面的 `IQueryable<Student> `，并在 switch 语句中对其进行修改：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=6-)]

 创建或修改 `IQueryable` 时，不会向数据库发送任何查询。 将 `IQueryable` 对象转换成集合后才能执行查询。 通过调用 `IQueryable` 等方法可将 `ToListAsync` 转换成集合。 因此，`IQueryable` 代码会生成单个查询，此查询直到出现以下语句才执行：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnlyRtn)]

`OnGetAsync` 可能获得包含大量列的详细信息。

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>向“学生索引”视图添加列标题超链接

将 Students/Index.cshtml 中的代码替换为以下突出显示的代码：

[!code-html[](intro/samples/cu/Pages/Students/Index2.cshtml?highlight=17-19,25-27)]

前面的代码：

* 向 `LastName` 和 `EnrollmentDate` 列标题添加超链接。
* 使用 `NameSort` 和 `DateSort` 中的信息为超链接设置当前的排序顺序值。

若要验证排序是否生效：

* 运行应用并选择“学生”选项卡。
* 单击“姓氏”。
* 单击“注册日期”。

若要更好地了解此代码：

* 请在 Student/Index.cshtml.cs 中的 `switch (sortOrder)` 上设置断点。
* 添加对 `NameSort` 和 `DateSort` 的监视。
* 在 Student/Index.cshtml 中的 `@Html.DisplayNameFor(model => model.Student[0].LastName)` 上设置断点。

单步执行调试程序。

## <a name="add-a-search-box-to-the-students-index-page"></a>向“学生索引”页添加搜索框

若要向“学生索引”页添加筛选：

* 需要向 Razor 页面添加一个文本框和一个提交按钮。 文本框会针对名字或姓氏提供一个搜索字符串。
* 页面模型随即更新以使用文本框值。

### <a name="add-filtering-functionality-to-the-index-method"></a>向 Index 方法添加筛选功能

用以下代码更新 Students/Index.cshtml.cs `OnGetAsync`：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

前面的代码：

* 向 `OnGetAsync` 方法添加 `searchString` 参数。 从下一部分中添加的文本框中所接收搜索字符串值。
* 已向 LINQ 语句添加 `Where` 子句。 `Where` 子句仅选择其名字或姓氏中包含搜索字符串的学生。 只有存在要搜索的值时才执行 LINQ 语句。

请注意：上述代码调用 `IQueryable` 对象上的 `Where` 方法，且在服务器上处理该筛选器。 在某些情况下，应用可能会将 `Where` 方法作为内存中集合上的扩展方法进行调用。 例如，假设 `_context.Students` 从 EF Core `DbSet` 更改为可返回 `IEnumerable` 集合的存储库方法。 结果通常是相同的，但在某些情况下可能不同。

例如，`Contains` 的 .NET Framework 实现会默认执行区分大小写的比较。 在 SQL Server 中，`Contains` 区分大小写由 SQL Server 实例的排序规则设置决定。 SQL Server 默认为不区分大小写。 可调用 `ToUpper`，进行不区分大小写的显式测试：

`Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())`

如果上述代码改为使用 `IEnumerable`，则该代码会确保结果区分大小写。 如果在 `IEnumerable` 集合上调用 `Contains`，则使用 .NET Core 实现。 如果在 `IQueryable` 对象上调用 `Contains`，则使用数据库实现。 从存储库返回 `IEnumerable` 可能会大幅降低性能：

1. 所有行均从 DB 服务器返回。
1. 筛选应用于应用程序中所有返回的行。

调用 `ToUpper` 不会对性能产生负面影响。 `ToUpper` 代码会在 TSQL SELECT 语句的 WHERE 子句中添加一个函数。 添加的函数会防止优化器使用索引。 如果安装的 SQL 区分大小写，则最好避免在不必要时调用 `ToUpper`。

### <a name="add-a-search-box-to-the-student-index-view"></a>向“学生索引”视图添加搜索框

在 Views/Student/Index.cshtml 中，添加以下突出显示的代码以创建“搜索”按钮和各种 chrome。

[!code-html[](intro/samples/cu/Pages/Students/Index3.cshtml?highlight=14-23&range=1-25)]

上述代码使用 `<form>` [标记帮助器](xref:mvc/views/tag-helpers/intro)来添加搜索文本框和按钮。 默认情况下，`<form>` 标记帮助器利用 POST 提交表单数据。 借助 POST，会在 HTTP 消息正文中而不是在 URL 中传递参数。 使用 HTTP GET 时，表单数据作为查询字符串在 URL 中进行传递。 通过查询字符串传递数据时，用户可对 URL 添加书签。 [W3C 指南](https://www.w3.org/2001/tag/doc/whenToUseGet.html)建议应在操作不引起更新的情况下使用 GET。

测试应用：

* 选择“学生”选项卡并输入搜索字符串。
* 选择“搜索”。

请注意，该 URL 包含搜索字符串。

```html
http://localhost:5000/Students?SearchString=an
```

如果页面具有书签，该书签将包含该页面的 URL 和 `SearchString` 查询字符串。 `form` 标记中的 `method="get"` 会导致生成查询字符串。

目前，选中列标题排序链接时，“搜索”框中的筛选值会丢失。 丢失的筛选值在下一部分进行修复。

## <a name="add-paging-functionality-to-the-students-index-page"></a>向“学生索引”页添加分页功能

本部分将创建一个 `PaginatedList` 类来支持分页。 `PaginatedList` 类使用 `Skip` 和 `Take` 语句在服务器上筛选数据，而不是检索所有表格行。 下图显示了分页按钮。

![带有分页链接的“学生索引”页](sort-filter-page/_static/paging.png)

在项目文件夹中，使用以下代码创建 `PaginatedList.cs`：

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

上述代码中的 `CreateAsync` 方法会提取页面大小和页码，并将相应的 `Skip` 和 `Take` 语句应用于 `IQueryable`。 当在 `IQueryable` 上调用 `ToListAsync` 时，它将返回仅包含所请求页的列表。 属性 `HasPreviousPage` 和 `HasNextPage` 用于启用或禁用“上一页”和“下一页”分页按钮。

`CreateAsync` 方法用于创建 `PaginatedList<T>`。 构造函数不能创建 `PaginatedList<T>` 对象；构造函数不能运行异步代码。

## <a name="add-paging-functionality-to-the-index-method"></a>向 Index 方法添加分页功能

在 Students/Index.cshtml.cs 中，将 `Student` 的类型从 `IList<Student>` 更新到 `PaginatedList<Student>`：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPageType)]

用以下代码更新 Students/Index.cshtml.cs `OnGetAsync`：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage&highlight=1-4,7-14,41-)]

上述代码会向方法签名添加页面索引、当前的 `sortOrder` 和 `currentFilter`。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage2)]

出现以下情况时，所有参数均为 NULL：

* 从“学生”链接调用页面。
* 用户尚未单击分页或排序链接。

单击分页链接后，页面索引变量将包含要显示的页码。

`CurrentSort` 为 Razor 页面提供当前排序顺序。 必须在分页链接中包含当前排序顺序才能在分页时保留排序顺序。

`CurrentFilter` 为 Razor 页面提供当前的筛选字符串。 `CurrentFilter` 值：

* 必须包含在分页链接中才能在分页过程中保留筛选设置。
* 必须在重新显示页面时还原到文本框。

如果在分页时更改搜索字符串，页码会重置为 1。 页面必须重置为 1，因为新的筛选器会导致显示不同的数据。 输入搜索值并选择“提交”时：

* 搜索字符串将会更改。
* `searchString` 参数不为 NULL。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage3)]

`PaginatedList.CreateAsync` 方法会将学生查询转换为支持分页的集合类型中的单个学生页面。 单个学生页面会传递到 Razor 页面。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage4)]

`PaginatedList.CreateAsync` 中的两个问号表示 [NULL 合并运算符](https://docs.microsoft.com/ dotnet/csharp/language-reference/operators/null-conditional-operator)。 NULL 合并运算符定义可为 NULL 的类型的默认值。 `(pageIndex ?? 1)` 表达式表示返回 `pageIndex` 的值（若带有值）。 如果 `pageIndex` 没有值，则返回 1。

## <a name="add-paging-links-to-the-student-razor-page"></a>向“学生”Razor 页面添加分页链接

更新 Students/Index.cshtml 中的标记。 突出显示所作更改：

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?highlight=28-31,37-40,68-)]

列标题链接使用查询字符串将当前搜索字符串传递到 `OnGetAsync` 方法，让用户可对筛选结果进行排序：

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?range=28-31)]

分页按钮由标记帮助器显示：

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?range=72-)]

运行应用并导航到学生页面。

* 为确保分页生效，请单击不同排序顺序的分页链接。
* 要验证确保分页后可正确地排序和筛选，请输入搜索字符串并尝试分页。

![带有分页链接的“学生索引”页](sort-filter-page/_static/paging.png)

若要更好地了解此代码：

* 请在 Student/Index.cshtml.cs 中的 `switch (sortOrder)` 上设置断点。
* 添加对 `NameSort`、`DateSort`、`CurrentSort` 和 `Model.Student.PageIndex` 的监视。
* 在 Student/Index.cshtml 中的 `@Html.DisplayNameFor(model => model.Student[0].LastName)` 上设置断点。

单步执行调试程序。

## <a name="update-the-about-page-to-show-student-statistics"></a>更新“关于”页以显示学生统计信息

此步骤将更新 Pages/About.cshtml，显示每个注册日期的已注册学生的数量。 更新需使用分组并包括以下步骤：

* 为“关于”页使用的数据创建视图模型。
* 修改“关于”Razor 页面和页面模型。

### <a name="create-the-view-model"></a>创建视图模型

在 Models 文件夹中创建一个 SchoolViewModels 文件夹。

在 SchoolViewModels 文件夹中，使用以下代码添加 EnrollmentDateGroup.cs：

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="update-the-about-page-model"></a>更新“关于”页面模型

用以下代码更新 Pages/About.cshtml.cs 文件：

[!code-csharp[Main](intro/samples/cu/Pages/About.cshtml.cs)]

LINQ 语句按注册日期对学生实体进行分组，计算每组中实体的数量，并将结果存储在 `EnrollmentDateGroup` 视图模型对象的集合中。

注意：EF Core 当前不支持 LINQ `group` 命令。 在上述代码中，所有学生记录均从 SQL Server 返回。 `group` 命令应用于 Razor 页面应用，而不是应用于 SQL Server。 EF Core 2.1 将支持此 LINQ `group` 运算符，并在 SQL Server 上进行分组。 请参阅 [Relational: Support translating GroupBy() to SQL](https://github.com/aspnet/EntityFrameworkCore/issues/2341)（关系：支持将 GroupBy() 转换到 SQL）。 [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/roadmap) 将随 .NET Core 2.1 一起发布。 有关详细信息，请参阅 [.NET Core Roadmap](https://github.com/dotnet/core/blob/master/roadmap.md)（.NET Core 路线图）。

### <a name="modify-the-about-razor-page"></a>修改“关于”Razor 页面

将 Views/Home/About.cshtml 文件中的代码替换为以下代码：

[!code-html[](intro/samples/cu/Pages/About.cshtml)]

运行应用并导航到“关于”页面。 表格中会显示每个注册日期的学生计数。

如果遇到无法解决的问题，请下载[本阶段的已完成应用](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting)。

![“关于”页面](sort-filter-page/_static/about.png)

## <a name="additional-resources"></a>其他资源

* [调试 ASP.NET Core 2.x 源](https://github.com/aspnet/Docs/issues/4155)

在下一教程中，应用将利用迁移更新数据模型。

>[!div class="step-by-step"]
[上一页](xref:data/ef-rp/crud)
[下一页](xref:data/ef-rp/migrations)
