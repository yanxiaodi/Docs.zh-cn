---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: "在转发器 (VB) 中使用 ConfirmButton |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的 ConfirmButton 扩展程序创建是 / 没有弹出窗口，当用户单击按钮时 （包括 LinkButton 控件）。 仅当是是..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 5da8491c157ad6f35c2c25803680f262a35ce6e1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="using-a-confirmbutton-in-a-repeater-vb"></a>在转发器 (VB) 中使用 ConfirmButton
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)

> AJAX 控件工具包中的 ConfirmButton 扩展程序创建是 / 没有弹出窗口，当用户单击按钮时 （包括 LinkButton 控件）。 仅当单击是，则执行是按钮的操作，否则取消。 也可以在中继器此设置。


## <a name="overview"></a>概述

AJAX 控件工具包中的 ConfirmButton 扩展程序创建是 / 没有弹出窗口，当用户单击按钮时 （包括 LinkButton 控件）。 仅当单击是，则执行是按钮的操作，否则取消。 也可以在中继器此设置。

## <a name="steps"></a>步骤

首先，数据源是必需的。 此示例使用 AdventureWorks 数据库和 Microsoft SQL Server 2005 Express Edition。 数据库是 （包括速成版） 的 Visual Studio 安装的可选部分，还可用作下单独下载[https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)。 AdventureWorks 数据库是 SQL Server 2005 示例和示例数据库的一部分 (在下载[https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;-4a83-b309-53b7b77edf78&displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 设置数据库的最简单方法是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;-4a83-b309-53b7b77edf78&displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) 和附加`AdventureWorks.mdf`数据库文件。

对于此示例中，我们假定的 SQL Server 2005 Express Edition 实例称为`SQLEXPRESS`和驻留在与 web 服务器; 相同的计算机上也是默认设置。 如果你的设置不同，你必须调整数据库的连接信息。

为了激活 ASP.NET AJAX 和控件工具包中的功能`ScriptManager`必须在页面上任意位置放置控件 (但内`<form>`元素):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

然后，数据源是必需的。 为简单起见，会检索仅 AdventureWorks 的供应商表中的前五个条目。 请注意，当使用 Visual Studio 向导创建数据源，表名 (`Vendors`) 当前不使用正确的前缀与`Purchasing`。 以下标记是正确的订阅：

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

然后可以内中继器使用此数据源。 像往常一样，`DataBinder.Eval()`方法从数据源中检索数据。 `ConfirmButtonExtender`控件然后必须放在`<ItemTemplate>`的转发器，使其显示为数据源中的每个条目的部分。

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]


[![数据源的每个条目旁边将显示确认按钮](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)

数据源的每个条目旁边将显示确认按钮 ([单击以查看实际尺寸的图像](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一篇](using-a-confirmbutton-in-a-repeater-cs.md)
