---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: "暂时性故障处理 （构建使用 Azure 真实世界云应用） |Microsoft 文档"
author: MikeWasson
description: "构建真实世界云应用程序与 Azure 的电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/03/2015
ms.topic: article
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: b743b04789c5e5ebf5ab922cf34a516a16a6d356
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>暂时性故障处理 （构建真实世界云应用与 Azure）
====================
通过[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下载修复此错误项目](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下载电子书](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **构建真实世界云应用程序与 Azure**电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，从而帮助你为成功开发适用于云中的 web 应用。 有关电子书的信息，请参阅[第一章](introduction.md)。


在设计真实世界云应用程序时，你需要考虑的事情之一是如何处理临时服务中断。 此问题是云应用程序的唯一重要，因为你因此依赖于网络连接和外部服务。 您可以频繁地获取自我通常修复的小难题，如果你不准备好以智能方式处理它们，它们将导致错误体验为您的客户。

## <a name="causes-of-transient-failures"></a>暂时性故障的原因

在云环境中你将找到的失败并已丢弃连接会定期发生的数据库。 这是部分，因为你将通过相比的本地环境的多个负载平衡器，你的 web 服务器和数据库服务器具有直接物理连接。 此外，有时，当你依赖于多租户服务时你将看到调用服务 get 速度较慢或超时，因为其他人使用该服务的人员很大程度达到它。 在其他情况下，你可能会过于频繁，达到该服务的用户和服务有意限制你 – 拒绝连接 – 若要防止对服务的其他租户产生负面影响。

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>使用智能重试/退让逻辑来缓解暂时性故障的影响

而不是引发异常，并向客户显示不可用或错误页，你可以识别错误的通常是暂时的并自动重试此操作中导致错误，希望，之后，长时间将会成功。 大多数情况下该操作将成功在第二次，并且你将从错误中恢复而无需客户曾遇到过感知时出现问题。

有几种方法可以实现智能重试逻辑。

- Microsoft 模式&amp;实践组具有[暂时性故障处理应用程序块](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx)执行的所有内容为你如果你使用 ADO.NET 来访问 SQL 数据库 （而不是通过实体框架）。 你只需设置重试 – 多少次重试查询策略或命令并等待多长时间之间尝试 – 和自动换行 SQL 中的代码*使用*块。

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    此外支持 TFH [Azure 角色中缓存](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx)和[Service Bus](https://azure.microsoft.com/services/service-bus/)。
- 当你使用实体框架时通常不使用直接与 SQL 连接，因此不能使用此模式和实践的包，但 Entity Framework 6 权限到框架生成这种重试逻辑。 以类似的方式指定重试策略，和 EF 然后使用该策略，只要它访问数据库。

    若要修复它应用中使用此功能，我们需要做只是添加一个类，派生自*DbConfiguration*开启重试逻辑。

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    对于框架标识为通常暂时性错误的 SQL 数据库异常，显示的代码指示 EF 使用指数退让延迟之间重试次数和最大延迟为 5 秒重试该操作最多 3 次。 指数退让意味着，每个失败的重试后它将等待更长的一段时间再重试。 如果行中的三个尝试失败，则将引发异常。 断路器有关的以下部分说明您为何指数退让和有限的数量的重试。

    在你使用 Azure 存储服务中，修复它应用程序的功能的 Blob，以及.NET 存储客户端 API 已实现相同的一种逻辑时，可能出现类似问题。 你只需指定重试策略，或甚至不需要执行此操作，如果您满意的默认设置。

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>断路器

为什么你不想重试太长一段时间内次数过多的原因包括：

- 永久重试失败的请求的用户太多可能会降低其他用户的体验。 所有使重复的重试请求数以百万计的人员是否你无法是占用 IIS 调度队列并阻止您的应用程序为它否则无法成功处理的请求提供服务。
- 如果每个人都正在重试由于服务失败，那里可能会太多请求排队等待启动恢复时，服务收到溢满。
- 如果错误是由于限制，并且没有的服务使用限制的时间窗口，继续重试无法移动该窗口，并造成限制，以继续。
- 你可能必须等待 web 页的用户，来呈现。 让人等待太长可能会更令人讨厌的该相对快一点建议他们选择以后再重试。

指数退让解决这些问题的一些限制的重试次数服务能够获得您的应用程序的频率。 但你还需要具有*断路器*： 这意味着，在某一特定重试您的应用程序将停止重试和采用一些其他操作，如下列项之一阈值：

- 自定义回退。 如果你不能从 Reuters 获取股票价格，可能是你可以从获取该： Bloomberg;或者，如果你不能从数据库中获取数据，可能是你可以将其从缓存。
- 失败无提示。 如果你从服务需要不全盘接受或者全盘为你的应用，只需返回 null 时无法获取数据。 如果要显示的修复它任务，而 Blob 服务未响应，则无法显示而无需映像的任务详细信息。
- 快速失败。 编写用户为避免填满该服务的具有错误重试请求可能导致其他用户的服务中断或扩展限制时段。 你可以显示友好"稍后重试"消息。

没有通用重试策略。 可以重试更多次，还可以在等待的时间再异步后台辅助进程不是像在用户正在等待响应同步 web 应用中。 可以等待时间之间的关系数据库服务的重试不是你的缓存服务一样。 以下是一些示例为建议重试策略，以让你了解如何数字可能会有所不同。 （"快速第一个"意味着不会出现在第一个重试之前延迟。

![示例重试策略](transient-fault-handling/_static/image1.png)

有关 SQL 数据库重试策略指南，请参阅[暂时性故障和连接到 SQL 数据库的错误的疑难解答](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/)。

## <a name="summary"></a>摘要

重试/退让策略可帮助使临时错误对不可见客户大多数情况下，以及 Microsoft 提供了可用于最大程度减少你的工作实现策略，无论你使用 ADO.NET、 实体框架中或 Azure 的框架存储服务。

在[下一章](distributed-caching.md)，我们将查看如何提高性能和可靠性，因为它使用分布式缓存。

## <a name="resources"></a>资源

有关更多信息，请参见以下资源：

文档

- [在 Azure 云服务上的大规模服务的设计的最佳实践](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)。 Mark Simms 和 Michael Thomassy 白皮书。 类似于防故障系列但进入更多操作指南的详细信息。 请参阅遥测和诊断部分。
- [防故障： 弹性云体系结构指南](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx)。 通过 Marc Mercuri、 Ulrich Homann 和 Andrew Townhill 白皮书。 网页上的防故障视频系列的版本。
- [Microsoft 模式和实践-Azure 指南](https://msdn.microsoft.com/library/dn568099.aspx)。 请参阅重试模式、 计划程序代理监督程序模式。
- [Azure SQL Database 中的容错能力](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx)。 Tony Petrossian 的博客文章。
- [实体框架的连接复原 / 重试逻辑](https://msdn.microsoft.com/data/dn456835)。 如何使用和自定义暂时性故障处理 Entity Framework 6 的功能。
- [连接复原和与实体框架中的 ASP.NET MVC 应用程序的命令截获](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)。 第四个在包含 9 个一部分教程系列中，演示如何为 SQL 数据库设置 EF 6 连接复原功能。

视频

- [防故障： 构建可扩展、 有弹性的云服务](https://channel9.msdn.com/Series/FailSafe)。 包含 9 个部分组成的系列 Ulrich Homann、 Marc Mercuri 和 Mark Simms。 高级概念和体系结构原理以非常可访问且有趣方式，提供与 Microsoft 客户咨询团队 (CAT) 体验与实际客户从绘制的情景。 请参阅断路器在段 3 开始 40:55 的讨论。
- [构建大： 从 Azure 客户的第 ii 部分到的经验教训](https://channel9.msdn.com/Events/Build/2012/3-030)。 Mark Simms 谈话有关设计失败，暂时性错误处理，和检测的所有内容。

代码示例

- [云服务在 Azure 中的基础知识](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)。 示例应用程序由 Microsoft Azure 客户咨询团队创建，它演示如何使用[企业库暂时性故障处理块](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)(TFH)。 有关详细信息，请参阅[云服务基础数据访问层-临时故障处理](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx)。 TFH 建议为直接 （不使用实体框架） 使用 ADO.NET 的数据库访问。

>[!div class="step-by-step"]
[上一页](monitoring-and-telemetry.md)
[下一页](distributed-caching.md)
