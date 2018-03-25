---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: "使用 Entity Framework 4.0 和 ObjectDataSource 控件，第 1 部分： 入门 |Microsoft 文档"
author: tdykstra
description: "本教程系列上的 Contoso 大学 web 应用程序创建的 Getting Started with 实体框架教程系列生成。 如果则..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 83fe815af9030aee10a5204718b00c79925e9126
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>使用 Entity Framework 4.0 和 ObjectDataSource 控件，第 1 部分： 入门
====================
通过[Tom Dykstra](https://github.com/tdykstra)

> 本教程系列上的 Contoso 大学 web 应用程序创建的生成[Getting Started with 实体 Framework 4.0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)教程系列。 如果未完成前面的教程，作为一个起始点本教程，你可以[下载应用程序](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)，你应已创建。 你还可以[下载应用程序](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)，它由完整教程系列创建。
> 
> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web 窗体应用程序。 示例应用程序是虚构的 Contoso 大学网站。 它包括诸如学生许可、 过程创建和教师分配等功能。
> 
> 本教程将说明在 C# 示例。 [可下载示例](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)包含 C# 和 Visual Basic 中的代码。
> 
> ## <a name="database-first"></a>第一个数据库
> 
> 有三种方法可以使用的实体框架中的数据： *Database First*， *Model First*，和*Code First*。 此教程适用于第一个数据库。 有关如何为你的方案选择最佳这些工作流和指南之间的差异信息，请参阅[实体框架开发工作流](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)。
> 
> ## <a name="web-forms"></a>Web Forms — Web 窗体
> 
> 入门教程系列中，如本系列教程使用 ASP.NET Web 窗体模型，并假设您了解如何使用 Visual Studio 中的 ASP.NET Web 窗体。 如果不这样做，请参阅[Getting Started with ASP.NET 4.5 Web 窗体](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)。 如果你更想要使用 ASP.NET MVC framework，请参阅[Getting Started with 使用 ASP.NET MVC 的 Entity Framework](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。
> 
> ## <a name="software-versions"></a>软件版本
> 
> | **本教程中所示** | **也适用于** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express for Web. 本教程不经过测试与更高版本的 Visual Studio。 有许多菜单中选择、 对话框框中，和模板中的差异。 |
> | .NET 4 | .NET 4.5 是与.NET 4 中，向后的兼容，但本教程未经过测试.NET 4.5。 |
> | 实体框架 4 | 本教程不经过测试与实体框架的更高版本。 从实体框架 5 开始，默认情况下使用 EF `DbContext API` ，引入了 EF 4.1。 EntityDataSource 控件的设计用于`ObjectContext`API。 有关如何使用 EntityDataSource 控件`DbContext`API，请参阅[这篇博客文章](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx)。 |
> 
> ## <a name="questions"></a>问题
> 
> 如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET 实体框架论坛](https://forums.asp.net/1227.aspx)、[实体框架和 LINQ to Entities 论坛](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)，或[StackOverflow.com](http://stackoverflow.com/)。


`EntityDataSource`控制，你可以非常快速地创建应用程序，但它通常需要你需要占用大量的业务逻辑和中的数据访问逻辑你*.aspx*页。 如果希望你的应用程序，以增大复杂性和需要持续不断的维护，您可以为了创建预先购买更多的开发时间*n 层*或*分层*应用程序结构这就是更易于维护。 若要实现此体系结构，请从业务逻辑层 (BLL) 和数据访问层 (DAL) 分隔的表示层。 若要实现此结构的一种方法是使用`ObjectDataSource`而不是控制`EntityDataSource`控件。 当你使用`ObjectDataSource`控件，实现您自己的数据访问代码，然后调用在*.aspx*页使用具有多个相同的控件功能的其他数据源控件。 这样就可以使用 Web 窗体控件进行数据访问的好处与合并的 n 层方法的优点。

`ObjectDataSource`控制使你更大的灵活性以及通过其他方式。 由于写入你自己的数据访问代码，它是更轻松地执行操作远不止读取、 插入、 更新或删除特定实体类型，这是任务的`EntityDataSource`控件设计执行。 例如，你可以执行日志记录每次更新实体时，每当删除的实体，或自动检查和更新与相关的数据根据需要插入具有外键的值的行时存档数据。

## <a name="business-logic-and-repository-classes"></a>业务逻辑和存储库类

`ObjectDataSource`控件通过调用你创建的类的工作原理。 类包括方法用于检索和更新数据，并提供到这些方法的名称`ObjectDataSource`标记中的控件。 在呈现或回发的处理过程`ObjectDataSource`调用你指定的方法。

除了基本的 CRUD 操作，创建要用于类`ObjectDataSource`控件可能需要执行业务逻辑时`ObjectDataSource`读取或更新数据。 例如，在更新部门时，你可能需要验证没有其他部门具有相同的管理员，因为人不能是多个部门的管理员。

在某些`ObjectDataSource`文档，如[ObjectDataSource 类概述](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx)，该控件调用的类称为*业务对象*，包括业务逻辑和数据访问逻辑. 在本教程中，你将创建单独的类为业务逻辑和数据访问逻辑。 封装数据访问逻辑的类称为*存储库*。 业务逻辑类包括业务逻辑方法和数据访问方法，但数据访问方法都会调用要执行数据访问任务的存储库。

你还将创建 BLL 和 DAL，它方便了自动的单元之间的抽象层的 BLL 测试。 此抽象层实现通过创建一个接口并使用该接口，当实例化的业务逻辑类中的存储库中。 这使你的业务逻辑类提供对任何实现存储库接口的对象的引用。 对于正常操作，你可以提供存储库对象，可与实体框架。 用于测试，你可以提供适用于一种方法，可以轻松地进行处理，如类变量定义为集合中存储的数据的存储库对象。

下图显示的业务逻辑类，其中包含数据访问逻辑，而无需存储库，另一个使用存储库之间的差异。

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

通过在其中创建网页，你将开始`ObjectDataSource`控件绑定直接到存储库，因为它仅执行基本的数据访问任务。 在下一步的教程中，你将创建包含验证逻辑的业务逻辑类并将绑定`ObjectDataSource`到此类上而不是存储库类的控件。 你还将创建单元测试验证逻辑。 在本系列第三个教程中，你将添加排序和筛选到应用程序的功能。

本教程中创建页适用的`Departments`中创建的数据模型的实体集[入门教程系列](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>更新数据库和数据模型

你将通过更改的数据库，这两种需要相应更改你在中创建的数据模型的两个开始本教程[实体框架和 Web 窗体入门](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)教程。 这些教程之一，设计器中手动数据库更改后与数据库同步的数据模型进行更改。 在本教程中，你将使用设计器的**从数据库更新模型**工具来自动更新数据模型。

### <a name="adding-a-relationship-to-the-database"></a>添加到数据库的关系

在 Visual Studio 中，打开 Contoso 大学 web 应用程序中创建[实体框架和 Web 窗体入门](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)教程系列，然后打开`SchoolDiagram`数据库关系图。

如果你看一下`Department`表在数据库关系图中，你将看到它具有`Administrator`列。 此列是外的键与`Person`表中，但任何外键关系定义在数据库中。 你需要创建关系，并更新数据模型，使实体框架可以自动处理这种关系。

在数据库关系图中，右键单击`Department`表，然后选择**关系**。

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

在**Foreign Key Relationships**中，单击**添加**，然后单击省略号**表和列规范**。

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

在**表和列**对话框框中，设置主键表和字段`Person`和`PersonID`，并设置外键表和字段`Department`和`Administrator`。 (执行此操作时，关系名称将更改从`FK_Department_Department`到`FK_Department_Person`。)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

单击**确定**中**表和列**框中，单击**关闭**中**Foreign Key Relationships**框中，并保存所做的更改。 如果系统询问你是否要保存`Person`和`Department`表，单击**是**。

> [!NOTE]
> 如果已删除`Person`对应于已在数据的行`Administrator`列中，你将无法保存此更改。 在这种情况下，使用中的表编辑器**服务器资源管理器**若要确保`Administrator`值在每个`Department`行包含中是否实际存在的记录 ID`Person`表。
> 
> 保存更改后，你将不能删除的行从`Person`表如果该人是部门管理员。 在生产应用程序中，你将提供特定的错误消息，当数据库约束，则阻止删除，或将指定级联删除。 有关如何指定级联删除的示例，请参阅[实体框架和 ASP.NET – 获取启动第 2 部分](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md)。


### <a name="adding-a-view-to-the-database"></a>向数据库添加视图

在新*Departments.aspx*你将需要创建的页上，你想要提供"姓"格式的名称的教师，下拉列表，以便用户可以选择部门管理员。 若要更加轻松地做到这一点，将在数据库中创建视图。 该视图将包括只需通过下拉列表所需的数据: （正确格式） 的完整名称和记录密钥。

在**服务器资源管理器**，展开*School.mdf*，右键单击**视图**文件夹，并选择**添加新视图**。

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

单击**关闭**时**添加表**对话框随即出现，并将以下 SQL 语句粘贴到 SQL 窗格：

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

保存此视图作为`vInstructorName`。

### <a name="updating-the-data-model"></a>更新数据模型

在*DAL*文件夹中，打开*SchoolModel.edmx*文件，右键单击设计图面，然后选择**从数据库更新模型**。

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

在**选择数据库对象**对话框中，选择**添加**选项卡并选择你刚刚创建的视图。

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

单击 **“完成”**。

在设计器中，你看到该工具创建`vInstructorName`实体和新之间的关联`Department`和`Person`实体。

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> 在**输出**和**错误列表**windows 可能会看到一条警告消息，告知你该工具会自动创建一个主键的新`vInstructorName`视图。 这是预期行为。


当你指向新`vInstructorName`在代码中的实体，你不想使用的前缀与其小写"v"的数据库约定。 因此，你将重命名的实体和实体模型中设置。

打开**模型浏览器**。 你看到`vInstructorName`列为实体类型和视图。

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

下**SchoolModel** (不**SchoolModel.Store**)，右键单击**vInstructorName**和选择**属性**。 在**属性**窗口中，更改**名称**"InstructorName"和更改的属性**实体集名称**"InstructorNames"的属性。

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

保存并关闭数据模型中，然后再重新生成项目。

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>使用存储库类和 ObjectDataSource 控件

创建新的类文件中*DAL*文件夹中，将其命名为*SchoolRepository.cs*，并将现有代码替换为以下代码：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

此代码提供单个`GetDepartments`返回中的实体的所有方法`Departments`实体集。 因为你知道你将访问`Person`为每个行的导航属性返回，您指定预先加载该属性使用`Include`方法。 类还实现`IDisposable`接口，以确保释放此对象时释放数据库连接。

> [!NOTE]
> 常见的做法是创建每个实体类型的存储库类。 在本教程中，使用多个实体类型的一个存储库类。 有关存储库模式的详细信息，请参阅中的文章[实体框架团队的博客](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)和[Julie Lerman 博客](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/)。


`GetDepartments`方法返回`IEnumerable`对象而非`IQueryable`以确保即使释放存储库对象本身返回的集合是可用的对象。 `IQueryable`每当访问，但可能会尝试呈现的数据的数据绑定控件时释放此存储库对象，对象可能会导致数据库访问。 无法返回另一个集合类型，例如`IList`对象而不是`IEnumerable`对象。 但是，返回`IEnumerable`对象，可以确保你可以如执行典型的只读列表处理任务`foreach`循环和 LINQ 查询，但你无法添加或删除该集合，这可能意味着此类更改将中的项保存到数据库。

创建*Departments.aspx*使用的页*Site.Master*母版页，并添加中的以下标记`Content`控件名为`Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

此标记将创建`ObjectDataSource`使用刚创建的存储库类的控件和`GridView`控件来显示数据。 `GridView`控件指定**编辑**和**删除**命令，但你尚未添加代码以尚未支持它们。

多个列使用`DynamicField`控件，使你可以利用的自动数据格式设置和验证功能。 对于这些数据来工作，你必须调用`EnableDynamicData`中的方法`Page_Init`事件处理程序。 (`DynamicControl`中未使用控件`Administrator`字段原因是它们不能具有导航属性。)

`Vertical-Align="Top"`特性将具有重要更高版本中添加具有嵌套的列时`GridView`控件添加到网格。

打开*Departments.aspx.cs*文件并添加以下`using`语句：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

然后添加以下处理程序已在页面的`Init`事件：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

在*DAL*文件夹中，创建名为的新类文件*Department.cs*和替换为以下代码替换现有代码：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

此代码添加到数据模型的元数据。 它指定`Budget`属性`Department`实体实际上表示货币，尽管其数据类型是`Decimal`，并指定的值必须介于 0 到 1000，000.00 $ 之间。 它还指定`StartDate`属性的格式应为 mm/dd/yyyy 格式的日期。

运行*Departments.aspx*页。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

请注意，尽管未指定在一个格式字符串*Departments.aspx*页标记**预算**或**开始日期**列，默认货币和日期格式设置已应用到通过在它们`DynamicField`控件，以使用元数据中提供*Department.cs*文件。

## <a name="adding-insert-and-delete-functionality"></a>添加的插入和删除功能

打开*SchoolRepository.cs*，添加以下代码以创建`Insert`方法和一个`Delete`方法。 代码还包括一个名为方法`GenerateDepartmentID`，可计算以供下一步的可用的记录密钥值`Insert`方法。 这是必需的因为未配置的数据库来计算此自动为`Department`表。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Attach 方法

`DeleteDepartment`方法调用`Attach`方法，以重新建立在内存中的实体和数据库之间的对象上下文的对象状态管理器中的维护的链接行它表示。 此方法调用之前必须发生`SaveChanges`方法。

术语*对象上下文*Entity Framework 类派生自引用`ObjectContext`类，该类用于访问实体集和实体。 在此项目的代码中，类名为`SchoolEntities`，和它的实例始终名为`context`。 对象上下文*对象状态管理器*是派生自的类`ObjectStateManager`类。 对象联系人来存储实体对象，并能够跟踪其相应的表行或数据库中的行与同步每个是否使用对象状态管理器。

当读取实体时，对象上下文将其存储在对象状态管理器和跟踪该对象表示形式是否与数据库同步。 例如，如果更改属性值，是设置标志以指示，你更改的属性将无法再与数据库同步。 然后调用`SaveChanges`方法，对象上下文知道在数据库中执行操作，因为对象状态管理器知道完全什么是实体的当前状态和数据库的状态之间差别。

但是，此过程通常不工作不在 web 应用程序，因为读取实体，以及其对象状态管理器中的所有内容的对象上下文实例已释放之后呈现一个页面。 必须应用更改的对象上下文实例是一个为回发处理实例化的新。 情况下`DeleteDepartment`方法，`ObjectDataSource`控件重新实体的原始版本为你创建从值在视图状态，但这重新创建`Department`对象状态管理器中不存在实体。 如果调用`DeleteObject`此重新创建实体的方法，调用将失败，因为对象上下文不知道实体是否与数据库同步。 但是，调用`Attach`方法重新建立最初自动执行操作时该实体已读取早期实例对象上下文中的数据库中重新创建的实体和值之间的相同跟踪。

有的时间，你不希望对象上下文来跟踪对象状态管理器中中的实体并可以设置标志以防止它执行该操作。 此示例中这一系列的更高版本教程所示。

### <a name="the-savechanges-method"></a>SaveChanges 方法

此简单的存储库类演示了如何执行 CRUD 操作的基本原则。 在此示例中，`SaveChanges`每次更新后立即调用方法。 在生产应用程序中你可能想要调用`SaveChanges`从单独的方法来为你提供更好地控制何时更新数据库的方法。 （在下一教程末尾将找到指向讨论的工作模式，即协调相关的更新的一种方法的单元的白皮书的链接。）此外请注意，在示例中，`DeleteDepartment`方法不包括用于处理并发冲突的代码; 将在本系列后面的教程中添加代码以执行该操作。

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>检索教师名称来选择插入时

用户必须能够从教师下拉列表中的列表中选择管理员创建的新部门时。 因此，以下代码添加到*SchoolRepository.cs*若要创建一个方法来检索教师使用前面创建的视图的列表：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>用于插入部门创建页

创建*DepartmentsAdd.aspx*使用的页*Site.Master*页上，并添加中的以下标记`Content`控件名为`Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

此标记将创建两个`ObjectDataSource`控件，一个用于插入新`Department`实体，一个用于检索教师名称`DropDownList`用于选择部门管理员的控制。 标记创建`DetailsView`控件输入新部门，和它指定为该控件的处理程序`ItemInserting`事件，以便你可以设置`Administrator`外键值。 在结束时`ValidationSummary`控件来显示错误消息。

打开*DepartmentsAdd.aspx.cs*并添加以下`using`语句：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

添加以下类变量和方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

`Page_Init`方法启用动态数据功能。 处理程序`DropDownList`控件的`Init`事件将保存对该控件，然后处理程序的引用`DetailsView`控件的`Inserting`事件使用该引用获取`PersonID`的所选的教师和更新的值`Administrator`的外键属性`Department`实体。

运行页面，为新部门，添加信息，然后单击**插入**链接。

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

输入值的另一个新的部门。 请输入一个数字大于 1000 中，000.00**预算**字段和 tab 键切换到下一个字段。 在字段中，将出现一个星号，如果鼠标指针悬停在其上时，你可以看到为该字段中输入的元数据中的错误消息。

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

单击**插入**，并查看显示的错误消息`ValidationSummary`在页面底部的控件。

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

接下来，关闭浏览器并打开*Departments.aspx*页。 添加到的删除功能*Departments.aspx*页上通过添加`DeleteMethod`属性设为`ObjectDataSource`控件，和一个`DataKeyNames`属性设为`GridView`控件。 这些控件开始标记现在将类似于下面的示例：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

运行页面。

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

删除在运行时添加的部门*DepartmentsAdd.aspx*页。

## <a name="adding-update-functionality"></a>添加更新功能

打开*SchoolRepository.cs*并添加以下`Update`方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

当你单击**更新**中*Departments.aspx*页上，`ObjectDataSource`控件创建两个`Department`实体要传递给`UpdateDepartment`方法。 一个包含视图状态中存储的原始值，另一个包含输入的新值`GridView`控件。 中的代码`UpdateDepartment`方法传递`Department`具有这些原始值与实体上`Attach`方法，以建立的实体之间定义什么是数据库中的跟踪。 然后在代码传递`Department`具有到新的值的实体`ApplyCurrentValues`方法。 对象上下文比较旧和新值。 如果新值不同于旧值，则对象上下文更改属性值。 `SaveChanges`方法然后更新数据库中仅已更改的列。 （但是，如果此实体的更新函数已映射到存储过程中，整个行将会更新无论哪个列已更改。）

打开*Departments.aspx*文件并添加以下属性到`DepartmentsObjectDataSource`控件：

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 此原因旧值应存储在视图状态，以便它们可以与中的新值比较`Update`方法。
- `OldValuesParameterFormatString="orig{0}"`   
 这可告知原始值参数的名称是该控件`origDepartment`。

开始标记的标记`ObjectDataSource`控件现在类似于下面的示例：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

添加`OnRowUpdating="DepartmentsGridView_RowUpdating"`属性设为`GridView`控件。 将使用它来设置`Administrator`属性值根据用户在下拉列表中选择的行。 `GridView`现在开始标记类似于下面的示例：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

添加`EditItemTemplate`控制`Administrator`列`GridView`控制后立即,`ItemTemplate`将该列的控件：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

这`EditItemTemplate`控件是类似于`InsertItemTemplate`中控制*DepartmentsAdd.aspx*页。 区别是使用设置控件的初始值`SelectedValue`属性。

之前`GridView`控件中，添加`ValidationSummary`控制中那样*DepartmentsAdd.aspx*页。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

打开*Departments.aspx.cs*和紧接在分部类声明中，添加以下代码以创建私有字段以引用`DropDownList`控件：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

然后添加处理程序`DropDownList`控件的`Init`事件和`GridView`控件的`RowUpdating`事件：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

处理程序`Init`事件将保存到的引用`DropDownList`类字段中的控件。 处理程序`RowUpdating`事件使用的引用以获取用户输入的值并将其应用于`Administrator`属性`Department`实体。

使用*DepartmentsAdd.aspx*页以添加新部门，然后运行*Departments.aspx*页上，单击**编辑**你添加的一行上。

> [!NOTE]
> 你将不能编辑未添加的行 (即，已在数据库中)，由于数据库; 中的数据无效创建与数据库的行的管理员是学生。 如果你尝试编辑其中之一，则会出现一个错误页面，报告类似的错误`'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

如果输入无效**预算**量，然后单击**更新**，你看到的相同星号和你在中看到的错误消息*Departments.aspx*页。

更改字段值，或者选择另一个管理员，请单击**更新**。 将显示更改。

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

这将完成使用简介`ObjectDataSource`控件的基本 CRUD （创建、 读取、 更新、 删除） 与实体框架的操作。 你已经构建一个简单的 n 层应用程序，但与数据访问层，这将使复杂化自动的单元测试仍紧密耦合的业务逻辑层。 在以下教程中，你将看到如何实现以便于单元测试的存储库模式。

>[!div class="step-by-step"]
[下一篇](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
