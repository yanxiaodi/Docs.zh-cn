---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: "如何开始使用实体框架 4.0 数据库和 ASP.NET 4 Web 窗体的第 2 部分 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建 ASP.NET Web 窗体应用程序使用实体框架。 该示例应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: a549bd62bd78573c368784fd1529a830e009b0d4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>如何开始使用实体框架 4.0 数据库和 ASP.NET 4 Web 窗体的第 2 部分
====================
通过[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web 窗体应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>EntityDataSource 控件

在以前的教程，你可以创建网站、 数据库和数据模型。 在本教程中您可以使用`EntityDataSource`ASP.NET 提供以使其更易于使用的实体框架数据模型的控件。 你将创建`GridView`控件用于显示和编辑学生数据`DetailsView`用于添加新学生的控件和`DropDownList`控制来选择 （其中你将使用用于显示关联的课程的更高版本） 的部门。

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

请注意，在此应用程序也不会输入的验证向添加页更新数据库，不会像将在生产应用程序所需的那样强健一些错误处理。 保留本教程侧重于实体框架，可以使其获取太长。 有关如何将这些功能添加到你的应用程序的详细信息，请参阅[验证用户输入中的 ASP.NET Web Pages](https://msdn.microsoft.com/library/7kh55542.aspx)和[ASP.NET 页及应用程序中的错误处理](https://msdn.microsoft.com/library/w16865z6.aspx)。

## <a name="adding-and-configuring-the-entitydatasource-control"></a>添加和配置 EntityDataSource 控件

将开始通过配置`EntityDataSource`控件来读取`Person`从实体`People`实体集。

请确保您有打开的 Visual Studio 并且你正在使用该项目在第 1 部分中创建。 如果由于创建数据模型或最后一个更改以来对其进行，尚未生成项目，请现在生成项目。 对数据模型的更改将不会提供给设计器在生成项目之前。

创建新的 web 页使用**使用母版页的 Web 窗体**模板，并将其命名*Students.aspx*。

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

指定*Site.Master*作为母版页。 所有为这些教程创建的页将使用此母版页。

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

在**源**查看、 添加`h2`标题到`Content`控件名为`Content2`，下面的示例中所示：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

从**数据**选项卡**工具箱**，拖动`EntityDataSource`控件到页，请将其放置在标题下，将 ID 更改为`StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

切换到**设计**查看，单击数据源控件的智能标记，然后单击**配置数据源**以启动**配置数据源**向导。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

在**配置 ObjectContext**向导步骤中，选择**SchoolEntities**的值作为**名为连接**，然后选择**SchoolEntities**作为**DefaultContainerName**值。 然后，单击 **“下一步”**。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

注意： 如果在此时获取以下对话框中，你必须生成项目，然后再继续。

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

在**配置数据选择**步骤中，选择**人员**的值作为**EntitySetName**。 下**选择**，请确保**选择 A** ll 复选框处于选中状态。 然后选择要启用更新和删除的选项。 完成后，单击**完成**。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>配置数据库规则，以便允许删除

你将创建一个允许用户删除从学生页面`Person`具有与其他表的三个关系的表 (`Course`， `StudentGrade`，和`OfficeAssignment`)。 默认情况下，数据库会阻止你删除中的行`Person`是否存在相关的行中其他表中的一个。 首先，可以手动删除相关的行，或者你可以配置要删除时自动删除它们的数据库`Person`行。 在本教程中的学生记录，你将配置要自动删除相关的数据的数据库。 因为学生可以有相关的行仅在`StudentGrade`表，你需要配置三个关系之一。

如果你使用*School.mdf*文件下载从用本教程的项目中，因为这些配置更改已完成，则可以跳过此部分。 如果你通过运行脚本创建数据库，请通过执行以下步骤配置该数据库。

在**服务器资源管理器**，打开你在第 1 部分中创建该数据库关系图。 右键单击之间的关系`Person`和`StudentGrade`（表之间的行），然后选择**属性**。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

在**属性**窗口中，展开**INSERT 和 UPDATE 规范**并设置**DeleteRule**属性**Cascade**。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

保存并关闭关系图。 如果系统询问你是否是否想要更新数据库，单击**是**。

若要确保使模型保持与数据库正在同步的内存中的实体，必须在数据模型中设置相应的规则。 打开*SchoolModel.edmx*，右击之间的关联行`Person`和`StudentGrade`，然后选择**属性**。

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

在**属性**窗口中，设置**End1 OnDelete**到**Cascade**。

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

保存并关闭*SchoolModel.edmx*文件，然后再重新生成项目。

一般情况下，当数据库更改时，你有多个选择如何同步模型：

- 对于某些类型的更改 （如添加或刷新表、 视图或存储的过程），请右键单击该设计器并选择**从数据库更新模型**若要在设计器使所做的更改自动。
- 重新生成数据模型。
- 请手动更新与此类似。

在这种情况下，无法重新生成模型或刷新受关系更改的表，但然后您将不得不进行更改后的字段名称，然后重新 (从`FirstName`到`FirstMidName`)。

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>如何使用 GridView 控件以读取和更新实体

在本部分将使用`GridView`控件以显示、 更新或删除学生。

打开或切换到*Students.aspx*并切换到**设计**视图。 从**数据**选项卡**工具箱**，拖动`GridView`右侧的控件`EntityDataSource`控件中，将其命名为`StudentsGridView`、 单击智能标记，，然后选择**StudentsEntityDataSource**作为数据源。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

单击**刷新架构**(单击**是**如果系统提示你确认)，然后单击**启用分页**，**启用排序**， **启用编辑**，和**启用删除**。

单击**编辑列**。

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

在**所选字段**框中，删除**PersonID**， **LastName**，和**HireDate**。 通常不显示给用户的记录密钥、 雇用日期不是与学生，并且将在一个字段中作出名称的两个部分，以便你只需要一个名称字段。）

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

选择**FirstMidName**字段，然后单击**将此字段转换为 TemplateField**。

为执行同样**EnrollmentDate**。

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

单击**确定**然后切换到**源**视图。 剩余的更改都将更轻松地直接在标记中执行。 `GridView`控制标记现在看起来，如下例所示。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

第一列后命令字段的模板字段当前显示的第一个名称。 更改为类似于下面的示例此模板字段的标记：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

在显示模式下，两个`Label`控件显示的第一个和最后一个名称。 在编辑模式，因此您可以更改第一个和最后一个名称，来提供两个文本框。 与`Label`控件中的显示模式，则使用`Bind`和`Eval`完全为你的表达式将与直接连接到数据库的 ASP.NET 数据源控件。 唯一的区别是你要指定实体属性，而不是数据库列。

最后一列是显示注册日期的模板字段。 更改此字段为类似于下面的示例的标记：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

在显示和编辑模式下，格式字符串"{0，d}"会导致显示"short 日期"格式的日期。 （你的计算机可能被配置为显示此格式不同于在本教程中所显示的屏幕图像。）

请注意，在每个这些模板字段，设计器使用`Bind`表达式中的，通过默认情况下，但您已更改，则为`Eval`中的表达式`ItemTemplate`元素。 `Bind`表达式使数据中可用`GridView`控制属性，以防你需要访问代码中的数据。 在此页中，不需要访问在代码中，此数据，因此你可以使用`Eval`，这是效率更高。 有关详细信息，请参阅[外数据控件将数据](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx)。

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>修订 EntityDataSource 控件标记，以提高性能

中的标记`EntityDataSource`控件中，删除`ConnectionString`和`DefaultContainerName`属性并将其替换为`ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`属性。 这是所进行的更改应每次创建`EntityDataSource`控制，除非你需要使用不同于进行硬编码的对象上下文类中的连接。 使用`ContextTypeName`属性提供了以下好处：

- 性能更好。 当`EntityDataSource`控件初始化数据模型使用`ConnectionString`和`DefaultContainerName`属性，它执行附加工作以加载对每个请求的元数据。 这不是必需的如果你指定`ContextTypeName`属性。
- 延迟加载已开启生成的对象上下文类在默认情况下 (如`SchoolEntities`在本教程中) 在实体 Framework 4.0。 这意味着导航属性加载了与相关数据自动权限时需要它。 在本教程后面详细详细阐述延迟加载。
- 已应用于对象上下文类的任何自定义 (在这种情况下，`SchoolEntities`类) 将可供使用的控件`EntityDataSource`控件。 自定义对象上下文类是高级的主题未涵盖在本教程系列。 有关详细信息，请参阅[扩展实体框架生成类型](https://msdn.microsoft.com/library/dd456844.aspx)。

标记现在将类似于下面的示例 （属性顺序可能不同）：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

`EnableFlattening`特性引用了一种功能，因为作为实体属性不公开外键列。 已在早期版本的实体框架需要。 当前版本让你可以使用*外键关联*，这意味着外键属性公开为以外的所有多对多关联。 如果你的实体具有外键属性，但未[复杂类型](https://msdn.microsoft.com/library/bb738472.aspx)，你可以将此属性设置为`False`。 不删除该属性标记，由于默认值为`True`。 有关详细信息，请参阅[平展对象 (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx)。

运行页面，你看到的学生和 （你将筛选的只是学生在下一教程中） 的员工列表。 一起显示的名字和姓氏。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

若要排序显示，请单击列名称。

单击**编辑**任何行中。 文本框将显示你可在其中更改第一个和最后一个名称。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

**删除**按钮也适用。 为已注册日期的行和行消失后，请单击删除。 （没有注册日期的行表示教师和你可能会出现引用完整性错误。 在下一教程中你将筛选此列表包括只需学生。）

## <a name="displaying-data-from-a-navigation-property"></a>显示从导航属性的数据

现在假设你想要知道多少课程中注册每个学生。 实体框架提供了在该信息`StudentGrades`导航属性`Person`实体。 由于数据库设计不允许注册课程中，而无需分配一个等级一名学生，本教程中你可以假定该采用行`StudentGrade`与某一课程相关联的表行是与正在注册过程中相同。 (`Courses`导航属性仅适用于教师。)

当你使用`ContextTypeName`属性`EntityDataSource`控件，实体框架自动检索信息的导航属性访问该属性时。 这称为*延迟加载*。 但是，这可能效率低下，因为这会导致单独调用需要每个时间的其他信息的数据库。 如果你需要从导航属性的数据对于返回的每个实体`EntityDataSource`控件，它会更加高效检索相关的数据以及对数据库的单个调用中的实体本身。 这称为*预先加载*，还可以通过设置指定导航属性的预先加载`Include`属性`EntityDataSource`控件。

在*Students.aspx*，你想要显示每个学生的课程，因此预先加载是最佳选择。 如果你已显示所有学生但显示的课程数仅为几个它们 （这将需要编写一些代码除了标记），延迟加载可能是更好的选择。

打开或切换到*Students.aspx*，切换到**设计**视图中，选择`StudentsEntityDataSource`，然后在**属性**窗口集**包括**属性**StudentGrades**。 (如果你想要获取多个导航属性，则可以指定其名称以逗号分隔 — 例如， **StudentGrades、 课程**。)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

切换到**源**视图。 在`StudentsGridView`控件后的最后一个`asp:TemplateField`元素中，添加以下新的模板字段：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

在`Eval`表达式，你可以引用导航属性`StudentGrades`。 由于此属性包含一个集合，它有`Count`可用于显示在其中注册学生课程数的属性。 在更高版本的教程中，你将看到如何显示从包含而不是集合的单个实体的导航属性的数据。 (请注意，不能使用`BoundField`元素来显示于导航属性的数据。)

运行页面，你现在看到多少课程学生中注册。

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>说明如何控制用于插入实体

下一步是创建一个页，具有`DetailsView`将使你可以添加新的学生的控件。 关闭浏览器，然后创建新的 web 页使用*Site.Master*母版页。 将该页命名为*StudentsAdd.aspx*，然后切换到**源**视图。

添加以下标记替换的现有标记`Content`控件名为`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

此标记将创建`EntityDataSource`类似于在你创建的控件*Students.aspx*，只是它可用于插入。 与`GridView`控制的绑定的字段`DetailsView`控件完全按照它们应该是一个数据控件，直接连接到数据库，只不过它们引用实体属性进行编码。 在这种情况下，`DetailsView`控件仅用于插入行，因此你已设置的默认模式为`Insert`。

运行页面，并添加新的学生。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

不会发生插入一个新的学生之后，但现在运行*Students.aspx*，你将看到新的学生信息。

## <a name="displaying-data-in-a-drop-down-list"></a>在下拉列表中显示数据

在以下步骤中将数据绑定`DropDownList`控件添加到的实体集使用`EntityDataSource`控件。 本教程的本部分，不会执行与此列表的很多操作。 不过，在后续部分中，将使用列表来让用户选择要显示与部门相关联的课程部门。

创建一个名为的新 web 页*Courses.aspx*。 在**源**视图中，添加到标题`Content`控件名为`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

在**设计**查看、 添加`EntityDataSource`到页面的控件就像之前，除了这一次将其命名`DepartmentsEntityDataSource`。 选择**部门**作为**EntitySetName**值，然后仅选择**DepartmentID**和**名称**属性。

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

从**标准**选项卡**工具箱**，拖动`DropDownList`控件到页面中，将其命名为`DepartmentsDropDownList`，单击智能标记，然后选择**选择数据源**到启动**数据源配置向导**。

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

在**选择数据源**步骤中，选择**DepartmentsEntityDataSource**作为数据源中，单击**刷新架构**，然后选择**名称**作为要显示的数据字段和**DepartmentID**作为值数据字段。 单击 **“确定”**。

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

如与其他 ASP.NET 数据源控件，除了你在指定的实体和实体属性，使用数据绑定到控件使用实体框架的方法都是相同的。

切换到**源**查看和添加"选择部门:"立即之前`DropDownList`控件。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

作为提示，更改的标记`EntityDataSource`控件此时，通过替换`ConnectionString`和`DefaultContainerName`属性与`ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`属性。 最好是通常创建在更改之前已链接到数据源控件的数据绑定控件后将一直等到`EntityDataSource`控制标记，因为你进行更改后，设计器将不会提供你**刷新架构**数据绑定控件中的选项。

运行页面，并可以从下拉列表中选择一个部门。

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

这将完成使用简介`EntityDataSource`控件。 使用此控件通常无异于使用的其他 ASP.NET 数据源控件，但在于引用实体和属性而不是表和列。 唯一的例外是当你想要访问导航属性。 在下一教程中你将看到语法使用`EntityDataSource`控件可能也不同于其他数据源控件时筛选、 分组和排序数据。

>[!div class="step-by-step"]
[上一页](the-entity-framework-and-aspnet-getting-started-part-1.md)
[下一页](the-entity-framework-and-aspnet-getting-started-part-3.md)
