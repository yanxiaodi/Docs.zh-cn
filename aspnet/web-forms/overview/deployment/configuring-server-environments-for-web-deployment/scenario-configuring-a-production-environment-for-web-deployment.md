---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: "方案： 配置 Web 部署的生产环境 |Microsoft 文档"
author: jrjlee
description: "本主题介绍典型 web 部署方案中的为生产环境，并说明需要完成为了设置类似的任务..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: cdd13f96ddf08ff86b01ef9de17ea82cf038ab28
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>方案： 配置 Web 部署的生产环境
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍典型 web 部署方案中的为生产环境，并说明需要为了设置相似的环境中完成的任务。


生产环境是为 web 应用程序或网站的最终目标。 此时，你的应用程序将已通过测试、 已部署到过渡环境，和已准备好"进入正常运行状态。" 生产环境中的特征可能会因广泛的性质和你的 web 内容的目的，你的组织，目标受众和许多其他因素的大小。 在企业级方案中，生产环境可能具有以下特征：

- 环境包含多个负载平衡的 web 服务器和一个或多个数据库服务器，通常与故障转移群集和数据库镜像。
- 如果环境处于面向 Internet 的则很可能以从内部网络隔离。 也可能在外围网络中的不同子网，它可能位于不同的域，和它可能完全不同的网络基础结构。
- 开发人员和生成服务器的进程帐户是极有可能不在生产服务器上具有管理员特权。
- 更改应用程序部署不太频繁地比测试或过渡部署。

> [!NOTE]
> 扩展跨多个服务器的数据库部署不在本教程的范围。 有关此区域的详细信息，请参阅[SQL Server 联机丛书](https://technet.microsoft.com/library/ms130214.aspx)。


例如，在我们[教程的情况下](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)，Team Build 服务器包括让用户生成联系人管理器解决方案并将其部署到过渡环境中单个步骤的生成定义。 应用程序准备好可部署到生产环境中，由于安全要求和网络基础结构，施加的约束时生产环境管理员必须手动复制到生产 web 服务器上的 web 包并导入它通过 Internet 信息服务 (IIS) 管理器。

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>解决方案概述

在此方案中，你可以推断出的部署要求分析从这些事实：

- 由于安全限制和网络配置，你无法配置要支持一次单击或自动部署的生产环境。 脱机部署是在此方案中的唯一可行方法。
- 生产环境包含多个 web 服务器，因此你可以使用 Web Farm Framework (WFF) 来创建服务器场。 使用此方法，管理员只需导入到一个 web 服务器 （主服务器），应用程序和 WFF 将复制生产环境中的所有 web 服务器上的部署。

以下主题提供完成这些任务所需的所有信息：

- [使用 Web 场框架创建服务器场](configuring-a-database-server-for-web-deploy-publishing.md)。 本主题介绍如何创建和配置服务器场使用 WFF，以便跨多个负载平衡的 web 服务器复制的 web 平台产品和组件、 配置设置和网站和应用程序。
- [Web 部署发布 （脱机部署） 配置 Web 服务器](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)。 本主题介绍如何生成该允许管理员导入和部署 web 包，手动从干净的 Windows Server 2008 R2 生成的 web 服务器。
- [配置数据库服务器的 Web 部署发布更改](configuring-a-database-server-for-web-deploy-publishing.md)。 本主题介绍如何配置数据库服务器以支持远程访问和部署，从 SQL Server 2008 R2 的默认安装。

## <a name="further-reading"></a>其他阅读材料

有关配置的典型的开发人员测试环境的指南，请参阅[方案： 为 Web 部署配置测试环境](scenario-configuring-a-test-environment-for-web-deployment.md)。 有关配置的典型的过渡环境的指南，请参阅[方案： 配置用于 Web 部署的暂存环境](scenario-configuring-a-staging-environment-for-web-deployment.md)。

>[!div class="step-by-step"]
[上一页](scenario-configuring-a-staging-environment-for-web-deployment.md)
[下一页](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
