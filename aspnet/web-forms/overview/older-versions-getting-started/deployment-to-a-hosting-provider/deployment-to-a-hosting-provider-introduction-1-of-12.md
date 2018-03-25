---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: "部署具有 SQL Server Compact 使用 Visual Studio 的 ASP.NET Web 应用程序： 简介-1 12 |Microsoft 文档"
author: tdykstra
description: "这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Stu 包含 SQL Server Compact 数据库..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: a0f38c83bd9231dbd37d3d505c90316af521b336
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>部署具有 SQL Server Compact 使用 Visual Studio 的 ASP.NET Web 应用程序： 简介-1 12
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 中包含 SQL Server Compact 数据库。 如果你安装 Web 发布更新，也可以使用 Visual Studio 2010。
> 
> 显示部署功能后，将 Visual Studio 2012 RC 版本中推出，演示如何部署 SQL Server Compact 以外的 SQL Server 版本并演示如何将部署到 Azure App Service Web Apps 的教程，请参阅[的 ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。
> 
> 这些教程将指导你完成第一次部署到测试，在本地开发计算机上的 IIS，然后执行到第三方托管提供商。 将部署的应用程序使用的应用程序数据库和 ASP.NET 成员资格数据库。 你首先使用 SQL Server Compact 和部署到 SQL Server Compact 和更高版本的教程介绍如何将数据库更改部署以及如何迁移到 SQL Server。
> 
> 教程也假设你知道如何使用 Visual Studio 中的 ASP.NET。 如果不这样做，是要启动一个好[基本的 ASP.NET Web 窗体教程](../tailspin-spyworks/tailspin-spyworks-part-1.md)或[基本的 ASP.NET MVC 教程](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)。
> 
> 如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET 部署论坛](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)。


## <a name="overview"></a>概述

这些教程将指导你完成第一次部署到测试，在本地开发计算机上的 IIS，然后执行到第三方托管提供商。 将部署的应用程序使用的应用程序数据库和 ASP.NET 成员资格数据库。 你首先使用 SQL Server Compact 和部署到 SQL Server Compact 和更高版本的教程介绍如何将数据库更改部署以及如何迁移到 SQL Server。

数的教程 – 在所有 11 以及故障排除页 – 可能会使部署过程看起来项极其困难。 事实上，部署站点的基本过程构成了教程集的相对较小一部分。 但是，在实际情况下，你经常需要部署的一些小但很重要的额外方面有关的信息 — 例如，在目标服务器上设置文件夹权限。 我们介绍了许多这些其他技术在教程中，可能会阻止您成功地部署实际的应用程序的信息不省略教程希望使用。

这些教程旨在运行在序列中，并且每个部分基于前面部分。 但是，你可以跳过不到你的情况相关的部分。 （跳过部分可能需要更高版本的教程中调整的过程。）

## <a name="intended-audience"></a>目标的受众

这些教程旨在 ASP.NET 开发人员在小型组织或其他环境中工作其中：

- 不使用持续集成过程 （自动的生成和部署）。
- 生产环境是第三方托管提供商。
- 通常，一个人会充当多个角色 （同一个人开发、 测试，并将部署）。

在企业环境中，它是更常见的做法实现持续集成过程，生产环境通常由公司自己的服务器承载的。 不同的人也通常可以执行不同的角色。 有关企业部署的信息，请参阅[企业方案中部署 Web 应用程序](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。

所有规模的组织还可以将部署 web 应用程序到 Azure，并显示这些教程中的过程的大部分同样适用于 Azure 应用程序服务 Web Apps。 有关 Azure 的简介，请参阅[https://azure.microsoft.com](https://azure.microsoft.com)。

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>托管提供商教程中所示

这些教程将指导你完成设置具有托管公司的帐户和应用程序部署到该宿主提供程序的过程。 特定的托管公司已选择，以便这些教程无法阐释的部署到实时网站的完整体验。 每个托管公司提供不同的功能，并向其服务器部署的体验不同而异某种程度上;但是，在本教程中所述的过程是典型的整体过程。

对于本教程，Cytanium.com，使用的宿主提供程序是一个许多可用，而在本教程中的使用它，不会导致的认可或推荐。

## <a name="deploying-web-site-projects"></a>部署网站项目

Contoso 大学是一个 Visual Studio web 应用程序项目。 大部分的部署方法和在本教程中演示的工具不适用于[网站项目](https://msdn.microsoft.com/library/dd547590.aspx)。 有关如何部署网站项目的信息，请参阅[ASP.NET 部署内容映射](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects)。

## <a name="deploying-aspnet-mvc-projects"></a>部署 ASP.NET MVC 项目

对于本教程将部署一个 ASP.NET Web 窗体项目，但了解如何执行的所有内容都是也适用于 ASP.NET MVC。 Visual Studio MVC 项目是另一个窗体的 web 应用程序项目。 唯一的区别是，如果你正在将部署到的宿主提供程序不支持 ASP.NET MVC 或它的目标版本中，你必须确保你已安装相应 ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0)或[MVC 4](http://nuget.org/packages/aspnetmvc)) 项目中的 NuGet 包。

## <a name="programming-language"></a>编程语言

示例应用程序使用 C#，但这些教程不需要的 C# 的知识和教程所示的部署技术并非特定于语言的。

## <a name="troubleshooting-during-this-tutorial"></a>本教程过程中的疑难解答

在部署期间，发生错误时，或已部署的站点无法正常运行，则错误消息不始终提供的解决方案。 若要帮助您完成某些常见的问题情况下，[故障排除参考页](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)可用。 如果你收到如下错误消息，或当你完成本教程的内容不起作用，请务必检查故障排除的页面。

## <a name="comments-welcome"></a>注释欢迎

对教程的注释都欢迎，并更新本教程时将保持尽一切可能要考虑的帐户更正或改进教程注释中提供的建议。

## <a name="prerequisites"></a>系统必备

在开始之前，请确保你具有 Windows 7 或更高版本，并且在你的计算机上安装以下产品之一：

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web](https://go.microsoft.com/fwlink/?LinkId=240162)

如果你有 Visual Studio 2010 SP1 或 Visual Web Developer Express 2010 SP1，也安装以下产品：

- [Azure SDK for.NET (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) （包括 Web 发布更新）
- [Microsoft Visual Studio 2010 SP1 Tools for SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

若要完成本教程中，要求一些其他软件，但无需具有尚未加载。 本教程将引导你完成在需要时安装它的步骤。

## <a name="downloading-the-sample-application"></a>下载示例应用程序

你将部署应用程序名为 Contoso 大学，并已为你创建。 它是大学网站上，松散基于 Contoso 大学应用程序中所述的简化的版本[ASP.NET 站点上的实体框架教程](https://asp.net/entity-framework/tutorials)。

在必须安装的先决条件，请下载[Contoso 大学 web 应用程序](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)。 *.Zip*文件包含多个版本的项目和 PDF 文件，其中包含 12 的所有教程。 若要逐步完成本教程的步骤，请使用 ContosoUniversity 开始启动。 若要查看项目的教程末尾的显示效果，请打开 ContosoUniversity 结束。 若要查看项目迁移到完整的 SQL Server 在教程中 10 之前的显示效果，请打开 ContosoUniversity AfterTutorial09。

若要准备逐步完成教程步骤，将保存到用于处理 Visual Studio 项目使用任何文件夹的 ContosoUniversity Begin。 默认情况下这是以下文件夹：

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(在本教程屏幕截图，为项目文件夹位于的根目录中`C`： 驱动器。)

启动 Visual Studio，打开项目，并按 CTRL + F5 以运行它。

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

网页可从菜单栏访问并且允许你执行以下功能：

- 显示学生统计信息 （关于页）。
- 显示、 编辑、 删除和添加学生。
- 显示和编辑课程。
- 显示和编辑教师。
- 显示和编辑部门。

以下是几个具有代表性的页面的屏幕截图。

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>查看影响部署的应用程序功能

应用程序的以下功能会影响你部署的方式，或者必须要做，以将其部署。 其中每个是详细说明了在序列中的以下教程。

- Contoso 大学使用 SQL Server Compact 数据库来存储应用程序数据，如学生和教师名称。 数据库包含多种测试数据和生产数据，并部署到生产环境时，需要以排除测试数据。 更高版本在系列教程中你将从迁移 SQL Server Compact 到 SQL Server。
- 应用程序使用 ASP.NET 成员身份系统，将用户帐户信息存储在 SQL Server Compact 数据库。 应用程序定义的管理员用户有权访问某些受限制的信息。 你需要部署成员资格数据库没有测试帐户但有一个管理员帐户。
- 由于应用程序数据库和成员资格数据库则使用 SQL Server Compact 作为数据库引擎，因此你需要将数据库引擎部署到托管提供商，以及数据库自身。
- 应用程序使用 ASP.NET 通用成员资格提供程序，因此，成员资格系统可以将其数据存储在 SQL Server Compact 数据库。 必须与应用程序部署包含通用成员资格提供程序的程序集。
- 应用程序使用实体框架 5.0 以访问应用程序数据库中的数据。 包含实体框架 5.0 的程序集必须随应用程序一起部署。
- 应用程序使用第三方的错误日志记录和报表实用程序。 一个程序集它必须与应用程序部署中提供了此实用程序。
- 错误日志记录实用程序将错误信息 XML 文件中写入的文件文件夹。 你必须确保 ASP.NET 下运行在已部署的站点中的帐户到此文件夹具有写入权限，并且你需要从部署中排除此文件夹。 （否则为在测试环境中的错误日志数据可能会部署到生产环境和/或生产错误日志文件可能会删除。）
- 该应用程序包括必须更改某些设置在部署*Web.config*具体取决于目标环境 （测试或生产） 和其他设置，必须根据生成发生更改的文件配置 （调试或发布）。
- Visual Studio 解决方案包括一个类库项目。 应该部署此项目生成的程序集，不是项目本身。

在此序列中的第一个教程中，你已下载示例的 Visual Studio 项目，并查看站点功能会影响你部署应用程序的方式。 在以下教程中，你为部署准备通过以下操作来自动处理的某些设置。 其他您采取措施的手动。

>[!div class="step-by-step"]
[下一篇](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
