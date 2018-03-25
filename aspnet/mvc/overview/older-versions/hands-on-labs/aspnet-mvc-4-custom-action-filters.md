---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: "ASP.NET MVC 4 自定义操作筛选器 |Microsoft 文档"
author: rick-anderson
description: "ASP.NET MVC 提供用于执行筛选逻辑之前或之后，则操作方法调用的操作筛选器。 操作筛选器是自定义特性 tha..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 639815cc92b7cb5f3dfb4e1a198f6b4c2476dc90
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>ASP.NET MVC 4 自定义操作筛选器

通过[Web 营地团队](https://twitter.com/webcamps)

[下载 Web 营地培训工具包](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 提供用于执行筛选逻辑之前或之后，则操作方法调用的操作筛选器。 操作筛选器是提供声明性方式，将预操作和后操作行为添加到控制器的操作方法的自定义属性。

在此动手实验将到 MvcMusicStore 解决方案，以捕获控制器的请求和记录插入数据库表的站点的活动来创建自定义操作筛选器属性。 你将能够通过注入的日志记录筛选器添加到任何控制器或操作。 最后，你将看到显示的访问者的列表的日志视图。

此动手实验假定你具有的基础知识**ASP.NET MVC**。 如果您未使用过**ASP.NET MVC**之前，我们建议你转到**ASP.NET MVC 4 基础知识**动手实验。

> [!NOTE]
> 所有的示例代码和代码段都包含在 Web 营地培训工具包，可从在[Microsoft 的 Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。 特定于此实验室项目位于[ASP.NET MVC 4 自定义操作筛选器](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters)。

<a id="Objectives"></a>
### <a name="objectives"></a>目标

在本动手实验中，你将了解如何：

- 创建要扩展的筛选功能的自定义操作筛选器属性
- 通过为特定级别的注入应用自定义的筛选器属性
- 全局注册自定义操作筛选器

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>系统必备

你必须具有要完成本实验的以下项：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更高 (读取[附录 A](#AppendixA)有关如何安装它的说明)。

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>安装

**安装代码片段**

为方便起见，你将沿此实验室管理大部分都是代码的可用作 Visual Studio 代码段。 若要安装运行的代码段**.\Source\Setup\CodeSnippets.vsi**文件。

如果你不熟悉 Visual Studio 代码段，并想要了解如何使用它们，你可以从该文档引用的附录&quot;[附录 c： 使用代码段](#AppendixC)&quot;。

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>练习

此动手实验包含通过在以下练习：

1. [练习 1： 日志记录操作](#Exercise1)
2. [练习 2： 管理多个操作筛选器](#Exercise2)

估计时间来完成该实验： **30 分钟**。

> [!NOTE]
> 每个练习均附带由**结束**包含生成您应该完成练习后获得的解决方案文件夹。 如果你需要通过在练习工作的更多帮助，可以使用此解决方案作为指南。


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>练习 1： 日志记录操作

在本练习中，你将学习如何使用 ASP.NET MVC 4 筛选器提供程序创建自定义操作日志筛选器。 为该目的将到将在所选的控制器中记录所有活动的 MusicStore 站点应用日志记录筛选器。

筛选器将扩展**ActionFilterAttributeClass** ，并重写**OnActionExecuting**方法来捕获每个请求，然后执行日志记录操作。 有关 HTTP 请求的上下文信息，执行方法、 结果和参数将会予以提供 ASP.NET MVC **ActionExecutingContext**类**。**

> [!NOTE]
> ASP.NET MVC 4 还具有默认筛选器提供程序可以使用而无需创建自定义筛选器。 ASP.NET MVC 4 提供以下类型的筛选器：
> 
> - **授权**筛选，这使得安全决策是否要执行的操作方法，如执行身份验证或验证的请求属性。
> - **操作**筛选器，用于包装操作方法的执行。 此筛选器可以执行其他处理，如提供到操作方法的额外数据、 检查返回的值，或取消执行的操作方法
> - **结果**筛选器，用于包装 ActionResult 对象执行。 此筛选器可以执行结果，例如修改 HTTP 响应的其他的处理。
> - **异常**筛选器，用于执行如果没有在操作方法中，开始授权筛选器，以与结果执行的某个位置引发未处理的异常。 异常筛选器可以用于任务，例如日志记录或显示错误页。
> 
> 有关筛选器提供程序的详细信息，请访问此 MSDN 链接: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx))。


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>有关 MVC 音乐应用商店应用程序日志记录功能

此音乐商店解决方案具有站点日志记录的新数据模型表**ActionLog**，使用以下字段： 接收请求，调用操作，客户端 IP 和时间戳的控制器名称。

![数据模型。ActionLog 表。](aspnet-mvc-4-custom-action-filters/_static/image1.png "数据模型。ActionLog 表。")

*数据模型的 ActionLog 表*

解决方案提供的操作日志中找不到在 ASP.NET MVC 视图**MvcMusicStores/视图/ActionLog**:

![操作日志视图](aspnet-mvc-4-custom-action-filters/_static/image2.png "操作日志视图")

*操作日志视图*

与此提供结构，情况下，所有工作将都侧重于中断控制器的请求和使用自定义筛选执行日志记录中。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>任务 1-创建一个自定义的筛选器，以捕获控制器的请求

在此任务将创建一个将包含的日志记录逻辑的自定义筛选器属性类。 为实现此目的，你将扩展 ASP.NET MVC **ActionFilterAttribute**类和实现接口**IActionFilter**。

> [!NOTE]
> **ActionFilterAttribute**是属性的所有筛选器的基类。 它提供以下方法之后和控制器操作执行之前执行特定的逻辑：
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext): 恰好在操作之前调用方法。
> - **OnActionExecuted**(ActionExecutedContext filterContext): 操作方法调用之后之前 （前视图呈现） 执行的结果。
> - **OnResultExecuting**(ResultExecutingContext filterContext): 刚好在 （在之前视图呈现） 执行的结果之前。
> - **OnResultExecuted**(ResultExecutedContext filterContext): （后呈现视图） 执行的结果后。
> 
> 通过重下列任一方法写到的派生类中，你可以执行筛选代码。


1. 打开**开始**解决方案位于**\Source\Ex01-LoggingActions\Begin**文件夹。

    1. 你将需要下载一些缺少的 NuGet 程序包，然后再继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
    > 
    > 有关详细信息，请参阅此文章： [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)。
2. 添加一个新 C# 类到**筛选器**文件夹并将其命名*CustomActionFilter.cs*。 此文件夹将存储所有自定义的筛选器。
3. 打开**CustomActionFilter.cs**并添加对引用**System.Web.Mvc**和**MvcMusicStore.Models**命名空间：

    (代码段- *ASP.NET MVC 4 自定义操作筛选器-Ex1 CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. 继承**CustomActionFilter**类**ActionFilterAttribute**然后进行**CustomActionFilter**类实现**IActionFilter**接口。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. 请**CustomActionFilter**类重写方法**OnActionExecuting**并添加必要的逻辑来筛选器的执行操作记录。 若要执行此操作，将添加以下突出显示的代码中**CustomActionFilter**类。

    (代码段- *ASP.NET MVC 4 自定义操作筛选器-Ex1 LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting**方法就使用**实体框架**添加新的 ActionLog 注册。 它会创建并填充新的实体实例的上下文信息从**filterContext**。
    > 
    > 你可以阅读更多有关**ControllerContext**类在[msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx)。

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>任务 2-将代码拦截器注入到存储控制器类

在此任务将通过注入到所有控制器类和将记录的控制器操作添加自定义筛选器。 本练习中，为了存储控制器类将具有一个日志。

该方法**OnActionExecuting**从**ActionLogFilterAttribute**在调用插入的元素时，将运行自定义筛选器。

还有可能截获特定控制器方法。

1. 打开**StoreController**在**MvcMusicStore\Controllers**并添加对引用**筛选器**命名空间：

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. 插入自定义筛选器**CustomActionFilter**到**StoreController**类通过添加**[CustomActionFilter]**之前类声明的属性。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

    > [!NOTE]
    > 当筛选器注入到控制器类时，是还将插入的所有操作。 如果你想要应用筛选器仅针对一组操作，你将不得不注入**[CustomActionFilter]**到每个：
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>任务 3-运行应用程序

在此任务中，将测试的日志记录筛选器正常工作。 将启动应用程序，请访问应用商店，，然后你将检查记录的活动。

1. 按 **F5** 运行该应用程序。
2. 浏览到**/ActionLog**若要查看日志视图初始状态：

    ![登录页的活动发生之前的跟踪器状态](aspnet-mvc-4-custom-action-filters/_static/image3.png "登录页的活动发生之前的跟踪器状态")

    *页上的活动发生之前的日志跟踪器状态*

    > [!NOTE]
    > 默认情况下，它将始终显示检索菜单现有风格时生成的一个项。
    > 
    > 为了简单起见我们正在清理**ActionLog**表每次应用程序运行时，因此它将仅显示每个特定任务的验证的日志。
    > 
    > 你可能需要删除以下代码从**会话\_启动**方法 (在**Global.asax**类)，以便保存对在存储区中执行的所有操作的历史日志控制器。
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. 单击其中一个**风格**从菜单并执行某些操作，如浏览可用唱片集。
4. 浏览到**/ActionLog**和如果日志已空按**F5**刷新该页面。 请检查跟踪，您的访问：

    ![与活动记录的操作日志](aspnet-mvc-4-custom-action-filters/_static/image4.png "与活动记录的操作日志")

    *与活动记录的操作日志*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>练习 2： 管理多个操作筛选器

在本练习将将第二个自定义操作筛选器添加到 StoreController 类，并定义将在其中执行两个筛选器的特定顺序。 然后将更新全局注册的筛选器的代码。

有定义的筛选器的执行顺序时，需要考虑的不同选项。 例如，Order 属性和筛选器的作用域:

你可以定义**作用域**对于每个筛选器，例如，你无法确定作用域的所有操作筛选器中运行**控制器都范围**，和所有授权筛选器中运行**全局作用都域**. 作用域具有定义的执行顺序。

此外，每个操作筛选器具有顺序属性用于确定筛选器的作用域中的执行顺序。

有关自定义操作筛选器执行顺序的详细信息，请访问此 MSDN 文章: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx))。

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>任务 1： 创建新的自定义操作筛选器

在此任务中，你将创建新的自定义操作筛选器将插入 StoreController 类中，学习如何管理筛选器的执行顺序。

1. 打开**开始**解决方案位于**\Source\Ex02-ManagingMultipleActionFilters\Begin**文件夹。 否则，可能会继续使用**结束**解决方案获取通过完成上一练习。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

        > [!NOTE]
        > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
        > 
        > 有关详细信息，请参阅此文章： [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)。
2. 添加一个新 C# 类到**筛选器**文件夹并将其命名*MyNewCustomActionFilter.cs*
3. 打开**MyNewCustomActionFilter.cs**并添加对引用**System.Web.Mvc**和**MvcMusicStore.Models**命名空间：

    (代码段- *ASP.NET MVC 4 自定义操作筛选器-Ex2 MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. 默认类声明替换为以下代码。

    (代码段- *ASP.NET MVC 4 自定义操作筛选器-Ex2 MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > 此自定义操作筛选器是与你在上一练习中创建几乎相同的。 主要区别是它有*&quot;记录通过&quot;*更新与此新类的名称以标识 wich 筛选器属性注册日志。

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>任务 2： 将新的代码拦截器注入到 StoreController 类

在此任务中，将将新的自定义筛选器添加到 StoreController 类，并运行解决方案，以验证如何两个筛选器协同工作。

1. 打开**StoreController**类位于**MvcMusicStore\Controllers**和插入新的自定义筛选器**MyNewCustomActionFilter**到**StoreController**类，如下面的代码所示。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. 现在，运行应用程序，以便了解这些两个自定义操作筛选器的工作。 若要执行此操作，请按**F5**并等待，直到应用程序启动。
3. 浏览到**/ActionLog**若要查看日志视图初始状态。

    ![登录页的活动发生之前的跟踪器状态](aspnet-mvc-4-custom-action-filters/_static/image5.png "登录页的活动发生之前的跟踪器状态")

    *页上的活动发生之前的日志跟踪器状态*
4. 单击其中一个**风格**从菜单并执行某些操作，如浏览可用唱片集。
5. 检查此时间;两次跟踪，您的访问： 后中添加自定义操作筛选器的每个**StorageController**类。

    ![与活动记录的操作日志](aspnet-mvc-4-custom-action-filters/_static/image6.png "与活动记录的操作日志")

    *与活动记录的操作日志*
6. 关闭浏览器。

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>任务 3： 管理筛选器排序

在此任务中，你将了解如何通过顺序属性进行管理的筛选器的执行顺序。

1. 打开**StoreController**类位于**MvcMusicStore\Controllers**并指定**顺序**中两个筛选器属性如下面所示。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. 现在，验证如何根据其顺序属性的值执行的筛选器。 您会发现具有最小的顺序值的筛选器 (**CustomActionFilter**) 是执行的第一个。 按**F5**并等待，直到应用程序启动。
3. 浏览到**/ActionLog**若要查看日志视图初始状态。

    ![登录页的活动发生之前的跟踪器状态](aspnet-mvc-4-custom-action-filters/_static/image7.png "登录页的活动发生之前的跟踪器状态")

    *页上的活动发生之前的日志跟踪器状态*
4. 单击其中一个**风格**从菜单并执行某些操作，如浏览可用唱片集。
5. 检查此期间，您的访问中跟踪按筛选器的顺序值进行排序： **CustomActionFilter**日志的第一个。

    ![与活动记录的操作日志](aspnet-mvc-4-custom-action-filters/_static/image8.png "与活动记录的操作日志")

    *与活动记录的操作日志*
6. 现在，你将更新的筛选器的顺序值，并验证日志记录顺序将如何变化。 在**StoreController**类中，更新的筛选器的顺序值，如下面所示。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. 再次运行该应用程序，通过按**F5**。
8. 单击其中一个**风格**从菜单并执行某些操作，如浏览可用唱片集。
9. 检查此期间，日志是否创建通过**MyNewCustomActionFilter**筛选器显示在最前面。

    ![与活动记录的操作日志](aspnet-mvc-4-custom-action-filters/_static/image9.png "与活动记录的操作日志")

    *与活动记录的操作日志*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>任务 4： 注册全局筛选器

在此任务中，你将更新解决方案以注册新的筛选器 (**MyNewCustomActionFilter**) 作为全局筛选器。 通过执行此操作，将会触发的所有操作执行应用程序中并不只在如下所示的上一任务 StoreController 的。

1. 在**StoreController**类中，删除**[MyNewCustomActionFilter]**属性和顺序属性从**[CustomActionFilter]**。 其外观应如下所示：

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. 打开**Global.asax**文件并找到**应用程序\_启动**方法。 请注意，每次在应用程序启动它正在注册全局筛选器，通过调用**RegisterGlobalFilters**方法内的**FilterConfig**类。

    ![在 Global.asax 中注册全局筛选器](aspnet-mvc-4-custom-action-filters/_static/image10.png "在 Global.asax 中注册全局筛选器")

    *在 Global.asax 中注册全局筛选器*
3. 打开**FilterConfig.cs**文件内**应用\_启动**文件夹。
4. 添加对使用 System.Web.Mvc; 的引用使用 MvcMusicStore.Filters;命名空间。

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. 更新**RegisterGlobalFilters**添加自定义的筛选器的方法。 若要执行此操作，添加突出显示的代码：

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. 通过按运行该应用程序**F5**。
7. 单击其中一个**风格**从菜单并执行某些操作，如浏览可用唱片集。
8. 检查该现在**[MyNewCustomActionFilter]**被注入到 HomeController 和 ActionLogController 过。

    ![与活动记录的操作日志](aspnet-mvc-4-custom-action-filters/_static/image11.png "与活动记录的操作日志")

    *具有全局活动记录的操作日志*

> [!NOTE]
> 此外，你可以部署此应用程序对 Windows Azure 网站以下[附录 b： 发布 ASP.NET MVC 4 应用程序使用 Web Deploy](#AppendixB)。


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>摘要

通过完成本动手实验中，你已学习如何扩展操作筛选器以执行自定义操作。 你已学习如何插入到页控制器的任何筛选器。 使用以下概念：

- 如何使用 ASP.NET MVC ActionFilterAttribute 类创建自定义操作筛选器
- 如何将筛选器注入到 ASP.NET MVC 控制器
- 如何管理筛选器排序使用的顺序属性
- 如何注册全局筛选器

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附录 a： 安装 Visual Studio Express 2012 for Web

你可以安装**Microsoft Visual Studio Express 2012 for Web**或另一个&quot;Express&quot;版本使用 **[Microsoft Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)**. 以下说明将指导你完成安装所需的步骤*Visual studio Express 2012 for Web*使用*Microsoft Web 平台安装程序*。

1. 转到[ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。 或者，如果你已安装 Web 平台安装程序，你可以打开它，并搜索产品&quot; *Visual Studio Express 2012 for Web 的 Windows Azure SDK*&quot;。
2. 单击**立即安装**。 如果你没有**Web 平台安装程序**将重定向以下载并请先安装它。
3. 一次**Web 平台安装程序**处于打开状态，单击**安装**以启动安装程序。

    ![安装 Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "安装 Visual Studio Express")

    *安装 Visual Studio Express*
4. 阅读所有产品的许可证和条款，然后单击**我接受**以继续。

    ![接受许可条款](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *接受许可条款*
5. 等待，直到下载和安装过程完成。

    ![安装进度](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *安装进度*
6. 当安装完成后时，单击**完成**。

    ![安装已完成](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *安装已完成*
7. 单击**退出**以关闭 Web 平台安装程序。
8. 若要打开 Visual Studio Express for Web，请转到**启动**屏幕并开始编写&quot; **VS Express**&quot;，然后单击**VS Express for Web**磁贴。

    ![Web 磁贴的 VS Express](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    *Web 磁贴的 VS Express*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>附录 b： 发布 ASP.NET MVC 4 应用程序使用 Web 部署

本附录将演示如何从 Windows Azure 管理门户创建新的网站和发布应用程序获取按照本实验中，利用 Windows Azure 提供的 Web 部署发布功能。

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>任务 1-创建新的 Web 站点从 Windows Azure 门户

1. 转到[Windows Azure 管理门户](https://manage.windowsazure.com/)并使用与你的订阅关联的 Microsoft 凭据登录。

    > [!NOTE]
    > 使用 Windows Azure 可以免费承载 10 个 ASP.NET 网站，然后随着流量增长而扩展。 你可以注册[此处](http://aka.ms/aspnet-hol-azure)。

    ![登录到 Windows Azure 门户](aspnet-mvc-4-custom-action-filters/_static/image17.png "登录到 Windows Azure 门户")

    *登录到 Windows Azure 管理门户*
2. 单击**新建**命令栏上。

    ![创建新网站](aspnet-mvc-4-custom-action-filters/_static/image18.png "创建新网站")

    *创建新网站*
3. 单击**计算** | **网站**。 然后选择**快速创建**选项。 为新网站提供可用的 URL，然后单击**创建网站**。

    > [!NOTE]
    > Windows Azure 网站是在云中，你可以控制和管理运行的 web 应用程序的宿主。 快速创建选项，可将已完成的 web 应用程序到 Windows Azure 网站从在门户外部部署。 它不包括用于设置数据库的步骤。

    ![创建新的网站使用快速创建](aspnet-mvc-4-custom-action-filters/_static/image19.png "创建新的网站使用快速创建")

    *创建新的网站使用快速创建*
4. 等到新**网站**创建。
5. 创建网站后单击下的链接**URL**列。 检查新的 Web 站点工作。

    ![浏览到新的 web 站点](aspnet-mvc-4-custom-action-filters/_static/image20.png "浏览到新的 web 站点")

    *浏览到新的 web 站点*

    ![运行网站](aspnet-mvc-4-custom-action-filters/_static/image21.png "运行的网站")

    *运行的网站*
6. 返回到门户并单击在网站的名称**名称**列以显示的管理页。

    ![打开网站管理页](aspnet-mvc-4-custom-action-filters/_static/image22.png "打开网站管理页")

    *打开网站管理页*
7. 在**仪表板**页上，在**速览**部分中，单击**下载发布配置文件**链接。

    > [!NOTE]
    > *发布配置文件*包含所有 web 应用程序发布到 Windows Azure 网站中的每个启用的发布方法所需的信息。 发布配置文件包含的 Url、 用户凭据和连接到并针对每个发布方法启用的终结点进行身份验证所需的数据库字符串。 **Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**和**Microsoft Visual Studio 2012**支持读取发布配置文件以自动执行的这些程序，以便配置web 应用程序发布到 Windows Azure 网站。

    ![下载网站发布配置文件](aspnet-mvc-4-custom-action-filters/_static/image23.png "下载网站发布配置文件")

    *下载网站发布配置文件*
8. 到一个已知位置下载发布配置文件。 进一步在本练习中您将了解如何使用此文件来发布 Visual Studio 中的 web 应用程序到 Windows Azure 网站。

    ![保存发布配置文件](aspnet-mvc-4-custom-action-filters/_static/image24.png "保存发布配置文件")

    *保存发布配置文件*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>任务 2-配置数据库服务器

如果你的应用程序将使用 SQL Server 数据库将需要创建 SQL 数据库服务器。 如果你想要部署的简单应用程序不使用 SQL Server 可能会跳过此任务。

1. 需要将用于存储应用程序数据库的 SQL 数据库服务器。 你可以从你的订阅在 Windows Azure 管理门户中查看 SQL 数据库服务器**Sql 数据库** | **服务器** | **服务器的仪表板**。 如果你没有创建的服务器，则可以创建一个使用**添加**命令栏上的按钮。 请记下的**服务器名称和 URL、 管理员登录名和密码**，如你将在接下来的任务中使用它们。 不创建数据库，它将在后面的阶段中创建。

    ![SQL 数据库服务器仪表板](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database 服务器仪表板")

    *SQL 数据库服务器仪表板*
2. 在下一个任务将测试从 Visual Studio 中，数据库连接，因此你需要在的服务器的列表中包括你的本地 IP 地址**允许的 IP 地址**。 若要做到这一点，请单击**配置**，选择的 IP 地址从**当前客户端 IP 地址**并将其粘贴在**起始 IP 地址**和**结束 IP 地址**文本框和单击![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png)按钮。

    ![添加客户端 IP 地址](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *添加客户端 IP 地址*
3. 一次**客户端 IP 地址**添加到允许的 IP 地址列表中，单击**保存**以确认所做的更改。

    ![确认做的更改](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *确认做的更改*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>任务 3-发布 ASP.NET MVC 4 应用程序使用 Web 部署

1. 返回到 ASP.NET MVC 4 解决方案。 在**解决方案资源管理器**，右键单击网站项目，然后选择**发布**。

    ![发布应用程序](aspnet-mvc-4-custom-action-filters/_static/image29.png "发布应用程序")

    *发布此网站*
2. 导入发布配置文件保存在第一个任务。

    ![导入发布配置文件](aspnet-mvc-4-custom-action-filters/_static/image30.png "导入发布配置文件")

    *导入发布配置文件*
3. 单击**验证连接**。 验证完成后单击**下一步**。

    > [!NOTE]
    > 请参阅验证连接按钮旁边将显示绿色的复选标记后，验证已完成。

    ![正在验证连接](aspnet-mvc-4-custom-action-filters/_static/image31.png "验证连接")

    *正在验证连接*
4. 在**设置**页上，在**数据库**部分中，单击你的数据库连接的文本框旁边的按钮 (即**DefaultConnection**)。

    ![Web 部署配置](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web 部署配置")

    *Web 部署配置*
5. 配置数据库连接，如下所示：

    - 在**服务器名称**类型 SQL 数据库服务器 URL 使用*tcp:*前缀。
    - 在**用户名**键入您的服务器管理员登录名。
    - 在**密码**键入服务器管理员登录密码。
    - 键入新的数据库名称。

    ![配置目标连接字符串](aspnet-mvc-4-custom-action-filters/_static/image33.png "配置目标连接字符串")

    *配置目标连接字符串*
6. 然后单击“确定” 。 当系统提示创建数据库单击**是**。

    ![创建数据库](aspnet-mvc-4-custom-action-filters/_static/image34.png "创建数据库字符串")

    *创建数据库*
7. 在默认连接文本框中显示了将用于连接到 Windows Azure 中的 SQL 数据库连接字符串。 然后，单击 **“下一步”**。

    ![连接字符串指向 SQL 数据库](aspnet-mvc-4-custom-action-filters/_static/image35.png "指向 SQL 数据库的连接字符串")

    *连接字符串指向 SQL 数据库*
8. 在**预览**页上，单击**发布**。

    ![Web 应用程序发布](aspnet-mvc-4-custom-action-filters/_static/image36.png "发布 web 应用程序")

    *发布 web 应用程序*
9. 完成发布过程后，默认浏览器将打开已发布的网站。

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>附录 c： 使用代码段

和代码片段，必须将你能够轻松获得所需的所有代码所示。 实验室文档将告诉您完全时你可以使用它们，如下图中所示。

![使用 Visual Studio 代码段将代码插入到你的项目](aspnet-mvc-4-custom-action-filters/_static/image37.png "使用 Visual Studio 代码片段，可将代码插入到你的项目")

*使用 Visual Studio 代码段将代码插入到你的项目*

***若要添加代码片段使用键盘 (仅限 C#)***

1. 将光标置于想要插入代码。
2. 开始键入代码段名称 （不带空格或连字符）。
3. 观看作为 IntelliSense 显示匹配的代码段的名称。
4. 选择正确的代码段 （或继续键入直到选中整个代码段的名称）。
5. 按 Tab 键两次以光标位置处插入代码段。

![开始键入代码段名称](aspnet-mvc-4-custom-action-filters/_static/image38.png "开始键入代码段名称")

*开始键入代码段名称*

![按 Tab 以选择突出显示代码段](aspnet-mvc-4-custom-action-filters/_static/image39.png "按选项卡以选择突出显示代码段")

*按 Tab 以选择突出显示代码段*

![再次按 Tab 和代码段将会扩展](aspnet-mvc-4-custom-action-filters/_static/image40.png "再次按 Tab 和代码段将会扩展")

*再次按 Tab 和代码段将会扩展*

***若要添加代码片段使用鼠标 （C#、 Visual Basic 和 XML）*** 1。 右键单击你想要插入代码段。

1. 选择**插入代码段**跟**我的代码段**。
2. 选择相关的代码段从列表中，通过单击它。

![右键单击你想要插入代码段，并选择插入代码段](aspnet-mvc-4-custom-action-filters/_static/image41.png "右键单击你想要插入代码段，并选择插入代码段")

*右键单击你想要插入代码段，并选择插入代码段*

![通过单击它选取列表中中的相关代码片段](aspnet-mvc-4-custom-action-filters/_static/image42.png "选取相关的代码段从列表中，通过单击它")

*通过单击它选取从列表中，相关代码段*
