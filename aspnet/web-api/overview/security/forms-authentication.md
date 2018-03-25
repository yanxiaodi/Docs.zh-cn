---
uid: web-api/overview/security/forms-authentication
title: "在 ASP.NET Web API 中窗体身份验证 |Microsoft 文档"
author: MikeWasson
description: "描述如何在 ASP.NET Web API 中使用 Forms 身份验证。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 9027d76bcf8854fc85f11906d3651511f350cd32
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="forms-authentication-in-aspnet-web-api"></a>ASP.NET Web API 中的窗体身份验证
====================
通过[Mike Wasson](https://github.com/MikeWasson)

窗体身份验证使用 HTML 窗体向服务器发送用户的凭据。 它不是一种 Internet 标准。 窗体身份验证才适用于 web 从 web 应用程序，调用的 Api，以便用户可以与 HTML 窗体进行交互。

| 优点 | 缺点 |
| --- | --- |
| -容易实现： 内置于 ASP.NET。 -使用 ASP.NET 成员资格提供程序，这样，就更容易地管理用户帐户。 | -不标准的 HTTP 身份验证机制;使用 HTTP cookie，而不是标准的 Authorization 标头。 -需要浏览器客户端。 的以纯文本形式发送凭据。 -容易受到跨网站请求伪造 (CSRF);需要反 CSRF 度量值。 -从 nonbrowser 客户端使用困难。 登录名需要浏览器。 用户凭据在请求中发送。 -某些用户禁用 cookie。 |

简而言之，在 ASP.NET 中的窗体身份验证工作原理如下：

1. 客户端请求需要身份验证的资源。
2. 如果用户未通过身份验证，服务器将返回 HTTP 302 （找到），而将重定向到登录页。
3. 用户输入凭据，并提交该表单。
4. 服务器返回另一个重定向回原始的 URI 的 HTTP 302。 此响应包含的身份验证 cookie。
5. 客户端再次请求资源。 请求包括身份验证 cookie，因此服务器授予请求。

![](forms-authentication/_static/image1.png)

有关详细信息，请参阅[概述的窗体身份验证。](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>使用 Web API 的窗体身份验证

若要创建使用窗体身份验证的应用程序，请在 MVC 4 项目向导中选择"Internet 应用程序"模板。 此模板创建 MVC 控制器用于帐户管理。 你还可以使用"单页面应用程序"模板，在 ASP.NET 秋季 2012年更新中提供。

在你的 web API 控制器，你可以通过使用限制访问`[Authorize]`特性，如中所述[使用 [Authorize] 属性](authentication-and-authorization-in-aspnet-web-api.md#auth3)。

窗体身份验证使用的会话 cookie 请求进行身份验证。 浏览器自动向目标网站发送所有相关 cookie。 此功能使窗体身份验证可能易受到跨站点请求伪造 (CSRF) 攻击，请参阅[防止跨站点请求伪造 (CSRF) 攻击](preventing-cross-site-request-forgery-csrf-attacks.md)。

窗体身份验证不加密用户的凭据。 因此，不安全的除非用于 SSL 窗体身份验证。 请参阅[使用 Web API 中的 SSL](working-with-ssl-in-web-api.md)。
