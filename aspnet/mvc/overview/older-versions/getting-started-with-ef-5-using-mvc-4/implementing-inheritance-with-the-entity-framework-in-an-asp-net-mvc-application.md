---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: "在 ASP.NET MVC 应用程序 (10 的第 8) 中实现继承，并使用实体框架 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 54e46c6f996b6fe86a227c851562e61678b02780
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>在 ASP.NET MVC 应用程序 (10 的第 8) 实现与实体框架的继承
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 你可以从头开始教程系列或[下载这一章的初学者项目](building-the-ef5-mvc4-chapter-downloads.md)和从这里开始。
> 
> > [!NOTE] 
> > 
> > 如果你遇到无法解决的问题[下载已完成的章](building-the-ef5-mvc4-chapter-downloads.md)并尝试重现你的问题。 你通常可以通过比较你的代码已完成的代码会发现问题的解决方案。 一些常见的错误和如何解决它们，请参阅[错误和解决方法。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


在前面的教程中，你将处理并发异常。 本教程将演示如何在数据模型中实现继承。

在面向对象编程中，你可以使用继承以消除冗余代码。 本教程中，你将更改`Instructor`和`Student`类，以使其从中派生`Person`基本类，其中包含属性，如`LastName`所共有的教师和学生。 不会添加或更改任何 web 页面，但你将更改某些代码并将在数据库中自动反映这些更改。

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>每个-层次结构一个表与每种类型一个表继承

在面向对象编程中，你可以使用继承以使其更轻松地使用相关的类。 例如，`Instructor`和`Student`中的类`School`数据模型共享多个属性，这将导致冗余代码：

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

假设你想要消除冗余代码由共享的属性以`Instructor`和`Student`实体。 你可以创建`Person`基类其中仅包含那些共享属性，然后进行`Instructor`和`Student`实体继承自该基类，如下面的插图中所示：

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

有几种方法无法在数据库中表示此继承结构。 您可以对`Person`包括有关学生和教师单个表中的信息的表。 某些列可能仅适用于教师 (`HireDate`)，某些仅向学生 (`EnrollmentDate`)、 某些对这两个 (`LastName`， `FirstName`)。 通常，您将需要*鉴别器*指示哪种类型的每一行的列表示。 例如，鉴别器列可能有"教师"教师和"学生"学生版。

![每个 hierarchy_example 表](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

从单个数据库表生成的实体继承结构的此模式称为*表每个层次结构*(TPH) 继承。

一种替代方法是使看上去更像继承结构数据库。 例如，您可以对只有名称字段`Person`表，并具有单独`Instructor`和`Student`具有日期字段的表。

![每个 type_inheritance 表](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

此模式的数据库表，针对每个实体类称为进行*每种类型的表*(TPT) 继承。

TPH 继承模式通常因为 TPT 模式可能会导致复杂的联接查询在实体框架中比 TPT 继承模式提供更好的性能。 本教程演示如何实现 TPH 继承。 将执行以下步骤来执行该操作：

- 创建`Person`类并更改`Instructor`和`Student`类派生自`Person`。
- 将模型数据库映射代码添加到数据库上下文类。
- 更改`InstructorID`和`StudentID`在整个到项目的引用`PersonID`。

## <a name="creating-the-person-class"></a>创建 Person 类

 注意： 你将无法将项目编译后更新使用这些类的控制器前创建下面的类。 

在*模型*文件夹中，创建*Person.cs*和模板代码替换为以下代码：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

在*Instructor.cs*，派生`Instructor`类`Person`类和删除密钥和名称字段。 代码将类似于以下示例：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

进行类似更改到*Student.cs*。 `Student`类将类似于以下示例：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>添加到模型的 Person 实体类型

在*SchoolContext.cs*，添加`DbSet`属性`Person`实体类型：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

这是所有实体框架所需配置每个层次结构一个表继承。 当你将看到的当数据库可重新创建，它将具有`Person`表代替了`Student`和`Instructor`表。

## <a name="changing-instructorid-and-studentid-to-personid"></a>分别更改为 PersonID InstructorID 和 StudentID

在*SchoolContext.cs*，在教师课程映射语句中，更改`MapRightKey("InstructorID")`到`MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

此更改不是必需的;它仅更改多对多联接表中的 InstructorID 列的名称。 如果保留 InstructorID 形式的名称，则应用程序将仍能正常工作。 此处是完成*SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

接下来，你需要更改`InstructorID`到`PersonID`和`StudentID`到`PersonID`在整个项目***除***中的带时间戳迁移文件*迁移*文件夹。 若要执行该操作将查找和打开仅的文件，需要更改，然后在打开的文件上执行全局更改。 中的唯一文件*迁移*应更改的文件夹是*Migrations\Configuration.cs。*

1. > [!IMPORTANT]
 > 开始关闭 Visual Studio 中所有打开的文件。
2. 单击**查找和替换--查找所有文件**中**编辑**菜单上，，然后搜索的项目中包含的所有文件`InstructorID`。  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. 打开中的每个文件**查找结果**窗口***除***&lt;时间戳&gt;*\_.cs* 中的迁移文件*迁移*文件夹中，通过双击每个文件的一行。  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. 打开**在文件中替换**对话框和更改**查找**到**所有打开的文档**。
5. 使用**在文件中替换**对话框以更改所有`InstructorID`到`PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. 找到包含项目中的所有文件`StudentID`。
7. 打开中的每个文件**查找结果**窗口***除***&lt;时间戳&gt;*\_\*.cs*迁移文件在*迁移*文件夹中，通过双击每个文件的一行。  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. 打开**在文件中替换**对话框和更改**查找**到**所有打开的文档**。
9. 使用**在文件中替换**对话框以更改所有`StudentID`到`PersonID`。   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. 生成项目。

(请注意，此示例演示*缺点*的`classnameID`命名主键的模式。 如果你命名一样主键 ID，而无需前缀的类名称，*没有*重命名需要现在。)

## <a name="create-and-update-a-migrations-file"></a>创建和更新迁移文件

在包管理器控制台 (PMC) 中，输入以下命令：

`Add-Migration Inheritance`

运行`Update-Database`PMC 命令。 由于我们有迁移不知道如何处理的现有数据，该命令将在此时失败。 你收到以下错误：

*ALTER TABLE 语句与外键约束冲突"FK\_dbo。部门\_dbo。人员\_PersonID"。冲突发生于数据库"ContosoUniversity"表"dbo。Person"，列 PersonID。*

打开*迁移\&lt; 时间戳&gt;\_Inheritance.cs*和替换`Up`方法替换为以下代码：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

运行`update-database`试命令。

> [!NOTE]
> 很可能会迁移数据和进行架构更改时获得其他错误。 如果您迁移错误无法解决，可以通过更改中的连接字符串来继续本教程*Web.config*文件或删除数据库。 最简单方法是在数据库重命名*Web.config*文件。 例如，将数据库名称更改为 CU\_测试下面的示例中所示：
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> 与新数据库，没有数据迁移，和`update-database`命令是更可能完成且未发生错误。 有关如何删除数据库的说明，请参阅[如何从 Visual Studio 2012 中删除数据库](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)。 如果你接受这种方法，以继续本教程，请跳过部署步骤在本教程中，末尾因为已部署的站点将获取相同的错误，它自动运行迁移时。 如果你想要迁移错误的排查，最佳资源是实体框架论坛或 StackOverflow.com 之一。


## <a name="testing"></a>测试

运行站点并尝试各种页面。 一切相同像以前一样。

在**服务器资源管理器，**展开**SchoolContext**然后**表**，和你看到**学生**和**教师**已替换为表**人员**表。 展开**人员**表，你会看到它具有所有使用中的列**学生**和**教师**表。

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Person 表中，右键单击，然后单击**显示表数据**才能看到鉴别器列。

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

下图说明了新的 School 数据库的结构：

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>摘要

每个层次结构一个表继承现在已实现的`Person`， `Student`，和`Instructor`类。 有关此设置和其他的继承结构的详细信息，请参阅[继承映射策略](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx)Morteza Manavi 博客上。 在下一教程中，你将看到一些方式来实现的存储库和单元的工作模式。

在找不到其他实体框架资源的链接[ASP.NET 数据访问内容映射](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一页](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[下一页](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
