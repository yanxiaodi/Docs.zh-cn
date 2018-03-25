---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: "使用颜色选取器控件扩展程序 (VB) |Microsoft 文档"
author: microsoft
description: "颜色选取器是客户端颜色选取功能提供 UI 弹出窗口控件中的 ASP.NET AJAX 扩展。 可以将它附加到任何 ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 7453845909b2c0bd8d6b476b19d0fbc5050f7460
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="using-the-colorpicker-control-extender-vb"></a>使用颜色选取器控件扩展程序 (VB)
====================
通过[Microsoft](https://github.com/microsoft)

> 颜色选取器是客户端颜色选取功能提供 UI 弹出窗口控件中的 ASP.NET AJAX 扩展。 可以将它附加到任何 ASP.NET 文本框控件。 它。


本教程旨在说明如何使用 AJAX 控件工具包颜色选取器控件扩展程序。 颜色选取器控件扩展程序显示弹出对话框中，可用于选择一种颜色。 无论何时你想要提供一个直观的用户界面，供用户选择一个颜色时，颜色选取器非常有用。

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>扩展带颜色选取器控件扩展程序的 TextBox 控件

例如，假设你想要创建一个网站来使访问者可以创建自定义的业务卡。 访问者可以用于业务卡输入文本并选取的颜色。 列表 1 中的 ASP.NET 页面包含两个名为 txtCardText 和 txtCardColor 的文本框控件。 在提交表单时，将显示所选的值 （请参见图 1）。


[![用于创建名片简单窗体](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

**图 01**： 用于创建名片简单窗体 ([单击以查看实际尺寸的图像](using-the-colorpicker-control-extender-vb/_static/image2.png))


**列表 1-CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

窗体中列出 1 工作原理，但它不提供出色的用户体验。 用户必须在文本框中键入一种颜色。 如果用户想专用的颜色-例如，只需右明暗度的 pea 绿色-然后用户必须找出 HTML 颜色代码而无需任何帮助。

颜色选取器控件扩展程序可用于创建更好的用户体验。 颜色选取器显示颜色对话框时将焦点移到 TextBox 控件 （请参见图 2）。


[![颜色选取器控件扩展](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

**图 02**: 颜色选取器控件扩展程序 ([单击以查看实际尺寸的图像](using-the-colorpicker-control-extender-vb/_static/image4.png))


你需要完成两个步骤将颜色选取器控件扩展程序的窗体中列出 1 用于：

1. ScriptManager 控件添加到页面
2. 颜色选取器控件扩展程序添加到页面

你可以使用颜色选取器之前，你必须将 ScriptManager 添加到你的页面。 添加 ScriptManager 的好时机是正下方打开服务器端&lt;窗体&gt;标记。 你可以从 （ScriptManager 位于 AJAX Extensions 选项卡） 工具箱拖到页面上 ScriptManager。 或者，你可以开始服务器端窗体标记下，到源视图中键入以下标记：

&lt;asp: ScriptManager ID ="ScriptManager1"runat ="server"/&gt;

颜色选取器控件扩展程序添加到页面的最简单方法是在设计视图中。 如果鼠标悬停 txtCardColor 文本框中，智能任务选项将显示使用，你可以添加扩展程序 （请参见图 3）。 如果您选择此选项，则将显示扩展程序向导 （请参见图 4）。


[![添加扩展程序](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

**图 03**： 添加扩展程序 ([单击以查看实际尺寸的图像](using-the-colorpicker-control-extender-vb/_static/image6.png))


[![选择一个控件扩展程序添加的扩展程序向导](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

**图 04**： 选择一个控件扩展程序添加的扩展程序向导 ([单击以查看实际尺寸的图像](using-the-colorpicker-control-extender-vb/_static/image8.png))


你可以选择要扩展 txtCardColor 文本框中使用的颜色选取器扩展程序的颜色选取器扩展程序。 单击确定以关闭对话框。

进行这些更改后，页上的源如下所示列出 2。

**列出 2-CreateCard.aspx （与颜色选取器）**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

请注意，页现在包含出现正下方的 txtCardColor TextBox 控件 ColorPickerExtender 控件。 ColorPickerExtender 控件扩展 txtCardColor 控件，使其显示颜色选取器对话框。

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>一个按钮用于启动颜色选取器对话框

颜色选取器扩展程序支持以下属性：

- PopupButtonId-显示颜色选取器对话框页上的按钮的 ID。
- PopupPosition-的位置，相对于目标控件，颜色选取器对话框。 可能的值是绝对、 Center、 BottomLeft、 BottomRight、、 左边框、 右上边框、 右、 和 （默认值为 BottomLeft） 的左侧。
- SampleControlId-显示所选的颜色的控件的 ID。
- SelectedColor-选定颜色选取器的初始颜色。

这些属性可用于自定义颜色选取器对话框的显示方式以及所选的颜色的显示方式。 中列出的 3 页说明了如何使用几个这些属性。

**列出 3-CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

中列出的 3 页包括取色按钮 （请参见图 5）。 单击此按钮时，颜色选取器对话框将出现上方文本框。 如果从对话框中选择一种颜色所选的颜色显示为 lblSample 标签控件的背景色。

颜色选取器 PopupButtonID 属性用于将选择颜色按钮的颜色选取器扩展程序与相关联。 你提供 PopupButtonID 属性的值，颜色选取器对话框不会再出现目标控件有焦点。 你必须单击按钮以显示对话框。

SampleControlID 属性用于将显示颜色选取器与所选的颜色的控件相关联。 颜色选取器更改为当前所选颜色的此控件的背景色。


[![显示具有一个按钮的颜色选取器对话框](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

**图 05**： 显示具有一个按钮的颜色选取器对话框 ([单击以查看实际尺寸的图像](using-the-colorpicker-control-extender-vb/_static/image10.png))


## <a name="summary"></a>摘要

在本教程中，您学习了如何使用颜色选取器控件扩展程序显示弹出窗口颜色选取器对话框。 首先，我们探讨了如何才能显示对话框时焦点移动到文本框控件。 接下来，您学习了如何创建显示颜色选取器对话框，单击该按钮的按钮。

>[!div class="step-by-step"]
[上一篇](using-the-colorpicker-control-extender-cs.md)
