---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: "部署 Web 包 |Microsoft 文档"
author: jrjlee
description: "本主题介绍如何在您可以发布 web 部署包到远程服务器通过使用 Internet 信息服务 (IIS) Web 部署工具 (Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: cd2bfa07262155b68ac4605fc7e9748d276d3193
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="deploying-web-packages"></a>部署 Web 包
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何在您可以发布 web 部署包到远程服务器通过使用 Internet 信息服务 (IIS) Web 部署工具 （Web 部署） 2.0。
> 
> 有两种主要方法，你可以在其中部署到远程服务器的 web 包：
> 
> - 你可以直接使用 MSDeploy.exe 命令行实用工具。
> - 你可以运行*[项目名称].deploy.cmd*生成过程生成的文件。
> 
> 最终结果是相同的无论哪种方法你使用。 从根本上来说，所有*。 deploy.cmd*文件的用途是运行 MSDeploy.exe 的某些预先确定的值，以便无需提供尽可能多的信息，以便部署包。 这可以简化部署过程。 另一方面，直接使用 MSDeploy.exe 可更大的灵活性通过完全你的包的部署方式。
> 
> 你使用哪种方法将取决于多种因素，包括控制程度您需要对部署过程，并且你是否面向 Web 部署远程代理服务或 Web 部署处理程序。 本主题说明如何使用每种方法，并标识时每种方法很适合。
> 
> 任务和本主题中的演练假定：
> 
> - 你已生成并打包 web 应用程序中所述[生成和打包 Web 应用程序项目](building-and-packaging-web-application-projects.md)。
> - 在你修改*SetParameters.xml*文件中所述为你的目标环境，提供正确的参数值[Web 包部署的配置参数](configuring-parameters-for-web-package-deployment.md)。


运行 [*项目名称*]*。 deploy.cmd*文件是部署 web 包的最简单方法。 具体而言，使用*。 deploy.cmd*文件提供通过直接使用 MSDeploy.exe 以下优势：

- 不需要指定的位置的 web 部署包 （&） #x 2014; *。 deploy.cmd*文件已知道其所在。
- 不需要指定的位置*SetParameters.xml*文件 （&) #x 2014; *。 deploy.cmd*文件已知道其所在。
- 不需要指定源和目标 MSDeploy 提供程序 （&） #x 2014; *。 deploy.cmd*文件已知道要使用的值。
- 不需要指定 MSDeploy 操作设置 （&） #x 2014; *。 deploy.cmd*文件需要将常需要的值自动添加到 MSDeploy.exe 命令。

在使用之前*。 deploy.cmd*文件将部署 web 包，您应该确保：

- *。 Deploy.cmd*文件，[*项目名称*]。*SetParameters.xml*文件和 web 包 ([*项目名称*]。*zip*) 位于同一文件夹中。
- 在运行的计算机上安装 web 部署 (MSDeploy.exe) *。 deploy.cmd*文件。

*。 Deploy.cmd*文件支持各种命令行选项。 从命令提示符运行该文件时，这是基本语法：


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


你必须指定**/T**标志或**/Y**标志，以指示你是否想要分别执行一次测试运行或实时的部署 （请勿使用这两个标志同一命令中）。 此表介绍了每个标记的用途。

| Flag | 描述 |
| --- | --- |
| **/T** | 调用与 MSDeploy.exe **– whatif**标志，它表示一种试运行。 而不是将包部署，它将创建一份如果未部署包，会发生什么情况。 |
| **/Y** | 调用而无需 MSDeploy.exe **– whatif**标志。 这将包部署到本地计算机或指定的目标服务器。 |
| **/M** | 指定的目标服务器名称或服务 URL。 你可以在此处提供的值的详细信息，请参阅**终结点注意事项**本主题中的部分。 如果省略**/M**标志，包将部署到本地计算机。 |
| **/A** | 指定应使用 MSDeploy.exe 执行部署的身份验证类型。 可能的值为**NTLM**和**基本**。 如果省略**/A**标志，身份验证类型将默认为**NTLM**部署到 Web 部署远程代理服务和**基本**以部署到 Web 部署处理程序。 |
| **/U** | 指定的用户名。 这仅适用于你使用基本身份验证。 |
| **/P** | 指定密码。 这仅适用于你使用基本身份验证。 |
| **/L** | 指示包应将部署到 IIS Express 的本地实例。 |
| **/G** | 指定使用部署包[tempAgent 提供程序设置](https://technet.microsoft.com/library/ee517345(WS.10).aspx)。 如果省略**/G**标志，则该值默认为**false**。 |

> [!NOTE]
> 每次生成过程创建一个 web 包，它还创建名为的文件*[项目名称].deploy readme.txt*说明这些部署选项。


除了这些标志，还可以为其他指定 Web 部署操作设置*。 deploy.cmd*参数。 你指定的任何其他设置直接传递到基础 MSDeploy.exe 命令。 有关这些设置的详细信息，请参阅[Web 部署操作设置](https://technet.microsoft.com/library/dd569089(WS.10).aspx)。

假设你想要将 ContactManager.Mvc web 应用程序项目部署到测试环境中，通过运行*。 deploy.cmd*文件。 你的测试环境配置为使用 Web 部署远程代理服务中所述[配置用于 Web 部署发布 （远程代理） 的 Web 服务器](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)。 若要部署 web 应用程序，你需要完成后续步骤。

**若要部署 web 应用程序使用。 deploy.cmd 文件**

1. 生成并打包 web 应用程序项目中中, 所述[生成和打包 Web 应用程序项目](building-and-packaging-web-application-projects.md)。
2. 修改*ContactManager.Mvc.SetParameters.xml*文件，以包含你的测试环境的正确的参数值中所述[Web 包部署的配置参数](configuring-parameters-for-web-package-deployment.md)。
3. 打开命令提示符窗口并导航到的位置*ContactManager.Mvc.deploy.cmd*文件。
4. 键入以下命令，然后按 Enter:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

在此示例中：

- **/Y**标志指示你想要实际部署包，而不是执行一个试用版运行。
- **/M**标志指示你想要将包部署到名为 TESTWEB1 的服务器。 此值，从 MSDeploy.exe 将尝试将包部署到 http://TESTWEB1/MSDeployAgentService 的 Web 部署远程代理服务。
- **/A**标志指示你想要使用 NTLM 身份验证。 在这种情况下，你不必指定用户名和密码。

为了说明如何使用*。 deploy.cmd*文件来简化部署过程，看一看获取生成并执行运行时的 MSDeploy.exe 命令*ContactManager.Mvc.deploy.cmd*使用上面所示的选项。


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


有关详细信息使用*。 deploy.cmd*文件来部署 web 包信息，请参阅[如何： 安装部署包 Using deploy.cmd 文件](https://msdn.microsoft.com/library/ff356104.aspx)。

## <a name="using-msdeployexe"></a>使用 MSDeploy.exe

尽管使用*。 deploy.cmd*文件通常可简化部署过程，有一些情况下直接使用 MSDeploy.exe 更可取的方法时。 例如:

- 如果你想要将部署到 Web 部署处理程序中作为非管理员用户，则无法使用*。 deploy.cmd*文件。 这是因为 Web Deploy 2.0 中的 bug 下所述**终结点注意事项**。
- 如果你想要手动切换不同*SetParameters.xml*不同位置中的文件，你可能希望直接使用 MSDeploy.exe。
- 如果你想要重写多个 MSDeploy.exe 命令行参数，你可能希望直接使用 MSDeploy.exe。

当使用 MSDeploy.exe 时，你需要提供的信息的三个主要部分：

- A **– 源**参数可指示你的数据来源于何处。
- A **– dest**参数可指示在何处你的数据到。
- A **– 谓词**参数可指示[操作](https://technet.microsoft.com/library/dd568989(WS.10).aspx)你想要执行。

MSDeploy.exe 依赖于[Web Deploy 提供程序](https://technet.microsoft.com/library/dd569040(WS.10).aspx)处理源和目标数据。 Web 部署包括大量的表示范围内的应用程序和数据源，它就能使用 （&） #x 2014年的提供程序; 例如，有提供程序的 SQL Server 数据库、 IIS web 服务器、 证书、 全局程序集缓存 (GAC) 程序集，各种不同的配置文件和很多其他类型的数据。 这两个**– 源**参数和**– dest**参数必须指定一个提供程序，在窗体中**– 源**: [*providerName*] = [*位置*]。 当你要部署到 IIS 网站的 web 包时，您应使用这些值：

- **– 源**提供程序始终[包](https://technet.microsoft.com/library/dd569019(WS.10).aspx)。 例如:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- **– Dest**提供程序始终[自动](https://technet.microsoft.com/library/dd569016(WS.10).aspx)。例如:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- **– 谓词**始终**同步**。

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

此外，你将需要指定各种其他[特定于提供程序的设置](https://technet.microsoft.com/library/dd569001(WS.10).aspx)和常规[操作设置](https://technet.microsoft.com/library/dd569089(WS.10).aspx)。 例如，假设你想要部署到过渡环境 ContactManager.Mvc web 应用程序。 部署面向 Web 部署处理程序，并且必须使用基本身份验证。 若要部署 web 应用程序，你需要完成后续步骤。

**部署 web 应用程序使用 MSDeploy.exe**

1. 生成并打包 web 应用程序项目中中, 所述[生成和打包 Web 应用程序项目](building-and-packaging-web-application-projects.md)。
2. 修改*ContactManager.Mvc.SetParameters.xml*文件，以包含你的过渡环境，正确的参数值中所述[Web 包部署的配置参数](configuring-parameters-for-web-package-deployment.md)。
3. 打开命令提示符窗口并浏览到 MSDeploy.exe 的位置。 这通常是在 %PROGRAMFILES%\IIS\Microsoft Web 部署 V2\msdeploy.exe。
4. 键入此命令，然后按 enter 键 （忽略换行符处换行）：

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

在此示例中：

- **– 源**参数指定**包**提供程序，并指示 web 包的位置。
- **– Dest**参数指定**自动**提供程序。 **ComputerName**设置目标服务器上提供的 Web 部署处理程序的服务 URL。 **Authtype**设置指示你想要使用基本身份验证，并且这种情况下，你需要提供**用户名**和**密码**。 最后， **includeAcls ="False"**设置指示你不想要将文件的访问控制列表 (Acl) 复制到目标服务器源 web 应用程序中。
- **– 谓词： 同步**参数指示你想要复制的目标服务器上的源内容。
- **– DisableLink**参数指示你不想要复制应用程序池、 虚拟目录配置或在目标服务器上的安全套接字层 (SSL) 证书。 有关详细信息，请参阅[Web 部署链接扩展](https://technet.microsoft.com/library/dd569028(WS.10).aspx)。
- **– SetParamFile**参数提供的位置*SetParameters.xml*文件。
- **– AllowUntrusted**开关表示 Web 部署应接受不受信任的证书颁发机构颁发的 SSL 证书。 如果你正在将它们部署到 Web 的部署处理程序，并且你已使用自签名的证书来保护服务 URL，你需要包含此开关。

## <a name="automating-web-package-deployment"></a>自动 Web 包部署

在大量的企业方案中，你将想要部署 web 包作为更大的单步执行或自动的部署的一部分。 无论你选择要部署 web 包，通过运行*。 deploy.cmd*文件，或者通过直接使用 MSDeploy.exe，你可以将命令参数化并调用它们从 Microsoft Build Engine (MSBuild) 中的目标项目文件。

在联系人管理器示例解决方案中，看一看**PublishWebPackages**中 target *Publish.proj*文件。 此目标运行一次每个*。 deploy.cmd*由名为的项列表标识文件**PublishPackages**。 目标使用属性和项的元数据来生成一套完整的参数值的每个*。 deploy.cmd*文件，然后使用**Exec**任务以运行该命令。


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> 示例解决方案中，以及对自定义项目文件的常规介绍中的项目文件模型的更广泛概述，请参阅[了解项目文件](understanding-the-project-file.md)和[了解该生成过程](understanding-the-build-process.md)。


## <a name="endpoint-considerations"></a>终结点注意事项

无论是否通过运行来部署 web 包*。 deploy.cmd*文件，或者通过直接使用 MSDeploy.exe，必须指定计算机名称或你的部署的服务终结点。

如果在目标 web 服务器配置为使用 Web 部署远程代理服务的部署，你将指定为目标的目标服务 URL。


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


或者，你可以指定单独的服务器名称为目标，并 Web 部署将推断远程代理服务 URL。


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


如果在目标 web 服务器配置为使用 Web 部署处理程序的部署，你需要的 IIS Web 管理服务 (WMSvc) 的终结点地址指定为目标。 默认情况下，这将采用的形式：


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


你可以为目标的任何使用这些终结点*。 deploy.cmd*文件或直接 MSDeploy.exe。 但是，如果你想要作为非管理员用户，将部署到 Web 部署处理程序中所述[配置用于 Web 部署发布 （Web 部署处理程序） 的 Web 服务器](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)，你需要将查询字符串添加到服务终结点地址。


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


这是因为非管理员用户不具有对 IIS; 服务器级访问权限他或她只具有特定的 IIS 网站的访问权限。 在撰写本文之际，由于 bug 中 Web 发布管道 (WPP)，无法运行*。 deploy.cmd*文件使用包括查询字符串的终结点地址。 在此方案中，你需要通过直接使用 MSDeploy.exe 部署 web 包。

> [!NOTE]
> 有关 Web 部署远程代理服务和 Web 部署处理程序的详细信息，请参阅[选择用于 Web 部署的右方法](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)。 有关如何配置特定于环境的项目文件将部署到这些终结点的指南，请参阅[配置为目标环境的部署属性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。


## <a name="authentication-considerations"></a>身份验证注意事项

无论是否通过运行来部署 web 包*。 deploy.cmd*文件，或者通过直接使用 MSDeploy.exe，您需要指定身份验证类型。 Web 部署接受两个可能值： **NTLM**或**基本**。 如果指定基本身份验证，你还需要提供用户名和密码。 有需要注意当选择身份验证类型的各种因素：

- 如果你要部署到 Web 部署远程代理服务，则必须使用 NTLM 身份验证。 远程代理服务不接受基本身份验证凭据。
- 如果你正在将部署到 Web 部署处理程序中，你可以使用 NTLM 或基本身份验证。 默认设置为基本身份验证。 基本身份验证依赖于用户名和密码以明文传输，尽管你的凭据会受到保护，因为 Web 部署处理程序始终使用 SSL 加密。
- 如果你的 web 包包括一个数据库，并且 web 服务器和数据库服务器是单独的计算机，你将无法使用 NTLM 身份验证由于部署数据库[NTLM"双跃点"限制](https://go.microsoft.com/?linkid=9805120)。 你需要在你部署连接字符串中使用 SQL Server 凭据或提供给 Web 部署的基本身份验证凭据。 中更详细地介绍了此问题[将成员资格数据库部署到企业环境](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md)。

## <a name="conclusion"></a>结束语

本主题描述如何部署 web 包通过运行*。 deploy.cmd*文件或通过直接使用 MSDeploy.exe。 它解释何时可能适当情况下，每种方法，它描述如何将参数化和部署命令作为更大的单步执行或自动生成过程的一部分运行。

## <a name="further-reading"></a>其他阅读材料

有关如何创建和参数化 web 部署包的指南，请参阅[生成和打包 Web 应用程序项目](building-and-packaging-web-application-projects.md)和[Web 包部署的配置参数](configuring-parameters-for-web-package-deployment.md)。 有关如何生成和部署 web 包从 Team Foundation Server (TFS) 实例的指南，请参阅[自动 Web 部署配置 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。 有关如何自定义以及解决在部署过程的信息，请参阅[排除文件和文件夹从部署](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md)。

>[!div class="step-by-step"]
[上一页](configuring-parameters-for-web-package-deployment.md)
[下一页](deploying-database-projects.md)
