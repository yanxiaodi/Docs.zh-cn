---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: "更改动画使用客户端代码 (VB) |Microsoft 文档"
author: wenz
description: "ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 此外可以动画..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 83d1a21fba37d8807be467d02b5550dc7d096e6c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="changing-an-animation-using-client-side-code-vb"></a>更改动画使用客户端代码 (VB)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip)或[下载 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)

> ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 也可以使用自定义客户端 JavaScript 代码更改动画。


## <a name="overview"></a>概述

ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 也可以使用自定义客户端 JavaScript 代码更改动画。

## <a name="steps"></a>步骤

首先，包括`ScriptManager`在页中; 然后，ASP.NET AJAX 库加载后，使其可以使用该控件工具包：

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

动画将应用于如下所示的文本的一个面板中：

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

在关联 CSS 类中的面板，定义一种很好的背景色，并且还设置面板的宽度固定:

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

实际动画是由 HTML 按钮启动：

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

然后，将添加`AnimationExtender`到页中，提供`ID`、`TargetControlID`属性和强制性`runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

请注意，没有任何`<Animations>`中的节点`AnimationExtender`控件。 自定义 JavaScript 代码用于提供要用于该控件的动画。

服务器 API 与`AnimationExtender`，没有将动画尚未分配到扩展程序的轻松方法。 但是扩展程序确实公开了几种方法来读取和写入动画注册与各种事件 (`OnClick`， `OnLoad`，依次类推)。 下面是一些可能的恶意活动：

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

返回值的格式`get_*()`函数和自变量的格式`set_*()`函数是一个 JSON 字符串，提供的对象表示形式的 XML 标记是什么。 目前，无法将传递一个对象，但可以从给定的动画读取某个对象 (`get_OnXXXBehavior()`方法)。

下面是一个 JSON 字符串 (不带分隔引号和精美格式) 表示动画触发了按钮，但通过调整其大小时和淡出在同一时间对面板进行动画处理：

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

下面的 JavaScript 代码将分配到此 JSON descripting`OnClick`动画的当前扩展程序并运行它：

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


[![动画运行立即，无需单击鼠标 （，很少的标记）](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)

动画会立即，运行没有鼠标单击 （且很少的标记） ([单击以查看实际尺寸的图像](changing-an-animation-using-client-side-code-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一页](executing-animations-using-client-side-code-vb.md)
[下一页](animating-an-updatepanel-control-vb.md)
