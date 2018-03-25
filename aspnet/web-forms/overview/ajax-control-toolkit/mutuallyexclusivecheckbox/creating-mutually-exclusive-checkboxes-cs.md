---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: "创建互斥复选框 (C#) |Microsoft 文档"
author: wenz
description: "如果可以选择仅一组的选项之一，通常将使用单选按钮。 但没有一个缺点： 选择一个单选按钮组中的后，..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: e165c3784b246effcaeafc0ad4274bc0ca81a99c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="creating-mutually-exclusive-checkboxes-c"></a>创建互斥复选框 (C#)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)

> 如果可以选择仅一组的选项之一，通常将使用单选按钮。 但没有一个缺点： 选择一个单选按钮组中的后，不能取消选中所有单选按钮。 复选框在任何时候可以是未选中状态，但是不互相排斥。 本教程提供这两种方法的最佳： 是互斥的复选框。


## <a name="overview"></a>概述

如果可以选择仅一组的选项之一，通常将使用单选按钮。 但没有一个缺点： 选择一个单选按钮组中的后，不能取消选中所有单选按钮。 复选框在任何时候可以是未选中状态，但是不互相排斥。 本教程提供这两种方法的最佳： 是互斥的复选框。

## <a name="steps"></a>步骤

ASP.NET AJAX 控件工具包中包含的 MutuallyExclusiveCheckBox 扩展程序。 这使程序员要分配给组名称的任何复选框 (`Key`属性)。 从同一组中的所有复选框，只有一个可能一次选择。

让我们开始使用新的 ASP.NET 页上将两个复选框。 可以有多个，但其中两个已足够用于演示原则：

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

为这两个复选框，必须将在页面上置于 MutuallyExclusiveCheckBoxExtender 控件。 这两个键属性需要具有相同的值，就像 HTML 单选按钮元素的属性必须相同来表示它们所属的组的值。 扩展程序的 TargetControlID 属性指向的复选框的 ID。

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

最后，包括 ASP.NET AJAX`ScriptManager`所需的 ASP.NET AJAX 控件工具包的所有元素：

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

保存并运行此页： 你可以检查并取消选中这两个复选框，但是在任何时间都可以两个复选框检查。


[![只有一个复选框可以检查一次](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)

只有一个复选框可以检查一次 ([单击以查看实际尺寸的图像](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))

>[!div class="step-by-step"]
[下一篇](creating-mutually-exclusive-checkboxes-vb.md)
