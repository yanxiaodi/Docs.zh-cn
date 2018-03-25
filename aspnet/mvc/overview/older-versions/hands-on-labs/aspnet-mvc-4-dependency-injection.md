---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: "ASP.NET MVC 4 依赖关系注入 |Microsoft 文档"
author: rick-anderson
description: "注意： 此动手实验假定你具有的 ASP.NET MVC 和 ASP.NET MVC 4 的筛选器的基本知识。 如果你没有使用 ASP.NET MVC 4 筛选器之前，我们建议..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: c735bbdafe4b8f0423abb6bacd076f173a1be9d8
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-mvc-4-dependency-injection"></a>ASP.NET MVC 4 依赖关系注入

通过[Web 营地团队](https://twitter.com/webcamps)

[下载 Web 营地培训工具包](https://aka.ms/webcamps-training-kit)

此动手实验假定你具有的基础知识**ASP.NET MVC**和**ASP.NET MVC 4 筛选**。 如果您未使用过**ASP.NET MVC 4 筛选**之前，我们建议你转到**ASP.NET MVC 自定义操作筛选器**动手实验。

> [!NOTE]
> 所有的示例代码和代码段都包含在 Web 营地培训工具包，可从在[Microsoft 的 Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。 特定于此实验室项目位于[ASP.NET MVC 4 依赖关系注入](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection)。

在**对象面向编程**范例，对象协同工作的协作模型其中有 contributors （参与者） 和使用者。 当然，此通信模型生成对象和组件，变得难以管理复杂性增加时之间的依赖关系。

![类依赖项和模型复杂性](aspnet-mvc-4-dependency-injection/_static/image1.png "类依赖项和模型复杂性")

*类依赖项和模型复杂性*

你可能听说**工厂模式**和接口和实现使用服务，往往负责服务位置的客户端对象之间的分离。

依赖关系注入模式是控制反向的特定实现。 **反向 (IoC) 控件的**意味着对象不会创建其他对象，它们依赖来完成其工作。 相反，它们获得所需从外部源 （例如，xml 配置文件） 的对象。

**依赖关系注入 (DI)**意味着这是通过无需对象干预，通常将构造函数参数传递一个 framework 组件，设置属性。

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>依赖关系注入 (DI) 设计模式

在高级别中，依赖关系注入的目的在于客户端类 (例如*golfer*) 需要满足接口的内容 (例如*IClub*)。 它不介意具体类型是什么 (例如*WoodClub，IronClub，WedgeClub*或*PutterClub*)，它想其他人来处理这种情况 (例如良好*托盘*)。 ASP.NET mvc 依赖项解析程序可以让你能够注册你的依赖项逻辑的其他位置 (例如容器或*俱乐部包*)。

![依赖关系注入图](aspnet-mvc-4-dependency-injection/_static/image2.png "依赖关系注入图")

*依赖关系注入-高尔夫类比*

使用依赖关系注入模式和控制反向的优点如下所示：

- 减少类耦合
- 会增加代码重用
- 提高代码可维护性
- 改进了应用程序测试

> [!NOTE]
> 依赖关系注入有时与抽象工厂设计模式中，但这两种方法之间存在细微的差异。 DI 具有后面致力于解决通过调用工厂和注册的服务的依赖关系的框架。


现在，你已了解依赖关系注入模式，你将学习在本实验整个如何将其应用于 ASP.NET MVC 4。 你将开始使用中的依赖关系注入**控制器**中包含数据库访问服务。 接下来，将应用到的依赖关系注入**视图**使用服务并显示信息。 最后，你将扩展 DI 到 ASP.NET MVC 4 筛选器，将注入解决方案中的自定义操作筛选器。

在本动手实验中，你将了解如何：

- 将 ASP.NET MVC 4 与 Unity 集成为使用 NuGet 包的依赖关系注入
- 使用一个 ASP.NET MVC 控制器内部的依赖关系注入
- 使用 ASP.NET MVC 视图内的依赖关系注入
- 使用 ASP.NET MVC 操作筛选器内的依赖关系注入

> [!NOTE]
> 此实验室使用的依赖项解析为 Unity.Mvc3 NuGet 包，但可以调整任何依赖关系注入框架，用于 ASP.NET MVC 4。


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

如果你不熟悉 Visual Studio 代码段，并想要了解如何使用它们，你可以从该文档引用的附录&quot;[附录 b： 使用代码段](#AppendixB)&quot;。

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>练习

此动手实验包含通过在以下练习：

1. [练习 1： 将注入控制器](#Exercise1)
2. [练习 2： 将注入视图](#Exercise2)
3. [练习 3： 将注入筛选器](#Exercise3)

> [!NOTE]
> 每个练习均附带由**结束**包含生成您应该完成练习后获得的解决方案文件夹。 如果你需要通过在练习工作的更多帮助，可以使用此解决方案作为指南。


估计时间来完成该实验： **30 分钟**。

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>练习 1： 将注入控制器

在此练习中，你将了解如何在 ASP.NET MVC 控制器中的依赖关系注入使用通过集成 Unity 使用 NuGet 包。 为此，将逻辑与数据访问分开你 MvcMusicStore 控制器包括服务。 服务将在控制器构造函数，它将使用的帮助的依赖关系注入解决中创建新的依赖关系**Unity**。

此方法将显示如何生成不太耦合应用程序、 更灵活且易于维护和测试。 你还将了解如何将与 Unity 集成 ASP.NET MVC。

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>有关 StoreManager 服务

现在提供开始解决方案中的 MVC 音乐存储包含管理名为的存储控制器数据的服务**StoreService**。 你将在下面找到存储服务实现。 请注意，所有方法将都返回模型的实体。

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController**开始新的解决方案现在将使用来自**StoreService**。 从中删除所有数据引用了**StoreController**，现在可以修改当前的数据访问接口，而无需更改任何使用的方法和**StoreService**。

你将在下面找到**StoreController**实现与具有依赖关系**StoreService**内类构造函数。

> [!NOTE]
> 在本练习中引入的依赖关系与**控制反向**(IoC)。
> 
> **StoreController**类构造函数接收**IStoreService**至关重要的是要执行从在类中的服务调用的类型参数。 但是， **StoreController**未实现默认构造函数 （不带任何参数） 的任何控制器必须具有要使用 ASP.NET MVC。
> 
> 若要解决此依赖关系，控制器已由 （返回指定任何的类型对象的类） 的抽象工厂来创建。


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> 当类尝试创建 StoreController 而不发送服务对象，因为没有声明没有无参数构造函数时，会出现错误。


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>任务 1-运行应用程序

在此任务中，你将运行 Begin 应用程序，其中包括插入应用程序逻辑分离开来的数据访问的存储控制器的服务。

在运行应用程序，你将收到异常，为控制器服务不默认情况下通过作为参数：

1. 打开**开始**解决方案位于**将 Source\Ex01 注入 Controller\Begin**。

    1. 你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
2. 按**Ctrl + F5**运行该应用程序而不进行调试。 你将收到错误消息&quot;**没有此对象定义的无参数构造函数**&quot;:

    ![运行 ASP.NET MVC 开始应用程序时出错](aspnet-mvc-4-dependency-injection/_static/image3.png "运行 ASP.NET MVC 开始应用程序时出错")

    *运行 ASP.NET MVC 开始应用程序时出错*
3. 关闭浏览器。

在以下步骤将处理音乐存储解决方案，将此控制器所需的依赖关系注入。

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>任务 2-到 MvcMusicStore 解决方案包括 Unity

在此任务中，将包括**Unity.Mvc3**到解决方案的 NuGet 包。

> [!NOTE]
> Unity.Mvc3 包旨在用于 ASP.NET MVC 3，但它是与 ASP.NET MVC 4 完全兼容。
> 
> Unity 实例是具有的可选支持的轻量、 可扩展的依赖关系注入容器，并键入截获。 它是在任何类型的.NET 应用程序中使用的通用容器。 它提供了在包括依赖关系注入机制中找到的所有常见功能： 对象创建、 抽象的通过指定运行时和灵活性，依赖项推迟到容器的组件配置的要求。


1. 安装**Unity.Mvc3**中 NuGet 包**MvcMusicStore**项目。 若要执行此操作，打开**程序包管理器控制台**从**视图** | **其他窗口**。
2. 运行以下命令。

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![安装 Unity.Mvc3 NuGet 包](aspnet-mvc-4-dependency-injection/_static/image4.png "安装 Unity.Mvc3 NuGet 包")

    *安装 Unity.Mvc3 NuGet 包*
3. 一次**Unity.Mvc3**安装包，请浏览的文件和文件夹，它会自动添加为了简化 Unity 配置。

    ![安装的 Unity.Mvc3 程序包](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 包安装")

    *Unity.Mvc3 包安装*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a>任务 3-在 Global.asax.cs 应用程序中注册 Unity\_启动

在此任务中，你将更新**应用程序\_启动**方法位于**Global.asax.cs**调用 Unity 引导程序初始值设定项，然后，更新引导程序文件注册服务和控制器将用于依赖关系注入。

1. 现在，你将挂钩引导程序，这是初始化 Unity 容器的文件和依赖项解析程序。 若要执行此操作，打开**Global.asax.cs**并添加以下突出显示的代码中**应用程序\_启动**方法。

    (代码段- *ASP.NET 依赖关系注入实验-Ex01-初始化 Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. 打开**Bootstrapper.cs**文件。
3. 包括以下命名空间： **MvcMusicStore.Services**和**MusicStore.Controllers**。

    (代码段- *ASP.NET 依赖关系注入实验-Ex01-引导程序添加命名空间*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. 替换**BuildUnityContainer**方法的内容替换为以下代码来注册存储控制器和存储服务。

    (代码段- *ASP.NET 依赖关系注入实验-Ex01-注册存储控制器和服务*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>任务 4-运行应用程序

在此任务中，你将运行应用程序以验证它现在在加载包括 Unity 之后。

1. 按**F5**运行该应用程序，应用程序应现在加载而不显示任何错误消息。

    ![使用依赖关系注入运行应用程序](aspnet-mvc-4-dependency-injection/_static/image6.png "使用依赖关系注入运行应用程序")

    *使用依赖关系注入运行应用程序*
2. 浏览到**应用商店**。 这将调用**StoreController**，现在为创建使用**Unity**。

    ![MVC 音乐商店](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC 音乐商店")

    *MVC 音乐商店*
3. 关闭浏览器。

在以下练习中，您将学习如何扩展使用在 ASP.NET MVC 视图和操作筛选器内的依赖关系注入作用域。

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>练习 2： 将注入视图

在此练习中，你将学习如何使用具有 ASP.NET MVC 4 的新功能的视图中的依赖关系注入程序 Unity 集成。 若要做到这一点，您将调用自定义服务内存储浏览视图中，其中将显示一条消息和下图。

然后，将与 Unity 集成项目，并创建自定义的依赖项解析程序将依赖关系注入。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>任务 1-创建使用服务的视图

在此任务中，你将创建执行服务调用，以生成新的依赖关系的视图。 服务包含在此解决方案中包含一个简单的消息传递服务。

1. 打开**开始**解决方案位于**将 Source\Ex02 注入 View\Begin**文件夹。 否则，可能会继续使用**结束**解决方案获取通过完成上一练习。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
    > 
    > 有关详细信息，请参阅此文章： [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)。
2. 包括**MessageService.cs**和**IMessageService.cs**类位于**源 \Assets**文件夹中的**/**。 要执行此操作，请右键单击**服务**文件夹，然后选择**添加现有项**。 浏览到文件的位置，并将其包含。

    ![添加消息服务和服务接口](aspnet-mvc-4-dependency-injection/_static/image8.png "添加消息服务和服务接口")

    *添加消息服务和服务接口*

    > [!NOTE]
    > **IMessageService**接口定义两个属性由实现**MessageService**类。 这些属性-**消息**和**ImageUrl**-存储要显示的消息和图像的 URL。
3. 创建文件夹**/页**在项目的根文件夹，并将现有类**MyBasePage.cs**从**Source\Assets**。 将继承从基本页具有以下结构。

    ![Pages 文件夹](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages 文件夹")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. 打开**Browse.cshtml**查看从**/视图/存储**文件夹，并使其从继承**MyBasePage.cs**。

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. 在**浏览**视图中，添加对的调用**MessageService**要显示的映像和服务检索一条消息。
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>任务 2-包括自定义的依赖项解析程序和自定义视图页激活器

在上一任务中，你将插入的视图，用于执行其内部服务调用在新的依赖关系。 现在，你将通过实现 ASP.NET MVC 依赖关系注入接口来解决该依赖关系**IViewPageActivator**和**IDependencyResolver**。 你将在解决方案中包括的实现**IDependencyResolver** ，将通过使用 Unity 处理服务检索。 然后，你将包括的另一个自定义实现**IViewPageActivator**将解决的视图创建的接口。

> [!NOTE]
> ASP.NET MVC 3 自依赖关系注入的实现已简化这些接口，以便注册服务。 **IDependencyResolver**和**IViewPageActivator**属于依赖关系注入的 ASP.NET MVC 3 功能。
> 
> **-IDependencyResolver**接口替换以前 IMvcServiceLocator。 IDependencyResolver 的实施者必须返回的服务或服务集合的实例。
>
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator**接口提供了更好地控制细化查看页通过依赖关系注入实例化的方式。 实现的类**IViewPageActivator**接口可以创建使用上下文信息的查看实例。
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. 创建 /**工厂**项目的根文件夹中的文件夹。
2. 包括**CustomViewPageActivator.cs**到你的解决方案从**/源/资产/**到**工厂**文件夹。 为此，请右键单击**/Factories**文件夹，选择**添加 |现有项**，然后选择**CustomViewPageActivator.cs**。 此类实现**IViewPageActivator**接口来保存 Unity 容器。

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator**负责使用 Unity 容器管理视图的创建。
3. 包括**UnityDependencyResolver.cs**文件从**/源/资产**到**/Factories**文件夹。 为此，请右键单击**/Factories**文件夹，选择**添加 |现有项**，然后选择**UnityDependencyResolver.cs**文件。

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > **UnityDependencyResolver**类是自定义 DependencyResolver for Unity。 当 Unity 容器中找不到服务时，被 invocated 基冲突解决程序。

在下面的任务将注册这两个实现，以便知道服务和视图的位置的模型。

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>任务 3-在 Unity 容器内注册依赖关系注入

在此任务中，您将放入所有以前的内容在一起以进行工作的依赖关系注入。

到目前为止你的解决方案具有以下元素：

- A**浏览**继承自的视图**MyBaseClass**和使用**MessageService**。
- 中间类-**MyBaseClass**-具有依赖关系注入声明服务接口。
- 服务- **MessageService** -及其界面**IMessageService**。
- Unity 的自定义的依赖项解析程序**UnityDependencyResolver** -服务检索的处理。
- 视图页中激活器- **CustomViewPageActivator** -创建页。

若要将注入**浏览**视图中，你将立即注册自定义的依赖项解析程序在 Unity 容器中。

1. 打开**Bootstrapper.cs**文件。
2. 注册实例**MessageService**到要初始化的服务的 Unity 容器：

    (代码段- *ASP.NET 依赖关系注入实验-Ex02-注册消息服务*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. 添加对的引用**MvcMusicStore.Factories**命名空间。

    (代码段- *ASP.NET 依赖关系注入实验-Ex02-工厂 Namespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. 注册**CustomViewPageActivator**到 Unity 容器视图页中激活器作为：

    (代码段- *ASP.NET 依赖关系注入实验-Ex02-注册 CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. 将替换的实例的 ASP.NET MVC 4 默认依赖项解析程序**UnityDependencyResolver**。 若要执行此操作，请替换**初始化**方法内容替换为以下代码：

    (代码段- *ASP.NET 依赖关系注入实验-Ex02-更新依赖项解析程序*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC 提供的默认依赖项解析程序类。 若要使用与我们已为 unity 创建的自定义的依赖项解析程序，此解析程序具有要替换。

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>任务 4-运行应用程序

在此任务中，你将运行应用程序以验证存储浏览器使用该服务，并说明的映像，以及检索到的消息：

1. 按 **F5** 运行该应用程序。
2. 单击**Rock**风格菜单和，请参阅内如何**MessageService**到视图而插入和加载在欢迎消息和图像。 在此示例中，我们正在输入到&quot; **Rock**&quot;:

    ![MVC 音乐商店-视图注入](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC 音乐商店-视图注入")

    *MVC 音乐商店-视图注入*
3. 关闭浏览器。

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>练习 3： 将注入操作筛选器

在以前的动手实验**自定义操作筛选器**使用筛选器自定义项和注入过。 在此练习中，你将了解如何将筛选器依赖关系注入插入使用 Unity 容器。 为此，请将向音乐商店解决方案添加一个将跟踪的站点的活动的自定义操作筛选器。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>任务 1-包括解决方案中的跟踪筛选器

在此任务中，你将包括在音乐存储区中跟踪事件的自定义操作筛选器。 为自定义操作筛选器概念已处理在上一实验室&quot;自定义操作筛选器&quot;，你将只包括本实验室中，资产文件夹中的筛选器类，然后创建用于 Unity 的筛选器提供程序：

1. 打开**开始**解决方案位于**Source\Ex03-将注入操作 Filter\Begin**文件夹。 否则，可能会继续使用**结束**解决方案获取通过完成上一练习。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
    > 
    > 有关详细信息，请参阅此文章： [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)。
2. 包括**TraceActionFilter.cs**文件从**/源/资产**到**/筛选**文件夹。

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > 此自定义操作筛选器执行 ASP.NET 跟踪。 你可以检查&quot;ASP.NET MVC 4 本地和动态操作筛选器&quot;实验室以进行更多引用。
3. 添加空类**FilterProvider.cs**到项目的文件夹中  **/筛选器。**
4. 添加**System.Web.Mvc**和**Microsoft.Practices.Unity**中的命名空间**FilterProvider.cs**。

    (代码段- *ASP.NET 依赖关系注入实验室-Ex03-筛选器提供程序添加命名空间*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. 请从继承的类**IFilterProvider**接口。

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. 添加**IUnityContainer**中的属性**FilterProvider**类，，然后创建一个类构造函数来分配容器。

    (代码段- *ASP.NET 依赖关系注入实验室-Ex03-筛选器提供程序构造函数*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > 不创建筛选器提供程序类构造函数**新**对象内。 容器传递作为参数，并且通过 Unity 解决依赖项。
7. 在**FilterProvider**类中，实现方法**GetFilters**从**IFilterProvider**接口。

    (代码段- *ASP.NET 依赖关系注入实验室-Ex03-筛选器提供程序 GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>任务 2-注册并启用筛选器

在此任务中，你将启用站点跟踪。 为此，请将注册中的筛选器**Bootstrapper.cs BuildUnityContainer**方法以启动跟踪：

1. 打开**Web.config**位于项目根目录和 System.Web 组启用跟踪跟踪。

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. 打开**Bootstrapper.cs**项目根目录下。
3. 添加对的引用**MvcMusicStore.Filters**命名空间。

    (代码段- *ASP.NET 依赖关系注入实验室-Ex03-引导程序添加命名空间*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. 选择**BuildUnityContainer**方法和寄存器 Unity 容器中的筛选器。 你将需要注册的筛选器提供程序，以及操作筛选器。

    (代码段- *ASP.NET 依赖关系注入实验室-Ex03-注册 FilterProvider 和 ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>任务 3-运行应用程序

在此任务中，你将运行应用程序和测试自定义操作筛选器跟踪活动：

1. 按 **F5** 运行该应用程序。
2. 单击**Rock**风格菜单中。 如果你愿意，你可以浏览到更多风格。

    ![音乐商店](aspnet-mvc-4-dependency-injection/_static/image11.png "音乐商店")

    *Music 商店*
3. 浏览到**/Trace.axd**以查看应用程序跟踪页，，然后单击**查看详细信息**。

    ![应用程序跟踪日志](aspnet-mvc-4-dependency-injection/_static/image12.png "应用程序跟踪日志")

    *应用程序跟踪日志*

    ![应用程序跟踪的请求详细信息](aspnet-mvc-4-dependency-injection/_static/image13.png "跟踪应用程序的请求的详细信息")

    *应用程序跟踪的请求详细信息*
4. 关闭浏览器。

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>摘要

通过完成本动手实验中，你已学习如何在 ASP.NET MVC 4 中的依赖关系注入使用通过集成 Unity 使用 NuGet 包。 若要实现此目的，你使用控制器、 视图和操作筛选器内的依赖关系注入。

介绍了以下概念：

- ASP.NET MVC 4 依赖关系注入功能
- Unity 集成使用 Unity.Mvc3 NuGet 包
- 在控制器中的依赖关系注入
- 在视图中的依赖关系注入
- 依赖关系注入的操作筛选器

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附录 a： 安装 Visual Studio Express 2012 for Web

你可以安装**Microsoft Visual Studio Express 2012 for Web**或另一个&quot;Express&quot;版本使用 **[Microsoft Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)**. 以下说明将指导你完成安装所需的步骤*Visual studio Express 2012 for Web*使用*Microsoft Web 平台安装程序*。

1. 转到[ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。 或者，如果你已安装 Web 平台安装程序，你可以打开它，并搜索产品&quot; *Visual Studio Express 2012 for Web 的 Windows Azure SDK*&quot;。
2. 单击**立即安装**。 如果你没有**Web 平台安装程序**将重定向以下载并请先安装它。
3. 一次**Web 平台安装程序**处于打开状态，单击**安装**以启动安装程序。

    ![安装 Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "安装 Visual Studio Express")

    *安装 Visual Studio Express*
4. 阅读所有产品的许可证和条款，然后单击**我接受**以继续。

    ![接受许可条款](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *接受许可条款*
5. 等待，直到下载和安装过程完成。

    ![安装进度](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *安装进度*
6. 当安装完成后时，单击**完成**。

    ![安装已完成](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *安装已完成*
7. 单击**退出**以关闭 Web 平台安装程序。
8. 若要打开 Visual Studio Express for Web，请转到**启动**屏幕并开始编写&quot; **VS Express**&quot;，然后单击**VS Express for Web**磁贴。

    ![Web 磁贴的 VS Express](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *Web 磁贴的 VS Express*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>附录 b： 使用代码片段

和代码片段，必须将你能够轻松获得所需的所有代码所示。 实验室文档将告诉您完全时你可以使用它们，如下图中所示。

![使用 Visual Studio 代码段将代码插入到你的项目](aspnet-mvc-4-dependency-injection/_static/image19.png "使用 Visual Studio 代码片段，可将代码插入到你的项目")

*使用 Visual Studio 代码段将代码插入到你的项目*

***若要添加代码片段使用键盘 (仅限 C#)***

1. 将光标置于想要插入代码。
2. 开始键入代码段名称 （不带空格或连字符）。
3. 观看作为 IntelliSense 显示匹配的代码段的名称。
4. 选择正确的代码段 （或继续键入直到选中整个代码段的名称）。
5. 按 Tab 键两次以光标位置处插入代码段。

![开始键入代码段名称](aspnet-mvc-4-dependency-injection/_static/image20.png "开始键入代码段名称")

*开始键入代码段名称*

![按 Tab 以选择突出显示代码段](aspnet-mvc-4-dependency-injection/_static/image21.png "按选项卡以选择突出显示代码段")

*按 Tab 以选择突出显示代码段*

![再次按 Tab 和代码段将会扩展](aspnet-mvc-4-dependency-injection/_static/image22.png "再次按 Tab 和代码段将会扩展")

*再次按 Tab 和代码段将会扩展*

***若要添加代码片段使用鼠标 （C#、 Visual Basic 和 XML）*** 1。 右键单击你想要插入代码段。

1. 选择**插入代码段**跟**我的代码段**。
2. 选择相关的代码段从列表中，通过单击它。

![右键单击你想要插入代码段，并选择插入代码段](aspnet-mvc-4-dependency-injection/_static/image23.png "右键单击你想要插入代码段，并选择插入代码段")

*右键单击你想要插入代码段，并选择插入代码段*

![通过单击它选取列表中中的相关代码片段](aspnet-mvc-4-dependency-injection/_static/image24.png "选取相关的代码段从列表中，通过单击它")

*通过单击它选取从列表中，相关代码段*
