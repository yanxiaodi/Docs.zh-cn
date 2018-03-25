---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: "方案： 配置 Web 部署的测试环境 |Microsoft 文档"
author: jrjlee
description: "本主题介绍典型 web 部署方案中的为开发人员或测试环境并说明需要为了设置 si 完成的任务..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 23e317c6e0b6daf2d7937b73738e5cb6fa32cde2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>方案： 配置 Web 部署的测试环境
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍典型 web 部署方案中的为开发人员或测试环境并说明需要为了设置相似的环境中完成的任务。


当开发人员 web 应用程序时，它们通常将向它们可用于在实际设置中测试更改向其应用程序的服务器环境中获得访问。 这种类型的开发或测试环境通常具有以下特征：

- 环境由单个 web 服务器和单个数据库服务器组成。
- 开发人员通常在服务器上，以允许这些配置环境到其应用程序的要求有管理员权限。
- 更改应用程序将部署频繁，因此环境需要支持单步执行或自动部署。

例如，在我们[教程的情况下](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)，Matt 婷是开发人员在 Fabrikam，inc.Matt 从事联系人管理器解决方案，并定期需要将更改部署到测试环境。 Matt 是测试 web 服务器和测试数据库服务器上的管理员。 最初，Matt 需要能够直接将解决方案部署到测试环境。

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

与工作的进展和多个开发人员加入团队解决方案配置为连续集成 (CI) 在 Team Foundation Server (TFS) 联系人管理器。 只要开发人员将签入内容，团队生成应生成解决方案，运行任何单元测试，并自动将解决方案部署到测试环境。

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>解决方案概述

测试环境需要支持单步执行或自动部署从远程计算机，因此你可以选择的两种主要方法。 你可以：

- 配置测试 web 服务器以支持使用 Web 部署代理服务 （"远程代理"） 的部署。
- 配置测试 web 服务器以支持使用 Web 部署的处理程序的部署。

> [!NOTE]
> 你也可以使用[按需部署的 Web](https://technet.microsoft.com/library/ee517345(WS.10).aspx) （"临时代理"）。 这是类似于在要求和约束方面的远程代理方法。


在这种情况下，开发人员在目标服务器上，具有管理员权限，并且测试环境不是受严格的安全约束，因此合理的选择是要配置测试 web 服务器以支持使用远程代理的部署。 这是不太复杂，要求不太初始配置与 Web 部署处理程序方法。 你还需要配置你的数据库服务器来支持远程访问和部署。

以下主题提供完成这些任务所需的所有信息：

- [配置 Web 服务器的 Web 部署发布 （远程代理）](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)。 本主题介绍如何生成支持 Web 部署发布，使用远程代理方法中，从干净的 Windows Server 2008 R2 生成的 web 服务器。
- [配置数据库服务器的 Web 部署发布更改](configuring-a-database-server-for-web-deploy-publishing.md)。 本主题介绍如何配置数据库服务器以支持远程访问和部署，从 SQL Server 2008 R2 的默认安装。

## <a name="further-reading"></a>其他阅读材料

有关配置的典型的过渡环境的指南，请参阅[方案： 配置用于 Web 部署的暂存环境](scenario-configuring-a-staging-environment-for-web-deployment.md)。 有关配置的典型生产环境的指南，请参阅[方案： 配置 Web 部署的生产环境](scenario-configuring-a-production-environment-for-web-deployment.md)。

>[!div class="step-by-step"]
[上一页](choosing-the-right-approach-to-web-deployment.md)
[下一页](scenario-configuring-a-staging-environment-for-web-deployment.md)
