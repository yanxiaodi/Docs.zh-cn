---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: "如何开始使用实体框架 4.0 数据库和 ASP.NET 4 Web 窗体的第 7 部分 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建 ASP.NET Web 窗体应用程序使用实体框架。 该示例应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: aeea122636f5235364e6a40cb6e041b1fe221317
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>如何开始使用实体框架 4.0 数据库和 ASP.NET 4 Web 窗体的第 7 部分
====================
通过[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web 窗体应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>使用存储过程

在以前的教程，你实现的每个层次结构一个表继承模式。 本教程将演示如何使用存储的过程以获得更好地控制数据库访问。

实体框架允许你指定它应为数据库访问使用存储的过程。 对于任何实体类型，你可以指定要用于创建、 更新或删除实体，该类型的存储的过程。 然后在数据模型中可以添加到存储过程可用于执行任务，例如检索的实体集的引用。

使用存储的过程是为数据库访问的常见要求。 在某些情况下数据库管理员可能需要的所有数据库访问都经历出于安全原因的存储过程。 在其他情况下你可能想要将业务逻辑内置于某些实体框架使用在其更新数据库时的进程。 例如，每当删除的实体时你可能想要将其复制到的存档数据库。 或者，每次更新行时你可能想要进行更改，用于记录到日志记录表中写入行。 你可以在实体框架删除的实体或更新实体时调用的存储过程来执行这些类型的任务。

如前面的教程，你将不创建任何新页。 相反，你将更改实体框架访问数据库，对于某些已创建的页的方式。

在本教程中，你将创建用于插入数据库中的存储的过程`Student`和`Instructor`实体。 你也将它们添加到数据模型中，并且你将指定，实体框架应使用这些添加`Student`和`Instructor`到数据库的实体。 你还将创建可用于检索存储的过程`Course`实体。

## <a name="creating-stored-procedures-in-the-database"></a>在数据库中创建存储的过程

(如果你使用*School.mdf*文件从可用于本教程使用下载的项目中，你可以跳过本部分因为已存在存储的过程。)

在**服务器资源管理器**，展开*School.mdf*，右键单击**存储过程**，然后选择**添加新存储过程**。

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

复制以下 SQL 语句并将其粘贴到存储的过程窗口中，替换主干存储的过程。

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student`实体具有四个属性： `PersonID`， `LastName`， `FirstName`，和`EnrollmentDate`。 数据库将自动生成的 ID 值和存储的过程接受其他三个参数。 存储的过程返回的新行的记录密钥的值，以使实体框架可以跟踪的它在内存中保留的实体的版本中。

保存并关闭存储的过程窗口。

创建`InsertInstructor`相同的方式，使用以下 SQL 语句中存储过程：

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

创建`Update`存储过程`Student`和`Instructor`实体还。 (数据库中已经有`DeletePerson`存储过程都将起作用`Instructor`和`Student`实体。)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

在本教程中，你将映射所有三个函数 （插入、 更新和删除） 为每个实体类型。 实体框架版本 4 允许你映射仅仅一个或两个工作者函数存储过程而不映射其他，有一个例外： 如果您映射更新函数，但不是删除函数时，实体框架将引发异常时你尝试删除实体。 在实体框架版本 3.5 中，则不会拥有此多大程度上的映射存储的过程： 如果映射一个函数都需要映射所有三个。

若要创建的读取，而不是更新数据的存储的过程，创建一个选择所有`Course`实体，使用以下 SQL 语句：

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>将存储的过程添加到数据模型

在数据库中，现在定义的存储的过程，但必须将它们添加到数据模型，使它们可对 Entity Framework。 打开*SchoolModel.edmx*，右键单击设计图面，然后选择**从数据库更新模型**。 在**添加**选项卡**选择数据库对象**对话框框中，展开**存储过程**，选择新创建的存储的过程和`DeletePerson`存储过程，并依次**完成**。

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>映射存储的过程

在数据模型设计器中，右键单击`Student`实体，然后选择**存储过程映射**。

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

**映射详细信息**窗口出现时，可以在其中指定实体框架应使用的插入、 更新和删除此类型的实体的存储的过程。

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

设置**插入**函数来**InsertStudent**。 窗口显示存储的过程参数，其中每个必须映射到实体属性的列表。 因为的名称相同，其中两项将自动映射。 没有名为实体属性`FirstName`，因此必须手动选择`FirstMidName`从显示可用的实体属性的下拉列表。 (这是因为你更改的名称`FirstName`属性`FirstMidName`在第一个教程。)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

在同一个**映射详细信息**窗口中，映射`Update`函数来`UpdateStudent`存储过程 (请确保你指定`FirstMidName`的参数值作为`FirstName`，就像`Insert`存储过程） 和`Delete`函数来`DeletePerson`存储过程。

[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

按照相同的过程来映射 insert、 update 和 delete 存储过程到讲师`Instructor`实体。

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

对于读取而不是更新数据的存储过程，你使用**模型浏览器**窗口映射到实体的存储的过程键入它返回。 在数据模型设计器中，右键单击设计图面并选择**模型浏览器**。 打开**SchoolModel.Store**节点，然后打开**存储过程**节点。 然后右键单击`GetCourses`存储过程和选择**添加函数导入**。

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

在**添加函数导入**对话框中，在**返回集合的**选择**实体**，然后选择`Course`作为实体类型返回。 完成后，单击**确定**。 保存并关闭*.edmx*文件。

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>使用插入、 更新和删除存储的过程

存储的过程以插入、 更新和删除数据由实体框架自动将其添加到数据模型和映射到适当的实体后。 你现在可以运行*StudentsAdd.aspx*页上，并且每次创建新的学生，将使用实体框架`InsertStudent`存储过程来向其中添加新行`Student`表。

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

运行*Students.aspx*页和新的学生将出现在列表。

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

更改名称，以验证更新函数适用，然后再删除学生以验证 delete 函数适用。

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>使用选择的存储的过程

实体框架不自动运行存储的过程如`GetCourses`，并且不能使用它们与`EntityDataSource`控件。 若要使用它们，它们从代码调用。

打开*InstructorsCourses.aspx.cs*文件。 `PopulateDropDownLists`方法使用 LINQ 实体查询来检索所有课程实体，以便它可以循环访问列表并确定的哪些教师分配给，哪些是未分配：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

将此替换为以下代码：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

页现在使用`GetCourses`存储过程来检索所有课程的列表。 运行页后，可以验证它像以前一样工作。

(与这些实体，具体取决于相关的数据，通过存储过程检索的实体的导航属性可能不自动填充`ObjectContext`默认设置。 有关详细信息，请参阅[加载相关对象](https://msdn.microsoft.com/library/bb896272.aspx)MSDN 库中。)

在下一步的教程中，你将了解如何使用动态数据功能来更简便地程序和测试数据格式设置和验证规则。 指定每个网页规则，如数据格式字符串和字段是必填，而不可以在数据模型元数据中指定此类规则并自动在每一页上将它们应用于。

>[!div class="step-by-step"]
[上一页](the-entity-framework-and-aspnet-getting-started-part-6.md)
[下一页](the-entity-framework-and-aspnet-getting-started-part-8.md)
