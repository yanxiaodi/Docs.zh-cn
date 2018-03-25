---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: "处理从没有 UpdatePanel (C#) 的弹出窗口控件的回发 |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的 PopupControl 扩展程序提供的其他任何控件被激活时触发一个弹出窗口的简单办法。 当回发发生在 su..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: df43052950b6186908fe1baf04808f40cb926f69
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a>处理从没有 UpdatePanel (C#) 的弹出窗口控件的回发
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip)或[下载 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)

> AJAX 控件工具包中的 PopupControl 扩展程序提供的其他任何控件被激活时触发一个弹出窗口的简单办法。 此类面板中的回发发生并页上有多个面板时很难确定被单击的面板。


## <a name="overview"></a>概述

AJAX 控件工具包中的 PopupControl 扩展程序提供的其他任何控件被激活时触发一个弹出窗口的简单办法。 此类面板中的回发发生并页上有多个面板时很难确定被单击的面板。

## <a name="steps"></a>步骤

使用时`PopupControl`回发时，但无`UpdatePanel`在页上，控件工具包不提供任何方法来确定哪些客户端元素已触发反过来导致回发的弹出窗口。 但是了小技巧，可以提供一种解决方法实现此方案。

首先，下面是基本安装程序： 两个文本框中，这两者都触发相同的弹出窗口，日历。 两个`PopupControlExtenders`将文本框和弹出项组合在一起。

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

基本理念都是隐藏的表单字段中添加&lt; `form` &gt;保存启动弹出项的文本框元素：

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

当加载页时，JavaScript 代码会将事件处理程序添加到两个文本框内： 在用户单击一个对文本框中，其名称写入到隐藏的表单字段：

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

在服务器端代码中，必须读取的隐藏字段的值。 由于隐藏的表单字段进行了一些无关紧要操作，一种白名单方法验证隐藏的值是必需的。 一旦已确定正确的文本框中，从日历日期被写入到它。

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


[![当用户单击的文本框，会显示日历](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)

当用户单击的文本框，会显示日历 ([单击以查看实际尺寸的图像](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))


[![单击在日期将其放在文本框中](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)

单击在日期将其放在文本框中 ([单击以查看实际尺寸的图像](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))

>[!div class="step-by-step"]
[上一页](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[下一页](using-multiple-popup-controls-vb.md)
