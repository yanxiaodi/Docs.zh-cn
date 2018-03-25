---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: "使用 Web 服务后端 (VB) 中创建数值加/减控件 |Microsoft 文档"
author: wenz
description: "而不是让用户在复选框中键入一个值，数字向上/向下 （即在 Windows 和其他操作系统的系统上存在） 的控件无法证明随着更多 c..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 5ceefd6c18761c2abe3f3a4298d340642a0951d6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a>使用 Web 服务后端 (VB) 中创建数值向上/向下控件
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip)或[下载 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)

> 而不是让用户在复选框中键入一个值，数字加/减控件 （即在 Windows 和其他操作系统的系统上存在） 无法证明随着更多喜欢。 默认情况下，NumericUpDown 控件始终增加或减少值 1，但 web 服务证明更大的灵活性。


## <a name="overview"></a>概述

而不是让用户在复选框中键入一个值，数字加/减控件 （即在 Windows 和其他操作系统的系统上存在） 无法证明随着更多喜欢。 默认情况下，`NumericUpDown`控件始终增加或减少值 1，但 web 服务证明更大的灵活性。

## <a name="steps"></a>步骤

ASP.NET AJAX 控件工具包中包含`NumericUpDown`会自动将两个按钮添加到文本框中的扩展： 一个用于增加其值，一个用于减少它。 但是该控件还支持的 web 服务调用 （或页方法调用）。 每当向上或向下按钮单击后的 JavaScript 代码连接到 web 服务器并执行的方法存在。 方法签名是以下之一：

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

`current`自变量是在文本框中; 的当前值`tag`属性是可以作为属性的设置的其他上下文数据`NumericUpDown`扩展程序 （但不是必需的）。

对于此示例，数值加/减控件应仅允许为 2 的幂的值： 1、 2、 4、 8、 16、 32、 64，依次类推。 因此，当用户想要增加值时执行的方法必须 double 的旧值;另一种方法必须将除以两个值。 因此下面是完整的 web 服务：

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

最后，创建一个新的 ASP.NET 页。 像往常一样，你需要`ScriptManager`控件，`TextBox`控件和`NumericUpDownExtender`控件。 对于后者，你需要提供 web 服务信息：

- `ServiceDownMethod`名称下的 web 方法或页面方法
- `ServiceDownPath`使用向下的服务方法; web 服务的路径如果你使用页方法，忽略
- `ServiceUpMethod`名称最多的 web 方法或页面方法
- `ServiceUpPath`具有最服务方法; web 服务的路径如果你使用页方法，忽略

下面是页的完整标记：

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

如果运行页面时，请注意如何在文本框中的值始终增加一倍时单击上限按钮，并单击较低的按钮时，减少了一半。


[![是 2 的幂的数字显示](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)

是 2 的幂的数字显示 ([单击以查看实际尺寸的图像](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一篇](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
