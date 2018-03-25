---
uid: whitepapers/aspnet-web-deployment-content-map
title: "ASP.NET Web 部署的推荐资源 |Microsoft 文档"
author: rick-anderson
description: "本主题提供有关如何部署的资源 （发布） ASP.NET web 文档的链接到 IIS 的应用程序通过使用 Visual Studio 2010 中，Visual Web De..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2014
ms.topic: article
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: 78ff183394b5ff92f789b50551d01d28f9bff93b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment---recommended-resources"></a>ASP.NET Web 部署的推荐资源
====================
> 本主题提供有关如何部署的资源 （发布） ASP.NET web 文档的链接到 IIS 的应用程序通过使用 Visual Studio 2010、 Visual Web Developer 2010 和更高版本。
> 
> 如果你知道出色的博客文章， [stackoverflow](http://stackoverflow.com)线程或都非常有用的任何其他链接[向我们发送一封电子邮件](mailto:aspnetue@microsoft.com?subject=Deployment Content Map)链接。
> 
> > [!NOTE] 
> > 
> > 许多这些资源描述仅安装最新版本时才可用的部署功能[Visual Studio Web 发布更新](https://go.microsoft.com/fwlink/?LinkID=208120)。 某些功能是仅在 Visual Studio 2012 或 Visual Studio 2013 中可用。


本主题包含以下各节：

- [了解有关 web 项目的部署选项](#understanding)
- [查找托管 ASP.NET 应用程序提供商](#findinghosting)
- [部署 web 应用程序从 Visual Studio](#fromvs)
- [部署 web 应用程序创建和安装 web 部署包](#package)
- [部署 web 应用程序使用持续集成 (CI) 过程](#ci)
- [使用 Web.config 转换来更改在部署过程中的目标 Web.config 文件或 app.config 文件的设置](#transforms)
- [使用 Web 部署参数来更改目标 web 应用程序在部署过程中的设置](#webdeployparms)
- [在部署过程中确保应用程序是脱机](#appoffline)
- [将数据库或更改部署到作为 web 应用程序部署的一部分的数据库](#databasewithweb)
- [部署 web 应用程序部署单独的数据库](#databaseseparate)
- [部署的 web 应用程序使用 ASP.NET 应用程序服务如成员资格和分析](#aspnetmembership)
- [为部署预编译](#precompiling)
- [部署的 intranet web 应用程序](#intranet)
- [自动执行常见系统不会自动在初始状态下的部署任务](#automating)
- [配置 web 服务器，使开发人员可以部署到它们使用 Web Deploy 的 web 应用程序](#configuringservers)
- [将服务器配置宿主提供程序](#hostingprovider)
- [故障排除部署问题](#troubleshooting)
- [获取与特定部署问题的帮助](#gettinghelp)
- [其他资源](#additional)


<a id="understanding"></a>


## <a name="understanding-deployment-options-for-web-projects"></a>了解有关 web 项目的部署选项

- [Visual Studio 和 ASP.NET 的 web 部署概述](https://msdn.microsoft.com/library/dd394698.aspx)(MSDN)。
- [如何部署 Windows Azure 网站](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy)。 介绍选项和链接的资源部署到 Windows Azure 网站，包括的 web 项目[持续交付](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)(自动从[源代码管理](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)) 以及使用 Visual Studio。
- [Visual Studio 2012 Web 发布改进](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md)（Scott Hanselman 的视频）。
- [在 VS 2010 中的 Web 部署概述 Post](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) （Vishal Joshi 博客）。 较旧的博客文章，但某些 Visual Studio 2010 资源链接到它具有仍适用于 Visual Studio 2012 的信息。


<a id="findinghosting"></a>


## <a name="finding-hosting-providers-for-an-aspnet-application"></a>查找托管 ASP.NET 应用程序提供商

- [ASP.NET 承载](https://asp.net/hosting)


<a id="fromvs"></a>


## <a name="deploying-a-web-application-from-visual-studio"></a>部署 web 应用程序从 Visual Studio

- [如何部署 Windows Azure 网站](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy)。 介绍选项并提供对资源的链接部署到 Windows Azure 网站的 web 项目。 包含有关从 Visual Studio 部署的部分。
- [使用 Visual Studio 的 ASP.NET Web 部署](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)。 12 篇的系列教程，演示如何部署 web 应用程序与 SQL Server 数据库。 数据库部署使用 dbDacFx 提供程序和 Entity Framework Code First 迁移。 此外包含下列相关信息[Web.config 文件转换](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)，[部署单个文件](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles)，[命令行部署](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md)，和[如何自定义 Visual Studio web 发布管道通过编辑.pubxml 文件](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md)。 适用于所有 ASP.NET web 项目，包括 Web 窗体、 MVC 和 Web API。）
- [如何： 部署 Web 项目使用一键式发布 Visual Studio 中](https://msdn.microsoft.com/library/dd465337.aspx)（引用为 Visual Studio Web 发布向导的信息。）
- [部署具有 SQL Server Compact 使用 Visual Studio 的 ASP.NET Web 应用程序](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md)。 这是早期版本的**使用 Visual Studio 的 ASP.NET Web 部署**在本部分的顶部列出。 主要用于现在有关如何部署 SQL Server Compact 数据库以及如何将从 SQL Server Compact 迁移到 SQL Server 的完整版的信息。
- [.NET 多层应用程序使用存储表、 队列和 Blob](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) （Microsoft Azure 网站）。 5 篇的系列教程，演示如何创建 MVC 项目并将其部署到 Windows Azure 云服务。


<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>部署 web 应用程序创建和安装 web 部署包

- [如何： 在 Visual Studio 中创建 Web 部署包](https://msdn.microsoft.com/library/dd465323.aspx)(MSDN)。
- [如何： 通过 Visual Studio 创建一个使用 deploy.cmd 文件的部署包的安装](https://msdn.microsoft.com/library/ff356104.aspx)(MSDN)。
- [使用 Web 部署包部署到开发人员框上的 IIS 和第三方主机](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx)（Sayed Hashimi 博客）。 如何使用 IIS 管理器在 IIS 中的部署包安装在本地计算机上和在另一托管公司，用于远程管理支持 IIS 管理器。
- [生成将 Web 部署包从 Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) （IIS.NET 网站）。 包括有关命令行包创建和安装说明。
- [包一次发布任何地方](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx)（Sayed Hashimi 博客）。 引入了 NuGet 包，以便可以将一个包部署到多个服务器转换为多个目标环境中，Web.config 文件的过程进行自动化。 另请参阅[PackageWeb 视频](https://www.youtube.com/watch?v=-LvUJFI8CzM)Sayed Hashimi 通过。

另请参阅以下部分。


<a id="ci"></a>


## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>部署 web 应用程序使用持续集成 (CI) 过程

- [持续集成和持续交付 （使用 Windows Azure 构建真实世界云应用）。](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) 电子书章节，其中引入了持续集成和持续交付。
- [如何部署 Windows Azure 网站](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy)。 介绍选项和链接的资源部署到 Windows Azure 网站的 web 项目的方法。 包含有关自动执行从源代码管理部署的部分。
- [部署在企业方案中的 Web 应用程序](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。 40 篇的系列教程，演示如何在使用 Visual Studio 2010 和 Team Foundation Server 2010 CI 过程自动执行部署。
- [深入了解 Microsoft 生成引擎： 使用 MSBuild 和 Team Foundation Build，通过 Sayed Hashimi 和 William Bartholomew](http://msbuildbook.com)。 这是一本书中，web 资源，但它是非常重要的指南以了解如何针对持续集成方案对 MSBuild 进行配置。
- [MSBuild 扩展包](https://github.com/mikefourie/MSBuildExtensionPack)。 包括部署任务。
- [Team Foundation 生成自定义指南](https://aka.ms/vsarsolutions)。 由 ALM Rangers 上设置 Team Foundation Server 的文档包含 web 部署，并包含教程和视频。
- [SlowCheetah XML 转换从 CI 服务器](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx)（Sayed Hashimi 博客）。 说明如何使用 SlowCheetah，Visual Studio 外接程序对于转换 app.config 和其他 XML 文件。

另请参阅[在部署过程中确保应用程序是脱机](aspnet-web-deployment-content-map.md#appoffline)更高版本在此页中。


<a id="transforms"></a>


## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>使用 Web.config 转换来更改在部署过程中的目标 Web.config 文件或 app.config 文件的设置

- [Web.config 文件转换](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)。
- [使用 Visual Studio Web 项目部署的 Web.config 转换语法](https://msdn.microsoft.com/library/dd465326.aspx)(MSDN)。
- [Web 工具 2012.2-web.config 转换](https://www.youtube.com/watch?v=HdPK8mxpKEI)（通过 Sayed Hashimi YouTube 视频）。 演示如何设置和预览 Web.config 转换。
- [如何禁用 Web.config 转换？](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN)。
- [何时应使用 Web 部署参数而不是 Web.config 转换？](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN)。
- [XDT （XML 文档转换） 发布在 codeplex.com 上](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx)（.NET Web 开发和工具博客）。 宣布推出的 Web.config 文件转换引擎的源代码的可用性，并列出了一些使用它的工具。
- [Windows Azure 网站： 如何应用程序字符串和连接字符串的工作原理](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)（Microsoft Azure 博客）。 如果你的目标环境是 Windows Azure 网站和你想要转换，将转换 Web.config 的替代方法`appSettings`或`connectionStrings`。


<a id="webdeployparms"></a>


## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>使用 Web 部署参数来更改目标 web 应用程序在部署过程中的设置

- [如何： 使用 Web 部署 Web 部署包中的参数](https://msdn.microsoft.com/library/ff398068.aspx)(MSDN)。
- [MSDeploy： 发布配置文件上如何更新上发布的应用程序设置基于](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx)（Sayed Hashimi 博客）。 演示如何将 Web 部署参数到 Visual Studio 中发布配置文件。
- [Web 部署参数化](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization)（IIS.NET 网站）。
- [Web 部署操作中的参数化](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html)（Vishal Joshi 博客）。
- [Web 部署参数化 vs。Web.config 转换](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html)（Vishal Joshi 博客）。
- [Windows Azure 网站： 如何应用程序字符串和连接字符串的工作原理](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)（Microsoft Azure 博客）。 Web 的替代方法部署参数，如果你的目标环境是 Windows Azure 网站和你想要参数化`appSettings`或`connectionStrings`。


<a id="appoffline"></a>


## <a name="making-sure-an-application-is-off-line-during-deployment"></a>在部署过程中确保应用程序是脱机

- [使用 Visual Studio 的 ASP.NET Web 部署： 部署某一代码更新](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md)。 请参阅明**使应用程序脱机在部署过程。**
- [脱机应用程序在发布前](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing)（IIS.net 网站）。 介绍了内置 Web 部署 3.0 自动执行的应用程序的处理功能\_offline.htm 文件。 此功能不适用于自定义应用\_offline.htm 文件。
- [如何在发布过程中执行 web 应用程序脱机](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx)（Sayed Hashimi 博客）。 如何使用自定义应用的过程中自动运行\_offline.htm 文件。
- [Web 应用程序脱机和 usechecksum 发布更新](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx)（Microsoft Web 开发博客）。 用于自动执行的应用程序使用的另一个选项\_offline.htm 文件。
- [Web 部署 3.5 RTW](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) （IIS.net 网站）。 Web 部署 3.5 中自定义应用程序的新功能\_offline.htm 文件。


<a id="databasewithweb"></a>


## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>将数据库或更改部署到作为 web 应用程序部署的一部分的数据库

- [在 Visual Studio 中配置数据库部署](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment)(MSDN)。 部署 web 项目具有的数据库的选项的概述。
- [使用 Visual Studio 的 ASP.NET Web 部署](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)。 12 篇的系列教程，说明数据库部署使用 dbDacFx 提供程序和 Entity Framework Code First 迁移。
- [如何： 部署 Web 项目使用一键式发布 Visual Studio 中](https://msdn.microsoft.com/library/dd465337.aspx)(MSDN)。
- [将包含成员资格、 OAuth 和 SQL 数据库的安全 ASP.NET MVC 5 应用程序部署到 Windows Azure 网站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 生成并部署使用单个 SQL Server 的应用程序的长教程数据库同时用于成员资格和应用程序数据。
- [部署具有 SQL Server Compact 使用 Visual Studio 的 ASP.NET Web 应用程序](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md)。 12 篇的系列教程，介绍如何部署 SQL Server Compact 数据库，以及如何从 SQL Server Compact 将迁移到 SQL Server 的完整版。

请参阅还将 web 应用程序部署通过创建和安装 web 部署包和部署 web 应用程序使用持续集成 (CI) 过程之前在此页。


<a id="databaseseparate"></a>


## <a name="deploying-a-database-separately-from-web-application-deployment"></a>部署 web 应用程序部署单独的数据库

- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN)。
- [SQL Server 数据库项目中包括数据](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx)（SQL Server Data Tools 团队博客）。 如何将数据库部署时部署架构和数据。
- [如何将数据库部署到 Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) （Microsoft Azure 网站）
- [将数据库迁移到 Windows Azure SQL 数据库 (以前称为 SQL Azure)](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN)。
- [将数据库迁移到 SQL Azure 中使用 SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) （SQL Server Data Tools 团队博客）。
- [以数据为中心应用程序迁移到 Windows Azure](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN)。
- [将 SQL Server 数据库迁移到 Windows Azure SQL 数据库](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx)(MSDN)。


<a id="aspnetmembership"></a>


## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>部署的 web 应用程序使用 ASP.NET 应用程序服务如成员资格和分析

- [将包含成员资格、 OAuth 和 SQL 数据库的安全 ASP.NET MVC 5 应用程序部署到 Windows Azure 网站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 生成并部署使用单个 SQL Server 的应用程序的长教程数据库同时用于成员资格和应用程序数据。
- [ASP.NET 标识](https://asp.net/identity/)。 ASP.NET 标识资源。
- [使用 Visual Studio 的 ASP.NET Web 部署](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)。 12 篇的系列教程，说明如何将部署 ASP.NET 成员资格数据库。
- [配置网站，以使用应用程序服务](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md)。 对于网站项目但也是适用于 web 应用程序项目。
- [用户和生产网站上的角色](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md)。 对于网站项目但也是适用于 web 应用程序项目。


<a id="precompiling"></a>


## <a name="precompiling-for-deployment"></a>为部署预编译

- [ASP.NET Web 应用程序项目预编译概述](https://msdn.microsoft.com/library/aa983464.aspx)(MSDN)。
- [打包/发布 Web 选项卡上，项目属性](https://msdn.microsoft.com/library/dd410108.aspx)(MSDN)。
- [高级预编译设置对话框](https://msdn.microsoft.com/library/hh475319.aspx)(MSDN)。


<a id="intranet"></a>


## <a name="deploying-an-intranet-web-application"></a>部署的 intranet web 应用程序

- [将本地组织的身份验证选项 (ADFS) 与在 Visual Studio 2013 的 ASP.NET 中使用](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)（通过 Vittorio Bertocci 的博客。）。
- [如何创建 Intranet 站点使用 ASP.NET MVC](https://msdn.microsoft.com/library/gg703322(VS.98).aspx) (MSDN)。 对于 Visual Studio 2010，较旧的演练写入不会反映在 Visual Studio 2013 中引入的 intranet 项目模板中的重大更改。


<a id="automating"></a>


## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>自动执行常见系统不会自动在初始状态下的部署任务

- [使用 Visual Studio 的 ASP.NET Web 部署： 部署额外文件](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md)。
- [Web 发布设置文件夹权限](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx)（Sayed Hashimi 博客）。
- [如何扩展要包括的 web 项目包注册表设置的目标文件](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx)（Web 开发工具博客）。
- [扩展 XML (Web.config) 转换](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx)（Sayed Hashimi 博客）。 演示如何创建自定义 XDT 转换。
- [Web 部署工具 (MSDeploy) 自定义提供程序采用 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) （Sayed Hashimi 博客）。 演示如何创建 Web 部署的自定义提供程序。
- [如何打包和部署 COM 组件](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx)（Web 开发工具博客）。
- [包.NET 程序集如何](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx)（Web 开发工具博客）。 如何部署到 GAC 的程序集。
- [编写脚本的所有内容-初始化 Your Windows Azure VM，为 IIS、 Web 部署和其他内容使用你的 Web 服务器](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff)（Tugberk Ugurlu 博客）。


<a id="configuringservers"></a>


## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>配置 web 服务器，使开发人员可以部署到它们使用 Web Deploy 的 web 应用程序

- [管理员和非管理员部署的安装和配置 Web 部署](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy)（IIS.net 网站）。


<a id="hostingprovider"></a>


## <a name="configuring-servers-for-a-hosting-provider"></a>将服务器配置宿主提供程序

- [Microsoft ASP.NET 4 托管部署指南 》](https://go.microsoft.com/fwlink/?LinkId=191365) （Microsoft 下载中心）。
- [生成配置文件 XML 文件](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file)（IIS.net 网站）。


<a id="troubleshooting"></a>


## <a name="troubleshooting-deployment-problems"></a>故障排除部署问题

- [Visual Studio 中的 Windows Azure 网站进行故障排除](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)（Microsoft Azure 网站）。
- [使用 Visual Studio 的 ASP.NET Web 部署： 故障排除](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md)。
- [故障排除的常见问题的 Web 部署](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy)。
- [Web 部署错误代码](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes)（IIS.net 网站）。
- [Web 部署常见问题 Visual Studio 和 ASP.NET 的](https://msdn.microsoft.com/library/ee942158.aspx)(MSDN)。
- [核心 IIS 和 ASP.NET 开发服务器之间的差异](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md)。
- [开发和生产环境之间的常见配置差异](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md)。
- [承载 ASP.NET 应用程序在中等信任](http://www.4guysfromrolla.com/articles/100307-1.aspx)（4 专家从 Rolla 站点）。


<a id="gettinghelp"></a>


## <a name="getting-help-with-a-specific-deployment-question"></a>获取与特定部署问题的帮助

- [ASP.NET 配置和部署论坛](https://forums.asp.net/26.aspx/1?Configuration and Deployment)。
- [StackOverflow.com](http://www.StackOverflow.com)。


<a id="additional"></a>


## <a name="additional-resources"></a>其他资源

本部分提供可用于更多地了解如何使用 Visual Studio 和 IIS 部署工具的其他资源的链接。

以下博客经常包含有关 Visual Studio web 部署的信息：

- [Microsoft 博客 web 开发工具](https://blogs.msdn.com/b/webdevtools/)。
- [Sayed Hashimi 博客](http://www.sedodream.com/)。

以下资源提供了有关 Web 部署，Visual Studio 用于执行 web 应用程序项目部署任务的 IIS framework 文档。 你可以提问有关 Web 部署在[Web 部署工具论坛](https://go.microsoft.com/fwlink/?LinkId=149411)IIS.net 网站上。

- [简介 Web 部署](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy)。
- [安装和配置 Web 部署](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy)。
- [PowerShell 脚本自动执行 Web 部署安装](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup)。
- [Web 部署工具](https://go.microsoft.com/fwlink/?LinkId=151481)。 Web 部署 TechNet 网站上的文档的内容节点的顶级表。 包括的有用参考信息，但大部分 TechNet 页面尚未更新年。
- [Microsoft.Web.Deployment Namespace](https://go.microsoft.com/fwlink/?LinkId=148630)。 自版本 1.0 以来尚未更新 API 文档。
- [Microsoft Web 部署团队博客](https://blogs.iis.net/msdeploy/default.aspx)。
- [IIS.net 网站中发布选项卡](https://www.iis.net/learn/publish)。
