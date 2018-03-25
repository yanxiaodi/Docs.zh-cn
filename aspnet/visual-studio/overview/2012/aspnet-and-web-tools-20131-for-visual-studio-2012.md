---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: "ASP.NET 和 Web Tools 2013.1 for Visual Studio 2012 发行说明 |Microsoft 文档"
author: microsoft
description: "本文档介绍 ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012 的版本。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: c11e2ef9c33b0cae1f196690533094ce1c342da5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>ASP.NET 和 Web Tools 2013.1 for Visual Studio 2012 的发行说明
====================
by [Microsoft](https://github.com/microsoft)

> 本文档介绍 ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012 的版本。


## <a name="contents"></a>内容

- [安装说明](#install)
- [软件要求](#requirements)
- ASP.NET 和 Web Tools 2013.1 for Visual Studio 2012 中的新增功能

    - [Bootstrap](#bootstrap)
    - [模板](#templates)

        - [ASP.NET MVC 5 模板](#mvc5template)
        - [ASP.NET Web API 2 模板](#apitemplate)
        - [项模板](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [ASP.NET 基架](#scaffold)
    - [Razor 编辑器](#razor)
    - [NuGet 2.7](#nuget)
- 已知的问题和重大更改

    - [ASP.NET 基架](#issuescaffolding)

        - [MVC 和 Web API 基架-HTTP 404 找不到错误](#404issue)
        - [Visual Studio Express 2012 for Web 添加基架的项后停止工作](#expressissue)
    - [ASP.NET Razor 3](#issuerazor)

        - [查看使用浏览或 F5 的 cshtml 文件会导致服务器错误](#browseissue)
        - [Url 重写和 Tilde(~)](#rewriteissue)
    - [模板](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>安装说明

[安装](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids)ASP.NET 和 Web Tools for Visual Studio 2012 2013.1。

<a id="requirements"></a>
## <a name="software-requirements"></a>软件要求

你必须具有 Visual Studio 2012 或 Visual Studio Express 2012 for Web。

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>ASP.NET 和 Web Tools 2013.1 for Visual Studio 2012 中的新增功能

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Bootstrap

当你创建的基架 MVC 5 控制器和视图时，使用视图的标记[Bootstrap](http://getbootstrap.com/)。

<a id="templates"></a>
### <a name="templates"></a>模板

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>ASP.NET MVC 5 模板

我们添加了新的 MVC 5 模板。 它所引用的最新的 MVC 5 NuGet 包，并且你可以使用基架添加控制器和视图。

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>ASP.NET Web API 2 模板

我们添加了新的 Web API 2 模板。 它引用最新的 Web API 2 NuGet 包，但你可以使用基架添加控制器和视图。

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>项模板

我们添加新项模板的 MVC 5 视图、 网页 (Razor 3) 和 Web API 2 控制器。 它们在添加新项时的相关的 NuGet 包安装到项目。

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

当你创建基架使用 Entity Framework 的 MVC 或 Web API 控制器时，我们将使用 Framework 6。 有关实体框架的详细信息，请参阅[实体框架版本历史记录](https://msdn.com/data/jj574253)。

你还可以下载和安装实体框架 6 Tools for Visual Studio 2012。 请参阅[获取实体框架](https://msdn.com/data/ee712906#tooling)。

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET 基架

ASP.NET 基架是用于 ASP.NET Web 应用程序的代码生成框架。 它可以轻松将样板文件代码添加到您的项目，与数据模型进行交互。

在以前版本的 Visual Studio 中，基架只限于 ASP.NET MVC 项目。 利用此更新，现在可以用于任何 ASP.NET 项目，包括 Web 窗体中使用基架。 此更新不支持 Web 窗体项目中，生成页，但你仍可以使用基架，Web 窗体通过向项目添加 MVC 依赖项。 将在将来更新中添加对生成的 Web 窗体页支持。

在使用基架，我们确保所有必需的项目中安装依赖关系。 例如，如果您首先 ASP.NET Web 窗体项目，然后使用基架添加一个 Web API 控制器，所需的 NuGet 程序包和引用将自动添加到你的项目。

若要将 MVC 基架添加到 Web 窗体项目，添加**新建基架项**和选择**MVC 5 依赖项**对话框窗口中。 有两个选项的基架 MVC;最小和完整。 如果你选择最小，只有的 NuGet 包和 ASP.NET MVC 的引用添加到项目中。 如果选择完整选项，添加最小依赖关系，以及对于 MVC 项目所需的内容文件。

对基架异步控制器支持使用 Entity Framework 6 中的新异步功能。

有关详细信息和教程，请参阅[ASP.NET 基架概述](../2013/aspnet-scaffolding-overview.md)。 这些教程介绍了基架使用 Visual Studio 2013 中，但它们也适用于 ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012。

<a id="razor"></a>
### <a name="razor-editor"></a>Razor 编辑器

利用此更新，Visual Studio 2012 现在支持 Razor 3 工具/编辑。

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 详细的介绍包含一组丰富的新的特征描述了这些[NuGet 2.7 发行说明](http://docs.nuget.org/docs/release-notes/nuget-2.7)。

此版本的 NuGet 中删除用户显式允许 NuGet 还原缺少的程序包的需要。 在安装 NuGet 2.7 时，用户隐式同意自动还原缺少的程序包。 用户可以显式选择退出通过 Visual Studio 中的 NuGet 设置包还原。 此更改简化了包还原的工作原理。

## <a name="known-issues-and-breaking-changes"></a>已知的问题和重大更改

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET 基架

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC 和 Web API 基架-HTTP 404 找不到错误

如果将基架的项添加到项目时遇到错误，，则可能你的项目将处于不一致状态。 某些将基架所做的更改将被回滚，但其他更改，例如已安装的 NuGet 程序包，将不会回滚。 如果回滚路由的配置更改，则用户将收到 HTTP 404 错误，当导航到基架项。

若要为 MVC 修复此错误，添加新的基架的项，然后选择 MVC 5 依赖项 （最小或完整）。 此过程将向你的项目添加所有所需的更改。

若要为 Web API 修复此错误：

1. 将下面的 WebApiConfig 类添加到你的项目。

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. 在应用程序中配置 WebApiConfig.Register\_，如下所示在 Global.asax 中启动方法：

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 for Web 添加基架的项后停止工作

如果 Visual Studio Express 2012 for Web 添加使用实体框架 （如 Web API 2 Controller 其操作使用 Entity Framework) 的基架的项后停止工作，则有可能，Visual Studio Express 无法加载程序集的本机映像依赖于 system.web.extensions 的引用。

若要更正此问题，请配置 Visual Studio Express 来使用 system.web.extensions 的引用的 MSIL 映像：

1. 在管理员模式下打开命令提示符。
2. 转到 %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE 或 %programfiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE （适用于 Windows 的 64 位）。
3. 在文本编辑器中打开 VWDExpress.exe.config。
4. 添加以下行下的&lt;配置&gt;/&lt;运行时&gt;元素：  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. 重新启动 Visual Studio Express 2012 for Web。

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a>查看服务器错误的 cshtml 文件 withBrowse WithorF5causes

当你在 Visual Studio 2012 （或在 Visual Studio 2013 中创建的 Visual Studio 2012 MVC 5 项目中打开） 中创建的 MVC 5 项目，并尝试通过使用浏览或 F5 查看 cshtml 文件时，你将收到错误，状态-**中的服务器错误'/' 应用程序**。 服务器尝试导航到`http://localhost:XXXX/Views/../XXXX.cshtml`

若要解决此问题，更改**启动操作**在您的项目中设置**特定页**。 不需要为页面提供一个值。

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

此更改后，选择 f5 键导航到你的应用程序的根目录 (`http://localhost:XXXX`)。 此行为不是在 Visual Studio 2013 中，MVC 5 项目的行为相同其中**当前页**设置启动打开页。

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Url 重写和 Tilde(~)

升级到 ASP.NET Razor 3 或 ASP.NET MVC 5 后，tilde(~) 表示法可能不再正常工作如果你使用的 URL 重写。 URL 重写会影响在 HTML 元素中的 tilde(~) 表示法，如&lt;A /&gt;，&lt;脚本 /&gt;，&lt;链接 /&gt;，并因此颚化符不再将映射到的根目录。

例如，如果你重写请求**asp.net/content**到**asp.net**中的 href 属性&lt;A href ="~/content/"/&gt;解析为**/content/内容 /**而不是 **/** 。 若要禁止显示此更改，你可以设置**IIS\_WasUrlRewritten**为 false 在每个 Web 页中或在上下文**应用程序\_BeginRequest** Global.asax 中。

<a id="templateissue"></a>
### <a name="templates"></a>模板

当你创建 ASP.NET MVC 项目与在 Windows 8.1 或 Windows Server 2012 R2，Visual Studio 的 Visual Studio 2012 显示错误消息，指出"配置 Web [url] 为 ASP.NET 4.5 失败。"

![配置错误](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

因为在这些版本的 Windows 上安装时，Visual Studio 2012 不会启用 ASP.NET 4.5 功能，你会看到此错误。 若要启用 ASP.NET 4.5，执行的步骤中所述[打开或关闭 Windows 功能](https://windows.microsoft.com/windows-8/turn-windows-features-on-off)。

![打开或关闭 Windows 功能](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

或者，你可以通过命令行启用 ASP.NET 4.5。

1. 在管理员模式下打开命令提示符。
2. 运行以下命令以启用 ASP.NET 4.5。  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
