---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: "使用 Visual Studio 的 ASP.NET Web 部署： 简介 |Microsoft 文档"
author: tdykstra
description: "本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure App Service Web Apps 或第三方托管提供程序，通过使用 V..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: a7c5b348e44a88a56680237b0cac75c999901ec3
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-web-deployment-using-visual-studio-introduction"></a>使用 Visual Studio 的 ASP.NET Web 部署： 简介
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure App Service Web Apps 或第三方托管提供程序，通过使用 Visual Studio 2012 与 Azure SDK for.NET。 过程的大部分都是类似于 Visual Studio 2013。
> 
> 开发 web 应用程序，以便使其能够通过 Internet 的人员。 但 web 编程教程通常停止后它们已向您演示如何获取某产品投入使用你的开发计算机上。 这一系列教程的开始其他停止： 已生成的 web 应用、 测试它，并且准备就绪。 后续步骤 这些教程介绍如何首次部署到测试，在本地开发计算机上的 IIS，然后执行到 Azure 或第三方托管提供程序的过渡和生产。 你将部署的示例应用程序是使用实体框架、 SQL Server 和 ASP.NET 成员资格系统的 web 应用程序项目。 示例应用程序使用 ASP.NET Web 窗体，但显示的过程也同样适用于 ASP.NET MVC 和 Web API。
> 
> 这些教程也假设你知道如何使用 Visual Studio 中的 ASP.NET。 如果不这样做，是要启动一个好[基本的 ASP.NET Web 窗体教程](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md)或[基本的 ASP.NET MVC 教程](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)。
> 
> 如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET 部署论坛](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)或[StackOverflow](http://stackoverflow.com)。
> 
> 此内容是还可用作免费电子书中[TechNet 电子图书库](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio)。


## <a name="overview"></a>概述

这些教程将指导你完成部署的 ASP.NET web 应用程序包括 SQL Server 数据库。 向测试，在本地开发计算机上的 IIS，然后执行到 Azure App Service 和 Azure SQL 数据库的过渡和生产中的 Web 应用，你将首先部署。 你将了解如何通过一键式发布，Visual Studio 部署，你将了解如何使用命令行进行部署。

教程数可能会使部署过程看起来项极其困难。 事实上，基本过程是简单的。 但是，在实际情况下，你通常需要执行额外的部署任务-例如，在目标服务器上设置文件夹权限。 我们已说明中的一些这些其他任务，可能会阻止您成功地部署实际的应用程序的信息不省略教程希望。

这些教程旨在运行在序列中，并且每个部分基于前面部分。 你可以跳过部分与你的情况，无关，但然后，您可能需要根据过程调整更高版本的教程中。

## <a name="intended-audience"></a>目标的受众

这些教程旨在 ASP.NET 开发人员在环境中工作其中：

- 生产环境是 Azure App Service Web Apps 或第三方托管提供商。
- 部署并不局限于持续集成过程，但可能直接从 Visual Studio 中完成。

从部署[源代码管理](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)使用[持续交付](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)过程未涵盖这些教程演示如何从命令行部署的一个教程除外中。 持续交付有关的信息，请参阅以下资源：

- [持续集成和持续交付 （使用 Windows Azure 构建真实世界云应用）](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [部署在 Azure App Service web 应用](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [部署在企业方案中的 Web 应用程序](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)（教程面向 Visual Studio 2010，它仍然具有企业环境的有用信息的较旧集。）

## <a name="using-a-third-party-hosting-provider"></a>使用第三方托管提供商

这些教程将指导你完成设置 Azure 帐户并部署到在 Azure App Service Web Apps 用于过渡和生产应用程序的过程。 但是，可用于相同的基本过程将部署到所选的第三方托管提供程序。 如果教程进程唯一通过转到 Azure 时，它们说明，并建议会在第三方托管提供商有哪些不同。

## <a name="deploying-web-app-projects"></a>部署 web 应用程序项目

下载并部署为这些教程的示例应用程序是一个 Visual Studio web 应用程序项目。 但是，如果你安装最新[for Visual Studio Web 发布更新](https://msdn.microsoft.com/tr-tr/library/jj161045.aspx)，可以为 web 应用程序项目中使用的相同的部署方法和工具。

## <a name="deploying-aspnet-mvc-projects"></a>部署 ASP.NET MVC 项目

该示例应用程序的 ASP.NET Web 窗体项目，但了解如何执行的所有内容都是也适用于 ASP.NET MVC。 Visual Studio MVC 项目是另一个窗体的 web 应用程序项目。 唯一的区别是，如果你正在将部署到的宿主提供程序不支持 ASP.NET MVC 或它的目标版本中，你必须确保你已安装相应 ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0)， [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0)或[MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) 项目中的 NuGet 包。

## <a name="programming-language"></a>编程语言

示例应用程序使用 C#，但这些教程不需要的 C# 的知识和教程所示的部署技术并非特定于语言的。

## <a name="database-deployment-methods"></a>数据库部署方法

有三种方法，你可以部署 SQL Server 数据库以及 Visual Studio 中的 web 部署：

- Entity Framework Code First 迁移
- DbDacFx Web Deploy 提供程序
- DbFullSql Web Deploy 提供程序

在本教程中，你将使用这些方法在前的两个。 DbFullSql Web Deploy 提供程序是不再建议除外某些特定的情况下，例如从 SQL Server Compact 迁移到 SQL Server 的传统方法。

在本教程中所示的方法适用于 SQL Server 数据库，不 SQL Server Compact。 有关如何部署 SQL Server Compact 数据库的信息，请参阅[Visual Studio Web 部署使用 SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md)。

在本教程中所示的方法都要求你使用 Web 部署发布方法。 如果你愿意为其他发布方法，例如 FTP、 文件系统或 FPSE，请参阅[部署独立于 web 应用程序部署数据库](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate)for Visual Studio 和 ASP.NET Web 部署内容映射中。

### <a name="entity-framework-code-first-migrations"></a>Entity Framework Code First 迁移

在实体框架版本 4.3 中，Microsoft 引入了 Code First 迁移。 Code First 迁移对数据模型进行增量更改以及那些将更改传播到数据库的过程进行自动化。 在的 Code First 的早期版本，你通常会让实体框架除去并重新创建数据库每次更改数据模型。 这不是在开发过程中的出现问题，因为测试数据很轻松地重新创建，但在生产环境中通常需要更新数据库架构，而不删除数据库。 使用迁移功能，Code First 更新而无需删除并重新创建它的数据库。 你可以让 Code First 自动决定如何进行所需的架构更改，也可以编写自定义所做的更改的代码。 Code First 迁移的简介，请参阅[Code First 迁移](https://msdn.microsoft.com/library/hh770484.aspx)。

在部署 web 项目时，Visual Studio 可以自动执行部署由 Code First 迁移的数据库的过程。 在创建发布配置文件时，你可以选择一个复选框，标记为执行 Code First 迁移 （应用程序启动时运行）。 此设置会导致部署过程以自动配置目标服务器上的应用程序 Web.config 文件，以便代码优先使用`MigrateDatabaseToLatestVersion`初始值设定项类。

Visual Studio 不执行任何与数据库在部署过程。 当部署的应用程序部署后首次访问数据库时，Code First 自动创建数据库或数据库架构更新到最新版本。 如果应用程序实施迁移 Seed 方法，方法将创建数据库或更新架构之后运行。

在本教程中，你将使用 Code First 迁移来部署应用程序数据库。

### <a name="the-dbdacfx-web-deploy-provider"></a>DbDacFx Web Deploy 提供程序

对于不受 Entity Framework Code First 的 SQL Server 数据库，你可以选择一个复选框，当你配置发布配置文件标记更新数据库。 在初始部署以后，dbDacFx 提供程序在目标数据库，以匹配源数据库中创建表和其他数据库对象。 在后续部署上提供程序确定源和目标数据库之间的差异和它会更新目标数据库，以匹配源数据库的架构。 默认情况下，提供程序不会带来会导致数据丢失，例如当删除表或列的任何更改。

此方法并不促使数据库表中数据的部署，但你可以创建脚本来执行此操作和配置 Visual Studio 以在部署过程中运行。 在部署过程中运行脚本的另一个原因是无法自动完成，因为它们可能会导致数据丢失的架构更改。

在本教程中，你将使用 dbDacFx 提供程序部署 ASP.NET 成员资格数据库。

## <a name="troubleshooting-during-this-tutorial"></a>本教程过程中的疑难解答

在部署期间，发生错误时，或已部署的站点无法正常运行，错误消息不始终提供最直接的解决方案。 若要帮助您完成某些常见的问题情况下，[故障排除参考页](troubleshooting.md)可用。 如果你收到如下错误消息，或当你完成本教程的内容不起作用，请务必检查故障排除的页面。

## <a name="comments-welcome"></a>欢迎使用注释

对教程的注释都欢迎，并更新本教程时将保持尽一切可能要考虑的帐户更正或改进教程注释中提供的建议。

<a id="prerequisites"></a>

## <a name="prerequisites"></a>系统必备

本教程是专门针对以下产品编写的：

- Windows 8 或 Windows 7。
- Visual Studio 2012 或 Visual Studio 2012 Express for Web 与[最新的更新](https://go.microsoft.com/fwlink/?LinkId=272486)。
- [Azure SDK for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

你可以遵循本教程通过使用 Visual Studio 2010 SP1 或 Visual Studio 2013 中，但一些屏幕快照将会不同，某些功能将不同。

如果你使用 Visual Studio 2013，安装[Azure SDK for Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322)。

如果你使用 Visual Studio 2010 SP1，安装以下软件：

- [Azure SDK for Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh500335.aspx)。

具体取决于 SDK 依赖项数量的上已有您的计算机，安装 Azure SDK 可能耗时较长，时间从几分钟到半小时或更多。 即使你打算将发布到第三方托管提供程序而不是到 Azure，由于该 SDK 包括最新的更新到 Visual Studio web 发布功能，你需要 Azure SDK。

> [!NOTE]
> 本教程使用的 Azure sdk 版本 1.8.1 编写。 从那时起已发布具有附加功能的较新版本。 已更新教程需要提到这些功能和对具有它们的详细信息的资源的链接。


说明和屏幕截图基于 Windows 8 中，但本教程说明适用于 Windows 7 的差异。

若要完成本教程中，要求一些其他软件，但无需使用，它尚未安装。 本教程将引导你完成在需要时安装它的步骤。

## <a name="download-the-sample-application"></a>下载示例应用程序

将部署的应用程序名为 Contoso 大学和已为你创建。 它是大学网站上，松散基于 Contoso 大学应用程序中所述的简化的版本[ASP.NET 站点上的实体框架教程](https://asp.net/entity-framework/tutorials)。

在必须安装的先决条件，请下载[Contoso 大学 web 应用程序](https://go.microsoft.com/fwlink/p/?LinkId=282627)。 *.Zip*文件包含多个版本的项目。 若要完成本教程的步骤，请位于 C# 文件夹中的项目开始。 若要查看项目的教程末尾的显示效果，请 ContosoUniversity 最终文件夹中打开项目。

若要准备教程中的步骤在处理项目，请执行以下步骤：

1. 从 C# 文件夹中名为 ContosoUniversity 用于处理 Visual Studio 项目使用任何文件夹中的文件夹中保存 ContosoUniversity 解决方案文件。

    默认情况下这是用于 Visual Studio 2012 的以下文件夹：

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (在本教程屏幕截图，为项目文件夹位于的根目录中`C`： 驱动器。)
2. 启动 Visual Studio 并打开该项目。
3. 在**解决方案资源管理器**，右键单击该解决方案，然后单击**EnableNuGet 程序包还原**。
4. 生成解决方案。
5. 如果你将得到编译错误，请手动还原 NuGet 包：

    1. 在**解决方案资源管理器**，右键单击该解决方案，然后单击**管理解决方案的 NuGet 包**。
    2. 在顶部**管理 NuGet 包**对话框中，你将看到**是此解决方案中缺少某些 NuGet 包。单击此项可还原。** 单击**还原**按钮。
    3. 重新生成解决方案。
6. 按 CTRL + F5 运行该应用程序。

    Contoso 大学主页上，打开应用程序。

    ![主页上开发人员](introduction/_static/image1.png)

    （可能有一个等待时间时 Visual Studio 启动 SQL Server Express LocalDB 实例中，并且你可能收到超时错误如果，过程耗时太长。 在此情况下只是启动项目再次。）

网页可从菜单栏访问并且允许你执行以下功能：

- 显示学生统计信息 （关于页）。
- 显示、 编辑、 删除和添加学生。
- 显示和编辑课程。
- 显示和编辑教师。
- 显示和编辑部门。

以下是几个具有代表性的页面的屏幕截图。

![学生页上的开发人员](introduction/_static/image2.png)

![添加学生页开发人员](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>查看影响部署的应用程序功能

应用程序的以下功能会影响你部署的方式，或者必须要做，以将其部署。 其中每个是详细说明了在序列中的以下教程。

- Contoso 大学使用 SQL Server 数据库来存储应用程序数据，如学生和教师名称。 数据库包含多种测试数据和生产数据，并部署到生产环境时，需要以排除测试数据。
- 应用程序使用 ASP.NET 成员身份系统，将用户帐户信息存储在 SQL Server 数据库。 应用程序定义的管理员用户有权访问某些受限制的信息。 你需要部署成员资格数据库测试帐户而非使用管理员帐户。
- 应用程序使用第三方的错误日志记录和报表实用程序。 一个程序集它必须与应用程序部署中提供了此实用程序。
- 错误日志记录实用程序将错误信息 XML 文件中写入的文件文件夹。 你必须确保 ASP.NET 下运行在已部署的站点中的帐户到此文件夹具有写入权限，并且你需要从部署中排除此文件夹。 （否则为在测试环境中的错误日志数据可能会部署到生产环境和/或生产错误日志文件可能会删除。）
- 该应用程序包括必须更改某些设置在部署*Web.config*具体取决于目标环境 （测试、 过渡或生产） 和其他设置，必须根据生成发生更改的文件配置 （调试或发布）。
- Visual Studio 解决方案包括一个类库项目。 应该部署此项目生成的程序集，不是项目本身。

## <a name="summary"></a>摘要

在此序列中的第一个教程中，你已下载示例的 Visual Studio 项目，并查看站点功能会影响你部署应用程序的方式。 在以下教程中，你为部署准备通过以下操作来自动处理的某些设置。 其他您采取措施的手动。

>[!div class="step-by-step"]
[下一篇](preparing-databases.md)
