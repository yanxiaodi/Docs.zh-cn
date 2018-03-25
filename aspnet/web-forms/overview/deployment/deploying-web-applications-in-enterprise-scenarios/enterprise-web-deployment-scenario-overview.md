---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: "企业 Web 部署： 方案概述 |Microsoft 文档"
author: jrjlee
description: "此组的教程使用的复杂性，一家虚构企业部署方案，以及现实级别的示例解决方案提供 ref..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: f90db22bf98456661c530e728e854ce109aec6fd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="enterprise-web-deployment-scenario-overview"></a>企业 Web 部署： 方案概述
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 此组的教程使用的示例解决方案与现实级别的复杂性，一家虚构企业部署方案，以及提供的参考实现并为提供的任务和演练常见上下文。 本主题介绍教程的情况下，并介绍了示例解决方案。


## <a name="scenario-description"></a>方案说明

Fabrikam，Inc.，虚构的公司中创建一个解决方案，让远程销售团队存储和检索联系信息从一个 web 界面。

在 Fabrikam，Inc.的应用程序生命周期管理 (ALM) 进程要求解决方案在软件开发过程的各个阶段中要部署到服务器的三个环境：

- 开发人员测试或"沙盒"环境。
- 基于 intranet 的过渡环境中。
- 面向 Internet 的生产环境。

每个这些环境中有不同的配置和安全要求，并每又带来了唯一的部署挑战。

### <a name="the-fabrikam-inc-server-infrastructure"></a>Fabrikam，inc.服务器基础结构

这是高级开发和部署基础结构在 Fabrikam，inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

开发人员工作站、 的源控件基础结构、 开发人员测试环境中，和所有驻留在 Fabrikam.net 域内的 intranet 网络的过渡环境。 生产环境驻留在外围网络 （也称为 DMZ、 外围安全区域和外围子网），这是通过防火墙从 intranet 网络隔离。 这是常见的部署方案： 你通常从内部服务器基础结构通过防火墙或网关服务器使用隔离面向 Internet 的 web 服务器。

在此示例中：

- 具有单独的生成服务器的 Team Foundation Server (TFS) 2010年服务器提供源控件和持续集成 (CI) 功能。
- 开发人员测试环境包括 Internet 信息服务 (IIS) 7.5 web 服务器和 SQL Server 2008 R2 数据库服务器。
- 生产环境包括通过 Web Farm Framework (WFF) 控制器服务器，以及 SQL Server 2008 R2 数据库服务器同步的多个 IIS 7.5 web 服务器。 在实践中，数据库服务器可能使用群集或镜像以提高可伸缩性和可用性。
- 过渡环境旨在尽可能地复制生产环境的配置。
- 防火墙和网络隔离策略不允许直接、 自动化部署从 intranet 向外围网络。

每个这些环境中介绍了更多详细信息在第二个教程中，[用于 Web 部署配置服务器环境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。

### <a name="team-roles-for-alm"></a>ALM 的团队角色

这些用户中创建、 管理、 生成和发布联系人管理器解决方案包括：

- Matt 婷是 web 应用程序开发人员在 Fabrikam，inc.他是团队的开发使用 Visual Studio 2010 的联系人管理器解决方案的一部分。 Matt 在开发人员测试环境中，允许他配置环境以满足其需求的服务器上具有完全管理员权限。 他还具有到他可以将存储为联系人管理器解决方案的源代码的 Visual Studio 2010 TFS 实例的用户访问权限。
- Rob Walters 是 Fabrikam，Inc.开发团队的服务器管理员。 Rob TFS 服务器上具有管理访问权限，以便他可以配置 TFS 和团队项目生成的所有方面。 Rob 还具有管理访问权限的测试和过渡 web 服务器，并充当用于测试和过渡环境中的数据库服务器的数据库管理员 (DBA)。 Rob 具有 Team Build 服务器上配置 TFS 执行这些任务：

    - 生成并运行单元测试应用程序，每当用户签入到 TFS 的文件。 这称为 CI。
    - 自动部署到测试环境 Contact Manager 应用程序，一旦应用程序通过单元测试。 这包括在初始部署之后发布到对初始部署和任何更新到数据库的测试服务器的数据库。
    - 联系人管理器将应用部署到单步执行过程中的过渡环境。
    - 创建 Web 包，Web 服务器管理员和 DBA 可将发布到生产环境的应用程序。
- Lisa 安德鲁斯是服务器管理员，负责应用程序部署到 Fabrikam，Inc.生产服务器。 她具有读取访问权限后它生成联系人管理器应用程序，其中就 TFS Team Build 存储的 web 部署包的共享。 她还了流向生产 web 服务器的管理访问权限，因此，她可以部署到生产应用程序。 此外，她充当 DBA 将数据库和数据库更新部署到生产环境中的数据库服务器。

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>联系人管理器解决方案

联系人管理器解决方案设计，以便已注册、 登录的用户添加和编辑通过 web 界面的联系信息。 联系人管理器解决方案包括四个单独项目：

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager.Mvc**。 这是表示解决方案的入口点的 ASP.NET MVC3 web 应用程序项目。 它提供了一些基本的 web 应用程序功能，如向用户提供的功能，可以创建和查看联系人详细信息。 应用程序依赖于 Windows Communication Foundation (WCF) 服务来管理联系人和管理身份验证和授权的 ASP.NET 应用程序服务数据库。
- **ContactManager.Database**。 这是 Visual Studio 2010 数据库项目。 项目定义存储联系人详细信息的数据库的架构。
- **ContactManager.Service**。 这是一个 WCF web 服务项目。 WCF 公开终结点，使调用方执行创建、 检索、 更新和删除 (CRUD) 操作联系人管理器数据库。 该服务依赖于联系人管理器数据库和 ContactManager.Common.dll 程序集。
- **ContactManager.Common**。 这是一个类库项目。 WCF 服务依赖于此程序集中定义的类型。

在本系列第一个教程中提供解决方案和其部署要求的全面审查[企业中的 Web 部署](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>部署任务

有几个不同的任务所涉及的应用程序部署到不同的环境中的大型组织。 这些是教程涵盖的关键任务：

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

下面是在部署过程中从本文档前面所述的用户的角度来看每个步骤的列表：

1. 团队的所有成员都查看 Visual Studio 2010，以确定部署的关键要求和问题中的联系人管理器解决方案。
2. Matt 婷可能部署该开发人员测试环境，以执行初始测试的部署逻辑直接从开发人员工作站的联系人管理器解决方案。
3. Matt 婷在 TFS 中添加到源代码管理应用程序。
4. 在 Team Build 中，Rob Walters 创建联系人管理器解决方案的各种生成定义。 一个生成定义使用 CI 将解决方案部署到开发人员测试环境中，只要用户将签入新代码。 另一个生成定义可让用户触发器部署到过渡环境根据需要。
5. 每次用户签入新代码，Team Build 自动生成的解决方案组件，运行单元测试，并将解决方案部署到开发人员测试环境中，如果生成成功并且已通过单元测试。
6. 当用户触发部署到过渡环境时，解决方案是打包和部署在单步执行过程。 此过程还会生成手动部署到生产环境的包。
7. Lisa 安德鲁斯通过手动导入步骤 6 中创建的 web 包部署到生产环境的应用程序。

### <a name="key-deployment-issues"></a>部署的关键问题

联系人管理器解决方案和 Fabrikam，Inc.方案突出显示各种常见的问题和在部署复杂，企业级解决方案时可能遇到的难题。 例如: 

- 你需要能够将项目部署到多个环境，如开发人员或测试环境中，暂存平台和生产服务器。 需要使用不同的配置设置为每个环境中部署的解决方案。
- 需要单步执行或自动生成和部署过程的一部分同时部署多个依赖项目。
- 你需要能够到驱动器部署从一个自动化过程。 例如，你想要使用 CI 过程时在签入新代码部署到过渡环境的 web 应用程序。
- 你需要能够控制部署过程并从 Visual Studio 外部，设置部署变量如开发人员一般不具备正确的配置设置或为每个目标环境所需的凭据。
- 你需要部署基于架构的数据库项目并保留在后续部署上的现有数据。
- 你需要部署上临时成员资格数据库而不部署用户帐户数据。 你可能还需要更新已部署的成员资格数据库的架构，而不会丢失现有用户帐户数据。
- 你需要在将内容部署到各种目标环境时排除特定文件或文件夹。

此外，管理部署频繁和增量更新时引发了一些额外挑战。 例如: 

- 每次开发人员签入新代码时，你可以运行单元测试。 你只想要部署解决方案，在代码传递单元测试。
- 当你部署到过渡或生产环境的 web 应用程序时，你想要重定向到用户*应用\_offline.htm*部署过程的持续时间的文件。
- 你想要记录部署活动。 在部署过程应将成功或失败的部署的电子邮件通知发送给指定收件人。
- 自动的部署失败时，部署过程应重试当前部署，或改为将以前的 web 包的部署。

>[!div class="step-by-step"]
[上一页](deploying-web-applications-in-enterprise-scenarios.md)
[下一页](application-lifecycle-management-from-development-to-production.md)
