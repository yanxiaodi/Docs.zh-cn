---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: "使用 ReorderList (C#) 的回发 |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的 ReorderList 控件提供可由用户通过拖放重新排序的列表。 每当列表重新排序后，采购订单..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 5f5c5e253f6d85203a488152c5ad908157e6ee0c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="using-postbacks-with-reorderlist-c"></a>回发使用 ReorderList (C#)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip)或[下载 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)

> AJAX 控件工具包中的 ReorderList 控件提供可由用户通过拖放重新排序的列表。 列表重新排序后，每当回发应通知的更改的服务器。


## <a name="overview"></a>概述

`ReorderList` AJAX 控件工具包中的控件提供可由用户通过拖放重新排序的列表。 列表重新排序后，每当回发应通知的更改的服务器。

## <a name="steps"></a>步骤

有几个可能的数据源`ReorderList`控件。 一个是使用`XmlDataSource`控件：

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

若要将绑定到此 XML`ReorderList`必须设置控制并启用回发，以下属性：

- `DataSourceID`： 数据源的 ID
- `SortOrderField`： 要作为排序依据的属性
- `AllowReorder`： 是否允许用户重新排列列表元素
- `PostBackOnReorder`： 是否创建回发时重新排列列表

下面是相应的控件的标记：

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

在`ReorderList`可能使用绑定控件，数据源中的特定数据`Eval()`方法：

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

在页面上任意位置，标签将在实际应用最后一个重新排序发生时保存的信息：

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

此标签填入处理回发的服务器端代码中的文本：

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

最后，以便激活 ASP.NET AJAX 和控件工具包中的功能`ScriptManager`控件必须放置在页面上：

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


[![每个重新排序触发回发](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)

每个重新排序触发回发 ([单击以查看实际尺寸的图像](using-postbacks-with-reorderlist-cs/_static/image3.png))

>[!div class="step-by-step"]
[下一篇](drag-and-drop-via-reorderlist-cs.md)
