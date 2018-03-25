---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
title: "配置目标环境的部署属性 |Microsoft 文档"
author: jrjlee
description: "本主题介绍如何配置特定于环境的属性，以便将示例联系人管理器解决方案部署到特定目标环境..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b5b86e03-b8ed-46e6-90fa-e1da88ef34e9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
msc.type: authoredcontent
ms.openlocfilehash: f27b8376b332ff21185be0fd5c00ced7d40a20bd
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="configuring-deployment-properties-for-a-target-environment"></a>配置目标环境的部署属性
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何进行配置以将示例联系人管理器解决方案部署到特定目标环境的特定于环境的属性。


本主题窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序解决方案 （&) #x 2014;Communication Foundation (WCF) 服务和数据库项目。

这些教程的核心的部署方法取决于中介绍的拆分项目文件方法[了解该生成过程](../web-deployment-in-the-enterprise/understanding-the-build-process.md)，在其中生成过程控制由两个项目文件 （&） #x 2014; 一个包含生成适用于每种目标环境和一个包含特定于环境的生成和部署设置的说明。 在生成期间，特定于环境的项目文件合并到环境无关的项目文件中以形成一组完整的生成说明。

## <a name="process-overview"></a>过程概述

你将使用来生成和部署联系人管理器解决方案的项目文件拆分为两个物理文件：

- 一个包含世界生成设置和指令 ( *Publish.proj*文件)。
- 一个包含特定于环境的生成设置 (*Env Dev.proj*， *Env Stage.proj*，依次类推)。

在生成期间，相应的特定于环境的项目文件合并到通用*Publish.proj*文件中以构成一组完整的生成说明。 你可以通过创建或自定义特定于环境的项目文件的描述你自己的部署方案的设置配置部署到特定目标环境。

大量这些值由你的目标环境的配置方式 （&） #x 2014年; 具体而言，是否你在目标 web 服务器被配置为使用 Web 部署代理服务 （远程代理） 或 Web 部署处理程序。 这些方法的详细信息并选择适当的方法自己的环境的指导，请参阅[选择用于 Web 部署的右方法](choosing-the-right-approach-to-web-deployment.md)。

[联系人管理器方案](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)需要两个特定于环境的项目文件：

- 部署到开发人员测试环境 (*Env Dev.proj*)。 开发人员的测试环境配置为接受使用远程代理的远程部署中所述[方案： 为 Web 部署配置测试环境](scenario-configuring-a-test-environment-for-web-deployment.md)。 此文件需要终结点地址，以及特定于位置的设置，例如，连接字符串和服务终结点提供远程代理。
- 部署到过渡环境 (*Env Stage.proj*)。 过渡环境配置为接受使用 Web 部署处理程序的远程部署中所述[方案： 配置用于 Web 部署的暂存环境](scenario-configuring-a-staging-environment-for-web-deployment.md)。 此文件需要提供的 Web 部署处理程序终结点地址以及特定于位置的设置，例如连接字符串和服务终结点。

务必要注意在特定于环境的项目文件中配置的设置不会影响 web 的内容包本身和 #x 2014年; 相反，它们控制如何部署包和时，提供了哪些参数值提取包。 要导入的 web 包到生产环境中所述手动[方案： 配置 Web 部署的生产环境](scenario-configuring-a-production-environment-for-web-deployment.md)和[手动安装 Web 包](../web-deployment-in-the-enterprise/manually-installing-web-packages.md)，因此，它并不重要哪些设置你使用在特定于环境的项目文件中，生成包时。 Internet 信息服务 (IIS) 管理器将提示你输入任何参数化值，如连接字符串和服务终结点时导入包。

要将联系人管理器解决方案部署到你自己的目标环境中，可以自定义此文件或可以将其用作模板，然后创建你自己的文件。

**若要配置的联系人管理器解决方案的特定于环境的部署设置**

1. 在 Visual Studio 2010 中打开 ContactManager WCF 解决方案。
2. 在**解决方案资源管理器**窗口中，展开**发布**文件夹中，展开**EnvConfig**文件夹，然后再双击**Env Dev.proj**.

    ![](configuring-deployment-properties-for-a-target-environment/_static/image1.png)
3. 替换中的属性值*Env Dev.proj*使用正确的值为你自己的测试环境的文件。

    > [!NOTE]
    > 此过程后面的表提供有关上述每个属性的详细信息。
4. 保存你的工作，然后关闭*Env Dev.proj*文件。

## <a name="choosing-the-right-deployment-properties"></a>选择正确的部署属性

下表介绍在示例特定于环境的项目文件中，每个属性的用途*Env Dev.proj*，并提供一些指导应提供的值。

| 属性名 | 详细信息 |
| --- | --- |
| **MSDeployComputerName**目标 web 服务器或服务终结点的名称。 | 如果你正在部署到目标 web 服务器上的远程代理服务，则可以指定的目标计算机名称 (例如， **TESTWEB1**或**TESTWEB1.fabrikam.net**)，也可以指定远程代理终结点 (例如， `http://TESTWEB1/MSDEPLOYAGENTSERVICE`)。 部署中每个用例的相同的方式。 如果你正在将部署到 Web 部署处理程序中的目标 web 服务器上，你应指定服务终结点，并且包括查询字符串参数作为 IIS 网站的名称 (例如， `https://STAGEWEB1:8172/MSDeploy.axd?site=DemoSite`)。 |
| **MSDeployAuth** Web 部署应使用对远程计算机进行身份验证的方法。 | 此属性应设置为**NTLM**或**基本**。 通常情况下，你将使用**NTLM**如果你正在部署到远程代理服务和**基本**如果你正在将它们部署到 Web 部署处理程序。 如果使用基本身份验证，你还需要指定的用户名和密码，IIS Web 部署工具 （Web 部署） 应模拟以执行部署。 在此示例中，这些值通过提供**MSDeployUsername**和**MSDeployPassword**属性。 如果使用 NTLM 身份验证，可以省略这些属性，或将其留空。 |
| **MSDeployUsername**如果使用基本身份验证时，Web 部署将使用此帐户在远程计算机上。 | 这应采用以下形式*域*\*用户名 * (例如， **FABRIKAM\matt**)。 如果指定基本身份验证，仅使用此值。 如果使用 NTLM 身份验证，则可以省略该属性。 如果提供一个值，则它将被忽略。 |
| **MSDeployPassword**如果使用基本身份验证时，Web 部署将使用此密码在远程计算机上。 | 这是中指定的用户帐户的密码**MSDeployUsername**属性。 如果指定基本身份验证，仅使用此值。 如果使用 NTLM 身份验证，则可以省略该属性。 如果提供一个值，则它将被忽略。 |
| **ContactManagerIisPath**你想要部署联系人管理器 MVC 应用程序的 IIS 路径。 | 这应为的路径，该窗体中出现在 IIS 管理器中，[*IIS 网站名称*] / [*web * * 应用程序名称*]。 请记住，需要部署应用程序之前存在的 IIS 网站。 例如，如果你已创建名为 DemoSite 的 IIS 网站，则可以作为 DemoSite/ContactManager 指定 MVC 应用程序的 IIS 路径。 |
| **ContactManagerServiceIisPath**你想要部署的联系人管理器 WCF 服务的 IIS 路径。 | 例如，如果你已创建名为 DemoSite 的 IIS 网站，则可以指定 WCF 服务的 IIS 路径**DemoSite/ContactManagerService**。 |
| **ContactManagerTargetUrl**可以从该处访问 WCF 服务的 URL。 | 这将采用格式 [*IIS 网站根 URL*] / [*服务应用程序名称*] / [*服务终结点*]。 例如，如果你已在端口 85 上创建 IIS 网站，URL 将采用以下形式`http://localhost:85/ContactManagerService/ContactService.svc`。 请记住 MVC 应用程序和 WCF 服务部署到同一台服务器。 因此，永远只有从安装它的计算机访问此 URL。 因此，它是在 URL 中使用 localhost 或 IP 地址，而不是计算机名称或主机标头，更好的做法。 如果你使用的计算机名称或主机标头，[环回检查](https://go.microsoft.com/?linkid=9805131)在 IIS 中的安全功能可能阻止 URL 和返回**HTTP 401.1-未授权**错误。 |
| **CmDatabaseConnectionString**数据库服务器的连接字符串。 | 连接字符串将确定这两个 VSDBCMD 将用于联系数据库服务器和创建数据库和 web 服务器应用程序池将用于联系数据库服务器和与数据库交互的凭据的凭据。 实质上是此处有两个选择。 可以指定**集成安全性 = true**，在这种情况下使用集成的 Windows 身份验证：**数据源 = TESTDB1; Integrated Security = true**在这种情况下，数据库将使用创建运行可执行文件、 VSDBCMD 的用户和应用程序的凭据将访问使用 web 服务器计算机帐户的标识的数据库。 或者，你可以指定的用户名和密码的 SQL Server 帐户。 同时 VSDBCMD 创建数据库以及要与数据库交互的应用程序池，在这种情况下，使用的 SQL Server 凭据：**数据源 = TESTDB1;用户 Id = ASqlUser;密码 = Pa$ $w0rd**本主题中的演练假定你将使用集成的 Windows 身份验证。 |
| **CmTargetDatabase**想要允许你将在数据库服务器创建数据库的名称。 | 作为参数情况下，你在此处提供的值添加到 VSDBCMD 命令。 此外可用于生成 web 服务器上的应用程序池可用于与数据库交互的完整连接字符串。 |
  

这些示例所示，你可以如何配置这些属性对于特定部署方案。

### <a name="example-1x2014deployment-to-the-remote-agent-service"></a>示例 1 & #x 2014年; 部署到的远程代理服务

在此示例中：

- 要部署到 TESTWEB1 上的远程代理服务。
- 要指示 Web Deploy 以使用 NTLM 身份验证。 Web 部署将使用用于调用 Microsoft Build Engine (MSBuild) 的凭据运行。
- 你要使用集成身份验证部署**ContactManager** TESTDB1 到的数据库。 数据库会使用用于调用 MSBuild 的凭据进行部署。


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample1.xml)]


### <a name="example-2x2014deployment-to-the-web-deploy-handler-endpoint"></a>示例 2 & #x 2014年; 部署到 Web 部署处理程序终结点

在此示例中：

- 要部署到 STAGEWEB1 上的 Web 部署处理程序服务终结点。
- 要指示 Web Deploy 以使用基本身份验证。
- 你要指定 Web 部署应模拟的远程计算机上的 FABRIKAM\stagingdeployer 帐户。
- 您正在使用 SQL Server 身份验证来部署**ContactManager** STAGEDB1 到的数据库。


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample2.xml)]


## <a name="conclusion"></a>结束语

此时，你的项目文件得到完全配置以生成并部署到一个或多个目标环境的联系人管理器解决方案。

若要使用这些项目文件作为单步执行、 可重复部署过程的一部分，你需要执行*Publish.proj*文件使用 MSBuild，并在特定于环境的项目文件的位置作为参数传递。 可以通过多种方式来执行此操作：

- 有关 MSBuild 和自定义项目文件的简介的概述，请参阅[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。
- 有关如何制定执行自定义项目文件的 MSBuild 命令的信息，请参阅[部署 Web 包](../web-deployment-in-the-enterprise/deploying-web-packages.md)。
- 有关如何将 MSBuild 命令合并到单步执行、 可重复部署的命令文件的信息，请参阅[创建并运行部署命令文件](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md)。
- 有关如何从 Team Build 执行自定义项目文件的信息，请参阅[创建该支持部署的生成定义](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)。

>[!div class="step-by-step"]
[上一篇](creating-a-server-farm-with-the-web-farm-framework.md)
