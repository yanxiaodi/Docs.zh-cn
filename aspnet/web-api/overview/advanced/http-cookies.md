---
uid: web-api/overview/advanced/http-cookies
title: "在 ASP.NET Web API 的 HTTP Cookie |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: e17c51946a268aa13ec035d18dc516928c9f4419
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="http-cookies-in-aspnet-web-api"></a>在 ASP.NET Web API 的 HTTP Cookie
====================
通过[Mike Wasson](https://github.com/MikeWasson)

本主题介绍如何发送和接收 Web API 中的 HTTP cookie。

## <a name="background-on-http-cookies"></a>HTTP Cookie 的背景信息

本部分提供了 cookie 在 HTTP 级别的实现方式的简要概述。 有关详细信息，请查阅[RFC 6265](http://tools.ietf.org/html/rfc6265)。

Cookie 是一种服务器发送的 HTTP 响应中的数据。 客户端 （可选） 将存储在 cookie 并根据 subsequet 请求对其进行返回。 这允许客户端和服务器共享状态。 若要设置一个 cookie，服务器，请在响应中包含集 Cookie 标头。 Cookie 的格式是名称 / 值对，具有可选特性。 例如: 

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

下面是包含属性的一个示例：

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

返回一个 cookie 到服务器，客户端包含 Cookie 标头中更高版本的请求。

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

HTTP 响应可以包括多个集 Cookie 标头。

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

客户端返回使用单个 Cookie 标头的多个 cookie。

[!code-console[Main](http-cookies/samples/sample5.cmd)]

作用域和持续时间 cookie 受集 Cookie 标头中的以下属性：

- **域**： 告知客户端的域应接收的 cookie。 例如，如果"example.com"域，则客户端会将 cookie 返回给每个子域 example.com。如果未指定，则域是源服务器。
- **路径**： 将 cookie 限制为在域中指定的路径。 如果未指定，则使用请求 URI 的路径。
- **过期**： 设置的 cookie 的到期日期。 当它过期时，客户端将删除 cookie。
- **最大有效期**： 设置的 cookie 的最长时间。 在到达最大存留期时，客户端将删除 cookie。

如果这两个`Expires`和`Max-Age`设置，`Max-Age`优先。 如果两者均未设置，客户端将在当前会话结束时删除 cookie。 （"会话"的确切含义确定用户代理中）。

但是，请注意客户端可能会忽略 cookie。 例如，用户可能会禁用隐私原因的 cookie。 客户端可能会删除 cookie 之前到期，或限制的存储的 cookie 数。 出于隐私原因，客户端通常拒绝"第三方"cookie，其中域与源服务器不匹配。 简单地说，服务器不应依赖于取回它所设置的 cookie 中。

## <a name="cookies-in-web-api"></a>Web API 中的 cookie

若要将 cookie 添加到 HTTP 响应中，创建**CookieHeaderValue**表示该 cookie 的实例。 然后调用**AddCookies**中定义的扩展方法**System.Net.Http。HttpResponseHeadersExtensions**类中，以添加该 cookie。

例如，下面的代码将添加一个 cookie 中的控制器操作：

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

请注意， **AddCookies**采用的数组**CookieHeaderValue**实例。

若要从客户端请求中提取 cookie，调用**GetCookies**方法：

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

A **CookieHeaderValue**包含一套**CookieState**实例。 每个**CookieState**表示一个 cookie。 使用索引器方法来获取**CookieState**按名称，如所示。

## <a name="structured-cookie-data"></a>结构化的 Cookie 数据

很多浏览器限制它们将存储的多少 cookie 和 #8212; 同时总数，以及每个域的数量。 因此，它可用于将结构化的数据置于一个 cookie，而不是设置多个 cookie。

> [!NOTE]
> RFC 6265 未定义 cookie 数据的结构。


使用**CookieHeaderValue**类，你可以将传递 cookie 数据的名称-值对的列表。 这些名称-值对被编码为 URL 编码格式集 Cookie 标头中的数据：

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

前面的代码生成以下集 Cookie 标头：

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

**CookieState**类提供了用于从请求消息的 cookie 中读取子值的索引器方法：

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>示例： 设置和检索在消息处理程序的 Cookie

前面的示例显示如何使用从 Web API 控制器中的 cookie。 另一个选项是使用[消息处理程序](http-message-handlers.md)。 在比控制器管道的前面部分中都调用消息处理程序。 消息处理程序可以读取请求的 cookie 之前请求到达控制器，, 或添加到响应的 cookie，控制器将生成响应后。

![](http-cookies/_static/image2.png)

下面的代码演示的消息处理程序创建的会话 Id。 会话 ID 存储在 cookie 中。 处理程序检查会话 cookie 的请求。 如果请求不包括该 cookie，该处理程序将生成一个新的会话 id。 在任一情况下，该处理程序存储中的会话 ID **HttpRequestMessage.Properties**属性包。 它还添加到 HTTP 响应的会话 cookie。

此实现不会验证服务器实际颁发从客户端的会话 ID。 不使用它为窗体的身份验证 ！ 该示例的点是显示 HTTP cookie 管理。

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

控制器可以获取会话 ID 从**HttpRequestMessage.Properties**属性包。

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
