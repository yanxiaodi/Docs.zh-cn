---
title: "托管和部署 ASP.NET Core"
author: tdykstra
description: "了解如何设置托管环境和部署 ASP.NET Core 应用。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/index
ms.openlocfilehash: baa77eba837ff8b86ad543a74ebeee51ace4c25d
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="host-and-deploy-aspnet-core"></a>托管和部署 ASP.NET Core

一般而言，向托管环境部署 ASP.NET Core 应用需执行以下操作：

* 将应用发布到托管服务器上的文件夹。
* 设置进程管理器，该管理器在收到请求时启动应用，并在应用发生故障或服务器重启后重新启动应用。
* 设置将请求转发到应用的反向代理。

## <a name="publish-to-a-folder"></a>发布到文件夹 

[dotnet 发布](/dotnet/articles/core/tools/dotnet-publish) CLI 命令编译应用代码并复制所需的文件，以便在“发布”文件夹中运行应用。 从 Visual Studio 进行部署时，在文件被复制到部署目标前，系统会自动执行 [dotnet publish](/dotnet/core/tools/dotnet-publish) 步骤。

### <a name="folder-contents"></a>文件夹内容

“发布”文件夹包含应用的“.exe”和“.dll”文件、应用依赖项以及 .NET 运行时（根据需要）。

.NET Core 应用可以作为“独立”应用或“依赖于框架”的应用进行发布。 如果应用是独立应用，则包含 .NET 运行时的“.dll”文件会包括在“发布”文件夹内。 如果应用依赖于框架，.NET 运行时文件就不包含在内，因为应用包含对服务器上安装的 .NET 版本的引用。 默认部署模型是依赖于框架的模型。 有关详细信息，请参阅 [.NET Core 应用程序部署](/dotnet/articles/core/deploying/index)。

除了“.exe”和“.dll”文件以外，ASP.NET Core 应用的“发布”文件夹通常包含配置文件、静态资产和 MVC 视图。 有关详细信息，请参阅[目录结构](xref:host-and-deploy/directory-structure)。

## <a name="set-up-a-process-manager"></a>设置进程管理器

ASP.NET Core 应用是一个控制台应用，在服务器启动时必须启动该应用，并且在出现故障后必须重新启动它。 若要自动启动和重新启动，需要使用进程管理器。 用于 ASP.NET Core 的最常见进程管理器是：

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [IIS](xref:host-and-deploy/iis/index)
  * [Windows 服务](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>设置反向代理

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

如果该应用使用的是 [Kestrel](xref:fundamentals/servers/kestrel) Web 服务器，则可以将 [Nginx](xref:host-and-deploy/linux-nginx)、[Apache](xref:host-and-deploy/linux-apache) 或 [IIS](xref:host-and-deploy/iis/index) 用作反向代理服务器。 反向代理服务器接收到来自 Internet 的 HTTP 请求，并在进行一些初步处理后将这些请求转发到 Kestrel。 有关详细信息，请参阅[何时结合使用 Kestrel 和反向代理](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy)。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

如果该应用使用的是 [Kestrel](xref:fundamentals/servers/kestrel) Web 服务器并将被公开到 Internet，则需将 [Nginx](xref:host-and-deploy/linux-nginx)、[Apache](xref:host-and-deploy/linux-apache) 或 [IIS](xref:host-and-deploy/iis/index) 用作反向代理服务器。 反向代理服务器接收到来自 Internet 的 HTTP 请求，并在进行一些初步处理后将这些请求转发到 Kestrel。 使用反向代理的主要原因是出于安全考虑。 有关详细信息，请参阅[何时结合使用 Kestrel 和反向代理](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy)。

---

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>使用 Visual Studio 和 MSBuild 自动执行部署

除将输出从 [dotnet publish](/dotnet/core/tools/dotnet-publish) 复制到服务器外，部署通常还需要其他任务。 例如，可能需要从“发布”文件夹获取或排除额外文件。 Visual Studio 使用 MSBuild 进行 Web 部署，用户可以自定义 MSBuild 以在部署期间执行其他任务。 有关详细信息，请参阅 [Visual Studio 中的发布配置文件](xref:host-and-deploy/visual-studio-publish-profiles)和[使用 MSBuild 和 Team Foundation Build](http://msbuildbook.com/) 一书。

通过[发布 Web 功能](xref:tutorials/publish-to-azure-webapp-using-vs)或[内置 Git 支持](xref:host-and-deploy/azure-apps/azure-continuous-deployment)，可以将应用从 Visual Studio 直接部署到 Azure 应用服务。 Visual Studio Team Services 支持[对 Azure App Service 进行持续部署](/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts)。

## <a name="publishing-to-azure"></a>发布到 Azure

有关如何使用 Visual Studio 将应用发布到 Azure 的说明，请参阅[使用 Visual Studio 将 ASP.NET Core Web 应用发布到 Azure App Service](xref:tutorials/publish-to-azure-webapp-using-vs)。 还可以从[命令行](xref:tutorials/publish-to-azure-webapp-using-cli)将该应用发布到 Azure。

## <a name="additional-resources"></a>其他资源

有关将 Docker 用作托管环境的信息，请参阅[在 Docker 中托管 ASP.NET Core 应用](xref:host-and-deploy/docker/index)。
