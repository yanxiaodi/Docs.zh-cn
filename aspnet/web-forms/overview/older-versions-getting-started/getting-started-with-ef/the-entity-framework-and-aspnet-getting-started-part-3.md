---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: "如何开始使用实体框架 4.0 数据库和 ASP.NET 4 Web 窗体的第 3 部分 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建 ASP.NET Web 窗体应用程序使用实体框架。 该示例应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 1ec8891c4ccf71494389ba562fdfb4b88055d12f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>如何开始使用实体框架 4.0 数据库和 ASP.NET 4 Web 窗体的第 3 部分
====================
通过[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web 窗体应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>筛选、 排序和分组数据

在以前的教程，你使用了`EntityDataSource`控件来显示和编辑数据。 在本教程中将筛选、 排序和分组数据。 当你执行此操作通过设置属性的`EntityDataSource`控件，语法是不同于其他数据源控件。 正如你将看到的但是，你可以使用`QueryExtender`控件最小化这些差异。

你将更改*Students.aspx*页后，可以筛选 （学生版），按名称排序，并在名称上的进行搜索。 你将更改*Courses.aspx*页后，可以按名称显示为所选的部门和搜索课程的课程。 最后，将添加到的学生统计信息*About.aspx*页。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>使用 EntityDataSource"Where"属性来筛选数据

打开*Students.aspx*你在前面的教程中创建的页。 按当前配置，`GridView`页中的控件显示来自所有名称`People`实体集。 但是，你想要显示仅学生，你可以通过选择查找`Person`具有非 null 注册日期的实体。

切换到**设计**查看和选择`EntityDataSource`控件。 在**属性**窗口中，设置`Where`属性`it.EnrollmentDate is not null`。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

在中使用的语法`Where`属性`EntityDataSource`控件是 Entity SQL。 实体 SQL 是类似于 TRANSACT-SQL，但它使用实体，而不是数据库对象，用于自定义。 在表达式中`it.EnrollmentDate is not null`，word`it`表示由查询返回的实体的引用。 因此，`it.EnrollmentDate`指`EnrollmentDate`属性`Person`实体，`EntityDataSource`控制返回。

运行页面。 学生列表现在包含仅学生。 (这里没有行没有注册日期。)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>使用订单数据的 EntityDataSource"OrderBy"属性

你还想此列表是按名称顺序首先显示时。 与*Students.aspx*仍在中打开的页**设计**视图中，与`EntityDataSource`控件仍处于选中状态，在**属性**窗口集**OrderBy**属性`it.LastName`。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

运行页面。 学生列表现在位于按姓氏的顺序。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>使用控制参数设置的"Where"属性

如与其他数据源控件，你可以向其传递参数值`Where`属性。 上*Courses.aspx*页上，你在本教程的第 2 部分中创建，你可以使用此方法以显示与用户从下拉列表中选择的部门相关联的课程。

打开*Courses.aspx*并切换到**设计**视图。 添加第二个`EntityDataSource`控制转移到页上，并将其命名`CoursesEntityDataSource`。 连接到`SchoolEntities`模型，并选择`Courses`作为**EntitySetName**值。

在**属性**窗口中，单击中的省略号**其中**属性框。 (请确保`CoursesEntityDataSource`控件仍在使用之前选定**属性**窗口。)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

**表达式编辑器**对话框随即显示。 在此对话框中，选择**自动生成 Where 表达式基于提供的参数**，然后单击**添加参数**。 该参数命名为`DepartmentID`，选择**控件**作为**参数源**值，然后选择**DepartmentsDropDownList**作为**ControlID**值。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

单击**显示高级属性**，然后在**属性**窗口**表达式编辑器**对话框中，更改`Type`属性`Int32`。

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

完成后，单击**确定**。

下面的下拉列表中，添加`GridView`控制到页面并将其命名`CoursesGridView`。 连接到`CoursesEntityDataSource`数据源控件，单击**刷新架构**，单击**编辑列**，并删除`DepartmentID`列。 `GridView`控件标记将类似于下面的示例。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

当用户更改所选的系下拉列表中时，你会想关联的课程，使其自动更改的列表。 若要简化此种情况发生，选择下拉列表中，然后在**属性**窗口集`AutoPostBack`属性`True`。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

现在，完成使用设计器后，切换到**源**查看和替换`ConnectionString`和`DefaultContainer`命名的属性`CoursesEntityDataSource`控件替换为`ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`属性。 完成后，控件的标记将类似于以下的示例。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

运行页面，并使用下拉列表选择不同的部门。 所选的部门提供的课程将显示在`GridView`控件。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>使用对数据进行分组的 EntityDataSource"GroupBy"属性

假设 Contoso 大学想要在其有关上放置一些学生正文统计信息。 具体而言，它想要按用户注册日期显示学生数的细分。

打开*About.aspx*，然后在**源**视图中，将现有内容`BodyContent`之间控件替换为"学生正文统计信息"`h2`标记：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

在标题、 添加`EntityDataSource`控制并将其命名`StudentStatisticsEntityDataSource`。 连接到`SchoolEntities`，选择`People`实体设置，并使**选择**框中保持不变的向导。 在中设置以下属性**属性**窗口：

- 若要筛选为仅学生，设置`Where`属性`it.EnrollmentDate is not null`。
- 若要按注册日期对结果进行分组，将设置`GroupBy`属性`it.EnrollmentDate`。
- 若要选择注册日期和学生的数量，设置`Select`属性`it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`。
- 若要按注册日期对结果进行排序，将设置`OrderBy`属性`it.EnrollmentDate`。

在**源**视图中，将`ConnectionString`和`DefaultContainer`名称与属性`ContextTypeName`属性。 `EntityDataSource`控件标记现在将类似于下面的示例。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

语法`Select`， `GroupBy`，和`Where`属性类似于 TRANSACT-SQL 除`it`指定当前实体的关键字。

添加以下标记以创建`GridView`控件来显示数据。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

运行该页面以查看按注册日期的显示学生的数量的列表。

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>用于筛选和排序的 QueryExtender 控件

`QueryExtender`控件使您能够指定筛选和排序在标记中。 语法是独立于你使用的数据库管理系统 (DBMS)。 还有通常独立于实体框架使用用于导航属性的语法是唯一的实体框架的异常。

在本教程的本部分将使用`QueryExtender`控件添加到筛选器和订单数据，并之一的排序依据字段将为导航属性。

(如果想要使用代码而不是标记扩展通过自动生成的查询`EntityDataSource`控件，你可以执行该操作处理`QueryCreated`事件。 这是如何`QueryExtender`控件扩展`EntityDataSource`还控制查询。)

打开*Courses.aspx*页，然后下以前添加的标记，插入以下标记以创建一个标题，用于输入搜索字符串，搜索按钮，一个文本框和一个`EntityDataSource`绑定到控件`Courses`实体集。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

请注意，`EntityDataSource`控件的`Include`属性设置为`Department`。 在数据库中，`Course`表不包含部门名称; 它将包含`DepartmentID`外键列。 如果已查询数据库直接，若要获取部门名称以及过程的数据，你将不得不加入`Course`和`Department`表。 通过设置`Include`属性`Department`，指定实体框架应执行获取相关的工作`Department`实体获取时`Course`实体。 `Department`实体然后存储在`Department`导航属性`Course`实体。 (默认情况下，`SchoolEntities`由数据模型设计器生成的类检索相关的数据后，需要你已将该数据源控件绑定到该类，因此设置`Include`属性不需要。 但是，将其设置可以提高性能的页上，因为否则实体框架会导致单独的数据库调用来检索数据`Course`实体和相关`Department`实体。)

后`EntityDataSource`控件刚刚创建，插入以下标记以创建`QueryExtender`绑定到的控件`EntityDataSource`控件。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

`SearchExpression`元素指定你想要选择其标题与在文本框中输入的值匹配的课程。 仅在文本框中输入更多的字符进行比较，因为`SearchType`属性指定`StartsWith`。

`OrderByExpression`元素指定结果集将按部门名称中的课程标题。 请注意如何指定部门名称： `Department.Name`。 因为之间的关联`Course`实体和`Department`实体是一对一，`Department`导航属性包含`Department`实体。 （如果这是一个对多关系，该属性将包含集合。）若要获取的部门名称，必须指定`Name`属性`Department`实体。

最后，添加`GridView`控件来显示的课程的列表：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

第一列是显示部门名称的模板字段。 数据绑定表达式指定`Department.Name`，就像你在中看到`QueryExtender`控件。

运行页面。 初始屏幕按部门和课程标题将按顺序显示所有课程的列表。

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

输入"m"，然后单击**搜索**若要查看其标题以"m"（搜索不是案例敏感） 开头的所有课程。

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>使用"Like"运算符来筛选数据

你可以实现的效果类似于`QueryExtender`控件的`StartsWith`， `Contains`，和`EndsWith`通过搜索类型`Like`中的运算符`EntityDataSource`控件的`Where`属性。 在本教程的此部分中，你将了解如何使用`Like`运算符来按名称搜索一名学生。

打开*Students.aspx*中**源**视图。 后`GridView`控件中，添加以下标记：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

此标记是类似于您已了解前面除`Where`属性值。 第二部分`Where`表达式定义的子字符串搜索 (`LIKE %FirstMidName% or LIKE %LastName%`)，搜索为任何在文本框中输入第一个和最后一个名称。

运行页面。 最初你看到所有学生因为的默认值为`StudentName`参数是"%"。

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

在文本框中输入字母"g"，然后单击**搜索**。 你看到学生的名字或姓氏名称中具有"g"的列表。

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

你已现在显示、 更新、 筛选、 排序，和对各个表中的数据分组。 在下一教程中你将开始处理的相关数据 （主 / 从方案）。

>[!div class="step-by-step"]
[上一页](the-entity-framework-and-aspnet-getting-started-part-2.md)
[下一页](the-entity-framework-and-aspnet-getting-started-part-4.md)
