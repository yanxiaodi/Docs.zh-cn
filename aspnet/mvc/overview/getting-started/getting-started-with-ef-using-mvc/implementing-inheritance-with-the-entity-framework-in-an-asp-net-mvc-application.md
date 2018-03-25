---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: "在 ASP.NET MVC 5 应用程序 (12 的 11) 中实现继承，并使用实体框架 6 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 的 ASP.NET MVC 5 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 118233338112a71216b909b1dabed2333bfa235e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a>在 ASP.NET MVC 5 应用程序 (11 12 的) 实现与 Entity Framework 6 的继承
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下载 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


在前面的教程中，你将处理并发异常。 本教程将演示如何在数据模型中实现继承。

在面向对象编程中，你可以使用[继承](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming))便于[代码重用](http://en.wikipedia.org/wiki/Code_reuse)。 本教程中，你将更改`Instructor`和`Student`类，以使其从中派生`Person`基本类，其中包含属性，如`LastName`所共有的教师和学生。 不会添加或更改任何 web 页面，但你将更改某些代码并将在数据库中自动反映这些更改。

## <a name="options-for-mapping-inheritance-to-database-tables"></a>将继承映射到数据库表的选项

`Instructor`和`Student`中的类`School`数据模型具有相同的多个属性：

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

假设你想要消除冗余代码由共享的属性以`Instructor`和`Student`实体。 或者，你想要编写可以设置名称格式而无需管理名称来自一个教师或学生的服务。 你可以创建`Person`基类其中仅包含那些共享属性，然后进行`Instructor`和`Student`实体继承自该基类，如下面的插图中所示：

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

有几种方法无法在数据库中表示此继承结构。 您可以对`Person`包括有关学生和教师单个表中的信息的表。 某些列可能仅适用于教师 (`HireDate`)，某些仅向学生 (`EnrollmentDate`)、 某些对这两个 (`LastName`， `FirstName`)。 通常，您将需要*鉴别器*指示哪种类型的每一行的列表示。 例如，鉴别器列可能有"教师"教师和"学生"学生版。

![Table-per-hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

从单个数据库表生成的实体继承结构的此模式称为*表每个层次结构*(TPH) 继承。

一种替代方法是使看上去更像继承结构数据库。 例如，您可以对只有名称字段`Person`表，并具有单独`Instructor`和`Student`具有日期字段的表。

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

此模式的数据库表，针对每个实体类称为进行*每种类型的表*(TPT) 继承。

还有另一种方法是将所有的非抽象类型映射到单个表。 类，包括继承的属性的所有属性将都映射到相应的表的列。 此模式称为表每个具体类 (TPC) 继承。 如果实现的 TPC 继承`Person`， `Student`，和`Instructor`类更早版本，所示`Student`和`Instructor`表将与以前实现继承后的外观没有什么不同。

TPC 和 TPH 继承模式通常提供更好的性能在实体框架中比 TPT 继承模式，因为 TPT 模式可能在复杂的联接查询中。

本教程演示如何实现 TPH 继承。 TPH 是实体框架中中的默认继承模式，因此你所要做的就是创建`Person`类中，更改`Instructor`和`Student`类派生自`Person`，添加到新的类`DbContext`，并创建迁移。 (有关如何实现其他继承模式的信息，请参阅[映射的每种类型一个表 (TPT) 继承](https://msdn.microsoft.com/data/jj591617#2.5)和[映射表每个具体类 (TPC) 继承](https://msdn.microsoft.com/data/jj591617#2.6)MSDN 中实体框架文档。）

## <a name="create-the-person-class"></a>创建 Person 类

在*模型*文件夹中，创建*Person.cs*和模板代码替换为以下代码：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>将 Student 与教师从人员继承的类

在*Instructor.cs*，派生`Instructor`类`Person`类和删除密钥和名称字段。 代码将类似于以下示例：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

进行类似更改到*Student.cs*。 `Student`类将类似于以下示例：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a>将 Person 实体类型添加到模型

在*SchoolContext.cs*，添加`DbSet`属性`Person`实体类型：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

这是所有实体框架所需配置每个层次结构一个表继承。 当你将看到更新数据库时，它将具有`Person`表代替了`Student`和`Instructor`表。

## <a name="create-and-update-a-migrations-file"></a>创建和更新迁移文件

在包管理器控制台 (PMC) 中，输入以下命令：

`Add-Migration Inheritance`

运行`Update-Database`PMC 命令。 由于我们有迁移不知道如何处理的现有数据，该命令将在此时失败。 您收到以下错误消息：

> *无法删除对象 dbo。教师因为 FOREIGN KEY 约束引用它。*


打开*迁移\&lt; 时间戳&gt;\_Inheritance.cs*和替换`Up`方法替换为以下代码：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

此代码将负责以下数据库更新任务：

- 删除外键约束和指向学生表的索引。
- 重命名为人员教师表，并使它来存储学生数据所需的更改：

    - 将可以为 null EnrollmentDate 添加学生版。
    - 添加鉴别器列来指示行是否适用于一名学生或一个教师。
    - 使雇用日期可以为 null，因为学生行不会产生雇佣日期。
    - 添加临时的字段，将用于更新指向学生的外键。 当将学生复制到 Person 表他们将获得新的主键值。
- 将数据从 Person 表到学生表。 这将导致学生以分配新的主键值。
- 修复了指向学生的外键值。
- 重新创建外键约束和索引，现在将它们指向 Person 表。

（如果你在作为主键类型使用而不是整数的 GUID，学生主键值不需要更改，并且这些步骤的几个可能已被省略。）

运行`update-database`试命令。

（在生产系统中你将进行相应更改 Down 方法以防你曾必须使用，以返回到以前的数据库版本。 本教程中你将不使用 Down 方法。）

> [!NOTE]
> 很可能会迁移数据和进行架构更改时获得其他错误。 如果您迁移错误无法解决，可以通过更改中的连接字符串来继续本教程*Web.config*文件或通过删除数据库。 最简单方法是在数据库重命名*Web.config*文件。 例如，将数据库名称更改为 ContosoUniversity2，如下面的示例中所示：
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> 与新数据库，没有数据迁移，和`update-database`命令是更可能完成且未发生错误。 有关如何删除数据库的说明，请参阅[如何从 Visual Studio 2012 中删除数据库](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)。 如果你接受这种方法，以继续本教程，请跳过部署步骤，在本教程末尾，或将部署到新站点和数据库。 如果将更新部署到您已部署到已在同一站点时，EF 会那里相同的错误时它自动运行迁移。 如果你想要迁移错误的排查，最佳资源是实体框架论坛或 StackOverflow.com 之一。


## <a name="testing"></a>正在测试

运行站点并尝试各种页面。 一切相同像以前一样。

在**服务器资源管理器，**展开**数据 Connections\SchoolContext**然后**表**，和你看到**学生**和**教师**已替换为表**人员**表。 展开**人员**表，你会看到它具有所有使用中的列**学生**和**教师**表。

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Person 表中，右键单击，然后单击**显示表数据**才能看到鉴别器列。

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

下图说明了新的 School 数据库的结构：

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>将部署到 Azure

此部分要求你已完成可选**将应用部署到 Azure**主题中[第 3 部分，排序、 筛选和分页](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)本系列教程。 如果必须解决通过删除本地项目中的数据库的迁移错误，跳过此步骤;或创建新站点和数据库，并部署到新的环境。

1. 在 Visual Studio 中，右键单击中的项目**解决方案资源管理器**和选择**发布**从上下文菜单。  
  
    ![在项目上下文菜单中发布](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. 单击“发布” 。  
  
    ![publish](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
 Web 应用将在默认浏览器中打开。
3. 测试应用程序以验证它是否正常工作。

    第一次运行某个页面访问数据库，实体框架将运行所有迁移`Up`使数据库保持最新的当前数据模型所需的方法。

## <a name="summary"></a>摘要

你已实现的每个层次结构一个表继承`Person`， `Student`，和`Instructor`类。 有关此设置和其他的继承结构的详细信息，请参阅[TPT 继承模式](https://msdn.microsoft.com/data/jj618293)和[TPH 继承模式](https://msdn.microsoft.com/data/jj618292)MSDN 上。 在下一步的教程中，你将看到如何处理各种相对较高级的实体框架方案。

在找不到其他实体框架资源的链接[ASP.NET 数据访问的推荐资源](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一页](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[下一页](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
