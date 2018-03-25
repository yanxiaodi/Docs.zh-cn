---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: "最新信息在 ASP.NET Web 页 3.2 |Microsoft 文档"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/30/2014
ms.topic: article
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: cdb0e259bbf27d1d3dcf6ada11e6636c9cefcc9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-pages-32"></a>什么是所需的 ASP.NET Web Pages 3.2 中的新增功能
====================
通过[Microsoft](https://github.com/microsoft)

本主题介绍什么是用于 ASP.NET Web Pages 3.2，网页 3.2.2 的新功能和[网页 3.2.3 beta1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>ASP.NET Web Pages 3.2

此版本修复了 bug，并引入了一项新功能。

## <a name="download"></a>下载

运行时功能将发布为 NuGet 库上的 NuGet 包。 所有运行时包按照[语义版本控制](http://semver.org/)规范。 ASP.NET 网页 3.2 包具有以下版本： &ldquo;3.2.0&rdquo;。 你可以安装或更新这些包通过[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/)。 版本还包括在 NuGet 上的相应本地化的包。

你可以安装或使用 NuGet 程序包管理器控制台更新到已发布的 NuGet 程序包：

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>新功能和 Bug 修复

我们修复了一个 bug，并在此版本中所做的一项次要功能增强。 您可以为同一发现查询[此处](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)。

## <a name="aspnet-web-pages-322"></a>ASP.NET Web Pages 3.2.2

此版本汇总更改[所需的 ASP.NET Web Pages 3.2.1 Beta 版本](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx)提供显著的性能提高了呈现大型 razor 页。 请参阅[Codeplex 问题 585](https://aspnetwebstack.codeplex.com/workitem/585)。 此版本与 MVC 5.2.2 一致现在将取决于此版本的包。

我们致力于呈现大型页与 MSN 团队。 当页呈现超过 80 千字节为单位的数据时，我们会得到大型对象堆上的对象。 使用多个层的布局时可以乘以此效果。

在服务器上的结果是额外的 CPU 使用率、 更长时间保留的内存，并甚至长暂停期间[第 2 代清理](https://msdn.microsoft.com/en-us/library/ms973837.aspx)在垃圾回收器。

下面是演示的分析结果的表[perfview](https://channel9.msdn.com/Series/PerfView-Tutorial)运行。 CPU 保留常量在大约 68%，而正在呈现大型页。 下表显示，已几乎完全消除第 2 代回收的数量，并将其结果是更高版本的请求速率，并且由垃圾回收暂停时间的相当大减少。

| **区域** | **Before (3.2)** | **后 (3.2.1)** | **增量 %** |
| --- | --- | --- | --- |
| 总请求 （计数） | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| 跟踪持续时间 （秒） | 196.20 | 198.60 | 1.20% |
| 请求/秒 | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| CPU 负载 | 68.80% | 68.50% |  -0.40% |
| GC CPU 示例 | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| 总分配 （计数） | 55,357,146 | 57,222,949 | 3.40% |
| 总 GC 暂停 （示例） | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| Gen0 GC （计数） | 403 | 1,216 | 201.70% |
| Gen1 GC （计数） | 290 | 367 | 26.60% |
| Gen2 GC （计数） | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| CPU / 请求 （示例数） | 19.73 | 16.47 | -16.50% |

| 颜色编码： | <font style="background-color: #00ff00">核心改善</font> | <font style="background-color: #4bacc6">积极对性能的影响</font> |
| --- | --- | --- |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET 网页 3.2.3 beta1

此版本包含仅 bug 修复。 你可以使用[此查询](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)若要查看此版本中修复的问题的列表。
