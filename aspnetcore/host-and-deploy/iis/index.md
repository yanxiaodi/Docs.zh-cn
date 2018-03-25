---
title: "使用 IIS 在 Windows 上托管 ASP.NET Core"
author: guardrex
description: "了解如何在 Windows Server Internet Information Services (IIS) 上托管 ASP.NET Core 应用。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/index
ms.openlocfilehash: fa9e60c52f143b20dbf179679fc4932e838a9137
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>使用 IIS 在 Windows 上托管 ASP.NET Core

作者：[Luke Latham](https://github.com/guardrex) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>支持的操作系统

支持下列操作系统：

* Windows 7 或更高版本
* Windows Server 2008 R2 或更高版本#8224;

&#8224;在概念上，本文档描述的 IIS 配置也适用于在 Nano Server IIS 上托管 ASP.NET Core 应用。 有关 Nano Server 的具体说明，请参阅 [Nano Server 上运行的 ASP.NET Core 与 IIS](xref:tutorials/nano-server) 教程。

[HTTP.sys 服务器](xref:fundamentals/servers/httpsys)（以前称为 [WebListener](xref:fundamentals/servers/weblistener)）在使用 IIS 的反向代理配置中不起作用。 请使用 [Kestrel 服务器](xref:fundamentals/servers/kestrel)。

## <a name="application-configuration"></a>应用程序配置

### <a name="enable-the-iisintegration-components"></a>启用 IISIntegration 组件

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

典型的 Program.cs 调用 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 以开始设置主机。 `CreateDefaultBuilder` 将 [Kestrel](xref:fundamentals/servers/kestrel) 配置为 Web 服务器，并通过配置 [ASP.NET Core 模块](xref:fundamentals/servers/aspnet-core-module)的基路径和端口来实现 IIS 集成：

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

ASP.NET Core 模块生成分配给后端进程的动态端口。 `UseIISIntegration` 方法获取该动态端口，并将 Kestrel 配置为侦听 `http://locahost:{dynamicPort}/`。 这将替代其他 URL 配置，如对 `UseUrls` 或 [Kestrel 的侦听 API](xref:fundamentals/servers/kestrel#endpoint-configuration) 的调用。 因此，使用模块时，不需要调用 `UseUrls` 或 Kestrel 的 `Listen` API。 如果调用 `UseUrls` 或 `Listen`，则 Kestrel 会侦听在没有 IIS 的情况下运行应用时指定的端口。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

在应用依赖项中加入对 [Microsoft.AspNetCore.Server.IISIntegration ](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/)包的依赖项。 通过向 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 添加 [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) 扩展方法来使用 IIS 集成中间件：

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

同时需要 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) 和 [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration)。 调用 `UseIISIntegration` 的代码不会影响代码可移植性。 如果应用不在 IIS 后面运行（例如，应用直接在 Kestrel 上运行），则 `UseIISIntegration` 不会运行。

ASP.NET Core 模块生成分配给后端进程的动态端口。 `UseIISIntegration` 方法获取该动态端口，并将 Kestrel 配置为侦听 `http://locahost:{dynamicPort}/`。 这将替代其他 URL 配置，如对 `UseUrls` 的调用。 因此，使用模块时无需调用 `UseUrls`。 如果调用 `UseUrls`，则 Kestrel 会侦听在没有 IIS 的情况下运行应用时指定的端口。

如果在 ASP.NET Core 1.0 应用中调用 `UseUrls`，请在调用 `UseIISIntegration` 前调用它，使模块配置的端口不会被覆盖。 ASP.NET Core 1.1 不需要此调用顺序，因为模块设置会重写 `UseUrls`。

---

有关托管的详细信息，请参阅 [ASP.NET Core 中的托管](xref:fundamentals/hosting)。

### <a name="iis-options"></a>IIS 选项

若要配置 IIS 选项，请在 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices) 中加入适用于 [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) 的服务配置。 在下面的示例中，禁用将客户端证书转发到应用以填充 `HttpContext.Connection.ClientCertificate`：

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| 选项                         | 默认 | 设置 |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | 若为 `true`，IIS 集成中间件将设置经过 [Windows 身份验证](xref:security/authentication/windowsauth)进行身份验证的 `HttpContext.User`。 若为 `false`，中间件仅提供 `HttpContext.User` 的标识并在 `AuthenticationScheme` 显式请求时响应质询。 必须在 IIS 中启用 Windows 身份验证使 `AutomaticAuthentication` 得以运行。 有关详细信息，请参阅 [Windows 身份验证](xref:security/authentication/windowsauth)主题。 |
| `AuthenticationDisplayName`    | `null`  | 设置在登录页上向用户显示的显示名。 |
| `ForwardClientCertificate`     | `true`  | 若为 `true`，且存在 `MS-ASPNETCORE-CLIENTCERT` 请求头，则填充 `HttpContext.Connection.ClientCertificate`。 |

### <a name="webconfig-file"></a>web.config 文件

web.config 文件配置 [ASP.NET Core 模块](xref:fundamentals/servers/aspnet-core-module)。 web.config 的创建、转换和发布 由 .NET Core Web SDK (`Microsoft.NET.Sdk.Web`) 处理。 SDK 设置在项目文件的顶部：

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

如果项目中不存在 web.config 文件，则会使用正确的 processPath 和参数创建该文件，以便配置 [ASP.NET Core 模块](xref:fundamentals/servers/aspnet-core-module)，并将该文件移动到[已发布的输出](xref:host-and-deploy/directory-structure)。

如果项目中存在 web.config 文件，则会使用正确的 processPath 和参数转换该文件，以便配置 ASP.NET Core 模块，并将该文件移动到已发布的输出。 转换不会修改文件中的 IIS 配置设置。

web.config 文件可能会提供其他 IIS 配置设置，以控制活动的 IIS 模块。 有关能够处理 ASP.NET Core 应用请求的 IIS 模块的信息，请参阅[使用 IIS 模块](xref:host-and-deploy/iis/modules)主题。

要防止 Web SDK 转换 web.config 文件，请使用项目文件中的 \<IsTransformWebConfigDisabled> 属性：

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

在禁用 Web SDK 转换文件时，开发人员应手动设置 processPath 和参数。 有关详细信息，请参阅 [ASP.NET Core 模块配置参考](xref:host-and-deploy/aspnet-core-module)。

### <a name="webconfig-file-location"></a>web.config 文件位置

ASP.NET Core 应用在 IIS 与 Kestrel 服务器之间的反向代理中托管。 为了创建反向代理，web.config 文件必须存在于已部署应用的内容根路径（通常为应用基路径）中。 该位置与向 IIS 提供的网站物理路径相同。 若要使用 Web 部署发布多个应用，应用的根路径中需要包含 web.config 文件。

敏感文件存在于应用的物理路径中，如 \<assembly>.runtimeconfig.json、\<assembly>.xml（XML 文档注释）和 \<assembly>.deps.json。 当存在 web.config 文件且站点正常启动的情况下，若要请求获取这些敏感文件，IIS 不会提供。 如果缺少 web.config 文件、命名不正确，或无法配置站点以正常启动，IIS 可能会公开提供敏感文件。

部署中必须始终存在 web.config 文件且正确命名，并可以配置站点以正常启动。**请勿从生产部署中删除 web.config 文件。**

## <a name="iis-configuration"></a>IIS 配置

**Windows Server 操作系统**

启用 Web 服务器 (IIS) 服务器角色并建立角色服务。

1. 通过“管理”菜单或“服务器管理器”中的链接使用“添加角色和功能”向导。 在“服务器角色”步骤中，选中“Web 服务器(IIS)”框。

   ![在选择服务器角色步骤中选择了“Web 服务器 IIS”角色。](index/_static/server-roles-ws2016.png)

1. 在“功能”步骤后，为 Web 服务器 (IIS) 加载“角色服务”步骤。 选择所需 IIS 角色服务，或接受提供的默认角色服务。

   ![在选择角色服务步骤中选择了默认角色服务。](index/_static/role-services-ws2016.png)

   **Windows 身份验证（可选）**  
   若要启用 Windows 身份验证，请展开以下节点：“Web 服务器” > “安全”。 选择“Windows 身份验证”功能。 有关详细信息，请参阅 [Windows 身份验证 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[配置 Windows 身份验证](xref:security/authentication/windowsauth)。

   **Websocket（可选）**  
   Websocket 支持 ASP.NET Core 1.1 或更高版本。 若要启用 Websocket，请展开以下节点：“Web 服务器” > “应用程序开发”。 选择“WebSocket 协议”功能。 有关详细信息，请参阅 [WebSockets](xref:fundamentals/websockets)。

1. 继续执行“确认”步骤，安装 Web 服务器角色和服务。 安装 Web 服务器 (IIS) 角色后无需重启服务器/IIS。

**Windows 桌面操作系统**

启用“IIS 管理控制台”和“万维网服务”。

1. 导航到“控制面板” > “程序” > “程序和功能” > “打开或关闭 Windows 功能”（位于屏幕左侧）。

1. 打开“Internet Information Services”节点。 打开“Web 管理工具”节点。

1. 选中“IIS 管理控制台”框。

1. 选中“万维网服务”框。

1. 接受“万维网服务”的默认功能，或自定义 IIS 功能。

   **Windows 身份验证（可选）**  
   若要启用 Windows 身份验证，请展开以下节点：“万维网服务” > “安全”。 选择“Windows 身份验证”功能。 有关详细信息，请参阅 [Windows 身份验证 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[配置 Windows 身份验证](xref:security/authentication/windowsauth)。

   **Websocket（可选）**  
   Websocket 支持 ASP.NET Core 1.1 或更高版本。 若要启用 Websocket，请展开以下节点：“万维网服务” > “应用程序开发功能”。 选择“WebSocket 协议”功能。 有关详细信息，请参阅 [WebSockets](xref:fundamentals/websockets)。

1. 如果 IIS 安装需要重新启动，则重新启动系统。

![在“Windows 功能”中选择了“IIS 管理控制台”和“万维网服务”。](index/_static/windows-features-win10.png)

---

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>安装 .NET Core Windows Server 托管捆绑包

1. 在托管系统上安装 [.NET Core Windows Server 托管捆绑包](https://aka.ms/dotnetcore-2-windowshosting)。 捆绑包可安装 .NET Core 运行时、.NET Core 库和 [ASP.NET Core 模块](xref:fundamentals/servers/aspnet-core-module)。 该模块创建 IIS 与 Kestrel 服务器之间的反向代理。 如果系统没有 Internet 连接，请先获取并安装 [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840)，再安装 .NET Core Windows Server 托管捆绑包。

   **重要提示！** 如果在 IIS 之前安装了托管捆绑包，则必须修复捆绑包安装。 在安装 IIS 后再次运行托管捆绑包安装程序。
   
   若要防止安装程序在 x64 操作系统上安装 x86 程序包，请通过管理员命令提示符使用开关 `OPT_NO_X86=1` 来运行安装程序。

1. 重启系统，或从命令提示符处依次执行 net stop was /y 和 net start w3svc。 重新启动 IIS 将选取安装程序对系统 PATH 所作的更改。

> [!NOTE]
> 有关 IIS 共享配置的信息，请参阅[使用 IIS 共享配置的 ASP.NET Core 模块](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>使用 Visual Studio 进行发布时安装 Web 部署

使用 [Web 部署](/iis/publish/using-web-deploy/introduction-to-web-deploy)将应用部署到服务器时，请在服务器上安装最新版本的 Web 部署。 要安装 Web 部署，请使用 [Web 平台安装程序 (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或直接从 [Microsoft 下载中心](https://www.microsoft.com/download/details.aspx?id=43717)获取安装程序。 建议使用 WebPI。 WebPI 为托管提供程序提供独立的安装程序和配置。

## <a name="create-the-iis-site"></a>创建 IIS 站点

1. 在托管系统上，创建一个文件夹以包含应用已发布的文件夹和文件。 [目录结构](xref:host-and-deploy/directory-structure)主题中介绍了应用的部署布局。

1. 在新文件夹中创建一个“日志”文件夹，用于在启用 stdout 日志记录时保存 ASP.NET Core 模块 stdout 日志。 如果部署应用时有效负载中包含了“日志”文件夹，请跳过此步骤。 有关如何启用 MSBuild 以在本地生成项目时自动创建“日志”文件夹的说明，请参阅[目录结构](xref:host-and-deploy/directory-structure)主题。

   **重要提示！** 仅使用 stdout 日志来解决应用启动失败的问题。 请勿使用 stdout 日志记录进行常规应用日志记录。 日志文件大小或创建的日志文件数没有限制。 有关 stdout 日志的详细信息，请参阅[日志创建和重定向](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)。 有关 ASP.NET Core 应用中的日志记录信息，请参阅[日志记录](xref:fundamentals/logging/index)主题。

1. 在“IIS 管理器”中，打开“连接”面板中的服务器节点。 右键单击“站点”文件夹。 选择上下文菜单中的“添加网站”。

1. 提供网站名称，并将物理路径设置为应用的部署文件夹。 提供“绑定”配置，并通过选择“确定”创建网站：

   ![在“添加网站”步骤中提供网站名称、物理路径和主机名。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > 不应使用顶级通配符绑定（`http://*:80/` 和 `http://+:80`）。 顶级通配符绑定可能会为应用带来安全漏洞。 此行为同时适用于强通配符和弱通配符。 使用显式主机名而不是通配符。 如果可控制整个父域（区别于易受攻击的 `*.com`），则子域通配符绑定（例如，`*.mysub.com`）不具有此安全风险。 有关详细信息，请参阅 [rfc7230 第 5.4 条](https://tools.ietf.org/html/rfc7230#section-5.4)。

1. 在服务器节点下，选择“应用程序池”。

1. 右键单击站点的应用池，然后从上下文菜单中选择“基本设置”。

1. 在“编辑应用程序池”窗口中，将“.NET CLR 版本”设置为“无托管代码”：

   ![将“.NET CLR 版本”设置为“无托管代码”。](index/_static/edit-apppool-ws2016.png)

    ASP.NET Core 在单独的进程中运行，并管理运行时。 ASP.NET Core 不依赖加载桌面 CLR。 将“.NET CLR 版本”设置为“无托管代码”为可选步骤。

1. 确认进程模型标识拥有适当的权限。

   如果将应用池的默认标识（“进程模型” > “标识”）从 ApplicationPoolIdentity 更改为另一标识，请验证新标识拥有所需的权限，可访问应用的文件夹、数据库和其他所需资源。 例如，应用池需要对文件夹的读取和写入权限，以便应用在其中读取和写入文件。

**Windows 身份验证配置（可选）**  
有关详细信息，请参阅[配置 Windows 身份验证](xref:security/authentication/windowsauth)。

## <a name="deploy-the-app"></a>部署应用

将应用部署到在托管系统上创建的文件夹。 建议使用的部署机制是 [Web 部署](/iis/publish/using-web-deploy/introduction-to-web-deploy)。

### <a name="web-deploy-with-visual-studio"></a>在 Visual Studio 内使用 Web 部署

要了解如何创建用于 Web 部署的发布配置文件，请参阅[用于 ASP.NET Core 应用部署的 Visual Studio 发布配置文件](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)。 如果托管提供程序提供了发布配置文件或支持创建发布配置文件，请下载配置文件并使用 Visual Studio 的“发布”对话框将其导入。

![“发布”对话框页](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>在 Visual Studio 之外使用 Web 部署

也可以在 Visual Studio 之外从命令行使用 [Web 部署](/iis/publish/using-web-deploy/introduction-to-web-deploy)。 有关详细信息，请参阅 [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool)（Web 部署工具）。

### <a name="alternatives-to-web-deploy"></a>Web 部署的替代方法

有多种方法可将应用移动到托管系统，例如手动复制、Xcopy、Robocopy 或 PowerShell，可使用其中任何一种方法。

## <a name="browse-the-website"></a>浏览网站

![Microsoft Edge 浏览器已加载 IIS 启动页。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a>锁定的部署文件

如果应用正在运行，部署文件夹中的文件会被锁定。 在部署期间，无法覆盖已锁定的文件。 若要在部署中解除已锁定的文件，请使用以下方法之一停止应用池：

* 使用 Web 部署并在项目文件中引用 `Microsoft.NET.Sdk.Web`。 系统会在 Web 应用目录的根目录中放置一个 app_offline.htm 文件。 该文件存在时，ASP.NET Core 模块会在部署过程中正常关闭该应用并提供 app_offline.htm 文件。 有关详细信息，请参阅 [ASP.NET Core 模块配置参考](xref:host-and-deploy/aspnet-core-module#appofflinehtm)。
* 在服务器上的 IIS 管理器中手动停止应用池。
* 使用 PowerShell 来停止和重新启动应用池（需要使用 PowerShell 5 或更高版本）：

  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

## <a name="data-protection"></a>数据保护

[ASP.NET Core 数据保护堆栈](xref:security/data-protection/index)由多个 ASP.NET Core [中间件](xref:fundamentals/middleware/index)使用，包括用于身份验证的中间件。 即使用户代码不调用数据保护 API，也应该使用部署脚本或在用户代码中配置数据保护，以创建持久的加密[密钥存储](xref:security/data-protection/implementation/key-management)。 如果不配置数据保护，则密钥存储在内存中。重启应用时，密钥会被丢弃。

如果密钥环存储于内存中，则在应用重启时：

* 所有基于 cookie 的身份验证令牌都无效。 
* 用户需要在下一次请求时再次登录。 
* 无法再解密使用密钥环保护的任何数据。 这可能包括 [CSRF 令牌](xref:security/anti-request-forgery#how-does-aspnet-core-mvc-address-csrf)和 [ASP.NET Core MVC tempdata cookie](xref:fundamentals/app-state#tempdata)。

若要在 IIS 下配置数据保护以持久化密钥环，请使用以下方法之一：

* **创建数据保护注册表项**

  ASP.NET Core 应用使用的数据保护密钥存储在应用外部的注册表中。 要持久保存给定应用的密钥，需为应用池创建注册表项。

  对于独立的非 Web 场 IIS 安装，可以对用于 ASP.NET Core 应用的每个应用池使用[数据保护 Provision-AutoGenKeys.ps1 PowerShell 脚本](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1)。 此脚本在 HKLM 注册表中创建注册表项，仅应用程序的应用池工作进程帐户可对其进行访问。 通过计算机范围的密钥使用 DPAPI 对密钥静态加密。

  在 web 场方案中，可以将应用配置为使用 UNC 路径存储其数据保护密钥环。 默认情况下，数据保护密钥未加密。 确保网络共享的文件权限仅限于应用在其下运行的 Windows 帐户。 可使用 X509 证书来保护静态密钥。 建议使用允许用户上传证书的机制：将证书放置在用户信任的证书存储中，并确保在运行用户应用的所有计算机上都可使用这些证书。 有关详细信息，请参阅[配置数据保护](xref:security/data-protection/configuration/overview)。

* **配置 IIS 应用程序池以加载用户配置文件**

  此设置位于应用池“高级设置”下的“进程模型”部分。 将“加载用户配置文件”设置为 `True`。 这会将密钥存储在用户配置文件目录下，并使用 DPAPI 和特定于用户帐户（用于应用池）的密钥进行保护。

* **将文件系统用作密钥环存储**

  调整应用代码，[将文件系统用作密钥环存储](xref:security/data-protection/configuration/overview)。 使用 X509 证书保护密钥环，并确保该证书是受信任的证书。 如果它是自签名证书，则将该证书放置于受信任的根存储中。

  当在 Web 场中使用 IIS 时：

  * 使用所有计算机都可以访问的文件共享。
  * 将 X509 证书部署到每台计算机。 [通过代码配置数据保护](xref:security/data-protection/configuration/overview)。

* **设置用于数据保护的计算机范围的策略**

  数据保护系统对以下操作提供有限支持：为使用数据保护 API 的所有应用设置默认[计算机范围的策略](xref:security/data-protection/configuration/machine-wide-policy)。 有关详细信息，请参阅[数据保护文档](xref:security/data-protection/index)。

## <a name="sub-application-configuration"></a>子应用程序配置

在根应用下添加的子应用不应将 ASP.NET Core 模块作为处理程序包含在其中。 如果在子应用的 web.config 文件中将该模块添加为处理程序，则在尝试浏览子应用时会收到“500.19 内部服务器错误”，即引用错误的配置文件。

以下示例显示 ASP.NET Core 子应用的已发布 web.config 文件：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

在 ASP.NET Core 应用下托管非 ASP.NET Core 子应用时，需在子应用的 web.config 文件中显式删除继承的处理程序：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

有关配置 ASP.NET Core 模块的详细信息，请参阅 [ASP.NET Core 模块简介](xref:fundamentals/servers/aspnet-core-module)和 [ASP.NET Core 模块配置参考](xref:host-and-deploy/aspnet-core-module)。

## <a name="configuration-of-iis-with-webconfig"></a>使用 web.config 配置 IIS

对于那些应用于反向代理配置的 IIS 功能，IIS 配置受 web.config 的 \<system.webServer> 部分影响。 如果在服务器级别将 IIS 配置为使用动态压缩，则可通过应用的 web.config 文件中的 \<urlCompression> 元素禁用它。

有关详细信息，请参阅 [\<system.webServer> 的配置参考](/iis/configuration/system.webServer/)、[ASP.NET Core 模块配置参考](xref:host-and-deploy/aspnet-core-module)和[配合使用 IIS 模块与 ASP.NET Core](xref:host-and-deploy/iis/modules)。 若要为在独立应用池中运行的各应用设置环境变量（IIS 10.0 或更高版本中支持此操作），请参阅 IIS 参考文档的[环境变量 \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主题中的“AppCmd.exe 命令”部分。

## <a name="configuration-sections-of-webconfig"></a>web.config 的配置节

ASP.NET Core 应用不会使用 web.config 中的 ASP.NET 4.x 应用的配置部分进行配置：

* **\<system.web>**
* \<appSettings>
* \<connectionStrings>
* \<location>

会使用其他的配置提供程序配置 ASP.NET Core 应用。 有关详细信息，请参阅[配置](xref:fundamentals/configuration/index)。

## <a name="application-pools"></a>应用程序池

在服务器上托管多个网站时，需在每个应用自己的应用池中运行各应用，以彼此隔离。 IIS“添加网站”对话框默认执行此配置。 提供了站点名称时，该文本会自动传输到“应用程序池”文本框。 添加站点时，会使用该站点名称创建新的应用池。

## <a name="application-pool-identity"></a>应用程序池标识

通过应用池标识帐户，可以在唯一帐户下运行应用，而无需创建和管理域或本地帐户。 在 IIS 8.0 或更高版本上，IIS 管理员工作进程 (WAS) 将使用新应用池的名称创建一个虚拟帐户，并默认在此帐户下运行应用池的工作进程。 在 IIS 管理控制台中，确保应用池“高级设置”下的“标识”设置为使用 ApplicationPoolIdentity：

![应用程序池“高级设置”对话框](index/_static/apppool-identity.png)

IIS 管理进程使用 Windows 安全系统中应用池的名称创建安全标识符。 可使用此标识保护资源。 但是，此标识不是真实的用户帐户，不会在 Windows 用户管理控制台中显示。

如果 IIS 工作进程需要对应用的高级访问权限，请为包含该应用的目录修改访问控制列表 (ACL)：

1. 打开 Windows 资源管理器并导航到目录。

1. 右键单击该目录，然后选择“属性”。

1. 在“安全”选项卡下，选择“编辑”按钮，然后单击“添加”按钮。

1. 选择“位置”按钮，并确保该系统处于选中状态。

1. 在“输入要选择的对象名称”区域中输入“IIS AppPool\\<app_pool_name>”。 选择“检查名称”按钮。 有关 DefaultAppPool，请检查使用 IIS AppPool\DefaultAppPool 的名称。 当选择“检查名称”按钮时，对象名称区域中会显示 DefaultAppPool 的值。 无法直接在对象名称区域中输入应用池名称。 检查对象名称时，请使用 IIS AppPool\\<app_pool_name> 格式。

   ![选择应用文件夹的用户或组对话框：在选择“检查名称”之前，将“DefaultAppPool”的应用池名称追加到对象名称区域中的“IIS AppPool\"。](index/_static/select-users-or-groups-1.png)

1. 选择“确定”。

   ![选择应用文件夹的用户或组对话框：选择“检查名称”后，对象名称“DefaultAppPool”会显示在对象名称区域中。](index/_static/select-users-or-groups-2.png)

1. 默认情况下应授予读取 &amp; 执行权限。 根据需要请提供其他权限。

也可使用 ICACLS 工具在命令提示符处授予访问权限。 以 DefaultAppPool 为例，使用以下命令：

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

有关详细信息，请参阅 [icacls](/windows-server/administration/windows-commands/icacls) 主题。

## <a name="additional-resources"></a>其他资源

* [对 IIS 上的 ASP.NET Core 进行故障排除](xref:host-and-deploy/iis/troubleshoot)
* [Azure 应用服务和 IIS 上 ASP.NET Core 的常见错误参考](xref:host-and-deploy/azure-iis-errors-reference)
* [ASP.NET Core 模块简介](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core 模块配置参考](xref:host-and-deploy/aspnet-core-module)
* [配合使用 IIS 模块与 ASP.NET Core](xref:host-and-deploy/iis/modules)
* [ASP.NET Core 简介](../index.md)
* [Microsoft IIS 官方网站](https://www.iis.net/)
* [Microsoft TechNet 库：Windows Server](/windows-server/windows-server-versions)
