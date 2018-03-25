---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: "项目 Katana 概述 |Microsoft 文档"
author: howarddierking
description: "ASP.NET Framework 已超过 10 年，并且平台已启用无数网站和服务的开发。 为 Web 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/30/2013
ms.topic: article
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: ceb7d3a7d1cb1685c0f1e62698f508c9a73e77c2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="an-overview-of-project-katana"></a>项目 Katana 事件的概述
====================
通过[Howard Dierking](https://github.com/howarddierking)

> ASP.NET Framework 已超过 10 年，并且平台已启用无数网站和服务的开发。 随着 Web 应用程序开发策略具有发展，框架已能够以与 ASP.NET MVC 和 ASP.NET Web API 等技术步骤而逐步演化。 随着 Web 应用程序开发采取到云计算世界上的下一个演化步骤，项目[Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET)提供基础的 ASP.NET 应用程序，从而实现才能灵活且可移植，这些组件轻量，并提供更好的性能 – put 另一种方法、 项目[Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET)云优化你的 ASP.NET 应用程序。


## <a name="why-katana--why-now"></a>为什么 Katana – 现在实施的原因？

 无论是否其中一个讨论开发人员 framework 或最终用户产品，务必要了解创建的基础动机产品 – 和在此过程包括了解谁创建产品。 ASP.NET 最初是通过记住的两个客户创建的。   
  
**第一组客户已经典 ASP 开发人员。** 当时，ASP 将已用于创建网站和应用程序动态、 数据驱动的交错执行标记和服务器端脚本的主要技术之一。 ASP 运行时提供服务器端脚本的一组对象的基础 HTTP 协议和 Web 服务器的核心方面中抽象出来，并提供访问的其他服务此类对会话和应用程序状态的管理缓存，等等。虽然功能强大，经典 ASP 应用程序变得很难管理，因为它们在大小和复杂性方面增长。 这是结构的代码的很大程度上是结构的代码的由于缺乏脚本结合导致的代码和标记交错的重复的环境中。 若要充分利用的经典 ASP 的优势，同时还解决了一些其难题，ASP.NET 利用代码组织语言提供的面向对象的.NET Framework 的同时还保留的服务器端编程模型到哪些经典 ASP 开发人员已达到习惯。

**第二个 ASP.NET 的目标客户组时 Windows 业务应用程序开发人员。** 与经典 ASP 开发人员，他们已习惯于编写 HTML 标记和代码，以生成更多的 HTML 标记，不同 （如之前 VB6 开发人员） WinForms 开发人员已习惯于包含画布和一组丰富的用户设计时体验界面控件。 ASP.NET – 的第一个版本也称为"Web 窗体"提供了类似的设计时体验以及用户界面组件的服务器端事件模型和基础结构功能 （如视图状态） 的一组创建无缝的开发人员体验之间客户端和服务器端编程。 Web 窗体有效地隐藏已熟悉的 WinForms 开发人员有状态的事件模式下的站点的无状态性质。

### <a name="challenges-raised-by-the-historical-model"></a>引发的历史模型的挑战

**最终结果是成熟、 功能丰富的运行时和开发人员的编程模型。** 但是，与该功能丰富功能附带几个值得注意的挑战。 首先，框架已**整体**，使用的同一程序集中 System.Web.dll （例如，使用 Web 窗体 framework 的核心 HTTP 对象） 进行紧密耦合的功能的逻辑上不同单位。 其次，ASP.NET 已包含的是一个更大的.NET framework，这意味着在**版本之间的时间为几年。** 这使得难以 ASP.NET 以跟上的所有更改快速发展的 Web 开发中发生的情况。 最后，System.Web.dll 本身已耦合在几个不同方面与特定的 Web 托管选项： Internet 信息服务 (IIS)。

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>演化步骤： ASP.NET MVC 和 ASP.NET Web API

和大量更改在 Web 开发过程中发生的情况 ！ 为一系列小，焦点组件，而不是大型框架越来越正在开发 web 应用程序。 组件，以及与该发布的频率数已增加过快的速度。 很显然，与 Web 节奏保留需要框架，而不是更大、 功能更丰富，因此获取较小，分离且更集中**ASP.NET 团队花费了几个演化步骤来启用 ASP.NET 作为一系列可插入 Web 组件，而不是单个 framework**。

早期的更改之一是受欢迎程度感谢如 Ruby 的 Web 开发框架的已知模型-视图-控制器 (MVC) 设计模式中出现增加，on Rails。 这种样式的构建 Web 应用程序提供开发人员更好地控制其应用程序的标记，同时仍保留标记和业务逻辑，这是 ASP.NET 的初始卖点之一的分离。 为了满足这种样式的 Web 应用程序开发的需求，Microsoft 花费了机会来为自身定位更适用于通过将来**带外开发 ASP.NET MVC** （和其不包括在.NET Framework）。 ASP.NET MVC 发布为独立的下载。 这使得工程团队能够灵活地比以前可能过更频繁提供更新。

Web 应用程序开发中的另一个重大的转变已通过生成客户端脚本进行通信的页的动态部分静态初始标记从动态、 服务器生成 Web 页面转向**与后端 Web Api 通过AJAX 请求**。 此体系结构 shift 有助于推动 Web Api 的增加和 ASP.NET Web API 框架的开发。 对于 ASP.NET MVC，如 ASP.NET Web API 的版本提供了另一个作为更加模块化框架进一步发展 ASP.NET 的机会。 工程团队花费了利用这个机会和**生成 ASP.NET Web API，以便它在任何 System.Web.dll 中找到的核心框架类型上没有依赖关系**。 这将启用以下两项操作： 首先，它意味着 ASP.NET Web API 无法以完全自包含的方式而逐步演化 （和它可能会继续以快速循环，因为它通过 NuGet 提供）。 其次，由于没有对 System.Web.dll，没有外部依赖关系，因此，对 IIS 没有依赖关系，因此 ASP.NET Web API 将包含在自定义主机 （例如，控制台应用程序、 Windows 服务等。） 中运行的功能

### <a name="the-future-a-nimble-framework"></a>敏捷 Framework 的未来：

通过分离从另一个 framework 组件，然后在 NuGet 上发布它们，框架现在无法**循环更独立和更快地**。 此外的功能和灵活性的 Web API 的自承载功能证明极具吸引力的开发人员可以想**小的轻型主机**针对其服务。 的确如此大的吸引力，事实上，使其他框架还希望此功能，和，因为每个框架已在其自己的主机进程中对其自身的基址运行并且需要进行管理 （启动、 停止等），这会显示一个新的挑战独立。 现代 Web 应用程序通常支持静态文件服务、 生成动态页面、 Web API 和多最近实时-一次性/推送通知。 应为每个这些服务应可运行和管理独立时只是不现实。

我们需要的是单个的宿主抽象，将启用开发人员可以编写从多种不同的组件和框架，应用程序，然后运行支持的主机上的该应用程序。

## <a name="the-open-web-interface-for-net-owin"></a>.NET (OWIN) 打开 Web 接口

 由通过实现的优势激发[机架](http://rack.github.io/)Ruby 社区，多个.NET 社区成员着手创建 Web 服务器和框架组件之间的抽象。 用于 OWIN 抽象的两个设计目标是它是简单，并且它对其他 framework 类型执行大程度地减少可能的依赖项。 这两个目标帮助确保：

- 新的组件可以更轻松地开发和使用。
- 应用程序无法将更轻松地移植主机和可能整个平台/操作系统之间。

生成抽象包含两个核心元素。 第一种是环境字典。 此数据结构是状态的负责存储所有用于处理 HTTP 请求和响应，以及任何相关的服务器状态所需。 环境字典定义如下：

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

OWIN 兼容 Web 服务器负责填充环境字典中，如正文流和 HTTP 请求和响应的标头集合的数据。 然后，它是用于填充或使用其他值更新字典和写入到响应正文流的应用程序或框架组件的责任。

除了指定环境字典类型，OWIN 规范定义的核心字典键/值对的列表。 例如下, 表显示 HTTP 请求的所需的词典密钥：

| 键名 | 值说明 |
| --- | --- |
| `"owin.RequestBody"` | 请求正文，如果任何流。 Stream.Null 可能用作占位符，如果没有任何请求正文。 请参阅[请求正文](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics)。 |
| `"owin.RequestHeaders"` | `IDictionary<string, string[]>`的请求标头。 请参阅[标头](http://owin.org/html/owin.html#3-3-headers)。 |
| `"owin.RequestMethod"` | A`string`包含请求的 HTTP 请求方法 (例如， `"GET"`， `"POST"`)。 |
| `"owin.RequestPath"` | A`string`包含请求路径。 该路径必须相对于"根"的应用程序委托中;请参阅[路径](http://owin.org/html/owin.html#5-3-paths)。 |
| `"owin.RequestPathBase"` | A`string`包含对应于应用程序委托中;"根目录"请求路径的一部分请参阅[路径](http://owin.org/html/owin.html#5-3-paths)。 |
| `"owin.RequestProtocol"` | A`string`包含的协议名称和版本 (例如`"HTTP/1.0"`或`"HTTP/1.1"`)。 |
| `"owin.RequestQueryString"` | A`string`包含查询字符串部分的 HTTP 请求的 URI，而无需前导"？"(例如， `"foo=bar&baz=quux"`)。 值可能为空字符串。 |
| `"owin.RequestScheme"` | A`string`包含用于请求的 URI 方案 (例如， `"http"`， `"https"`); 请参阅[URI 方案](http://owin.org/html/owin.html#5-1-uri-scheme)。 |

OWIN 第二个关键元素为应用程序的委托。 这是一个函数签名，它可作为 OWIN 应用程序中的所有组件之间的主要接口。 应用程序委托的定义如下所示：

`Func<IDictionary<string, object>, Task>;`

应用程序委托则只需 Func 委托类型，该函数接受环境字典作为输入并返回一个任务的实现。 此设计具有面向开发人员的多个含义：

- 有极小的数字的目的是将写入 OWIN 组件所需的类型依赖项。 这极大地提高了开发人员的 OWIN 的可访问性。
- 异步设计允许的抽象，可以高效地使用其处理的计算资源，尤其是在多个 I/O 密集型操作。
- 因为应用程序委托是一个执行的原子单元，并且由于环境字典所为的委托的参数进行的因此可以轻松地链接 OWIN 组件在一起以创建复杂的 HTTP 处理管道。

从实现的角度看，OWIN 是一种规范 ([http://owin.org/html/owin.html](http://owin.org/html/owin.html))。 其目标是不进行下一步的 Web 框架，但而是一个用于 Web 框架和 Web 服务器的交互方式的规范。

如果已调查[OWIN](http://owin.org/)或[Katana](https://github.com/aspnet/AspNetKatana/wiki)，你可能还会发现[Owin NuGet 包](http://nuget.org/packages/Owin)和 Owin.dll。 此库包含单个接口， [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs)，它把正式化，并对中所述的启动顺序[节的第 4](http://owin.org/html/owin.html#4-application-startup)的 OWIN 规范。 尽管不要求这样做才能生成 OWIN 服务器[IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs)接口可提供具体的参考点，而且它由 Katana 项目组件。

## <a name="project-katana"></a>项目 Katana

而同时[OWIN](http://owin.org/html/owin.html)规范和*Owin.dll*是拥有的社区和社区运行开源工作、 [Katana](https://github.com/aspnet/AspNetKatana/wiki)项目表示的一套 OWIN尽管仍处于打开状态的源的创建和 Microsoft 发布的组件。 这些组件包括基础结构组件，例如主机和服务器，以及功能组件，如身份验证组件和绑定到框架如[SignalR](../../../signalr/index.md)和[ASP.NET WebAPI](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md)。 该项目包含以下三个高级目标： 

- **可移植**– 组件应该能够轻松地替换为新组件变得可用。 这包括所有类型的组件，从服务器和主机的 framework。 此目标的含意为，第三方框架可以无缝 Microsoft 在服务器上运行时 Microsoft 框架可以在第三方服务器和主机上运行。
- **模块化/灵活**– 与很多框架包括大量的默认开启的功能，不同 Katana 项目组件应小且集中，将控制权交给应用程序开发人员在确定哪些组件在她的应用程序中使用。
- **轻型/高性能/可扩展**– 中断的传统概念是一个框架到小型的一组组件添加显式由应用程序开发人员，生成的 Katana 应用程序可以使用更少的计算的已设定焦点资源，并因此，与其他类型的服务器和框架比处理更多负载。 如应用程序所要求的底层基础结构中的多个功能，则可以添加到 OWIN 管道，但这应该是应用程序开发人员部分的显式决定。 此外，较低级别组件的可替换性意味着，当它们变得可用，新的高性能服务器可以无缝引入来提高而不会破坏这些应用程序的 OWIN 应用程序的性能。

## <a name="getting-started-with-katana-components"></a>开始使用 Katana 组件

当首次引入的一个方面[Node.js](http://nodejs.org/)与未立即绘制人的关注的框架时，一个无法与其制作和运行 Web 服务器的简单性。 如果 Katana 目标已确定框架，light 的[Node.js](http://nodejs.org/)，一个可能 Katana 提供了许多的优势来汇总它们[Node.js](http://nodejs.org/) （和类似的框架） 而不进行强制开发人员引发她就会了解有关开发 ASP.NET Web 应用程序的所有内容。 对于此语句中为真的，如何开始使用 Katana 项目应为到性质同样简单[Node.js](http://nodejs.org/)。

## <a name="creating-hello-world"></a>创建"Hello World ！"

JavaScript 和.NET 开发之间的一个显著区别是编译器的存在 （或不存在）。 在这种情况下，简单的 Katana 服务器的起始点是一个 Visual Studio 项目。 但是，我们可以开始使用最少的项目类型： 空的 ASP.NET Web 应用程序。

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

接下来，我们将安装[Microsoft.Owin.Host.SystemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)到项目的 NuGet 包。 此包提供在 ASP.NET 请求管道中运行的 OWIN 服务器。 可以在上找到[NuGet 库](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)，可以使用 Visual Studio 包管理器对话框或包管理器控制台使用以下命令安装：

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

安装`Microsoft.Owin.Host.SystemWeb`包将安装几个其他包为依赖关系。 这些依赖项之一是`Microsoft.Owin`，一个库其中提供了几个帮助器类型和用于开发 OWIN 应用程序的方法。 我们可以使用这些类型来快速编写以下的"你好 world"服务器。

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

现在可以使用 Visual Studio 运行此非常简单的 Web 服务器**F5**命令并提供对调试的完整支持。

## <a name="switching-hosts"></a>切换主机

默认情况下，前面的"你好 world"示例在 ASP.NET 请求管道中，在 IIS 的上下文中使用.w e b 中运行。 这可以自行添加了巨大的价值因为它使我们能够受益的灵活性和 OWIN 管道的管理功能的可组合性和总体更加成熟，IIS。 但是，可能是由 IIS 提供的好处是不需要，愿意不适用于较小的更轻型主机。 所需的然后，运行 IIS 和 System.Web 之外我们简单 Web 服务器？

为了说明可移植性目标，将从命令行主机的 Web 服务器主机需要只需将新的服务器和主机依赖项添加到项目的输出文件夹，然后启动主机。 在此示例中，我们将主机名的 Katana 主机中我们 Web 服务器`OwinHost.exe`，并将使用基于 Katana HttpListener 的服务器。 同样的其他 Katana 组件，这些将获取从 NuGet 使用以下命令：

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

从命令行中，我们可以然后导航到项目根文件夹，并只需运行`OwinHost.exe`（这其各自的 NuGet 包的工具文件夹中安装的）。 默认情况下，`OwinHost.exe`配置为查找的基于 HttpListener 的服务器，因此不需进行任何其他配置。 在 Web 浏览器中导航`http://localhost:5000/`显示通过控制台现在正在运行的应用程序。

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Katana 体系结构

 Katana 组件体系结构将划分成四个逻辑层，应用程序，如下所示：*主机、 服务器、 中间件，*和*应用程序*。 组件体系结构计入，实现这些层可轻松地使用替代，在许多情况下，而无需重新编译的应用程序的方式。   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Host

 主机负责下列操作：

- 管理基础的过程。
- 将处理安排在导致服务器的选择和通过的请求 OWIN 管道的构造工作流。

 目前，有 3 个主宿主选项基于 Katana 的应用程序：  
  
**IIS/ASP.NET**： 使用标准的 HttpModule 和 HttpHandler 类型，OWIN 管道可以在 IIS 上运行的 ASP.NET 请求流的一部分。 ASP.NET 承载支持被通过将 Microsoft.AspNet.Host.SystemWeb NuGet 包安装到 Web 应用程序项目。 此外，因为 IIS 可以充当主机和服务器，OWIN 服务器/主机区别交织在一起在此 NuGet 包中，这意味着如果使用 SystemWeb 主机，开发人员不能替换备用服务器实现。  
  
**自定义主机**: Katana 组件套件使开发人员能够对在她自己自定义过程中，主机应用程序无论是控制台应用程序、 Windows 服务，等等。此功能类似于 Web API 提供的自承载功能。 下面的示例演示自定义主机的 Web API 代码：  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

类似，Katana 应用程序的自承载安装程序：

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Web API 和 Katana 自承载的示例之间的一个显著区别是缺少 Katana 自承载示例中的 Web API 配置代码。 要使可移植性和可组合性，Katana 将从配置请求处理管道的代码中启动的服务器代码分离。 用于配置 Web API，然后启动，此外还作为 WebApplication.Start 中的类型参数指定的类中包含的代码。

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

将在本文的后面的更详细地讨论 startup 类。 但是，需要启动 Katana 自承载的进程在惊人类似于可能会使用当前在 ASP.NET Web API 自承载应用程序中的代码的代码。

**OwinHost.exe**： 一些将希望能够编写一个自定义进程用于运行应用程序的 Katana Web，许多想要只需启动预建的可执行文件，可以启动服务器并运行其应用程序。 对于此方案中，Katana 组件套件包括`OwinHost.exe`。 时从项目的根目录中运行，此可执行文件将启动的服务器 （它使用 HttpListener 服务器默认情况下），并使用约定来查找并运行用户的 startup 类。 对于更精细的控制，可执行文件提供了多个其他命令行参数。

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>服务器

 负责启动和维护的过程在其中应用程序运行时，服务器的责任主机时若要打开网络套接字，侦听请求，然后通过 OWIN 组件的管道发送它们由指定用户 （如你可能具有已注意到，在应用程序开发人员的 Startup 类中指定此管道）。 目前，Katana 项目包含两个服务器实现： 

- **Microsoft.Owin.Host.SystemWeb**： 如前所述，ASP.NET 管道配合 IIS 充当主机和服务器。 因此，当选择此宿主选项，IIS 同时管理主机级问题，如进程激活并侦听的 HTTP 请求。 对于 ASP.NET Web 应用程序，它然后将请求发送到 ASP.NET 管道。 Katana SystemWeb 主机注册的 ASP.NET HttpModule 和 HttpHandler 以截获请求，因为它们通过 HTTP 管道流并通过用户指定 OWIN 管道发送它们。
- **Microsoft.Owin.Host.HttpListener**： 此 Katana 服务器正如其名称，使用.NET Framework 的 HttpListener 类打开一个套接字并将请求发送到开发人员指定 OWIN 管道。 这是当前的 Katana 自承载 API 和 OwinHost.exe 的默认服务器选择。

## <a name="middlewareframework"></a>中间件/framework

 如前文所述，当服务器接受来自客户端的请求它负责通过指定由开发人员的启动代码的 OWIN 组件的管道。 这些管道组件被称为中间件。  
 在非常基本级别，OWIN 中间件组件只需要实现 OWIN 应用程序委托，以便它可调用。

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

但是，为了简化开发和中间件组件的撰写，Katana 中间件组件支持几种约定和帮助器类型。 最常见的这些是`OwinMiddleware`类。 使用此类生成的自定义的中间件组件将与以下类似： 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 此类派生自`OwinMiddleware`，实现作为其自变量之一接受管道中的下一步中间件实例，然后将其传递给基构造函数的构造函数。 用于配置该中间件的其他参数将在下一步的中间件参数的后面还声明为构造函数参数。   
  
在运行时，该中间件将执行通过重写`Invoke`方法。 此方法采用单个参数的类型`OwinContext`。 此上下文对象由`Microsoft.Owin`NuGet 包前面所述，并提供强类型对字典的访问请求、 响应和环境，以及几个帮助器类型。   
  
中间件类可以轻松地添加到 OWIN 管道中的应用程序启动代码，如下所示：   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

因为 Katana 基础结构只需生成的 OWIN 中间件组件管道，所以组件只是需要以支持要参与管道的应用程序委托的中间件组件范围可以是从简单的复杂性如 ASP.NET，Web API 的整个框架的记录器或[SignalR](../../../signalr/index.md)。 例如，将 ASP.NET Web API 添加到以前的 OWIN 管道，需要添加以下的启动代码：

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

Katana 基础结构将生成的中间件组件添加到配置方法中的 IAppBuilder 对象的顺序所基于的管道。 然后，在我们的示例，LoggerMiddleware 可处理流经管道，而不考虑如何最终处理这些请求的所有请求。 这可启用功能强大的中间件组件 （例如身份验证组件） 可以在其中处理包括多个组件和框架 （例如 ASP.NET Web API、 SignalR 和静态文件服务器） 的管道的请求的方案。
 
## <a name="applications"></a>应用程序

如前面的示例所示，OWIN 和 Katana 项目应不被认为是作为新的应用程序编程模型中，但而是作为一个抽象以便将应用程序的编程模型和从服务器和承载基础结构的框架中分离出来。 例如，在生成 Web API 应用程序时，开发人员 framework 将继续使用 ASP.NET Web API 框架，而不考虑使用 Katana 项目中的组件到 OWIN 管道中运行应用程序。 OWIN 相关的代码将在其中对应用程序开发人员可见的一个位置将为应用程序启动代码，其中开发人员编写 OWIN 管道。 在启动代码中，开发人员将注册一系列 UseXx 语句，通常是一个用于处理传入请求，将每个中间件组件。 这种体验将具有相同的效果与在当前的 System.Web 世界注册 HTTP 模块。 通常情况下，更大框架中间件，如 ASP.NET Web API 或[SignalR](../../../signalr/index.md)将注册在管道末尾。 横切中间件组件，例如，那些用于身份验证或缓存，通常会开始，从管道的注册，以便它们将处理的框架和更高版本在管道中注册的组件的所有请求。 这种分离的中间件组件从每个其他和底层基础结构组件使在不同的速度发展方式，同时确保整个系统保持不变的组件。

## <a name="components--nuget-packages"></a>组件 – NuGet 包

许多当前库和框架，如 Katana 项目组件作为一组 NuGet 包一起传递。 有关即将发布版本 2.0，Katana 包依赖项关系图将如下所示。 （单击图像可查看更大的图中）。

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Katana 项目中的几乎每个包取决于，直接或间接 Owin 包。 您可能还记得，这是包含 IAppBuilder 接口，从而提供 OWIN 规范的第 4 节中所述的应用程序启动顺序的具体实现的包。 此外，许多包依赖于 Microsoft.Owin，用于处理 HTTP 请求和响应中提供的一组帮助器类型。 包的其余部分可以归类为宿主基础结构包 （服务器或主机） 或中间件。 包和依赖项外部的 Katana 项目显示为橙色。

Katana 2.0 的宿主基础结构包括 SystemWeb 以及基于 HttpListener 的服务器，运行使用 OwinHost.exe 的 OWIN 应用程序的 OwinHost 包和 Microsoft.Owin.Hosting 程序包对于自承载 OWIN 应用程序中的自定义主机 （例如控制台应用程序、 Windows 服务等）

为 Katana 2.0 的中间件组件主要侧重于提供的身份验证的不同方法。 提供一个附加的中间件组件以诊断，启用对开始和错误页的支持。 随着 OWIN 增长到事实上宿主抽象，中间件组件，这两个那些由 Microsoft 和第三方开发的生态系统将还增长年数。

## <a name="conclusion"></a>结束语

 从其开始 Katana 项目的目标尚未创建和从而迫使开发人员了解另一个 Web 框架。 相反，目标已被创建为.NET Web 应用程序开发人员提供更多的选择不是以前曾出现可能的抽象。 来分割成一组可替换的组件的典型 Web 应用程序堆栈的逻辑层，Katana 项目使整个堆栈以提高在任何速率适合这些组件的组件。 通过构建简单的 OWIN 抽象周围的所有组件，Katana 使框架和基于它们的应用程序可以跨各种不同的服务器和主机移植。 通过将开发人员放入控件的堆栈，Katana 可确保开发人员进行有关如何轻量的最终选择或其 Web 堆栈应为如何功能丰富。  
  

## <a name="for-more-information-about-katana"></a>详细了解 Katana

- GitHub 上的 Katana 项目： [https://github.com/aspnet/AspNetKatana/](https://github.com/aspnet/AspNetKatana/)。
- 视频： [Katana 项目-ASP.NET 的 OWIN](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET)，通过 Howard Dierking。

## <a name="acknowledgements"></a>致谢

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick 是 Microsoft 将重点放在 Azure 和 MVC 的高级编程编写器。
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
