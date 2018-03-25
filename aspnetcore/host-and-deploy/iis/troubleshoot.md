---
title: "解决在 IIS 上的 ASP.NET 核心"
author: guardrex
description: "了解如何诊断问题的 ASP.NET Core 应用的 Internet 信息服务 (IIS) 部署。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 65173e0101a17c64f4cde583e5bbb9fb0a9c7718
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/11/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>解决在 IIS 上的 ASP.NET 核心

作者：[Luke Latham](https://github.com/guardrex)

本文说明了如何诊断 ASP.NET Core 应用启动问题使用承载时[Internet 信息服务 (IIS)](/iis)。 这篇文章中的信息适用于在 Windows Server 和 Windows 桌面上的 IIS 中承载。

在 Visual Studio 中，ASP.NET Core 项目默认为[IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)承载在调试过程。 A *502.5 进程失败*本地调试可能会使用本主题中的建议的 troubleshooted 时发生。

其他故障排除主题：

[对 Azure 应用服务上的 ASP.NET Core 进行故障排除](xref:host-and-deploy/azure-apps/troubleshoot)  
尽管应用程序服务使用[ASP.NET 核心模块](xref:fundamentals/servers/aspnet-core-module)和 IIS 承载应用程序，请参阅说明特定于应用程序服务的专用的主题。

[错误处理](xref:fundamentals/error-handling)  
了解如何在本地系统上的开发过程中处理 ASP.NET Core 应用中的错误。

[了解如何使用 Visual Studio 进行调试](/visualstudio/debugger/getting-started-with-the-debugger)  
本主题介绍 Visual Studio 调试器的功能。

## <a name="app-startup-errors"></a>应用程序启动错误

**502.5 进程故障**  
工作进程将失败。 不启动应用程序。

ASP.NET 核心模块尝试启动工作进程，但它无法启动。 进程启动失败的原因通常根据中的条目[应用程序事件日志](#application-event-log)和[ASP.NET 核心模块 stdout 日志](#aspnet-core-module-stdout-log)。

*502.5 进程失败*当承载或应用程序配置错误导致工作进程失败时返回错误页：

![显示 502.5 进程失败页的浏览器窗口](troubleshoot/_static/process-failure-page.png)

**500 内部服务器错误**  
启动该应用程序，但某个错误阻止在完成请求的服务器。

启动过程中或创建响应时，应用程序的代码中出现此错误。 响应可能包含任何内容，或响应可能显示为*500 内部服务器错误*浏览器中。 应用程序事件日志通常表明应用程序正常启动。 从服务器的角度来看，这是正确的。 应用程序未启动，但它无法生成有效的响应。 [在命令提示符下运行应用程序](#run-the-app-at-a-command-prompt)服务器上或[启用 ASP.NET 核心模块 stdout 日志](#aspnet-core-module-stdout-log)以解决该问题。

**连接重置**

如果发送标头后，将发生错误，则服务器尝试发送太晚**500 内部服务器错误**发生错误时。 响应的复杂对象的序列化期间发生错误时经常会出现这种情况。 此类型的错误显示为*连接重置*客户端上的错误。 [应用程序日志记录](xref:fundamentals/logging/index)可以帮助解决这些类型的错误。

## <a name="default-startup-limits"></a>默认启动限制

ASP.NET 核心模块配置了默认值*startupTimeLimit*的 120 秒。 时保留为默认值，应用可能需要最多两分钟，以启动之前模块记录进程失败。 有关模块的配置的信息，请参阅[aspNetCore 元素的特性的](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。

## <a name="troubleshoot-app-startup-errors"></a>应用程序启动错误疑难解答

### <a name="application-event-log"></a>应用程序事件日志

访问应用程序事件日志：

1. 打开开始菜单，搜索**事件查看器**，然后选择**事件查看器**应用。
1. 在**事件查看器**，打开**Windows 日志**节点。
1. 选择**应用程序**以打开应用程序事件日志。
1. 搜索与失败应用相关联的错误。 错误都具有值*IIS AspNetCore 模块*或*IIS Express AspNetCore 模块*中*源*列。

### <a name="run-the-app-at-a-command-prompt"></a>在命令提示符下运行应用程序

许多启动错误未生成应用程序事件日志中的有用信息。 你可以通过在命令提示符下运行应用，在主机系统上找到某些错误的原因。

**依赖于框架的部署**

如果在应用程序处于[framework 相关部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

1. 在命令提示符处，导航到部署文件夹，并通过执行应用程序的程序集与运行应用*dotnet.exe*。 在下面的命令中，应用程序的程序集的名称替换\<程序集 _ 名称 >: `dotnet .\<assembly_name>.dll`。
1. 控制台应用程序中，显示任何错误，从输出写入到控制台窗口。
1. 如果向应用程序发出请求时出现错误，请向主机和其中 Kestrel 侦听的端口发出请求。 使用默认主机和开机自检，请发出请求以`http://localhost:5000/`。 如果应用程序通常响应 Kestrel 终结点地址，该问题更可能是相关的反向代理配置和不太可能在应用内。

**自包含的部署**

如果在应用程序处于[独立的部署](/dotnet/core/deploying/#self-contained-deployments-scd):

1. 在命令提示符处，导航到部署文件夹并运行应用程序的可执行文件。 在下面的命令中，应用程序的程序集的名称替换\<程序集 _ 名称 >: `<assembly_name>.exe`。
1. 控制台应用程序中，显示任何错误，从输出写入到控制台窗口。
1. 如果向应用程序发出请求时出现错误，请向主机和其中 Kestrel 侦听的端口发出请求。 使用默认主机和开机自检，请发出请求以`http://localhost:5000/`。 如果应用程序通常响应 Kestrel 终结点地址，该问题更可能是相关的反向代理配置和不太可能在应用内。

### <a name="aspnet-core-module-stdout-log"></a>ASP.NET 核心模块 stdout 日志

用于启用和查看 stdout 日志：

1. 导航到主机系统上的站点的部署文件夹。
1. 如果*日志*文件夹不存在、 创建文件夹。 有关如何启用 MSBuild 的说明创建*日志*部署中的文件夹自动，请参阅[目录结构](xref:host-and-deploy/directory-structure)主题。
1. 编辑*web.config*文件。 设置**stdoutLogEnabled**到`true`和更改**stdoutLogFile**路径以指向*日志*文件夹 (例如， `.\logs\stdout`)。 `stdout`在路径中是日志文件名称前缀。 时间戳、 进程 id 和文件扩展名自动添加了时创建日志。 使用`stdout`作为文件名称前缀，典型的日志文件名为*stdout_20180205184032_5412.log*。 
1. 保存已更新*web.config*文件。
1. 向应用程序发出的请求。
1. 导航到*日志*文件夹。 查找并打开最新的标准输出日志。
1. 研究错误的日志。

**重要 ！** 禁用完成故障排除时，日志记录 stdout。

1. 编辑*web.config*文件。
1. 设置**stdoutLogEnabled**到`false`。
1. 保存该文件。

> [!WARNING]
> 若要禁用 stdout 日志可能会导致应用程序或服务器失败。 没有对日志文件大小没有限制或创建的日志文件数。
>
> 对于例程日志记录在 ASP.NET Core 应用程序，使用限制日志文件大小和旋转日志的日志记录库。 有关详细信息，请参阅[第三方日志记录提供程序](xref:fundamentals/logging/index#third-party-logging-providers)。

## <a name="enabling-the-developer-exception-page"></a>启用开发人员异常页

`ASPNETCORE_ENVIRONMENT` [环境变量可以添加到 web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)在开发环境中运行应用程序。 只要环境不重写中的应用程序启动`UseEnvironment`主机生成器中，在设置环境变量允许[开发人员异常页](xref:fundamentals/error-handling)显示应用程序运行时。

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

设置环境变量中的`ASPNETCORE_ENVIRONMENT`建议只在过渡和测试不公开到 Internet 的服务器上使用。 删除从该环境变量*web.config*诊断后的文件。 有关设置环境变量的信息*web.config*，请参阅[environmentVariables 子元素的 aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。

## <a name="common-startup-errors"></a>常见的启动错误 

请参阅[ASP.NET 核心常见错误参考](xref:host-and-deploy/azure-iis-errors-reference)。 中的参考主题介绍了大部分常见防止应用程序启动的问题。

## <a name="slow-or-hanging-app"></a>慢速或悬挂应用程序

当应用程序响应速度很慢或挂起的请求时，获取并分析[转储文件](/visualstudio/debugger/using-dump-files)。 可以使用任何以下工具获取转储文件：

* [ProcDump](/sysinternals/downloads/procdump)
* [DebugDiag](https://www.microsoft.com/download/details.aspx?id=49924)
* WinDbg:[下载适用于 Windows 调试工具](https://developer.microsoft.com/windows/hardware/download-windbg)，[调试使用 WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg)

## <a name="remote-debugging"></a>远程调试

请参阅[在 Visual Studio 2017 远程 IIS 计算机上的远程调试 ASP.NET Core](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) Visual Studio 文档中。

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/)提供从托管的 IIS，包括错误日志记录和报告功能的应用程序的遥测。 Application Insights 只能针对在应用程序启动时应用的日志记录功能变得可用之后发生的错误报告。 有关详细信息，请参阅[Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)。

## <a name="additional-troubleshooting-advice"></a>其他故障排除建议

有时会正常运行的应用程序升级任一.NET 核心 SDK 在应用内的开发计算机或包版本之后立即失败。 在某些情况下，不同的包可能在执行主要升级时中断应用。 可以按以下说明来修复大部分这些问题：

1. 删除*bin*和*obj*文件夹。
1. 在包的缓存清除*%userprofile%\\.nuget\\包*和*%localappdata%\\Nuget\\v3 缓存*。
1. 还原和重新生成项目。
1. 确认已完全重新部署应用程序之前删除以前部署到服务器上。

> [!TIP]
> 清除包缓存的简便方法是执行`dotnet nuget locals all --clear`从命令提示符。
> 
> 清除包缓存还可通过使用[nuget.exe](https://www.nuget.org/downloads)工具并执行命令`nuget locals all -clear`。 *nuget.exe*必须从单独获取和不与 Windows 桌面操作系统的捆绑的安装[NuGet 网站](https://www.nuget.org/downloads)。

## <a name="additional-resources"></a>其他资源

* [ASP.NET Core 中的错误处理简介](xref:fundamentals/error-handling)
* [Azure 应用服务和 IIS 上 ASP.NET Core 的常见错误参考](xref:host-and-deploy/azure-iis-errors-reference)
* [ASP.NET Core 模块配置参考](xref:host-and-deploy/aspnet-core-module)
* [对 Azure 应用服务上的 ASP.NET Core 进行故障排除](xref:host-and-deploy/azure-apps/troubleshoot)
