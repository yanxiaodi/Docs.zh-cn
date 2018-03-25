---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: "阻止 JavaScript 注入攻击 (VB) |Microsoft 文档"
author: StephenWalther
description: "防止 JavaScript 注入式攻击和跨站点脚本攻击发生给您。 在本教程中，Stephen Walther 解释了如何可以轻松地 de..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: 1d49d4d1afa30247d3452a96c8004441ba417ac8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="preventing-javascript-injection-attacks-vb"></a>阻止 JavaScript 注入攻击 (VB)
====================
通过[Stephen Walther](https://github.com/StephenWalther)

[下载 PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> 防止 JavaScript 注入式攻击和跨站点脚本攻击发生给您。 在本教程中，Stephen Walther 解释了如何轻松地让这些类型的通过 HTML 编码内容的攻击。


本教程旨在说明如何在 ASP.NET MVC 应用程序阻止 JavaScript 注入式攻击。 本教程讨论了两种方法来保护你的网站对 JavaScript 注入攻击。 了解如何通过编码的数据，将其显示阻止 JavaScript 注入式攻击。 你还了解了如何通过编码你接受的数据，防止 JavaScript 注入攻击。

## <a name="what-is-a-javascript-injection-attack"></a>什么是 JavaScript 注入式攻击？

无论何时你接受用户输入，并重新显示用户输入，你将打开到 JavaScript 注入式攻击你的网站。 让我们检查的具体的应用程序可能会受到 JavaScript 注入式攻击。

假设你已创建客户反馈网站 （请参见图 1）。 客户可以访问的网站，并输入使用您的产品其体验的反馈。 客户提交时将这些反馈，反馈将重新显示在反馈页上。


[![客户反馈网站](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**图 01**： 客户反馈网站 ([单击以查看实际尺寸的图像](preventing-javascript-injection-attacks-vb/_static/image3.png))


客户反馈网站使用`controller`列表 1 中。 这`controller`包含名为的两个操作`Index()`和`Create()`。

**列表 1 –`HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

`Index()`方法显示`Index`视图。 此方法将传递所有以前的客户反馈到`Index`通过 （使用 LINQ to SQL 查询） 从数据库检索反馈的视图。

`Create()`方法创建一个新的反馈项并将其添加到数据库。 在窗体中输入客户的消息传递给`Create()`消息参数中的方法。 将创建一个反馈项和消息分配给反馈项`Message`属性。 反馈项提交到与数据库`DataContext.SubmitChanges()`方法调用。 最后，在距访客重定向回`Index`视图显示所有的反馈。

`Index`视图包含在清单 2。

**列出 2 –`Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

`Index`视图具有两个部分。 顶部的部分包含的实际客户反馈表单。 下半部分包含一个 For...每个循环循环遍历所有以前的客户反馈项并显示每个反馈项的 EntryDate 和消息属性。

客户反馈网站是一个简单的网站。 遗憾的是，网站是 JavaScript 注入攻击。

假设在客户反馈表单中输入以下文本：

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

此文本表示显示警报消息框中的 JavaScript 脚本。 某人为反馈提交此脚本后窗体中，消息*Boo ！*将显示时的任何人访问客户反馈网站将来 （请参见图 2）。


[![JavaScript 注入](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**图 02**: JavaScript 注入 ([单击以查看实际尺寸的图像](preventing-javascript-injection-attacks-vb/_static/image6.png))


现在，您对 JavaScript 注入式攻击的初始响应可能 apathy。 您可能会认为 JavaScript 注入式攻击是只需一类型的*篡改*攻击。 你可能会认为，没有人可以任何操作真正恶魔通过提交 JavaScript 注入攻击。

遗憾的是，黑客可以执行一些实际上，通过将 JavaScript 注入到网站的实际恶意操作。 JavaScript 注入攻击可用于执行跨站点脚本 (XSS) 攻击。 在跨站点脚本攻击中，你将窃取用户机密信息，并将信息发送到另一个网站。

例如，黑客可以使用 JavaScript 注入攻击来窃取来自其他用户的浏览器 cookie 的值。 如果敏感信息-如密码、 信用卡号或身份证号 – 存储在浏览器 cookie，然后黑客可以使用 JavaScript 注入攻击来窃取此信息。 或者，如果用户在已受威胁与 JavaScript 攻击页中包含窗体字段中输入敏感信息，然后黑客可以使用插入的 JavaScript 获取窗体数据并将其发送到另一个网站。

*请将害怕*。 认真对待 JavaScript 注入式攻击和保护用户的机密信息。 在接下来的两部分中，我们将讨论你可以使用来保护从 JavaScript 注入式攻击你的 ASP.NET MVC 应用程序的两种技术。

## <a name="approach-1-html-encode-in-the-view"></a>方法 #1: HTML 编码的视图中

一种阻止 JavaScript 注入攻击的简单方法是 html 编码在重新显示在视图中的数据时，网站用户输入的任何数据。 已更新`Index`列出 3 中的视图遵循这种方法。

**列出 3 – `Index.aspx` (HTML 编码)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

请注意，值`feedback.Message`为 HTML 编码之前的值显示替换为以下代码：

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

所带来的平均值为 HTML 编码字符串？ 一个字符串进行编码时 HTML，危险字符如`<`和`>`如 HTML 实体引用替换为`&lt;`和`&gt;`。 因此，在字符串`<script>alert("Boo!")</script>`为 HTML 编码，它将转换为`&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`。 为 JavaScript 脚本由浏览器解释时不再执行编码的字符串。 相反，图 3 中获得无害的页。


[![失效的 JavaScript 攻击](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**图 03**： 失效的 JavaScript 攻击 ([单击以查看实际尺寸的图像](preventing-javascript-injection-attacks-vb/_static/image9.png))


请注意，在`Index`查看列出 3 中的值`feedback.Message`进行编码。 值`feedback.EntryDate`未编码。 只需对用户输入的数据进行编码。 因为 EntryDate 的值在与控制器中生成时，你不需要为 HTML 编码此值。

## <a name="approach-2-html-encode-in-the-controller"></a>方法 2： 在控制器中编码的 HTML

而不是 HTML 编码数据在视图中显示数据时，你可以 HTML 提交到数据库的数据之前对数据进行编码。 此第二种方法均的情况下`controller`列出 4 中。

**列出 4 – `HomeController.cs` (HTML 编码)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

请注意，消息的值是 HTML 编码值提交到该数据库在之前`Create()`操作。 当消息将重新显示在视图中时，消息为 HTML 编码，并且不执行任何插入的消息中的 JavaScript。

通常情况下，你应该倾向于通过此第二种方法在本教程中讨论的第一个方法。 此第二种方法的问题是，你结束，HTML 编码数据在数据库中。 换而言之，你数据库的数据是变脏与有趣查找的字符。

为什么这是错误的？ 如果您需要在以外的网页中显示的数据库数据，则将遇到问题。 例如，可以在 Windows 窗体应用程序不再方便地显示数据。

## <a name="summary"></a>摘要

本教程的目的是让你有关 JavaScript 注入攻击的潜在客户。 本教程讨论防护 JavaScript 注入式攻击 ASP.NET MVC 应用程序的两种方法： 可以是 HTML 编码用户提交中的视图或你的数据可以 HTML 编码用户提交在控制器中的数据。

>[!div class="step-by-step"]
[上一篇](authenticating-users-with-windows-authentication-vb.md)
