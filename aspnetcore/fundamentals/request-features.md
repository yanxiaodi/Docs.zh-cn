---
title: "ASP.NET Core 中的请求功能"
author: ardalis
description: "了解与 ASP.NET Core 的接口中定义的 HTTP 请求和响应相关的 Web 服务器实现详细信息。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/request-features
ms.openlocfilehash: c79ad6001e106a3e3104b0f804a386fe8b0ee30a
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2018
---
# <a name="request-features-in-aspnet-core"></a>ASP.NET Core 中的请求功能

作者：[Steve Smith](https://ardalis.com/)

与 HTTP 请求和响应相关的 Web 服务器实现详细信息在接口中定义。 服务器实现和中间件使用这些接口来创建和修改应用程序的托管管道。

## <a name="feature-interfaces"></a>功能接口

ASP.NET Core 在 `Microsoft.AspNetCore.Http.Features` 中定义了许多 HTTP 功能接口，服务器使用这些接口来标识其支持的功能。 以下功能接口处理请求并返回响应：

`IHttpRequestFeature` 定义 HTTP 请求的结构，包括协议、路径、查询字符串、标头和正文。

`IHttpResponseFeature` 定义 HTTP 响应的结构，包括状态代码、标头和响应的正文。

`IHttpAuthenticationFeature` 定义支持基于 `ClaimsPrincipal` 来标识用户并指定身份验证处理程序。

`IHttpUpgradeFeature` 定义对 [HTTP 升级](https://tools.ietf.org/html/rfc2616.html#section-14.42)的支持，允许客户端指定在服务器需要切换协议时要使用的其他协议。

`IHttpBufferingFeature` 定义禁用请求和/或响应缓冲的方法。

`IHttpConnectionFeature` 为本地和远程地址以及端口定义属性。

`IHttpRequestLifetimeFeature` 定义支持中止连接，或者检测是否已提前终止请求（如由于客户端断开连接）。

`IHttpSendFileFeature` 定义异步发送文件的方法。

`IHttpWebSocketFeature` 定义支持 Web 套接字的 API。

`IHttpRequestIdentifierFeature` 添加一个可以实现的属性来唯一标识请求。

`ISessionFeature` 为支持用户会话定义 `ISessionFactory` 和 `ISession` 抽象。

`ITlsConnectionFeature` 定义用于检索客户端证书的 API。

`ITlsTokenBindingFeature` 定义使用 TLS 令牌绑定参数的方法。

> [!NOTE]
> `ISessionFeature` 不是服务器功能，而是由 `SessionMiddleware` 实现（请参阅[管理应用程序状态](app-state.md)）。

## <a name="feature-collections"></a>功能集合

`HttpContext` 的 `Features` 属性为获取和设置当前请求的可用 HTTP 功能提供了一个接口。 由于功能集合即使在请求的上下文中也是可变的，所以可使用中间件来修改集合并添加对其他功能的支持。

## <a name="middleware-and-request-features"></a>中间件和请求功能

虽然服务器负责创建功能集合，但中间件既可以添加到该集合中，也可以使用集合中的功能。 例如，`StaticFileMiddleware` 访问 `IHttpSendFileFeature` 功能。 如果该功能存在，则用于从其物理路径发送所请求的静态文件。 否则，使用较慢的替代方法来发送文件。 如果可用，`IHttpSendFileFeature` 允许操作系统打开文件并执行直接内核模式复制到网卡。

另外，中间件可以添加到由服务器建立的功能集合中。 中间件甚至可以取代现有的功能，以便增加服务器的功能。 添加到集合中的功能稍后将在请求管道中立即用于其他中间件或基础应用程序本身。

通过结合自定义服务器实现和特定的中间件增强功能，可构造应用程序所需的精确功能集。 这样一来，无需更改服务器即可添加缺少的功能，并确保只公开最少的功能，从而限制攻击外围应用并提高性能。

## <a name="summary"></a>摘要

功能接口定义给定请求可能支持的特定 HTTP 功能。 服务器定义功能的集合，以及该服务器支持的初始功能集，但中间件可用于增强这些功能。

## <a name="additional-resources"></a>其他资源

* [服务器](xref:fundamentals/servers/index)
* [中间件](xref:fundamentals/middleware/index)
* [.NET 的开放 Web 接口 (OWIN)](xref:fundamentals/owin)
