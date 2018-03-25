---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: "选取列表 (C#) 之中的一个动画 |Microsoft 文档"
author: wenz
description: "ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 框架还允许..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: a24c4ffe49df4eb663f833eb1814f7cbcf15e07e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="picking-one-animation-out-of-a-list-c"></a>选取列表 (C#) 之中的一个动画
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip)或[下载 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)

> ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 该框架还允许程序员也要选取列表之中的一个动画的动画，具体取决于一些 JavaScript 代码的计算。


## <a name="overview"></a>概述

ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 该框架还允许程序员也要选取列表之中的一个动画的动画，具体取决于一些 JavaScript 代码的计算。

## <a name="steps"></a>步骤

首先，包括`ScriptManager`在页中; 然后，ASP.NET AJAX 库加载后，使其可以使用该控件工具包：

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

动画将应用于如下所示的文本的一个面板中：

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

在关联 CSS 类中的面板，定义一种很好的背景色，并且还设置面板的宽度固定:

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

然后，将添加`AnimationExtender`到页中，提供`ID`、`TargetControlID`属性和强制性`runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

在`<Animations>`节点，请使用`<OnLoad>`以运行动画，一旦页已完全加载。 而不是一个正则动画，`<Case>`元素派上用场。 计算其 SelectScript 属性的值;返回值必须是数字。 根据此数量，其中一个内子动画&lt;用例&gt;执行。 例如，如果 SelectScript 计算结果为 2，该控件工具包将运行中的第三个动画&lt;用例&gt;（计数 0 开始）。

下面的标记定义三个子动画： 调整大小的宽度、 高度，调整大小和淡出。JavaScript 代码 (`Math.floor(3 * Math.random())`) 然后选取介于 0 和 2，以便的三个动画之一运行：

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]


[![可能的三个动画之一: 面板获取更宽](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)

可能的三个动画之一: 面板获取更宽 ([单击以查看实际尺寸的图像](picking-one-animation-out-of-a-list-cs/_static/image3.png))

>[!div class="step-by-step"]
[上一页](animation-depending-on-a-condition-cs.md)
[下一页](animating-in-response-to-user-interaction-cs.md)
