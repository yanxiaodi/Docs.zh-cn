---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: "部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 作为测试环境-5 12，将部署到 IIS |Microsoft 文档"
author: tdykstra
description: "这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Stu 包含 SQL Server Compact 数据库..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: a7995844ee6ed19efa130c4f6c019214d6652ea7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 作为测试环境-5 12，将部署到 IIS
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 中包含 SQL Server Compact 数据库。 如果你安装 Web 发布更新，也可以使用 Visual Studio 2010。 有关序列的简介，请参阅[序列中的第一个教程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 显示部署功能后，将 Visual Studio 2012 RC 版本中推出，演示如何部署 SQL Server Compact 以外的 SQL Server 版本并演示如何将部署到 Azure App Service Web Apps 的教程，请参阅[的 ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>概述

本教程演示如何部署到 IIS 在本地计算机上的 ASP.NET web 应用。

当你开发应用程序时，你通常测试通过在 Visual Studio 中运行它。 默认情况下，这意味着你要使用 Visual Studio 开发服务器 (也称为 Cassini)。 Visual Studio 开发服务器，可以轻松地在 Visual Studio 中开发期间测试，但它不与 IIS 的完全一样工作。 因此，很可能，当应用程序在 Visual Studio 中，对其进行测试，但在到 IIS 部署在托管环境中时失败时，将正确运行。

你可以通过以下方式更可靠地测试你的应用程序：

1. 使用 IIS Express 或完整 IIS 而不是 Visual Studio 开发服务器时在 Visual Studio 中测试在开发过程。 此方法通常会模拟更准确地在 IIS 下运行你的站点将如何。 但是，此方法不测试您的部署过程或验证部署过程的结果都会正常运行。
2. 通过使用相同的过程，你将使用更高版本来将其部署到生产环境部署到 IIS 开发计算机上的应用程序。 此方法验证除了验证你在 IIS 下将正确运行你的应用程序的部署过程。
3. 将应用部署到尽可能接近于生产环境的测试环境。 由于这些教程的生产环境是第三方托管提供商，理想的测试环境将具有托管提供商的第二个帐户。 你将此第二个帐户仅用于测试，但将其设置与生产帐户相同的方式。

本教程演示选项 2 的步骤。 选项 3 指南提供在末尾[将部署到生产环境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教程中，并且在本教程末尾有用于选项 1 的资源链接。

提示： 如果你收到如下错误消息，或当你完成本教程的内容不起作用，请务必检查[故障排除页](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="configuring-the-application-to-run-in-medium-trust"></a>在中等信任配置为运行该应用程序

在安装 IIS 并部署到它之前, 将以便站点运行更像它在典型的共享宿主环境中将更改 Web.config 文件设置。

托管提供商通常在运行您的网站*中等信任*，这意味着它不允许执行几项内容。 例如，应用程序代码不能访问 Windows 注册表和无法读取或写入应用程序的文件夹层次结构之外的文件。 默认情况下你的应用程序运行*高度信任*在本地计算机，这意味着应用程序可能能够执行时将其部署到生产环境将失败的操作。 因此，若要使测试环境的详细信息准确地反映生产环境，你将配置要在中等信任中运行的应用程序。

在应用程序 Web.config 文件中，添加**信任**中的元素**system.web**元素，如本示例中所示。

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

即使在本地计算机上，现在在 IIS 中的中级信任应用程序将运行。 此设置可由应用程序代码以执行其他操作会导致生产失败尽早捕获任何尝试。

> [!NOTE]
> 如果使用 Entity Framework Code First 迁移，请确保您拥有版本 5.0 或更高版本。 在实体框架版本 4.3，迁移以更新数据库架构需要完全信任。


## <a name="installing-iis-and-web-deploy"></a>安装 IIS 和 Web 部署

若要将部署到 IIS 在开发计算机上，你必须 IIS 和 Web 部署安装。 此外，默认 Windows 7 配置中不会包含这些信息。 如果你已经安装了 IIS 和 Web 部署，请跳至下一节。

使用[Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)是首选的方法来安装 IIS 和 Web 部署，因为 Web 平台安装程序的 IIS 安装推荐的配置，并自动安装 IIS 和 Web 的先决条件如有必要部署。

若要运行 Web 平台安装程序安装 IIS 和 Web 部署，请使用以下链接。 如果你已安装 IIS、 Web 部署或任何其所需的组件，Web 平台安装程序仅安装功能丢失。

- [安装 IIS 和 Web 部署使用 WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>将默认应用程序池设置为.NET 4

安装 IIS 后，运行**IIS 管理器**若要确保.NET Framework 版本 4 分配给默认应用程序池。

从 Windows**启动**菜单上，选择**运行**，输入"inetmgr"，然后单击**确定**。 (如果**运行**命令将不在你**启动**菜单上，你可以按 Windows 键和 R，以将其打开。 或右键单击任务栏中，单击**属性**，选择**开始菜单**选项卡上，单击**自定义**，然后选择**运行命令**。)

在**连接**窗格中，展开服务器节点，然后选择**应用程序池**。 在**应用程序池**窗格中，如果**DefaultAppPool**是分配给.NET framework 版本 4，如下图所示，跳到下一节。

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

如果你看到只有两个应用程序池，并且这两个设置为.NET Framework 2.0，你必须安装在 IIS 中的 ASP.NET 4:

- 通过右键单击打开命令提示符窗口**命令提示符**中 Windows**启动**菜单并选择**以管理员身份运行**。 然后运行[aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx)在 IIS 中，使用以下命令安装 ASP.NET 4。 （在 64 位系统中，替换与"Framework64"的"Framework"。）

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    此命令为.NET Framework 4 中，创建新的应用程序池，但默认应用程序池将仍设置为 2.0。 您要部署应用程序面向.NET 4 到该应用程序池，因此你需要为.NET 4 中更改应用程序池。

如果您关闭**IIS 管理器**，再次运行，展开服务器节点中，，再单击**应用程序池**以显示**应用程序池**再次窗格。

在**应用程序池**窗格中，单击**DefaultAppPool**，然后在**操作**窗格中，单击**基本设置**。

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

在**编辑应用程序池**对话框中，更改**.NET Framework 版本**到**.NET Framework v4.0.30319**单击**确定**。

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

现在你就可以将发布到 IIS。

## <a name="publishing-to-iis"></a>发布到 IIS

有多种方法可以部署使用 Visual Studio 2010 和 Web 部署：

- 使用一键式发布的 Visual Studio。
- 创建*部署包*和使用 IIS 管理器 UI 安装它。 部署包所包含的*.zip*包含的所有文件和在 IIS 中安装站点所需的元数据的文件。
- 创建部署包并将其使用命令行安装。

你已完成在前面的教程，若要将 Visual Studio 设置来自动执行部署任务适用于所有这三种方法的过程。 这些教程中，你将使用第一种方法。 有关使用部署包的信息，请参阅[ASP.NET 部署内容映射](https://msdn.microsoft.com/library/bb386521.aspx)。

在发布前，请确保在管理员模式下运行 Visual Studio。 (在 Windows 7**启动**菜单中，右键单击要使用的 Visual Studio 版本的图标，然后选择**以管理员身份运行**。)管理员模式下是发布仅当你准备向 IIS 发布在本地计算机上所必需的。

在**解决方案资源管理器**，右键单击 ContosoUniversity 项目 （而不是 ContosoUniversity.DAL 项目） 并选择**发布**。

**发布 Web**向导显示。

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

在下拉列表中，选择**&lt;新建...&gt;**.

在**新的配置文件**对话框中，输入"Test"，然后单击**确定**。

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

此名称是与 Web.Test.config 中间节点相同转换之前创建的文件。 这种对应关系是什么因素会导致在发布使用此配置文件时要应用的 Web.Test.config 转换。

该向导将自动前进到**连接**选项卡。

在**服务 URL**框中，输入*localhost*。

在**站点/应用程序**框中，输入*默认网站/ContosoUniversity*。

在**目标 URL**框中，输入`http://localhost/ContosoUniversity`。

**目标 URL**设置不是必需的。 Visual Studio 完成部署应用程序，它将自动打开默认浏览器到此 URL。 如果你不想要在部署后自动打开浏览器，请将此框留空。

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

单击**验证连接**以验证设置是否正确，并可以在本地计算机上连接到 IIS。

绿色的复选标记验证连接成功。

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

单击**下一步**以转到**设置**选项卡。

**配置**下拉框指定要部署的生成配置。 默认值为版本中，这是你希望。

保留**删除目标位置的其他文件**清除复选框。 由于这是你第一次部署时，不会有任何文件在目标文件夹中尚未。

在**数据库**部分中，在连接字符串框中输入以下值**SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

在部署过程会将此连接字符串置于部署的 Web.config 文件因为**在运行时使用此连接字符串**选择。

此外，在**SchoolContext**，选择**应用 Code First 迁移**。 此选项将导致配置已部署的 Web.config 文件，以指定部署过程`MigrateDatabaseToLatestVersion`初始值设定项。 此初始值设定项应用程序部署后首次访问数据库时，自动将数据库更新为最新版本。

中的连接字符串框**DefaultConnection**，输入以下值：

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

保留**更新数据库**清除。 将通过在应用程序中复制的.sdf 文件部署成员资格数据库\_数据，而您不希望部署过程以执行任何其他与此数据库操作。

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

单击**下一步**以转到**预览**选项卡。

在**预览**选项卡上，单击**开始预览**以查看将复制的文件的列表。

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

单击“发布” 。

如果在管理员模式下，Visual Studio 不是，你可能会指示权限错误的错误消息。 在这种情况下，关闭 Visual Studio，在管理员模式下打开它并尝试再次发布。

在管理员模式下，Visual Studio 是否**输出**窗口报表成功生成和发布。

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

浏览器将自动打开到 Contoso 大学主页在 IIS 中在本地计算机上运行。

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>在测试环境中测试

请注意环境指示器，显示"（测试）"而不是"（开发）"，其中显示*Web.config*环境指示器的转换是否成功。

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

运行**学生**页后，可以验证是否已部署的数据库具有任何学生。 当选择此页可能需要几分钟才能加载，因为 Code First 创建数据库，然后运行`Seed`方法。 （它未执行操作，因为应用程序未尝试访问数据库尚未年在主页上。）

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

运行**教师**页以验证 Code First 种子教师数据的数据库：

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

选择**添加学生**从**学生**菜单上，添加一名学生，，然后查看在新的学生**学生**页以验证是否可以成功写入到数据库:

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

从**课程**菜单上，选择**更新信用额度**。 **更新信用额度**页需要管理员权限，因此**Log In**显示页。 输入你创建更早版本 （"admin"和"Pa$ w0rd"） 的管理员帐户凭据。 **更新信用额度**页面显示时，该验证你在前面的教程中创建的管理员帐户已正确部署到测试环境。

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

验证*Elmah*文件夹存在与仅中它的占位符文件。

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>对于 Code First 迁移查看自动 Web.config 更改

打开*Web.config*在已部署的应用程序中的文件*C:\inetpub\wwwroot\ContosoUniversity* ，你可以看到部署过程自动配置 Code First 迁移到其中将数据库更新为最新版本。

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

在部署过程还创建新的连接字符串的代码优先迁移来更新数据库架构的以独占方式使用：

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

此额外的连接字符串，可指定一个用户帐户对数据库架构更新和应用程序数据访问的其他用户帐户。 例如，您无法将数据库\_到 Code First 迁移和 db 所有者角色\_datareader 和 db\_datawriter 角色添加至应用程序。 这是一种常见的深层防御模式，可防止在应用程序不能更改数据库架构中的潜在恶意代码。 （例如，这可能会发生在成功的 SQL 注入式攻击。）这些教程不使用此模式。 它不适用于 SQL Server Compact 和在本系列后面的教程中迁移到 SQL Server 时不适用。 Cytanium 站点提供了一个访问在 Cytanium 创建的 SQL Server 数据库的用户帐户。 如果你将能够在你的方案中实现此模式，可以通过执行以下步骤来执行此操作：

1. 在**设置**选项卡**发布 Web**向导中，输入完整的数据库架构更新权限，使用指定的用户的连接字符串，然后清除**使用此连接字符串在运行时**复选框。 在已部署的 Web.config 文件中，这将成为`DatabasePublish`连接字符串。
2. 创建 Web.config 文件转换为你想要在运行时使用的应用程序的连接字符串。

你现在已在开发计算机上部署到 IIS 应用程序，并且存在测试。 这将验证部署过程将应用程序的内容复制到正确的位置 （不包括你确实不想要部署的文件），并还该 Web 部署已配置的 IIS 正确在部署过程。 在下一步的教程中，你将运行一个查找尚未完成的部署任务的详细测试： 上设置文件夹权限*Elmah*文件夹。

## <a name="more-information"></a>详细信息

在 Visual Studio 中运行 IIS 或 IIS Express 的信息，请参阅以下资源：

- [IIS Express 概述](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)IIS.net 网站上。
- [引入了 IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) Scott Guthrie 的博客上。
- [如何： 为 Visual Studio 中的 Web 项目指定 Web 服务器](https://msdn.microsoft.com/library/ms178108.aspx)。
- [核心差异之间 IIS 和 ASP.NET Development Server](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) ASP.NET 站点上。
- [在 30 秒内测试你的 ASP.NET MVC 或 Web 窗体应用程序在 IIS 7 上](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx)Rick Anderson 博客上。 此条目提供使用 Visual Studio 开发服务器 (用于 Cassini) 来测试为何不不如在 IIS Express 中，测试可靠和在 IIS Express 测试为何不不如在 IIS 中测试可靠的示例。

在中等信任中运行你的应用程序时，可能出现哪些问题有关的信息，请参阅[在中等信任环境中承载 ASP.NET 应用程序](http://www.4guysfromrolla.com/articles/100307-1.aspx)上从 Rolla 站点 4 专家。

>[!div class="step-by-step"]
[上一页](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
[下一页](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
