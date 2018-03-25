---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: "Web 开发最佳做法 （使用 Azure 构建真实世界云应用） |Microsoft 文档"
author: MikeWasson
description: "构建真实世界云应用程序与 Azure 的电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: de536a0ca39cb752c0962f0c4ae36eb00b586bff
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Web 开发最佳做法 （使用 Azure 构建真实世界云应用）
====================
通过[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下载修复此错误项目](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下载电子书](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **构建真实世界云应用程序与 Azure**电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，从而帮助你为成功开发适用于云中的 web 应用。 有关电子书的信息，请参阅[第一章](introduction.md)。


有关设置敏捷开发过程; 已的前三个模式rest 是有关体系结构和代码。 它是 web 开发最佳做法的集合：

- [无状态 web 服务器](#stateless)智能负载平衡器后面。
- [避免会话状态](#sessionstate)（或如果你不能避免数据丢失，使用分布式的缓存，而不是数据库）。
- [使用 CDN](#cdn)边缘缓存静态文件资产 （图像、 脚本）。
- [使用.NET 4.5 的异步支持](#async)以避免阻塞调用。

这些实践适用于所有的 web 开发，而不仅仅用于云应用程序，但它们的云应用程序尤为重要。 它们协同工作来帮助你更好地利用的高度灵活的缩放提供的云环境。 如果您不遵循这些做法，你将会遇到限制中，当你尝试将应用程序缩放时。

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>智能负载平衡器后的无状态 web 层

*无状态 web 层*意味着你不要将任何应用程序数据存储在 web 服务器的内存或文件系统中。 保持您的 web 层无状态，可同时提供更好的客户体验并节省资金：

- 如果 web 层是无状态，并且它位于负载平衡器后，你可以快速响应的应用程序流量中的更改，通过动态添加或删除服务器。 在其中你只需为付费的服务器资源，只要你实际使用云环境中，作出响应的需求变化该功能可以转换大幅节省。
- 无状态 web 层是体系结构上要简单得多横向扩展应用程序。 它太使你必须响应更快地缩放需求并花费少的钱大量上开发和测试过程中。
- 云服务器，如本地服务器，需要修补和偶尔; 重新启动并且，如果无状态 web 层，当服务器暂时出现故障时重新传送流量不会导致错误或意外的行为。

大多数实际应用程序需要 web 会话; 存储状态此处的主要位置不是将其存储在 web 服务器上。 在 cookie 或进程服务器端中使用缓存提供程序的 ASP.NET 会话状态外，客户端上，可以如存储以其他方式的状态。 你可以存储在文件[Windows Azure Blob 存储](unstructured-blob-storage.md)而不是本地文件系统。

作为示例，了解要缩放的应用程序在 Windows Azure 网站中，如果您的 web 层是无状态是多么容易，请参阅**缩放**选项卡上为 Windows Azure 网站管理门户中：

![缩放选项卡](web-development-best-practices/_static/image1.png)

如果你想要添加 web 服务器，可以只需将实例计数滑块拖动右侧。 设置为 5，然后单击**保存**，并且在秒内具备 5 的 web 服务器在处理你的网站的流量的 Windows Azure 中。

![五个实例](web-development-best-practices/_static/image2.png)

你可以轻松地设置下 3 或反馈至 1 的实例计数。 当你缩放后时，你启动节约资金立即因为 Windows Azure 的费用按分钟，也不是按小时。

你还可以指示 Windows Azure 自动增加或减少的基于 CPU 使用率的 web 服务器的数量。 在下面的示例中，当 CPU 使用率低于 60%，web 服务器的数量将减少至最少为 2，而且，如果 CPU 使用率高于 80%时，web 服务器的数目将增加最大为 4。

![按 CPU 使用率缩放](web-development-best-practices/_static/image3.png)

或者，如果你知道，你的站点将只是繁忙工作时间内？ 您可以让 Windows Azure 以多个服务器在白天运行，并减少至 evenings、 晚上，一台服务器和周末。 以下一系列的屏幕截图显示如何将 web 站点设置为在工作时间从上午 8 点到下午 5 点过程中运行一台服务器在下班时间和 4 台服务器。

![按计划缩放](web-development-best-practices/_static/image4.png)

![设置计划时间](web-development-best-practices/_static/image5.png)

![白天计划](web-development-best-practices/_static/image6.png)

![Weeknight 计划](web-development-best-practices/_static/image7.png)

![周末计划](web-development-best-practices/_static/image8.png)

和当然上述所有操作均可以按如下所示门户以及脚本。

处理程序，但前提是避免了对动态通过添加或删除服务器 Vm 保持无状态 web 层，应用程序，以向外扩展的能力是在 Windows Azure 中，几乎不限。

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>避免会话状态

它通常是不现实在真实世界云应用程序中以避免存储某种形式的状态，以便用户会话，但某些方法影响性能和可伸缩性比其他更多。 如果你需要存储状态，最佳解决方案是使状态量保持较小，并将其存储在 cookie 中。 如果这不可行下, 一步的最佳解决方案是将 ASP.NET 会话状态提供程序用于[分布式内存中缓存](distributed-caching.md#sessionstate)。 从性能和可伸缩性的角度来看最差的解决方案是使用数据库支持的会话状态提供程序。

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>使用 CDN 来缓存静态文件资产

CDN 是内容交付网络的首字母缩写。 向 CDN 提供商，提供静态文件的资产，例如映像和脚本文件，并提供程序缓存世界各地的数据中心内这些文件，以便用户访问你的应用程序，只要它们获取相对快速响应和低延迟的已缓存资产。 这加快站点的总体负载时间并减少你的 web 服务器上的负载。 Cdn 是如果到达的受众的广泛地理位置分布尤其重要。

Windows Azure CDN，并且你可以在 Windows Azure 或任何 web 托管环境中运行的应用程序中使用其他 Cdn。

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>使用.NET 4.5 的异步支持以避免阻塞调用

.NET 4.5 增强以使其以异步方式处理任务要简单得多的 C# 和 VB 的编程语言。 异步编程的好处不再仅仅用于并行处理的情况下，例如当你想要同时启动多个 web 服务调用。 它还使你的 web 服务器来更有效地执行和在高负载情况下可靠。 Web 服务器仅有有限的数量的线程可用，并且在高负载情况下当所有线程均已在使用中，传入的请求必须等待，直到线程会被释放。 如果你的应用程序代码不处理任务，如数据库查询和 web 服务调用以异步方式，多个线程不必要地占用了服务器等待 I/O 响应时。 此限制的服务器能够在高负载情况下处理的流量。 使用异步编程时，线程中正在等待的 web 服务或数据库返回数据最新请求提供服务之前释放数据接收。 在繁忙的 web 服务器上，数百或数千个请求可以然后处理立即其中否则将等待线程释放。

如您前面看到的它是一样简单，若要减少的 web 服务器处理您的网站，因为它是以增加数量。 因此如果一台服务器可以实现更大的吞吐量，则不需要为许多，你可以降低成本，因为你需要为给定的流量较少服务器否则会比。

支持.NET 4.5 的异步编程模型包含在 ASP.NET 4.5 Web 窗体、 MVC，和 Web API;在实体框架 6，和[Windows Azure 存储 API](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx)。

### <a name="async-support-in-aspnet-45"></a>ASP.NET 4.5 中的异步支持

在 ASP.NET 4.5，支持具有已添加异步编程，而不仅仅是对语言但 MVC、 Web 窗体和 Web API 框架。 例如，ASP.NET MVC 控制器操作方法接收 web 请求的数据，并将数据传递到然后创建 HTML 中以发送到浏览器的视图。 频繁的操作方法需要从数据库或 web 服务获取数据，为了在网页中显示它，或将保存 web 页中输入的数据。 在这些方案中很容易地将为异步，操作方法： 而不是返回*ActionResult*对象，则返回*任务&lt;ActionResult&gt;* 和标记的方法与*异步*关键字。 在方法中，当某个代码行启动时，涉及等待时间的操作时你将其标记使用 await 关键字。

下面是为数据库查询中调用的存储库方法相当简单的操作方法：

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

而以下是以异步方式处理数据库调用的相同方法：

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

在内部编译器将生成相应的异步代码。 当应用程序发出调用`FindTaskByIdAsync`，ASP.NET 使`FindTask`请求，展开辅助线程，然后使它可处理其他请求。 当`FindTask`操作的请求，线程重新启动才能继续处理后该调用的代码。 在此期间后`FindTask`启动请求和时返回的数据，必须可用于执行有用的工作，否则为这将占用等待响应的线程。

对于异步代码，一些系统开销，但在低负载情况下该开销不计，同时在你能够处理等候可用线程都将保存的请求的高负载情况下。

已可以进行异步编程，因为 ASP.NET 1.1 版中，这种类型，但它却难以编写、 易出错且难以调试。 现在，我们还简化了中的编码为其 ASP.NET 4.5，则没有理由这样做了。

### <a name="async-support-in-entity-framework-6"></a>Entity Framework 6 中的异步支持

4.5 中的异步支持的一部分我们提供异步支持 web 服务调用、 套接字和文件系统 I/O，但 web 应用程序的最常见模式是命中数据库和我们的数据库不支持异步。 现在 Entity Framework 6 增加了对数据库访问权限的异步支持。

Entity Framework 6 中会导致查询或命令以发送到数据库的所有方法都有异步版本。 此处的示例显示的异步版本*查找*方法。

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

与此异步支持适用于插入、 删除、 更新和网络发现查找简单而不仅仅是，则它还适用于 LINQ 查询：

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

没有`Async`版本`ToList`方法，因为它在此代码是，则查询发送到数据库的方法。 `Where`和`OrderByDescending`方法只能配置查询，而`ToListAsync`方法执行查询，并将存储中的响应`result`变量。

## <a name="summary"></a>摘要

你可以实现在任何 web 编程框架和任何云环境，此处所述的 web 开发最佳做法，但我们无在方便的 ASP.NET 和 Windows Azure 中的工具。 如果你按照这些模式，可以轻松地横向扩展您的 web 层，并将尽量减少您的费用，因为每个服务器将能够处理更多流量。

[下一章](single-sign-on.md)考察云中如何启用单一登录方案。

## <a name="resources"></a>资源

有关详细信息请参阅以下资源。

无状态 web 服务器：

- [Microsoft 模式和实践-自动缩放指南](https://msdn.microsoft.com/library/dn589774.aspx)。
- [禁用 ARR 实例 Windows Azure 网站中的相关性](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/)。 通过 Erez Benari 的博客文章介绍了会话相关性在 Windows Azure 网站。

CDN:

- [防故障： 构建可扩展、 有弹性的云服务](https://channel9.msdn.com/Series/FailSafe)。 通过 Ulrich Homann、 Marc Mercuri 和 Mark Simms 九一部分视频系列。 请参阅中段 3 从 1:34:00 开始的 CDN 讨论。
- [Microsoft 模式和实践静态内容托管模式](https://msdn.microsoft.com/library/dn589776.aspx)
- [CDN 评审](http://www.cdnreviews.com/)。 许多 Cdn 的概述。

异步编程：

- [在 ASP.NET MVC 4 中使用异步方法](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)。 由 Rick Anderson 的教程。
- [异步编程使用 Async 和 Await （C# 和 Visual Basic）](https://msdn.microsoft.com/library/vstudio/hh191443.aspx)。 介绍用于异步编程的基本原理、 ASP.NET 4.5 中的工作方式以及如何编写代码来实现 MSDN 白皮书。
- [实体框架异步查询和保存](https://msdn.microsoft.com/data/jj819165)
- [如何生成 ASP.NET Web 应用程序使用 Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7)。 通过 Rowan Miller 的视频演示。 包括图形演示还可以帮助大大提高 web 服务器的吞吐量在高负载情况下如何异步编程。
- [防故障： 构建可扩展、 有弹性的云服务](https://channel9.msdn.com/Series/FailSafe)。 通过 Ulrich Homann、 Marc Mercuri 和 Mark Simms 九一部分视频系列。 供大家讨论有关影响的可伸缩性的异步编程，请参阅段 4 和段 8。
- [使用 ASP.NET 4.5 以及重要隐患的异步方法的幻](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx)。 主要有关在 ASP.NET Web 窗体应用程序中使用异步 Scott Hanselman 的博客文章。

其他 web 开发最佳做法，请参阅以下资源：

- [修复它示例应用程序的最佳实践](the-fix-it-sample-application.md#bestpractices)。 此电子书的附录部分列出了一些在修复该应用程序中实现实现的最佳做法。
- [Web 开发人员清单](http://webdevchecklist.com/asp.net)

>[!div class="step-by-step"]
[上一页](continuous-integration-and-continuous-delivery.md)
[下一页](single-sign-on.md)
