---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: "如何开始使用实体框架 4.0 数据库和 ASP.NET 4 Web 窗体-第 6 部分 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建 ASP.NET Web 窗体应用程序使用实体框架。 该示例应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 164c2002a119420555d2c7065c5a79a5f433a725
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>如何开始使用实体框架 4.0 数据库和 ASP.NET 4 Web 窗体-第 6 部分
====================
通过[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web 窗体应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>实现“每个层次结构一个表”继承

在以前的教程，你使用相关的数据处理，通过添加和删除关系并添加到现有实体的关系的新实体。 本教程将演示如何在数据模型中实现继承。

在面向对象编程中，你可以使用继承以使其更轻松地使用相关的类。 例如，你可以创建`Instructor`和`Student`派生自的类`Person`基类。 你可以在实体框架中创建相同类型的实体之间的继承结构。

在本教程的此部分中，不会创建任何新的 web 页。 相反，你将添加到数据模型中派生的实体和修改现有的页面，以使用新的实体。

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>每个-层次结构一个表与每种类型一个表继承

在一个表或多个表中，数据库可以存储有关相关的对象的信息。 例如，在`School`数据库，`Person`表包含有关学生和教师单个表中的信息。 某些列仅适用于教师 (`HireDate`)，某些仅向学生 (`EnrollmentDate`)，而另一些同时向 (`LastName`， `FirstName`)。

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

你可以配置实体框架可以创建`Instructor`和`Student`继承的实体`Person`实体。 从单个数据库表生成的实体继承结构的此模式称为*表每个层次结构*(TPH) 继承。

对于课程，`School`数据库使用不同的模式。 联机课程和现场课程存储在单独的表，其中每个具有外键指向`Course`表。 仅在存储信息普遍适用于这两种过程类型`Course`表。

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

你可以配置实体框架数据模型，以便`OnlineCourse`和`OnsiteCourse`实体继承自`Course`实体。 从具有引用返回到的表中存储数据普遍适用于所有类型，每个单独的表的每个类型的单独表中生成的实体继承结构的此模式称为*每种类型的表*(TPT) 继承。

TPH 继承模式通常因为 TPT 模式可能会导致复杂的联接查询在实体框架中比 TPT 继承模式提供更好的性能。 本演练演示如何实现 TPH 继承。 将执行以下步骤来执行该操作：

- 创建`Instructor`和`Student`实体类型的派生自`Person`。
- 适用于从派生的实体的移动属性`Person`到派生的实体的实体。
- 在派生类型中设置属性的约束。
- 请`Person`实体抽象实体。
- 映射每个派生的实体访问`Person`具有一个条件以指定如何确定表是否`Person`行派生类型的表示。

## <a name="adding-instructor-and-student-entities"></a>添加教师和学生实体

打开*SchoolModel.edmx*文件中，右击设计器中，选择中的未占用的区域**添加**，然后选择**实体***。*

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

在**添加实体**对话框中，命名实体`Instructor`并设置其**基类型**选项设为`Person`。

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

单击“确定”。 设计器创建`Instructor`派生自的实体`Person`实体。 新的实体还没有任何属性。

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

重复此过程来创建`Student`又派生自的实体`Person`。

只有教师有雇佣日期，因此你需要将该属性从移动`Person`实体到`Instructor`实体。 在`Person`实体，右键单击`HireDate`属性，然后单击**剪切**。 然后右键单击**属性**中`Instructor`实体，然后单击**粘贴**。

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

雇佣日期`Instructor`实体不能为 null。 右键单击`HireDate`属性，单击**属性**，然后在**属性**窗口更改`Nullable`到`False`。

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

重复此过程以移动`EnrollmentDate`属性从`Person`实体到`Student`实体。 请确保也设置`Nullable`到`False`为`EnrollmentDate`属性。

现在，`Person`实体包含所共有的属性`Instructor`和`Student`实体 （除了外导航属性，这些属性在不移动的），该实体可以仅用作基实体中的继承结构。 因此，你需要确保将永远不会视为一个独立的实体。 右键单击`Person`实体中，选择**属性**，然后在**属性**窗口更改的值**抽象**属性**True**。

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>将教师和学生实体映射到 Person 表

现在你需要告诉实体框架如何区分`Instructor`和`Student`数据库中的实体。

右键单击`Instructor`实体，然后选择**表映射**。 在**映射详细信息**窗口中，单击**添加表或视图**和选择**人员**。

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

单击**添加条件**，然后选择**HireDate**。

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

更改**运算符**到**是**和**值 / 属性**到**Not Null**。

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

重复的过程`Students`实体，指定此实体映射到`Person`表时`EnrollmentDate`列不为 null。 然后保存并关闭数据模型。

生成项目以便创建为类的新实体，并使它们在设计器中可用。

## <a name="using-the-instructor-and-student-entities"></a>使用教师和学生实体

创建工作学生和教师数据，你将数据绑定到的网页时`Person`实体集，且上筛选`HireDate`或`EnrollmentDate`属性来限制返回的数据为学生或教师。 但是，现在当你将绑定到每个数据源控件`Person`实体集，你可以仅指定`Student`或`Instructor`应选择实体类型。 因为实体框架知道如何区分学生和讲师`Person`实体集，你可以删除`Where`你手动输入要这样做的属性设置。

在 Visual Studio 设计器中，你可以指定实体类型`EntityDataSource`控件应选择中**EntityTypeFilter**下拉列表框`Configure Data Source`向导，如下面的示例中所示。

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

然后在**属性**窗口，可移除`Where`子句不再需要如下面的示例中所示的值。

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

但是，因为你已更改的标记`EntityDataSource`要使用的控件`ContextTypeName`属性，不能运行**配置数据源**向导上`EntityDataSource`你所创建的控件。 因此，你将通过更改标记改为使所需的更改。

打开*Students.aspx*页。 在`StudentsEntityDataSource`控件中，删除`Where`属性，并添加`EntityTypeFilter="Student"`属性。 标记现在将类似于下面的示例：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

设置`EntityTypeFilter`属性可确保`EntityDataSource`控件将自动选择仅指定的实体类型。 如果你想要同时检索`Student`和`Instructor`实体类型，则不要设置此属性。 (你可以检索多个实体类型具有一个选择`EntityDataSource`控制仅当你使用只读数据访问控件。 如果你使用`EntityDataSource`控件插入、 更新或删除实体，并绑定到的实体集可以包含多个类型，如果你仅可以使用一种实体类型和你需要将此属性设置。)

重复的过程`SearchEntityDataSource`控制，但删除仅的一部分`Where`选择的属性`Student`而不是完全删除此属性的实体。 控件的开始标记现在将类似于下面的示例：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

运行页后，可以验证仍然工作像以前一样。

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

更新你在早期的教程中，以便他们使用新创建的以下页面`Student`和`Instructor`实体而不是`Person`实体，然后运行它们以验证它们之前一样的正常工作：

- 在*StudentsAdd.aspx*，添加`EntityTypeFilter="Student"`到`StudentsEntityDataSource`控件。 标记现在将类似于下面的示例： 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- 在*About.aspx*，添加`EntityTypeFilter="Student"`到`StudentStatisticsEntityDataSource`控制和删除`Where="it.EnrollmentDate is not null"`。 标记现在将类似于下面的示例： 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- 在*Instructors.aspx*和*InstructorsCourses.aspx*，添加`EntityTypeFilter="Instructor"`到`InstructorsEntityDataSource`控制和删除`Where="it.HireDate is not null"`。 中的标记*Instructors.aspx*现在类似于下面的示例： 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    中的标记*InstructorsCourses.aspx*现在将类似于下面的示例：

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

由于这些更改，你改进了以下几种方式的 Contoso 大学应用程序的可维护性。 已移动的 UI 层之外的选择和验证逻辑 (*.aspx*标记) 及做出该数据访问层的必要组成部分。 这可帮助将你的应用程序代码与您可能在将来对数据库架构或数据模型进行的更改隔离。 例如，你可能决定学生可能受雇成为教师的帮助，并因此将获取雇佣日期。 然后可以添加一个新的属性，来从教师区分学生和更新数据模型。 Web 应用程序中的任何代码将不需要更改除非你想要显示学生版雇佣日期。 添加的另一个好处`Instructor`和`Student`实体，则是你的代码比它称为时可以更轻松地理解`Person`的实际学生对象的或教师。

现在，你已了解一种方法在实体框架中实施的继承模式。 在以下教程中，你将了解如何使用存储的过程以更好地控制实体框架中如何访问数据库。

>[!div class="step-by-step"]
[上一页](the-entity-framework-and-aspnet-getting-started-part-5.md)
[下一页](the-entity-framework-and-aspnet-getting-started-part-7.md)
