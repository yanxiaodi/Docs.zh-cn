---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: "将特定的生成部署 |Microsoft 文档"
author: jrjlee
description: "本主题介绍如何部署 web 包和新的目标位置，如过渡或生产环境的流程图从特定的上一个生成数据库脚本..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: be1000f0cbc2f509f5014789c2bc47ce2b12fb2f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="deploying-a-specific-build"></a>将特定的生成部署
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何部署 web 包和数据库脚本从特定的上一个生成到新的目标，如过渡或生产环境。


本主题窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器解决方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; 来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序Communication Foundation (WCF) 服务和数据库项目。

这些教程的核心的部署方法取决于中介绍的拆分项目文件方法[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，请在生成和部署过程控制由两个项目文件 & #x 2014年; one 包含生成适用于每种目标环境和一个包含特定于环境的生成和部署设置的说明进行操作。 在生成期间，特定于环境的项目文件合并到环境无关的项目文件中以形成一组完整的生成说明。

## <a name="task-overview"></a>任务概述

到目前为止，此教程的组中的主题具有侧重于如何生成、 打包和部署 web 应用程序和数据库的单步一部分或自动化流程。 但是，在某些常见的情况下，你将想要从在放置文件夹中生成的列表中选择你部署的资源。 换而言之，最新版本可能不是你想要部署的生成。

请考虑上一主题中描述的持续集成 (CI) 情况[创建生成定义，支持部署](creating-a-build-definition-that-supports-deployment.md)。 你已创建生成定义在 Team Foundation Server (TFS) 2010年。 每次一名开发人员将代码签入 TFS，Team Build 将生成你的代码、 生成过程的一部分中创建 web 包和数据库脚本，运行任何单元测试，和将你的资源部署到测试环境。 具体取决于在创建生成定义时配置的保留策略，TFS 将保留一定数量的以前的版本。

![](deploying-a-specific-build/_static/image1.png)

现在，假设执行了验证和验证针对以下方法之一的测试生成在测试环境，并已准备好应用程序部署到过渡环境。 在此期间，开发人员可能签入新代码。 你不想要重新生成解决方案并将其部署到过渡环境，并且你不想要将最新版本部署到过渡环境。 相反，你想要部署你已验证，并验证测试服务器上的特定生成。

若要实现此目的，你需要告诉在何处查找 web 包和特定生成生成数据库脚本的 Microsoft Build Engine (MSBuild)。

## <a name="overriding-the-outputroot-property"></a>重写 OutputRoot 属性

在[示例解决方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)、 *Publish.proj*文件声明一个名为属性**OutputRoot**。 顾名思义，这是包含生成过程生成的所有内容的根文件夹。 在*Publish.proj*文件，你可以看到， **OutputRoot**属性引用的所有部署资源的根位置。

> [!NOTE]
> **OutputRoot**是常用的属性名称。 Visual C# 和 Visual Basic 项目文件还声明此属性来存储所有生成输出的根位置。


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


如果你想项目文件来部署 web 包和数据库脚本从的不同位置 （&） #x 2014年; 类似的输出的以前的 TFS 生成 （&） #x 2014; 只需重写**OutputRoot**属性。 在 Team Build 服务器上，你应设置为相关的生成文件夹的属性值。 如果你在从命令行运行 MSBuild，则可以指定的值**OutputRoot**作为命令行自变量：


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


在实践中，但是，你还想要跳过**生成**目标 （&) #x 2014; 在生成你的解决方案，如果你不打算使用的生成输出中没有点。 你可以通过指定你想要从命令行执行的目标来执行此操作：


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


但是，在大多数情况下，你将需要为 TFS 生成定义，生成你的部署逻辑。 这使用户与**生成排队**触发到的 TFS 服务器的连接任何 Visual Studio 安装中部署的权限。

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>创建生成定义以部署特定生成

下一步的过程描述如何创建生成定义，使用户触发器部署到过渡环境中使用单个命令可以。

在这种情况下，你不希望生成定义以实际生成的任何内容 （& a) #x 2014年; 你只是希望它以在你的自定义项目文件中执行的部署逻辑。 *Publish.proj*文件包含跳过的条件逻辑**生成**面向如果在 Team Build 中运行该文件。 这是通过评估内置**BuildingInTeamBuild**属性，将自动设置为**true**如果你在 Team Build 中运行你的项目文件。 因此，你可以跳过生成过程，只需运行要部署的现有生成的项目文件。

**若要创建生成定义，若要手动触发部署**

1. 在 Visual Studio 2010 中，在**团队资源管理器**窗口中，展开你的团队项目节点，右键单击**生成**，然后单击**新建生成定义**。

    ![](deploying-a-specific-build/_static/image2.png)
2. 上**常规**选项卡上，为生成定义指定一个名称 (例如， **DeployToStaging**) 和可选描述。
3. 上**触发器**选项卡上，选择**手动 – 签入，不会触发新的生成**。
4. 上**生成默认值**选项卡上，在**生成输出复制到以下放置文件夹**框中，键入 drop 文件夹的通用命名约定 (UNC) 路径 (例如，  **\\TFSBUILD\Drops**)。

    ![](deploying-a-specific-build/_static/image3.png)
5. 上**过程**选项卡上，在**生成过程文件**下拉列表中，保留**DefaultTemplate.xaml**选。 这是添加到新的所有团队项目的默认生成过程模板之一。
6. 在**生成过程参数**表，请单击中**生成的项**行，然后依次**省略号**按钮。

    ![](deploying-a-specific-build/_static/image4.png)
7. 在**生成的项**对话框中，单击**添加**。
8. 在**类型的项**下拉列表中，选择**MSBuild 项目文件**。
9. 浏览到与其控制部署过程，选择的文件，然后单击自定义项目文件的位置**确定**。

    ![](deploying-a-specific-build/_static/image5.png)
10. 在**生成的项**对话框中，单击**确定**。
11. 在**生成过程参数**表中，展开**高级**部分。
12. 在**MSBuild 参数**行，指定的位置特定于环境的项目文件并添加一个占位符，供你生成文件夹的位置：

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > 你将需要重写**OutputRoot**值每次对生成进行排队。 这一点在下一个过程。
13. 单击“保存” 。

时触发的生成，你需要更新**OutputRoot**属性以指向你想要部署的生成。

**若要部署从生成定义的特定版本**

1. 在**团队资源管理器**窗口中，右键单击生成定义，并依次**使新生成入队**。

    ![](deploying-a-specific-build/_static/image7.png)
2. 在**队列生成**对话框中，在**参数**选项卡上，展开**高级**部分。
3. 在**MSBuild 参数**行中，替换的值**OutputRoot**属性替换为你生成的文件夹的位置。 例如:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > 请务必包括尾部斜杠生成文件夹的路径末尾。
4. 单击**队列**。

当你将生成排队、 项目文件将部署数据库脚本和 web 包从生成放置文件夹中指定**OutputRoot**属性。

## <a name="conclusion"></a>结束语

本主题描述如何发布部署资源，如 web 包和数据库脚本，从上一个特定生成使用拆分项目文件部署模型。 它介绍了如何重写**OutputRoot**属性以及如何将部署逻辑合并到 TFS 生成定义。

## <a name="further-reading"></a>其他阅读材料

创建生成定义的详细信息，请参阅[创建基本生成定义](https://msdn.microsoft.com/library/ms181716.aspx)和[定义生成过程](https://msdn.microsoft.com/library/ms181715.aspx)。 队列生成的更多指南，请参阅[对生成进行排队](https://msdn.microsoft.com/library/ms181722.aspx)。

>[!div class="step-by-step"]
[上一页](creating-a-build-definition-that-supports-deployment.md)
[下一页](configuring-permissions-for-team-build-deployment.md)
