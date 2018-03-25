---
title: "在 Windows 服务中的主机 ASP.NET 核心"
author: tdykstra
description: "了解如何托管的 ASP.NET Core 应用 Windows 服务中。"
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: f3455e47cfc06a4492dc4e34871b348184c6ecfb
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="host-aspnet-core-in-a-windows-service"></a>在 Windows 服务中的主机 ASP.NET 核心

作者：[Tom Dykstra](https://github.com/tdykstra)

没有使用 IIS 是在运行承载 ASP.NET Core 应用在 Windows 上的推荐的方式[Windows 服务](/dotnet/framework/windows-services/introduction-to-windows-service-applications)。 当托管为 Windows 服务，应用程序可以自动启动之后启动重新启动和崩溃而无需人工干预。

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）。 有关说明如何运行示例应用程序，请参阅示例的*README.md*文件。

## <a name="prerequisites"></a>系统必备

* 应用程序必须在.NET Framework 运行时上运行。 在*.csproj*文件中，指定为相应值[TargetFramework](/nuget/schema/target-frameworks)和[RuntimeIdentifier](/dotnet/articles/core/rid-catalog)。 以下是一个示例：

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  当在 Visual Studio 中创建项目，请使用**ASP.NET 核心应用程序 (.NET Framework)**模板。

* 如果应用程序接收来自 Internet （而不仅仅是从内部网络） 的请求，它必须使用[HTTP.sys](xref:fundamentals/servers/httpsys) web 服务器 (以前称为[WebListener](xref:fundamentals/servers/weblistener)对于 ASP.NET Core 1.x 应用程序) 而不是[Kestrel](xref:fundamentals/servers/kestrel)。 IIS 被建议用于为反向代理服务器的 Kestrel 边缘部署。 有关详细信息，请参阅[何时结合使用 Kestrel 和反向代理](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。

## <a name="get-started"></a>入门

本部分介绍将现有的 ASP.NET Core 项目设置为在服务中运行所需的最小更改。

1. 安装 NuGet 程序包[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/)。

1. 进行中的以下更改`Program.Main`:
  
   * 调用`host.RunAsService`而不是`host.Run`。
  
   * 如果代码调用`UseContentRoot`，到发布位置而不是使用路径`Directory.GetCurrentDirectory()`。

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

1. 将应用发布到的文件夹。 使用[dotnet 发布](/dotnet/articles/core/tools/dotnet-publish)或[Visual Studio 发布配置文件](xref:host-and-deploy/visual-studio-publish-profiles)，它发布到的文件夹。

1. 通过创建和启动服务测试。

   使用管理特权才能使用打开命令行界面[sc.exe](https://technet.microsoft.com/library/bb490995)命令行工具来创建和启动服务。 如果该服务名为 MyService，发布到`c:\svc`，以及名为 AspNetCoreService，命令：

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   `binPath`值是应用程序的可执行文件，其中包括可执行文件名称的路径。

   ![控制台窗口创建并启动示例](windows-service/_static/create-start.png)

   在这些命令完成后，浏览到相同的路径作为控制台应用程序运行时 (默认情况下， `http://localhost:5000`):

   ![服务中运行](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a>提供服务的外部运行的方法

很容易地测试和调试时运行外部服务，因此通常将调用的代码添加`RunAsService`仅在某些情况下。 例如，应用程序可以作为包含的控制台应用程序运行`--console`命令行自变量，或如果附加调试器：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a>处理停止和启动事件

若要处理`OnStarting`， `OnStarted`，和`OnStopping`事件，进行以下其他更改：

1. 创建一个类，派生自`WebHostService`:

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

1. 创建扩展方法`IWebHost`，通过自定义`WebHostService`到`ServiceBase.Run`:

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

1. 在`Program.Main`，调用新的扩展方法， `RunAsCustomService`，而不是`RunAsService`:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

如果自定义`WebHostService`代码需要从依赖关系注入 （如记录器） 服务，以获取从`Services`属性`IWebHost`:

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="acknowledgments"></a>致谢

本文是已发布的源的帮助下编写的：

* [作为 Windows 服务承载 ASP.NET 核心](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [如何托管 Windows 服务中将 ASP.NET 核心](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
