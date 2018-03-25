---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: 拍摄 Web 应用程序脱机与 Web 部署 |Microsoft 文档
author: jrjlee
description: 本主题介绍如何采用脱机用于自动部署使用 Internet 信息服务 (IIS) Web 部署的持续时间的 web 应用程序...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: 1c262ec7b834107524a18c6552b171f731452c91
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="taking-web-applications-offline-with-web-deploy"></a>拍摄 Web 应用程序脱机与 Web 部署
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何采用脱机用于自动部署使用 Internet 信息服务 (IIS) Web 部署工具 （Web 部署） 的持续时间的 web 应用程序。 浏览到 web 应用程序的用户将重定向到*应用\_offline.htm*文件之前部署已完成。


本主题窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器解决方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; 来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序Communication Foundation (WCF) 服务和数据库项目。

这些教程的核心的部署方法取决于中介绍的拆分项目文件方法[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在其中生成过程控制由两个项目文件 （&） #x 2014; 一个包含生成适用于每种目标环境和一个包含特定于环境的生成和部署设置的说明。 在生成期间，特定于环境的项目文件合并到环境无关的项目文件中以形成一组完整的生成说明。

## <a name="task-overview"></a>任务概述

在许多方案，你将想要使 web 应用程序脱机时对相关的组件，如数据库或 web 服务进行更改。 通常情况下，在 IIS 和 ASP.NET 中，你完成此操作通过将名为的文件*应用\_offline.htm*的 IIS 网站或 web 应用程序的根文件夹中。 *应用\_offline.htm*文件是一个标准的 HTML 文件，通常包含一种简单消息，通知用户该站点是否由于维护而暂时不可用。 虽然*应用\_offline.htm*文件是否存在于网站的根文件夹，IIS 将自动任何将请求重定向到文件。 当你已进行更新时，也就删除*应用\_offline.htm*文件和网站恢复照常为请求提供服务。

当使用 Web 部署来执行自动进行的或单步执行到目标环境的部署时，你可能想要合并添加和删除*应用\_offline.htm*文件到你的部署过程。 若要执行此操作，你将需要完成这些高级任务：

- 在 Microsoft Build Engine (MSBuild) 项目文件中用于控制在部署过程，创建的 MSBuild 目标复制*应用\_offline.htm*到之前的任何部署任务在目标服务器的文件开始。
- 添加另一个中移除的 MSBuild 目标*应用\_offline.htm*从目标服务器时所有的部署任务都已完成的文件。
- 在 web 应用程序项目中，创建*。 wpp.targets*文件，它可确保*应用\_offline.htm* Web 部署调用时，将文件添加到部署包。

本主题将演示如何执行这些过程。 任务和本主题中的演练假定你已创建了包含至少一个 web 应用程序项目中，一个解决方案和使用自定义项目文件来控制部署过程中所述[中的 Web 部署企业](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。 或者，可以使用[联系人管理器](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)示例解决方案遵循主题中的示例。

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>添加应用\_到 Web 应用程序项目的脱机文件

你需要完成的第一个任务是添加*应用\_脱机*文件到你的 web 应用程序项目：

- 若要防止文件干扰开发过程 （不希望永久脱机使应用程序），你应将其称为以外*应用\_offline.htm*。 例如，你无法将文件*应用\_脱机 template.htm*。
- 若要防止文件作为部署的是，你应将生成操作设置为**无**。

**添加一个应用程序\_到 web 应用程序项目的脱机文件**

1. 在 Visual Studio 2010 中打开你的解决方案。
2. 在**解决方案资源管理器**窗口中，右键单击你的 web 应用程序项目，指向**添加**，然后单击**新项**。
3. 在**添加新项**对话框中，选择**HTML 页**。
4. 在**名称**框中，键入**应用\_脱机 template.htm**，然后单击**添加**。

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. 添加一些简单的 HTML，以通知用户应用程序已不可用，，然后保存该文件。 不包括任何服务器端标记 (例如，任何带有前缀的标记"asp:")。 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. 在**解决方案资源管理器**窗口中，右键单击新的文件，并依次**属性**。
7. 在**属性**窗口，请在**生成操作**行中，选中**无**。

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>部署和删除应用\_脱机文件

下一步是修改你部署逻辑来将文件复制到目标服务器部署过程的开头和末尾删除它。

> [!NOTE]
> 下一步过程假设你正在使用自定义 MSBuild 项目文件来控制你的部署过程中所述[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。 如果你正在部署直接从 Visual Studio，你将需要使用不同的方法。 Sayed Ibrahim Hashimi 介绍中的一个此类方法[如何采用您 Web 应用程序脱机期间发布](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx)。


若要部署*应用\_脱机*文件到目标的 IIS 网站，你需要调用 MSDeploy.exe 使用[Web 部署**contentPath**提供程序](https://technet.microsoft.com/library/dd569034(WS.10).aspx)。 **ContentPath**提供程序支持的物理目录路径和 IIS 网站或应用程序路径，使其同步 Visual Studio 项目文件夹和 IIS web 应用程序之间的文件的最佳选择。 若要部署该文件，MSDeploy 命令应类似如下：


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


若要从目标站点，在部署过程结束时删除该文件，MSDeploy 命令应类似如下：


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


若要生成和部署过程的一部分自动运行这些命令，你需要将其集成到自定义 MSBuild 项目文件。 下一步的过程描述如何执行此操作。

**若要部署和删除应用\_脱机文件**

1. 在 Visual Studio 2010 中，打开控制您的部署过程的 MSBuild 项目文件。 在[联系人管理器](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)示例解决方案中，这是*Publish.proj*文件。
2. 根目录中**项目**元素，创建一个新**PropertyGroup**元素存储变量*应用\_脱机*部署：

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. **SourceRoot**属性中其他位置定义*Publish.proj*文件。 它指示源内容相对于当前路径 （&） #x 2014年; 换句话说，位置相对的根文件夹的位置*Publish.proj*文件。
4. **ContentPath**提供程序将不会接受相对文件路径，因此你需要获取到你的源代码文件的绝对路径，然后将其部署。 你可以使用[ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx)任务以执行此操作。
5. 添加新**目标**元素名为**GetAppOfflineAbsolutePath**。 在此目标使用**ConvertToAbsolutePath**任务来获取到的绝对路径*应用\_脱机模板*项目文件夹中的文件。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. 此目标中，采用的相对路径*应用\_脱机模板*项目文件夹中的文件并将其保存到新的属性为绝对文件路径。 **BeforeTargets**属性指定你想要执行之前此目标**DeployAppOffline**目标，你将在下一步中创建。
7. 添加名为的新目标**DeployAppOffline**。 在此目标内调用 MSDeploy.exe 命令以部署你*应用\_脱机*到目标 web 服务器的文件。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. 在此示例中， **ContactManagerIisPath**属性在项目文件中其他位置定义。 这是只需一个 IIS 应用程序路径的形式*[IIS 网站名称] / [应用程序名称]*。 目标中包括条件使用户能够切换*应用\_脱机*部署打开或关闭通过更改属性值或提供命令行参数。
9. 添加名为的新目标**DeleteAppOffline**。 在此目标内调用 MSDeploy.exe 命令删除你*应用\_脱机*从目标 web 服务器的文件。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. 最后一项任务是在你的项目文件的执行过程中调用这些新的目标在适当的位置。 可以通过多种方式来执行此操作。 例如，在*Publish.proj*文件， **FullPublishDependsOn**属性指定必须执行的目标的列表在排序时**FullPublish**默认调用目标。
11. 修改 MSBuild 项目文件调用**DeployAppOffline**和**DeleteAppOffline**在发布过程中适当点的目标。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

当你运行自定义 MSBuild 项目文件，*应用\_脱机*文件将成功生成后立即部署到服务器。 它将然后从服务器删除后部署的所有任务都都已完成。

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>添加应用\_到部署包的脱机文件

根据你如何配置目标 IIS web 应用程序 & #x 2014; 你的部署，任何现有内容喜欢*应用\_offline.htm*文件 （&) #x 2014; 部署 web 时可能会自动删除到目标的包。 若要确保*应用\_offline.htm*文件仍保持就地部署的持续时间内，你需要将该文件中的 web 部署包本身此外包括到部署直接开头的文件部署过程中。

- 如果你已按照本主题中前面的任务，你将已添加*应用\_offline.htm*到你 web 应用程序项目的其他文件名的文件 (我们使用*应用\_脱机 template.htm*) 并且你将安装的生成操作**无**。 不需要以防止干扰开发和调试中的文件进行这些更改。 因此，你需要自定义以确保在打包过程*应用\_offline.htm* web 部署包中包含了文件。

Web 发布管道 (WPP) 使用名为的项列表**FilesForPackagingFromProject**生成的文件应包含在 web 部署包的列表。 你可以将您自己的项添加到此列表自定义 web 包的内容。 若要执行此操作，你需要完成以下高级步骤：

1. 创建一个名为的自定义项目文件*[项目名称].wpp.targets*项目文件所在的文件夹中。

    > [!NOTE]
    > *。 Wpp.targets*文件需要在与你的 web 应用程序项目文件 （&） #x 2014; 位于同一文件夹中转等*ContactManager.Mvc.csproj*（& a) 与任何 #x 2014; 而不是在同一个文件夹管理生成和部署过程使用自定义项目文件。
2. 在*。 wpp.targets*文件中，创建新的 MSBuild 目标执行*之前* **CopyAllFilesToSingleFolderForPackage**目标。 这是生成要在包中包括的事项列表的 WPP 目标。
3. 在新的目标中，创建**ItemGroup**元素。
4. 在**ItemGroup**元素中，添加**FilesForPackagingFromProject**项并指定*应用\_offline.htm*文件。

*。 Wpp.targets*文件应类似如下：


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


这些是在此示例中注意的要点：

- **BeforeTargets**属性将插入它应立即之前执行通过指定 WPP 到此目标**CopyAllFilesToSingleFolderForPackage**目标。
- **FilesForPackagingFromProject**项使用**DestinationRelativePath**要重命名的文件的元数据值*应用\_脱机 template.htm*到*应用\_offline.htm*被添加到列表。

下一个过程演示如何添加此*。 wpp.targets*到 web 应用程序项目的文件。

**若要添加。 web 部署包的 wpp.targets 文件**

1. 在 Visual Studio 2010 中打开你的解决方案。
2. 在**解决方案资源管理器**窗口中，右键单击你的 web 应用程序项目节点 (例如， **ContactManager.Mvc**)，指向**添加**，然后单击**新项**。
3. 在**添加新项**对话框中，选择**XML 文件**模板。
4. 在 **名称** 框中，键入 *[项目名称]***.wpp.targets** (例如， **ContactManager.Mvc.wpp.targets**)，然后单击 **添加**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > 如果你将新项添加到项目的根节点时，会在项目文件所在的文件夹中创建文件。 可以通过在 Windows 资源管理器中打开文件夹对此进行验证。
5. 在文件中，添加前面所述的 MSBuild 标记。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. 保存并关闭*[项目名称].wpp.targets*文件。

下一次你生成和包你的 web 应用程序项目，将自动检测 WPP *。 wpp.targets*文件。 *应用\_脱机 template.htm*将作为生成的 web 部署包中包含文件*应用\_offline.htm*。

> [!NOTE]
> 如果你的部署失败，*应用\_offline.htm*文件将保留在原位和你的应用程序将保持脱机状态。 这通常是所需的行为。 若要使你的应用程序重新联机，你可以删除*应用\_offline.htm*从你的 web 服务器的文件。 或者，如果更正任何错误，运行成功的部署，*应用\_offline.htm*将删除文件。


## <a name="conclusion"></a>结束语

本主题描述如何采用通过发布的 web 应用程序脱机的持续时间的部署，*应用\_offline.htm*到目标服务器在部署过程中启动文件并将其在删除结束日期。 它还介绍了如何包括*应用\_offline.htm* web 部署包中的文件。

## <a name="further-reading"></a>其他阅读材料

打包和部署过程的详细信息，请参阅[生成和打包 Web 应用程序项目](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)， [Web 包部署的配置参数](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)，和[部署 Web 包](../web-deployment-in-the-enterprise/deploying-web-packages.md)。

如果发布 web 应用程序直接从 Visual Studio 中，而不使用这些教程中所述的自定义 MSBuild 项目文件方法，你将需要使用稍有不同的方法来执行你的应用程序脱机在发布过程过程。 有关详细信息，请参阅[如何在发布过程中执行 web 应用程序脱机](https://go.microsoft.com/?linkid=9805135)（博客文章）。

>[!div class="step-by-step"]
[上一页](excluding-files-and-folders-from-deployment.md)
[下一页](running-windows-powershell-scripts-from-msbuild-project-files.md)
