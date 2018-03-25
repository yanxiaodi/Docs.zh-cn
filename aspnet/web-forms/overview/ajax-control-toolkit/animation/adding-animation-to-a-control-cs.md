---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: "将动画添加到控件 (C#) |Microsoft 文档"
author: wenz
description: "ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 本教程演示如何..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7016ae3c92c665136579a8588818e6e4179a102a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="adding-animation-to-a-control-c"></a>将动画添加到控件 (C#)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip)或[下载 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)

> ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 本教程演示如何设置此类动画。


## <a name="overview"></a>概述

ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 本教程演示如何设置此类动画。

## <a name="steps"></a>步骤

第一步是像往常一样包括`ScriptManager`在页中，以便加载 ASP.NET AJAX 库，并可以使用该控件工具包：

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

在此方案中动画将应用于如下所示的文本的一个面板中：

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

面板关联的 CSS 类定义的背景颜色和宽度：

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

接下来，我们需要`AnimationExtender`。 后提供`ID`和常用`runat="server"`、`TargetControlID`属性必须设置为该控件，要进行动画处理，在本例中为面板：

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

使用遗憾的是当前不完全支持 Visual Studio IntelliSense 的 XML 语法以声明方式，应用整个动画。 根节点是`<Animations>;`在此节点中，这将决定当动画 take(s) 位置允许多个事件：

- `OnClick`（鼠标单击）
- `OnHoverOut`（当鼠标离开控件）
- `OnHoverOver`(当鼠标悬停在控件上，停止`OnHoverOut`动画)
- `OnLoad`（如果已加载页）
- `OnMouseOut`（当鼠标离开控件）
- `OnMouseOver`(当鼠标悬停在控件上，不停止`OnMouseOut`动画)

框架附带的动画，每个由其自己的 XML 元素表示一组。 下面是所选内容：

- `<Color>`（更改一种颜色）
- `<FadeIn>`（淡入淡出中）
- `<FadeOut>`（淡出）
- `<Property>`（更改控件的属性）
- `<Pulse>`(pulsating)
- `<Resize>`（更改大小）
- `<Scale>`（按比例的大小发生更改）

在此示例中，面板应淡出。动画应该采取 1.5 秒 (`Duration`属性)，显示 24 （动画步骤） 每秒帧数 (`Fps` attributs)。 下面是完整标记`AnimationExtender`控件：

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

运行此脚本时，面板会显示，并以一个半秒为单位淡出。


[![面板淡出](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

面板淡出 ([单击以查看实际尺寸的图像](adding-animation-to-a-control-cs/_static/image3.png))

>[!div class="step-by-step"]
[下一篇](executing-several-animations-at-the-same-time-cs.md)
