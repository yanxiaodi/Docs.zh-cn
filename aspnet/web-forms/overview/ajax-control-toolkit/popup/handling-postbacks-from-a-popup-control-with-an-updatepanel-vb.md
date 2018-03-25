---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: "处理从具有 UpdatePanel (VB) 的弹出窗口控件的回发 |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的 PopupControl 扩展程序提供的其他任何控件被激活时触发一个弹出窗口的简单办法。 必须格外小心，要执行..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 4445437f25925429d240b7fe2d019855afc52fe0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a>处理从具有 UpdatePanel (VB) 的弹出窗口控件的回发
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip)或[下载 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)

> AJAX 控件工具包中的 PopupControl 扩展程序提供的其他任何控件被激活时触发一个弹出窗口的简单办法。 必须格外小心，要执行此类弹出窗口内的回发发生时。


## <a name="overview"></a>概述

AJAX 控件工具包中的 PopupControl 扩展程序提供的其他任何控件被激活时触发一个弹出窗口的简单办法。 必须格外小心，要执行此类弹出窗口内的回发发生时。

## <a name="steps"></a>步骤

使用时`PopupControl`回发时，`UpdatePanel`可以防止引起的回发页面刷新。 下面的标记定义几个重要元素：

- A`ScriptManager`控制以便 ASP.NET AJAX 控件工具包能起作用
- 两个`TextBox`控件就会触发一个弹出窗口
- A`Panel`将充当弹出窗口的控件
- 在面板中，`Calendar`中嵌入控件`UpdatePanel`控件
- 两个`PopupControlExtender`将该面板分配到的文本框中的控件

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

请注意，`OnSelectionChanged`属性`Calendar`控件设置。 因此，当用户选择在日历中的日期，产生的回发和服务器端方法`c1_SelectionChanged()`执行。 在该方法中，必须检索并回写到文本框中的当前日期。

如下所示的语法是： 首先，代理对象`PopupControlExtender`在页面上，必须生成。 ASP.NET AJAX 控件工具包提供`GetProxyForCurrentPopup()`方法。 此方法返回的对象支持`Commit()`将值发送回控件触发的弹出窗口 （不触发方法调用的控件 ！） 的方法。 下面的代码提供的自变量作为所选的日期`Commit()`方法，从而导致代码以便将所选的日期写回到文本框中：

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

现在只要你单击日历日期，所选的日期将显示在关联的文本框中，创建日期选取器控件可以当前找到许多网站上。


[![当用户单击的文本框，会显示日历](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)

当用户单击的文本框，会显示日历 ([单击以查看实际尺寸的图像](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))


[![单击在日期将其放在文本框中](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)

单击在日期将其放在文本框中 ([单击以查看实际尺寸的图像](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))

>[!div class="step-by-step"]
[上一页](using-multiple-popup-controls-vb.md)
[下一页](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)
