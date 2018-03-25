---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: "反击机器人 (C#) |Microsoft 文档"
author: wenz
description: "自动化的机器人石膏的垃圾邮件，提交注释窗体，而无需任何用户交互的网络日志和其他网站。 在 ASP.NET AJAX Con NoBot 控件..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: b8eedff4691c1115e242be884f9e74663dc0b4f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="fighting-bots-c"></a>反击机器人 (C#)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip)或[下载 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)

> 自动化的机器人石膏的垃圾邮件，提交注释窗体，而无需任何用户交互的网络日志和其他网站。 ASP.NET AJAX 控件工具包中的 NoBot 控件可帮助防范这些机器人。


## <a name="overview"></a>概述

自动化的机器人石膏的垃圾邮件，提交注释窗体，而无需任何用户交互的网络日志和其他网站。 ASP.NET AJAX 控件工具包中的 NoBot 控件可帮助防范这些机器人。

## <a name="steps"></a>步骤

一种常见的方法来攻克机器人是使用 CAPTCHAs 完全自动公共执行测试来告诉计算机和理解相隔。 执行测试最初测试人需要用来确定通信合作伙伴是否用户或计算机。 在 web 中，验证码通常由以在其上某些失真字母的图像组成。 思路是仅用户可以读取在图像上的字母而 OCR 算法将失败。

有几个优点和缺点，这种方法，但本主题的讨论不在本教程的范围。 但是还有 ASP.NET AJAX 控件工具包提供类似的方法中的控件： `NoBot`。 它可以更轻松地克服比验证码，但非常易于使用以及如其中它被视为成功，如果大多数垃圾邮件尝试博客网站上的运行状况非常好的可为失败，其中`NoBot`控件可以执行操作。

`NoBot`截获当前的 ASP.NET web 窗体回的发，如果满足至少这些条件之一：

- 浏览器无法解决一个 JavaScript 难题 （例如当 JavaScript 停用）
- 用户提交到快速表单
- 客户端 IP 地址太通常在一段时间中提交了表单。

请检查这些条件，以便`NoBot`控件需要这些属性 （所有这些可选）：

- `ResponseMinimumDelaySeconds`最小量之间回发的秒数
- `CutoffWindowSeconds`从一个 IP 的回发中度量值的时间间隔的长度
- `CutoffMaximumInstances`每个时间间隔的秒的最大量

以下标记要求至少两个秒之间回发经过和有只有五回发或小于 30 秒间隔内：

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

然后照常确保包括`ScriptManager`在页中，以便加载 ASP.NET AJAX 库，并可以使用该控件工具包：

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

因为大多数检查`NoBot`正在执行的操作发生在服务器端中，你需要检查这些验证的结果。 这可以通过调用`NoBot`的`IsValid()`方法。 它具有一个自变量 (作为`out`参数 /`ByRef`参数) 它属于类型`NoBotState`。 如果检查失败，其字符串表示形式包含的原因和`Valid`否则为。 以下代码将输出一条消息，根据`NoBot`的结果：

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

最后，你需要一个用于提交窗体和 label 元素将消息输出，你已完成 ！

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

当运行此脚本和停用 JavaScript 或在前两秒内提交表单或在 30 秒内七次提交表单时，你将收到错误消息。 但是合理地使用此控件，因为只有大约 90 95%的用户具有激活的 JavaScript，因此将失败 5-10%的用户`NoBot`的测试。


[![此错误消息可能已引起 bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)

此错误消息可能已由 bot ([单击以查看实际尺寸的图像](fighting-bots-cs/_static/image3.png))

>[!div class="step-by-step"]
[下一篇](fighting-bots-vb.md)
