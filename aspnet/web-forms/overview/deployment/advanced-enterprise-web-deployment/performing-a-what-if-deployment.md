---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: "如果执行是什么部署 |Microsoft 文档"
author: jrjlee
description: "本主题描述如何执行假设 （或模拟） 使用 Internet 信息服务 (IIS) Web 部署工具 （Web 部署） 和 V 部署..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: cea805c86f0764c7443ccc5c9f89248860a6a842
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="performing-a-what-if-deployment"></a>执行"假设"部署
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题描述如何执行"假设"（或模拟） 使用的 Internet 信息服务 (IIS) Web 部署工具 （Web 部署） 和 VSDBCMD 部署。 这使你确定你的部署逻辑的对特定目标环境的影响，在实际部署你的应用程序之前。


本主题窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器解决方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; 来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序Communication Foundation (WCF) 服务和数据库项目。

这些教程的核心的部署方法取决于中介绍的拆分项目文件方法[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，请在生成和部署过程控制由两个项目文件 & #x 2014年; one 包含生成适用于每种目标环境和一个包含特定于环境的生成和部署设置的说明进行操作。 在生成期间，特定于环境的项目文件合并到环境无关的项目文件中以形成一组完整的生成说明。

## <a name="performing-a-what-if-deployment-for-web-packages"></a>执行"假设"部署 Web 包

Web 部署包括可以使用"假设"执行中的部署的功能 （或试用版） 模式。 在部署项目在"假设"模式下时，Web 部署生成日志文件，就像执行了该部署，但它不会实际更改目标服务器上的任何内容。 查看日志文件可帮助你了解你的部署将特别是对目标服务器上，产生的影响：

- 获取添加内容。
- 将获取更新的内容。
- 内容将会被删除。

因为"假设"部署实际上不会更改任何内容在目标服务器上，它做些什么无法始终是预测部署将会成功。

中所述[部署 Web 包](../web-deployment-in-the-enterprise/deploying-web-packages.md)，你可以部署使用 Web 部署两个方式 & #x 2014年; 通过使用 MSDeploy.exe 命令行实用工具直接或通过运行 web 包*。 deploy.cmd*生成过程生成的文件。

如果你直接使用 MSDeploy.exe，你可以通过添加运行"假设"部署**– whatif**标志设为你的命令。 例如，若要评估如果 ContactManager.Mvc.zip 包部署到过渡环境，会发生什么情况，MSDeploy 命令应类似如下：


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


如果你所使用"假设"部署的结果感到满意，你可以删除**– whatif**标志来运行实时部署。

> [!NOTE]
> MSDeploy.exe 命令行选项的详细信息，请参阅[Web 部署操作设置](https://technet.microsoft.com/library/dd569089(WS.10).aspx)。


如果你使用*。 deploy.cmd*文件，你可以通过包括运行"假设"部署**/t**标志 （试用模式） 标志而不是**/y**中的标志 （"是，"或更新模式）你的命令。 例如，若要评估如果通过运行部署 ContactManager.Mvc.zip 包会发生什么情况*。 deploy.cmd*文件，你的命令应类似如下：


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


如果你所使用你的"试用模式"部署的结果感到满意，你可以替换**/t**标志与**/y**标志来运行实时部署：


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> 有关详细信息的命令行选项*。 deploy.cmd*文件，请参阅[如何： 安装部署包 Using deploy.cmd 文件](https://msdn.microsoft.com/library/ff356104.aspx)。 如果你运行*。 deploy.cmd*文件无需指定任何标志，命令提示符将显示可用标志的列表。


## <a name="performing-a-what-if-deployment-for-databases"></a>执行"假设"部署数据库

本部分假设你使用的 VSDBCMD 实用程序来执行增量、 基于架构的数据库部署。 这种方法在更多详细信息中所述[部署数据库项目](../web-deployment-in-the-enterprise/deploying-database-projects.md)。 我们建议您了解与本主题之前应用此处所述的概念。

当你使用在 VSDBCMD**部署**模式下，你可以使用**/dd** (或**/DeployToDatabase**) 是否 VSDBCMD 实际部署了数据库，或只生成控制标志部署脚本。 如果你正在部署.dbschema 文件，这是行为：

- 如果指定**/dd+**或**/dd**，VSDBCMD 将生成部署脚本并部署数据库。
- 如果指定**/dd-**或省略开关，VSDBCMD 将生成部署脚本。

> [!NOTE]
> 如果你正在部署.deploymanifest 文件而不是.dbschema 文件，行为**/dd**开关是更加复杂。 VSDBCMD 实质上，将忽略的值**/dd**切换如果.deploymanifest 文件包含**DeployToDatabase**具有的值的元素**True**。 [部署数据库项目](../web-deployment-in-the-enterprise/deploying-database-projects.md)描述完全此行为。


例如，若要生成部署脚本**ContactManager**数据库而不实际部署的数据库，VSDBCMD 命令应类似如下：


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


VSDBCMD 是一个差异数据库部署工具，并且这种情况下部署脚本动态生成以包含所有更新当前数据库中，如果存在，指定架构所需的 SQL 命令。 查看部署脚本是任何有用的方法来确定有何影响你的部署将对当前数据库和它所包含的数据。 例如，你可能想要确定：

- 是否将删除任何现有表，并且是否这将导致数据丢失。
- 是否操作的顺序会带来风险的数据丢失，例如，如果你要拆分或合并表。

如果你满意部署脚本，您可以重复使用 VSDBCMD **/dd+**标志来进行更改。 或者，你可以编辑用于满足你的需求，然后在数据库服务器上手动执行的部署脚本。

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>将"假设"功能集成到自定义项目文件

在更复杂的部署方案中，你将想要使用自定义的 Microsoft Build Engine (MSBuild) 项目文件来封装你的生成和部署逻辑中所述[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。 例如，在[联系人管理器](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)示例解决方案， *Publish.proj*文件：

- 生成解决方案。
- 使用 Web 部署来打包和部署 ContactManager.Mvc 应用程序。
- 使用 Web 部署来打包和部署 ContactManager.Service 应用程序。
- 将部署**ContactManager**数据库。

当您将多个 web 包和/或数据库的部署集成到这种方式中的单步执行进程时，您可能还要执行整个部署以在"假设"模式下的选项。

*Publish.proj*文件演示如何执行此操作。 首先，你需要创建一个属性存储"假设"值：


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


在这种情况下，你已创建一个名为属性**WhatIf**默认值为**false**。 用户可以通过将属性设置为覆盖此值**true**在命令行参数，当你将看到很快。

下一步是参数化任何 Web 部署和 VSDBCMD 命令，以便标志反映**WhatIf**属性值。 例如下, 一步的目标 (摘自*Publish.proj*文件，并简化) 运行*。 deploy.cmd*文件将部署 web 包。 默认情况下，该命令包括**/Y**开关 （"是，"或更新模式）。 如果**WhatIf**设置为**true**，这将替换为**/T**开关 （试用版或"假设"模式）。


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


同样下, 一步目标使用 VSDBCMD 实用程序来部署数据库。 默认情况下， **/dd**不包含开关。 这意味着 VSDBCMD 将生成部署脚本，但不是会将部署的数据库 （&） #x 2014; 换句话说，"假设"方案。 如果**WhatIf**属性未设置为**true**、 **/dd**添加交换机和 VSDBCMD 将部署数据库。


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


相同的方法可用于所有相关的命令参数化项目文件中。 如果你想要运行"假设"部署，你可以然后只需提供**WhatIf**从命令行的属性值：


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


这种方式，你可以运行"假设"部署的单个步骤中的所有项目组件。

## <a name="conclusion"></a>结束语

本主题介绍如何运行"假设"使用 Web 部署、 VSDBCMD 和 MSBuild 的部署。 "假设"部署可让你实际到目标环境进行任何更改前评估影响的建议部署。

## <a name="further-reading"></a>其他阅读材料

有关 Web 部署命令行语法的详细信息，请参阅[Web 部署操作设置](https://technet.microsoft.com/library/dd569089(WS.10).aspx)。 有关当你使用的命令行选项的指导*。 deploy.cmd*文件，请参阅[如何： 安装部署包 Using deploy.cmd 文件](https://msdn.microsoft.com/library/ff356104.aspx)。 有关 VSDBCMD 命令行语法的指南，请参阅[VSDBCMD 的命令行参考。EXE （部署和架构导入）](https://msdn.microsoft.com/library/dd193283.aspx)。

>[!div class="step-by-step"]
[上一页](advanced-enterprise-web-deployment.md)
[下一页](customizing-database-deployments-for-multiple-environments.md)
