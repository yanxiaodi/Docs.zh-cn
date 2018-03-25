---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: "测试的强度的密码 (VB) |Microsoft 文档"
author: wenz
description: "密码是必需几乎任意位置，以便延迟用户倾向于选择简单密码，这很容易中断。 在 ASP PasswordStrength 控件。N..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: 7f09a05fd4b5771b7ab532d40476fe45cbd3fe38
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="testing-the-strength-of-a-password-vb"></a>测试的强度的密码 (VB)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip)或[下载 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)

> 密码是必需几乎任意位置，以便延迟用户倾向于选择简单密码，这很容易中断。 ASP.NET AJAX 控件工具包中的 PasswordStrength 控件可以检查密码的好坏。


## <a name="overview"></a>概述

密码是必需几乎任意位置，以便延迟用户倾向于选择简单密码，这很容易中断。 `PasswordStrength` ASP.NET AJAX 控件工具包中的控件可以检查密码的好坏。

## <a name="steps"></a>步骤

`PasswordStrength`控件扩展文本框中，并检查是否足够好中它的密码。 它提供了大量的属性，则通过选项以下是只是其中一些：

- `MinimumNumericCharacters`密码中所需的数字字符的最小数量
- `MinimumSymbolCharacters`密码中所需的最小数量的符号字符 （不字母和数字）
- `PreferredPasswordLength`最小密码长度
- `RequiresUpperAndLowerCaseCharacters`是否需要使用大写和小写字符的密码

`StrengthIndicatorType`提供的信息如何显示为文本的密码的强度 (值`"Text"`) 或作为一种类型的进度栏 (值`"BarIndicator"`)。 在`DisplayPosition`属性中，你将配置信息出现的位置。 下面是完整的示例，包括 ASP.NET AJAX`ScriptManager`控件，`PasswordStrength`控制和当然文本框中，用户可能在其中输入密码。 为了演示后, 一种形式字段是一个常规文本域而不是密码字段，这样您可以看到在开发过程中进行键入。

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

运行页面，键入消失： 仅密码输入小写字母、 大写字母、 数字和符号后，被视为为不可换行。


[![现在密码适合 （相当）](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)

现在密码 （非常） 适合 ([单击以查看实际尺寸的图像](testing-the-strength-of-a-password-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一篇](testing-the-strength-of-a-password-cs.md)
