---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: "部署 Web 应用程序在企业方案中使用的 Visual Studio 2010 |Microsoft 文档"
author: jrjlee
description: "此系列教程描述工具和技术可用于部署各种企业方案中的 web 应用程序。 它还说明了如何充分利用..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 99bab371dd34b30f3554843e49bbec7f57c3f96c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>部署 Web 应用程序中使用的 Visual Studio 2010 的企业方案
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 此系列教程描述工具和技术可用于部署各种企业方案中的 web 应用程序。 它还说明了如何最好地使用 Visual Studio 2010、 Microsoft Build Engine (MSBuild)、 Internet 信息服务 (IIS) 7.5、 IIS Web 部署工具 （Web 部署）、 Web Farm Framework (WFF) 和 VSDBCMD.exe 到类似的实用工具等技术简化和管理在部署过程。 它包括概念概述和将帮助你的面向任务的指南：
> 
> - 查看并建立一个企业级 web 应用程序的部署要求。
> - 配置以支持 web 部署的测试、 过渡和生产 web 服务器环境。
> - 配置 Team Foundation Server (TFS) 持续集成 (CI) 进程，以支持自动的 web 部署。
> - 部署到具有不同的要求和限制的其他服务器环境的企业级 web 应用程序。
> - 将更改部署到 web 应用程序在不同的服务器环境中运行。
> 
> > [!NOTE]
> > 虽然这些教程介绍 TFS 用作 CI 服务器，则本指南可以轻松适应 CI 的任何服务器。 不需要了解和利用教程的 TFS 的详细的知识。
> 
> 
> 这些教程的意大利语翻译，请访问[http://www.lucamorelli.it](http://www.lucamorelli.it)。


## <a name="about-the-authors"></a>关于作者

Jason Lee 是与主体技术[内容 Master](http://www.contentmaster.com/)其中他一直与 Microsoft 产品和技术，尤其是 SharePoint 和 ASP.NET，几年。 Jason 拥有在计算博士并且当前 MCPD 和 MCTS 认证。 你可以阅读在 Jason 的技术博客[www.jrjlee.com](http://www.jrjlee.com/)。

Benjamin.柯里是与主体技术[内容 Master](http://www.contentmaster.com/)人员已任职期间他写白皮书、 SDK 文档、 PowerPoint 演示文稿和讲师和 online 培训课程。 ASP.NET 文档团队的原始成员，他已经与 Microsoft 的 web 技术为过去的十年。

## <a name="target-audience"></a>目标受众

此系列教程适用于 ASP.NET web 应用程序开发人员和解决方案架构师使用 Visual Studio 2010 创建企业级 web 应用程序。 若要从内容中获取最大价值，你应具有丰富的使用的 Visual Studio 2010，和 TFS，以及 Microsoft web 平台技术，如 ASP.NET MVC 3、 Windows Communication Foundation (WCF)、 IIS、 SQL 感知基本熟悉服务器和 Visual Studio 数据库项目。 但是，你不需要熟悉部署工具和技术或需要知道如何设置 CI 系统。

## <a name="requirements"></a>要求

若要遵循演练和执行这些教程描述的任务，你需要在开发计算机上安装此软件：

- Visual Studio 2010 Premium 或 Ultimate Edition 不带 Service Pack 1
- .NET framework 4.0
- .NET framework 3.5 Service Pack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express
- SQL Server Express 2008 R2

若要执行在所有这些演练所述的部署步骤，你将需要有权访问示例 Web 应用程序部署环境。 为获得最佳结果，这些环境应反映你的组织的企业部署模式。 然后，你可以修改以反映的部署环境和你自己组织的要求本文档中提供的演练。

## <a name="series-contents"></a>系列内容

本介绍性部分包含两个进一步主题。 这些服务设计为按照教程提供一些更广泛的上下文：

- [企业 Web 部署： 方案概述](enterprise-web-deployment-scenario-overview.md)。 本主题介绍支持每个本系列教程的方案。 此方案侧重于名为 Fabrikam，Inc.，因为它开发一个企业级 web 应用程序的虚构公司的应用程序生命周期管理 (ALM) 要求。
- [应用程序生命周期管理： 从开发到生产环境](application-lifecycle-management-from-development-to-production.md)。 本主题提供部署过程的高级别，端到端概述。 它阐释了 Fabrikam，Inc.如何移动企业级 ASP.NET web 应用程序通过测试、 过渡和生产环境持续开发过程的一部分。

该系列包括四个教程集。 每个主要关注的 web 部署的不同方面：

- [Web 部署在企业中的](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。 本教程提供 MSBuild 项目文件的概念介绍、 Web 发布管道、 Web 部署和其他相关的技术。 它还说明了如何使用这些工具在一起来管理复杂部署进程。
- [用于 Web 部署配置服务器环境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。 本教程介绍如何配置 Windows server 以支持各种部署方案，包括使用 Web 部署代理服务 （"远程代理"） 或 Web 部署处理程序和远程数据库部署的远程 web 包部署。 它提供有关选择你自己的环境的适当的部署方法的指导，并描述了如何使用 WFF 跨服务器场中的所有 web 服务器复制已部署的 web 应用程序。
- [用于 Web 部署配置 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。 本教程介绍如何配置 TFS 以支持各种部署方案，包括 CI 过程的一部分的自动的部署和手动触发的特定生成的部署。
- [高级企业级 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)。 本教程介绍如何完成各种更高级的部署任务，如自定义数据库部署为多个环境、 从部署中排除文件和文件夹以及在部署过程中将 web 应用程序脱机.

## <a name="where-to-start"></a>从何处着手

此组的教程使用的示例解决方案与现实级别的复杂性，一家虚构企业部署方案，以及提供的参考实现并为提供的任务和演练常见上下文。 下一主题[企业 Web 部署： 方案概述](enterprise-web-deployment-scenario-overview.md)，引入了该方案和示例解决方案。 在这里您可以处理教程和主题的最接近你的需求。

>[!div class="step-by-step"]
[下一篇](enterprise-web-deployment-scenario-overview.md)
