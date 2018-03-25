---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: "允许文本框 (VB) 中的某些字符 |Microsoft 文档"
author: wenz
description: "ASP.NET 验证控件可以确保只有某些字符允许在用户输入。 但是这仍不会阻止用户键入无效..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: b41ec1dfda5d85c625026e1f1e1ecd7e190ee3ce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a>允许仅某些字符在文本框中 (VB)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> ASP.NET 验证控件可以确保只有某些字符允许在用户输入。 但是这仍不会阻止用户键入无效的字符并尝试提交表单。


## <a name="overview"></a>概述

ASP.NET 验证控件可以确保只有某些字符允许在用户输入。 但是这仍不会阻止用户键入无效的字符并尝试提交表单。

## <a name="steps"></a>步骤

ASP.NET AJAX 控件工具包中包含`FilteredTextBox`控件扩展了一个文本框。 一旦激活，可能会在字段中输入某组的字符。

为此，我们首先需要像往常一样 ASP.NET AJAX`ScriptManager`这会将加载 JavaScript 库，后者也由 ASP.NET AJAX 控件工具包：

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

然后，我们需要一个文本框：

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

最后，`FilteredTextBoxExtender`控件负责限制允许用户键入的字符。 首先，设置`TargetControlID`属性设为`ID`的`TextBox`控件。 然后，选择的某个可用`FilterType`值：

- `Custom`默认设置;你必须提供有效的字符的列表
- `LowercaseLetters`仅为小写字母
- `Numbers`仅为数字
- `UppercaseLetters`仅大写字母

如果`Custom FilterType`使用时，`ValidChars`属性必须是一组并提供可能键入的字符的列表。 顺便说一下： 如果你尝试将文本粘贴到文本框中，删除所有的无效字符。

下面是有关标记`FilteredTextBoxExtender`仅允许数字的控件 (这也已经可能与`FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

运行页，然后重试如果启用 JavaScript，请输入一个字母，它不起作用;但是，页上会显示数字。 但是请注意，保护`FilteredTextBox`提供不是高防护： 启用如果 JavaScript，因此你必须使用其他验证方法，即 ASP 可能在文本框中，输入任何数据。NET 的验证控件。


[![可能输入仅位数字](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

可能输入仅数字 ([单击以查看实际尺寸的图像](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一篇](allowing-only-certain-characters-in-a-text-box-cs.md)
