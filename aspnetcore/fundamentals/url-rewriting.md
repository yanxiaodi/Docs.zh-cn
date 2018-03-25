---
title: "ASP.NET Core 中的 URL 重写中间件"
author: guardrex
description: "了解如何在 ASP.NET Core 应用程序中使用 URL 重写中间件进行 URL 重写和重定向。"
manager: wpickett
ms.author: riande
ms.date: 08/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/url-rewriting
ms.openlocfilehash: ca1f2f366bcf12cd3df83c3cdefa460cb9a68e2a
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2018
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>ASP.NET Core 中的 URL 重写中间件

作者：[Luke Latham](https://github.com/guardrex) 和 [Mikael Mengistu](https://github.com/mikaelm12)

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

URL 重写是根据一个或多个预定义规则修改请求 URL 的行为。 URL 重写会在资源位置和地址之间创建一个抽象，使位置和地址不紧密相连。 在以下几种方案中，URL 重写很有价值：
* 暂时或永久移动或替换服务器资源，同时维护这些资源的稳定定位符
* 在不同应用或同一应用的不同区域中拆分请求处理
* 删除、添加或重新组织传入请求上的 URL 段
* 优化搜索引擎优化 (SEO) 的公共 URL
* 允许使用友好的公共 URL 帮助用户预测他们将通过链接找到的内容
* 将不安全请求重定向到安全终结点
* 阻止映像盗链

可通过多种方式定义更改 URL 的规则，包括正则表达式、Apache mod_rewrite 模块规则、IIS 重写模块规则以及使用自定义规则逻辑。 本文档介绍 URL 重写并说明如何在 ASP.NET Core 应用中使用 URL 重写中间件。

> [!NOTE]
> URL 重写可能会降低应用的性能。 如果可行，应限制规则的数量和复杂度。

## <a name="url-redirect-and-url-rewrite"></a>URL 重定向和 URL 重写
URL 重定向和 URL 重写之间的用词差异乍一看可能很细微，但这对于向客户端提供资源具有重要意义。 ASP.NET Core 的 URL 重写中间件能够满足两者的需求。

URL 重定向是客户端操作，指示客户端访问另一个地址的资源。 这需要往返服务器。 客户端对资源发出新请求时，返回客户端的重定向 URL 会出现在浏览器地址栏。 

如果将 `/resource` 重定向到 `/different-resource`，客户端会请求`/resource`。 服务器通过指示重定向是临时还是永久的状态代码作出响应，表示客户端应该获取 `/different-resource` 处的资源。 客户端在重定向 URL 处对资源执行新的请求。

![WebAPI 服务终结点已暂时从服务器上的版本 1 (v1) 更改为版本 2 (v2)。 客户端向版本 1 路径 /v1/api 上的服务发出请求。 服务器发回“302 (已找到)”响应，其中包括版本 2 /v2/api 上服务的新临时路径。 客户端向重定向 URL 上的服务发出第二个请求。 服务器以“200 (正常)”状态代码作出响应。](url-rewriting/_static/url_redirect.png)

将请求重定向到不同的 URL 时，可指示重定向是永久的还是临时的。 如果资源有一个新的永久性 URL，并且你希望指示客户端所有将来的资源请求都使用新 URL，则使用“301 (永久移动)”状态代码。 收到 301 状态代码时，客户端可能会缓存响应。 如果重定向是临时的或一般会更改的，则使用“302 (已找到)”状态代码，以使客户端将来不应存储和重用重定向 URL。 有关详细信息，请参阅 [RFC 2616：状态代码定义](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)。

URL 重写是服务器端操作，提供来自不同资源地址的资源。 重写 URL 不需要往返服务器。 重写的 URL 不会返回客户端，也不会出现在浏览器地址栏。 `/resource` 重写到 `/different-resource` 时，客户端会请求 `/resource`，并且服务器会在内部提取 `/different-resource` 处的资源。 尽管客户端可能能够检索已重写 URL 处的资源，但是，客户端发出请求并收到响应时，并不会知道已重写 URL 处存在的资源。

![WebAPI 服务终结点已从服务器上的版本 1 (v1) 更改为版本 2 (v2)。 客户端向版本 1 路径 /v1/api 上的服务发出请求。 重写请求 URL 以访问版本 2 路径 /v2/api 上的服务。 服务以“200 (正常)”状态代码对客户端作出响应。](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>URL 重写示例应用
可使用 [URL 重写示例应用](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)了解 URL 重写中间件的功能。 此应用使用重写和重定向规则，并显示重写或重定向 URL。

## <a name="when-to-use-url-rewriting-middleware"></a>何时使用 URL 重写中间件
无法结合使用 [URL 重写模块](https://www.iis.net/downloads/microsoft/url-rewrite)和 Windows Server 上的 IIS、Apache Server 上的 [Apache mod_rewrite 模块](https://httpd.apache.org/docs/2.4/rewrite/)、[Nginx 上的 URL 重写](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)，或者应用托管在 [HTTP.sys 服务器](xref:fundamentals/servers/httpsys)（以前称为 [WebListener](xref:fundamentals/servers/weblistener)）上时，请使用 URL 重写中间件。 在 IIS、Apache 或 Nginx 中使用基于服务器的 URL 重写技术的主要原因是，中间件不支持这些模块的全部功能，而且中间件的性能可能与模块的性能不匹配。 但是，服务器模块的某些功能不适用于 ASP.NET Core 项目，例如 IIS 重写模块的 `IsFile` 和 `IsDirectory` 约束。 在这些情况下，请改为使用中间件。

## <a name="package"></a>Package
要将中间件包含在项目中，请添加对 [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) 包的引用。 此功能适用于面向 ASP.NET Core 1.1 或更高版本的应用。

## <a name="extension-and-options"></a>扩展和选项
通过使用扩展方法为每条规则创建 `RewriteOptions` 类的实例，建立 URL 重写和重定向规则。 按所需的处理顺序链接多个规则。 使用 `app.UseRewriter(options);` 将 `RewriteOptions` 添加到请求管道时，它会被传递到 URL 重写中间件。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]

---

### <a name="url-redirect"></a>URL 重定向
使用 `AddRedirect` 将请求重定向。 第一个参数包含用于匹配传入 URL 路径的正则表达式。 第二个参数是替换字符串。 第三个参数（如有）指定状态代码。 如不指定状态代码，则默认为“302 (已找到)”，指示资源暂时移动或替换。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]

---

在启用了开发人员工具的浏览器中，向路径为 `/redirect-rule/1234/5678` 的示例应用发出请求。 正则表达式匹配 `redirect-rule/(.*)` 上的请求路径，且该路径会被 `/redirected/1234/5678` 替代。 重定向 URL 以“302 (已找到)”状态代码发回客户端。 浏览器会在浏览器地址栏中出现的重定向 URL 上发出新请求。 由于示例应用中的任何规则均不匹配重定向 URL，因此第二个请求会收到来自应用的“200 (正常)”响应，且响应正文会显示此重定向 URL。 重定向 URL 时，系统将向服务器进行一次往返。

> [!WARNING]
> 建立重定向规则时务必小心。 系统会根据应用的每个请求（包括重定向后的请求）对重定向规则进行评估。 很容易便会意外创建无限重定向循环。

原始请求：`/redirect-rule/1234/5678`

![开发人员工具正跟踪请求和响应的浏览器窗口](url-rewriting/_static/add_redirect.png)

括号内的表达式部分称为“捕获组”。 表达式的点 (`.`) 表示匹配任何字符。 星号 (`*`) 表示零次或多次匹配前面的字符。 因此，URL 的最后两个路径段 `1234/5678` 由捕获组 `(.*)` 捕获。 在请求 URL 中提供的位于 `redirect-rule/` 之后的任何值均由此单个捕获组捕获。

在替换字符串中，将捕获组注入带有美元符号 (`$`)、后跟捕获序列号的字符串中。 获取的第一个捕获组值为 `$1`，第二个为 `$2`，并且正则表达式中的其他捕获组值将依次继续排列。 示例应用的重定向规则正则表达式中只有一个捕获组，因此替换字符串中只有一个注入组，即 `$1`。 如果应用此规则，URL 将变为 `/redirected/1234/5678`。

<a name="url-redirect-to-secure-endpoint"></a>
### <a name="url-redirect-to-a-secure-endpoint"></a>URL 重定向到安全的终结点
使用 `AddRedirectToHttps` 将 HTTP 请求重定向到采用 HTTPS (`https://`) 的相同主机和路径。 如不提供状态代码，则中间件默认为“302(已找到)”。 如不提供端口，则中间件默认为 `null`，这表示协议更改为 `https://` 且客户端访问端口 443 上的资源。 该示例演示如何将状态代码设置为“301(永久移动)”并将端口更改为 5001。
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
使用 `AddRedirectToHttpsPermanent` 将不安全的请求重定向到采用安全 HTTPS 协议（端口 443 上的 `https://`）的相同主机和路径。 中间件将状态代码设置为“301 (永久移动)”。

示例应用能够演示如何使用 `AddRedirectToHttps` 或 `AddRedirectToHttpsPermanent`。 将扩展方法添加到 `RewriteOptions`。 在任何 URL 上向应用发出不安全的请求。 消除自签名证书不受信任的浏览器安全警告。

使用 `AddRedirectToHttps(301, 5001)` 的原始请求：`/secure`

![开发人员工具正跟踪请求和响应的浏览器窗口](url-rewriting/_static/add_redirect_to_https.png)

使用 `AddRedirectToHttpsPermanent` 的原始请求：`/secure`

![开发人员工具正跟踪请求和响应的浏览器窗口](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>URL 重写
使用 `AddRewrite` 创建重写 URL 的规则。 第一个参数包含用于匹配传入 URL 路径的正则表达式。 第二个参数是替换字符串。 第三个参数 `skipRemainingRules: {true|false}` 指示如果当前规则适用，中间件是否要跳过其他重写规则。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

原始请求：`/rewrite-rule/1234/5678`

![开发人员工具正跟踪请求和响应的浏览器窗口](url-rewriting/_static/add_rewrite.png)

在正则表达式中首先注意到的是表达式开头的脱字号 (`^`)。 这意味着匹配从 URL 路径的开头处开始。

在上述采用重定向规则 `redirect-rule/(.*)` 的示例中，正则表达式的开头没有任何脱字号；因此，成功匹配的路径中的任何字符都可以位于 `redirect-rule/` 之前。

| 路径                               | 匹配 |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | 是   |
| `/my-cool-redirect-rule/1234/5678` | 是   |
| `/anotherredirect-rule/1234/5678`  | 是   |

重写规则 `^rewrite-rule/(\d+)/(\d+)` 只能与以 `rewrite-rule/` 开头的路径匹配。 请注意以下重写规则和以上重定向规则之间的匹配差异。

| 路径                              | 匹配 |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | 是   |
| `/my-cool-rewrite-rule/1234/5678` | 否    |
| `/anotherrewrite-rule/1234/5678`  | 否    |

在表达式的 `^rewrite-rule/` 部分之后，有两个捕获组 `(\d+)/(\d+)`。 `\d` 表示与数字匹配。 加号 (`+`) 表示与前面的一个或多个字符匹配。 因此，URL 必须包含数字加正斜杠加另一个数字的形式。 这些捕获组以 `$1` 和 `$2` 的形式注入重写 URL 中。 重写规则替换字符串将捕获组放入查询字符串中。 重写 `/rewrite-rule/1234/5678` 的请求路径，获取 `/rewritten?var1=1234&var2=5678` 处的资源。 如果原始请求中存在查询字符串，则重写 URL 时会保留此字符串。

无需往返服务器来获取资源。 如果资源存在，系统会提取资源并以“200 (正常)”状态代码返回给客户端。 因为客户端不会被重定向，所以浏览器地址栏中的 URL 不会发生更改。 对客户端来说，从未发生过 URL 重写操作。

> [!NOTE]
> 尽可能使用 `skipRemainingRules: true`，因为匹配规则代价高昂且会减少应用响应时间。 对于最快的应用响应：
> * 按照从最频繁匹配的规则到最不频繁匹配的规则排列重写规则。
> * 如果出现匹配项且无需处理任何其他规则，则跳过剩余规则的处理。

### <a name="apache-modrewrite"></a>Apache mod_rewrite
使用 `AddApacheModRewrite` 应用 Apache mod_rewrite 规则。 请确保将规则文件与应用一起部署。 有关 mod_rewrite 规则的详细信息和示例，请参阅 [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

`StreamReader` 用于读取 ApacheModRewrite.txt 规则文件中的规则。

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

第一个参数采用 `IFileProvider`，后者可通过[依赖关系注入](dependency-injection.md)提供。 注入 `IHostingEnvironment` 以提供 `ContentRootFileProvider`。 第二个参数是规则文件（即示例应用中的 ApacheModRewrite.txt）的路径。

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]

---

示例应用将请求从 `/apache-mod-rules-redirect/(.\*)` 重定向到 `/redirected?id=$1`。 响应状态代码为“302 (已找到)”。

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

原始请求：`/apache-mod-rules-redirect/1234`

![开发人员工具正跟踪请求和响应的浏览器窗口](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a>受支持的服务器变量
中间件支持下列 Apache mod_rewrite 服务器变量：
* CONN_REMOTE_ADDR
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_FORWARDED
* HTTP_HOST
* HTTP_REFERER
* HTTP_USER_AGENT
* HTTPS
* IPV6
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_METHOD
* REQUEST_SCHEME
* REQUEST_URI
* SCRIPT_FILENAME
* SERVER_ADDR
* SERVER_PORT
* SERVER_PROTOCOL
* TIME
* TIME_DAY
* TIME_HOUR
* TIME_MIN
* TIME_MON
* TIME_SEC
* TIME_WDAY
* TIME_YEAR

### <a name="iis-url-rewrite-module-rules"></a>IIS URL 重写模块规则
要使用适用于 IIS URL 重写模块的规则，请使用 `AddIISUrlRewrite`。 请确保将规则文件与应用一起部署。 当 web.config 文件在 Windows Server IIS 上运行时，请勿指示中间件使用该文件。 使用 IIS 时，应将这些规则存储在 web.config 之外，避免与 IIS 重写模块发生冲突。 有关 IIS URL 重写模块规则的详细信息和示例，请参阅 [Using Url Rewrite Module 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)（使用 URL 重写模块 2.0）和 [URL Rewrite Module Configuration Reference](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)（URL 重写模块配置引用）。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

`StreamReader` 用于读取 IISUrlRewrite.xml 规则文件中的规则。

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

第一个参数采用 `IFileProvider`，而第二个参数是 XML 规则文件（即示例应用中的 IISUrlRewrite.xml）的路径。

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]

---

示例应用将请求从 `/iis-rules-rewrite/(.*)` 重写为 `/rewritten?id=$1`。 以“200 (正常)”状态代码作为响应发送到客户端。

[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]

原始请求：`/iis-rules-rewrite/1234`

![开发人员工具正跟踪请求和响应的浏览器窗口](url-rewriting/_static/add_iis_url_rewrite.png)

如果有配置了服务器级别规则（可对应用产生不利影响）的活动 IIS 重写模块，则可禁用应用的 IIS 重写模块。 有关详细信息，请参阅[禁用 IIS 模块](xref:host-and-deploy/iis/modules#disabling-iis-modules)。

#### <a name="unsupported-features"></a>不支持的功能

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

与 ASP.NET Core 2.x 一同发布的中间件不支持以下 IIS URL 重写模块功能：
* 出站规则
* 自定义服务器变量
* 通配符
* LogRewrittenUrl

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

与 ASP.NET Core 1.x 一同发布的中间件不支持以下 IIS URL 重写模块功能：
* 全局规则
* 出站规则
* 重写映射
* CustomResponse 操作
* 自定义服务器变量
* 通配符
* Action:CustomResponse
* LogRewrittenUrl

---

#### <a name="supported-server-variables"></a>受支持的服务器变量
中间件支持下列 IIS URL 重写模块服务器变量：
* CONTENT_LENGTH
* CONTENT_TYPE
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_HOST
* HTTP_REFERER
* HTTP_URL
* HTTP_USER_AGENT
* HTTPS
* LOCAL_ADDR
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_URI

> [!NOTE]
> 也可通过 `PhysicalFileProvider` 获取 `IFileProvider`。 这种方法可为重写规则文件的位置提供更大的灵活性。 请确保将重写规则文件部署到所提供路径的服务器中。
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>基于方法的规则
使用 `Add(Action<RewriteContext> applyRule)` 在方法中实现自己的规则逻辑。 为了在方法中使用 `HttpContext`，`RewriteContext` 会将其公开。 `context.Result` 决定如何处理其他管道进程。

| context.Result                       | 操作                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules`（默认值） | 继续应用规则                                         |
| `RuleResult.EndResponse`             | 停止应用规则并发送响应                       |
| `RuleResult.SkipRemainingRules`      | 停止应用规则并将上下文发送给下一个中间件 |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]

---

示例应用演示了如何对以 .xml 结尾的路径的请求进行重定向。 如果为 `/file.xml` 发出请求，则它将重定向到 `/xmlfiles/file.xml`。 状态代码设置为“301 (永久移动)”。 对于重定向，必须明确设置响应的状态代码；否则会返回 “200 (正常)”状态代码，且在客户端上不会进行重定向。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]

---

原始请求：`/file.xml`

![开发人员工具正跟踪 file.xml 的请求和响应的浏览器窗口](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>基于 IRule 的规则
使用 `Add(IRule)` 在派生自 `IRule` 的类中实现自己的规则逻辑。 通过 `IRule` 为使用基于方法的规则方式提供更大的灵活性。 派生类可能包含构造函数，你可在其中传入 `ApplyRule` 方法的参数。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]

---

检查示例应用中 `extension` 和 `newPath` 的参数值是否符合多个条件。 `extension` 须包含一个值，并且该值必须是 .png、.jpg 或 .gif。 如果 `newPath` 无效，则会引发 `ArgumentException`。 如果为 image.png 发出请求，则它将重定向到 `/png-images/image.png`。 如果为 image.jpg 发出请求，则它将重定向到 `/jpg-images/image.jpg`。 状态代码设置为“301 (永久移动)”，`context.Result` 设置为停止处理规则并发送响应。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]

---

原始请求：`/image.png`

![开发人员工具正跟踪 image.png 的请求和响应的浏览器窗口](url-rewriting/_static/add_redirect_png_requests.png)

原始请求：`/image.jpg`

![开发人员工具正跟踪 image.jpg 的请求和响应的浏览器窗口](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>正则表达式示例

| 目标 | 正则表达式字符串和<br>匹配示例 | 替换字符串和<br>输出示例 |
| ---- | :-----------------------------: | :------------------------------------: |
| 将路径重写为查询字符串 | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| 去掉尾部反斜杠 | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| 强制添加尾部反斜杠 | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| 避免重写特定请求 | `^(.*)(?<!\.axd)$` 或 `^(?!.*\.axd$)(.*)$`<br>正确：`/resource.htm`<br>错误：`/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| 重新排列 URL 段 | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| 替换 URL 段 | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>其他资源
* [应用程序启动](startup.md)
* [中间件](xref:fundamentals/middleware/index)
* [.NET 中的正则表达式](/dotnet/articles/standard/base-types/regular-expressions)
* [正则表达式语言 - 快速参考](/dotnet/articles/standard/base-types/quick-ref)
* [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)
* [使用 URL 重写模块 2.0（适用于 IIS）](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [URL Rewrite Module Configuration Reference](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)（URL 重写模块配置引用）
* [IIS URL 重写模块论坛](https://forums.iis.net/1152.aspx)
* [Keep a simple URL structure](https://support.google.com/webmasters/answer/76329?hl=en)（保持简单的 URL 结构）
* [10 URL Rewriting Tips and Tricks](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)（10 个 URL 重写提示和技巧）
* [To slash or not to slash](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)（用斜杠或不用斜杠）
