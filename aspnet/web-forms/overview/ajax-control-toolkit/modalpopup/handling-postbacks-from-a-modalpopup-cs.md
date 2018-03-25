---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: "处理从 ModalPopup (C#) 的回发 |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的 ModalPopup 控制提供一种简单的方法，以创建模式的弹出项，使用客户端的方式。 必须格外小心时 pos..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 096c32d6004474d3d5952ce12d7349b9ebda6003
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-modalpopup-c"></a>处理回发从 ModalPopup (C#)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)

> AJAX 控件工具包中的 ModalPopup 控制提供一种简单的方法，以创建模式的弹出项，使用客户端的方式。 从弹出窗口中创建回发时，必须采用格外小心。


## <a name="overview"></a>概述

AJAX 控件工具包中的 ModalPopup 控制提供一种简单的方法，以创建模式的弹出项，使用客户端的方式。 从弹出窗口中创建回发时，必须采用格外小心。

## <a name="steps"></a>步骤

为了激活 ASP.NET AJAX 和控件工具包中的功能`ScriptManager`必须在页面上任意位置放置控件 (但内`<form>`元素):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

接下来，添加一个面板，它可作为模式弹出窗口。 存在，用户可以输入一个名称和电子邮件地址。 一个按钮用于关闭弹出窗口，并保存信息。 请注意，`OnClick`特性设置，以便当单击此按钮的回发时发生：

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

页本身包含的完全相同的信息的两个标签： 名称和电子邮件地址。 一个按钮用于触发模式弹出窗口：

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

若要显示的弹出窗口，添加`ModalPopupExtender`控件。 设置`PopupControlID`属性面板的 ID 和`TargetControlID`到按钮的 ID:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

现在只要`Save`单击模式弹出窗口中的按钮时，服务器端`SaveData()`执行方法。 存在，无法将输入的数据保存在数据存储中。 为简单起见，新的数据只需标签中的输出：

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

此外，应使用当前名称和电子邮件填充模式的弹出窗口中将文本框控件。 但是这是仅有必要无回发发生时。 如果回发，则 ASP.NET viewstate 功能将自动填充使用适当的值的文本框。

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


[![模式弹出导致回发](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)

模式弹出导致回发 ([单击以查看实际尺寸的图像](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))

>[!div class="step-by-step"]
[上一页](using-modalpopup-with-a-repeater-control-cs.md)
[下一页](positioning-a-modalpopup-cs.md)
