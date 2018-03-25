---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: "配置 TFS 生成服务器用于 Web 部署 |Microsoft 文档"
author: jrjlee
description: "本主题介绍如何准备要生成和部署解决方案使用 Team Build 和 Internet Informat 的 Team Foundation Server (TFS) 生成服务器..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: de31a9dffb95b863a4ec38b74fd2c6e03f287a7f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="configuring-a-tfs-build-server-for-web-deployment"></a>配置用于 Web 部署的 TFS 生成服务器
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何准备要生成和部署你使用 Team Build 和 Internet 信息服务 (IIS) Web 部署工具 （Web 部署） 的解决方案的 Team Foundation Server (TFS) 生成服务器。


本主题窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器解决方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; 来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序Communication Foundation (WCF) 服务和数据库项目。

这些教程的核心的部署方法取决于中介绍的拆分项目文件方法[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在其中生成过程控制由两个项目文件 （&） #x 2014; 一个包含生成适用于每种目标环境和一个包含特定于环境的生成和部署设置的说明。 在生成期间，特定于环境的项目文件合并到环境无关的项目文件中以形成一组完整的生成说明。

## <a name="task-overview"></a>任务概述

若要准备要生成和部署解决方案的生成服务器，你将需要：

- 安装和配置 TFS 生成服务。
- 安装 Visual Studio 2010。
- 安装任何产品或生成你的解决方案，如版本的.NET Framework 或 ASP.NET MVC 的所需的组件。
- 安装 Web 部署 2.0 或更高版本。

本主题将演示如何执行这些过程，或指向在它们存在的其他资源。 任务和本主题中的演练假定：

- 你刚开始使用干净服务器生成运行 Windows Server 2008 R2 Service Pack 1。
- 该服务器已加入域的具有静态 IP 地址。
- 你已在单独服务器上，安装在 TFS 应用层中, 所述[企业 Web 部署： 方案概述](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)。

### <a name="who-performs-these-procedures"></a>这些过程的执行者是谁？

在大多数情况下，TFS 管理员将负责配置生成服务器。 在某些情况下，开发人员团队可能需要特定的生成服务器的所有权。

## <a name="install-and-configure-the-tfs-build-service"></a>安装和配置 TFS 生成服务

当你配置生成服务器时，你的第一个任务是安装和配置 TFS 生成服务。 作为此过程的一部分，你将需要：

- 安装 TFS 生成服务和配置服务帐户。 任何生成任务，包括部署中，将使用的生成服务帐户的标识运行。
- 创建*生成控制器*和一个或多个*生成代理*。 每个生成控制器管理一套生成代理。 当您对生成进行排队时，生成控制器会将生成任务分配给可用的生成代理。 在 TFS 中的每个团队项目集合映射到单个生成控制器。
- 配置你的生成输出将放置文件夹。 这是网络共享。 任何生成输出，与 web 部署包类似，发送到投递文件夹。

[管理 Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) MSDN 上的第章包含所需执行这些任务的所有资源：

- 有关 Team Foundation Build 的概念性概述，包括生成服务、 生成控制器和生成代理，请参阅[了解 Team Foundation Build 系统](https://msdn.microsoft.com/library/dd793166.aspx)。
- 有关安装和配置生成服务的信息，请参阅[配置生成计算机](https://msdn.microsoft.com/library/ms181712.aspx)。
- 有关创建生成控制器的信息，请参阅[创建和使用生成的控制器工作](https://msdn.microsoft.com/library/ee330987.aspx)。
- 有关创建生成代理的信息，请参阅[创建和使用生成代理工作](https://msdn.microsoft.com/library/bb399135.aspx)。
- 有关创建和配置的放置文件夹的信息，请参阅[设置放置文件夹](https://msdn.microsoft.com/library/bb778394.aspx)。

## <a name="install-required-products-and-components"></a>安装必需的产品和组件

若要启用要生成解决方案的生成服务器，必须安装任何产品、 组件或您的解决方案需要的程序集。 在安装任何 web 平台组件之前，应在生成服务器上安装 Visual Studio 2010 （任何版本）。 这可确保可用于生成服务的核心 Microsoft Build Engine (MSBuild) 目标文件和 Web 发布管道 (WPP) 目标文件。 Visual Studio 安装程序还应安装 Web 部署，其中你将需要如果你打算部署 web 包作为生成过程的一部分。

安装常见 web 平台组件的最佳方法是使用[Web 平台安装程序](https://go.microsoft.com/?linkid=9805118)。 这可确保您正在安装每个产品的最新版本，并且它还会自动检测并安装所有必备组件的每个产品。 情况下[联系人管理器](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)解决方案，你应使用 Web 平台安装程序来安装这些产品和组件：

- **.NET framework 4.0**。 这是运行在此版本的.NET Framework 生成的应用程序所需要的。
- **Web 部署工具 2.1 或更高版本**。 这在你的服务器上安装 Web 部署 （和其基础可执行文件，MSDeploy.exe）。 作为此过程的一部分，它会安装并启动 Web 部署代理服务。 此服务可让你部署 web 包从远程计算机。
- **ASP.NET MVC 3**。 这将安装你需要运行 ASP.NET MVC 3 应用程序的程序集。

**若要安装必需的产品和组件**

1. 安装 Visual Studio 2010。 当系统提示选择要安装的功能，您应包括：

    1. 你需要编译任何编程语言。
    2. Visual Web Developer。 这可确保所 WPP 目标添加到你的生成服务器。

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Visual Studio 2010 的安装完成后，下载并安装[Visual Studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) （如果它尚未包括您的安装媒体中）。

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1 解决可能会阻止 MSBuild 查找可执行 MSDeploy bug。
3. 下载并启动[Web 平台安装程序](https://go.microsoft.com/?linkid=9805118)。
4. 在顶部**Web Platform Installer 3.0**窗口中，单击**产品**。
5. 在左侧的窗口中，在导航窗格中，单击**框架**。
6. 在**Microsoft.NET Framework 4**行，如果尚未安装.NET Framework，请单击**添加**。

    > [!NOTE]
    > 你可能已安装.NET Framework 4.0 通过 Windows 更新。 如果已安装的产品或组件，则 Web 平台安装程序将指示这一点通过将**添加**按钮，其文本**已安装**。

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. 在**ASP.NET MVC 3 (Visual Studio 2010)**行中，单击**添加**。
8. 在导航窗格中，单击**服务器**。
9. 在**Web 部署工具 2.1**行中，单击**添加**。
10. 单击“安装” 。 Web 平台安装程序将显示列表的产品和 #x 2014年; 以及要安装任何关联的依赖关系 （&） #x 2014; 并将提示你接受许可条款。
11. 查看许可条款，然后如果同意条款，单击**我接受**。
12. 安装完成后，单击**完成**，然后关闭**Web Platform Installer 3.0**窗口。

> [!NOTE]
> 如果你部署的过程包括使用 VSDBCMD.exe 或 SQLCMD.exe 等工具，你将需要确保这些安装在你的生成服务器上。 VSDBCMD.exe 是一个 Visual Studio 工具，通常会被添加到服务器时安装 Team Foundation Build。 SQLCMD.exe 是一个 SQL Server 工具。 你可以下载 SQLCMD.exe 从独立版本[Microsoft SQL Server 2008 R2 功能包](https://go.microsoft.com/?linkid=9805134)页。


## <a name="conclusion"></a>结束语

此时，你的生成服务器已准备好开始构建和部署你的 web 应用程序项目。 下一主题[创建生成定义，支持部署](creating-a-build-definition-that-supports-deployment.md)，描述如何创建和配置生成定义，以控制何时以及如何生成和部署你的项目。

## <a name="further-reading"></a>其他阅读材料

使用团队生成的更多常规指南，请参阅[管理 Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx)。

>[!div class="step-by-step"]
[上一页](adding-content-to-source-control.md)
[下一页](creating-a-build-definition-that-supports-deployment.md)
