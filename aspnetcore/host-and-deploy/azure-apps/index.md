---
title: "在 Azure 应用服务上托管 ASP.NET Core"
author: guardrex
description: "通过指向有用资源的链接，了解如何在 Azure 应用服务中托管 ASP.NET Core 应用。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: cefbc27c8091a2ed1441663e3779d67aae2c64dd
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a>在 Azure 应用服务上托管 ASP.NET Core

[Azure 应用服务](https://azure.microsoft.com/services/app-service/)是一个用于托管 Web 应用（包括 ASP.NET Core）的 [Microsoft 云计算平台服务](https://azure.microsoft.com/)。

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

## <a name="useful-resources"></a>有用资源

Azure 应用文档、教程、示例、操作指南和其他资源由 Azure [Web 应用文档](/azure/app-service/)提供。 两个有关托管 ASP.NET Core 应用的重要教程为：

[快速入门：在 Azure 中创建 ASP.NET Core Web 应用](/azure/app-service/app-service-web-get-started-dotnet)  
使用 Visual Studio 创建 ASP.NET Core Web 应用，并将其部署到 Windows 上的 Azure 应用服务。

[快速入门：在 Linux 上的应用服务中创建 .NET Core Web 应用](/azure/app-service/containers/quickstart-dotnetcore)  
使用命令行创建 ASP.NET Core Web 应用，并将其部署到 Linux 上的 Azure 应用服务。

ASP.NET Core 文档中提供以下文章：

[使用 Visual Studio 发布到 Azure](xref:tutorials/publish-to-azure-webapp-using-vs)  
了解如何使用 Visual Studio 将 ASP.NET Core 应用发布到 Azure 应用服务。

[使用 CLI 工具发布到 Azure](xref:tutorials/publish-to-azure-webapp-using-cli)  
了解如何使用 Git 命令行客户端将 ASP.NET Core 应用发布到 Azure 应用服务。

[使用 Visual Studio 和 Git 持续部署到 Azure](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
了解如何使用 Visual Studio 创建 ASP.NET Core Web 应用并使用 Git 将它部署到 Azure 应用服务以实现持续部署。

[使用 VSTS 持续部署到 Azure](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
为 ASP.NET Core 应用设置 CI 生成，然后创建针对 Azure 应用服务的持续部署发布。

[Azure Web 应用沙盒](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
探索 Azure Apps 平台强制实施的 Azure 应用服务运行时执行限制。

## <a name="application-configuration"></a>应用程序配置

在 ASP.NET Core 2.0 和更高版本中，[Microsoft.AspNetCore.All 元包](xref:fundamentals/metapackage)中的三个包为部署到 Azure 应用服务的应用提供自动日志记录功能：

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) 使用 [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) 提供 ASP.NET Core 与 Azure 应用服务的启动集成。 添加的日志记录功能由 `Microsoft.AspNetCore.AzureAppServicesIntegration` 包提供。
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) 执行 [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics)，在 `Microsoft.Extensions.Logging.AzureAppServices` 包中添加 Azure 应用服务诊断日志记录提供程序。
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) 提供记录器实现，支持 Azure 应用服务诊断日志和日志流式处理功能。

## <a name="monitoring-and-logging"></a>监视和日志记录

有关监视、日志记录和故障排除的信息，请参阅以下文章：

[如何：在 Azure 应用服务中监视应用](/azure/app-service/web-sites-monitor)  
了解如何查看应用和应用服务计划的配额和指标。

[在 Azure 应用服务中为应用启用诊断日志记录](/azure/app-service/web-sites-enable-diagnostic-log)  
了解如何启用和访问 HTTP 状态代码、失败请求和 Web 服务器活动的诊断日志记录。

[ASP.NET Core 中的错误处理简介](xref:fundamentals/error-handling)  
了解在 ASP.NET Core 应用中处理错误的常见方法。

[对 Azure 应用服务上的 ASP.NET Core 进行故障排除](xref:host-and-deploy/azure-apps/troubleshoot)  
了解如何使用 ASP.NET Core 应用诊断 Azure 应用服务部署问题。

[Azure 应用服务和 IIS 上 ASP.NET Core 的常见错误参考](xref:host-and-deploy/azure-iis-errors-reference)  
使用故障排除建议查看 Azure 应用服务/IIS 托管的应用的常见部署配置错误。

## <a name="data-protection-key-ring-and-deployment-slots"></a>数据保护密钥环和部署槽位

[数据保护密钥](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)保存在 %HOME%\ASP.NET\DataProtection-Keys 文件夹中。 此文件夹由网络存储提供支持，并跨托管应用的所有计算机同步。 密钥不是静态保护的。 此文件夹向单个部署槽位中应用的所有实例提供密钥环。 各部署槽位（例如过渡槽和生成槽）不共享密钥环。

在部署槽位之间交换时，使用数据保护的任意系统都无法使用之前槽位中的密钥环来解密存储的数据。 ASP.NET Cookie 中间件使用数据保护来保护其 cookie。 这导致用户注销使用标准 ASP.NET Cookie 中间件的应用。 对于独立于槽位的密钥环解决方案，请使用外部密钥环提供程序，例如：

* Azure Blob 存储
* Azure Key Vault
* SQL 存储
* Redis 缓存

有关详细信息，请参阅[密钥存储提供程序](xref:security/data-protection/implementation/key-storage-providers)。

## <a name="additional-resources"></a>其他资源

* [Web 应用概述（5 分钟概述视频）](/azure/app-service/app-service-web-overview)
* [Azure 应用服务：托管 .NET 应用的最佳位置（55 分钟概述视频）](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday：Azure 应用服务诊断和故障排除体验（12 分钟视频）](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Azure 应用服务诊断概述](/azure/app-service/app-service-diagnostics)

Windows Server 上的 Azure 应用服务使用 [Internet Information Services (IIS)](https://www.iis.net/)。 以下是基础 IIS 技术的相关主题：

* [使用 IIS 在 Windows 上托管 ASP.NET Core](xref:host-and-deploy/iis/index)
* [ASP.NET Core 模块简介](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core 模块配置参考](xref:host-and-deploy/aspnet-core-module)
* [配合使用 IIS 模块与 ASP.NET Core](xref:host-and-deploy/iis/modules)
* [Microsoft TechNet 库：Windows Server](/windows-server/windows-server-versions)
