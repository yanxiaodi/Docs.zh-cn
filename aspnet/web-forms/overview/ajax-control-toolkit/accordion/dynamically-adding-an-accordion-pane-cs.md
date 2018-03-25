---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: "动态添加的折叠窗格 (C#) |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的 Accordion 控件提供多个窗格，并允许用户一次显示其中一个。 面板通常声明 w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 1c6af79186ca21082647beec500c47974a47c35a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-adding-an-accordion-pane-c"></a>动态添加的折叠窗格 (C#)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip)或[下载 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> AJAX 控件工具包中的 Accordion 控件提供多个窗格，并允许用户一次显示其中一个。 面板中通常声明页本身，但可以使用服务器端代码来实现相同的结果。


## <a name="overview"></a>概述

AJAX 控件工具包中的 Accordion 控件提供多个窗格，并允许用户一次显示其中一个。 面板中通常声明页本身，但可以使用服务器端代码来实现相同的结果。

## <a name="steps"></a>步骤

Accordion 控件可公开到服务器端代码中的所有重要属性。 除了别的之外`Panes`属性授予对构成 Accordion 的窗格的集合的访问。 每个窗格中的类型没有`AccordionPane`。 因此，它是普通用于创建此类窗格：

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

`HeaderContainer`属性`AccordionPane`提供访问在窗格中; 的标头部分的 ASP.NET 控件`ContentContainer`属性`AccordionPane`执行相同的操作的窗格的内容部分。 这样，用于向窗格添加内容的 ASP.NET 代码：

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

最后，必须将窗格添加到`Panes`Accordion 的集合：

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

下面是将两个窗格添加到 Accordion 控件的完整服务器端代码：

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

唯一缺少元素是 Accordion 本身，这取决于是否存在的 ASP.NET`ScriptManager`控件：

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

若要完成该示例，两个 Accordion 控件中引用的 CSS 类提供浏览器样式的信息：

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


[![服务器端代码动态添加可折叠的面板中的数据](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

服务器端代码动态添加可折叠的面板中的数据 ([单击以查看实际尺寸的图像](dynamically-adding-an-accordion-pane-cs/_static/image3.png))

>[!div class="step-by-step"]
[上一页](databinding-to-an-accordion-cs.md)
[下一页](databinding-to-an-accordion-vb.md)
