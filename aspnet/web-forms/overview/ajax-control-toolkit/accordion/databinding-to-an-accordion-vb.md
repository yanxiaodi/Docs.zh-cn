---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
title: "数据绑定到 Accordion (VB) |Microsoft 文档"
author: wenz
description: "AJAX 控件工具包中的 Accordion 控件提供多个窗格，并允许用户一次显示其中一个。 面板通常声明 w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b19f0875-7d3e-4ecf-baa1-a0c693c765b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
msc.type: authoredcontent
ms.openlocfilehash: aeff732e4daed6ed22fd5f3b6adcdeb6082aae53
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="databinding-to-an-accordion-vb"></a>数据绑定到 Accordion (VB)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip)或[下载 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)

> AJAX 控件工具包中的 Accordion 控件提供多个窗格，并允许用户一次显示其中一个。 面板中通常声明页本身，但绑定到数据源提供了更大的灵活性。


## <a name="overview"></a>概述

AJAX 控件工具包中的 Accordion 控件提供多个窗格，并允许用户一次显示其中一个。 面板中通常声明页本身，但绑定到数据源提供了更大的灵活性。

## <a name="steps"></a>步骤

首先，数据源是必需的。 此示例使用 AdventureWorks 数据库和 Microsoft SQL Server 2005 Express Edition。 数据库是 （包括速成版） 的 Visual Studio 安装的可选部分，还可用作下单独下载[https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)。 AdventureWorks 数据库是 SQL Server 2005 示例和示例数据库的一部分 (在下载[https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;-4a83-b309-53b7b77edf78&displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 设置数据库的最简单方法是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;-4a83-b309-53b7b77edf78&displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) 和附加`AdventureWorks.mdf`数据库文件。

对于此示例中，我们假定的 SQL Server 2005 Express Edition 实例称为`SQLEXPRESS`和驻留在与 web 服务器; 相同的计算机上也是默认设置。 如果你的设置不同，你必须调整数据库的连接信息。

为了激活 ASP.NET AJAX 和控件工具包中的功能`ScriptManager`必须在页面上任意位置放置控件 (但内`<form>`元素):

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample1.aspx)]

然后，将数据源添加到页面中。 若要使用的数据量有限，我们仅在 AdventureWorks 数据库的供应商表中选择前五个项。 如果使用 Visual Studio 助手来创建数据源，请记住的当前版本中的 bug 不前缀表名 (`Vendor`) 与`Purchasing`。 以下标记显示正确的语法：

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample2.aspx)]

请记住数据源的名称 (ID)。 然后必须在中使用此非常标识`DataSourceID`Accordion 控件的属性：

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample3.aspx)]

你可以在 Accordion 控件中，提供该控件，包括标头的各部分的模板 (`<HeaderTemplate>`) 和内容 (`<ContentTemplate>`)。 这些元素中只输出数据与数据源，使用`DataBinder.Eval()`方法：

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample4.aspx)]

加载页面后，数据源必须绑定到此服务器端代码与 accordion 中：

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample5.aspx)]

若要结束此示例，你需要定义 Accordion 控件中的两个引用的 CSS 类 (在其属性`HeaderCssClass`和`ContentCssClass`)。 将以下标记放入`<head>`页的部分：

[!code-css[Main](databinding-to-an-accordion-vb/samples/sample6.css)]


[![可折叠的面板中的数据直接来自数据源](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)

可折叠的面板中的数据直接来自数据源 ([单击以查看实际尺寸的图像](databinding-to-an-accordion-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一页](dynamically-adding-an-accordion-pane-cs.md)
[下一页](dynamically-adding-an-accordion-pane-vb.md)
