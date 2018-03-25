---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: "方案： 配置 Web 部署的过渡环境 |Microsoft 文档"
author: jrjlee
description: "本主题介绍典型 web 部署方案中的过渡环境，并说明需要为了设置类似 env 完成的任务..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 683a0cf88225fee762e82925afe3785a2defd5bf
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>方案： 配置 Web 部署的过渡环境
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍典型 web 部署方案中的过渡环境，并说明需要为了设置相似的环境中完成的任务。


很多组织使用过渡环境来预览更新对 web 应用程序或网站。 这样，组织内的人员有机会浏览并查看新功能或内容之前的站点"实时，在显示"或换而言之部署到生产环境。 过渡环境旨在尽可能真实地复制生产环境，以便提供真实的预览。 这种过渡环境通常具有以下特征：

- 环境包含多个负载平衡的 web 服务器和一个或多个数据库服务器，通常与故障转移群集和数据库镜像。
- 由开发团队或自动由 Team Build 服务器可能手动部署应用程序。
- 用户或部署应用程序的进程帐户不太可能可以临时服务器上具有管理员权限。
- 更改应用程序将部署频繁，因此环境需要支持单步执行或自动部署。

> [!NOTE]
> 扩展跨多个服务器的数据库部署不在本教程的范围。 有关此区域的详细信息，请参阅[SQL Server 联机丛书](https://technet.microsoft.com/library/ms130214.aspx)。


例如，在我们[教程的情况下](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)，Team Foundation Server (TFS) 管理联系人管理器解决方案。 TFS 管理员，Rob Walters 已创建，触发部署到过渡环境所需的开发人员可以生成定义。

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

请注意，在大多数情况下，你将不一定需要将最新版本部署到过渡环境。 相反，你很多更有可能想要部署已进行了验证和验证在测试环境中的特定版本。

## <a name="solution-overview"></a>解决方案概述

在此方案中，你可以推断出的部署要求分析从这些事实：

- 执行部署的用户或进程帐户不会在暂存服务器上拥有管理员权限，以便过渡的 web 服务器必须支持非管理员部署。 在这种情况下，你将需要配置过渡的 web 服务器，使用 Web 部署处理程序，而不是远程代理。
- 过渡环境包括多个 web 服务器，但它需要支持一次单击或自动部署，因此将需要使用 Web Farm Framework (WFF) 来创建服务器场。 使用此方法，你可以部署到一个 web 服务器 （主服务器），应用程序和 WFF 将复制在过渡环境中的所有 web 服务器上的部署。
- 用户或执行部署的进程帐户必须有权创建数据库。 在这种情况下，你将需要将帐户添加到**dbcreator**在数据库服务器上，除了配置数据库服务器来支持远程访问和部署的服务器角色。

以下主题提供完成这些任务所需的所有信息：

- [使用 Web 场框架创建服务器场](creating-a-server-farm-with-the-web-farm-framework.md)。 本主题介绍如何创建和配置服务器场使用 WFF，以便跨多个负载平衡的 web 服务器复制的 web 平台产品和组件、 配置设置和网站和应用程序。
- [Web 部署发布更改配置 Web 服务器 （Web 部署处理程序）](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)。 本主题介绍如何生成支持 Web 部署发布，使用远程代理方法中，从干净的 Windows Server 2008 R2 生成的 web 服务器。
- [配置数据库服务器的 Web 部署发布更改](configuring-a-database-server-for-web-deploy-publishing.md)。 本主题介绍如何配置数据库服务器以支持远程访问和部署，从 SQL Server 2008 R2 的默认安装。

## <a name="further-reading"></a>其他阅读材料

有关配置的典型的开发人员测试环境的指南，请参阅[方案： 为 Web 部署配置测试环境](scenario-configuring-a-test-environment-for-web-deployment.md)。 有关配置的典型生产环境的指南，请参阅[方案： 配置 Web 部署的生产环境](scenario-configuring-a-production-environment-for-web-deployment.md)。

>[!div class="step-by-step"]
[上一页](scenario-configuring-a-test-environment-for-web-deployment.md)
[下一页](scenario-configuring-a-production-environment-for-web-deployment.md)
