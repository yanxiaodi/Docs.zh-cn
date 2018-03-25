---
uid: web-api/overview/security/basic-authentication
title: "ASP.NET Web API 中的基本身份验证 |Microsoft 文档"
author: MikeWasson
description: "描述如何在 ASP.NET Web API 中使用基本身份验证。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4b8e6410668b2db289488bb4b6cd26d881e70f4c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="basic-authentication-in-aspnet-web-api"></a>ASP.NET Web API 中的基本身份验证
====================
通过[Mike Wasson](https://github.com/MikeWasson)

基本身份验证在中定义[RFC 2617、 HTTP 身份验证： 基本和摘要式访问身份验证](http://www.ietf.org/rfc/rfc2617.txt)。

缺点

- 用户凭据在请求中发送。
- 以纯文本形式发送凭据。
- 随每个请求一起发送凭据。
- 无法注销，除非通过结束浏览器会话。
- 易受到跨站点请求伪造 (CSRF);需要反 CSRF 度量值。

优点

- Internet 标准。
- 支持所有主要浏览器。
- 相对简单的协议。

基本身份验证工作原理如下：

1. 如果请求要求身份验证，则服务器将返回 401 （未授权）。 响应包含 Www-authenticate 标头，，该值指示服务器支持基本身份验证。
2. 客户端将发送另一个请求，授权标头中的客户端凭据。 凭据的格式设置为"名称： 密码"，base64 编码的字符串。 不加密凭据。

在"领域"。 的上下文中执行基本身份验证 服务器的 Www-authenticate 标头中包含的领域的名称。 用户的凭据是在该领域内有效。 领域的确切作用域由服务器定义。 例如，你可以按顺序对分区资源定义几个领域。

![](basic-authentication/_static/image1.png)

将凭据发送未加密，因为基本身份验证才安全通过 HTTPS。 请参阅[使用 Web API 中的 SSL](working-with-ssl-in-web-api.md)。

基本身份验证也是很容易受到 CSRF 攻击。 用户输入的凭据后，浏览器向自动发送它们在后续请求同一域中，会话的持续时间。 这包括 AJAX 请求。 请参阅[防止跨站点请求伪造 (CSRF) 攻击](preventing-cross-site-request-forgery-csrf-attacks.md)。

## <a name="basic-authentication-with-iis"></a>使用 IIS 基本身份验证

IIS 支持基本身份验证，但需要注意： 用户针对其 Windows 凭据进行身份验证。 这意味着用户必须具有与服务器的域帐户。 面向公众的网站，你通常想要针对 ASP.NET 成员资格提供程序进行身份验证。

若要启用基本身份验证使用 IIS，请将身份验证模式设置为 ASP.NET 项目的 Web.config 中的"Windows":

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

在此模式下，IIS 使用 Windows 凭据进行身份验证。 此外，你必须启用 IIS 中的基本身份验证。 在 IIS 管理器中，转到功能视图、 选择身份验证，并启用基本身份验证。

![](basic-authentication/_static/image2.png)

在 Web API 项目中，添加`[Authorize]`所的任何控制器操作需要身份验证的属性。

客户端自行进行身份验证请求中设置的 Authorization 标头。 浏览器客户端自动执行此步骤。 Nonbrowser 客户端将需要设置标头。

## <a name="basic-authentication-with-custom-membership"></a>使用自定义成员资格基本身份验证

如前文所述，内置于 IIS 基本身份验证使用 Windows 凭据。 这意味着你需要的宿主服务器上创建你的用户帐户。 但对于 internet 应用程序，用户帐户通常存储在外部数据库。

下面的代码如何执行基本身份验证的 HTTP 模块。 您可以轻松插入的 ASP.NET 成员资格提供程序中通过将`CheckPassword`方法，这是在此示例中的虚拟方法。

在 Web API 2 中，你应考虑编写[身份验证筛选器](authentication-filters.md)或[OWIN 中间件](../../../aspnet/overview/owin-and-katana/index.md)，而不是 HTTP 模块。

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

若要启用 HTTP 模块，将以下代码添加到 web.config 文件中**system.webServer**部分：

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

"程序集名称"替换 （不包括"dll"扩展） 的程序集的名称。

应禁用其他身份验证方案，如窗体或 Windows 验证
