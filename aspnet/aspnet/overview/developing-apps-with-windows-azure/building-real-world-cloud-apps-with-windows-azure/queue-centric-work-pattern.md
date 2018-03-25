---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: "以队列为中心的工作模式 （使用 Azure 构建真实世界云应用） |Microsoft 文档"
author: MikeWasson
description: "构建真实世界云应用程序与 Azure 的电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: ccfbaa26cbf610f847811e6f3c612458277046ed
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>以队列为中心的工作模式 （使用 Azure 构建真实世界云应用）
====================
通过[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下载修复此错误项目](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下载电子书](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **构建真实世界云应用程序与 Azure**电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，从而帮助你为成功开发适用于云中的 web 应用。 有关电子书的信息，请参阅[第一章](introduction.md)。


更早版本，我们已了解使用多个服务可以导致"复合"SLA，其中应用程序的有效 SLA 是*产品*的单个 Sla。 例如，修复它应用程序使用网站、 存储和 SQL 数据库。 如果这些服务的任何一个失败，则应用程序将向用户返回错误。

缓存是一种好的方法来处理暂时性故障只读内容。 但是，如果你的应用程序需要完成的工作？ 例如，当用户提交新修复它任务，该应用程序不能只需将此任务放置到缓存。 应用程序需要将修复它任务写入到永久性数据存储，以便它可以处理。

这是以队列为中心的工作模式传入的位置。 此模式可让 web 层和后端服务之间的松散耦合。

以下是模式的工作原理。 当应用程序收到请求时，它将工作项放置到队列，并立即返回响应。 然后单独的后端进程拉取队列中的工作项，并执行该操作。

以队列为中心的工作模式适合用于：

- 是耗时 （高延迟） 的工作。
- 需要可能始终不可用的外部服务的工作。
- 工作，它是资源密集型 (高 CPU)。
- 会带来好处分级 （激增突然负载） 的速率的工作。

## <a name="reduced-latency"></a>降低的延迟

队列可任何时候执行耗时的工作。 如果任务需要几秒钟或更长，而不是阻止最终用户，放入队列工作项。 告知用户"我们正在努力解决它，"，然后使用队列侦听器以处理在后台任务。

例如，当你在一家网上零售商购买时，web 站点用于立即确认你的订单。 但这并不意味着你的资料已在卡车传递。 它们将任务放在队列中，并在后台，它们的执行信用检查，供发运，准备你的项和等等。

对于短滞后时间的方案，总的端到端时间可能较长使用队列，相比于以同步方式执行任务。 但是，即使这样，其他好处可以超过该缺点。

## <a name="increased-reliability"></a>更高的可靠性

在修复它，则我们一直在考虑在到目前为止的版本，web 前端与 SQL 数据库后端紧密耦合。 如果 SQL 数据库服务不可用，用户会收到错误。 如果重试不起作用 （也就是说，失败是暂时性多个），可以执行唯一操作是显示错误，并要求用户稍后重试。

![关系图显示 web 前端失败的 SQL 数据库后端时失败](queue-centric-work-pattern/_static/image1.png)

使用队列，当用户提交修复它任务时，应用程序将消息写入队列。 消息负载[JSON](http://json.org/)任务的表示形式。 一旦将消息写入到队列，该应用返回，并立即向用户显示一条成功消息。

如果任何后端服务 – 例如，SQL 数据库或队列侦听器--进入脱机状态，用户仍然可以提交新修复它任务。 除非后端服务是再次可用，否则，只需将排队等待消息。 此时后, 端服务将赶上积压工作上。

![显示 web 前端持续运行 SQL 数据库错误时的图示](queue-centric-work-pattern/_static/image2.png)

此外，现在你可以添加更多的后端逻辑而无需担心前端的复原。 例如，你可能想要每当新修复它分配给所有者发送电子邮件或 SMS 消息。 如果电子邮件或 SMS 服务变为不可用，可以处理其他任何内容，并将消息放入单独的队列用于发送电子邮件/SMS 消息。

以前，我们有效的 SLA 是 Web Apps&times;存储&times;SQL Database = 99.7%。 (请参阅[设计得以失败](design-to-survive-failures.md)。)

当我们更改应用程序以使用队列时，web 前端只依赖于 Web 应用和存储，为复合 99.8%的 SLA。 （请注意，队列都属于 Azure 存储服务，以便将它们包括在 blob 存储作为相同的 SLA。）

如果需要比 99.8%更理想的做法，你可以在两个不同区域中创建两个队列。 将作为主，另一个指定为辅助数据库。 在你的应用，故障转移到辅助队列未提供主队列时。 这两个不可在同一时间的机会是非常小。

## <a name="rate-leveling-and-independent-scaling"></a>速率分级和独立扩展性

队列还可以用于称为*速率分级*或*负载分级*。

Web 应用程序通常都容易遭受流量的突现高峰。 尽管你可以使用自动缩放以自动添加 web 服务器来处理增加的 web 流量，自动缩放可能不能响应速度足够快处理突然峰值负载。 如果 web 服务器可以卸载一些操作，他们需要通过一条消息写入队列执行操作，它们可以处理更多流量。 然后后, 端服务可以从队列中读取消息并处理它们。 队列深度将扩大或收缩当传入负载变化。

使用其大部分耗时工作卸载到后端服务，web 层可以更轻松地响应突然出现流量高峰。 因为任何给定的流量可以通过更少的 web 服务器来处理节省资金。

可以独立地扩展 web 层和后端服务。 例如，你可能需要三个 web 服务器，但只有一个服务器处理队列消息。 或者，如果你在后台中运行计算密集型任务，则可能需要更多的后端服务器。

![](queue-centric-work-pattern/_static/image3.png)

自动缩放的工作原理与后端服务以及 web 层。 你可以向上扩展或缩减正在处理的任务在队列中，根据 CPU 使用情况的后端 Vm 的 Vm 数目。 或者，你可以基于多少项在队列中的自动缩放。 例如，你可以判断自动缩放来尝试保留在队列中的不超过 10 项。 如果队列中有 10 个以上的项，自动缩放将添加 Vm。 当它们赶上时，自动缩放将关闭额外的 Vm。

## <a name="adding-queues-to-the-fix-it-application"></a>添加到该修补程序对其进行排队应用程序

若要实现的队列模式，我们需要更改修复它应用的两个。

- 当用户提交新修复它任务时，则将此任务放置在队列中，而不是写入数据库。
- 创建处理队列中的消息的后端服务。

对于队列中，我们将使用[Azure 队列存储服务](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/)。 另一个选项是使用[Azure Service Bus](https://docs.microsoft.com/azure/service-bus/)。

若要决定要使用的队列服务，请考虑您的应用程序需要发送和接收消息队列中的如何：

- 如果你有协同生产者和使用者竞争，请考虑使用 Azure 队列存储服务。 "共同完成的生成者"意味着多个进程要将消息添加到队列。 "使用者竞争"意味着多个进程要将消息从队列来处理，但任何给定的消息仅由一个"使用者。" 如果你需要更大的吞吐量不是你可以使用单个队列，，使用额外的队列和/或其他存储帐户。
- 如果你需要[发布/订阅模型](http://en.wikipedia.org/wiki/Publish/subscribe)，请考虑使用 Azure Service Bus 队列。

修复它应用适合协同生产者和竞争的使用者模型。

另一个注意事项是应用程序可用性。 队列存储服务是我们正在使用 blob 存储，因此它没有任何效果在我们的 SLA 的相同服务的一部分。 Azure Service Bus 是具有其自己的 SLA 的单独服务。 如果我们使用 Service Bus 队列，我们必须考虑其他的 SLA 百分比，并且我们的复合 SLA 会较低。 选择队列服务，请确保你了解所选应用程序可用性的影响。 有关详细信息，请参阅[资源](#resources)部分。

## <a name="creating-queue-messages"></a>创建队列消息

若要将修复它任务放置在队列中，web 前端，请执行以下步骤：

1. 创建[CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx)实例。 `CloudQueueClient`实例用于执行对队列服务的请求。
2. 创建队列，如果它尚不存在。
3. 序列化修复它任务。
4. 调用[CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx)以将消息放到队列。

我们将执行此构造函数中的工作和`SendMessageAsync`方法的一个新`FixItQueueManager`类。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

此处我们将使用[Json.NET](https://github.com/JamesNK/Newtonsoft.Json)要序列化到 JSON 格式 fixit 库。 你可以使用任何您喜欢的序列化方法。 JSON 采用的优点，用户可读，但比 XML。

生产高质量代码将添加错误处理逻辑，暂停如果数据库变得不可用、 更完全处理恢复、 应用程序启动时创建队列和管理"[有害"消息](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx)。 （病毒消息是由于某种原因无法处理的消息。 你不希望病毒消息发送到队列，其中辅助角色将不断尝试处理它们、 失败、 再试一次、 失败，和等。）

在前端 MVC 应用程序，我们需要更新创建新任务的代码。 而不是将任务放入存储库，调用`SendMessageAsync`上面所示方法。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>处理队列消息

若要处理队列中的消息，我们将创建的后端服务。 后端服务将运行一个无限循环，执行以下步骤：

1. 从队列中获取下一条消息。
2. 反序列化到修复它任务的消息。
3. 修复它任务写入数据库。

若要托管后端服务，我们将创建 Azure 云服务包含*辅助角色*。 辅助角色包含可以执行后端处理的一个或多个 Vm。 在这些 Vm 中运行的代码将请求在变得可用队列中的消息。 对于每个消息中，我们将反序列化 JSON 负载，并将修复它任务实体的实例写入到数据库，使用我们之前在 web 层中使用相同的存储库。

以下步骤演示如何将工作人员添加到具有标准 web 项目的解决方案的角色项目。 已修复它在项目中，你可以下载完成这些步骤。

首先将云服务项目添加到 Visual Studio 解决方案。 右键单击解决方案并选择**添加**，然后**新项目**。 在左窗格中，展开**Visual C#**和选择**云**。

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

在**新建 Azure 云服务**对话框中，展开**Visual C#**左窗格中的节点。 选择**辅助角色**，然后单击右箭头图标。

![](queue-centric-work-pattern/_static/image6.png)

(请注意，你还可以添加*web 角色*。 我们无法修复它前端相同的云服务，而不是运行它在 Azure 网站中运行。 具有一些优势中使前端和后端之间的连接更方便地协调。 但是，为了简化本演示，我们要保持前端在 Azure App Service Web 应用并且仅在云服务中运行后端。）

默认名称分配给辅助角色。 若要更改名称，将鼠标悬停在右窗格中，辅助角色，然后单击铅笔图标。

![](queue-centric-work-pattern/_static/image7.png)

单击**确定**以完成对话框。 这会将两个项目添加到 Visual Studio 解决方案。

- Azure 项目定义云服务，包括配置信息。
- 辅助角色项目，定义辅助角色。

![](queue-centric-work-pattern/_static/image8.png)

有关详细信息，请参阅[使用 Visual Studio 创建 Azure 项目。](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

在辅助角色中，我们轮询消息通过调用`ProcessMessageAsync`方法`FixItQueueManager`我们前面看到的类。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

`ProcessMessagesAsync`方法检查是否正在等待消息。 如果没有一个，反序列化到消息`FixItTask`实体，并将该实体保存在数据库中。 它循环，直到队列为空。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

轮询的队列消息会产生少量的事务进行收费，因此，如果没有消息等待处理，辅助角色的`RunAsync`方法等待第二个轮询之前再次通过调用`Task.Delay(1000)`。

在 web 项目中，添加异步代码可以自动提高性能，因为 IIS 管理有限的线程池。 即不是这种情况在辅助角色项目。 若要提高可伸缩性的辅助角色，你可以编写多线程的代码，或使用异步代码来实现[并行编程](https://msdn.microsoft.com/library/ff963553.aspx)。 此示例未实现的并行编程，但演示如何使代码异步，因此您可以实施并行编程。

## <a name="summary"></a>摘要

在本章中，你已了解如何通过实现以队列为中心的工作模式来提高应用程序响应能力、 可靠性和可伸缩性。

这是此电子书中介绍的 13 模式的最后一个但当然还有许多其他模式和实践，从而帮助你生成成功的云应用程序。 [最后一章](more-patterns-and-guidance.md)的主题，尚未已涉及的这些 13 模式提供的资源的链接。

<a id="resources"></a>
## <a name="resources"></a>资源

有关队列的详细信息，请参阅以下资源。

文档：

- [Microsoft Azure 存储队列第 1 部分： 入门](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/)。 通过罗马 Schacherl 的文章。
- [执行后台任务](https://msdn.microsoft.com/library/ff803365.aspx)的第 5 章[移动应用程序迁移到云，第三版](https://msdn.microsoft.com/library/ff728592.aspx)从 Microsoft 模式和实践。 (具体而言，部分["使用 Azure 存储队列"](https://msdn.microsoft.com/library/ff803365.aspx#sec7)。)
- [最大程度提高可伸缩性和成本的效率的 Azure 上的基于队列的消息传送解决方案的最佳做法](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx)。 通过 Valery Mizonov 白皮书。
- [比较 Azure 队列和 Service Bus 队列](https://msdn.microsoft.com/magazine/jj159884.aspx)。 MSDN 杂志文章提供了可帮助您选择要使用的队列服务的其他信息。 本文提及 Service Bus 是依赖于 ACS 进行身份验证，这意味着 ACS 不可用时，你的 SB 队列也将不可用。 但是，由于在撰写本文时，SB 已进行更改以使你能够使用[SAS 令牌](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx)作为 ACS 的替代方法。
- [Microsoft 模式和实践-Azure 指南](https://msdn.microsoft.com/library/dn568099.aspx)。 请参阅异步消息传送入门、 管道和筛选器模式、 补偿事务模式、 使用者竞争模式、 CQRS 模式。
- [CQRS 旅程](https://msdn.microsoft.com/library/jj554200)。 有关 Microsoft 模式与实践中的 CQRS 电子书。

视频：

- [防故障： 构建可扩展、 有弹性的云服务](https://channel9.msdn.com/Series/FailSafe)。 通过 Ulrich Homann、 Marc Mercuri 和 Mark Simms 九一部分视频系列。 高级概念和体系结构原理以非常可访问且有趣方式，提供与 Microsoft 客户咨询团队 (CAT) 体验与实际客户从绘制的情景。 有关 Azure 存储服务和队列的介绍，请参阅段 5 开始 35:13。

>[!div class="step-by-step"]
[上一页](distributed-caching.md)
[下一页](more-patterns-and-guidance.md)
