---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: "使用 Visual Studio 的 ASP.NET Web 部署： 将部署到生产 |Microsoft 文档"
author: tdykstra
description: "本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure App Service Web Apps 或第三方托管提供程序，使用的..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: abd3f3f78dd9a9e6394e2f61aa9bd692810ca875
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>使用 Visual Studio 的 ASP.NET Web 部署： 将部署到生产
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure App Service Web Apps 或第三方托管提供程序，通过使用 Visual Studio 2012 或 Visual Studio 2010。 有关序列的信息，请参阅[序列中的第一个教程](introduction.md)。


## <a name="overview"></a>概述

在本教程中，设置 Microsoft Azure 帐户、 创建过渡和生产环境和部署 ASP.NET web 应用程序到过渡环境和生产环境中的通过使用 Visual Studio 一键式发布功能。

如果你愿意，你可以将其部署到第三方托管提供商。 在本教程中所述的过程的大部分都是相同的宿主提供程序或 azure，只不过每个提供程序具有其自己的帐户和 web 站点管理的用户界面。 你可以查找中的宿主提供程序[的提供程序库](https://www.microsoft.com/web/hosting)Microsoft.com web 站点上。

提示： 如果你收到如下错误消息，或当你完成本教程的内容不起作用，请务必检查本系列教程中的疑难解答页。

## <a name="get-a-microsoft-azure-account"></a>获取一个 Microsoft Azure 帐户

如果你还没有 Azure 帐户，可以在几分钟内创建一个免费试用帐户。 有关详细信息，请参阅[Azure 免费试用版](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。

## <a name="create-a-staging-environment"></a>创建过渡环境

> [!NOTE]
> 由于在撰写本教程时，Azure App Service 添加了新功能来自动执行许多创建过渡和生产环境的过程。 请参阅[设置过渡环境在 Azure App Service 的 web 应用的](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/)。


中所述[部署到测试环境教程](deploying-to-iis.md)、 最可靠的测试环境是在具有就像生产网站托管提供商的网站。 许多托管服务提供商，您将不得不重量此功能的优势与重要的其他成本，但在 Azure 中，你可以创建其他的免费 web 应用程序作为你的过渡应用。 你还需要一个数据库，并为该通过生产数据库的费用的额外费用将为无或最小。 在 Azure 中您支付你使用的数据库存储量而不是每个数据库，并将过渡环境中使用的其他存储量将最小。

中所述[部署到测试环境教程](deploying-to-iis.md)、 过渡和生产你要将两个数据库部署到一个数据库中。 如果你想要将它们分开，过程将相同，只不过将会产生的每个环境的其他数据库并在创建发布配置文件时，将选择每个数据库的正确的目标字符串。

在本教程的本部分中，你将要创建一个 web 应用和数据库要用于过渡环境，并将部署到过渡环境，还可以创建和部署到生产环境之前，那里测试。

> [!NOTE]
> 以下步骤演示如何使用 Azure 管理门户在 Azure App Service 中创建 web 应用。 在 Azure SDK 的最新版本，你也可以这样做无需离开 Visual Studio 中，通过使用服务器资源管理器。 在 Visual Studio 2013 中，还可以直接从发布对话框创建 web 应用。 有关详细信息，请参阅[在 Azure App Service 中创建 ASP.NET web 应用。](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. 在[Azure 管理门户](https://manage.windowsazure.com/)，单击**网站**，然后单击**新建**。
2. 单击**网站**，然后单击**自定义创建**。

    **新建网站-自定义创建**向导随即打开。 **自定义创建**向导可以同时创建网站和数据库。
3. 在**创建网站**步骤的向导中，输入中的字符串**URL**框，以用作你的应用程序的唯一 URL 的过渡环境。 例如，输入 ContosoUniversity staging123 （包括结尾的随机数字，以确保其唯一性，以防执行 ContosoUniversity 过渡）。

    完整的 URL 将包含的你在此处输入和文本框旁边看到的后缀。
4. 在**区域**下拉列表中，选择离你的区域。

    此设置指定你的 web 应用将在中运行哪个数据中心。
5. 在**数据库**下拉列表中，选择**创建新的 SQL 数据库**。
6. 在**数据库连接字符串名称**框中，保留默认值， *DefaultConnection*。
7. 单击框底部右侧箭头。

    下图显示**创建网站**与示例中它的值的对话框。 将不同的 URL 和您输入的区域。

    ![创建网站的步骤](deploying-to-production/_static/image1.png)

    向导将前进到**指定数据库设置**步骤。
8. 在**名称**框中，输入*ContosoUniversity*加上随机的编号，以使之成为唯一的例如*ContosoUniversity123*。
9. 在**服务器**框中，选择**新建 SQL 数据库服务器**。
10. 输入管理员名称和密码。

    不要输入现有名称和密码。 要输入新名称和你定义现在要以后访问数据库时使用的密码。
11. 在**区域**框中，选择为 web 应用所选的同一区域。

    将 web 服务器和数据库服务器放置在同一区域提供最佳性能并最大程度减少费用。
12. 单击底部的框以指示你已完成的复选标记。

    下图显示**指定数据库设置**与示例中它的值的对话框。 你输入的值可能不同。

    ![数据库设置步骤使用数据库向导创建的新的网站的](deploying-to-production/_static/image2.png)

    管理门户返回到网站页面中，与**状态**列显示正在创建的 web 应用。 （通常小于一分钟），一段时间后**状态**列显示已成功创建 web 应用。 在左侧导航栏中，在你的帐户拥有的 web 应用数旁边将出现**网站**旁边显示图标和数据库的数目**SQL 数据库**图标。

    ![管理门户中，创建网站的网站页面](deploying-to-production/_static/image3.png)

    您的 web 应用名称将不同于在图中的示例应用程序。

## <a name="deploy-the-application-to-staging"></a>部署到过渡环境应用程序

既然你已经创建 web 应用和过渡环境的数据库，你可以将项目部署到它。

> [!NOTE]
> 这些说明演示如何创建通过下载发布配置文件*.publishsettings*文件，其工作方式不只供 Azure 也为第三方宿主提供程序。 最新 Azure SDK 还可以直接连接到 Azure 从 Visual Studio 中，并从具有 Azure 帐户中的 web 应用的列表中选择。 在 Visual Studio 2013 中，你可以登录到 Azure 从**Web 发布**对话框或从**服务器资源管理器**窗口。 有关详细信息，请参阅[在 Azure App Service 中创建 ASP.NET web 应用](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)。


### <a name="download-the-publishsettings-file"></a>下载.publishsettings 文件

1. 单击你刚刚创建的 web 应用的名称。

    ![单击要转到仪表板的站点](deploying-to-production/_static/image4.png)
2. 下**速览**中**仪表板**选项卡上，单击**下载发布配置文件**。

    ![下载发布配置文件链接](deploying-to-production/_static/image5.png)

    此步骤将下载包含所有所需应用程序部署到你的 web 应用的设置的文件。 因此你不必手动输入此信息，将导入 Visual Studio 的此文件。
3. 保存*.publishsettings*可以从 Visual Studio 访问的文件夹中的文件。

    ![保存.publishsettings 文件](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > 安全- *.publishsettings*文件包含你用于管理你的 Azure 订阅和服务的凭据 （未编码）。 此文件的最佳安全方案是将其临时存储 （例如在 Libraries\Documents 文件夹中），源目录以外，然后在导入完毕后删除它。 恶意用户获得访问权*.publishsettings*文件可以编辑、 创建和删除你的 Azure 服务。

### <a name="create-a-publish-profile"></a>创建发布配置文件

1. 在 Visual Studio 中，右键单击中的 ContosoUniversity 项目**解决方案资源管理器**和选择**发布**从上下文菜单。

    **发布 Web**向导随即打开。
2. 单击**配置文件**选项卡。
3. 单击“导入” 。
4. 导航到*.publishsettings*更早版本，下载文件，然后单击**打开**。

    ![导入发布设置对话框](deploying-to-production/_static/image7.png)
5. 在**连接**选项卡上，单击**验证连接**若要确保设置正确无误。

    在验证连接后，旁边出现一个绿色的复选标记**验证连接**按钮。

    对于某些宿主提供程序，当你单击**验证连接**，你可能会看到**证书错误**对话框。 如果执行操作时，请验证服务器名称是否符合你的预期。 如果服务器名称正确，请选择**Visual Studio 的将来会话保存此证书**单击**接受**。 （此错误表明，托管提供商已选择免去为您要部署到的 URL 中购买的 SSL 证书的开支。 如果想要通过使用有效的证书建立安全连接，请与你托管提供商。）
6. 单击 **“下一步”**。

    ![连接成功图标和连接选项卡中的下一步按钮](deploying-to-production/_static/image8.png)
7. 在**设置**选项卡上，展开**文件发布选项**，然后选择**将应用程序中排除文件\_数据文件夹**。

    璝惠下的其他选项**文件发布选项**，请参阅[部署到 IIS](deploying-to-iis.md)教程。 此步骤和以下的数据库配置步骤的结果是在数据库配置步骤末尾用于显示的屏幕截图。
8. 下**DefaultConnection**中**数据库**部分中，配置成员资格数据库的数据库部署。
9. 1. 选择**更新数据库**。

        **远程连接字符串**直接下面框**DefaultConnection**使用.publishsettings 文件中的连接字符串填充。连接字符串包含纯文本中存储的 SQL Server 凭据*.pubxml*文件。 如果你不想将其存储在永久，可以部署数据库后，则会删除它们从发布配置文件，并将它们存储在 Azure 中。 有关详细信息，请参阅[如何保护你的 ASP.NET 数据库连接字符串从源部署到 Azure 时](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx)Scott Hanselman 的博客上。
    2. 单击**配置数据库更新**。
    3. 在**配置数据库更新**对话框中，单击**添加 SQL 脚本**。
    4. 在**添加 SQL 脚本**框中，导航到*aspnet 数据 prod.sql*脚本，你前面保存在解决方案文件夹中，然后单击**打开**。
    5. 关闭**配置数据库更新**对话框。
10. 下**SchoolContext**中**数据库**部分中，选择**执行 Code First 迁移 （应用程序启动时运行）**。

    Visual Studio 将显示**执行 Code First 迁移**而不是**更新数据库**为`DbContext`类。 如果你想要使用 dbDacFx 提供程序而不是迁移部署的数据库，你通过使用访问`DbContext`类，请参阅[如何部署没有迁移的 Code First 数据库？](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) Visual Studio Web 部署常见问题中和 MSDN 上的 ASP.NET。

    **设置**选项卡现在看起来像下面的示例：

    ![用于过渡的设置选项卡](deploying-to-production/_static/image9.png)
11. 执行以下步骤以保存配置文件并将其命名为*过渡*:

    1. 单击**配置文件**选项卡上，并依次**管理配置文件**。
    2. 导入创建两个新的配置文件，一个用于 FTP，一个用于 Web 部署。 配置 Web 部署配置文件： 重命名此配置文件以*过渡*。

        ![将配置文件重命名为临时](deploying-to-production/_static/image10.png)
    3. 关闭**编辑 Web 发布配置文件**对话框。
    4. 关闭**发布 Web**向导。

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>配置发布配置文件转换为环境的指示符

> [!NOTE]
> 本部分演示如何设置 Web.config 转换为环境的指示符。 因为指示器位于`<appSettings>`元素，必须在要部署到 Azure App Service 时指定转换所需的另一种可选。 有关详细信息，请参阅[在 Azure 中的指定 Web.config 设置](web-config-transformations.md#watransforms)。


1. 在**解决方案资源管理器**，展开**属性**，然后展开**PublishProfiles**。
2. 右键单击*Staging.pubxml*，然后单击**添加配置转换**。

    ![为临时添加配置转换](deploying-to-production/_static/image11.png)

    Visual Studio 将创建*Web.Staging.config*转换文件并将其打开。
3. 在*Web.Staging.config*转换文件中，插入以下代码打开后立即`configuration`标记。

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    当使用过渡发布配置文件时，此转换将设置到"Prod"的环境指示器。 在已部署的 web 应用中，不会在"Contoso 大学"H1 标题后看到任何后缀，例如"（开发）"（测试）"。
4. 右键单击*Web.Staging.config*文件并单击**预览转换**以确保的转换编码时产生预期的更改。

    **Web.config 预览**窗口中显示的应用同时结果*Web.Release.config*转换和*Web.Staging.config*转换。

### <a name="prevent-public-use-of-the-test-app"></a>防止公共使用的测试应用程序

过渡应用的重要考虑事项是： 将实时在 Internet 上，但你不希望使用它的公共。 若要尽量减少人将查找并使用它的可能性越小，可以使用一个或多个以下方法：

- 设置允许对访问过渡应用仅用于测试过渡的 IP 地址的防火墙规则。
- 使用是不可能猜到的经过模糊处理的 URL。
- 创建*robots.txt*文件以确保，搜索引擎将不爬网测试应用程序和报表链接到它在搜索结果中。

第一种方法最有效，但因为它将需要你将部署到 Azure 云服务而不是 Azure App Service 中本教程未涉及。 有关云服务的详细信息和在 Azure 中的 IP 限制，请参阅[选项提供的计算托管 Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me)和[阻止特定 IP 地址访问 Web 角色](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx)。 如果将部署到第三方托管提供程序，请与联系提供商联系以了解如何实施 IP 限制。

对于本教程中，将创建*robots.txt*文件。

1. 在**解决方案资源管理器**，右键单击 ContosoUniversity 项目，然后单击**添加新项**。
2. 创建一个新**文本文件**名为*robots.txt*，并向其中放置以下文本：

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    `User-agent`一行通知文件中的规则应用于所有搜索引擎 web 爬网程序 （机器人） 的搜索引擎和`Disallow`行指定应在站点上的没有页已爬网。

    您希望搜索引擎以目录生产应用程序，因此你需要从生产部署中排除此文件。 为此，你将配置在生产环境中的设置发布配置文件时创建它。

### <a name="deploy-to-staging"></a>部署到过渡环境

1. 打开**发布 Web**向导通过右键单击 Contoso 大学项目并单击**发布**。
2. 请确保**过渡**选择配置文件。
3. 单击“发布” 。

    **输出**窗口将显示已执行的部署操作并报告已成功完成的部署。 默认浏览器将自动打开到已部署的 web 应用的 URL。

## <a name="test-in-the-staging-environment"></a>在过渡环境中测试

请注意环境指示器不存在 (没有任何"（测试）"或"（开发）"后的 H1 标题，用于显示*Web.config*环境指示器的转换是否成功。

![主页过渡](deploying-to-production/_static/image12.png)

运行**学生**页后，可以验证是否已部署的数据库具有任何学生。

运行**教师**页以验证 Code First 种子教师数据的数据库：

选择**添加学生**从**学生**菜单上，添加一名学生，，然后查看在新的学生**学生**页以验证是否可以成功写入到数据库.

从**课程**页上，单击**更新信用额度**。 **更新信用额度**页需要管理员权限，因此**Log In**显示页。 输入你创建更早版本 （"admin"和"prodpwd"） 的管理员帐户凭据。 **更新信用额度**页面显示时，该验证你在前面的教程中创建的管理员帐户已正确部署到测试环境。

请求无效的 URL，将引发错误 ELMAH 将跟踪，然后请求 ELMAH 错误报告。 如果将部署到第三方托管提供商，您可能会发现该报告的相同的原因，它为空以前一教程中为空。 你将需要使用托管提供商的帐户管理工具来配置文件夹的权限以启用 ELMAH 要写入的日志文件夹。

你创建的应用程序现在运行在云中就如同将用于生产的 web 应用中。 由于一切正常运行下, 一步是部署到生产环境。

## <a name="deploy-to-production"></a>部署到生产环境

创建生产 web 应用程序和部署到生产环境的过程是临时，与相同，只不过你需要排除*robots.txt*从部署。 若要执行此操作将编辑发布配置文件。

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>创建生产环境和生产发布配置文件

1. 在 Azure 中，执行用于过渡的同一过程中创建的生产 web 应用和数据库。

    在创建数据库时，你可以选择将其放在同一台服务器更早版本，创建或创建新的服务器。
2. 下载*.publishsettings*文件。
3. 通过导入生产环境中创建的发布配置文件*.publishsettings*文件，用于过渡执行同一过程。

    不要忘记将配置数据部署脚本在**DefaultConnection**中**数据库**部分**设置**选项卡。
4. 重命名为发布配置文件*生产*。
5. 配置发布配置文件转换为环境的指示符，用于过渡执行同一过程...

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>编辑要排除 robots.txt 的.pubxml 文件

发布配置文件文件被命名为&lt;profilename&gt;*.pubxml*并位于*PublishProfiles*文件夹。 *PublishProfiles*文件夹低于*属性*文件夹中的 C# web 应用程序项目，在*我的项目*中 VB web 应用程序项目中，或以下的文件夹*应用\_数据*文件夹中的 web 应用程序项目。 每个*.pubxml*文件包含将应用于某个设置发布配置文件。 在发布 Web 向导中输入的值存储在这些文件，并可以编辑它们以便创建或更改不会在 Visual Studio UI 中提供的设置。

默认情况下， *.pubxml*文件包括在项目中，在你创建的发布配置文件中，但你可以从项目中排除它们和 Visual Studio 仍将使用它们。 Visual Studio 将查找*PublishProfiles*文件夹*.pubxml*文件，无论它们是否包括在项目中。

每个*.pubxml*文件没有*。 pubxml.user*文件。 *。 Pubxml.user*文件包含加密的密码，如果你选择**保存密码**选项，并且默认情况下会从项目中排除。

A *.pubxml*文件包含适用于特定的发布配置文件的设置。 如果你想要配置应用于所有配置文件的设置，则可以创建*。 wpp.targets*文件。 生成过程中导入到这些文件*.csproj*或*.vbproj*项目文件中，因此可以在这些文件中配置大多数设置都可以在项目文件中配置。 有关详细信息*.pubxml*文件和*。 wpp.targets*文件，请参阅[如何： 编辑发布配置文件 (.pubxml) 文件中的部署设置与。 wpp.targets Visual Studio 中的文件Web 项目发布](https://msdn.microsoft.com/library/ff398069.aspx)。

1. 在**解决方案资源管理器**，展开**属性**展开**PublishProfiles**。
2. 右键单击*Production.pubxml*单击**打开**。

    ![打开.pubxml 文件](deploying-to-production/_static/image13.png)
3. 右键单击*Production.pubxml*单击**打开**。
4. 添加以下行在关闭之前立即`PropertyGroup`元素：

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    .Pubxml 文件现在看起来像下面的示例：

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    有关如何排除文件和文件夹的详细信息，请参阅[可以我排除特定文件或文件夹从部署？](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment)中**for Visual Studio 和 ASP.NET 的 Web 部署常见问题**MSDN 上。

### <a name="deploy-to-production"></a>部署到生产环境

1. 打开**发布 Web**向导确保**生产**发布配置文件已选择，然后单击**开始预览**上**预览**选项卡以确认*robots.txt*文件都不会复制到生产应用程序。

    ![要发布到生产环境的文件的预览](deploying-to-production/_static/image14.png)

    查看将复制的文件的列表。 你会看到所有的*.cs*文件，包括*。 aspx.cs*， *。 aspx.designer.cs*， *Master.cs*，和*Master.designer.cs*文件被忽略。 所有此代码编译为*ContosoUniversity.dll*和*ContosUniversity.pdb*将在中找到的文件*bin*文件夹。 因为仅*.dll*需要的运行该应用程序，并且你前面指定应部署仅运行该应用程序所需的文件，则不*.cs*文件被复制到目标环境。 *Obj*文件夹和*ContosoUniversity.csproj*和*。 csproj.user*文件省略出于同样的原因。

    单击**发布**将部署到生产环境。
2. 在生产环境，用于过渡执行同一过程中测试。

    所有内容等同于过渡 URL 除外和没有*robots.txt*文件。

## <a name="summary"></a>摘要

你现在已成功部署和测试你的 web 应用并可公开通过 Internet。

![主页生产](deploying-to-production/_static/image15.png)

在下一步的教程中，将更新应用程序代码，并将更改部署到测试、 过渡和生产环境。

> [!NOTE]
> 在生产环境中使用你的应用程序时你应实现恢复计划。 也就是说，你应将定期备份你的数据库从生产应用到安全存储位置，并您应保留多个代这样的备份。 在更新数据库时，你应在更改之前立即从的备份副本。 然后，如果犯错，进而不发现它之前，部署到生产环境之后，你仍将能够将数据库恢复到它损坏之前的状态。 有关详细信息，请参阅[Azure SQL Database 备份和还原](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx)。


> [!NOTE]
> 在本教程中 SQL Server 部署到的版本是 Azure SQL 数据库。 类似于其他版本的 SQL Server 部署过程时，实际生产应用程序可能在某些情况下为 Azure SQL 数据库需要特殊的代码。 有关详细信息，请参阅[使用 Azure SQL 数据库](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb)和[SQL Server 和 Azure SQL 数据库之间进行选择](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing)。

>[!div class="step-by-step"]
[上一页](setting-folder-permissions.md)
[下一页](deploying-a-code-update.md)
