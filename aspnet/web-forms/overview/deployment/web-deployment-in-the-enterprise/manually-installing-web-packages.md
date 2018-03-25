---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: "手动安装 Web 包 |Microsoft 文档"
author: jrjlee
description: "本主题介绍如何手动导入 web 部署包到 Internet 信息服务 (IIS)。 主题生成和打包 Web Applicati..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: e06d37c01ab66f0723b687f4ed1ee72561099aef
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="manually-installing-web-packages"></a>手动安装 Web 包
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何手动导入 web 部署包到 Internet 信息服务 (IIS)。
> 
> 主题[生成和打包 Web 应用程序项目](building-and-packaging-web-application-projects.md)描述如何 IIS Web 部署工具 （Web 部署），结合 Microsoft Build Engine (MSBuild) 和 Web 发布管道 (WPP) 允许您打包你到单个 zip 文件的 web 应用程序项目。 此文件，通常称为 web 部署包 （或只需部署包），包含需要 IIS，以便重新创建 web 应用程序的 web 服务器上的所有内容和配置信息。
> 
> 一旦你已创建 web 部署包，你可以将其发布到 IIS 服务器以各种方式。 在许多方案，你将想要利用 MSBuild、 WPP，和 Web 部署来创建并自动进行的或单步执行的生成和部署过程的一部分远程安装 web 包之间的集成点。 此过程所述[部署 Web 包](deploying-web-packages.md)。 但是，这并不总是可行。 假设你想要部署 web 应用程序到面向 Internet 的生产环境。 出于安全原因，生产环境中此类位于非常少，可能需要独立于生成服务器上，在外围网络 （也称为 DMZ、 外围安全区域和外围子网） 的子网防火墙后面。 很多情况下，在生产环境将在单独的域或在物理上隔离的网络。
> 
> 在这些情况下，唯一的选项可能是端口 web 包部署到目标服务器上的，手动将其导入到 IIS。 虽然这种方法，不能自动的部署，它仍是一种用于发布的 web 应用程序 （&） #x 2014年的技术很有效; 您只需将单个 zip 文件复制到你的 web 服务器，然后使用向导来指导你完成导入过程。


本主题窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器解决方案](the-contact-manager-solution.md)& #x 2014; 来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序Communication Foundation (WCF) 服务和数据库项目。

## <a name="task-overview"></a>任务概述

你将需要完成以下高级任务，以将 web 部署包导入到 IIS:

- 创建 web 部署包使用 MSBuild 命令行、 Team Build 或 Visual Studio 2010。
- 将 web 包复制到目标 web 服务器。
- 使用导入应用程序包向导在 IIS 管理器安装的 web 包，并为变量，如连接字符串和服务终结点提供值。

本主题将演示如何执行这些过程。 任务和本主题中的演练假定你已经熟悉 web 包、 Web 部署和 WPP 背后的概念。 有关详细信息，请参阅[生成和打包 Web 应用程序项目](building-and-packaging-web-application-projects.md)。

> [!NOTE]
> 本主题最结合使用[配置用于 Web 部署发布 （脱机部署） 的 Web 服务器](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)，其中解释了如何安装所需的组件和准备要包导入的 IIS 网站。


## <a name="create-a-web-deployment-package"></a>创建 Web 部署包

第一个任务是创建 web 部署包为你想要部署的 web 应用程序项目。 各种不同的方式，可以创建 web 包。

**方法 1： 使用 Visual Studio 创建的包生成过程的一部分**

你可以配置 web 应用程序项目中以通过每次生成后创建 web 部署包**打包/发布 Web**项目属性页上的选项卡。 此过程所述[生成和打包 Web 应用程序项目](building-and-packaging-web-application-projects.md)。

**方法 2： 使用 MSBuild 创建作为生成过程的一部分的包**

如果生成你的 web 应用程序项目使用 MSBuild 直接，通过自定义 MSBuild 项目文件中或从命令行中，你可以创建 web 部署包作为生成过程的一部分由包括**DeployOnBuild = true**和**DeployTarget = 包**您的命令中的属性。 此过程所述[了解该生成过程](understanding-the-build-process.md)。

**方法 3： 在 Visual Studio 中按需创建包**

你可以在任何时间在 Visual Studio 2010 中创建 web 应用程序项目的 web 部署包。 为此，请在**解决方案资源管理器**窗口中，右键单击 web 应用程序项目，并依次**生成部署包**。

![](manually-installing-web-packages/_static/image1.png)

**方法 4： 从命令行进行按需创建包**

你可以从命令行创建 web 部署包，通过调用**包**上 web 应用程序项目使用 MSBuild 目标。 该命令应类似如下：


[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]


者为准接近你使用，最终结果相同。 WPP 为 zip 文件，以及各种支持的资源，你的 web 应用程序项目的输出文件夹中创建 web 部署包。

![](manually-installing-web-packages/_static/image2.png)

当你计划手动导入的 web 包时，你需要仅的 zip 文件。 将此文件复制到目标 web 服务器，你可以开始导入过程。

## <a name="import-a-web-package-into-iis"></a>将 Web 包导入到 IIS

下一步过程可用于将 web 部署包从本地文件系统导入的 IIS 网站。 执行此过程之前，请确保你有：

- 复制到 web 服务器的 web 部署包。
- 配置 IIS web 服务器来承载你的应用程序。

有关配置 IIS web 服务器以支持 web 部署包的详细信息，请参阅[配置用于 Web 部署发布 （脱机部署） 的 Web 服务器](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)。

**若要导入使用 IIS 管理器的 web 部署包**

1. 在 IIS 管理器中，在**连接**窗格中，右键单击你的 IIS 网站，指向**部署**，然后单击**导入应用程序**。

    ![](manually-installing-web-packages/_static/image3.png)
2. 在导入应用程序的包向导中，在**选择的包**页上，浏览到你的 web 部署包的位置，然后单击**下一步**。
3. 上**选择包的内容**页上，清除你不需要，，然后单击的任何内容**下一步**。

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > 在许多情况下，你可能不想要导入附带 web 部署包的全部内容。 例如，可能不想要允许 Web Deploy 以将关联的数据库。  
    > **授予权限**条目目标文件系统，以确保应用程序池标识可访问存储网站内容的物理文件夹上设置权限。 此外，匿名身份验证用户授予读取权限以便允许应用程序提供多用途 Internet 邮件扩展 (MIME) 类型文件文件夹。 如果你愿意，你可以删除这些条目并手动配置权限。
4. 上**输入应用程序包信息**页上，提供所请求的信息。

    ![](manually-installing-web-packages/_static/image5.png)
5. 当创建 web 包时，WPP 分析你的应用程序的配置文件，并检测到任何变量，如连接字符串和服务终结点。 这种情况下：

    1. **应用程序路径**是你想要安装应用程序的 IIS 路径。 此设置是通用的 WPP 创建的所有部署包。
    2. **ContactService 服务终结点地址**是应用程序应使用与已部署的 WCF 服务进行通信的地址。 此设置对应于将项记入*web.config*文件。
    3. 第一个**连接字符串**设置是 Web 部署应使用来部署应用程序 （在这种情况下的 ASP.NET 成员资格数据库中） 与关联的数据库的连接字符串。 此设置对应于上的设置**打包/发布 SQL** Visual Studio 中的选项卡。
    4. 第二个**连接字符串**设置是你的应用程序实际上将用于启动并正在运行时与数据库通信的连接字符串。 这对应于中的连接字符串项*web.config*文件。

        > [!NOTE]
        > 有关这些参数来自何处的详细信息，请参阅[Web 包部署的配置参数](configuring-parameters-for-web-package-deployment.md)。
6. 单击 **“下一步”**。
7. 如果这不是第一次你已部署到此网站应用程序，将提示您指定是否想要删除在安装之前的所有现有内容。 选择适合你的要求，选项，然后单击**下一步**。

    ![](manually-installing-web-packages/_static/image6.png)
8. 完成 IIS 已安装的包，请单击**完成**。

    ![](manually-installing-web-packages/_static/image7.png)

此时，您已成功发布到 IIS web 应用程序。

## <a name="conclusion"></a>结束语

本主题描述如何将 web 部署包导入到 IIS 网站使用 IIS 管理器。 当安全或基础结构约束进行远程部署，无法执行或不需要时，web 应用程序发布到此种方法很适合。

## <a name="further-reading"></a>其他阅读材料

有关如何配置 IIS web 服务器以支持手动导入 web 包的指南，请参阅[配置用于 Web 部署发布 （脱机部署） 的 Web 服务器](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)。 有关部署 web 包的更多常规指南，请参阅[演练： 部署 Web 应用程序项目使用 Web 部署包 (4 的第 1 部分)](https://msdn.microsoft.com/library/dd483479.aspx)。

>[!div class="step-by-step"]
[上一篇](creating-and-running-a-deployment-command-file.md)
