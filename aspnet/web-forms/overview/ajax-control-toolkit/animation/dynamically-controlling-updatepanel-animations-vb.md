---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: "动态控制 UpdatePanel 动画 (VB) |Microsoft 文档"
author: wenz
description: "ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 内容..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: 75b1f169724d8d3f8bcbc46198d320eeb8e7260c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a>动态控制 UpdatePanel 动画 (VB)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)

> ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 UpdatePanel 的内容，对于一个特殊的扩展程序添加存在很大程度依赖于动画 framework: UpdatePanelAnimation。 它还可以与 UpdatePanel 触发器协作运行。


## <a name="overview"></a>概述

ASP.NET AJAX 控件工具包中的动画控件不只是一个控件，但一个整个框架，以向控件添加动画。 内容`UpdatePanel`，特殊的扩展程序存在很大程度依赖于动画 framework: `UpdatePanelAnimation`。 它还可以一起使用`UpdatePanel`触发器。

## <a name="steps"></a>步骤

第一步是像往常一样包括`ScriptManager`在页中，以便加载 ASP.NET AJAX 库，并可以使用该控件工具包：


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

在此方案中动画将应用于当前时间的显示。 此信息可写入标签使用`Page_Load()`方法，或 （为简单起见） 使用下面的内联代码：


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

此外，创建一个按钮以触发更新时间：


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

然后将此代码放入`<ContentTemplate>`部分`UpdatePanel`元素。 面板的`UpdateMode`属性必须设置为`"Conditional"`，因为仅触发器可能会更新面板中的内容。 在`<Triggers>`部分`UpdatePanel`，创建并绑定到一个异步回发的触发器`Click`该按钮的事件。 因此，如果用户单击按钮，`UpdatePanel`进行刷新。 下面是有关标记`UpdatePanel`控件：


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

最后，`UpdatePanelAnimationExtender`必须配置： 设置`TargetControlID`属性面板的 ID 和定义中扩展器动画。 在使淡出意义上，创建好 visual 主要侧重于更新的时间。 扩展程序标记可能如下所示：


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

在浏览器中运行该文件。 只要你单击按钮时，当前时间是显示在面板中，始终为 1 秒的持续时间内淡入。


[![当前时间淡入淡出](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)

当前时间淡入 ([单击以查看实际尺寸的图像](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一篇](animating-an-updatepanel-control-vb.md)
