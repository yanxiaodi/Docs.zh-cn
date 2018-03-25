---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: "设计得以失败 （使用 Azure 构建真实世界云应用） |Microsoft 文档"
author: MikeWasson
description: "构建真实世界云应用程序与 Azure 的电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 62a16e9a99cd1298b0db5f8099092212f9f6850b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>设计得以失败 （使用 Azure 构建真实世界云应用）
====================
通过[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下载修复此错误项目](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下载电子书](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **构建真实世界云应用程序与 Azure**电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，从而帮助你为成功开发适用于云中的 web 应用。 有关电子书的信息，请参阅[第一章](introduction.md)。


你必须考虑在生成的应用程序，但尤其是在云中其中很多人将使用它，将运行的是任何类型的项目之一是如何设计应用程序，以便它可以正常处理故障并继续提供的价值高达可能。 只要有足够的时间，操作将会出现在任何环境或任何软件系统错误。 你的应用程序处理这些情况的方式确定你的客户将获得如何不快和多少时间必须分析和修复问题的部分付费。

## <a name="types-of-failures"></a>类型的故障

有两种基本类别的你将需要以不同方式处理的失败：

- 暂时性故障，自我修复失败，如间歇性网络连接问题。
- 持久需要干预的故障。

对于临时故障，你可以实现重试策略，以确保该大多数情况下应用程序恢复快速和自动。 你的客户可能会注意到较长的响应时间，但否则它们不会受到影响。 我们将介绍一些方法处理中的这些错误[暂时性故障处理章](transient-fault-handling.md)。

持久故障，你可以实现监视和日志记录时出现问题，并且，便于根本原因分析立即通知你的功能。 我们将介绍一些方法来帮助你保持在这些类型中的错误的之上[监视和遥测章节](monitoring-and-telemetry.md)。

## <a name="failure-scope"></a>失败作用域

你还必须考虑失败作用域 – 是否受到影响一台计算机，例如 SQL 数据库或存储或整个区域的整个服务。

![失败作用域](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>计算机失败

在 Azure 中，发生故障的服务器自动替换为一个新的活动，并设计良好的云应用程序快速自动地从这种类型的故障恢复。 前面我们强调无状态 web 层、 可伸缩性优点和容易从发生故障的服务器恢复是无状态的另一个好处。 容易恢复也是平台即服务 (PaaS) 功能，例如 SQL 数据库和 Azure App Service Web Apps 的优势之一。 硬件故障很少见，但当它们发生这些服务处理这些自动保存功能。你甚至无需编写代码来处理计算机失败，在你使用这些服务之一时。

### <a name="service-failures"></a>服务失败

云应用程序通常使用多个服务。 例如，修复它应用使用的 SQL 数据库服务，存储服务，并且 web 应用部署到 Azure App Service。 将你的应用程序做什么如果其中一个依赖于服务失败？ 某些服务失败时，一个友好"很抱歉，稍后重试"消息可能是的最佳做法。 但在许多情况下你可以做得好。 例如，当后端数据存储区已关闭，你可以接受用户输入，显示"你的请求已收到，"以及存储其他任何位置的输入暂时;然后您需要的服务是再次，正常运行时可以检索输入并对其进行处理。

[以队列为中心的工作模式](queue-centric-work-pattern.md)章演示一种方法处理这种情况。 修复它应用存储在 SQL 数据库中的任务，但它无需退出工作时 SQL 数据库已关闭。 这一章中我们将了解如何在队列中，存储的任务的用户输入并使用工作进程来读取队列和更新任务。 如果 SQL 已关闭，创建修复它的任务的能力会受到影响;工作进程可以等待，SQL 数据库不可用时处理新的任务。

### <a name="region-failures"></a>区域故障

整个区域可能会失败。 自然灾难操作可能会破坏数据中心，它可能由 meteor 获取平展，无法通过 burying backhoe 等 cow 乡间剪切 trunk 行到数据中心。如果 stricken 数据中心托管你的应用程序做什么？ 就可以在 Azure 中的应用程序设置在多个区域同时运行，以便如果在其中一个灾难，你继续在另一个区域中运行。 这种故障均极少出现，并通过确保通过这种故障不间断的服务所需圈，大多数应用不跳转。 请参阅有关如何保护你的应用程序可通过区域失败甚至章末尾的资源部分。

Azure 目标是使处理所有这些类型的故障容易得多，并且你将看到如何我们执行的操作，在以下章节中的一些示例。

## <a name="slas"></a>Sla

人们通常了解云环境中的服务级别协议 (Sla)。 基本上，这些是有关如何可靠其服务是公司做出的承诺。 99.9 %sla 意味着你应预计要工作正常 99.9%的时间的服务。 这是 SLA 的相当典型值和这听起来像非常高的值，但你可能没有意识到多少停工时间。 1%到实际金额。 下面是演示各种 SLA 百分比通过每一年、 月和每周达到多少停机时间的表。

![SLA 表](design-to-survive-failures/_static/image2.png)

因此 99.9 %sla 意味着你的服务可能是下 8.76 小时中，是每一年或每月的 43.2 分钟。 这是更停机时间不是大多数人实现。 作为开发人员，因此你想要注意，可能会经过一定量的停机时间和正常方式处理它。 在某一时刻人想要使用你的应用，和服务将已关闭，并且你想要最大程度减少该客户的负面影响。

你应了解的有关 SLA 的一件事情是它到引用的哪个时间段： 没有时钟获取重置每周、 每月或每年？ 在 Azure 中我们将时钟重置优于为你每年的 SLA，因为每年 SLA 无法通过偏移它们具有一系列的良好月隐藏错误个月的每个月。

当然我们始终能够以合理方式执行性能优于 SLA;通常，你将准备下比少得多。 承诺是，如果我们曾经关闭的时间超过停机时间的最大则可以要求退款。 金额，则将取回可能无法完全补偿你的停机时间，过多的业务影响，但该方面的 SLA 充当强制策略，并让你知道，我们不要认为这非常很严重。

### <a name="composite-slas"></a>复合 Sla

要考虑您时查看 Sla 的一个重点是在应用程序，将多个服务使用每个服务具有单独的 SLA 的影响。 例如，修复它应用使用 Azure App Service Web Apps、 Azure 存储空间和 SQL 数据库。 下面是截至日期在 2013 年 12 月，写入此电子书其 SLA 号码：

![SLA Web 站点，存储，SQL 数据库](design-to-survive-failures/_static/image3.png)

停机时间预期的应用程序中基于这些服务 Sla 的最大值是什么？ 你可能认为，停机时间将是等于的最差的 SLA 百分比或 99.9%在这种情况下。 如果所有三个服务始终失败一次，但这不一定会发生什么实际情况，将为 true。 每个服务可能会失败独立地在不同时间，因此你必须通过将乘以单个 SLA 数字计算的复合 SLA。

![复合 SLA](design-to-survive-failures/_static/image4.png)

因此你的应用程序无法得停机 43.2 分钟不只是一个月，但该 3 次数量，每月 – 108 分钟，仍可以在 Azure SLA 的限制内。

此问题不是 Azure 所特有的。 我们实际上提供最佳的云服务级别协议的任何云服务可用，并且你将有类似的问题，如果你使用任何供应商的云服务处理。 这突出显示是考虑如何可以设计您的应用程序适当，处理不可避免地服务失败，因为它们可能碰巧频率不够高，影响会客户或用户的重要性。

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>云服务级别协议相比企业停机时间体验

人有时说，"我的企业应用程序中永远不会遇到这些问题。" 如果你需要多少每月的停工时间他们实际上有，他们通常说，"，它发生情况有时。" 如果你需要何种频率，它们地接受，"有时我们需要备份或安装新的服务器或更新软件"。 当然的计数为停机时间。 大多数企业应用，除非它们是时间的尤其是时间的任务关键型其实是时间的向下我们服务的 Sla 所允许量超过。 但在你的服务器和基础结构，并且你已负责它，并控制它，往往会更少 angst 看法停机时间。 在云环境中你是依赖于其他人并不知道发生了什么情况，因此你可能会变得更加担心它。

在企业来实现更大的正常运行时间百分比比从云中 SLA 获取，它们可以执行此操作通过在硬件上花费大量更多的资金。 云服务可执行该操作，但必须向其服务的其他更多。 相反，你充分利用经济高效的服务，并设计你的软件，以便不可避免地故障会对你的客户造成最小中断。 你为云应用程序设计器的作业不是非常避免并避免灾难故障和执行此操作通过将重点放在软件，不在硬件上。 企业应用尽可能最大化平均无故障时间，而云应用程序尽可能最小化平均恢复时间。

### <a name="not-all-cloud-services-have-slas"></a>并非所有云服务都具有服务级别协议

注意还不是每个云服务甚至具有 SLA。 如果你的应用程序依赖于具有无法正常运行时间保证的服务，你可能已停机远远超过可能假设。 例如，如果您启用到你的站点使用 Facebook 或 Twitter 等社交提供程序日志中，检查与服务提供商，以查看是否没有 SLA 下，且你可能会发现有不包括。 但如果身份验证服务出现故障，或无法支持大量引发它的请求，将你的客户解锁您的应用程序。 你可能已停机天或更长时间。 一个新的应用程序的创建者应数百个数以百万计的下载和依赖关系对 Facebook 身份验证 – 但不太迟继续实时和发现之前与 Facebook 交流，没有为该服务没有 SLA。

### <a name="not-all-downtime-counts-toward-slas"></a>并非所有停机时间将计入 Sla

如果你的应用过度使用它们，某些云服务可能会有意拒绝服务。 这称为*限制*。 如果服务具有 SLA，它应当在其下你可能会被阻止，，并应用设计应避免这些条件和限制，如果发生或相应地作出反应的条件。 例如，如果服务请求的启动失败时超过每秒一定数量，想要确保不太快，它们会导致若要继续，限制发生自动重试。 我们将有更详细说明了在限制[暂时性故障处理章](transient-fault-handling.md)。

## <a name="summary"></a>摘要

本章已尝试帮助你实现具有真实世界云应用程序设计为能够正常经受故障的原因。 从开始[下一章](monitoring-and-telemetry.md)，这一系列中的剩余模式转到更详细地介绍一些策略你可以使用它们来执行该操作：

- 不会有什么具有[监视和遥测](monitoring-and-telemetry.md)，以便你了解快速故障需要干预，并具有足够的信息来解决这些问题。
- [处理暂时性故障](transient-fault-handling.md)通过实现智能重试逻辑，以便你的应用程序将自动恢复时可以和回退到[断路器](transient-fault-handling.md#circuitbreakers)逻辑时它不能。
- 使用[分布式缓存](distributed-caching.md)以尽量减少数据库访问吞吐量、 延迟和连接问题。
- 实现松散耦合通过[以队列为中心的工作模式](queue-centric-work-pattern.md)，以便可以继续工作的后端出现故障时应用程序前端。

## <a name="resources"></a>资源

有关详细信息，请参阅后面的章节中此电子书和以下资源。

文档：

- [防故障： 弹性云体系结构指南](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx)。 通过 Marc Mercuri、 Ulrich Homann 和 Andrew Townhill 白皮书。 网页上的防故障视频系列的版本。
- [在 Azure 云服务上的大规模服务的设计的最佳实践](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)。 Mark Simms 和 Michael Thomassy 白皮书。
- [Azure 业务连续性技术指南](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx)。 Patrick Wickline 和 Jason Roth 白皮书。
- [灾难恢复和高可用性 Azure 应用程序](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx)。 由 Michael McKeown、 Hanu Kommalapati 和 Jason Roth 白皮书。
- [Microsoft 模式和实践-Azure 指南](https://msdn.microsoft.com/library/dn568099.aspx)。 请参阅多数据中心部署指南，断路器模式。
- [Azure 支持的服务级别协议](https://azure.microsoft.com/support/legal/sla/)。
- [Azure SQL Database 中的业务连续性](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx)。 有关 SQL 数据库高可用性和灾难恢复功能的文档。
- [高可用性和灾难恢复的 Azure 虚拟机中 SQL Server](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx)。

视频：

- [防故障： 构建可扩展、 有弹性的云服务](https://channel9.msdn.com/Series/FailSafe)。 包含 9 个部分组成的系列 Ulrich Homann、 Marc Mercuri 和 Mark Simms。 高级概念和体系结构原理以非常可访问且有趣方式，提供与 Microsoft 客户咨询团队 (CAT) 体验与实际客户从绘制的情景。 集 1 和 8 进入深度在设计云应用程序后存在失败的原因。 另请参阅后续讨论的限制在段 2 开始 49:57、 故障点和在段 2 开始 56:05，故障模式的讨论和的断路器在段 3 开始 40:55 讨论。
- [构建大： 从 Azure 客户的第 ii 部分到的经验教训](https://channel9.msdn.com/Events/Build/2012/3-030)。 Mark Simms 介绍的失败的设计和检测的所有内容。 类似于防故障系列但进入更多操作指南的详细信息。

>[!div class="step-by-step"]
[上一页](unstructured-blob-storage.md)
[下一页](monitoring-and-telemetry.md)
