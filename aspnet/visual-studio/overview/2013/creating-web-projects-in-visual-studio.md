---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: "创建 Visual Studio 2013 中的 ASP.NET Web 项目 |Microsoft 文档"
author: tdykstra
description: "本主题介绍在使用此处更新 3 的 Visual Studio 2013 中创建 ASP.NET web 项目的选项是一些用于 web 开发 c 的新增功能..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/01/2014
ms.topic: article
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: aacae7a9ccf483b21d3c6796c0411d558fa3c75b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>在 Visual Studio 2013 中创建 ASP.NET Web 项目
====================
通过[Tom Dykstra](https://github.com/tdykstra)

> 本主题说明了在 Visual Studio 2013 Update 3 中创建 ASP.NET web 项目的选项
> 
> 下面是一些用于相对于早期版本的 Visual Studio 的 web 开发的新增功能：
> 
> - 用于创建一个简单的 UI 项目提供[支持多个 ASP.NET 框架](#add)（Web 窗体、 MVC 和 Web API）。
> - [ASP.NET 标识](#indauth)，新的 ASP.NET 成员资格系统中所有的 ASP.NET 框架和工作原理相同适用于 web 托管非 IIS 的软件。
> - 使用[Bootstrap](#bootstrap)提供响应式设计和主题功能。
> - 用于仅为 MVC，如提供的 Web 窗体的新功能[自动测试项目创建](#testproj)和[Intranet 站点模板](#winauth)。
> 
> 有关如何为 Azure 云服务或 Azure 移动服务中创建 web 项目的信息，请参阅[Azure 云服务和 ASP.NET 入门](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/)和[使用 Azure 移动服务.NET 创建排行榜应用程序后端](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)。


<a id="prerequisites"></a>
## <a name="prerequisites"></a>系统必备

本文适用于[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)与[Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409)安装。

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>与网站项目的 web 应用程序项目

ASP.NET 为你提供了两种类型的 web 项目之间进行选择： *web 应用程序项目*和*网站项目*。 我们建议对于新开发，web 应用程序项目，并且本文仅适用于 web 应用程序项目。 有关详细信息，请参阅[Web 应用程序项目与 Visual Studio 中的网站项目](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx)MSDN 网站上的。

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Web 应用程序项目创建的概述

以下步骤演示了如何创建 web 项目：

1. 单击**新项目**中**启动**页或在**文件**菜单。
2. 在**新项目**对话框中，单击**Web**的左窗格中和**ASP.NET Web 应用程序**在中间窗格中。

    ![“新建项目”对话框](creating-web-projects-in-visual-studio/_static/image1.png)

    你可以选择**云**在左窗格中创建[Azure 云服务](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)， [Azure 移动服务](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx)，或[Azure web 作业](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs)。 本主题并未涵盖这些模板。
3. 在右窗格中，单击**向项目添加 Application Insights**复选框，如果你希望运行状况和使用情况监视你的应用程序。 有关详细信息，请参阅[监视 web 应用程序性能的](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/)。
4. 指定项目**名称**，**位置**，和其他选项，，然后单击**确定**。

    **新建 ASP.NET 项目**此时将显示对话框。

    ![“新建项目”对话框](creating-web-projects-in-visual-studio/_static/image2.png)
5. 单击模板。

    ![选择模板](creating-web-projects-in-visual-studio/_static/image3.png)
6. 如果你想要添加未包含模板中的其他框架的支持，请单击相应的复选框。 （在示例中所示，你无法 MVC 和/或 Web API 向项目中添加 Web 窗体。）

    ![添加框架](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>如果你想要添加单元测试项目，请单击**添加单元测试**。

    ![添加单元测试](creating-web-projects-in-visual-studio/_static/image5.png)
8. 如果希望比默认情况下所提供的模板的不同的身份验证方法，请单击**更改身份验证**。

    ![配置身份验证按钮](creating-web-projects-in-visual-studio/_static/image6.png)

    ![配置身份验证对话框](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>在 Azure 中创建 web 应用程序或虚拟机

Visual Studio 包含使其易于使用的 Azure 服务以托管 web 应用程序的功能。 例如，可以直接从 Visual Studio IDE 执行所有以下操作：

- 创建和管理 web 应用程序或使你的应用程序可通过 Internet 的虚拟机。
- 查看在云中运行应用程序创建的日志。
- 在调试模式下远程运行应用程序在云中运行时。
- Viiew 和管理 SQL 数据库之类的其他 Azure 服务。

你可以[创建 Azure 帐户](https://www.windowsazure.com/pricing/free-trial/)免费，包括基本的服务，如 web apps，并且如果你是 MSDN 订阅者可以[激活权益](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/)，为你提供其他 azure 的月度信用额度服务。 

默认情况下**新建 ASP.NET 项目**对话框中，可以创建 web 应用程序或新的 web 项目的虚拟机。 如果你不想要创建新的 web 应用程序或虚拟机，清除**在云中托管**复选框。

![创建远程资源](creating-web-projects-in-visual-studio/_static/image8.png)

复选框标题可能**在云中托管**或**创建远程资源**，并在任一情况下效果都是相同。 如果你离开选中此复选框，Visual Studio 创建的 web 应用在 Azure App Service 中默认情况下。 你可以使用下拉框更改，则为**虚拟机**如果你愿意。 如果你尚未登录到 Azure，将提示你输入 Azure 凭据。 登录后，对话框中可配置 Visual Studio 将为你的项目创建的资源。 下图显示了为 web 应用; 的对话框如果你选择创建虚拟机，将显示不同的选项。

![配置 Azure 应用程序设置](creating-web-projects-in-visual-studio/_static/image9.png)

有关如何使用此过程来创建 Azure 资源的详细信息，请参阅[Azure 和 ASP.NET 入门](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)和[使用 Visual Studio 创建用于网站的虚拟机](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/)。

此文章的剩余部分提供有关可用的模板和其选项的详细信息。 文章还介绍了 Bootstrap、 布局和主题 framework 了模板中使用。

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Visual Studio 2013 Web 项目模板

Visual Studio 将使用模板创建 web 项目。 项目模板可以在新项目中创建文件和文件夹，安装 NuGet 包，并为一个基本的工作应用程序提供的示例代码。 模板实现最新的 web 标准，用于演示如何使用 ASP.NET 技术，以及为你提供跳转的最佳实践开始创建自己的应用程序。

Visual Studio 2013 为对面向.NET 4.5 或更高版本的.NET framework 的项目的 web 项目模板提供以下几种选择：

- [空模板](#empty)
- [Web 窗体模板](#wf)
- [MVC 模板](#mvc)
- [Web API 模板](#webapi)
- [单页面应用程序模板](#spa)
- [Azure 移动服务模板](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Visual Studio 2012 模板](#vs2012)

你还可以安装 Visual Studio 扩展，提供[Facebook 模板](#facebook)。

有关如何创建.NET 4 为目标的项目的信息，请参阅[Visual Studio 2012 模板](#vs2012)本主题中更高版本。

有关如何创建移动客户端的 ASP.NET 应用程序的信息，请参阅[在 ASP.NET 中的移动支持](../../../mobile/index.md)。

<a id="empty"></a>
### <a name="empty-template"></a>空模板

空模板提供的裸机最小的文件夹和 ASP.NET web 应用程序，如项目文件的文件 (*.csproj*或。*vbproj*) 和一个*Web.config*文件。 你可以使用下面的复选框添加 Web 窗体、 MVC，和/或 Web API 的支持**将文件夹添加和核心引用：**标签。

空的模板没有身份验证选项才可用。 在示例应用程序中实现身份验证功能和空模板不会创建示例应用程序。

<a id="wf"></a>
### <a name="web-forms-template"></a>Web 窗体模板

框架提供了以下功能，让你快速构建丰富，UI 和数据中的网站的 Web 窗体访问功能：

- Visual Studio 中的所见即所得设计器。
- 呈现 HTML 和，你可以通过设置属性和样式自定义的服务器控件。
- 丰富多种类型的控件对数据访问和数据显示。
- 公开哪个应用可以您这类程序的事件的事件模型将程序的客户端应用程序，例如 WPF。
- 自动保留的 HTTP 请求之间的状态 （数据）。

一般情况下，创建 Web 窗体应用程序需要更少编程的工作量比使用 ASP.NET MVC framework 创建相同的应用程序。 但是，Web 窗体不再仅仅用于快速开发应用程序。 有许多复杂的商业应用程序和基于 Web 窗体的框架。

Web 窗体页和页上的控件自动生成大部分发送到浏览器的标记，因为你没有对 ASP.NET MVC 提供的 HTML 细化的控制的类型。 配置页面和控件的声明性模型最大程度减少必须编写，但会隐藏一些 HTML 和 HTTP 的行为的代码量。 例如，它并不总是可以指定控件可能完全生成哪些标记。

Web 窗体 framework 不会将自身添加 ASP.NET MVC 为随时到基于模式的开发实践如[测试驱动开发](http://en.wikipedia.org/wiki/Test-driven_development)，[关注点分离](http://en.wikipedia.org/wiki/Separation_of_concerns)，[反向的控件](http://en.wikipedia.org/wiki/Inversion_of_control)，和[依赖关系注入](http://en.wikipedia.org/wiki/Dependency_injection)。 如果你想要编写代码考虑这样一来，你可以;不只是因为它是 ASP.NET MVC framework 中为自动。 [ASP.NET Web 窗体 MVP](http://webformsmvp.com/)项目演示一种方法，它方便了问题和可测试性的同时保持生成 Web 窗体提供的快速发展的分离。 Microsoft SharePoint 是在 Web 窗体 MVP 基础的。

Web 窗体模板创建的示例 Web 窗体应用程序使用[Bootstrap](#bootstrap)以提供响应式设计和主题功能。 下图显示主页页面。

![Web 窗体模板的应用程序主页](creating-web-projects-in-visual-studio/_static/image10.png)

有关 Web 窗体的详细信息，请参阅[ASP.NET Web 窗体](https://asp.net/web-forms)。 有关 Web 窗体模板为你的作用的信息，请参阅[生成基本 Web 窗体应用程序使用 Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx)。

<a id="mvc"></a>
### <a name="mvc-template"></a>MVC 模板

ASP.NET MVC 旨在如促进基于模式的开发实践[测试驱动开发](http://en.wikipedia.org/wiki/Test-driven_development)，[关注点分离](http://en.wikipedia.org/wiki/Separation_of_concerns)，[反向的控件](http://en.wikipedia.org/wiki/Inversion_of_control)，和[依赖关系注入](http://en.wikipedia.org/wiki/Dependency_injection)。 框架鼓励将业务逻辑层中其表示层的 web 应用程序分开。 通过将应用程序划分为模型 (M)、 视图 (V) 和控制器 (C)，ASP.NET MVC 可以更加轻松地管理更大的应用程序中的复杂性。

通过 ASP.NET MVC，您可以更直接与 HTML 和比在 Web 窗体中的 HTTP。 例如，Web 窗体可自动保留状态之间的 HTTP 请求，但你必须在 MVC 显式代码。 MVC 模型的优点是它，您可以完全控制完全你的应用程序正在执行什么操作和 web 环境中的行为方式。 其缺点在于，你必须编写更多代码。

MVC 的设计目标是可扩展的提供 power 开发人员能够自定义其应用程序需求的框架。 此外，ASP.NET MVC 源代码，请参阅 OSI 许可证。

MVC 模板中创建的示例 MVC 5 应用程序使用[Bootstrap](#bootstrap)以提供响应式设计和主题功能。 下图显示主页页面。

![MVC 示例应用程序](creating-web-projects-in-visual-studio/_static/image11.png)

有关 MVC 的详细信息，请参阅[ASP.NET MVC](https://asp.net/mvc)。 有关如何选择 MVC 4 模板的信息，请参阅[Visual Studio 2012 模板](#vs2012)本文后续部分中。

<a id="webapi"></a>
### <a name="web-api-template"></a>Web API 模板

Web API 模板创建基于 Web API，包括 API 帮助页基于 MVC 的示例 web 服务。

ASP.NET Web API 是一个框架，可以轻松地生成覆盖广泛的客户端，包括浏览器和移动设备的 HTTP 服务。 ASP.NET Web API 是用于在.NET Framework 上生成 RESTful 服务的理想平台。

Web API 模板创建的示例 web 服务。 下图显示示例帮助页。

![Web API 帮助页](creating-web-projects-in-visual-studio/_static/image12.png)

![获取 API 的 web API 帮助页](creating-web-projects-in-visual-studio/_static/image13.png)

有关 Web API 的详细信息，请参阅[ASP.NET Web API](https://asp.net/web-api)。

<a id="spa"></a>
### <a name="single-page-application-template"></a>单页面应用程序模板

单页应用程序 (SPA) 模板创建的示例应用程序使用 JavaScript，HTML 5 和[KnockoutJS](http://knockoutjs.com/)上客户端和服务器上的 ASP.NET Web API。

SPA 模板的唯一身份验证选项是[单个用户帐户](#indauth)。

下图显示的示例应用程序的 SPA 模板生成的初始状态。

![SPA 示例应用程序](creating-web-projects-in-visual-studio/_static/image14.png)

有关如何创建了应用程序使用 SPA 模板的信息，请参阅[Web API 的外部身份验证服务](../../../web-api/overview/security/external-authentication-services.md)。

有关 ASP.NET 单页面应用程序，以及使用 JavaScript 框架 KnockoutJS 以外的其他 SPA 模板的详细信息，请参阅以下资源：

- [ASP.NET 单页面应用程序](../../../single-page-application/index.md)。
- [VS2013 rc 了解 SPA 模板中的安全功能](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [单页面应用程序： 生成现代的响应式 Web 应用程序的 ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Facebook 模板

你可以安装[Visual Studio 扩展，提供了一个 Facebook 模板](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409)。 此模板创建的示例应用程序设计为 Facebook 网站中运行。 它基于 ASP.NET MVC 并实时更新功能将使用 Web API。

任何身份验证选项不是适用于 Facebook 模板，因为 Facebook 应用程序的 Facebook 站点内运行，并依赖于 Facebook 的身份验证。

有关 ASP.NET Facebook 应用程序的详细信息，请参阅[更新 MVC Facebook API](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx)。

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Visual Studio 2012 模板

Visual Studio 2013 web 项目创建对话框不提供对在 Visual Studio 2012 中提供的一些模板的访问。 如果你想要使用这些模板之一，你可以单击 Visual Studio 的新项目对话框的左窗格中的 Visual Studio 2012 节点。

![Visual Studio 2012 模板](creating-web-projects-in-visual-studio/_static/image15.png)

**Visual Studio 2012**节点允许您选择不具有以下 web 模板将在模板的默认列表的访问 Visual Studio 2013:

- ASP.NET MVC 4 Web 应用程序
- ASP.NET 动态数据实体 Web 应用程序
- ASP.NET AJAX 服务器控件
- ASP.NET AJAX 服务器控件扩展程序
- ASP.NET 服务器控件

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>启动 Visual Studio 2013 web 项目模板中

Visual Studio 2013 项目模板使用[Bootstrap](http://getbootstrap.com/)，由 Twitter 的布局和主题框架。 Bootstrap 使用 CSS3 提供响应灵敏的设计，这意味着布局可以动态调整以适应不同的浏览器窗口大小。 例如，在宽的浏览器窗口中创建 Web 窗体模板的主页上类似于下图：

![Web 窗体模板的应用程序主页](creating-web-projects-in-visual-studio/_static/image16.png)

使窗口变窄，请和水平排列的列将移动到垂直排列：

![启动垂直列排列方式](creating-web-projects-in-visual-studio/_static/image17.png)

有点令缩小窗口并水平顶部的菜单将转变为可单击以展开到垂直方向菜单的图标：

![启动菜单图标](creating-web-projects-in-visual-studio/_static/image18.png)

![启动垂直菜单](creating-web-projects-in-visual-studio/_static/image19.png)

Bootstrap 的主题功能还可用于轻松地影响应用程序的外观和行为的更改。 例如，你可以执行以下步骤以更改的主题。

1. 在浏览器中，转到[http://Bootswatch.com](http://Bootswatch.com)，选择一个主题，然后单击**下载**。 (这会将下载*bootstrap.min.css*默认情况下; 如果你想要检查 CSS 代码，获取*bootstrap.css*而不是缩减的版本。)
2. 下载的 CSS 文件的内容复制。
3. 在 Visual Studio 中，创建一个新**样式表**名为文件*bootstrap theme.css*中*内容*文件夹，然后粘贴下载 CSS 代码到它。
4. 打开*应用\_Start/Bundle.config*和更改*bootstrap.css*到*bootstrap theme.css*。

再次运行该项目，并且该应用程序的新面貌。 下图显示 Amelia 主题的效果：

![Bootstrap Amelia 主题](creating-web-projects-in-visual-studio/_static/image20.png)

许多 Bootstrap 主题有免费和高级版的版本。 Bootstrap 还提供了各种用户界面组件，如[下拉列表](http://twitter.github.io/bootstrap/components.html#dropdowns)，[按钮组](http://twitter.github.io/bootstrap/components.html#buttonGroups)，和[图标](http://twitter.github.io/bootstrap/base-css.html#images)。 有关 Bootstrap 的详细信息，请参阅[启动站点](http://twitter.github.io/bootstrap/)。

如果在 Visual Studio 中使用 Web 窗体设计器，请注意设计器不支持 CSS3，因此它准确地反映的 Bootstrap 主题或响应式布局更改的所有结果。 但是，Web 窗体页面将显示正确使用的浏览器查看时。

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>添加对其他框架的支持

当你选择的模板时，自动选择模板使用 framework(s) 复选框。 例如，如果你选择**Web 窗体**模板， **Web 窗体**复选框处于选中状态，无法将其清除。

![选择模板](creating-web-projects-in-visual-studio/_static/image21.png)

![添加框架](creating-web-projects-in-visual-studio/_static/image22.png)

你可以选择不包括在模板以创建项目时添加对该 framework 的支持的框架复选框。 例如，若要启用的 Web 窗体使用*.aspx*页，如果你已选择 MVC 模板中，选择**Web 窗体**复选框。 若要在你使用 Web 窗体模板时，请启用 MVC，单击或**MVC**复选框。 添加一个框架，使设计时，以及运行时支持。 例如，如果 MVC 支持添加到 Web 窗体项目时，你将能够创建控制器和视图的基架。

如果你将 Web 窗体和 MVC 合并项目中，启用[友好 Url](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx)在 Web 窗体，可能会有意外路由问题有一个 URL，其中具有多个可能的目标。 定义的路由首先将优先。 例如，如果你有`Home`控制器和一个*Home.aspx*页上， `http://contoso.com/home` URL 将转到*Home.aspx*如果调用`EnableFriendlyUrls`方法之前调用`MapRoute`中的方法*RouteConfig.cs*，或者相同的 URL 将转到的默认视图你`Home`控制器如果调用`MapRoute`之前`EnableFriendlyUrls`。

添加一个框架，不会添加任何示例应用程序功能。 例如，如果你添加 Web 窗体支持如果你已选择 MVC 模板中，不*Default.aspx*创建主页文件。 添加文件夹、 文件和支持 framework 所需的引用。 因此，添加框架不会更改由通过模板创建的示例应用程序中的代码实现的身份验证选项。 例如，如果你选择空模板并添加 Web 窗体或 MVC 支持，**配置身份验证**仍为已禁用按钮。

下列各节简要描述每个复选框的效果。

### <a name="add-web-forms-support"></a>添加 Web 窗体支持

创建空*应用\_数据*和*模型*文件夹和*Global.asax*文件。 这些已创建的所有模板而不是空的模板，因此选择 Web 窗体复选框没有任何区别其他模板。

默认情况下，但当你添加到其他模板支持通过选择友好的 Url 不会自动启用 Web 窗体复选框的 Web 窗体时，Web 窗体模板使友好的 Url。

### <a name="add-mvc-support"></a>添加的 MVC 支持

安装 MVC、 Razor 和网页 NuGet 包，创建空*应用\_数据*，*控制器*，*模型*，和*视图*文件夹，创建*应用\_启动*文件夹*RouteConfig.cs*文件中，并创建*Global.asax*文件。

### <a name="add-web-api-support"></a>添加 Web API 支持

安装 WebApi 和 Newtonsoft.Json NuGet 包，创建空*应用\_数据*，*控制器*，和*模型*文件夹，创建*应用\_启动*文件夹*WebApiConfig.cs*文件中，并创建*Global.asax*文件。

<a id="auth"></a>
## <a name="authentication-methods"></a>身份验证方法

Visual Studio 2013 提供 Web 窗体、 MVC 和 Web API 模板的多个身份验证选项：

- [无身份验证](#noauth)
- [单个用户帐户](#indauth)（ASP.NET 标识，以前称为 ASP.NET 成员资格）
- [组织帐户](#orgauth)（Windows Server Active Directory 或 Azure Active Directory）
- [Windows 身份验证](#winauth)(Intranet)

![配置身份验证对话框](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>无身份验证

如果你选择**无身份验证**、 示例应用程序将包含用于登录的任何网页、 不 UI 中，该值指示登录的用户，没有实体类的成员资格数据库和的成员资格数据库没有连接字符串。

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>单个用户帐户

如果你选择**单个用户帐户**，示例应用程序将配置为使用 ASP.NET 标识 （以前称为 ASP.NET 成员资格） 进行用户身份验证。 ASP.NET 标识使用户可以注册一个帐户，通过在站点上创建用户名和密码或使用社交提供程序，如 Facebook、 Google、 Microsoft 帐户或 Twitter 登录。 在 ASP.NET Identity 中的用户配置文件的默认数据存储是一个 SQL Server LocalDB 数据库，它可以在生产站点对 SQL Server 或 Azure SQL 数据库部署。

在 Visual Studio 2013 中这些功能与 Visual Studio 2012 中的相同，但已重新编写 ASP.NET 成员资格系统的基础代码。 新的基本代码的优点包括：

- 新的成员资格系统基于[OWIN](http://owin.org/)而不是 ASP.NET 窗体身份验证模块。 这意味着，你可以使用相同的身份验证机制，是否在你在 IIS 中，使用 Web 窗体或 MVC 或你在自承载 Web API 或 SignalR。
- 新的成员资格数据库由 Entity Framework Code First，和由你可以修改的实体类表示的所有表。 这意味着可以轻松自定义的数据库架构和配置文件相关的 web UI，以满足你自己的需求，并且你可以轻松部署使用 Code First 迁移更新。

在新的模板中，自动实现新的成员身份系统，并且它可以是实现在面向.NET 4.5 的任何项目中手动或更高版本。

如果你要创建 Internet 网站，这是主要用于外部客户，ASP.NET 标识是一个不错的选择。 如果你的组织使用 Active Directory 或 Office 365 和你想要创建的项目是启用单一登录为员工和业务合作伙伴**组织帐户**选项可能是更好的选择。

有关单个用户帐户选项的详细信息，请参阅以下资源：

- [www.asp.net/identity](../../../identity/index.md)。 有关 ASP.NET Identity ASP.NET web 站点上的文档。
- [创建 ASP.NET MVC 5 应用程序使用 Facebook 和 Google OAuth2 和 OpenID 登录](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。 此外演示如何自定义用户配置文件数据。
- [Web API 的外部身份验证服务](../../../web-api/overview/security/external-authentication-services.md)
- [将外部登录名添加到你在 Visual Studio 2013 的 ASP.NET 应用程序](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>组织帐户

如果你选择**组织帐户**，示例应用程序将配置为使用 Windows Identity Foundation (WIF) 进行基于 Azure Active Directory (Azure AD，其中包括 Office 365) 中的用户帐户的身份验证或Windows Server Active Directory。 有关详细信息，请参阅[组织帐户身份验证选项](#orgauthoptions)本主题中更高版本。

<a id="winauth"></a>
### <a name="windows-authentication"></a>Windows 身份验证

如果你选择**Windows 身份验证**，示例应用程序将配置为使用 Windows 身份验证 IIS 模块进行身份验证。 应用程序将显示 Active directory 或本地计算机帐户的登录到 Windows，但不会包括用户注册或登录 UI 的域和用户 ID。 此选项适用于 Intranet 网站。

或者，可以创建 Intranet 站点使用 AD 身份验证通过选择[本地组织的帐户下的选项](#orgauthonprem)。 本地选项使用 Windows Identity Foundation (WIF)，而不是 Windows 身份验证模块。 若要设置的本地选项中，需要执行一些其他步骤，但 WIF 使不可适用于 Windows 身份验证模块的功能。 例如，与 WIF 可以配置应用程序访问在 Active Directory 和查询目录数据。

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>组织帐户身份验证选项

**配置身份验证**对话框为你提供了 Azure Active Directory (Azure AD，其中包括 Office 365) 或 Windows Server Active Directory (AD) 帐户身份验证的几个选项：

- [云-单个组织](#orgauthsingle)(Azure AD 或使用与 Azure AD 目录集成的 AD)
- [云-多组织](#orgauthmulti)(Azure AD 或使用与 Azure AD 目录集成的 AD)
- [在本地](#orgauthonprem)(AD)

如果你想要尝试 Azure AD 选项之一，但还没有帐户，[单击此处注册 Azure AD 帐户](https://go.microsoft.com/fwlink/?LinkId=309942)。

> [!NOTE]
> 如果你选择的 Azure AD 选项之一，你的项目需要一个数据库，您必须为你的 Azure AD 租户全局管理员帐户登录。 输入组织帐户的名称和密码 (例如， admin@contoso.onmicrosoft.com)，其对你的 Azure AD 租户的管理权限。
> 
> **不要输入 Microsoft 帐户的凭据 (例如， contoso@hotmail.com) 在登录对话框中。**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>云-单个组织身份验证

![单个组织身份验证](creating-web-projects-in-visual-studio/_static/image24.png)

如果你想要启用的用户帐户的一个 Azure AD 中定义的身份验证，请选择此选项[租户](https://technet.microsoft.com/library/jj573650.aspx)。 例如，该网站为 contoso.com，而且它将可供 contoso.onmicrosoft.com 租户中的 Contoso 公司员工。 你将无法配置 Azure AD，以允许用户从其他租户可以访问应用程序。

#### <a name="domain"></a>Domain

输入你想要在设置应用程序，例如 Azure AD 域： `contoso.onmicrosoft.com`。 如果你有[自定义域](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/)，如`contoso.com`而不是`contoso.onmicrosoft.com`，你可以在此处输入的。

#### <a name="access-level"></a>访问级别

如果应用程序需要查询或通过使用 Graph API 更新目录信息，请选择**上单一登录、 读取目录数据**或**上单一登录，读取和写入目录数据**。 否则，请选择**上单一登录**。 有关详细信息，请参阅[应用程序访问级别](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels)和[使用 Graph API 查询 Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx)。

#### <a name="application-id-uri"></a>应用程序 ID URI

默认情况下，该模板通过将项目名称追加到 Azure AD 域中创建用于你的应用程序 ID URI。 例如，如果项目名称为`Example`，域为`contoso.onmicrosoft.com`，应用程序 ID URI 将成为`https://contoso.onmicrosoft.com/Example`。 如果你想要手动指定应用程序 ID URI，则展开**更多选项**部分，然后在文本框中输入应用程序 ID URI。 应用程序 ID URI 必须以开头`https://`。

默认情况下，如果已在 Azure AD 中设置应用程序具有相同应用程序 ID URI，作为一个 Visual Studio 使用项目，项目将连接到现有应用程序而不是预配一个新。 如果你想要在这种情况下设置新应用程序，清除**覆盖应用程序条目，如果已经存在一个具有相同 ID**复选框。

如果**覆盖**清除复选框，并且 Visual Studio 找到具有相同的应用程序 ID URI 的现有应用程序，它通过将大量追加到它已将使用的 URI 创建一个新的 URI。 例如，假定项目名称是`Example`，将文本框留空，则清除**覆盖**复选框，并且 Azure AD 租户已具有 URI 的应用程序`https://contoso.onmicrosoft.com/Example`。 在这种情况下，新的应用程序将设置与应用程序 ID URI 喜欢`https://contoso.onmicrosoft.com/Example_20130619330903`。

#### <a name="provisioning-the-application-in-azure-ad"></a>设置 Azure AD 中的应用程序

若要设置 Azure AD 中的应用程序或将项目连接到现有应用程序，Visual Studio 为域需要的全局管理员的凭据。 当你单击**确定**中**配置身份验证**对话框中，提示你输入的用户名和密码作为你指定的域的全局管理员。 以后，当你单击**创建项目**中**新建 ASP.NET 项目**对话框中，Visual Studio 中设置 Azure AD 中的应用程序。 请注意，此过程的一部分 Visual Studio 客户端密钥值中嵌入过期一年后创建的 Web.config 文件。

有关如何创建使用的应用程序信息**云-单个组织**身份验证，请参阅以下资源：

- [Azure 身份验证](../2012/windows-azure-authentication.md)
- [将登录名添加到 Web 应用程序使用 Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [借助 Azure Active Directory 开发 ASP.NET 应用](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [保护与 Azure AD 的 ASP.NET Web API 和 Microsoft OWIN 组件](https://msdn.microsoft.com/magazine/dn463788.aspx)

这些教程具有尚未更新 Visual Studio 2013;哪些教程指示你手动执行的一些是在 Visual Studio 2013 中自动运行。

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>云-多组织身份验证

![多个组织身份验证](creating-web-projects-in-visual-studio/_static/image25.png)

如果你想要启用的用户帐户在多个 Azure AD 中定义的身份验证，请选择此选项[租户](https://technet.microsoft.com/library/jj573650.aspx)。 例如，该网站为 contoso.com，而且它将可供员工的 Contoso 公司都在 contoso.onmicrosoft.com 租户中和 Fabrikam 公司的 fabrikam.onmicrosoft.com 租户中。

你输入的设置和应用程序设置步骤是类似于[单个组织身份验证](#orgauthsingle)。

有关如何创建使用的应用程序信息**云-多组织**身份验证，请参阅以下资源：

- [轻松与 Azure Active Directory，ASP.NET 的 Web 应用程序集成&amp;Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) Active Directory 团队博客上。
- [开发多租户 Web 应用程序与 Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx)教程。 本教程尚未尚未更新 Visual Studio 2013;什么本教程将引导你手动执行的一些是在 Visual Studio 2013 中自动运行。
- [你必须注册订阅与你自己的多个组织 ASP.NET 应用，你可以登录之前](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/)。 在创建项目，使用多组织身份验证时，会遇到说明了如何解决常见的问题人员的 Vittorio Bertocci 的博客。

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>在本地组织的身份验证

![在本地组织的身份验证](creating-web-projects-in-visual-studio/_static/image26.png)

如果你想要启用身份验证的用户帐户的定义在 Windows Server Active Directory (AD)，并且你不想要使用 Azure AD，请选择此选项。 此选项可用于创建 Intranet 站点或 Internet 站点。 对于 Internet 站点，使用 Active Directory 联合身份验证服务 (ADFS) 以提供对 AD 的访问。 有关详细信息，请参阅[使用 Visual Studio 2013 中的本地组织身份验证选项 (ADFS) 与 ASP.NET](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)。

对于 Intranet 站点，或者你可以选择[Windows 身份验证](#winauth)而不是此选项。 为 Windows 身份验证选项，你无需提供元数据文档 URL。 但是，Windows 身份验证不使你能够控制 Active Directory 中的应用程序访问或查询目录数据。

#### <a name="on-premises-authority"></a>本地机构

输入指向元数据文档的 URL。 元数据文档包含颁发机构的坐标。 应用程序将使用这些坐标来驱动 web 登录流程。

#### <a name="application-id-uri"></a>应用程序 ID URI

提供 AD 可用于标识此应用程序，或留空以让 Visual Studio 创建一个唯一 URI。

<a id="nextsteps"></a>
## <a name="next-steps"></a>后续步骤

本文档提供一些基本帮助让 Visual Studio 2013 中创建新的 ASP.NET web 项目。 有关使用 for Visual Studio 进行 web 开发的详细信息，请参阅[https://www.asp.net/visual-studio/](../../index.md)。
