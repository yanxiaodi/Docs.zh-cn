---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: "创建自定义路由约束 (C#) |Microsoft 文档"
author: StephenWalther
description: "Stephen Walther 演示如何创建自定义的路由约束。 我们实现一个简单阻止路由的自定义约束匹配 w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: c31ba3382b9dbe22a6826b9f858944c223efdd9d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-route-constraint-c"></a>创建自定义路由约束 (C#)
====================
通过[Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther 演示如何创建自定义的路由约束。 我们实现简单的自定义约束，以防止从远程计算机进行浏览器请求时要匹配的路由。


本教程的目标是演示如何创建自定义的路由约束。 自定义的路由约束，可防止一个路由，除非匹配某些自定义的条件匹配。

在本教程中，我们创建 Localhost 路由约束。 Localhost 路由约束仅匹配来自本地计算机发出的请求。 通过 Internet 从远程请求都不匹配。

通过实现 IRouteConstraint 接口实现一个自定义的路由约束。 这是一个极简单的接口，其中介绍了一种方法：

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

该方法返回一个布尔值。 如果返回 false，则与约束关联的路由不会匹配浏览器请求。

列表 1 中包含的 Localhost 约束。

**列表 1-LocalhostConstraint.cs**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

列表 1 中的约束充分利用由 HttpRequest 类公开 IsLocal 属性。 该请求的 IP 地址是任一 127.0.0.1 或请求的 IP 等同于服务器的 IP 地址时，此属性返回 true。

使用自定义在 Global.asax 文件中定义的路由约束。 列出 2 中的 Global.asax 文件使用 Localhost 约束以防止任何人都要求管理员页，除非它们从本地服务器发出请求。 例如，/Admin/DeleteAll 的请求将失败时从远程服务器进行。

**列出 2-Global.asax**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

管理员路由定义中使用 Localhost 约束。 此路由将不匹配的远程浏览器请求。 但是，请注意，在 Global.asax 中定义的其他路由可能匹配同一请求。 请务必了解约束可以防止特定路由匹配请求以及在 Global.asax 文件中定义并不是所有路由。

请注意，默认路由已被注释掉列出 2 中的 Global.asax 文件。 如果包括默认路由，默认路由将匹配的管理控制器的请求。 在这种情况下，远程用户仍可以调用的管理控制器的操作，即使它们的请求不匹配的管理路由。

>[!div class="step-by-step"]
[上一页](creating-a-route-constraint-cs.md)
[下一页](asp-net-mvc-controller-overview-vb.md)
