---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: "用于 Web 部署配置服务器环境 |Microsoft 文档"
author: jrjlee
description: "本教程将演示如何设置支持一次单击，或自动、 网站部署和如何在各种不同 scen 中的发布的服务器环境..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 8a07d283e3e4344e5513152cf760ac90481d9f4b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="configuring-server-environments-for-web-deployment"></a>用于 Web 部署配置服务器环境
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本教程将演示如何设置支持一次单击，或自动、 网站部署和如何在各种不同方案中的发布的服务器环境。 本教程包括主题，以指导您完成各种任务，例如，配置 web 服务器以支持特定的方法来部署和设置 Web Farm Framework (WFF) 服务器场中，以及提供的基于方案的概述更高级别的端到端指南。
> 
> 本教程使用 Fabrikam，Inc.部署方案中所述[企业 Web 部署： 方案概述](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)作为示例和网络基础结构的引用点。
> 
> 这些教程的意大利语翻译，请访问[http://www.lucamorelli.it](http://www.lucamorelli.it)。


本教程包括以下主题：

- [选择 Web 部署到适当的方法](choosing-the-right-approach-to-web-deployment.md)
- [方案： 配置 Web 部署的测试环境](scenario-configuring-a-test-environment-for-web-deployment.md)
- [方案： 配置 Web 部署的过渡环境](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [方案： 配置 Web 部署的生产环境](scenario-configuring-a-production-environment-for-web-deployment.md)
- [配置 Web 服务器的 Web 部署发布 （远程代理）](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [配置 Web 服务器的 Web 部署发布 （Web 部署处理程序）](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [配置 Web 服务器的 Web 部署发布 （脱机部署）](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Web 部署发布更改为配置数据库服务器](configuring-a-database-server-for-web-deploy-publishing.md)
- [使用 Web 场框架创建服务器场](creating-a-server-farm-with-the-web-farm-framework.md)
- [配置目标环境的部署属性](configuring-deployment-properties-for-a-target-environment.md)

第一个主题，[选择用于 Web 部署的右方法](choosing-the-right-approach-to-web-deployment.md)，描述可用于发布 web 应用程序通过使用 Internet 信息服务 (IIS) Web 部署工具 （Web 部署） 的主要方式 2.0。 它还标识映射到每种方法的方案。 从此处，每个方案主题提供你需要完成的任务的高级概述，并确定你将需要通过工作，以便帮助你完成这些任务的主题。

如果你使用的拆分项目文件方法中所述[了解该生成过程](../web-deployment-in-the-enterprise/understanding-the-build-process.md)生成和部署你的解决方案的最后一个主题，[将部署属性配置为目标环境](configuring-deployment-properties-for-a-target-environment.md)，描述如何配置以部署到不同的目标环境的特定于环境的项目文件。

## <a name="key-technologies"></a>主要的技术

本教程重点介绍如何使用这些产品和技术支持 web 部署：

- IIS 7.5
- Web 部署 2.x
- WFF 2.x
- IIS Web 管理服务 (WMSvc)

本教程还涉及使用 Windows Server 2008 R2、 SQL Server 2008 R2、 ASP.NET 4.0 和 ASP.NET MVC 3。

## <a name="other-tutorials-in-this-series"></a>在本系列中其他教程

它在企业级 web 部署构成的五个教程系列中的一部分。 这些是序列中的其他教程：

- [部署在企业方案中的 Web 应用程序](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。 本介绍性内容的系列教程提供上下文的背景。 描述该教程的方案，以及它阐释了任务和在整个系列所述的演练如何适应更广泛的应用程序生命周期管理 (ALM) 过程。
- [Web 部署在企业中的](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。 本教程提供 Microsoft Build Engine (MSBuild) 项目文件的概念介绍、 Web 发布管道、 Web 部署和其他相关的技术。 它还说明了如何使用这些工具在一起来管理复杂部署进程。
- [用于 Web 部署配置 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。 本教程介绍如何配置 Team Foundation Server (TFS) 以支持各种部署方案，包括作为持续集成 (CI) 过程的一部分的自动的部署和手动触发的特定生成的部署。
- [高级企业级 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)。 本教程介绍如何完成各种更高级的部署任务，如自定义数据库部署为多个环境、 从部署中排除文件和文件夹以及在部署过程中将 web 应用程序脱机.

>[!div class="step-by-step"]
[下一篇](choosing-the-right-approach-to-web-deployment.md)
