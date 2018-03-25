---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: "ASP.NET 和 Web 工具 2013.2 for Visual Studio 2013 发行说明 |Microsoft 文档"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2014
ms.topic: article
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 0e7ad52662f7ceaa1f087d007d0b14b610f90bee
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET 和 Web Tools 2013.2 for Visual Studio 2013 发行说明
====================
by [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>安装说明

ASP.NET 和 Web Tools for Visual Studio 2013.2 在主安装程序中捆绑的和可以作为的一部分下载[Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)。

## <a name="documentation"></a>文档

教程和 Visual Studio 2013.2 ASP.NET 和 Web 工具其他信息可以从[ASP.NET 网站](https://www.asp.net/)。

## <a name="software-requirements"></a>软件要求

ASP.NET 和 Web Tools for Visual Studio 2013.2 需要 Visual Studio 2013。

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>ASP.NET 和 Web Tools for Visual Studio 2013.2 中的新增功能

以下各节描述了已在版本中引入的功能。

- [一个 ASP.NET 项目模板](#oneaspnet)
- [支持 SSL，在启动上 IIS Express Web 应用程序](#ssl)
- [Visual Studio Web 编辑器增强功能](#vswebeditor)
- [浏览器链接](#browserlink)
- [对 Visual Studio 中的 Azure App Service Web 应用的支持](#waws)
- [在创建新的 Web 项目时创建远程 Azure 资源](#AzureResources)
- [Web 发布增强功能](#webpublish)
- [ASP.NET 基架](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [ASP.NET Web 窗体](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.NET Web API 2.1.2](#webapi)
- [ASP.NET Web Pages 3.1.2](#webpages)
- [实体框架 6.1](#ef)
- [ASP.NET 标识 2.0.0](#identity)
- [Microsoft OWIN 组件](#owin)
- [ASP.NET SignalR 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>一个 ASP.NET 项目模板

- 对 ASP.NET 项目模板，以支持帐户确认和密码重置的更新。
- 更新 ASP.NET Web API 模板以支持使用在本地组织帐户身份验证。
- ASP.NET SPA 模板现在包含基于 MVC 和服务器端视图的身份验证。 该模板具有 WebAPI 控制器仅可通过身份验证的用户访问。

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>支持 SSL，在启动上 IIS Express Web 应用程序

若要消除安全警告浏览和调试在本地主机上的 HTTPS 时，我们添加了一个对话框，以允许 Internet Explorer 和 Chrome 信任自签名的 IIS express SSL 证书。

例如，web 项目属性可以设置为使用 SSL。 单击 F4 以显示属性对话框。 更改**启用 SSL**为 true。 复制 SSL URL。

![启用 SSL 的属性](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

基于 URL 集 web 项目属性页 web 选项卡为使用 HTTPS (SSL URL 将为`https://localhost:44300/`除非你之前已创建 SSL 网站。)

![设置项目 URL (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

按 Ctrl+F5 运行应用程序。 按照说明信任 IIS Express 生成的自签名的证书。

![SSL Warning](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

读取**安全警告**对话框，然后单击**是**如果你想要安装表示本地主机的证书。

![安全警告](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

在 IE 或 Chrome 中将显示站点而不在浏览器中的证书发出警告。

![HTTPS 页没有收到警告](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox 会使用其自己的证书存储，因此它将显示一条警告。

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Visual Studio Web 编辑器增强功能

- **新的 JSON 报表项和编辑器**： 我们已添加 JSON 项目项和编辑器对 Visual Studio。 当前的 JSON 编辑器功能包括着色、 语法验证、 括号补全、 大纲显示、 工具选项设置和的详细信息。

    ![JSON 编辑器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense 现在支持[JSON 架构](http://json-schema.org/)v3 和 v4。 没有架构组合框，以选择现有的架构，编辑本地架构路径，或只需拖放到它，以获取相对路径项目 JSON 文件。

    ![JSON Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![JSON 架构编辑器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **新 Sass (SCSS) 编辑器**： 我们添加小于 VS2013 RTM，且我们现在具有 Sass 项目项和编辑器。 特性可以与的 LESS 编辑器，并包括着色、 变量和 Mixins IntelliSense，取消注释注释 /、 快速信息、 格式设置、 语法验证、 大纲显示、 转到定义、 颜色选取器 sass 编辑器工具选项等设置。

    ![添加新项： SCSS 样式表](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![样式表编辑器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **HTML Razor，在 CSS 中，更少的新 URL 选取器和 Sass 文档：** VS 2013 随附在 Web 窗体页面之外没有 URL 选取器。 HTML Razor，css，更少的新 URL 选取器和 Sass 编辑器是无对话框的、 fluent 键入选取器理解.. 和筛选器文件列出相应的 img 标记和链接。

    ![图像标记的的 URL 选取器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![视图的 URL 选取器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![针对 CSS URL 选取器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **通过添加更多的功能的 LESS 编辑器的更新**
- **Knockout Intellisense 升级**： 我们添加 VS intelliSense 的非标准 KnockOut 语法"ko vs 编辑器 viewModel:"语法。 它可以用于绑定到窗体中使用注释页面上的多个视图模型：

    ![Knockout Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    此外，我们增加了对嵌套的 ViewModel IntelliSense 支持，以便您可以深化到视图模型上的深度嵌套对象。

    `<div data-bind="text: foo.bar.baz.etc" />`

    显示 IntelilSense 是完整的 IntelliSense 的 JavaScript 对象。

    ![Intellisense 显示完整的 JavaScript 对象](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **HTML Razor，在 CSS 中，更少的新 URL 选取器和 Sass 文档**: VS 2013 随附在 Web 窗体页面之外没有 URL 选取器。 HTML Razor，css，更少的新 URL 选取器和 Sass 编辑器是无对话框的、 fluent 键入选取器理解.. 和筛选器文件列出相应的 img 标记和链接。

    ![图像标记的的 URL 选取器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![视图的 URL 选取器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![针对 CSS URL 选取器](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>浏览器链接

- Browser Link 现在支持 HTTPS 连接，将列出，在仪表板与其他连接，只要受浏览器信任的证书。
- 静态 HTML 源映射
- SPA 支持映射数据
- 自动更新映射数据

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>对 Visual Studio 中的 Azure App Service Web 应用的支持

- **支持 Azure 登录。**
- **远程调试和 web 应用的远程视图**： 我们现在支持[为在 Azure App Service web apps 远程调试](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)和远程视图在服务器资源管理器中的 web 应用内容文件，以及。

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>在创建新的 Web 项目时创建远程 Azure 资源

我们添加 Azure ["创建远程资源"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)新的 web 应用程序对话框上的复选框。 通过选择它，你将能够将创建一个新的 web 应用程序，设置测试，并在几个简单步骤中创建发布配置文件的 Azure 发布站点的体验集成。

![使用 Azure 资源的新项目](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![发布到 Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Web 发布增强功能

- 改善用户体验发布。

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET 基架

- **枚举支持：**如果您的模型正在使用的枚举，则 MVC 基架将为枚举生成下拉列表。 这将在 MVC 中使用的枚举帮助器。
- **启动支持**： 更新中 MVC 基架的 EditorFor 模板，以便他们使用的启动类。
- **包支持**: MVC 和 Web API Scaffolders 将 MVC 和 Web API 添加 5.1 包

以下屏幕截图演示基架模型。

- 模型代码：

     ![模型代码](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- 编译模型代码，右键单击，并选择**添加**，**新建基架项**。

     ![添加新的基架的项](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- 选择**MVC5 控制器的包含视图使用 Entity Framework**:

     ![添加新 MVC5 控制器与视图](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **添加控制器**使用模型：

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- 检查生成的代码，例如 Views/WeekdayModels/Edit.cshtml 包含`@Html.EnumDropDownListFor`:![查看包含 EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- 运行页面，请参阅枚举组合框生成，请注意，是否值可以为 null，空字符串可以选择的组合框。 例如，**创建**页显示以下：

    ![允许空字符串的组合框](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 RTM 将在 2014 年 4 月发布。 以下是从发行说明中，突出的点，但请检查[完整的发行说明](http://docs.nuget.org/docs/release-notes/nuget-2.8)有关这些更改的详细信息。

- **面向 Windows Phone 8.1 应用程序**: NuGet 2.8.1 现在支持针对 Windows Phone 8.1 应用程序使用目标框架名字对象 WindowsPhoneApp、 WPA、 WindowsPhoneApp81 和 WPA81。
- **依赖项的修补程序解析**: NuGet 包的依赖项时，已从历史上看实现选择满足对包的依赖项的最低主版本号和次包版本的策略。 与不同的主版本号和次版本，但是，修补程序版本是始终解析为最高的版本。 尽管该行为是善意，它创建缺乏确定性包安装具有依赖项。
- **DependencyVersion 交换机**： 尽管 NuGet 2.8 更改*默认*解析的依赖关系的行为，它还添加了更精确地控制通过中的-DependencyVersion 交换机的依赖项解析过程包管理器控制台。 开关可以启用对可能的最低版本 （默认行为）、 可能的最高版本，或最高的次要或修补程序版本的解决依赖关系。 此交换机仅适用于 powershell 命令中的安装包。
- **DependencyVersion 属性**： 除了上面详细说明-DependencyVersion 交换机，NuGet 还允许在定义的默认值什么，如果-DependencyVersion 切换 nuget.config 文件中设置新的属性的功能安装包的调用中未指定。 NuGet 包管理器对话框中的任何安装包操作还会遵循此值。 若要设置此值，请将以下属性添加到 nuget.config 文件：

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **预览-whatif NuGet 的操作与**： 某些 NuGet 包可以深的依赖项关系图，并且在这种情况下，它可以在安装过程中会有所帮助、 卸载或更新操作，以首先查看将发生什么情况。 NuGet 2.8 添加标准 PowerShell-如果切换到启用可视化的命令将应用到的包的整个闭包的安装包、 卸载包和更新包命令。
- **降级包**： 并不少见安装包的预发布版本，以便调查新功能，然后决定要回滚到最后一个稳定版本。 在 NuGet 2.8 之前，这是一个多步骤过程以及卸载预发布的包和其依赖项，然后安装较早版本。 使用 NuGet 2.8，但是，更新包将现在回滚整个包闭包 （例如包的依赖关系树） 到以前的版本。
- **开发依赖关系**： 可以为 NuGet 包-包括用于优化开发过程的工具提供多种不同类型的功能。 这些组件，它们可有助于开发新的包，而不应该将更高版本时，新的包的依赖项发布。 NuGet 2.8 使包能够在.nuspec 文件中以 developmentDependency 标识自身。 安装时，此元数据将也添加到已在其中安装了包的项目的 packages.config 文件。 更高版本该 packages.config 文件分析为 NuGet 依赖关系在 nuget.exe 包过程中，它将排除这些依赖项标记为开发依赖关系。
- **针对不同平台的单独 packages.config 文件**： 时开发针对多个目标平台的应用程序，它很常见各自生成环境的每个不同的项目文件。 此外就常见使用不同的项目文件中的不同 NuGet 程序包的程序包具有不同级别的不同平台的支持。 NuGet 2.8 支持改进了这种情况下通过创建不同的特定于平台的项目文件的不同 packages.config 文件。
- **回退到本地缓存**： 尽管 NuGet 包会通常使用从远程库如[NuGet 库](http://www.nuget.org)使用网络连接，有许多未连接客户端的情况。 无网络连接，NuGet 客户端无法成功安装包-，即使这些包已在本地的 NuGet 缓存中的客户端的计算机上。 NuGet 2.8 将自动缓存回退添加到包管理器控制台。

    缓存回退功能不需要任何特定的命令参数。 此外，仅在包管理器控制台当前有效缓存回退-行为目前不能在包管理器对话框。
- **Bug 修复**： 主要所做的 bug 修复之一就是更新包中的性能改善-重新安装命令。

    除了这些功能和前面提到的性能修复程序，此版本的 NuGet 还包括许多其他 bug 修复。 出现了 181 总问题在版本中解决。 有关完整列表的工作项中修复 NuGet 2.8 请视图[NuGet 问题跟踪程序](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all)对于此版本。

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>ASP.NET Web 窗体

- Web 窗体模板现在显示如何对 ASP.NET Identity 中执行操作帐户确认和密码重置。
- 实体数据源控件和用于 Entity Framework 6 的动态数据提供程序。 有关更多详细信息，请参阅下面的 MSDN 博客：[动态数据提供程序和 Entity Framework 6 的 EntityDataSource 控件](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx)。

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [属性路由改进](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Bootstrap 支持编辑器模板](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [在视图中的枚举支持](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Unobstrusive 支持 MinLength / MaxLength 属性](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [支持在非介入式 Ajax 中的 this 上下文](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- 各种[bug 修复](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web API 2.1.2

- [全局错误处理](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [属性路由的增强功能](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [帮助页的改进](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [IgnoreRoute 支持](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [BSON 媒体类型格式化程序](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [更好地支持异步筛选器](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [查询分析格式库客户端](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- 各种[bug 修复](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>ASP.NET Web Pages 3.1.2

- 各种[bug 修复](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>实体框架 6.1

实体框架已更新为版本 6.1 运行时和工具。 Entity Framework (EF) 6.1 到 Entity Framework 6 的一项次要更新，并包含大量的 bug 修复和新功能。 有关详细信息 EF6.1，包括指向文档的新功能，请参阅[实体框架版本历史记录](https://msdn.microsoft.com/data/jj574253)。 此版本中的新功能包括：

- **工具合并**提供一致的方法来创建新的 EF 模型。 此功能扩展 ADO.NET 实体数据模型向导，以支持创建 Code First 模型，包括从现有数据库反向工程。 这些功能都在 EF Power Tools Beta 质量以前不可用。
- **处理的事务提交失败**提供新[System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx)这样使用新引入的功能截获事务操作。 **CommitFailureHandler**允许从连接故障自动恢复，同时提交事务。
- **IndexAttribute**允许要置于 Code First 模型的属性 （或属性） 的某个属性指定的索引。 代码首先将然后创建相应的索引在数据库中。
- **公共映射 API**提供对 EF 对如何属性和类型映射到列和表在数据库中的信息的访问。 在以前的版本此 API 是内部。
- **能够配置通过 App/Web.config 文件的拦截器**（允许拦截器添加无需重新编译应用程序）。
- **DatabaseLogger**是可以轻松地记录到文件的所有数据库操作新侦听器。 在与以前的功能结合使用，这将允许你轻松地切换的部署的应用程序，而无需重新编译的数据库操作的日志记录。
- **迁移模型更改检测**得到了改进，以便更准确; 的更改检测过程的性能也大大增强基架的迁移。
- **性能改进**在初始化期间降低的数据库操作，包括在 LINQ 查询中，null 相等比较的优化更快地查看生成 （模型创建） 中更多方案，且更为高效具有多个关联的被跟踪实体的具体化。

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET 标识 2.0.0

- **双因素身份验证**: ASP.NET 标识现在支持双因素身份验证。 双因素身份验证提供了一层额外的你在其中遭到入侵你的密码的情况下的用户帐户的安全。 此外，还有针对两个因素代码遭到暴力破解攻击的保护。
- **帐户锁定：**提供一种方式锁定用户，如果用户输入其密码或双因素代码不正确。 无效尝试次数和时间跨度的用户被锁定可以配置。 开发人员可以根据需要关闭帐户锁定为特定用户帐户应在需要。
- **帐户确认：** ASP.NET 标识系统现在支持帐户确认。 这是相当常见方案，在今天大多数网站中，当你注册网站上的新帐户，你需要以确认你的电子邮件之前您可以执行的网站中的任何操作。 电子邮件确认很有用，因为它可防止虚假帐户创建。 这是网站的非常有用，如果你使用的电子邮件作为与你论坛网站、 银行、 电子商务或社交网站等的用户进行通信的方法。
- **密码重置：**密码重置是一项功能，用户才能重置密码，如果用户忘记其密码。
- **安全戳 （注销无处不在）：**支持一种方式，以重新生成安全令牌的用户在情况下，当用户更改其密码或任何其他安全相关的删除关联的登录名 （如 Facebook、 Google、 等的信息Microsoft 帐户和等等）。 需要这些信息来确保使用旧密码生成任何标记将会失效。 在示例项目中，如果你更改用户的密码然后为用户生成新的令牌和任何以前的令牌失效。 此功能提供一层额外的安全到你的应用程序，因为当你更改你的密码，你将被注销从 everywhere （所有其他浏览器），您已登录到此应用程序。
- **创建主键的用户和角色可扩展该类型**： 在 ASP.NET 标识 1.0，主键表用户和角色的类型为字符串。 这意味着通过使用实体框架中的 ASP.NET 标识系统已保留 SQL Server 中时，我们已使用 nvarchar。 没有堆栈溢出此默认实现许多主题讨论并基于传入的反馈。 我们提供了扩展性挂钩，你可以在其中指定了应用户和角色表的主键。 扩展性挂钩，此方法特别有用，如果你要迁移你的应用程序和应用程序已存储的 Userid 是否 Guid 或整数。
- **对用户和角色支持 IQueryable**： 添加了对 UsersStore 和 RolesStore IQueryable 的支持，可以轻松地获取的用户和角色的列表。
- **支持通过 UserManager 的删除操作**
- **索引的用户名**： 在 ASP.NET 标识实体框架实现中，我们已添加唯一索引的用户名在 EF 6.1.0 中使用新的 IndexAttribute。 这可确保用户名始终是唯一的而且没有任何争用条件，在其中你最终可能会得到重复的用户名。
- **增强的密码验证程序：**出厂在 ASP.NET 标识 1.0 中的密码验证程序已仅已验证的最小长度非常基本的密码验证程序。 没有新的密码验证程序，可让你更好地控制密码的复杂性。 请注意，即使你打开此密码中的所有设置，我们建议你在启用双因素身份验证的用户帐户。
- **IdentityFactory 中间件 / CreatePerOwinContext**:

    - **用户管理器**： 你可以使用工厂实现从 OWIN 上下文中获取的 UserManager 实例。 此模式为类似于我们为获取 AuthenticationManager OWIN 上下文中登录和注销的使用。 这是推荐的方法来获取每个应用程序的请求的 UserManager 实例。
    - **DbContextFactory**: ASP.NET 标识使用 Entity Framework 进行持久保存在 SQL Server 中的标识系统。 若要这样做标识系统具有对 ApplicationDbContext 的引用。 DbContextFactory 中间件返回每个请求可以在你的应用程序中使用 ApplicationDbContext 的实例。
- **ASP.NET 标识示例 NuGet 包**: 示例 NuGet 包可以更加轻松地安装和运行示例 ASP.NET 标识并遵循的最佳做法。 这是一个示例 ASP.NET MVC 应用程序。 请修改以满足你的应用程序，你将它部署在生产环境中之前的代码。 此示例应安装在一个空的 ASP.NET 应用程序。 有关包的详细信息，请转到以下博客帖子：[宣布 RTM 的 ASP.NET 标识 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Microsoft OWIN 组件

没有大量在此版本中修复的 bug。 请参阅[2.1.0 的发行说明版本](https://katanaproject.codeplex.com/releases/view/113281)更多详细信息。

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

没有大量在此版本中修复的 bug。 请参阅[2.0.2 的发行说明版本](https://github.com/SignalR/SignalR/releases/tag/2.0.2)更多详细信息。
