---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: "全局错误处理 ASP.NET Web API 2 中 |Microsoft 文档"
author: davidmatson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: c593c56ba3d0ee8ebf6dc425408d2c3b91c83f93
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>全局错误处理 ASP.NET Web API 2 中
====================
通过[David Matson](https://github.com/davidmatson)， [Rick Anderson](https://github.com/Rick-Anderson)

现在以记录或者全局处理错误的 Web API 中是简单的方法。 可以通过处理某些未经处理的异常[异常筛选器](exception-handling.md)，但有异常筛选器无法处理的事例的数量。 例如:

1. 从控制器构造函数引发的异常。
2. 从消息处理程序引发的异常。
3. 在路由过程中引发的异常。
4. 在响应内容序列化期间引发的异常。

我们想要提供一种简单一致的方式来记录和 （如果可能） 处理这些异常。 

有两种主要的情况下处理的异常，这种情况，其中我们能够发送错误响应，其中所有我们可以执行这种情况是日志异常。 后一种情况的示例是当中间流式处理响应内容，则引发异常在这种情况下它为时已晚发送新的响应消息，因为状态代码、 标头，以及部分内容已经跨网络，因此我们只需中止该连接。 即使无法处理异常，以生成新的响应消息，我们仍支持日志记录的异常。 在我们可以检测到错误的情况下，我们可以返回相应的错误响应，如如下所示：

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>现有选项

除了[异常筛选器](exception-handling.md)，[消息处理程序](../advanced/http-message-handlers.md)可用今天来观察所有 500 级响应，但对这些响应很困难，因为它们缺少有关原始错误的上下文。 消息处理程序还具有一些有关的用例，它们可以处理的异常筛选器与相同的限制。Web API 具有捕获错误条件的跟踪基础结构时跟踪基础结构用于诊断目的和设计，也没有适合用于在生产环境中运行。 全局异常处理和日志记录应为服务，可以在生产期间运行，并插入到现有的监视解决方案 (例如， [ELMAH](https://code.google.com/p/elmah/) )。

### <a name="solution-overview"></a>解决方案概述

 我们提供两个新的用户可更换服务[IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md)和 IExceptionHandler，日志，并处理未经处理的异常。 服务是非常相似，两个主要区别：

1. 我们支持注册多个异常记录器但仅在单个异常处理程序。
2. 异常记录器始终会调用，即使我们即将中止连接。 当我们仍能够选择要发送的响应消息时仅获取调用异常处理程序。

这两个服务提供对包含异常已检测到的点中的相关信息的异常上下文访问特别[HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx)、 [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx)，则引发异常和异常源 （下面详细信息）。

### <a name="design-principles"></a>设计原则

1. **任何重大更改**因为在次要版本中，指影响解决方案的重要约束是不有任何重大更改，或者键入合同或行为，此功能将被添加。 此约束排除我们想要从现有 catch 块内打开到 500 响应异常方面进行一些清理。 此附加清理是我们在考虑用于后续的主版本。 如果这是对重要请对它在提出建议[ASP.NET Web API 用户之声](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception)。
2. **维护与 Web API 保持一致构造**Web API 的筛选器管道是应用在特定于操作的、 特定于控制器的或全局作用域的逻辑可以灵活地处理跨领域问题的好办法。 筛选器，包括异常筛选器始终具有操作和控制器上下文，即使在全局范围内注册。 协定筛选器，对有意义，但是它意味着，异常筛选器，即使全局范围的并不适合某些异常处理情况下，例如从消息处理程序的异常，其中没有操作或控制器上下文存在。 如果我们想要使用的灵活作用域提供用于异常处理筛选器，我们仍需要异常筛选器。 但如果我们需要处理在控制器上下文之外的异常，我们还需要一个单独的构造 （内容无控制器上下文和操作上下文约束） 的完整全局错误处理。

### <a name="when-to-use"></a>何时使用

- 异常记录器都是看到所有未经处理的异常捕获了 Web API 的解决方案。
- 异常处理程序自定义未经处理的异常由 Web API 捕获所有可能作出响应的解决方案。
- 异常筛选器是最简单的解决方案，用于处理与特定操作或控制器相关的子集的未经处理的异常。

### <a name="service-details"></a>服务详细信息

 异常记录器和处理程序服务接口是简单的异步方法采取相应的上下文： 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 我们还为这两种接口提供基类。 重写的核心 （同步或异步） 的方法是所有所需记录或者在推荐的处理时间。 对于日志记录，`ExceptionLogger`基类将确保核心日志记录方法仅对于每个异常的一次调用 （即使它更高版本传播进一步调用堆栈并再次捕获）。 `ExceptionHandler`基类将调用处理方法，仅对于调用堆栈，忽略嵌套的旧顶部的异常的 catch 块的核心。 （这些基类的简化的版本是下面的附录中。）同时`IExceptionLogger`和`IExceptionHandler`接收有关通过异常的信息`ExceptionContext`。

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

当框架调用异常记录器或异常处理程序时，它将始终提供`Exception`和`Request`。 单元测试，除外，它将也始终提供`RequestContext`。 极少数情况下，它将提供`ControllerContext`和`ActionContext`（仅在从 catch 块中调用异常筛选器）。 极少数情况下，它将提供`Response`（仅在当中间尝试将响应写入某些 IIS 情况下）。 请注意，因为其中一些属性可能`null`由使用者便可以检查`null`之前访问异常类的成员。`CatchBlock` 一个字符串，表示的 catch 块看到异常。 Catch 块字符串如下所示：

- HttpServer （SendAsync 方法）
- HttpControllerDispatcher （SendAsync 方法）
- HttpBatchHandler （SendAsync 方法）
- IExceptionFilter （ApiController 的处理的异常筛选器管道中 ExecuteAsync）
- OWIN 主机：

    - HttpMessageHandlerAdapter.BufferResponseContentAsync （适用于缓冲输出）
    - HttpMessageHandlerAdapter.CopyResponseContentAsync （适用于流输出）
- Web 主机：

    - HttpControllerHandler.WriteBufferedResponseContentAsync （适用于缓冲输出）
    - HttpControllerHandler.WriteStreamedResponseContentAsync （适用于流输出）
    - HttpControllerHandler.WriteErrorResponseContentAsync （有什么在缓冲的输出模式下的错误恢复故障）

Catch 块字符串的列表也会提供通过静态只读属性。 （核心 catch 块字符串位于静态 ExceptionCatchBlocks; 其余部分显示在一个静态类中每个为 OWIN 和 web 主机）。`IsTopLevelCatchBlock` 对于以下建议的模式的处理异常仅在调用堆栈的顶部有帮助。 而不是打开到 500 响应随处嵌套的 catch 块内的异常，异常处理程序可以让异常传播直到它们将要查看的主机。

除了`ExceptionContext`，记录器获取一条详细通过完整的信息`ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

第二个属性， `CanBeHandled`，使记录器可以通过标识不能处理的异常。 当连接即将中止和可以发送任何新的响应消息，将调用记录器但处理程序将***不***调用，并且记录器可以确定这种情况下，此属性。

在附加到`ExceptionContext`，处理程序获得一个它可以在完整设置的详细属性`ExceptionHandlerContext`地处理异常：

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

异常处理程序指示它已通过设置处理异常`Result`操作结果的属性 (例如， [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx)， [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx)， [StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx)，或自定义结果)。 如果`Result`属性为 null，会处理异常，并且将重新引发原始异常。

对于调用堆栈顶部的异常，我们花费了额外的步骤以确保响应适合 API 调用方。 如果在异常传播到主机时，调用方将看到死亡的黄色屏幕或某些其他主机提供响应这通常是 HTML 和通常不适当的 API 错误响应。 在这些情况下，则结果将启动该出非 null，且仅当自定义异常处理程序显式将其设置回`null`（未处理的） 将该异常会传播到主机。 设置`Result`到`null`在这种情况下会很有用的两种方案：

1. OWIN 承载 Web API 的自定义异常处理注册前/外部 Web API 的中间件。
2. 本地调试通过浏览器，其中死亡的黄色屏幕是实际的有用响应未经处理的异常。

对于异常记录器和异常处理程序，我们不执行任何操作来恢复如果记录器或处理程序本身引发异常。 （而不让异常传播，保留在此页底部的反馈，如果你具有更好的方法）。异常记录器，并处理程序的协定是，它们不应让异常向上传播到其调用方;否则，该异常将只需传播，通常一直到主机从而导致 HTML 错误 （如 ASP。NET 的黄色屏幕） 发送回客户端 （这通常并不是预期 JSON 或 XML 的 API 调用方的首选的选项）。

## <a name="examples"></a>示例

### <a name="tracing-exception-logger"></a>跟踪异常记录器

下面的异常记录器将异常数据发送到已配置跟踪源 （包括 Visual Studio 中的调试输出窗口）。

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>自定义错误消息异常处理程序

下面生成对客户端，包括电子邮件地址联系支持人员的自定义错误响应。

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>注册异常筛选器

如果你使用"ASP.NET MVC 4 Web 应用程序"项目模板创建你的项目，将 Web API 配置代码内的放置`WebApiConfig`类，在*应用/启动 （_s)*文件夹：

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>附录： 基类详细信息

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
