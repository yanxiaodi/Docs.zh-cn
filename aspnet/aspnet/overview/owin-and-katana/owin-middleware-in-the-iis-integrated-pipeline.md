---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: "在 IIS 中的 OWIN 中间件集成管道 |Microsoft 文档"
author: Praburaj
description: "这篇文章演示如何运行 OWIN 中间件组件 (OMCs) 在 IIS 集成管道、 如何设置管道事件 OMC 上并运行。 你应该..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2013
ms.topic: article
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 5f6ed1ae0309e9bdd3ca4ae229195835f20bc729
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>在 IIS 集成管道中的 OWIN 中间件
====================
通过[Praburaj Thiagarajan](https://github.com/Praburaj)， [Rick Anderson](https://github.com/Rick-Anderson)

> 这篇文章演示如何运行 OWIN 中间件组件 (OMCs) 在 IIS 集成管道、 如何设置管道事件 OMC 上并运行。 你应查看[项目概述 Katana](an-overview-of-project-katana.md)和[OWIN 启动类检测](owin-startup-class-detection.md)阅读本教程之前。 本教程编写由 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )，Chris 跨、 Praburaj Thiagarajan 和 Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) )。


尽管[OWIN](an-overview-of-project-katana.md)中间件组件 (OMCs) 主要用于在服务器不可知管线中运行，则你可以在 IIS 集成管道也中运行 OMC (**经典模式是*不*支持**)。 可进行 OMC IIS 集成管道中的工作后，通过安装以下包从包管理器控制台 (PMC):

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

这意味着所有应用程序框架，甚至那些尚未不能在 IIS 和 System.Web，外部运行可以受益于现有的 OWIN 中间件组件。 

> [!NOTE]
> 所有`Microsoft.Owin.Security.*`包随附在 Visual Studio 2013 中新的标识系统 (例如： Cookie、 Microsoft 帐户、 Google、 Facebook、 Twitter[持有者令牌](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html)，OAuth，授权服务器，JWT，Azure Active目录中，和 Active directory 联合身份验证服务） 被编写为 OMCs，并可以在自承载和承载于 IIS 中的方案中使用。

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>OWIN 中间件 IIS 集成管道中的执行方式

OWIN 控制台应用程序，应用程序管道使用构建为[启动配置](owin-startup-class-detection.md)由组件添加使用的顺序设置`IAppBuilder.Use`方法。 也就是说，在 OWIN 管道[Katana](an-overview-of-project-katana.md)运行时将使用注册的顺序处理 OMCs `IAppBuilder.Use`。 在 IIS 集成管道请求管道组成[Httpmodule](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx)如订阅一组预定义的管道事件[BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx)， [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)， [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)等。

如果我们将进行比较的 OMC [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx)在 ASP.NET 世界中，OMC 必须注册到正确的预定义的管道事件。 例如，HttpModule`MyModule`请求到达时将调用[AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)管道中的阶段：

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

用于 OMC 参与此相同的、 基于事件的执行排序顺序[Katana](an-overview-of-project-katana.md)通过扫描运行时代码[启动配置](owin-startup-class-detection.md)和订阅的每个到中间件组件集成的管道事件。 例如，以下的 OMC 和注册代码允许你查看的中间件组件的默认事件注册。 (有关更多详细创建的 OWIN 启动类的说明，请参阅[OWIN 启动类检测](owin-startup-class-detection.md)。)

1. 创建一个空 web 应用程序项目并将其命名**owin2**。
2. 从包管理器控制台 (PMC) 中，运行以下命令： 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. 添加`OWIN Startup Class`并将其命名`Startup`。 生成的代码替换为以下 （突出显示所做的更改）：  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. 按 F5 运行应用程序。

启动配置设置了三个中间件组件、 与前两个显示的诊断信息并且对事件作出响应的最后一个 （也显示诊断信息） 的管道。 `PrintCurrentIntegratedPipelineStage`方法显示此中间件调用的集成的管道事件和消息。 输出窗口显示以下信息：

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Katana 运行时映射到的 OWIN 中间件组件的每个[PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx)默认情况下，它对应于 IIS 管道事件[PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx)。

## <a name="stage-markers"></a>阶段标记

你可以将标记 OMCs 通过使用在特定的管道阶段都执行`IAppBuilder UseStageMarker()`扩展方法。 若要在特定阶段运行的一组中间件组件，阶段标记在右侧插入后的最后一个组件是在注册过程的组。 有关管道的哪个阶段，你可以执行中间件的规则和顺序组件必须运行 （在教程后面部分解释了规则）。 添加`UseStageMarker`方法`Configuration`代码如下所示：

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

`app.UseStageMarker(PipelineStage.Authenticate)`调用配置 （在此情况下，我们两个诊断组件） 的所有以前注册的中间件组件在管道的身份验证阶段上运行。 将在上运行的最后一个的中间件组件 （该显示诊断并响应请求）`ResolveCache`阶段 ( [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx)事件)。

按 F5 运行应用程序。输出窗口显示以下信息：

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>阶段标记规则

Owin 中间件组件 (OMC) 可以配置为在以下 OWIN 管道阶段事件运行：

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. 默认情况下，在最后一个事件将运行 OMCs (`PreHandlerExecute`)。 这正是我们第一个示例代码显示"PreExecuteRequestHandler"。
2. 你可以使用`app.UseStageMarker`中列出的方法来注册 OMC 运行更早版本，在任何阶段 OWIN 管道`PipelineStage`枚举。
3. OWIN 管道和 IIS 管道已经过排序，因此调用`app.UseStageMarker`必须按顺序。 无法将事件处理程序设置为之前使用注册到的最后一个事件的事件`app.UseStageMarker`。 例如，*后*调用：

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

 调用`app.UseStageMarker`传递`Authenticate`或`PostAuthenticate`不会遵循，并且会引发任何异常。 在最新的阶段，即默认情况下运行的 OMCs `PreHandlerExecute`。 阶段标记用于使其运行更早版本。 如果你指定无序的阶段标记，则我们舍入到早期标记中。 换而言之，添加一个阶段标记将显示"最晚阶段 X 运行"。 在其后添加 OWIN 管道中的最早阶段标记 OMC 的运行。
4. 对的调用的最早阶段`app.UseStageMarker`wins。 例如，如果你切换的顺序`app.UseStageMarker`我们上一示例中的调用：

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

 输出窗口将显示： 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

 在所有运行的 OMCs`AuthenticateRequest`阶段，因为最后一个 OMC 注册与`Authenticate`事件，和`Authenticate`事件之前的所有其他事件。
