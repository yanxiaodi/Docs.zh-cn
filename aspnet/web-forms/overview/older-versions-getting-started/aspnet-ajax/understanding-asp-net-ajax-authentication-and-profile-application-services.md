---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: "了解 ASP.NET AJAX 身份验证和配置文件应用程序服务 |Microsoft 文档"
author: scottcate
description: "身份验证服务允许用户提供凭据才能接收身份验证 cookie，并为该网关服务以便进行自定义用户..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: 182276f9f91b99beb1ce0fc40dcda1f19376669a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>了解 ASP.NET AJAX 身份验证和配置文件应用程序服务
====================
通过[Scott 类别](https://github.com/scottcate)

[下载 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> 身份验证服务允许用户提供凭据才能接收身份验证 cookie，并由 ASP.NET 提供的网关服务，以允许自定义用户配置文件。 ASP.NET AJAX 身份验证服务的使用是标准 ASP.NET 窗体身份验证，与兼容的因此应用程序当前使用窗体身份验证 （如使用的登录名控制） 将不会中断通过升级到 AJAX 身份验证服务。


## <a name="introduction"></a>介绍

作为.NET Framework 3.5 的一部分，Microsoft 提供了相当多的环境升级报告中。不只是一个新的开发环境可用，但新的语言集成查询 (LINQ) 功能和其他语言增强功能即将推出。 此外，正在作为.NET Framework 基类库的第一类成员包含的其他工具集，值得注意的是 ASP.NET AJAX Extensions 中，一些常见的功能。 这些扩展使许多新的丰富的客户端功能，包括部分呈现页，而无需完整的页面刷新，能够通过 （包括 ASP.NET 分析 API） 的客户端脚本和广泛的客户端 API 访问 Web 服务用于镜像许多 ASP.NET 服务器端控件集中所示的控件方案。

本白皮书审视的实现和使用 ASP.NET 分析的并窗体身份验证服务当它们公开的 Microsoft ASP.NET AJAX ExtensionsThe AJAX Extensions 使窗体身份验证非常轻松地提供支持，因为它 （以及分析服务中） 通过 Web 服务代理脚本公开。 AJAX Extensions 还支持通过 AuthenticationServiceManager 类的自定义身份验证。

此白皮书基于 Visual Studio 2008 Beta 2 版本和.NET Framework 3.5。 此白皮书还假定你将使用 Visual Studio 2008 Beta 2，不 Visual Web Developer Express，并且将提供根据 Visual Studio 的用户界面的演练。 一些代码示例可能利用 Visual Web Developer 学习版中不可用的项目模板。

## <a name="profiles-and-authentication"></a>*配置文件和身份验证*

Microsoft ASP.NET 配置文件和身份验证服务由 ASP.NET 窗体身份验证系统，并且是标准的 ASP.NET 组件。 使用 ASP.NET AJAX Extensions 提供对这些服务通过脚本代理，通过在客户端 AJAX 库的 Sys.Services 命名空间下模型非常简单的脚本访问。

身份验证服务允许用户提供凭据才能接收身份验证 cookie，并由 ASP.NET 提供的网关服务，以允许自定义用户配置文件。 ASP.NET AJAX 身份验证服务的使用是标准 ASP.NET 窗体身份验证，与兼容的因此应用程序当前使用窗体身份验证 （如使用的登录名控制） 将不会中断通过升级到 AJAX 身份验证服务。

配置文件服务允许自动集成和根据成员资格，身份验证服务所提供的用户数据的存储。 Web.config 文件中，指定存储的数据和各种的分析服务提供程序处理的数据管理。 与身份验证服务，AJAX 配置文件服务适用于标准 ASP.NET 配置文件服务，以便不应由于包括 AJAX 支持中断当前特有的功能的 ASP.NET 配置文件服务的页。

并入应用程序的 ASP.NET 身份验证和分析服务本身是本白皮书的范围之外。 有关主题的详细信息，请参阅 MSDN 库引用文章在使用成员资格管理用户[https://msdn.microsoft.com/library/tw292whz.aspx](https://msdn.microsoft.com/library/tw292whz.aspx)。 ASP.NET 还包括一个实用工具来自动将使用 SQL Server，这是默认身份验证服务提供程序的 ASP.NET 成员身份的成员资格设置。 有关详细信息，请参阅文章 ASP.NET SQL 服务器注册工具 (Aspnet\_regsql.exe) 在[https://msdn.microsoft.com/library/ms229862(vs.80).aspx](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*使用 ASP.NET AJAX 身份验证服务*

必须在 web.config 文件中启用 ASP.NET AJAX 身份验证服务：

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

身份验证服务都需要 ASP.NET 窗体身份验证启用并且需要在客户端浏览器 （一个脚本无法启用无 cookie 会话由于无 cookie 会话需要 URL 参数） 上启用的 cookie。

一旦启用了 AJAX 身份验证服务，并将其配置上，客户端脚本可以立即充分利用 Sys.Services.AuthenticationService 对象。 主要的是，客户端脚本将想要利用`login`方法和`isLoggedIn`属性。 存在几个属性对于登录方法，可以接受的参数数量大提供默认值。

*Sys.Services.AuthenticationService 成员*

*login 方法：*

Login （） 方法开始的请求进行身份验证用户的凭据。 此方法是异步的不会阻止执行。

*参数：*

| **参数名称** | **含义** |
| --- | --- |
| userName | 必须的。 要进行身份验证的用户名。 |
| 密码 | 可选 （默认值为 null）。 用户的密码。 |
| isPersistent | 可选 （默认值为 false）。 是否在会话之间应保持用户的身份验证 cookie。 如果为 false，则关闭浏览器或会话过期时用户将可以注销。 |
| redirectUrl | 可选 （默认值为 null）。要为身份验证成功后的将浏览器重定向的 URL。 如果此参数为 null 或空字符串，则会不发生任何重定向。 |
| customInfo | 可选 （默认值为 null）。 此参数是当前未使用，保留待将来使用。 |
| loginCompletedCallback | 可选 （默认值为 null）。要在该登录名已成功完成时调用的函数。 如果指定，此参数将取代 defaultLoginCompleted 属性。 |
| failedCallback | 可选 （默认值为 null）。要在登录失败时调用的函数。 如果指定，此参数将取代 defaultFailedCallback 属性。 |
| userContext | 可选 （默认值为 null）。 应传递给回调函数的自定义用户上下文数据。 |

*返回值：*

此函数不包括返回值。 但是，有多种行为都是调用的在完成对此函数包括：

- 当前页将被刷新，或者如果更改`redirectUrl`参数已既不为 null，也不为空字符串。
- 但是，如果参数为 null 或空字符串，`loginCompletedCallback`参数，或`defaultLoginCompletedCallback`调用属性。
- 如果对 web 服务调用失败，`failedCallback`参数`defaultFailedCallback`调用属性。

*注销方法：*

Logout （） 方法将移除凭据 cookie，并注销当前用户从 web 应用程序。

*参数：*

| **参数名称** | **含义** |
| --- | --- |
| redirectUrl | 可选 （默认值为 null）。要为身份验证成功后的将浏览器重定向的 URL。 如果此参数为 null 或空字符串，则会不发生任何重定向。 |
| logoutCompletedCallback | 可选 （默认值为 null）。用于注销已成功完成时调用的函数。 如果指定，此参数将取代 defaultLogoutCompleted 属性。 |
| failedCallback | 可选 （默认值为 null）。要在登录失败时调用的函数。 如果指定，此参数将取代 defaultFailedCallback 属性。 |
| userContext | 可选 （默认值为 null）。 应传递给回调函数的自定义用户上下文数据。 |

*返回值：*

此函数不包括返回值。 但是，有多种行为都是调用的在完成对此函数包括：

- 当前页将被刷新，或者如果更改`redirectUrl`参数已既不为 null，也不为空字符串。
- 但是，如果参数为 null 或空字符串，`logoutCompletedCallback`参数，或`defaultLogoutCompletedCallback`调用属性。
- 如果对 web 服务调用失败，`failedCallback`参数`defaultFailedCallback`调用属性。

*defaultFailedCallback 属性 （get、 组）：*

此属性指定如果失败与 web 服务进行通信，则应调用的函数。 它应接收的委托 （或函数引用）。

此属性指定的函数引用应具有以下签名：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*参数：*

| **参数名称** | **含义** |
| --- | --- |
| 错误 | 指定的错误的信息。 |
| userContext | 指定提供了登录或注销函数调用时的用户上下文信息。 |
| 方法名称 | 调用方法的名称。 |

*defaultLoginCompletedCallback 属性 （get、 组）：*

此属性指定应在完成登录 web 服务调用时调用的函数。 它应接收的委托 （或函数引用）。

此属性指定的函数引用应具有以下签名：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*参数：*

| **参数名称** | **含义** |
| --- | --- |
| validCredentials | 指定用户是否提供有效凭据。 `true`如果用户成功登录;否则为`false`。 |
| userContext | 指定提供调用 login 函数时的用户上下文信息。 |
| 方法名称 | 调用方法的名称。 |

*defaultLogoutCompletedCallback 属性 （get、 组）：*

此属性指定应完成注销 web 服务调用时调用的函数。 它应接收的委托 （或函数引用）。

此属性指定的函数引用应具有以下签名：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*参数：*

| **参数名称** | **含义** |
| --- | --- |
| result | 此参数将始终为`null`; 它保留供将来使用。 |
| userContext | 指定提供调用 login 函数时的用户上下文信息。 |
| 方法名称 | 调用方法的名称。 |

*isLoggedIn 属性 (get):*

此属性获取用户; 的当前身份验证状态它在页请求期间由 ScriptManager 对象设置。

此属性返回`true`如果用户当前登录; 否则为它将返回`false`。

*路径属性 （get、 组）：*

此属性以编程方式确定身份验证 web 服务的位置。 它可以用于重写默认的身份验证提供程序，以及一个在 ScriptManager 控件的认证子节点的路径属性中以声明方式设置 (有关详细信息，请参阅使用自定义身份验证服务提供程序下面的主题）。

请注意，默认身份验证服务的位置不会更改。 但是，ASP.NET AJAX，可指定 web 服务提供相同的类接口的 ASP.NET AJAX 身份验证服务代理的位置。

另请注意，此属性应不设置为一个值，可将脚本请求定向从当前站点中移出。 当前应用程序将不会收到的身份验证凭据，因为它可能会毫无用处;此外，技术基础 AJAX 不应该发布跨站点请求，并可能在客户端浏览器中生成安全性异常。

此属性是`String`到身份验证 web 服务中表示该路径的对象。

*超时属性 （get、 组）：*

此属性确定失败的总时间，假定该登录请求之前等待的身份验证服务。 如果在超时到期等待调用完成时，将调用的请求失败的回调，并调用将无法完成。

此属性是`Number`对象表示要等待的时间从身份验证服务结果的毫秒数。

*代码示例： 登录到身份验证服务*

以下标记是一个示例 ASP.NET 页通过简单的脚本调用认证类的登录和注销方法。

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>访问的 ASP.NET 分析数据通过 AJAX

通过使用 ASP.NET AJAX Extensions 还公开的 ASP.NET 分析服务。 由于 ASP.NET 分析服务提供丰富的细粒度的 API，用于存储和检索用户数据，这可以是一个很好的工作效率工具。

必须在 web.config; 中启用配置文件服务它不是默认情况下。 为此，请确保`profileService`子元素已启用 = true 指定在 web.config 文件中，并指定哪些属性可以读取或写入，如下所示：

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

此外必须配置配置文件服务。 尽管本白皮书的范围之外是分析服务的配置，但很值得注意的是在配置文件配置设置中定义的组将组名的子属性的可访问性。 例如，使用指定的以下配置文件部分：

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

客户端脚本将可以访问作为页面类的属性字段的属性的名称、 Address.Line1、 Address.Line2、 Address.City、 Address.State、 Address.Zip 和 BackgroundColor。

AJAX 分析服务配置后，它可立即在页面; 例如：但是，它将需要在使用前一次加载。

*Sys.Services.ProfileService 成员*

*属性字段：*

属性字段将所有配置的配置文件数据公开为可由.运算符名约定引用的子属性。 属性组的子级的属性称为 GroupName.PropertyName。 在上面介绍的示例配置文件配置中，以获取状态的用户，你可以使用以下标识符：

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*load 方法：*

从服务器加载选定的列表或所有属性。

*参数：*

| **参数名称** | **含义** |
| --- | --- |
| propertyNames | 可选 （默认值为 null）。 要从服务器中加载的属性。 |
| loadCompletedCallback | 可选 （默认值为 null）。 要在加载已完成时调用的函数。 |
| failedCallback | 可选 （默认值为 null）。 要在出错时调用的函数。 |
| userContext | 可选 （默认值为 null）。 要传递给回调函数的上下文信息。 |

加载函数没有返回值。 如果已成功完成的调用，它将调用`loadCompletedCallback`参数或`defaultLoadCompletedCallback`属性。 如果调用失败，或在超时时间已`failedCallback`参数或`defaultFailedCallback`将会调用此属性。

如果`propertyNames`未提供参数，从服务器检索所有读取配置的属性。

*save 方法：*

Save （） 方法将指定的属性列表 （或所有属性） 保存到用户的 ASP.NET 配置文件。

*参数：*

| **参数名称** | **含义** |
| --- | --- |
| propertyNames | 可选 （默认值为 null）。 要保存到服务器的属性。 |
| saveCompletedCallback | 可选 （默认值为 null）。 要在保存时调用的函数已完成。 |
| failedCallback | 可选 （默认值为 null）。 要在出错时调用的函数。 |
| userContext | 可选 （默认值为 null）。 要传递给回调函数的上下文信息。 |

保存函数没有返回值。 如果调用成功完成，它将调用`saveCompletedCallback`参数或`defaultSaveCompletedCallback`属性。 如果调用失败，或在超时时间已`failedCallback`或`defaultFailedCallback`将会调用此属性。

如果`propertyNames`参数为 null，所有配置文件属性将发送到服务器，且服务器将确定哪些属性可以保存和哪些不能。

*defaultFailedCallback 属性 （get、 组）：*

此属性指定如果失败与 web 服务进行通信，则应调用的函数。 它应接收的委托 （或函数引用）。

此属性指定的函数引用应具有以下签名：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*参数：*

| **参数名称** | **含义** |
| --- | --- |
| Error | 指定的错误的信息。 |
| userContext | 指定时提供的用户上下文信息负载或者保存函数进行了调用。 |
| 方法名称 | 调用方法的名称。 |

*defaultSaveCompleted 属性 （get、 组）：*

此属性指定应在完成保存用户的配置文件数据时调用的函数。 它应接收的委托 （或函数引用）。

此属性指定的函数引用应具有以下签名：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*参数：*

| **参数名称** | **含义** |
| --- | --- |
| numPropsSaved | 指定已保存的属性的数目。 |
| userContext | 指定时提供的用户上下文信息负载或者保存函数进行了调用。 |
| 方法名称 | 调用方法的名称。 |

*defaultLoadCompleted 属性 （get、 组）：*

此属性指定应用户的配置文件数据的加载完成后调用的函数。 它应接收的委托 （或函数引用）。

此属性指定的函数引用应具有以下签名：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*参数：*

| **参数名称** | **含义** |
| --- | --- |
| numPropsLoaded | 指定加载的属性数目。 |
| userContext | 指定时提供的用户上下文信息负载或者保存函数进行了调用。 |
| 方法名称 | 调用方法的名称。 |

*路径属性 （get、 组）：*

此属性以编程方式确定配置文件的 web 服务的位置。 它可以用于重写默认配置文件服务提供程序，以及一个 ScriptManager 控件的页面的子节点的路径属性中以声明方式设置。

请注意，默认配置文件服务的位置不会更改。 但是，ASP.NET AJAX，可指定 web 服务提供相同的类接口的 ASP.NET AJAX 身份验证服务代理的位置。

另请注意，此属性应不设置为一个值，可将脚本请求定向从当前站点中移出。 技术基础 AJAX 不应该发布跨站点请求，并可能在客户端浏览器中生成安全性异常。

此属性是`String`到配置文件的 web 服务中表示该路径的对象。

*超时属性 （get、 组）：*

此属性确定失败的总时间假设负载之前，等待配置文件服务或保存请求。 如果在超时到期等待调用完成时，将调用的请求失败的回调，并调用将无法完成。

此属性是`Number`对象表示要等待的时间从配置文件服务结果的毫秒数。

*代码示例： 加载在页面加载配置文件数据*

下面的代码将检查是否已验证用户，而如果是这样，它将为页面中加载用户的首选的背景色。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*使用自定义身份验证服务提供程序*

ASP.NET AJAX 扩展允许您通过公开你通过自定义 web 服务的功能来创建自定义脚本身份验证服务提供程序。 为了便于使用，你的 web 服务必须公开两种方法，`Login`和`Logout`; 这些方法都必须指定具有相同的方法签名，在默认 ASP.NET AJAX 身份验证 web 服务。

一旦你已创建自定义 web 服务，你将需要指定到其中，是以声明方式在页面上，路径，以编程方式在代码中，或通过客户端脚本。

*若要以声明方式设置路径：*

若要以声明方式设置的路径，包括 ASP.NET 页上的 ScriptManager 对象的认证子级：

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*在代码中设置路径：*

若要以编程方式设置的路径，指定通过脚本管理器的实例的路径：

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*在脚本中设置路径：*

若要在脚本中以编程方式设置路径，利用`path`认证类的属性：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*自定义身份验证的示例 Web 服务*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>摘要

ASP.NET 服务-专门分析、 成员身份和身份验证服务-轻松地向 JavaScript 公开在客户端浏览器上。 这允许开发人员其客户端代码使用的身份验证机制无缝集成，而无需根据控件如 UpdatePanels 如何完成的繁重任务。 可以从客户端，通过利用 web 配置设置; 保护配置文件数据不会提供默认情况下，数据和开发人员必须选择性加入到配置文件属性。

此外，通过使用等效方法签名中创建简化的 web 服务实现，开发人员可以创建自定义脚本提供程序这些内部函数的 ASP.NET 服务。 有关这些方法的支持简化了丰富的客户端应用程序，同时开发人员提供了各种各样的灵活性来满足特定需求的开发。

## <a name="bio"></a>*Bio*

Scott 类别自 1997 年以来处理与 Microsoft Web 技术，并且是 myKB.com 总裁 ([www.myKB.com](http://www.myKB.com)) 其中他专注于编写 ASP.NET 基于侧重于知识库软件解决方案的应用程序。 可以通过在电子邮件联系 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或在其博客地址[ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[上一页](understanding-asp-net-ajax-updatepanel-triggers.md)
[下一页](understanding-asp-net-ajax-localization.md)
