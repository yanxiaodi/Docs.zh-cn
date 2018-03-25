---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: "在 TFS 中创建团队项目 |Microsoft 文档"
author: jrjlee
description: "本主题介绍如何创建在 Team Foundation Server (TFS) 2010年的新团队项目。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: ef8cddb7733c1f8cacd24c5cf341a42741d25a95
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-team-project-in-tfs"></a>在 TFS 中创建团队项目
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何创建在 Team Foundation Server (TFS) 2010年的新团队项目。


本主题窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器解决方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; 来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序Communication Foundation (WCF) 服务和数据库项目。

## <a name="task-overview"></a>任务概述

若要设置并在 TFS 中使用新的团队项目，你将需要完成以下高级步骤：

- 向将创建新的团队项目的用户授予权限。
- 创建团队项目。
- 向将在项目工作的团队成员授予权限。
- 签入一些内容。

本主题将演示如何执行这些过程，以及它将确定用户和作业很可能是负责针对每个步骤的角色。 请注意，具体取决于你的组织的结构，其中每个任务可能是不同的人员的责任。

任务和本主题中的演练假定你已安装并配置 TFS，，你已创建团队项目集合配置过程的一部分。 有关这些假设，详细信息和方案的更多常规背景信息，请参阅[用于 Web 部署配置 TFS 生成服务器](configuring-a-tfs-build-server-for-web-deployment.md)。

## <a name="grant-permissions-to-the-team-project-creator"></a>向团队项目创建者授予权限

若要创建新的团队项目，你需要这些权限：

- 你必须具有**创建新项目**TFS 应用层上的权限。 通常通过将用户添加到授予此权限**项目集合管理员**TFS 组。 **Team Foundation Administrators**全局组还包括此权限。
- 你必须有权创建 SharePoint 站点集合对应的 TFS 团队项目集合中的新团队站点。 通常通过将用户添加到与 SharePoint 组授予此权限**完全控制**权限在 sharepoint 站点集合。
- 如果你使用的 SQL Server Reporting Services 功能，你必须属于**Team Foundation 内容管理器**Reporting Services 中的角色。

### <a name="who-performs-these-procedures"></a>这些过程的执行者是谁？

通常情况下，用户或组的管理 TFS 部署还执行这些过程。

由于这是一个高特权的权限集，新团队项目，通常由创建用户一小部分与负责管理 TFS 部署。 开发人员将不通常被授予创建新的团队项目所需的权限。

### <a name="grant-permissions-in-tfs"></a>授予在 TFS 中的权限

如果你想要使用户能够创建新的团队项目，第一项高级任务是将用户添加到**项目集合管理员**组为团队项目集合。

**若要将用户添加到项目集合管理员组**

1. 在 TFS 服务器上，在**启动**菜单上，指向**所有程序**，单击**Microsoft Team Foundation Server 2010**，然后单击**Team Foundation管理控制台**。
2. 在导航树视图中，展开**应用程序层**，然后单击**团队项目集合**。

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. 在**团队项目集合**窗格中，选择团队项目你想要管理的集合。

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. 上**常规**选项卡上，单击**组成员身份**。

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. 在**全局组**对话框中，选择**项目集合管理员**分组，并依次**属性**。
6. 在**Team Foundation Server 组属性**对话框中，选择**Windows 用户或组**，然后单击**添加**。

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. 在**选择用户、 计算机或组**对话框中，键入你想要能够创建新的团队项目，用户的用户名单击**检查名称**，然后单击**确定**.

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. 在**Team Foundation Server 组属性**对话框中，单击**确定**。
9. 在**全局组**对话框中，单击**关闭**。

### <a name="grant-permissions-in-sharepoint-services"></a>授予 SharePoint Services 中的权限

接下来，你需要为用户授予 SharePoint 站点集合对应于 TFS 团队项目集合中创建新的团队网站的权限。

**若要授予对 SharePoint 站点集合的完全控制权限**

1. 在 Team Foundation Server 的管理控制台中，在**团队项目集合**页上，选择你想要管理的团队项目集合。
2. 上**SharePoint 站点**选项卡上，记下的值**当前默认站点位置**URL。

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. 打开 Internet Explorer，然后转到步骤 2 中记下的 URL。

    > [!NOTE]
    > 如果不作为创建团队项目集合的用户登录到 Windows，你将需要以登录到 SharePoint 此用户才能继续。
4. 上**站点操作**菜单上，单击**站点设置**。

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. 上**站点设置**页上，在**用户和权限**，单击**人员和组**。
6. 在左侧的导航面板中，单击**组**。

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. 上**人员和组： 所有组**页上，单击**为此站点设置用户组**。

    ![](creating-a-team-project-in-tfs/_static/image9.png)

    > [!NOTE]
    > 你可能会收到**HTTP 404 未找到**由于 double 的 HTTP 编码 bug 的错误。 如果发生这种情况，将替换此 URL:   
    > [*站点集合 URL*] /\_layouts/permsetup.aspx  
    > 例如:  
    > http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx
8. 上**为此站点设置用户组**页上，添加将创建到团队项目的用户**所有者**分组，并依次**确定**。

    ![](creating-a-team-project-in-tfs/_static/image10.png)

允许用户创建团队项目集合中的新团队项目的详细信息，请参阅[为团队项目集合设置管理员权限](https://msdn.microsoft.com/library/dd547204.aspx)。

## <a name="create-a-new-team-project-and-add-users"></a>创建新的团队项目并添加用户

所需的权限之后，你可以使用**团队资源管理器**Visual Studio 2010 创建新的团队项目中的窗口。 此方法提供了收集所有必要的信息并执行必要的任务在 TFS、 SharePoint 和 SQL Server Reporting Services 中的向导。 你还需要新的团队项目的权限授予的开发人员团队的成员，以使他们能够添加和修改内容。

### <a name="who-performs-these-procedures"></a>这些过程的执行者是谁？

通常是 TFS 管理员或开发人员团队主管执行这些过程。

### <a name="create-a-new-team-project"></a>创建新的团队项目

下一步的过程描述如何在 TFS 2010 中创建新的团队项目。

**若要创建新的团队项目**

1. 上**启动**菜单上，指向**所有程序**，单击**Microsoft Visual Studio 2010**，右键单击**Microsoft Visual Studio 2010**，然后单击**以管理员身份运行**。

    > [!NOTE]
    > 如果你不以管理员身份运行 Visual Studio 2010，新的团队项目向导将失败的最后一个步骤。
2. 如果**用户帐户控制**显示对话框，请单击**是**。
3. 在 Visual Studio 中，在**团队**菜单上，单击**连接到 Team Foundation Server**。

    > [!NOTE]
    > 如果你已配置 TFS 服务器的连接，则可以省略步骤 4-7。
4. 在**连接到团队项目**对话框中，单击**服务器**。
5. 在**添加/移除 Team Foundation Server**对话框中，单击**添加**。
6. 在**添加 Team Foundation Server**对话框中，提供你的 TFS 实例的详细信息，然后单击**确定**。

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. 在**添加/移除 Team Foundation Server**对话框中，单击**关闭**。
8. 在**连接到团队项目**对话框中，选择你想要连接到，选择团队的 TFS 实例项目的集合你想要添加到，，然后单击**连接**。

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. 在**团队资源管理器**窗口中，右键单击团队项目集合，然后单击**新团队项目**。

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. 在**新团队项目**对话框中，提供名称和团队项目的描述，然后单击**下一步**。

    > [!NOTE]
    > 如果你的团队项目包含空格，您可能会遇到一些问题，到达以 Internet 信息服务 (IIS) Web 部署工具 （Web 部署） 用于部署的输出路径中的包。 路径中的空格会使运行 Web 部署命令更加困难。

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. 上**选择过程模板**页上，选择你想要使用以管理开发过程中，然后单击的过程模板**下一步**。

    > [!NOTE]
    > 有关 TFS 过程模板的更多信息，请参阅[过程模板和工具](https://msdn.microsoft.com/vstudio/aa718795)。
12. 上**团队站点设置**页上，保留默认设置保持不变，，然后单击**下一步**。
13. 此设置创建，或标识，与 TFS 团队项目相关联的 SharePoint 团队站点。 你的开发团队可以使用此站点来管理文档、 参与讨论、 创建 wiki 页面和执行代码的不相关的各种其他任务。 有关详细信息，请参阅[交互之间 SharePoint 产品和 Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx)。
14. 上**指定源代码管理设置**页上，保留默认设置保持不变，，然后单击**下一步**。
15. 此设置标识，或在 TFS 文件夹层次结构将充当你的内容的根文件夹中创建的位置。
16. 上**确认团队项目设置**页上，单击**完成**。
17. 在新的团队项目已成功创建，在**创建的团队项目**页上，单击**关闭**。

### <a name="add-users-to-a-team-project"></a>将用户添加到团队项目

现在，你已创建新的团队项目，可以将权限授予用户，使他们可以开始添加和对内容进行协作。

**若要将用户添加到团队项目**

1. 在 Visual Studio 2010 中，在**团队资源管理器**窗口中，右键单击团队项目，指向**团队项目设置**，然后单击**组成员身份**。

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. 若要使用户能够添加、 修改和删除在源代码管理下的代码，你需要添加他或她到**contributors （参与者)**组。
3. 在**项目组**对话框中，选择**contributors （参与者)**分组，并依次**属性**。

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. 在**Team Foundation Server 组属性**对话框中，选择**Windows 用户或组**，然后单击**添加**。

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. 在**选择用户、 计算机或组**对话框中，键入你想要添加到团队项目中，用户的用户名单击**检查名称**，然后单击**确定**。

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. 在**Team Foundation Server 组属性**对话框中，单击**确定**。
7. 在**项目组**对话框中，单击**关闭**。

## <a name="conclusion"></a>结束语

此时，新的团队项目已准备好使用，并且你的开发人员团队可以开始添加内容和开发过程进行协作。

下一主题[将内容添加到源代码管理](adding-content-to-source-control.md)，介绍如何将内容添加到源代码管理。

## <a name="further-reading"></a>其他阅读材料

有关在 TFS 中创建团队项目的更广泛指南，请参阅[创建团队项目](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx)。 允许用户创建团队项目集合中的新团队项目的详细信息，请参阅[为团队项目集合设置管理员权限](https://msdn.microsoft.com/library/dd547204.aspx)。 将用户添加到团队项目的详细信息，请参阅[向团队项目添加用户](https://msdn.microsoft.com/library/bb558971.aspx)。

>[!div class="step-by-step"]
[上一页](configuring-team-foundation-server-for-web-deployment.md)
[下一页](adding-content-to-source-control.md)
