---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: "阻止 ASP.NET Web API 中的跨网站请求伪造 (CSRF) 攻击 |Microsoft 文档"
author: MikeWasson
description: "描述跨网站请求伪造 (CSRF) 攻击以及如何在 ASP.NET Web API 中实现反 CSRF 度量值。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: 1cd03f3b396cc2ece1d8dbe6820f6277c02d8e62
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-web-api"></a>阻止 ASP.NET Web API 中的跨网站请求伪造 (CSRF) 攻击
====================
通过[Mike Wasson](https://github.com/MikeWasson)

跨站点请求伪造 (CSRF) 是一种攻击，恶意网站将请求发送到在当前登录用户的易受攻击站点的其中

下面是 CSRF 攻击的示例：

1. 用户登录到 www.example.com 时，使用窗体身份验证。
2. 服务器对用户进行身份验证。 来自服务器的响应包括身份验证 cookie。
3. 且无需注销，用户访问恶意网站。 此恶意站点包含的以下 HTML 窗体： 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    请注意，窗体操作发送到易受攻击的站点，不适用于恶意站点。 这是 CSRF 的"跨站点"部分。
4. 用户可单击提交按钮。 浏览器包括与请求的身份验证 cookie。
5. 请求与用户的身份验证上下文，在服务器上运行，并可以执行任何身份验证的用户可以执行的操作。

虽然此示例需要用户单击窗体按钮，恶意页差不多一样轻松地运行自动提交该表单的脚本。 此外，使用 SSL 不会阻止 CSRF 攻击中，因为恶意站点可以发送"https://"请求。

通常情况下，CSRF 攻击是可能对网站进行身份验证，使用 cookie 的因为浏览器向目标网站发送所有相关 cookie。 但是，CSRF 攻击并不局限于利用 cookie。 例如，基本和摘要式身份验证也是易受攻击的。 之后使用基本或摘要式身份验证的用户登录。 浏览器会自动发送凭据，直到会话结束。

## <a name="anti-forgery-tokens"></a>防伪令牌

为了帮助防止 CSRF 攻击，ASP.NET MVC 使用防伪令牌，也称为*请求验证令牌*。

1. 客户端请求一个包含窗体的 HTML 页面。
2. 服务器在响应中包含两个令牌。 一个标记是作为 cookie 发送。 其他放置在隐藏的表单字段中。 以便攻击者无法猜测值，将随机生成令牌。
3. 当客户端提交表单时，它必须发送两种令牌返回到服务器。 客户端发送的 cookie 令牌作为 cookie，然后发送窗体数据内的窗体标记。 （浏览器客户端会自动完成此用户提交表单时。）
4. 如果请求不包括两种令牌，服务器不允许该请求。

下面是与隐藏的表单令牌以 HTML 形式的示例：

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

防伪令牌，因为恶意页无法读取用户的令牌，由于同源策略工作。 ([同源策略](http://www.w3.org/Security/wiki/Same_Origin_Policy)阻止访问彼此的内容的两个不同站点上托管的文档。 因此，在前面的示例中，恶意的页可以将请求发送到 example.com，但它无法读取响应。）

若要阻止 CSRF 攻击，请使用任何身份验证协议使用防伪令牌由浏览器以无提示方式发送凭据后在用户登录。 这包括基于 cookie 的身份验证协议，例如窗体身份验证，以及等基本和摘要式身份验证协议。

你应要求防伪令牌将用于任何 nonsafe 方法 （POST、 PUT、 DELETE）。 此外，请确保安全的方法 （GET、 HEAD） 没有任何副作用。 此外，如果您启用跨域支持，如 CORS 或 JSONP，则 GET 等甚至安全方法都可能容易受到 CSRF 攻击，从而允许攻击者能够读取可能敏感的数据。

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>ASP.NET mvc 防伪令牌

若要将防伪令牌添加到 Razor 页中，使用**HtmlHelper.AntiForgeryToken**帮助器方法：

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

此方法添加隐藏的表单字段，并且还设置 cookie 的标记。

## <a name="anti-csrf-and-ajax"></a>反 CSRF 和 AJAX

窗体标记可能 AJAX 请求的问题，因为 AJAX 请求可能会发送 JSON 数据，不 HTML 窗体数据。 一种解决方案是将令牌发送自定义的 HTTP 标头中。 下面的代码使用 Razor 语法生成令牌，，然后将令牌添加到 AJAX 请求。 在服务器通过调用生成令牌**AntiForgery.GetTokens**。

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

处理请求时，请从请求标头中提取令牌。 然后调用**AntiForgery.Validate**方法来验证令牌。 **验证**方法引发异常，如果令牌无效。

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
