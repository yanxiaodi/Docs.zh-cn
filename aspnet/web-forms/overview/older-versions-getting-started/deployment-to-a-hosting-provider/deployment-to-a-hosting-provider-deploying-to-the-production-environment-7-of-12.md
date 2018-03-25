---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: "部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 将部署到生产环境-7 / 12 |Microsoft 文档"
author: tdykstra
description: "这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Stu 包含 SQL Server Compact 数据库..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: 4aa6766c2c7765f499f5c5380962a5fe443e8c9d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 将部署到生产环境-7 / 12
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 中包含 SQL Server Compact 数据库。 如果你安装 Web 发布更新，也可以使用 Visual Studio 2010。 有关序列的简介，请参阅[序列中的第一个教程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 显示部署功能后，将 Visual Studio 2012 RC 版本中推出，演示如何部署 SQL Server Compact 以外的 SQL Server 版本并演示如何将部署到 Azure App Service Web Apps 的教程，请参阅[的 ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>概述

在本教程中，可以设置使用宿主提供程序帐户和部署 ASP.NET web 应用程序到生产环境中通过使用 Visual Studio 一键式发布功能。

提示： 如果你收到如下错误消息，或当你完成本教程的内容不起作用，请务必检查[故障排除页](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="selecting-a-hosting-provider"></a>选择托管提供商

对于 Contoso 大学应用程序和本系列教程，你需要支持 ASP.NET 4 和 Web 部署的提供程序。 特定的托管公司已选择，以便这些教程无法阐释的部署到实时网站的完整体验。 每个托管公司提供不同的功能，并且将部署到其服务器的体验会稍有不同。 但是，在本教程中所述的过程是典型的整体过程。 对于本教程，Cytanium.com，使用的宿主提供程序是一个许多可用，而在本教程中的使用它，不会导致的认可或推荐。

准备就绪，可选择你自己的宿主提供程序后，你可以比较功能和价格的[的提供程序库](https://www.microsoft.com/web/hosting)Microsoft.com/web 站点上。

## <a name="creating-an-account"></a>创建帐户

在你选择的提供程序创建的帐户。 如果支持完整的 SQL Server 数据库是已添加额外，不需要为本教程中，选择它，但你将需要它的[迁移到 SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)更高版本中这一系列教程。

有关这些教程中，你无需注册一个新的域名。 你可以测试通过使用提供程序分配给站点的临时 URL 验证成功部署。

创建帐户之后，你通常会收到包含所需部署和管理你的站点的所有信息的欢迎电子邮件。 你托管提供商向你发送的信息将类似于此处所示。 Cytanium 欢迎电子邮件发送到新的帐户所有者包括以下信息：

- 提供程序的控件面板网站，你可以为您的网站管理设置的 URL。 欢迎电子邮件以方便参考本部分中包括的 ID 和你指定的密码。 （同时具有已更改为此图演示值。）

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- 默认.NET Framework 版本和有关如何对其进行更改的信息。 许多托管站点默认值为 2.0，它适用于 ASP.NET 应用程序面向.NET Framework 2.0、 3.0 或 3.5。 但是 Contoso 大学是一个.NET Framework 4 应用程序中，因此你必须更改此设置。 （为 ASP.NET 4.5 应用程序应该使用.NET 4.0 设置。）

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- 可用于访问您的网站的临时 URL。 在创建此帐户后，"contosouniversity.com"输入与现有域的名称。 因此临时 URL 是`http://contosouniversity.com.vserver01.cytanium.com`。

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- 若要了解如何设置数据库和所需对其进行访问的连接字符串：

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- 有关工具和部署你的网站的设置的信息。 （从 Cytanium 电子邮件还提及 WebMatrix 中，这此处省略。）

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>.NET Framework 版本设置

Cytanium 欢迎电子邮件包含有关如何更改.NET Framework 的版本的说明的链接。 这些说明解释，这可以通过 Cytanium 控件面板。 其他提供程序有一个控件面板站点，看起来相同，或它们可以指导您执行此操作以不同的方式。

请转到控制面板 URL。 登录后你的用户名和密码，你将看到控制面板。

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

在**承载空间**框中，将指针停留在 Web 图标，然后选择**网站**从菜单。

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

在**网站**框中，单击**contosouniversity.com** （你在你创建的帐户时使用的站点名称）。

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

在**Web 站点属性**框中，选择**扩展**选项卡。

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

更改从 ASP.NET **2.0 集成的管道**到**4.0 （集成管道）**，然后单击**更新**。

## <a name="publishing-to-the-hosting-provider"></a>发布到托管提供商

托管提供商发送的欢迎电子邮件包括所有所需发布该项目的设置和您可以手动输入该信息到发布配置文件。 但你将使用更容易和更少易出错的方法来配置部署到提供程序： 你将下载*.publishsettings*文件并将其导入发布配置文件。

在浏览器中，转到 Cytanium 控制面板，并选择**Web** ，然后选择**网站。**

![选择网站的控制面板](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

选择**contosouniversity.com** web 站点。

![选择 contosouniversity.com 控制面板](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

选择**Web 发布**选项卡。

![控件面板 Web 发布选项卡](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

创建要用于 web 发布通过输入用户名和密码凭据。 你可以输入你用于登录到控制面板上的相同凭据。 然后单击**启用**。

![控制面板创建发布凭据](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

单击**下载发布配置文件添加为此网站**。

![控件面板下载发布配置文件](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

当提示你打开或保存该文件时，请将其保存。

![保存发布配置文件](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

在**解决方案资源管理器**在 Visual Studio 中，右键单击 ContosoUniversity 项目并选择**发布**。 **发布 Web**对话框随即打开上**预览**与选项卡上**测试**选择，因为它是你使用的最后一个配置文件的配置文件。

选择**配置文件**选项卡，然后单击**导入**。

![发布 Web 向导导入按钮](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

在**导入发布设置**对话框中，选择*.publishsettings*文件你下载，并单击**打开**。 该向导将前进到连接选项卡使用的所有字段填充。

![发布 Web 向导连接选项卡](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

.Publishsettings 文件中的目标 URL 框中，将该站点的计划永久 URL，但如果你尚未尚未购买该域，则将值替换为临时的 URL。 对于此示例中，URL 是 *[http://contosouniversity.com.vserver01.cytanium.com](http://contosouniversity.com.vserver01.cytanium.com)。*此框的唯一目的是指定哪些浏览器将打开到自动后成功部署后的 URL。 如果你将其留空，一个唯一的后果是浏览器不会自动启动部署后。

单击**验证连接**以验证设置是否正确，并可以连接到服务器。 如您前面看到的一个绿色复选标记将验证连接成功。

单击验证连接，你可能会看到**证书错误**对话框。 如果执行操作时，请验证服务器名称是否符合你的预期。 如果是，选择**Visual Studio 的将来会话保存此证书**单击**接受**。 （此错误表明，托管提供商已选择免去为您要部署到的 URL 中购买的 SSL 证书的开支。 如果想要通过使用有效的证书建立安全连接，请与你托管提供商。）

![证书错误](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

单击 **“下一步”**。

在**数据库**部分**设置**选项卡上，输入相同的测试输入值，发布配置文件。 下拉列表中，你将找到你需要的连接字符串。

- 中的连接字符串框**SchoolContext，**选择`Data Source=|DataDirectory|School-Prod.sdf`
- 下**SchoolContext**，选择**应用 Code First 迁移**。
- 中的连接字符串框**DefaultConnection**，选择`Data Source=|DataDirectory|aspnet-Prod.sdf`
- 下**DefaultConnection**，保留**更新数据库**清除。

![发布 Web 向导设置选项卡](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

单击 **“下一步”**。

在**预览**选项卡上，单击**开始预览**以查看将复制的文件的列表。 你看到你此前看到在本地计算机上部署到 IIS 时的相同列表。

在发布之前，更改配置文件的名称，以便将应用你 Web.Production.config 转换文件。 选择**配置文件**选项卡，单击**管理配置文件**。

![发布 Web 向导管理配置文件](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

在**编辑 Web 发布配置文件**对话框框中，选择生产配置文件，单击**重命名**，并将配置文件名称更改为生产。 然后单击**关闭**。

![编辑 Web 发布配置文件对话框中](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

单击“发布” 。

应用程序发布到托管提供商。 结果显示在**输出**窗口。

![部署后的输出窗口](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

浏览器将自动打开指向中输入的 URL**目标 URL**框**连接**选项卡**发布 Web**向导。 你看到相同的主页上作为 Visual Studio 中运行该站点的情况除外现在没有"（测试）"或"（开发）"的标题栏中的环境指示器。 这表示环境指示器*Web.config*转换工作正常。

> [!NOTE]
> 如果你仍看到标题中的"（测试）"，删除*obj*从 ContosoUniversity 项目和重新部署文件夹。 在预发行版本中的软件，以前应用的转换文件 (Web.Test.config) 可能获取再次应用尽管你使用生产配置文件。


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

在运行会导致数据库访问的页之前，请确保 Elmah 将能够登录出现的任何错误。

## <a name="setting-folder-permissions-for-elmah"></a>Elmah 的的设置文件夹权限

从以前的教程，此序列中，您还记得，必须确保应用程序具有 Elmah 存储错误日志文件的位置是在你的应用程序的文件夹的写入访问权限。 当你部署到 IIS 本地计算机上时，你手动设置这些权限。 在此部分中，你将了解如何在 Cytanium 设置权限。 （某些托管提供商不允许你执行此操作; 它们可能会提供一个或多个预定义的文件夹具有写权限。 在这种情况下你将需要应用程序修改为使用指定的文件夹。）

在 Cytanium 控制面板中，可以设置文件夹权限。 请转到控件面板 URL，然后选择**文件管理器**。

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

在**文件管理器**框中，选择**contosouniversity.com**然后**wwwrooot**若要查看应用程序的根文件夹。 单击挂锁图标旁边**Elmah**。

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

在**文件**/**文件夹权限**窗口中，选择**读取**和**编写**对应的复选框**contosouniversity.com**单击**设置权限**。

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

请确保 Elmah 具有写入访问权限*Elmah*通过导致了错误，然后显示 Elmah 错误报告的文件夹。 请求的无效 URL *Studentsxxx.aspx*。 正如之前，您看到*GenericErrorPage.aspx*页。 单击**注销**链接，然后再运行*Elmah.axd*。 你获取**Log In**页首先，该验证*Web.config*转换已成功添加 Elmah 授权。 你在登录后，你将看到的报表，将会显示仅导致的错误。

[![Elmah.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>在生产环境中测试

运行**学生**页。 应用程序尝试以第一次，随即将会触发代码优先迁移来创建数据库访问 School 数据库。 显示页时在一段时间延迟后，将它不显示任何学生。

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

运行**教师**页以验证种子数据已成功在数据库中插入教师数据。

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

就像在测试环境中，你想要验证的数据库更新工作量在生产环境中，但通常不希望插入您的生产数据库输入测试数据。 对于本教程中，你将使用未在测试中的相同方法。 但在实际应用中你可能想要找到验证该数据库的方法更新均可成功完成而不会引入到生产数据库的测试数据。 在某些应用程序，则可能是可行添加下列内容，然后将其删除。

添加一名学生，然后查看你输入中的数据**学生**页后，可以验证是否可以更新数据库中的数据。

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

验证授权规则是否正常工作，可以通过选择**更新信用额度**从**课程**菜单。 **Log In**显示页。 输入你的管理员帐户凭据，请单击**Log In**，和**更新信用额度**显示页。

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

如果登录成功，**更新信用额度**显示页。 这指示 ASP.NET 成员资格数据库 （使用单个管理员帐户中） 已成功部署。

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

你现在已成功部署和测试你的站点，它可以公开通过 Internet。

## <a name="creating-a-more-reliable-test-environment"></a>创建更可靠的测试环境

中所述[将部署到测试环境](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)教程中，最可靠的测试环境将在具有就像生产帐户托管提供商的第二个帐户。 这将是高于作为测试环境中，使用本地 IIS，因为您将不得不注册的第二个托管帐户。 但如果它可以防止生产站点错误或服务中断，则可以决定它是否值得成本。

创建和部署到测试帐户的过程的大部分都类似于什么你已符合部署到生产环境：

- 创建*Web.config*转换文件。
- 在托管提供商创建一个帐户。
- 创建新的发布配置文件并部署到测试帐户。

### <a name="preventing-public-access-to-the-test-site"></a>禁止对测试站点的公共访问权限

测试帐户的重要考虑事项是： 将实时在 Internet 上，但你不希望公共若要使用它。 若要保持站点私有可以使用一个或多个以下方法：

- 联系托管提供商能够设置防火墙规则来允许访问到测试站点仅用于测试的 IP 地址。
- 伪装 URL，以便它不是类似于公共站点的 URL。
- 使用*robots.txt*文件以确保，搜索引擎将不爬网测试站点和报表链接到它在搜索结果中。

第一种方法是很明显最安全的但该过程特定于每个托管提供商和将不会在本教程中介绍。 如果你执行安排你托管提供商，以允许仅你的 IP 地址以浏览到测试帐户 URL，你理论上无需担心搜索引擎爬网它。 但即使在这种情况下，部署*robots.txt*文件作为备份是一个好主意，以防您是否意外关闭该防火墙规则。

*Robots.txt*文件将在你的项目文件夹中，在它应该具有以下文本：

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

`User-agent`一行通知文件中的规则应用于所有搜索引擎 web 爬网程序 （机器人） 的搜索引擎和`Disallow`行指定应在站点上的没有页已爬网。

您可能希望搜索引擎以目录生产站点，因此你需要从生产部署中排除此文件。 若要做到这一点，请参阅**可以我排除特定文件或文件夹从部署？**中[ASP.NET Web 应用程序项目部署常见问题](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment)。 请确保仅为生产发布配置文件时，才指定排除。

创建第二个托管帐户是一种方法使用的测试环境不是必需的但可能值得额外的费用。 在以下教程中，你将继续使用 IIS 作为你的测试环境。

在下一步的教程中，将更新应用程序代码，并将所做的更改部署到测试和生产环境。

>[!div class="step-by-step"]
[上一页](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
[下一页](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
