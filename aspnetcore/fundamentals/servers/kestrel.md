---
title: "ASP.NET Core 中的 Kestrel Web 服务器实现"
author: tdykstra
description: "介绍基于 libuv 的跨平台 ASP.NET Core Web 服务器 Kestrel。"
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: bfe7644891296c7c3485c9a870d90951ba87e9e8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>ASP.NET Core 中的 Kestrel Web 服务器实现简介

作者：[Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher) 和 [Stephen Halter](https://twitter.com/halter73)

Kestrel 是跨平台 [ASP.NET Core Web 服务器](index.md)，它基于 [libuv](https://github.com/libuv/libuv)（一个跨平台异步 I/O 库）。 Kestrel 是 Web 服务器，默认包括在 ASP.NET Core 项目模板中。 

Kestrel 支持以下功能：

  * HTTPS
  * 用于启用 [WebSocket](https://github.com/aspnet/websockets) 的不透明升级
  * 用于获得 Nginx 高性能的 Unix 套接字 

.NET Core 支持的所有平台和版本均支持 Kestrel。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[查看或下载 2.x 示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[查看或下载 1.x 示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>何时结合使用 Kestrel 和反向代理

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

可以单独使用 Kestrel，也可以将其与反向代理服务器（如 IIS、Nginx 或 Apache）结合使用。 反向代理服务器接收到来自 Internet 的 HTTP 请求，并在进行一些初步处理后将这些请求转发到 Kestrel。

![Kestrel 直接与 Internet 通信，不使用反向代理服务器](kestrel/_static/kestrel-to-internet2.png)

![Kestrel 通过反向代理服务器（如 IIS、Nginx 或 Apache）间接与 Internet 进行通信](kestrel/_static/kestrel-to-internet.png)

如果仅向内部网络公开 Kestrel，可以使用任一配置（无需考虑是否使用反向代理服务器）。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

如果应用程序仅接受来自内部网络的请求，则可以单独使用 Kestrel。

![Kestrel 直接与你的内部网络进行通信](kestrel/_static/kestrel-to-internal.png)

如果将应用程序公开到 Internet，则必须将 IIS、Nginx 或 Apache 用作反向代理服务器。 反向代理服务器接收到来自 Internet 的 HTTP 请求，并在进行一些初步处理后将这些请求转发到 Kestrel。

![Kestrel 通过反向代理服务器（如 IIS、Nginx 或 Apache）间接与 Internet 进行通信](kestrel/_static/kestrel-to-internet.png)

出于安全考虑，边缘部署需要使用反向代理（从 Internet 公开到流量）。 Kestrel 的 1.x 版本不包含对防御攻击的充分补充。 这包括但不限于相应的超时、大小限制和并发连接限制。

---

需要反向代理的一种方案是，具有共享在单个服务器上运行的相同 IP 和端口的多个应用程序。 此方案无法直接使用 Kestrel，因为 Kestrel 不支持在多个进程之间共享相同 IP 和端口。 配置 Kestrel 来侦听端口时，它会处理该端口的所有流量而不考虑主机头。 可共享端口的反向代理随后必须通过唯一 IP 和端口转发到 Kestrel。

即使不需要反向代理服务器，但出于以下其他原因，使用反向代理服务器可能是一个不错的选择：

* 它可以限制公开的外围应用。
* 它可以提供额外的可选配置和防护层。
* 它可以更好地与现有基础结构集成。
* 它可以简化负载均衡和 SSL 设置。 仅反向代理服务器需要 SSL 证书，并且该服务器可使用普通 HTTP 在内部网络上与应用程序服务器通信。

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>如何在 ASP.NET Core 应用中使用 Kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Microsoft.AspNetCore.All 元包](xref:fundamentals/metapackage)中包括 [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) 包。

默认情况下，ASP.NET Core 项目模板使用 Kestrel。 在 Program.cs 中，模板代码调用 `CreateDefaultBuilder`，后者在后台调用 [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)。

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

如果需要配置 Kestrel 选项，请调用 Program.cs 中的 `UseKestrel`，如以下示例所示：

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

安装 [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet 包。

在 `WebHostBuilder` 上调用 `Main` 方法中的 [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) 扩展方法，指定所需的任何 [Kestrel 选项](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)，如下一部分中所示。

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Kestrel 选项

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Kestrel Web 服务器具有约束配置选项，这些选项在面向 Internet 的部署中尤其有用。 下面是一些可以设置的限制：

- 客户端最大连接数
- 请求正文最大大小
- 请求正文最小数据速率

可在 [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) 类的 `Limits` 属性中设置这些约束和其他约束。 `Limits` 属性包含 [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) 类的实例。 

**客户端最大连接数**

可使用以下代码为整个应用程序设置并发打开 TCP 的最大连接数：

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

对于已从 HTTP 或 HTTPS 升级到另一个协议（例如，Websocket 请求）的连接，有一个单独的限制。 连接升级后，不会计入 `MaxConcurrentConnections` 限制。 

默认情况下，最大连接数不受限制 (NULL)。

**请求正文最大大小**

默认的请求正文最大大小为 30,000,000 字节，大约 28.6MB。 

在 ASP.NET Core MVC 应用中替代限制的推荐方法是在操作方法上使用 [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) 属性：

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

以下示例演示如何为整个应用程序和每个请求配置约束：

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

可在中间件中替代特定请求的设置：

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
如果在应用程序开始读取请求后尝试配置请求限制，则会引发异常。 通过 `IsReadOnly` 属性可知道，如果 `MaxRequestBodySize` 属性处于只读状态，则意味着配置限制已经太迟了。

**请求正文最小数据速率**

Kestrel 每秒检查一次数据是否以指定的速率（字节/秒）进入。 如果速率低于最小值，则连接超时。宽限期是 Kestrel 提供给客户端用于将其发送速率提升到最小值的时间量；在此期间不会检查速率。 宽限期有助于避免最初由于 TCP 慢启动而以较慢速率发送数据的连接中断。

默认的最小速率为 240 字节/秒，包含 5 秒的宽限期。

最小速率也适用于响应。 除了属性和接口名称中具有 `RequestBody` 或 `Response` 以外，用于设置请求限制和响应限制的代码相同。 

以下示例演示如何在 Program.cs 中配置最小数据速率：

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

可在中间件中配置每个请求的速率：

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

有关其他 Kestrel 选项的信息，请参阅以下类：

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

有关 Kestrel 选项的信息，请参阅 [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)（KestrelServerOptions 类）。

---

### <a name="endpoint-configuration"></a>终结点配置

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

默认情况下，ASP.NET Core 绑定到 `http://localhost:5000`。 通过调用 `KestrelServerOptions` 上的 `Listen` 或 `ListenUnixSocket` 方法，配置 URL 前缀和 Kestrel 要侦听的端口。 （`UseUrls`、`urls` 命令行参数以及 ASPNETCORE_URLS 环境变量也有用，但具有[本文后面](#useurls-limitations)注明的限制。）

**绑定到 TCP 套接字**

使用 `Listen` 方法绑定到 TCP 套接字，并可通过选项 lambda 配置 SSL 证书：

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

请注意此示例如何使用 [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs) 为特定终结点配置 SSL。 可使用相同 API 为特定终结点配置其他 Kestrel 设置。

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**绑定到 Unix 套接字**

可侦听 Unix 套接字以提高 Nginx 的性能，如以下示例所示：

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**端口 0**

如果指定端口号 0，Kestrel 将动态绑定到可用端口。 以下示例演示如何确定 Kestrel 在运行时实际绑定到的端口：

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**UseUrls 限制**

可通过调用 `UseUrls` 方法或使用 `urls` 命令行参数或 ASPNETCORE_URLS 环境变量来配置终结点。 如果希望将代码用于 Kestrel 以外的服务器，这些方法非常有用。 但是，请注意以下限制：

* 不能将这些方法与 SSL 结合使用。
* 如果同时使用 `Listen` 方法和 `UseUrls`，`Listen` 终结点将替代 `UseUrls` 终结点。

**IIS 的终结点配置**

如果使用 IIS，IIS 的 URL 绑定将替代通过调用 `Listen` 或 `UseUrls` 设置的任何绑定。 有关详细信息，请参阅 [ASP.NET Core 模块简介](aspnet-core-module.md)。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

默认情况下，ASP.NET Core 绑定到 `http://localhost:5000`。 可使用 `UseUrls` 扩展方法、`urls` 命令行参数或 ASP.NET Core 配置系统来配置 URL 前缀和 Kestrel 要侦听的端口。 有关这些方法的详细信息，请参阅[承载](../../fundamentals/hosting.md)。 要了解使用 IIS 作为反向代理时 URL 绑定的工作原理，请参阅 [ASP.NET Core 模块](aspnet-core-module.md)。 

---

### <a name="url-prefixes"></a>URL 前缀

如果调用 `UseUrls` 或使用 `urls` 命令行参数或 ASPNETCORE_URLS 环境变量，URL 前缀可采用以下任意格式。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

仅 HTTP URL 前缀有效；使用 `UseUrls` 配置 URL 绑定时，Kestrel 不支持 SSL。

* 包含端口号的 IPv4 地址

  ```
  http://65.55.39.10:80/
  ```

  0.0.0.0 是一种绑定到所有 IPv4 地址的特殊情况。


* 包含端口号的 IPv6 地址

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  [::] 是 IPv4 0.0.0.0 的 IPv6 等效项。


* 包含端口号的主机名

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  主机名、* 和 + 并不特殊。 不是已识别 IP 地址或“localhost”的任何内容都将绑定到所有 IPv4 和 IPv6 IP。 如果需要将不同主机名绑定到相同端口上的不同 ASP.NET Core 应用程序，请使用 [HTTP.sys](httpsys.md) 或 IIS、Nginx 或 Apache 等反向代理服务器。

* 包含端口号的“Localhost”名称或包含端口号的环回 IP

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  指定 `localhost` 后，Kestrel 将尝试绑定到 IPv4 和 IPv6 环回接口。 如果其他服务正在任一环回接口上使用请求的端口，则 Kestrel 将无法启动。 如果任一环回接口出于任何其他原因（通常是因为 IPv6 不受支持）而不可用，则 Kestrel 将记录一个警告。 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* 包含端口号的 IPv4 地址

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  0.0.0.0 是一种绑定到所有 IPv4 地址的特殊情况。


* 包含端口号的 IPv6 地址

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  [::] 是 IPv4 0.0.0.0 的 IPv6 等效项。


* 包含端口号的主机名

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  主机名、\* 和 + 并不特殊。 不是已识别 IP 地址或“localhost”的任何内容都将绑定到所有 IPv4 和 IPv6 IP。 如果需要将不同主机名绑定到相同端口上的不同 ASP.NET Core 应用程序，请使用 [WebListener](weblistener.md) 或 IIS、Nginx 或 Apache 等反向代理服务器。

* 包含端口号的“Localhost”名称或包含端口号的环回 IP

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  指定 `localhost` 后，Kestrel 将尝试绑定到 IPv4 和 IPv6 环回接口。 如果其他服务正在任一环回接口上使用请求的端口，则 Kestrel 将无法启动。 如果任一环回接口出于任何其他原因（通常是因为 IPv6 不受支持）而不可用，则 Kestrel 将记录一个警告。 

* Unix 套接字

  ```
  http://unix:/run/dan-live.sock  
  ```

**端口 0**

如果指定端口号 0，Kestrel 将动态绑定到可用端口。 除 `localhost` 名称外，任何主机名或 IP 都可绑定到端口 0。

以下示例演示如何确定 Kestrel 在运行时实际绑定到的端口：

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**SSL 的 URL 前缀**

如果调用 `UseHttps` 扩展方法，请务必在 URL 前缀中包括 `https:`，如下所示。

```csharp
var host = new WebHostBuilder() 
    .UseKestrel(options => 
    { 
        options.UseHttps("testCert.pfx", "testPassword"); 
    }) 
   .UseUrls("http://localhost:5000", "https://localhost:5001") 
   .UseContentRoot(Directory.GetCurrentDirectory()) 
   .UseStartup<Startup>() 
   .Build(); 
```

> [!NOTE]
> 不能在相同端口上托管 HTTPS 和 HTTP。

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a>后续步骤

有关更多信息，请参见以下资源：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [针对 2.x 的示例应用](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [Kestrel 源代码](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [针对 1.x 的示例应用](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [Kestrel 源代码](https://github.com/aspnet/KestrelHttpServer)

---
