---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: "使用多个弹出控件 (C#) |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的 PopupControl 扩展程序提供的其他任何控件被激活时触发一个弹出窗口的简单办法。 它还可使用 m..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: d8c9742bb39b655115cb1862ff6e867989e63665
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="using-multiple-popup-controls-c"></a>使用多个弹出控件 (C#)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip)或[下载 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)

> AJAX 控件工具包中的 PopupControl 扩展程序提供的其他任何控件被激活时触发一个弹出窗口的简单办法。 还有可能要使用多个在一页上的弹出窗口控件。


## <a name="overview"></a>概述

AJAX 控件工具包中的 PopupControl 扩展程序提供的其他任何控件被激活时触发一个弹出窗口的简单办法。 还有可能要使用多个在一页上的弹出窗口控件。

## <a name="steps"></a>步骤

为了激活 ASP.NET AJAX 和控件工具包中的功能`ScriptManager`必须在页面上任意位置放置控件 (但内`<form>`元素):

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

接下来，添加一个面板，它可作为弹出窗口。 在当前的方案中，面板包含`Calendar`控件。 为了避免引起的日历的回发页面将刷新，面板将内`UpdatePanel`控件：

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

此页还包含两个文本框。 对于每个文本框中，一旦激活文本框中应显示日历弹出窗口。

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

现在扩展与两个文本框中的每个`PopupControlExtender`。 `TargetControlID`属性提供绑定到扩展程序控件的 ID。 `PopupControlID`属性包含弹出面板中的 ID。 在这种情况下，这两个扩展器显示相同的面板中，但不同的面板也有可能。

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

现在只要你单击某个文本字段中，会显示日历的字段下允许您选择一个日期。 （在选定的日期取回向文本框中介绍了在不同的教程。）


[![当用户单击的文本框，会显示日历](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)

当用户单击的文本框，会显示日历 ([单击以查看实际尺寸的图像](using-multiple-popup-controls-cs/_static/image3.png))

>[!div class="step-by-step"]
[下一篇](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
