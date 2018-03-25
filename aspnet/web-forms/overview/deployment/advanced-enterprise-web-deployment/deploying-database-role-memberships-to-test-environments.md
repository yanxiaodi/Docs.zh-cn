---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: "将数据库角色成员身份部署到测试环境 |Microsoft 文档"
author: jrjlee
description: "本主题介绍如何将用户帐户添加到解决方案部署到测试环境的一部分的数据库角色。 当你部署的解决方案包含..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: 226c28622f76e866fba1fc33cf9b9b7a01e5295b
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="deploying-database-role-memberships-to-test-environments"></a>将数据库角色成员身份部署到测试环境
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何将用户帐户添加到解决方案部署到测试环境的一部分的数据库角色。
> 
> 在部署包含过渡或生产环境的数据库项目的解决方案时，你通常不想开发人员可以自动执行添加到数据库角色的用户帐户。 在大多数情况下，开发人员不会知道哪些用户帐户需要添加到哪些数据库角色，并随时可以更改这些要求。 但是，当你部署的解决方案包含一个数据库项目与一个开发或测试环境，这种情况通常是而不同：
> 
> - 开发人员通常重新部署解决方案定期，通常多次一天。
> - 通常在每个部署，这意味着数据库用户必须创建和添加到角色中每个部署后重新创建该数据库。
> - 开发人员通常具有对目标开发或测试环境的完全控制。
> 
> 在此方案中，通常很有利，可自动创建数据库用户并将数据库角色成员身份分配作为部署过程的一部分。
> 
> 关键因素是，此操作需要条件基于目标环境。 如果你要部署到过渡或生产环境，你想要跳过该操作。 如果你正在将部署到开发人员或测试环境，你想要部署角色成员身份，而无需进一步的干预。 本主题介绍一种方法可用来应对此挑战。


本主题窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器解决方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; 来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序Communication Foundation (WCF) 服务和数据库项目。

这些教程的核心的部署方法取决于中介绍的拆分项目文件方法[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在其中生成过程控制由两个项目文件 （&） #x 2014; 一个包含生成适用于每种目标环境和一个包含特定于环境的生成和部署设置的说明。 在生成期间，特定于环境的项目文件合并到环境无关的项目文件中以形成一组完整的生成说明。

## <a name="task-overview"></a>任务概述

本主题假定：

- 中所述使用解决方案部署的拆分项目文件方法[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。
- 中所述从项目文件以部署数据库项目时，调用 VSDBCMD[了解该生成过程](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。

若要创建数据库用户并分配角色成员身份，数据库项目部署到测试环境时，你将需要：

- 创建一个将做出必要的数据库更改的 Transact 结构化查询语言 (Transact SQL) 脚本。
- 创建的 Microsoft Build Engine (MSBuild) 目标，使用 sqlcmd.exe 实用程序来运行 SQL 脚本。
- 配置你的项目文件时要调用目标正在将解决方案部署到测试环境。

本主题将演示如何执行每个这些过程。

## <a name="scripting-the-database-role-memberships"></a>脚本的数据库角色成员身份

可以在许多不同的方式，创建 TRANSACT-SQL 脚本，然后在任何位置，你选择。 最简单的方法是在 Visual Studio 2010 中创建你的解决方案内的脚本。

**若要创建 SQL 脚本**

1. 在**解决方案资源管理器**窗口中，展开你的数据库项目节点。
2. 右键单击**脚本**文件夹，指向**添加**，然后单击**新文件夹**。
3. 类型**测试**作为文件夹名称，然后按 enter 键。
4. 右键单击**测试**文件夹，指向**添加**，然后单击**脚本**。
5. 在**添加新项**对话框框中，为你的脚本提供有意义的名称 (例如， **AddRoleMemberships.sql**)，然后单击**添加**。

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. 在*AddRoleMemberships.sql*文件中，添加 TRANSACT-SQL 语句的：

    1. 创建将访问你的数据库的 SQL Server 登录名的数据库用户。
    2. 将数据库用户添加到任何所需的数据库角色。
7. 该文件应类似如下：

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. 保存该文件。

## <a name="executing-the-script-on-the-target-database"></a>在目标数据库上执行脚本

理想情况下，在部署您的数据库项目时将作为部署后脚本的一部分运行任何所需的 TRANSACT-SQL 脚本。 但是，部署后脚本不允许您执行有条件地根据解决方案配置或生成属性的逻辑。 替代项是直接从 MSBuild 项目文件中，运行您的 SQL 脚本，通过创建**目标**执行 sqlcmd.exe 命令的元素。 此命令可用于在目标数据库上运行你的脚本：


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> Sqlcmd 命令行选项的详细信息，请参阅[sqlcmd 实用工具](https://msdn.microsoft.com/library/ms162773.aspx)。


此命令嵌入的 MSBuild 目标之前，你需要考虑在什么条件下你想要运行的脚本：

- 在更改其角色成员身份之前，必须存在目标数据库。 在这种情况下，你需要运行此脚本*后*的数据库部署。
- 你需要包括的条件，以便仅用于测试环境执行该脚本。
- 如果你正在运行的"假设"部署 （&） #x 2014; 换而言之，如果你正在生成部署脚本，但不是实际运行它们 （&） #x 2014; 你不应运行 SQL 脚本。

如果你使用的拆分项目文件方法中所述[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，如的联系人管理器示例解决方案所示，你可以将构建说明拆分 SQL 脚本如下：

- 任何所需的特定于环境的属性，属性，用于确定是否将部署权限，以及应该在特定于环境的项目文件中 (例如， *Env Dev.proj*)。
- MSBuild 目标本身，以及将不会更改目标环境之间的任何属性应该在通用项目文件中 (例如， *Publish.proj*)。

在特定于环境的项目文件中，你需要定义数据库服务器名称、 目标数据库名称和允许用户指定是否将部署角色成员身份的布尔值属性。


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


在通用项目文件中，你需要提供 sqlcmd 可执行文件的位置以及你想要运行的 SQL 脚本的位置。 这些属性将保持不变，而目标环境无关。 你还需要创建用于执行 sqlcmd 命令 MSBuild 目标。


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


请注意将 sqlcmd 可执行文件的位置添加为静态属性，这可能十分有用到其他目标。 与此相反，你定义你的 SQL 脚本的位置和 sqlcmd 命令的语法为目标内的动态属性将不需要执行目标之前。 在这种情况下， **DeployTestDBPermissions**如果满足这些条件，将仅执行目标：

- **DeployTestDBRoleMemberships**属性设置为**true**。
- 用户未指定**WhatIf = true**标志。

最后，不要忘记调用目标。 在*Publish.proj*文件，你可以执行此操作通过将目标添加到默认值的依赖项列表**FullPublish**目标。 你需要确保**DeployTestDBPermissions**直到才执行目标**PublishDbPackages**目标已执行。


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>结束语

本主题介绍一种方法可以在其中你添加数据库用户和角色成员身份作为后期部署操作部署数据库项目时。 当你定期重新创建在测试环境中，一个数据库，但它通常如果应避免在将数据库部署到过渡环境或生产环境时，这是通常有用。 在这种情况下，应确保你使用的所需的条件逻辑，以便相应若要这样做时仅创建数据库用户和角色成员身份。

## <a name="further-reading"></a>其他阅读材料

使用 VSDBCMD 部署数据库项目的详细信息，请参阅[部署数据库项目](../web-deployment-in-the-enterprise/deploying-database-projects.md)。 有关自定义不同的目标环境的数据库部署的指南，请参阅[为多个环境自定义数据库部署](customizing-database-deployments-for-multiple-environments.md)。 使用自定义 MSBuild 项目文件来控制部署过程的详细信息，请参阅[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)和[了解该生成过程](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。 Sqlcmd 命令行选项的详细信息，请参阅[sqlcmd 实用工具](https://msdn.microsoft.com/library/ms162773.aspx)。

>[!div class="step-by-step"]
[上一页](customizing-database-deployments-for-multiple-environments.md)
[下一页](deploying-membership-databases-to-enterprise-environments.md)
