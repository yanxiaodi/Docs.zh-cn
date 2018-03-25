---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: "ASP.NET MVC 4 基础知识 |Microsoft 文档"
author: rick-anderson
description: "此动手实验开始算起 MVC （模型视图控制器） 音乐商店的教程应用程序介绍，并说明如何使用 ASP.NET MV 分步..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: f93f51219403cd5aeca2dd3648444a84690c3d25
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-mvc-4-fundamentals"></a>ASP.NET MVC 4 基础知识

通过[Web 营地团队](https://twitter.com/webcamps)

[下载 Web 营地培训工具包](https://aka.ms/webcamps-training-kit)

此动手实验开始算起 MVC （模型视图控制器） 音乐商店的介绍，并说明如何使用 ASP.NET MVC 和 Visual Studio 分步教程应用程序。 在实验室中，你将学习简单起见，尚未电源的结合使用这些技术。 你将通过简单的应用程序启动，并且将生成它，直到有一个完全正常运行的 ASP.NET MVC 4 Web 应用。

此实验室适用于 ASP.NET MVC 4。

如果你想要浏览的教程应用程序的 ASP.NET MVC 3 版本，你可以找到它在[MVC 音乐商店](https://github.com/evilDave/MVC-Music-Store)。

此动手实验假定开发人员上可以体验可用于 Web 开发技术，如 HTML 和 JavaScript。

> [!NOTE]
> 在 Web 营地培训工具包中，在包括所有的示例代码和代码段[Microsoft 的 Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。 特定于此实验室项目位于[ASP.NET MVC 4 基础知识](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals)。

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>音乐商店应用程序

音乐商店 web 应用程序将在本实验整个生成包含三个主要部分： 购物、 结帐和管理。 访问者将能够浏览按风格的专辑、 将唱片集添加到购物车、 查看它们的选择和最后转到结帐登录并完成该订单。 此外，存储管理员将能够管理可用的专辑，以及它们的主要属性。

![音乐商店屏幕](aspnet-mvc-4-fundamentals/_static/image1.png "音乐商店屏幕")

*音乐商店屏幕*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 Essentials

音乐商店应用程序将使用生成**模型视图控制器 (MVC)**，分隔到三个主要组件的应用程序的体系结构模式：

- **模型**： 模型对象是实现域逻辑的应用程序的部件。 通常情况下，模型对象还检索和存储在数据库模型状态。
- **视图：**视图是显示应用程序的用户界面 (UI) 的组件。 通常情况下，此 UI 会从模型数据创建。 示例将显示文本框和下拉列表基于唱片集对象的当前状态的唱片集的编辑视图。
- **控制器：**控制器是处理用户交互，操作模型，并最终选择的视图来呈现的 UI 的组成部分。 在 MVC 应用程序中，视图仅显示信息；控制器处理并响应用户输入和交互。

MVC 模式可帮助你创建单独的应用程序 （输入的逻辑、 业务逻辑和 UI 逻辑），同时提供这些元素间的松散耦合的不同方面的应用程序。 这种隔离可帮助你管理复杂性，在生成应用程序，因为它使你能够专注于一次的实现的一个方面。 此外，MVC 模式可以轻松地应用程序，还鼓励测试驱动开发 (TDD) 将用于创建应用程序进行测试。

**ASP.NET MVC** framework 为创建基于 ASP.NET MVC Web 应用程序提供了 ASP.NET Web 窗体模式的替代方法。 **ASP.NET MVC**框架是一种轻型、 高度可测试演示框架的 （与基于 Web 的窗体应用程序） 与现有的 ASP.NET 功能，如母版页和基于成员资格的集成身份验证，以便获取的核心.NET framework 的所有功能。 这是因为你已经使用的所有库也可用 ASP.NET MVC 4 中，您还已熟悉 ASP.NET Web 窗体的情况下很有用。

此外，MVC 应用程序的三个主要组件之间的松耦合还可并行开发。 例如，一位开发人员可以处理视图、 控制器逻辑，可以处理第二个开发人员和第三个开发人员可以专注于模型中的业务逻辑。

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>目标

在本动手实验中，你将了解如何：

- 基于音乐应用商店应用程序教程从头开始创建 ASP.NET MVC 应用程序
- 添加控制器来处理在主页上的站点和浏览其主要功能的 Url
- 添加自定义其样式以及显示的内容视图
- 添加模型类来包含和管理数据和域逻辑
- 使用视图模型模式以将信息从控制器操作传递到视图模板
- 浏览 internet 应用程序的 ASP.NET MVC 4 新模板

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>系统必备

你必须具有要完成本实验的以下项：

- [Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (读取[附录 A](#AppendixA)有关如何安装它的说明)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>安装

**安装代码片段**

为方便起见，你将沿此实验室管理大部分都是代码的可用作 Visual Studio 代码段。 若要安装运行的代码段**.\Source\Setup\CodeSnippets.vsi**文件。

如果你不熟悉 Visual Studio 代码段，并想要了解如何使用它们，你可以从该文档引用的附录&quot;[附录 c： 使用代码段](#AppendixC)&quot;。

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>练习

此动手实验包含通过在以下练习：

1. [练习 1： 创建 MusicStore ASP.NET MVC Web 应用程序项目](#Exercise1)
2. [练习 2： 创建控制器](#Exercise2)
3. [练习 3： 将参数传递给控制器](#Exercise3)
4. [练习 4： 创建视图](#Exercise4)
5. [练习 5： 创建视图模型](#Exercise5)
6. [练习 6： 视图中使用参数](#Exercise6)
7. [练习 7: ASP.NET MVC 4 新模板浏览](#Exercise7)

> [!NOTE]
> 每个练习均附带由**结束**包含生成您应该完成练习后获得的解决方案文件夹。 如果你需要通过在练习工作的更多帮助，可以使用此解决方案作为指南。


估计时间来完成该实验： **60 分钟**。

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>练习 1： 创建 MusicStore ASP.NET MVC Web 应用程序项目

在此练习中，你将了解如何在 Visual Studio 2012 Express 中创建 ASP.NET MVC 应用程序，用于 Web，以及其主文件夹组织。 此外，你将学习如何添加新的控制器，并使其在应用程序的主页中显示一个简单的字符串。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>任务 1-创建 ASP.NET MVC Web 应用程序项目

1. 在此任务中，你将创建一个空的 ASP.NET MVC 应用程序项目，它使用 MVC Visual Studio 模板。 启动**VS Express for Web**。
2. 在“文件”菜单上，单击“新建项目”。
3. 在**新项目**对话框框中，选择**ASP.NET MVC 4 Web 应用程序**项目类型，位于**Visual C#，** **Web**模板列表。
4. 更改**名称**到*MvcMusicStore*。
5. 设置在新解决方案的位置**开始**文件夹在本练习的源文件夹中，例如**[YOUR HOL 路径] \Source\Ex01-CreatingMusicStoreProject\Begin**。 单击 **“确定”**。

    ![创建新项目对话框](aspnet-mvc-4-fundamentals/_static/image2.png "创建新项目对话框")

    *创建新项目对话框*
6. 在**新建 ASP.NET MVC 4 项目**对话框框中，选择**基本**模板并确保**视图引擎**所选**Razor**。 单击 **“确定”**。

    ![新的 ASP.NET MVC 4 项目对话框](aspnet-mvc-4-fundamentals/_static/image3.png "新建 ASP.NET MVC 4 项目对话框")

    *新的 ASP.NET MVC 4 项目对话框*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>任务 2-浏览解决方案结构

ASP.NET MVC framework 包含可帮助您创建 Web 应用程序支持的 MVC 模式的 Visual Studio 项目模板。 此模板创建新的 ASP.NET MVC Web 应用程序使用所需的文件夹、 项模板和配置文件条目。

在此任务中，你将检查该解决方案结构，以了解涉及的元素和它们之间的关系。 以下文件夹包含在所有 ASP.NET MVC 应用程序，因为默认情况下的 ASP.NET MVC framework 使用&quot;配置约定&quot;方法，而使某些默认假设基于文件夹命名约定。

1. 创建项目后，查看已在右侧的解决方案资源管理器中创建的文件夹结构：

    ![在解决方案资源管理器的 ASP.NET MVC 文件夹结构](aspnet-mvc-4-fundamentals/_static/image4.png "解决方案资源管理器中的 ASP.NET MVC 文件夹结构")

    *在解决方案资源管理器的 ASP.NET MVC 文件夹结构*

    1. **控制器**。 此文件夹将包含控制器类。 在基于 MVC 的应用中，控制器负责处理最终用户交互、 操作模型，以及最终选择的视图来呈现的 UI。

        > [!NOTE]
        > MVC framework 要求以结尾的所有控制器的名称&quot;控制器&quot;-例如，HomeController、 LoginController 或 ProductController。
    2. **模型**。 表示 MVC Web 应用程序的应用程序模型的类，提供此文件夹。 这通常包括定义对象和与数据存储区交互的逻辑的代码。 通常情况下，实际的模型对象将在单独的类库。 但是，在创建新的应用程序时，可能包含的类，并且然后在开发周期中将它们移到单独的类库，请在以后。
    3. **视图**。 此文件夹是视图，负责显示应用程序的用户界面组件的建议的位置。 视图使用.aspx、.ascx、.cshtml 和.master 文件，除了与呈现视图相关的任何其他文件。 视图文件夹包含一个用于每个控制器; 文件夹此文件夹称为具有控制器名称前缀。 例如，如果你有一个名为的控制器**HomeController**，Views 文件夹将包含名为主页的文件夹。 默认情况下，当 ASP.NET MVC framework 加载视图，它查找具有 Views\controllerName 文件夹中的请求的视图名称的.aspx 文件 (**视图 [ControllerName] [操作].aspx**) 或 (**视图 [ControllerName][操作].cshtml**) Razor 视图。

    > [!NOTE]
    > 除了前面列出的文件夹，MVC Web 应用程序使用**Global.asax**文件以设置全局 URL 路由默认值，并使用**Web.config**文件来配置应用程序。

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>任务 3-添加 HomeController

在不使用 MVC 框架的 ASP.NET 应用程序，用户交互被组织的页面，周围和围绕引发和处理这些页面中的事件。 与此相反，对 ASP.NET MVC 应用程序的用户交互是围绕控制器和其操作方法进行组织。

另一方面，ASP.NET MVC framework 将 Url 映射到称为控制器的类。 控制器处理传入的请求、 处理用户输入和交互、 执行相应的应用程序逻辑和确定将发送回客户端的响应 （显示 HTML，下载的文件，将重定向到一个不同的 URL，等等。）。 对于显示 HTML，了控制器类通常调用要生成请求的 HTML 标记的单独视图组件。 在 MVC 应用程序中，视图仅显示信息；控制器处理并响应用户输入和交互。

在此任务中，你将添加了控制器类将处理到音乐商店站点主页的 Url。

1. 右键单击**控制器**在解决方案资源管理器，选择文件夹**添加**然后**控制器**命令：

    ![添加控制器命令](aspnet-mvc-4-fundamentals/_static/image5.png "添加控制器命令")

    *添加控制器命令*
2. **添加控制器**此时将显示对话框。 控制器*HomeController*按**添加**。

    ![添加控制器对话框](aspnet-mvc-4-fundamentals/_static/image6.png "添加控制器对话框")

    *添加控制器对话框*
3. 文件**HomeController.cs**中创建**控制器**文件夹。 为了具有**HomeController**其索引操作上返回一个字符串，请更换**索引**方法替换为以下代码：

    (代码段- *ASP.NET MVC 4 基础-Ex1 HomeController 索引*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>任务 4-运行应用程序

在此任务中，你还将尝试在 web 浏览器应用程序。

1. 按**F5**运行该应用程序。 编译项目，并在本地 IIS Web 服务器将启动。 本地 IIS Web 服务器将自动打开 web 浏览器指向 Web 服务器的 URL。

    ![Web 浏览器中运行的应用程序](aspnet-mvc-4-fundamentals/_static/image7.png "web 浏览器中运行的应用程序")

    *Web 浏览器中运行的应用程序*

    > [!NOTE]
    > 本地 IIS Web 服务器将上一个随机免费端口号来运行网站。 在上图中，站点运行在`http://localhost:50103/`，因此它正在使用端口 50103。 端口号可能会有所不同。
2. 关闭浏览器。

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>练习 2： 创建控制器

在此练习中，你将了解如何更新控制器以实现简单的音乐商店应用程序的功能。 该控制器将定义用于处理每个以下的特定请求的操作方法：

- 音乐商店中音乐风格列表页面
- 列出所有特定流派音乐集浏览页
- 显示有关特定音乐唱片集的信息的详细信息页

对于本练习的作用域，这些操作将只返回一个字符串到目前为止。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>任务 1-添加新 StoreController 类

在此任务中，你将添加新的控制器。

1. 如果尚未打开，启动**VS Express for Web 2012**。
2. 在**文件**菜单上，选择**打开项目**。 在打开项目对话框中，浏览到**Source\Ex02 CreatingAController\Begin**，选择**Begin.sln**单击**打开**。 或者，你也可以继续使用该解决方案完成上一练习后获得。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
3. 添加新控制器。 要执行此操作，请右键单击**控制器**在解决方案资源管理器，选择文件夹**添加**然后**控制器**命令。 更改**控制器名称**到*StoreController*，然后单击**添加**。

    ![添加控制器对话框](aspnet-mvc-4-fundamentals/_static/image8.png "添加控制器对话框")

    *添加控制器对话框*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>任务 2-修改 StoreController 的操作

在此任务中，你将要修改调用控制器方法**操作**。 操作负责处理 URL 请求和确定应发送回浏览器或调用 URL 的用户的内容。

1. **StoreController**类已经**索引**方法。 你将使用它更高版本在本实验室中来实现页，其中列出所有的音乐商店风格。 现在，只需将**索引**方法替换为以下代码返回字符串的&quot;Hello from Store.Index()&quot;:

    (代码段- *ASP.NET MVC 4 基础-Ex2 StoreController 索引*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. 添加**浏览**和**详细信息**方法。 若要执行此操作，将添加到下面的代码**StoreController**:

    (代码段- *ASP.NET MVC 4 基础-Ex2 StoreController BrowseAndDetails*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>任务 3-运行应用程序

在此任务中，你还将尝试在 web 浏览器应用程序。

1. 按**F5**运行该应用程序。
2. 在启动项目**主页**页。 更改 URL 以验证每个操作的实现。

    1. **/ 存储**。 你将看到 **&quot;Hello from Store.Index()&quot;**。
    2. **/ 应用商店/浏览**。 你将看到 **&quot;Hello from Store.Browse()&quot;**。
    3. **/ 存储/详细信息**。 你将看到 **&quot;Hello from Store.Details()&quot;**。

        ![浏览 StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "浏览 StoreBrowse")

        *浏览 /Store/Browse*
3. 关闭浏览器。

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>练习 3： 将参数传递给控制器

到目前为止，你具有已返回的常量字符串从控制器。 在本练习中，你将了解如何将参数传递到的控制器使用的 URL 和查询字符串，，然后再进行对浏览器响应文本的方法操作。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>任务 1-添加 StoreController 流派参数

在此任务中，你将使用**querystring**以发送到的参数**浏览**中的操作方法**StoreController**。

1. 如果尚未打开，启动**VS Express for Web**。
2. 在**文件**菜单上，选择**打开项目**。 在打开项目对话框中，浏览到**Source\Ex03 PassingParametersToAController\Begin**，选择**Begin.sln**单击**打开**。 或者，你也可以继续使用该解决方案完成上一练习后获得。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
3. 打开**StoreController**类。 为此，请在**解决方案资源管理器**，展开**控制器**文件夹并双击**StoreController.cs**。
4. 更改**浏览**方法，添加要为特定流派请求的字符串参数。 ASP.NET MVC 将自动传递任何查询字符串或窗体发布参数名为**流派**给此操作方法调用时。 若要执行此操作，请替换**浏览**方法替换为以下代码：

    (代码段- *ASP.NET MVC 4 基础-Ex3 StoreController BrowseMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

    > [!NOTE]
    > 你使用**HttpUtility.HtmlEncode**到实用程序方法会阻止用户将 Javascript 注入到视图中，使用的链接  **/存储/浏览？流派 =&lt;脚本&gt;window.location=[http://hackersite.com](http://hackersite.com)&lt;/&gt;**。
    > 
    > 有关更多说明，请访问[此 msdn 文章](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx)。

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>任务 2-运行应用程序

在此任务中，将尝试使用 web 浏览器中的应用程序并使用**流派**参数。

1. 按**F5**运行该应用程序。
2. 在启动项目**主页**页。 将 URL 更改为  */存储/浏览？流派 = Disco*以验证操作接收流派参数。

    ![浏览 StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "浏览 StoreBrowseGenre = Disco")

    *Browsing /Store/Browse?Genre=Disco*
3. 关闭浏览器。

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>任务 3-添加在 URL 中嵌入一个 Id 参数

在此任务中，你将使用**URL**传递**Id**参数**详细信息**操作方法**StoreController**。 ASP.NET MVC 的默认路由约定是在操作方法名称后面的 url 段视为一个名为参数**Id**。如果你的操作方法具有名为 Id 参数，然后 ASP.NET MVC 将会自动传递的 URL 段向你作为参数。 在 URL 中**存储/详细信息/5**， **Id**将解释为**5**。

1. 更改**详细信息**方法**StoreController**、 添加**int**参数调用**id**。若要执行此操作，请替换**详细信息**方法替换为以下代码：

    (代码段- *ASP.NET MVC 4 基础-Ex3 StoreController DetailsMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>任务 4-运行应用程序

在此任务中，将尝试使用 web 浏览器中的应用程序并使用**Id**参数。

1. 按**F5**运行该应用程序。
2. 在启动项目**主页**页。 将 URL 更改为*/Store/Details/5*以验证操作接收 id 参数。

    ![浏览 StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "浏览 StoreDetails5")

    *浏览 /Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>练习 4： 创建视图

到目前为止你具有已返回字符串的控制器操作。 尽管是了解控制器的工作原理的有效方法，它是不实际的 Web 应用程序生成方式。 视图是为更好的方法，以生成 HTML 回浏览器中使用的模板文件使用的组件。

在本练习中，你将了解如何添加布局的母版页设置常见 HTML 内容，以增强的站点和最后一个视图模板来启用 HomeController 返回 HTML 外观的样式表的模板。

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a>任务 1-修改文件\_layout.cshtml

文件**~/Views/Shared/\_layout.cshtml**允许你设置用于常见 HTML 中的整个网站中使用的模板。 在此任务将向主页和存储区域添加布局母版页与具有链接的常见标头。

1. 如果尚未打开，启动**VS Express for Web**。
2. 在**文件**菜单上，选择**打开项目**。 在打开项目对话框中，浏览到**Source\Ex04 CreatingAView\Begin**，选择**Begin.sln**单击**打开**。 或者，你也可以继续使用该解决方案完成上一练习后获得。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
3. 文件 **\_layout.cshtml**包含站点上的所有页的 HTML 容器布局。 它包括 **&lt;html&gt;**  HTML 响应中，元素以及**&lt;头&gt;**和**&lt;正文&gt;**元素。 **@RenderBody（)**在 HTML 内正文指定区域模板将能够使用动态内容填充该视图。
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. 将具有链接的常见标头添加到站点中的所有页面上的主页和应用商店区域。 若要做到这一点，请添加下面的代码下面&lt;正文&gt;语句。
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. 包括 div 呈现每一页的正文部分。 替换 **@RenderBody（)**替换为以下 higlighted 代码: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > 你知道吗？ Visual Studio 2012 具有它可以更方便地将常用的代码添加在 HTML、 代码文件和的详细信息的代码段 ！ 尝试通过键入 **&lt;div&gt;** 按**选项卡**两次以插入的完整**div**标记。

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>任务 2-添加 CSS 样式表

空项目模板包括非常简化的 CSS 文件，其中只包含用于显示基本窗体和验证消息的样式。 你将使用其他 CSS 和映像 （可能由设计器） 来增强网站的外观和感觉。

在此任务中，你将添加的 CSS 样式表定义的站点的样式。

1. CSS 文件和映像都将纳入**Source\Assets\Content**这个实验室的文件夹。 要将它们添加到应用程序，将从其内容**Windows 资源管理器**到窗口**解决方案资源管理器**在 Visual Studio Express for Web，如下所示：

    ![拖动样式内容](aspnet-mvc-4-fundamentals/_static/image12.png "拖动样式内容")

    *拖动样式内容*
2. 警告将显示一个对话框，询问是否确定要替换**Site.css**文件和某些现有的映像。 检查**应用于所有项目**单击**是**。

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>任务 3-添加视图模板

在此任务中，你将添加一个视图模板以生成将使用布局主控页的 HTML 响应和 CSS 添加在本练习中。

1. 若要浏览该网站的主页时，请使用视图模板，你将首先需要而不是返回一个字符串，则指示**HomeController 索引**方法将返回**视图**。 打开**HomeController**类并更改其**索引**方法以返回**ActionResult**，并将其返回**View()**。

    (代码段- *ASP.NET MVC 4 基础-Ex4 HomeController 索引*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. 现在，你需要添加适当的视图模板。 若要这样做，**右键单击**内**索引**操作方法，然后选择**添加视图**。 此时会弹出**添加视图**对话框。

    ![添加从索引方法内的某个视图](aspnet-mvc-4-fundamentals/_static/image13.png "添加从索引方法内的某个视图")

    *添加从索引方法内的某个视图*
3. **添加视图**对话框将显示生成的视图模板文件。 默认情况下，此对话框预填充视图模板的名称，使其匹配将使用它的操作方法。 因为你使用**添加视图**中的上下文菜单**索引**内 HomeController，操作方法**添加视图**对话框有作为默认视图名称的索引。 单击 **“添加”**。

    ![添加视图对话框](aspnet-mvc-4-fundamentals/_static/image14.png "添加视图对话框")

    *添加视图对话框*
4. Visual Studio 将生成**Index.cshtml**中的视图模板**views/home**文件夹，然后打开它。

    ![主索引视图创建](aspnet-mvc-4-fundamentals/_static/image15.png "主页索引视图创建")

    *创建的主索引视图*

    > [!NOTE]
    > 名称和位置**Index.cshtml**文件是相关，并遵循的默认 ASP.NET MVC 命名约定。
    > 
    > 文件夹 \Views\**主页** 与控制器名称匹配 (**主页**控制器)。 视图模板名称 (**索引**)，与匹配的控制器操作方法，这将会显示该视图。
    > 
    > 这种方式，可避免 ASP.NET MVC，无需显式指定的名称或视图模板的位置时使用此命名约定来返回的视图。
5. 生成的视图模板基于 **\_layout.cshtml**前面定义的模板。 更新将 ViewBag.Title 属性设为**主页**，并更改到主要内容**这就是起始页**，下面的代码中所示：


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. 选择**MvcMusicStore**项目中的解决方案资源管理器和按**F5**运行该应用程序。

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>任务 4： 验证

若要验证已正确执行所有步骤在前面的练习，请继续执行，如下所示：

在浏览器中打开应用程序后，你应该注意:

1. HomeController 的 Index 操作方法找到并显示**\Views\Home\Index.cshtml**查看模板，即使代码调用**返回 View()**，因为视图模板后接标准的命名约定。
2. 主页上显示中定义的欢迎消息**\Views\Home\Index.cshtml**查看模板。
3. 使用主页 **\_layout.cshtml**模板，因此，在欢迎消息包含在标准站点 HTML 布局。

    ![主页使用定义 LayoutPage 和样式的索引视图](aspnet-mvc-4-fundamentals/_static/image16.png "主页使用定义 LayoutPage 和样式的索引视图")

    *使用定义 LayoutPage 和样式的主索引视图*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>练习 5： 创建视图模型

目前为止，进行硬编码 HTML，显示您的视图，但若要创建动态 web 应用程序，视图模板应从控制器接收信息。 要用于此目的的一个常见方法是**ViewModel**模式，允许要打包生成相应的 HTML 响应所需的所有信息的控制器。

在本练习中，您将学习如何创建 ViewModel 类并添加所需的属性： 风格商店和这些风格的列表中的数。 你还将更新 StoreController 用于创建视图模型，并且最后，你将创建一个新的视图模板将在页中显示所述的属性。

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>任务 1-创建 ViewModel 类

在此任务中，你将创建将实现存储风格列表方案 ViewModel 类。

1. 如果尚未打开，启动**VS Express for Web**。
2. 在**文件**菜单上，选择**打开项目**。 在打开项目对话框中，浏览到**Source\Ex05 CreatingAViewModel\Begin**，选择**Begin.sln**单击**打开**。 或者，你也可以继续使用该解决方案完成上一练习后获得。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
3. 创建**Viewmodel**要用于保存 ViewModel 文件夹。 要执行此操作，请右键单击顶级**MvcMusicStore**项目，依次选择**添加**然后**新文件夹**。

    ![添加一个新文件夹](aspnet-mvc-4-fundamentals/_static/image17.png "添加一个新文件夹")

    *添加一个新文件夹*
4. 将文件夹命名为*Viewmodel*。

    ![在解决方案资源管理器的 Viewmodel 文件夹](aspnet-mvc-4-fundamentals/_static/image18.png "Viewmodel 文件夹在解决方案资源管理器")

    *在解决方案资源管理器的 Viewmodel 文件夹*
5. 创建**ViewModel**类。 若要执行此操作，右键单击**Viewmodel**文件夹最近创建的选择**添加**然后**新项**。 下**代码**，选择**类**项并将该文件命名*StoreIndexViewModel.cs*，然后单击**添加**。

    ![添加新类](aspnet-mvc-4-fundamentals/_static/image19.png "添加新类")

    *添加新类*

    ![创建 StoreIndexViewModel 类](aspnet-mvc-4-fundamentals/_static/image20.png "创建 StoreIndexViewModel 类")

    *创建 StoreIndexViewModel 类*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>任务 2-将属性添加到 ViewModel 类

有两个参数来传递从 StoreController 到视图模板以生成预期的 HTML 响应： 风格商店和这些风格的列表中的数。

在此任务中，你将添加到这些 2 属性**StoreIndexViewModel**类： **NumberOfGenres** （整数） 和**风格**（的字符串列表）。

1. 添加**NumberOfGenres**和**风格**属性设置为**StoreIndexViewModel**类。 若要执行此操作，请向类定义添加下面的两行代码：

    (代码段- *ASP.NET MVC 4 基础-Ex5 StoreIndexViewModel 属性*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

    > [!NOTE]
    > **{获取; 设置;}**表示法将使用 C# 的自动实现的属性功能。 它而无需我们可以声明一个后备字段提供一个属性的优点。

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>任务 3-更新 StoreController 使用 StoreIndexViewModel

**StoreIndexViewModel**类封装从将传递所需的信息**StoreController**的**索引**到视图模板以生成响应的方法.

在此任务中，你将更新**StoreController**使用**StoreIndexViewModel**。

1. 打开**StoreController**类。

    ![打开 StoreController 类](aspnet-mvc-4-fundamentals/_static/image21.png "打开 StoreController 类")

    *打开 StoreController 类*
2. 若要使用**StoreIndexViewModel**类**StoreController**，在顶部添加以下命名空间**StoreController**代码：

    (代码段- *ASP.NET MVC 4 基础-使用 Viewmodel Ex5 StoreIndexViewModel*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. 更改**StoreController**的**索引**操作方法，以便它创建和填充**StoreIndexViewModel**对象，然后将其传递到的视图模板生成它的 HTML 响应。

    > [!NOTE]
    > 在实验室中&quot;ASP.NET MVC 模型和数据访问&quot;你将编写从数据库中检索的应用商店风格列表的代码。 在下面的代码中，你将创建**列表**将填充的虚拟数据风格的**StoreIndexViewModel**。
    > 
    > 创建和设置后**StoreIndexViewModel**对象，它将作为自变量传递**视图**方法。 这表示视图模板将使用该对象来生成它的 HTML 响应。
4. 替换**索引**方法替换为以下代码：

    (代码段- *ASP.NET MVC 4 基础-Ex5 StoreController Index 方法*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

    > [!NOTE]
    > 如果你熟悉 C#，你可能会假定使用**var**意味着**viewModel**变量是后期绑定。 不正确-C# 编译器使用类型推理基于分配给变量来确定， **viewModel**属于类型**StoreIndexViewModel**。 此外，通过编译本地**viewModel**变量作为**StoreIndexViewModel** get 编译时检查和 Visual Studio 代码编辑器支持类型。

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>任务 4-创建使用 StoreIndexViewModel 的视图模板

在此任务中，你将创建将用于从控制器传递 StoreIndexViewModel 对象显示的风格列表视图模板。

1. 在创建新的视图模板之前, 让我们来生成项目，以便**添加视图对话框**就会了解有关**StoreIndexViewModel**类。 通过选择生成项目**生成**菜单项，然后**生成 MvcMusicStore**。

    ![生成项目](aspnet-mvc-4-fundamentals/_static/image22.png "生成项目")

    *生成项目*
2. 创建一个新的视图模板。 为此，请右键单击内**索引**方法，然后选择**添加视图**。

    ![添加视图](aspnet-mvc-4-fundamentals/_static/image23.png "添加视图")

    *添加视图*
3. 因为**添加视图对话框**从调用**StoreController**，它将在默认情况下添加的视图模板**\Views\Store\Index.cshtml**文件。 检查**创建强类型化的视图**复选框，然后选择**StoreIndexViewModel**作为**模型类**。 此外，请确保选择的视图引擎是**Razor**。 单击 **“添加”**。

    ![添加视图对话框](aspnet-mvc-4-fundamentals/_static/image24.png "添加视图对话框")

    *添加视图对话框*

    **\Views\Store\Index.cshtml**创建视图模板文件并将其打开。 基于提供给信息**添加视图**在最后一个步骤中，模板将预期的视图的对话框**StoreIndexViewModel**实例作为要用来生成 HTML 响应数据。 你会发现模板继承`ViewPage<musicstore.viewmodels.storeindexviewmodel>`C# 中。

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>任务 5-更新视图模板

在此任务中，你将更新来检索数量风格的页内的其名称的最后一个任务中创建的视图模板。

> [!NOTE]
> 你将使用语法 @ (通常称为&quot;代码问题&quot;) 能够查看模板中执行代码。


1. 在**Index.cshtml**文件内,**存储**文件夹，其代码替换为以下代码：


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > 一旦完成键入后单词期间**模型**，Visual Studio Intellisense 将显示可能的属性和方法可供选择的列表。
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *获取模型属性和方法具有 Visual Studio 的 IntelliSense*
    > 
    > **模型**属性引用**StoreIndexViewModel**控制器传递给视图模板对象。 这意味着，你可以访问的所有数据从控制器传递给视图模板通过**模型**属性，并设置其格式到相应的 HTML 响应中查看模板。
    > 
    > 只需选择即可**NumberOfGenres**属性从智能感知列表而不是键入它，然后它将自动完成它按**tab 键**。
2. 循环访问中的 genre 列表**StoreIndexViewModel**并创建一个 HTML  **&lt;ul&gt;** 列表使用**foreach**循环。
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. 按**F5**运行该应用程序和浏览**应用商店**。 您将看到的传入的风格列表**StoreIndexViewModel**对象**StoreController**到视图模板。

    ![显示的风格的列表视图](aspnet-mvc-4-fundamentals/_static/image26.png "视图显示的风格的列表")

    *显示的风格的列表视图*
4. 关闭浏览器。

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>练习 6： 视图中使用参数

在练习 3 中你学习了如何将参数传递到的控制器。 在此练习中，你将了解如何在视图模板中使用这些参数。 出于这个目的，将向你介绍第一次将帮助你管理你的数据和域的逻辑的模型类。 此外，您将学习如何创建的 ASP.NET MVC 应用程序中的页面链接安心的编码的 URL 路径等。

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>任务 1-添加模型类

与 Viewmodel 创建只是为了将从控制器的信息传递给视图，不同模型类而生成包含和管理数据和域的逻辑。 在此任务中，你将添加两个模型类来表示这些概念：**流派**和**唱片集**。

1. 如果尚未打开，启动**VS Express for Web**
2. 在**文件**菜单上，选择**打开项目**。 在打开项目对话框中，浏览到**Source\Ex06 UsingParametersInView\Begin**，选择**Begin.sln**单击**打开**。 或者，你也可以继续使用该解决方案完成上一练习后获得。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
3. 添加**流派**模型类。 要执行此操作，请右键单击**模型**文件夹中的**解决方案资源管理器**，选择**添加**然后**新项**选项。 下**代码**，选择**类**项并将该文件命名*Genre.cs*，然后单击**添加**。

    ![添加类](aspnet-mvc-4-fundamentals/_static/image27.png "添加类")

    *添加新项*

    ![添加流派模型类](aspnet-mvc-4-fundamentals/_static/image28.png "添加流派模型类")

    *添加流派模型类*
4. 添加**名称**流派类属性。 若要执行此操作，添加以下代码：

    (代码段- *ASP.NET MVC 4 基础-Ex6 流派*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. 按照相同的过程之前，添加**唱片集**类。 要执行此操作，请右键单击**模型**文件夹中的**解决方案资源管理器**，选择**添加**然后**新项**选项。 下**代码**，选择**类**项并将该文件命名*Album.cs*，然后单击**添加**。
6. 将两个属性添加到唱片集类：**流派**和**标题**。 若要执行此操作，添加以下代码：

    (代码段- *ASP.NET MVC 4 基础-Ex6 唱片集*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>任务 2-添加 StoreBrowseViewModel

A **StoreBrowseViewModel**将用于在此任务中显示匹配所选的风格唱片集。 在此任务中，你将创建此类，然后添加两个属性来处理**流派**及其**唱片集**的列表。

1. 添加**StoreBrowseViewModel**类。 要执行此操作，请右键单击**Viewmodel**文件夹中的**解决方案资源管理器**，选择**添加**然后**新项**选项。 下**代码**，选择**类**项并将该文件命名*StoreBrowseViewModel.cs*，然后单击**添加**。
2. 添加对中的模型的引用**StoreBrowseViewModel**类。 若要执行此操作，将添加以下使用命名空间：

    (代码段- *ASP.NET MVC 4 基础-Ex6 UsingModel*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. 添加到两个属性**StoreBrowseViewModel**类：**流派**和**专辑**。 若要执行此操作，添加以下代码：

    (代码段- *ASP.NET MVC 4 基础-Ex6 ModelProperties*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

    > [!NOTE]
    > 什么是**列表&lt;唱片集&gt;** ？: 使用此定义**列表&lt;T&gt;** 类型，其中**T**约束类型设置为此哪些元素**列表**属于，在这种情况下**唱片集**（或其任意后代）。
    > 
    > 这种设计类和方法的操作延迟的一个或多个类型的规范的类或方法声明，并且由客户端代码实例化是一项功能的 C# 语言的能力称为**泛型**。
    > 
    > **列表&lt;T&gt;** 泛型等效于**ArrayList**键入和位于**System.Collections.Generic**命名空间。 使用的好处之一**泛型**是，由于指定的类型，不需要处理的类型检查操作，例如强制转换到的元素**唱片集**需要像一样**ArrayList**。

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>任务 3-在 StoreController 中使用新视图模型

在此任务中，你将要修改**StoreController**的**浏览**和**详细信息**操作方法，以使用新**StoreBrowseViewModel**.

1. 添加对中的 Models 文件夹的引用**StoreController**类。 若要执行此操作，展开**控制器**文件夹中的**解决方案资源管理器**并打开**StoreController**类。 然后添加以下代码：

    (代码段- *ASP.NET MVC 4 基础-Ex6 UsingModelInController*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. 替换**浏览**要使用的操作方法**StoreViewBrowseController**类。 你将创建一种风格和两个新的专辑对象具有虚拟数据 （在下一步的动手实验中你将使用数据库中的实际数据）。 若要执行此操作，请替换**浏览**方法替换为以下代码：

    (代码段- *ASP.NET MVC 4 基础-Ex6 BrowseMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. 替换**详细信息**要使用的操作方法**StoreViewBrowseController**类。 你将创建一个新**唱片集**对象返回给**视图**。 若要执行此操作，请替换**详细信息**方法替换为以下代码：

    (代码段- *ASP.NET MVC 4 基础-Ex6 DetailsMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>任务 4-添加浏览视图模板

在此任务中，你将添加**浏览**视图，以显示特定流派找到唱片集。

1. 在创建新的视图模板之前, 应生成项目，以便**添加视图**对话框就会了解有关**ViewModel**要使用的类。 通过选择生成项目**生成**菜单项，然后**生成 MvcMusicStore**。
2. 添加**浏览**视图。 要执行此操作，右键单击**浏览**操作方法**StoreController**单击**添加视图**。
3. 在**添加视图**对话框框中，验证是否视图名称**浏览**。 检查**创建强类型化视图**复选框，然后选择**StoreBrowseViewModel**从**模型类**下拉列表。 将其他字段保留其默认值。 然后单击 **“添加”**。

    ![添加浏览视图](aspnet-mvc-4-fundamentals/_static/image29.png "添加浏览视图")

    *添加浏览视图*
4. 修改**Browse.cshtml**以显示流派的信息，请访问**StoreBrowseViewModel**传递给视图模板对象。 若要执行此操作，将内容替换为以下: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>任务 5-运行应用程序

在此任务中，你将测试**浏览**方法检索从专辑**浏览**方法操作。

1. 按**F5**运行该应用程序。
2. 在主页页面中启动项目。 将 URL 更改为  **/存储/浏览？流派 = Disco**以验证操作，返回两个唱片集。

    ![浏览应用商店 Disco 专辑](aspnet-mvc-4-fundamentals/_static/image30.png "浏览应用商店 Disco 专辑")

    *浏览应用商店 Disco 专辑*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>任务 6-显示信息有关的特定唱片集

在此任务中，你将实现**商店/详细信息**视图以显示有关特定唱片集的信息。 在本动手实验中，将显示有关唱片集的所有内容已包含在**视图**模板。 这样，而不是创建**StoreDetailsViewModel**类，你将使用当前**StoreBrowseViewModel**将唱片集传递给它的模板。

1. 关闭浏览器，如果需要可以返回到 Visual Studio 窗口。 添加新**详细信息**查看**StoreController**的**详细信息**操作方法。 要执行此操作，请右键单击**详细信息**中的方法**StoreController**类并单击**添加视图**。
2. 在**添加视图**对话框中，验证**视图名称**是**详细信息**。 检查**创建强类型化视图**复选框，然后选择**唱片集**从**模型类**下拉列表。 将其他字段保留其默认值。 然后单击 **“添加”**。 这将创建并打开**\Views\Store\Details.cshtml**文件。

    ![添加详细信息视图](aspnet-mvc-4-fundamentals/_static/image31.png "添加详细信息视图")

    *添加详细信息视图*
3. 修改**Details.cshtml**文件以显示唱片集的信息，请访问**唱片集**传递给视图模板对象。 若要执行此操作，将内容替换为以下: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>任务 7-运行应用程序

在此任务中，你将测试**详细信息**视图检索唱片集的信息从**详细说明了操作**方法。

1. 按**F5**运行该应用程序。
2. 在启动项目**主页**页。 将 URL 更改为**/Store/Details/5**验证唱片集的信息。

    ![浏览唱片集详细信息](aspnet-mvc-4-fundamentals/_static/image32.png "浏览唱片集详细信息")

    *浏览唱片集的详细信息*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>任务 8-添加页面之间的链接

在此任务中，你将添加要为相应的每个风格名称中有一个链接的存储区视图中的链接**/存储/浏览**URL。 这样一来，例如单击一种风格时**Disco**，它将会定位到**/存储/浏览？ 流派 = Disco** URL。

1. 关闭浏览器，如果需要可以返回到 Visual Studio 窗口。 更新**索引**页后，可以添加一个链接到**浏览**页。 为此，请在**解决方案资源管理器**展开**视图**文件夹，则**存储**文件夹并双击**Index.cshtml**页。
2. 指示所选风格浏览视图中添加的链接。 若要执行此操作，请将以下突出显示的代码中 **&lt;li&gt;** 标记: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

    > [!NOTE]
    > 另一种方法将直接链接的页，代码如下所示：
    > 
    > &lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;
    > 
    > 尽管此方法适用，则它将依赖于硬编码字符串。 如果你更高版本重命名控制器，你将需要手动更改此指令。 更好的替代方法是使用**的 HTML 帮助器**方法。 ASP.NET MVC 包括一个 HTML 帮助器方法，这是可用于此类任务。 **Html.ActionLink()**帮助器方法，可以轻松地生成 HTML  **&lt;&gt;** 链接，并确保 URL 路径正确进行 URL 编码。
    > 
    > Htlm.ActionLink 具有好几个重载。 在本练习中，你将使用一个采用三个参数：
    > 
    > 1. 链接文本，将显示流派名称
    > 2. 控制器操作名称 (**浏览**)
    > 3. 路由参数值，指定这两个名称 (**流派**) 和值 (**流派名称**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>任务 9-运行应用程序

在此任务中，你将测试的相应的链接显示了每个风格**/存储/浏览**URL。

1. 按**F5**运行该应用程序。
2. 在主页页面中启动项目。 将 URL 更改为**应用商店**验证每个风格链接到相应**/存储/浏览**URL。

    ![其中包含指向浏览页浏览风格](aspnet-mvc-4-fundamentals/_static/image33.png "其中包含指向浏览页浏览风格")

    *其中包含指向浏览页浏览风格*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>任务 10-使用动态 ViewModel 集合来传递值

在此任务中，你将学习的简单且功能强大的方法不在模型中进行任何更改的情况下将控制器和视图之间的值。 ASP.NET MVC 4 提供的集合&quot;ViewModel&quot;，这可以分配给任何动态值并在控制器和视图以及访问。

你现在将使用 ViewBag 动态集合的列表传递&quot;**星形风格**&quot;从到视图控制器。 将访问存储索引视图**ViewModel** ，并显示信息。

1. 关闭浏览器，如果需要可以返回到 Visual Studio 窗口。 打开**StoreController.cs**和修改**索引**方法来创建一份到 ViewModel 集合星形风格：


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > 你也可以使用语法**ViewBag [&quot;Starred&quot;]**访问属性。
2. 星形图标 **&quot;starred.png&quot;** 纳入**Source\Assets\Images**这个实验室的文件夹。 要将其添加到应用程序，将从其内容**Windows 资源管理器**到窗口**解决方案资源管理器**Visual Web Developer 学习版中，如下所示：

    ![添加到解决方案的星型映像](aspnet-mvc-4-fundamentals/_static/image34.png "添加到解决方案的星型映像")

    *将星型映像添加到解决方案*
3. 打开视图**Store/Index.cshtml**并修改的内容。 将读取&quot;星形&quot;中的属性**ViewBag**集合，并要求当前流派名称是否在列表中。 在这种情况下将显示从右到流派链接星形图标。
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>任务 11-运行应用程序

在此任务中，你将测试标有星号的风格显示星形图标。

1. 按**F5**运行该应用程序。
2. 在启动项目**主页**页。 将 URL 更改为**应用商店**以验证每个特色的风格有 respecting 标签：

    ![使用标有星号元素浏览风格](aspnet-mvc-4-fundamentals/_static/image35.png "浏览风格标有星号的元素")

    *使用标有星号元素浏览风格*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>练习 7: ASP.NET MVC 4 新模板浏览

在此练习中，您将了解在 ASP.NET MVC 4 项目模板中的增强功能讲到新模板的最相关的功能。

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>任务 1： 浏览 ASP.NET MVC 4 Internet 应用程序模板

1. 如果尚未打开，启动**VS Express for Web**
2. 选择**文件 |新 |项目**菜单命令。 在**新项目**对话框中，选择**Visual C# |Web**左窗格中的模板树，并选择**ASP.NET MVC 4 Web 应用程序**。 **名称**项目*MusicStore*和更新**解决方案名称**到*开始*，然后选择一个位置 （或保留默认值），然后单击**确定**.

    ![创建新的 ASP.NET MVC 4 项目](aspnet-mvc-4-fundamentals/_static/image36.png "创建新的 ASP.NET MVC 4 项目")

    *创建新的 ASP.NET MVC 4 项目*
3. 在**新建 ASP.NET MVC 4 项目**对话框中，选择**Internet 应用程序**项目模板，然后单击**确定**。 请注意你可以选择 Razor 或 ASPX 作为视图引擎。

    ![新建 ASP.NET MVC 4 Internet 应用程序](aspnet-mvc-4-fundamentals/_static/image37.png "新建 ASP.NET MVC 4 Internet 应用程序")

    *新建 ASP.NET MVC 4 Internet 应用程序*

    > [!NOTE]
    > ASP.NET MVC 3 中引入了 razor 语法。 其目标是字符和在文件中，需要启用快速和流体编码工作流的击键的数量降至最低。 Razor 利用现有 C# /VB （或其他） 语言技能和提供使出色的 HTML 构造流的模板标记语法。
4. 按**F5**若要运行解决方案并查看续订的模板。 你可以查看以下功能：

    1. **现代样式模板**

        已续订的模板，提供更多的外观现代的样式。

        ![ASP.NET MVC 4 其样式模板](aspnet-mvc-4-fundamentals/_static/image38.png "其样式模板的 ASP.NET MVC 4")

        *ASP.NET MVC 4 其样式模板*
    2. **自适应呈现**

        签出调整浏览器窗口的大小，请注意如何页面布局可以动态地适应新的窗口大小。 这些模板使用的自适应呈现技术在没有任何自定义项的桌面和移动平台中正确呈现。

        ![在不同的浏览器大小中的 ASP.NET MVC 4 项目模板](aspnet-mvc-4-fundamentals/_static/image39.png "中不同的浏览器大小的 ASP.NET MVC 4 项目模板")

        *在不同的浏览器大小中的 ASP.NET MVC 4 项目模板*
5. 关闭浏览器以停止调试器并返回到 Visual Studio。
6. 现在，你就能够浏览解决方案和签出引入的一些新功能由 ASP.NET MVC 4 项目模板中。

    ![ASP.NET MVC4-internet 的应用程序的项目的模板](aspnet-mvc-4-fundamentals/_static/image40.png "ASP.NET MVC 4 Internet 应用程序项目模板")

    *ASP.NET MVC 4 Internet 应用程序项目模板*

    1. **HTML5 标记**

        浏览模板视图若要了解的新的主题标记，例如打开**About.cshtml**内查看**主页**文件夹。

        ![使用 Razor 和 HTML5 标记的新模板](aspnet-mvc-4-fundamentals/_static/image41.png "使用 Razor 和 HTML5 标记的新模板")

        *使用 Razor 和 HTML5 标记的新模板*
    2. **包含的 JavaScript 库**

        1. **jQuery**: jQuery 简化了 HTML 文档遍历、 事件处理，对进行动画处理，和 Ajax 交互。
        2. **jQuery UI**： 此库提供低级别交互和动画，高级效果和受主题影响小组件，基于 jQuery JavaScript 库的抽象。

            > [!NOTE]
            > 你可以了解有关 jQuery 和 jQuery UI 中[ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/)。
        3. **KnockoutJS**: ASP.NET MVC 4 默认模板现在包括**KnockoutJS**，JavaScript MVVM 框架，它允许你创建使用 JavaScript 和 HTML 的丰富且响应度高的 web 应用程序。 如在 ASP.NET MVC 3 中，jQuery 和 jQuery UI 库也包括在 ASP.NET MVC 4。

            > [!NOTE]
            > 你可以获取有关此链接中的 KnockOutJS 库的详细信息： [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)。
        4. **Modernizr**： 此库自动运行，使您的网站与较旧的浏览器兼容，在使用 HTML5 和 CSS3 技术时。

            > [!NOTE]
            > 你可以获取有关此链接中的 Modernizr 库的详细信息： [http://www.modernizr.com/](http://www.modernizr.com/)。
    3. **解决方案中包含的 SimpleMembership**

        SimpleMembership 旨在以取代以前的 ASP.NET 角色和成员资格提供程序系统。 它具有许多新功能，能够更轻松地开发人员的安全 web 页面以更灵活的方式。

        Internet 模板已具有设置需要集成 SimpleMembership 的一些事项，例如，AccountController 中已准备好使用 OAuthWebSecurity （适用于 OAuth 帐户注册、 登录、 管理等） 和 Web 安全。

        ![解决方案中包含 SimpleMembership](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership 解决方案中包含")

        *解决方案中包含 SimpleMembership*

        > [!NOTE]
        > 查找更多有关[OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) MSDN 中。

> [!NOTE]
> 此外，你可以部署此应用程序对 Windows Azure 网站以下[附录 b： 发布 ASP.NET MVC 4 应用程序使用 Web Deploy](#AppendixB)。


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>摘要

通过完成本动手实验中，你已经学习了 ASP.NET MVC 的基础知识：

- 核心元素的 MVC 应用程序和它们交互的方式
- 如何创建 ASP.NET MVC 应用程序
- 如何添加和配置控制器来处理参数传递的 URL 和查询字符串
- 如何添加布局的母版页设置有关常见的 HTML 内容，以增强外观和感觉和查看模板，以显示 HTML 内容的样式表模板
- 如何使用用于将属性传递给视图模板 ViewModel 模式显示动态信息
- 如何使用传递给控制器视图模板中的参数
- 如何将链接添加到 ASP.NET MVC 应用程序中的页面
- 如何添加和使用在视图中的动态属性
- ASP.NET MVC 4 项目模板中的增强功能

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附录 a： 安装 Visual Studio Express 2012 for Web

你可以安装**Microsoft Visual Studio Express 2012 for Web**或另一个&quot;Express&quot;版本使用 **[Microsoft Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)**. 以下说明将指导你完成安装所需的步骤*Visual studio Express 2012 for Web*使用*Microsoft Web 平台安装程序*。

1. 转到[ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。 或者，如果你已安装 Web 平台安装程序，你可以打开它，并搜索产品&quot; *Visual Studio Express 2012 for Web 的 Windows Azure SDK*&quot;。
2. 单击**立即安装**。 如果你没有**Web 平台安装程序**将重定向以下载并请先安装它。
3. 一次**Web 平台安装程序**处于打开状态，单击**安装**以启动安装程序。

    ![安装 Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "安装 Visual Studio Express")

    *安装 Visual Studio Express*
4. 阅读所有产品的许可证和条款，然后单击**我接受**以继续。

    ![接受许可条款](aspnet-mvc-4-fundamentals/_static/image44.png)

    *接受许可条款*
5. 等待，直到下载和安装过程完成。

    ![安装进度](aspnet-mvc-4-fundamentals/_static/image45.png)

    *安装进度*
6. 当安装完成后时，单击**完成**。

    ![安装已完成](aspnet-mvc-4-fundamentals/_static/image46.png)

    *安装已完成*
7. 单击**退出**以关闭 Web 平台安装程序。
8. 若要打开 Visual Studio Express for Web，请转到**启动**屏幕并开始编写&quot; **VS Express**&quot;，然后单击**VS Express for Web**磁贴。

    ![Web 磁贴的 VS Express](aspnet-mvc-4-fundamentals/_static/image47.png)

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

    ![登录到 Windows Azure 门户](aspnet-mvc-4-fundamentals/_static/image48.png "登录到 Windows Azure 门户")

    *登录到 Windows Azure 管理门户*
2. 单击**新建**命令栏上。

    ![创建新网站](aspnet-mvc-4-fundamentals/_static/image49.png "创建新网站")

    *创建新网站*
3. 单击**计算** | **网站**。 然后选择**快速创建**选项。 为新网站提供可用的 URL，然后单击**创建网站**。

    > [!NOTE]
    > Windows Azure 网站是在云中，你可以控制和管理运行的 web 应用程序的宿主。 快速创建选项，可将已完成的 web 应用程序到 Windows Azure 网站从在门户外部部署。 它不包括用于设置数据库的步骤。

    ![创建新的网站使用快速创建](aspnet-mvc-4-fundamentals/_static/image50.png "创建新的网站使用快速创建")

    *创建新的网站使用快速创建*
4. 等到新**网站**创建。
5. 创建网站后单击下的链接**URL**列。 检查新的 Web 站点工作。

    ![浏览到新的 web 站点](aspnet-mvc-4-fundamentals/_static/image51.png "浏览到新的 web 站点")

    *浏览到新的 web 站点*

    ![运行网站](aspnet-mvc-4-fundamentals/_static/image52.png "运行的网站")

    *运行的网站*
6. 返回到门户并单击在网站的名称**名称**列以显示的管理页。

    ![打开网站管理页](aspnet-mvc-4-fundamentals/_static/image53.png "打开网站管理页")

    *打开网站管理页*
7. 在**仪表板**页上，在**速览**部分中，单击**下载发布配置文件**链接。

    > [!NOTE]
    > *发布配置文件*包含所有 web 应用程序发布到 Windows Azure 网站中的每个启用的发布方法所需的信息。 发布配置文件包含的 Url、 用户凭据和连接到并针对每个发布方法启用的终结点进行身份验证所需的数据库字符串。 **Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**和**Microsoft Visual Studio 2012**支持读取发布配置文件以自动执行的这些程序，以便配置web 应用程序发布到 Windows Azure 网站。

    ![下载网站发布配置文件](aspnet-mvc-4-fundamentals/_static/image54.png "下载网站发布配置文件")

    *下载网站发布配置文件*
8. 到一个已知位置下载发布配置文件。 进一步在本练习中您将了解如何使用此文件来发布 Visual Studio 中的 web 应用程序到 Windows Azure 网站。

    ![保存发布配置文件](aspnet-mvc-4-fundamentals/_static/image55.png "保存发布配置文件")

    *保存发布配置文件*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>任务 2-配置数据库服务器

如果你的应用程序将使用 SQL Server 数据库将需要创建 SQL 数据库服务器。 如果你想要部署的简单应用程序不使用 SQL Server 可能会跳过此任务。

1. 需要将用于存储应用程序数据库的 SQL 数据库服务器。 你可以从你的订阅在 Windows Azure 管理门户中查看 SQL 数据库服务器**Sql 数据库** | **服务器** | **服务器的仪表板**。 如果你没有创建的服务器，则可以创建一个使用**添加**命令栏上的按钮。 请记下的**服务器名称和 URL、 管理员登录名和密码**，如你将在接下来的任务中使用它们。 不创建数据库，它将在后面的阶段中创建。

    ![SQL 数据库服务器仪表板](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database 服务器仪表板")

    *SQL 数据库服务器仪表板*
2. 在下一个任务将测试从 Visual Studio 中，数据库连接，因此你需要在的服务器的列表中包括你的本地 IP 地址**允许的 IP 地址**。 若要做到这一点，请单击**配置**，选择的 IP 地址从**当前客户端 IP 地址**并将其粘贴在**起始 IP 地址**和**结束 IP 地址**文本框和单击![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png)按钮。

    ![添加客户端 IP 地址](aspnet-mvc-4-fundamentals/_static/image58.png)

    *添加客户端 IP 地址*
3. 一次**客户端 IP 地址**添加到允许的 IP 地址列表中，单击**保存**以确认所做的更改。

    ![确认做的更改](aspnet-mvc-4-fundamentals/_static/image59.png)

    *确认做的更改*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>任务 3-发布 ASP.NET MVC 4 应用程序使用 Web 部署

1. 返回到 ASP.NET MVC 4 解决方案。 在**解决方案资源管理器**，右键单击网站项目，然后选择**发布**。

    ![发布应用程序](aspnet-mvc-4-fundamentals/_static/image60.png "发布应用程序")

    *发布此网站*
2. 导入发布配置文件保存在第一个任务。

    ![导入发布配置文件](aspnet-mvc-4-fundamentals/_static/image61.png "导入发布配置文件")

    *导入发布配置文件*
3. 单击**验证连接**。 验证完成后单击**下一步**。

    > [!NOTE]
    > 请参阅验证连接按钮旁边将显示绿色的复选标记后，验证已完成。

    ![正在验证连接](aspnet-mvc-4-fundamentals/_static/image62.png "验证连接")

    *正在验证连接*
4. 在**设置**页上，在**数据库**部分中，单击你的数据库连接的文本框旁边的按钮 (即**DefaultConnection**)。

    ![Web 部署配置](aspnet-mvc-4-fundamentals/_static/image63.png "Web 部署配置")

    *Web 部署配置*
5. 配置数据库连接，如下所示：

    - 在**服务器名称**类型 SQL 数据库服务器 URL 使用*tcp:*前缀。
    - 在**用户名**键入您的服务器管理员登录名。
    - 在**密码**键入服务器管理员登录密码。
    - 键入新的数据库名称，例如： *MVC4SampleDB*。

    ![配置目标连接字符串](aspnet-mvc-4-fundamentals/_static/image64.png "配置目标连接字符串")

    *配置目标连接字符串*
6. 然后单击“确定” 。 当系统提示创建数据库单击**是**。

    ![创建数据库](aspnet-mvc-4-fundamentals/_static/image65.png "创建数据库字符串")

    *创建数据库*
7. 在默认连接文本框中显示了将用于连接到 Windows Azure 中的 SQL 数据库连接字符串。 然后，单击 **“下一步”**。

    ![连接字符串指向 SQL 数据库](aspnet-mvc-4-fundamentals/_static/image66.png "指向 SQL 数据库的连接字符串")

    *连接字符串指向 SQL 数据库*
8. 在**预览**页上，单击**发布**。

    ![Web 应用程序发布](aspnet-mvc-4-fundamentals/_static/image67.png "发布 web 应用程序")

    *发布 web 应用程序*
9. 完成发布过程后，默认浏览器将打开已发布的网站。

    ![应用程序发布到 Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "应用程序发布到 Windows Azure")

    *应用程序发布到 Windows Azure*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>附录 c： 使用代码段

和代码片段，必须将你能够轻松获得所需的所有代码所示。 实验室文档将告诉您完全时你可以使用它们，如下图中所示。

![使用 Visual Studio 代码段将代码插入到你的项目](aspnet-mvc-4-fundamentals/_static/image69.png "使用 Visual Studio 代码片段，可将代码插入到你的项目")

*使用 Visual Studio 代码段将代码插入到你的项目*

***若要添加代码片段使用键盘 (仅限 C#)***

1. 将光标置于想要插入代码。
2. 开始键入代码段名称 （不带空格或连字符）。
3. 观看作为 IntelliSense 显示匹配的代码段的名称。
4. 选择正确的代码段 （或继续键入直到选中整个代码段的名称）。
5. 按 Tab 键两次以光标位置处插入代码段。

![开始键入代码段名称](aspnet-mvc-4-fundamentals/_static/image70.png "开始键入代码段名称")

*开始键入代码段名称*

![按 Tab 以选择突出显示代码段](aspnet-mvc-4-fundamentals/_static/image71.png "按选项卡以选择突出显示代码段")

*按 Tab 以选择突出显示代码段*

![再次按 Tab 和代码段将会扩展](aspnet-mvc-4-fundamentals/_static/image72.png "再次按 Tab 和代码段将会扩展")

*再次按 Tab 和代码段将会扩展*

***若要添加代码片段使用鼠标 （C#、 Visual Basic 和 XML）*** 1。 右键单击你想要插入代码段。

1. 选择**插入代码段**跟**我的代码段**。
2. 选择相关的代码段从列表中，通过单击它。

![右键单击你想要插入代码段，并选择插入代码段](aspnet-mvc-4-fundamentals/_static/image73.png "右键单击你想要插入代码段，并选择插入代码段")

*右键单击你想要插入代码段，并选择插入代码段*

![通过单击它选取列表中中的相关代码片段](aspnet-mvc-4-fundamentals/_static/image74.png "选取相关的代码段从列表中，通过单击它")

*通过单击它选取从列表中，相关代码段*
