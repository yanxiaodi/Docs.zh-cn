---
title: "ASP.NET Core 中的托管"
author: guardrex
description: "了解有关 ASP.NET Core 中负责应用启动和生存期管理的 Web 主机。"
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 78209c8d34fa1a2a164ae333d625feca1e145e89
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="hosting-in-aspnet-core"></a>ASP.NET Core 中的托管

作者：[Luke Latham](https://github.com/guardrex)

ASP.NET Core 应用配置和启动“主机”。 主机负责应用程序启动和生存期管理。 至少，主机配置服务器和请求处理管道。

## <a name="setting-up-a-host"></a>设置主机

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

创建使用 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 实例的主机。 通常在应用的入口点来执行 `Main` 方法。 在项目模板中，`Main` 位于 Program.cs。 典型的 Program.cs 调用 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 以开始设置主机：

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

`CreateDefaultBuilder` 执行下列任务：

* 配置 [Kestrel](servers/kestrel.md) 为 Web 服务器。 有关 Kestrel 默认选项，请参阅[在 ASP.NET Core 中 Kestrel Web 服务器实现的 Kestrel 选项部分](xref:fundamentals/servers/kestrel#kestrel-options)。
* 将内容根设置为由 [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) 返回的路径。
* 从下列选项中加载可选配置：
  * appsettings.json。
  * appsettings.{Environment}.json。
  * 应用在 `Development` 环境中运行时的[用户机密](xref:security/app-secrets)。
  * 环境变量。
  * 命令行参数。
* 配置控制台和调试输出的[日志记录](xref:fundamentals/logging/index)。 日志记录包含 appsettings.json 或 appsettings.{Environment}.json 文件的日志记录配置部分中指定的[日志筛选](xref:fundamentals/logging/index#log-filtering)规则。
* 在 IIS 后方运行时，启用 [IIS 集成](xref:host-and-deploy/iis/index)。 当使用 [ASP.NET Core 模块](xref:fundamentals/servers/aspnet-core-module)时配置基路径和被服务器侦听的端口。 该模块创建 IIS 与 Kestrel 之间的反向代理。 还配置应用到[捕获启动错误](#capture-startup-errors)。 有关 IIS 默认选项，请参阅[使用 IIS 在 Windows 上托管 ASP.NET Core 的 IIS 选项部分](xref:host-and-deploy/iis/index#iis-options)。

内容根确定主机搜索内容文件（如 MVC 视图文件）的位置。 应用从项目的根文件夹启动时，会将项目的根文件夹用作内容根。 这是 [Visual Studio](https://www.visualstudio.com/) 和 [dotnet new 模板](/dotnet/core/tools/dotnet-new)中使用的默认值。

有关应用配置的详细信息，请参阅 [ ASP.NET Core 中的配置](xref:fundamentals/configuration/index)。

> [!NOTE]
> 作为使用静态 `CreateDefaultBuilder` 方法的替代方法，从 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 创建主机是一种受 ASP.NET Core 2.x 支持的方法。 有关详细信息，请参阅 ASP.NET Core 1.x 选项卡。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

创建使用 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 实例的主机。 通常执行 `Main` 方法，在应用的入口点来创建主机。 在项目模板中，`Main` 位于 Program.cs：

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

`WebHostBuilder` 需要[实现 IServer 的服务器](servers/index.md)。 内置服务器是 [Kestrel](servers/kestrel.md) 和 [HTTP.sys](servers/httpsys.md)（在 ASP.NET Core 2.0 之前的版本中，HTTP.sys 叫做 [WebListener](xref:fundamentals/servers/weblistener)）。 在此示例中，[UseKestrel 扩展方法](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)指定 Kestrel 服务器。

内容根确定主机搜索内容文件（如 MVC 视图文件）的位置。 默认内容根通过 [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1) 为 `UseContentRoot` 获得。 应用从项目的根文件夹启动时，会将项目的根文件夹用作内容根。 这是 [Visual Studio](https://www.visualstudio.com/) 和 [dotnet new 模板](/dotnet/core/tools/dotnet-new)中使用的默认值。

若要使用 IIS 作为反向代理，调用 [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) 作为生成主机的一部分。 `UseIISIntegration` 不配置服务器，而 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) 要配置。 当使用 [ASP.NET Core 模块](xref:fundamentals/servers/aspnet-core-module)创建 Kestrel 和 IIS 之间的反向代理时，`UseIISIntegration` 配置基路径和被服务器侦听的端口。 要在 ASP.NET Core 中使用 IIS，必须指定 `UseKestrel` 和 `UseIISIntegration`。 `UseIISIntegration` 仅在 IIS 或 IIS Express 后方运行时激活。 有关详细信息，请参阅 [ASP.NET Core 模块简介](xref:fundamentals/servers/aspnet-core-module)和 [ASP.NET Core 模块配置参考](xref:host-and-deploy/aspnet-core-module)。

配置主机 （和 ASP.NET Core 应用） 的最小实现包括指定服务器和应用的请求管道的配置项：

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

---

设置主机时，可以提供[配置](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1)和 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) 方法。 如果指定 `Startup` 类，必须定义 `Configure` 方法。 有关详细信息，请参阅 [ASP.NET Core 中的应用程序启动](startup.md)。 多次调用 `ConfigureServices` 将追加到另一个。 多次调用 `WebHostBuilder` 上的 `Configure` 或 `UseStartup` 将替换以前的设置。

## <a name="host-configuration-values"></a>主机配置值

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 依赖于以下的方法设置主机配置值：

* 主机生成器配置，其中包括格式 `ASPNETCORE_{configurationKey}` 的环境变量。 例如 `ASPNETCORE_URLS`。
* 显式方法，如 `CaptureStartupErrors`。
* [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) 和关联键。 使用 `UseSetting` 设置值时，该值设置为无论何种类型的字符串。

主机使用任何一个选项设置上一个值。 有关详细信息，请参阅下一部分中的[重写配置](#overriding-configuration)。

### <a name="capture-startup-errors"></a>捕获启动错误

此设置控制启动错误的捕获。

**键**：captureStartupErrors  
**类型**：布尔型（`true` 或 `1`）  
**默认值**：默认为 `false`，除非应用使用 Kestrel 在 IIS 后方运行，其中默认值是 `true`。  
**设置使用**：`CaptureStartupErrors`  
**环境变量**：`ASPNETCORE_CAPTURESTARTUPERRORS`

当 `false` 时，启动期间出错导致主机退出。 当 `true` 时，主机在启动期间捕获异常并尝试启动服务器。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a>内容根

此设置确定 ASP.NET Core 开始搜索内容文件，如 MVC 视图等。 

**键**：contentRoot  
**类型**：string  
**默认值**：默认为应用程序集所在的文件夹。  
**设置使用**：`UseContentRoot`  
**环境变量**：`ASPNETCORE_CONTENTROOT`

内容根也用作 [Web 根设置](#web-root)的基路径。 如果路径不存在，主机将无法启动。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a>详细错误

确定是否应捕获详细错误。

**键**：detailedErrors  
**类型**：布尔型（`true` 或 `1`）  
**默认值**：false  
**设置使用**：`UseSetting`  
**环境变量**：`ASPNETCORE_DETAILEDERRORS`

启用（或当<a href="#environment">环境</a>设置为 `Development` ）时，应用捕获详细的异常。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a>环境

设置应用的环境。

**键**：环境  
**类型**：string  
**默认值**：生产  
**设置使用**：`UseEnvironment`  
**环境变量**：`ASPNETCORE_ENVIRONMENT`

环境可以设置为任何值。 框架定义的值包括 `Development``Staging` 和 `Production`。 值不区分大小写。 默认情况下，从 `ASPNETCORE_ENVIRONMENT` 环境变量读取环境。 使用 [Visual Studio](https://www.visualstudio.com/) 时，可能会在 launchSettings.json 文件中设置环境变量。 有关详细信息，请参阅[使用多个环境](xref:fundamentals/environments)。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a>承载启动程序集

设置应用的承载启动程序集。

**键**：hostingStartupAssemblies  
**类型**：string  
**默认值**：空字符串  
**设置使用**：`UseSetting`  
**环境变量**：`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`

承载启动程序集的以分号分隔的字符串在启动时加载。 此功能是 ASP.NET Core 2.0 中的新增功能。

虽然配置值默认为空字符串，但是承载启动程序集会始终包含应用的程序集。 提供承载启动程序集时，当应用在启动过程中生成其公用服务时将它们添加到应用的程序集加载。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

此功能在 ASP.NET Core 1.x 中不可用。

---

### <a name="prefer-hosting-urls"></a>首选承载 URL

指示主机是否应该侦听使用 `WebHostBuilder` 配置的 URL，而不是使用 `IServer` 实现配置的 URL。

**键**：preferHostingUrls  
**类型**：布尔型（`true` 或 `1`）  
**默认值**：true  
**设置使用**：`PreferHostingUrls`  
**环境变量**：`ASPNETCORE_PREFERHOSTINGURLS`

此功能是 ASP.NET Core 2.0 中的新增功能。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

此功能在 ASP.NET Core 1.x 中不可用。

---

### <a name="prevent-hosting-startup"></a>阻止承载启动

阻止承载启动程序集自动加载，包括应用的程序集所配置的承载启动程序集。 有关详细信息，请参阅[使用 IHostingStartup 从外部程序集添加应用功能](xref:host-and-deploy/ihostingstartup)。

**键**：preventHostingStartup  
**类型**：布尔型（`true` 或 `1`）  
**默认值**：false  
**设置使用**：`UseSetting`  
**环境变量**：`ASPNETCORE_PREVENTHOSTINGSTARTUP`

此功能是 ASP.NET Core 2.0 中的新增功能。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

此功能在 ASP.NET Core 1.x 中不可用。

---

### <a name="server-urls"></a>服务器 URL

指示 IP 地址或主机地址，其中包含服务器应针对请求侦听的端口和协议。

**键**：urls  
**类型**：string  
**默认值**：http://localhost:5000  
**设置使用**：`UseUrls`  
**环境变量**：`ASPNETCORE_URLS`

设置为服务器应响应的以分号分隔 (;) 的 URL 前缀列表。 例如 `http://localhost:123`。 使用“\*”指示服务器应针对请求侦听的使用特定端口和协议（例如 `http://*:5000`）的 IP 地址或主机名。 协议（`http://` 或 `https://`）必须包含每个 URL。 不同的服务器支持的格式有所不同。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

Kestrel 具有自己的终结点配置 API。 有关详细信息，请参阅 [ASP.NET Core 中的 Kestrel Web 服务器实现](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a>关闭超时

指定等待 Web 主机关闭的时长。

**键**：shutdownTimeoutSeconds  
**类型**：int  
**默认值**：5  
**设置使用**：`UseShutdownTimeout`  
**环境变量**：`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`

虽然键使用 `UseSetting` 接受 int（例如 `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`），但是 `UseShutdownTimeout` 扩展方法采用 `TimeSpan`。 此功能是 ASP.NET Core 2.0 中的新增功能。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

此功能在 ASP.NET Core 1.x 中不可用。

---

### <a name="startup-assembly"></a>启动程序集

确定要在其中搜索 `Startup` 类的程序集。

**键**：startupAssembly  
**类型**：string  
**默认值**：应用的程序集  
**设置使用**：`UseStartup`  
**环境变量**：`ASPNETCORE_STARTUPASSEMBLY`

按名称（`string`）或类型（`TStartup`）的程序集可以引用。 如果调用多个 `UseStartup` 方法，优先选择最后一个方法。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a>Web 根路径

设置应用的静态资产的相对路径。

**键**：webroot  
**类型**：string  
**默认值**：如果未指定，默认值是“(Content Root)/wwwroot”（如果该路径存在）。 如果该路径不存在，则使用无操作文件提供程序。  
**设置使用**：`UseWebRoot`  
**环境变量**：`ASPNETCORE_WEBROOT`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a>替代配置

使用[配置](xref:fundamentals/configuration/index)以配置主机。 在以下示例中，在 hosting.json 文件中指定主机配置（可选）。 从 hosting.json 文件加载的任何配置可能会由命令行参数替代。 生成的配置（在 `config` 中）用于通过 `UseConfiguration` 配置主机。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

hosting.json：

```json
{
    urls: "http://*:5005"
}
```

首先用 hosting.json 配置替代 `UseUrls` 提供的配置，然后是命令行参数配置：

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

hosting.json：

```json
{
    urls: "http://*:5005"
}
```

首先用 hosting.json 配置替代 `UseUrls` 提供的配置，然后是命令行参数配置：

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 扩展方法当前不能分析由 `GetSection` 返回的配置部分（例如 `.UseConfiguration(Configuration.GetSection("section"))`。 `GetSection` 方法将配置键筛选到所请求的部分，但将节名称保留在键上（例如 `section:urls`、`section:environment`）。 `UseConfiguration` 方法需要键来匹配 `WebHostBuilder` 键（例如 `urls`、`environment`）。 键上存在的节名称阻止节的值配置主机。 将在即将发布的版本中解决此问题。 有关详细信息和解决方法，请参阅[将配置节传入到 WebHostBuilder.UseConfiguration 使用完整的键](https://github.com/aspnet/Hosting/issues/839)。

若要指定在特定的 URL 上运行的主机，所需的值可以在执行 `dotnet run` 时从命令提示符传入。 命令行参数替代 hosting.json 文件中的 `urls` 值，且服务器侦听端口 8080：

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a>启动主机

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**运行**

`Run` 方法启动 Web 应用并阻止调用线程，直到关闭主机：

```csharp
host.Run();
```

**Start**

通过调用 `Start` 方法以非阻止方式运行主机：

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

如果 URL 列表传递给 `Start` 方法，该列表侦听指定的 URL：

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

应用可以使用通过静态便捷方法预配置的 `CreateDefaultBuilder` 默认值初始化并启动新的主机。 这些方法在没有控制台输出的情况下启动服务器，并使用 [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) 等待中断（Ctrl-C/SIGINT 或 SIGTERM）：

**Start(RequestDelegate app)**

从 `RequestDelegate` 开始：

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

在浏览器中向 `http://localhost:5000` 发出请求，接收响应“Hello World!” `WaitForShutdown` 受到阻止，直到发出中断（Ctrl-C/SIGINT 或 SIGTERM）。 应用显示 `Console.WriteLine` 消息并等待 keypress 退出。

**Start(string url, RequestDelegate app)**

从 URL 和 `RequestDelegate` 开始：

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

生成与 Start(RequestDelegate app) 相同的结果，除非应用在 `http://localhost:8080` 上响应。

**Start(Action<IRouteBuilder> routeBuilder)**

使用 `IRouteBuilder` 的实例 ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) 用于路由中间件：

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

该示例中使用以下浏览器请求：

| 请求                                    | 响应                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | Hello, Martin!                           |
| `http://localhost:5000/buenosdias/Catrina` | Buenos dias, Catrina!                    |
| `http://localhost:5000/throw/ooops!`       | 使用“ooops!”字符串引发异常 |
| `http://localhost:5000/throw`              | 使用“Uh oh!”字符串引发异常 |
| `http://localhost:5000/Sante/Kevin`        | Sante, Kevin!                            |
| `http://localhost:5000`                    | Hello World!                             |

`WaitForShutdown` 受到阻止，直到发出中断（Ctrl-C/SIGINT 或 SIGTERM）。 应用显示 `Console.WriteLine` 消息并等待 keypress 退出。

**Start(string url, Action<IRouteBuilder> routeBuilder)**

使用 URL 和 `IRouteBuilder` 实例：

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

生成与 Start(Action<IRouteBuilder> routeBuilder) 相同的结果，除非应用在 `http://localhost:8080` 上响应。

**StartWith(Action<IApplicationBuilder> app)**

提供委托以配置 `IApplicationBuilder`：

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

在浏览器中向 `http://localhost:5000` 发出请求，接收响应“Hello World!” `WaitForShutdown` 受到阻止，直到发出中断（Ctrl-C/SIGINT 或 SIGTERM）。 应用显示 `Console.WriteLine` 消息并等待 keypress 退出。

**StartWith(string url, Action<IApplicationBuilder> app)**

提供 URL 和委托以配置 `IApplicationBuilder`：

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

生成与 StartWith(Action<IApplicationBuilder> app) 相同的结果，除非应用在 `http://localhost:8080` 上响应。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**运行**

`Run` 方法启动 Web 应用并阻止调用线程，直到关闭主机：

```csharp
host.Run();
```

**Start**

通过调用 `Start` 方法以非阻止方式运行主机：

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

如果 URL 列表传递给 `Start` 方法，该列表侦听指定的 URL：


```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

---

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment 接口

[IHostingEnvironment 接口](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment)提供有关应用的 Web 承载环境的信息。 使用[构造函数注入](xref:fundamentals/dependency-injection)获取 `IHostingEnvironment` 以使用其属性和扩展方法：

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

[基于约定的方法](xref:fundamentals/environments#startup-conventions)可以用于在启动时基于环境配置应用。 或者，将 `IHostingEnvironment` 注入到 `Startup` 构造函数用于 `ConfigureServices`：

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> 除了 `IsDevelopment` 扩展方法，`IHostingEnvironment` 提供 `IsStaging`、`IsProduction` 和 `IsEnvironment(string environmentName)` 方法。 有关详细信息，请参阅[使用多个环境](xref:fundamentals/environments)。

`IHostingEnvironment` 服务还可以直接注入到 `Configure` 方法以设置处理管道：

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

创建自定义[中间件](xref:fundamentals/middleware#writing-middleware)时可以将 `IHostingEnvironment` 注入 `Invoke` 方法：

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a>IApplicationLifetime 接口

[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) 允许后启动和关闭活动。 接口上的三个属性是用于注册 `Action` 方法（用于定义启动和关闭事件）的取消标记。 此外，还有 `StopApplication` 方法。

| 取消标记    | 触发条件 |
| --------------------- | --------------------- |
| `ApplicationStarted`  | 主机已完全启动。 |
| `ApplicationStopping` | 主机正在执行正常关闭。 仍在处理请求。 关闭受到阻止，直到完成此事件。 |
| `ApplicationStopped`  | 主机正在完成正常关闭。 应处理所有请求。 关闭受到阻止，直到完成此事件。 |

| 方法            | 操作                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | 当前应用程序请求终止。 |

```csharp
public class Startup 
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime) 
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

## <a name="troubleshooting-systemargumentexception"></a>疑难解答：System.ArgumentException

**仅适用于 ASP.NET Core 2.0**

主机可能通过将 `IStartup` 直接注入依赖关系注入容器生成，而不是调用 `UseStartup` 或 `Configure`：

```csharp
services.AddSingleton<IStartup, Startup>();
```

如果主机以这种方式生成，可能会发生以下错误：

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

这是因为 [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey)（当前程序集）需扫描 `HostingStartupAttributes`。 如果应用手动将 `IStartup` 注入到依赖关系注入容器中，将以下调用添加到具有指定程序集名称的 `WebHostBuilder`：

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

或者，将虚拟 `Configure` 添加到自动设置 `applicationName` (`ApplicationKey`) 的 `WebHostBuilder` 中：

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

**注意**：仅当使用 ASP.NET Core 2.0 发行版且应用不调用 `UseStartup` 或 `Configure` 时，需要执行此操作。

有关详细信息，请参阅[公告：已删除 Microsoft.Extensions.PlatformAbstractions（注释）](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938)和 [StartupInjection 示例](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)。

## <a name="additional-resources"></a>其他资源

* [使用 IIS 在 Windows 上进行托管](xref:host-and-deploy/iis/index)
* [在 Linux 上使用 Nginx 进行托管](xref:host-and-deploy/linux-nginx)
* [在 Linux 上使用 Apache 进行托管](xref:host-and-deploy/linux-apache)
* [在 Windows 服务中进行托管](xref:host-and-deploy/windows-service)
