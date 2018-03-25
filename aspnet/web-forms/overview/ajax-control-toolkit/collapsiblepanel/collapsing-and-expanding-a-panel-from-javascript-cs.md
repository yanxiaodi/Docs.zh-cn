---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: "展开和折叠的面板从 JavaScript (C#) |Microsoft 文档"
author: wenz
description: "ASP.NET AJAX 控件工具包中的 CollapsiblePanel 控件扩展一个面板，并为它提供的功能折叠其内容，并将其展开..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 666f3e212ccdd5b26b466f4672134ce751dc5dd1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a>展开和折叠的面板从 JavaScript (C#)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)

> ASP.NET AJAX 控件工具包中的 CollapsiblePanel 控件扩展一个面板，并为它提供的功能折叠其内容，并再次将其展开。 这两个操作也可以通过自定义 JavaScript 代码来触发。


## <a name="overview"></a>概述

ASP.NET AJAX 控件工具包中的 CollapsiblePanel 控件扩展一个面板，并为它提供的功能折叠其内容，并再次将其展开。 这两个操作也可以通过自定义 JavaScript 代码来触发。

## <a name="steps"></a>步骤

首先，创建一个新的 ASP.NET 页并包括`ScriptManager`中`<form>`元素。 这将加载的 ASP.NET AJAX 库所需的控件工具包：

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

然后，使用一些文本创建一个面板，因此，可以看见折叠/展开效果：

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

如你所见，面板将引用的 CSS 类用于 （现在基本上定义背景色和面板的宽度） 如下所示：

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

`CollapsiblePanelExtender`控件要求`TargetControlID`属性，以便该工具包知道哪个面板以折叠或展开发出请求后：

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

遗憾的是，扩展程序当前未公开特定 API 的折叠或展开面板中，但某些未记录的方法都将执行。 首先，将三个 HTML 按钮添加到然后触发客户端 JavaScript 以折叠或展开面板中的内容的页面：

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

在客户端 JavaScript 代码 (入门`<script type="text/javascript">`)，则`$find()`方法需要使用访问`CollapsiblePanelExtender`。 `$find("cpe")`将返回对它的引用。 从该处上，特定的方法将解决手头的任务。

方法 （扩展） 中打开面板称为`_doOpen()`; 下面的代码实现`doOpen()`单击第一个按钮时调用的函数：

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

用于关闭，或折叠面板，`_doClose()`方法需要执行。 因此当用户单击第二个按钮上时，调用下面的 JavaScript 代码：

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

第三个按钮切换面板的状态： 从折叠到已展开，反之亦然。 `CollapsiblePanelExtender`公开`toggle()`后者执行具体的方法： 反转面板的状态。 但是还有另一种方法 (内部使用`toggle()`方法):`get_Collapsed()`方法`CollapsiblePanelExtender()`告诉我们是否折叠的面板。 根据此函数的返回值，面板为则可以展开 (`_doOpen()`方法) 或折叠 (`_doClose()`) 方法：

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


[![第三个按钮更改面板的状态： 从折叠到展开且后](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)

第三个按钮更改面板的状态： 从折叠到展开且后 ([单击以查看实际尺寸的图像](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))

>[!div class="step-by-step"]
[下一篇](collapsing-and-expanding-a-panel-from-javascript-vb.md)
