---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: "动画根据条件 (VB) |Microsoft 文档"
author: wenz
description: "ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 动画是否..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: cc8600f33f9c27e1045f5083a126b9d2d1e90303
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="animation-depending-on-a-condition-vb"></a>动画根据条件 (VB)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip)或[下载 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)

> ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 动画是否运行或不可以还依赖于某些 JavaScript 代码形式的条件。


## <a name="overview"></a>概述

ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 动画是否运行或不可以还依赖于某些 JavaScript 代码形式的条件。

## <a name="steps"></a>步骤

首先，包括`ScriptManager`在页中; 然后，ASP.NET AJAX 库加载后，使其可以使用该控件工具包：

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

动画将应用于如下所示的文本的一个面板中：

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

在关联 CSS 类中的面板，定义一种很好的背景色，并且还设置面板的宽度固定:

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

然后，将添加`AnimationExtender`到页中，提供`ID`、`TargetControlID`属性和强制性`runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

在`<Animations>`节点，请使用`<OnLoad>`以运行动画，一旦页已完全加载。 而不是一个正则动画，`<Condition>`元素派上用场。 作为值提供的 JavaScript 代码`ConditionScript`在运行时执行属性。 如果其计算结果为 true 时，动画将执行，否则不。 以下标记提供了两个动画，每个要在 50%的情况下在随机时执行。 因为可能只在一个动画`<OnLoad>`，这两个`<Condition>`一起使用来联接动画`<Sequence>`元素：

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

请注意，小于号 (`<`) 中`ConditionScript`属性必须是转义 （）。 当你运行此脚本，没有动画运行或这两个空格，或同时执行操作。


[![面板淡出而不调整大小，因此第二个动画运行，第一个未](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

面板淡出而不调整大小，因此第二个动画运行，第一个未 ([单击以查看实际尺寸的图像](animation-depending-on-a-condition-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一页](executing-several-animations-after-each-other-vb.md)
[下一页](picking-one-animation-out-of-a-list-vb.md)
