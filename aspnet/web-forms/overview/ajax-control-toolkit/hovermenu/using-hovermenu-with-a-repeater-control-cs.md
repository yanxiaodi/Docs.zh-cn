---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
title: "转发器控件 (C#) 使用 HoverMenu |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的 HoverMenu 控件提供简单的弹出窗口效果： 当鼠标指针悬停在元素上时，指定在显示弹出窗口..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e7700e7b-edc3-4183-a713-70e507cc7490
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: aac5a26191cc633204549274c327e065578f4226
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="using-hovermenu-with-a-repeater-control-c"></a>转发器控件 (C#) 使用 HoverMenu
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)

> AJAX 控件工具包中的 HoverMenu 控件提供简单的弹出窗口效果： 当鼠标指针悬停在元素上时，在指定位置显示弹出窗口。 还有可能要使用此控件在中继器内。


## <a name="overview"></a>概述

`HoverMenu` AJAX 控件工具包中的控件提供一个简单的弹出窗口效果： 当鼠标指针悬停在元素上时，在指定位置显示弹出窗口。 还有可能要使用此控件在中继器内。

## <a name="steps"></a>步骤

首先，数据源是必需的。 此示例使用 AdventureWorks 数据库和 Microsoft SQL Server 2005 Express Edition。 数据库是 （包括速成版） 的 Visual Studio 安装的可选部分，还可用作下单独下载[https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)。 AdventureWorks 数据库是 SQL Server 2005 示例和示例数据库的一部分 (在下载[https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;-4a83-b309-53b7b77edf78&displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 设置数据库的最简单方法是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;-4a83-b309-53b7b77edf78&displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) 和附加`AdventureWorks.mdf`数据库文件。

对于此示例中，我们假定的 SQL Server 2005 Express Edition 实例称为`SQLEXPRESS`和驻留在与 web 服务器; 相同的计算机上也是默认设置。 如果你的设置不同，你必须调整数据库的连接信息。

为了激活 ASP.NET AJAX 和控件工具包中的功能`ScriptManager`必须在页面上任意位置放置控件 (但内`<form>`元素):

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample1.aspx)]

然后，将数据源添加到页面中。 若要使用的数据量有限，我们仅在 AdventureWorks 数据库的供应商表中选择前五个项。 如果使用 Visual Studio 助手来创建数据源，请记住的当前版本中的 bug 不前缀表名 (`Vendor`) 与`Purchasing`。 以下标记显示正确的语法：

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample2.aspx)]

接下来，添加一个面板，它可作为模式弹出窗口：

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample3.aspx)]

现在，`HoverMenuExtender`派上用场。 扩展器，以便数据源中的每个元素获取其自己的弹出窗口，必须放置在转发器的`<ItemTemplate>`部分。 此处为标记：

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample4.aspx)]

现在，数据源中的每个项向右显示一个弹出窗口 (`PopupPosition`属性) 的 50 毫秒的延迟后 (`PopDelay`属性)。


[![转发器在每个项旁边会出现的悬停菜单](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)

转发器在每个项旁边会出现的悬停菜单 ([单击以查看实际尺寸的图像](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))

>[!div class="step-by-step"]
[下一篇](using-hovermenu-with-a-repeater-control-vb.md)
