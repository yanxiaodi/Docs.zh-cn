---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: "处理与实体框架 4.0 ASP.NET 4 Web 应用程序中的并发 |Microsoft 文档"
author: tdykstra
description: "本教程系列上的 Contoso 大学 web 应用程序创建的 Getting Started with 实体 Framework 4.0 教程系列生成。 我..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 7bdcf610458631749531ed1279d27e90572f0371
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>处理与实体框架 4.0 ASP.NET 4 Web 应用程序中的并发
====================
通过[Tom Dykstra](https://github.com/tdykstra)

> 本教程系列上的 Contoso 大学 web 应用程序创建的生成[Getting Started with 实体 Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started)教程系列。 如果未完成前面的教程，作为一个起始点本教程，你可以[下载应用程序](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)，你应已创建。 你还可以[下载应用程序](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)，它由完整教程系列创建。 如果你有疑问的教程，你可以发布到[ASP.NET 实体框架论坛](https://forums.asp.net/1227.aspx)。


在以前的教程，您学习了如何进行排序和筛选器数据使用`ObjectDataSource`控件和 Entity Framework。 本教程演示了用于处理并发的 ASP.NET web 应用程序使用实体框架中的选项。 你将创建一个新的网页，专用于更新教师办公室分配。 将处理并发问题在该页以及你之前创建部门页。

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>并发冲突

一个用户编辑一个记录，并在第一个用户的更改写入到数据库之前，另一用户编辑同一个记录时发生并发冲突。 如果不设置实体框架以检测此类冲突，任何人都更新数据库上次将覆盖其他用户的更改。 在许多应用程序，这种风险很可以接受的并无需配置应用程序能够处理可能并发冲突。 (如果有很多用户或几个更新，或如果并不真正重要的一些更改将被覆盖，如果并发编程的开销可能超过带来的好处。)如果你不需要担心并发冲突，则可以跳过本教程;序列中的其余两个教程不依赖于在此生成的任何内容。

### <a name="pessimistic-concurrency-locking"></a>保守式并发 （锁定）

如果你的应用程序需要防止并发方案中的意外数据丢失，做到这一点的一种方法是使用数据库锁。 这称为*保守式并发*。 例如，从数据库读取行之前，你请求的锁只读的或更新访问权限。 如果锁定的行更新访问权限时，不允许任何其他用户锁定该行为只读的或更新访问权限，因为它们将获取正在进行更改的数据的副本。 如果锁定用于只读访问的行，其他人可以还将其锁定用于只读访问权限但不是用于更新。

管理锁具有某些缺点。 它可以是复杂到程序。 它需要大量的数据库管理资源，并且可能导致性能问题的应用程序的用户数会增加 （即，它不能很好）。 出于这些原因，不是所有数据库管理系统都支持保守式并发。 本教程不向你展示如何实现它，并且实体框架提供，没有内置支持。

### <a name="optimistic-concurrency"></a>开放式并发

保守式并发替代方法是*开放式并发*。 开放式并发意味着允许并发冲突发生，然后相应地反应，如果他们这样做。 例如，John 运行*Department.aspx*页上，单击**编辑**链接历史记录部门，并减少**预算**到 $ 从 $ 1000，000.00 量125,000.00。 （John 管理竞争部门并且想要释放自己部门 money。）

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

John 单击之前**更新**，Jane 运行同一页上，单击**编辑**历史系，然后更改链接**开始日期**字段从 2011 年 1 月 10 日到 1/1 /1999。 （Jane 管理历史系并且想要为其提供详细高低。）

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

John 单击**更新**第一次，然后 Jane 单击**更新**。 Jane 的浏览器现在列表**预算**作为 $ 1000，000.00，但这样的量不正确，因为已由 John 到 $125,000.00 更改量。

某些可在此方案中执行的操作包括：

- 你可以跟踪的用户进行了修改的属性，并更新仅在数据库中的相应列。 在示例方案中，任何数据将不会丢失，因为不同的属性已更新由两个用户。 下一次有人浏览历史记录部门，他们将看到 1/1/1999年和 $125,000.00。 

    这是实体框架中中的默认行为，它可以显著减少可能导致数据丢失的冲突数。 但是，此行为不避免数据丢失，如果实体的同一属性进行竞争的更改。 此外，此行为并不总是可行;当存储的过程映射到的实体类型时，所有实体的属性更新数据库中进行任何更改到实体时。
- 你可以让 Jane 的更改覆盖 John 的更改。 Jane 单击后**更新**、**预算**量将返回到 1000，000.00 $。 这称为*客户端优先*或*在 Wins 中最后一个*方案。 （客户端的值优先于什么是在数据存储。）
- 你可以防止在数据库中正在更新 Jane 的更改。 通常情况下，将显示一条错误消息、 她显示的数据的当前状态和让她可以如果她仍然想要对它们进行重新输入其更改。 通过将保存其输入和为她提供机会重新将其应用而无需重新输入它，你无法进一步自动执行的过程。 这称为*存储 Wins*方案。 （数据存储值优先于提交的客户端的值。）

### <a name="detecting-concurrency-conflicts"></a>检测并发冲突

在实体框架中，您可以通过处理中解决冲突`OptimisticConcurrencyException`实体框架引发的异常。 为了知道何时引发这些异常，实体框架必须能够检测到冲突。 因此，你必须从数据库和数据模型正确配置。 有关启用冲突检测某些选项包括：

- 在数据库中，包括可以用于确定何时已更改行的表列。 然后，你可以配置实体框架可以包括中的列`Where`的 SQL 子句`Update`或`Delete`命令。

    目的就在于`Timestamp`中的列`OfficeAssignment`表。

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    数据类型`Timestamp`列也被称为`Timestamp`。 但是，列实际上不包含日期或时间值。 相反，值是每次更新行时递增序列号。 在`Update`或`Delete`命令，`Where`子句包含原始`Timestamp`值。 如果正在更新的行已由其他用户时中的值更改`Timestamp`不同于原始值，因此`Where`子句将返回要更新的行。 在实体框架查找由当前已更新了任何行`Update`或`Delete`命令 （即，在受影响的行数为零） 时，它将，解释为并发冲突。
- 配置实体框架可以包括在表中的每个列的原始值`Where`子句`Update`和`Delete`命令。

    如下所示的第一个选项，如果自第一次读取一行，行中的任何内容已更改`Where`子句不会返回的行，若要更新，该实体框架会将解释为并发冲突。 此方法是尽可能使用更有效`Timestamp`字段，但可能效率很低。 对于包含许多列的数据库表，它会导致非常大`Where`子句，并在 web 应用程序也可能要求您维护大量的状态。 维护大量的状态会影响应用程序性能，因为它需要服务器资源 （例如，会话状态） 或必须包含在 web 页本身 （例如，视图状态）。

在本教程中，你将添加错误处理不具有跟踪属性的实体的开放式并发冲突 (`Department`实体) 和实体跟踪属性 (`OfficeAssignment`实体)。

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>不跟踪属性的情况下处理开放式并发

若要实现的乐观并发`Department`实体，没有一个跟踪 (`Timestamp`) 属性，你将完成以下任务：

- 更改数据模型以启用跟踪的并发`Department`实体。
- 在`SchoolRepository`类中的句柄并发异常`SaveChanges`方法。
- 在*Departments.aspx*页上，通过向用户警告所尝试的更改没有成功显示一条消息的处理并发异常。 然后，用户可以查看的当前值，然后重试所做的更改，如果仍然需要它们。

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>启用跟踪数据模型中的并发

在 Visual Studio 中，打开 Contoso 大学 web 应用程序已使用在上一本系列教程中的。

打开*SchoolModel.edmx*，然后在数据模型设计器中，右键单击`Name`中的属性`Department`实体，然后单击**属性**。 在**属性**窗口中，更改`ConcurrencyMode`属性`Fixed`。

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

为其他非主键标量属性执行相同的 (`Budget`， `StartDate`，和`Administrator`。)（你无法执行此操作用于导航属性。）这将指定，只要实体框架生成`Update`或`Delete`SQL 命令以更新`Department`（与原始值） 的这些列必须包含在数据库中的实体，`Where`子句。 如果未不找到任何行`Update`或`Delete`命令执行，实体框架将引发乐观并发异常。

保存并关闭数据模型。

### <a name="handling-concurrency-exceptions-in-the-dal"></a>在 DAL 处理并发异常

打开*SchoolRepository.cs*并添加以下`using`语句`System.Data`命名空间：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

添加以下新`SaveChanges`方法，用于处理乐观并发异常：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

如果调用此方法时，将发生并发错误，在内存中的实体的属性值被替换为数据库中的当前值。 以便网页上可以处理它，则将被重新引发并发异常。

在`DeleteDepartment`和`UpdateDepartment`方法，将替换现有调用`context.SaveChanges()`通过调用`SaveChanges()`以便调用新方法。

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>表示层中的处理并发异常

打开*Departments.aspx*并添加`OnDeleted="DepartmentsObjectDataSource_Deleted"`属性设为`DepartmentsObjectDataSource`控件。 控件的开始标记现在将类似于下面的示例。

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

在`DepartmentsGridView`控制，指定表的列中的所有`DataKeyNames`特性，如下面的示例中所示。 请注意，这将创建非常大的视图状态字段，这是一个原因使用的跟踪字段的原因通常是跟踪并发冲突的首选的方法。

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

打开*Departments.aspx.cs*并添加以下`using`语句`System.Data`命名空间：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

添加以下新方法，将从数据源控件的调用`Updated`和`Deleted`事件处理程序处理并发异常：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

此代码检查异常类型，同时并发异常是否，代码将动态创建`CustomValidator`反过来显示一条消息中的控件`ValidationSummary`控件。

调用中的新方法`Updated`前面添加的事件处理程序。 此外，创建一个新`Deleted`事件处理程序调用同一方法 （但不执行任何其他操作）：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>在部门页中测试开放式并发

运行*Departments.aspx*页。

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

单击**编辑**行中更改中的值和**预算**列。 (请记住，你只能编辑记录在创建供本教程中，因为现有`School`数据库记录包含一些无效的数据。 经济性部门的记录是安全的一个试用。）

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

打开新的浏览器窗口并运行页再次 （复制第二个浏览器窗口从第一个浏览器窗口的地址框中的 URL）。

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

单击**编辑**在同一行你前面编辑和更改**预算**到的其他内容的值。

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

在第二个浏览器窗口中，单击**更新**。 **预算**量成功更改为此新值。

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

在第一个浏览器窗口中，单击**更新**。 更新失败。 **预算**量将重新显示使用在第二个浏览器窗口中，设置的值，你看到一条错误消息。

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>使用跟踪属性的处理开放式并发

若要处理的具有跟踪属性的实体的开放式并发，你将完成以下任务：

- 将存储的过程添加到数据模型来管理`OfficeAssignment`实体。 （跟踪属性和存储的过程不需要一起使用; 它们只是分组在一起此处是为了进行说明。）
- 将 CRUD 方法添加到 DAL 和为 BLL`OfficeAssignment`实体，包括代码来处理中 DAL 的开放式并发异常。
- 创建一个办公室分配 web 页面。
- 测试新的 web 页中的开放式并发。

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>添加 OfficeAssignment 存储到数据模型的过程

打开*SchoolModel.edmx*文件在模型设计器中，右键单击设计图面，然后单击**从数据库更新模型**。 在**添加**选项卡**选择数据库对象**对话框框中，展开**存储过程**，然后选择这三种`OfficeAssignment`存储过程 (请参阅以下屏幕截图），然后单击**完成**。 （这些存储的过程时，已在数据库中下载或使用脚本进行创建。）

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

右键单击`OfficeAssignment`实体，然后选择**存储过程映射**。

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

设置**插入**，**更新**，和**删除**函数用于及其相应的存储过程。 有关`OrigTimestamp`参数`Update`函数中，设置**属性**到`Timestamp`和选择**使用原始值**选项。

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

当实体框架调用`UpdateOfficeAssignment`存储的过程，它将通过的原始值`Timestamp`中的列`OrigTimestamp`参数。 存储的过程使用此参数在其`Where`子句：

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

存储的过程还将选择的新值`Timestamp`更新后的列，以便能够获得实体框架`OfficeAssignment`与相应的数据库行同步是在内存中的实体。

(请注意，删除一个办公室分配的存储的过程不具有`OrigTimestamp`参数。 因此，实体框架无法验证实体不删除它之前变。）

保存并关闭数据模型。

### <a name="adding-officeassignment-methods-to-the-dal"></a>添加与 DAL OfficeAssignment 方法

打开*ISchoolRepository.cs*并添加以下用于 CRUD 方法`OfficeAssignment`实体集：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

添加到以下新方法*SchoolRepository.cs*。 在`UpdateOfficeAssignment`方法时，你可以调用本地`SaveChanges`方法而不是`context.SaveChanges`。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

在测试项目中，打开*MockSchoolRepository.cs*并添加以下`OfficeAssignment`集合和向其 CRUD 方法。 （模拟的存储库必须实现存储库的接口，或将不进行编译解决方案）。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>为 BLL 添加 OfficeAssignment 方法

在主项目中，打开*SchoolBL.cs*并添加以下用于 CRUD 方法`OfficeAssignment`实体设置为它：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>创建 OfficeAssignments Web 页

创建新的 web 页使用*Site.Master*母版页并将其命名*OfficeAssignments.aspx*。 添加到以下标记`Content`控件名为`Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

请注意，在`DataKeyNames`属性，此标记指定`Timestamp`属性以及记录密钥 (`InstructorID`)。 指定属性中的`DataKeyNames`属性将导致要将它们保存在 （这是类似于查看状态） 的控件状态的控件，以便在回发的处理过程中这些原始值都可用。

如果您没有保存`Timestamp`值，实体框架将不会为`Where`的 sql 子句`Update`命令。 因此执行任何操作会找到要更新。 因此，实体框架会引发乐观并发异常每次`OfficeAssignment`更新实体。

打开*OfficeAssignments.aspx.cs*并添加以下`using`的数据访问层的语句：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

添加以下`Page_Init`方法，使动态数据功能。 此外添加以下处理程序`ObjectDataSource`控件的`Updated`可加快检查并发错误的事件：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>在 OfficeAssignments 页中测试开放式并发

运行*OfficeAssignments.aspx*页。

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

单击**编辑**行中更改中的值和**位置**列。

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

打开新的浏览器窗口并运行页再次 （复制到第二个浏览器窗口中的第一个浏览器窗口的 URL）。

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

单击**编辑**在同一行你前面编辑和更改**位置**到的其他内容的值。

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

在第二个浏览器窗口中，单击**更新**。

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

切换到第一个浏览器窗口，然后单击**更新**。

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

你看到一条错误消息和**位置**值是否已更新以显示在第二个浏览器窗口中更改到的值。

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>处理与 EntityDataSource 控件的并发

`EntityDataSource`控件包括内置识别数据模型中的并发设置的逻辑和句柄更新和相应地删除操作。 但是，与所有异常，必须处理`OptimisticConcurrencyException`异常自己以便提供用户友好错误消息。

接下来，你将配置*Courses.aspx*页面 (使用`EntityDataSource`控件) 来允许更新和删除操作并显示一条错误消息，如果发生并发冲突。 `Course`实体不具有跟踪列，因此你将使用相同的方法，就像处理并发`Department`实体： 跟踪的所有非键属性的值。

打开*SchoolModel.edmx*文件。 非键属性的`Course`实体 (`Title`， `Credits`，和`DepartmentID`)，请设置**并发模式**属性`Fixed`。 然后保存并关闭数据模型。

打开*Courses.aspx*页上并进行以下更改：

- 在`CoursesEntityDataSource`控件中，添加`EnableUpdate="true"`和`EnableDelete="true"`属性。 现在，该控件的开始标记类似于下面的示例：

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- 在`CoursesGridView`控件中，更改`DataKeyNames`属性值到`"CourseID,Title,Credits,DepartmentID"`。 然后添加`CommandField`元素`Columns`显示的元素**编辑**和**删除**按钮 (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`)。 `GridView`控件现在类似于下面的示例：

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

运行页面和创建发生冲突，就像之前在部门页。 在两个浏览器窗口中运行页面，单击**编辑**在同一个在每个窗口中，行，然后在每个进行其他更改。 单击**更新**在一个窗口，然后单击**更新**在其他窗口中。 当你单击**更新**第二次，你将看到错误页得出未处理的并发异常。

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

处理方式非常类似于如何处理为中的出现此错误`ObjectDataSource`控件。 打开*Courses.aspx*页上，然后在`CoursesEntityDataSource`控件中，指定的处理程序`Deleted`和`Updated`事件。 控件的开始标记现在类似于下面的示例：

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

之前`CoursesGridView`控件中，添加以下`ValidationSummary`控件：

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

在*Courses.aspx.cs*，添加`using`语句`System.Data`命名空间，添加一个方法来检查并发异常，并添加处理程序`EntityDataSource`控件的`Updated`和`Deleted`处理程序。 代码将如下所示：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

此代码，并为执行的唯一区别`ObjectDataSource`控件在于，在这种情况下并发异常采用`Exception`属性的事件自变量对象而不是该异常中`InnerException`属性。

运行页面，并重新创建并发冲突。 此时会显示一条错误消息：

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

这将完成简介处理并发冲突。 下一教程将提供有关如何提高性能的 web 应用程序使用实体框架中的指导。

>[!div class="step-by-step"]
[上一页](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
[下一页](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
