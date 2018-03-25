---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: "将内容添加到源代码管理 |Microsoft 文档"
author: jrjlee
description: "本主题说明如何将内容添加到源代码管理中 Team Foundation Server (TFS) 2010年。 它描述如何将解决方案和项目添加到团队 proje..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: d46e2697d10ca27f8e08533350a6e7f2354b4a43
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="adding-content-to-source-control"></a>将内容添加到源代码管理
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题说明如何将内容添加到源代码管理中 Team Foundation Server (TFS) 2010年。 它介绍如何将解决方案和项目添加到团队项目在 TFS 中，并介绍如何将如框架或程序集的外部依赖关系添加到源代码管理。


本主题窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器解决方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; 来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序Communication Foundation (WCF) 服务和数据库项目。

## <a name="task-overview"></a>任务概述

在大多数情况下，每个开发人员团队成员应能够将内容添加到源代码管理。 若要将解决方案添加到 TFS 中的源代码管理，你将需要完成以下高级步骤：

- 连接到团队项目。
- 将在服务器上的团队项目文件夹结构映射到本地计算机上的文件夹结构。
- 将解决方案及其内容添加到源代码管理。
- 添加到源代码管理的任何外部依赖关系。

本主题将演示如何执行这些过程。

任务和本主题中的演练假定你已创建了一个新的 TFS 团队项目来管理你的内容。 创建新的团队项目的详细信息，请参阅[在 TFS 中创建团队项目](creating-a-team-project-in-tfs.md)。

### <a name="who-performs-these-procedures"></a>这些过程的执行者是谁？

在大多数情况下，每个开发人员团队成员应能来添加和修改特定团队项目中的内容。

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>连接到团队项目并创建的文件夹映射

向源代码管理中添加任何内容之前，你需要连接到团队项目并在本地计算机上创建服务器上的文件夹结构和文件系统之间的映射。

**连接到团队项目和映射的本地路径**

1. 你开发人员在工作站上，打开 Visual Studio 2010。
2. 在 Visual Studio 中，在**团队**菜单上，单击**连接到 Team Foundation Server**。

    > [!NOTE]
    > 如果你已配置 TFS 服务器的连接，则可以省略步骤 3-6。
3. 在**连接到团队项目**对话框中，单击**服务器**。
4. 在**添加/移除 Team Foundation Server**对话框中，单击**添加**。
5. 在**添加 Team Foundation Server**对话框中，提供你的 TFS 实例的详细信息，然后单击**确定**。

    ![](adding-content-to-source-control/_static/image1.png)
6. 在**添加/移除 Team Foundation Server**对话框中，单击**关闭**。
7. 在**连接到团队项目**对话框中，选择你想要连接到，选择团队的 TFS 实例项目集合中，选择你想要添加到团队项目，然后单击**连接**。

    ![](adding-content-to-source-control/_static/image2.png)
8. 在**团队资源管理器**窗口中，展开你的团队项目，然后双击**源代码管理**。

    ![](adding-content-to-source-control/_static/image3.png)
9. 上**源代码管理器**选项卡上，单击**未映射**。

    ![](adding-content-to-source-control/_static/image4.png)
10. 在**映射**对话框中，在**本地文件夹**框中，浏览到 （或创建） 的本地文件夹，以充当团队项目的根文件夹，然后单击**映射**。

    ![](adding-content-to-source-control/_static/image5.png)
11. 当系统提示你下载的源文件时，则单击**是**。

    ![](adding-content-to-source-control/_static/image6.png)

此时，你具有到你的开发人员工作站上的本地文件夹映射团队项目的服务器端文件夹。 您已为您的本地文件夹结构从团队项目中下载任何现有内容。 你现在可以开始将你自己的内容添加到源代码管理。

## <a name="add-projects-and-solutions-to-source-control"></a>添加到源代码管理项目和解决方案

若要添加到源代码管理项目和解决方案，你首先需要将它们移到团队项目的映射文件夹中，你的本地计算机上。 然后可以将您添加与服务器同步的内容中进行检查。

**若要将项目添加到源代码管理**

1. 你开发人员在工作站上，到团队项目的映射的文件夹结构中的相应位置中移动你的项目和解决方案。

    > [!NOTE]
    > 许多组织都有项目和解决方案应该如何组织在源代码管理中的首选的方法。 有关如何构建文件夹的指南，请参阅[How To： 结构你源控件文件夹，Team Foundation Server 中](https://msdn.microsoft.com/library/bb668992.aspx)。
2. 在 Visual Studio 2010 中打开解决方案。
3. 在**解决方案资源管理器**窗口中，右键单击该解决方案，然后单击**将解决方案添加到源代码管理**。

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > 在某些情况下，具体取决于你的组织如何喜欢结构内容在 TFS 中，你可能需要将项目添加到源控件单独以提供更好地控制细化你的源代码的组织方式。
4. 验证**源代码管理器**选项卡显示你已添加为团队项目的服务器文件夹结构中的内容。

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > **源代码管理器**选项卡显示您没有任何进一步提示因为将你的解决方案添加到本地文件系统上的映射文件夹的内容。 如果你的解决方案位于未映射的位置，会提示你用于 TFS 和你的本地文件系统中指定文件夹位置。
5. 上**源代码管理器**选项卡上，在**文件夹**窗格中，右键单击团队项目 (例如， **ContactManager**)，然后单击**签入挂起的更改**。
6. 在**签入 – 源文件**对话框中，键入注释，，然后单击**签入**。

    ![](adding-content-to-source-control/_static/image9.png)

此时已添加到 TFS 中的源代码管理你的解决方案。

## <a name="add-external-dependencies-to-source-control"></a>添加到源代码管理的外部依赖关系

当你添加到源代码管理的项目或解决方案时，，系统将也添加任何文件和文件夹在你的项目或解决方案中。 但是，在许多情况下，项目和解决方案还依赖于外部依赖关系，如本地程序集，才能正常工作。 你需要添加到源代码管理以便 Team Build 和开发人员团队其他成员的任何此类资源已成功生成你的代码。

例如，联系人管理器示例解决方案的文件夹结构包括名为包的文件夹。 这包含 ADO.NET 实体框架 4.1 程序集和各种支持的资源。 包文件夹不是解决方案的一部分联系人管理器，但没有它无法成功生成解决方案。 若要启用 Team Build 要生成解决方案，你需要将包文件夹添加到源代码管理。

> [!NOTE]
> 包含的包文件夹的是典型的实体框架中或类似资源添加到你使用适用于 Visual Studio 2010 NuGet 扩展的解决方案时，会发生什么情况。


**若要添加到源代码管理的非项目内容**

1. 确保想要添加它 （例如，包文件夹） 的项位于本地文件系统上的映射文件夹中的适当位置。
2. 在 Visual Studio 2010 中，在**团队资源管理器**窗口中，展开你的团队项目，然后双击**源代码管理**。

    ![](adding-content-to-source-control/_static/image10.png)
3. 上**源代码管理器**选项卡上，在**文件夹**窗格中，选择要添加的文件夹包含项或项。
4. 单击**将项添加到文件夹**按钮。

    ![](adding-content-to-source-control/_static/image11.png)
5. 在**添加到源代码管理**对话框框中，选择你想要添加，，然后单击的文件夹**下一步**。

    ![](adding-content-to-source-control/_static/image12.png)
6. 上**排除项**选项卡上，选择任何所需的项目，已自动排除 （例如，程序集），然后单击**包括项目**。

    ![](adding-content-to-source-control/_static/image13.png)
7. 上**要添加项**选项卡上，验证你想要包括的所有文件未列出，然后单击**完成**。

    ![](adding-content-to-source-control/_static/image14.png)
8. 在**源代码管理器**窗口中，单击**签入**按钮。

    ![](adding-content-to-source-control/_static/image15.png)
9. 在**签入 – 源文件**对话框中，键入注释，，然后单击**签入**。

此时，你具有到源代码管理，为你的解决方案中添加的外部依赖关系。

## <a name="conclusion"></a>结束语

本主题介绍如何连接到团队项目，将映射的文件夹结构，并将内容添加到源代码管理。 有关如何使用源代码管理下的项的详细信息，请参阅[使用版本控制](https://msdn.microsoft.com/library/ms181368.aspx)。

下一主题[用于 Web 部署配置 TFS 生成服务器](configuring-a-tfs-build-server-for-web-deployment.md)，介绍如何准备 TFS Team Build 连接的服务器，以生成和部署你的解决方案。

## <a name="further-reading"></a>其他阅读材料

有关在 TFS 中的源控件使用的更全面信息，请参阅[使用版本控制](https://msdn.microsoft.com/library/ms181368.aspx)。

>[!div class="step-by-step"]
[上一页](creating-a-team-project-in-tfs.md)
[下一页](configuring-a-tfs-build-server-for-web-deployment.md)
