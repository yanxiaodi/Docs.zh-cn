---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: "使用 CascadingDropDown (C#) 的自动回发 |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的 CascadingDropDown 控件扩展的 DropDownList 控件，使得一个 DropDownList 负载中的更改关联中 anoth 值..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: cd103283f46223d5158e58227bb53c00c74bc7d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a>自动回发使用 CascadingDropDown (C#)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip)或[下载 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)

> AJAX 控件工具包中的 CascadingDropDown 控件扩展的 DropDownList 控件，使得一个 DropDownList 负载中的更改关联中另一个 DropDownList 值。 但是使用 CascadingDropDown 控件，ASP 时。NET 的 DropDownList 控件的有些功能不起作用，因为以异步方式将数据加载到列表生成的 （不必要的） 的回发本身。 使用的一些 JavaScript 代码，可以避免这种效果。


## <a name="overview"></a>概述

AJAX 控件工具包中的 CascadingDropDown 控件扩展的 DropDownList 控件，使得一个 DropDownList 负载中的更改关联中另一个 DropDownList 值。 （例如，一个列表提供了一份我们状态，并且用处于该状态的主要城市然后填充下一个列表。）但是使用 CascadingDropDown 控件，ASP 时。NET 的 DropDownList 控件的有些功能不起作用，因为以异步方式将数据加载到列表生成的 （不必要的） 的回发本身。 使用的一些 JavaScript 代码，可以避免这种效果。

## <a name="steps"></a>步骤

为了激活 ASP.NET AJAX 和控件工具包中的功能`ScriptManager`必须在页面上任意位置放置控件 (但内&lt; `form` &gt;元素):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

然后，需要进行的 DropDownList 控制：

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

对于此列表中，添加 CascadingDropDown 扩展程序以提供 web 服务 URL 和方法的信息：

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

然后 CascadingDropDown 扩展程序以异步方式调用具有以下方法签名的 web 服务：

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

该方法返回类型 CascadingDropDown 值的数组。 该类型的构造函数首先需要列表项的标题，然后值 (HTML`value`属性)。

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

加载浏览器中将填充下拉列表中的与三个供应商，第二个正在预先选定状态。 此外，ASP.NET 定义`__doPostBack()`JavaScript 方法。 一旦已加载页面，此 JavaScript 调用添加到下拉列表中，但仅中是否存在元素它。 如果在列表中没有元素，该控件工具包当前正在加载它们，以便 JavaScript 代码使用超时，并尝试再次在半秒。

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

这样一来，在列表中没有实际元素且用户选择一个条目时，回发只执行。


[![选择一个列表元素将导致回发](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)

选择一个列表元素将导致回发 ([单击以查看实际尺寸的图像](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))

>[!div class="step-by-step"]
[上一页](presetting-list-entries-with-cascadingdropdown-cs.md)
[下一页](filling-a-list-using-cascadingdropdown-vb.md)
