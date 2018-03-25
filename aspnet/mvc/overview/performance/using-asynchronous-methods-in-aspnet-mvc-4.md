---
uid: mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
title: "在 ASP.NET MVC 4 中使用异步方法 |Microsoft 文档"
author: Rick-Anderson
description: "本教程将教您生成使用 Visual Studio Express 2012 for Web，这是免费遇到异步 ASP.NET MVC Web 应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/06/2012
ms.topic: article
ms.assetid: a56572ba-81c3-47af-826d-941e9c4775ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: b4280c6ab1b6d8d2ceaa7cef14fce94ab8c6df53
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="using-asynchronous-methods-in-aspnet-mvc-4"></a>在 ASP.NET MVC 4 中使用异步方法
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> 本教程将教您生成异步 ASP.NET MVC Web 应用程序中使用的基础知识[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11)，这是 Microsoft Visual Studio 的免费版。 你还可以使用[Visual Studio 2012](https://www.microsoft.com/visualstudio/11)。

> 本教程中在 github 上提供的完整示例[https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/)


ASP.NET MVC 4[控制器](https://msdn.microsoft.com/library/system.web.mvc.controller(VS.108).aspx)中组合类[.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) ，你可以编写的异步操作方法的返回类型的对象[任务&lt;ActionResult&gt;](https://msdn.microsoft.com/library/dd321424(VS.110).aspx). .NET Framework 4 引入了称为异步编程概念[任务](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)和 ASP.NET MVC 4 支持[任务](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)。 任务表示通过**任务**类型和相关的类型中[System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx)命名空间。 .NET Framework 4.5 基于此异步支持[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)和[异步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)使用的关键字[任务](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)比以前更复杂的对象异步方法。 [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)关键字是用于指示语法的一段代码应以异步方式等待代码的其他部分的速记。 [异步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)关键字都表示一个提示，可用于将方法标记为基于任务的异步方法。 组合**await**，**异步**，和**任务**对象使可以你在.NET 4.5 中编写异步代码更加容易。 调用异步方法的新模型*基于任务的异步模式*(**点击**)。 本教程假定你已具备一定的异步编程使用[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)和[异步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)关键字和[任务](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)命名空间。

有关详细信息使用[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)和[异步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)关键字和[任务](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)命名空间，请参阅以下引用。

- [.NET 中的白皮书： 异步](https://go.microsoft.com/fwlink/?LinkId=204844)
- [异步/等待常见问题](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Visual Studio 异步编程](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>线程池处理请求的方式

在 web 服务器上，.NET Framework 维护一个用于 ASP.NET 请求提供服务的线程池。 当请求到达时，会调度池中的线程来处理该请求。 如果同步处理请求时，处理请求的线程将是忙时请求正在处理，而且线程无法服务另一个请求。   
  
这可能不是问题，因为线程池可以设置得足够大以容纳许多的忙线程。 但是，在线程池中的线程数会受到限制 （最大长度为.NET 4.5 默认值为 5000）。 在大型应用程序与高并发的长时间运行的请求，所有可用线程可能太忙。 这种情况称为线程资源不足。 当达到这种情况时，web 服务器将请求排队。 如果请求队列已满，web 服务器将拒绝与 （服务器太忙） 的 HTTP 503 状态的请求。 CLR 线程池将限制对新线程注入。 如果并发性突发 （即，您的网站突然可以获取大量的请求） 和所有可用请求线程忙由于后端调用，高延迟，则有限的线程注入速率可以使应用程序非常差响应。 此外，添加到线程池每个新线程的开销 （如堆栈内存的 1 MB)。 Web 应用程序使用的服务高延迟调用同步方法，线程池可增长到.NET 4.5 默认最大为 5，000 线程将占用大约 5 GB 的内存量大于应用程序能够服务相同，使用请求。异步方法，并仅 50 线程。 在执行异步工作时，你不始终使用一个线程。 例如，在进行异步 web 服务请求时，ASP.NET 将不使用任何线程之间**异步**方法调用和**await**。 为请求提供服务的线程池使用高延迟可能会导致大型内存占用量和的服务器硬件的利用率不佳。

## <a name="processing-asynchronous-requests"></a>处理异步请求

在 web 应用程序看到大量的启动时的并发请求或具有突发负载 （其中并发可增加突然），使这些 web 服务调用异步将增加你的应用程序的响应能力。 异步请求花费的时间来处理为同步的请求相同的量。 例如，如果请求将发出 web 服务调用需要两秒钟来完成，请求采用两个秒是否同步或异步执行。 但是，在过程的异步调用线程不受阻止等待完成的第一个请求时响应其他请求。 因此，异步请求阻止请求排队和线程池增长时有很多并发请求，以调用长时间运行的操作。

## <a id="ChoosingSyncVasync"></a>选择同步或异步操作方法

本部分列出准则来确定何时使用同步或异步操作的方法。 这些是只是指南;检查每个应用程序单独以确定是否异步方法帮助提高性能。

一般情况下，在以下情况使用同步方法：

- 操作很简单或短时间运行。
- 保持简单很比效率更重要。
- 操作是主要 CPU 操作而不是涉及大量的磁盘或网络开销的操作。 使用上 CPU 绑定的操作的异步操作方法不提供任何好处，并且导致更多的开销。

 一般情况下，使用异步方法，在以下情况：

- 正在呼叫通过异步方法，可以使用的服务和你使用.NET 4.5 或更高版本。
- 操作是网络绑定或我/O 绑定而不是 CPU 绑定。
- 并行度是代码的比简单更重要。
- 你想要提供一种机制，允许取消长时间运行请求的用户。
- 当切换出的线程的好处权重上下文切换的成本。 一般情况下，你应方法异步在不执行任何工作时在 ASP.NET 请求线程上等待同步方法。 通过进行以下调用异步，ASP.NET 请求线程已不停止等待完成的 web 服务请求时任何工作。
- 测试显示阻止操作的性能瓶颈的站点，并且 IIS，可以通过使用异步方法对这些阻塞调用服务更多的请求。

 可下载的示例演示如何有效地使用异步操作方法。 提供的示例旨在提供简单的 ASP.NET MVC 4 使用.NET 4.5 中的异步编程的演示。 该示例不是为 ASP.NET MVC 中的异步编程参考体系结构。 在示例程序调用[ASP.NET Web API](../../../web-api/index.md)反过来调用的方法[Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx)以模拟长时间运行 web 服务调用。 大多数生产应用程序将不会显示此类到使用异步操作方法的明显优势。   
  
个别应用程序需要所有操作方法都是异步的。 通常情况下，将几个同步的操作方法转换为异步方法提供所需的工作量会显著增加。

## <a id="SampleApp"></a>示例应用程序

你可以下载示例应用程序从[https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET)上[GitHub](https://github.com/)站点。 存储库中由三个项目组成：

- *Mvc4Async*: ASP.NET MVC 4 项目包含本教程中使用的代码。 Web API 调用**WebAPIpgw**服务。
- *WebAPIpgw*： 实现的 ASP.NET MVC 4 Web API 项目`Products, Gizmos and Widgets`控制器。 它提供的数据*WebAppAsync*项目和*Mvc4Async*项目。
- *WebAppAsync*： 另一个教程中使用的 ASP.NET Web 窗体项目。

## <a id="GizmosSynch"></a>Gizmos 同步操作方法

 下面的代码演示`Gizmos`同步操作方法，用于显示 gizmos 的列表。 （对于本文中，零件为虚构的机械设备。） 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample1.cs)]

下面的代码演示`GetGizmos`零件服务的方法。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample2.cs)]

`GizmoService GetGizmos`方法将 URI 传递给 ASP.NET Web API HTTP 服务返回 gizmos 数据的列表。 *WebAPIpgw*项目包含的 Web API 实现`gizmos, widget`和`product`控制器。  
下图显示将 gizmos 视图从示例项目。

![Gizmos](using-asynchronous-methods-in-aspnet-mvc-4/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>创建异步 Gizmos 操作方法

此示例使用新[异步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)和[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)关键字 （在.NET 4.5 和 Visual Studio 2012 中可用） 让编译器可以负责维护所需的复杂的转换异步编程。 编译器允许你编写代码使用 C# 的同步的控制流构造，编译器会自动应用需使用回调，以避免阻止线程的转换。

下面的代码演示`Gizmos`同步方法和`GizmosAsync`异步方法。 如果你的浏览器支持[HTML 5`<mark>`元素](http://www.w3.org/wiki/HTML/Elements/mark)，你将看到中的更改`GizmosAsync`黄色突出显示框中。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample3.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample4.cs?highlight=1,3,5)]

 已在应用以下更改以允许`GizmosAsync`异步高。

- 该方法将标有[异步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)关键字，它告诉编译器生成的正文部分的回调并自动创建`Task<ActionResult>`返回。
- &quot;异步&quot;附加到的方法名称。 追加"Async"不是必需的但在编写异步方法时很约定。
- 返回类型已从更改`ActionResult`到`Task<ActionResult>`。 返回类型`Task<ActionResult>`表示正在进行的工作，并提供与通过其来等待异步操作完成的句柄的方法的调用方。 在这种情况下，调用方是 web 服务。 `Task<ActionResult>`表示正在进行的结果一起工作`ActionResult.`
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)关键字应用于 web 服务调用。
- 调用异步的 web 服务 API 时 (`GetGizmosAsync`)。

内`GetGizmosAsync`方法正文另一种异步方法，`GetGizmosAsync`调用。 `GetGizmosAsync`立即返回`Task<List<Gizmo>>`有可用的数据时，最终将完成。 你不想将零件数据之前执行任何其他操作，因为代码等待任务 (使用**await**关键字)。 你可以使用**await**仅在使用批注的方法中的关键字**异步**关键字。

**Await**关键字不会阻止线程，直到任务完成。 它注册为该任务，在回调方法的其余部分，并立即返回。 在最终完成等待的任务，它将调用该回调并因此恢复中断的位置的方法权限执行。 有关详细信息使用[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)和[异步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)关键字和[任务](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)命名空间，请参阅[异步引用](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async)。

下面的代码演示`GetGizmos`和`GetGizmosAsync`方法。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample5.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample6.cs?highlight=1,4-8)]

 异步更改是对所做的内容相似**GizmosAsync**上面。 

- 使用已批注的方法签名[异步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)关键字，则返回类型已更改为`Task<List<Gizmo>>`，和*异步*附加到的方法名称。
- 异步[HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)而不是使用类[WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx)类。
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)关键字应用于[HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)异步方法。

下图显示了异步零件视图。

![async](using-asynchronous-methods-in-aspnet-mvc-4/_static/image2.png)

浏览器表示法的 gizmos 数据等同于在同步调用创建的视图。 唯一的区别是异步版本可能是在高负载情况下更高的性能。

## <a id="Parallel"></a>并行执行多个操作

异步操作方法在某项操作必须执行几个独立的操作时有一个明显的优势，转移同步方法。 在此示例中提供，同步方法`PWG`（对于产品、 小组件和 Gizmos） 显示三个 web 服务调用，若要获取的产品、 小组件和 gizmos 列表的结果。 [ASP.NET Web API](../../../web-api/index.md)提供这些服务的项目服务使用[Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx)模拟延迟或慢速网络调用。 当延迟设置为 500 毫秒，异步`PWGasync`方法采用稍有超过 500 毫秒完成时同步`PWG`版本将于 1500 毫秒。 同步`PWG`方法显示在下面的代码。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample7.cs)]

异步`PWGasync`方法显示在下面的代码。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample8.cs?highlight=1,3,12)]

下图显示了从返回的视图**PWGasync**方法。

![pwgAsync](using-asynchronous-methods-in-aspnet-mvc-4/_static/image3.png)

## <a id="CancelToken"></a>使用取消标记

异步操作方法会返回`Task<ActionResult>`是否可取消，即它们将采用[CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx)参数时使用提供的一个[AsyncTimeout](https://msdn.microsoft.com/library/system.web.mvc.asynctimeoutattribute(VS.108).aspx)属性。 下面的代码演示`GizmosCancelAsync`具有的 150 毫秒的超时值的方法。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample9.cs?highlight=1-3,5,10)]

下面的代码演示 GetGizmosAsync 重载，它采用[CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx)参数。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample10.cs)]

在提供示例应用程序的情况下，选择*取消令牌演示*链接调用`GizmosCancelAsync`方法，并演示异步调用的取消。

## <a id="ServerConfig"></a>对高的并发高延迟 Web 服务调用的服务器配置

若要实现异步 web 应用程序的优点，你可能需要进行一些更改为默认服务器配置。 请注意配置时和压力测试异步 web 应用程序记住以下。

- Windows 7、 Windows Vista 和所有 Windows 客户端操作系统具有最多 10 个并发请求。 你将需要 Windows 服务器操作系统以查看在高负荷的异步方法的好处。
- 向 IIS 注册.NET 4.5，从提升的命令提示符：  
 %windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet\_regiis -i  
 请参阅[ASP.NET IIS 注册工具 (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- 你可能需要增加[HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture)队列限制从 1000 到 5000 的默认值。 如果设置得太低，可能会看到[HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture)拒绝的 HTTP 503 状态的请求。 若要更改 HTTP.sys 队列限制：

    - 打开 IIS 管理器并导航到应用程序池窗格中。
    - 目标应用程序池上右键单击并选择**高级设置**。  
        ![advanced](using-asynchronous-methods-in-aspnet-mvc-4/_static/image4.png)
    - 在**高级设置**对话框中，更改*队列长度*为 5000 数为 1000。  
        ![队列长度](using-asynchronous-methods-in-aspnet-mvc-4/_static/image5.png)  
  
 请注意，在更高版本，映像的.NET framework 列为 v4.0，即使应用程序池使用.NET 4.5。 若要了解这种差异，请参阅以下：

    - [.NET 版本控制和多目标的.NET 4.5 是就地升级到.NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
    - [如何设置 IIS 应用程序或应用程序池使用 ASP.NET 3.5 而不是 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
    - [.NET framework 版本和依赖关系](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- 如果你的应用程序正在使用 web 服务或 System.NET 为使用后端通过 HTTP 通信你可能需要增加[connectionManagement/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx)元素。 对于 ASP.NET 应用程序，这受限制的自动配置功能的 Cpu 数的 12 倍。 这意味着，在四核处理器，你可以拥有最多 12 \* 4 = 48 到 IP 终结点的并发连接。 因为这绑定到[autoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx)，最简单的方法，以提高`maxconnection`在 ASP.NET 应用程序是设置[System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx)以编程方式在从`Application_Start`中的方法*global.asax*文件。 请参阅有关示例下载示例。
- 在.NET 4.5 中，默认值为 5000 [maxconcurrentrequestspercpu 配置](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)应保留。
