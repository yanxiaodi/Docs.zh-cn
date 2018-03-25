---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: "触发动画中另一个控件 (VB) |Microsoft 文档"
author: wenz
description: "ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 通常情况下，启动..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ce1d29cbd06ef8a470780ff4c7bda8039575d59f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="triggering-an-animation-in-another-control-vb"></a>触发动画中另一个控件 (VB)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip)或[下载 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)

> ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 通常情况下，启动动画由触发与同一个控件中的用户交互。 但是也可能与一个控件，然后动画交互另一个控件。


## <a name="overview"></a>概述

ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 通常情况下，启动动画由触发与同一个控件中的用户交互。 但是也可能与一个控件，然后动画交互另一个控件。

## <a name="steps"></a>步骤

首先，包括`ScriptManager`在页中; 然后，ASP.NET AJAX 库加载后，使其可以使用该控件工具包：

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

动画将应用于如下所示的文本的一个面板中：

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

在关联 CSS 类中的面板，定义一种很好的背景色，并且还设置面板的宽度固定:

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

若要启动动画面板，请使用 HTML 按钮。 请注意，`<input type="button" />`通过 favoured`<asp:Button />`因为我们不希望回发，当用户单击该按钮。

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

然后，将添加`AnimationExtender`到页中，提供`ID`、`TargetControlID`属性和强制性`runat="server"`。 务必要设置`TargetControlID`按钮 （触发动画元素） 的 id 不为面板 （正在进行动画处理的元素） 的 ID

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

在`<Animations>`节点，照常位置动画。 为了使其更改面板，而不是按钮，请将设置`AnimationTarget`中每个动画元素的属性`AnimationExtender`。 值`AnimationTarget`当然是面板的 ID。 这样一来，动画发生的面板中，不与触发的按钮。 下面是`AnimationExtender`这种情况下的标记：

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

请注意在其中的单个动画显示的特殊顺序。 首先，一旦动画运行，则会停用按钮。 由于没有任何`AnimationTarget`属性中`<EnableAction>`元素，此动画应用于源的控件： 按钮。 下面的两个动画步骤应执行 parallelly (`<Parallel>`元素)。 两个具有其`AnimationTarget`属性设置为`"Panel1"`，因此对面板中，而不是按钮进行动画处理。


[![按钮上的鼠标单击启动面板动画](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)

按钮上的鼠标单击启动面板动画 ([单击以查看实际尺寸的图像](triggering-an-animation-in-another-control-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一页](disabling-actions-during-animation-vb.md)
[下一页](modifying-animations-from-the-server-side-vb.md)
