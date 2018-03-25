---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
title: "与不同版本的 IIS (C#) 使用 ASP.NET MVC |Microsoft 文档"
author: microsoft
description: "在本教程中，你将了解如何使用 ASP.NET MVC 和 URL 路由，用于不同版本的 Internet Information Services。 了解不同的策略..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: b0cf4a34-2c1d-4717-bb54-ff029e722990
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
msc.type: authoredcontent
ms.openlocfilehash: 8f2b98d5e5ae677fdac32336d542202a40290e21
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-c"></a>ASP.NET MVC 使用不同版本的 IIS (C#)
====================
by [Microsoft](https://github.com/microsoft)

> 在本教程中，你将了解如何使用 ASP.NET MVC 和 URL 路由，用于不同版本的 Internet Information Services。 了解关于使用 IIS 7.0 （经典模式）、 IIS 6.0 和早期版本的 IIS 中使用 ASP.NET MVC 的不同策略。


将浏览器请求路由到控制器操作情况下，ASP.NET MVC framework 依赖于 ASP.NET 路由状态。 若要充分利用 ASP.NET 路由，你可能需要 web 服务器上执行其他配置步骤。 这完全取决于 Internet 信息服务 (IIS) 和请求处理你的应用程序模式的版本。

下面是 IIS 的不同版本的摘要：

- IIS 7.0 （集成模式）-使用 ASP.NET 路由所需任何特殊配置。
- IIS 7.0 （经典模式）-你需要执行特殊的配置来使用 ASP.NET 路由。
- IIS 6.0 或下面-你需要执行特殊的配置来使用 ASP.NET 路由。

安装最新版本的 IIS 是版本 7.5 （在 Win7)。 IIS 7 的 IIS 是包含 Windows Server 2008 和 VISTA/SP1 和更高版本。 此外可以安装在任何版本的除 Home Basic Vista 操作系统上的 IIS 7.0 (请参阅[https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx))。

IIS 7.0 支持两种模式，用于处理请求。 你可以使用集成模式下或经典模式。 你不需要在集成模式下使用 IIS 7.0 时执行任何特殊的配置步骤。 但是，你需要在经典模式下使用 IIS 7.0 时执行其他配置。

Microsoft Windows Server 2003 包括 IIS 6.0。 使用 Windows Server 2003 操作系统时，不能向 IIS 7.0 升级 IIS 6.0。 使用 IIS 6.0 时，必须执行其他配置步骤。

Microsoft Windows XP Professional 包括 IIS 5.1。 使用 IIS 5.1 时，必须执行其他配置步骤。

最后，Microsoft Windows 2000 和 Microsoft Windows 2000 Professional 包括 IIS 5.0。 使用 IIS 5.0 时，必须执行其他配置步骤。

## <a name="integrated-versus-classic-mode"></a>集成与经典模式

IIS 7.0 可以处理使用两种不同的请求处理模式的请求： 集成和经典。 集成模式下提供更好的性能和更多的功能。 经典模式下为包含的向后兼容早期版本的 IIS。

请求处理模式取决于应用程序池。 你可以确定特定的 web 应用程序正在将哪种处理模式使用通过确定与应用程序关联的应用程序池。 请执行这些步骤：

1. 启动 Internet Information Services 管理器
2. 在连接窗口中，选择应用程序
3. 在操作窗口中，单击**基本设置**链接以打开编辑应用程序对话框框中 （请参见图 1）
4. 记下所选的应用程序池。

默认情况下，配置 IIS 以支持两个应用程序池： **DefaultAppPool**和**经典版.NET AppPool**。 如果选择了 DefaultAppPool，然后在集成的请求处理模式下运行你的应用程序。 如果选择经典版.NET AppPool，则你的应用程序在经典请求处理模式下运行。

[![新项目对话框](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.png)

**图 1**： 检测的请求处理模式 ([单击以查看实际尺寸的图像](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.png))

请注意，你可以修改在编辑应用程序对话框中的请求处理模式。 单击选择按钮并更改与应用程序关联的应用程序池。 请注意，我们兼容性问题时将 ASP.NET 应用程序从经典更改为集成模式下。 有关详细信息，请参阅以下文章：

- 升级到 Windows Vista 和 Windows Server 2008-上的 IIS 7.0 的 ASP.NET 1.1 [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)
- 与 IIS 7.0 的 ASP.NET 集成[https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

如果 ASP.NET 应用程序使用 DefaultAppPool，你不需要执行任何额外的步骤，以获得 ASP.NET 路由 （以及因此 ASP.NET MVC） 工作。 但是，如果 ASP.NET 应用程序配置为使用经典.NET 应用程序池，然后继续阅读，你将具有更多工作要做。

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>与较旧版本的 IIS 使用 ASP.NET MVC

如果你需要使用 IIS 的版本低于 IIS 7.0、 ASP.NET MVC 或你需要在经典模式下使用 IIS 7.0，则可以使用两个选项。 首先，你可以修改要使用文件扩展名的路由表。 例如，而不是请求的 URL，如 /Store/Details，将请求的 URL，如 /Store.aspx/Details。

第二个选项是创建称为*通配符脚本映射*。 通配符脚本映射，可将每个请求映射到 ASP.NET 框架。

如果你无权访问你的 web 服务器 (例如，应用程序托管的 Internet 服务提供商你 ASP.NET MVC) 到你将需要使用第一个选项。 如果不想修改你的 Url 的外观，并且可以访问你的 web 服务器，你可以使用第二个选项。

我们探讨以下各节详细地每个选项。

## <a name="adding-extensions-to-the-route-table"></a>将扩展添加到路由表

获取 ASP.NET 路由要使用较旧版本的 IIS 的最简单方法是修改路由表中的 Global.asax 文件。 默认值并列出 1 中的未修改的 Global.asax 文件配置一个名为默认路由的路由。

**列表 1-Global.asax （未修改）**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample1.cs)]

在列出 1 中配置的默认路由，可以路由 Url，如下所示：

/ Home/Index

/Product/Details/3

/ 产品

遗憾的是，较旧版本的 IIS 不会将这些请求传递给 ASP.NET 框架。 因此，不会获取这些请求路由到控制器中。 例如，如果你对浏览器请求进行 URL /Home/索引然后将在图 2 中获得的错误页。

[![新项目对话框](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.png)

**图 2**： 接收 404 未找到错误 ([单击以查看实际尺寸的图像](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.png))

较旧版本的 IIS 只能映射到 ASP.NET 框架的某些请求。 请求必须是针对正确的文件扩展名的 URL。 例如，/SomePage.aspx 请求获取映射到 ASP.NET 框架。 但是，/SomePage.htm 的请求，将不执行。

因此，若要获取 ASP.NET 路由来工作，我们必须修改默认路由，以便其包括文件扩展名映射到 ASP.NET 框架。

完成此操作使用名为的脚本`registermvc.wsf`。 它不包括在 ASP.NET MVC 1 发行版`C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`，但自 ASP.NET 2 起此脚本已移动到在 ASP.NET Future [http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978)。

执行此脚本向 IIS 注册一个新的.mvc 扩展。 注册.mvc 扩展后，你可以修改 Global.asax 文件中的路由，以便使用.mvc 扩展的路由。

修改后的 Global.asax 文件中列出 2 适用于较旧版本的 IIS。

**列出 2-Global.asax （修改与扩展）**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample2.cs)]

**重要**： 请记住更改 Global.asax 文件后，再次生成 ASP.NET MVC 应用程序。

有两项重要变化列出 2 中的 Global.asax 文件。 现在有两个在 Global.asax 中定义的路由。 默认路由的第一个路由的 URL 模式现在如下所示：

{controller}.mvc/{action}/{id}

.Mvc 扩展添加更改 ASP.NET 路由模块截获的文件的类型。 进行此更改后，ASP.NET MVC 应用程序现在将如下所示的请求路由：

/Home.mvc/Index/

/Product.mvc/Details/3

/Product.mvc/

第二个路由，根路由，是新增功能。 此根路由的 URL 模式为一个空字符串。 此路由是必要的匹配对你应用程序的根目录发出的请求。 例如，根路由将与如下所示的请求匹配：

[http://www.YourApplication.com/](http://www.YourApplication.com/)

进行这些修改到路由表之后, 将需要确保你的应用程序中的链接的所有这些新的 URL 模式兼容。 换而言之，请确保您的所有链接包括.mvc 扩展名。 如果使用 Html.ActionLink() 帮助器方法来生成你的链接，然后不应需要进行任何更改。

而不是使用 registermvc.wcf 脚本，你可以向映射到 ASP.NET framework 手动的 IIS 添加一个新的扩展。 在您自己添加一个新的扩展，请确保复选框标记为**验证文件是否存在**未选中。

## <a name="hosted-server"></a>托管的服务器

始终不到你的 web 服务器可以访问。 例如，如果托管的 ASP.NET MVC 应用程序中使用的 Internet 承载提供程序，然后你将不一定有权 IIS。

在这种情况下，你应使用现有的文件扩展名映射到 ASP.NET 框架之一。 文件扩展名映射到 ASP.NET 的示例包括.aspx、.axd 和.ashx 扩展。

例如，修改后的 Global.asax 文件中列出的 3，而不是.mvc 扩展使用.aspx 扩展。

**列出 3-Global.asax （修改.aspx 扩展名）。**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample3.cs)]

列出 3 中的 Global.asax 文件正是只不过它使用.aspx 扩展而非.mvc 扩展以前的 Global.asax 文件相同。 你无需使用.aspx 扩展你远程 web 服务器上执行任何设置。

## <a name="creating-a-wildcard-script-map"></a>创建通配符脚本映射

如果你不想要修改 ASP.NET MVC 应用程序的 Url，并且可以访问你的 web 服务器，则可以使用一个附加选项。 你可以创建通配符脚本映射到 web 服务器以 ASP.NET framework 映射的所有请求。 这样一来，你可以使用 IIS 7.0 （在经典模式下） 或 IIS 6.0 中使用默认 ASP.NET MVC 路由表。

请注意，此选项会导致 IIS 以截获对 web 服务器所做的每个请求。 这包括图像、 经典 ASP 页面和 HTML 页的请求。 因此，启用通配符脚本映射到 ASP.NET 未产生性能影响。

下面是如何为 IIS 7.0 中启用通配符脚本映射：

1. 在连接窗口中选择你的应用程序
2. 请确保**功能**选择视图
3. 双击**处理程序映射**按钮
4. 单击**添加通配符脚本映射**链接 （请参见图 3）
5. 输入的路径 aspnet\_isapi.dll 文件 （你可以从 PageHandlerFactory 脚本映射复制此路径）
6. 输入名称 MVC
7. 单击**确定**按钮

[![新项目对话框](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.png)

**图 3**： 使用 IIS 7.0 创建通配符脚本映射 ([单击以查看实际尺寸的图像](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image6.png))

请按照以下步骤创建带有 IIS 6.0 的通配符脚本映射操作：

1. 右键单击网站并选择属性
2. 选择**主目录**选项卡
3. 单击**配置**按钮
4. 选择**映射**选项卡
5. 单击**插入**按钮 （请参见图 4）
6. 粘贴到 aspnet 路径\_isapi.dll 到可执行 （这可以从.aspx 文件的脚本映射中复制此路径） 的字段
7. 取消选中复选框标记为**验证该文件存在**
8. 单击**确定**按钮

[![新项目对话框](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image7.png)

**图 4**： 使用 IIS 6.0 中创建通配符脚本映射 ([单击以查看实际尺寸的图像](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image8.png))

启用通配符脚本映射后，你需要修改 Global.asax 文件中的路由表，以使其包括根路由。 否则，你将得到错误页图 5 中，你的应用程序的根页请求时。 你可以使用修改后的 Global.asax 文件中列出 4。

[![新项目对话框](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image9.png)

**图 5**： 缺少根路由错误 ([单击以查看实际尺寸的图像](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image10.png))

**列出 4-Global.asax （使用根路由修改）**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample4.cs)]

为 IIS 7.0 或 IIS 6.0 中启用通配符脚本映射后，你可以使用默认的路由表，如下所示的请求：

/

/ Home/Index

/Product/Details/3

/ 产品

## <a name="summary"></a>摘要

本教程的目的是说明如何使用 ASP.NET MVC 时使用较旧版本的 IIS （或在经典模式下的 IIS 7.0）。 我们讨论两种方法可以获取 ASP.NET 路由要使用较旧版本的 IIS： 修改默认的路由表或者创建一个通配符脚本映射。

第一个选项要求你修改 ASP.NET MVC 应用程序中使用的 Url。 此第一个选项的一个非常重要优点是，不需要访问 web 服务器的权限才能修改路由表。 这意味着，你可以使用此第一个选项，即使承载的 ASP.NET MVC 应用程序的 Internet 托管公司。

第二个选项是创建通配符脚本映射。 此第二个选项的优点是不需要修改你的 Url。 此第二个选项的缺点是它可以影响 ASP.NET MVC 应用程序的性能。

>[!div class="step-by-step"]
[下一篇](using-asp-net-mvc-with-different-versions-of-iis-vb.md)
