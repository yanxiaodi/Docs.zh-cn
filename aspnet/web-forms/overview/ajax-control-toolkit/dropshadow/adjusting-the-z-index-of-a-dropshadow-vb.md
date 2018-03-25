---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: "调整 Z-index DropShadow (VB) |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的 DropShadow 控件扩展带投影一个面板。 但是此卷影有时与其他控件，以便 insta 冲突..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: 844ea00c2ef1c974aa72c7dd627819b0429d612e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a>调整 Z-index DropShadow (VB)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)

> AJAX 控件工具包中的 DropShadow 控件扩展带投影一个面板。 但是此卷影有时与其他控件，例如 ASP.NET 菜单控件冲突。 当一个菜单项将弹出，似乎后面投影。


## <a name="overview"></a>概述

AJAX 控件工具包中的 DropShadow 控件扩展带投影一个面板。 但是此卷影有时与其他控件，例如 ASP.NET 菜单控件冲突。 当一个菜单项将弹出，似乎后面投影。

## <a name="steps"></a>步骤

与面板本身，以便面板包含足够多的效果是可见的文本包含足够多的文本，代码便会开始：

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

直接前后放置另一个面板`panelShadow`面板。 它包含与水平方向一个菜单，以便通过显示菜单项 (确切地说： 下)`dropShadow`面板):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

然后，`DropShadowExtender`加以扩展`panelShadow`面板与投影效果：

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

最后，ASP.NET AJAX`ScriptManager`控件使控件工具包工作：

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

运行此脚本时的菜单项将显示在面板的下方。 但是菜单使用 CSS 类`panel`其中只需定义以下两项操作，以使显示于另一个面板的元素：

- 相对定位
- 正 z 索引

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

然后，`DropShadowExtender`控件不会与菜单控件不再冲突。


[![之前： 菜单项不可见](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)

之前： 菜单项是不可见 ([单击以查看实际尺寸的图像](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))


[![菜单项的显示之后：](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)

之后： 显示菜单项 ([单击以查看实际尺寸的图像](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))

>[!div class="step-by-step"]
[上一页](manipulating-dropshadow-properties-from-client-code-cs.md)
[下一页](manipulating-dropshadow-properties-from-client-code-vb.md)
