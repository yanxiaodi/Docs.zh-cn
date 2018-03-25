---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: "使用 AJAX 控件工具包控件和控件扩展程序 (VB) |Microsoft 文档"
author: microsoft
description: "了解如何将 AJAX 控件工具包控件和扩展程序添加到 ASP.NET 页。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: 7b248855a1b82f3e8f172b439ee36502f95a39ca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a>使用 AJAX 控件工具包控件和控件扩展程序 (VB)
====================
通过[Microsoft](https://github.com/microsoft)

> 了解如何将 AJAX 控件工具包控件和扩展程序添加到 ASP.NET 页。


AJAX 控件工具包中包含控件和控件扩展程序的集。 在此简易教程，您将学习如何将控件和控件扩展程序添加到 ASP.NET 页。

> [!NOTE] 
> 
> 有关安装 AJAX 控件工具包和将 AJAX 控件工具包添加到 Visual Studio/Visual Web Developer 工具箱中，说明，请参阅本教程[入门 AJAX 控件工具包](get-started-with-the-ajax-control-toolkit-vb.md)。


## <a name="using-ajax-control-toolkit-controls"></a>使用 AJAX 控件工具包控件

AJAX 控件工具包控件的工作方式就像普通的 ASP.NET 控件一样。 你可以将控件从工具箱中拖动到 ASP.NET 页。 可以将控件添加到设计视图或源视图中的页面。

使用 AJAX 控件工具包中的控件时一个特殊的要求。 页面必须包含 ScriptManager 控件。 ScriptManager 控件负责包括所有必要 AJAX 控件工具包控件所需的 JavaScript。

例如，AJAX 控件工具包选项卡包括名为编辑器控件的控件。 此控件显示的丰富的 HTML 编辑器。 请按照下列步骤编辑器控件添加到页面：

1. 创建一个名为 ShowEditor.aspx 的新 ASP.NET 页
2. 在工具箱中选择 AJAX Extensions 选项卡下 ScriptManager 控件并将控件拖到该页面。
3. 在工具箱中选择 AJAX 控件工具包选项卡下的编辑器控件并将控件拖动到该页面上 （请参见图 1）。 设计器应该如图 2 所示。
4. 通过选择菜单选项来运行网站**调试、 启动调试**或按 F5 键。
5. 你应看到图 3 中的页。


[![选择的 HTML 编辑器控件](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)

**图 01**： 选择的 HTML 编辑器控件 ([单击以查看实际尺寸的图像](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))


[![与 ScriptManager 和编辑控件的 visual Studio 设计器](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)

**图 02**： 与 ScriptManager 和编辑控件的 Visual Studio 设计器 ([单击以查看实际尺寸的图像](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))


[![DisplayEditor.aspx 页](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)

**图 03**: DisplayEditor.aspx 页 ([单击以查看实际尺寸的图像](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))


## <a name="using-ajax-control-toolkit-control-extenders"></a>使用 AJAX 控件工具包控件扩展程序

AJAX 控件工具包还包含的控件扩展程序。 顾名思义，一个控件扩展程序添加扩展现有控件的功能。 例如，ConfirmButton 控件扩展程序扩展标准的 ASP.NET 按钮控件。 扩展程序更改按钮控件的行为，以便当你单击它时，按钮将显示确认对话框。

控件扩展，就像的 AJAX 控件工具包控件，需要 ScriptManager 控件。 在开始使用的页中的控件扩展程序之前，你必须将 ScriptManager 控件添加到页。

请按照下列步骤，使用 ConfirmButton 控件扩展程序操作：

1. 创建一个名为 ShowConfirmButton.aspx 的新 ASP.NET 页
2. 将 ScriptManager 控件添加到页面中，通过将控件拖动到该页面上，从 AJAX Extensions 选项卡。
3. 将一个标准按钮控件添加到页面中，通过在工具箱中拖动到设计器图面上拖动标准选项卡下的按钮。
4. 单击**添加扩展程序**任务选项 （请参见图 4）。
5. 在选择扩展程序对话框中，选择 ConfirmButtonExtender （请参见图 5），然后单击确定按钮。
6. 在设计器中选择按钮控件和展开 Extender，Button1\_ConfirmButtonExtender 节点在属性窗口中的 （请参阅图 6）。 将值分配*真正？*到 ConfirmText 属性。
7. 通过选择菜单选项运行页面**调试、 启动调试**或按 F5 键。


[![添加扩展程序任务选项](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)

**图 04**: 添加的扩展程序任务选项 ([单击以查看实际尺寸的图像](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))


[![选择 ConfirmButton 控件扩展程序](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)

**图 05**： 选择 ConfirmButton 控件扩展程序 ([单击以查看实际尺寸的图像](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))


[![设置 ConfirmButton 属性](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)

**图 06**： 设置 ConfirmButton 属性 ([单击以查看实际尺寸的图像](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))


当该页打开时，你会看到一个按钮。 单击按钮时，获取确认对话框中图 7。


[![显示确认对话框](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)

**图 07**： 显示确认对话框中 ([单击以查看实际尺寸的图像](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))


请注意，你通常不要拖动一个控件扩展程序添加到页面。 相反，你使用**添加扩展程序**任务将扩展程序添加到你已添加到页上的控件的选项。 此外，请注意，你设置控件扩展程序属性通过打开要扩展控件的属性表。

可以由多个控件扩展扩展单个 ASP.NET 控件。 要扩展控件的属性表将列出所有与该控件关联的控件扩展程序。

>[!div class="step-by-step"]
[上一页](get-started-with-the-ajax-control-toolkit-vb.md)
[下一页](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)
