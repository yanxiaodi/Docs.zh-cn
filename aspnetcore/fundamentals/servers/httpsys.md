---
title: "ASP.NET Core 中的 HTTP.sys Web 服务器实现"
author: tdykstra
description: "了解 Windows 上适用于 ASP.NET Core 的 Web 服务器 HTTP.sys。 HTTP.sys 构建于 HTTP.sys 内核模式驱动程序之上，是 Kestrel 的一种替代选择，可用来直接连接到 Internet，而无需使用 IIS。"
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: d7ae6c070c7eecfd714086e15f32eff96c0943d9
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>ASP.NET Core 中的 HTTP.sys Web 服务器实现

作者：[Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher) 和 [Luke Latham](https://github.com/guardrex)

> [!NOTE]
> 本主题仅适用于 ASP.NET Core 2.0 或更高版本。 在早期版本的 ASP.NET Core 的中，HTTP.sys 被命名为 [WebListener](xref:fundamentals/servers/weblistener)。

[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) 是仅在 Windows 上运行的[适用于 ASP.NET Core 的 Web 服务器](xref:fundamentals/servers/index)。 HTTP.sys 是 [Kestrel](xref:fundamentals/servers/kestrel) 的替代选择，提供了一些 Kestrel 不提供的功能。

> [!IMPORTANT]
> HTTP.sys 与 [ASP.NET Core 模块](xref:fundamentals/servers/aspnet-core-module)不兼容，不能与 IIS 或 IIS Express 结合使用。

HTTP.sys 支持以下功能：

* [Windows 身份验证](xref:security/authentication/windowsauth)
* 端口共享
* 具有 SNI 的 HTTPS
* 基于 TLS 的 HTTP/2（Windows 10 或更高版本）
* 直接文件传输
* 响应缓存
* WebSocket（Windows 8 或更高版本）

受支持的 Windows 版本：

* Windows 7 或更高版本
* Windows Server 2008 R2 或更高版本

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

## <a name="when-to-use-httpsys"></a>何时使用 HTTP.sys

HTTP.sys 对于以下情形的部署来说很有用：

* 需要将服务器直接公开到 Internet 而不使用 IIS 的部署。

  ![HTTP.sys 直接与 Internet 进行通信](httpsys/_static/httpsys-to-internet.png)

* 内部部署需要 Kestrel 中没有的功能，如 [Windows 身份验证](xref:security/authentication/windowsauth)。

  ![HTTP.sys 直接与内部网络进行通信](httpsys/_static/httpsys-to-internal.png)

HTTP.sys 是一项成熟的技术，可以抵御多种攻击，并提供可靠、安全、可伸缩的全功能 Web 服务器。 IIS 本身作为 HTTP.sys 之上的 HTTP 侦听器运行。 

## <a name="how-to-use-httpsys"></a>如何使用 HTTP.sys

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a>配置 ASP.NET Core 应用以使用 HTTP.sys

1. 使用 [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.All/))（ASP.NET Core 2.0 或更高版本）时，不需要项目文件中的包引用。 未使用 `Microsoft.AspNetCore.All` 元包时，向 [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) 添加包引用。

1. 构建 Web 主机时调用 [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) 扩展方法，同时指定所需的 [HTTP.sys 选项](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions)：

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   通过[注册表设置](https://support.microsoft.com/kb/820129)处理其他 HTTP.sys 配置。

   **HTTP.sys 选项**

   | 属性 | 描述 | 默认 |
   | -------- | ----------- | :-----: |
   | [AllowSynchronousIO](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.allowsynchronousio) | 控制是否允许 `HttpContext.Request.Body` 和 `HttpContext.Response.Body` 的同步输入/输出。 | `true` |
   | [Authentication.AllowAnonymous](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.allowanonymous) | 允许匿名请求。 | `true` |
   | [Authentication.Schemes](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.schemes) | 指定允许的身份验证方案。 可能在处理侦听器之前随时修改。 通过 [AuthenticationSchemes 枚举](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes) `Basic`、`Kerberos`、`Negotiate`、`None` 和 `NTLM` 提供值。 | `None` |
   | [EnableResponseCaching](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.enableresponsecaching) | 尝试[内核模式](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)缓存，响应合格的标头。 该响应可能不包括 `Set-Cookie`、`Vary` 或 `Pragma` 标头。 它必须包括属性为 `public` 的 `Cache-Control` 标头和 `shared-max-age` 或 `max-age` 值，或 `Expires` 标头。 | `true` |
   | [MaxAccepts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxaccepts) | 最大并发接受数量。 | 5 &times; [环境。<br>ProcessorCount](/dotnet/api/system.environment.processorcount) |
   | [MaxConnections](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxconnections) | 要接受的最大并发连接数。 使用 `-1` 实现无限。 通过 `null` 使用注册表的计算机范围内的设置。 | `null`<br>（无限制） |
   | [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) | 请参阅 <a href="#maxrequestbodysize">MaxRequestBodySize</a> 部分。 | 30000000 个字节<br>(~28.6 MB) |
   | [RequestQueueLimit](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.requestqueuelimit) | 队列中允许的最大请求数。 | 1000 |
   | [ThrowWriteExceptions](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.throwwriteexceptions) | 指示由于客户端断开连接而失败的响应主体写入应引发异常还是正常完成。 | `false`<br>（正常完成） |
   | [超时](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts) | 公开 HTTP.sys [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) 配置，也可以在注册表中进行配置。 请访问 API 链接详细了解每个设置，包括默认值：<ul><li>[Timeouts.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.drainentitybody) &ndash; 允许 HTTP 服务器 API 在保持活动的连接上排出实体正文的时间。</li><li>[Timeouts.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.entitybody) &ndash; 允许请求实体正文到达的时间。</li><li>[Timeouts.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.headerwait) &ndash; 允许 HTTP 服务器 API 分析请求头的时间。</li><li>[Timeouts.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.idleconnection) &ndash; 对空闲连接允许的时间。</li><li>[Timeouts.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.minsendbytespersecond) &ndash; 响应的最小发送速率。</li><li>[Timeouts.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.requestqueue) &ndash; 在应用选取请求前，允许请求在请求队列中停留的时间。</li></ul> |  |
   | [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) | 指定 [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) 以注册 HTTP.sys。 最有用的是 [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add)，它用于将前缀添加到集合中。 可能在处理侦听器之前随时对这些设置进行修改。 |  |

   <a name="maxrequestbodysize"></a>
   **MaxRequestBodySize**

   允许的请求正文的最大大小（以字节计）。 当设置为 `null` 时，最大请求正文大小不受限制。 此限制不会影响升级后的连接，这始终不受限制。

   在 ASP.NET Core MVC 应用中为单个 `IActionResult` 替代限制的推荐方法是在操作方法上使用 [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) 属性：
   
   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   如果在应用开始读取请求后尝试配置请求限制，则会引发异常。 `IsReadOnly` 属性可用于指示 `MaxRequestBodySize` 属性是否处于只读状态。只读状态意味着已经太迟了，无法配置限制。

   如果应用应替代每个请求的 [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize)，则使用 [IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature)：

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

1. 如果使用的是 Visual Studio，请确保应用未经配置以运行 IIS 或 IIS Express。

   在 Visual Studio 中，默认启动配置文件是针对 IIS Express 的。 若要作为控制台应用运行该项目，请手动更改所选配置文件，如以下屏幕截图中所示：

   ![选择控制台应用配置文件](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a>配置 Windows Server

1. 如果应用为[框架相关部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，则安装 .NET Core、.NET Framework 或两者（如果应用是面向 .NET Framework 的 .NET Core 应用）。

   * **.NET Core** &ndash; 如果应用需要 .NET Core，则从 [.NET 下载](https://www.microsoft.com/net/download/windows)获取并运行 .NET Core 安装程序。
   * **.NET Framework** &ndash; 如果应用需要 .NET Framework，请参阅 [.NET Framework：安装指南](/dotnet/framework/install/)查找安装说明。 安装所需的 .NET Framework。 最新的 .NET Framework 的安装程序可从 [.NET 下载](https://www.microsoft.com/net/download/windows)中找到。

1. 配置应用的 URL 和端口。

   默认情况下，ASP.NET Core 绑定到 `http://localhost:5000`。 若要配置 URL 前缀和端口，选项包括使用：

   * [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
   * `urls` 命令行参数
   * `ASPNETCORE_URLS` 环境变量
   * [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)

   下方的代码示例演示了如何使用 [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)：

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=11)]

   `UrlPrefixes` 的一个优点是会为格式不正确的前缀立即生成一条错误消息。

   `UrlPrefixes` 中的设置替代 `UseUrls`/`urls`/`ASPNETCORE_URLS` 设置。 因此，`UseUrls`、`urls` 和 `ASPNETCORE_URLS` 环境变量的一个优点是在 Kestrel 和 HTTP.sys 之间切换变得更加容易。 有关 `UseUrls`、`urls` 和 `ASPNETCORE_URLS` 的更多信息，请参阅[托管](xref:fundamentals/hosting)。

   HTTP.sys 使用 [HTTP 服务器 API UrlPrefix 字符串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。

   > [!WARNING]
   > 不应使用顶级通配符绑定（`http://*:80/` 和 `http://+:80`）。 顶级通配符绑定可能会为应用带来安全漏洞。 此行为同时适用于强通配符和弱通配符。 使用显式主机名而不是通配符。 如果可控制整个父域（区别于易受攻击的 `*.com`），则子域通配符绑定（例如，`*.mysub.com`）不具有此安全风险。 有关详细信息，请参阅 [rfc7230 第 5.4 条](https://tools.ietf.org/html/rfc7230#section-5.4)。

1. 预先注册 URL 前缀以绑定到 HTTP.sys，并设置 x.509 证书。

   如果未在 Windows 中预先注册 URL 前缀，请使用管理员特权运行应用。 唯一的例外是当使用端口号大于 1024 的 HTTP（而非 HTTPS）绑定到 localhost 时。 在这种情况下，无需使用管理员特权。

   1. 用于配置 HTTP.sys 的内置工具为 *netsh.exe*。 *netsh.exe* 用于保留 URL 前缀并分配 X.509 证书。 此工具需要管理员特权。

      以下示例显示了保留端口 80 和 443 的 URL 前缀的命令：

      ```console
      netsh http add urlacl url=http://+:80/ user=Users
      netsh http add urlacl url=https://+:443/ user=Users
      ```

      以下示例显示了如何分配 X.509 证书：

      ```console
      netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
      ```

      *netsh.exe* 的参考文档：

      * [Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)（超文本传输协议 (HTTP) 的 Netsh 命令）
      * [UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)（UrlPrefix 字符串）

   1. 如果需要，请创建自签名的 X.509 证书。

     [!INCLUDE[How to make an X.509 cert](../../includes/make-x509-cert.md)]

1. 打开防火墙端口以允许流量到达 HTTP.sys。 使用 *netsh.exe* 或 [PowerShell cmdlet](https://technet.microsoft.com/library/jj554906)。

## <a name="additional-resources"></a>其他资源

* [HTTP 服务器 API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [aspnet/HttpSysServer GitHub 存储库（源代码）](https://github.com/aspnet/HttpSysServer/)
* [承载](xref:fundamentals/hosting)
