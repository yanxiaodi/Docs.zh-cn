---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: "如何： 将移动页面添加到 ASP.NET Web 窗体 / MVC 应用程序 |Microsoft 文档"
author: rick-anderson
description: "本介绍通过多种方式来提供针对 ASP.NET Web 窗体中的移动设备进行了优化的页面 / MVC 应用程序，并提供的建议体系结构和..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2011
ms.topic: article
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: aac359b26c508784793a67260dc2e65c30db687a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>如何： 将移动页面添加到 ASP.NET Web 窗体 / MVC 应用程序
====================
> **适用于**
> 
> - ASP.NET Web 窗体版本 4.0
> - ASP.NET MVC 版本 3.0
> 
> **摘要**
> 
> 本介绍通过多种方式来提供针对 ASP.NET Web 窗体中的移动设备进行了优化的页面 / MVC 应用程序和提供的建议体系结构和设计时目标大范围的设备要考虑的问题。 为什么从 ASP.NET 2.0 到 3.5 ASP.NET 移动控件现已过时，并讨论某些现代的替代方法，还介绍了此文档。


## <a name="contents"></a>内容

- 概述
- 体系结构选项
- 浏览器和设备的检测
- ASP.NET Web 窗体应用程序可以如何提供移动特定页
- ASP.NET MVC 应用程序可以如何提供移动特定页
- 其他资源

有关 ASP.NET Web 窗体和 MVC 演示这份白皮书的技术的可下载代码示例，请参阅[移动应用和使用 ASP.NET 站点](https://docs.microsoft.com/aspnet/mobile/overview)。

## <a name="overview"></a>概述

移动设备 – 智能手机、 功能手机和平板电脑 – 不断增大受欢迎程度作为一种方法访问 Web。 对于许多 web 开发人员和面向 web 的企业，这意味着越来越，务必为访客使用这些设备提供了很好的浏览体验。

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>如何早期版本的 ASP.NET 支持移动浏览器

ASP.NET 版本 2.0 到 3.5 包含*ASP.NET 移动控件*： 一组中的移动设备的服务器控件*System.Web.Mobile.dll*程序集和*System.Web.UI.MobileControls*命名空间。 程序集仍包含在 ASP.NET 4 中，但它已弃用。 开发人员建议将迁移到更现代的方法，如本白皮书中的描述。

ASP.NET 移动控件为什么已标记为过时的原因是它们的设计面向盛行围绕 2005年和更早版本的手机。 控件被主要设计用于呈现 WML 或 cHTML 标记 （而不是正则 HTML) 为该纪元的 WAP 浏览器。 但 WAP、 WML 和 cHTML 不再适用于大多数项目，因为 HTML 现在变得无处不在标记语言中的移动和桌面浏览器相似。

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>目前支持移动设备的面临的挑战

即使移动浏览器现在几乎普遍支持 HTML，仍将面临着许多挑战，旨在创建极佳移动浏览体验时：

- ***屏幕大小***-在表单中，极大地改变的移动设备，并且其屏幕通常有很多小于桌面监视器。 因此，你可能需要为其设计完全不同的页面布局。
- ***输入方法***– 某些设备具有键盘、 一些具有 styluses、 其他人使用触摸屏输入。 你可能需要考虑多个导航机制和数据的输入的方法。
- ***你遵从标准***– 许多移动浏览器不支持的最新的 HTML、 CSS 或 JavaScript 标准。
- ***带宽***– 移动电话网络数据网络性能会发生变化巨大，和一些最终用户是对关税收费兆字节。

没有任何通用型解决方案;你的应用程序将需要的外观和行为以不同的方式根据访问它的设备。 具体取决于你想何种级别的移动，这可能是支持的为 web 开发人员比以往桌面"浏览器大战"一个更大难题。

通常最初接近的第一次移动浏览器支持的开发人员认为它是仅重要支持最新且最复杂的智能手机 （例如，Windows Phone 7、 iPhone 或 Android），可能是因为开发人员通常个人拥有此类设备。 但是，更便宜手机仍然非常受欢迎，而且其所有者并使用它们来浏览网页-尤其是在移动电话是更轻松地获得比宽带连接的国家/地区。 你的业务需要决定哪个范围的设备，以支持通过考虑其可能的客户。 如果你正在生成 luxury 运行状况 spa 在线小册子，可能实现的业务决定仅面向高级智能手机，而如果你要创建影院票证预订系统，你可能需要考虑为访客使用较不强大的功能手机。

## <a name="architectural-options"></a>体系结构选项

我们获取的 ASP.NET Web 窗体或 MVC 的特定技术详细信息之前，请注意 web 开发人员通常有三个主要的可能选项，用于支持移动浏览器：

1. ***不执行任何操作 –***你只需创建标准的、 面向桌面的 web 应用程序，并依赖于移动浏览器是可以接受呈现。 

    - **利用**： 它是最便宜的选择实现并维护 – 不额外的工作
    - **缺点**： 提供的最差的最终用户体验： 

        - 最新的智能手机作为桌面浏览器中，同样可能会导致你 HTML 但来缩放和滚动水平和垂直来使用你在小屏幕上的内容，仍会强制用户。 这是远离最佳。
        - 较旧的设备和功能手机可能无法令人满意的方式呈现你的标记。
        - 即使在最新的平板电脑设备 （其屏幕可以是便携式计算机屏幕一样大），不同的交互规则适用。 触摸基于输入最适用于较大的按钮和链接 spread 进一步分离，并且没有没有办法将鼠标光标鼠标悬停在弹出菜单。
2. ***解决客户端上的问题*–**与小心使用 CSS 和[渐进增强](http://en.wikipedia.org/wiki/Progressive_enhancement)标记、 样式和适应任何浏览器运行这些脚本可以创建。 例如，对于[CSS 3 媒体查询](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/)，你可以创建在屏幕的比的所选阈值窄的设备将变为单个列布局的多列布局。 

    - **利用**： 优化使用，即使对于根据它们具有所需的屏幕，输入的特征的未知未来设备中的特定设备的呈现
    - **利用**： 你在所有设备类型 – 最小重复代码或工作量之间共享服务器端逻辑，可以轻松
    - **缺点**： 移动设备是因此不同于桌面版设备，你可能确实要移动页面是完全不同于桌面页面，其中显示了不同的信息。 此类变体可能效率很低或不可能实现能够可靠地通过 CSS 单独出现时，尤其要考虑如何不一致旧设备解释 CSS 规则。 这是 CSS 3 特性的尤其如此。
    - **缺点**： 未提供对不同的服务器端逻辑和适用于不同设备的工作流支持。 例如，不能通过 CSS 单独实现的简化购物车签出工作流为移动用户。
    - **缺点**： 效率不高的带宽使用。 你的服务器可能需要传输的标记和样式的适用于所有可能的设备，即使目标设备将仅使用该信息的子集。
3. ***解决此问题在服务器上的*–**如果你的服务器知道哪些设备正在访问它 – 或在该设备，例如其屏幕大小和输入的方法的最小的特征和移动设备 – 是否它可以运行不同的逻辑和输出不同的 HTML 标记。 

    - **优点：**最大灵活性。 没有任何限制到多少可以优化你的标记中的所需的、 特定于设备的布局或你的服务器端逻辑的移动而有所不同。
    - **优点：**高效带宽使用。 只需将标记和目标设备要使用的样式信息传输。
    - **缺点：**有时强制工作量或代码的重复 （例如，使你创建的 Web 窗体页或 MVC 视图的类似但略有不同的副本项）。 其中可能将抽取常见逻辑到基础层或服务，但仍，你的 UI 代码或标记的某些部分可能需要复制，然后在并行中维护。
    - **缺点：**设备检测并不繁琐。 它要求的列表或已知的设备类型和其特征 （这并非始终在完全保持最新） 的数据库，并且并不能保证准确匹配每个传入请求。 本文档介绍某些选项，并且其缺陷更高版本。

若要获得最佳结果，大多数开发人员查找他们需要合并选项 (2) 和 (3)。 使用 CSS 或甚至 JavaScript 客户端上最容纳样式存在细微差异，而数据、 工作流或标记中的主要区别最有效地在服务器端代码中实现。

### <a name="this-paper-focuses-on-server-side-techniques"></a>本白皮书重点介绍服务器端技术

因为 ASP.NET Web 窗体和 MVC 是这两种主要的服务器端技术，本白皮书将重点服务器端技巧，以帮助你制作不同标记和移动浏览器的逻辑。 当然，还可以将此合并使用任何客户端技术 （例如，CSS 3 媒体查询，渐进式增强功能 JavaScript），但这是更一种 web 设计比 ASP.NET 开发。

## <a name="browser-and-device-detection"></a>浏览器和设备的检测

有关为支持移动设备的所有服务器端方法的密钥先决条件是要了解你的访问者正在使用何种设备。 实际上，甚至更好了解知道该设备的制造商和型号数*特征*的设备。 特征可能包括：

- 是移动设备？
- 输入的法 （鼠标/键盘、 触摸、 键盘、 游戏杆，...）
- 屏幕大小 （以物理方式和以像素为单位）
- 支持的介质和数据格式
- Etc.

它是更好的做法因为做出决策基于特征与型号，然后您将能够更好地处理的未来设备。

### <a name="using-aspnets-built-in-browser-detection-support"></a>使用 ASP。NET 的内置浏览器检测支持

ASP.NET Web 窗体和 MVC 的开发人员可以立即发现正在访问的浏览器的重要特征，通过检查属性*Request.Browser*对象。 有关示例，请参阅

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ...以及许多其他

在后台，ASP.NET 平台之一匹配传入*用户代理*针对一组浏览器定义 XML 文件中的正则表达式 (UA) HTTP 标头。 默认情况下该平台包括很多常见的移动设备的定义，你可以为其他你想要识别添加自定义浏览器定义文件。 有关更多详细信息，请参阅 MSDN 页面[ASP.NET Web 服务器控件和浏览器功能](https://msdn.microsoft.com/library/x3k2ssx2.aspx)。

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>使用通过 51Degrees.mobi Foundation WURFL 设备数据库

尽管 ASP。NET 的内置浏览器检测支持将足以满足许多应用程序，有两种主要的情况下它可能无法满足：

- ***你想要识别最新的设备***（无需手动为其创建浏览器定义文件）。 请注意，.NET 4 的浏览器定义文件不不够新，无法识别 Windows Phone 7、 Android 手机、 Opera Mobile 浏览器或 Apple Ipad。
- ***你需要有关设备功能的更多详细的信息***。 你可能需要了解的有关设备的输入方法 （例如，触摸 vs 键盘），或哪些音频格式浏览器支持。 在标准的浏览器定义文件中，此信息不可用。

[*无线通用资源文件*(WURFL) 项目](http://wurfl.sourceforge.net/)维护更多最新信息和详细信息中使用的移动设备有关今天。

为.NET 开发人员令人欣慰是该 ASP。NET 的浏览器检测功能是可扩展的，因此可以对它若要解决这些问题进行增强。 例如，你可以添加开放源代码[ *51Degrees.mobi Foundation* ](https://github.com/51Degrees/dotNET-Device-Detection)到你的项目的库。 它是一个 ASP.NET IHttpModule 或浏览器功能提供程序 （Web 窗体和 MVC 应用程序可用），直接读取 WURFL 数据并将其挂钩到 ASP。NET 的内置浏览器检测机制。 一旦你已安装模块， *Request.Browser*突然将包含大量更准确的详细信息： 它将正确识别很多前面提到的设备，并列出其功能 （包括其他功能，例如输入法）。 请参阅更多详细信息的项目的文档。

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Web 窗体应用程序可以如何提供移动特定页

默认情况下，下面是一个全新的 Web 窗体应用程序在常见的移动设备上的显示方式：

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

显然，既不布局看上去非常适合移动应用 – 此页旨在用于大型、 面向布局的监视器，不能为小面向纵向的屏幕。 因此你可以做什么信息？

如之前在本白皮书中所述，有多种方法来定制移动设备的页面。 一些技术都是基于服务器的其他人在客户端上运行。

### <a name="creating-a-mobile-specific-master-page"></a>创建移动特定的母版页

根据你的要求，你可能能够使用相同的 Web 窗体的所有访问者，但具有两个单独的主控页： 一个用于桌面的访问者，另一个为移动的访客。 这允许你灵活地更改 CSS 样式表或你顶级的 HTML 标记，以满足而不强制你要复制的任何页逻辑的移动设备。

这是很简单的操作。 例如，你可以将如下所示的 PreInit 处理添加到 Web 窗体：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

现在，创建母版页调用 Mobile.Master 顶级文件夹中的应用程序，并检测到移动设备时，将使用它。 如有必要，移动母版页可以引用特定于移动的 CSS 样式表。 桌面的访问者将看到你默认主页面上，不是移动答案。

### <a name="creating-independent-mobile-specific-web-forms"></a>创建独立移动特定 Web 窗体

最大的灵活性，你可以更进一步比单纯单独的主控页，针对不同设备类型。 你可以实现两个*完全分隔的 Web 窗体页面的集*– 一个为桌面浏览器设置，另一套用于移动。 这最适用如果你想要移动的访问者向提供非常不同的信息或工作流。 本部分的其余部分介绍详细的这种方法。

假设你已有设计用于桌面浏览器的 Web 窗体应用程序，旨在创建一个在项目中，称为"Mobile"子文件夹和生成移动页面存在的最简单的方法，以继续。 您可以构造整个子站点，具有其自己的主控页、 样式表和页，使用的所有相同你将用于任何其他 Web 窗体应用程序的技术。 你不一定需要生成移动等效*每个*桌面站点中的页上，你可以选择何种功能子集合理的移动访问者。

移动页面可以共享公共静态资源 (如图像、 JavaScript 或 CSS 文件) 与常规页面如果你想。 由于你"Mobile"的文件夹将*不*标记作为单独的应用程序时 （它是只需简单的子文件夹 Web 窗体页） 的 IIS 中承载它将也是共享所有相同配置、 会话数据和其他基础结构，即你桌面的页数。

> [!NOTE]
> 由于这种方法通常涉及一些重复代码 （移动页面有可能与桌面页共享一些相似之处），务必分解出任何常见业务逻辑或数据访问代码到共享的基础层或服务。 否则，你将双精度的创建和维护你的应用程序的工作量。


#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>重定向到你移动页的移动访问者

通常很方便，若要仅在重定向到移动页的移动访问者*第一个*请求在其浏览会话中 （以及不在其会话中的每个请求），因为：

- 然后，你可以轻松地允许移动访问者能够访问你的桌面页，根据自己的意愿 – 只需将链接放在你将转到"桌面版本"的主页面上。 在距访客不会重定向回移动页，因为它不再第一个请求在其会话。
- 它可以避免干扰对任何动态资源 （例如，如果你具有的你的站点的桌面和移动部件会显示在 IFRAME 中或某些 Ajax 处理程序的常见 Web 窗体） 的你的站点的桌面和移动部分之间共享的请求的风险

若要执行此操作，可以将放在你重定向逻辑**会话\_启动**方法。 例如，将以下方法添加到 Global.asax.cs 文件：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>配置采用了移动页的 Forms 身份验证

请注意窗体身份验证使进行某些假设，它可以将重定向访客期间和之后的身份验证过程：

- 当用户需要进行身份验证时，窗体身份验证将他们重定向到您的桌面登录页，而不考虑它们是否是桌面或移动用户 (因为它仅有的概念*一个*登录 URL)。 假设你想要以不同方式样式对移动登录页，需要增强您的桌面登录页，以便将移动用户重定向到单独移动登录页。 例如，以下将代码添加到你**桌面**隐藏代码的登录页： 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- 用户成功登录后，窗体身份验证将默认情况下将它们重定向到桌面的主页 (因为它仅有的概念*一个*默认页)。 你需要增强您的移动登录页，以便它将重定向到移动主页后成功日志中。 例如，以下将代码添加到你**移动**隐藏代码的登录页： 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
 此代码假定你的页面具有登录名的服务器控件调用 LoginUser，如下所示的默认项目模板。

### <a name="working-with-output-caching"></a>使用输出缓存

如果你使用的输出缓存，请注意，默认情况下，它是桌面的用户可以访问 （从而导致其输出缓存） 的某些 URL 后, 跟然后接收缓存的桌面输出移动用户。 你只是改变你母版页按设备类型，或者实现每个设备类型的完全独立 Web 窗体是否将应用此警告。

若要避免此问题，你可以指示 ASP.NET 改变根据访问者是否正在使用移动设备的缓存项。 将 VaryByCustom 参数添加到页面的 OutputCache 声明，如下所示：

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

接下来，定义*isMobileDevice*作为自定义缓存通过添加以下方法的参数重写到 Global.asax.cs 文件：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

这将确保移动到页的访问者没有收到以前通过桌面访问者放入缓存的输出。

### <a name="a-working-example"></a>获得可运行示例

若要查看这些技术在操作中的，下载[这份白皮书的代码示例](https://docs.microsoft.com/aspnet/mobile/overview)。 Web 窗体示例应用程序自动重移动用户定向到一组称为移动的子文件夹中的移动特定页面。 可从以下屏幕截图中看到针对移动浏览器中，更好地优化的标记和这些页面的样式：

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

有关针对移动浏览器进行优化您的标记和 CSS 的更多技巧，请参阅"样式移动页的移动浏览器"在本文档后面的部分。

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>ASP.NET MVC 应用程序可以如何提供移动特定页

由于模型-视图-控制器模式将脱耦 （在控制器） 的应用程序逻辑与表示逻辑 （在视图中），你可以从任何以下方法来处理服务器端代码中的移动支持：

1. ***对于桌面版和移动浏览器，使用相同的控制器和视图，但呈现的视图有不同的 Razor 布局，具体取决于设备类型 *。** 如果你要在所有设备上显示相同的数据，但只是想要提供不同的 CSS 样式表或更改为移动的几个顶级 HTML 元素，此选项效果最佳。
2. ***对于桌面版和移动浏览器，使用同一个控制器，但呈现根据设备类型的不同视图***。 如果是显示大致相同的数据并提供相同的工作流的最终用户，此选项效果最佳，但想要呈现非常不同的 HTML 标记，以满足当前使用的设备。
3. ***创建桌面和移动浏览器，每个实现独立控制器和视图的单独区域 *。** 如果你显示非常不同的屏幕，包含不同的信息和前导用户通过优化为其设备类型的不同工作流，此选项效果最佳。 这可能意味着一些重复代码，但你也可以通过将出常见逻辑分解到基础层或服务对的最小化。

如果你想要利用**第一个**选项和变化仅 Razor 布局每种设备类型，它是非常简单。 只需修改你\_ViewStart.cshtml 文件，如下所示：

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

现在，你可以创建移动特定布局调用\_LayoutMobile.cshtml 与页结构和 CSS 规则针对移动设备进行了优化。

如果你想要利用**第二个**选项呈现器根据访问者的设备类型的完全不同视图，请参阅[Scott Hanselman 的博客文章](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx)。

本文的其余部分侧重于**第三个**选项 – 创建单独的控制器*和*视图适用于移动设备 – 以便您可以控制完全为移动的访客提供哪些功能的子集。

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>ASP.NET MVC 应用程序中的移动区域设置

你可以添加以常规方式称为"移动"到现有的 ASP.NET MVC 应用程序的区域： 右键单击你的项目名称，在解决方案资源管理器，然后选择添加 à 区域。 根据需要对 ASP.NET MVC 应用程序内的任何其他区域，然后可以添加控制器和视图。 例如，将添加到你移动区域调用 HomeController 作为移动访客主页的新控制器。

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>确保 URL /Mobile 达到移动主页

如果你想 URL /Mobile 来访问上 HomeController 移动区域内的索引操作，你将需要对路由配置进行以下两个小型更改。 首先，更新 MobileAreaRegistration 类以便 HomeController 默认控制器你移动所在区域，如下面的代码中所示：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

这意味着移动主页现在将在 /Mobile，而不是/移动/主页，位于因为"主页"现在为隐式移动页面的默认控制器名称。

接下来，请注意，通过将第二个 HomeController 添加到你的应用程序 （即，移动一个，除了现有桌面一个），你将破坏了你正则桌面主页。 它将失败并出现错误"*找到多个类型符合名为主页控制器*"。 若要解决此问题，请更新顶层路由配置 （在 Global.asax.cs) 指定歧义时，你的桌面 HomeController 的执行优先级：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

现在该错误会离开和 URL http://*yoursite*/ 将到达的桌面主页和 http://*yoursite*/mobile/ 将到达移动主页。

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>重定向到你移动区域的移动访问者

有许多不同的扩展点在 ASP.NET MVC 中，因此有许多可能的方法，以插入重定向逻辑。 一种很不错的选择是创建一个筛选器属性，[RedirectMobileDevicesToMobileArea]，执行的重定向，如果满足以下条件：

1. 它是用户的会话中的第一个请求 （即，Session.IsNewSession 等于 true）
2. 请求来自移动浏览器 （即，Request.Browser.IsMobileDevice 等于 true）
3. 用户未在已请求的移动区域中的资源 (即，*路径*URL 的一部分不会以开头 /Mobile)

随附这份白皮书的可下载示例包括此逻辑的实现。 它作为授权筛选器，从 AuthorizeAttribute，这意味着它可以正确起作用，即使你使用输出缓存 （否则为如果桌面的访问者的第一个访问某些 URL，桌面输出可能缓存，然后提供给派生实现后续移动访问者）。

由于它是筛选器，你可以选择将它应用到特定控制器和操作，例如，

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… 你可以将其应用于所有控制器和操作如 MVC 3 或*全局筛选器*Global.asax.cs 文件中：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

可下载的示例还演示如何创建此特性将重定向到你移动区域中的特定位置的子类。 例如，这意味着你可以：

- 注册全局筛选器如下所示上述默认情况下发送到移动主页的移动访问者。
- 也适用于采用移动到具有请求的任何产品页面的移动版本的访问者的"查看产品"操作的特殊 [RedirectMobileDevicesToMobileProductPage] 筛选器。
- 此外将筛选器的其他特殊子类应用于特定操作，将重定向到等效的移动页的移动访问者

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>配置采用了移动页的 Forms 身份验证

如果你使用的窗体身份验证，则应注意，当用户需要登录时，它自动将用户重定向到单个特定"登录"的 URL，即默认情况下**/帐户/登录**。 这意味着，移动用户可能会重定向到你的桌面"登录"操作。

若要避免此问题，请向你的桌面"登录"操作添加逻辑，以便它将重定向移动用户再次到"登录"的移动操作。 如果你使用的默认 ASP.NET MVC 应用程序模板，更新 AccountController 的登录操作，如下所示：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… 然后在你移动所在区域调用 AccountController 控制器上实现合适的移动特定"登录"操作。

### <a name="working-with-output-caching"></a>使用输出缓存

如果你使用 [OutputCache] 筛选器，必须强制执行要根据设备类型而变化的缓存项。 例如，编写：

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

然后，将以下方法添加到 Global.asax.cs 文件：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

这将确保移动到页的访问者没有收到以前通过桌面访问者放入缓存的输出。

### <a name="a-working-example"></a>获得可运行示例

若要查看这些技术在操作中的，下载[这份白皮书的代码的关联示例](https://docs.microsoft.com/aspnet/mobile/overview)。 此示例包括增强，从而支持使用上面所述的方法的移动设备的 ASP.NET MVC 3 （候选发布版） 应用程序。

## <a name="further-guidance-and-suggestions"></a>更多指导和建议

下面的讨论适用于 Web 窗体和 MVC 的开发人员使用本文档中阐述的技术。

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>提高使用 51Degrees.mobi Foundation 你重定向逻辑

此文档中所示的重定向逻辑可能会完全能够满足你的应用程序，但它不起作用，如果你需要禁用会话，或与拒绝的 cookie （这些不能具有会话），因为它不会知道是否给定请求的移动浏览器从该访问者选择第一个。

你已学习了如何开放源代码 51Degrees.mobi Foundation 可以提高 ASP 的准确性。NET 的浏览器检测。 它还具有一项内置功能来重定向到在 Web.config 中配置的特定位置的移动访问者。能够根据 ASP.NET 会话正常 (并因此 cookie) 通过将存储的访问者的 HTTP 标头和 IP 地址的哈希的临时日志，以便它知道每个请求是否是从给定 vistor 的第一个。

下面的元素添加到 web.config 文件的 fiftyOne 节将重定向到页从检测到的移动设备的第一个请求 ~ / Mobile/Default.aspx。 移动文件夹下的页的任何请求将*不*被重定向，无论何种设备类型。 如果移动设备已处于非活动状态一段的 20 分钟或更多的设备将被遗忘和后续请求将被视为出于的重定向的新的。

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

有关更多详细信息，请参阅[51degrees.mobi Foundation 文档](https://github.com/51Degrees/dotNET-Device-Detection)。

> [!NOTE]
> 你*可以*使用 51Degrees.mobi Foundation 重定向功能，在 ASP.NET MVC 应用程序，但你将需要定义纯 Url，不是从路由的参数或通过将放在 MVC 筛选器根据你重定向的配置上操作。 这是因为 （在撰写本文时） 51Degrees.mobi Foundation 无法识别筛选器或路由。


### <a name="disabling-transcoders-and-proxy-servers"></a>禁用转码器和代理服务器

移动网络运营商在移动 internet 其方法中具有两个主要目标：

1. 提供作为尽可能得多相关内容
2. 最大化可以共享的有限的单选网络带宽的客户的数

由于大多数 web 页中为大型桌面尺寸屏幕和快速修复行宽带连接设计，许多运算符使用*转码器*或*代理服务器*，动态更改 web 内容。 它们可能会修改你的 HTML 标记或 CSS 以适合较小屏幕 （特别是对于"功能手机"缺少处理复杂的布局的处理能力），和它们可能重新压缩映像 （显著减少其质量） 以提高页传递速度。

但如果你已取得以生成你的站点的移动优化版本的工作，您可能不需要网络运营商干扰它任何进一步。 你可以将以下行添加到页\_任何 ASP.NET Web 窗体中的负载事件：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

或者，为 ASP.NET MVC 控制器，你可以添加以下方法重写，以便它适用于在该控制器上的所有操作：

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

生成的 HTTP 消息通知 W3C 符合转码器和代理服务器不以更改内容。 当然，没有移动网络运营商将遵守此消息能保证。

### <a name="styling-mobile-pages-for-mobile-browsers"></a>移动浏览器样式移动页

它超出了范围本文详细说明的哪些类型的 HTML 标记工作正确或哪些 web 设计技术最大化特定设备上的可用性。 该类具有设置到你可以找到足够简单布局中，优化移动大小屏幕，而无需使用不可靠 HTML 或 CSS 定位技巧。 但是，是一个重要的技术，值得一提的是，*视区元标记*。

某些现代移动浏览器，用于桌面监视器工作量显示网页中的呈现虚拟画布，也称为"视区"上的页面 （例如，虚拟视区是 980 像素宽 iPhone 上, 和 850 像素宽 Opera Mobile 在默认情况下），然后缩减结果以适合屏幕的物理像素。 然后，用户可以放大并移动该视区。 这样做的优势，它可让浏览器显示其预期的布局中的页面，但它也是具有它强制缩放和平移，其中对于用户来说不方便的缺点。 如果你设计移动，最好是设计窄屏幕，以便有必要进行没有缩放或水平滚动。

告诉移动浏览器将视区应如何宽的方法是通过非标准*视区*meta 标记。 例如，如果你将以下代码添加到页面的 HEAD 部分中，

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… 然后支持智能手机浏览器将布局 480 像素宽虚拟画布上的页面。 这意味着，如果你的 HTML 元素定义以百分比表示的宽度，百分比将解释此 480 像素宽度，不默认视区宽度方面。 因此，用户很少需要缩放和平移操作水平 – 显著提高的移动浏览体验。

如果你想要匹配设备的物理像素的视区宽度时，你可以指定以下各项：

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

为此正常工作，你必须不显式强制超过该宽度的元素 (例如，使用*宽度*属性或 CSS 属性)，否则浏览器会强制使用更大的视区而不考虑。 另请参阅：[更多详细信息使用了非标准的视区标记](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html)。

大多数现代智能手机支持*双重方向*： 它们可以保存在纵向或横向模式中。 因此，务必不假设的屏幕宽度 （像素）。 不甚至假设，修复后的屏幕宽度，因为用户可以重新定向其设备时它们位于你的页面。

通知他们内容页标题中的以下元标记已经过优化，适用于移动，因此不应转换，也可接受较旧的 Windows Mobile 和 Blackberry 设备。

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>其他资源

有关移动设备仿真程序和可用于测试移动的 ASP.NET web 应用程序的列表，请参阅页面[模拟常用的移动设备以进行测试](../mobile/device-simulators.md)。

## <a name="credits"></a>信用

- 主要作者： Steven Sanderson
- 审阅者 / 其他内容编写器： James Rosewell、 Mikael Söderström、 Scott Hanselman、 Scott 搜寻
