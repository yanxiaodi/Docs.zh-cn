---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: "操作从客户端代码 (VB) DropShadow 属性 |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的 DropShadow 控件扩展带投影一个面板。 此外可以使用客户端 JavaScrip 更改此扩展程序属性..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 706ccb5a95873aad4c1b9e0942ab06cf4162710a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a>操作从客户端代码 (VB) DropShadow 属性
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)

> AJAX 控件工具包中的 DropShadow 控件扩展带投影一个面板。 此外可以使用客户端 JavaScript 代码更改的此扩展的属性。


## <a name="overview"></a>概述

AJAX 控件工具包中的 DropShadow 控件扩展带投影一个面板。 此外可以使用客户端 JavaScript 代码更改的此扩展的属性。

## <a name="steps"></a>步骤

代码开头面板包含某些行的文本：

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

关联的 CSS 类为面板提供了一种很好的背景色：

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

`DropShadowExtender`加以扩展与投影效果，设置为 50%的不透明度面板：

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

然后，ASP.NET AJAX`ScriptManager`控件使控件工具包工作：

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

另一个面板包含两个设置的不透明度的投影的 JavaScript 链接： 减号链接减少阴影的不透明度，加号链接会增加它。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

JavaScript 函数`changeOpacity()`然后必须首先找到`DropShadowExtender`页上的控件。 ASP.NET AJAX 定义`$find()`完全该任务的方法。 然后，`get_Opacity()`方法检索当前的不透明度，`set_Opacity()`方法将其设置。 然后，JavaScript 代码将当前的不透明度值放入`<label>`元素：

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


[![在客户端上更改不透明度](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)

在客户端上更改不透明度 ([单击以查看实际尺寸的图像](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一篇](adjusting-the-z-index-of-a-dropshadow-vb.md)
