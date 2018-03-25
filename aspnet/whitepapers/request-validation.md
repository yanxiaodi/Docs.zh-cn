---
uid: whitepapers/request-validation
title: "请求验证-阻止脚本攻击 |Microsoft 文档"
author: rick-anderson
description: "本白皮书介绍了请求验证功能的位置，默认情况下，应用程序允许处理未编码的 HTML 内容 submitt 的 ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0b24fe2193d2c7a858667505bad9ed0b1d70a328
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="request-validation---preventing-script-attacks"></a>请求验证-阻止脚本攻击
====================
> 本白皮书介绍了其中，默认情况下，应用程序允许处理提交到服务器的未编码的 HTML 内容的 ASP.NET 请求验证功能。 在设计应用程序安全地处理 HTML 数据时，可以禁用此请求验证功能。
> 
> 适用于 ASP.NET 1.1 和 ASP.NET 2.0。


请求验证的 ASP.NET 1.1 版中，由于一项功能将阻止服务器接受内容包含未编码 HTML。 此功能旨在帮助防止凭此客户端脚本代码或 HTML 可以不知情的情况下提交到服务器、 存储，以及随后呈现给其他用户的一些脚本注入攻击。 我们仍强烈建议你验证所有输入的数据和 HTML 对其在适当的时候进行编码。

例如，你可以创建请求用户的电子邮件地址，然后将该电子邮件地址在数据库中的存储的网页。 如果用户输入&lt;脚本&gt;警报 （"你好从脚本"）&lt;/&gt;而不是有效的电子邮件地址，已提交该数据，此脚本可以执行如果内容未正确编码。 ASP.NET 的请求验证功能可以避免这种情况发生。

## <a name="why-this-feature-is-useful"></a>此功能是有用的原因

许多站点不知道它们是简单的脚本注入攻击。 无论这些攻击的目的是破坏站点通过显示 HTML，或可能执行客户端脚本可以将用户重定向到黑客的站点，脚本注入式攻击都是 Web 开发人员必须与争用问题。

脚本注入攻击是需考虑的所有 web 开发人员，无论他们正在使用 ASP.NET、 ASP 或其他 web 开发技术。

ASP.NET 请求验证功能主动会阻止通过不允许未编码的 HTML 内容，除非开发人员决定允许该内容由服务器处理的这些攻击。

## <a name="what-to-expect-error-page"></a>希望得到什么： 错误页

下面的屏幕快照显示了一些示例 ASP.NET 代码：

![](request-validation/_static/image1.png)

上运行此代码生成的简单页面，您可以在文本框中输入一些文本，单击按钮，并在标签控件中显示的文本：

![](request-validation/_static/image2.png)

但是，只能在 JavaScript 中，如`<script>alert("hello!")</script>`输入并提交我们将收到异常：

![](request-validation/_static/image3.png)

错误消息指出潜在的危险 Request.Form 的检测到值并提供更多详细信息中并完全发生了什么以及如何更改的行为的说明。 例如:

请求验证程序检测到潜在危险的客户端的输入的值，并处理的请求已中止。 此值可能指示尝试危及安全的应用程序，如跨站点脚本攻击。 你可以通过设置来禁用请求验证`validateRequest=false`Page 指令中或在配置部分。 但是，强烈建议，你的应用程序显式检查所有的输入内容在这种情况下。

## <a name="disabling-request-validation-on-a-page"></a>禁用页面上的请求验证

若要禁用请求验证页，您必须设置`validateRequest`属性的页指令`false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> 当禁用请求验证时，内容可以提交到页;它是页开发人员，以确保该内容的责任是正确编码或处理。

## <a name="disabling-request-validation-for-your-application"></a>禁用请求验证你的应用程序

若要禁用请求验证你的应用程序，必须修改或创建你的应用程序的 Web.config 文件并设置的 validateRequest 属性`<pages />`部分到`false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

如果你想要禁用请求验证你的服务器上的所有应用程序，你可以进行这种修改 Machine.config 文件。

> [!CAUTION]
> 当禁用请求验证时，内容可以提交到你的应用程序;它是应用程序开发人员，以确保该内容的责任是正确编码或处理。

下面的代码进行修改来关闭请求验证：

![](request-validation/_static/image4.png)

现在，如果文本框中输入以下 JavaScript`<script>alert("hello!")</script>`结果将是：

![](request-validation/_static/image5.png)

若要防止这类情况发生，使用请求验证已关闭，我们需要到 HTML 对内容进行编码。

## <a name="how-to-html-encode-content"></a>如何为 HTML 编码内容

如果你已禁用请求验证，它是对 HTML 编码的内容将存储供将来使用的好做法。 HTML 编码将自动替换任何&lt;'或'&gt;（与其他几个符号） 一起使用的其相应的 HTML 编码表示形式。 例如，&lt;替换为&amp;lt; 和&gt;替换为&amp;gt;。 浏览器使用这些特殊的代码来显示&lt;'或'&gt;在浏览器中。

内容可以轻松地在服务器使用编码 HTML `Server.HtmlEncode(string)` API。 内容也可以轻松地 HTML 解码，也就是说，就会返回到标准 HTML 使用`Server.HtmlDecode(string)`方法。

![](request-validation/_static/image6.png)

导致：

![](request-validation/_static/image7.png)
