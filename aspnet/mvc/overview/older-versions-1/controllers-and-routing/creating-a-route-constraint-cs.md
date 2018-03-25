---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: "创建路由约束 (C#) |Microsoft 文档"
author: StephenWalther
description: "在本教程中，Stephen Walther 演示如何控制如何浏览器请求匹配路由通过使用正则表达式中创建路由约束。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: ee83a134dcbdd1abfb296f3126a64c7d4ebab7f5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-route-constraint-c"></a>创建路由约束 (C#)
====================
通过[Stephen Walther](https://github.com/StephenWalther)

> 在本教程中，Stephen Walther 演示如何控制如何浏览器请求匹配路由通过使用正则表达式中创建路由约束。


使用路由约束来限制相匹配的特定路由的浏览器请求。 正则表达式可用于指定的路由约束。

例如，假设你已经定义的路由列表 1 中在 Global.asax 文件。

**列表 1-Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

列表 1 包含名为产品的路由。 你可以使用产品路由将浏览器请求映射到包含在清单 2 ProductController。

**列出 2-Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

请注意，在产品控制器公开的 Details() 操作接受一个名为产品 id 的单个参数。 此参数是一个整数参数。

列表 1 中定义的路由将匹配任何以下 Url:

- / 产品/23
- / 产品/7

遗憾的是，路由还将匹配的以下 Url:

- / 产品/被废弃
- / 产品/apple

由于 Details() 操作需要一个整数参数，则发出请求，其中包含整数值以外的会导致错误。 例如，如果你的浏览器中键入 URL /Product/apple 然后会在图 1 中出现错误页。


[![新项目对话框](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)

**图 01**： 看到分解的页 ([单击以查看实际尺寸的图像](creating-a-route-constraint-cs/_static/image2.png))


你确实希望做什么是仅与包含正确的整数 productId 的 Url 相匹配。 你可以使用约束时定义路由来限制与路由匹配的 Url。 列出 3 中的已修改的产品路由包含仅与整数相匹配的正则表达式约束。

**列出 3-Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

正则表达式 \d+ 匹配一个或多个整数。 此约束会导致产品路由，以匹配以下 Url:

- / 产品/3
- / 产品/8999

但不是以下 Url:

- / 产品/apple
- / 产品

- 这些浏览器请求将由另一个路由或是否存在任何匹配的路由，*找不到资源*将返回错误。

>[!div class="step-by-step"]
[上一页](creating-custom-routes-cs.md)
[下一页](creating-a-custom-route-constraint-cs.md)
