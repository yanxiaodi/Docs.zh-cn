---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: "预设置具有 CascadingDropDown (VB) 的列表项 |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的 CascadingDropDown 控件扩展的 DropDownList 控件，使得一个 DropDownList 负载中的更改关联中 anoth 值..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: c28c7893c39d9ba9f828c34da7ffdce525ee248e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="presetting-list-entries-with-cascadingdropdown-vb"></a>预设置具有 CascadingDropDown (VB) 的列表项
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip)或[下载 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)

> AJAX 控件工具包中的 CascadingDropDown 控件扩展的 DropDownList 控件，使得一个 DropDownList 负载中的更改关联中另一个 DropDownList 值。 很少的代码很可能动态加载数据后，预先选择一个列表元素。


## <a name="overview"></a>概述

AJAX 控件工具包中的 CascadingDropDown 控件扩展的 DropDownList 控件，使得一个 DropDownList 负载中的更改关联中另一个 DropDownList 值。 （例如，一个列表提供了一份我们状态，并且用处于该状态的主要城市然后填充下一个列表。）很少的代码很可能动态加载数据后，预先选择一个列表元素。

## <a name="steps"></a>步骤

为了激活 ASP.NET AJAX 和控件工具包中的功能`ScriptManager`必须在页面上任意位置放置控件 (但内`<form>`元素):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

然后，需要进行的 DropDownList 控制：

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

对于此列表中，添加 CascadingDropDown 扩展程序以提供 web 服务 URL 和方法的信息：

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

然后 CascadingDropDown 扩展程序以异步方式调用具有以下方法签名的 web 服务：

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

该方法返回类型 CascadingDropDown 值的数组。 该类型的构造函数首先需要列表项的标题，然后值 (HTML`value`属性)。 如果第三个自变量设置为 true，列表元素中自动选择浏览器。

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

加载浏览器中将填充下拉列表中的与三个供应商，第二个正在预先选定状态。


[![填充和预先自动选择列表](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)

填充和预先自动选择列表 ([单击以查看实际尺寸的图像](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一页](using-cascadingdropdown-with-a-database-vb.md)
[下一页](using-auto-postback-with-cascadingdropdown-vb.md)
