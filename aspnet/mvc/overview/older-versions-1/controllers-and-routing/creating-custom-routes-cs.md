---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: "创建自定义路由 (C#) |Microsoft 文档"
author: microsoft
description: "了解如何将自定义的路由添加到 ASP.NET MVC 应用程序。 在本教程中，您将学习如何修改 Global.asax 文件中的默认路由表。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: d1542103298f2fa09dc71706284afb18d8381403
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="creating-custom-routes-c"></a>创建自定义路由 (C#)
====================
通过[Microsoft](https://github.com/microsoft)

> 了解如何将自定义的路由添加到 ASP.NET MVC 应用程序。 在本教程中，您将学习如何修改 Global.asax 文件中的默认路由表。


在本教程中，您将学习如何将自定义的路由添加到 ASP.NET MVC 应用程序。 了解如何修改在 Global.asax 文件中使用自定义路由的默认路由表。

对于许多简单的 ASP.NET MVC 应用程序，默认的路由表将正常工作。 但是，你可能会发现有专门的路由的需要。 在这种情况下，你可以创建一个自定义路由。

例如，假定，您正在构建博客应用程序。 你可能想要处理传入请求，如下所示：

/ 存档/12-25-2009

当用户输入此请求时，你想要返回到日期相对应的博客项 2009 年 12 月 25 日。 若要处理这种类型的请求，你需要创建一个自定义路由。

列表 1 中的 Global.asax 文件包含一个新的自定义路由，名为博客，如下所示 /Archive/ 哪些句柄请求*条目日期*。

**列表 1-Global.asax （与自定义路由）**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

你将添加到路由表的路由的顺序很重要。 在现有的默认路由之前添加了我们新的自定义博客路由。 如果撤消顺序，则默认路由始终将获取调用而不是自定义路由。

自定义博客路由与匹配开头/存档/任何请求。 因此，它与匹配的所有以下 Url:

- / 存档/12-25-2009

- / 存档/10-6-2004

- / 存档/apple

自定义的路由将传入请求映射到名为存档的控制器，并调用 Entry() 操作。 当调用 Entry() 方法时，输入日期作为一个名为 entryDate 参数传递。

你可以使用清单 2 中的控制器博客自定义路由。

**列出 2-ArchiveController.cs**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

请注意，列出 2 中的 Entry() 方法接受类型为 DateTime 的参数。 足够智能，可将从该 URL 输入日期转换为日期时间值中时将自动 MVC 框架。 如果从 URL 条目 date 参数无法转换为 DateTime，将引发错误 （请参见图 1）。

**图 1-从将参数转换的错误**


[![新项目对话框](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)

**图 01**： 从将参数转换的错误 ([单击以查看实际尺寸的图像](creating-custom-routes-cs/_static/image2.png))


## <a name="summary"></a>摘要

本教程的目标是演示如何创建一个自定义路由。 您学习了如何将自定义的路由添加到表示博客条目 Global.asax 文件中的路由表。 我们讨论了如何将博客条目的请求映射到名为 ArchiveController 的控制器和名为 Entry() 的控制器操作。

>[!div class="step-by-step"]
[上一页](aspnet-mvc-controllers-overview-cs.md)
[下一页](creating-a-route-constraint-cs.md)
