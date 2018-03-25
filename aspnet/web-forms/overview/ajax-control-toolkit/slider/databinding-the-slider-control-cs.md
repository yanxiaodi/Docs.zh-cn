---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: "数据绑定滑块控件 (C#) |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的滑块控件提供图形滑块，可以使用鼠标进行控制。 就可以将绑定当前 positio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 2aa770bce5969a4ab57893d5ceecc127cf7f7872
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="databinding-the-slider-control-c"></a>数据绑定滑块控件 (C#)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip)或[下载 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)

> AJAX 控件工具包中的滑块控件提供图形滑块，可以使用鼠标进行控制。 很可能要绑定到另一个 ASP.NET 控件的滑块的当前位置。


## <a name="overview"></a>概述

AJAX 控件工具包中的滑块控件提供图形滑块，可以使用鼠标进行控制。 很可能要绑定到另一个 ASP.NET 控件的滑块的当前位置。

## <a name="steps"></a>步骤

为了激活 ASP.NET AJAX 和控件工具包中的功能`ScriptManager`必须在页面上任意位置放置控件 (但内`<form>`元素):

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

接下来，添加两个`TextBox`到页面的控件。 一个将转换为图形滑块，并且另一个将保留将滑块的位置。

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

下一步已是最后一步。 `SliderExtender` ASP.NET AJAX 控件工具包中的控制使滑块外的第一个文本框和滑块位置更改时自动更新第二个文本框。 为了使让此操作生效，`SliderExtender`的`TargetControlID`属性必须设置为第一个文本框; 的 ID`BoundControlID`属性必须设置为第二个文本框的 ID。

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

正如您可以看到在浏览器中，数据绑定的工作原理在两个方向： 在文本框中输入新值更新滚动条的位置。 如果使只读第二个文本框中，可能先将弱保护添加到文本字段中，以便更难要手动更新也没有值的用户。


[![滑块和文本框是同步](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)

滑块和文本框是同步 ([单击以查看实际尺寸的图像](databinding-the-slider-control-cs/_static/image3.png))

>[!div class="step-by-step"]
[上一页](using-the-slider-control-with-auto-postback-cs.md)
[下一页](using-the-slider-control-with-auto-postback-vb.md)
