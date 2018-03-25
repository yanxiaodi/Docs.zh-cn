---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: "执行动画使用客户端代码 (VB) |Microsoft 文档"
author: wenz
description: "ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 动画执行中..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: c97ce87addd566ed1ba63ccdb81206c6449f49a2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="executing-animations-using-client-side-code-vb"></a>执行动画使用客户端代码 (VB)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip)或[下载 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)

> ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 此外可能使用自定义客户端 JavaScript 代码会触发动画执行。


## <a name="overview"></a>概述

ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 此外可能使用自定义客户端 JavaScript 代码会触发动画执行。

## <a name="steps"></a>步骤

首先，包括`ScriptManager`在页中; 然后，ASP.NET AJAX 库加载后，使其可以使用该控件工具包：

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

动画将应用于如下所示的文本的一个面板中：

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

在关联 CSS 类中的面板，定义一种很好的背景色，并且还设置面板的宽度固定:

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

然后，将添加`AnimationExtender`到页中，提供`ID`、`TargetControlID`属性和强制性`runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

在`<Animations>`节点，请使用`<OnClick>`运行动画一次用户单击面板上。 添加两个动画 parallelly 执行：

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

为了演示，此动画 （和使用控件工具包创建的任何其他动画） 将被执行后页将运行使用 JavaScript 代码。 我们首先需要访问`AnimationExtender`控件。 ASP.NET AJAX 库提供了`$find()`对此任务的函数：

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

`AnimationExtender`控件可公开一个丰富 API，包括使用名称相同的 XML 标记中使用的事件处理程序方法： `OnClick()`， `OnLoad()`，依次类推。 例如，调用`OnClick()`方法执行内的动画`<OnClick>`元素`AnimationExtender`控件：

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

下面是模拟的面板上，单击页面完全加载后的完整客户端 JavaScript 代码，请注意，`pageLoad()`它由调用 ASP.NET AJAX 一次页使用函数名称和所有包含库已的 JavaScript加载。

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


[![动画运行立即，而无需单击鼠标](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)

动画会立即，运行没有鼠标单击 ([单击以查看实际尺寸的图像](executing-animations-using-client-side-code-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一页](modifying-animations-from-the-server-side-vb.md)
[下一页](changing-an-animation-using-client-side-code-vb.md)
