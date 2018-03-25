---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: "数据分区策略 （使用 Azure 构建真实世界云应用） |Microsoft 文档"
author: MikeWasson
description: "构建真实世界云应用程序与 Azure 的电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: dca016cb6293a346f5622cc272e510b182c86d58
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>数据分区策略 （使用 Azure 构建真实世界云应用）
====================
通过[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下载修复此错误项目](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下载电子书](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **构建真实世界云应用程序与 Azure**电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，从而帮助你为成功开发适用于云中的 web 应用。 有关序列的信息，请参阅[第一章](introduction.md)。


前面我们已了解通过添加和删除 web 服务器扩展的云应用程序，web 层是多么容易。 但是，如果它们在所有达到相同的数据存储，你的应用程序瓶颈从前端移到后端，并且数据层，最难缩放。 本章我们了解如何，通过将数据分区为多个关系数据库，或通过将与其他数据存储选项结合使用关系数据库存储，可以使数据层可缩放。

设置分区方案是最佳完成提前与前面所述相同的原因： 很难，若要在应用程序在生产中之后，更改你的数据存储策略。 如果你考虑硬提前不同的方法，你可以避免"Twitter 片刻"时你的应用程序崩溃或长时间出现故障，而重新组织您的应用程序数据和数据访问代码。

## <a name="the-three-vs-of-data-storage"></a>数据存储在三个 Vs

为了确定是否需要分区策略和它应该是什么，请考虑你的数据有关的三个问题：

- 卷 – 数据量将你最终存储？ 几千兆字节？ 几个几百千兆字节为单位？ 兆兆字节？ Petabytes？
- 速度 – 你的数据会的速率是什么？ 它是不生成大量数据的内部应用？ 将上载客户，图像和视频到外部应用程序？
- 各种 – 哪种类型的数据将你存储？ 关系，图像、 键-值对、 社交关系图？

如果你认为你要有大量的卷、 速度或各种，你必须仔细考虑哪种类型的分区方案最启用您的应用程序缩放高效且有效地增加，以及以确保不会遇到任何瓶颈。

主要有三种分区的方法：

- 垂直分区
- 水平分区
- 混合分区

## <a name="vertical-partitioning"></a>垂直分区

垂直 portioning 就像拆分表的列： 一组列将进入一个数据存储和另一组列会进入不同的数据存储。

例如，假设我的应用程序存储数据的人员，包括映像：

![数据表](data-partitioning-strategies/_static/image1.png)

当表示此类数据作为表并查看不同类型的数据时，你可以查看在左侧的三个列具有可以高效地存储通过关系数据库中，在右侧的两个列时实质上是字节数组的字符串数据，c从图像文件 ome。 可以存储图像文件数据的关系数据库中，但很多人这样做是因为它们不想将数据保存到文件系统。 它们可能不具有能够存储的数据所需的卷的文件系统，或者它们可能不想要管理单独备份和还原系统。 此方法适用适用于本地数据库和适用于少量的云数据库中的数据。 在本地环境中，它可能更轻松地只需让数据库管理员负责的一切内容。

但在云数据库中，存储代价相对较大，并且有很高的映像无法使超出速率它可以有效地限制增长的数据库的大小。 可以通过将数据垂直分区解决这些问题，这意味着在你的数据的表中选择最合适的数据存储区为每个列。 新增功能可能适合于此示例是将字符串数据放在关系数据库和 Blob 存储中的映像中。

![垂直分区的数据表](data-partitioning-strategies/_static/image2.png)

将映像存储在 Blob 存储而非数据库是在本地环境中比云中更为可行，因为你不必担心如何设置文件服务器或管理备份和还原存储在关系数据库之外的数据： 所有为你自动由处理 Blob 存储服务。

这是分区方法中修复该应用程序中，我们实现和我们在该代码将查看[Blob 存储章](unstructured-blob-storage.md)。 如果没有此分区方案和假设 3 兆字节为单位的平均图像大小，修复它应用仅会能够存储才命中 150 千兆字节的最大数据库大小约 40000 个任务。 删除映像之后，数据库可以存储 10 次为许多任务;你可以更长的时间之前，你需要考虑实现水平分区方案。 作为应用程序可以进行扩展，您的费用更慢增长因为你的存储需求的大容量正进入非常成本较低的 Blob 存储。

## <a name="horizontal-partitioning-sharding"></a>水平分区 （分片）

水平 portioning 就像表按行拆分： 的行的一组将进入一个数据存储，并在另一组行中断到不同的数据存储区。

提供相同的数据集，另一个选项是在不同的数据库中存储的客户名称不同的范围。

![水平分区的数据表](data-partitioning-strategies/_static/image3.png)

你想要格外小心以确保数据均匀地分布以避免产生热点你分片方案。 这个简单的示例，使用姓氏的第一个字母不能满足该需求，因为很多人具有以某些常见的字母开头的姓氏。 将早于你所料因为某些数据库将变得很大，而大多数将保持较小命中表的大小限制。

水平分区的缺点是，因此可能很难跨所有数据都执行的查询。 在此示例中，查询必须绘制来自最多 26 个不同的数据库，若要获取所有应用所存储的数据。

## <a name="hybrid-partitioning"></a>混合分区

你可以组合垂直和水平分区。 例如，在示例数据，你可以在 Blob 存储中存储图像，并水平分区的字符串数据。

![数据分区的表混合](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>分区生产应用程序

从概念上讲很容易地了解如何将工作原理的分区方案，但任何分区方案也会增加代码复杂性和带来许多新的复杂情况，你必须处理的。 如果你要将移动到 blob 存储的映像，你做什么存储服务出现故障时？ 如何处理 blob 安全？ 如果将数据库和 blob 存储获取同步的会发生什么情况？ 如果你是分片，您将如何处理跨所有数据库都查询？

处理程序，但前提是你转到生产环境之前，你计划为其，复杂性是易于管理。 未执行此操作的许多人希望它们具有更高版本。 我们的客户咨询团队 (CAT) 团队中非常大的方式，采用其应用的客户从获取有关每月一次不稳定电话呼叫的平均值和它们未进行此规划。 和他们所说的内容类似的内容:"帮助 ！ 我将所有内容放在单个数据存储和 45 天内我要在其上运行空间不足 ！" 并且，如果你有大量的内置于你如何访问数据存储区的业务逻辑，并且必须使用你的应用程序的客户，没有没有合适的时间为一天就出现故障，在迁移时。 我们得到其数据 herculean 工作量来帮助客户分区经过在运行过程中使用无需停机。 它是非常令人兴奋和非常有点复杂，并且不适合你想要参与如果您可以避免使用它 ！ 提前考虑这并将其集成到你的应用将使你更容易得多如果应用程序的增长更高版本。

## <a name="summary"></a>摘要

有效的分区方案可以启用你的云应用程序扩展到数千兆而无需瓶颈云中的数据。 和无需支付提前大规模机或广泛的基础结构为的方式可能就像在本地数据中心中运行应用程序。 在云中可以随着您需要它，并且您要仅付款，则根据你正在使用当你使用此选项以增量方式可以增加容量。

在[下一章](unstructured-blob-storage.md)我们将了解如何解决它应用实施垂直分区通过将图像存储在 Blob 存储中。

## <a name="resources"></a>资源

有关分区策略的详细信息，请参阅以下资源。

文档：

- [在 Windows Azure 云服务上的大规模服务的设计的最佳实践](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)。 Mark Simms 和 Michael Thomassy 白皮书。
- [Microsoft 模式和实践-云设计模式](https://msdn.microsoft.com/library/dn568099.aspx)。 请参阅数据分区指南，分片模式。

视频：

- [防故障： 构建可扩展、 有弹性的云服务](https://channel9.msdn.com/Series/FailSafe)。 包含 9 个部分组成的系列 Ulrich Homann、 Marc Mercuri 和 Mark Simms。 高级概念和体系结构原理以非常可访问且有趣方式，提供与 Microsoft 客户咨询团队 (CAT) 体验与实际客户从绘制的情景。 请参阅段 7 中的分区描述。
- [构建大： 从 Windows Azure 客户的第 I 部分到的经验教训](https://channel9.msdn.com/Events/Build/2012/3-029)。Mark Simms 讨论分区方案，分片策略如何实现分片和 SQL Database 联合，开始 19:49。 类似于防故障系列但进入更多操作指南的详细信息。

示例代码：

- [云服务在 Windows Azure 中的基础知识](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)。 示例包括分片数据库的应用程序。 有关实现的分片方案的说明，请参阅[DAL – 分片的 RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) Windows Azure 博客上。

>[!div class="step-by-step"]
[上一页](data-storage-options.md)
[下一页](unstructured-blob-storage.md)
