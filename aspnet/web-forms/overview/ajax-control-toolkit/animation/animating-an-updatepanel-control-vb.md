---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: "对进行动画处理 UpdatePanel 控件 (VB) |Microsoft 文档"
author: wenz
description: "ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 内容..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 0d1056fc798e22254e94e5cad54436576a297f7d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="animating-an-updatepanel-control-vb"></a>对进行动画处理 UpdatePanel 控件 (VB)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)

> ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 UpdatePanel 的内容，对于一个特殊的扩展程序添加存在很大程度依赖于动画 framework: UpdatePanelAnimation。 本教程演示如何为 UpdatePanel 设置此类动画。


## <a name="overview"></a>概述

ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 内容`UpdatePanel`，特殊的扩展程序存在很大程度依赖于动画 framework: `UpdatePanelAnimation`。 本教程演示如何设置此类动画`UpdatePanel`。

## <a name="steps"></a>步骤

第一步是像往常一样包括`ScriptManager`在页中，以便加载 ASP.NET AJAX 库，并可以使用该控件工具包：

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

在此方案中动画将应用到 ASP.NET`Wizard`驻留在的 web 控件`UpdatePanel`。 三个 （任意） 的步骤提供足够的选项来触发回发：

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

所需的标记`UpdatePanelAnimationExtender`控件是非常类似于用于标记`AnimationExtender`。 在`TargetControlID`我们提供的属性`ID`的`UpdatePanel`要进行动画处理; 在`UpdatePanelAnimationExtender`控件，`<Animations>`元素包含动画的 XML 标记。 但是没有一个区别： 事件和事件处理程序是有限的相对于`AnimationExtender`。 有关`UpdatePanels`，只有两个其中存在：

- `<OnUpdated>`UpdatePanel 何时已更新
- `<OnUpdating>`UpdatePanel 启动更新

在此方案中，新内容的`UpdatePanel`（之后回发） 应淡入。 这是为此时必需的标记：

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

现在每次回发发生 UpdatePanel 中时，面板中的新内容淡入平稳。


[![下一步的向导步骤淡入](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)

下一步的向导步骤淡入 ([单击以查看实际尺寸的图像](animating-an-updatepanel-control-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一页](changing-an-animation-using-client-side-code-vb.md)
[下一页](dynamically-controlling-updatepanel-animations-vb.md)
