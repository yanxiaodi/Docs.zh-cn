---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: "身份验证和 ASP.NET Web API 中的授权 |Microsoft 文档"
author: MikeWasson
description: "ASP.NET Web API 中提供了身份验证和授权的常规概述。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/27/2012
ms.topic: article
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9d7cbb9505afb6461ba4c2087d57e9ea0da38ede
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a>身份验证和 ASP.NET Web API 中的授权
====================
通过[Mike Wasson](https://github.com/MikeWasson)

您已经创建 web API，但现在你想要控制对它的访问。 在此系列文章中，我们将查看从未经授权的用户保护 web API 的一些选项。 这一系列将介绍身份验证和授权。

- *身份验证*知道用户的标识。 例如，Alice 使用她的用户名和密码，登录和服务器使用密码进行身份验证 Alice。
- *授权*确定是否允许用户执行操作。 例如，Alice 已获取资源，但不是创建资源的权限。

序列中的第一篇文章提供了 ASP.NET Web API 中的身份验证和授权的常规概述。 其他主题说明为 Web API 的常见身份验证方案。

> [!NOTE]
> 感谢人都可以查看这一系列并提供有价值的反馈： Rick Anderson、 Levi Broderick、 Barry Dorrans、 Tom Dykstra、 Hongmei Ge、 David Matson、 Daniel Roth、 Tim Teebken。


## <a name="authentication"></a>身份验证

Web API 假定该身份验证发生在主机。 对于 web 承载的该主机是 IIS，使用 HTTP 模块进行身份验证。 你可以配置你的项目中使用任何内置于 IIS 或 ASP.NET 中，身份验证模块或编写自己的 HTTP 模块来执行自定义身份验证。

当主机对用户进行身份验证时，它将创建*主体*，即[IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx)表示运行代码的安全上下文的对象。 主机将主体附加到当前线程，通过设置**Thread.CurrentPrincipal**。 主体包含一个关联**标识**对象，其中包含有关用户的信息。 如果用户进行身份验证， **Identity.IsAuthenticated**属性返回**true**。 对于匿名请求， **IsAuthenticated**返回**false**。 有关主体的详细信息，请参阅[基于角色的安全性](https://msdn.microsoft.com/library/shz8h065.aspx)。

### <a name="http-message-handlers-for-authentication"></a>HTTP 消息处理程序进行身份验证

而不是使用进行身份验证的主机，你可以将身份验证逻辑注入[HTTP 消息处理程序](../advanced/http-message-handlers.md)。 在这种情况下，消息处理程序将检查 HTTP 请求和设置的主体。

如果应将消息处理程序用于身份验证？ 下面是一些的折衷方案：

- HTTP 模块会看到通过 ASP.NET 管道的所有请求。 消息处理程序只看到路由到 Web API 的请求。
- 你可以设置每个路由消息处理程序，，这样就可以应用于特定的路由的身份验证方案。
- HTTP 模块是特定于 IIS。 消息处理程序是主机无关，因此它们可以用于 web 承载和自承载。
- HTTP 模块参与 IIS 日志记录、 审核和等等。
- HTTP 模块运行之前在管道中。 如果处理消息处理程序中的身份验证时，主体不获取或设置处理程序运行之前。 此外，主体数据库将恢复到以前的主体时响应离开的消息处理程序。

通常，如果你不需要支持自承载，则 HTTP 模块是更好的选择。 如果你需要支持自承载，请考虑消息处理程序。

### <a name="setting-the-principal"></a>设置主体

如果你的应用程序执行的任何自定义身份验证逻辑，则必须在两个位置上设置主体：

- **Thread.CurrentPrincipal**。 此属性是在.NET 中设置线程的主体的标准方式。
- **HttpContext.Current.User**. 此属性是特定于 ASP.NET。

下面的代码演示如何设置主体：

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

对于 web 承载的您必须在这两个位置; 设置主体否则的安全上下文可能会变得不一致。 对于自承载，但是， **HttpContext.Current**为 null。 若要确保你的代码是不可知的主机，因此，null 之前检查向分配**HttpContext.Current**，如所示。

## <a name="authorization"></a>授权

授权发生更高版本在管道中，更接近于控制器。 该视图使你作出更精细的选择，如果您授予对资源的访问。

- *授权筛选器*之前的控制器操作运行。 如果请求未被授权，筛选器将返回错误响应，并且未调用的操作。
- 中的控制器操作，你可以从当前主体**ApiController.User**属性。 例如，您可以筛选基于用户名称的资源的列表返回只能属于该用户的资源。

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>使用 [授权] 属性

Web API 提供了内置授权筛选器， [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx)。 此筛选器检查用户进行身份验证。 否则，它将返回 HTTP 状态代码 401 （未经授权），而无需调用该操作。

你可以应用筛选器全局范围内，在控制器级别，或各个操作级别。

**全局**： 若要限制访问每个 Web API 控制器，请添加**AuthorizeAttribute**到全局筛选器列表的筛选器：

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**控制器**： 若要限制为特定控制器的访问，筛选器作为属性添加到控制器：

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**操作**： 来限制对特定操作，请将属性添加到操作方法：

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

或者，你可以限制控制器，然后通过使用允许匿名访问特定操作`[AllowAnonymous]`属性。 在下面的示例中，`Post`方法是受限制，但`Get`方法允许匿名访问。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

在前面的示例中，筛选器允许任何经过身份验证的用户才能访问受限制的方法;仅匿名用户将保留。您还可以限制对特定用户或特定角色中的用户访问权限：

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> **AuthorizeAttribute** Web API 控制器的筛选器位于**System.Web.Http**命名空间。 没有用于 MVC 控制器中的类似筛选器**System.Web.Mvc**命名空间，与 Web API 控制器不兼容。


### <a name="custom-authorization-filters"></a>自定义授权筛选器

若要编写自定义授权筛选器，派生自这些类型之一：

- **AuthorizeAttribute**。 可扩展此类以执行基于当前用户和用户的角色的授权逻辑。
- **AuthorizationFilterAttribute**。 可扩展此类以执行在当前用户或角色不一定基于同步的授权逻辑。
- **IAuthorizationFilter**. 实现此接口可执行异步授权逻辑;例如，如果你的授权逻辑执行异步 I/O 或网络调用。 (如果你的授权逻辑是 CPU 绑定的它会更易于派生自**AuthorizationFilterAttribute**，因为不需要编写的异步方法。)

下图显示的类层次结构**AuthorizeAttribute**类。

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>内的控制器操作的授权

在某些情况下，可能允许继续，但更改基于主体的行为的请求。 例如，返回的信息可能会更改具体取决于用户的角色。 在控制器方法中，你可以从当前的原则**ApiController.User**属性。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
