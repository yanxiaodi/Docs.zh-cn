---
title: "ASP.NET Core MVC 和 EF Core - 排序、筛选、分页 - 第 3 个教程，共 10 个教程"
author: tdykstra
description: "本教程将使用 ASP.NET Core 和 Entity Framework Core 向页面添加排序、筛选和分页功能。"
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: feb4a50c9e5602064e7d493b6991485949903f47
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a>排序、筛选、分页和分组 - EF Core 和 ASP.NET Core MVC 教程（第 3 个，共 10 个）

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University 示例 Web 应用程序演示如何使用 Entity Framework Core 和 Visual Studio 创建 ASP.NET Core MVC Web 应用程序。 若要了解教程系列，请参阅[本系列中的第一个教程](intro.md)。

在上一个教程中，已为 Student 实体实现了一组网页用于执行基本的 CRUD 操作。 在本教程中，将向学生索引页添加排序、筛选和分页功能。 同时，还将创建一个执行简单分组的页面。

下图展示了完成操作后的页面外观。 列标题是用户可以单击以按该列排序的链接。 重复单击列标题可在升降排序顺序之间切换。

![“学生索引”页](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>向学生索引页添加列排序链接

要向学生索引页添加排序功能，需更改学生控制器的 `Index` 方法并将代码添加到学生索引视图。

### <a name="add-sorting-functionality-to-the-index-method"></a>向 Index 方法添加排序功能

在 StudentsController.cs 中，将 `Index` 方法替换为以下代码：

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

此代码接收来自 URL 中的查询字符串的 `sortOrder` 参数。 查询字符串值由 ASP.NET Core MVC 提供，作为操作方法的参数。 该参数将是一个字符串，可为“Name”或“Date”，可选择后跟下划线和字符串“desc”来指定降序。 默认排序顺序为升序。

首次请求索引页时，没有任何查询字符串。 学生按照姓氏升序显示，这是 `switch` 语句中的 fall-through 事例所建立的默认值。 当用户单击列标题超链接时，查询字符串中会提供相应的 `sortOrder` 值。

视图使用两个 `ViewData` 元素（NameSortParm 和 DateSortParm）来为列标题超链接配置相应的查询字符串值。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

这些是三元语句。 第一个指定如果 `sortOrder` 参数为 NULL 或空，则应将 NameSortParm 设置为“name_desc”；否则，应将其设置为空字符串。 通过这两个语句，视图可如下设置列标题超链接：

|  当前排序顺序  | 姓氏超链接 | 日期超链接 |
|:--------------------:|:-------------------:|:--------------:|
| 姓氏升序  | descending          | ascending      |
| 姓氏降序 | ascending           | ascending      |
| 日期升序       | ascending           | descending     |
| 日期降序排列 | 升序 | 升序 |

该方法使用 LINQ to Entities 指定要作为排序依据的列。 该代码在 switch 语句之前创建一个 `IQueryable` 变量，在 switch 语句中对其进行修改，并在 `switch` 语句后调用 `ToListAsync` 方法。 当创建和修改 `IQueryable` 变量时，不会向数据库发送任何查询。 只有通过调用 `ToListAsync` 之类的方法将 `IQueryable` 对象转换为集合，查询才会执行。 因此，此代码导致直到 `return View` 语句才会执行单个查询。

此代码可能获得包含大量列的详细信息。 [本系列的最后一个教程](advanced.md#dynamic-linq)演示了如何编写在字符串变量中传递 `OrderBy` 列名的代码。

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>向“学生索引”视图添加列标题超链接

将 Views / Students / Index.cshtml 中的代码替换为以下代码，以添加列标题超链接。 已更改的行为突出显示状态。

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

此代码使用 `ViewData` 属性中的信息来设置具有相应查询字符串值的超链接。

运行应用，选择“学生”选项卡，然后单击“姓氏”和“注册日期”列标题以验证排序是否正常工作。

![按姓名顺序的学生索引页](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>向“学生索引”页添加搜索框

要向学生索引页添加筛选功能，需将文本框和提交按钮添加到视图，并在 `Index` 方法中做出相应的更改。 在文本框中输入一个字符串以在名字和姓氏字段中进行搜索。

### <a name="add-filtering-functionality-to-the-index-method"></a>向 Index 方法添加筛选功能

在 StudentsController.cs 中，将 `Index` 方法替换为以下代码（所做的更改为突出显示状态）。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

已向 `Index` 方法添加 `searchString` 参数。 从要添加到索引视图的文本框中接收搜索字符串值。 并且，还向 LINQ 语句添加了 where 子句，该子句仅选择名字或姓氏中包含搜索字符串的学生。 只有在有搜索值的情况下，才会执行添加了 where 子句的语句。

> [!NOTE]
> 此处正在调用 `IQueryable` 对象上的 `Where` 方法，并且将在服务器上处理该筛选器。 在某些情况下，可能会将 `Where` 方法作为内存中集合上的扩展方法进行调用。 （例如，假设将引用更改为 `_context.Students`，这样它引用的不再是 EF `DbSet`，而是一个返回 `IEnumerable` 集合的存储库方法。）结果通常是相同的，但在某些情况下可能不同。
>
>例如，`Contains` 方法的 .NET Framework 实现在默认情况下执行区分大小写比较，但在 SQL Server 中，这由 SQL Server 实例的排序规则设置决定。 该设置默认为不区分大小写。 可调用 `ToUpper` 方法使测试显式不区分大小写：Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())。 如果稍后更改代码以使用返回 `IEnumerable` 集合而不是 `IQueryable` 对象的存储库，则可确保结果保持不变。 （在 `IEnumerable` 集合上调用 `Contains` 方法时，将获得 .NET Framework 实现；当在 `IQueryable` 对象上调用它时，将获得数据库提供程序实现。）但是，此解决方案会降低性能。 `ToUpper` 代码将在 TSQL SELECT 语句的 WHERE 子句中放置一个函数。 这将阻止优化器使用索引。 鉴于 SQL 主要安装为不区分大小写，在迁移到区分大小写的数据存储之前，最好避免使用 `ToUpper` 代码。

### <a name="add-a-search-box-to-the-student-index-view"></a>向“学生索引”视图添加搜索框

在 Views/Student/Index.cshtml 中，请在打开表格标签之前立即添加突出显示的代码，以创建标题栏、文本框和搜索按钮。

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

此代码使用 `<form>` [标记帮助器](xref:mvc/views/tag-helpers/intro)添加搜索文本框和按钮。 默认情况下，`<form>` 标记帮助器使用 POST 提交表单数据，这意味着参数在 HTTP 消息正文中传递，而不是作为查询字符串在 URL 中传递。 当指定 HTTP GET 时，表单数据作为查询字符串在 URL 中传递，从而使用户能够将 URL 加入书签。 W3C 指南建议在操作不会导致更新时使用 GET。

运行应用，选择“学生”选项卡，输入搜索字符串，然后单击“搜索”以验证筛选是否正常工作。

![带有筛选功能的学生索引页](sort-filter-page/_static/filtering.png)

请注意，该 URL 包含搜索字符串。

```html
http://localhost:5813/Students?SearchString=an
```

如果将此页加入书签，当使用书签时，将获得已筛选的列表。 向 `form` 标记添加 `method="get"` 是导致生成查询字符串的原因。

在此阶段，如果单击列标题排序链接，则会丢失已在“搜索”框中输入的筛选器值。 此问题将在下一部分得以解决。

## <a name="add-paging-functionality-to-the-students-index-page"></a>向“学生索引”页添加分页功能

要向学生索引页添加分页功能，需创建一个使用 `Skip` 和 `Take` 语句的 `PaginatedList` 类来筛选服务器上的数据，而不总是对表中的所有行进行检索。 然后在 `Index` 方法中进行其他更改，并将分页按钮添加到 `Index` 视图。 下图显示了分页按钮。

![带有分页链接的“学生索引”页](sort-filter-page/_static/paging.png)

在项目文件夹中，创建 `PaginatedList.cs`，然后用以下代码替换模板代码。

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

此代码中的 `CreateAsync` 方法将提取页面大小和页码，并将相应的 `Skip` 和 `Take` 语句应用于 `IQueryable`。 当在 `IQueryable` 上调用 `ToListAsync` 时，它将返回仅包含请求页的列表。 属性 `HasPreviousPage` 和 `HasNextPage` 可用于启用或禁用“上一页”和“下一页”的分页按钮。

由于构造函数不能运行异步代码，因此使用 `CreateAsync` 方法来创建 `PaginatedList<T>` 对象，而非构造函数。

## <a name="add-paging-functionality-to-the-index-method"></a>向 Index 方法添加分页功能

在 StudentsController.cs 中，将 `Index` 方法替换为以下代码。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

该代码向方法签名中添加一个页码参数、一个当前排序顺序参数和一个当前筛选器参数。

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

第一次显示页面时，或者如果用户没有单击分页或排序链接，所有参数都将为 NULL。  如果单击了分页链接，页面变量将包含要显示的页码。

名为 CurrentSort 的 `ViewData` 元素为视图提供当前排序顺序，因为此值必须包含在分页链接中，以便在分页时保持排序顺序相同。

名为 CurrentFilter 的 `ViewData` 元素为视图提供当前筛选器字符串。 此值必须包含在分页链接中，以便在分页过程中保持筛选器设置，并且在页面重新显示时必须将其还原到文本框中。

如果在分页过程中搜索字符串发生变化，则页面必须重置为 1，因为新的筛选器会导致显示不同的数据。 在文本框中输入值并按下“提交”按钮时，搜索字符串将被更改。 在这种情况下，`searchString` 参数不为 NULL。

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

在 `Index` 方法最后，`PaginatedList.CreateAsync` 方法会将学生查询转换为支持分页的集合类型中的学生的单个页面。 然后将学生的单个页面传递给视图。

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

`PaginatedList.CreateAsync` 方法需要一个页码。 两个问号表示 NULL 合并运算符。 NULL 合并运算符为可为 NULL 的类型定义默认值；表达式 `(page ?? 1)` 表示如果 `page` 有值，则返回该值，如果 `page` 为 NULL，则返回 1。

## <a name="add-paging-links-to-the-student-index-view"></a>向学生索引视图添加分页链接

在 Views/Students/Index.cshtml 中，将现有代码替换为以下代码。 突出显示所作更改。

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

页面顶部的 `@model` 语句指定视图现在获取的是 `PaginatedList<T>` 对象，而不是 `List<T>` 对象。

列标题链接使用查询字符串向控制器传递当前搜索字符串，以便用户可以在筛选结果中进行排序：

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

分页按钮由标记帮助器显示：

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

运行应用并转到“学生”页。

![带有分页链接的“学生索引”页](sort-filter-page/_static/paging.png)

单击不同排序顺序的分页链接，以确保分页正常工作。 然后输入一个搜索字符串并再次尝试分页，以验证分页也可以正确地进行排序和筛选。

## <a name="create-an-about-page-that-shows-student-statistics"></a>创建显示学生统计信息的“关于”页

对于 Contoso 大学网站的“关于”页，将显示每个注册日期注册了多少名学生。 这需要对组进行分组和简单计算。 若要完成此操作，需要执行以下操作：

* 为需要传递给视图的数据创建一个视图模型类。

* 修改主控制器中的“关于”方法。

* 修改“关于”视图。

### <a name="create-the-view-model"></a>创建视图模型

在 Models 文件夹中创建一个 SchoolViewModels 文件夹。

在新文件夹中，添加一个类文件 EnrollmentDateGroup.cs，并用以下代码替换模板代码：

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a>修改主控制器

在 HomeController.cs 中，使用文件顶部的语句添加以下内容：

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

在类的左大括号之后立即为数据库上下文添加一个类变量，并从 ASP.NET Core DI 获取上下文的实例：

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

将 `About` 方法替换为以下代码：

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

LINQ 语句按注册日期对学生实体进行分组，计算每组中实体的数量，并将结果存储在 `EnrollmentDateGroup` 视图模型对象的集合中。
> [!NOTE] 
> 在 1.0 版 Entity Framework Core 中，整个结果集被返回给客户端，并在客户端上完成分组。 在某些情况下，这可能会造成性能问题。 请务必使用生产数据量测试性能，必要时请使用原始 SQL 在服务器上进行分组。 有关如何使用原始 SQL 的信息，请参阅[本系列中的最后一个教程](advanced.md)。

### <a name="modify-the-about-view"></a>修改“关于”视图

将 Views/Home/About.cshtml 文件中的代码替换为以下代码：

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

运行应用并转到“关于”页。 表格中会显示每个注册日期的学生计数。

![“关于”页面](sort-filter-page/_static/about.png)

## <a name="summary"></a>摘要

在本教程中，你已了解了如何执行排序、筛选、分页和分组。 在下一个教程中，你将了解如何使用迁移来处理数据模型更改。

>[!div class="step-by-step"]
[上一页](crud.md)
[下一页](migrations.md)  
