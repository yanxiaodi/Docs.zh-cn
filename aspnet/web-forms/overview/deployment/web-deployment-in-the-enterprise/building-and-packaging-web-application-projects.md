---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: "生成和打包 Web 应用程序项目 |Microsoft 文档"
author: jrjlee
description: "如果你想要将 web 应用程序项目部署到远程服务器环境，您的首要任务是生成项目并生成 web 部署 packa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: c05f725c9e6b493a6af8f5b5d20dbc9ff73a1ef8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="building-and-packaging-web-application-projects"></a>生成和打包 Web 应用程序项目
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 如果你想要将 web 应用程序项目部署到远程服务器环境，您的首要任务是生成项目并生成 web 部署包。 本主题介绍如何生成过程适用于 web 应用程序项目。 具体而言，它还说明了：
> 
> - 如何 Web 发布管道 (WPP) 扩展生成过程，包括部署功能。
> - 如何 Internet 信息服务 (IIS) Web 部署工具 （Web 部署） 将启用 web 应用程序到部署包。
> - 如何生成和打包进程的工作原理以及创建哪些文件。


在 Visual Studio 2010 中，WPP 支持 web 应用程序项目的生成和部署过程。 WPP 提供扩展 MSBuild 的功能并使它能够使用 Web 部署集成的 Microsoft Build Engine (MSBuild) 目标的一的组。 在 Visual Studio 中，您可以看到的属性页上的此扩展的功能，web 应用程序项目。 **打包/发布 Web**页上，与一起**打包/发布 SQL**页上，使你能够配置生成过程完成后，为部署打包你的 web 应用程序项目是如何。

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>WPP 是如何工作的？

如果你需要在一看项目文件的 C# 的基于的 web 的应用程序项目，你可以看到它将两个.targets 文件导入。


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]


第一个**导入**语句是普遍适用于所有 Visual C# 项目。 此文件， *Microsoft.CSharp.targets*，包含目标和特定的 Visual C# 的任务。 例如，C# 编译器 (**Csc**) 此处调用任务。 *Microsoft.CSharp.targets*反过来文件导入*Microsoft.Common.targets*文件。 这将定义目标所共有的所有项目，如**生成**，**重新生成**，**运行**，**编译**，和**清理**. 第二个**导入**语句是特定于 web 应用程序项目。 *Microsoft.WebApplication.targets*反过来文件导入*Microsoft.Web.Publishing.targets*文件。 *Microsoft.Web.Publishing.targets*文件实质上是*是*WPP。 它定义目标，如**包**和**MSDeployPublish**，调用 Web Deploy 以完成不同的部署任务。

若要了解如何使用这些其他的目标，在联系人管理器示例解决方案中，打开*Publish.proj*文件并看一看**BuildProjects**目标。


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]


此目标使用**MSBuild**任务，以生成各种项目。 请注意**DeployOnBuild**和**DeployTarget**属性：

- **DeployOnBuild = true**属性本质上意味着"我想要在生成成功完成时执行的其他目标。"
- **DeployTarget**属性标识你想时要执行的目标的名称**DeployOnBuild**属性等于**true**。 在这种情况下，你要指定你想要执行的 MSBuild**包**后生成项目的目标。

**包**中定义目标*Microsoft.Web.Publishing.targets*文件。 实质上，此目标中，采用你的 web 应用程序项目的生成输出，并将其转换成可以发布到 IIS web 服务器的 web 部署包。

> [!NOTE]
> 若要查看的项目文件 (例如， *ContactManager.Mvc.csproj*) 在 Visual Studio 2010 中，你首先需要卸载你的解决方案中的项目。 在**解决方案资源管理器**窗口中，右键单击项目节点，然后单击**卸载项目**。 再次右键单击项目节点，并依次**编辑***[项目文件]*)。 项目文件将在原始 XML 形式中打开。 请记住完成后，重新加载项目。  
> 有关 MSBuild 目标，任务的详细信息和**导入**语句，请参阅[了解项目文件](understanding-the-project-file.md)。 项目文件和 WPP 的更深入介绍，请参阅[内 Microsoft 生成引擎： 使用 MSBuild 和 Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi 和 William Bartholomew，ISBN: 978-0-7356-4524-0。


## <a name="what-is-a-web-deployment-package"></a>Web 部署包是什么？

当生成和部署 web 应用程序项目中，通过使用 Visual Studio 2010 或直接使用 MSBuild 时，最终结果是通常*web 部署包*。 Web 部署包是一个.zip 文件。 它包含的所有内容 IIS 和 Web 部署需重新创建 web 应用程序，包括：

- Web 应用程序，包括内容、 资源文件、 配置文件、 JavaScript 和级联样式表 (CSS) 资源和等等的已编译的输出。
- 程序集为你的 web 应用程序项目和任何引用解决方案中的项目。
- 若要生成你正在与 web 应用程序将部署任何数据库的 SQL 脚本。

一旦已生成的 web 部署包，你可以将其发布到 IIS web 服务器以各种方式。 例如，你可以通过将其部署远程面向 Web 部署远程代理服务或 Web 部署处理程序在目标 web 服务器上，或者你可以使用 IIS 管理器中手动导入在目标 web 服务器上的包。 有关部署到这些方法的详细信息，请参阅[选择用于 Web 部署的右方法](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)。

## <a name="how-does-the-build-process-work"></a>生成过程是如何工作的？

下面的示例演示当生成并打包 web 应用程序项目时，会发生什么情况：

![](building-and-packaging-web-application-projects/_static/image2.png)

生成过程生成的 web 应用程序项目时，生成一个名为文件*[项目名称]。SourceManifest.xml*。 以及项目文件和生成输出中，这*。SourceManifest.xml*文件告知 Web 部署它需要在 web 部署包中包括。 使用这些输入，Web 部署生成名为 web 部署包*[项目名称].zip*。

Web 部署包，以及生成过程生成可以帮助你使用包的两个文件：

- *。 Deploy.cmd*文件包括一组将你的 web 部署包发布到远程的 IIS web 服务器的 Web Deploy (MSDeploy.exe) 的参数化命令。 运行*。 deploy.cmd*文件，结合适当的参数，通常提供一种更快，并且变得更容易替代手动构造 MSDeploy.exe 命令自己。
- *SetParameters.xml*文件提供了一套到 MSDeploy.exe 命令的参数值。 这些值包括之类的 IIS web 应用程序到你想要部署包的任何服务终结点的值和连接字符串中定义的名称的属性*web.config*文件和任何部署属性在项目属性页上定义的值。

*SetParameters.xml*文件是管理部署过程的关键。 此文件是根据你的 web 应用程序项目的内容在动态生成的。 例如，如果你添加的连接字符串你*web.config*文件，生成过程将自动检测连接字符串，相应地，参数化部署并创建中的条目*SetParameters.xml*文件，以便可以修改连接字符串作为部署过程的一部分。 下一主题[Web 包部署的配置参数](configuring-parameters-for-web-package-deployment.md)、 介绍的更详细地此文件的角色和介绍您在其中可以修改它生成和部署过程的不同方法。

> [!NOTE]
> 在 Visual Studio 2010 中，WPP 不支持预编译之前打包 web 应用程序中的页。 下一版本的 Visual Studio 和 WPP 将包括能够预编译将 web 应用程序作为打包选项。


## <a name="conclusion"></a>结束语

本主题提供有关在 Visual Studio 2010 中的 web 应用程序项目的生成和打包过程的概述。 它描述 WPP 如何允许调用 MSBuild，从 Web 部署命令，并且它说明了如何生成和打包进程的工作原理。

一旦你已创建 web 部署包下, 一步是将其部署。 有关这方面的详细信息，请参阅[Web 包部署的配置参数](configuring-parameters-for-web-package-deployment.md)和[部署 Web 包](deploying-web-packages.md)。

## <a name="further-reading"></a>其他阅读材料

本教程中中的下一步主题[Web 包部署的配置参数](configuring-parameters-for-web-package-deployment.md)和[部署 Web 包](deploying-web-packages.md)，提供有关如何使用你已创建的 web 包的指南。 在本系列中的最后一个教程[高级企业级 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)，提供有关如何自定义以及解决在打包过程的指导。

项目文件和 WPP 的更深入介绍，请参阅[内 Microsoft 生成引擎： 使用 MSBuild 和 Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi 和 William Bartholomew，ISBN: 978-0-7356-4524-0。

>[!div class="step-by-step"]
[上一页](understanding-the-build-process.md)
[下一页](configuring-parameters-for-web-package-deployment.md)
