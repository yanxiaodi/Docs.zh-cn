---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: "在 ASP.NET 4.5 和 Visual Studio 2012 中的新增功能 |Microsoft 文档"
author: rick-anderson
description: "本文档介绍新功能和 ASP.NET 4.5 中引入的增强功能。 它还介绍了为 web 开发正在进行的改进..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/29/2012
ms.topic: article
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 16a2fa4b1b6617430703853543cbf29e18ba1103
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>在 ASP.NET 4.5 和 Visual Studio 2012 中的新增功能
====================
> 本文档介绍新功能和 ASP.NET 4.5 中引入的增强功能。 它还介绍了对 Visual Studio 2012 中的 web 开发的改进。 本文档最初在 2012 年 2 月 29 日发布。


- [ASP.NET 核心运行时和 Framework](#_Toc318097372)

    - [以异步方式读取和写入 HTTP 请求和响应](#_Toc318097373)
    - [对 HttpRequest 处理改进](#_Toc318097374)
    - [异步刷新响应](#_Toc318097375)
    - [支持*await*和*任务*-基于异步模块和处理程序](#_Toc318097376)
    - [异步 HTTP 模块](#_Toc318097377)
    - [异步 HTTP 处理程序](#_Toc318097378)
    - [新的 ASP.NET 请求验证功能](#_Toc318097379)
    - [延迟 （"迟缓"） 请求验证](#_Toc318097380)
    - [对未经过验证的请求的支持](#_Toc318097381)
    - [AntiXSS 库](#_Toc318097382)
    - [Websocket 协议的支持](#_Toc318097383)
    - [捆绑和缩小](#_Toc318097384)
    - [用于 Web 托管的性能改进](#_Toc_perf)

        - [关键性能因素](#_Toc_perf_1)
        - [新的性能功能的要求](#_Toc_perf_2)
        - [共享公共程序集](#_Toc_perf_3)
        - [用于启动速度更快的多核 JIT 编译](#_Toc_perf_4)
        - [若要针对内存优化的优化垃圾回收](#_Toc_perf_5)
        - [预提取的 web 应用程序](#_Toc_perf_6)
- [ASP.NET Web 窗体](#_Toc318097385)

    - [强类型化数据控件](#_Toc318097386)
    - [模型绑定](#_Toc318097387)

        - [选择数据](#_Toc318097388)
        - [值在提供程序](#_Toc318097389)
        - [筛选来自控件的值](#_Toc318097390)
    - [HTML 编码数据绑定表达式](#_Toc318097391)
    - [非介入式验证](#_Toc318097392)
    - [HTML5 更新](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [ASP.NET Web Pages 2](#_Toc318097395)
- [Visual Studio 2012 候选发布版本](#_Toc318097396)

    - [Visual Studio 2010 和 Visual Studio 2012 候选发布版本 （项目兼容性） 之间共享的项目](#project-compatibility)
    - [ASP.NET 4.5 网站模板中的配置更改](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [在 IIS 7 ASP.NET 路由中的本机支持](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [HTML 编辑器](#_Toc318097397)

        - [智能任务](#_Toc318097398)
        - [WAI ARIA 支持](#_Toc318097399)
        - [新的 HTML5 代码段](#_Toc318097400)
        - [提取到用户控件](#_Toc318097401)
        - [在属性中的代码而有价值的 IntelliSense](#_Toc318097402)
        - [当你重命名一个开始标记或结束标记时匹配标记的自动重命名](#_Toc318097403)
        - [事件处理程序生成](#_Toc318097404)
        - [智能缩进](#_Toc318097405)
        - [自动减少语句结束](#_Toc318097406)
    - [JavaScript Editor](#_Toc318097407)

        - [代码大纲显示](#_Toc318097408)
        - [大括号匹配](#_Toc318097409)
        - [转到定义](#_Toc318097410)
        - [ECMAScript5 支持](#_Toc318097411)
        - [DOM IntelliSense](#_Toc318097412)
        - [VSDOC 签名重载](#_Toc318097413)
        - [隐式引用](#_Toc318097414)
    - [CSS 编辑器](#_Toc318097415)

        - [自动减少语句结束](#_Toc318097416)
        - [分层缩进。](#_Toc318097417)
        - [CSS hacks 支持](#_Toc318097418)
        - [供应商特定架构 (-moz-，易于使用的功能)](#_Toc318097419)
        - [注释和 uncommenting 支持](#_Toc318097420)
        - [颜色选取器](#_Toc318097421)
        - [片段](#_Toc318097422)
        - [自定义区域](#_Toc318097423)
    - [Page Inspector](#_Toc318097424)
    - [发布](#_Toc318097425)

        - [发布配置文件](#_Toc318097426)
        - [ASP.NET 预编译和合并](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Disclaimer](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>ASP.NET 核心运行时和 Framework

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>以异步方式读取和写入 HTTP 请求和响应

ASP.NET 4 引入了能够读取 HTTP 请求实体作为流使用*HttpRequest.GetBufferlessInputStream*方法。 此方法提供流式处理访问请求实体。 但是，它以同步方式执行，该请求的持续时间内绑定到某个线程。

ASP.NET 4.5 支持能够读取流在 HTTP 请求实体，以异步方式和异步刷新的能力。 ASP.NET 4.5 还为你提供了双缓冲提供与下游 HTTP 处理程序，例如.aspx 页处理程序和 ASP.NET MVC 的轻松集成控制器的 HTTP 请求实体的功能。

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>对 HttpRequest 处理改进

ASP.NET 4.5，从返回的流引用*HttpRequest.GetBufferlessInputStream*支持同步和异步读取的方法。 *流*从返回的对象*GetBufferlessInputStream*现在实现 BeginRead 和 EndRead 方法。 异步*流*方法使你可以以异步方式在读取请求实体中的区块，同时 ASP.NET 释放当前线程之间的异步读取循环每次迭代。

ASP.NET 4.5 还添加了用于缓冲方式读取请求实体的助理方法： *HttpRequest.GetBufferedInputStream*。 此新的重载的工作原理类似*GetBufferlessInputStream*，支持同步和异步读取。 但是，如读取、 *GetBufferedInputStream*还将实体字节复制到 ASP.NET 内部缓冲区，这样，下游模块和处理程序仍可以访问的请求实体。 例如，如果某些上游管道中的代码具有已读取请求实体使用*GetBufferedInputStream*，你仍然可以使用*HttpRequest.Form*或*HttpRequest.Files*. 这允许你对执行异步处理的请求 （例如，流式处理大型文件上载到数据库），但仍运行的.aspx 页和 MVC ASP.NET 控制器之后。

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>异步刷新响应

发送到 HTTP 客户端的响应可能需要相当长的时间，当客户端很远，或具有低带宽连接。 通常 ASP.NET 缓冲响应字节数，因为它们创建应用程序。 ASP.NET 然后执行一个发送操作的请求处理的最末尾的应计缓冲区。

如果所有缓冲的响应较大 （例如，流式处理的大型文件，以便客户端），则必须定期调用*HttpResponse.Flush*将缓冲的输出发送到客户端并保持受控制的内存使用量。 但是，因为*刷新*是同步调用，以迭代方式调用*刷新*仍然可能长时间运行的请求的持续时间内消耗线程。

ASP.NET 4.5 增加了支持对使用以异步方式执行刷新的*BeginFlush*和*EndFlush*方法*HttpResponse*类。 使用这些方法，可以创建异步的模块和异步处理程序以增量方式将数据发送到客户端而不会占用操作系统线程。 在此期间*BeginFlush*和*EndFlush*调用，ASP.NET 释放当前线程。 这样会明显降低支持长时间运行 HTTP 下载所需的活动线程的总数。

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>支持*await*和*任务*-基于异步模块和处理程序

.NET Framework 4 引入了称为异步编程概念*任务*。 任务表示通过*任务*类型和相关的类型中*System.Threading.Tasks*命名空间。 .NET Framework 4.5 上生成了编译器增强功能，请使用*任务*对象简单。 在.NET Framework 4.5，编译器支持两个新的关键字： *await*和*异步*。 *Await*关键字是用于指示语法的一段代码应以异步方式等待代码的其他部分的速记。 *异步*关键字都表示一个提示，可用于将方法标记为基于任务的异步方法。

组合*await*，*异步*，和*任务*对象使可以你在.NET 4.5 中编写异步代码更加容易。 ASP.NET 4.5 支持可让你编写异步 HTTP 模块和使用新的编译器增强功能的异步 HTTP 处理程序的新 Api 与这些简化形式。

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>异步 HTTP 模块

假设你想要执行返回的方法内的异步工作*任务*对象。 下面的代码示例定义的异步方法，使要下载 Microsoft 主页上的异步调用。 请注意，使用*异步*方法签名中的关键字和*await*调用*DownloadStringTaskAsync*。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

这就是所有你必须编写 —.NET Framework 自动将处理时等待下载完成，以及下载完成后自动还原调用堆栈展开调用堆栈。

现在假设你想要在异步 ASP.NET HTTP 模块中使用此异步方法。 ASP.NET 4.5 包括一个帮助器方法 (*EventHandlerTaskAsyncHelper*) 和新的委托类型 (*TaskEventHandler*) 可用于集成有较旧的基于任务的异步方法公开的 ASP.NET HTTP 管道的异步编程模型。 此示例演示如何：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>异步 HTTP 处理程序

在 ASP.NET 中编写异步处理程序的传统方法是实现*IHttpAsyncHandler*接口。 ASP.NET 4.5 引入了*HttpTaskAsyncHandler*异步基类型，您可以从其中，这样，就可以更轻松地编写异步处理程序。

*HttpTaskAsyncHandler*类型为抽象类，并要求你重写*ProcessRequestAsync*方法。 内部 ASP.NET 负责集成返回签名 (*任务*对象) 的*ProcessRequestAsync*与由 ASP.NET 管道的较旧异步编程模型。

下面的示例演示如何使用*任务*和*await*作为异步 HTTP 处理程序的实现的一部分：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>新的 ASP.NET 请求验证功能

默认情况下，ASP.NET 将执行请求验证-它会检查请求以查找标记或脚本中的字段、 标头，cookie 和等等。 如果检测到任何时，ASP.NET 将引发异常。 将充当防范潜在的跨站点脚本攻击第一行。

ASP.NET 4.5，可以轻松有选择地读取未经过验证的请求数据。 ASP.NET 4.5 还集成常用 AntiXSS 库中，以前外部库。

开发人员已经常要求有选择地关闭请求验证其应用程序的功能。 例如，如果你的应用程序是论坛软件，你可能想要允许用户提交 HTML 格式论坛帖子和注释，但仍确保请求验证检查其他所有内容。

ASP.NET 4.5 引入了容易让你可以有选择地处理未经过验证的输入的两个功能： 延迟 （"迟缓"） 请求验证并对未经验证的请求数据的访问。

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>延迟 （"迟缓"） 请求验证

在 ASP.NET 4.5 中，默认情况下所有请求数据都将遵循请求验证。 但是，你可以配置应用程序，以便将实际访问请求数据延迟请求验证。 （这有时称为延迟请求验证，基于等延迟加载对于某些数据方案的术语。）你可以配置应用程序在 Web.config 文件中使用延迟的验证，通过设置*requestValidationMode*属性设为在 4.5 *httpRUntime*元素，如以下示例所示：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

在请求验证模式设置为 4.5，仅可用于特定请求值，而仅在你的代码访问该值时，才会触发请求验证。 例如，如果你的代码获取的值的 Request.Form["forum\_文章"]，仅为该元素在窗体集合调用请求验证。 中的其他元素的任何*窗体*集合进行验证。 在以前版本的 ASP.NET，请求验证已访问集合中的任何元素时找不到整个请求触发。 新行为使容易一下不触发请求验证其他介质上的不同片段的请求数据的不同应用程序组件。

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>对未经过验证的请求的支持

单独的延迟的请求验证不解决此问题的有选择地绕过请求验证。 Request.Form["forum 调用\_文章"] 仍触发器请求该特定请求值的验证。 但是，你可能想要访问此字段不触发验证，因为你想要允许该字段中的标记。

若要做到这一点，ASP.NET 4.5 现在支持未经过验证的访问请求数据。 ASP.NET 4.5 包括一个新*Unvalidated*集合属性中的*HttpRequest*类。 此集合提供访问所有请求数据的常见值如*窗体*， *QueryString*， *Cookie*，和*Url*。

使用论坛示例中，若要能够读取未经验证的请求数据，首先需要配置应用程序以使用新的请求验证模式：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

然后，可以使用*HttpRequest.Unvalidated*要读取未经过验证的窗体值属性：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> 安全-*谨慎使用未经验证的请求数据 ！* ASP.NET 4.5 增加的未经验证的请求属性和集合以使其更轻松地访问非常具体未经验证的请求数据。 但是，你仍必须在原始请求数据，以确保危险的文本不呈现给用户来执行自定义验证。


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>AntiXSS 库

由于 Microsoft AntiXSS 库受欢迎程度，ASP.NET 4.5 现在集成了核心编码例程与库的版本 4.0。

由实现编码例程*AntiXssEncoder*在新的类型*System.Web.Security.AntiXss*命名空间。 你可以使用*AntiXssEncoder*直接通过调用静态的类型中实现的编码任何的方法类型。 但是，使用新的反 XSS 例程的最简单方法是配置 ASP.NET 应用程序使用*AntiXssEncoder*默认情况下的类。 若要执行此操作，请将以下属性添加到 Web.config 文件：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

当*encoderType*属性设置为使用*AntiXssEncoder*类型，所有输出自动编码在 ASP.NET 中使用新的编码例程。

这些是已合并到 ASP.NET 4.5 的外部 AntiXSS 库的部分：

- *HtmlEncode*， *HtmlFormUrlEncode*，和*HtmlAttributeEncode*
- *XmlAttributeEncode*和*XmlEncode*
- *执行 UrlEncode*和*UrlPathEncode* （新）
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Websocket 协议的支持

Websocket 协议是基于标准的定义如何通过 HTTP 建立客户端和服务器之间的安全、 实时双向通信的网络协议。 Microsoft 已经与 IETF 和 W3C 标准正文以帮助定义的协议。 与方面投入大量资源客户端和移动操作系统上支持 Websocket 协议的 Microsoft （而不仅仅是浏览器），任何客户端支持的 Websocket 协议。

Websocket 协议，使得可以更轻松地创建客户端和服务器之间的长时间运行的数据传输。 例如，编写的聊天应用程序是要容易得多的因为你可以建立 true 长时间运行连接客户端和服务器之间。 无需采用如定期轮询或 HTTP 长轮询来模拟套接字的行为的解决方法。

ASP.NET 4.5 和 IIS 8 包括低级别的 Websocket 支持，使 ASP.NET 开发人员能够使用托管的 Api，用于以异步方式读取和 Websocket 对象上写入字符串和二进制数据。 为 ASP.NET 4.5，新增了一个*System.Web.WebSockets*包含用于使用 Websocket 协议的类型的命名空间。

浏览器客户端通过创建一个 DOM 建立的 Websocket 连接*WebSocket*指向中 ASP.NET 应用程序，如以下示例所示的 URL 的对象：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

你可以在 ASP.NET 中使用任何类型的模块或处理程序创建 Websocket 终结点。 在前面的示例中，使用.ashx 文件，因为.ashx 文件创建一个处理程序的快速方法。

根据 Websocket 协议，ASP.NET 应用程序接受客户端的 Websocket 请求通过指示请求应通过 HTTP GET 请求升级到的 Websocket 请求。 以下是一个示例：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

*AcceptWebSocketRequest*方法接受一个函数委托，因为 ASP.NET 展开当前 HTTP 请求，然后将控件传输到的函数委托。 从概念上讲，此方法非常类似于你如何使用*计数*、 你在其中定义在哪个后台执行工作线程开始委托。

ASP.NET 和客户端已成功完成的 Websocket 握手后，ASP.NET 将调用委托和 Websocket 应用程序开始运行。 下面的代码示例演示一个简单的回显应用程序，在 ASP.NET 中使用内置的 Websocket 支持：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

有关.NET 4.5 中的支持*await*关键字和基于任务的异步操作是天生适合对编写 Websocket 应用程序。 代码示例演示在 ASP.NET 内完全以异步方式运行的 Websocket 请求。 在应用程序以异步方式等待要通过调用从客户端发送的消息*await 套接字。ReceiveAsync*。 同样，你可以将异步消息到客户端通过调用*await 套接字。SendAsync*。

在浏览器中，应用程序接收通过 Websocket 消息*onmessage*函数。 若要从浏览器发送消息，请调用*发送*方法*WebSocket* DOM 类型，如本示例中所示：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

将来，我们可能会发布与此功能，抽象消失的低级别的编码，它一些需要在此版本中用于 Websocket 应用程序的更新。

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>绑定和缩减

绑定允许您将各个 JavaScript 和 CSS 文件合并到一个包，可以被视为单个文件。 缩减将 JavaScript 和 CSS 文件集中删除空格和不需要的其他字符。 这些功能适用于 Web 窗体、 ASP.NET MVC 和 Web 页。

捆绑包是使用捆绑类或其子类，ScriptBundle 和 StyleBundle 之一创建的。 在配置后的捆绑包实例，捆绑包将提供给传入的请求通过只需将其添加到全局 BundleCollection 实例。 在默认模板中，捆绑包配置都被执行 BundleConfig 文件中。 此默认配置创建的核心脚本和 css 文件的模板使用所有的捆绑包。

捆绑包从内引用视图通过使用几个可能的帮助器方法之一。 为了支持呈现时在发布模式下与调试捆绑包的不同标记，ScriptBundle 和 StyleBundle 类具有呈现的帮助器方法。 如果处于调试模式，呈现器将生成捆绑中的每个资源的标记。 在发布模式下，呈现器将生成整个捆绑的单个标记元素。 切换之间调试和发布，可以通过如下所示修改 web.config 中的元素的调试属性实现模式：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

此外，可以直接通过 BundleTable.EnableOptimizations 属性设置启用或禁用优化。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

当文件会捆绑时，它们是首先按字母顺序排序 (中所显示的方式**解决方案资源管理器**)。 然后，它们进行组织，以便已知库和首先加载其自定义扩展 （如 jQuery、 MooTools 和 Dojo）。 例如，脚本文件夹如上所示的绑定的最终顺序将：

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js
4. a.js

CSS 文件也按字母顺序排序，然后重新组织以便 reset.css 和 normalize.css 之上的任何其他文件。 最终排序的绑定上面所示的样式文件夹将为此：

1. reset.css
2. content.css
3. forms.css
4. globals.css
5. menu.css
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>用于 Web 托管的性能改进

.NET Framework 4.5 和 Windows 8 引入了可帮助您实现显著的性能提升为 web 服务器工作负荷的功能。 这包括减少 （最多 35%) 在这两个启动时间和 web 托管使用 ASP.NET 站点上的内存需求量。

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>关键性能因素

理想情况下，所有网站应都处于活动状态，并且在内存中以确保快速响应下一个请求，每当涉及。 可能会影响网站响应能力的因素包括：

- 要重新启动后应用程序池进行回收的站点所花费的时间。 这是以启动 web 服务器进程的站点的站点程序集不再在内存中时的时间。 （平台程序集是仍在内存中，因为它们由其他站点。）这种情况下被称为"冷站点暖 framework 启动"，或只是"冷站点启动。"
- 站点占用的内存量。 此术语是"每个站点的内存使用情况"非共享工作集。"

新的性能改进专注于这两种因素。

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>新的性能功能的要求

新功能的要求可以分为以下几类：

- 在.NET Framework 4 运行的改进。
- 需要.NET Framework 4.5，但可以在任何版本的 Windows 上运行的改进。
- 有只能与 Windows 8 上运行的.NET Framework 4.5 的改进。

性能会增加，你将能够启用的改进的每个级别。

一些.NET Framework 4.5 改进可利用的更广泛应用于其他方案，以及的性能功能。

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>共享公共程序集

**要求**:.NET Framework 4 和 Visual Studio 11 开发者预览版 SDK

在服务器上的不同站点通常使用相同的帮助程序程序集 （例如，从初学者工具包或示例应用程序的程序集）。 每个站点有其自己的这些程序集副本在其 Bin 目录中。 即使的程序集的对象代码是相同的它们物理上独立的程序集，因此每个程序集必须在冷站点启动期间单独读取并单独保存在内存中。

新的 interning 功能解决了这种低效情况，并可减少内存需求和负载时间。 暂留可让 Windows 在文件系统中，来使每个程序集的单个副本和站点 Bin 文件夹中的单个程序集将替换对单个副本的符号链接。 如果单个站点时需要不同版本的程序集，符号链接替换为新版本的程序集，并仅该站点受到影响。

共享程序集使用符号链接，需要一个名为 aspnet 的新工具\_intern.exe，这样就可以创建和管理暂存的程序集的存储。 它提供作为 Visual Studio 11 开发人员预览版 SDK 的一部分。 (但是，它将在仅安装.NET Framework 4，如果你已安装最新的系统上运行[更新](https://support.microsoft.com/kb/2468871)。)

若要确保所有符合条件的程序集具有已暂留，你可以运行 aspnet\_intern.exe 定期 （例如，每周一次作为计划任务）。 一个典型用途是，如下所示：

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

若要查看所有选项，请不带任何参数运行该工具。

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>用于启动速度更快的多核 JIT 编译

**要求**:.NET Framework 4.5

对于冷站点启动时，不仅能执行程序集具有要读取从磁盘，但的站点必须是 JIT 编译。 对于复杂的站点，这可以添加明显的延迟。 .NET Framework 4.5 中的新通用技术可减少这些延迟 JIT 编译分散可用处理器内核。 它尽可能多执行此操作并尽早使用期间收集的信息之前启动的站点。 由实现此功能[System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx)方法。

JIT 编译的使用多个内核处于启用状态默认情况下，在 ASP.NET 中，因此不需要执行任何操作来利用此功能。 如果你想要禁用此功能，请在 Web.config 文件中进行以下设置：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>若要针对内存优化的优化垃圾回收

**要求**:.NET Framework 4.5

站点运行后，垃圾回收器 (GC) 堆其使用可以为其内存使用量的重要考虑因素。 任意垃圾回收器，如.NET Framework GC 使 CPU 时间 （频率和重要性的集合） 和内存消耗 （用于新的、 已释放，或可释放的对象的额外空间） 之间的折衷方案。 为以前版本中，我们提供指导如何配置以实现最佳平衡 GC (有关示例，请参阅[ASP.NET 2.0/3.5 共享承载配置](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration))。

.NET Framework 4.5，而不是多个独立设置，工作负荷定义配置设置可用，它将使所有以前建议的 GC 设置，以及新的优化，可提供有关每个站点的其他性能工作集。

若要启用优化的 GC 内存，请向 Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config 文件中添加以下设置：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(如果你熟悉到 aspnet.config 更改的上一个指南，请注意，此设置将替代旧设置-例如，则不需要设置 gcServer、 gcConcurrent，等等。你无需删除旧的设置。）

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>预提取的 web 应用程序

**要求**: Windows 8 上运行的.NET Framework 4.5

Windows 具有几个版本包含一种技术称为[取](http://en.wikipedia.org/wiki/Prefetcher)可减少应用程序启动的磁盘读取成本。 因为冷启动客户端应用程序的主要问题，此技术尚未包含在 Windows Server 中，其中包括对 server 至关重要的组件。 预提取现已在 Windows Server，它可以在其中优化启动单独的网站的最新版本。

对于 Windows Server，在默认情况下不启用取。 若要启用和配置用于高密度 web 托管取，请在命令行运行以下命令集：

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

然后，为了与 ASP.NET 应用程序集成取，将以下代码添加到 Web.config 文件：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web 窗体

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>强类型化的数据控件

在 ASP.NET 4.5 Web 窗体包含用于使用数据的一些改进。 第一个改善是强类型化的数据控制。 对于在以前版本的 ASP.NET Web 窗体控件，显示数据绑定值使用*Eval*和数据绑定表达式：

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

对于双向数据绑定，你使用*绑定*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

在运行时，这些调用使用反射来读取的指定成员的值，然后在标记中显示的结果。 这种方法轻松对任意、 unshaped 数据的数据绑定。

但是，这样的数据绑定表达式不支持 IntelliSense 等功能的成员名称、 导航 （例如转到定义） 或编译时检查这些名称。

若要解决此问题，ASP.NET 4.5，请添加了声明将控件绑定到的数据的数据类型的功能。 执行此操作使用新*ItemType*属性。 当设置此属性时，两个新的类型化的变量是否为数据绑定表达式的作用域中可用：*项*和*BindItem*。 由于强类型化变量，你将获取 Visual Studio 开发体验的全部好处。


对于双向数据绑定表达式中，使用*BindItem*变量：

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


支持数据绑定的 ASP.NET Web 窗体 framework 中的大多数控件已更新，以支持*ItemType*属性。

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>模型绑定

模型绑定扩展 ASP.NET Web 窗体控件中的数据绑定来使用代码为中心的数据访问。 它包含从概念*ObjectDataSource*控件和从 ASP.NET MVC 中的模型绑定。

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>选择数据

若要配置一个数据控件要使用模型绑定来选择数据，你设置的控件的*SelectMethod*属性页的代码中的方法的名称。 数据控件在页生命周期中适当的时间调用的方法，并自动将绑定返回的数据。 没有无需显式调用*DataBind*方法。

在下面的示例中， *GridView*控件配置为使用一个名为方法*GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

你创建*GetCategories*页的代码中的方法。 对于简单的选择操作，该方法不需要参数，且应返回*IEnumerable*或*IQueryable*对象。 如果新*ItemType*设置属性 (它使强类型化数据绑定表达式下所述[强类型化数据控件](#_Toc318097386)更早版本)，这些接口的泛型版本应返回 — *IEnumerable&lt;T&gt;* 或*IQueryable&lt;T&gt;*，与*T*参数匹配的一种*ItemType*属性 (例如， *IQueryable&lt;类别&gt;*)。

下面的示例演示的代码*GetCategories*方法。 此示例使用 Northwind 示例数据库使用 Entity Framework Code First 模型。 这段代码将确保查询返回通过每个类别的相关产品的详细信息*包括*方法。 (这可确保*TemplateField*标记中的元素中显示的产品计数每个类别中，而无需[n + 1 选择](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem)。)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

当运行此页，则*GridView*控制调用*GetCategories*方法自动并呈现返回的数据使用的配置的字段：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

因为选择方法将返回*IQueryable*对象， *GridView*控件可以在执行前进一步操作查询。 例如， *GridView*控件可以添加用于排序和分页到返回的查询表达式*IQueryable*对象之前执行，以便这些操作由基础执行LINQ 提供程序。 在这种情况下，实体框架将确保在数据库中执行这些操作。

下面的示例演示*GridView*控件修改，以便允许排序和分页：

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

现在页运行时，控件可确保，显示数据的当前页并按所选列进行排序：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

若要筛选返回的数据，参数必须添加到选择的方法。 这些参数将在运行时，模型绑定用来填充和可用于返回数据前更改查询。

例如，假设你想要让用户筛选器产品，通过在查询字符串中输入一个关键字。 你可以向方法添加参数，并更新代码以使用参数值：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

此代码包括*其中*如果提供的值的表达式*关键字*然后返回查询结果。

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>值在提供程序

前面的示例不是有关在何处特定的值*关键字*来自参数。 若要指示此信息，可以使用参数属性。 对于此示例中，你可以使用*QueryStringAttribute*中类*System.Web.ModelBinding*命名空间：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

这会指示要尝试将值绑定到查询字符串中的模型绑定*关键字*在运行时的参数。 （这可能涉及执行类型转换，尽管它不在此情况下。）如果不能提供的值且不可为 null 的类型，是引发异常。

这些方法的值的源统称为值在提供程序，并指示要使用的值提供程序的参数属性统称为值提供程序属性。 Web 窗体将包括在 Web 窗体应用程序，例如查询字符串、 cookie、 窗体值、 控件、 视图状态、 会话状态和配置文件属性的值在提供程序和所有用户输入的典型源的相应属性。 你还可以编写自定义值在提供程序。

默认情况下，参数名称用于作为键的值提供程序集合中查找一个值。 在示例中，代码将查找名为关键字的查询字符串值 (例如，~ / default.aspx?keyword=chef)。 你可以通过将它作为自变量传递给参数属性中指定的自定义密钥。 例如，若要使用名为 q 的查询字符串变量的值，无法执行此操作：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

如果此方法在页的代码中，用户可以通过使用查询字符串的关键字筛选结果：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

模型绑定可实现那些原本要进行手动编码的许多任务： 读取的值、 检查是否有 null 值，尝试将其转换为适当的类型，检查指示转换是否成功，和最后，使用中的值查询。 模型绑定结果，在更少代码和进行重复使用在整个应用程序的功能的能力。

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>筛选来自控件的值

假设你想要扩展的示例，以使用户能够从下拉列表中选择一个筛选器值。 将下面的下拉列表添加到标记并将其配置为从另一个方法使用获取其数据*SelectMethod*属性：

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

通常还会添加*要*元素*GridView*控制，以便控件将显示一条消息，如果不找到任何匹配产品：

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

在网页代码中，将添加新选择的下拉列表的方法：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

最后，更新*GetProducts*选择方法才能包含下拉列表从所选类别的 ID 的新参数：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

现在页运行时，用户可以从下拉列表中，选择类别和*GridView*控件是自动显示经过筛选的数据重新绑定。 这可能是因为模型绑定跟踪选择方法的参数的值，并检测是否在回发后已更改任何参数值。 如果是这样，模型绑定强制关联的数据控件重新绑定到数据。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>HTML 编码数据绑定表达式

你可以现在进行 HTML 编码的数据绑定表达式的结果。 将一个冒号 （:） 添加到末尾&lt;%# 前缀将标记数据绑定表达式：

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>非介入式验证

你现在可以配置用于非介入式 JavaScript 进行客户端验证逻辑的内置的验证程序控件。 这显著缩短 JavaScript 呈现的页标记中的内联量，并减少总体的页大小。 你可以在以下任一方式来配置非介入式 JavaScript 的验证程序控件：

- 全局通过添加以下设置来 *&lt;appSettings&gt;*  Web.config 文件中的元素： 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- 通过设置静态全局*System.Web.UI.ValidationSettings.UnobtrusiveValidationMode*属性*UnobtrusiveValidationMode.WebForms* (通常在*应用程序\_启动*Global.asax 文件中的方法)。
- 通过设置新的页为单独*UnobtrusiveValidationMode*属性*页*类到*UnobtrusiveValidationMode.WebForms*。

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>HTML5 更新

某些已得到改进 Web 窗体到服务器控件，若要利用的 HTML5 的新功能：

- *TextMode*属性*文本框中*已更新控件以支持如新的 HTML5 输入的类型*电子邮件*， *datetime*，和等等。
- *FileUpload*控件现在支持多个文件上载从支持此 HTML5 功能的浏览器。
- 验证程序控制现在支持验证 HTML5 输入的元素。
- 具有现在表示 URL 的特性的新 HTML5 元素支持 runat ="server"。 因此，你愿意，可以使用 ASP.NET 约定 URL 路径中 ~ 运算符来表示应用程序根目录 (例如，&lt;视频 runat ="server"src="~/myVideo.wmv"/&gt;)。
- *UpdatePanel*控件具有已固定的以便支持发布 HTML5 输入的字段。

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 Beta 现附带 Visual Studio 11 Beta。 ASP.NET MVC 是一个框架，用于开发高度可测试和可维护 Web 应用程序通过利用模型-视图-控制器 (MVC) 模式。 ASP.NET MVC 4 可以轻松地生成适用于移动 Web 应用程序，并包含 ASP.NET Web API，可帮助你生成可以访问任何设备的 HTTP 服务。 有关详细信息，请参阅[ASP.NET MVC 4 发行说明](mvc4-release-notes.md)。

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>ASP.NET 网页 2

新功能包括：

- 新的和更新站点模板。
- 添加服务器端和客户端验证使用*验证*帮助器。
- 注册脚本使用资产管理器的能力。
- 启用从 Facebook 和其他站点使用 OAuth 和 OpenID 登录名。
- 使用添加图*映射*帮助器。
- 运行 Web 页面应用程序的并行。
- 移动设备的呈现页。

有关这些功能和整页的代码示例的详细信息，请参阅[顶部功能网页 2 beta](https://go.microsoft.com/fwlink/?LinkID=227824)。

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

本部分提供有关在 Visual Web Developer 11 Beta 和 Visual Studio 2012 候选发布版本中的 web 开发的改进的信息。

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Visual Studio 2010 和 Visual Studio 2012 候选发布版本 （项目兼容性） 之间共享的项目

Visual Studio 2012 候选发布版本，直到在较新版本的 Visual Studio 中打开现有项目启动转换向导。 这为未向后兼容的新格式强制升级的内容 （资产） 的项目和解决方案。 因此，在转换后你无法打开该项目中较旧版本的 Visual Studio。

许多客户告诉我们这不是适当的方法。 在 Visual Studio 11 Beta，我们现在支持共享的项目和解决方案与 Visual Studio 2010 SP1。 这意味着，如果你在 Visual Studio 2012 候选发布版本中打开 2010年项目，你仍将能够在 Visual Studio 2010 SP1 中打开项目。

> [!NOTE]
> Visual Studio 2010 SP1 和 Visual Studio 2012 候选发布版本之间无法共享几种类型的项目。 这些包括一些 （如 ASP.NET MVC 2 项目） 的较旧项目 （如安装程序项目） 的特殊用途。

在 Visual Studio 11 Beta 中首次打开 Visual Studio 2010 SP1 Web 项目时，以下属性添加到项目文件：

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

升级项目文件的过程使用 FileUpgradeFlags、 UpgradeBackupLocation 和 OldToolsVersion。 它们会产生任何影响使用 Visual Studio 2010 中的项目。

VisualStudioVersion 是使用 MSBuild 4.5，该值指示当前项目的版本的 Visual Studio 的一个新属性。 因为此属性没有在 MSBuild 4.0 （Visual Studio 2010 SP1 使用的 MSBuild 的版本） 中，我们将默认值插入项目文件。

VSToolsPath 属性用于确定正确的.targets 文件以导入从 MSBuildExtensionsPath32 设置所表示的路径。

也有一些与导入元素相关的更改。 这些更改是为了支持这两个版本的 Visual Studio 之间的兼容性必需的。

> [!NOTE]
> 如果项目 Visual Studio 2010 SP1 和 Visual Studio 11 Beta 之间两个不同的计算机上，共享的并且该项目包括在应用程序的本地数据库\_数据文件夹，你必须确保使用的数据库的 SQL Server 的版本是在这两台计算机上安装。

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>ASP.NET 4.5 网站模板中的配置更改

为默认值进行了以下更改*Web.config*在 Visual Studio 2012 候选发布版本中使用的网站模板创建的网站文件：

- 在`<httpRuntime>`元素，`encoderType`属性现在设置默认情况下，若要使用已添加到 ASP.NET AntiXSS 类型。 有关详细信息，请参阅[AntiXSS 库](#_Toc318097382)。
- 另外，请在`<httpRuntime>`元素，`requestValidationMode`属性设置为"4.5"。 这意味着默认情况下，请求验证配置为使用延迟的 （"迟缓"） 验证。 有关详细信息，请参阅[新的 ASP.NET 请求验证功能](#_Toc318097379)。
- `<modules>`元素`<system.webServer>`部分不包含`runAllManagedModulesForAllRequests`属性。 （其默认值为 false）这意味着，如果你使用的 IIS 7 且尚未更新到 SP1 的版本，你可能具有与新站点中的路由的问题。 有关详细信息，请参阅[在 IIS 7 ASP.NET 路由中的本机支持](#Native_Support_In_IIS7_For_ASPNET_Routine)。

这些更改不会影响现有应用程序。 但是，它们可能表示现有的网站和为 ASP.NET 4.5 使用新的模板创建的新网站之间的行为差异。

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>在 IIS 7 ASP.NET 路由中的本机支持

这不是 ASP.NET 的更改在这种情况下，但如果你正在尚未应用 SP1 更新的 IIS 7 的版本会影响你的新网站项目的模板中的更改。

在 ASP.NET 中，你可以将以下配置设置添加到应用程序以支持路由：

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

时**runAllManagedModulesForAllRequests**是 true，如 URL`http://mysite/myapp/home`就会转到 ASP.NET，即使没有任何*.aspx*， *.mvc*，或类似的扩展名URL。

IIS 7，已更新使**runAllManagedModulesForAllRequests**设置不必要和支持 ASP.NET 本机路由。 (有关更新的信息，请参阅 Microsoft 支持文章[有可用启用某些 IIS 7.0 或 IIS 7.5 的处理程序，来处理请求其 Url 不要以句点结尾的更新](https://support.microsoft.com/kb/980368)。)

如果在 IIS 7 上运行你的网站和 IIS 已更新，如果你不需要设置**runAllManagedModulesForAllRequests**为 true。 事实上，将其设置为 true 不是建议，因为它会不必要的处理开销到请求。 当此设置为 true 时，所有请求，包括*.htm*， *.jpg*，和其他静态文件，也需要通过 ASP.NET 请求管道。

如果创建新的 ASP.NET 4.5 网站，以使用 Visual Studio 2012 RC 中提供的模板时，该网站的配置不包括**runAllManagedModulesForAllRequests**设置。 这意味着默认情况下设置为 false。

然后，如果你运行的网站在 Windows 7 上没有安装 SP1 的情况下，IIS 7 将不包含所需的更新。 因此，路由无法工作，你将看到错误。 如果你有问题，其中路由不起作用，可以执行以下任一操作：

- 更新到 SP1，会将更新添加到 IIS 7 的 Windows 7。
- 安装前面列出的 Microsoft 支持文章中所述的更新。
- 设置**runAllManagedModulesForAllRequests**为 true，该网站的 Web.config 文件中。 请注意，这将向请求添加一些开销。

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>HTML 编辑器

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>智能任务

在设计视图中，复杂属性的服务器控件通常具有关联的对话框和向导，以便可以方便地设置它们。 例如，可以使用特殊的对话框中添加到数据源*中继器*控制或将列添加到*GridView*控件。

但是，这种类型的复杂属性的 UI 帮助尚未在源视图中可用。 因此，Visual Studio 11 引入了源视图的智能任务。 智能任务是在 C# 和 Visual Basic 编辑器的常用功能的上下文感知快捷方式。

对于 ASP.NET Web 窗体控件，智能任务在上显示服务器标记为小型的标志符号时插入点位于元素内：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

在你单击的标志符号或按 CTRL + 的扩展智能任务。 （点），代码编辑器中一样。 然后，它将显示类似于设计视图中的智能任务的快捷方式。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

例如，智能任务在上图中显示的 GridView 任务选项。 如果选择编辑列，将显示以下对话框中：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

填写对话框设置相同的属性可以设置在设计视图中。 当你单击确定时，会随新的设置更新控件的标记：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>WAI ARIA 支持

编写可访问的网站变得越来越重要。 [WAI ARIA 辅助功能标准](http://www.w3.org/WAI/intro/aria)定义开发人员编写可访问的网站的方式。 在 Visual Studio 中现在完全支持此标准。

例如，*角色*属性现在具有完整的 IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

WAI ARIA 标准还引入了带有前缀的特性*aria-*能够让你将语义添加到 HTML5 文档。 Visual Studio 还完全支持这些*aria-*属性：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>新的 HTML5 代码段

若要使其更快且更易于编写常用的 HTML5 标记，Visual Studio 提供了大量的代码段。 视频段是一个示例：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

调用代码段，请按 tab 键两次时在 IntelliSense 中选择了该元素：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

这将生成你可以自定义代码段。

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>提取到用户控件

在大型网页中，它可以将各个部分移到用户控件的一个好办法。 这种形式的重构有助于提高页的可读性，并可以简化页结构。

若要简化此过程，当您编辑在源视图中的 Web 窗体页面时，你可以现在在页中选择文本，右键单击它，，然后选择提取到用户控件：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>在属性中的代码而有价值的 IntelliSense

Visual Studio 始终提供的任何页面或控件中的服务器端代码而有价值的 IntelliSense。 现在 Visual Studio 包括 IntelliSense 中以及 HTML 特性的代码而有价值。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

这使得更轻松地创建数据绑定表达式：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>当你重命名一个开始标记或结束标记时匹配标记的自动重命名

如果重命名的 HTML 元素 (例如，更改*div*标记要*标头*标记)，则相应的打开或关闭标记也会更改实时。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

这有助于避免忘记更改结束标记或更改的错误的一个错误。

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>事件处理程序生成

Visual Studio 现在包含在源视图来帮助你编写事件处理程序并手动将其绑定中的功能。 如果你正在编辑源视图中的事件名称，IntelliSense 将显示&lt;创建新事件&gt;，这将创建一个事件处理程序页的代码中包含正确的签名：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

默认情况下，事件处理程序将使用控件的 ID，用于事件处理方法的名称：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

生成的事件处理程序将如下所示 （在这种情况下，在 C# 中）：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>智能缩进

按 enter 键空 HTML 元素中的时，编辑器将将插入点放在正确的位置：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

如果按 enter 键，在此位置，结束标记是向下移动和缩进以匹配的开始标记。 插入点还缩进：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>自动减少语句结束

Visual Studio 现在筛选器基于，使其显示仅相关选项输入的内容中的 IntelliSense 列表：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense 还筛选器基于的智能感知列表中的各单词的词首字母大写。 例如，如果键入"dl"，dl 和 asp: DataList 显示：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

此功能可以更快地获取已知元素的语句结束。

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript 编辑器

Visual Studio 2012 候选发布版本中的 JavaScript 编辑器是全新并极大地改善了使用 Visual Studio 中的 JavaScript 的体验。

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>代码大纲显示

大纲区域，现在将自动创建适用于所有功能，从而可以折叠不可与你当前焦点相关的文件部分。

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>大括号匹配

将插入点放在一个开始标记或右大括号后，编辑器突出显示匹配的一个。

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>转到定义

转到定义命令，可以跳转到函数或变量的源。

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>ECMAScript5 支持

编辑器在 ECMAScript5，描述 JavaScript 语言的标准的最新版本中支持的新语法和 Api。

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>DOM IntelliSense

DOM Api 的 IntelliSense 得到了提高，支持的许多新 HTML5 Api 包括与*querySelector*，DOM 存储，跨文档消息传递，和*画布*。 按一个单一的简单 JavaScript 文件，而不是本机类型库定义现在驱动 DOM IntelliSense。 这使得容易扩展或替换。

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>VSDOC 签名重载

详细的 IntelliSense 注释现在为 JavaScript 函数的单独重载使用新的声明*&lt;签名&gt;*元素，如本示例中所示：

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>隐式引用

你现在可以将 JavaScript 文件添加到将隐式包含在文件列表中，任何给定的 JavaScript 文件或块的引用，这意味着，你将获得 IntelliSense 其内容的中央列表。 例如，可以将 jQuery 文件添加到中央列表的文件，并且你将获取 IntelliSense jQuery 函数在文件中，任何 JavaScript 块是否已显式引用它 (使用 / /&lt;引用 /&gt;) 或不。

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>CSS 编辑器

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>自动减少语句结束

用于 CSS 现在筛选器基于的 CSS 属性和所选架构支持的值的智能感知列表。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense 还支持标题大小写搜索：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>分层缩进

CSS 编辑器用于缩进显示层次结构的规则，这为你提供的级联的规则的逻辑上组织方式概述。 在下面的示例中，#list 选择器级联子级的列表，并且因此缩进。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

下面的示例演示更复杂的继承：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

由其父规则确定的规则的缩进。 默认情况下，启用分层缩进，但你可以禁用它 （工具，从菜单栏中的选项） 选项对话框：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>CSS hacks 支持

数百个真实世界 CSS 文件分析显示 CSS 黑客攻击是很常见，并且 Visual Studio 现在支持使用最广泛的。 这种支持包括 IntelliSense 和验证在星型 (\*) 和下划线 (\_) 属性黑客攻击：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

典型的选择器黑客攻击也支持以便即使在它们应用维护分层缩进。 用于对目标 Internet Explorer 7 典型的选择器 hack 是前面附加与选择器 *\*： 第一个子级 + html*。 使用该规则将维护的分层缩进：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>供应商特定架构 (-moz--webkit)

CSS3 引入了在不同时间由不同的浏览器实现的许多属性。 以前，这使用特定于供应商的语法通过强制为特定浏览器的代码的开发人员。 这些浏览器特定属性现在包含在 IntelliSense 中。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>注释和 uncommenting 支持

现在，你可以注释，并取消注释使用代码编辑器 (Ctrl + K、 注释的 C 和 Ctrl + K，你可以取消注释） 中使用的同一个快捷方式的 CSS 规则。

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>颜色选取器

在以前版本的 Visual Studio 中，与颜色相关的属性的智能感知功能包括命名的颜色值的下拉列表。 全功能颜色选取器已替换为该列表。

当你输入颜色值时，颜色选取器会自动显示，并显示以前用过跟默认颜色调色板的颜色的列表。 你可以选择使用鼠标或键盘的颜色。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

列表可以扩展到完成颜色选取器。 选取器，可通过自动将任何颜色转换 RGBA 时移动的不透明度滑块来控制 alpha 通道：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>代码段

CSS 编辑器中的代码段使其更加容易和快速创建跨浏览器样式。 需要特定于浏览器的设置的许多 CSS3 属性现在已回滚到代码段。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

CSS 代码段支持高级的方案 （如 CSS3 媒体查询），通过键入在符号 (@)，其中说明了智能感知列表。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

当选择@media值并按 tab 键，CSS 编辑器将插入以下代码段：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

与代码段，你可以创建自己的 CSS 代码段。

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>自定义区域

名为代码区域，在已经可用在代码编辑器中，现可用于编辑 CSS。 这样可以轻松地分组相关的样式块。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

当区域处于折叠状态时将显示区域的名称：

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Page Inspector

Page Inspector 是一种工具，可以呈现网页 （HTML、 Web 窗体、 ASP.NET MVC 或网页） 在 Visual Studio IDE 中，并允许您检查源代码和生成的输出。 适用于 ASP.NET 页中，Page Inspector 可让你确定哪些服务器端代码生成到浏览器呈现的 HTML 标记。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

有关 Page Inspector 的详细信息，请参阅以下教程：

- 使用 Page Inspector 中[ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- 使用 Page Inspector 中[ASP.NET Web 窗体](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>发布

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>发布配置文件

在 Visual Studio 2010 中，对于 Web 应用程序项目的发布信息不会存储在版本控制而不是与其他人共享。 Visual Studio 2012 候选发布版本中，已更改的发布配置文件的格式。 已经过一个团队项目，并且现在可以轻松地从基于 MSBuild 的生成使用。 生成配置信息位于发布对话框中，以便您可以轻松地切换在发布前的生成配置。

发布配置文件将存储在 PublishProfiles 文件夹。 文件夹的位置取决于你正在使用哪种编程语言：

- C#: Properties\PublishProfiles
- Visual Basic： 我 Project\PublishProfiles

每个配置文件是一个 MSBuild 文件。 在发布期间，此文件将导入项目的 MSBuild 文件。 在 Visual Studio 2010 中，如果你想要更改发布或包过程中，则必须将你的自定义放入名为的文件**ProjectName**。 wpp.targets。 仍支持此行为，但现在可以将你的自定义放入发布配置文件本身。 这样一来，将仅对该配置文件使用自定义项。

你可以现在还利用发布配置文件，从 MSBuild。 为此，请使用以下命令生成项目时：

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Project.csproj 值是从项目的路径，ProfileName 是要发布的配置文件的名称。 或者，而不是传递的配置文件名称*PublishProfile*属性，您可以传入的完整路径到发布配置文件。

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>ASP.NET 预编译和合并

对于 Web 应用程序项目，Visual Studio 2012 候选发布版本中添加一个可用于预编译和合并发布时的站点的内容或包项目的打包/发布 Web 属性页上的选项。 若要查看这些选项，用鼠标右键单击解决方案资源管理器中的项目，选择属性，然后选择打包/发布 Web 属性页。 下图显示 Precompile 此应用程序，然后发布选项。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

选中此选项后，Visual Studio 预编译的应用程序每次您发布或打包 web 应用程序。 如果你想要控制如何预编译站点或如何合并程序集，请单击高级按钮以配置这些选项。

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

在 Visual Studio 中的测试 web 项目的默认 web 服务器现在是 IIS Express。 Visual Studio 开发服务器仍是本地 web 服务器在开发期间，一个选项，但 IIS Express 现在是建议的服务器。 在 Visual Studio 11 Beta 中使用 IIS Express 的体验是非常类似于使用 Visual Studio 2010 SP1 中。

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>免责声明

这是一份初稿，并可能在本文所述软件最终商业发布之前进行大幅更改。

本文所含信息代表 Microsoft Corporation 对截至发布之日所讨论问题持有的当前观点。 由于 Microsoft 必须对不断变化的市场情况作出响应，所以不应将本文解释为是 Microsoft 做出的承诺，Microsoft 并不保证所提供的任何信息在公布之日后的准确性。

本白皮书仅用于提供信息。 MICROSOFT 对本文档中的信息不做任何明示、暗示或法定的担保。

遵守所有适用的著作权法是用户的责任。 未经 Microsoft Corporation 明确的书面许可，不得出于任何目的或以任何形式或任何手段（电子、机械、影印、记录或其他方法）复制本文档的任何部分，或者将其存储或引入检索系统，或者将其进行传播。受版权法保护的权利不受此限制。

对于本文档中的主题，Microsoft 可能具有专利、专利申请、商标、版权或其他知识产权。 除非 Microsoft 的任何书面许可协议明确提出，否则，本文档的提供并不表示 Microsoft 已将这些专利、商标、版权或其他知识产权的任何许可权限授予您。

除非另行说明，示例公司、 组织、 产品、 域名、 电子邮件地址、 徽标、 人物、 地点和事件此处所述虚构，是的无意与任何真实的公司、 组织、 产品、 域名、 电子邮件地址、 徽标、 人员、 位置或事件旨在或妄加推断。

(C) 2012 Microsoft Corporation。 保留所有权利。

Microsoft 和 Windows 是 Microsoft Corporation 在美国和/或其他国家/地区的注册商标或商标。

此处提到的真实公司和产品的名称可能是其各自所有者的商标。
