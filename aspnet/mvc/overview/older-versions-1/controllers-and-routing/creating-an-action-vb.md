---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: "创建操作 (VB) |Microsoft 文档"
author: microsoft
description: "了解如何向 ASP.NET MVC 控制器中添加新操作。 了解有关该方法将被某项操作的要求。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: 3d1d355599c17df597f9c08d9d7f129fffc1a2e4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="creating-an-action-vb"></a>创建操作 (VB)
====================
通过[Microsoft](https://github.com/microsoft)

> 了解如何向 ASP.NET MVC 控制器中添加新操作。 了解有关该方法将被某项操作的要求。


本教程旨在说明如何创建新的控制器操作。 你将了解操作方法的要求。 你还了解了如何防止将方法公开为一个操作。

## <a name="adding-an-action-to-a-controller"></a>将操作添加到控制器

通过将新方法添加到控制器可将新的操作添加到控制器中。 例如，列表 1 中的控制器包含名为 index （） 的操作和名为 SayHello() 操作。 作为操作公开这两种方法。

**列表 1-Controllers\HomeController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

以便公开到为操作 universe，方法必须满足某些要求：

- 该方法必须是公共的。
- 该方法不能为静态方法。
- 该方法不能为扩展方法。
- 该方法不能为构造函数、 getter 或 setter。
- 该方法不能有开放式泛型类型。
- 该方法不是控制器的基本类的方法。
- 方法不能包含**ref**或**出**参数。

请注意，有的控制器操作的返回类型没有限制。 控制器操作可以返回一个字符串，DateTime 或 void 随机类的实例。 ASP.NET MVC framework 将转换并不是转换为字符串操作结果的任何返回类型，并呈现到浏览器的字符串。

当添加不违反到控制器的这些要求的任何方法时，该方法将公开为控制器操作。 此处必须小心。 连接到 Internet 的任何人都可以调用的控制器操作。 例如，不要创建 DeleteMyWebsite() 控制器操作。

## <a name="preventing-a-public-method-from-being-invoked"></a>防止调用公共方法

如果你需要创建一个公共方法中了控制器类并且你不想要公开的控制器操作作为方法，则可以防止使用调用该方法&lt;NonAction&gt;属性。 例如，清单 2 中的控制器包含一个名为 CompanySecrets() 用修饰的公共方法&lt;NonAction&gt;属性。

**列出 2-Controllers\WorkController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

如果你尝试通过在你的浏览器的地址栏中键入 /Work/CompanySecrets 调用 CompanySecrets() 控制器操作然后将图 1 中获得的错误消息。


[![调用 NonAction 方法](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**图 01**： 调用 NonAction 方法 ([单击以查看实际尺寸的图像](creating-an-action-vb/_static/image2.png))

>[!div class="step-by-step"]
[上一页](creating-a-controller-vb.md)
[下一页](aspnet-mvc-controllers-overview-cs.md)
