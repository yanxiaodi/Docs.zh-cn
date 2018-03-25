---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: "定位 ModalPopup (VB) |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的 ModalPopup 控制提供一种简单的方法，以创建模式的弹出项，使用客户端的方式。 但是该控件不提供..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 4b9c0790d1696bcf478bcdea089d4d3d92450369
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="positioning-a-modalpopup-vb"></a>定位 ModalPopup (VB)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)

> AJAX 控件工具包中的 ModalPopup 控制提供一种简单的方法，以创建模式的弹出项，使用客户端的方式。 但是该控件不提供内置的功能，用于定位弹出窗口。


## <a name="overview"></a>概述

AJAX 控件工具包中的 ModalPopup 控制提供一种简单的方法，以创建模式的弹出项，使用客户端的方式。 但是该控件不提供内置的功能，用于定位弹出窗口。

## <a name="steps"></a>步骤

为了激活 ASP.NET AJAX 和控件工具包中的功能`ScriptManager`。 必须在页面上任意位置放置控件 (但内`<form>`元素):

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

接下来，添加一个面板，它可作为模式弹出窗口。 一个按钮用于关闭弹出窗口：

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

每当显示弹出窗口，它应位于在页中的某个特定位置处。 对于此任务，创建一个客户端 JavaScript 函数。 它首先尝试访问面板。 如果成功，使用 CSS 和 JavaScript （更改将在弹出窗口的位置） 设置面板的位置。 但是，`ModalPopupExtender`还会控制尝试定位弹出窗口。 因此，JavaScript 代码重复定位弹出窗口中，每隔 10 秒。

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

如你所见的的返回值`setTimeout()`JavaScript 方法将保存在全局变量。 这样就可以停止按需、 弹出窗口的重复定位使用`clearTimeout()`方法：

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

现在剩下所要做的所有是使浏览器调用这些函数可能的情况。 `movePanel()`触发面板中单击该按钮时，必须调用 JavaScript 函数：

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

与`stopMoving()`函数派上用场时，就可以在关闭弹出窗口中触发`ModalPopupExtender`控件：

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


[![模式的弹出窗口出现在指定的位置](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)

模式的弹出窗口出现在指定的位置 ([单击以查看实际尺寸的图像](positioning-a-modalpopup-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一篇](handling-postbacks-from-a-modalpopup-vb.md)
