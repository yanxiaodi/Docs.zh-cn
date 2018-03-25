---
title: "Azure App Service 和 ASP.NET Core 了 IIS 的常见错误参考"
author: guardrex
description: "承载 Azure 应用程序服务和 IIS 上的 ASP.NET Core 应用时，请区分常见错误。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: cd9f8fc310ba0258477db51aa416c03debadeffe
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Azure App Service 和 ASP.NET Core 了 IIS 的常见错误参考

作者：[Luke Latham](https://github.com/guardrex)

以下不是错误的完整列表。 如果你遇到此处未列出的错误[打开一个新问题](https://github.com/aspnet/Docs/issues/new)与再现错误的详细说明。

收集以下信息：

* 浏览器的行为
* 应用程序事件日志条目
* ASP.NET 核心模块 stdout 日志条目

比较以下常见错误的信息。 如果找到匹配项，请按照故障排除建议。

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a>安装程序无法获取 VC++ Redistributable

* **安装程序异常：**0x80072efd 或 0x80072f76 - 未指定的错误

* **安装程序日志异常&#8224;：**0x80072efd 或 0x80072f76 错误：未能执行 EXE 包

  &#8224;此日志位于 C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log。

疑难解答：

* 如果在安装服务器托管捆绑包时系统无法访问 Internet，则在系统阻止安装程序获取 Microsoft Visual C++ 2015 Redistributable 时会出现此异常。 获取从安装[Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840)。 如果安装失败，服务器可能不会收到托管依赖于框架的部署 (FDD) 所需.NET Core 运行时。 如果托管 FDD，确认在程序中安装了运行时&amp;功能。 如果需要获取从运行时安装[.NET 下载](https://www.microsoft.com/net/download/core)。 安装运行时后，重启系统，或通过从命令提示符依次执行 net stop was /y 和 net start w3svc 来重启 IIS。

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>OS 升级删除了 32 位 ASP.NET Core 模块

* **应用程序日志：**未能加载模块 DLL C:\WINDOWS\system32\inetsrv\aspnetcore.dll。 该数据是个错误。

疑难解答：

* OS 升级期间不会保留 C:\Windows\SysWOW64\inetsrv 目录中的非 OS 文件。 如果之前安装 ASP.NET 核心模块操作系统升级，然后任何应用程序池运行时在 32 位模式下 OS 升级之后，遇到此问题。 在 OS 升级后修复 ASP.NET Core 模块。 请参阅[安装 .NET Core Windows Server 托管捆绑包](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle)。 选择**修复**安装运行时。

## <a name="platform-conflicts-with-rid"></a>平台与 RID 冲突

* **浏览器：**HTTP 错误 502.5 - 进程失败

* **应用程序日志：**应用程序 ' MACHINE/WEBROOT/APPHOST / {程序集} 与物理根 c:\{路径}\'无法使用命令行启动进程"c:\\{路径} {程序集}。 {exe | dll}"，错误代码 ="0x80004005: ff。

* **ASP.NET 核心模块日志：**未经处理的异常： System.BadImageFormatException： 无法加载文件或程序集 {程序集}.dll。 试图加载的程序的格式不正确。

疑难解答：

* 确认应用在 Kestrel 上本地运行。 进程失败可能是由应用的内部问题导致的。 有关详细信息，请参阅[故障排除](xref:host-and-deploy/iis/troubleshoot)。

* 确认`<PlatformTarget>`中*.csproj*不会与 RID 冲突。 例如，未指定`<PlatformTarget>`的`x86`和发布与的 RID `win10-x64`，通过使用*dotnet 发布-c 版本-r win10 x64*或通过设置`<RuntimeIdentifiers>`中*.csproj*到`win10-x64`。 项目在不显示任何警告或错误的情况下发布，但会失败，且系统上出现上述记录的异常。

* 如果此异常发生的 Azure 应用程序部署升级应用程序时，以及手动部署更新的程序集，从以前的部署中删除所有文件。 部署升级的应用时，延迟的不兼容程序集可能造成 `System.BadImageFormatException` 异常。

## <a name="uri-endpoint-wrong-or-stopped-website"></a>URI 终结点错误或网站已停止

* **浏览器：**ERR_CONNECTION_REFUSED

* **应用程序日志：**没有任何条目

* **ASP.NET Core 模块日志：**未创建日志文件

疑难解答：

* 确认正在使用正确的 URI 终结点应用程序。 请检查绑定中。

* 确认 IIS 网站不在*已停止*状态。

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>已禁用 CoreWebEngine 或 W3SVC 服务器功能

* **OS 异常：**必须安装 IIS 7.0 CoreWebEngine 和 W3SVC 功能，以便使用 ASP.NET Core 模块。

疑难解答：

* 确认已启用适当的角色和功能。 请参阅 [IIS 配置](xref:host-and-deploy/iis/index#iis-configuration)。

## <a name="incorrect-website-physical-path-or-app-missing"></a>不正确的网站物理路径或缺少应用程序

* **浏览器：**403 禁止访问 - 访问被拒绝 --或者-- 403.14 禁止访问 - Web 服务器被配置为不列出此目录的内容。

* **应用程序日志：**没有任何条目

* **ASP.NET Core 模块日志：**未创建日志文件

疑难解答：

* 检查 IIS 网站**基本设置**和物理应用文件夹。 确认应用程序是否在 IIS 网站的文件夹中**物理路径**。

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>角色不正确、未安装模块或权限不正确

* **浏览器：**500.19 内部服务器错误 - 无法访问请求的页面，因为该页面的相关配置数据无效。

* **应用程序日志：**没有任何条目

* **ASP.NET Core 模块日志：**未创建日志文件

疑难解答：

* 确认启用了适当的角色。 请参阅 [IIS 配置](xref:host-and-deploy/iis/index#iis-configuration)。

* 检查“程序和功能”，确认已安装Microsoft ASP.NET Core 模块。 如果已安装程序列表中没有 Microsoft ASP.NET Core 模块，请安装该模块。 请参阅[安装 .NET Core Windows Server 托管捆绑包](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle)。

* 请确保**应用程序池** > **进程模型** > **标识**设置为**ApplicationPoolIdentity**或自定义标识具有访问应用程序的部署文件夹的正确权限。

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>processPath 不正确、缺少 PATH 变量、未安装托管捆绑包、未重启系统/IIS、未安装 VC++ Redistributable 或 dotnet.exe 访问冲突

* **浏览器：**HTTP 错误 502.5 - 进程失败

* **应用程序日志：**应用程序 ' MACHINE/WEBROOT/APPHOST / {程序集} 与物理根 c:\\{路径}\'无法使用命令行启动进程"。\{程序集}.exe"，错误代码 ="0x80070002: 0。

* **ASP.NET Core 模块日志：**已创建日志文件，但该日志文件为空

疑难解答：

* 确认应用在 Kestrel 上本地运行。 进程失败可能是由应用的内部问题导致的。 有关详细信息，请参阅[故障排除](xref:host-and-deploy/iis/troubleshoot)。

* 检查*processPath*属性`<aspNetCore>`中的元素*web.config*以确认它是*dotnet*为依赖于框架的部署 (FDD) 或*.\{程序集}.exe*自包含的部署 (SCD)。

* 对于 FDD，可能无法通过路径设置访问 dotnet.exe。 确认系统路径设置中存在 *C:\Program Files\dotnet\*。

* 对于 FDD，应用程序池的用户标识可能无法访问 dotnet.exe。 确认应用程序池用户标识具有访问 C:\Program Files\dotnet 目录的权限。 确认没有拒绝规则配置上的应用程序池用户标识*C:\Program Files\dotnet*和应用程序目录。

* FDD 可能已部署，.NET 核心安装，无需重新启动 IIS。 重启服务器，或通过从命令提示符依次执行 net stop was /y 和 net start w3svc 来重启 IIS。

* FDD 可能已不在主机系统上安装.NET Core 运行时的情况下部署。 如果尚未安装.NET Core 运行时，运行**.NET 核心 Windows 服务器承载捆绑安装**系统上。 请参阅[安装 .NET Core Windows Server 托管捆绑包](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle)。 如果尝试在未连接到 Internet 的系统上安装.NET Core 运行时，获取从运行时[.NET 下载](https://www.microsoft.com/net/download/core)并运行托管的捆绑包安装，安装 ASP.NET 核心模块。 重启系统，或通过从命令提示符依次执行 net stop was /y 和 net start w3svc 来重启 IIS，完成安装。

* FDD 可能已部署和*Microsoft Visual c + + 2015年可再发行组件 (x64)*系统上未安装。 获取从安装[Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840)。

## <a name="incorrect-arguments-of-aspnetcore-element"></a>\<aspNetCore\> 元素的参数不正确

* **浏览器：**HTTP 错误 502.5 - 进程失败

* **应用程序日志：**应用程序 ' MACHINE/WEBROOT/APPHOST / {程序集} 与物理根 c:\\{路径}\'无法使用命令行启动进程"dotnet"。\{程序集}.dll，错误代码 ="0x80004005: 80008081。

* **ASP.NET 核心模块日志：**要执行的应用程序不存在: 路径\{程序集}.dll

疑难解答：

* 确认应用在 Kestrel 上本地运行。 进程失败可能是由应用的内部问题导致的。 有关详细信息，请参阅[故障排除](xref:host-and-deploy/iis/troubleshoot)。

* 检查*参数*属性`<aspNetCore>`中的元素*web.config*要确认它是否是 (a) *。\{程序集}.dll*依赖于框架的部署 (FDD); 或 (b) 不存在空字符串 (*参数 =""*)，或应用程序的自变量列表 (*参数 ="arg1，arg2，..."*)为独立部署 (SCD)。

## <a name="missing-net-framework-version"></a>缺少 .NET Framework 版本

* **浏览器：**502.3 错误网关 - 尝试路由请求时出现连接错误。

* **应用程序日志：** ErrorCode = 应用程序 ' MACHINE/WEBROOT/APPHOST / {程序集} 与物理根 c:\\{路径}\'无法使用命令行启动进程"dotnet"。\{程序集}.dll，错误代码 ="0x80004005: 80008081。

* **ASP.NET Core 模块日志：**缺少方法、文件或程序集异常。 在异常中指定的方法、文件或程序集是 .NET Framework 方法、文件或程序集。

疑难解答：

* 安装系统缺少的 .NET Framework 版本。

* 对于依赖于框架的部署 (FDD)，确认正确的运行时系统上安装。 如果项目从 1.1 升级到 2.0，部署到主机系统，并且此异常结果，请确保 2.0 framework 是在主机系统。

## <a name="stopped-application-pool"></a>应用程序池已停止

* **浏览器：**503 服务不可用

* **应用程序日志：**没有任何条目

* **ASP.NET Core 模块日志：**未创建日志文件

疑难解答

* 确认应用程序池不在*已停止*状态。

## <a name="iis-integration-middleware-not-implemented"></a>未实现 IIS 集成中间件

* **浏览器：**HTTP 错误 502.5 - 进程失败

* **应用程序日志：**应用程序 ' MACHINE/WEBROOT/APPHOST / {程序集} 与物理根 c:\\{路径}\'使用命令行创建过程"c:\\{路径}\{程序集}。 {exe | dll}"但损坏或未不响应，或者没有侦听给定的端口 {PORT}，错误代码 ="0x800705b4"

* **ASP.NET Core 模块日志：**已创建日志文件，且该日志文件显示正常操作。

疑难解答

* 确认应用在 Kestrel 上本地运行。 进程失败可能是由应用的内部问题导致的。 有关详细信息，请参阅[故障排除](xref:host-and-deploy/iis/troubleshoot)。

* 请确认：
  * IIS 集成中间件是 referencedby 调用`UseIISIntegration`上应用的方法`WebHostBuilder`(ASP.NET Core 1.x)
  * 应用程序使用`CreateDefaultBuilder`方法 (ASP.NET Core 2.x)。
  
  有关详细信息，请参阅 [ASP.NET Core 中的承载](xref:fundamentals/hosting)。

## <a name="sub-application-includes-a-handlers-section"></a>子应用程序包括 \<handlers\> 部分

* **浏览器：**HTTP 错误 500.19 - 内部服务器错误

* **应用程序日志：**没有任何条目

* **ASP.NET 核心模块日志：**日志文件创建并显示有关根应用程序的正常操作。 不会为订阅应用程序创建的日志文件。

疑难解答

* 确认子应用的 web.config 文件不包括 `<handlers>` 部分。

## <a name="application-configuration-general-issue"></a>应用程序配置常见问题

* **浏览器：**HTTP 错误 502.5 - 进程失败

* **应用程序日志：**应用程序 ' MACHINE/WEBROOT/APPHOST / {程序集} 与物理根 c:\\{路径}\'使用命令行创建过程"c:\\{路径}\{程序集}。 {exe | dll}"但损坏或未不响应，或者没有侦听给定的端口 {PORT}，错误代码 ="0x800705b4"

* **ASP.NET Core 模块日志：**已创建日志文件，但该日志文件为空

疑难解答

* 此常规异常指示进程无法启动，很可能由于应用程序配置问题。 引用[目录结构](xref:host-and-deploy/directory-structure)，确认应用程序的部署文件和文件夹是适当和应用程序的配置文件是否存在并且包含正确的设置为应用程序和环境。 有关详细信息，请参阅[故障排除](xref:host-and-deploy/iis/troubleshoot)。
