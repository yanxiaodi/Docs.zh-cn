---
title: "ASP.NET Core MVC 和 EF Core - 继承 - 第 9 个教程（共 10 个）"
author: tdykstra
description: "本教程将演示如何使用 ASP.NET Core 应用程序中的 Entity Framework Core 在数据模型中实现继承。"
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 985cc38b10ef830b8274e40ad5f7050157fd4d86
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2018
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a>继承 - EF Core 和 ASP.NET Core MVC 教程（第 9 个，共 10 个）
作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University 示例 Web 应用程序演示如何使用 Entity Framework Core 和 Visual Studio 创建 ASP.NET Core MVC Web 应用程序。 若要了解教程系列，请参阅[本系列中的第一个教程](intro.md)。

在上一个教程中，已经处理了并发异常。 本教程将演示如何在数据模型中实现继承。

在面向对象的编程中，可以使用继承以便于重用代码。 在本教程中，将更改 `Instructor` 和 `Student` 类，以便从 `Person` 基类中派生，该基类包含教师和学生所共有的属性（如 `LastName`）。 不会添加或更改任何网页，但会更改部分代码，并将在数据库中自动反映这些更改。

## <a name="options-for-mapping-inheritance-to-database-tables"></a>将继承映射到数据库表的选项

学校数据模型中的 `Instructor` 和 `Student` 类具有多个相同的属性：

![Student 和 Instructor 类](inheritance/_static/no-inheritance.png)

假设想要消除由 `Instructor` 和`Student` 实体共享的属性的冗余代码。 或者想要写入可以格式化姓名的服务，而无需关注该姓名来自教师还是学生。 可以创建只包含这些共享属性的 `Person` 基类，然后使 `Instructor` 和 `Student` 类继承该基类，如下图所示：

![从 Person 类派生的 Student 和 Instructor 类](inheritance/_static/inheritance.png)

有多种方法可以在数据库中表示此继承结构。 可以创建一个 Person 表，将学生和教师的相关信息包含在一个表中。 某些列可能仅适用于教师 (HireDate)，某些列仅适用于学生 (EnrollmentDate)，某些列同时适用于两者（LastName、FirstName）。 通常情况下，将有一个鉴别器列来指示每行所代表的类型。 例如，鉴别器列可能包含“Instructor”来指示教师，包含“Student”来指示学生。

![每个层次结构一张表示例](inheritance/_static/tph.png)

从单个数据库表生成实体继承结构的模式称为每个层次结构一张 (TPH) 继承。

另一种方法是使数据库看起来更像继承结构。 例如，可以仅将姓名字段包含到 Person 表中，在单独的 Instructor 和 Student 表中包含日期字段。

![每种类型一个表继承](inheritance/_static/tpt.png)

为每个实体类创建数据库表的模式称为每个类型一张表 (TPT) 继承。

另一种方法是将所有非抽象类型映射到单独的表。 类的所有属性（包括继承的属性）映射到相应表的列。 此模式称为每个具体类一张表 (TPC) 继承。 如果为前面所示的 Person、Student 和 Instructor 类实现了 TPC 继承，那么在实现继承之后，Student 和 Instructor 表看起来将与以前没什么不同。

TPC 和 TPH 继承模式的性能通常比 TPT 继承模式好，因为 TPT 模式会导致复杂的联接查询。

本教程将演示如何实现 TPH 继承。 TPH 是 Entity Framework Core 唯一支持的继承模式。  需要执行的操作是创建 `Person` 类、将 `Instructor` 和 `Student` 类更改为从 `Person` 派生、将新的类添加到 `DbContext`，以及创建迁移。

> [!TIP] 
> 在进行以下更改之前，请考虑保存项目的副本。  如果遇到问题并需要重新开始，可以更轻松地从已保存的项目开始，而不用反向操作本教程中的步骤或者返回到整个系列的开始。

## <a name="create-the-person-class"></a>创建 Person 类

在 Models 文件夹中，创建 Person.cs 并使用以下代码替换模板代码：

[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>使 Student 和 Instructor 类从 Person 继承

在 Instructor.cs 中，从 Person 类派生 Instructor 类并删除键和姓名字段。 代码将如下所示：

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

在 Student.cs 中做出相同更改。

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a>将 Person 实体类型添加到数据模型

将 Person 实体类型添加到 SchoolContext.cs。 新的行突出显示。

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

以上是 Entity Framework 配置每个层次结构一张表继承所需的全部操作。 正如将看到的，更新数据库时，将有一个 Person 表来代替 Student 和 Instructor 表。

## <a name="create-and-customize-migration-code"></a>创建和自定义迁移代码

保存更改并生成项目。 随后在项目文件夹中打开命令窗口并输入以下命令：

```console
dotnet ef migrations add Inheritance
```

暂不运行 `database update` 命令。 该命令将导致数据丢失，因为它将删除 Instructor 表并将 Student 表重命名为 Person。 需要提供自定义代码来保留现有数据。

打开 Migrations/\<timestamp>_Inheritance.cs 并使用以下代码替换 `Up` 方法：

[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

此代码负责以下数据库更新任务：

* 删除指向 Student 表的外键约束和索引。

* 将 Instructor 表重命名为 Person，根据需要做出更改以存储学生数据：

* 为学生添加可为 NULL 的 EnrollmentDate。

* 添加鉴别器列来指示行代表学生还是教师。

* HireDate 可为 NULL，因为学生行不会包含聘用日期。

* 添加临时字段，用于更新指向学生的外键。 将学生复制到 Person 表时，将获取新的主键值。

* 将数据从 Student 表复制到 Person 表。 这将使学生获取分配的新主键值。

* 修复指向学生的外键值。

* 重新创建外键约束和索引，现在将它们指向 Person 表。

（如果已使用 GUID 而不是整数作为主键类型，那么将不需要更改学生主键值，并且可能已省略其中多个步骤。）

运行 `database update` 命令：

```console
dotnet ef database update
```

（在生产系统中，可以对 `Down` 方法进行相应更改，以防必须使用该方法返回到以前的数据库版本。 本教程中将不使用 `Down` 方法。）

> [!NOTE] 
> 在包含现有数据的数据库中更改架构时，可能会发生其他错误。 如果出现无法解决的迁移错误，可以在连接字符串中更改数据库名或者删除数据库。 若是新数据库，则没有要迁移的数据，因此在完成更新数据库命令时很可能不会出错。 若要删除数据库，请使用 SSOX 或运行 `database drop` CLI 命令。

## <a name="test-with-inheritance-implemented"></a>使用已实现的继承进行测试

运行应用并尝试各种页面。 一切都和以前一样。

在“SQL Server 对象资源管理器” 中，展开“数据连接/SchoolContext”和“表”，将看到 Student 和 Instructor 表已替换为 Person 表。 打开 Person 表设计器，将看到它包含在 Student 和 Instructor 表中使用的所有列。

![SSOX 中的 Person 表](inheritance/_static/ssox-person-table.png)

右键单击 Person 表，然后单击“显示表数据”以查看鉴别器列。

![SSOX 中的 Person 表 - 表数据](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a>摘要

你已经为 `Person`、`Student` 和 `Instructor` 类实现了每个层次结构一张表继承。 若要详细了解 Entity Framework Core 中的继承，请参阅[继承](https://docs.microsoft.com/ef/core/modeling/inheritance)。 下一个教程将介绍如何处理各种相对高级的 Entity Framework 方案。

>[!div class="step-by-step"]
[上一页](concurrency.md)
[下一页](advanced.md)  
