---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: "在同一时间 (VB) 中执行几个动画 |Microsoft 文档"
author: wenz
description: "ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 它允许运行跌落造成的严重..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: 8461f5ea303a9e1166f694d4039d4c1aedd1caa8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="executing-several-animations-at-the-same-time-vb"></a>在同一时间 (VB) 中执行几个动画
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip)或[下载 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)

> ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 它允许以并行方式运行多个动画。


## <a name="overview"></a>概述

ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 它允许以并行方式运行多个动画。

## <a name="steps"></a>步骤

首先，包括`ScriptManager`在页中; 然后，ASP.NET AJAX 库加载后，使其可以使用该控件工具包：

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

动画将应用于如下所示的文本的一个面板中：

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

在关联 CSS 类中的面板，定义一种很好的背景色，并且还设置面板的宽度固定:

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

然后，将添加`AnimationExtender`到页中，提供`ID`、`TargetControlID`属性和强制性`runat="server"`:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

在`<Animations>`节点，请使用`<OnLoad>`以运行动画，一旦页已完全加载。 通常情况下，`<OnLoad>`仅接受一个动画。 动画框架允许你加入到一个使用多个动画`<Parallel>`元素。 中的所有动画`<Parallel>`在同一时间执行。

下面是有关可能的标记`AnimationExtender`控件，淡出以及调整面板大小在同一时间：

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

和确实： 运行此脚本时，面板会显示，然后调整大小时 (超过 threefolding 其宽度和 halfing 窗体的高度) 和在同一时间淡出。


[![面板是淡出和调整大小 （包括其内容，感谢浏览器的呈现引擎）](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)

面板是淡出和调整大小 （包括其内容，浏览器的呈现引擎感谢） ([单击以查看实际尺寸的图像](executing-several-animations-at-the-same-time-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一页](adding-animation-to-a-control-vb.md)
[下一页](executing-several-animations-after-each-other-vb.md)
