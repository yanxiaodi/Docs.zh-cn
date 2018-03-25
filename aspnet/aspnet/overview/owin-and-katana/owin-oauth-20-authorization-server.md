---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: "OWIN OAuth 2.0 授权服务器 |Microsoft 文档"
author: hongyes
description: "本教程将指导你如何实现 OAuth 2.0 授权服务器使用 OWIN OAuth 中间件。 这是该唯一 outlin 的高级的教程..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/20/2014
ms.topic: article
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: e5968f8d19191c3f44e9bd58f8e22a39d8d8faff
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="owin-oauth-20-authorization-server"></a>OWIN OAuth 2.0 授权服务器
====================
通过[Hongye Sun](https://github.com/hongyes)， [Praburaj Thiagarajan](https://github.com/Praburaj)， [Rick Anderson](https://github.com/Rick-Anderson)

> 本教程将指导你如何实现 OAuth 2.0 授权服务器使用 OWIN OAuth 中间件。 这是仅概述了创建 OWIN OAuth 2.0 授权服务器的步骤的高级的教程。 这不是一个分步教程。 [下载的示例代码](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip)。
> 
> > [!NOTE]
> > 该轮廓不符合预期要用于创建安全的生产应用程序。 本教程旨在提供仅概述了有关如何实现 OAuth 2.0 授权服务器使用 OWIN OAuth 中间件。
> 
> 
> ## <a name="software-versions"></a>软件版本
> 
> | **本教程中所示** | **也适用于** |
> | --- | --- |
> | Windows 8.1 | Windows 8, Windows 7 |
> | [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) | [Visual Studio 2013 Express for 桌面](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express)。 应运行最新的更新 visual Studio 2012，但本教程未进行测试，并且某些菜单选项和对话框都有所不同。 |
> | .NET 4.5 |  |
> 
> ## <a name="questions-and-comments"></a>问题和意见
> 
> 如果你有与本教程不直接相关的问题，可以将其在发布[GitHub 上的 Katana 项目](https://github.com/aspnet/AspNetKatana/)。 问题和意见教程本身，请参阅在页底部的注释部分。


[OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749)使第三方应用程序，以获取对 HTTP 服务的有限访问权限。 而不是使用资源所有者的凭据来访问受保护的资源，客户端获取访问令牌 (它是一个字符串表示特定的作用域、 生存期和其他访问属性)。 访问令牌被颁发给第三方客户端由授权服务器的资源所有者批准。

本教程将介绍：

- 如何创建授权服务器以支持四个授权类型，以及刷新令牌：
    - 授权代码授予
    - 隐式授权
    - 资源所有者密码凭据授予
    - 客户端凭据授予
- 创建访问令牌通过受保护的资源服务器。
- 创建 OAuth 2.0 客户端。

<a id="prerequisites"></a>
## <a name="prerequisites"></a>系统必备

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions)或免费[Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express)，如下所示**软件版本**页顶部。
- 带 OWIN 的熟悉程度。 请参阅[Getting Started with Katana 项目](https://msdn.microsoft.com/magazine/dn451439.aspx)和[OWIN 和 Katana 中的新增](index.md)。
- 熟悉[OAuth](http://tools.ietf.org/html/rfc6749)术语，包括[角色](http://tools.ietf.org/html/rfc6749#section-1.1)，[协议流](http://tools.ietf.org/html/rfc6749#section-1.2)，和[授权](http://tools.ietf.org/html/rfc6749#section-1.3)。 [OAuth 2.0 简介](http://tools.ietf.org/html/rfc6749#section-1)提供良好的介绍。

## <a name="create-an-authorization-server"></a>创建授权服务器

在本教程中，我们将大致草拟如何使用[OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx)和 ASP.NET MVC 创建服务器的授权。 我们希望很快提供可供下载的已完成的示例中，因为本教程不包括每个步骤。 首先，创建名为的空 web 应用*AuthorizationServer*并安装以下程序包：

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google （或任何其他社交登录如 Microsoft.Owin.Security.Facebook 程序包）

添加[OWIN 启动类](owin-startup-class-detection.md)名为的项目根文件夹下*启动*。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

创建*应用\_启动*文件夹。 选择*应用\_启动*文件夹，然后使用 Shift + Alt + A 若要添加的下载的版本*AuthorizationServer\App\_Start\Startup.Auth.cs*文件。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

上面的代码中启用应用程序/外部登录 cookie 和 Google 身份验证，授权服务器本身用于管理帐户。

`UseOAuthAuthorizationServer`扩展方法是设置授权服务器。 安装程序选项包括：

- `AuthorizeEndpointPath`: 请求路径，其中客户端应用程序会将用户代理，以便获取用户同意颁发的令牌或代码。 例如，具有前导斜杠，必须以"`/Authorize`"。
- `TokenEndpointPath`: 请求路径客户端应用程序直接进行通信以获取访问令牌。 它必须以前导斜杠，如"/"开头。 如果颁发客户端[客户端\_机密](http://tools.ietf.org/html/rfc6749#appendix-A.2)，它必须提供给此终结点。
- `ApplicationCanDisplayErrors`： 将设置为`true`如果 web 应用程序想要在生成的客户端验证错误的自定义错误页`/Authorize`终结点。 这才必需的情况下，浏览器不会重定向回客户端应用程序，例如，当`client_id`或`redirect_uri`不正确。 `/Authorize`终结点应该会看到"oauth。错误"、"oauth。ErrorDescription"和"oauth。ErrorUri"属性添加到 OWIN 环境中。 

    > [!NOTE]
    > 如果不为 true，则授权服务器将返回的错误详细信息的默认错误页。
- `AllowInsecureHttp`： 如果为允许授权和令牌请求到达 HTTP URI 地址，并允许传入`redirect_uri`授权请求参数，为具有 HTTP URI 地址。 

    > [!WARNING]
    > 安全-这是用于开发仅。
- `Provider`： 提供了应用程序处理事件引发的授权服务器中间件的对象。 应用程序可能完全，实现接口或它可能会创建的实例`OAuthAuthorizationServerProvider`并分配此服务器支持的 OAuth 流程所需的委托。
- `AuthorizationCodeProvider`： 生成单使用授权代码，以返回到客户端应用程序。 为 OAuth 服务器要保护的应用程序**必须**提供程序实例`AuthorizationCodeProvider`令牌由`OnCreate/OnCreateAsync`事件都被视为有效只有一个调用`OnReceive/OnReceiveAsync`。
- `RefreshTokenProvider`： 生成可用于生成新的访问令牌时所需的刷新令牌。 如果未提供授权服务器将不返回刷新令牌从`/Token`终结点。

## <a name="account-management"></a>帐户管理

OAuth 不介意位置和方式如何管理用户帐户信息。 它具有[ASP.NET 标识](../../../identity/index.md)对它负责。 在本教程中，我们将简化帐户管理代码并只需确保该用户可以登录使用 OWIN cookie 中间件。 以下是有关的简化的示例代码`AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri`用于验证客户端使用其注册的重定向 URL。 `ValidateClientAuthentication`检查的基本方案标头和窗体正文以获取客户端的凭据。

登录页如下所示：

![](owin-oauth-20-authorization-server/_static/image1.png)

查看 IETF 的 OAuth 2[授权代码授予](http://tools.ietf.org/html/rfc6749#section-4.1)现在部分。 

**提供程序**（在下表中） 是[OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx)。提供程序，它的类型是`OAuthAuthorizationServerProvider`，其中包含所有 OAuth 服务器事件。 

| 流步骤，可从授权代码授予部分 | 示例下载执行这些步骤： |
| --- | --- |
|  |  |
| （A） 客户端通过将定向到授权终结点资源所有者的用户代理启动流。 客户端包括其客户端标识符、 请求的作用域、 本地状态中，和向其授权服务器将发送用户代理只有完成授予 （或拒绝） 访问后返回的重定向 URI。 | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| （B） 授权服务器进行身份验证 （通过用户代理） 的资源所有者，并建立是否资源所有者授予或拒绝客户端的访问请求。 | **&lt;如果用户授予访问权限&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| （C） 假定资源所有者授予访问权限，授权服务器将用户代理重定向回客户端使用重定向 URI 提供早期 （在请求中或在客户端注册）。 ... |  |
|  |  |
| （D） 客户端通过包含上一步中收到的授权代码来从授权服务器的令牌终结点请求访问令牌。 在发出请求时，使用授权服务器进行身份验证客户端。 客户端包括重定向 URI 用于获取验证的授权代码。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

实现示例`AuthorizationCodeProvider.CreateAsync`和`ReceiveAsync`控制创建和验证的授权代码如下所示。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

上面的代码中使用的内存中并发字典来存储代码和标识票证和在接收到代码后还原标识。 在实际应用中，必须先将它替换为永久性数据存储。 授权终结点是资源所有者授予对客户端访问权限。 通常情况下，它需要一个用户界面来允许用户单击按钮，然后确认授予。 OWIN OAuth 中间件允许应用程序代码来处理授权终结点。 在我们的示例应用程序中，我们将使用调用一个 MVC 控制器`OAuthController`进行处理。 下面是示例实现：

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

`Authorize`操作将首先检查是否用户已登录到授权服务器。 如果没有，身份验证中间件将要求调用方使用"应用程序"cookie 进行身份验证和将重定向到登录页。 （请参阅上面的突出显示的代码。）如果用户已登录，它会呈现授权视图中，如下所示：

![](owin-oauth-20-authorization-server/_static/image2.png)

如果**授予**选择按钮时，`Authorize`操作将创建新的"Bearer"标识和使用它登录。 它将触发授权服务器生成的持有者令牌并将其发送回客户端使用 JSON 负载。 

### <a name="implicit-grant"></a>隐式授权

请参阅 IETF 的 OAuth 2[隐式授权](http://tools.ietf.org/html/rfc6749#section-4.2)现在部分。

 [隐式授权](http://tools.ietf.org/html/rfc6749#section-4.2)图 4 中所示的数据流是流并映射其 OWIN OAuth 中间件遵循。  

| 流步骤，可从隐式授权部分 | 示例下载执行这些步骤： |
| --- | --- |
|  |  |
| （A） 客户端通过将定向到授权终结点资源所有者的用户代理启动流。 客户端包括其客户端标识符、 请求的作用域、 本地状态中，和向其授权服务器将发送用户代理只有完成授予 （或拒绝） 访问后返回的重定向 URI。 | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| （B） 授权服务器进行身份验证 （通过用户代理） 的资源所有者，并建立是否资源所有者授予或拒绝客户端的访问请求。 | **&lt;如果用户授予访问权限&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| （C） 假定资源所有者授予访问权限，授权服务器将用户代理重定向回客户端使用重定向 URI 提供早期 （在请求中或在客户端注册）。 ... |  |
|  |  |
| （D） 客户端通过包含上一步中收到的授权代码来从授权服务器的令牌终结点请求访问令牌。 在发出请求时，使用授权服务器进行身份验证客户端。 客户端包括重定向 URI 用于获取验证的授权代码。 |  |

由于我们已实现授权终结点 (`OAuthController.Authorize`操作) 的授权代码授予，它会自动启用以及隐式流。 注意：`Provider.ValidateClientRedirectUri`用于验证的客户端 ID 与其重定向 URL，用于防止隐式授予流发送令牌来恶意客户端的访问 ([拦截中攻击](https://www.owasp.org/index.php/Man-in-the-middle_attack))。

### <a name="resource-owner-password-credentials-grant"></a>资源所有者密码凭据授予

请参阅 IETF 的 OAuth 2[资源所有者密码凭据授予](http://tools.ietf.org/html/rfc6749#section-4.3)现在部分。

 [资源所有者密码凭据授予](http://tools.ietf.org/html/rfc6749#section-4.3)图 5 所示的数据流是流并映射其 OWIN OAuth 中间件遵循。  

| 流步骤，可从资源所有者密码凭据授予部分 | 示例下载执行这些步骤： |
| --- | --- |
|  |  |
| （A） 资源所有者提供客户端使用其用户名和密码。 |  |
|  |  |
| （B) 客户端通过包括来自资源所有者的凭据从授权服务器的令牌终结点请求访问令牌。 在发出请求时，使用授权服务器进行身份验证客户端。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| （C) 授权服务器对客户端进行身份验证和验证资源的所有者凭据，以及如果有效，将发出访问令牌。 |  |

下面是示例实现`Provider.GrantResourceOwnerCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> 上面的代码是要解释本教程的本部分和不能在安全或生产应用。 它不会检查资源所有者凭据。 它假定每个凭据有效，并为其创建一个新的标识。 新的标识将用于生成访问令牌和刷新令牌。 请将代码替换你自己的安全帐户管理代码。


### <a name="client-credentials-grant"></a>客户端凭据授予

请参阅 IETF 的 OAuth 2[客户端凭据授予](http://tools.ietf.org/html/rfc6749#section-4.4)现在部分。

 [客户端凭据授予](http://tools.ietf.org/html/rfc6749#section-4.4)图 6 所示的数据流是流并映射其 OWIN OAuth 中间件遵循。  

| 流步骤，可从客户端凭据授予部分 | 示例下载执行这些步骤： |
| --- | --- |
|  |  |
| （A） 客户端向授权服务器进行身份验证，并从令牌的终结点请求访问令牌。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| （B） 授权服务器验证客户端，并且如果有效，将发出访问令牌。 |  |

下面是示例实现`Provider.GrantClientCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> 上面的代码是要解释本教程的本部分和不能在安全或生产应用。 请将代码替换你自己的安全客户端管理代码。


### <a name="refresh-token"></a>刷新令牌

请参阅 IETF 的 OAuth 2[刷新令牌](http://tools.ietf.org/html/rfc6749#section-1.5)现在部分。

 [刷新令牌](http://tools.ietf.org/html/rfc6749#section-1.5)图 2 所示的数据流是流并映射其 OWIN OAuth 中间件遵循。  

| 流步骤，可从客户端凭据授予部分 | 示例下载执行这些步骤： |
| --- | --- |
|  |  |
| （G） 客户端通过使用授权服务器进行身份验证，并将提供刷新令牌请求新的访问令牌。 客户端身份验证要求基于客户端类型而授权服务器策略。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| （H） 授权服务器对客户端进行身份验证和验证刷新令牌，以及如果有效，将发出一个新的访问令牌 （和 （可选） 新的刷新令牌）。 |  |

下面是示例实现`Provider.GrantRefreshToken`: 

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>创建访问令牌通过受保护的资源服务器

创建空 web 应用程序项目，并安装以下项目中的包：

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

创建一个 startup 类，并配置身份验证和 Web API。 请参阅*AuthorizationServer\ResourceServer\Startup.cs*示例下载中。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

请参阅*AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs*示例下载中。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

请参阅*AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs*示例下载中。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors`方法允许在所有域的 CORS。
- `UseOAuthBearerAuthentication`方法可实现 OAuth 持有者令牌的身份验证中间它将接收并验证从 authorization 标头中请求的持有者令牌。
- `Config.SuppressDefaultHostAuthenticaiton`取消显示默认承载的应用中的身份验证的主体，因此所有请求都将匿名在此调用。
- `HostAuthenticationFilter`启用身份验证只是为了指定的身份验证类型。 在这种情况下，它是持有者身份验证类型。

为了演示身份验证的标识，我们创建 ApiController 输出当前用户的声明。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

如果授权服务器和资源服务器不在同一台计算机，OAuth 中间件将使用不同的计算机密钥进行加密和解密持有者访问令牌。 为了共享这两个项目之间的相同私钥，我们将同一`machinekey`设置中同时*web.config*文件。

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>创建 OAuth 2.0 客户端

 我们使用[DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) NuGet 包来简化客户端的代码。

### <a name="authorization-code-grant-client"></a>授权代码授予客户端

 此客户端是一个 MVC 应用程序。 它将触发授权代码授予流来获取访问令牌从后端。 它具有单个页面，如下所示：

![](owin-oauth-20-authorization-server/_static/image3.png)

- **Authorize**按钮将浏览器重定向到授权服务器便可以通知资源所有者授予对此客户端访问权限。
- **刷新**按钮将获取新的访问令牌和刷新令牌使用当前的刷新令牌。
- **访问受保护资源 API**按钮将调用以获取当前用户的声明数据并在页面上显示它们的资源服务器。

下面是示例代码的`HomeController`的客户端。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth`默认情况下需要 SSL。 由于我们的演示程序使用 HTTP，你需要添加以下配置文件中的设置：

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> 安全-永远不会禁用 SSL 在生产应用程序中。 你的登录凭据是现在以发送明文跨网络。 上面的代码说明仅针对本地示例调试和浏览。


### <a name="implicit-grant-client"></a>隐式授予客户端

此客户端使用 JavaScript 到：

1. 打开新窗口，并将重定向到授权服务器的授权终结点。
2. 从获取访问令牌 URL 片段时它将重定向回。

下图显示了此过程：

![](owin-oauth-20-authorization-server/_static/image4.png)

客户端应包含两页： 一个用于主页上，另一个用于回调。下面是示例 JavaScript 代码中找到*Index.cshtml*文件：

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

下面是处理代码中的回调*SignIn.cshtml*文件：

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> 最佳做法是将 JavaScript 移动到外部文件，不将其嵌入到其中的 Razor 标记。 若要使此示例简单，现在合并它们。


### <a name="resource-owner-password-credentials-grant-client"></a>资源所有者密码凭据授予客户端

我们使用的控制台应用来演示此客户端。 以下是代码：

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>客户端凭据授予客户端

与资源所有者密码凭据授予类似，此处是控制台应用程序代码：

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
