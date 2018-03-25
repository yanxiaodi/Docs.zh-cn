---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: "部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 设置文件夹权限-6 12 |Microsoft 文档"
author: tdykstra
description: "这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Stu 包含 SQL Server Compact 数据库..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 42085fff5f1aed1440f49e1e2ceee0cf0e751e2c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 设置文件夹权限-6 12
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 中包含 SQL Server Compact 数据库。 如果你安装 Web 发布更新，也可以使用 Visual Studio 2010。 有关序列的简介，请参阅[序列中的第一个教程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 显示部署功能后，将 Visual Studio 2012 RC 版本中推出，演示如何部署 SQL Server Compact 以外的 SQL Server 版本并演示如何将部署到 Azure App Service Web Apps 的教程，请参阅[的 ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>概述

在本教程中，你设置的文件夹权限*Elmah*文件夹中已部署的 web 站点，以便应用程序可以在该文件夹中创建日志文件。

当你使用 Visual Studio 开发服务器 (用于 Cassini) 的 Visual Studio 中测试 web 应用程序时，应用程序运行你的身份。 你是开发计算机上的最有可能的管理员并具有完整权限对任何文件夹中的任何文件执行任何操作。 但它时在 IIS 下运行应用程序，在定义为站点分配给应用程序池的标识下运行。 这通常是一个系统定义的帐户具有有限的权限。 默认情况下，它具有读取和执行权限 web 应用程序的文件和文件夹，但它不具有写访问权限。

如果你的应用程序创建或更新文件，这是一个常见需要 web 应用程序中，这将成为问题。 在 Contoso 大学应用程序，Elmah 创建中的 XML 文件*Elmah*才能保存有关错误的详细信息的文件夹。 即使不使用 Elmah 类似，您的网站可能允许用户将文件上载或执行将数据写入到你网站的文件夹中其他任务。

提示： 如果你收到如下错误消息，或当你完成本教程的内容不起作用，请务必检查[故障排除页](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="testing-error-logging-and-reporting"></a>测试错误日志记录和报表

若要查看如何应用程序无法正常工作在 IIS 中 （尽管它未在 Visual Studio 中测试它时），你可能会导致的错误，通常会记录的 Elmah，然后打开 Elmah 错误日志，以查看详细信息。 如果 Elmah 无法创建 XML 文件和存储的错误详细信息，你会看到空的错误报告。

打开浏览器并转到`http://localhost/ContosoUniversity`，然后请求的无效 URL *Studentsxxx.aspx*。 请参阅而不是系统生成的错误页*GenericErrorPage.aspx*页，因为`customErrors`Web.config 文件中的设置为"RemoteOnly"和本地运行 IIS:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

现在运行*Elmah.axd*若要查看错误报表。 因为 Elmah 无法创建 XML 文件中的，你会看到一个空的错误日志页面*Elmah*文件夹：

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>设置 Elmah 文件夹的写权限

你可以手动设置文件夹权限，或使其自动部署过程的一部分。 使其自动需要复杂的 MSBuild 代码，并且由于只需执行此操作首次部署，因此，本教程仅演示如何执行此手动操作。 (有关如何进行此部分部署过程的信息，请参阅[设置文件夹权限 Web 发布](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx)Sayed Hashimi 博客上。)

在**Windows 资源管理器**，导航到*C:\inetpub\wwwroot\ContosoUniversity*。 右键单击*Elmah*文件夹，选择**属性**，然后选择**安全**选项卡。

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(如果看不到**DefaultAppPool**中**组或用户名**列表中，你可能使用比指定在本教程中的其他某种方法来设置您的计算机上的 IIS 和 ASP.NET 4。 在这种情况下，找出哪些标识由应用程序池分配给 Contoso 大学应用程序和该标识授予写入权限。 请参阅有关应用程序池标识链接在本教程末尾。）

单击“编辑” 。 在**权限 Elmah**对话框中，选择**DefaultAppPool**，然后选择**编写**中的复选框**允许**列。

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

单击**确定**两个对话框中。

## <a name="retesting-error-logging-and-reporting"></a>重新测试错误日志记录和报表

测试通过再次导致错误相同的方式 （请求错误 URL），然后运行**错误日志**页。 这一次错误显示在此页中。

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

此外需要编写上的权限*应用\_数据*文件夹因为该文件夹中具有 SQL Server Compact 数据库文件，并且你想要能够更新这些数据库中的数据。 在这种情况下，但是，你无需执行任何额外操作，因为部署过程会自动设置上的写权限*应用\_数据*文件夹。

你现在已经完成获取 Contoso 大学所必需的任务的所有本地计算机上在 IIS 中正常运行。 在下一步的教程中，你会使站点公开可通过将其部署到托管提供商。

## <a name="more-information"></a>详细信息

在此示例中，Elmah 已无法保存日志文件的原因是相当明显的。 你可以在其中的问题的原因并不那么明显; 的情况下使用 IIS 跟踪请参阅[故障排除失败的请求使用跟踪在 IIS 7 中](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)IIS.net 网站上。

有关如何向应用程序池标识授予权限的详细信息，请参阅[应用程序池标识](https://www.iis.net/learn/manage/configuring-security/application-pool-identities)和[中通过文件系统 Acl IIS 安全内容](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls)IIS.net 网站上。

>[!div class="step-by-step"]
[上一页](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
[下一页](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
