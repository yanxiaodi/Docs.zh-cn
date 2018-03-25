---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "读取与实体框架中的 ASP.NET MVC 应用程序 (5 的 10) 的相关的数据 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9093fb90a52b297f173c5cddb6f332d2d1a25135
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>读取与相关的数据与实体框架中的 ASP.NET MVC 应用程序 (5 的 10)
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 你可以从头开始教程系列或[下载这一章的初学者项目](building-the-ef5-mvc4-chapter-downloads.md)和从这里开始。
> 
> > [!NOTE] 
> > 
> > 如果你遇到无法解决的问题[下载已完成的章](building-the-ef5-mvc4-chapter-downloads.md)并尝试重现你的问题。 你通常可以通过比较你的代码已完成的代码会发现问题的解决方案。 一些常见的错误和如何解决它们，请参阅[错误和解决方法。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


在前面的教程中，你将完成 School 数据模型。 在本教程中，你将读取并显示相关的数据-即，实体框架将加载到导航属性的数据。

下图显示了您将使用的页。

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>延迟、 Eager，和显式加载相关的数据

有多种实体框架可以加载到实体的导航属性的相关的数据：

- *延迟加载*。 当第一次读取实体时，不检索相关的数据。 但是，第一次尝试访问的导航属性，将自动检索该导航属性所需的数据。 这会导致发送到数据库的多个查询 — 一个用于在实体自身，一个必须检索每个相关实体的数据的时间。 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *预先加载*。 实体中读取时，会随之检索相关的数据。 这通常会导致检索所有具有所需的数据的单一联接查询。 使用指定预先加载`Include`方法。

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *显式加载*。 这是类似于延迟加载的只不过显式检索代码; 中的相关的数据它不会发生自动访问导航属性时。 你相关手动加载数据通过有关实体，以及调用中获取的对象状态管理器项`Collection.Load`集合的方法或`Reference.Load`保存单个实体的属性的方法。 (在下面的示例中，如果你想要加载管理员导航属性，则将替换`Collection(x => x.Courses)`与`Reference(x => x.Administrator)`。)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

因为它们不立即检索的属性值，延迟加载和显式加载也同时称为*延迟加载*。

一般情况下，如果你知道需要相关的数据检索，预先加载提供最佳性能，每个实体因为单个查询发送到数据库通常比用于检索每个实体的单独查询效率更高。 例如，在上面的示例中，假设每个部门都有十个相关的课程。 预先加载示例将导致仅单 （联接） 查询和单个往返到数据库。 延迟加载和显式加载示例将都导致十一种查询和十一个往返行程到数据库。 与数据库额外的往返延迟较高时尤其是对性能造成不利影响。

另一方面，在某些情况下延迟加载会更加高效。 预先加载可能会导致非常复杂的联接为其生成 SQL Server 无法有效地处理它。 或如果你需要访问仅针对实体集的子集的实体的导航属性要处理，延迟加载可能更好地执行，因为预先加载将检索比你需要更多的数据。 如果性能非常重要，则最好测试以便做出最佳选择这两种方式的性能。

通常，你将使用显式加载，仅当你已启用延迟加载关闭时。 你应打开延迟加载关闭时的一个方案是在序列化过程。 延迟加载和序列化，不要混合并且如果你不小心处理可以得到查询更多的数据不按预期时延迟加载已启用。 序列化通常可通过访问类型的实例上每个属性。 属性访问触发延迟加载，并为这些延迟加载的实体的序列化。 然后，序列化过程访问的延迟加载的实体，可能会导致更多延迟加载和序列化的每个属性。 若要防止此逐种链式反应，打开延迟加载关闭之前序列化实体。

数据库上下文类默认情况下执行延迟加载。 有两种方法，若要禁用延迟加载：

- 对于特定的导航属性，省略`virtual`关键字声明的属性时。
- 对于所有导航属性，设置`LazyLoadingEnabled`到`false`。 例如，你可以将下面的代码置于上下文类的构造函数： 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

延迟加载可以屏蔽导致性能问题的代码。 例如，未指定 eager 或显式加载但处理大量的实体并在每次迭代中使用多个导航属性的代码可能非常低效，（由于到数据库的许多往返过程）。 执行在开发使用在本地 SQL server 中良好运行的应用程序可能具有性能问题时由于的延迟时间增加和延迟加载移到 Azure SQL 数据库。 分析的实际的测试加载的数据库的查询将帮助你确定延迟加载是否适用。 有关详细信息请参阅[Demystifying 实体框架策略： 加载相关数据](https://msdn.microsoft.com/magazine/hh205756.aspx)和[使用实体框架与 SQL Azure 到减少网络延迟](https://msdn.microsoft.com/magazine/gg309181.aspx)。

## <a name="create-a-courses-index-page-that-displays-department-name"></a>创建该显示部门名称的课程索引页

`Course`实体包括一个导航属性，包含`Department`过程分配给的部门实体。 若要显示分配的部门的名称列表中的课程，你需要获取`Name`属性从`Department`实体`Course.Department`导航属性。

创建名为的控制器`CourseController`为`Course`实体类型，使用相同的选项，你此前对`Student`控制器，如下面的插图中所示 （上下文类 DAL 的命名空间与图中，不同的是除外不是模型命名空间）：

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

打开*Controllers\CourseController.cs*并查看`Index`方法：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

自动的基架已指定为预先加载`Department`导航属性使用`Include`方法。

打开*Views\Course\Index.cshtml*和替换为以下代码替换现有代码。 突出显示所做的更改：

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

你已对基架代码进行以下更改：

- 更改标题从**索引**到**课程**。
- 向左移动行链接。
- 添加的列标题下**数**显示`CourseID`属性值。 （默认情况下，主键不基架因为它们通常是向最终用户无意义。 但是，在这种情况下的主键有意义，以及你想要对其进行说明。）
- 更改从最后一个列标题**DepartmentID** (外键与名称`Department`实体) 到**部门**。

请注意，最后一列，则基架的代码显示`Name`属性`Department`实体加载到`Department`导航属性：

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

运行页面 (选择**课程**Contoso 大学主页上的选项卡) 若要查看使用部门名称列表。

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>创建显示 Courses，并将注册一个教师索引页面

在本节中你将创建一个控制器，以及查看`Instructor`为了显示教师索引页的实体：

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

此页读取，并通过以下方式显示相关的数据：

- 教师的列表显示来自的相关的数据`OfficeAssignment`实体。 `Instructor`和`OfficeAssignment`实体位于对零或一一个关系。 你将使用的预先加载`OfficeAssignment`实体。 如前面所述，预先加载是通常更高效，您需要为所有检索行的主表的相关的数据。 在这种情况下，你想要显示的所有显示教师 office 分配。
- 当用户选择一个教师，相关`Course`显示实体。 `Instructor`和`Course`实体位于多对多关系。 你将使用的预先加载`Course`实体和其相关`Department`实体。 在这种情况下，因为你只为所选教师需要课程，延迟加载可能会更有效。 但是，此示例演示如何使用预先加载用于本身是导航属性中的实体内的导航属性。
- 当用户选择某一课程时，与相关的数据从`Enrollments`显示实体集。 `Course`和`Enrollment`实体位于一个对多关系。 你将添加显式加载`Enrollment`实体和其相关`Student`实体。 （显式加载则无需因为启用了延迟加载，但下面的示例演示如何显式加载。）

### <a name="create-a-view-model-for-the-instructor-index-view"></a>为教师索引视图创建视图模型

教师索引页显示三个不同的表。 因此，你将创建包括三个属性，每个保存的表中的一个数据视图模型。

在*Viewmodel*文件夹中，创建*InstructorIndexData.cs*和替换为以下代码替换现有代码：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>为选定的行添加样式

若要将选定的行的标记，你需要不同的背景色。 若要为此 UI 提供样式，请将以下突出显示的代码添加到部分`/* info and errors */`中*Content\Site.css*，如下所示：

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>正在创建教师控制器和视图

创建`InstructorController`控制器下图中所示：

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

打开*Controllers\InstructorController.cs*并添加`using`语句`ViewModels`命名空间：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

中的基架的代码`Index`方法指定仅对预先加载`OfficeAssignment`导航属性：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

替换`Index`方法替换为以下代码以加载其他相关数据，并将其置于视图模型：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

该方法将接受可选路由数据 (`id`) 和查询字符串参数 (`courseID`)，提供所选的教师和所选的过程的 ID 值并将所有所需的数据传递到该视图。 参数提供的**选择**网页超链接。

> [!TIP] 
> 
> **路线数据**
> 
> 路由数据是在路由表中指定的 URL 段中找到模型联编程序的数据。 例如，默认路由指定`controller`， `action`，和`id`段：
> 
> routes.MapRoute(  
>  名称:"默认情况下，"  
>  url:"{controller} / {action} / {id}"，  
>  默认值： new {控制器 ="主页"，操作 ="Index"，id = UrlParameter.Optional}  
> );
> 
> 在下面的 URL，将默认路由映射`Instructor`作为`controller`，`Index`作为`action`并将它作为 1 `id`; 这些是路由数据值。
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> "？ courseID = 2021年"是一个查询字符串值。 模型联编程序还将起作用，如果你通过`id`作为查询字符串值：
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> 通过创建 Url `ActionLink` Razor 视图中的语句。 在下面的代码中，`id`参数和默认路由匹配，因此`id`将添加到路由数据。
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> 在下面的代码中，`courseID`不匹配默认路由中的参数，因此，它将添加作为查询字符串。
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


代码首先通过在创建视图模型的实例并在其中放置教师的列表。 该代码指定为预先加载`Instructor.OfficeAssignment`和`Instructor.Courses`导航属性。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

第二个`Include`方法加载的课程，并为每个课程加载完成了预先加载`Course.Department`导航属性。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

如前所述，预先加载不是必需的但做是为了提高性能。 因为视图始终需要`OfficeAssignment`实体，它会在同一查询中提取的更加高效。 `Course`中网页上，选择一个教师，以便预先加载优于延迟加载，仅当与比而无需选择某一课程更通常显示的页面时，实体所需。

如果选择 instructor ID，则会将所选的教师检索从教师视图模型中的列表。 视图模型`Courses`属性然后加载`Course`从该 instructor 实体`Courses`导航属性。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

`Where`方法将返回一个集合，但条件在此情况下传递到方法导致只有一个`Instructor`所返回的实体。 `Single`方法将集合转换为单个`Instructor`实体，使你可以访问该实体的`Courses`属性。

你使用[单个](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx)时你知道该集合的集合上的方法将有只有一项。 `Single`方法引发异常时传递给它的集合是否为空或没有多个项。 替代方法是[SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx)，它返回默认值 (`null`在这种情况下) 如果该集合为空。 但是，在这种情况下，仍会生成异常 (从尝试查找`Courses`属性`null`引用)，和异常消息将不太清楚地指示问题的原因。 当调用`Single`方法，你还可以通过`Where`条件而不是调用`Where`方法单独：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

而不是：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

接下来，如果选择某一课程，从列表中的课程，视图模型中检索所选的课程。 然后视图模型的`Enrollments`属性加载与`Enrollment`从该过程的实体`Enrollments`导航属性。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>修改教师索引视图

在*Views\Instructor\Index.cshtml*，替换为以下代码替换现有代码。 突出显示所做的更改：

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

你已对现有代码做出以下更改：

- 更改到模型类`InstructorIndexData`。
- 更改页标题从**索引**到**教师**。
- 向左移动行列链接。
- 删除**FullName**列。
- 添加**Office**显示的列`item.OfficeAssignment.Location`才`item.OfficeAssignment`不为 null。 (由于这是一个对零或一一个关系，因此可能不具备相关`OfficeAssignment`实体。) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- 添加将动态添加的代码`class="selectedrow"`到`tr`的所选教师的元素。 此设置使用 CSS 类之前创建所选行的背景色。 (`valign`属性将在以下教程中有用，当你将多行的列添加到表。) 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- 添加一个新`ActionLink`标记为**选择**立即之前每个行中的其他链接，这将导致所选的教师 ID 发送到`Index`方法。

运行应用程序并选择**教师**选项卡。该页面将显示`Location`的相关属性`OfficeAssignment`实体和一个空表单元时没有相关的否`OfficeAssignment`实体。

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

在*Views\Instructor\Index.cshtml*结束后的文件，`table`元素 （在文件末尾），添加以下突出显示的代码。 此时将显示一个教师选中与一个教师相关的课程的列表。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

此代码读取`Courses`要显示的课程列表的视图模型的属性。 它还提供了`Select`超链接，以便将发送到所选课程 ID`Index`操作方法。

> [!NOTE]
> *.Css*由浏览器缓存文件。 如果你运行应用程序时，你看不到所做的更改，请执行硬刷新 (请按住 CTRL 键同时单击**刷新**按钮，或按 CTRL + F5)。


运行页面，然后选择一个教师。 此时你会看到一个网格，其中显示分配给所选教师的课程，对于每个课程中，你将看到分配部门的名称。

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

之后你刚添加的代码块，添加下面的代码。 当选中该过程时，此值显示课程中注册的学生的列表。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

此代码读取`Enrollments`为了显示学生的列表视图模型的属性在本课程中注册。

运行页面，然后选择一个教师。 然后选择要查看的已注册的学生和其年级列表的课程。

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>添加显式加载

打开*InstructorController.cs*并查看如何`Index`方法获取有关所选过程的注册的列表：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

时检索教师的列表，您已指定为预先加载`Courses`导航属性和`Department`当然，每个属性。 则你将放`Courses`集合在视图模型中，并且现在正在访问`Enrollments`从该集合中的一个实体导航属性。 因为你未指定为预先加载`Course.Enrollments`导航属性，该属性中的数据显示在由于延迟加载的页中。

如果不更改任何其他方式中的代码的情况下禁用延迟加载`Enrollments`属性将为 null 而不考虑过程实际上有多少注册。 在此情况下，加载`Enrollments`属性，你将需要指定预先加载或显式加载。 你已经了解如何执行预先加载。 若要查看的显式加载示例，替换`Index`方法替换为以下代码，此操作将显式加载`Enrollments`属性。 更改的代码将突出显示。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

获取所选后`Course`实体，新的代码显式加载该课程`Enrollments`导航属性：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

然后它显式加载每个`Enrollment`实体的相关`Student`实体：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

请注意，你使用`Collection`方法以加载集合属性，但对于一个属性，保存一个实体，你使用`Reference`方法。 你可以立即运行教师索引页和虽然已经更改了如何检索的数据，你会看到在页上，显示的显示没有区别。

## <a name="summary"></a>摘要

你现在使用所有三种方法 （延迟、 eager，和显式） 相关的数据加载到导航属性。 在下一教程你将学习如何更新相关的数据。

在找不到其他实体框架资源的链接[ASP.NET 数据访问内容映射](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一页](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
[下一页](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
