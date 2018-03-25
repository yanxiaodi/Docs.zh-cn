---
title: ".NET 的开放 Web 接口 (OWIN)"
author: ardalis
description: "了解 ASP.NET Core 如何支持 .NET 的开放 Web 接口 (OWIN)，将 Web 应用与 Web 服务器分离。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/owin
ms.openlocfilehash: 1a6a49715840d66dc37465758d3a896af96e2976
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2018
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a>.NET 的开放 Web 接口 (OWIN) 的介绍

作者：[Steve Smith](https://ardalis.com/) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 支持 .NET 的开放 Web 接口 (OWIN)。 OWIN 允许 Web 应用从 Web 服务器分离。 它定义了在管道中使用中间件来处理请求和相关响应的标准方法。 ASP.NET Core 应用程序和中间件可以与基于 OWIN 的应用程序、服务器和中间件进行互操作。

OWIN 提供了一个分离层，可一起使用具有不同对象模型的两个框架。 `Microsoft.AspNetCore.Owin` 包提供了两个适配器实现：
- ASP.NET Core 到 OWIN 
- OWIN 到 ASP.NET Core

此方法可将 ASP.NET Core 托管在兼容 OWIN 的服务器/主机上，或在 ASP.NET Core 上运行其他兼容 OWIN 的组件。

注意：使用这些适配器会带来性能成本。 仅使用 ASP.NET Core 组件的应用程序不应使用 Owin 包或适配器。

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a>在 ASP.NET 管道中运行 OWIN 中间件

ASP.NET Core 的 OWIN 支持作为 `Microsoft.AspNetCore.Owin` 包的一部分进行部署。 可通过安装此包将 OWIN 支持导入到项目中。

OWIN 中间件符合 [OWIN 规范](http://owin.org/spec/spec/owin-1.0.0.html)，该规范要求使用 `Func<IDictionary<string, object>, Task>` 接口，并设置特定的键（如 `owin.ResponseBody`）。 以下简单的 OWIN 中间件显示“Hello World”：

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

示例签名返回 `Task`，并接受 OWIN 所要求的 `IDictionary<string, object>`。

以下代码显示了如何使用 `UseOwin` 扩展方法将 `OwinHello` 中间件（如上所示）添加到 ASP.NET 管道。

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

可配置在 OWIN 管道中要进行的其他操作。

> [!NOTE]
> 响应标头只能在首次写入响应流之前进行修改。

> [!NOTE]
> 由于性能原因，不建议多次调用 `UseOwin`。 组合在一起时 OWIN 组件的性能最佳。

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a>在基于 OWIN 的服务器中使用 ASP.NET 托管

基于 OWIN 的服务器可托管 ASP.NET 应用程序。 [Nowin](https://github.com/Bobris/Nowin)（.NET OWIN Web 服务器）属于这种服务器。 本文的示例中包含了一个引用 Nowin 的项目，并使用它创建了一个自托管 ASP.NET Core 的 `IServer`。

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

`IServer` 是需要 `Features` 属性和 `Start` 方法的接口。

`Start` 负责配置和启动服务器，在此情况下，此操作通过一系列 Fluent API 调用完成，这些调用设置从 IServerAddressesFeature 分析的地址。 请注意，`_builder` 变量的 Fluent 配置指定请求将由方法中之前定义的 `appFunc` 来处理。 对于每个请求，都会调用此 `Func` 以处理传入请求。

我们还将添加一个 `IWebHostBuilder` 扩展，以便添加和配置 Nowin 服务器。

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

准备就绪后，只需使用此自定义服务器来运行 ASP.NET 应用程序以调用 Program.cs 中的扩展：

```csharp

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}

```

了解有关 ASP.NET [服务器](servers/index.md)的更多信息。

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a>在基于 OWIN 的服务器上运行 ASP.NET Core 并使用其 WebSocket 支持

ASP.NET Core 如何利用基于 OWIN 的服务器功能的另一个示例是访问 WebSocket 等功能。 前面示例中使用的 .NET OWIN Web 服务器支持内置的 Web 套接字，可由 ASP.NET Core 应用程序利用。 下面的示例显示了简单的 Web 应用，它支持 Web 套接字并回显通过 WebSocket 发送到服务器的所有内容。

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

使用与前一个相同的 `NowinServer` 来配置此[示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) - 唯一的区别是如何在其 `Configure` 方法中配置应用程序。 使用[简单的 websocket 客户端](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en)的测试演示应用程序：

![Web 套接字测试客户端](owin/_static/websocket-test.png)

## <a name="owin-environment"></a>OWIN 环境

可使用 `HttpContext` 来构造 OWIN 环境。

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a>OWIN 键

OWIN 依赖于 `IDictionary<string,object>` 对象，以在整个 HTTP请求/响应交换中传达信息。 ASP.NET Core 实现以下所列的键。 请参阅[主规范、扩展](http://owin.org/#spec)和 [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html)（OWIN 键指南和常用键）。

### <a name="request-data-owin-v100"></a>请求数据 (OWIN v1.0.0)

| 键               | 值（类型） | 描述 |
| ----------------- | ------------ | ----------- |
| owin.RequestScheme | `String` |  |
| owin.RequestMethod  | `String` | |    
| owin.RequestPathBase  | `String` | |    
| owin.RequestPath | `String` | |     
| owin.RequestQueryString  | `String` | |    
| owin.RequestProtocol  | `String` | |    
| owin.RequestHeaders | `IDictionary<string,string[]>`  | |
| owin.RequestBody | `Stream`  | |

### <a name="request-data-owin-v110"></a>请求数据 (OWIN v1.1.0)

| 键               | 值（类型） | 描述 |
| ----------------- | ------------ | ----------- |
| owin.RequestId | `String` | Optional |

### <a name="response-data-owin-v100"></a>响应数据 (OWIN v1.0.0)

| 键               | 值（类型） | 描述 |
| ----------------- | ------------ | ----------- |
| owin.ResponseStatusCode | `int` | Optional |
| owin.ResponseReasonPhrase | `String` | Optional |
| owin.ResponseHeaders | `IDictionary<string,string[]>`  | |
| owin.ResponseBody | `Stream`  | |


### <a name="other-data-owin-v100"></a>其他数据 (OWIN v1.0.0)

| 键               | 值（类型） | 描述 |
| ----------------- | ------------ | ----------- |
| owin.CallCancelled | `CancellationToken` |  |
| owin.Version  | `String` | |   


### <a name="common-keys"></a>常用键

| 键               | 值（类型） | 描述 |
| ----------------- | ------------ | ----------- |
| ssl.ClientCertificate | `X509Certificate` |  |
| ssl.LoadClientCertAsync  | `Func<Task>` | |    
| server.RemoteIpAddress  | `String` | |    
| server.RemotePort | `String` | |     
| server.LocalIpAddress  | `String` | |    
| server.LocalPort  | `String` | |    
| server.IsLocal  | `bool` | |    
| server.OnSendingHeaders  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a>SendFiles v0.3.0

| 键               | 值（类型） | 描述 |
| ----------------- | ------------ | ----------- |
| sendfile.SendAsync | 请参阅[委托签名](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) | 每请求 |


### <a name="opaque-v030"></a>Opaque v0.3.0

| 键               | 值（类型） | 描述 |
| ----------------- | ------------ | ----------- |
| opaque.Version | `String` |  |
| opaque.Upgrade | `OpaqueUpgrade` | 请参阅[委托签名](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| opaque.Stream | `Stream` |  |
| opaque.CallCancelled | `CancellationToken` |  |


### <a name="websocket-v030"></a>WebSocket v0.3.0

| 键               | 值（类型） | 描述 |
| ----------------- | ------------ | ----------- |
| websocket.Version | `String` |  |
| websocket.Accept | `WebSocketAccept` | 请参阅[委托签名](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| websocket.AcceptAlt |  | 非规范 |
| websocket.SubProtocol | `String` | 请参阅 [RFC6455 4.2.2 节](https://tools.ietf.org/html/rfc6455#section-4.2.2)步骤 5.5 |
| websocket.SendAsync | `WebSocketSendAsync` | 请参阅[委托签名](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.ReceiveAsync | `WebSocketReceiveAsync` | 请参阅[委托签名](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.CloseAsync | `WebSocketCloseAsync` | 请参阅[委托签名](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.CallCancelled | `CancellationToken` |  |
| websocket.ClientCloseStatus | `int` | Optional |
| websocket.ClientCloseDescription | `String` | Optional |

## <a name="additional-resources"></a>其他资源

* [中间件](xref:fundamentals/middleware/index)
* [服务器](xref:fundamentals/servers/index)
