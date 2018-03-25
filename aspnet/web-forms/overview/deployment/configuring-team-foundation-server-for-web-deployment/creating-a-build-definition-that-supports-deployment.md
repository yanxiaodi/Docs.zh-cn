---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: "创建生成定义支持部署 |Microsoft 文档"
author: jrjlee
description: "如果你想要执行任何种类的生成在 Team Foundation Server (TFS) 2010年，你需要创建你的团队项目中的生成定义。 此主题 des..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: e5610753968328e5d0f1dba4cbbfed08480fd773
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-build-definition-that-supports-deployment"></a>创建生成定义支持部署
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 如果你想要执行任何种类的生成在 Team Foundation Server (TFS) 2010年，你需要创建你的团队项目中的生成定义。 本主题介绍如何在 TFS 中创建新的生成定义以及如何控制 Team Build 中的生成过程的一部分的 web 部署。


本主题窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器解决方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; 来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序Communication Foundation (WCF) 服务和数据库项目。

这些教程的核心的部署方法取决于中介绍的拆分项目文件方法[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，请在生成和部署过程控制由两个项目文件 & #x 2014年; one 包含生成适用于每种目标环境和一个包含特定于环境的生成和部署设置的说明进行操作。 在生成期间，特定于环境的项目文件合并到环境无关的项目文件中以形成一组完整的生成说明。

## <a name="task-overview"></a>任务概述

生成定义是控制 TFS 中团队项目的方式和时间进行生成的机制。 指定每个生成定义：

- 你希望生成时，Visual Studio 解决方案文件或自定义 Microsoft Build Engine (MSBuild) 项目文件等操作。
- 确定何时应执行生成的条件放置，如手动触发器，持续集成 (CI)，或者封闭签入的。
- Team Build 应向其发送生成输出，包括部署 web 包和数据库脚本等项目，项目的位置。
- 应保留每个生成的时间量。
- 在生成过程中的各种其他参数。

> [!NOTE]
> 生成定义的详细信息，请参阅[定义生成过程](https://msdn.microsoft.com/library/ms181715.aspx)。


本主题将演示如何创建使用 CI 使用，请生成定义，以便开发人员签入新的内容时触发生成。 如果生成成功，则生成服务运行将解决方案部署到测试环境的自定义项目文件。

时触发的生成，这些操作将需要发生这种情况：

- 首先，Team Build 应生成解决方案。 此过程的一部分，作为团队生成可调用 Web 发布管道 (WPP) 为每个解决方案中的 web 应用程序项目中生成 web 部署包。 Team Build 还将运行任何单元测试与解决方案相关联。
- 如果解决方案生成失败，则 Team Build 应执行任何进一步操作。 单元测试失败应视为生成失败。
- 如果解决方案生成成功，则 Team Build 应运行控制部署解决方案的自定义项目文件。 此过程的一部分，作为团队生成将会调用 Internet 信息服务 (IIS) Web 部署工具 (Web Deploy) 以在目标 web 服务器上安装打包的 web 应用程序和它将调用 VSDBCMD.exe 实用工具来运行数据库创建在目标数据库服务器上的脚本。

这说明了该过程：

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

[联系人管理器](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)示例解决方案包含自定义 MSBuild 项目文件中， *Publish.proj*，你可以运行 MSBuild 或 Team Build。 中所述[了解该生成过程](../web-deployment-in-the-enterprise/understanding-the-build-process.md)，此项目文件定义将你的 web 包和数据库部署到目标环境的逻辑。 该文件包含运行时将其在 Team Build 离开只需部署任务运行中省略的构建和打包过程的逻辑。 这是因为在自动化部署以这种方式时，将通常想要确保成功生成解决方案，并通过任何单元测试，在部署过程开始之前。

下一部分将说明如何通过创建新的生成定义来实现此过程。

> [!NOTE]
> 此过程 & #x 2014; 单一的自动化的过程生成中的测试，并将部署的解决方案 （&） #x 2014年; 可能是最适合部署以测试环境。 有关过渡和生产环境，你很多更有可能想要将内容从以前的生成，你已经验证并验证在测试环境中部署。 这种方法下一主题中所述[部署对特定生成](deploying-a-specific-build.md)。


### <a name="who-performs-this-procedure"></a>此过程的执行者是谁？

通常，TFS 管理员执行此过程。 在某些情况下，开发人员团队主管可能在 TFS 中需要负责的团队项目集合。 若要创建新的生成定义，你需要是成员的**项目集合生成管理员**组包含你的解决方案的团队项目集合。

## <a name="create-a-build-definition-for-ci-and-deployment"></a>CI 和部署创建生成定义

下一步的过程描述如何创建 CI 触发的生成定义。 如果生成成功，部署了解决方案，在自定义 MSBuild 项目文件中使用的逻辑。

**若要为 CI 和部署创建生成定义**

1. 在 Visual Studio 2010 中，在**团队资源管理器**窗口中，展开你的团队项目节点，右键单击**生成**，然后单击**新建生成定义**。

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. 上**常规**选项卡上，为生成定义指定一个名称 (例如， **DeployToTest**) 和可选描述。
3. 上**触发器**选项卡上，选择你要在其触发新的生成的条件。 例如，如果你想要生成解决方案，每次开发人员签入新代码时，将部署到测试环境，选择**持续集成**。
4. 上**生成默认值**选项卡上，在**生成输出复制到以下放置文件夹**框中，键入 drop 文件夹的通用命名约定 (UNC) 路径 (例如，  **\\TFSBUILD\Drops**)。

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > 此放置位置存储几个版本，具体取决于你配置的保留策略。 当你想要发布到过渡或生产环境的部署项目从特定生成时，这将是你可以找到它们。
5. 上**过程**选项卡上，在**生成过程文件**下拉列表中，保留**DefaultTemplate.xaml**选。 这是添加到新的所有团队项目的默认生成过程模板之一。
6. 在**生成过程参数**表，请单击中**生成的项**行，然后依次**省略号**按钮。

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. 在**生成的项**对话框中，单击**添加**。
8. 浏览到你的解决方案文件的位置，然后单击**确定**。

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. 在**生成的项**对话框中，单击**添加**。
10. 在**类型的项**下拉列表中，选择**MSBuild 项目文件**。
11. 浏览到与其控制部署过程，选择的文件，然后单击自定义项目文件的位置**确定**。

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. **生成的项**对话框现在应显示两个项。 单击 **“确定”**。

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. 上**过程**选项卡上，在**生成过程参数**表中，展开**高级**部分。
14. 在**MSBuild 参数**行中，添加任何 MSBuild 命令行参数，*任一*项目生成的需要。 在联系人管理器解决方案方案中，这些自变量是必需的：

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. 在此示例中：

    1. **DeployOnBuild = true**和**DeployTarget = 包**参数是必需的当你构建联系人管理器解决方案。 这会指示 MSBuild 创建 web 部署包生成每个 web 应用程序项目之后, 中所述[生成和打包 Web 应用程序项目](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)。
    2. **TargetEnvPropsFile**参数是必需的在生成时*Publish.proj*文件。 此属性指示的位置特定于环境的配置文件中所述[了解该生成过程](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。
16. 上**保留策略**选项卡上，配置想要根据需要保留每种类型的多少生成。
17. 单击“保存” 。

## <a name="queue-a-build"></a>将生成排入队列

此时，你已创建至少一个新的生成定义。 你定义的生成过程现在将根据你在生成定义中指定的触发器运行。

如果已配置你的生成定义，以使用 CI，你可以通过两种方式来测试你的生成定义：

- 签入到团队项目来触发自动生成一些内容。
- 手动对生成进行排队。

**若要手动对生成进行排队**

1. 在**团队资源管理器**窗口中，右键单击生成定义，并依次**使新生成入队**。

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. 在**队列生成**对话框中，查看生成属性中，，然后单击**队列**。

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

若要查看的进度和结果的生成 （&） #x 2014年; 而不考虑它是否触发手动或自动和 #x 2014; 双击生成定义中的**团队资源管理器**窗口。 这将打开**生成资源管理器**选项卡。

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

从这里，您可以解决失败的生成。 如果双击的单个生成后，你可以查看摘要信息和详细的日志文件通过单击。

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

可以使用此信息来排查生成失败，并解决任何问题，然后再尝试另一个生成。

> [!NOTE]
> 执行部署逻辑的生成很可能之前已在目标环境所需的任何权限授予生成服务器失败。 有关详细信息，请参阅[配置团队生成部署的权限](configuring-permissions-for-team-build-deployment.md)。


## <a name="monitor-the-build-process"></a>监视器生成过程

TFS 提供广泛的功能来帮助你监视生成过程。 例如，TFS 可以向你发送一封电子邮件或生成完成后，在任务栏通知区域显示警报。 有关详细信息，请参阅[运行和监视器生成](https://msdn.microsoft.com/library/ms181721.aspx)。

## <a name="conclusion"></a>结束语

本主题描述如何在 TFS 中创建生成定义。 因此，生成过程运行只要开发人员将签入到团队项目的内容，将为 CI 使用，请配置生成定义。 生成定义执行自定义的 MSBuild 项目文件，以将 web 包和数据库脚本部署到目标服务器环境。

为了使自动部署才能成功生成过程的一部分，你将需要授予对生成服务帐户的目标 web 服务器和目标数据库服务器上的合适权限。 在本教程的最后一个主题[配置团队生成部署的权限](configuring-permissions-for-team-build-deployment.md)，介绍如何标识和配置用于从 Team Build 服务器的自动部署所需的权限。

## <a name="further-reading"></a>其他阅读材料

创建生成定义的详细信息，请参阅[创建基本生成定义](https://msdn.microsoft.com/library/ms181716.aspx)和[定义生成过程](https://msdn.microsoft.com/library/ms181715.aspx)。 队列生成的更多指南，请参阅[对生成进行排队](https://msdn.microsoft.com/library/ms181722.aspx)。

>[!div class="step-by-step"]
[上一页](configuring-a-tfs-build-server-for-web-deployment.md)
[下一页](deploying-a-specific-build.md)
