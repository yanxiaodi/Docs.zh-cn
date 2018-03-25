---
title: "在 ASP.NET Core 中使用多个环境"
author: rick-anderson
description: "了解 ASP.NET Core 如何为控制多个环境中的应用行为提供支持。"
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: b40ee9b1c6feae4942f05d22dab776d3cf6c26a0
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="working-with-multiple-environments"></a>使用多个环境

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 提供通过环境变量在运行时针对设置应用程序行为的支持。

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

## <a name="environments"></a>环境

ASP.NET Core 在应用程序启动时读取环境变量 `ASPNETCORE_ENVIRONMENT`，并将该值存储在 [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName) 中。 `ASPNETCORE_ENVIRONMENT` 可设置为任意值，但框架支持[三个值](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0)：[开发](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0)、[暂存](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0)和[生产](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0)。 如果未设置 `ASPNETCORE_ENVIRONMENT`，将默认为 `Production`。

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

前面的代码：

* 当 `ASPNETCORE_ENVIRONMENT` 设置为 `Development` 时，调用 [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 和 [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_)。
* 当 `ASPNETCORE_ENVIRONMENT` 的值设置为下列之一时，调用 [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_)：

    * `Staging`
    * `Production`
    * `Staging_2`

[环境标记帮助程序](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)使用 `IHostingEnvironment.EnvironmentName` 的值用于包含或排除元素中的标记：

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

注意：在 Windows 和 macOS 上，环境变量和值不区分大小写。 默认情况下，Linux 环境变量和值要区分大小写。

### <a name="development"></a>开发

开发环境可以启用不应该在生产中公开的功能。 例如，ASP.NET Core 模板在开发环境中启用了[开发人员异常页](xref:fundamentals/error-handling#the-developer-exception-page)。

本地计算机开发环境可以在项目的 Properties\launchSettings.json 文件中设置。 在 launchSettings.json 中设置的环境值替代在系统环境中设置的值。

以下 JSON 显示 launchSettings.json 文件中的三个配置文件：

[!code-json[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

使用 `dotnet run` 启动应用程序时，将使用具有 `"commandName": "Project"` 的第一个配置文件。 `commandName` 的值指定要启动的 Web 服务器。 `commandName` 可以是以下之一：

* IIS Express
* IIS
* 项目（启动 Kestrel 的项目）

当使用 `dotnet run` 启动应用时：

* 如果可用，读取 launchSettings.json。 launchSettings.json 中的 `environmentVariables` 设置会替代环境变量。
* 此时显示承载环境。


以下输出显示了使用 `dotnet run` 启动的应用：
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Visual Studio“调试”选项卡提供 GUI 来编辑 launchSettings.json 文件：

![项目属性设置环境变量](environments/_static/project-properties-debug.png)

在 Web 服务器重新启动之前，对项目配置文件所做的更改可能不会生效。 必须重新启动 Kestrel 才能检测到对其环境所做的更改。

>[!WARNING]
> launchSettings.json 不应存储机密。 [机密管理器工具](xref:security/app-secrets)可用于存储本地开发的机密。

### <a name="production"></a>生产

生产环境应配置为最大限度地提高安全性、性能和应用程序可靠性。 生产环境可能具有的某些常见设置可能与开发环境不同，其中包括：

* 缓存。
* 客户端资源被捆绑和缩小，并可能从 CDN 提供。
* 已禁用诊断错误页。
* 已启用友好错误页。
* 已启用生产记录和监视。 例如，[Application Insights](/azure/application-insights/app-insights-asp-net-core)。

## <a name="setting-the-environment"></a>设置环境

为测试设置特定环境通常很有用。 如果未设置环境，默认值为 `Production`，这会禁用大多数调试功能。

设置环境的方法取决于操作系统。

### <a name="azure"></a>Azure

对于 Azure 应用服务：

* 选择“应用程序设置”边栏选项卡。
* 将键和值添加到“应用设置”。


### <a name="windows"></a>Windows
若要为当前会话设置 `ASPNETCORE_ENVIRONMENT`，如果应用使用 `dotnet run` 启动，则使用以下命令

**命令行**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

这些命令仅对当前窗口有效。 窗口关闭时，ASPNETCORE_ENVIRONMENT 设置将恢复为默认设置或计算机值。 若要在 Windows 上全局设置该值，请打开“控制面板” > “系统” > “高级系统设置”并添加或编辑 `ASPNETCORE_ENVIRONMENT` 值。

![系统高级属性](environments/_static/systemsetting_environment.png)

![ASPNET Core 环境变量](environments/_static/windows_aspnetcore_environment.png)


**web.config**

请参阅 [ASP.NET Core 模块配置参考](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)主题的“设置环境变量”部分。

**每个 IIS 应用程序池**

若要为独立应用程序池中运行的单个应用设置环境变量（IIS 10.0+ 中支持此操作），请参阅[环境变量 \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主题中的“AppCmd.exe 命令”部分。

### <a name="macos"></a>macOS
设置 macOS 的当前环境可在运行应用程序时完成；

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
或在运行应用之前使用 `export` 对其进行设置。

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
在 .bashrc 或 .bash_profile 文件中设置计算机级别环境变量。 使用任何文本编辑器编辑文件并添加以下语句。

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux
对于 Linux 发行版，请在命令行中使用 `export` 命令进行基于会话的变量设置，并使用 bash_profile 文件进行计算机级别环境设置。

### <a name="configuration-by-environment"></a>按环境配置

有关详细信息，请参阅[按环境配置](xref:fundamentals/configuration/index#configuration-by-environment)。

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a>基于环境的 Startup 类和方法

当 ASP.NET Core 应用启动时，[Startup 类](xref:fundamentals/startup)启动应用。 如果类 `Startup{EnvironmentName}` 存在，那么将为类 `EnvironmentName` 调用该类：

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

注意：调用 [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) 替代配置节。

[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 和 [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) 支持窗体 `Configure{EnvironmentName}` 和 `Configure{EnvironmentName}Services` 的环境特定版本：

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>其他资源

* [应用程序启动](xref:fundamentals/startup)
* [配置](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
