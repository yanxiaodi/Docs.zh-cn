---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/06/2010
ms.topic: article
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: 058b7c6fe6eeeea4403e7e5fdc05bb44d2ba1300
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
- [概述](#overview)
- [安装说明](#installation-notes)
- [软件要求](#software-requirements)
- [文档](#documentation)
- [支持](#support)
- [将 ASP.NET MVC 2 项目升级到 ASP.NET MVC 3 Tools 更新](#upgrading)
- [ASP.NET MVC 3 Tools 更新 (2011 年 4 月 12 日)](#tu-changes)

    - ["添加控制器"对话框中可以现在搭建基架视图和数据访问代码的域控制器](#tu-AddControllerDialog)
    - [对改进"ASP.NET MVC 3 新项目"对话框](#tu-ImprovementsNewDialogBox)
    - [项目模板现在包括 Modernizr 1.7](#tu-Modernizr)
    - [项目模板包括的 jQuery、 jQuery UI 中和 jQuery 的更新的版本验证](#tu-UpdatedJQuery)
    - [项目模板现在包括 ADO.NET 实体框架 4.1 作为预安装 NuGet 程序包](#tu-EF)
    - [项目模板包括 JavaScript 库作为预安装 NuGet 包](#tu-JavaScriptLibsNuget)
    - [已知问题](#tu-KI)
- [ASP.NET MVC 3 RTM (2011 年 1 月 13 日)](#MVC3RTM)

    - [更改： 更新到 1.8.7 jQuery UI 的版本](#RTM-1)
    - [更改： 更改 ModelMetadataProvider 回 DataAnnotationsModelMetadataProvider 了默认值](#RTM-2)
    - [已修复： 粘贴 Razor 表达式包含空格导致它被反转的部分](#RTM-3)
    - [语法着色和 IntelliSense 重命名在编辑器中打开的 Razor 文件禁用已修复：](#RTM-4)
    - [已知问题](#RTM-KI)
    - [重大更改](#RTM-BC)
- [ASP.NET MVC 3 Release Candidate 2 （2010 年 12 月 10 日）](#_Toc2)

    - [项目模板更改，以便包括 jQuery 1.4.4、 jQuery 验证 1.7 和 jQuery UI 1.8.6y UI 1.8.6](#_Toc2_1)
    - [添加"AdditionalMetadataAttribute"类](#_Toc2_2)
    - [改进的视图基架](#_Toc2_3)
    - [添加的 Html.Raw 方法](#_Toc2_3)
    - [重命名"Controller.ViewModel"属性并将"视图"属性为"ViewBag"](#_Toc2_4)
    - [重命名"ControllerSessionStateAttribute"类"SessionStateAttribute"](#_Toc2_5)
    - [重命名的 RemoteAttribute"字段"属性设置为"AdditionalFields"](#_Toc2_6)
    - [重命名"SkipRequestValidationAttribute"到"AllowHtmlAttribute"](#_Toc2_7)
    - [更改"Html.ValidationMessage"方法，以显示第一条有用的错误消息](#_Toc2_8)
    - [固定@model声明，以向文档添加空格](#_Toc2_9)
    - [添加"FileExtensions"属性设置为视图引擎，以支持引擎特定文件的名称](#_Toc2_10)
    - [用于发出"For"属性的正确值的固定"LabelFor"帮助](#_Toc2_11)
    - [固定"RenderAction"方法，以便为模型绑定期间的显式值优先](#_Toc2_12)
    - [重大更改](#_Toc2_BC)
    - [已知问题](#_Toc2_KI)
- [ASP.NET MVC 3 候选发布版 (2010 年 11 月 9 日)](#TOC_ASP_NET_3_RC)

    - [ASP.NET MVC 3 RC 中的新增功能](#_Toc276711785)
    - [NuGet 包管理器](#_Toc276711786)
    - [改进了"新项目"对话框](#_Toc276711787)
    - [无会话控制器](#_Toc276711788)
    - [新的验证属性](#_Toc276711789)
    - [有关"LabelFor"和"LabelForModel"方法的新重载](#_Toc276711790)
    - [子操作输出缓存](#_Toc276711791)
    - ["添加视图"对话框框中改进](#_Toc276711792)
    - [精细请求验证](#_Toc276711793)
    - [重大更改](#_Toc276711794)
    - [已知问题](#_Toc276711795)
- [ASP。MVC 3 Beta 说明 (2010 年 10 月 6 日)](#TOC_ASP_NET_3_Beta)

    - [ASP.NET MVC 3 Beta 中的新增功能](#0.1__Toc274034215)
    - [NuPack 程序包管理器](#0.1__Toc274034216)
    - [改进的新项目对话框中](#0.1__Toc274034217)
    - [简化的方法来指定强类型化模型，在 Razor 视图中](#0.1__Toc274034218)
    - [支持新的 ASP.NET Web 页的帮助器方法](#0.1__Toc274034219)
    - [附加依赖项注入支持](#0.1__Toc274034220)
    - [对非介入式基于 jQuery 的 Ajax 的全新支持](#0.1__Toc274034221)
    - [有关非介入式 jQuery 验证新的支持](#0.1__Toc274034222)
    - [用于客户端验证和非介入式 JavaScript 的新应用程序范围内标志](#0.1__Toc274034223)
    - [对视图运行之前运行的代码的新支持](#0.1__Toc274034224)
    - [对 VBHTML Razor 语法的全新支持](#0.1__Toc274034225)
    - [更精细地控制 ValidateInputAttribute](#0.1__Toc274034226)
    - [帮助器将连字符下划线转换为使用匿名对象指定的 HTML 属性名称](#0.1__Toc274034227)
    - [Bug 修复](#0.1__Toc274034228)
    - [重大更改](#0.1__Toc274034229)
    - [已知问题](#0.1__Toc274034230)
- [Disclaimer](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>概述

本文档介绍 Visual Studio 2010 的 ASP.NET MVC 3 RTM 版。 ASP.NET MVC 是一个框架，用于开发 Web 应用程序使用模型-视图-控制器 (MVC) 模式。 ASP.NET MVC 3 安装程序包括以下组件：

- ASP.NET MVC 3 运行时组件
- ASP.NET MVC 3 Visual Studio 2010 tools
- ASP.NET 网页运行时组件
- ASP.NET 网页 Visual Studio 2010 tools
- Microsoft.net (NuGet) 的包管理器
- 支持的 Razor 语法的 Visual Studio 2010 的更新。 （有关详细信息，请参阅知识库文章 2483190。）

可以在以下 URL ASP.NET 网站上找到的每个预发行版本的 ASP.NET MVC 3 发行说明的完整集：

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>安装说明

若要安装 ASP.NET MVC 3 RTM 使用 Web 平台安装程序 (Web PI)，请访问以下页面：

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

或者，你可以从以下页面，for Visual Studio 2010 中下载的安装程序 ASP.NET MVC 3 RTM:

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 可以安装，并可以运行的并行与 ASP.NET MVC 2。

<a id="software-requirements"></a>
## <a name="software-requirements"></a>软件要求

ASP.NET MVC 3 的运行时组件需要以下软件：

- .NET framework 版本 4。 

    ASP.NET MVC 3 Visual Studio 2010 tools 需要以下软件：
- Visual Studio 2010 或 Visual Web Developer 2010 Express。

<a id="documentation"></a>
## <a name="documentation"></a>文档

在以下 URL MSDN 网站上提供了 ASP.NET MVC 的文档：

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

在以下 URL 的 ASP.NET Web 站点的 MVC 页上有教程和有关 ASP.NET MVC 的其他信息：

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>支持

这是完全受支持的版本。 有关获取技术支持的信息可以在找到[Microsoft 支持网站](https://support.microsoft.com/)。

此外可随意发布到 ASP.NET MVC 论坛，ASP.NET 社区的成员会经常能够提供非正式支持的有关此版本的问题：

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>将 ASP.NET MVC 2 项目升级到 ASP.NET MVC 3 Tools 更新

ASP.NET MVC 3 可以在同一台计算机，这将使您能够灵活地选择何时升级到 ASP.NET MVC 3 ASP.NET MVC 2 应用程序上的与 ASP.NET MVC 2 并行安装。

若要手动升级到版本 3 现有的 ASP.NET MVC 2 应用程序，请执行以下操作：

1. 在你的计算机上创建新的空 ASP.NET MVC 3 项目。 此项目将包含所需的升级某些文件。
2. 从 ASP.NET MVC 3 项目的以下文件复制到你的 ASP.NET MVC 2 项目的相应位置中。 你将需要更新对 jQuery 库来应对新的文件名 (jQuery 1.5.1.js) 的任何引用： 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*.js
    - /Content/themes/\*.\*
3. 复制*包*空的 ASP.NET MVC 3 项目解决方案到你的解决方案，它是解决方案的.sln 文件所在的目录中的根的根目录中的文件夹。
4. 如果你的 ASP.NET MVC 2 项目包含的任何区域，/Views/Web.config 将文件复制到*视图*的每个区域的文件夹。
5. 在这两个 ASP.NET MVC 2 项目中的 Web.config 文件，全局搜索和替换的 ASP.NET MVC 版本。 找到如下信息： 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    使用以下标记替换它：

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. 在解决方案资源管理器，删除对引用*System.Web.Mvc* （哪些点对该 DLL 从版本 2），然后添加对的引用*System.Web.Mvc* (v3.0.0.0)。
7. 添加对 System.Web.WebPages.dll 和 System.Web.Helpers.dll 的引用。 这些程序集位于以下文件夹： 

    - %ProgramFiles%\Microsoft ASP.NET\ASP.NET MVC 3\Assemblies
    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies
8. 在解决方案资源管理器，右键单击项目名称并选择卸载项目。 再次右键单击项目名称，然后选择编辑*ProjectName*.csproj。
9. 找到*ProjectTypeGuids*元素，并替换 {F85E285D-A4E0-4152-9332-AB1D724D3325} 为 {E53F8FEA-EAE0-44A6-8774-FFD645390401}。
10. 保存所做的更改，右键单击该项目，然后选择重新加载项目。
11. 在应用程序的根 Web.config 文件中，添加以下设置到*程序集*部分。 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. 如果该项目引用使用 ASP.NET MVC 2 编译任何第三方库，添加以下突出显示*bindingRedirect*元素下的应用程序根目录中的 Web.config 文件*配置*部分： 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>更改在 ASP.NET MVC 3 Tools 更新

本部分介绍了 ASP.NET MVC 3 RTM 发布后，ASP.NET MVC 3 Tools 更新版本中所做的更改。

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>"添加控制器"对话框中可以现在搭建基架视图和数据访问代码的域控制器

基架是一种快速生成控制器和你的应用程序的视图。 生成的代码后，你可以编辑它以满足你的项目的要求。

若要启动*添加控制器*对话框在 ASP.NET MVC 3，右键单击*控制器*文件夹中的*解决方案资源管理器*，单击*添加*，然后单击*控制器*。 对话框中已得到增强，以便提供更多基架选项。

![](mvc3-release-notes/_static/image1.png)

有三个基架模板默认情况下。

#### <a name="empty-controller"></a>空控制器

此模板生成的空控制器文件。 此模板是等效于不检查*添加操作创建、 编辑、 详细信息，请删除方案*在以前版本的 ASP.NET MVC。 如果选择此操作时，没有进一步的选项将可用。

#### <a name="controller-with-empty-readwrite-actions"></a>包含空读/写操作的控制器

此模板生成的方法中具有所有所需的操作方法，但没有实现代码的控制器文件。 此模板等效于检查*添加操作创建、 编辑、 详细信息，请删除方案*在以前版本的 ASP.NET MVC。 如果选择此操作时，没有进一步的选项将可用。

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>读/写操作和视图，使用实体框架的控制器

此模板可快速创建工作数据输入的用户界面。 它会生成处理的一系列常见要求和方案，如下所示的代码：

- *数据访问*。 生成的代码读取，并在数据库中写入实体。 如果选择现有的数据上下文类，或让生成一个新的模板配合 Entity Framework Code First 方法*DbContext*类。 它还适用于的实体框架 Database First 或 Model First 方法如果你选择现有*ObjectContext*类。
- *验证*。 生成的代码使用 ASP.NET MVC 模型绑定和元数据功能，以便窗体提交验证根据模型类上声明的规则。 这包括内置的验证规则，如*所需*和*StringLength*属性和自定义验证规则。
- *一个对多关系*。 如果模型类之间定义一个对多的外键关系，则生成的代码将生成用于选择相关的实体的下拉列表。 例如，你可以定义以下 Entity Framework Code First 约定的以下模型类： 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    然后搭建的控制器*产品*类，其视图将允许用户选择*类别*每个对象*产品*实例。

    此模板将启用中的其他选项*添加控制器*对话框。 有关*模型类*，则可以在你确定的用户将能够创建或编辑的数据类型的解决方案中选择任何模型类：
- 如果你想要使用 Entity Framework Code First，你可以选择任何模型类。
- 如果你使用实体框架 Database First 或 Entity Framework First 模型，请务必选择概念模型中定义的实体类。

有关*数据上下文类*，可在进行这些选择：

- 如果你想要使用 Code First 和没有现有的数据上下文类中，选择 * * 新的数据上下文 * *。 然后将为你生成的数据上下文类。
- 如果你想要使用 Code First 和具有现有的数据上下文类，在此处选择。 它将更新以保留您选择的模型类。
- 如果你使用 Database First 或 Model First，选择你的对象上下文类。

对于视图，选择你想要使用，视图引擎，或选择无如果你不想要创建的任何视图的基架。

你可以选择高级选项可用来进一步指定用于生成视图选项。 例如，你可以选择的布局或 master 页后，可以使用。

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>对改进"ASP.NET MVC 3 新项目"对话框

使用来创建新的 ASP.NET MVC 3 项目对话框中包括多项改进，如下所示。

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>新的"Intranet 项目"模板

项目模板列表中包括新的 Intranet 应用程序模板。 此模板包含用于生成 web 应用程序而不窗体身份验证使用 Windows 身份验证设置。 由于 intranet 应用程序需要某些不能在项目模板中封装的 IIS 设置，该模板将包含说明如何使工作在 IIS 中的项目模板的自述文件。 文档新的 Intranet 应用程序模板是在以下 URL MSDN 网站上提供：

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>项目模板现在是启用的 HTML5

新项目对话框中现在包含一个选项以将 HTML5 特定功能添加到项目模板。 选择的选项会导致视图以生成包含新的 HTML5 `<header>`， `<footer>`，和`<navigation>`元素。

请注意，早期版本的浏览器不支持 HTML5 特定标记。 若要解决此限制，HTML5 项目模板，请包括对 Modernizr 库的引用。 （请参阅下一节。）

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>项目模板现在包括 Modernizr 1.7

Modernizr 是启用对支持 CSS 3 和 HTML5 尚不支持这些功能的浏览器中的 JavaScript 库。 此库是用作 ASP.NET MVC 3 项目的模板中的预安装 NuGet 包。 有关 Modernizr 的详细信息，请参阅[http://www.modernizr.com/](http://www.modernizr.com/)。

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>项目模板包括的 jQuery、 jQuery UI 中和 jQuery 的更新的版本验证

项目模板现在包括以下版本的 jQuery 脚本：

- jQuery 1.5.1
- jQuery 验证 1.8
- jQuery UI 1.8.11

这些库是作为预安装 NuGet 包。

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>项目模板现在包括 ADO.NET 实体框架 4.1 作为预安装 NuGet 程序包

ADO.NET 实体框架 4.1 包括代码第一项功能。 代码首先是 ADO.NET 实体框架提供的现有的数据库第一个和第一个模型模式的替代方法的新开发模式。

代码首先是为中心，定义使用 POCO 类 （"纯旧 CLR 对象"） 在 Visual Basic 或 C# 编写你的模型。 这些类可以然后映射到现有数据库，或用于生成数据库架构。 可以使用提供其他配置*DataAnnotations*属性或使用 fluent Api。

从以下 Url ASP.NET 网站上提供了有关使用代码 Firstwith ASP.NET MVC 的文档：

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>项目模板包括 JavaScript 库作为预安装 NuGet 包

该项目时创建新的 ASP.NET MVC 3 项目时，通过安装它们包括前面提到的 JavaScript 文件 （例如，Modernizr 库） 而不直接将脚本添加到脚本文件夹中的项目模板中使用 NuGet内容。 这使你能够使用 NuGet 来更新到最新版本的脚本，在发布新版本的脚本。

例如，给定 jQuery 最新发布的频率，jQuery 包含在项目模板的版本在某一时刻将过期。 但是，由于 jQuery 是包含为已安装的 NuGet 包，你将通知在 NuGet 对话框中可用的 jQuery 的较新版本时。

由于 jQuery 文件名中包含的版本号，到最新版本更新 jQuery 还需要更新`<script>`引用要使用新的文件名称的 jQuery 文件的标记。 其他包含的脚本库不包括的版本号在脚本名称中，因此它们可以更轻松地更新到了其最新版本。

<a id="tu-KI"></a>
## <a name="known-issues"></a>已知问题

- 在某些情况下，安装可能失败，并错误消息"安装失败，出现错误代码 (: 0x80070643)"。 有关如何解决此问题的信息，请参阅[知识库文章 2531566](https://support.microsoft.com/kb/2531566)。
- 添加控制器的基架不搭建基架充分利用在实体框架的实体继承支持的实体。 例如，给定一个基*人员*由继承的类*学生*类，基架*学生*类将导致生成不进行编译的代码。
- 创建新的 ASP.NET MVC 3 项目在解决方案文件夹内原因*NullReferenceException*错误。 解决方法是解决方案的在根目录中创建 ASP.NET MVC 3 项目，然后将其移到的解决方案文件夹。
- 安装 ReSharper 时，Razor 语法的 IntelliSense 不会无法正常工作。 如果你已安装的 ReSharper 并想要利用 ASP.NET MVC 3 中的 Razor IntelliSense 支持，请参阅文章[Razor Intellisense 和 ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) Hadi Hariri 博客，其中讨论了如何立即使用它们在一起。
- 在安装期间，最终用户许可协议接受对话框中该小于预期大小的窗口中显示许可条款。
- 当你正在编辑的 Razor 视图 (.cshtml 或。*vbhtml*文件)，视图。 ASP.NET MVC 3 不包括任何段 Razor 视图...aspxselecting ASP.NET MVC 的代码段将显示有关代码段
- 如果您在其中未安装 Visual Studio 的计算机上的 Visual Web Developer Express 为安装 ASP.NET MVC 3，然后更高版本安装 Visual Studio，则必须重新安装 ASP.NET MVC 3。 Visual Studio 和 Visual Web Developer Express 共享的 ASP.NET MVC 3 安装程序升级的组件。 ASP.NET MVC 3 是 for Visual Studio 在没有 Visual Web Developer Express，并且更高版本安装 Visual Web Developer Express 的计算机上安装适用于属于相同问题。

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>在 ASP.NET MVC 3 RTM 中的更改

本部分介绍更改和自 RC2 版本以来在 ASP.NET MVC 3 RTM 版本所做的 bug 修补程序。

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>更改： 更新到 1.8.7 jQuery UI 的版本

Visual Studio 的 ASP.NET MVC 项目模板已更新以包括 jQuery UI 库的最新版本。 模板还包括所需的 jQuery UI 中，如关联的 CSS 和图像文件的资源文件的最小集。

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>更改： 更改 ModelMetadataProvider 回 DataAnnotationsModelMetadataProvider 了默认值

ASP.NET MVC 3 引入 RC2 版本*CachedDataAnnotationsMetadataProvider*类，提供基于现有缓存*DataAnnotationsModelMetadataProvider*作为类性能改进。 但是，某些已报告的 bug 与此实现中，以便还原更改并将其移入 MVC Future 项目，位于[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack)。

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>已修复： 粘贴 Razor 表达式包含空格导致它被反转的部分

在预发行版本的 ASP.NET MVC 3，当将粘贴到 Razor 文件，包含空格的 Razor 表达式的一部分生成的表达式是恰好相反。 例如，考虑以下 Razor 代码块：

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

如果您在第一种方法中选择"第一个 param"文本，并将其作为自变量粘贴到第二种方法，则结果是，如下所示：

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

正确的行为是粘贴操作应导致以下：

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

已在 RTM 版本中修复此问题，以便表达式正确地保留在粘贴操作。

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>语法着色和 IntelliSense 重命名在编辑器中打开的 Razor 文件禁用已修复：

重命名 Razor 文件在编辑器窗口中打开文件时使用解决方案资源管理器会导致语法突出显示和 IntelliSense 功能，停止使用该文件。 此问题已修复，以便突出显示和 IntelliSense 将保留在重命名后。

<a id="RTM-KI"></a>
## <a name="known-issues"></a>已知问题

- 如果你关闭 Visual Studio 2010 SP1 Beta，打开 NuGet 包管理器控制台时，Visual Studio 崩溃，并尝试重新启动。 此问题将修复 Visual Studio 2010 SP1 的 RTM 版本中。
- ASP.NET MVC 3 安装程序才能够安装 NuGet 包管理器的初始版本。 已安装的初始版本后，NuGet 可以安装和使用 Visual Studio Extension Manager 更新。 如果你已安装 NuGet，请转到 Visual Studio 扩展库更新到最新版本的 NuGet。
- 创建新的 ASP.NET MVC 3 项目在解决方案文件夹内原因*NullReferenceException*错误。 解决方法是解决方案的在根目录中创建 ASP.NET MVC 3 项目，然后将其移到的解决方案文件夹。
- 安装程序可能需要比以前版本的 ASP.NET MVC 完成长得多。 这是因为它会更新组件的 Visual Studio 2010。
- 安装 ReSharper 时，Razor 语法的 IntelliSense 不会无法正常工作。 如果你已安装的 ReSharper 并想要利用 ASP.NET MVC 3 中的 Razor IntelliSense 支持，请参阅文章[Razor Intellisense 和 ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) Hadi Hariri 博客，其中讨论了如何立即使用它们在一起。
- 使用 ASP.NET MVC 3 的 Beta 版本创建的 CCSHTML 和 VBHTML 视图没有正确设置这些文件的生成操作，使用这些查看结果类型时省略了该项目发布。 这些文件的生成操作值应设置为"内容"。 ASP.NET MVC 3 RTM 修复此问题对新文件，但不更正与预发布版本创建的项目的现有文件的设置。
- ![](mvc3-release-notes/_static/image3.png)
- 在安装期间，最终用户许可协议接受对话框中该小于预期大小的窗口中显示许可条款。
- 编辑 Razor 视图 （.cshtml 文件），请转到控制器菜单项在 Visual Studio 中的将不可用，并且没有任何代码段。
- 如果您在其中未安装 Visual Studio 的计算机上的 Visual Web Developer Express 为安装 ASP.NET MVC 3，然后更高版本安装 Visual Studio，则必须重新安装 ASP.NET MVC 3。 Visual Studio 和 Visual Web Developer Express 共享的 ASP.NET MVC 3 安装程序升级的组件。 ASP.NET MVC 3 是 for Visual Studio 在没有 Visual Web Developer Express，并且更高版本安装 Visual Web Developer Express 的计算机上安装适用于属于相同问题。

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>重大更改

- 在以前版本的 ASP.NET MVC，操作筛选器是创建每个在少数情况下除外的请求。 此行为永远不会有保证的行为，但只是实现详细信息，且筛选器的协定为设法无状态。 在 ASP.NET MVC 3 中，筛选器会更加主动地缓存。 因此，实例状态中存储任何自定义操作筛选器可能会被破坏。
- 异常筛选器的执行顺序已更改为具有相同的异常筛选器*顺序*值。 ASP.NET MVC 2 及更早版本，异常筛选器在具有相同的控制器上*顺序*值上的操作方法在上的操作方法的异常筛选器之前执行。 这通常会出现此情况，当异常筛选器应用而无需指定*顺序*值。 在 ASP.NET MVC 3 中，此具有已反转顺序，以便最具体的异常处理程序，则首先执行。 如下所示早期版本中，如果*顺序*显式指定属性，以指定顺序运行筛选器。
- 名为的新属性*FileExtensions*已添加到*VirtualPathProviderViewEngine*基类。 时 ASP.NET 查找视图按路径 （而不是按名称），都只能指定此新属性的列表中包含的文件扩展名的视图。 这是一项重大更改应用程序中的，其中自定义生成提供程序注册以启用 Web 窗体视图的自定义文件扩展，且该提供程序通过使用完整路径，而不是名称来引用这些视图。 解决方法是修改的值*FileExtensions*属性以包含自定义的文件扩展名。
- 直接实现的自定义控制器工厂实现*IControllerFactory*接口必须提供的新实现*GetControllerSessionBehavior*方法的添加到此版本中的接口。 一般情况下，建议你不直接实现此接口和相反派生您的类从*DefaultControllerFactory*。

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>ASP.NET MVC 3 RC2 中的更改

本部分介绍以来的 RC 版本中的 ASP.NET MVC 3 RC2 版本所做的更改 （新功能和缺陷修复）。

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>项目模板更改，以便包括 jQuery 1.4.4、 jQuery 验证 1.7 和 jQuery UI 1.8.6

ASP.NET MVC 3 的项目模板现在包括最新版本的 jQuery、 jQuery 验证和 jQuery UI。 jQuery UI 是一项新增到的项目模板，并提供有用的用户界面小组件。 有关 jQuery UI 的详细信息，请访问其主页： [http://jqueryui.com/](http://jqueryui.com/)。

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>添加"AdditionalMetadataAttribute"类

你可以使用*AdditionalMetadataAttribute*类来填充*ModelMetadata.AdditionalValues*模型属性的字典。

例如，假设视图模型具有应仅向管理员显示的属性。 该模型可以使用新的属性使用 AdminOnly 的键和 true 作为值，如以下示例所示添加批注：

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

呈现产品视图模型时，此元数据将提供给任何显示或编辑器模板。 它是完全取决于您的应用程序开发人员解释的元数据信息。

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>改进的视图基架

现在使用的基架视图的 T4 模板生成对模板帮助程序方法的调用，如*EditorFor*而不是如的帮助器*TextBoxFor*。 在添加视图对话框中生成一个视图时，此更改可改进对上的数据批注特性的窗体中的模型的元数据支持。

添加视图基架还包括改进的检测和使用情况的模型，基于约定的主要密钥信息。 例如，添加视图对话框中使用此信息以确保主键值不为一个可编辑窗体字段基架。

默认编辑和创建模板包括对所需的客户端验证的 jQuery 脚本引用。

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>添加的 Html.Raw 方法

默认情况下，Razor 查看所有值进行 HTML 编码的引擎。 例如，下面的代码段将编码的问候语变量中的 HTML，以便显示在页中作为`<strong>Hello World!</strong>`。

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

新*Html.Raw*方法提供一种简单显示未编码的 HTML 时知道的内容是安全。 下面的示例显示相同的字符串，但字符串呈现为标记：

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>重命名"Controller.ViewModel"属性并将"视图"属性为"ViewBag"

以前， *ViewModel*属性*控制器*对应于*视图*视图的属性。 这两种属性提供一种方法访问值*ViewDataDictionary*对象使用动态属性访问器的语法。 这两个属性已重命名为同一为避免混淆，且要更加一致。

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>重命名"ControllerSessionStateAttribute"类"SessionStateAttribute"

*ControllerSessionStateAttribute*类在 ASP.NET MVC 3 的 RC 版本中引入。 该属性已重命名为更简洁。

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>重命名的 RemoteAttribute"字段"属性设置为"AdditionalFields"

*RemoteAttribute*类的*字段*属性导致某些混淆，在用户之间。 重命名此属性设置为*AdditionalFields*阐明了其意图。

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>重命名"SkipRequestValidationAttribute"到"AllowHtmlAttribute"

*SkipRequestValidationAttribute*属性已重命名为*AllowHtmlAttribute*来更好地表示其预期的用法。

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>更改"Html.ValidationMessage"方法，以显示第一条有用的错误消息

*Html.ValidationMessage*已固定的方法，以便显示第一个的有用的错误消息，而不是只需显示的第一个错误。

在模型绑定*ModelState*可以从使用有关的属性，包括从模型本身的错误消息的多个源填充字典 (如果它实现*IValidatableObject*)，从验证特性应用于属性，以及从在访问属性时引发的异常。

当*Html.ValidationMessage*方法会显示一条验证消息，它会模型状态，其中可包含一个异常，跳过，因为这些通常不应为最终用户。 相反，该方法查找不是与异常相关联，并显示该消息的第一条验证消息。 如果不找到任何此类消息，则它将默认为与第一个异常关联的一般错误消息。

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>固定@model声明，以向文档添加空格

在早期版本中，  *@model* 视图顶部的声明添加到的呈现的 HTML 输出一个空行。 此问题已修复，以便该声明不会引入的空格。

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>添加"FileExtensions"属性设置为视图引擎，以支持引擎特定文件的名称

视图引擎可以返回的视图使用显式视图路径如以下示例所示：

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

第一个视图引擎始终尝试呈现视图。 默认情况下，Web 窗体视图引擎是第一个视图引擎;因为 Web 窗体引擎无法呈现 Razor 视图中，将会出错。 视图引擎现在都有*FileExtensions*它们支持的属性，用于指定哪些文件扩展名。 在 ASP.NET 确定视图引擎是否可以呈现文件时，会检查此属性。 这是一项重大更改，更多详细信息包含在[的重大更改](#_Toc2_BC)本文档部分。

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>用于发出"For"属性的正确值的固定"LabelFor"帮助

已修复 bug 的位置*LabelFor*方法呈现*为*匹配属性*输入*元素的*名称*特性其 id。 根据 W3C，*为*特性应与匹配*输入*元素的 id。

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>固定"RenderAction"方法，以便为模型绑定期间的显式值优先

在早期版本中，已将传递给的显式值*RenderAction*方法已支持当前的窗体值在模型绑定内的子操作期间忽略的。 修补程序可确保，在模型绑定优先的显式值。

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>重大更改

- 在以前版本的 ASP.NET MVC，每个请求除外在少数情况下创建的操作筛选器。 此行为永远不会有保证的行为，但只是实现详细信息，且筛选器的协定为设法无状态。 在 ASP.NET MVC 3 中，筛选器会更加主动地缓存。 因此，实例状态中存储任何自定义操作筛选器可能会被破坏。
- 异常筛选器的执行顺序已更改为具有相同的异常筛选器*顺序*值。 ASP.NET MVC 2 及更早版本，异常筛选器在包含相同的控制器上*顺序*值上的操作方法已在上的操作方法的异常筛选器之前执行。 这通常会出现此情况，当异常筛选器应用而无需指定*顺序*值。 在 ASP.NET MVC 3 中，此具有已反转顺序，以便最具体的异常处理程序，则首先执行。 如下所示早期版本中，如果*顺序*显式指定属性，以指定顺序运行筛选器。
- 名为的新属性*FileExtensions*已添加到*VirtualPathProviderViewEngine*基类。 时 ASP.NET 查找视图按路径 （而不是按名称），都只能指定此新属性的列表中包含的文件扩展名的视图。 这是一项重大更改应用程序中的，其中自定义生成提供程序注册以启用 Web 窗体视图的自定义文件扩展，且该提供程序通过使用完整路径，而不是名称来引用这些视图。 解决方法是修改的值*FileExtensions*属性以包含自定义的文件扩展名。
- 直接实现的自定义控制器工厂实现*IControllerFactory*接口必须提供的新实现*GetControllerSessionBehavior * * 已添加到的方法在此版本中的接口*。 一般情况下，建议你不直接实现此接口和相反派生您的类从*DefaultControllerFactory*。

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>已知问题

- ASP.NET MVC 3 安装程序才能够安装 NuGet 包管理器的初始版本。 已安装的初始版本后，NuGet 可以安装和使用 Visual Studio Extension Manager 更新。 如果你已安装 NuGet，请转到 Visual Studio 扩展库更新到最新版本的 NuGet。
- 创建新的 ASP.NET MVC 3 项目在解决方案文件夹内原因*NullReferenceException*错误。 解决方法是解决方案的在根目录中创建 ASP.NET MVC 3 项目，然后将其移到的解决方案文件夹。
- 安装程序可能需要比以前版本的 ASP.NET MVC 完成长得多。 这是因为它会更新组件的 Visual Studio 2010。
- 安装 ReSharper 时，Razor 语法的 IntelliSense 不会无法正常工作。 如果你已安装的 ReSharper 并想要利用 ASP.NET MVC 3 RC2 中的 Razor IntelliSense 支持，请参阅文章[Razor Intellisense 和 ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) Hadi Hariri 博客，其中讨论了如何立即使用它们在一起。
- 使用 ASP.NET MVC 3 的 Beta 版本创建的 CSHTML 和 VBHTML 视图没有正确设置这些文件的生成操作，在发布项目时将类型省略这些查看结果。 *生成操作*这些文件应设置为内容的值"。 ASP.NET MVC 3 RC2 修复对新文件，此问题，但不更正使用 Beta 版本创建的项目的现有文件的设置。![](mvc3-release-notes/_static/image4.png)
- 在安装期间，最终用户许可协议接受对话框中该小于预期大小的窗口中显示许可条款。
- 编辑 Razor 视图 （.cshtml 文件），请转到控制器菜单项在 Visual Studio 中的将不可用，并且没有任何代码段。
- 如果您在其中未安装 Visual Studio 的计算机上的 Visual Web Developer Express 为安装 ASP.NET MVC 3，然后更高版本安装 Visual Studio，则必须重新安装 ASP.NET MVC 3。 Visual Studio 和 Visual Web Developer Express 共享的 ASP.NET MVC 3 安装程序升级的组件。 ASP.NET MVC 3 是 for Visual Studio 在没有 Visual Web Developer Express，并且更高版本安装 Visual Web Developer Express 的计算机上安装适用于属于相同问题。
- 安装 ASP.NET MVC 3 RC 2 不更新 NuGet，如果你没有安装它。 若要升级 NuGet，请转到 Visual Studio 扩展管理器，它应显示为可用的更新。 你可以升级到中存在的最新版本的 NuGet。

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>ASP.NET MVC 3 候选发布版

ASP.NET MVC 候选发布版已于 2010 年 11 月 9 日发布。

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>ASP.NET MVC 3 RC 中的新增功能

本部分介绍已引入的功能在 ASP.NET MVC 3 RC 版本以来 Beta 版本。

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>NuGet 程序包管理器

ASP.NET MVC 3 包括 NuGet 包管理器 （以前称为 NuPack），它是将库和工具添加到 Visual Studio 项目的集成的包管理工具。 此工具自动执行开发人员采取今天到其源树中获取的库的步骤。

作为命令行工具、 在 Visual Studio 2010 中，从 Visual Studio 上下文菜单中，一个集成式的控制台窗口和一组 PowerShell cmdlet，你可以使用 NuGet。

有关 NuGet 的详细信息，请阅读[Nuget 文档](https://docs.microsoft.com/nuget/)。

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>改进了"新项目"对话框

当创建新项目时，新建项目对话框中现在允许你指定视图引擎，以及 ASP.NET MVC 项目类型。

![](mvc3-release-notes/_static/image5.png)

此版本中包括了对修改的模板和引擎在对话框中列出的视图列表的支持。

默认模板如下所示：

为空。 包含一个 ASP.NET MVC 项目，包括默认目录结构对于 ASP.NET MVC 项目，包含的默认的 ASP.NET MVC 样式，然后包含默认的 JavaScript 文件的脚本目录的 Site.css 文件的文件的最小集。

Internet 应用程序。 包含演示如何使用 ASP.NET MVC 使用成员资格提供程序的示例功能。

在对话框中显示的项目模板列表指定在 Windows 注册表中。

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>无会话控制器

新*ControllerSessionStateAttribute*将授予你更好地控制会话状态行为的控制器通过指定[System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx)枚举值。

下面的示例演示如何关闭到控制器的所有请求的会话状态。

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

下面的示例演示如何将所有请求的只读的会话状态设置为一个控制器。

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>新的验证属性

#### <a name="compareattribute"></a>CompareAttribute

新*CompareAttribute*验证特性，可以比较两个不同模型属性的值。 在下面的示例中， *ComparePassword*属性必须与匹配*密码*字段才有效。

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

新*RemoteAttribute*验证属性充分利用验证即插即用接程序的 jQuery 远程验证程序，它使客户端验证在执行实际验证逻辑的服务器上调用的方法。

在下面的示例中，*用户名*属性具有*RemoteAttribute*应用。 客户端验证当编辑此属性在编辑视图中的，将调用名为操作*UserNameAvailable*上*UsersController*若要验证此字段的类。

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

下面的示例显示相应的控制器。

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

默认情况下，此特性应用到的属性名称是作为查询字符串参数发送到的操作方法。

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>有关"LabelFor"和"LabelForModel"方法的新重载

为添加新的重载了*LabelFor*和*LabelForModel*让你的方法指定标签文本。 下面的示例演示如何使用这些重载。

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>子操作输出缓存

*OutputCacheAttribute*支持输出缓存使用调用的子操作的*Html.RenderAction*或*Html.Action*帮助器方法。 下面的示例演示调用另一个操作的视图。

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

*GetDate*操作进行批注*OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

此代码运行时，到 Html.Action("GetDate") 调用的结果被缓存 100 秒。

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>"添加视图"对话框框中改进

当你添加的强类型化的视图时，添加视图对话框中现在筛选出比以前版本中，如许多核心.NET Framework 类型中的多个不适用的类型。 此外，现在排序列表，按类名而不是它可以更轻松地查找类型的完全限定的类型名称。 例如，类型名称现在显示如以下示例所示：

类名 （命名空间）

在早期版本中，这将显示如下所示：

Namespace.ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>精细请求验证

*排除*属性*ValidateInputAttribute*不再存在。 相反，如果希望在模型绑定期间跳过的特定属性的模型的请求验证，请使用新*SkipRequestValidationAttribute*。

例如，假设操作方法用于编辑博客文章：

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

下面的示例显示博客文章的视图模型。

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

当用户提交 Description 属性一些标记时，模型绑定将因请求验证。 若要禁用请求验证博客文章说明的模型绑定期间，应用*SkipRequpestValidationAttribute*给属性，如本示例中所示:。

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

或者，若要关闭该模型的每个属性的请求验证，应用*ValidateInputAttribute*值为*false*到操作方法：

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>重大更改

- 异常筛选器的执行顺序已更改为具有相同的异常筛选器*顺序*值。 ASP.NET MVC 2 及更早版本，异常筛选器在包含相同的控制器上*顺序*上操作方法已在上的操作方法的异常筛选器之前执行。 这通常会出现此情况，当异常筛选器应用而无需指定*顺序*值。 在 ASP.NET MVC 3 中，此具有已反转顺序，以便最具体的异常处理程序，则首先执行。 如下所示早期版本中，如果*顺序*显式指定属性，以指定顺序运行筛选器。
- 添加名为的新属性*FileExtensions*到*VirtualPathProviderViewEngine*基类。 查找视图由路径中 （而不是名称），仅具有文件扩展名中包含的视图时才被视为此新的属性指定的列表。 这是一项重大更改的用户的用户注册自定义生成提供程序以启用 web 窗体视图的自定义文件扩展，并通过使用完整路径，而不是名称引用这些视图。 解决方法是修改的值*FileExtensions*属性以包含自定义的文件扩展名。

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>已知问题

- 安装程序可能需要比以前版本的 ASP.NET MVC，才能完成，因为它会更新组件的 Visual Studio 2010 长得多。
- 选择 astrongly 时的添加视图基架类型的视图的基架只写属性。 这些始终应忽略的基架。 添加视图对话框还的基架只读属性生成的"编辑"创建"视图时。 仅只读属性显示和列表视图的基架。
- 与异步 CTP 一起安装 ASP.NET MVC 3 时，调试不起作用。 ASP.NET MVC 3 不能为与异步 CTP 并行安装。 卸载异步 CTP 修复调试。 有关详细信息，请阅读[这篇博客文章](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html)有关卸载 ASP.NET MVC 3 RC 的所有部分。
- 安装 Resharper 时，razor Intellisense 不会无法正常工作。 如果你已安装的 ReSharper 并想要利用 ASP.NET MVC 3 RC，请阅读 Razor intellisense 支持的[这篇博客文章](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/)从 JetBrains 其中讨论了如何立即使用它们在一起。
- 使用 ASP.NET MVC 3 的测试版创建的 CSHTML 和 VBHTML 视图没有这些文件的生成操作正确它省略它们的发布。 *生成操作*这些文件应设置为"内容"。 ASP.NET MVC 3 RC 解决了对新文件，此问题，但不更正使用试用版创建的项目的现有文件的设置。
- 安装程序可能需要比以前版本的 ASP.NET MVC，才能完成，因为它会更新组件的 Visual Studio 2010 长得多。
- 如果选择了"编辑"强类型的基架视图中的添加视图基架只读属性。 同样，"显示"视图中基架只写属性。
- 在安装期间，最终用户许可协议接受对话框中该小于预期大小的窗口中显示许可条款。
- 安装 Visual Studio 异步 CTP 引起冲突 Razor 版本中的 ASP.NET MVC 3 工具安装的一部分。 请确保不尝试在同一台计算机上安装 Visual Studio 异步 CTP 和 Razor 版本。
- 编辑 Razor 视图 （.cshtml 文件），请转到控制器菜单项在 Visual Studio 中的将不可用，并且没有任何代码段。

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>ASP.NET MVC 3 Beta

ASP.NET MVC 3 Beta 已于 2010 年 10 月 6 日发布。 以下说明特定于 Beta 版本中，并且受到任何更新或更改在上面的 ASP.NET MVC 3 预发布版一节中的引用。

## <a id="0.1__Toc274034215"></a>新 Featuresin ASP.NET MVC 3 Beta

<a id="0.1__Default_validation_system"></a>本部分介绍已引入的功能在 ASP.NET MVC 3 Beta 版本。

### <a id="0.1__Toc274034216"></a>NuGet 包管理器

ASP.NET MVC 3 包括 NuGet 包管理器，它是添加的库的集成的包管理工具和 Visual Studio 项目的工具。 大多数情况下，它可以自动化开发人员采取今天到其源树中获取的库的步骤。

作为命令行工具、 在 Visual Studio 2010 中，从 Visual Studio 上下文菜单中，一个集成式的控制台窗口和组的 PowerShell cmdlet，你可以使用 NuGet。

有关 NuGet 的详细信息，请阅读[NuGet 文档](https://docs.microsoft.com/nuget/)。

### <a id="0.1__Toc274034217"></a>改进的新项目对话框中

当创建新项目时，新建项目对话框中现在允许你指定视图引擎，以及 ASP.NET MVC 项目类型。

![](mvc3-release-notes/_static/image6.png)

此版本中不包括对修改的模板和引擎在对话框中列出的视图列表的支持。

默认模板如下所示：

为空。 包含一个 ASP.NET MVC 项目，包括默认目录结构对于 ASP.NET MVC 项目，默认的 ASP.NET MVC 样式，然后包含默认的 JavaScript 文件的脚本目录包含一个小 Site.css 文件的文件的最小集。

Internet 应用程序。 包含演示如何使用 ASP.NET MVC 中的成员资格提供程序的示例功能。

### <a id="0.1__Toc274034218"></a>简化的方法来指定强类型化模型，在 Razor 视图中

通过使用新简化了指定强类型化 Razor 视图的模型类型的方式@modelCSHTML 视图的指令和@ModelTypeVBHTML 视图的指令。 在早期版本的 ASP.NET MVC，则会指定 Razor 的强类型化的模型视图这种方式：

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

在此版本中，你可以使用以下语法：

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>支持新的 ASP.NET Web 页的帮助器方法

新的 ASP.NET Web Pages 技术包括一组可用于将常用的功能添加到视图和控制器的帮助器方法。 ASP.NET MVC 3 支持使用控制器和视图中的这些帮助器方法 （如果适用）。 这些方法包含在 System.Web.Helpers 程序集。 下表列出了 ASP.NET Web Pages 帮助器方法。

| **Helper** | **说明** |
| --- | --- |
| Chart | 呈现的视图中的图表。 包含如 Chart.ToWebImage、 Chart.Save 和 Chart.Write 的方法。 |
| 加密 | 使用哈希算法来创建正确 salted 和哈希处理密码。 |
| WebGrid | 为网格中呈现的对象 （通常情况下，数据库中的数据） 的集合。 支持分页和排序。 |
| WebImage | 呈现图像。 |
| WebMail | 发送电子邮件。 |

快速参考本主题列出的帮助器和基本语法可用作 ASP.NET Razor 语法文档在以下 URL 的一部分：

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>附加依赖项注入支持

ASP.NET MVC 3 预览 1 发行版上构建，当前版本包括添加了对这两个新的服务和四个现有服务，支持和改进的依赖项解析和常见服务定位符支持。

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>有关细化控制器实例化新 IControllerActivator 接口

新的 IControllerActivator 接口提供了更好地控制细化控制器通过依赖关系注入实例化的方式。 下面的示例显示了界面：

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

相比之下到控制器工厂的角色。 控制器工厂是负责对于查找控制器类型和实例化该控制器类型的实例的 IControllerFactory 接口的实现。

控制器激活器负责仅实例化控制器类型的实例。 它们不会执行控制器类型查找。 找到正确的控制器类型之后, 控制器工厂应委托 IControllerActivator 的实例来处理的控制器的实际实例化。

DefaultControllerFactory 类具有新的构造函数接受 IControllerFactory 实例。 这使你能够应用依赖关系注入以管理控制器创建的这个方面，而无需重写默认控制器类型查找行为。

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>替换为 IDependencyResolver IServiceLocator 接口

根据社区反馈，ASP.NET MVC 3 Beta 版本已替换为 IServiceLocator 接口使用特定于 ASP.NET MVC 的需求的简化下 IDependencyResolver 接口。 下面的示例显示了新的界面：

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

作为此更改的一部分，ServiceLocator 类也已替换 DependencyResolver 类。 注册的依赖项解析程序是类似于早期版本的 ASP.NET MVC:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

此接口的实现应只需将委托给基础的依赖关系注入容器提供所请求类型的已注册的服务。

当存在请求的类型的任何已注册的服务时，ASP.NET MVC 需要实现此接口来从 GetService 返回 null 和从 GetServices 返回空集合。

新的 DependencyResolver 类使您能够注册实现新 IDependencyResolver 接口或通用服务定位器接口 (IServiceLocator) 的类。 有关常见服务定位符的详细信息，请参阅[GitHub 上的 CommonServiceLocator](https://github.com/unitycontainer/commonservicelocator)。

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>有关细化视图页实例化新 IViewActivator 接口

新的 IViewPageActivator 接口提供了更好地控制细化查看页通过依赖关系注入实例化的方式。 这适用于 WebFormView 实例和 RazorView 实例。 下面的示例显示了新的界面：

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

这些类现在接受 IViewPageActivator 构造函数自变量，该对话框允许你使用依赖关系注入来控制 ViewPage、 ViewUserControl 和 WebViewPage 类型实例化方式。

#### <a name="new-dependency-resolver-support-for-existing-services"></a>对于现有服务的新依赖项解析程序支持

新版本包括以下服务的依赖项解析支持：

- 为模型验证提供程序。 实现 ModelValidatorProvider 的类可以注册在依赖项解析程序，并且系统将使用它们以支持客户端和服务器端验证。
- 模型元数据提供程序。 单个类实现 ModelMetadataProvider 可以注册在依赖项解析程序，并系统将使用它来提供的模板化和验证系统的元数据。
- 值在提供程序。 实现 ValueProviderFactory 的类可以注册在依赖项解析程序，并且系统将使用它们创建控制器和模型绑定期间使用的值提供程序。
- 模型联编程序。 实现 IModelBinderProvider 的类可以注册在依赖项解析程序，并且系统将使用它们创建模型联编程序是由模型绑定系统。

### <a id="0.1__Toc274034221"></a>对非介入式基于 jQuery 的 Ajax 的全新支持

ASP.NET MVC 包括 Ajax 帮助器方法，如下所示：

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

这些方法用于 JavaScript 调用在服务器上，而不是使用完整的回发的操作方法。 此功能已进行更新，以利用 jQuery 非介入式的方式。 而不是干扰的方式发出内联客户端脚本，这些帮助器方法分开行为标记发出 HTML5 属性使用*数据 ajax*前缀。 然后将行为应用于标记通过引用适当的 JavaScript 文件。 请确保引用以下 JavaScript 文件：

- jquery-1.4.1.js
- jquery.unobtrusive.ajax.js

此功能默认情况下，在 ASP.NET MVC 3 新项目模板中，Web.config 文件中启用，但默认情况下的现有项目处于禁用状态。 有关详细信息，请参阅[添加用于客户端验证和非介入式 JavaScript 的应用程序范围内标志](#0.1_AddedApplicationWideFlagsForClientValida)本文档后面部分。

### <a id="0.1__Toc274034222"></a>有关非介入式 jQuery 验证新的支持

默认情况下，ASP.NET MVC 3 Beta 使用 jQuery 验证以便执行客户端验证非介入式的方式中。 若要启用非介入式客户端验证，请如下所示的视图中的以下方面：

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

这需要 ViewContext.UnobtrusiveJavaScriptEnabled 属性设置为 true，则你可以通过进行以下调用：

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

此外请确保以下 JavaScript 文件引用。

- jquery-1.4.1.js
- jquery.validate.js
- jquery.validate.unobtrusive.js

此功能在 Web.config 文件中的 ASP.NET MVC 3 新项目模板，默认情况下启用，但默认情况下的现有项目处于禁用状态。 有关详细信息，请参阅[用于客户端验证和非介入式 JavaScript 的新应用程序范围内标志](#0.1_AddedApplicationWideFlagsForClientValida)本文档后面部分。

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>用于客户端验证和非介入式 JavaScript 的新应用程序范围内标志

你可以启用或禁用客户端验证和全局使用 HtmlHelper 类，如以下示例所示的静态成员的非介入式 JavaScript:

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

默认情况下，默认的项目模板启用非介入式 JavaScript。 你还可以启用或禁用使用以下设置的应用程序的根 Web.config 文件中的这些功能：

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

由于默认情况下，你可以启用这些功能，让你重写默认设置，如下面的示例中所示的 HtmlHelper 类引入新的重载：

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

为了向后兼容，这两项功能默认处于禁用状态。

### <a id="0.1__Toc274034224"></a>对视图运行之前运行的代码的新支持

你现在可以将一个名为文件\_viewstart.cshtml (或\_viewstart.vbhtml) Views 目录中并将代码添加到它将在多个视图之间共享该目录及其子目录中。 例如，可能会将下面的代码插入\_viewstart.cshtml 页 ~/Views 文件夹中：

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

这将设置在视图的文件夹内的每个视图和所有其子文件夹以递归方式的布局页。 当视图呈现中的代码\_viewstart.cshtml 文件运行之前查看代码运行。 \_Viewstart.cshtml 代码适用于该文件夹中的每个视图。

默认情况下中的代码\_viewstart.cshtml 文件也适用于任何子文件夹中的视图。 但是，各个子文件夹，可以有各自版本\_viewstart.cshtml 文件;，因为情况下，本地版本将优先。 例如，若要运行普遍适用于为 HomeController 的所有视图的代码，将\_viewstart.cshtml ~/Views/Home 文件夹中的文件。

### <a id="0.1__Toc274034225"></a>对 VBHTML Razor 语法的全新支持

以前的 ASP.NET MVC 预览版包括对使用基于 C# 的 Razor 语法的视图的支持。 这些视图使用.cshtml 文件扩展名。 作为正在进行的工作以支持 Razor 的一部分，ASP.NET MVC 3 Beta 引入了有关在 Visual Basic 中使用的.vbhtml 文件扩展名的 Razor 语法的支持。

在 VBHTML 页中使用 Visual Basic 语法的说明，请参阅本教程在以下 URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>更精细地控制 ValidateInputAttribute

ASP.NET MVC 始终具有包括 ValidateInputAttribute 类，该类时，将调用核心 ASP.NET 请求验证基础结构以确保传入的请求不包含潜在的恶意输入。 默认情况下，启用了输入的验证。 很可能要禁用请求验证使用 ValidateInputAttribute 特性，如以下示例所示：

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

但是，许多 web 应用程序有需要时剩余的字段不应允许 HTML，单个表单域。 ValidateInputAttribute 类现在允许你指定的字段不应包含在请求验证的列表。

例如，如果你正在开发博客引擎，你可能想要允许的正文和摘要字段中的标记。 这些字段可能由两个输入元素，每个都有对应的属性名称 （"正文"和"摘要"） 的名称特性表示。 若要禁用请求验证为这些字段，指定 （以逗号分隔） ValidateInput 类，如以下示例所示的排除属性中的名称：

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>帮助器将连字符下划线转换为使用匿名对象指定的 HTML 属性名称

帮助器方法，你可以指定属性名称/值对使用匿名对象，如以下示例所示：

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

因为 ASP.NET 中的属性名称不能使用连字符，此方法不允许你使用的属性名称中中的连字符。 但是，连字符是重要的自定义的 HTML5 属性;例如，HTML5 使用"数据-"前缀。

同时，下划线不能用于在 HTML 中，属性名称，但在属性名称内有效。 因此，如果指定使用的匿名对象的属性和属性名称包含下划线、 帮助器方法会将下划线转换为连字符。 例如，以下帮助器语法使用下划线：

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

帮助器运行时，前面的示例将会呈现以下标记：

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>Bug 修复

EditorFor 和 DisplayFor 模板帮助程序的默认对象模板现在支持 DisplayAttribute.Order 属性中指定的顺序。 （在以前版本，顺序设置未使用。）

客户端验证现在支持验证的已应用的验证特性的重写属性。

默认情况下，现在已注册 JsonValueProviderFactory。

## <a id="0.1__Toc274034229"></a>重大更改

异常筛选器的执行顺序已更改为具有相同的顺序值的异常筛选器。 ASP.NET MVC 2 及更早版本，异常筛选器具有相同的顺序在控制器上，如上操作方法已在上的操作方法的异常筛选器之前执行。 异常筛选器已应用而无需指定的顺序值时，这通常会出现此情况。 在 ASP.NET MVC 3 中，此具有已反转顺序，以便最具体的异常处理程序，则首先执行。 如下所示早期版本中，如果显式指定顺序属性，以指定顺序运行筛选器。

## <a id="0.1__Toc274034230"></a>已知的问题

在安装期间，最终用户许可协议接受对话框中该小于预期大小的窗口中显示许可条款。

Razor 视图没有 IntelliSense 支持也不语法突出显示。 我们已预见到将会更高版本的一部分包括对 Visual Studio 中的 Razor 语法的支持。

在编辑 Razor 视图 （CSHTML 文件） 时<a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a>转到控制器 Visual Studio 中的菜单项将不可用，并且不有任何代码段。

使用时@model语法来指定强类型化的 CSHTML 视图中，无法识别的类型的特定于语言的快捷方式。 例如， @model int 不起作用，但@modelInt32 将起作用。 此 bug 的解决方法是指定的模型类型时使用的实际类型名称。

使用时@model语法来指定强类型化的 CSHTML 视图 (或@ModelType指定强类型化的 VBHTML 视图)，不支持可以为 null 的类型和数组声明。 例如， @model int？ 不支持。 请改用`@model Nullable<Int32>`。 语法@modelstring []，也不支持; 请改用`@model IList<string>`。

在 ASP.NET MVC 2 项目升级到 ASP.NET MVC 3 时，请确保将以下内容添加到 Web.config 文件的 appSettings 节：

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

没有导致窗体身份验证，始终将未经身份验证的用户重定向到 ~/Account/登录名，忽略在 Web.config 中使用的窗体身份验证设置的已知的问题。解决方法是添加以下应用程序设置。

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>  Disclaimer

© 2011 Microsoft Corporation. 保留所有权利。 本文档提供"作为-是。" 信息和包括 URL 和其他 Internet 网站引用，本文档中表达的观点可能更改恕不另行通知。 您自行承担其使用风险。

本文档未向您提供任何 Microsoft 产品中任何知识产权的任何合法权利。 您可为了内部参考目的复制和使用本文档。
