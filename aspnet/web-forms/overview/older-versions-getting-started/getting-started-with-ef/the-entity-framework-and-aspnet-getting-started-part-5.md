---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: 如何开始使用实体框架 4.0 数据库和 ASP.NET 4 Web 窗体的第 5 部分 |Microsoft 文档
author: tdykstra
description: Contoso 大学示例 web 应用程序演示如何创建 ASP.NET Web 窗体应用程序使用实体框架。 该示例应用程序...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 5efc5ff367d5da5df060eba0028399af898a69fa
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>如何开始使用实体框架 4.0 数据库和 ASP.NET 4 Web 窗体的第 5 部分
====================
通过[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web 窗体应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>使用相关数据，继续执行

在以前的教程，你开始使用`EntityDataSource`控件处理的相关数据。 显示多个级别的层次结构和编辑导航属性中的数据。 在本教程中你将继续通过添加和删除关系并添加新实体具有与现有实体的关系，以便使用相关数据。

你将创建一个页，将添加到部门分配的课程。 部门已存在，并且当你创建新的课程，同时你将建立与已存在的部门之间的关系。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

你还将创建具有多对多关系，即可将一名教师分配给课程 （添加你选择的两个实体之间的关系） 的页或从课程中删除一个教师 (删除两个实体之间的关系你选择）。 在数据库中，添加一个教师和过程之间的关系会导致新行添加到`CourseInstructor`关联表; 中删除关系涉及删除从行`CourseInstructor`关联表。 但是，你执行此操作在实体框架通过设置导航属性，而不会引用`CourseInstructor`显式表。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>将具有关系的实体添加到现有实体

创建一个名为的新 web 页*CoursesAdd.aspx*使用*Site.Master*母版页，并添加以下标记`Content`控件名为`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

此标记将创建`EntityDataSource`控件选择课程，这样的插入，并指定的处理程序`Inserting`事件。 你将使用该处理程序来更新`Department`导航属性时新`Course`创建实体。

标记还会创建`DetailsView`要用于添加新控件`Course`实体。 此标记使用的绑定的字段`Course`实体属性。 你必须输入`CourseID`因为这不是系统生成的 ID 字段值。 相反，它是创建过程时必须手动指定课程编号。

你使用的模板字段`Department`导航属性因为导航属性不能与使用`BoundField`控件。 模板字段提供用于选择部门的下拉列表。 下拉列表绑定到`Departments`实体集使用`Eval`而非`Bind`、 试原因你不能直接绑定以更新它们的导航属性。 指定的处理程序`DropDownList`控件的`Init`事件，以便你可以通过更新的代码存储对用于控件的引用`DepartmentID`外键。

在*CoursesAdd.aspx.cs*之后分部类声明中，添加一个类字段以保存对引用`DepartmentsDropDownList`控件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

添加的处理程序`DepartmentsDropDownList`控件的`Init`事件，以便你可以存储对控件的引用。 这样就可以获取用户输入的值并使用它更新`DepartmentID`值`Course`实体。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

添加的处理程序`DetailsView`控件的`Inserting`事件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

当用户单击`Insert`、`Inserting`才能插入新记录，将引发事件。 处理程序中的代码获取`DepartmentID`从`DropDownList`控制并使用它来设置值，将用于`DepartmentID`属性`Course`实体。

实体框架将会负责处理添加到此课程`Courses`关联的导航属性`Department`实体。 它还会添加到部门`Department`导航属性`Course`实体。

运行页面。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

输入 ID、 标题、 大量的信用额度，并选择一个部门，然后单击**插入**。

运行*Courses.aspx*页上，然后选择同一部门，以查看新的过程。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>使用多对多关系

之间的关系`Courses`实体集和`People`实体集是多对多关系。 A`Course`实体具有名为的导航属性`People`可以包含零个、 一个或多个相关`Person`（表示教师分配来教授该过程） 的实体。 和`Person`实体具有名为的导航属性`Courses`可以包含零个、 一个或多个相关`Course`实体 (表示课程该教师分配来教授)。 一个教师可能教授多个课程，并可能由多个教师讲授了一个过程。 在本演练的此部分中，你将添加和删除之间的关系`Person`和`Course`通过更新相关的实体的导航属性的实体。

创建一个名为的新 web 页*InstructorsCourses.aspx*使用*Site.Master*母版页，并添加以下标记`Content`控件名为`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

此标记将创建`EntityDataSource`检索的名称的控件和`PersonID`的`Person`讲师的实体。 A`DropDrownList`控件绑定到`EntityDataSource`控件。 `DropDownList`控件指定的处理程序`DataBound`事件。 你将使用到 databind 两个下拉列表中，显示课程此处理程序。

标记还会创建以下控件组的用于将过程分配给所选教师：

- A`DropDownList`控制来选择要分配的课程。 此控件将使用当前未分配给所选教师的课程进行填充。
- A`Button`控件以启动分配。
- A`Label`控件来显示一条错误消息，如果分配将失败。

最后，标记还会创建一组控件用于从所选教师删除过程。

在*InstructorsCourses.aspx.cs*，添加 using 语句：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

添加用于填充显示课程的两个下拉列表的方法：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

此代码获取从所有课程`Courses`实体设置和都获取从课程`Courses`导航属性`Person`所选教师的实体。 然后，它将确定哪些课程分配给该教师并相应地填充下拉列表。

添加的处理程序`Assign`按钮的`Click`事件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

此代码获取`Person`所选教师的实体获取`Course`实体所选的课程，并将添加到所选的课程`Courses`教师的导航属性`Person`实体。 然后将所做的更改保存到数据库，并重新填充下拉列表，因此会立即看到结果。

添加的处理程序`Remove`按钮的`Click`事件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

此代码获取`Person`所选教师的实体获取`Course`实体所选的课程，并删除从所选的课程`Person`实体的`Courses`导航属性。 然后将所做的更改保存到数据库，并重新填充下拉列表，因此会立即看到结果。

将代码添加到`Page_Load`时没有错误，以报告，并为添加处理程序，可确保错误消息的方法不可见`DataBound`和`SelectedIndexChanged`教师下拉列表来填充课程下拉列表的事件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

运行页面。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

选择一个教师。 **指派课程**下拉列表显示教师不讲述，课程和**删除某一课程**下拉列表显示教师已分配给的课程。 在**指派课程**部分，选择某一课程，然后单击**分配**。 本课程将移到**删除某一课程**下拉列表。 选择在课程 **删除某一课程** 部分并单击 **删除***。* 本课程将移到**指派课程**下拉列表。

您现在已经了解一些更多的方法，以便使用相关数据。 在以下教程中，你将了解如何在数据模型中使用继承，以提高你的应用程序的可维护性。

>[!div class="step-by-step"]
[上一页](the-entity-framework-and-aspnet-getting-started-part-4.md)
[下一页](the-entity-framework-and-aspnet-getting-started-part-6.md)
