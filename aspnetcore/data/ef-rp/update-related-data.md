---
title: "Razor 页面和 EF Core - 更新相关数据 - 第 7 个教程，共 8 个教程"
author: rick-anderson
description: "本教程将通过更新外键字段和导航属性来更新相关数据。"
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 5c91c91ab938f3aa4abc55049c54f399469f6163
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2018
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a>更新相关数据 - EF Core Razor 页面（第 7 个教程，共 8 个）

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

本教程演示如何更新相关数据。 如果遇到无法解决的问题，请下载[本阶段的已完成应用](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7)。

下图显示了部分已完成页面。

![课程“编辑”页](update-related-data/_static/course-edit.png)
![讲师”编辑”页](update-related-data/_static/instructor-edit-courses.png)

查看并测试“创建”和“编辑”课程页。 创建新课程。 系按其主键（一个整数）进行选择，而不是按名称。 编辑新课程。 完成测试后，请删除新课程。

## <a name="create-a-base-class-to-share-common-code"></a>创建基类以共享通用代码

“课程/创建”和“课程/编辑”页分别需要一个系名称列表。 针对“创建”和“编辑”页创建 Pages/Courses/DepartmentNamePageModel.cshtml.cs 基类：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

上面的代码创建 [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) 以包含系名称列表。 如果指定了 `selectedDepartment`，可在 `SelectList` 中选择该系。

“创建”和“编辑”页模型类将派生自 `DepartmentNamePageModel`。

## <a name="customize-the-courses-pages"></a>自定义课程页

创建新的课程实体时，新实体必须与现有院系有关系。 若要在创建课程的同时添加系，则“创建”和“编辑”的基类必须包含用于选择系的下拉列表。 该下拉列表设置 `Course.DepartmentID` 外键 (FK) 属性。 EF Core 使用 `Course.DepartmentID` FK 加载 `Department` 导航属性。

![创建课程](update-related-data/_static/ddl.png)

使用以下代码更新“创建”页模型：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

前面的代码：

* 派生自 `DepartmentNamePageModel`。
* 使用 `TryUpdateModelAsync` 防止[过多发布](xref:data/ef-rp/crud#overposting)。
* 将 `ViewData["DepartmentID"]` 替换为 `DepartmentNameSL`（来自基类）。

`ViewData["DepartmentID"]` 将替换为强类型 `DepartmentNameSL`。 建议使用强类型而非弱类型。 有关详细信息，请参阅[弱类型数据（ViewData 和 ViewBag）](xref:mvc/views/overview#VD_VB)。

### <a name="update-the-courses-create-page"></a>更新“课程创建”页

使用以下标记更新 Pages/Courses/Create.cshtml：

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

上述标记进行以下更改：

* 将标题从“DepartmentID”更改为“Department”。
* 将 `"ViewBag.DepartmentID"` 替换为 `DepartmentNameSL`（来自基类）。
* 添加“选择系”选项。 此更改将导致呈现“选择系”而不是第一个系。
* 在未选择系时添加验证消息。

Razor 页面使用[选择标记帮助器](xref:mvc/views/working-with-forms#the-select-tag-helper)：

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

测试“创建”页。 “创建”页显示系名称，而不是系 ID。

### <a name="update-the-courses-edit-page"></a>更新“课程编辑”页。

使用以下代码更新“编辑”页模型：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

这些更改与在“创建”页模型中所做的更改相似。 在上面的代码中，`PopulateDepartmentsDropDownList` 在系 ID 中传递并将选择下拉列表中指定的系。

使用以下标记更新 Pages/Courses/Edit.cshtml：

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

上述标记进行以下更改：

* 显示课程 ID。 通常不显示实体的主键 (PK)。 PK 对用户不具有任何意义。 在这种情况下，PK 就是课程编号。
* 将标题从“DepartmentID”更改为“Department”。
* 将 `"ViewBag.DepartmentID"` 替换为 `DepartmentNameSL`（来自基类）。
* 添加“选择系”选项。 此更改将导致呈现“选择系”而不是第一个系。
* 在未选择系时添加验证消息。

该页面包含课程编号的隐藏域 (`<input type="hidden">`)。 添加具有 `asp-for="Course.CourseID"` 的 `<label>` 标记帮助器也同样需要隐藏域。 用户单击“保存”时，需要 `<input type="hidden">`，以便在已发布的数据中包括课程编号。

测试更新的代码。 创建、编辑和删除课程。

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>将 AsNoTracking 添加到“详细信息”和“删除”页模型

[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) 可以在不需要跟踪时提高性能。 将 `AsNoTracking` 添加到“删除”和“详细信息”页模型。 下面的代码显示更新的“删除”页模型：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

在 Pages/Courses/Details.cshtml.cs 文件中更新 `OnGetAsync` 方法：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>修改“删除”和“详细信息”页

使用以下标记更新“删除”Razor 页面：

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

对“详细信息”页执行相同更改。

### <a name="test-the-course-pages"></a>测试“课程”页

测试“创建”、“编辑”、“详细信息”和“删除”。

## <a name="update-the-instructor-pages"></a>更新“讲师”页

以下各部分介绍更新“讲师”页。

### <a name="add-office-location"></a>添加办公室位置

编辑讲师记录时，可能希望更新讲师的办公室分配。 `Instructor` 实体与 `OfficeAssignment` 实体是一对零或一关系。 讲师代码必须处理：

* 如果用户清除办公室分配，则需删除 `OfficeAssignment` 实体。
* 如果用户输入办公室分配，且办公室分配原本为空，则需创建一个新 `OfficeAssignment` 实体。
* 如果用户更改办公室分配，则需更新 `OfficeAssignment` 实体。

使用以下代码更新讲师“编辑”页模型：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

前面的代码：

- 使用 `OfficeAssignment` 导航属性的预先加载从数据库获取当前的 `Instructor` 实体。
- 用模型绑定器中的值更新检索到的 `Instructor` 实体。 `TryUpdateModel` 可防止[过多发布](xref:data/ef-rp/crud#overposting)。
- 如果办公室位置为空，则将 `Instructor.OfficeAssignment` 设置为 null。 当 `Instructor.OfficeAssignment` 为 null 时，`OfficeAssignment` 表中的相关行将会删除。

### <a name="update-the-instructor-edit-page"></a>更新讲师“编辑”页

使用办公室位置更新 Pages/Instructors/Edit.cshtml：

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

验证是否可以更改讲师办公室位置。

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>向“讲师编辑”页添加课程分配

讲师可能教授任意数量的课程。 本部分将添加更改课程分配的功能。 下图显示更新的讲师“编辑”页：

![带课程信息的讲师“编辑”页](update-related-data/_static/instructor-edit-courses.png)

`Course` 和 `Instructor` 具有多对多关系。 若要添加和删除关系，可以从 `CourseAssignments` 联接实体集中添加和删除实体。

通过复选框可对分配给讲师的课程进行更改。 数据库中的每一门课程均有对应显示的复选框。 已分配给讲师的课程将会被勾选。 用户可以通过选择或清除复选框来更改课程分配。 如果课程数量过多：

* 可以使用其他用户界面显示课程。
* 使用联接实体创建或删除关系的方法不会发生更改。

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>添加类以支持“创建”和“编辑”讲师页

使用以下代码创建 SchoolViewModels/AssignedCourseData.cs：

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

`AssignedCourseData` 类包含的数据可用于为讲师已分配的课程创建复选框。

创建 Pages/Instructors/InstructorCoursesPageModel.cshtml.cs 基类：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

`InstructorCoursesPageModel` 是将用于“编辑”和“创建”页模型的基类。 `PopulateAssignedCourseData` 读取所有 `Course` 实体以填充 `AssignedCourseDataList`。 该代码将设置每门课程的 `CourseID` 和标题，并决定是否为讲师分配该课程。 [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) 用于创建高效查找。

### <a name="instructors-edit-page-model"></a>讲师“编辑”页模型

使用以下代码更新讲师“编辑”页模型：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

上面的代码处理办公室分配更改。

更新“讲师”Razor 视图：

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> 将代码粘贴到 Visual Studio 中时，换行符会发生更改并导致代码中断。 按 Ctrl+Z 一次可撤消自动格式设置。 按 Ctrl+Z 可以修复换行符，使其看起来如此处所示。 缩进不一定要完美，但 `@</tr><tr>`、`@:<td>`、`@:</td>` 和 `@:</tr>` 行必须各成一行（如下所示）。 选中新的代码块后，按 Tab 三次，使新代码与现有代码对齐。 [通过此链接](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html)投票或查看此 bug 的状态。

上面的代码将创建一个具有三列的 HTML 表。 每列均具有一个复选框和包含课程编号及标题的标题。 所有复选框均具有相同的名称（“selectedCourses”）。 使用相同名称可指示模型绑定器将它们视为一个组。 每个复选框的值特性都设置为 `CourseID`。 发布页面时，模型绑定器会传递一个数组，该数组只包括所选复选框的 `CourseID` 值。

初次呈现复选框时，分配给讲师的课程具有已勾选的特性。

运行应用并测试更新的讲师“编辑”页。 更改某些课程分配。 这些更改将反映在“索引”页上。

注意：此处所使用的编辑讲师课程数据的方法适用于数量有限的课程。 对于规模远大于此的集合，则使用不同的 UI 和不同的更新方法会更实用和更高效。

### <a name="update-the-instructors-create-page"></a>更新讲师“创建”页

使用以下代码更新讲师“创建”页模型：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

前面的代码与 Pages/Instructors/Edit.cshtml.cs 代码类似。

使用以下标记更新讲师“创建”Razor 页面：

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

测试讲师“创建”页。

## <a name="update-the-delete-page"></a>更新“删除”页

使用以下代码更新“删除”页模型：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

上面的代码执行以下更改：

* 对 `CourseAssignments` 导航属性使用预先加载。 必须包含 `CourseAssignments`，否则删除讲师时将不会删除课程。 为避免阅读它们，可以在数据库中配置级联删除。

* 如果要删除的讲师被指派为任何系的管理员，则需从这些系中删除该讲师分配。

>[!div class="step-by-step"]
[上一页](xref:data/ef-rp/read-related-data)
[下一页](xref:data/ef-rp/concurrency)
