---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: "配置的团队生成权限部署 |Microsoft 文档"
author: jrjlee
description: "本主题介绍如何配置权限以使您生成服务器可以自动 b 的一部分将内容部署到 web 服务器和数据库服务器..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: cb3d013d69e36f97335ea31dd6e4997772ba2d8e
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="configuring-permissions-for-team-build-deployment"></a>配置权限的团队的生成部署
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何配置权限以使您生成服务器可以将内容部署到 web 服务器和数据库服务器，作为自动的生成过程的一部分。


本主题窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器解决方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; 来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序Communication Foundation (WCF) 服务和数据库项目。

这些教程的核心的部署方法取决于中介绍的拆分项目文件方法[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在其中生成过程控制由两个项目文件 （&） #x 2014; 一个包含生成适用于每种目标环境和一个包含特定于环境的生成和部署设置的说明。 在生成期间，特定于环境的项目文件合并到环境无关的项目文件中以形成一组完整的生成说明。

## <a name="task-overview"></a>任务概述

当你安装 Team Foundation Server (TFS) 2010年生成服务时，你指定想要运行的服务的标识。 默认情况下，这是网络服务帐户。 或者，你可以配置生成服务使用域帐户运行。

需要 Windows 身份验证，以及你打算自动执行使用 Team Build，任何部署任务将使用的生成服务标识运行。 在这种情况下，你将需要授予你的 web 服务器和数据库服务器上任何所需的权限的生成服务标识。

> [!NOTE]
> 网络服务帐户使用的计算机帐户向其他计算机进行身份验证。 计算机帐户采用格式 * [域名]\[计算机名称] ***$**& #x 2014年; 例如， **FABRIKAM\TFSBUILD$**。 在这种情况下，如果你的生成服务运行时使用的网络服务标识，则应为你的生成服务器授予对计算机帐户标识任何所需的权限。


## <a name="configuring-web-server-permissions"></a>配置 Web 服务器权限

中所述[选择用于 Web 部署的右方法](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)，如果你想要将 web 包部署到远程 web 服务器可以使用两种主要方法：

- 通过以目标部署应用程序从远程位置*Web 部署代理服务*（也称为远程代理） 目标服务器上。
- 通过以目标部署应用程序从远程位置*Internet Information Services* (*IIS) Web 部署处理程序*目标服务器上。

远程代理在这种情况下具有两个主要限制：

- 远程代理支持仅 NTLM 身份验证。 换而言之，部署必须使用的生成服务标识 （&） #x 2014年; 不能模拟另一个帐户。
- 若要使用远程代理，请执行部署的帐户必须是目标服务器上的管理员。

总之，这些两个限制使远程代理方法不希望出现自动 Team Build 部署。 若要使用此方法，你需要使生成服务帐户在任何目标 web 服务器上的管理员。

与此相反，Web 部署处理程序方法提供了各种优势：

- Web 部署处理程序通过 HTTPS，允许你将备用帐户的凭据传递给 IIS Web 部署工具 （Web 部署） 支持基本身份验证。
- 你可以配置目标 web 服务器，以允许非管理员用户将内容部署到特定使用 Web 部署处理程序的 IIS 网站。

因此，很显然更可取的方法时自动执行 Team Build 中的 web 包部署的目标 Web 部署处理程序。 这是建议的过程：

1. 创建用于部署的低特权域帐户。
2. 配置 Web 部署处理程序和中所述向帐户授予所需的权限，将内容部署到特定的 IIS 网站，[配置用于 Web 部署发布 （Web 部署处理程序） 的 Web 服务器](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)。
3. 调用 Web 部署和面向 Web 部署处理程序，使用基本身份验证并提供域帐户的凭据创建，以执行部署。

在[联系人管理器](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)示例解决方案，你指定的身份验证类型 (基本或 NTLM)，Web 部署凭据和特定于环境的项目文件中的终结点地址 （远程代理或 Web 部署处理程序）。 这些值用于制定和项目文件执行时运行 Web 部署命令。 有关详细信息，请参阅[部署 Web 包](../web-deployment-in-the-enterprise/deploying-web-packages.md)。

有关详细信息配置 Web 部署处理程序，包括如何配置权限，请参阅[配置用于 Web 部署发布 （Web 部署处理程序） 的 Web 服务器](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)。 有关配置远程代理的详细信息，请参阅[配置用于 Web 部署发布 （远程代理） 的 Web 服务器](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)。

## <a name="configuring-database-server-permissions"></a>配置数据库服务器权限

若要将数据库部署到 SQL Server，你必须：

- SQL Server 实例上创建部署的帐户的登录名。
- 登录名授予**DBCreator** SQL Server 实例上的权限。
- 初始部署之后，将登录添加到**db\_所有者**目标数据库上的角色。 这是必需的因为在后续部署上您要修改现有数据库，而不创建新的数据库。

你可以进行身份验证到使用 NTLM 身份验证或 SQL Server 身份验证的 SQL Server 实例：

- 如果使用 NTLM 身份验证，你需要授予上面所述与生成服务帐户的权限。
- 如果你使用 SQL Server 身份验证，你需要授予上述到 SQL Server 帐户的权限。 你还需要在你用于部署数据库的连接字符串中包含的 SQL Server 用户名和密码。

有关如何配置数据库部署的权限的分步详细信息，请参阅[数据库服务器配置用于 Web 部署发布](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md)。

## <a name="conclusion"></a>结束语

此时，你应该了解所需的权限，以及打开你的身份验证选项时自动进行 Team Build 中的 web 应用程序和数据库部署。 你还应是能够实现对 IIS web 服务器和 SQL Server 数据库服务器的所需的权限。

## <a name="further-reading"></a>其他阅读材料

有关配置 Windows server 环境以支持远程部署的详细信息，请参阅[用于 Web 部署配置服务器环境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。

>[!div class="step-by-step"]
[上一篇](deploying-a-specific-build.md)
