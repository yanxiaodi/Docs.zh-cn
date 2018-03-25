---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: "在 ASP.NET MVC 应用程序 (9 的 10) 中实现的存储库和单元的工作模式 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 02b1de31b9513247facc92bc6b72247865d176f9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>在 ASP.NET MVC 应用程序 (9 的 10) 中实现的存储库和单元的工作模式
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 应用程序。 有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 你可以从头开始教程系列或[下载这一章的初学者项目](building-the-ef5-mvc4-chapter-downloads.md)和从这里开始。
> 
> > [!NOTE] 
> > 
> > 如果你遇到无法解决的问题[下载已完成的章](building-the-ef5-mvc4-chapter-downloads.md)并尝试重现你的问题。 你通常可以通过比较你的代码已完成的代码会发现问题的解决方案。 一些常见的错误和如何解决它们，请参阅[错误和解决方法。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


在以前的教程，你用于继承减少冗余代码中的`Student`和`Instructor`实体类。 在本教程中，你将看到的 CRUD 操作中使用的存储库和单元的工作模式的一些方法。 如前面的教程，在此将更改你的代码适用于页你已创建而不是创建新的页的方式。

## <a name="the-repository-and-unit-of-work-patterns"></a>存储库和单元的工作模式

存储库和单元的工作模式旨在创建数据访问层和应用程序的业务逻辑层之间的抽象层。 实现这些模式可帮助防止你的应用程序数据存储区中的更改，而且很容易自动的单元测试驱动开发 (TDD)。

在本教程中，你将实施每个实体类型的存储库类。 有关`Student`你将创建存储库接口和存储库类的实体类型。 当你的控制器中实例化存储库时，你将使用该接口，以便控制器将接受对任何实现存储库接口的对象的引用。 当控制器运行在 web 服务器下时，它将接收适用于实体框架的存储库。 当控制器运行下一个单元测试类时，它将接收适用于你可以轻松地操作进行测试，例如，内存中集合的方式存储数据的存储库。

将稍后在本教程中使用多个存储库和单元的工作类`Course`和`Department`中的实体类型`Course`控制器。 工作类的单元通过创建单个数据库上下文类由所有这些共享协调多个存储库的工作。 如果你想要能够执行自动的单元测试，您可以创建并使用这些类的接口，与用于相同的方式`Student`存储库。 但是，为了简化本教程，将创建并没有接口的情况下使用这些类。

下图显示了一种方法有助于概念化控制器和上下文类相比于不在任何使用的存储库或单元的工作模式之间的关系。

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

在本系列教程中，不会创建单元测试。 MVC 应用程序使用的存储库模式 TDD 的介绍，请参阅[演练： 使用 ASP.NET MVC 使用 TDD](https://msdn.microsoft.com/library/ff847525.aspx)。 有关存储库模式的详细信息，请参阅以下资源：

- [存储库模式](https://msdn.microsoft.com/library/ff649690.aspx)MSDN 上。
- [使用实体框架 4.0 中使用存储库和单元的工作模式](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)实体框架团队博客上。
- [敏捷的实体框架 4 存储库](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/)系列 Julie Lerman 博客上的文章。
- [生成快速 HTML5/jQuery 应用程序的帐户](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx)Dan Wahlin 博客上。

> [!NOTE]
> 有多种方法来实现的存储库和单元的工作模式。 你可以使用存储库类，具有或不工作类的一个单元。 你可以实现所有实体类型，或另一个用于每种类型的单个存储库。 如果你实现一个用于每种类型，你可以使用单独的类、 泛型基类和派生的类中，或一个抽象基类和派生的类。 你可以在你的存储库中包含业务逻辑或限制对数据访问逻辑。 你也可以生成一个抽象层入数据库上下文类通过[IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx)存在接口而不是[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx)实体集的类型。 实现在本教程中所示的抽象层的方法是你需要考虑，不建议所有方案和环境的一个选项。


## <a name="creating-the-student-repository-class"></a>创建学生存储库类

在*DAL*文件夹中，创建一个名为的类文件*IStudentRepository.cs*和替换为以下代码替换现有代码：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

此代码声明了一组典型的 CRUD 方法，包括两个读取的方法-一个返回所有`Student`实体和查找单个的一个`Student`实体的 id。

在*DAL*文件夹中，创建一个名为的类文件*StudentRepository.cs*文件。 将现有代码替换为以下代码，实现`IStudentRepository`接口：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

在类变量中，定义的数据库上下文和构造函数需要调用对象上下文的实例中传递：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

无法实例化新的上下文中存储库，但如果你使用一个控制器中的多个存储库，每个将最终会与单独的上下文。 稍后你将使用中的多个存储库`Course`控制器，你将看到一套工作类可以确保所有存储库使用相同的上下文。

存储库实现[IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx)并释放数据库上下文，你在前面看到控制器上，以及其 CRUD 方法前面所看到的相同方式进行调用的数据库上下文。

## <a name="change-the-student-controller-to-use-the-repository"></a>更改为使用存储库的学生控制器

在*StudentController.cs*，当前类中的代码替换为以下代码。 突出显示所做的更改。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

控制器现在声明实现的对象的类变量`IStudentRepository`而不是上下文类的接口：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

默认 （无参数） 构造函数创建新的上下文实例，和一个可选的构造函数允许调用方传入的上下文实例。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(如果你使用*依赖关系注入*，或 DI，你不需要默认构造函数因为 DI 软件将确保始终将提供正确的存储库对象。)

在 CRUD 方法中，而非上下文现在称为存储库：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

与`Dispose`方法立即释放而非上下文存储库：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

运行站点，然后单击**学生**选项卡。

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

页查找，并像之前更改代码以使用存储库，并且其他学生页也适用相同的操作方式相同。 但是，没有方式的一项重大差异`Index`控制器方法执行筛选和排序。 此方法的原始版本包含以下代码：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

已更新`Index`方法包含以下代码：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

仅突出显示的代码已更改。

该代码的原始版本中`students`被类型化为`IQueryable`对象。 查询不发送到数据库中，直到它被转换为使用一种方法，如集合`ToList`，这不会出现在之前的索引视图访问学生模型。 `Where`在上面的原始代码的方法将成为`WHERE`发送到数据库的 SQL 查询中的子句。 这反过来意味着仅所选的实体由数据库。 但是，由于更改`context.Students`到`studentRepository.GetStudents()`、`students`变量后此语句是`IEnumerable`包含所有学生在数据库中的集合。 应用的最终结果`Where`方法相同，但现在在 web 服务器上而不是数据库的内存中完成工作。 对于返回大量数据的查询，这可能效率低下。

> [!TIP] 
> 
> **IQueryable vs。IEnumerable**
> 
> 实现存储库如下所示，即使输入中的一些东西后**搜索**框发送给 SQL Server 的查询返回所有学生行，因为它不包括您的搜索条件：
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> 因为存储库中执行查询，而不必知道有关搜索条件，此查询将返回所有学生数据。 排序、 应用搜索条件，以及选择对应的数据子集 （在这种情况下显示只需 3 行） 的分页在内存中完成的过程更高版本在`ToPagedList`方法调用`IEnumerable`集合。
> 
> 在以前版本的代码 （之前实现存储库中），查询不发送到数据库之前应用搜索条件之后, 时`ToPagedList`上调用`IQueryable`对象。
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> 当上调用 ToPagedList`IQueryable`对象，查询发送到 SQL Server 指定搜索字符串中，和作为结果返回满足搜索条件的行，并且没有筛选需要在内存中完成。
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> （以下教程说明如何检查查询发送到 SQL Server）。


以下部分演示如何实现存储库，可以指定由数据库应进行这项工作的方法。

你现在已创建控制器和实体框架数据库上下文之间的抽象层。 如果你已会执行自动的单元测试与此应用程序，你可以实现的单元测试项目中创建一个备用的存储库类`IStudentRepository` *。* 而不是调用的上下文以读取和写入数据，此模拟的存储库类可能会为了测试控制器函数操作内存中的集合。

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>实现泛型的存储库和单元的工作类

创建每个实体类型的存储库类可能会导致大量的冗余代码，并且它可能导致部分更新。 例如，假设你需要更新作为同一事务的一部分的两个不同的实体类型。 如果每个使用单独的数据库上下文实例，一个可能成功，并且其他可能会失败。 最大程度减少冗余代码的一种方法是使用泛型的存储库，并确保一种方法，所有存储库使用相同的数据库上下文 （并因此协调所有更新） 是使用一套工作类。

在本教程的本部分中，你将创建`GenericRepository`类和一个`UnitOfWork`类，并使用它们在`Course`控制器同时访问`Department`和`Course`实体集。 如前面所述，为了简化本教程的此部分，你不创建这些类的接口。 但如果你已将使用它们来促进 TDD，你将通常实现它们与接口相同的方式`Student`存储库。

### <a name="create-a-generic-repository"></a>创建泛型的存储库

在*DAL*文件夹中，创建*GenericRepository.cs*和替换为以下代码替换现有代码：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

数据库上下文和存储库为实例化的实体集，声明类变量：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

构造函数接受的数据库上下文实例并初始化实体设置变量：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

`Get`方法使用 lambda 表达式来允许调用的代码指定的筛选器条件和通过，对结果进行排序的列和字符串参数允许调用方为预先加载提供以逗号分隔的导航属性的列表：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

代码`Expression<Func<TEntity, bool>> filter`意味着调用方将提供 lambda 表达式基于`TEntity`类型和此表达式将返回一个布尔值。 例如，如果存储库为实例化`Student`实体类型调用方法中的代码可能会指定`student => student.LastName == "Smith`&quot;为`filter`参数。

代码`Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy`也意味着调用方将提供 lambda 表达式。 但在这种情况下，该表达式的输入是`IQueryable`对象`TEntity`类型。 该表达式将返回的有序的版本`IQueryable`对象。 例如，如果存储库为实例化`Student`实体类型调用方法中的代码可能会指定`q => q.OrderBy(s => s.LastName)`为`orderBy`参数。

中的代码`Get`方法创建`IQueryable`对象，然后将应用筛选器表达式，如果有一个：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

接下来，它分析的逗号分隔列表后应用 eager 加载表达式：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

最后，它采用`orderBy`如果有一个表达式并返回结果; 否则它将返回结果从无序的查询：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

当调用`Get`方法，你可以执行操作筛选和排序`IEnumerable`由而不是提供这些函数的参数的方法返回的集合。 但排序和筛选工作然后应该在 web 服务器上的内存中。 通过使用这些参数，可以确保工作由数据库而不是 web 服务器完成。 一种替代方法是创建针对特定实体类型的派生的类并添加专用`Get`方法，如`GetStudentsInNameOrder`或`GetStudentsByName`。 但是，在复杂应用程序，这可以导致大量的此类派生的类和专业化的方法，这可能是多工作以维护。

中的代码`GetByID`， `Insert`，和`Update`methods 是类似于你在非泛型存储库中看到的内容。 (不提供中的预先加载参数`GetByID`签名，因为不能使用的预先加载`Find`方法。)

两个重载为提供`Delete`方法：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

只需在 ID 中的实体要删除、 传递这些允许之一，且其中一个使用实体实例。 正如你在看到[处理并发](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)教程，并发处理你需要的`Delete`采用实体实例的方法包括跟踪属性的原始值。

此泛型的存储库将处理典型 CRUD 要求。 当特定实体类型具有特殊要求，如更复杂的筛选或排序，你可以创建具有该类型的其他方法的派生的类。

## <a name="creating-the-unit-of-work-class"></a>创建工作类的单元

单元工作类的一个用途： 若要确保当你使用多个存储库，它们共享单个数据库上下文。 这样一来，完成的工作单元时可以调用`SaveChanges`上下文的该实例上的方法并确保所有相关的更改都将协调。 所有这些类需求是`Save`方法和每个存储库的属性。 每个存储库属性返回已与其他存储库实例使用相同的数据库上下文实例实例化的存储库实例。

在*DAL*文件夹中，创建一个名为的类文件*UnitOfWork.cs*和模板代码替换为以下代码：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

该代码创建的数据库上下文和每个存储库的类变量。 有关`context`实例化的变量，新的上下文是：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

每个存储库属性检查是否已存在存储库。 否则，它实例化的存储库，并传入上下文实例。 因此，所有存储库共享同一上下文实例。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

`Save`方法调用`SaveChanges`上的数据库上下文。

与实例化数据库上下文在类变量中，任何类一样`UnitOfWork`类实现`IDisposable`并释放上下文。

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>更改用于 UnitOfWork 类和存储库的课程控制器

你目前在设置的代码替换*CourseController.cs*替换为以下代码：

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

此代码将添加的类变量`UnitOfWork`类。 (如果已使用接口，此处所示，不会初始化这里的变量; 相反，你会实施两个构造函数的模式，就像未`Student`存储库。)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

类的其余部分，对数据库上下文的所有引用替换都为相应的存储库，对引用使用`UnitOfWork`属性访问存储库。 `Dispose`方法会释放`UnitOfWork`实例。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

运行站点，然后单击**课程**选项卡。

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

页查找并确实执行此操作之前所做的更改，以及其他课程页也适用相同的操作方式相同。

## <a name="summary"></a>摘要

你现在已实现的存储库和单元的工作模式。 你为泛型存储库中的方法参数中使用 lambda 表达式。 有关如何使用与这些表达式的详细信息`IQueryable`对象，请参阅[IQueryable(T) 接口 (System.Linq)](https://msdn.microsoft.com/library/bb351562.aspx) MSDN 库中。 在下一个教程中，你将了解如何处理某些高级方案中。

在找不到其他实体框架资源的链接[ASP.NET 数据访问内容映射](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一页](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[下一页](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
