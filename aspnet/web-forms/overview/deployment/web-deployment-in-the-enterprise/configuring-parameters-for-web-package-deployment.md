---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: "为 Web 包部署中配置参数 |Microsoft 文档"
author: jrjlee
description: "本主题介绍如何设置参数的值，如 Internet 信息服务 (IIS) web 应用程序名称、 连接字符串和服务终结点..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: 12a4ba8ad30df43e7192500ad4514dfa9679f899
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="configuring-parameters-for-web-package-deployment"></a>为 Web 包部署中配置参数
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何在 web 包部署到远程 IIS web 服务器时设置参数的值，如 Internet 信息服务 (IIS) web 应用程序名称、 连接字符串和服务终结点。


生成的 web 应用程序项目、 生成和打包过程时生成三个关键文件：

- A *[项目名称].zip*文件。 这是你的 web 应用程序项目的 web 部署包。 此程序包包含所有程序集、 文件、 数据库脚本和重新创建远程 IIS web 服务器上的 web 应用程序所需资源。
- A *[项目名称].deploy.cmd*文件。 这包含一组将你的 web 部署包发布到远程的 IIS web 服务器的 Web Deploy (MSDeploy.exe) 的参数化命令。
- A *[项目名称]。SetParameters.xml*文件。 这提供了一套到 MSDeploy.exe 命令的参数值。 你可以更新此文件中的值，并将其传递给 Web 部署作为命令行参数部署 web 包时。

> [!NOTE]
> 生成和打包过程的详细信息，请参阅[生成和打包 Web 应用程序项目](building-and-packaging-web-application-projects.md)。


*SetParameters.xml*文件动态生成从你的 web 应用程序项目文件和你的项目中任何配置文件。 当生成并打包你的项目，Web 发布管道 (WPP) 将自动检测大量倾向于部署环境，如目标 IIS web 应用程序和任何数据库连接字符串之间发生更改的变量。 这些值自动参数化 web 部署包中，添加到*SetParameters.xml*文件。 例如，如果你添加的连接字符串*web.config*文件在 web 应用程序项目中，生成过程将检测此更改，并将添加到一个条目*SetParameters.xml*文件相应地。

在许多情况下，此自动参数化将足够。 但是，如果你的用户需要更改其他设置之间部署环境，如应用程序设置或服务终结点 Url，你需要告诉 WPP 参数化部署包中的这些值并将相应的项添加到*SetParameters.xml*文件。 以下各节说明如何执行此操作。

### <a name="automatic-parameterization"></a>自动参数化

当生成和打包 web 应用程序时，WPP 将自动参数化以下操作：

- 目标 IIS web 应用程序路径和名称。
- 中的任何连接字符串你*web.config*文件。
- 你将添加到任何数据库的连接字符串**打包/发布 SQL**在项目属性页中的选项卡。

例如，如果你打算生成并打包[联系人管理器](the-contact-manager-solution.md)而无需触摸以任何方式，WPP 的参数化过程的示例解决方案也会生成此*ContactManager.Mvc.SetParameters.xml*文件：


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


这种情况下：

- **IIS Web 应用程序名称**参数是你想要部署 web 应用程序的 IIS 路径。 默认值取自**打包/发布 Web**在项目属性页中的页。
- **ApplicationServices Web.config 连接字符串**参数从生成**添加 connectionStrings/**中的元素*web.config*文件。 它表示应用程序应使用联系成员资格数据库的连接字符串。 此处将提供的值替换到部署*web.config*文件。 默认值取自预部署*web.config*文件。

WPP 还使它生成的部署包中的这些属性进行参数化。 当你安装的部署包时，你可以为这些属性提供值。 如果你安装包手动通过 IIS 管理器中所述[手动安装 Web 包](manually-installing-web-packages.md)，安装向导会提示您提供任何参数的值。 当你安装使用远程包*。 deploy.cmd*文件，如下所述[部署 Web 包](deploying-web-packages.md)，Web 部署将此查找*SetParameters.xml*到文件提供参数值。 你可以编辑中的值*SetParameters.xml*文件手动，或可以自定义自动化的生成和部署过程的一部分的文件。 本主题中后面的更详细地描述了此过程。

### <a name="custom-parameterization"></a>自定义参数化

在更复杂的部署方案中，通常需要在部署你的项目之前，参数化的其他属性。 通常情况下，你应参数化的任何属性和将改变目标环境之间的设置。 这些错误可能包括：

- 服务中的终结点*web.config*文件。
- 应用程序设置中的*web.config*文件。
- 你希望的任何其他声明性属性提示用户指定。

参数化这些属性的最简单方法是添加*parameters.xml*到你的 web 应用程序项目的根文件夹的文件。 例如，解决方案中的联系人管理器、 ContactManager.Mvc 项目包括*parameters.xml*的根文件夹中的文件。

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

如果打开此文件，你将看到它包含单个**参数**条目。 该条目使用 XML 路径语言 (XPath) 查询来查找并参数化中的 ContactService Windows Communication Foundation (WCF) 服务的终结点 URL *web.config*文件。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


除了部署包中的终结点 URL 中参数化，WPP 还添加到一个对应的条目*SetParameters.xml*与部署包一起获取生成的文件。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


如果你手动安装的部署包，IIS 管理器将提示你自动参数化的属性旁边的服务终结点地址。 当你通过运行安装了部署包*。 deploy.cmd*文件，你可以编辑*SetParameters.xml*文件服务终结点地址以及为值提供的值自动参数化的属性。

有关如何创建的完整详细信息*parameters.xml*文件，请参阅[如何： 安装到配置部署设置包时使用参数](https://msdn.microsoft.com/library/ff398068.aspx)。 名为的过程**要用于 Web.config 文件设置的部署参数**提供分步说明。

## <a name="modifying-the-setparametersxml-file"></a>修改 SetParameters.xml 文件

如果你计划手动部署 web 应用程序包 & #x 2014年; 通过运行*。 deploy.cmd*文件或通过从命令行 （&） #x 2014; 运行 MSDeploy.exe 没有要阻止您手动编辑内容*SetParameters.xml*之前部署的文件。 但是，如果你正在对企业级解决方案，你可能需要将 web 应用程序包部署更大、 自动生成和部署过程的一部分。 在此方案中，你需要 Microsoft Build Engine (MSBuild) 修改*SetParameters.xml*为你的文件。 你可以执行此操作使用 MSBuild **XmlPoke**任务。

[联系人管理器示例解决方案](the-contact-manager-solution.md)阐释了此过程。 下面的代码示例进行了编辑，显示与此示例相关的详细信息。

> [!NOTE]
> 示例解决方案中，以及对自定义项目文件的常规介绍中的项目文件模型的更广泛概述，请参阅[了解项目文件](understanding-the-project-file.md)和[了解该生成过程](understanding-the-build-process.md)。


首先，感兴趣的参数值指特定于环境的项目文件中的属性 (例如， *Env Dev.proj*)。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> 有关如何自定义服务器环境的特定于环境的项目文件的指南，请参阅[配置为目标环境的部署属性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。


接下来， *Publish.proj*文件导入这些属性。 因为每个*SetParameters.xml*文件关联*。 deploy.cmd*文件和我们最终需要调用每个项目文件*。 deploy.cmd*文件，项目文件将创建 MSBuild*项*每个*。 deploy.cmd*文件，并定义为感兴趣的属性*项元数据*。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


这种情况下：

- **ParametersXml**元数据的值指示的位置*SetParameters.xml*文件。
- **IisWebAppName**值是你想要部署 web 应用程序的 IIS 路径。
- **MembershipDBConnectionString**值是成员资格数据库的连接字符串和**MembershipDBConnectionName**值是**名称**属性中的相应参数*SetParameters.xml*文件。
- **ServiceEndpointValue**值是目标服务器上的 WCF 服务的终结点地址和**ServiceEndpointParamName**值中的相应参数的名称属性*SetParameters.xml*文件。

最后，在*Publish.proj*文件， **PublishWebPackages**目标使用**XmlPoke**任务要修改这些值在*SetParameters.xml*文件。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


你将会发现，每个**XmlPoke**任务指定四个属性值：

- **XmlInputPath**属性告知任务在哪里可以找到你想要修改的文件。
- **查询**属性是一个标识你想要更改的 XML 节点的 XPath 查询。
- **值**属性是你想要插入到所选的 XML 节点的新值。
- **条件**属性是该任务应在其运行或未运行的条件。 在这些情况下，该条件可确保不尝试插入 null 或为空值*SetParameters.xml*文件。

## <a name="conclusion"></a>结束语

本主题所述的角色*SetParameters.xml*文件并且说明了在生成 web 应用程序项目时的生成方式。 它说明如何通过添加参数化的其他设置*parameters.xml*到你的项目文件。 它还介绍了如何修改*SetParameters.xml*文件作为更大、 自动生成过程，通过使用的一部分**XmlPoke**项目文件中的任务。

下一主题[部署 Web 包](deploying-web-packages.md)，描述你如何可以部署 web 包通过运行*。 deploy.cmd*文件，或者通过使用 MSDeploy.exe 命令直接。 在这两种情况下，你可以指定你*SetParameters.xml*文件作为部署参数。

## <a name="further-reading"></a>其他阅读材料

有关如何创建 web 包的信息，请参阅[生成和打包 Web 应用程序项目](building-and-packaging-web-application-projects.md)。 有关如何实际部署 web 包的指南，请参阅[部署 Web 包](deploying-web-packages.md)。 有关如何创建的分步演练*parameters.xml*文件，请参阅[如何： 安装到配置部署设置包时使用参数](https://msdn.microsoft.com/library/ff398068.aspx)。

有关 Web 部署中的参数化的更多常规信息，请参阅[Web 操作中的部署参数化](https://go.microsoft.com/?linkid=9805119)（博客文章）。

>[!div class="step-by-step"]
[上一页](building-and-packaging-web-application-projects.md)
[下一页](deploying-web-packages.md)
