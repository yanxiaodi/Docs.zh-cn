---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: "在每个其他 (C#) 后执行几个动画 |Microsoft 文档"
author: wenz
description: "ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 它允许运行跌落造成的严重..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 9d4322690132fe3829e3454f0aa7ff38acd8eb04
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="executing-several-animations-after-each-other-c"></a>在每个其他 (C#) 后执行几个动画
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip)或[下载 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)

> ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 它允许在其他后运行几个动画一个。


ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 它允许在其他后运行几个动画一个。

## <a name="steps"></a>步骤

首先，包括`ScriptManager`在页中; 然后，ASP.NET AJAX 库加载后，使其可以使用该控件工具包：

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

动画将应用于如下所示的文本的一个面板中：

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

在关联 CSS 类中的面板，定义一种很好的背景色，并且还设置面板的宽度固定:

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

然后，将添加`AnimationExtender`到页中，提供`ID`、`TargetControlID`属性和强制性`runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

在`<Animations>`节点，请使用`<OnLoad>`以运行动画，一旦页已完全加载。 通常情况下，`<OnLoad>`仅接受一个动画。 动画框架允许你加入到一个使用多个动画`<Sequence>`元素。 中的所有动画`<Sequence>`执行后另一个。 下面是有关可能的标记`AnimationExtender`控件，首先进行更宽的面板以及增大或减小其高度：

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

当你运行此脚本，面板中第一次获得更宽且然后较小。


[![首先增加了宽度](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)

第一次增加宽度 ([单击以查看实际尺寸的图像](executing-several-animations-after-each-other-cs/_static/image3.png))


[![然后减少高度](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)

然后减少高度 ([单击以查看实际尺寸的图像](executing-several-animations-after-each-other-cs/_static/image6.png))

>[!div class="step-by-step"]
[上一页](executing-several-animations-at-the-same-time-cs.md)
[下一页](animation-depending-on-a-condition-cs.md)
