---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: "分布式缓存 （使用 Azure 构建真实世界云应用） |Microsoft 文档"
author: MikeWasson
description: "构建真实世界云应用程序与 Azure 的电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2015
ms.topic: article
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 24ede9cb9289c84140f6e2573f9d526f19cac64b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>分布式缓存 （构建真实世界云应用程序与 Azure）
====================
通过[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下载修复此错误项目](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下载电子书](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **构建真实世界云应用程序与 Azure**电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，从而帮助你为成功开发适用于云中的 web 应用。 有关电子书的信息，请参阅[第一章](introduction.md)。


前一章讨论过暂时性故障处理和提及缓存作为断路器策略。 本章提供更多背景信息有关缓存，包括何时使用它，使用它的常见模式以及如何在 Azure 中实现它。

## <a name="what-is-distributed-caching"></a>什么被分布式缓存

缓存提供高吞吐量、 低延迟访问通常访问的应用程序数据，通过将数据存储在内存中。 对于云应用程序的最有用的一种缓存是分布式的缓存，这意味着在单独的 web 服务器的内存而是在其他云资源，不会存储数据，缓存的数据可供所有应用程序的 web 服务器 （或其他云 Vm 该 are 由应用程序的)。

![显示访问相同的缓存服务器的多个 web 服务器的图示](distributed-caching/_static/image1.png)

当应用程序扩展通过添加或删除服务器，或者服务器替换由于升级或故障，缓存的数据仍然可供运行该应用程序的每个服务器访问。

通过避免永久性数据存储的高延迟数据访问权限，缓存可以显著提高应用程序响应能力。 例如，从缓存中检索数据是比从关系数据库检索快得多。

缓存的一个连带好处减少为永久性数据存储到永久性数据存储，这有数据流出量时，可能会导致降低成本的流量收费。

## <a name="when-to-use-distributed-caching"></a>何时使用分布式缓存

最适合应用程序工作负荷执行多个读取比写入数据，以及当数据模型支持用于存储和检索缓存中的数据的键/值组织的缓存工作方式。 它也是更有用时应用程序用户共享大量的常见数据;例如，缓存就无法提供尽可能多的好处，如果每个用户通常检索该用户所特有的数据。 缓存可能会非常有用的一个示例是产品目录，因为数据不经常，更改和所有客户正在都寻求通过相同的数据。

缓存的好处将成为越来越多地显著提高应用程序的比例越多，随着的吞吐量限制和延迟延迟的持久数据存储多个应用程序总体性能方面的限制。 但是，您可能会实现缓存性能也比其他原因。 对于不一定是完全最新时向用户显示的数据，缓存访问可以充当断路器为永久性数据存储时停止响应或不可用。

## <a name="popular-cache-population-strategies"></a>常用缓存填充策略

为了能够从缓存中检索数据，必须将其第一次存储存在。 有几种策略到缓存中获取所需的数据：

- 按需 / 缓存端

    应用程序尝试从缓存检索数据并时缓存不包含数据 ("miss")，应用程序存储数据在缓存中，以便它将提供的下一步的时间。 在尝试获取相同的数据，应用程序的下一步时它找到它寻找的内容在缓存中 （"命中"）。 若要防止对数据库中提取已更改的缓存的数据，使在向数据存储区做出更改时失效缓存。
- 后台数据推送

    后台服务将数据推送到缓存定期计划，和应用程序始终将从缓存。 高延迟数据源，不需要您始终使用此方法适用出色返回最新数据。
- 断路器

    应用程序通常将直接与通信永久性数据存储，但应用程序时永久性数据存储可用性问题，从缓存检索数据。 可能会使用保留缓存或后台数据推送策略的缓存中使数据。 这是错误处理策略，而不是性能增强策略。

为了使缓存中的数据当前，你可以删除相关的缓存条目，你的应用程序更新，创建或删除数据。 如果它为你的应用程序有时会得到略有过期的数据是这样，你可以依赖于可配置的过期时间，以在多长缓存数据可以是上设置的限制。

你可以配置绝对过期 （的缓存项创建以来的时间长度） 或滑动过期 （自上次访问缓存项后的时间量）。 您有很大的缓存过期机制，以防止数据变得太陈旧上时，将使用绝对过期。 在修复它应用中，我们将手动将过期的缓存项和我们将使用滑动过期在缓存中保留最新数据。 无论你选择的过期策略，缓存将自动逐出最旧的 （最近最少使用或 LRU） 项目，当达到缓存的内存限制。

## <a name="sample-cache-aside-code-for-fix-it-app"></a>修复它的应用的示例缓存端代码

在下面的示例代码中，我们在缓存中检查第一次检索修复它任务时。 如果在缓存中找到该任务，我们将返回它;如果未找到，我们从数据库获取并将其存储在缓存中。 所做的更改将添加到缓存`FindTaskByIdAsync`方法将突出显示。

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

当更新或删除修复它任务时，你必须使无效 （删除） 的已缓存任务。 否则，将来尝试读取该任务将继续从缓存中获取旧的数据。

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

这些是示例来演示简单的缓存代码;缓存不实现了可下载修复它在项目中。

## <a name="azure-caching-services"></a>Azure 缓存服务

Azure 提供以下缓存服务： [Azure Redis 缓存](https://msdn.microsoft.com/library/dn690523.aspx)和[Azure 托管缓存](https://msdn.microsoft.com/library/dn386094.aspx)。 Azure Redis 缓存基于流行[开放源代码 Redis 缓存](http://redis.io/)和大多数的第一个选择缓存方案。

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>ASP.NET 会话状态使用缓存提供程序

中所述[web 开发最佳做法章](web-development-best-practices.md)，最佳做法是避免使用会话状态。 如果你的应用程序需要会话状态下, 一步的最佳做法是避免默认内存中提供程序，因为不支持横向扩展 （web 服务器的多个实例）。 ASP.NET SQL Server 会话状态提供程序启用多个 web 服务器以使用会话状态, 运行的站点，但它会导致相比于内存中提供程序的高延迟成本。 如果你需要使用会话状态的最佳解决方案是使用缓存提供程序，如[用于 Azure Cache 的会话状态提供程序](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx)。

## <a name="summary"></a>摘要

你已了解如何解决它应用可以实现缓存为了改进响应时间和可伸缩性，并使应用程序以继续数据库不可用时能够快地响应进行读取操作。 在[下一章](queue-centric-work-pattern.md)我们将演示如何以进一步提高可缩放性和使应用程序继续能够快地响应进行写入操作。

## <a name="resources"></a>资源

有关缓存的详细信息，请参阅以下资源。

文档

- [Azure 缓存](https://msdn.microsoft.com/library/gg278356.aspx)。 在 Azure 中的缓存上的正式 MSDN 文档。
- [Microsoft 模式和实践-Azure 指南](https://msdn.microsoft.com/library/dn568099.aspx)。 请参阅缓存指南和缓存端模式。
- [防故障： 弹性云体系结构指南](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx)。 通过 Marc Mercuri、 Ulrich Homann 和 Andrew Townhill 白皮书。 有关缓存，请参阅部分。
- [在 Azure 云服务上的大规模服务的设计的最佳实践](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)。 W. Mark Simms 和 Michael Thomassy 白皮书。 在分布式缓存，请参阅部分。
- [分布式缓存可伸缩性的路径](https://msdn.microsoft.com/magazine/dd942840.aspx)。 较旧的 （2009 年） MSDN 杂志文章，但通常情况下; 分布式缓存的明确书面的介绍进入比防故障和最佳做法白皮书的缓存部分的更深入。

视频

- [防故障： 构建可扩展、 有弹性的云服务](https://channel9.msdn.com/Series/FailSafe)。 包含 9 个部分组成的系列 Ulrich Homann、 Marc Mercuri 和 Mark Simms。 提供如何构建云应用程序的 400 级视图。 这一系列主要关注理论和原因;有关更多操作指南的详细信息，请参阅 Mark Simms 构建大型系列。 请参阅缓存中段 3 开始 1:24:14 讨论。
- [构建大： 从 Azure 客户的第 I 部分到的经验教训](https://channel9.msdn.com/Events/Build/2012/3-029)。人 Simon Davies 讨论 46:00 处的分布式缓存开始。 类似于防故障系列但进入更多操作指南的详细信息。 演示文稿提供 2012 年 10 月 31 日，因此它不涵盖在 2013年引入了的 Azure App Service Web apps 的缓存服务。

代码示例

- [云服务在 Azure 中的基础知识](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)。 实现分布式缓存的示例应用程序。 请参阅随附的博客文章[云服务基础知识 – 缓存的基础知识](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx)。

>[!div class="step-by-step"]
[上一页](transient-fault-handling.md)
[下一页](queue-centric-work-pattern.md)
