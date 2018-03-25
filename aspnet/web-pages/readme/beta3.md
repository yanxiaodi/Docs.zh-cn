---
uid: web-pages/readme/beta3
title: "Web 矩阵和 ASP.NET Web 页 (Razor) Beta 3 版本自述文件 |Microsoft 文档"
author: rick-anderson
description: "Web Matrix 和 ASP.NET Web 页 (Razor) Beta 3 版本自述文件"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: def2f4b3e54c8de539e10c1b526a1dababeca8fb
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Web Matrix 和 ASP.NET Web 页 (Razor) Beta 3 版本自述文件
====================
> Web Matrix 和 ASP.NET Web 页 (Razor) Beta 3 版本自述文件

2010 年 11 月 9日

## <a name="contents"></a>内容

- [概述](#Overview)
- [安装](#Installation_Notes)
- [新功能、 更改和 Beta 3 版本中的已知问题](#Known_Issues)

    - [WebMatrix 安装问题](#Known_Issues_Installation)
    - [ASP.NET 网页](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [安装应用程序](#Known_Issues_Installing_Applications)
    - [发布应用程序](#Known_Issues_Publishing_Applications)
    - [其他问题](#Known_Issues_Other_Issues)
- [有关详细信息](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>概述

> Microsoft WebMatrix Beta 是以分钟为单位安装免费的 web 开发堆栈。 它与数据库和编程框架创建单一的集成体验集成的 web 服务器。 你可以使用 WebMatrix Beta 来简化的方法的代码、 测试和发布你自己的 ASP.NET 或 PHP 网站，或者你可以使用 WebMatrix Beta 以启动使用如 DotNetNuke、 Umbraco、 WordPress、 Joomla 的常用的开源应用程序的新网站。 WebMatrix Beta 使用相同的功能强大的 web 服务器、 数据库引擎和运行你的网站在 internet 上，从而可以从开发到生产环境的转换平滑无缝的框架环境。


<a id="Installation_Notes"></a>

## <a name="installation"></a>安装

> 若要安装 WebMatrix Beta 3，你可以使用[Microsoft Web 平台安装程序 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)。 安装 Web 平台安装程序后，可用于安装 WebMatrix Beta 3。
> 
> 如果在安装过程中遇到问题，请参阅[Microsoft Web 平台安装程序的故障排除问题](https://go.microsoft.com/fwlink/?LinkId=196212)。


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>发布应用程序的说明

> 请参阅[发布应用程序的分步说明](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>新功能、 更改 andKnown 问题

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>WebMatrix Beta 3 安装

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>问题： WebMatrix Beta 3 才支持 Microsoft.NET Framework 4 的平台上可用

> .NET Framework 版本 4 是必需的 WebMatrix Beta。 在某些情况下，WebMatrix Beta 安装程序将让你尝试在不受支持的配置集的一部分的平台上安装。 具体而言，不带 SP1 更新的 Windows Vista 将你可以开始安装 WebMatrix Beta，但该.NET Framework 4 组件将失败并阻止你安装。
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


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>如果没有 Microsoft Visual Studio 2008 SP1 的情况下已安装 Microsoft Visual Studio 2008，问题： 不能安装 WebMatrix Beta 3

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

文档本部分介绍的新增功能、 更改和 Beta 3 版本的 ASP.NET Web Pages 使用 Razor 语法的已知的问题。

- [新功能](#NewFeatures)
- [更改](#Changes)
- [问题](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>使用 Razor 语法的 ASP.NET 网页测试版 3 中的新增功能

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>"Html.Raw"方法呈现未编码的标记的新增内容：

> 新`Html.Raw`方法可让你呈现为而不是呈现编码的输出的标记的 HTML 标记。 （默认情况下，ASP.NET Razor 编码字符串在呈现它们之前。）语法为：
> 
> `Html.Raw(value)`
> 
> 下面的示例演示如何使用 `Html.Raw`：
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>使用 Razor 语法的 ASP.NET 网页测试版 3 中的更改

#### <a name="change-hrefattribute-method-removed"></a>删除的更改:"HrefAttribute"方法

> `HrefAttribute`方法`WebPage`类已删除。 此帮助器用于在 Url 中的不安全字符进行编码。 它不再需要因为 ASP.NET Razor 自动对字符串进行编码。 (使用新`Html.Raw`方法呈现未编码的字符串。)


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>更改： 语法声明性"@helper"更改帮助器

> 在 Beta 3 版本中，ASP.NET 更改如何分析帮助程序，使用创建`@helper`语法。 从本质上讲，`@helper`语法现在解析为一个代码块而不是作为标记可以包含代码块。 因此，帮助器内的代码不需要用`@{ }`块。 相反，标记帮助器内的必须是显式包含在 HTML 元素或 ASP.NET Razor`<text></text>`标记。
> 
> 例如，以下`@helper`语法适用于 Beta 3 版本：
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> 在 Beta 3 版本中，必须更改此帮助器以类似于以下示例：
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> 请注意，`@{ }`不再使用的初始代码的帮助程序中的字符。 这是因为以下帮助器的内容视为一个代码块，默认情况下。 帮助器呈现标记，开头开始`<a>`标记。 如果帮助器必须呈现的纯文本或不包含结束标记的标记 (例如，`<meta>`标记)，要呈现的内容必须在`<text></text>`标记。


#### <a name="change-webpagecontexthttpcontext-removed"></a>Change: "WebPageContext.HttpContext" removed

> `WebPageContext.HttpContext`属性已删除。 请改用 `HttpContext.Current`。 (`WebPageContext.HttpContext`属性只需包装这。)


#### <a name="change-facebook-helper-moved-to-new-package"></a>移动到新的包的更改:"Facebook"帮助程序

> `Facebook`帮助器已移至*Facebook.Helper*库，其中包括`Facebook`帮助器和其他功能。 你必须安装此库作为一个单独的包，"安装帮助器使用程序包管理器"中所述在本教程[Getting Started with ASP.NET 页](https://go.microsoft.com/fwlink/?LinkId=202889)。


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>更改： 成员资格、 角色和安全类型移动到新的程序集

> 以下类型都已移动到`WebMatrix.WebData`程序集：
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>移动到 System.Web.WebPages.dll 程序集的更改:"TagBuilder"类

> `TagBuilde` R 类已移动到 System.Web.WebPages.dll 程序集。 以前，这样做的程序集中的是 ASP.NET MVC 的一部分。 此更改意味着你不需要安装 ASP.NET MVC，以便使用`TagBuilder`类。
> 
> 但是，此类是仍在`System.Web.Mvc`命名空间。 若要使用`TagBuilder`类 （例如，在自定义 ASP.NET Razor 帮助程序），你必须引用命名空间 (例如，通过添加`@using System.Web.Mvc`到你的代码)。


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>更改： 请求验证语法更改;删除的"验证"类

> 在 Beta 3 版本中，若要禁用验证的单个字段或集的字段，你可以调用`Validation.Exclude`方法，同时传入的名称或从验证中排除的字段的名称。 新的语法是跳过验证的 Beta 3 版本中可用。 `Validation`中 Beta 3 方法，用于已删除。
> 
> > [!NOTE]
> > 如果用户尝试上载 HTML 标记 （例如，通过在页面上使用的富文本编辑器） 未禁用请求验证，如果网站将报告类错误*从客户端检测到潜在危险的Request.Form值*和未接受用户输入。 如果禁用请求验证，你必须手动检查用户输入，以确保它不包含具有潜在危险标记或者脚本使用类似[Microsoft 反跨站点脚本库 V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651)。
> 
> 
> 若要禁用自动请求验证，请调用`Request.Unvalidated`方法，将其传递的字段或你想要绕过请求验证的其他文章对象的名称。 你可以使用此方法绕过验证中的任何项`Form`， `QueryString`， `Cookies`，和`ServerVariables`集合。 下面的示例演示如何使用`Unvalidated`方法：
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>使用 Razor 语法的 ASP.NET 网页的已知的问题

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>问题： 意外的行为时使用的成员身份的自定义用户表

> 若要初始化的成员资格提供程序的 ASP.NET Razor 网站，你可以调用`WebSecurity.InitializeDatabaseConnection`方法。 (在 WebMatrix 中，入门站点模板包括对在此方法的调用 *\_AppStart.cshtml*文件。)如果`autoCreateTables`此方法的参数设置为 true (默认情况下，它设置为 true 的入门站点模板中)，并且如果无法识别的表名称传递给方法 （第二个参数），该方法不会引发错误。 相反，它会自动创建表。
> 
> 这可能会造成问题，如果你想要使用的成员身份的自定义用户表，但传递到的错误的表名称`WebSecurity.InitializeDatabaseConnection`方法。 因为该方法不会默认情况下引发错误如果您指定的表不存在，而且它改为创建一个新表，则应用程序可以似乎无法正常工作。 但是，依赖于自定义用户表 （和在其中的字段） 的应用程序代码可能最终失败，意外错误。
> 
> **Workaround**  
> 请确保名称传递中`InitializeDatabaseConnection`方法匹配项的用户配置文件表中成员资格数据库中，或确保`autoCreateTables`参数设置为 false。


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>问题:"无法生成 SQL Server 的用户实例"错误

> 如果 WebMatrix Web 应用程序使用 SQL Server Express 和 Windows 7 或 Windows Server 2008 R2 上运行 IIS 7.5，可能会看到一个错误，指示 SQL Server 无法在运行时检索用户的本地应用程序路径。
> 
> **解决方法**请确保应用程序 （通常网络服务） 下运行的 Windows 帐户如具有读/写权限的应用程序的根文件夹和子文件夹*应用\_数据*. 知识库文章中提供了更多详细的信息[SQL Server Express 用户实例化和 ASP.net Web 应用程序项目问题](https://support.microsoft.com/kb/2002980)。


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>问题： 在 Visual Studio 中，自定义程序集 (Dll) 的命名空间不自动导入

> 如果你在 Visual Studio 中的项目中使用自定义程序集，这些程序集中声明的命名空间不自动导入在设计时。 因此，对自定义类型的引用可能无法识别设计时和标记为无法识别中的 Visual Studio （使用"波形曲线"）。 仅在 Visual Studio; 中的设计时出现此问题应用程序本身能够正常运行。
> 
> **Workaround**  
> 包括`using`语句 (`imports`在 Visual Basic 中) 引用在设计时无法识别的实体。


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>问题： Visual Studio IntelliSense 和项目模板仅在 ASP.NET MVC 3 版本中可用

> 安装 ASP.NET 网页不还安装 tools for Visual Studio 的 ASP.NET Web Pages 应用程序的智能感知和项目模板等。
> 
> **解决方法**若要使用 Visual Studio 中的 ASP.NET Web Pages 应用智能感知和项目模板，ASP.NET MVC 3 RC 通过 Web 平台安装程序的安装或[独立安装程序](https://go.microsoft.com/fwlink/?LinkID=191797)。


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>问题:"&lt;帮助器&gt;找不到类"错误

> 你升级到 Beta 3 后，你可能会看到错误的帮助器类 (例如，`Facebook`类) 不能找不到。 在 Beta 2 开始，一直在 Beta 3 中，帮助器均已移至显式必须安装的包。 现有站点将不升级，以便包括这些包;这包括在站点*\My Documents\IISExpress*或*\My Documents\My 网站*文件夹。 具体而言，你将看到此错误，如果使用中的默认站点*我的网站*(WebSite1)，其中包括对引用`Twitter`帮助器。
> 
> **Workaround**  
> 注释掉对任何帮助器中运行的站点的调用*\_管理员*页上，并安装包或包中包括你想要使用的帮助器。 安装包后，你可以取消引用帮助器行的注释。


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>问题： 将 Beta 3 ASP.NET Razor 程序集部署到 Bin 文件夹可能不适用于托管网站

> 如果将 ASP.NET Web Pages 网站部署到托管站点，并且将 ASP.NET Razor Beta 3 程序集部署到站点的*Bin*文件夹中，你可能会遇到错误，包括以下：
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> 如果托管提供商已将 ASP.NET Web Pages Beta 1 程序集安装到服务器的全局应用程序缓存 (GAC)，也可能发生。 GAC 中的程序集通过程序集安装在本地获取优先*Bin*文件夹。
> 
> **解决方法**联系托管提供商来确认你看到的错误是因为提供程序的版本之间发生冲突的程序集和你的帐户。 如果是这样，请求托管提供商更新服务器的 GAC 中的程序集。


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>问题： 读取源或其他通过代理服务器的外部数据

> 如果运行站点的服务器位于代理服务器后面，你可能需要配置中的代理信息*Web.config*为了能够读取来自你的站点之外的信息的文件。 例如，如果你使用`ReCaptcha`帮助器，帮助器与 reCAPTCHA 服务通信，但可能会阻止由你的代理服务器。 同样，使用在 ASP.NET Web 页中，如源包管理器使用的源可能需要代理配置。
> 
> 如果你遇到中使用的外部服务或使用源的包的问题，将以下元素放入应用程序的根*Web.config*文件：
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> 有关配置代理服务器的详细信息，请参阅[&lt;代理&gt;元素 （网络设置）](https://msdn.microsoft.com/library/sa91de1e.aspx) MSDN 网站上。


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>问题:"无法加载 Microsoft.Web.Infrastructure.dll"错误

> 如果您以前使用 Razor 语法安装 Beta 1 版本的 ASP.NET 网页，然后安装 Beta 3 版本，将所有适当的程序集安装在 GAC 中，除*Microsoft.Web.Infrastructure.dll*。 因此，当你运行 ASP.NET Razor 页，你看到错误，该值指示*Microsoft.Web.Infrastructure.dll*无法加载。
> 
> 如果你加载 Beta 3 版本上的干净计算机，则此问题不会发生。
> 
> **Workaround**  
> 在 Control Panel 中，卸载 ASP.NET 网页。 然后重新安装 Beta 3 版本。


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>问题： 卸载.NET Framework 版本 4 禁用带有 Razor 语法的 ASP.NET Web Pages

> 如果你卸载.NET Framework 版本 4，然后重新安装它，将禁用使用 Razor 语法的 ASP.NET 网页。 与页*.cshtml*扩展可能无法正常运行。 ASP.NET Web Pages 机根目录中注册程序集*Web.config*文件，并删除.NET Framework 中删除该文件。 重新安装.NET Framework 安装新版本的配置文件中，但不会添加 ASP.NET 网页的程序集引用。
> 
> **解决方法**后重新安装.NET Framework，请重新安装 ASP.NET 网页使用 Razor 语法。 这将添加到下面的元素*Web.config*机根目录，这通常是在以下位置中的文件：  
>   
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
>   
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>问题： 使用 ASP.NET 的 Bin 文件夹中的程序集以前部署的应用程序遇到错误

> 在部署期间，ASP.NET 网页程序集的副本 (例如， *Microsoft.WebPages.dll*) 到*Bin*的服务器上的网站的文件夹。 (这可能是自动在部署过程或由于开发人员显式复制程序集。)但是，当安装 Beta 3 版本时，错误发生，如找不到某些类型的错误。 这是因为大量的 ASP.NET Web Pages 类型已移动到不同的命名空间对于 Beta 3 版本。
> 
> **解决方法**   
> 清除*Bin*文件夹部署的应用程序，将新的程序集复制到文件夹 （或重新部署应用程序），然后重新启动应用程序。


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
> - 如果您没有对服务器计算机的控制 （例如，您要部署到托管网站），将以下代码添加到网站的*Web.config*文件：
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>问题： 在同一应用程序中使用 Web 应用程序项目或 ASP.NET MVC 和 ASP.NET Web 页

> 如果你在 Web 应用程序项目或 ASP.NET MVC 应用程序中使用 ASP.NET 网页，你可能会看到错误， *WebPageHttpApplication*找不到。
> 
> **Workaround**  
> 如果收到此错误，将更改应用程序所派生的基类。 在*Global.asax*文件中，更改以下行：
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> 更改为：
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> 这实际上将反转引入的更改使用 Razor 语法的 Beta 1 发行版的 ASP.NET Web 页。


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>问题： 应用程序部署到不具有 SQL Server Compact 安装的计算机

> 其中 SQL Server Compact 未安装的计算机上可以运行的应用程序包括 SQL Server Compact 数据库。 Microsoft WebMatrix Beta 3 自动为你复制这些二进制文件，并执行相应*Web.config*文件转换。
> 
> **解决方法**如果你需要将这些文件复制并使*Web.config*文件更改手动，执行以下操作：
> 
> 1. 将复制到的数据库引擎程序集*Bin*文件夹 （和子文件夹） 的目标计算机上的应用程序： 
> 
>     - Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*
>     - 复制*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **到** *\Bin\x86*
>     - 复制*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **到** *\Bin\amd64*
> 2. 在网站的根文件夹中创建或打开*Web.config*文件。 (在 WebMatrix Beta 3 中，此文件类型是如果你单击可用**所有**中**选择文件类型**对话框。)
> 3. 添加以下元素的子级**&lt;配置&gt;**元素 (而不是在 **&lt;system.web&gt;** 元素):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>问题： 数据库和 WebGrid 帮助器中不起作用中等信任在 Visual Basic 中

> 如果你使用的 Visual Basic (创建*.vbhtml*文件)，则`Database`和`WebGrid`如果应用程序设置为使用中等信任帮助器将无法工作。
> 
> **Workaround**  
> 暂时将设置用于完全信任的应用程序。

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>问题： 无法识别"加密"属性

> SQL Server Compact 4.0 无法识别`Encrypt`属性`SqlCeConnection`类。 不应使用此属性来加密数据库文件。 `Encrypt`属性在 SQL Server Compact 3.5 版本中已弃用，并且仅为向后兼容性保留了。 
> 
> **Workaround**  
> 使用`Encryption Mode`属性`SqlCeConnection`类用于 SQL Server Compact 4.0 数据库文件进行加密。 下面的示例演示如何创建加密的 SQL Server Compact 4.0 数据库使用`Encryption Mode`属性：
>  
> [!code-csharp[Main](beta3/samples/sample11.cs)]
>  
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> 若要更改现有的 SQL Server Compact 4.0 数据库的加密模式，请执行以下操作：
>  
> [!code-csharp[Main](beta3/samples/sample13.cs)]
>  
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> 要加密未加密的 SQL Server Compact 4.0 数据库，请执行以下操作：
>  
> [!code-csharp[Main](beta3/samples/sample15.cs)]
>  
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Microsoft Visual c + + 2008年运行时库所需的问题：

> 本机 Dll 的 SQL Server Compact 4.0 需要 Microsoft Visual c + + 2008年运行时库 （x86、 IA64 和 x64）、 Service Pack 1。
> 
> **Workaround**  
> 安装.NET Framework 3.5 SP1。 这也将安装 Visual c + + 2008年运行时库 SP1。 你可以从以下位置下载这些库：   
>   
> [Microsoft Visual c + + 2008 Service Pack 1 可再发行组件包 ATL 安全更新](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> 请注意，安装.NET Framework 2.0，3.0，或 4 不*不*安装 Visual c + + 2008年运行时库 SP1。


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>问题： 如果 SQL Server Compact 安装的计算机上安装.NET Framework 之前，其提供程序固定名称未注册.NET Framework machine.config 文件中

> SQL Server Compact 可以安装在没有安装，因为 SQL Server Compact 确实需要.NET framework 的.NET Framework 的计算机上。 如果安装了.NET Framework 版本 3.5 也 4 都不必在安装 SQL Server Compact 之前，SQL Server Compact 安装程序未注册中的其提供程序固定名称*machine.config*文件。 任何应用程序依赖于中的 SQL Server Compact 条目*machine.config*文件将失败。 中的固定名称注册条目*machine.config*如以下示例所示：
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Workaround**  
> 卸载 SQL Server Compact 4.0 CTP1。 下载并安装.NET Framework 的完整版本，从以下位置：
> 
> [Microsoft.NET Framework 3.5 Service pack 1 （完全包）](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Microsoft.NET Framework 4.0 版本 （完整包）](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> 然后重新安装[SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en)。


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>安装应用程序

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>问题： 安装应用程序可能需要长时间如果用户的 My Documents 文件夹重定向到网络共享

> **Workaround**  
> 无。 应用程序可能需要一段时间才能安装，但将正确安装。


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>发布应用程序

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


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>其他问题

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>问题： 搜索/筛选器不适合在报表中分组依据： 问题类型

> 当你运行的报表可用于站点时，如果输入中的文本*url 的筛选器*框中，单击*搜索*，会执行任何操作。 这是因为此控件不起作用时*Group By*报告的状态设置为*问题类型*，这是默认设置。
> 
> **解决方法**中*Group By*选项卡的功能区中，单击*URL*进行分组由其源 URL 的条目。 文本框和按钮用于筛选条目都能运行时处于此状态。


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>问题： WCF 应用程序无法运行使用 IIS Express

> 浏览到 WCF 应用程序会导致类似以下错误：
> 
> *无法加载文件或程序集 Microsoft.Web.Administration，Version = 7.0.0.0，区域性 = neutral，PublicKeyToken = 31bf3856ad364e35 或其依赖项之一。系统找不到指定的文件。*
> 
> 因为默认情况下，IIS Express Beta 版本不支持 WCF，将发生这种情况。
> 
> **解决方法**使用下列解决方法之一 (解决方法 #2 要求 Microsoft Windows Vista 或更高版本):
> 
> 
> 1. 复制*Microsoft.Web.dll*和*Microsoft.Web.Administration.dll*到 WebMatrix 安装位置中的程序集*bin* WCF 目录应用程序。 默认情况下，在安装 WebMatrix *Microsoft WebMatrix*的子文件夹下系统的*Program Files*文件夹。
> 2. 在 Microsoft Windows Vista 或更高版本，创建指向中的程序集的*bin*使用以下命令的目录。 （这种方法的优点它不创建程序集的副本。）
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. 安装在 GAC 中的两个程序集。 从提升的提示符，运行以下命令：
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>问题： WebMatrix Beta 3 不能执行某些任务需要提升权限

> WebMatrix Beta 3 不能执行某些任务，需要提升，例如在以下情况下安装其他组件：
> 
> - 在 Windows Vista 或 Windows 7，使用不具备管理员权限的帐户登录和用户帐户控制 (UAC) 处于禁用状态。
> - 您正在使用 Microsoft Windows XP 或 Microsoft Windows Server 2003。
> 
> **Workaround**  
> WebMatrix Beta 3 中的大多数任务不需要管理权限。 对于那些确实，你可以执行该操作作为管理员，或请按照下列步骤：
> 
> - 在 Windows Vista 或 Windows 7 上，启用 UAC。
> - 在 Windows XP 中，将用户添加到管理员安全组。


#### <a name="issue-site-from-web-gallery-is-disabled"></a>问题:"从 Web 库的站点"处于禁用状态

> **站点从 Web 库**如果未安装 Web 平台安装程序 3.0 选项被禁用。
> 
> **Workaround**  
> 安装[Microsoft Web 平台安装程序 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)。


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>问题： 在 Windows Server 2003，IIS Express 不会启动非管理用户

> 在 Windows Server 2003，当您启动页或启动 IIS Express，IIS Express 不启动。 对 Web 页会显示错误，指示非管理用户，启动应用程序。
> 
> **Workaround**  
> 启动 WebMatrix Beta 3 作为管理用户。 有关更多详细信息，请参阅以下知识库文章：  
>   
> [由非管理用户启动的应用程序无法侦听到在其应用程序运行在 Windows Vista、 Windows Server 2003 或 Windows XP 的计算机的 HTTP 流量。](https://support.microsoft.com/kb/939786)


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


#### <a name="issue-the-relationships-button-is-disabled"></a>问题:"关系"按钮处于禁用状态

> **关系**按钮下**表**选项卡中**数据库**为 SQL Server Compact 数据库禁用工作区。
> 
> **Workaround**  
> 无。 SQL Server Compact 不支持表之间的关系。


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>问题： 参数化的 SQL 查询引发异常

> 在 SQL Server Compact 4.0，如果不会如指定的数据类型`SqlDbType`或`DbType`中参数化查询的参数，运行查询时引发异常。
> 
> **Workaround**  
> 显式设置参数的数据类型，如`SqlDbType`或`DbType`。 这是在 BLOB 数据类型的情况下关键 (`image`和`ntext`)。 使用代码如下所示：
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
>  
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a>更多信息

有关 WebMatrix Beta 3 的详细信息，请参阅以下网站：

- [IIS.net](http://iis.net/)
- [ASP.NET 2.0](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

* * *

© 2010 Microsoft Corporation. 保留所有权利。 [使用条款](https://msdn.microsoft.cos/cc300389.aspx)。
