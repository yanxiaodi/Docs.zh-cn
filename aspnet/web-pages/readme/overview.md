---
uid: web-pages/readme/overview
title: "WebMatrix 自述文件 |Microsoft 文档"
author: rick-anderson
description: "WebMatrix 和 ASP.NET Web 页 (Razor) 1.0 版本自述文件"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: b8402aa3db1b2566878c4d56212facbbb2925eec
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="webmatrix-readme"></a>WebMatrix 自述文件
====================
2011 年 1 月 13日

## <a name="contents"></a>内容

> [!NOTE]
> 本自述文件适用于 1.0 版本的 WebMatrix。


- [概述](#Overview)
- [安装](#Installation_Notes)
- [如何发布应用程序](#InstructionsForPublishingApplications)
- [更改和问题](#ChangesAndIssues)

    - [WebMatrix 1.0 安装](#Known_Issues_Installation)
    - [ASP.NET 网页](#Known_Issues_ASPNET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [安装应用程序](#Known_Issues_Installing_Applications)
    - [发布应用程序](#Known_Issues_Publishing_Applications)
- [有关详细信息](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>概述

> Microsoft WebMatrix 1.0 是一个免费 web 开发堆栈，以分钟为单位安装。 它与数据库和编程框架创建单一的集成体验集成的 web 服务器。 你可以使用 WebMatrix 来简化的方法的代码、 测试和发布你自己的 ASP.NET 或 PHP 网站，或你可以使用 WebMatrix 启动使用如 DotNetNuke、 Umbraco、 WordPress、 Joomla 的常用的开源应用程序的新网站。 WebMatrix 使用相同的功能强大的 web 服务器、 数据库引擎和运行你的网站在 internet 上，从而可以从开发到生产环境的转换平滑无缝的框架环境。


<a id="Installation_Notes"></a>

## <a name="installation"></a>安装

> 若要安装 WebMatrix 1.0，必须首先安装[Microsoft Web 平台安装程序 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)。 安装 Web 平台安装程序后，可用于安装 WebMatrix。
> 
> 如果在安装过程中遇到问题，请参阅[Microsoft Web 平台安装程序的故障排除问题](https://go.microsoft.com/fwlink/?LinkId=196212)。


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>如何发布应用程序

> 请参阅[发布应用程序的分步说明](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>更改和问题

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>WebMatrix 1.0 安装问题

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>问题： WebMatrix 1.0 是仅在支持 Microsoft.NET Framework 4 的平台上可用

> .NET Framework 版本 4 是必需的 WebMatrix。 在某些情况下，WebMatrix 1.0 安装程序将让你尝试在不受支持的配置集的一部分的平台上安装。 具体而言，不带 SP1 更新的 Windows Vista 将你可以开始安装 WebMatrix 中，但该.NET Framework 4 组件将失败并阻止你安装。
> 
> **Workaround**  
> 安装上受支持的平台，其中包括：
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista SP1 或更高版本
> - Windows XP SP3
> - Windows Server 2003 SP2


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>问题： 无法安装 WebMatrix 1.0，如果没有 Microsoft Visual Studio 2008 SP1 的情况下已安装 Microsoft Visual Studio 2008

> **Workaround**  
> 安装[Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en)从 Microsoft 下载中心获取。


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>问题： SQL Server Compact 4.0 某些程序集未安装在 GAC 中

> 在 64 位计算机上安装 SQL Server Compact 4.0 和计算机具有仅.NET Framework 3.5 SP1 客户端安装配置文件时，SQL Server Compact 4.0 的托管程序集不会放置在全局程序集缓存 (GAC)。 未安装在 GAC 中的托管程序集是：
> 
> - *System.Data.SqlServerCe.dll* （ADO.NET 提供程序）
> - *System.Data.SqlServerCe.Entity.dll* （ADO.NET 实体框架）
> 
> **Workaround**  
> 卸载 SQL Server Compact 4.0。 下载并安装.NET Framework 3.5 SP1 的完整版本，从以下位置：  
>   
> [Microsoft.NET Framework 3.5 Service pack 1 （完全包）](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> 然后重新安装 SQL Server Compact 4.0。


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>问题： 无法卸载 SQL Server Compact 使用命令行

> 卸载 SQL Server Compact 使用命令行选项不在此版本中无效。
> 
> **Workaround**  
> 使用*程序和功能*Windows 控制面板卸载 Microsoft SQL Server Compact 4.0 中。


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>ASP.NET 网页

文档本部分介绍的新增功能、 更改和 1.0 版本的 ASP.NET Web Pages 使用 Razor 语法的已知的问题。

- [新功能](#NewFeatures)
- [更改](#Changes)
- [问题](#Issues)

#### <a id="NewFeatures"></a>新功能

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>配置设置添加禁用包管理器的新增内容：

> 一个新`asp:AdminManagerEnabled`密钥是可用于`<appSettings>`中的元素*web.config*文件，可让你完全禁用程序包管理器。 此元素的默认值为 true，这意味着，如果它不包括在*web.config*是否启用了文件，包管理器。 若要禁用的程序包管理器，以下将元素添加到*web.config*在网站的根目录中的文件：
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>更改

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>重命名为"asp: AdminFolderVirtualPath"的更改:"webPages:AdminFolderVirtualPath"密钥

> `webPages:AdminFolderVirtualPath`可以添加到的密钥*web.config*文件，以指定包管理器的位置具有已重命名为使用`asp:`命名空间而不是`webPages`命名空间。 如果你已使用此元素，必须在配置文件中时将它重命名。


#### <a id="Issues"></a>已知的问题

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>问题： 不再识别的成员资格用户的密码

> 用于创建和存储成员身份 （登录名） 密码算法已更改为更安全。 因此，将不识别为成员 （用户） 创建的 ASP.NET Razor 的 Beta 版本中存储的密码。 
> 
> **解决方法**站点尚未放到生产环境，如果从成员资格数据库中删除用户记录。 如果实时数据库，以编程方式重新生成成员资格数据库中的现有密码。


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>问题： 意外的行为时使用的成员身份的自定义用户表

> 若要初始化的成员资格提供程序的 ASP.NET Razor 网站，你可以调用`WebSecurity.InitializeDatabaseConnection`方法。 (在 WebMatrix 中，入门站点模板包括对在此方法的调用 *\_AppStart.cshtml*文件。)如果`autoCreateTables`此方法的参数设置为 true (默认情况下，它设置为 true 的入门站点模板中)，并且如果无法识别的表名称传递给方法 （第二个参数），该方法不会引发错误。 相反，它会自动创建表。
> 
> 这可能会造成问题，如果你想要使用的成员身份的自定义用户表，但传递到的错误的表名称`WebSecurity.InitializeDatabaseConnection`方法。 因为该方法不会默认情况下引发错误如果您指定的表不存在，而且它改为创建一个新表，则应用程序可以似乎无法正常工作。 但是，依赖于自定义用户表 （和在其中的字段） 的应用程序代码可能最终失败，意外错误。
> 
> **Workaround**  
> 请确保名称传递中`InitializeDatabaseConnection`方法匹配项的用户配置文件表中成员资格数据库中，或确保`autoCreateTables`参数设置为 false。


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a>问题： 错误消息"管理员模块需要访问 ~/App\_数据"

> 在某些情况下，尝试创建用户，或以其他方式处理 ASP.NET 成员资格系统可能会导致页后，可以显示错误*该管理员模块需要访问 ~/App\_数据*。 发生这种情况是如果 IIS 或 IIS Express 下运行的帐户不具有权限才能创建和写入*应用\_数据*网站根下的文件夹。 
> 
> **解决方法**手动创建*应用\_数据*网站的文件夹。 请确保应用程序 （通常网络服务） 下运行的 Windows 帐户具有读/写权限的应用程序的根文件夹和子文件夹，例如，应用程序\_数据。 知识库文章中提供了更多详细的信息[SQL Server Express 用户实例化和 ASP.net Web 应用程序项目问题](https://support.microsoft.com/kb/2002980)。


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>问题:"无法生成 SQL Server 的用户实例"错误

> 如果 WebMatrix Web 应用程序使用 SQL Server Express 和 Windows 7 或 Windows Server 2008 R2 上运行 IIS 7.5，可能会看到一个错误，指示 SQL Server 无法在运行时检索用户的本地应用程序路径。
> 
> **解决方法**请确保应用程序 （通常网络服务） 下运行的 Windows 帐户如具有读/写权限的应用程序的根文件夹和子文件夹*应用\_数据*. 知识库文章中提供了更多详细的信息[SQL Server Express 用户实例化和 ASP.net Web 应用程序项目问题](https://support.microsoft.com/kb/2002980)。


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>问题： 包含包管理器资源或包管理器密码的文件是 servable 下 IIS 6.0 及更早版本

> 如果你部署的 ASP.NET Web 页 (Razor) 应用程序使用 RC2 版本中，生成和应用程序包含*password.txt*或*packagesources.txt*文件下*/App\_数据/admin*，IIS 6.0 将处理该文件，如果请求，可能要公开你的包管理器实例的密码。 
> 
> **解决方法**重命名*password.txt*或*packagesources.txt*文件为*password.config*或*packagesources.config*.默认情况下，IIS 6.0 不适用于具有文件*.config*扩展。 (在 IIS 7 中，在任何文件*应用\_数据*文件夹提供服务，因此不需要重命名文件。)


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>问题： 卸载使用 Beta 3 版本安装的包不会完全删除包组件

> 如果你安装包在 Beta 3 版本中使用程序包管理器，然后尝试使用当前版本卸载它，未完全卸载包。 使用包管理器的**卸载**按钮删除某些组件，但会导致包的库的代码并不能更新*package.config*文件。
> 
> **解决方法**   
> 执行以下步骤：  
> 1. 删除*应用\_Data\packages*文件夹。 这将删除所有包。   
> 2. 删除*packages.config*在网站的根目录中的文件。


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>问题： 在 Visual Studio 中，调用的基于 web 的程序包管理器使应用程序脱机

> 如果你在 Visual Studio (不 WebMatrix) 中工作，并使用*\_管理员*启动包管理器，Visual Studio 的功能使应用程序脱机，并发布*应用\_offline.htm*到网站根目录中，这使你能够使用包管理器。
> 
> [!NOTE]
> 尽管使用基于 web 的程序包管理器界面时，通常会看到此行为，但相同的行为发生如果添加、 删除或修改中的任何文件*应用\_数据*文件夹。
> 
> **解决方法**   
> 若要使用 Visual Studio 中的包，而不是基于 web 的程序包管理器使用 NuGet 扩展。 有关信息，请参阅[NuGet 文档](https://docs.microsoft.com/nuget/)。 如果你正在使用中的其他文件*应用\_数据*文件夹，请考虑在其他位置来避免此问题保留这些文件。 如果不可行，删除*应用\_offline.htm*手动文件，或者等待，直到站点重新联机自动 （默认为在 30 秒后）。


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>问题： Visual Studio IntelliSense 和项目模板仅在 ASP.NET MVC 3 版本中可用

> 安装 ASP.NET 网页不还安装 tools for Visual Studio 的 ASP.NET Web Pages 应用程序的智能感知和项目模板等。
> 
> **解决方法**若要使用 Visual Studio 中的 ASP.NET Web Pages 应用智能感知和项目模板，ASP.NET MVC 3 RC 通过 Web 平台安装程序的安装或[独立安装程序](https://go.microsoft.com/fwlink/?LinkID=191797)。


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>问题： 读取源或其他通过代理服务器的外部数据

> 如果运行站点的服务器位于代理服务器后面，你可能需要配置中的代理信息*web.config*为了能够读取来自你的站点之外的信息的文件。 例如，如果你使用`ReCaptcha`帮助器，帮助器与 reCAPTCHA 服务通信，但可能会阻止由你的代理服务器。 同样，使用在 ASP.NET Web 页中，如源包管理器使用的源可能需要代理配置。
> 
> 如果你遇到中使用的外部服务或使用源的包的问题，将以下元素放入应用程序的根*web.config*文件：
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> 有关配置代理服务器的详细信息，请参阅[&lt;代理&gt;元素 （网络设置）](https://msdn.microsoft.com/library/sa91de1e.aspx) MSDN 网站上。


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>问题： 卸载.NET Framework 版本 4 禁用带有 Razor 语法的 ASP.NET Web Pages

> 如果你卸载.NET Framework 版本 4，然后重新安装它，将禁用使用 Razor 语法的 ASP.NET 网页。 与页*.cshtml*扩展可能无法正常运行。 ASP.NET Web Pages 机根目录中注册程序集*web.config*文件，并删除.NET Framework 中删除该文件。 重新安装.NET Framework 安装新版本的配置文件中，但不会添加 ASP.NET 网页的程序集引用。
> 
> **解决方法**后重新安装.NET Framework，请重新安装 ASP.NET 网页使用 Razor 语法。 这将添加到下面的元素*web.config*机根目录，这通常是在以下位置中的文件：  
>   
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>问题： 无扩展名的 Url 未找到 IIS 7 或 IIS 7.5 上的.cshtml/.vbhtml 文件

> 在 IIS 7 或 IIS 7.5 上，如下所示 URL 的请求不能找到页具有*.cshtml*或*.vbhtml*扩展：  
>   
> `http://www.example.com/ExampleSite/ExampleFile`  
>   
> 由于 URL 重写未启用默认情况下为 IIS 7 或 IIS 7.5，因此会出现此问题。 很可能的方案是，你看不见问题时测试使用本地 IIS Express，但在将你的网站部署到托管的网站时，会遇到它。
> 
> **Workaround**
> 
> - 如果你可以控制在服务器计算机，在服务器计算机上安装的更新中所述[有可用启用某些 IIS 7.0 或 IIS 7.5 的处理程序，来处理请求其 Url 不要以句点结尾的更新](https://support.microsoft.com/kb/980368)。
> - 如果您没有对服务器计算机的控制 （例如，您要部署到托管网站），将以下代码添加到网站的*web.config*文件： 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>问题： 应用程序部署到不具有 SQL Server Compact 安装的计算机

> 其中 SQL Server Compact 未安装的计算机上可以运行的应用程序包括 SQL Server Compact 数据库。 Microsoft WebMatrix 1.0 自动为你复制这些二进制文件，并执行相应*web.config*文件转换。
> 
> **解决方法**如果你需要将这些文件复制并使*web.config*文件更改手动，执行以下操作：
> 
> 1. 将复制到的数据库引擎程序集*Bin*文件夹 （和子文件夹） 的目标计算机上的应用程序：  
> 
>     - Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>         **to** *\Bin*
>     - 复制*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\* * * 到 * * * \Bin\x86*
>     - 复制*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **到 * * * \Bin\amd64*
> 2. 在网站的根文件夹中创建或打开*web.config*文件。 (在 WebMatrix 1.0 中，此文件类型是如果你单击可用**所有**中**选择文件类型**对话框。)
> 3. 添加以下元素的子级`<configuration>`元素 (而不是在`<system.web>`元素):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>问题:"数据库"和"WebGrid"帮助程序中不起作用中等信任在 Visual Basic 中

> 如果你使用的 Visual Basic (创建*.vbhtml*文件)，则`Database`和`WebGrid`如果应用程序设置为使用中等信任帮助器将无法工作。
> 
> **Workaround**  
> 如果你使用 Visual Studio 2010，你可以通过安装 Service Pack 1 发行版来解决此问题。 可用的 SP1 版本的最终版本之前，你可以下载从 SP1 Beta 版本[Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) Microsoft Download Center 上的页。   
>   
> 如果这不可行，或如果不使用 Visual Studio 2010，则可以暂时设置用于完全信任的应用程序。


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>问题:"ApplicationPart"资源是从外部访问

> 如果程序集包含派生自的对象`ApplicationPart`类，程序集的资源公开的`ResourceRouteHandler`类。 例如，考虑以下 URL:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> 此请求下载中的资源字符串的所有*System.Web.WebPages.Administration.dll*程序集。 下载的所有嵌入的资源 （甚至那些不应为静态内容提供）。 如果嵌入的资源包含敏感信息，这可以是一个安全风险。 
> 
> **解决方法**   
> 如果你创建**ApplicationPart**对象，请确保与该关联的嵌入的资源**ApplicationPart**对象的程序集不包含敏感信息。


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> WebMatrix 安装问题的信息，请参阅[WebMatrix 安装问题](#Known_Issues_Installation)本文档前面的。


文档本部分介绍 WebMatrix 开发环境的已知的问题。

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>问题： 用户名或密码的 web.config 文件中的数据库连接字符串中的更改不会反映在数据库工作区

> **Workaround**  
> 
> 1. 在*web.config*文件中，更改连接字符串中的数据库名称 （例如，将"1"添加到它）。
> 2. 保存*web.config*文件。
> 3. 单击**数据库**和刷新。
> 4. 更改中的连接字符串中的数据库名称*web.config*回原始的数据库名称的文件。
> 5. 保存*web.config*文件。
> 6. 单击**数据库**和刷新。


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>问题： 无法删除文件夹创建的 WebMatrix

> 如果使用提升的权限运行 WebMatrix (即你开始使用 WebMatrix**以管理员身份运行**Windows 中的选项)，不能使用 Windows 资源管理器删除创建的 WebMatrix 的文件夹。
> 
> **Workaround**  
> 运行 Windows 资源管理器使用提升的权限。 请执行这些步骤：  
> 
> 1. 在 Windows 中，单击**启动**。
> 2. 输入"Windows 资源管理器"，然后右键单击的项**Windows 资源管理器**。
> 3. 单击**以管理员身份运行**。 然后，你可以删除这些文件夹。


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>问题： WebMatrix 1.0 不能执行某些任务需要提升权限

> WebMatrix 1.0 程序无法执行某些任务，需要提升，例如在以下情况下安装其他组件：
> 
> - 在 Windows Vista 或 Windows 7，使用不具备管理员权限的帐户登录和用户帐户控制 (UAC) 处于禁用状态。
> - 您正在使用 Microsoft Windows XP 或 Microsoft Windows Server 2003。
> 
> **Workaround**  
> 在 WebMatrix 1.0 中的大多数任务不需要管理权限。 对于那些确实，你可以执行该操作作为管理员，或请按照下列步骤：
> 
> - 在 Windows Vista 或 Windows 7 上，启用 UAC。
> - 在 Windows XP 中，将用户添加到管理员安全组。


#### <a name="issue-site-from-web-gallery-is-disabled"></a>问题:"从 Web 库的站点"处于禁用状态

> **站点从 Web 库**如果未安装 Web 平台安装程序 3.0 选项被禁用。
> 
> **Workaround**  
> 安装[Microsoft Web 平台安装程序 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)。


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>问题： Google Chrome 不能作为运行选项

> 在浏览器的列表中不显示 Google Chrome**运行**上**主页**选项卡。
> 
> **Workaround**  
> 某些版本的 Google Chrome 并不能注册正确与 Windows 中的默认程序功能。 一种解决方法，启动 Google Chrome 中，单击*自定义和控制 Google Chrome*菜单上，单击*选项*，然后单击*请 Google Chrome 我默认浏览器*。


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>问题:"外键"对话框中不允许输入为主键

> **外键**对话框中不允许你从主键表中输入的主键名称。
> 
> **Workaround**  
> 这是有意。 不需要输入主键表的主键的名称。


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>问题： IntelliSense 不能在 WebMatrix 的 Razor 语法、 C# 或 Visual Basic

> 在 WebMatrix 中用于 HTML 和 CSS 支持 IntelliSense。 但是，它也无法供其他语言。 
> 
> **解决方法**   
> 无。


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>问题： 用于 HTML 和 CSS IntelliSense 建议不是上下文相关的元素

> 在 WebMatrix 中标记的 IntelliSense 支持使用 HTML [XHTML 1.0 Transitional 架构](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional)和 CSS 使用[CSS 2.1 架构](http://www.w3.org/TR/CSS2/)。 IntelliSense 基于这些特定的架构，因为某些标记、 属性可能不适合用于当前页或样式定义建议。 为 HTML，它还可能导致意外的建议，在内容中，可能会被解释为格式不正确 XHTML （例如，当未关闭标记）。 此问题可能会更明显，如果插入点位于一个不完整的标记;在这种情况下，IntelliSense 可能建议新开始标记，或提供其他不正确的建议。 
> 
> **解决方法**   
> 对于 HTML，请确保你正在在格式正确，完成 XHTML 页面内。 针对 CSS，目前没有解决方法。


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>问题： IntelliSense 不会被调用键入时

> 有时，如 HTML 或 CSS 在编辑器中输入可能不调用 IntelliSense。 具体而言，这可能会发生时插入点，则直接旁边另一个元素或在文件末尾。 
> 
> **解决方法**   
> 请确保没有绕插入点的空白和插入点不在文件末尾。 你也可以通过按 Ctrl + Space 手动调用 IntelliSense。


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>问题： 没有用户界面是可用于禁用 IntelliSense

> WebMatrix 1.0 允许禁用 IntelliSense 的任何 UI 或笔势。 
> 
> **解决方法**   
> 启动 WebMatrix 使用以下命令，其中包括禁用 IntelliSense 的交换机：  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

IIS Express 具有其自己的自述文件，它是可通过以下 URL:

[https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact 具有其自己的自述文件，它是可通过以下 URL:

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

有关涉及 WebMatrix 的一部分安装 SQL Server Compact 的问题的信息，请参阅[WebMatrix 安装问题](#Known_Issues_Installation)本文档前面的。

### <a id="Known_Issues_Installing_Applications"></a>安装应用程序

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>问题： 安装应用程序可能需要长时间如果用户的 My Documents 文件夹重定向到网络共享

> **Workaround**  
> 无。 应用程序可能需要一段时间才能安装，但将正确安装。


### <a id="Known_Issues_Publishing_Applications"></a>发布应用程序

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>发布 SQL Compact 数据库时需要问题:"无法获得的权限"错误

> WebMatrix 不完全支持为 SQL Server Compact 的支持的二进制文件部署到正在运行.NET Framework 版本 3.5 使用中等信任配置的服务器。
> 
> **Workaround**  
> 首选的解决方法是在服务器上安装.NET Framework 4。 或者，请执行以下操作：
> 
> 1. 添加到下列元素`SecurityClasses`主题中*Web\_MediumTrust.config*文件：
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. 创建一个新权限集*Web\_MediumTrust.config*文件所需的以下权限：
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. 应用将权限设置为 SQL Server Compact 通过将以下元素放入*Web\_MediumTrust.config*文件：
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>问题： 库和 PhpBB web 应用程序在发布后显示"服务不可用"错误

> 在某些情况下，发布的应用程序将导致"服务不可用"错误。
> 
> **Workaround**  
> 在 WebMatrix 中，添加一个反斜杠 (\)到中的服务器名称的末尾**发布设置**窗口，然后将发布再次将应用程序。


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>问题： Moodle 网站布局和链接后中断发布

> 发布 Moodle 应用程序后，应用程序无法正常工作。
> 
> **Workaround**  
> 在 WebMatrix 中，将斜杠 （/） 添加到末尾**站点名称**字段**发布设置**窗口，然后将发布再次将应用程序。


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>问题： 发布 nopCommerce 失败，出现数据库错误

> 发布 nopCommerce 失败并报告数据库错误，如"插入 nop\_日志表失败。"
> 
> **Workaround**  
> 
> 1. 在 WebMatrix 中，单击**运行**以启动 nopCommerce 本地。
> 2. 登录到管理页。
> 3. 单击**系统**菜单。
> 4. 单击**日志**选项。
> 5. 单击**清除日志**按钮。
> 6. 再次发布 nopCommerce。


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>问题： 当你下载的已发布的站点 Silverstripe CMS 显示"HTTP 500 PHP FCGI 错误"

> **Workaround**  
> 单击后**下载发布站点**，跳过`silverstripe-cache/manifest_main`中**发布预览**。 此文件用于缓存目的，并且是特定于每台计算机。


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>问题： 从属文本显示"服务器错误 '/' 应用程序中"时下载已发布的站点

> **Workaround**  
> 打开站点的*web.config*文件并将替换 SQL Server 管理员凭据 （"sa"凭据） 的用户 ID 和数据库连接字符串中的密码。
> 
> 或者，遵循这些步骤，才能让你登录时所用的用户帐户`db_owner`权限：
> 
> 1. 安装 SQL Server Management Studio 使用 Web 平台安装程序。
> 2. 连接到本地的 SQL Server Express 实例 (默认情况下， `.\SQLEXPRESS`)。
> 3. 单击**数据库** &gt; *[localSubtextDatabase]* &gt; **安全** &gt; **用户**&gt; *[localSubtextUser*] (默认值是`subtextuser`]，右键单击，然后单击**属性**。
> 4. 选择**db\_所有者**角色成员资格部分中。


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>问题： 站点可能无法在发布如果"目标 URL"字段不 http:// 或 https:// 开头的前缀后

> 在**发布设置**对话框中，如果目标 URL 不以开头`http://`或`https://`，在部署之后，站点可能不工作。
> 
> **Workaround**  
> 请确保在发布站点时中的目标 URL 之前**发布设置**对话框开头`http://`或`https://`。


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>问题： 发布 MySQL 数据库失败，出现错误"未能发布数据库。 可能的原因是远程数据库无法运行脚本。"

> 多种原因可能会导致错误。 你可以看到此错误的原因之一是如果数据库脚本包含单引号字符 （'） 和目标 MySQL 数据库的默认字符集不为 utf-8。
> 
> **Workaround**  
> 设置的默认字符集远程 MySQL 数据库为 utf-8。


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>问题： 某些链接中是不可见 DotNetNuke 后发布或从其下载站点

> 如果发布或下载 DotNetNuke 站点时，你可能需要清除缓存以获取要在站点上显示的新链接。
> 
> **Workaround**
> 
> 1. "主机"以登录。
> 2. 转到的主机菜单并选择**主机设置**。
> 3. 向下和下的滚动**高级设置**，展开**性能设置**。
> 4. 单击**清除缓存**页的链接。
> 5. 转到页面底部，重新启动应用程序。


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>问题： 一些链接 AtomSite 中的可能会中断后你下载的已发布的站点

> **Workaround**  
> 在*service.config*文件， *users.config*文件，以及所有*.xml*文件，将 URL 字符串 (例如， `http://myhost.com/atomsite`) 替换为本地 (例如， `http://localhost:1239`).


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>问题： WordPress 等基于 MySQL 的应用程序不能发布，并将数据库错误报告

> 默认情况下，WebMatrix utf-8 字符集安装 MySQL。 如果你将 MySQL 安装在你自己和设置的字符不是 utf-8 （例如，它是 Latin1），数据库的发布过程可能会失败。
> 
> **Workaround**
> 
> 1. 更改为 utf-8 的 MySQL 的字符集。 (有关详细信息，请参阅[服务器字符集和排序规则](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html)MySQL 网站上。)
> 2. 重新安装应用程序。
> 3. 重新发布应用程序。


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>问题:"下载发布站点"具有基于浏览器的安装程序的应用程序失败

> 某些应用程序 (例如，Kentico CMS) 要求你在浏览器中启动它们以执行安装后安装程序，如创建数据库。 如果你发布此类应用程序，而不完成基于浏览器的安装程序，尝试从远程服务器下载相同的站点将失败。
> 
> **Workaround**  
> 在将站点发布之前完成基于浏览器的设置。


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>问题:"下载发布站点"失败，出现数据库错误 DotNetNuke 和 Kooboo CMS

> 如果你尝试从服务器下载的应用程序，您必须在数据库连接字符串中的管理员凭据**发布设置**对话框中，你可能会看到发布日志中的以下错误：
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Workaround**  
> 如果可行，重新发布该网站 （或将其发布） 的数据库使用非管理员凭据。


<a id="More_Info"></a>

## <a name="for-more-information"></a>更多信息

有关 WebMatrix 1.0 的详细信息，请参阅以下网站：

- [IIS.net](http://iis.net/)
- [ASP.NET 2.0](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. 保留所有权利。 [使用条款](https://msdn.microsoft.cos/cc300389.aspx)。
