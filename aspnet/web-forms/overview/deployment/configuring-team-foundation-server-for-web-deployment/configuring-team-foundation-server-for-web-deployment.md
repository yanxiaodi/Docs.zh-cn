---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: "用于 Web 部署配置 Team Foundation Server |Microsoft 文档"
author: jrjlee
description: "本教程将演示如何配置 Team Foundation Server (TFS) 2010年若要生成解决方案，并将 web 内容部署到各种目标环境。 这..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 72f60841a1381380c0ea6167077420f960180dc7
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="configuring-team-foundation-server-for-web-deployment"></a>用于 Web 部署配置 Team Foundation Server
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本教程将演示如何配置 Team Foundation Server (TFS) 2010年若要生成解决方案，并将 web 内容部署到各种目标环境。 这包括持续集成 (CI) 方案中，在其中部署内容自动每次一名开发人员进行了更改。 它还包括手动触发器方案中，管理员可能想要验证并在测试环境中验证生成之后触发部署到过渡环境特定生成的位置。 在本教程中的主题将指导你完成整个配置过程中，包括：
> 
> - 如何在 TFS 中创建新的团队项目。
> - 如何将内容添加到源代码管理。
> - 如何配置生成服务器以支持 CI 和部署。
> - 如何创建生成定义，其中包含部署逻辑。
> - 如何配置用于自动部署的权限。
> 
> 这些教程的意大利语翻译，请访问[ http://www.lucamorelli.it ](http://www.lucamorelli.it)。


本教程假定你已安装 TFS 2010 并创建团队项目集合作为初始配置过程的一部分。 [For Visual Studio 2010 Team Foundation 安装指南](https://go.microsoft.com/?linkid=9805132)提供全面指导，这些任务。

## <a name="context"></a>上下文

此窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序解决方案 （&) #x 2014;Communication Foundation (WCF) 服务和数据库项目。

这些教程的核心的部署方法取决于中介绍的拆分项目文件方法[了解该生成过程](../web-deployment-in-the-enterprise/understanding-the-build-process.md)，在其中生成过程控制由两个项目文件 （&） #x 2014; 一个包含生成适用于每种目标环境和一个包含特定于环境的生成和部署设置的说明。 在生成期间，特定于环境的项目文件合并到环境无关的项目文件中以形成一组完整的生成说明。

## <a name="scenario-overview"></a>方案概述

这些教程的高级方案所述[企业 Web 部署： 方案概述](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)。 我们建议您开始本教程之前查看此主题。

## <a name="how-to-use-this-tutorial"></a>如何使用本教程

如果这是首次执行了本教程中所述的任务，或如果你想要遵照使用的示例解决方案的示例，你应解决顺序中的教程主题。 或者，可以使用单个主题作为指导特定任务。 本教程包括以下主题：

- [在 TFS 中创建团队项目](creating-a-team-project-in-tfs.md)。 团队项目是源代码管理、 进程管理，以及在 TFS 中的生成的核心单元。 你需要创建团队项目，然后才能添加要源代码管理或创建生成定义的内容。
- [将内容添加到源代码管理](adding-content-to-source-control.md)。 一旦你已创建团队项目，即可将内容添加到源代码管理。 你将需要之前要添加的项目和解决方案，以及任何外部依赖关系，你可以开始配置生成。
- [用于 Web 部署配置 TFS 生成服务器](configuring-a-tfs-build-server-for-web-deployment.md)。 如果你想要生成你的团队项目内容，你将需要配置生成服务器。 在大多数情况下，这应该是从你的 TFS 安装一台计算机上。 若要配置生成服务器，你需要安装和配置 TFS 生成服务，安装 Visual Studio 2010、 创建生成控制器和生成代理，安装任何产品或组件，你的代码需要才能成功，生成和安装Internet Information Services (IIS) Web 部署工具 （Web 部署）。
- [创建生成定义支持部署](creating-a-build-definition-that-supports-deployment.md)。 在可以开始队列或触发在 TFS 中的生成之前，你需要为你的团队项目创建至少一个生成定义。 生成定义中定义生成，包括应在生成中包括哪些内容、 应触发生成，以及其中 Team Build 应发送的生成输出每个的方面。 你可以配置生成定义以运行自定义 Microsoft Build Engine (MSBuild) 项目文件，这样就可以自动生成中包括部署逻辑。
- [将特定的生成部署](deploying-a-specific-build.md)。 在许多方案，你将想要将特定生成，而不是最新的生成、 部署到目标环境。 在这种情况下，你可以配置将特定的放置文件夹中的内容部署的生成定义。
- [配置的团队生成权限部署](configuring-permissions-for-team-build-deployment.md)。 如果生成服务是将内容部署自动化的生成过程的一部分，你需要授予给任何目标 web 服务器和数据库服务器上的生成服务帐户不同的权限。

## <a name="key-technologies"></a>主要的技术

本教程重点介绍如何使用这些产品和技术以支持自动的生成和 web 部署：

- Visual Studio Team Foundation Server 2010
- Team Build 和 MSBuild
- Web Deploy

本教程还涉及使用 Windows Server 2008 R2、 IIS 7.5、 SQL Server 2008 R2、 ASP.NET 4.0 和 ASP.NET MVC 3。

## <a name="other-tutorials-in-this-series"></a>在本系列中其他教程

它在企业级 web 部署构成的五个教程系列中的一部分。 这些是序列中的其他教程：

- [部署在企业方案中的 Web 应用程序](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。 本介绍性内容的系列教程提供上下文的背景。 描述该教程的方案，以及它阐释了任务和在整个系列所述的演练如何适应更广泛的应用程序生命周期管理 (ALM) 过程。
- [Web 部署在企业中的](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。 本教程提供 MSBuild 项目文件的概念介绍、 Web 发布管道 (WPP)、 Web 部署和其他相关的技术。 它还说明了如何使用这些工具在一起来管理复杂部署进程。
- [用于 Web 部署配置服务器环境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。 本教程介绍如何配置 Windows server 以支持各种部署方案，包括使用 Web 部署代理服务 （远程代理） 或 Web 部署处理程序和远程数据库部署的远程 web 包部署。 它提供有关选择你自己的环境的适当的部署方法的指导，并描述了如何使用 Web Farm Framework (WFF) 在服务器场中的所有 web 服务器上复制已部署的 web 应用程序。
- [高级企业级 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)。 本教程介绍如何完成各种更高级的部署任务，如自定义数据库部署为多个环境、 从部署中排除文件和文件夹以及在部署过程中将 web 应用程序脱机.

>[!div class="step-by-step"]
[下一篇](creating-a-team-project-in-tfs.md)
