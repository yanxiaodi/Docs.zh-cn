---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: "启动从服务器代码 (VB) 模式弹出窗口 |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的 ModalPopup 控制提供一种简单的方法，以创建模式的弹出项，使用客户端的方式。 但是，某些情况下需要该 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: c4bcf3e32b3aa91bb73e01296bc1fc1a2e064711
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a>启动从服务器代码 (VB) 模式弹出窗口
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)

> AJAX 控件工具包中的 ModalPopup 控制提供一种简单的方法，以创建模式的弹出项，使用客户端的方式。 但是一些方案要求在服务器端时触发的模式的弹出窗口打开。


## <a name="overview"></a>概述

AJAX 控件工具包中的 ModalPopup 控制提供一种简单的方法，以创建模式的弹出项，使用客户端的方式。 但是一些方案要求在服务器端时触发的模式的弹出窗口打开。

## <a name="steps"></a>步骤

首先，ASP.NET 按钮 web 控件需要演示 ModalPopup 控制工作原理。 添加此类中的某个按钮&lt;窗体&gt;在新页上的元素：

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

然后，你需要针对你想要创建弹出项标记。 它定义为`<asp:Panel>`控制并确保它包括一个按钮控件。 ModalPopup 控件提供功能，以便让此类按钮关闭弹出窗口;否则，没有让它消失的轻松方法。

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

接下来向页面添加 ASP.NET AJAX 工具包中的 ModalPopup 控件。 设置了加载控件的按钮，这样就会消失，按钮和实际的弹出项的 ID 属性。

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

与基于 ASP.NET AJAX; 的所有 web 页脚本管理器需要加载另一个目标浏览器的必要 JavaScript 库：

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

在浏览器中运行示例。 当你单击按钮时，将显示模式的弹出窗口。 若要实现使用服务器端代码相同的效果，新按钮是必需的：

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

如你所见，单击按钮生成回发，并执行`ServerButton_Click()`服务器上的方法。 在此方法，调用 JavaScript 函数`launchModal()`执行要准确，JavaScript 函数将执行加载页面后：

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

作业`launchModal()`是显示 ModalPopup。 `launchModal()`加载完整的 HTML 页后，将执行函数。 在该时刻，但是，ASP.NET AJAX 框架尚未完全加载。 因此，`launchModal()`函数只会将设置 ModalPopup 控件都必须更高版本显示的变量：

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

`pageLoad()` JavaScript 函数是 ASP.NET AJAX 已完全加载后执行的特殊函数。 因此我们将代码添加到此函数可显示 ModalPopup 控件，但仅当`launchModal()`之前已调用：

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

`$find()`函数正在寻找的页上的已命名元素，并需要服务器端 ID 作为参数。 因此，`$find("mpe")`返回的客户端表示形式 ModalPopup 控件; 其`show()`方法允许显示弹出窗口。


[![模式的弹出窗口出现时的任一按钮单击](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)

模式的弹出窗口出现时的任一按钮单击 ([单击以查看实际尺寸的图像](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一页](positioning-a-modalpopup-cs.md)
[下一页](using-modalpopup-with-a-repeater-control-vb.md)
