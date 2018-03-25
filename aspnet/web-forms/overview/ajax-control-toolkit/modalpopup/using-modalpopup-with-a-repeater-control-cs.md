---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
title: "转发器控件 (C#) 使用 ModalPopup |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的 ModalPopup 控制提供一种简单的方法，以创建模式的弹出项，使用客户端的方式。 它还可使用此 contr...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d686d84a-1c58-492e-8a77-3eb5a0cfe918
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: d945b8decf4debbcbf415163e486e2bce8097701
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="using-modalpopup-with-a-repeater-control-c"></a>转发器控件 (C#) 使用 ModalPopup
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)

> AJAX 控件工具包中的 ModalPopup 控制提供一种简单的方法，以创建模式的弹出项，使用客户端的方式。 还有可能要使用此控件在中继器内。


## <a name="overview"></a>概述

AJAX 控件工具包中的 ModalPopup 控制提供一种简单的方法，以创建模式的弹出项，使用客户端的方式。 还有可能要使用此控件在中继器内。

## <a name="steps"></a>步骤

首先，数据源是必需的。 此示例使用 AdventureWorks 数据库和 Microsoft SQL Server 2005 Express Edition。 数据库是 （包括速成版） 的 Visual Studio 安装的可选部分，还可用作下单独下载[https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)。 AdventureWorks 数据库是 SQL Server 2005 示例和示例数据库的一部分 (在下载[https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;-4a83-b309-53b7b77edf78&displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 设置数据库的最简单方法是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;-4a83-b309-53b7b77edf78&displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) 和附加`AdventureWorks.mdf`数据库文件。 对于此示例中，我们假定的 SQL Server 2005 Express Edition 实例称为`SQLEXPRESS`和驻留在与 web 服务器; 相同的计算机上也是默认设置。 如果你的设置不同，你必须调整数据库的连接信息。 为了激活 ASP.NET AJAX 和控件工具包中的功能`ScriptManager`必须在页面上任意位置放置控件 (但内`<form>`元素):

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample1.aspx)]

然后，将数据源添加到页面中。 若要使用的数据量有限，我们仅在 AdventureWorks 数据库的供应商表中选择前五个项。 如果使用 Visual Studio 助手来创建数据源，请记住的当前版本中的 bug 不前缀表名 (`Vendor`) 与`Purchasing`。 以下标记显示正确的语法：

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample2.aspx)]

接下来，添加一个面板，它可作为模式弹出窗口。 它包含`Button`控件再次关闭弹出窗口：

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample3.aspx)]

为了使在转发器中, 工作的弹出窗口`ModalPopupExtender`控件必须放置在`<ItemTemplate>`的转发器的部分。 因此面板位于外部转发器，但扩展程序位于。 下面是转发器的标记：

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample4.aspx)]

然后，将包含用于触发模式弹出它旁边的按钮显示数据源中的每个项。


[![可以为每个数据源条目触发模式弹出窗口](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)

可以为每个数据源条目触发模式弹出窗口 ([单击以查看实际尺寸的图像](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))

>[!div class="step-by-step"]
[上一页](launching-a-modal-popup-window-from-server-code-cs.md)
[下一页](handling-postbacks-from-a-modalpopup-cs.md)
