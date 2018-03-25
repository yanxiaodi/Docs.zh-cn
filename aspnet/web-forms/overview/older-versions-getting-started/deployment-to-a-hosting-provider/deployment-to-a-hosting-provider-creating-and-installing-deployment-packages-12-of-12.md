---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: "部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 疑难解答 (12 的 12) |Microsoft 文档"
author: tdykstra
description: "这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Stu 包含 SQL Server Compact 数据库..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: d8c4931a1d26af49ee61c896897fa6ddf12fccea
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 疑难解答 (12 的 12)
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 中包含 SQL Server Compact 数据库。 如果你安装 Web 发布更新，也可以使用 Visual Studio 2010。 有关序列的简介，请参阅[序列中的第一个教程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 显示部署功能后，将 Visual Studio 2012 RC 版本中推出，演示如何部署 SQL Server Compact 以外的 SQL Server 版本并演示如何将部署到 Windows Azure 网站的教程，请参阅[的 ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


本页介绍通过使用 Visual Studio 部署 ASP.NET web 应用程序时可能出现的一些常见问题。 每个提供了一个或多个可能的原因和相应的解决方案。

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>服务器错误 '/' 应用程序-中当前的自定义错误设置防止错误的详细信息其他人远程查看

### <a name="scenario"></a>方案

部署到远程主机的站点之后, 你将收到错误消息，提及的 Web.config 文件中的 customErrors 设置，但并不指示错误的实际原因是：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

默认情况下，ASP.NET 仅在本地计算机上运行 web 应用程序时显示详细的错误信息。 通常你不想要在 web 应用程序是在 Internet 上公开提供的因为黑客可能能够使用此信息来查找应用程序中的漏洞时显示详细的错误信息。 但是，当要部署的站点或更新到站点，有时内容将会出错，您需要先获取实际错误消息。

若要启用要在远程主机上运行时显示详细的错误消息的应用程序，编辑 Web.config 文件以设置`customErrors`模式关闭、 重新部署应用程序，并运行一次，应用程序：

1. 如果应用程序 Web.config 文件具有`customErrors`中的元素`system.web`元素中，更改`mode`属性为"关闭"。 否则，添加`customErrors`中的元素`system.web`具有元素`mode`属性设置为"off"，如下面的示例中所示：

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. 部署应用程序。
3. 运行应用程序并重复执行任何你此前导致发生错误。 现在，你可以看到什么是实际错误消息。
4. 在纠正错误后，将还原原始`customErrors`设置并重新部署应用程序。

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>访问被拒绝网页中使用 SQL Server Compact

### <a name="scenario"></a>方案

在部署使用 SQL Server Compact 的站点访问数据库的已部署站点中运行某个页面时，你将看到以下错误消息：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

服务器上的网络服务帐户需要能够读取 SQL 服务 Compact 中的本机二进制文件*bin\amd64*或*bin\x86*文件夹中，但它没有读取这些文件夹的权限。 设置在读取网络服务的权限*bin*文件夹，同时确保扩展的子文件夹的权限。

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>无法读取配置文件由于权限不足而

### <a name="scenario"></a>方案

当你单击 Visual Studio 发布按钮以应用程序部署到 IIS 在本地计算机，发布失败和**输出**窗口将显示一条错误消息类似于此：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

若要使用一次单击将发布到 IIS 在本地计算机上，你必须具有管理员权限运行 Visual Studio。 关闭 Visual Studio 并重新启动它具有管理员权限。

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>无法连接到目标计算机...使用指定的进程

### <a name="scenario"></a>方案

当你单击 Visual Studio 发布按钮以部署应用程序，发布失败和**输出**窗口将显示一条错误消息类似于此：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

代理服务器中断与目标服务器的通信。 从 Windows 控制面板或 Internet Explorer 中，选择**Internet 选项**和选择**连接**选项卡。在**互联**对话框中，单击**LAN 设置**。 在**局域网 (LAN) 设置**对话框中，清除**自动检测设置**复选框。 然后再次单击发布按钮。

如果问题仍然存在，请与系统管理员联系，以确定什么可通过代理或防火墙设置。 因为 Web 部署为 Web 管理服务部署 (8172); 使用非标准端口仍然出现该问题对于其他连接，Web 部署使用端口 80。 当将部署到第三方托管提供商时，你通常使用 Web 管理服务。

## <a name="default-net-40-application-pool-does-not-exist"></a>默认.NET 4.0 的应用程序池不存在

### <a name="scenario"></a>方案

当你部署的应用程序需要.NET Framework 4 时，你将看到以下错误消息：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

ASP.NET 4 未安装在 IIS 中。 如果您要部署到的服务器是开发计算机，并且在其上安装 Visual Studio 2010，ASP.NET 4 的计算机上安装，但可能未安装在 IIS 中。 在服务器上要部署到，打开提升的命令提示符并安装在 IIS 中的 ASP.NET 4 中，通过运行以下命令：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

你可能还需要手动设置默认应用程序池的.NET Framework 版本。 有关详细信息，请参阅[作为测试环境部署到 IIS](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)教程。

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>初始化字符串的格式不符合从索引 0 处开始的规范。

### <a name="scenario"></a>方案

部署应用程序使用一次单击后发布，当你运行的页访问数据库获得以下错误消息：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

打开*Web.config*文件中的已部署的站点和检查连接字符串值开头`$(ReplacableToken_`，下面的示例所示：

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

如果连接字符串看上去像此示例中，编辑项目文件并添加以下属性`PropertyGroup`是适用于所有生成配置的元素：

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

然后重新部署应用程序。

## <a name="http-500-internal-server-error"></a>HTTP 500 内部服务器错误

### <a name="scenario"></a>方案

运行已部署的站点时，你将看到以下错误消息，没有特定信息，该值指示错误的原因：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

有许多原因的 500 错误，但如果按照这些教程的一个可能原因是可将一个 XML 元素放入错误的位置中的 XML 转换文件之一。 例如，你会遇到此错误，如果你将插入的转换`<location>`元素下的`<system.web>`而不是直接在`<configuration>`。 在这种情况下的解决方案是更正 XML 转换文件并重新部署。

## <a name="http-50021-internal-server-error"></a>HTTP 500.21 内部服务器错误

### <a name="scenario"></a>方案

运行已部署的站点时，你将看到以下错误消息：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

你已为其部署 ASP.NET 4 中，但 ASP.NET 4 未注册在 IIS 中在服务器的目标站点。 在服务器上打开提升的命令提示符，并通过运行以下命令来注册 ASP.NET 4:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

你可能还需要手动设置默认应用程序池的.NET Framework 版本。 有关详细信息，请参阅[作为测试环境部署到 IIS](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)教程。

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>登录失败打开 SQL Server Express 数据库中应用\_数据

### <a name="scenario"></a>方案

您提供更新*Web.config*文件连接字符串以指向 SQL Server Express 数据库作为*.mdf*文件在你*应用\_数据*文件夹中和第一个运行应用程序，你看到以下错误消息的时间：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

名称*.mdf*文件不能匹配任何已曾经存在你计算机的 SQL Server Express 数据库的名称，即使你删除*.mdf*先前存在的数据库文件。 更改的名称*.mdf*从未为数据库名称和更改使用的名称的文件*Web.config*文件以使用新名称。 作为替代方法，你可以使用[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)删除先前存在的 SQL Server Express 数据库。

## <a name="model-compatibility-cannot-be-checked"></a>模型兼容性无法检查

### <a name="scenario"></a>方案

您提供更新*Web.config*文件连接字符串以指向新的 SQL Server Express 数据库，并运行应用程序首次你看到以下错误消息：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

如果你在计算机上，数据库可能已存在与某些表之前，已曾经使用 Web.config 文件中放置的数据库名称。 选择尚未使用在之前的计算机和更改的新名称*Web.config*文件以点以使用此新的数据库名称。 作为替代方法，你可以使用[SQL Server Express 实用工具](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990)或[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)删除现有数据库。

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>SQL 错误时的脚本尝试创建用户或角色

### <a name="scenario"></a>方案

您要使用在上配置的数据库部署**打包/发布 SQL**选项卡上，在部署过程中运行的 SQL 脚本包括 Create User 或创建角色命令和脚本执行失败时执行这些命令。 你可能会看到更多详细消息，如下所示：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

如果你已配置中的数据库部署时会发生此错误**发布 Web**向导而不是**打包/发布 SQL**选项卡上，创建中的线程[配置和部署](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)论坛和解决方案将添加到此故障排除的页面。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

用来执行部署的用户帐户没有权限来创建用户或角色。 例如，托管公司可能将分配`db_datareader`， `db_datawriter`，和`db_ddladmin`到它为你设置的用户帐户的角色。 这些是足够有关创建大多数的数据库对象，而不是创建用户或角色。 若要避免错误的一种方法是通过从数据库部署中排除用户和角色。 你可以执行此操作通过编辑`PreSource`数据库的元素的自动生成脚本，以使它包括以下属性：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

有关如何编辑`PreSource`元素在项目文件中，请参阅[如何： 编辑项目文件中的部署设置](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx)。 如果用户或开发数据库中的角色需要将位于目标数据库中，请与你托管提供商联系以获取帮助。

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>SQL Server 超时错误时在部署过程中运行自定义脚本

### <a name="scenario"></a>方案

您指定了自定义 SQL 脚本以在部署期间，运行，并且当 Web 部署运行它们时，它们的超时时间。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

运行具有不同的事务模式的多个脚本可能导致超时错误。 默认情况下自动生成的脚本运行在事务中，但自定义脚本不这样做。 如果你选择**抽取数据和/或从现有数据库的架构**选项**打包/发布 SQL**选项卡上，如果你添加自定义 SQL 脚本，必须更改某些脚本上的事务设置，以便所有脚本都使用相同的事务设置。 有关详细信息，请参阅[如何： 部署数据库与 Web 应用程序项目](https://msdn.microsoft.com/library/dd465343.aspx)。

如果你已配置事务设置，以便所有都相同，但仍遇到此错误，可能的解决方法是单独运行这些脚本。 在**数据库脚本**网格中的**打包/发布**SQL 选项卡上，将其清除**包括**会导致超时错误，该脚本的复选框然后发布项目。 然后转回**数据库脚本**网格中，选择该脚本**包括**复选框，然后清除**包括**其他脚本对应的复选框。 然后再次发布该项目。 发布时，此次运行时仅选择自定义的脚本。

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>站点清单的流数据尚不可用

### <a name="scenario"></a>方案

当你要安装包使用*deploy.cmd*文件`t`（测试） 选项，请参阅以下的错误消息：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

错误消息表示命令无法生成测试报告。 但是，如果你使用运行该命令可能会`y`（实际安装） 选项。 此消息仅指示在测试模式下运行该命令的问题。

## <a name="this-application-requires-managedruntimeversion-v40"></a>此应用程序需要 ManagedRuntimeVersion v4.0

### <a name="scenario"></a>方案

当你尝试部署时，你将看到以下错误消息：

 错误： 流数据的 sitemanifest/dbFullSql [@path= C:\TEMP\AdventureWorksGrant.sql']/sqlScript 尚不可用。 你正在使用的应用程序池具有 managedRuntimeVersion 属性设置为 v2.0。 此应用程序需要 v4.0。 

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

ASP.NET 4 未安装在 IIS 中。 如果您要部署到的服务器是开发计算机，并且在其上安装 Visual Studio 2010，ASP.NET 4 的计算机上安装，但可能未安装在 IIS 中。 在服务器上要部署到，打开提升的命令提示符并安装在 IIS 中的 ASP.NET 4 中，通过运行以下命令：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>无法将 Microsoft.Web.Deployment.DeploymentProviderOptions 强制转换

### <a name="scenario"></a>方案

在部署包时，你将看到以下错误消息：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

你尝试部署从 IIS 管理器中使用 Web 部署 1.1 UI 到已安装的 Web 部署 2.0 的服务器。 如果你使用 IIS 的远程管理工具来部署通过导入包中，检查**新提供的功能**建立连接时的对话框。 （此对话框中可能才会显示一次当首次建立连接。 若要清除连接并重新启动，关闭 IIS 管理器并它再次启动通过输入`inetmgr /reset`在命令提示符下。)如果列出的功能之一是**Web 部署 UI**，并且它有一个版本号低于 8，您要部署到的服务器可能必须 1.1 和 2.0 版的 Web 部署安装。 若要从具有 2.0 安装的客户端部署，服务器必须仅 Web Deploy 2.0 安装。 你将需要联系托管提供商若要解决此问题。

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>无法加载 SQL Server Compact 的本机组件

### <a name="scenario"></a>方案

运行已部署的站点时，你将看到以下错误消息：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

已部署的站点并没有*amd64*和*x86*使用本机的程序集，该应用程序下的子文件夹*bin*文件夹。 在上 SQL Server Compact 安装的计算机，本机程序集位于*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*。 到正确的文件夹中的 Visual Studio 项目中获取正确的文件的最佳方法是安装 NuGet SqlServerCompact 包。 包安装添加后期生成脚本，以将复制到本机程序集*amd64*和*x86*。 为了使这些部署，但是，必须手动将其包含在项目中。 有关详细信息，请参阅[部署 SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)教程。

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>部署 Entity Framework Code First 的应用程序后，"路径不是有效的"错误

### <a name="scenario"></a>方案

部署应用程序，如 SQL Server Compact 后者将其数据库存储在应用程序中的文件使用 Entity Framework Code First 迁移和 DBMS\_数据文件夹。 必须配置为在第一个部署之后创建数据库的 Code First 迁移。 当你运行应用程序可以一条错误消息如下例所示：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

代码首先尝试创建数据库，但应用\_数据文件夹不存在。 未有任何文件*应用\_数据*文件夹时部署，或者与您选择**排除应用\_数据**上**打包/发布 Web**选项卡**项目属性**窗口。 如果要复制到服务器的文件夹中不有任何文件，在部署过程不会在服务器上创建一个文件夹。 如果您已经有站点中设置的数据库，在部署过程将删除的文件和*应用\_数据*文件夹本身如果你选择**删除目标位置的其他文件**中发布配置文件。 若要解决此问题，将占位符文件，如中的.txt 文件*应用\_数据*文件夹，请确保你没有**排除应用\_数据**选择，并重新部署。 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"不能使用已分开其基础 RCW 的 COM 对象"。

### <a name="scenario"></a>方案

你已成功使用一键式发布来部署你的应用程序，然后你启动遇到此错误：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

关闭和重新启动 Visual Studio 通常是所有需要来解决此错误。

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>部署失败由于用户凭据用于发布没有 setACL 颁发机构

### <a name="scenario"></a>方案

发布将失败并出错类型的值，该值指示你没有设置文件夹权限 （所使用的用户帐户没有 setACL 机构） 的授权。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

默认情况下，Visual Studio 集对站点的根文件夹的读取权限和写应用程序权限\_数据文件夹。 如果您知道站点文件夹上的默认权限是否正确，并且不需要设置，则禁用此行为通过添加 **&lt;IncludeSetACLProviderOn 目标&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** 到发布配置文件 （以影响一个配置文件） 或 （要影响的所有配置文件） 的 wpp.targets 文件。 有关如何编辑这些文件的信息，请参阅[如何： 编辑配置文件 (.pubxml) 文件中的部署设置](https://msdn.microsoft.com/library/ff398069.aspx)。 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>当应用程序尝试写入到应用程序文件夹时，访问被拒绝错误

### <a name="scenario"></a>方案

你的应用程序错误时它将尝试创建或编辑的文件中的一个应用程序文件夹中，因为它不具有该文件夹的写权限。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

默认情况下，Visual Studio 集对站点的根文件夹的读取权限和写应用程序权限\_数据文件夹。 如果你的应用程序需要的子文件夹的写访问权限，则可以设置为该文件夹的权限中所示[设置文件夹权限](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)和[将部署到生产环境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教程。 如果你的应用程序需要的站点的根文件夹的写访问权限，则必须防止它在根文件夹上设置只读访问权限，通过添加 **&lt;IncludeSetACLProviderOn 目标&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** 到发布配置文件 （以影响一个配置文件） 或 （要影响的所有配置文件） 的 wpp.targets 文件。 有关如何编辑这些文件的信息，请参阅[如何： 编辑配置文件 (.pubxml) 文件中的部署设置](https://msdn.microsoft.com/library/ff398069.aspx)。 <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>配置错误的 targetFramework 特性引用晚于安装的.NET Framework 版本的版本

### <a name="scenario"></a>方案

已成功发布的 web 项目是面向 ASP.NET 4.5，但当你运行应用程序 (与`customErrors`模式设置为"关闭"Web.config 文件中) 收到以下错误：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

错误页的源错误框突出从 Web.config 的以下行显示为错误的原因：

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

服务器不支持 ASP.NET 4.5。 联系托管提供商来确定何时及是否可添加对 ASP.NET 4.5。 如果升级服务器不是一个选项，则必须部署面向 4 或更早版本的 ASP.NET web 项目相反。如果将 ASP.NET 4 或更早版本的 web 项目部署到相同的目标中，选择**删除目标位置的其他文件**上的复选框**设置**选项卡**发布 Web**向导。 如果你未选中**删除目标位置的其他文件**，你将继续获取配置错误页。

项目**属性**windows 包含目标框架下拉列表，但你不能解决此问题通过只需更改，从**.NET Framework 4.5**到**.NET Framework 4**. 如果你将目标框架更改为更早版本的 framework 版本时，项目将仍具有对的更高版本的 framework 版本的程序集的引用，而不会运行。 你必须手动更改这些引用，或创建新的项目面向.NET Framework 4 或更早版本。 有关详细信息，请参阅[.NET Framework 面向网站](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx)。

>[!div class="step-by-step"]
[上一篇](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
