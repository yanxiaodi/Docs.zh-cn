---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: "动手实验： 使用 SignalR 实时 Web 应用程序 |Microsoft 文档"
author: rick-anderson
description: "实时 Web 应用程序功能推送服务器端的情况下，实时连接的客户端到内容的能力。 对于 ASP.NET 开发人员，ASP..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 22123a9c61e6830f3f9f66a45182e1e923950341
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a>使用 SignalR 的动手实验： 实时 Web 应用程序
====================
通过[Web 营地团队](https://twitter.com/webcamps)

[下载 Web 营地培训工具包](http://aka.ms/webcamps-training-kit)

> 实时 Web 应用程序功能推送服务器端的情况下，实时连接的客户端到内容的能力。 对于 ASP.NET 开发人员， **ASP.NET SignalR**是一个库将实时 web 功能添加到其应用程序。 它充分利用多个传输，自动选择客户端和服务器的最佳可用传输最佳的可用传输。 它利用**WebSocket**，HTML5 API，使浏览器和服务器之间的双向通信。
> 
> **SignalR**拍摄服务器到客户端 RPC 还提供简单、 高级别 API （在从服务器端.NET 代码的客户端的浏览器中调用 JavaScript 函数） 在 ASP.NET 应用程序，以及添加用于连接管理的有用挂钩如连接/断开事件、 分组连接和授权。
> 
> **SignalR**是上某些要求进行客户端和服务器之间的实时工作的传输的抽象。 A **SignalR**连接作为 HTTP，启动，并随后将提升到**WebSocket**如果可用的连接。 **WebSocket**是有关的理想传输**SignalR**，因为它使最有效的服务器内存使用具有最低的延迟，并且具有最基础的功能 (如客户端之间的全双工通信和服务器），但它也有最严格的要求： **WebSocket**要求服务器使用**Windows Server 2012**或**Windows 8**，以及**.NET framework 4.5**。 如果不满足这些要求， **SignalR**将尝试使用其他传输，以使其连接 (如*Ajax 长轮询*)。
> 
> **SignalR** API 包含用于客户端和服务器之间进行通信的两个模型：**永久连接**和**中心**。 A**连接**代表一个简单的终结点发送单个接收方组合在一起，或将消息广播。 A**中心**是基于连接 API，从而使你的客户端和服务器相互直接调用方法的更多高级管道。
> 
> ![SignalR 体系结构](real-time-web-applications-with-signalr/_static/image1.png)
> 
> 在 Web 营地培训工具包中，在包括所有的示例代码和代码段[http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)。


<a id="Overview"></a>
## <a name="overview"></a>概述

<a id="Objectives"></a>
### <a name="objectives"></a>目标

在此动手实验中，你将了解如何：

- 向使用 SignalR 客户端，从服务器发送通知。
- 横向扩展 SignalR 应用程序使用**SQL Server**。

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>系统必备

完成本动手实验需要以下：

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)或更高版本

<a id="Setup"></a>
### <a name="setup"></a>安装

若要运行本动手实验中的练习，你将需要先设置你的环境。

1. 打开 Windows 资源管理器窗口并浏览到本实验的**源**文件夹。
2. 右键单击**Setup.cmd**和选择**以管理员身份运行**以启动安装过程将配置您的环境并安装本实验的 Visual Studio 代码段。
3. 如果显示用户帐户控制对话框中，确认操作以继续。

> [!NOTE]
> 请确保您已在运行安装程序之前检查本实验的所有依赖项。


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>使用代码片段

在整个实验文档中，你将指示要插入代码块。 为方便起见，大多数的此代码提供作为 Visual Studio 代码段，你可以从访问在 Visual Studio 2013，为了避免必须手动添加它。

> [!NOTE]
> 每个练习均附带的开始解决方案位于**开始**本练习，您可以完成相互独立地每个练习的文件夹。 请注意在练习过程中添加的代码段缺少这些开始解决方案中，并且可能不工作，直到完成练习。 在练习的源代码，你将找到**结束**包含具有完成相应练习中的步骤得到的代码的 Visual Studio 解决方案文件夹。 如果您在演练本动手实验需要其他帮助，你可以使用这些解决方案作为指南。


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>练习

本动手实验包括以下练习：

1. [使用使用 SignalR 的实时数据](#Exercise1)
2. [向外扩展使用的 SQL Server](#Exercise2)

估计时间来完成该实验： **60 分钟**

> [!NOTE]
> 当你首次启动 Visual Studio 时，你必须选择一个预定义的设置集合。 每个预定义的集合用于匹配特定的开发风格和确定窗口布局、 编辑器行为、 IntelliSense 代码段和对话框选项。 在本实验中的过程描述完成给定的任务 Visual Studio 中使用时所需的操作**常规开发设置**集合。 如果您为开发环境选择不同的设置集合，则表明可能存在你应考虑到帐户的步骤中的差异。


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>练习 1： 使用使用 SignalR 的实时数据

尽管聊天经常使用作为示例，你可以执行整个许多内容与实时 Web 功能。 用户刷新网页可看到新数据或页实现 Ajax 长轮询要检索新数据，随时可以使用 SignalR。

SignalR 支持**服务器推送**或**广播**功能; 它将自动处理的连接管理。 在经典 HTTP 连接中客户端服务器通信，为每个请求，重新建立连接时，但 SignalR 提供客户端和服务器之间的持续性连接。 在服务器代码调用到使用远程过程调用 (RPC) 的浏览器中的客户端代码的 SignalR，而不是请求-响应模型我们知道今天。

在本练习中，您将配置**专家 Quiz**应用程序以使用 SignalR 显示与而无需刷新整个页面的更新度量值统计信息仪表板。

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>任务 1 – 浏览专家测验统计信息页

在此任务中，你将通过应用程序并验证统计信息页的显示方式以及可以如何改进信息的方式更新。

1. 打开**Visual Studio Express 2013 for Web**并打开**GeekQuiz.sln**解决方案位于**Source\Ex1 WorkingWithRealTimeData\Begin**文件夹。
2. 按**F5**运行该解决方案。 **登录**页应出现在浏览器。

    ![运行解决方案](real-time-web-applications-with-signalr/_static/image2.png "运行解决方案")

    *运行解决方案*
3. 单击**注册**右上角的页后，可以在应用程序中创建新用户。

    ![注册链接](real-time-web-applications-with-signalr/_static/image3.png "注册链接")

    *注册链接*
4. 在**注册**页上，输入**用户名**和**密码**，然后单击**注册**。

    ![注册用户](real-time-web-applications-with-signalr/_static/image4.png "注册用户")

    *注册用户*
5. 应用程序注册新帐户和用户经过身份验证和重定向回主页上显示的第一个测验问题。
6. 打开**统计信息**页在新窗口中，并将放**主页**页和**统计信息**页上的并行。

    ![并排显示 windows](real-time-web-applications-with-signalr/_static/image5.png "并排端 windows")

    *并排显示 windows*
7. 在**主页**页上，通过单击某个选项来回答问题。

    ![回答问题](real-time-web-applications-with-signalr/_static/image6.png "回答问题")

    *回答问题*
8. 单击后的一个按钮，应显示答案。

    ![问题答案正确](real-time-web-applications-with-signalr/_static/image7.png "正确回答的问题")

    *正确回答的问题*
9. 请注意的统计信息页中提供的信息已过时。 刷新该页才能看到更新后的结果。

    ![统计信息页](real-time-web-applications-with-signalr/_static/image8.png "统计信息页")

    *统计信息页*
10. 返回到 Visual Studio 和停止调试。

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>任务 2 – 添加 SignalR 到奇测验，以显示联机图表

在此任务中，将向解决方案中添加 SignalR，并将更新发送到客户端自动在新的答案发送到服务器时。

1. 从**工具**菜单在 Visual Studio 中，选择**库程序包管理器**，然后单击**程序包管理器控制台**。
2. 在**程序包管理器控制台**窗口中，执行以下命令：

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![SignalR 包安装](real-time-web-applications-with-signalr/_static/image9.png "SignalR 包安装")

    *SignalR 包安装*

    > [!NOTE]
    > 在安装时**SignalR** NuGet 包版本 2.0.2 从全新的 MVC 5 应用程序，你将需要手动更新**OWIN**程序包更新到版本 2.0.1 （或更高版本） 安装 SignalR 前的准备。 若要执行此操作，你可以执行以下脚本中的**程序包管理器控制台**:
    > 
    > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
    > 
    > 在 SignalR 将来版本中，将自动更新 OWIN 依赖关系。
3. 在**解决方案资源管理器**，展开**脚本**文件夹，请注意，SignalR *js*文件已添加到解决方案。

    ![SignalR JavaScript 引用](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript 引用")

    *SignalR JavaScript 引用*
4. 在**解决方案资源管理器**，右键单击**GeekQuiz**项目，依次选择**添加** | **新文件夹**，并将其命名**中心**。
5. 右键单击**中心**文件夹，然后选择**添加 |新项**。

    ![添加新项](real-time-web-applications-with-signalr/_static/image11.png "添加新项")

    *添加新项*
6. 在**添加新项**对话框中，选择**Visual C# |Web |SignalR**的左窗格中，选择的节点**SignalR Hub Class (v2)**从中间窗格中，命名该文件**StatisticsHub.cs**单击**添加**。

    ![添加新项对话框](real-time-web-applications-with-signalr/_static/image12.png "添加新项对话框")

    *添加新项对话框*
7. 中的代码替换**StatisticsHub**类替换为以下代码。

    (代码段- *RealTimeSignalR-Ex1-StatisticsHubClass*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. 打开**Startup.cs**并在末尾添加以下行**配置**方法。

    (代码段- *RealTimeSignalR-Ex1-MapSignalR*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. 打开**StatisticsService.cs**页内**服务**文件夹并添加以下 using 指令。

    (代码段- *RealTimeSignalR-Ex1-UsingDirectives*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. 若要通知连接的客户端的更新，请首先检索**上下文**当前连接的对象。 **中心**对象包含用于将消息发送到单个客户端或广播到所有连接的客户端的方法。 添加以下方法**StatisticsService**类广播的统计信息数据。

    (代码段- *RealTimeSignalR-Ex1-NotifyUpdatesMethod*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > 在上面的代码中，你使用的是任意的方法名称在客户端上调用的函数 (即： *updateStatistics*)。 你指定的方法名称被解释为动态对象，这意味着没有 IntelliSense 或它的编译时验证。 在运行时计算表达式。 当方法调用执行时，SignalR 将方法名称和参数值发送到客户端。 如果客户端有一个与名称匹配的方法，该方法调用和参数值传递给它。 如果客户端上不找到任何匹配方法，则不会引发错误。 有关详细信息，请参阅[ASP.NET SignalR 中心 API 指南](../guide-to-the-api/hubs-api-guide-server.md)。
11. 打开**TriviaController.cs**页内**控制器**文件夹并添加以下 using 指令。

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. 添加以下突出显示的代码**Post**操作方法。

    (代码段- *RealTimeSignalR-Ex1-NotifyUpdatesCall*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. 打开**Statistics.cshtml**页内**视图 |主页**文件夹。 找到**脚本**部分，并在部分的开头添加以下脚本引用。

    (代码段- *RealTimeSignalR-Ex1-SignalRScriptReferences*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > 当你将 SignalR 和其他脚本库添加到 Visual Studio 项目中时，包管理器可能会安装本主题中显示的版本比更新 SignalR 脚本文件的版本。 请确保你的代码中的脚本引用匹配在项目中安装的脚本库的版本。
14. 添加以下突出显示的代码，以将客户端连接到 SignalR 集线器和更新统计信息数据，从中心接收新消息时。

    (代码段- *RealTimeSignalR-Ex1-SignalRClientCode*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    在此代码中，所以在创建中心代理并注册事件处理程序以侦听的服务器发送的消息。 在这种情况下，侦听消息通过发送*updateStatistics*方法。

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>任务 3 – 运行解决方案

在此任务中，你将运行该解决方案以验证使用在应答新问题后 SignalR 自动更新统计信息视图。

1. 按**F5**运行该解决方案。

    > [!NOTE]
    > 如果尚未登录到应用程序，使用在任务 1 中创建的用户登录。
2. 打开**统计信息**页在新窗口中，并将放**主页**页和**统计信息**页上的并行，就像在任务 1 中。
3. 在**主页**页上，通过单击某个选项来回答问题。

    ![回答另一个问题](real-time-web-applications-with-signalr/_static/image13.png "回答另一个问题")

    *回答另一个问题*
4. 单击后的一个按钮，应显示答案。 请注意，回答这个问题与更新的信息，而无需刷新整个页面后自动更新页上的统计信息。

    ![统计信息页面刷新后应答](real-time-web-applications-with-signalr/_static/image14.png "在应答后刷新统计信息页")

    *刷新后应答的统计信息页*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>练习 2： 向外扩展使用的 SQL Server

当缩放 web 应用程序，你可以通常选择之间*向上扩展*和*向外扩展*选项。 *向上扩展*意味着更大的服务器，使用更多的资源 （CPU、 RAM 等） 时*向外扩展*意味着添加更多服务器以处理负载。 后者的问题是，可以获取客户端路由到不同的服务器。 连接到一台服务器的客户端不会收到从另一台服务器发送的消息。

通过使用名为的组件来解决这些问题*底板*、 邮件服务器之间进行转发。 启用底板，每个应用程序实例将消息发送到底板，与底板将其转发给其他应用程序实例。

目前，有三种类型的适用于 SignalR 的背板：

- **Windows Azure Service Bus**。 Service Bus 是允许组件发送松散耦合的消息的消息传递基础结构。
- **SQL Server**。 SQL Server 底板将消息写入 SQL 表。 底板为有效的消息传递使用 Service Broker。 但是，它还适用如果未启用 Service Broker。
- **Redis**。 Redis 是一个内存中键 / 值存储。 Redis 支持发布/订阅 ("pub/sub") 模式，用于将消息发送。

通过消息总线发送每条消息。 消息总线实现[IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)接口，从而提供发布/订阅抽象。 通过替换默认的工作的底板**IMessageBus**含总线为该底板设计的。

每个服务器实例连接到通过总线底板。 发送一条消息，它将转到面板，并底板将其发送到每个服务器。 当服务器从底板收到一条消息时，它将在其本地缓存中存储消息。 服务器然后将从其本地缓存消息传送到客户端。

有关 SignalR 基架的工作原理，阅读此[文章](../performance/scaleout-in-signalr.md)。

> [!NOTE]
> 有底板可能成为瓶颈的其中一些方案。 这里有一些典型的 SignalR 方案：
> 
> - [服务器广播](tutorial-server-broadcast-with-signalr.md)（例如，股票行情）： 背板很适合此方案中，因为该服务器控制速率发送消息的速率。
> - [客户端到客户端](tutorial-getting-started-with-signalr.md)（如聊天）： 在此方案中，底板可能成为瓶颈如果的消息数会随着客户端的数量; 也就是说，如果消息的速率增长按比例随着越来越多的客户端将加入。
> - [高频率实时](tutorial-high-frequency-realtime-with-signalr.md)（例如，实时游戏）： 底板不建议用于此方案。


在本练习中，你将使用**SQL Server**分发跨邮件**专家 Quiz**应用程序。 将单个测试计算机以了解如何设置配置，不过，若要获取完整的效果上运行这些任务，你将需要 SignalR 将应用部署到两个或多个服务器。 在一台服务器，或单独的专用服务器上，还必须安装 SQL Server。

![横向扩展使用 SQL Server 关系图](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>任务 1-了解方案

在此任务中，你将运行 2 个实例的**专家 Quiz**模拟多个 IIS 实例在本地计算机上。 在此方案，回答琐事问题上一个应用程序时，更新将不会收到第二个实例的统计信息页上。 此模拟类似于你的应用程序多个实例的部署的环境和使用负载平衡器与它们通信。

1. 打开**Begin.sln**解决方案位于**源/Ex2-ScalingOutWithSQLServer/开始**文件夹。 加载后，您将会发现**服务器资源管理器**解决方案必须使用完全相同的两个项目结构但使用不同的名称。 这将模拟在本地计算机上运行同一应用程序的两个实例。

    ![开始解决方案模拟专家测验的 2 个实例](real-time-web-applications-with-signalr/_static/image16.png "开始模拟专家测验的 2 个实例的解决方案")

    *开始解决方案模拟专家测验的 2 个实例*
2. 打开解决方案的属性页上，右键单击解决方案节点并选择**属性**。 下**启动项目**，选择**多启动项目**和更改**操作**到这两个项目的值*启动*。

    ![启动多个项目](real-time-web-applications-with-signalr/_static/image17.png "启动多个项目")

    *启动多个项目*
3. 按**F5**运行该解决方案。 应用程序将启动两个实例**专家 Quiz**在不同的端口，模拟的同一个应用程序的多个实例。 在左侧和右侧屏幕的其他固定浏览器之一。 使用你的凭据登录或注册一个新用户。 登录后，在左侧保留琐事页并转到**统计信息**浏览器中的右侧。

    ![专家测验并排显示](real-time-web-applications-with-signalr/_static/image18.png)

    *专家测验并排显示*

    ![专家测验中不同的端口](real-time-web-applications-with-signalr/_static/image19.png)

    *专家测验中不同的端口*
4. 左侧的浏览器中启动回答问题，你将注意到，**统计信息**右浏览器中没有进行更新。 这是因为**SignalR**使用本地缓存，可将消息分布跨其客户端和这种情况下模拟多个实例，因此它们之间不共享缓存。 你可以验证**SignalR**测试相同的步骤，但是使用的单个应用程序是否工作。 下面的任务中，你将配置底板在实例上复制消息。
5. 返回到 Visual Studio 和停止调试。

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>任务 2 – 创建 SQL Server 底板

在此任务中，你将创建数据库将充当基架**专家 Quiz**应用程序。 你将使用**SQL Server 对象资源管理器**浏览你的服务器和初始化数据库。 此外，你将启用**Service Broker**。

1. 在**Visual Studio**，打开菜单**视图**和选择**SQL Server 对象资源管理器**。
2. 通过右键单击连接到 LocalDB 实例**SQL Server**节点并选择**添加 SQL Server...**选项。

    ![添加 SQL Server 实例](real-time-web-applications-with-signalr/_static/image20.png "添加 SQL Server 实例")

    *将 SQL Server 实例添加到 SQL Server 对象资源管理器*
3. 设置**服务器名称**到*(localdb) \v11.0*并使**Windows 身份验证**作为身份验证模式。 单击**连接**以继续。

    ![连接到 LocalDB](real-time-web-applications-with-signalr/_static/image21.png "连接到 LocalDB")

    *连接到 LocalDB*
4. 现在，你将连接到 LocalDB 实例，你将需要创建将表示 SQL Server 底板 SignalR 的一个数据库。 要执行此操作，请右键单击**数据库**节点，然后选择**添加新数据库**。

    ![将新数据库添加](real-time-web-applications-with-signalr/_static/image22.png "添加新的数据库")

    *添加新的数据库*
5. 将数据库名称设置为*SignalR*单击**确定**来创建它。

    ![创建 SignalR 数据库](real-time-web-applications-with-signalr/_static/image23.png "创建 SignalR 数据库")

    *创建 SignalR 数据库*

    > [!NOTE]
    > 您可以选择数据库的任何名称。
6. 若要从底板更有效地接收更新，建议为数据库启用 Service Broker。 Service Broker 提供的消息和队列在 SQL Server 中的本机支持。 底板也适用于而无需 Service Broker。 通过右键单击数据库并选择打开一个新查询**新查询**。

    ![打开新查询](real-time-web-applications-with-signalr/_static/image24.png "打开新查询")

    *打开新查询*
7. 若要检查是否启用 Service Broker，请查询**是\_broker\_启用**中的列**sys.databases**目录视图。 在最近打开的查询窗口中执行以下脚本。

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![查询服务 Broker 状态](real-time-web-applications-with-signalr/_static/image25.png "查询服务代理状态")

    *查询服务代理状态*
8. 如果值**是\_broker\_启用**数据库中的列是&quot;0&quot;，使用以下命令来启用它。 替换 **&lt;YOUR 数据库&gt;**与创建数据库时设置的名称 (例如： SignalR)。

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![启用 Service Broker](real-time-web-applications-with-signalr/_static/image26.png "启用 Service Broker")

    *启用 Service Broker*

    > [!NOTE]
    > 如果此查询出现死锁，请确保没有连接到的数据库应用程序。

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>任务 3 – 配置 SignalR 应用程序

在此任务中，您将配置**专家 Quiz**连接到 SQL Server 底板。 首先，你将添加**SignalR.SqlServer** NuGet 包并设置连接字符串以底板数据库。

1. 打开**程序包管理器控制台**从**工具** | **库程序包管理器**。 请确保**GeekQuiz**中选择项目**默认项目**下拉列表。 键入以下命令以安装**Microsoft.AspNet.SignalR.SqlServer** NuGet 包。

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. 对项目重复上一步但这次**GeekQuiz2**。
3. 若要配置 SQL Server 底板，打开**Startup.cs**文件**GeekQuiz**项目，并添加以下代码**配置**方法。 替换 **&lt;YOUR 数据库&gt;**替换为你创建 SQL Server 底板时使用的数据库名称。 重复此步骤**GeekQuiz2**项目。

    (代码段- *RealTimeSignalR-Ex2-StartupConfiguration*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. 现在，这两个项目配置为使用 SQL Server 底板，按**F5**同时运行它们。
5. 同样， **Visual Studio**将启动两个实例**专家 Quiz**中不同的端口。 在左侧和右侧屏幕的其他固定一个浏览器并登录你的凭据。 在左侧保留琐事页并转到**统计信息**pagein 右浏览器。
6. 启动回答问题在左侧的浏览器中。 此时，**统计信息**页更新底板感谢。 应用程序之间切换 (**统计信息**现在位于左侧，和**琐事**位于右侧) 并重复执行测试来验证它正在为两个实例。 底板用作*共享缓存*消息的每个连接的服务器，且每个服务器将存储消息在其自己要分发到连接的客户端的本地缓存中。
7. 返回到 Visual Studio 和停止调试。
8. SQL Server 底板组件会自动生成对指定的数据库所需的表。 在**SQL Server 对象资源管理器**面板中，打开你在底板中创建的数据库 (例如： SignalR) 并展开其表。 你应该会看到以下表：

    ![底板生成表](real-time-web-applications-with-signalr/_static/image27.png)

    *底板生成表*
9. 右键单击**SignalR.Messages\_0**表，然后选择**查看数据**。

    ![视图 SignalR 基架消息表](real-time-web-applications-with-signalr/_static/image28.png)

    *视图 SignalR 基架消息表*
10. 你可以看到不同的消息发送到**中心**时回答琐事问题。 底板将分发到任何连接实例这些消息。

    ![底板消息表](real-time-web-applications-with-signalr/_static/image29.png)

    *底板消息表*

* * *

<a id="Summary"></a>
## <a name="summary"></a>摘要

在此动手实验中，你学习了如何添加**SignalR**你应用程序和发送通知从服务器到你使用的连接的客户端**中心**。 此外，您学习了如何通过使用横向扩展你的应用程序*底板*组件时你的应用程序部署在多个 IIS 实例中。
