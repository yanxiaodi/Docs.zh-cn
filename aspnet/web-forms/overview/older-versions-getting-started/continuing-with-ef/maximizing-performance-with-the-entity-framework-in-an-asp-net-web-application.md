---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: "最大程度地使用实体框架 4.0 ASP.NET 4 Web 应用程序中的性能 |Microsoft 文档"
author: tdykstra
description: "本教程系列上的 Contoso 大学 web 应用程序创建的 Getting Started with 实体 Framework 4.0 教程系列生成。 I..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 40a53a110115e5f6342d2a97d21b64470450fd3c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>最大程度地使用实体框架 4.0 ASP.NET 4 Web 应用程序中的性能
====================
通过[Tom Dykstra](https://github.com/tdykstra)

> 本教程系列上的 Contoso 大学 web 应用程序创建的生成[Getting Started with 实体 Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started)教程系列。 如果未完成前面的教程，作为一个起始点本教程，你可以[下载应用程序](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)，你应已创建。 你还可以[下载应用程序](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)，它由完整教程系列创建。 如果你有疑问的教程，你可以发布到[ASP.NET 实体框架论坛](https://forums.asp.net/1227.aspx)。


在前面的教程，您将了解如何处理并发冲突。 本教程演示了用于改进性能的 ASP.NET web 应用程序使用实体框架的选项。 你将了解几种方法最大化性能或诊断性能问题。

显示在以下部分中的信息可在各种方案中很有用：

- 有效地加载相关的数据。
- 管理视图状态。

显示在以下部分中的信息可能非常有用如果单个查询该存在性能问题：

- 使用`NoTracking`合并选项。
- 预编译的 LINQ 查询。
- 检查查询命令发送到数据库。

以下部分中显示的信息是可能适用于应用程序具有过大的数据模型：

- 预生成视图。

> [!NOTE]
> Web 应用程序性能受许多因素，包括如请求和响应数据的大小、 数据库查询、 多少个请求服务器可以排入队列和如何快速它就可以处理它们，并甚至任一提高效率的速度你可能正在使用的客户端脚本库。 如果性能非常重要中你的应用程序，或如果测试或体验显示，应用程序性能并不令人满意时，应遵循有关性能调整正常协议。 度量值来确定性能瓶颈的发生位置，然后解决将对应用程序总体性能的影响最大的区域。
> 
> 本主题主要侧重于在其中可以潜在地提高 ASP.NET 中的实体框架专门的性能的方式。 建议此处非常有用，如果您确定数据访问不在你的应用程序的性能瓶颈之一。 但如前所述，此处所述的方法不应视为&quot;最佳实践&quot;通常-其中的许多应该仅在异常的情况下或地址非常特定的一种性能瓶颈。


若要开始本教程，请启动 Visual Studio 并打开与你共事中以前的教程，Contoso 大学 web 应用程序。

## <a name="efficiently-loading-related-data"></a>有效地加载相关的数据

有多种实体框架可以加载到实体的导航属性的相关的数据：

- *延迟加载*。 当第一次读取实体时，不检索相关的数据。 但是，第一次尝试访问的导航属性，将自动检索该导航属性所需的数据。 这会导致发送到数据库的多个查询 — 一个用于在实体自身，一个必须检索每个相关实体的数据的时间。 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*预先加载*。 实体中读取时，会随之检索相关的数据。 这通常会导致检索所有具有所需的数据的单一联接查询。 使用指定预先加载`Include`方法，当你已看到这些教程中。

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *显式加载*。 这是类似于延迟加载的只不过显式检索代码; 中的相关的数据它不会发生自动访问导航属性时。 使用手动的相关的数据加载`Load`方法的导航属性的集合，或者你使用`Load`保存单个对象的属性引用属性的方法。 (例如，调用`PersonReference.Load`方法以加载`Person`导航属性`Department`实体。)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

因为它们不立即检索的属性值，延迟加载和显式加载也同时称为*延迟加载*。

延迟加载是由设计器生成的对象上下文的默认行为。 如果你打开*SchoolModel.Designer.cs*文件，它定义对象上下文类，你将找到三个构造函数方法，以及每个包含以下语句：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

一般情况下，如果你知道需要相关的数据检索，预先加载提供最佳性能，每个实体因为单个查询发送到数据库通常比用于检索每个实体的单独查询效率更高。 另一方面，如果你需要仅不常访问实体的导航属性，还是仅为一个较小的实体、 延迟加载或显式加载集可能因更高效，因为预先加载将检索比你需要更多的数据。

在 web 应用程序，延迟加载可能的相对较小的值，由于会影响对相关数据的需求的用户操作发生在浏览器，没有连接到对象上下文呈现页。 另一方面，当你数据绑定控件，你通常知道哪些数据需要并因此它通常是最佳选择预先加载或基于的延迟的加载时什么是适用于每个方案。

此外，数据绑定控件可能使用的实体对象后释放对象上下文。 在这种情况下，若要延迟加载的导航属性的尝试会失败。 您收到的错误消息十分明显：&quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

`EntityDataSource`控件默认情况下禁用延迟加载。 有关`ObjectDataSource`控制你使用的当前教程 （或如果你从页的代码访问的对象上下文），有几种方法，你可以延迟加载默认禁用。 当实例化对象上下文时，你可以禁用它。 例如，可以将以下行添加到构造函数方法`SchoolRepository`类：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

对于 Contoso 大学应用程序，你将使自动禁用延迟加载的因此，此属性不必须时实例化上下文设置的对象上下文。

打开*SchoolModel.edmx*数据模型，单击设计图面上，，然后在属性窗格中设置**延迟加载启用**属性`False`。 保存并关闭数据模型。

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>管理视图状态

为了提供更新功能，ASP.NET 网页时，必须存储实体的原始属性值中呈现页。 处理控件的回发期间重新创建该实体的原始状态，调用实体的`Attach`方法，然后才能应用更改并调用`SaveChanges`方法。 默认情况下，ASP.NET Web 窗体数据控件使用视图状态来存储的原始值。 但是，视图状态会影响性能，因为它存储在会显著增加发送到和从浏览器的页面大小的隐藏字段。

技术用于管理视图状态或替代产品，例如会话状态，并不到实体框架中，唯一的因此本教程不转到本主题中详细信息。 有关详细信息，请参阅链接，在本教程末尾。

但是，版本 4 的 ASP.NET 提供了使用 Web 窗体应用程序的每个 ASP.NET 开发人员应注意的视图状态的新方法：`ViewStateMode`属性。 可以在页或控件级别，设置此新属性，它可用于页面的默认情况下禁用视图状态，并仅为需要它的控件中启用它。

性能关键的应用程序，一个好的做法是始终禁用页级的视图状态，并仅为需要它的控件中启用它。 Contoso 大学页中的视图状态的大小不会显著减少通过这种方法，但若要查看其工作原理，你将执行此操作*Instructors.aspx*页。 该页面包含很多控件，包括`Label`禁用了视图状态的控件。 无此页上的控件实际上需要已启用状态的视图。 (`DataKeyNames`属性`GridView`控件指定必须回发，之间保持的状态，但这些值保存在通过不受影响的控件状态`ViewStateMode`属性。)

`Page`指令和`Label`控件标记当前类似于下面的示例：

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

进行以下更改：

- 添加`ViewStateMode="Disabled"`到`Page`指令。
- 删除`ViewStateMode="Disabled"`从`Label`控件。

标记现在将类似于下面的示例：

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

视图状态现已禁用的所有控件。 如果你稍后添加控件确实需要使用视图状态，你需要做是包括`ViewStateMode="Enabled"`该控件的属性。

## <a name="using-the-notracking-merge-option"></a>使用 NoTracking 合并选项

当对象上下文检索数据库行，并创建表示它们的实体对象时，默认情况下它还跟踪这些实体对象使用其对象状态管理器。 此跟踪数据充当缓存，并更新实体时使用。 Web 应用程序通常具有生存期较短的对象上下文实例，因为查询通常返回不需要进行跟踪，因为将释放对象上下文读取它们，再次使用它读取的实体的任何之前的数据或更新。

在实体框架中，你可以指定的对象上下文是否通过设置跟踪实体对象*合并选项*。 你可以设置的单个查询或实体集的合并选项。 如果将其设置为实体集，这意味着你要设置的默认合并选项为该实体集创建的所有查询。

对于 Contoso 大学应用程序，跟踪无需任何你访问从存储库，因此你可以设置的合并选项为的实体集`NoTracking`为这些实体集时实例化的对象上下文中的存储库类。 （请注意，在本教程中，设置的合并选项不会产生明显影响对应用程序的性能。 `NoTracking`选项有可能使仅在某些大数据容量方案中的可观察到的性能改进。)

在 DAL 文件夹中，打开*SchoolRepository.cs*文件并添加的实体设置存储库访问设置的合并选项的构造函数方法：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>预编译的 LINQ 查询

实体框架所执行的生存期内的 Entity SQL 查询的第一个时间给定`ObjectContext`实例，需要一段时间来编译查询。 缓存的编译结果，这意味着后续执行的查询快得多。 LINQ 查询按照类似的模式，只不过某些编译查询所需的工作完成每次执行查询。 换而言之，对于 LINQ 查询，默认情况下不是所有的编译结果进行缓存。

如果你有你希望重复运行生活中的对象上下文的 LINQ 查询，你可以编写代码，使所有编译将缓存第一次运行 LINQ 查询的结果。

为说明，你需要执行此操作适用于两个`Get`中的方法`SchoolRepository`类，其中不带任何参数 (`GetInstructorNames`方法)，和一个需要一个参数 (`GetDepartmentsByAdministrator`方法)。 这些方法为代表现在实际上不需要编译，因为它们不是 LINQ 查询：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

但是，以便你还可以尝试已编译的查询，你将继续进行，犹如这些编写为以下 LINQ 查询：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

具有上面所示和运行应用程序来验证其工作在继续之前，无法更改这些方法中的代码。 但以下说明阅读右本文档中创建预编译的版本。

创建中的类文件*DAL*文件夹中，将其命名为*SchoolEntities.cs*，并将现有代码替换为以下代码：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

此代码将创建扩展自动生成的对象上下文类的分部类。 分部类包含两个已编译的 LINQ 查询使用`Compile`方法`CompiledQuery`类。 它还将创建可用于调用查询的方法。 保存并关闭此文件。

接下来，在*SchoolRepository.cs*，更改现有`GetInstructorNames`和`GetDepartmentsByAdministrator`储存库中的方法的类，使它们调用已编译的查询：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

运行*Departments.aspx*页以验证它能否像以前一样工作。 `GetInstructorNames`以便填充管理员下拉列表中，调用方法和`GetDepartmentsByAdministrator`单击时，方法调用**更新**为了验证没有教师是否的多个管理员部门。

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

在 Contoso 大学应用程序只是为了不是因为它会明显提高性能，请参阅如何做到这一点，已预编译的查询。 预编译的 LINQ 查询未添加到代码中的复杂性，因此请确保你执行此操作仅对实际表示你的应用程序中的性能瓶颈的查询。

## <a name="examining-queries-sent-to-the-database"></a>检查查询发送到数据库

当你正在调查性能问题时，有时会有帮助以了解确切实体框架将发送到数据库的 SQL 命令。 如果你正在使用`IQueryable`对象，执行此操作的一种方法是使用`ToTraceString`方法。

在*SchoolRepository.cs*，更改中的代码`GetDepartmentsByName`方法以匹配以下示例：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

`departments`变量必须强制转换为`ObjectQuery`键入只是因为`Where`方法在前面的行的末尾创建`IQueryable`对象; 而无需`Where`方法，该强制转换不是必需。

上设置断点`return`行，然后再运行*Departments.aspx*在调试器中的页。 当命中断点时，检查`commandText`变量中**局部变量**窗口并使用文本可视化工具 (在放大镜**值**列) 以显示其值在**文本可视化工具**窗口。 你可以看到此代码生成的整个 SQL 命令：

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

作为替代方法，在 Visual Studio Ultimate 中的 IntelliTrace 功能使您能够查看生成的实体框架不需要你更改代码或甚至设置断点的 SQL 命令。

> [!NOTE]
> 仅当你有 Visual Studio Ultimate，您可以执行以下过程。


还原中的原始代码`GetDepartmentsByName`方法，并再次运行*Departments.aspx*在调试器中的页。

在 Visual Studio 中，选择**调试**菜单，然后**IntelliTrace**，，然后**IntelliTrace 事件**。

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

在**IntelliTrace**窗口中，单击**全部中断**。

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

**IntelliTrace**窗口显示最新事件的列表：

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

单击**ADO.NET**行。 它将展开以显示命令文本：

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

你可以将整个命令文本字符串复制到从剪贴板**局部变量**窗口。

假设具有多个表、 关系和的列比简单的数据库与你共事`School`数据库。 你可能会发现收集的所有信息的查询需要在单个`Select`包含多个语句`Join`子句变得太复杂，能够高效工作。 在这种情况下，你就可以从预先加载到显式加载可以简化查询切换。

例如，尝试更改中的代码`GetDepartmentsByName`中的方法*SchoolRepository.cs*。 当前，方法您可以具有一个对象查询`Include`方法`Person`和`Courses`导航属性。 替换`return`，执行显式加载，如下面的示例中所示的代码语句：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

运行*Departments.aspx*在调试器中页上，并检查**IntelliTrace**再次为你的窗口之前一致。 现在，其中没有单个查询之前，你将看到一长串的它们。

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

单击第一个**ADO.NET**行以查看发生了什么复杂查询你查看更早版本。

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

部门中的查询已变得简单`Select`查询没有`Join`子句，但它后跟检索相关的课程，又是管理员的单独查询，使用为每个部门的一组的两个查询返回的原始查询。

> [!NOTE]
> 如果你离开延迟加载启用，你使用相同的查询重复很多时候，在这里，看的模式可能会导致从延迟加载。 你通常想要避免的模式是主表的每一行的延迟加载相关的数据。 除非你已验证的单一联接查询太过复杂，来有效地，通常将可以通过更改要使用预先加载的主查询来提高在这些情况下的性能。


## <a name="pre-generating-views"></a>预生成视图

当`ObjectContext`对象首次创建新的应用程序域中，实体框架生成一组类，并用于访问数据库。 这些类称为*视图*，而如果你有一个非常大的数据模型，生成这些视图可以延迟页的第一个请求的网站的响应后初始化新的应用程序域。 你可以通过在编译时而不在运行时创建视图来减少此第一个请求延迟。

> [!NOTE]
> 如果你的应用程序没有过大的数据模型，或者如果它具有较大的数据模型，但无需考虑性能问题影响仅第一个页面请求回收 IIS 后，你可以跳过此部分。 每次您实例化时也不会发生创建的视图`ObjectContext`对象，因为视图将缓存应用程序域中。 因此，除非你经常要回收 IIS 中的应用程序，非常少的页请求将受益于预生成视图。


你可以预生成视图使用*EdmGen.exe*命令行工具或通过使用*文本模板转换工具包*(T4) 模板。 在本教程中，你将使用 T4 模板。

在*DAL*文件夹中，添加文件使用**文本模板**模板 (它位于**常规**中的节点**已安装的模板**列表)，并将其命名*SchoolModel.Views.tt*。 文件中的现有代码替换为以下代码：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

此代码生成视图*.edmx*文件位于与模板相同的文件夹并为模板文件具有相同的名称。 例如，如果名为你的模板文件*SchoolModel.Views.tt*，它将查找名为的数据模型文件*SchoolModel.edmx*。

保存该文件，然后右键单击中的文件**解决方案资源管理器**和选择**运行自定义工具**。

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio 将生成的代码文件创建视图，这名为*SchoolModel.Views.cs*在基于的模板。 (你可能已经注意到甚至在你选择之前，生成的代码文件**运行自定义工具**，就会立即保存模板文件。)

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

你现在可以运行应用程序，并确认它像以前一样的方式工作。

有关预生成视图的详细信息，请参阅以下资源：

- [如何： 预生成视图提高查询性能](https://msdn.microsoft.com/library/bb896240.aspx)MSDN 网站上。 说明如何使用`EdmGen.exe`视图预生成的命令行工具。
- [隔离与预编译/前 generated 视图实体 Framework 4 中的性能](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx)Windows Server AppFabric 客户咨询团队博客上。

这将完成简介提高的 ASP.NET web 应用程序使用实体框架中的性能。 有关更多信息，请参见以下资源：

- [性能注意事项 （实体框架）](https://msdn.microsoft.com/library/cc853327.aspx) MSDN 网站上。
- [实体框架团队博客上与性能相关的文章](https://blogs.msdn.com/b/adonet/archive/tags/performance/)。
- [EF 合并选项和已编译的查询](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx)。 说明的已编译的查询和合并的意外的行为的博客文章选项如`NoTracking`。 如果你打算使用已编译的查询或操作应用程序中的合并选项设置，这首先阅读。
- [数据和建模客户咨询团队博客中的实体框架相关文章](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/)。 包括有关已编译的查询和使用 Visual Studio 2010 探查器来发现性能问题的文章。
- [使用提升的非常复杂的查询性能的建议的实体框架论坛线程](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6)。
- [ASP.NET 状态管理建议](https://msdn.microsoft.com/library/z1hkazw7.aspx)。
- [使用实体框架和 ObjectDataSource： 自定义分页](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx)。 这些教程中创建的 ContosoUniversity 应用程序说明如何实现中的分页生成的博客文章*Departments.aspx*页。

下一教程查看一些对 Entity Framework 版本 4 中新增的重要增强功能。

>[!div class="step-by-step"]
[上一页](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
[下一页](what-s-new-in-the-entity-framework-4.md)
