---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: "滑块控件使用自动回发 (C#) |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的滑块控件提供图形滑块，可以使用鼠标进行控制。 它是可以进行滑块自动过帐..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: f6291f4162b3069a6316a60b4b29f82f55121aac
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="using-the-slider-control-with-auto-postback-c"></a>滑块控件使用自动回发 (C#)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)

> AJAX 控件工具包中的滑块控件提供图形滑块，可以使用鼠标进行控制。 它是可以进行滑块有些一次将其值更改。


## <a name="overview"></a>概述

AJAX 控件工具包中的滑块控件提供图形滑块，可以使用鼠标进行控制。 它是可以进行滑块有些一次将其值更改。

## <a name="steps"></a>步骤

为了使滑块更改时自动回发，两个文本框内需要的特性`AutoPostBack="true"`： 文本框将成为滑块本身，并包含滑块的位置的文本框。 下面是该所需的标记：

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

`SliderExtender` ASP.NET AJAX 控件工具包中的控件将滑块功能分配给两个文本框中：

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

其他标签元素以后可用于通知的回发用户：

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

最后，`ScriptManager`控件的 ASP.NET AJAX 加载控件工具包工作所需的 JavaScript:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

现在返回; 发布滑块在服务器端，可能捕获到此事件，并采取行动中：

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


[![将滑块移动触发回发](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)

将滑块移动触发回发 ([单击以查看实际尺寸的图像](using-the-slider-control-with-auto-postback-cs/_static/image3.png))


[![然后，此更改的日期写入标签中](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)

然后，此更改的日期写入标签中 ([单击以查看实际尺寸的图像](using-the-slider-control-with-auto-postback-cs/_static/image6.png))

>[!div class="step-by-step"]
[下一篇](databinding-the-slider-control-cs.md)
