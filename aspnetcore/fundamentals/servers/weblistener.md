---
title: "ASP.NET Core 中的 WebListener Web 服务器实现"
author: rick-anderson
description: "介绍适用于 Windows 上 ASP.NET Core 的 Web 服务器 WebListener。 WebListener 构建于 Http.Sys 内核模式驱动程序之上，是 Kestrel 的一种替代选择，可用来直接连接到 Internet，而无需 IIS。"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/weblistener
ms.openlocfilehash: fb2e0621645a48f4e603d754d8babbc07a78cae4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>ASP.NET Core 中的 WebListener Web 服务器实现

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> 本主题仅适用于 ASP.NET Core 1.x。 在 ASP.NET Core 2.0 中，WebListener 被命名为 [HTTP.sys](httpsys.md)。

WebListener 是仅在 Windows 上运行的[适用于 ASP.NET Core 的 Web 服务器](index.md)。 它构建于 [Http.Sys 内核模式驱动程序](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)之上。 WebListener 是 [Kestrel](kestrel.md) 的一种替代选择，可用来直接连接到 Internet，而无需依赖 IIS 作为反向代理服务器。 事实上，WebListener 不能与 IIS 或 IIS Express 结合使用，因为它与 [ASP.NET Core 模块](aspnet-core-module.md)不兼容。

尽管 ASP.NET Core 针对 WebListener 开发，但是它可以通过 [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet 包直接在任何 .NET Core 或 .NET Framework 应用程序中使用。

WebListener 支持以下功能：

- [Windows 身份验证](xref:security/authentication/windowsauth)
- 端口共享
- 具有 SNI 的 HTTPS
- 基于 TLS 的 HTTP/2 (Windows 10)
- 直接文件传输
- 响应缓存
- WebSocket (Windows 8)

受支持的 Windows 版本：

- Windows 7 和 Windows Server 2008 R2 及更高版本

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

## <a name="when-to-use-weblistener"></a>何时使用 WebListener

WebListener 对于在无需使用 IIS 的情况下直接向 Internet 公开服务器的部署非常有用。

![Weblistener 直接与 Internet 进行通信](weblistener/_static/weblistener-to-internet.png)

由于 WebListener 构建于 Http.Sys 之上，因此它不需要反向代理服务器来抵御攻击。 Http.Sys 是一项成熟的技术，可以抵御多种攻击，并提供可靠、安全、可伸缩的全功能 Web 服务器。 IIS 本身作为 Http.Sys 之上的 HTTP 侦听器运行。 

当你无法通过 Kestrel 获取它提供的某个功能时，WebListener 对于内部部署也是一个不错的选择。

![Weblistener 直接与你的内部网络进行通信](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a>如何使用 WebListener

下面是主机操作系统和 ASP.NET Core 应用程序的安装任务概述。

### <a name="configure-windows-server"></a>配置 Windows Server

* 安装应用程序要求的 .NET 版本，例如 [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) 或 .NET Framework 4.5.1。

* 预先注册 URL 前缀以绑定到 WebListener，并设置 SSL 证书

   如果在 Windows 中不预先注册 URL 前缀，则必须使用管理员权限运行应用程序。 唯一的例外是，如果你使用端口号大于 1024 的 HTTP（不是 HTTPS）绑定到 localhost，在这种情况下不需要管理员权限。

   有关详细信息，请参阅本文后续部分的[如何预先注册前缀和配置 SSL](#preregister-url-prefixes-and-configure-ssl)。

* 打开防火墙端口以允许流量到达 WebListener。

   你可以使用 netsh.exe 或 [PowerShell cmdlet](https://technet.microsoft.com/library/jj554906)。

此外，还有 [Http.Sys 注册表设置](https://support.microsoft.com/kb/820129)。

### <a name="configure-your-aspnet-core-application"></a>配置 ASP.NET Core 应用程序

* 安装 NuGet 包 [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/)。 这还将安装 [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) 作为依赖项。

* 在 `Main` 方法中，调用 [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) 上的 `UseWebListener` 扩展方法，指定所需的任何 WebListener [选项](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs)和[设置](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs)，如以下示例所示：

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* 配置要侦听的 Url 和端口 

  默认情况下，ASP.NET Core 绑定到 `http://localhost:5000`。 若要配置 URL 前缀和端口，可以使用 `UseURLs` 扩展方法、`urls` 命令行参数或 ASP.NET Core 配置系统。 有关详细信息，请参阅[托管](../../fundamentals/hosting.md)。

  Web 侦听器使用 [Http.Sys 前缀字符串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。 没有特定于 WebListener 的前缀字符串格式要求。

  > [!NOTE]
  > 请确保在服务器上预先注册的 `UseUrls` 中指定相同的前缀字符串。 

* 请确保应用程序未配置为运行 IIS 或 IIS Express。

  在 Visual Studio 中，默认启动配置文件是针对 IIS Express 的。  若要作为控制台应用程序运行该项目，必须手动更改所选配置文件，如以下屏幕截图中所示。

  ![选择控制台应用配置文件](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>如何在 ASP.NET Core 之外使用 WebListener

* 安装 [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet 包。

* [预先注册 URL 前缀以绑定到 WebListener，并设置 SSL 证书](#preregister-url-prefixes-and-configure-ssl)，因为在 ASP.NET Core 中会使用到。

此外，还有 [Http.Sys 注册表设置](https://support.microsoft.com/kb/820129)。


下面的代码示例演示在 ASP.NET Core 之外使用 WebListener：

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a>预先注册 URL 前缀和配置 SSL

IIS 和 WebListener 都依赖于基础 Http.Sys 内核模式驱动程序，以便侦听请求和执行初始化处理。 在 IIS 中，管理 UI 提供一种相对简单的方法来配置所有内容。 但是，如果正在使用 WebListener，则需要自行配置 Http.Sys。 执行该操作的内置工具是 netsh.exe。 

需要用到 netsh.exe 的最常见的任务是保留 URL 前缀和分配 SSL 证书。

对于初学者来说，NetSh.exe 并不是一个简单的工具。 以下示例演示保留 80 和 443 端口的 URL 前缀所需的最低要求：

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

以下示例演示如何分配 SSL 证书：

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

下面是官方的参考文档：

* [Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)（超文本传输协议 (HTTP) 的 Netsh 命令）
* [UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)（UrlPrefix 字符串）

以下资源提供几个方案的详细说明。 涉及 `HttpListener` 的文章同样适用于 `WebListener`，因为两者都基于 Http.Sys。

* [如何：使用 SSL 证书配置端口](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html)（HTTPS 通信 - 基于 HttpListener 的托管和客户端证书）这是很久之前的一个第三方博客，但仍有一些有用的信息。
* [How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/)（如何：使用 HttpListener 或 Http 服务器非托管代码 (C++) 作为 SSL 简单服务器进行演练），这也是很久之前的一个博客，但仍有一些有用的信息。
* [How Do I Set Up A .NET Core WebListener With SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)（如何使用 SSL 设置 .NET Core WebListener？）

以下是一些比 netsh.exe 命令行更易使用的第三方工具。 它们不是由 Microsoft 提供或认可的。 默认情况下，这些工具以管理员身份运行，因为 netsh.exe 本身需要管理员权限。

* [http.sys 管理器](http://httpsysmanager.codeplex.com/)提供用于列出和配置 SSL 证书和选项、前缀保留和证书信任列表的 UI。 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) 允许用户列出或配置 SSL 证书和 URL 前缀。 UI 比 http.sys 管理器更加完善并且提供更多的配置选项，但在其他方面它提供类似的功能。 它不能创建新的证书信任列表 (CTL)，但可以分配现有证书信任列表。

为生成自签名 SSL 证书，Microsoft 提供命令行工具：[MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) 和 PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate)。 此外，还提供第三方 UI 工具，可让你更加轻松地生成自签名 SSL 证书：

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [Makecert UI](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>后续步骤

有关更多信息，请参见以下资源：

* [本文的示例应用](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [WebListener 源代码](https://github.com/aspnet/HttpSysServer/)
* [承载](../hosting.md)
