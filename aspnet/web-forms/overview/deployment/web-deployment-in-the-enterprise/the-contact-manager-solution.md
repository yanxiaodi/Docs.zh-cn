---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: "联系人管理器解决方案 |Microsoft 文档"
author: jrjlee
description: "这一系列的教程使用的示例解决方案 （&) #x 2014年; 联系人管理器解决方案 & #x 2014; 来表示现实调配的企业级应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: b7f691a1ee855788f6a57616aea35d960e4c85c7
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="the-contact-manager-solution"></a>联系人管理器解决方案
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 这[系列教程](web-deployment-in-the-enterprise.md)使用的示例解决方案 （&) #x 2014年; 联系人管理器解决方案 & #x 2014; 来表示现实级别的复杂程度的企业级应用程序。 本主题介绍联系人管理器解决方案，描述该解决方案的关键组件，并标识部署这种企业环境中的各种目标平台的应用程序所面临的挑战。
> 
> 当在这些教程工作通过主题，可将联系人管理器解决方案用作演示如何可以满足企业部署方案的特定需求的参考实现。 下一主题[设置联系人管理器解决方案](setting-up-the-contact-manager-solution.md)，介绍如何下载并在你开发人员的工作站上运行解决方案。


## <a name="solution-overview"></a>解决方案概述

联系人管理器解决方案包括四个单独项目：

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**. 这是表示解决方案的入口点的 ASP.NET MVC 3 web 应用程序项目。 它提供了一些基本的 web 应用程序功能，如向用户提供的功能，可以创建和查看联系人详细信息。 应用程序依赖于 Windows Communication Foundation (WCF) 服务来管理联系人和管理身份验证和授权的 ASP.NET 应用程序服务数据库。
- **ContactManager.Database**. 这是 Visual Studio 数据库项目。 项目定义存储联系人详细信息的数据库的架构。
- **ContactManager.Service**. 这是一个 WCF web 服务项目。 WCF 服务公开终结点，使调用方执行创建、 检索、 更新和删除 (CRUD) 操作在**ContactManager**数据库。 该服务依赖于**ContactManager**数据库和**ContactManager.Common.dll**程序集。
- **ContactManager.Common**. 这是一个类库项目。 WCF 服务依赖于此程序集中定义的类型。

该解决方案还包括一个名为发布的解决方案文件夹。 这包括各种自定义项目文件和演示如何控制和操作的生成和部署过程的命令文件。 这些是在本教程后面更详细地介绍您的问题。

在概念级别，该解决方案的组件组合在一起，如下：

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> 而 ASP.NET MVC 3 web 应用程序使用 ASP.NET 成员资格提供程序，在 web 应用程序内的所有页面将都允许匿名访问。 这不明确的实际配置。 但是，解决方案是设置以这种方式，以使其更轻松地部署和测试解决方案，而不配置用户帐户和角色。


## <a name="deployment-challenges"></a>部署难题

联系人管理器解决方案阐释了多个部署难题所共有的大量企业部署方案：

- 此解决方案由多个依赖项目组成。 你需要同时部署这些项目。
- 连接字符串和服务终结点需要为每个环境中，更新并在许多情况下此信息将不会给开发人员可用。
- 当部署**ContactManager**数据库的过渡和生产环境，你需要保留在后续部署上的现有数据。
- 在部署 ASP.NET 应用程序服务数据库时，你需要部署某些配置数据，但省略任何用户帐户数据。
- 项目包含某些文件和不应部署的文件夹。 你需要在部署过程中排除这些文件和文件夹。
- 需要支持从 Team Foundation Server (TFS) 生成服务器的自动的部署的解决方案。

## <a name="conclusion"></a>结束语

本主题提供的联系人管理器解决方案的高级概述，并标识一些固有的部署问题所共有的大量企业部署方案。 在本教程的其余主题介绍一些可用于解决这些难题的技术。

下一主题[设置联系人管理器解决方案](setting-up-the-contact-manager-solution.md)，介绍如何下载并在你开发人员的工作站上运行解决方案。

>[!div class="step-by-step"]
[上一页](web-deployment-in-the-enterprise.md)
[下一页](setting-up-the-contact-manager-solution.md)
