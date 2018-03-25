---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: "部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 配置项目属性-4 12 |Microsoft 文档"
author: tdykstra
description: "这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Stu 包含 SQL Server Compact 数据库..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5632b801586c13084f887c4c414fc8686731094c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 配置项目属性-4 12
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 中包含 SQL Server Compact 数据库。 如果你安装 Web 发布更新，也可以使用 Visual Studio 2010。 有关序列的简介，请参阅[序列中的第一个教程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 显示部署功能后，将 Visual Studio 2012 RC 版本中推出，演示如何部署 SQL Server Compact 以外的 SQL Server 版本并演示如何将部署到 Azure App Service Web Apps 的教程，请参阅[的 ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>概述

存储在项目文件中的项目属性中配置一些部署选项 ( *.csproj*或*.vbproj*文件)。 在大多数情况下，这些设置的默认值是所需的但你可以使用**项目属性**UI 内置于 Visual Studio 以使用这些设置，如果你需要更改它们。 在本教程中，你将查看中的部署设置**项目属性**。 你还创建一个占位符文件导致要部署的空文件夹。

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>在项目属性窗口中配置部署设置

影响会发生什么情况在部署过程的大多数设置都将纳入发布配置文件，正如在以下教程看到。 你应注意的几个设置位于**打包/发布**选项卡**项目属性**窗口。 每个生成配置指定了这些设置-即，具有不同的设置的发布版本数量超过拥有的调试版本。

在**解决方案资源管理器**，右键单击**ContosoUniversity**项目，依次选择**属性**，然后选择**打包/发布 Web**选项卡。

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

当显示窗口时，它默认为显示设置的任何生成配置当前处于活动状态的解决方案。 如果**配置**框没有表明**活动 （发行版）**，选择**版本**以显示发布生成配置设置。 你将部署到你的测试和生产环境的发布版本。

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

与**活动 （发行版）**或**版本**选择，请参阅部署使用发布生成配置时有效的值：

- 在**项部署**框中，**仅运行该应用程序所需的文件**选择。 其他选项**此项目中的所有文件**或**此项目文件夹中的所有文件**。 通过将保留不变的默认选择可以避免例如部署源代码文件。 此设置是为什么包含 SQL Server Compact 的二进制文件的文件夹必须包括在项目中的原因。 有关此设置的详细信息，请参阅**为什么不我的项目文件夹中的文件的所有部署呢？**中[ASP.NET Web 应用程序项目部署常见问题](https://msdn.microsoft.com/library/ee942158.aspx)。
- **排除生成调试符号**选择。 你不会使用此生成配置时调试。
- **将应用程序中排除文件\_数据文件夹**未选中。 用于成员资格数据库的 SQL Server Compact 文件是在该文件夹中，你必须先部署它。 当你部署不包括数据库更改的更新时，你将选择此复选框。
- **预编译此应用程序，然后发布**未选中。 在大多数情况下，没有必要预编译 web 应用程序项目。 有关此选项的详细信息，请参阅[打包/发布 Web 选项卡上，项目属性](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx)和[高级预编译设置对话框](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx)。
- **包括在打包/发布 SQL 选项卡中配置的所有数据库**选择，但此选项没有任何作用现在不配置因为**打包/发布 SQL**选项卡。该选项卡是唯一的选项来部署 SQL Server 数据库所用的旧数据库部署方法。 你将使用**打包/发布 SQL**选项卡中[迁移到 SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)教程。
- **Web 部署包设置**部分不适用于因为您正在使用一键式发布这些教程中。

更改**配置**调试以查看默认设置为调试版本下拉列表框。 值是相同的除**排除生成的调试符号**，以便你能够调试时部署调试版本处于未选中状态。

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>发出确保 Elmah 文件夹获取部署

正如你在前面的教程，看到[Elmah NuGet 包](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)提供错误日志记录和报表功能。 Contoso 大学应用程序中 Elmah 已配置为将错误详细信息存储在名为的文件夹*Elmah*:

![Elmah 文件夹](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

从部署中排除特定文件或文件夹是常见的要求;另一个示例是用户可以将文件上载到的文件夹。 你不希望日志文件，或在开发环境以便部署到生产环境中创建的文件上载。 并且，如果您要将更新部署到生产您不想删除在生产环境中存在的文件的部署过程。 （具体取决于如何设置部署选项，如果在部署时，目标站点，但不是源站点中存在的文件，Web 部署将其删除的目标。）

正如你此前在本教程中，看到**项部署**选项**打包/发布 Web**选项卡设置为**只需运行此应用程序文件**。 因此，在开发过程中创建的 Elmah 的日志文件将不部署，这是你想要发生这种情况。 (若要部署，他们可能需要包括在项目且其**生成操作**属性必须设置为**内容**。 有关详细信息，请参阅**为什么不我的项目文件夹中的文件的所有部署呢？**中[ASP.NET Web 应用程序项目部署常见问题](https://msdn.microsoft.com/library/ee942158.aspx))。 但是，Web 部署不会创建一个文件夹在目标站点中除非存在至少一个文件将复制到它。 因此，你将添加*.txt*使其作为一个占位符，以便将复制文件夹的文件夹的文件。

在**解决方案资源管理器**，右键单击*Elmah*文件夹，选择**添加新项**，并创建名为的文件*Placeholder.txt*。 放入其中的以下文本:"这是一个占位符文件，以确保文件夹获取部署"。 并保存文件。 这就是你所要做为了确保 Visual Studio 部署此文件和文件夹，因为所有**生成操作**属性*.txt*文件设置为**内容**默认情况下。

你现在已经完成的所有部署设置任务。 在下一步的教程中，你将 Contoso 大学网站部署到测试环境，并对其进行测试。

>[!div class="step-by-step"]
[上一页](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
[下一页](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
