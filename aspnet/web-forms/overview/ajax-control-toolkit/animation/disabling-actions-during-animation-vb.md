---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: "动画 (VB) 期间禁用操作 |Microsoft 文档"
author: wenz
description: "ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 它还支持操作..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 0f68d27f60e9b224a7cc0d598962553afeb3f28f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="disabling-actions-during-animation-vb"></a>禁用操作期间动画 (VB)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip)或[下载 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 它还支持操作，例如，鼠标单击。 但是当鼠标单击启动一个动画，这是需要在动画期间禁用鼠标单击。


## <a name="overview"></a>概述

ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 它还支持操作，例如，鼠标单击。 但是当鼠标单击启动一个动画，这是需要在动画期间禁用鼠标单击。

## <a name="steps"></a>步骤

首先，包括`ScriptManager`在页中; 然后，ASP.NET AJAX 库加载后，使其可以使用该控件工具包：

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

动画将应用到一个 HTML 按钮如下：

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

请注意，由于我们不希望该按钮以创建回发; 而不是 Web 控件使用 HTML 控件它只应启动我们的客户端动画。

然后，将添加`AnimationExtender`到页中，提供`ID`、`TargetControlID`属性和强制性`runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

在`<Animations>`节点，`<OnClick>`是正确的元素，用于处理鼠标单击。 但是，无法在动画期间单击该按钮。 `<EnableAction>`元素可以负责。 设置`Enabled="false"`作为一部分动画禁用的按钮。 因为我们要使用多个 （禁用的按钮和实际的动画） 的单个动画`<Parallel>`元素必须为要在其中粘附组合在一起成为一个单个的动画。 下面是完整标记`AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

此外可以在动画，使用下面的 XML 元素列表的末尾后重新启用到按钮：

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

但是在给定方案这将是没有用处，因为按钮淡出，且不在动画末尾可见。


[![动画运行时，就会立即将禁用的按钮](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

动画运行时，就会立即将禁用的按钮 ([单击以查看实际尺寸的图像](disabling-actions-during-animation-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一页](animating-in-response-to-user-interaction-vb.md)
[下一页](triggering-an-animation-in-another-control-vb.md)
