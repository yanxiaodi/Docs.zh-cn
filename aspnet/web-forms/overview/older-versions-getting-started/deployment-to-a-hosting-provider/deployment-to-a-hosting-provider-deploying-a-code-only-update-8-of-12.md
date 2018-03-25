---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: "部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 部署 Code-Only 更新-8 12 |Microsoft 文档"
author: tdykstra
description: "这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Stu 包含 SQL Server Compact 数据库..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: a07ac968482866ba9a4eca25722d99fd641080e6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 部署 Code-Only 更新-8 12
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 中包含 SQL Server Compact 数据库。 如果你安装 Web 发布更新，也可以使用 Visual Studio 2010。 有关序列的简介，请参阅[序列中的第一个教程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 显示部署功能后，将 Visual Studio 2012 RC 版本中推出，演示如何部署 SQL Server Compact 以外的 SQL Server 版本并演示如何将部署到 Azure App Service Web Apps 的教程，请参阅[的 ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>概述

在初始部署后维护和开发您的网站的工作仍然存在，并且在长时间之前你将想要将更新部署。 本教程将指导您完成将更新部署到你的应用程序代码的过程。 此更新不涉及数据库更改;你将看到有关部署下一教程中的数据库更改的差异。

提示： 如果你收到如下错误消息，或当你完成本教程的内容不起作用，请务必检查[故障排除页](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="making-a-code-change"></a>进行代码更改

为你的应用程序到更新的简单示例，你将添加到**教师**页上通过所选教师讲授的课程的列表。

如果你运行**教师**页上，你会注意到没有**选择**链接在网格中，但它们不执行任何操作生成以外行背景变灰。

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

现在你将添加时运行的代码**选择**链接单击，并显示由所选教师讲授的课程的列表。

在*Instructors.aspx*，添加以下标记后立即**ErrorMessageLabel** `Label`控件：

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

运行页面，然后选择一个教师。 你看到通过该教师讲授的课程的列表。

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>代码将更新部署到测试环境

部署到测试环境，运行一次单击即可重新发布。 若要使此过程更快，可以使用**Web 单键发布**工具栏。

在**视图**菜单上，选择**工具栏**，然后选择**Web 单键发布**。

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

在**解决方案资源管理器**，选择 ContosoUniversity 项目。

**Web 单键发布**工具栏上，选择**测试**发布配置文件，然后单击**发布 Web** （带有箭头指向左侧和右侧的图标）。

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio 部署更新的应用程序，并浏览器将自动打开到主页。 运行教师页并选择一个教师以验证更新成功部署。

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

你通常会还执行回归测试 （即，测试站点后，若要确保新的更改不中断任何现有功能的其余部分）。 但对于本教程将跳过该步骤并继续将更新部署到生产环境。

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>防止重新部署到生产环境的初始数据库状态

在实际应用中，用户将您的生产站点进行在初始部署之后交互，并且数据库填充了实时数据。 因此，你不想要重新部署它将擦除所有的实时数据的初始状态中的成员资格数据库。 由于 SQL Server Compact 数据库中的文件*应用\_数据*文件夹中，你需要防止这种通过更改部署设置，因此，在文件*应用\_数据*文件夹未部署。

打开**项目属性**ContosoUniversity 项目，然后选择窗口**打包/发布 Web**选项卡。请确保**配置**下拉框中已**活动 （发行版）**或**版本**选择，选择**将应用程序中排除文件\_数据文件夹**。

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

如果你决定在将来部署调试版本，它是一个好办法进行相同的更改适用于调试生成配置： 更改**配置**到**调试**，然后选择**排除从应用程序的文件\_数据文件夹**。

保存并关闭**打包/发布 Web**选项卡。

> [!NOTE] 
> 
> [!IMPORTANT]
> 请确保你没有**删除目标位置的其他文件**选择发布配置文件中。 如果你选择该选项，在部署过程将删除具有在应用程序中的数据库\_中的数据已部署的站点，以及它将删除该应用\_数据文件夹本身。


## <a name="preventing-user-access-to-the-production-site-during-update"></a>禁止对生产站点更新的过程中的用户访问

要部署的更改现在是对单个页面的简单更改。 但有时部署较大的更改，并且在这种情况下站点可以异常如果用户请求页，在部署完成之前。 若要防止此情况，可以使用*应用\_offline.htm*文件。 当将一个名为文件*应用\_offline.htm*在你的应用程序根文件夹中，IIS 会自动显示该文件而不是运行你的应用程序。 因此，若要在部署过程中阻止访问，你将*应用\_offline.htm*在根文件夹中，运行在部署过程，然后删除*应用\_offline.htm*。

在**解决方案资源管理器**，右键单击该解决方案 （不是一种项目） 并选择**新建解决方案文件夹**。

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

将文件夹命名为*SolutionFiles*。

在新的文件夹中创建名为 HTML 页*应用\_offline.htm*。 将现有内容替换为以下标记：

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

你可以复制*应用\_offline.htm*到通过使用 FTP 连接站点的文件或**文件管理器**实用工具托管提供商的控制面板中。 对于本教程中，你将使用**文件管理器**。

打开控制面板，然后选择**文件管理器**中那样[将部署到生产环境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教程。 选择**contosouniversity.com**然后**wwwroot**以获取你的应用程序根文件夹，然后单击**上载**。

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

在**上载文件**对话框中，选择*应用\_offline.htm*文件，然后单击**上载**。

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

浏览到你网站的 URL。 你看到*应用\_offline.htm*页现在显示而不是你的主页。

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

现在你就可以部署到生产环境。

## <a name="deploying-the-code-update-to-the-production-environment"></a>代码将更新部署到生产环境

在**Web 单键发布**工具栏上，选择**生产**发布配置文件，然后单击**发布 Web**。

Visual Studio 将部署更新的应用程序，并打开浏览器向站点的主页。 *应用\_offline.htm*显示文件。 你可以对其进行测试以验证是否成功的部署之前，必须删除*应用\_offline.htm*文件。

返回到**文件管理器**控制面板中应用程序。 选择**contosouniversity.com**和**wwwroot**，选择**应用\_offline.htm**，然后单击**删除**。

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

在浏览器中，在公共网站中，打开教师页，然后选择一个教师以验证更新成功部署。

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

现在，你已部署不涉及数据库更改的应用程序更新。 下一教程演示了如何部署数据库更改。

>[!div class="step-by-step"]
[上一页](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
[下一页](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
