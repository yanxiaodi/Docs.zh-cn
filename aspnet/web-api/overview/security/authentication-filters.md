---
uid: web-api/overview/security/authentication-filters
title: "ASP.NET Web API 2 中的身份验证筛选器 |Microsoft 文档"
author: MikeWasson
description: "身份验证筛选器是对 HTTP 请求进行身份验证的组件。 Web API 2 和 MVC 5 都支持身份验证筛选器，但它们略有不同..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/25/2014
ms.topic: article
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: 16e451f52799625983368bc938091eff47019b52
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>ASP.NET Web API 2 中的身份验证筛选器
====================
通过[Mike Wasson](https://github.com/MikeWasson)

> 身份验证筛选器是对 HTTP 请求进行身份验证的组件。 Web API 2 和 MVC 5 都支持身份验证筛选器，但它们略有不同，主要是在筛选器接口的命名约定。 本主题描述了 Web API 身份验证筛选器。


身份验证筛选器使你能够为各个控制器或操作设置身份验证方案。 这样一来，你的应用程序可以为不同的 HTTP 资源支持不同的身份验证机制。

在本文中，我将展示代码从[基本身份验证](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt)示例上[http://aspnet.codeplex.com](http://aspnet.codeplex.com)。此示例演示实现 HTTP 基本访问身份验证方案 (RFC 2617) 的身份验证筛选器。 名为类中实现筛选器`IdentityBasicAuthenticationAttribute`。 我不会显示所有的示例代码演示如何编写一个身份验证筛选器中的部分。

## <a name="setting-an-authentication-filter"></a>将身份验证筛选器设置

与其他过滤器一样身份验证筛选器可以应用的每个控制器，每个操作或全局范围内为所有 Web API 控制器。

若要应用到的控制器的身份验证筛选器，修饰具有筛选器属性的控制器类。 下面的代码设置`[IdentityBasicAuthentication]`上了控制器类，它使所有控制器的操作的基本身份验证的筛选器。

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

若要应用到一个操作筛选器，请使用筛选器修饰操作。 下面的代码设置`[IdentityBasicAuthentication]`在控制器上的筛选器`Post`方法。

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

若要将筛选器应用于所有的 Web API 控制器，将其添加到**GlobalConfiguration.Filters**。

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>实现 Web API 身份验证筛选器

在 Web API 中，身份验证筛选器实现[System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx)接口。 它们还应从继承**System.Attribute**，以便作为属性被应用。

**IAuthenticationFilter**接口有两种方法：

- **AuthenticateAsync**正在验证在请求中，凭据进行身份验证请求，如果存在。
- **ChallengeAsync** HTTP 响应中，如果需要添加身份验证质询。

这些方法对应于在中定义的身份验证流[RFC 2612](http://tools.ietf.org/html/rfc2616)和[RFC 2617](http://tools.ietf.org/html/rfc2617):

1. 客户端将 Authorization 标头中发送凭据。 这通常发生在客户端从服务器收到 401 （未经授权） 响应之后。 但是，客户端可以发送凭据的任何请求，而不仅仅是后获取 401。
2. 如果服务器不接受凭据，则将返回 401 （未经授权） 响应。 响应包含 Www-authenticate 标头，包含一个或多个挑战。 每个质询指定服务器识别身份验证方案。

服务器还可以通过匿名请求返回 401。 事实上，这通常是如何启动身份验证过程：

1. 客户端发送的匿名请求。
2. 服务器将返回 401。
3. 客户端重新发送凭据的请求。

此流同时包含*身份验证*和*授权*步骤。

- 身份验证证明客户端的标识。
- 授权确定客户端是否可以访问特定资源。

在 Web API 中，身份验证筛选器处理身份验证，但未授权。 应完成授权，授权筛选器或内的控制器操作。

下面是在 Web API 2 管道中的流：

1. 在调用操作之前, Web API 创建针对该操作的身份验证筛选器列表。 这包括操作作用域、 控制器作用域和全局范围的筛选的器。
2. Web API 调用**AuthenticateAsync**列表中每个筛选器。 每个筛选器来验证请求中的凭据。 如果任何筛选器已成功验证凭据，筛选器创建**IPrincipal**并将其附加到请求。 筛选器还可以在此时触发错误。 如果是这样，则不运行的管道的其余部分。
3. 假设没有错误，请求流经管道的其余部分。
4. 最后，Web API 调用每个身份验证筛选器的**ChallengeAsync**方法。 筛选器使用此方法将一项挑战添加到响应中，如果需要。 通常 （但不是总是），将发生在 401 错误响应。

下图显示了两种可能的情况。 在第一个，身份验证筛选器已成功验证该请求、 授权筛选器将为请求，授权和控制器操作返回 200 （正常）。

![](authentication-filters/_static/image1.png)

在第二个示例中，身份验证筛选器进行身份验证请求，但授权筛选器返回 401 （未授权）。 在这种情况下，不调用控制器操作。 身份验证筛选器将 Www-authenticate 标头添加到响应。

![](authentication-filters/_static/image2.png)

其他组合可能会出现&mdash;例如，如果控制器操作允许匿名请求，你可能有一个身份验证的筛选器，但没有授权。

## <a name="implementing-the-authenticateasync-method"></a>实现 AuthenticateAsync 方法

**AuthenticateAsync**方法尝试进行身份验证请求。 下面是方法签名：

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

**AuthenticateAsync**方法必须执行下列操作之一：

1. 执行任何操作 （无操作）。
2. 创建**IPrincipal**并将其设置针对此请求。
3. 设置了错误的结果。

选项 (1) 表示请求中未包含任何筛选器理解的凭据。 选项 (2) 意味着筛选器成功通过身份验证请求。 选项 (3) 意味着请求具有无效凭据、 （如错误的密码），随即将会触发错误响应。

下面是用于实现的通用大纲**AuthenticateAsync**。

1. 查找在请求中的凭据。
2. 如果不有任何凭据，则不执行任何操作并返回 （无操作）。
3. 如果凭据，但筛选器无法识别的身份验证方案，不执行任何操作并返回 （无操作）。 管道中的另一个筛选器可能理解此方案。
4. 如果有筛选器理解的凭据，请尝试对它们进行身份验证。
5. 如果凭据错误，请通过设置返回 401 `context.ErrorResult`。
6. 如果凭据有效，创建**IPrincipal**并设置`context.Principal`。

以下代码所示**AuthenticateAsync**方法从[基本身份验证](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt)示例。 注释中描述每个步骤。 代码演示几种类型的错误： 不带凭据、 格式不正确的凭据和错误的用户名/密码的 Authorization 标头。

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>设置了错误的结果

如果凭据无效，必须设置筛选器`context.ErrorResult`到**IHttpActionResult**创建错误响应。 有关详细信息**IHttpActionResult**，请参阅[Web API 2 中的操作结果](../getting-started-with-aspnet-web-api/action-results.md)。

基本身份验证示例包括`AuthenticationFailureResult`适合于此目的的类。

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>实现 ChallengeAsync

用途**ChallengeAsync**方法是将添加身份验证质询的响应，如果需要。 下面是方法签名：

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

在请求管道中每个身份验证筛选器上调用方法。

务必了解， **ChallengeAsync**称为*之前*HTTP 响应已创建，且可能甚至在控制器操作运行之前。 当**ChallengeAsync**调用时，`context.Result`包含**IHttpActionResult**，用于更高版本创建的 HTTP 响应。 因此，在**ChallengeAsync**是调用，你不知道有关 HTTP 响应的任何尚未。 **ChallengeAsync**方法应替换的原始值`context.Result`用新**IHttpActionResult**。 这**IHttpActionResult**必须包装原始`context.Result`。

![](authentication-filters/_static/image3.png)

我将调用原始**IHttpActionResult** *内部结果*，并且新**IHttpActionResult** *外部结果*。 外部结果必须执行以下操作：

1. 调用内部的结果，以创建 HTTP 响应。
2. 检查该响应。
3. 如果需要请添加身份验证质询的响应。

下面的示例摘自基本身份验证示例。 它定义**IHttpActionResult**外部的结果。

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

`InnerResult`属性包含内部**IHttpActionResult**。 `Challenge`属性表示 Www 身份验证标头。 请注意， **ExecuteAsync**首先调用`InnerResult.ExecuteAsync`创建 HTTP 响应中，如果需要然后添加所面临的挑战。

添加所面临的挑战之前检查响应代码。 大多数身份验证方案只能添加一项挑战如果响应为 401，如下所示。 但是，某些身份验证方案希望向成功响应添加一项挑战。 有关示例，请参阅[Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559)。

给定`AddChallengeOnUnauthorizedResult`类中的实际代码**ChallengeAsync**很简单。 你刚刚创建的结果，并将其附加到`context.Result`。

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

注意： 基本身份验证示例提取此逻辑有点，通过将其放置在扩展方法。

## <a name="combining-authentication-filters-with-host-level-authentication"></a>组合使用主机级身份验证的身份验证筛选器

"主机级身份验证"在请求到达 Web API 框架之前是由 （如 IIS)，主机的身份验证。

通常情况下，你可能想要启用你的应用程序的其余部分的主机级身份验证，但禁用它为你的 Web API 控制器。 例如，典型的方案是启用表单身份验证在主机级别，但对 Web API 使用基于令牌的身份验证。

若要禁用 Web API 管道内的主机级身份验证，调用`config.SuppressHostPrincipal()`中你的配置。 这会导致 Web API 删除**IPrincipal**从输入 Web API 管道的任何请求。 实际上，它&quot;取消的进行身份验证&quot;请求。

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>其他资源

[ASP.NET Web API 安全筛选](https://msdn.microsoft.com/magazine/dn781361.aspx)（MSDN 杂志）
