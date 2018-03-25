---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: "如何开始使用实体框架 4.0 数据库和 ASP.NET 4 Web 窗体的第 4 部分 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建 ASP.NET Web 窗体应用程序使用实体框架。 该示例应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 06d129384fc78db21ad1b9224781deab6a0e91a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>如何开始使用实体框架 4.0 数据库和 ASP.NET 4 Web 窗体的第 4 部分
====================
通过[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web 窗体应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>使用相关数据

在以前的教程，你使用了`EntityDataSource`控件添加到筛选器、 排序和分组数据。 在本教程中将显示和更新相关的数据。

你将创建用于显示教师的列表的教师页。 当你选择一个教师时，你会看到通过该教师讲授的课程的列表。 当你选择某一课程时，你将看到过程和学生课程中注册的列表详细的信息。 你可以编辑教师名称、 雇用日期和办公室分配。 Office 分配是通过导航属性访问的单独的实体集。

你可以链接到详细信息数据在标记或代码中的主数据。 在本教程的此部分中，你将使用这两种方法。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>显示和更新 GridView 控件中的相关的实体

创建一个名为的新 web 页*Instructors.aspx*使用*Site.Master*母版页，并添加以下标记`Content`控件名为`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

此标记将创建`EntityDataSource`控件，以选择教师和启用更新。 `div`元素会配置，以便您可以稍后在右侧添加列呈现在左侧的标记。

之间`EntityDataSource`标记和结束`</div>`标记中，添加以下标记，创建`GridView`控件和`Label`控件，你将使用的错误消息：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

这`GridView`控件启用行选择、 突出显示所选的行与为浅灰色背景颜色，并指定处理程序 （稍后要创建的） 为`SelectedIndexChanged`和`Updating`事件。 它还指定`PersonID`为`DataKeyNames`属性，以便可以将所选行的密钥的值传递到稍后将添加的另一个控件。

最后一列包含教师 office 分配，此操作中的导航属性存储`Person`实体因为它是来自关联的实体。 请注意，`EditItemTemplate`元素指定`Eval`而不是`Bind`，这是因为`GridView`控件不能直接绑定到导航属性以更新它们。 你将更新代码中的 office 分配。 若要做到这一点，你需要对引用`TextBox`控制，以及你将获取并保存，在`TextBox`控件的`Init`事件。

以下`GridView`控件是`Label`错误消息将要使用的控件。 控件的`Visible`属性是`false`，和视图状态已关闭，以便仅当代码使其可见响应错误中显示标签。

打开*Instructors.aspx.cs*文件并添加以下`using`语句：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

分部类名称声明，以保存对 office 分配文本框中的引用后立即添加私有类字段。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

添加存根出于`SelectedIndexChanged`你将为更高版本中添加代码的事件处理程序。 此外将添加的处理程序办公室分配`TextBox`控件的`Init`事件，以便你可以存储的引用`TextBox`控件。 你将使用此引用来获取用户输入以更新与导航属性关联的实体的值。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

你将使用`GridView`控件的`Updating`事件以更新`Location`属性关联的`OfficeAssignment`实体。 添加以下处理程序`Updating`事件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

当用户单击运行此代码**更新**中`GridView`行。 该代码使用 LINQ to Entities 检索`OfficeAssignment`当前与关联的实体`Person`实体，使用`PersonID`事件自变量从所选行。

代码然后将以下操作之一中的值根据`InstructorOfficeTextBox`控件：

- 如果文本框中有一个值，并且没有任何`OfficeAssignment`实体来更新，它将创建一个。
- 如果文本框中有一个值，并且没有`OfficeAssignment`更新实体，`Location`属性值。
- 如果文本框为空和`OfficeAssignment`实体存在，它会删除该实体。

此后，它将保存所做的更改到数据库。 如果发生异常，则会显示一条错误消息。

运行页面。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

单击**编辑**和所有字段都更改到文本框中。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

更改任何这些值，包括**Office 分配**。 单击**更新**，你将看到反映在列表中的更改。

## <a name="displaying-related-entities-in-a-separate-control"></a>在单独控件中显示相关的实体

每个教师可以教授一个或多个课程，因此你将添加`EntityDataSource`控件和`GridView`控件以列出与在教师中选择任何教师关联的课程`GridView`控件。 若要创建一个标题和`EntityDataSource`控件课程实体中，添加以下标记之间的错误消息`Label`控制和结束`</div>`标记：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

`Where`参数包含的值`PersonID`中选择其行教师的`InstructorsGridView`控件。 `Where`属性包含一个可获取所有关联的嵌套 select 语句命令`Person`从实体`Course`实体的`People`导航属性并选择`Course`实体才关联之一`Person`实体包含所选`PersonID`值。

若要创建`GridView`控制。 添加以下标记紧跟`CoursesEntityDataSource`控件 (在关闭前`</div>`标记):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

因为如果选择没有教师，则将显示没有课程`EmptyDataTemplate`才包含元素。

运行页面。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

选择一个教师有一个或多个课程分配，并出现在列表中的过程或课程。 (注意： 尽管数据库架构允许多个课程，但在提供与数据库的测试数据没有教师实际上具有多个过程。 你可以课程到数据库为自己添加使用**服务器资源管理器**窗口或*CoursesAdd.aspx*页上，你将在后面的教程中添加。)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

`CoursesGridView`控件将显示几个课程字段。 若要显示为课程的所有详细信息，你将使用`DetailsView`过程用户选择的控件。 在*Instructors.aspx*，在结束后添加以下标记`</div>`标记 (请确保将此标记**后**结束 div 标记，不在此之前它):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

此标记将创建`EntityDataSource`绑定到控件`Courses`实体集。 `Where`属性选择课程使用`CourseID`课程中所选行的值`GridView`控件。 此标记指定的处理程序`Selected`事件，你将使用用于显示学生成绩的更高版本，这是另一个层次结构中较低的级别。

在*Instructors.aspx.cs*，以下为创建的存根`CourseDetailsEntityDataSource_Selected`方法。 （你将填写此存根 （stub） 在教程后面部分; 现在，你需要它，以便将编译和运行页面。）

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

运行页面。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

最初没有过程的详细信息由于不选择任何过程。 选择一个教师有分配，课程，然后选择要查看的详细信息的课程。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>使用 EntityDataSource"选择"显示相关的数据的事件

最后，你想要显示的所有已注册的学生和所选课程其年级。 若要执行此操作，将使用`Selected`事件`EntityDataSource`控件绑定到过程`DetailsView`。

在*Instructors.aspx*，添加以下标记后的`DetailsView`控件：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

此标记将创建`ListView`显示所选课程其年级学生的列表的控件。 因为你将数据绑定控件在代码中，未不指定任何数据源。 `EmptyDataTemplate`元素提供不选择任何课程时显示一条消息 — 在这种情况下，没有显示任何学生。 `LayoutTemplate`元素创建一个 HTML 表以显示列表中，与`ItemTemplate`指定要显示的列。 学生 ID 和学生成绩是从`StudentGrade`实体，并且学生名称是从`Person`实体框架使中可用的实体`Person`导航属性`StudentGrade`实体。

在*Instructors.aspx.cs*，替换存根处理 out`CourseDetailsEntityDataSource_Selected`方法替换为以下代码：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

此事件的事件自变量提供的一个集合，如果未选择任何内容的零项或一项将具有窗体中的所选的数据如果`Course`选择实体。 如果`Course`选择实体后，该代码使用`First`方法将集合转换为单个对象。 然后，它获取`StudentGrade`实体从导航属性，将其转换为一个集合，并将绑定`GradesListView`控件添加到集合。

这是足够以显示分级，但你想要确保空数据模板中的消息显示了第一次显示页面，每当未选择某一课程。 为此，请创建以下方法，它将调用从两个位置：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

调用此新方法从`Page_Load`方法以显示将显示的页的空数据模板第一个时间。 并将其从`InstructorsGridView_SelectedIndexChanged`方法选择一个教师时，将引发该事件，因为这意味着新的课程加载到课程`GridView`尚未选择控件和 none。 以下是两个调用：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

运行页面。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

选择已分配，课程教师，然后选择过程。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

您现在已经了解几种方法可以处理的相关数据。 在以下教程中，你将了解如何添加现有的实体之间的关系如何删除关系，以及如何添加与现有实体建立了关系的新实体。

>[!div class="step-by-step"]
[上一页](the-entity-framework-and-aspnet-getting-started-part-3.md)
[下一页](the-entity-framework-and-aspnet-getting-started-part-5.md)
