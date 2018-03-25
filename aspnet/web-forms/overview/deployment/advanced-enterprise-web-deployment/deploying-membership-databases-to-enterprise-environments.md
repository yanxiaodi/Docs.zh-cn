---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: "将成员资格数据库部署到企业环境 |Microsoft 文档"
author: jrjlee
description: "本主题介绍的关键注意事项以及你将需要设置 ASP.NET 应用程序服务数据库 （多个通用...时需要解决的挑战"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 27fade9fc5cae917579d4963da7bca12f6a5cda1
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>将成员资格数据库部署到企业环境
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍的关键注意事项和你将需要设置 ASP.NET 应用程序服务在测试、 临时或生产环境中 （通常称为成员资格数据库） 的数据库时需要解决的挑战。 它还介绍了可用来应对这些挑战的方法。


本主题窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器解决方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; 来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序Communication Foundation (WCF) 服务和数据库项目。

这些教程的核心的部署方法取决于中介绍的拆分项目文件方法[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在其中生成过程控制由两个项目文件 （&） #x 2014; 一个包含生成适用于每种目标环境和一个包含特定于环境的生成和部署设置的说明。 在生成期间，特定于环境的项目文件合并到环境无关的项目文件中以形成一组完整的生成说明。

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>部署成员资格数据库时，问题是什么？

在大多数情况下，制定部署策略对于数据库，你需要考虑的第一个操作是你想要部署哪些的数据。 在开发或测试环境中，你可能想要将用户帐户数据，以便于快速且轻松地测试部署。 在过渡或生产环境中，是不太可能想要将用户帐户数据部署。

遗憾的是，ASP.NET 成员资格数据库引入作出此决定大量更复杂一些特定难题：

- 仅限架构的部署将使处于不可操作状态的状态的成员资格数据库。 这是因为成员资格数据库包含某些配置数据 (在**aspnet\_SchemaVersions**表)，在数据库要求才能正常。 在这种情况下，如果以排除用户帐户数据执行成员资格数据库的仅有架构的部署，你将需要运行一个后期部署脚本来添加基本配置数据。
- 根据成员资格数据库的配置方式，成员资格提供程序可能使用计算机密钥对密码进行加密，并将其存储在数据库中。 在这种情况下，与数据库一起部署的所有用户帐户数据将都成为目标服务器上不可用。 为此，将用户帐户数据部署不受支持的方案。

## <a name="choosing-a-membership-database-strategy"></a>选择成员资格数据库策略

当你选择如何来设置企业服务器环境中的成员资格数据库，请使用以下准则：

- 只要有可能，则不要部署成员资格数据库。 相反，在目标数据库服务器上手动创建成员资格数据库。 如果你尚未自定义成员资格数据库架构，你可以只需创建一个新就地在目标中使用[ASP.NET SQL 服务器注册工具 (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx)。
- 如果你别无选择，但若要部署的成员资格数据库 （&） #x 2014; 例如，如果你已大量修改的数据库架构 （&） #x 2014; 你应执行的成员资格数据库，若要排除用户帐户数据，仅限架构的部署和然后运行一个后期部署脚本来添加任何所需的配置数据。 您可以在这些方法中找到全面的指南[如何： 部署 ASP.NET 成员资格数据库而不包括用户帐户](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx)。

务必记住*成员资格数据库的架构是，可能需要相当静态*。 即使你已自定义成员资格数据库，它不太，你将需要更新的定期 （&） #x 2014年上的架构; 它不要更改 web 应用程序或数据库项目中的代码的频率相同。 在这种情况下，则不需要任何自动进行的或单步执行部署过程中包括成员资格数据库。

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>使用 VSDBCMD 更新成员资格数据库架构

如果在第一个部署之后修改了成员资格数据库的结构，你可能不想要使用 Internet 信息服务 (IIS) Web 部署工具 （Web 部署） 来部署该数据库。 Web 部署中的数据库部署功能不包括的功能，以向的目标数据库 （&） #x 2014年差异更新，而是，Web 部署必须删除并重新创建该数据库。 这意味着，你会失去在过渡环境或生产环境中通常不需要任何现有用户帐户数据。

替代方法是使用 VSDBCMD 实用程序来更新目标数据库的架构。 VSDBCMD 包括两个重要功能。 首先，它可以将现有数据库架构导入.dbschema 文件。 其次，它可以为差异更新，这意味着它只是使将最新的目标数据库所需的更改，并且不会丢失任何数据，对现有数据库中部署.dbschema 文件。

你可以使用以下高级步骤更新成员资格数据库架构：

1. 使用 VSDBCMD**导入**操作来生成你的源成员资格数据库的.dbschema 文件。 中介绍了此过程[如何： 在命令提示符下导入架构](https://msdn.microsoft.com/library/dd172135.aspx)。
2. 使用 VSDBCMD**部署**操作将.dbschema 文件部署到你的目标成员资格数据库。 中介绍了此过程[VSDBCMD 的命令行参考。EXE （部署和架构导入）](https://msdn.microsoft.com/library/dd193283.aspx)。

## <a name="conclusion"></a>结束语

本主题介绍一些你可能需要进行设置各种目标环境中的 ASP.NET 成员资格数据库时所面临的挑战。 具体而言，它解释为什么仅限架构的部署将使成员资格数据库处于不可操作状态的状态和原因将用户帐户数据部署不支持。 本主题还提供有关如何设置、 部署和更新成员资格数据库中不同的方案的指南。

## <a name="further-reading"></a>其他阅读材料

有关更多指导和如何使用 VSDBCMD 的示例，请参阅[VSDBCMD 的命令行参考。EXE （部署和架构导入）](https://msdn.microsoft.com/library/dd193283.aspx)和[如何： 在命令提示符下导入架构](https://msdn.microsoft.com/library/dd172135.aspx)。 有关详细信息使用 aspnet\_regsql.exe 创建成员资格数据库，请参阅[ASP.NET SQL 服务器注册工具 (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx)。 有关部署成员资格数据库的更多常规指南，请参阅[如何： 部署 ASP.NET 成员资格数据库而不包括用户帐户](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx)。

>[!div class="step-by-step"]
[上一页](deploying-database-role-memberships-to-test-environments.md)
[下一页](excluding-files-and-folders-from-deployment.md)
