---
title: "Razor 页面和 EF Core - 读取相关数据 - 第 6 个教程（共 8 个）"
author: rick-anderson
description: "在本教程中，将读取并显示相关数据 - 即 Entity Framework 加载到导航属性中的数据。"
manager: wpickett
ms.author: riande
ms.date: 11/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/read-related-data
ms.openlocfilehash: ccb1e95ae2b43fd0a4c4b1ac9ed58a4d474ab3b6
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a>读取相关数据 - EF Core 和 Razor 页面 - 第 6 个教程（共 8 个）

作者：[Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

在本教程中，将读取和显示相关数据。 相关数据为 EF Core 加载到导航属性中的数据。

如果遇到无法解决的问题，请下载[本阶段的已完成应用](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related)。

下图显示了本教程中已完成的页面：

![“课程索引”页](read-related-data/_static/courses-index.png)

![“讲师索引”页](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>相关数据的预先加载、显式加载和延迟加载

EF Core 可采用多种方式将相关数据加载到实体的导航属性中：

* [预先加载](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading)。 预先加载是指对查询某类型的实体时一并加载相关实体。 读取实体时，会检索其相关数据。 此时通常会出现单一联接查询，检索所有必需数据。 EF Core 将针对预先加载的某些类型发出多个查询。 与存在单一查询的 EF6 中的某些查询相比，发出多个查询可能更有效。 预先加载通过 `Include` 和 `ThenInclude` 方法进行指定。

 ![预先加载示例](read-related-data/_static/eager-loading.png)
 
 当包含集合导航时，预先加载会发送多个查询：

 * 一个查询用于主查询 
 * 一个查询用于加载树中每个集合“边缘”。

* 使用 `Load` 的单独查询：可在单独的查询中检索数据，EF Core会“修复”导航属性。 “修复”是指 EF Core 自动填充导航属性。 与预先加载相比，使用 `Load` 的单独查询更像是显式加载。

 ![单独查询示例](read-related-data/_static/separate-queries.png)

 请注意：EF Core 会将导航属性自动“修复”为之前加载到上下文实例中的任何其他实体。 即使导航属性的数据非显式包含在内，但如果先前加载了部分或所有相关实体，则仍可能填充该属性。

* [显式加载](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading)。 首次读取实体时，不检索相关数据。 必须编写代码才能在需要时检索相关数据。 使用单独查询进行显式加载时，会向数据库发送多个查询。 该代码通过显式加载指定要加载的导航属性。 使用 `Load` 方法进行显式加载。 例如：

 ![显式加载示例](read-related-data/_static/explicit-loading.png)

* [延迟加载](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading)。 [EF Core 当前不支持延迟加载](https://github.com/aspnet/EntityFrameworkCore/issues/3797)。 首次读取实体时，不检索相关数据。 首次访问导航属性时，会自动检索该导航属性所需的数据。 首次访问导航属性时，都会向数据库发送一个查询。

* `Select` 运算符仅加载所需的相关数据。

## <a name="create-a-courses-page-that-displays-department-name"></a>创建显示院系名称的“课程”页

课程实体包括一个带 `Department` 实体的导航属性。 `Department` 实体包含要分配课程的院系。

要在课程列表中显示已分配院系的名称：

* 从 `Department` 实体中获取 `Name` 属性。
* `Department` 实体来自于 `Course.Department` 导航属性。

![ourse.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a>为课程模型创建基架

* 退出 Visual Studio。
* 打开项目目录（包含 Program.cs、Startup.cs 和 .csproj 文件的目录）中的命令窗口。
* 运行下面的命令：

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

上述命令为 `Course` 模型创建基架。 在 Visual Studio 中打开项目。

生成项目。 此版本生成如下错误：

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 将 `_context.Course` 全局更改为 `_context.Courses`（即向 `Course` 添加一个“s”）。 找到并更新 7 个匹配项。

打开 Pages/Courses/Index.cshtml.cs 并检查 `OnGetAsync` 方法。 基架引擎为 `Department` 导航属性指定了预先加载。 `Include` 方法指定预先加载。

运行应用并选择“课程”链接。 院系列显示 `DepartmentID`（该项无用）。

使用以下代码更新 `OnGetAsync` 方法：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

上述代码添加了 `AsNoTracking`。 由于未跟踪返回的实体，因此 `AsNoTracking` 提升了性能。 未跟踪实体，因为未在当前上下文中更新这些实体。

使用以下突出显示的标记更新 Views/Courses/Index.cshtml：

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

对基架代码进行了以下更改：

* 将标题从“索引”更改为“课程”。
* 添加了显示 `CourseID` 属性值的“数字”列。 默认情况下，不针对主键进行架构，因为对最终用户而言，它们通常没有意义。 但在此情况下主键是有意义的。
* 更改“院系”列，显示院系名称。 该代码显示已加载到 `Department` 导航属性中的 `Department` 实体的 `Name` 属性：

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

运行应用并选择“课程”选项卡，查看包含系名称的列表。

![“课程索引”页](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a>使用 Select 加载相关数据

`OnGetAsync` 方法使用 `Include` 方法加载相关数据：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`Select` 运算符仅加载所需的相关数据。 对于单个项（如 `Department.Name`），它使用 SQL INNER JOIN。 对于集合，它使用另一个数据库访问，但集合上的 `Include`  运算符也是如此。

以下代码使用 `Select` 方法加载相关数据：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`CourseViewModel`：

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

有关完整示例的信息，请参阅 [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) 和 [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs)。

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>创建显示“课程”和“注册”的“讲师”页

在本部分中，将创建“讲师”页。

<a name="IP"></a>
![“讲师索引”页](read-related-data/_static/instructors-index.png)

该页面通过以下方式读取和显示相关数据：

* 讲师列表显示 `OfficeAssignment` 实体（上图中的办公室）的相关数据。 `Instructor` 和 `OfficeAssignment` 实体之间存在一对零或一的关系。 预先加载适用于 `OfficeAssignment` 实体。 需要显示相关数据时，预先加载通常更高效。 在此情况下，会显示讲师的办公室分配。
* 当用户选择一名讲师（上图中的 Harui）时，显示相关的 `Course` 实体。 `Instructor` 和 `Course` 实体之间存在多对多关系。 对 `Course` 实体及其相关的 `Department` 实体使用预先加载。 这种情况下，单独查询可能更有效，因为仅需显示所选讲师的课程。 此示例演示如何在位于导航实体内的实体中预先加载这些导航实体。
* 当用户选择一门课程（上图中的化学）时，显示 `Enrollments` 实体的相关数据。 上图中显示了学生姓名和成绩。 `Course` 和 `Enrollment` 实体之间存在一对多的关系。

### <a name="create-a-view-model-for-the-instructor-index-view"></a>创建“讲师索引”视图的视图模型

“讲师”页显示来自三个不同表格的数据。 创建一个视图模型，该模型中包含表示三个表格的三个实体。

在 SchoolViewModels 文件夹中，使用以下代码创建 InstructorIndexData.cs：

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>为讲师模型创建基架

* 退出 Visual Studio。
* 打开项目目录（包含 Program.cs、Startup.cs 和 .csproj 文件的目录）中的命令窗口。
* 运行下面的命令：

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

上述命令为 `Instructor` 模型创建基架。 在 Visual Studio 中打开项目。

生成项目。 此版本生成错误。

将 `_context.Instructor` 全局更改为 `_context.Instructors`（即向 `Instructor` 添加一个“s”）。 找到并更新 7 个匹配项。

运行应用并导航到“讲师”页。

将 Pages/Instructors/Index.cshtml.cs 替换为以下代码：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

`OnGetAsync` 方法接受所选讲师 ID 的可选路由数据。

检查 Pages/Instructors/Index.cshtml 页上的查询：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

查询包括两项内容：

* `OfficeAssignment`：在[讲师视图](#IP)中显示。
* `CourseAssignments`：课程的教学内容。


### <a name="update-the-instructors-index-page"></a>更新“讲师索引”页

使用以下标记更新 Pages/Instructors/Index.cshtml：

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

上述标记进行以下更改：

* 将 `page` 指令从 `@page` 更新为 `@page "{id:int?}"`。 `"{id:int?}"` 是一个路由模板。 路由模板将 URL 中的整数查询字符串更改为路由数据。 例如，当页面指令生成如下所示的 URL 时，单击讲师的“选择”链接 ：

    `http://localhost:1234/Instructors?id=2`

    当页面指令是 `@page "{id:int?}"` 时，之前的 URL 为：

    `http://localhost:1234/Instructors/2`

* 页标题为“讲师”。
* 添加了仅在 `item.OfficeAssignment` 不为 null 时才显示 `item.OfficeAssignment.Location` 的“办公室”列。 由于这是一对零或一的关系，因此可能没有相关的 OfficeAssignment 实体。

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* 添加了显示每位讲师所授课程的“课程”列。 有关此 Razor 语法的详细信息，请参阅[使用 `@:` 进行显式行转换](xref:mvc/views/razor#explicit-line-transition-with-)。

* 添加了向所选讲师的 `tr` 元素中动态添加 `class="success"` 的代码。 此时会使用 Bootstrap 类为所选行设置背景色。

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* 添加了标记为“选择”的新的超链接。 该链接将所选讲师的 ID 发送给 `Index` 方法并设置背景色。

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

运行应用并选择“讲师”选项卡。该页显示来自相关 `OfficeAssignment` 实体的 `Location`（办公室）。 如果 OfficeAssignment` 为 NULL，则显示空白表格单元格。

![未选择“讲师索引”页中的任何项](read-related-data/_static/instructors-index-no-selection.png)

单击“选择”链接。 随即更改行样式。

### <a name="add-courses-taught-by-selected-instructor"></a>添加由所选讲师教授的课程

将 Pages/Instructors/Index.cshtml.cs 中的 `OnGetAsync` 方法替换为以下代码：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

检查更新后的查询：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

先前查询添加了 `Department` 实体。

选择讲师时 (`id != null`)，将执行以下代码。 从视图模型中的讲师列表检索所选讲师。 向视图模型的 `Courses` 属性加载来自讲师 `CourseAssignments` 导航属性的 `Course` 实体。

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

`Where` 方法返回一个集合。 在前面的 `Where` 方法中，仅返回单个 `Instructor` 实体。 `Single` 方法将集合转换为单个 `Instructor` 实体。 `Instructor` 实体提供对 `CourseAssignments` 属性的访问。 `CourseAssignments` 提供对相关 `Course` 实体的访问。

![讲师-课程 m:M](complex-data-model/_static/courseassignment.png)

当集合仅包含一个项时，集合使用 `Single` 方法。 如果集合为空或包含多个项，`Single` 方法会引发异常。 还可使用 `SingleOrDefault`，该方式在集合为空时返回默认值（本例中为 null）。 在空集合上使用 `SingleOrDefault`：

* 引发异常（因为尝试在空引用上找到 `Courses` 属性）。
* 异常信息不太能清楚指出问题原因。

选中课程时，视图模型的 `Enrollments` 属性将填充以下代码：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

在 Pages/Courses/Index.cshtml Razor 页面末尾添加以下标记：

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

上述标记显示选中某讲师时与该讲师相关的课程列表。

测试应用。 单击讲师页面上的“选择”链接。

![已选择“讲师索引”页中的讲师](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a>显示学生数据

在本部分中，更新应用以显示所选课程的学生数据。

使用以下代码在 Pages/Instructors/Index.cshtml.cs 中更新 `OnGetAsync` 方法中的查询：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

更新 Pages/Instructors/Index.cshtml。 在文件末尾添加以下标记：

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

上述标记显示已注册所选课程的学生列表。

刷新页面并选择讲师。 选择一门课程，查看已注册的学生及其成绩列表。

![已选择“讲师索引”页中的讲师和课程](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>使用 Single 方法

`Single` 方法可在 `Where` 条件中进行传递，无需分别调用 `Where` 方法：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

使用 `Where` 时，前面的 `Single` 方法不适用。 一些开发人员更喜欢 `Single` 方法样式。

## <a name="explicit-loading"></a>显式加载

当前代码为 `Enrollments` 和 `Students` 指定预先加载：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

假设用户几乎不希望课程中显示注册情况。 在此情况下，可仅在请求时加载注册数据进行优化。 在本部分中，会更新 `OnGetAsync` 以使用 `Enrollments` 和 `Students` 的显式加载。

使用以下代码更新 `OnGetAsync`：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

上述代码取消针对注册和学生数据的 ThenInclude 方法调用。 如果已选中课程，则突出显示的代码会检索：

* 所选课程的 `Enrollment` 实体。
* 每个 `Enrollment` 的 `Student` 实体。

请注意，上述代码为 `.AsNoTracking()` 加上注释。 对于跟踪的实体，仅可显式加载导航属性。

测试应用。 对用户而言，该应用的行为与上一版本相同。

下一个教程将介绍如何更新相关数据。

>[!div class="step-by-step"]
>[上一页](xref:data/ef-rp/complex-data-model)
>[下一页](xref:data/ef-rp/update-related-data)
