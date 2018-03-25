---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: "启用 ASP.NET Web API 2 中的跨源请求 |Microsoft 文档"
author: MikeWasson
description: "演示如何在 ASP.NET Web API 中支持跨域资源共享 (CORS)。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 453ad29ff4f10f9660f3aa8bab358519b4cfd48b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a>启用 ASP.NET Web API 2 中的跨源请求
====================
通过[Mike Wasson](https://github.com/MikeWasson)

> 浏览器安全阻止到另一个域进行 AJAX 请求 web 页。 此限制称为*同源策略*，，可防止恶意站点读取另一个站点中的敏感数据。 但是，有时你可能想要让调用你的 web API 的其他站点。
> 
> [跨域资源共享](http://www.w3.org/TR/cors/)(CORS) 是一种 W3C 标准，允许服务器放宽了同源策略。 使用 CORS，服务器可以显式允许某些跨源请求时拒绝其他人。 CORS 是更安全、 更灵活比早期技术如[JSONP](http://en.wikipedia.org/wiki/JSONP)。 本教程演示如何在 Web API 应用程序中启用 CORS。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - [Visual Studio 2013 Update 2](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2.2


<a id="intro"></a>
## <a name="introduction"></a>介绍

本教程演示在 ASP.NET Web API 中的 CORS 支持。 我们将开始通过创建两个 ASP.NET 项目 – 一个称为"web 服务"，其中承载 Web API 控制器和其他调用"WebClient"，后者将调用 web 服务。 由于两个应用程序承载在不同的域中，从 WebClient 对 web 服务的 AJAX 请求是对跨域请求。

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>什么是"相同源"？

如果它们具有相同的方案、 主机和端口，两个 Url 将具有相同的原点。 ([RFC 6454](http://tools.ietf.org/html/rfc6454))

这些两个 Url 具有相同的来源：

- `http://example.com/foo.html`
- `http://example.com/bar.html`

这些 Url 有两个比以前的不同来源：

- `http://example.net`-不同的域
- `http://example.com:9000/foo.html`-不同的端口
- `https://example.com/foo.html`-不同的方案
- `http://www.example.com/foo.html`的不同子域

> [!NOTE]
> 比较来源时，Internet Explorer 不考虑端口。


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a>创建 web 服务项目

> [!NOTE]
> 本部分假定你已了解如何创建 Web API 项目。 如果没有，请参阅[Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)。


启动 Visual Studio 并创建一个新**ASP.NET Web 应用程序**项目。 选择**空**项目模板。 在"添加文件夹和核心引用"，选择**Web API**复选框。 （可选） 选择"云中托管"选项将应用部署到 Mircosoft Azure。 Microsoft 提供了可用的 web 宿主中的最多 10 个网站[免费 Azure 试用帐户](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

添加名为的 Web API 控制器`TestController`替换为以下代码：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

你可以本地运行应用程序或部署到 Azure。 （对于本教程中的屏幕截图，我部署到 Azure App Service Web Apps。）若要验证的 web API 是否正常工作，请导航到`http://hostname/api/test/`，其中*主机名*是将应用程序的部署的域。 你应看到响应文本，&quot;获取： 测试消息&quot;。

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a>创建 WebClient 项目

创建另一个 ASP.NET Web 应用程序项目，然后选择**MVC**项目模板。 （可选） 选择**更改身份验证** > **无身份验证**。 本教程中，不需要身份验证。

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

在解决方案资源管理器，打开文件 Views/Home/Index.cshtml。 将此文件中的代码替换为以下：

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

有关*serviceUrl*变量，使用 web 服务应用程序的 URI。 现在在本地运行 WebClient 应用或将其发布到另一个网站。

单击"Try It"按钮时将提交到 web 服务应用程序，使用中列出的 HTTP 方法的 AJAX 请求 （GET、 POST 或 PUT） 的下拉框。 这可以让我们检查不同的跨域请求。 现在，web 服务应用程序不支持 CORS，因此如果你单击按钮时，你会收到错误。

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> 如果你观看在工具中的 HTTP 流量诸如[Fiddler](http://www.telerik.com/fiddler)，你将看到，浏览器发送 GET 请求，并且请求成功，但 AJAX 调用将返回错误。 务必了解同源策略不会阻止浏览器中的从*发送*请求。 相反，它会阻止应用程序看到*响应*。


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a>启用 CORS

现在让我们在 web 服务应用程序中启用 CORS。 首先，添加 CORS NuGet 包。 在 Visual Studio 中，从**工具**菜单上，选择**库程序包管理器**，然后选择**程序包管理器控制台**。 在 Package Manager Console 窗口中，键入以下命令：

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

此命令安装最新的包和更新所有依赖关系，包括核心 Web API 库。 用户-版本标志，以面向特定版本。 CORS 包需要 Web API 2.0 或更高版本。

打开文件应用\_Start/WebApiConfig.cs。 以下代码添加到**WebApiConfig.Register**方法。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

接下来，添加**[EnableCors]**属性设为`TestController`类：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

有关*来源*参数，请使用 WebClient 应用程序的部署位置的 URI。 这样，跨域请求来自 WebClient，同时仍不允许其他所有跨域请求。 更高版本，我将介绍的参数**[EnableCors]**中更多详细信息。

不包含正斜杠末尾*来源*URL。

重新部署更新的 web 服务应用程序。 你不需要更新 WebClient。 现在应该成功从 WebClient AJAX 请求。 所有允许使用 GET、 PUT 和 POST 方法。

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a>CORS 的工作原理

本部分介绍在 CORS 请求中，在 HTTP 消息的级别会发生什么情况。 务必了解 CORS 的工作原理，以便你可以配置**[EnableCors]**属性是否正确，并解决如果操作不能按预期工作。

CORS 规范引入了几个新的 HTTP 标头启用跨域请求。 如果浏览器支持 CORS，则将自动为跨源请求; 这些标头设置你不必执行任何特殊的 JavaScript 代码中。

下面是跨域请求的示例。 "源"标头提供的域的正在发出请求的站点。

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

如果服务器允许该请求，则将设置访问控制的允许的域标头。 此标头的值匹配的 Origin 标头，或是通配符值"\*"，允许任何来源的含义。

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

如果响应不包含访问控制的允许的域标头，AJAX 请求失败。 具体而言，浏览器不允许该请求。 即使服务器将返回成功的响应，浏览器不进行响应可供客户端应用程序。

**预检请求**

对于某些 CORS 请求，浏览器发送一个其他的请求，调用"预检请求，"，再将发送实际请求的资源。

如果以下条件为真，浏览器可以跳过预检请求：

- 请求方法是 GET、 HEAD 或 POST，*和*
- 应用程序不会设置以外，接受语言内容 Accept-language、 任何请求标头内容类型或最后一个事件 ID*和*
- 内容类型标头 (如果设置) 是以下之一： 

    - application/x-www-form-urlencoded
    - multipart/窗体的数据
    - 文本/无格式

有关请求标头的该规则适用于应用程序将通过调用设置的标头**setRequestHeader**上**XMLHttpRequest**对象。 （CORS 规范调用这些"作者请求标头"。）此规则不适用于标头*浏览器*可以设置，如用户代理、 主机或内容长度。

下面是预检请求的示例：

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

预检请求使用 HTTP OPTIONS 方法。 它包括两个特殊的标头：

- 访问控制的请求的方法： 将用作实际请求 HTTP 方法。
- 访问-控制-请求的标头： 请求标头的列表，*应用程序*实际请求上设置。 （同样，这不包括浏览器设置的标头。）

下面是假定服务器允许请求的示例响应：

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

响应包括列出了允许的方法的访问-控制的允许的方法标头和 （可选） 访问的控制的允许的标头标头，其中列出了允许的标头。 如果预检请求成功，则浏览器发送实际请求，如前面所述。

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a>[EnableCors] 的范围规则

在你的应用程序中，可以每个操作，每个控制器，或全局范围内为所有 Web API 控制器启用 CORS。

**每个操作**

若要为单个操作启用 CORS，设置**[EnableCors]**上的操作方法的属性。 以下示例启用 CORS 的`GetItem`仅限方法。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**每个控制器**

如果你设置**[EnableCors]**控制器类中，它适用于在控制器上的所有操作。 若要禁用 CORS 操作，将添加**[DisableCors]**属性设为该操作。 下面的示例为每个方法，但启用 CORS `PutItem`。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**全局**

若要为你的应用程序中的所有 Web API 控制器启用 CORS，请将传递**EnableCorsAttribute**到实例**EnableCors**方法：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

如果你在多个作用域中设置该属性的优先顺序为：

1. 操作
2. 控制器
3. Global

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a>设置允许的来源

*来源*参数**[EnableCors]**属性指定允许哪些来源访问该资源。 值是允许的来源的以逗号分隔列表。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

你还可以使用通配符值"\*"以允许来自任何源的请求。

允许从任何源的请求之前应该认真考虑。 这意味着按原义任何网站可 AJAX 调用你的 web API。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a>设置允许的 HTTP 方法

*方法*参数**[EnableCors]**属性指定允许哪些 HTTP 方法的访问的资源。 若要允许所有方法，使用通配符值"\*"。 下面的示例允许仅 GET 和 POST 请求。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a>设置允许的请求标头

前面我已经说明如何预检请求可能包括一个访问控制的请求标头标头中，列出由应用程序设置的 HTTP 标头 (所谓的"创作请求标头")。 *标头*参数**[EnableCors]**属性指定允许哪些作者请求标头。 若要允许任何标头，设置*标头*到"\*"。 到白名单特定的标头，设置*标头*以逗号分隔列表的允许的标头：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

但是，浏览器中都不完全一致它们如何设置访问控制的请求标头。 例如，Chrome 当前包括"源";虽然 FireFox 不包括标准标头"Accept"，例如，即使在脚本中应用程序设置它们。

如果你设置*标头*以外的其他任何内容"\*"，则应包含至少"接受"，"内容类型"，"源"和任何你想要支持的自定义标头。

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a>设置允许的响应标头

默认情况下，浏览器不公开所有向应用程序的响应标头。 默认为可用的响应标头是：

- 缓存控制
- 内容语言
- 内容类型
- 过期
- 上次修改
- 杂注

CORS 规范调用这些[简单响应标头](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header)。 若要使其他标头可用于应用程序，将设置*exposedHeaders*参数**[EnableCors]**。

在以下示例中，控制器的`Get`方法设置的名为 X-自定义的标头的自定义标头。 默认情况下，浏览器并不会跨域请求中的此标头。 若要提供标头中, 包含 X-自定义的标头*exposedHeaders*。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a>在跨域请求中传递凭据

凭据需要 CORS 请求中的特殊处理。 默认情况下，浏览器不发送任何凭据与跨域请求。 凭据包括 cookie 和 HTTP 身份验证方案。 若要发送的跨域请求的凭据，客户端必须设置**XMLHttpRequest.withCredentials**为 true。

使用**XMLHttpRequest**直接：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

在 jQuery:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

此外，则服务器必须允许凭据。 若要允许跨域凭据 Web API 中，设置**SupportsCredentials**属性设置为 true; 如果**[EnableCors]**属性：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

如果此属性为 true，HTTP 响应将包括一个访问控制的允许的凭证标头。 此标头通知浏览器服务器允许跨域请求凭据。

如果浏览器发送凭据，但响应不包含有效的访问控制的允许的凭证标头，浏览器并不会对应用程序的响应，并且 AJAX 请求失败。

要非常谨慎设置**SupportsCredentials**为 true，因为这意味着在另一个域网站可以不在用户不知情的情况下向用户的代表，Web API 发送登录的用户的凭据。 CORS 规范还会说明该设置*来源*到&quot; \* &quot;无效如果**SupportsCredentials**为 true。

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a>自定义 CORS 策略提供程序

**[EnableCors]**特性实现**ICorsPolicyProvider**接口。 您可以通过创建派生自的类提供您自己的实现**属性**并实现**ICorsProlicyProvider**。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

现在你可以将的特性应用任何位置，可使**[EnableCors]**。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

例如，自定义的 CORS 策略提供程序无法从配置文件读取设置。

作为使用属性的替代方法，你可以注册**ICorsPolicyProviderFactory**创建对象**ICorsPolicyProvider**对象。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

若要设置**ICorsPolicyProviderFactory**，调用**SetCorsPolicyProviderFactory**在启动时，，如下所示的扩展方法：

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a>浏览器支持

Web API CORS 包是服务器端技术。 用户的浏览器还需要支持 CORS。 幸运的是，当前版本的所有主要浏览器都包括[支持 CORS](http://caniuse.com/cors)。

Internet Explorer 8 和 Internet Explorer 9 有部分支持 CORS，而不 XMLHttpRequest 使用旧的 XDomainRequest 对象。 有关详细信息，请参阅[XDomainRequest-限制、 限制和解决方法](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx)。
