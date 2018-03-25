---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "使用实体框架 ASP.NET MVC 应用程序 (6 的 10) 中更新相关的数据 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 2ca76364a2e9a71dc92644bd579345ae3c304a69
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>使用实体框架 ASP.NET MVC 应用程序 (6 的 10) 中更新相关的数据
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 你可以从头开始教程系列或[下载这一章的初学者项目](building-the-ef5-mvc4-chapter-downloads.md)和从这里开始。
> 
> > [!NOTE] 
> > 
> > 如果你遇到无法解决的问题[下载已完成的章](building-the-ef5-mvc4-chapter-downloads.md)并尝试重现你的问题。 你通常可以通过比较你的代码已完成的代码会发现问题的解决方案。 一些常见的错误和如何解决它们，请参阅[错误和解决方法。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


在前面的教程中显示相关的数据;在本教程中，你将更新相关的数据。 对于大多数关系，这可以通过更新相应的外键字段。 对于多对多关系，实体框架不联接表直接公开，因此你必须显式添加和删除实体到和从相应的导航属性。

下图显示了您将使用的页。

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>课程中自定义创建和编辑页

创建新的课程实体时，它必须具有与现有部门的关系。 为了便于执行这种情况，基架的代码包括控制器方法以及创建和编辑视图中包括用于选择部门的下拉列表。 下拉列表设置`Course.DepartmentID`外键属性，而这正是实体框架需要以便加载`Department`使用相应的导航属性`Department`实体。 你将使用基架的代码，但将其更改略有为添加错误处理和下拉列表进行排序。

在*CourseController.cs*，删除四个`Edit`和`Create`方法，并用它们替换下面的代码：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

`PopulateDepartmentsDropDownList`方法获取按名称排序的所有部门的列表，创建`SelectList`集合有关的下拉列表，并将集合传递到该视图`ViewBag`属性。 该方法接受可选`selectedDepartment`允许指定呈现下拉列表时将选定的项的调用代码的参数。 该视图将传递名称`DepartmentID`到[`DropDownList`帮助器](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md)，和帮助器就会知道要查找的`ViewBag`对象`SelectList`名为`DepartmentID`。

`HttpGet` `Create`方法调用`PopulateDepartmentsDropDownList`而无需设置选定的项，因为新课程部门不尚未建立的方法：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`HttpGet` `Edit`方法设置选定的项，基于部门已分配给过程中正在编辑的 ID:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpPost`方法同时`Create`和`Edit`还包括当它们在错误之后重新显示页时设置选定的项的代码：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

此代码可确保当重新显示页以显示错误消息，选择任何部门保持所选。

在*Views\Course\Create.cshtml*，添加突出显示的代码，以创建新之前的课程数字字段**标题**字段。 默认情况下，前面的教程中所述，主键字段不基架，但此主键是有意义，因此你希望用户能够输入密钥的值。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

在*Views\Course\Edit.cshtml*， *Views\Course\Delete.cshtml*，和*Views\Course\Details.cshtml*，添加课程数字字段之前**标题**字段。 因为它是主键，它会显示，但不能更改。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

运行**创建**页 (显示课程索引页，然后单击**新建**) 和新的课程中输入数据：

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

单击 **“创建”**。 课程索引页会显示新添加到列表中的过程。 索引页列表中的部门名称来自于导航属性，显示已正确建立关系。

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

运行**编辑**页 (显示课程索引页，然后单击**编辑**课程上)。

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

更改页面上的数据，然后单击**保存**。 课程索引页会显示已更新的课程数据。

## <a name="adding-an-edit-page-for-instructors"></a>添加教师编辑页

编辑教师记录时，你想要能够更新教师的办公室分配。 `Instructor`实体具有对零或一一个关系`OfficeAssignment`实体，这意味着必须处理的以下情况：

- 如果用户清除 office 分配，并且最初具有一个值，你必须删除并删除`OfficeAssignment`实体。
- 如果用户输入 office 分配值，并且它最初为空，则必须创建一个新`OfficeAssignment`实体。
- 如果用户更改一个办公室分配的值，则必须更改中的现有值`OfficeAssignment`实体。

打开*InstructorController.cs*并查看`HttpGet``Edit`方法：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

此处的基架的代码不符合要求。 设置数据的下拉列表中，但您需要的是文本框。 此方法替换为以下代码：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

此代码将删除`ViewBag`语句，并将添加为关联的预先加载`OfficeAssignment`实体。 无法执行与预先加载`Find`方法，因此`Where`和`Single`方法用于改为选择 instructor。

替换`HttpPost``Edit`方法替换为以下代码。 可处理 office 分配更新：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

该代码执行以下任务：

- 获取当前`Instructor`中使用的预先加载的数据库实体`OfficeAssignment`导航属性。 这是你未与相同`HttpGet``Edit`方法。
- 更新检索`Instructor`模型联编程序中的值的实体。 [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx)使用重载使你能够*白名单*你想要包括的属性。 这可以防止过度发布中所述[第二个教程](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)。

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- 如果 office 位置为空，设置`Instructor.OfficeAssignment`为 null 的属性，以便中的相关的行`OfficeAssignment`表将被删除。

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- 将所做的更改保存到数据库。

在*Views\Instructor\Edit.cshtml*后面`div`元素**雇用日期**字段中，添加以进行编辑的办公地点的新字段：

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

运行页面 (选择**教师**选项卡，然后单击**编辑**教师上)。 更改**办公地点**单击**保存**。

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>添加课程分配到教师编辑页

教师可能讲解任意数量的课程。 现在你将通过添加更改过程分配使用一组复选框，如下面的屏幕快照中所示的功能增强教师编辑页：

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

之间的关系`Course`和`Instructor`实体是多对多，这意味着你没有直接访问联接表。 相反，你将在其中添加和删除实体，并从`Instructor.Courses`导航属性。

使你能够更改哪些课程 UI 教师是分配给是一组的复选框。 显示数据库中每个课程复选框，并选择 instructor 当前分配给那些。 用户可以选择或清除复选框来更改过程分配。 如果课程数大得多，你将可能想要使用不同的方法来在视图中，显示数据的但你将使用相同的方法的操作才能创建或删除关系的导航属性。

若要提供的复选框列表视图的数据，你将使用视图模型类。 创建*AssignedCourseData.cs*中*Viewmodel*文件夹和替换现有代码替换为以下代码：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

在*InstructorController.cs*，替换`HttpGet``Edit`方法替换为以下代码。 突出显示所做的更改。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

该代码将添加为预先加载`Courses`导航属性并调用新`PopulateAssignedCourseData`方法以提供有关复选框的数组使用信息`AssignedCourseData`查看模型类。

中的代码`PopulateAssignedCourseData`方法读取通过所有`Course`才能加载课程使用视图的列表的实体模型类。 对于每个课程中，代码检查过程中是否存在于教师`Courses`导航属性。 若要检查是否将某一课程分配给教师时，请创建高效的查找，分配给教师的课程已放入[HashSet](https://msdn.microsoft.com/library/bb359438.aspx)集合。 `Assigned`属性设置为`true`教师分配课程。 该视图将使用此属性来确定哪些复选框必须显示为选择。 最后，该列表传递给中的视图`ViewBag`属性。

接下来，添加在用户单击时执行的代码与**保存**。 替换`HttpPost``Edit`方法替换为以下代码，调用更新的新方法`Courses`导航属性`Instructor`实体。 突出显示所做的更改。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

由于该视图不包含一套`Course`实体，不能自动更新模型联编程序`Courses`导航属性。 而不是使用模型联编程序更新的课程导航属性，则将执行该操作在新`UpdateInstructorCourses`方法。 因此你需要以排除`Courses`模型绑定中的属性。 这不需要对调用代码的任何更改[TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx)因为您正在使用*白名单*重载和`Courses`不在包括列表中。

如果没有复选框已选中中的代码`UpdateInstructorCourses`初始化`Courses`对空集合的导航属性：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

该代码，然后循环访问数据库中的所有课程，并检查对当前分配给与在视图中选择的教师的每个课程。 为了便于高效的查找，后者的两个集合都存储在`HashSet`对象。

如果选择某一课程复选框，但过程不在`Instructor.Courses`导航属性，过程添加到集合中的导航属性。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

如果未选择某一课程复选框，但过程处于`Instructor.Courses`导航属性，过程删除从导航属性。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

在*Views\Instructor\Edit.cshtml*，添加**课程**字段通过添加以下突出显示的复选框的数组的代码后立即`div`元素`OfficeAssignment`字段：

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

此代码将创建一个 HTML 表，包含三列。 处于每个列组成课程编号及标题的标题后跟一个复选框。 所有对应的复选框具有相同的名称 ("selectedCourses")，通知它们将被视为一组模型联编程序。 `value`的每个复选框的属性设置的值为`CourseID.`发页面，当模型联编程序会将数组传递给组成的控制器`CourseID`仅复选框进行选择的值。

当最初呈现复选框时，为分配给教师的课程的那些具有`checked`属性，后者选择它们 （它们检查的显示）。

更改课程作业之后, 你将需要能够验证所做的更改，当站点恢复时`Index`页。 因此，你需要将列添加到该页面中的表。 在这种情况下不需要使用`ViewBag`对象，因为已经在你想要显示的信息`Courses`导航属性`Instructor`您传递到页其作为模型的实体。

在*Views\Instructor\Index.cshtml*，添加**课程**标题紧跟**Office**标题下，如下面的示例中所示：

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

然后添加新的详细信息单元紧跟 office 位置详细信息单元：

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

运行**教师索引**页后，可以查看分配给每个教师课程：

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

单击**编辑**教师若要查看编辑页上。

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

更改某些课程作业，然后单击**保存**。 所做的更改会反映在索引页面。

 注意： 编辑教师课程数据而采取的做法适用于没有有限的数量的课程。 对于则大得多的集合，会需要不同的 UI 以及不同的更新方法。  
 

## <a name="update-the-delete-method"></a>更新 Delete 方法

更改 HttpPost 删除方法中的代码，以便在 office 分配记录 （如果有） 被删除时删除教师：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


如果你尝试删除分配给以管理员身份的部门的教师，你将获得一个引用完整性错误。 请参阅[本教程的当前版本](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)将自动删除任何部门其中教师分配以管理员身份从教师的其他代码。

## <a name="summary"></a>摘要

你现在已经完成此简介使用相关数据。 到目前为止这些教程中所做的一项完整的 CRUD 的操作，但尚未处理并发问题。 下一教程将介绍并发的主题，说明用于处理它，选项并添加到您已经编写了一个实体类型的 CRUD 代码处理的并发。

可以在的末尾找到的其他实体框架资源的链接[本系列最后一个教程](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)。

>[!div class="step-by-step"]
[上一页](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[下一页](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
