---
uid: whitepapers/aspnet4/overview
title: "ASP.NET 4 和 Visual Studio 2010 Web 开发概述 |Microsoft 文档"
author: rick-anderson
description: "本文档提供有关 ASP.NET 中的.net Framework 4 和 Visual Studio 2010 中包含的许多新功能的概述。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 29d5b2f4c04b899b900427ac202c0a4f57f8076f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>ASP.NET 4 和 Visual Studio 2010 Web 开发概述
====================
> 本文档提供有关 ASP.NET 中的.net Framework 4 和 Visual Studio 2010 中包含的许多新功能的概述。
> 
> [下载此白皮书](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


**内容**

**[核心服务](#0.2__Toc253429238 "_Toc253429238")**  
[Web.config 文件重构](#0.2__Toc253429239 "_Toc253429239")  
[可扩展的输出缓存](#0.2__Toc253429240 "_Toc253429240")  
[自动启动 Web 应用程序](#0.2__Toc253429241 "_Toc253429241")  
[永久重定向页面](#0.2__Toc253429242 "_Toc253429242")  
[收缩会话状态](#0.2__Toc253429243 "_Toc253429243")  
[扩展该范围允许 Url](#0.2__Toc253429244 "_Toc253429244")  
[可扩展请求验证](#0.2__Toc253429245 "_Toc253429245")  
[对象缓存和对象缓存扩展性](#0.2__Toc253429246 "_Toc253429246")  
[可扩展的 HTML、 URL 和 HTTP 标头编码](#0.2__Toc253429247 "_Toc253429247")  
[为在单个辅助进程中的单个应用程序性能监视](#0.2__Toc253429248 "_Toc253429248")  
[多目标](#0.2__Toc253429249 "_Toc253429249")

**[Ajax](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery 包含在 Web 窗体和 MVC](#0.2__Toc253429251 "_Toc253429251")  
[内容传送网络支持](#0.2__Toc253429252 "_Toc253429252")  
[ScriptManager 显式脚本](#0.2__Toc253429253 "_Toc253429253")

**[Web 窗体](#0.2__Toc253429256 "_Toc253429256")**  
[设置与 Page.MetaKeywords 和 Page.MetaDescription 属性的 Meta 标记](#0.2__Toc253429257 "_Toc253429257")  
[启用视图状态的单个控件](#0.2__Toc253429258 "_Toc253429258")  
[更改为浏览器功能](#0.2__Toc253429259 "_Toc253429259")  
[ASP.NET 4 中的路由](#0.2__Toc253429260 "_Toc253429260")  
[设置客户端 Id](#0.2__Toc253429261 "_Toc253429261")  
[保持数据控件中的行选择](#0.2__Toc253429262 "_Toc253429262")  
[ASP.NET 图表控件](#0.2__Toc253429263 "_Toc253429263")  
[筛选数据与 QueryExtender 控件](#0.2__Toc253429264 "_Toc253429264")  
[Html 编码的代码表达式](#0.2__Toc253429265 "_Toc253429265")  
[项目模板更改](#0.2__Toc253429266 "_Toc253429266")  
[CSS 改进](#0.2__Toc253429267 "_Toc253429267")  
[隐藏 div 元素周围隐藏字段](#0.2__Toc253429268 "_Toc253429268")  
[模板化控件呈现的外部表](#0.2__Toc253429269 "_Toc253429269")  
[ListView 控件增强功能](#0.2__Toc253429270 "_Toc253429270")  
[按和说明如何增强功能](#0.2__Toc253429271 "_Toc253429271")  
[菜单控件改进](#0.2__Toc253429272 "_Toc253429272")  
[向导和 CreateUserWizard 控件 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[区域支持](#0.2__Toc253429275 "_Toc253429275")  
[数据注释属性验证支持](#0.2__Toc253429276 "_Toc253429276")  
[模板化帮助器](#0.2__Toc253429277 "_Toc253429277")

**[动态数据](#0.2__Toc253429278 "_Toc253429278")**  
[启用动态数据的现有项目](#0.2__Toc253429279 "_Toc253429279")  
[声明性 DynamicDataManager 控件语法](#0.2__Toc253429280 "_Toc253429280")  
[实体模板](#0.2__Toc253429281 "_Toc253429281")  
[Url 和电子邮件地址的新字段模板](#0.2__Toc253429282 "_Toc253429282")  
[创建链接与 DynamicHyperLink 控件](#0.2__Toc253429283 "_Toc253429283")  
[对数据模型中的继承支持](#0.2__Toc253429284 "_Toc253429284")  
[对多对多关系 （仅用于实体框架） 支持](#0.2__Toc253429285 "_Toc253429285")  
[控件显示和支持枚举的新特性](#0.2__Toc253429286 "_Toc253429286")  
[对筛选器的增强支持](#0.2__Toc253429287 "_Toc253429287")

**[Visual Studio 2010 Web 开发改进](#0.2__Toc253429288 "_Toc253429288")**  
[改进的 CSS 兼容性](#0.2__Toc253429289 "_Toc253429289")  
[HTML 和 JavaScript 代码段](#0.2__Toc253429290 "_Toc253429290")  
[JavaScript IntelliSense 增强功能](#0.2__Toc253429291 "_Toc253429291")

**[Web 应用程序部署使用 Visual Studio 2010](#0.2__Toc253429292 "_Toc253429292")**  
[Web 打包](#0.2__Toc253429293 "_Toc253429293")  
[Web.config 转换](#0.2__Toc253429294 "_Toc253429294")  
[数据库部署](#0.2__Toc253429295 "_Toc253429295")  
[单击发布为 Web 应用程序](#0.2__Toc253429296 "_Toc253429296")  
[资源](#0.2__Toc253429297 "_Toc253429297")

**[免责声明](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>核心服务

ASP.NET 4 引入了大量改进输出缓存和会话状态存储等的核心 ASP.NET 服务的功能。

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>重构的 Web.config 文件

`Web.config`文件，其中包含配置有关 Web 应用程序变得相当通过过去的几个版本的.NET framework 已添加新功能，如 Ajax，如路由，以及与 IIS 7 的集成。 这得更难来配置或启动新的 Web 应用程序，而无需 Visual Studio 之类的工具。 来 NET Framework 4，主要的配置元素均已移至`machine.config`文件和应用程序现在继承这些设置。 这允许`Web.config`ASP.NET 4 应用程序可以为空，或者以包含仅的以下行，其指定 Visual Studio 的哪个版本的应用程序所面向的 framework 中的文件：

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>可扩展的输出缓存

自 ASP.NET 1.0 发布的时间，输出缓存已启用了开发人员可以在内存中存储生成的输出的页、 控件和 HTTP 响应。 在后续的 Web 请求，ASP.NET 可提供内容更快地服务通过从内存而不是重新生成从零开始的输出检索生成的输出。 但是，此方法有一个限制-生成的内容始终具有要存储在内存中，并在服务器上遇到通信流量过大，占用输出缓存的内存可以争取从 Web 应用程序的其他部分的内存需求。

ASP.NET 4 将添加一个扩展点的输出缓存，使你能够配置一个或多个自定义输出缓存提供程序。 输出缓存提供程序可以使用任何存储机制保留 HTML 的内容。 这使它能够为各种持久性机制，可以包括本地或远程磁盘，云存储，创建自定义输出缓存提供程序和分布式缓存引擎。

为从新派生的类创建自定义输出缓存提供程序*System.Web.Caching.OutputCacheProvider*类型。 然后，你可以配置中的提供程序`Web.config`使用新的文件*提供程序*的小节*outputCache*元素，如下面的示例中所示：

[!code-xml[Main](overview/samples/sample2.xml)]

默认情况下，在 ASP.NET 4 中，所有 HTTP 响应，呈现的页面和控件使用内存中输出缓存中，如下所示在前面的示例，其中*defaultProvider*属性设置为 AspNetInternalProvider。 你可以更改 Web 应用程序使用通过指定不同的提供程序名称的默认输出缓存提供程序*defaultProvider*。

此外，你可以选择不同的输出缓存提供程序每个控件和每个请求。 选择不同的输出缓存提供程序为不同 Web 用户控件的最简单方法是以声明方式使用新进行*providerName*属性中控件指令，如下面的示例中所示：

[!code-aspx[Main](overview/samples/sample3.aspx)]

指定的 HTTP 请求不同的输出缓存提供程序需要稍有更多工作。 而不是以声明方式指定提供程序，则重写新*GetOuputCacheProviderName*中的方法`Global.asax`文件以编程方式指定要用于特定请求的提供程序。 下面的示例演示如何执行此操作。

[!code-csharp[Main](overview/samples/sample4.cs)]

为 ASP.NET 4 的输出缓存提供程序扩展性的添加后，您现在可以为您的网站通过更主动和更智能的输出缓存策略。 例如，现可以同时在缓存磁盘获得较低流量的页面缓存在内存中，站点的"Top 10"页。 或者，可以缓存呈现的页上，每个不同的组合，但使用分布式的缓存，以便将内存消耗卸载从前端 Web 服务器。

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>自动启动 Web 应用程序

某些 Web 应用程序需要加载大量数据或执行昂贵的初始化，然后才能处理相关的第一个请求处理。 在早期版本的 ASP.NET，针对这些情况你必须设计自定义方法"唤醒"ASP.NET 应用程序，然后运行过程中的初始化代码*应用程序\_负载*中的方法`Global.asax`文件。

一个名为的新的可伸缩性功能*自动启动*，直接地址提供了此方案的 ASP.NET 4 运行时在 Windows Server 2008 R2 上的 IIS 7.5 上。 自动启动功能提供了启动应用程序池，初始化 ASP.NET 应用程序，然后接受 HTTP 请求提供受控的方法。

> [!NOTE] 
> 
> IIS 7.5 的 IIS 应用程序预热模块
> 
> IIS 团队已为 IIS 7.5 中发布应用程序预热模块的第一个测试版本。 这使得预热你的应用程序更加容易，不是前面所述。 而不是编写自定义代码，你可以指定要执行之前的 Web 应用程序接受来自网络的请求资源的 Url。 在 IIS 服务的启动期间发生此预热 (如果配置 IIS 应用程序池作为*AlwaysRunning*) 和 IIS 工作进程的回收时。 在回收，旧的 IIS 工作进程继续执行请求，直到完全预热新生成的工作进程，以便应用程序发生任何中断或由于 unprimed 缓存其他问题。 请注意，此模块的工作原理的任何版本的 ASP.NET，从 2.0 版开始。
> 
> 有关详细信息，请参阅[应用程序预热](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net)IIS.net 网站上。 有关演示如何使用预热功能的演练，请参阅[Getting Started with IIS 7.5 应用程序预热模块](https://www.iis.net/learn/manage)IIS.net 网站上。


若要使用自动启动功能，是 IIS 管理员设置的应用程序池使用中的以下配置为自动启动的 IIS 7.5 中`applicationHost.config`文件：

[!code-xml[Main](overview/samples/sample5.xml)]

由于单个应用程序池可以包含多个应用程序，你指定要通过使用中的以下配置自动启动的单个应用程序`applicationHost.config`文件：

[!code-xml[Main](overview/samples/sample6.xml)]

IIS 7.5 冷启动 IIS 7.5 服务器时，或单独的应用程序池回收时，使用中的信息`applicationHost.config`文件以确定哪些 Web 应用程序需要自动启动。 对于每个应用程序标记为自动启动，IIS7.5 发送一个请求到 ASP.NET 4，以启动应用程序处于状态在此期间应用程序暂时不接受 HTTP 请求。 ASP.NET 在这种状态时，都会定义的类型实例化*serviceAutoStartProvider*属性 （如前面的示例中所示） 并调用到其公共入口点。

你创建具有必要的入口点通过实现托管的自动启动类型*IProcessHostPreloadClient*接口，如下面的示例中所示：

[!code-csharp[Main](overview/samples/sample7.cs)]

在中运行代码在你初始化后*预加载*方法和方法返回时，ASP.NET 应用程序已准备好处理请求。

添加到 IIS.5 和 ASP.NET 4 自动启动后，你现在可以执行成本高昂的应用程序初始化之前处理的第一个 HTTP 请求提供明确定义的方法。 例如，可以使用新的自动启动功能以初始化应用程序和应用程序已初始化并准备接受 HTTP 流量，则然后信号负载平衡器。

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>永久重定向页面

它是在 Web 应用程序随着时间推移，这会导致的搜索引擎中的陈旧链接的累积移动页面和其他内容周围的常见做法。 在 ASP.NET 中，开发人员已传统上处理对旧 Url 的请求通过使用通过使用*Response.Redirect*方法以将请求转发到新 URL。 但是，*重定向*方法会将一个 HTTP 302 找到 （临时重定向） 响应，这会导致额外 HTTP 往返当用户尝试访问的旧 Url。

添加一个新的 ASP.NET 4 *RedirectPermanent*帮助器方法，可以方便地对问题 HTTP 301 永久移动响应，如以下示例所示：

[!code-csharp[Main](overview/samples/sample8.cs)]

搜索引擎和识别永久重定向的其他用户代理将存储与内容，从而消除了不必要的往返行程由临时重定向的浏览器相关联的新 URL。

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>收缩会话状态

ASP.NET 提供用于在 Web 场中存储会话状态的两个默认选项： 的会话状态提供程序时，将调用的进程外会话状态服务器，并在 Microsoft SQL Server 数据库中存储数据的会话状态提供程序。 由于这两种方法涉及存储 Web 应用程序的工作进程之外的状态信息，会话状态已发送到远程存储之前要序列化。 具体取决于开发人员将保存在会话状态多少信息，序列化数据的大小可以变得相当大。

ASP.NET 4 为这两种进程外会话状态提供程序引入了新的压缩选项。 当*compressionEnabled*下面的示例所示的配置选项设置为*true*，ASP.NET 将压缩 （和解压缩） 使用.NET Framework序列化的会话状态*System.IO.Compression.GZipStream*类。

[!code-xml[Main](overview/samples/sample9.xml)]

通过简单的新特性的添加`Web.config`文件，使用备用的 CPU 周期，在 Web 服务器上的应用程序可以实现的序列化的会话状态数据的大小大幅降低。

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>扩展该范围允许 Url

ASP.NET 4 引入了用于扩展应用程序 Url 的大小的新选项。 ASP.NET 的早期版本约束 URL 路径长度为 260 个字符，基于 NTFS 文件路径限制。 在 ASP.NET 4 中，你可以选择增加 （或减少） 根据你的应用程序，此限制使用两个新*httpRuntime*配置特性。 下面的示例演示这些新属性。

[!code-xml[Main](overview/samples/sample10.xml)]

若要允许长于或短的路径 （不包括协议、 服务器名称和查询字符串的 URL 的部分），修改 *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* 属性。 若要允许长于或短的查询字符串，可修改的值 *[maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* 属性。

ASP.NET 4 还可配置的 URL 字符检查使用的字符。 当 ASP.NET 的 url 的路径部分中找到无效的字符时，它将拒绝该请求，并发出 HTTP 400 错误。 在以前版本的 ASP.NET，URL 字符检查是限于一组固定的字符。 在 ASP.NET 4 中，你可以自定义的一套使用新的有效字符*requestPathInvalidChars*属性*httpRuntime*配置元素，如下面的示例中所示：

[!code-xml[Main](overview/samples/sample11.xml)]

默认情况下， *requestPathInvalidChars*属性定义为无效的八个字符。 (分配给字符串中*requestPathInvalidChars*默认情况下*，*小于 (&lt;)、 大于 (&gt;)，和 & 符 (&amp;) 字符都是编码，因为`Web.config`文件是一个 XML 文件。)根据需要可以自定义的一套无效字符。

> [!NOTE]
> 请注意 ASP.NET 4 始终拒绝包含 0x00 到 0x1F、 ASCII 范围内的字符的 URL 路径，因为这些无效的 URL 字符中的 IETF RFC 2396 定义 ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt))。 在 Windows Server 版本上运行 IIS 6 或更高版本，http.sys 协议设备驱动程序会自动拒绝 Url 有了这些字符。


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>可扩展请求验证

ASP.NET 请求验证在跨站点脚本 (XSS) 攻击中搜索字符串的常用的传入 HTTP 请求的数据。 如果找到潜在 XSS 字符串，请求验证标记的可疑的字符串，并返回错误。 仅当它找到 XSS 攻击中使用的最常见字符串时，内置请求验证将返回错误。 以前进行 XSS 验证更为严格的尝试导致误报过多。 但是，客户可能希望更加严格，或反之可能想要有意放宽 XSS 请求验证检查的特定页面，或者为特定类型的请求。

在 ASP.NET 4 中，请求验证功能已经成为可扩展，以便你可以使用自定义请求验证逻辑。 若要扩展请求验证，你可以创建从新派生的类*System.Web.Util.RequestValidator*类型，并且你配置应用程序 (在*httpRuntime*一部分`Web.config`文件) 以使用自定义的类型。 下面的示例演示如何配置自定义请求验证类：

[!code-xml[Main](overview/samples/sample12.xml)]

新*requestValidationType*特性需要指定的类，提供自定义请求验证的标准.NET Framework 类型标识符字符串。 对于每个请求，ASP.NET 将调用自定义的类型，以处理每个传入的 HTTP 请求数据片段。 传入的 URL、 所有 HTTP 标头 （cookie 和自定义标头），以及实体正文都可用于检查由自定义请求验证类类似下面的示例所示：

[!code-csharp[Main](overview/samples/sample13.cs)]

有关在你不希望检查传入的 HTTP 数据片段的情况下，请求验证类可以回退到使用让 ASP.NET 默认请求验证运行通过只需调用*基。IsValidRequestString。*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>对象缓存和对象缓存扩展性

自其第一个版本中，ASP.NET 就提供了强大的内存中对象缓存 (*System.Web.Caching.Cache*)。 缓存实现已很常见，它已用于非 Web 应用程序。 但是，它是一个 Windows 窗体或 WPF 应用程序，以包含对引用冲`System.Web.dll`只是为了能够使用 ASP.NET 对象缓存。

若要使缓存可供所有应用程序，.NET Framework 4 引入了新的程序集、 新的命名空间、 某些基本类型和一个具体缓存实现。 新`System.Runtime.Caching.dll`程序集包含在一个新的缓存 API *System.Runtime.Caching*命名空间。 命名空间包含两个核心组类：

- 为生成任何类型的自定义缓存实现提供了基础的抽象类型。
- 具体的内存中对象的缓存实现 ( *system.runtime.caching.memorycache 有关的问题*类)。

新*MemoryCache*类密切建模于 ASP.NET 缓存，并在共享的内部缓存引擎逻辑大部分与 ASP.NET 搭配使用。 尽管公共缓存 Api *System.Runtime.Caching*已经更新，以支持开发的自定义缓存，如果你已使用 ASP.NET*缓存*对象，您会发现在熟悉的概念新的 Api。

新的深度讨论*MemoryCache*类和受支持的基 Api 都需要整个文档。 但是，下面的示例揭示了新的缓存 API 的工作原理。 该示例针对没有任何依赖项的 Windows 窗体应用程序的编写上`System.Web.dll`。

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>可扩展的 HTML、 URL 和编码的 HTTP 标头

在 ASP.NET 4 中，你可以创建以下常见文本编码任务的自定义编码例程：

- HTML 编码。
- URL 编码。
- HTML 编码属性。
- 编码出站 HTTP 标头。

你可以通过从新派生创建自定义编码器*System.Web.Util.HttpEncoder*类型中，然后配置 ASP.NET 用于中的自定义类型*httpRuntime*部分`Web.config`文件中，为下面的示例所示：

[!code-xml[Main](overview/samples/sample15.xml)]

已配置自定义编码器后，ASP.NET 会自动调用每当公共编码方法的自定义的编码实现*System.Web.HttpUtility*或*System.Web.HttpServerUtility*类称为。 这允许创建自定义编码器实现积极的字符编码，而 Web 开发团队的其余部分将继续使用公共 ASP.NET 编码 Api 的 Web 开发团队的一个组成部分。 通过集中配置自定义编码器在*httpRuntime*元素，则可保证通过自定义编码器进行路由从公共 ASP.NET 编码 Api 的所有文本编码调用。

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>为在单个辅助进程中的单个应用程序性能监视

若要增加可以在一台服务器承载的网站数，许多托管商的单个辅助进程中运行多个 ASP.NET 应用程序。 但是，如果多个应用程序使用单个共享的辅助进程，很难服务器管理员能够找出单个应用程序中遇到问题。

ASP.NET 4 利用由 CLR 引入的新资源监视功能。 若要启用此功能，你可以添加到下面的 XML 配置片段`aspnet.config`配置文件。

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> 请注意`aspnet.config`文件位于安装.NET Framework 的目录。 它不是`Web.config`文件。


当*appDomainResourceMonitoring*已启用功能、"ASP.NET 应用程序"性能类别中提供了两个新的性能计数器：*托管处理器时间百分比*和*管理内存使用*。 这两个这些性能计数器使用新的 CLR 应用程序域资源管理功能跟踪估计的 CPU 时间和各个 ASP.NET 应用程序托管的内存使用率。 因此，与 ASP.NET 4 中，管理员现在可以在单个辅助进程中运行的单个应用程序的资源消耗更加详细地查看。

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>多目标

你可以创建面向.NET Framework 的特定版本的应用程序。 在 ASP.NET 4 中中的新属性*编译*元素`Web.config`文件，可以面向.NET Framework 4 及更高版本。 如果你明确指定目标.NET Framework 4 中，并且包含中的可选元素`Web.config`文件的项如*system.codedom*，这些元素必须是正确的针对.NET Framework 4。 (如果不显式面向.NET Framework 4，目标框架，则从将项记入缺乏推断`Web.config`文件。)

下面的示例演示如何使用*targetFramework*属性中*编译*元素`Web.config`文件。

[!code-xml[Main](overview/samples/sample17.xml)]

请注意下列有关面向特定的.NET framework 版本：

- .NET Framework 4 的应用程序池中，ASP.NET 生成系统的时候承担.NET Framework 4 为目标`Web.config`文件不包含*targetFramework*属性或如果`Web.config`找不到文件。 （你可能需要更改你的应用程序，以使其在.NET Framework 4 下运行的编码。）
- 如果包括*targetFramework*属性，并且如果*system.codeDom*中定义了元素`Web.config`文件，此文件必须包含正确的输入，针对.NET Framework 4。
- 如果你使用*aspnet\_编译器*命令预编译你的应用程序 （如生成环境），则必须使用的正确版本*aspnet\_编译器*在目标框架的命令。 使用编译器随附.NET Framework 2.0 （%windir%\microsoft.net\framework\v2.0.50727) 为.NET Framework 3.5 和更早版本编译。 使用使用.NET Framework 4 附带的编译器来编译创建的应用程序使用该框架或使用更高版本。
- 在运行时，编译器使用的计算机安装的最新的 framework 程序集 (并因此在 gac 中)。 如果框架到更高版本进行了更新 （例如，安装假设版本 4.1），你将能够即使框架的更新版本中使用功能*targetFramework*属性以较低的版本为目标（例如 4.0)。 (但是，在设计时在 Visual Studio 2010 或当你使用*aspnet\_编译器*命令时，使用较新的 framework 的功能将导致编译器错误)。

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery 包含在 Web 窗体和 MVC

Web 窗体和 MVC 的 Visual Studio 模板包括开放源代码 jQuery 库。 当你创建新网站或项目时，创建包含以下 3 个文件的脚本文件夹：

- jQuery 1.4.1.js – 用户可读，unminified jQuery 库版本。
- jQuery-14.1.min.js – jQuery 库的缩减版本。
- jQuery-1.4.1-vsdoc.js – jQuery 库的 Intellisense 文档文件。

在开发应用程序时包括 jQuery 的 unminified 的版本。 包括对于生产应用程序的 jQuery 的缩减的版本。

例如，下面的 Web 窗体页显示如何使用 jQuery 更改 ASP.NET TextBox 控件以黄色当它们具有焦点时的背景色。

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>内容传送网络支持

Microsoft Ajax 内容交付网络 (CDN)，可轻松地将 ASP.NET Ajax 和 jQuery 脚本添加到 Web 应用程序。 例如，你可以开始使用 jQuery 库只需通过添加`<script>`标记添加到你指向 Ajax.microsoft.com 如下的页：

[!code-html[Main](overview/samples/sample19.html)]

通过利用 Microsoft Ajax CDN，就可以显著提高 Ajax 应用程序的性能。 Microsoft Ajax CDN 的内容进行缓存在世界各地的服务器上。 此外，Microsoft Ajax CDN 使浏览器重用缓存的 JavaScript 文件位于不同域中的网站。

Microsoft Ajax 内容交付网络支持 SSL (HTTPS)，以防你需要服务网页上使用安全套接字层。

若要了解有关 Microsoft Ajax CDN 的详细信息，请访问以下网站：

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

ASP.NET ScriptManager 支持 Microsoft Ajax CDN。 只需通过设置一个属性，EnableCdn 属性中，你可以从 CDN 检索所有 ASP.NET framework JavaScript 文件：

[!code-aspx[Main](overview/samples/sample20.aspx)]

EnableCdn 属性设置为值 true 后，ASP.NET 框架将从 CDN 包括用于验证和 UpdatePanel 的所有 JavaScript 文件中检索所有 ASP.NET framework JavaScript 文件。 设置此一个属性可以极大影响 web 应用程序的性能。

通过使用 web 资源属性，可以对 JavaScript 文件设置 CDN 路径。 新的 CdnPath 属性指定 CDN 使用 EnableCdn 属性设置为值 true 时的路径：

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>ScriptManager 显式脚本

在过去，如果你使用 ASP.NET ScriptManger 然后需要加载整个整体 ASP.NET Ajax 库。 通过利用新的 ScriptManager.AjaxFrameworkMode 属性，您可以控制完全加载 ASP.NET Ajax 库的组件和加载仅需要 ASP.NET Ajax 库的组件。

ScriptManager.AjaxFrameworkMode 属性可以设置为以下值：

- 启用-指定 ScriptManager 控件自动包括 MicrosoftAjax.js 脚本文件，它是每个核心框架脚本 （旧行为） 的组合的脚本文件。
- 禁用-指定所有 Microsoft Ajax 脚本功能将被都禁用，ScriptManager 控件不会自动引用的任何脚本。
- 显式-指定将显式包含了对你的页面要求的单个 framework 核心脚本文件的脚本引用，并确保将包含每个脚本文件所需的依赖关系的引用。

例如，如果 AjaxFrameworkMode 属性设置为值 Explicit 则可以指定所需的特定 ASP.NET Ajax 组件脚本：

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Web Forms — Web 窗体

Web 窗体的 ASP.NET 1.0 发布以来已在 ASP.NET 中的一项核心功能。 许多增强功能已在此区域中为 ASP.NET 4，其中包括：

- 能够设置*元*标记。
- 更好地控制视图状态。
- 若要使用浏览器功能的简单方法。
- 支持使用 ASP.NET 路由与 Web 窗体。
- 更好地控制生成的 Id。
- 可以保存数据控件中的选定的行。
- 更好地控制中呈现的 HTML *FormView*和*ListView*控件。
- 筛选对数据源控件的支持。

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>设置与 Page.MetaKeywords 和 Page.MetaDescription 属性的元标记

ASP.NET 4 将两个属性添加*页*类， *MetaKeywords*和*MetaDescription*。 这两个属性表示对应*元*标记在页中，如下面的示例中所示：

[!code-aspx[Main](overview/samples/sample23.aspx)]

这两个属性的工作方式相同的方式页面的*标题*属性。 它们遵循以下规则：

1. 如果有任何*元*标记中*头*属性的名称匹配的元素 (也就是说，命名 ="关键字" *Page.MetaKeywords*和名称 = "描述"*Page.MetaDescription*，尚未设置这些属性的含义)、*元*标记将在呈现时添加到页面。
2. 如果已有*元*与这些名称的标记，这些属性执行操作作为 get 和 set 方法的现有标记的内容。

你可以在运行时，它可让你从数据库或其他源获取的内容和它可让你设置标记动态来描述什么设置这些属性的特定页适用。

你还可以设置*关键字*和*说明*中的属性*@ Page*指令顶部的 Web 窗体页标记中，如以下示例所示：

[!code-aspx[Main](overview/samples/sample24.aspx)]

这将覆盖*元*标记已在页中声明的内容 （如果有）。

说明的内容*元*标记用于改善搜索列出 Google 中的预览。 (有关详细信息，请参阅[提高代码段与元说明 makeover](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) Google 站长中央博客上。)Google 和 Windows Live 搜索不使用关键字的内容的任何内容，但其他搜索引擎可能。 有关详细信息，请参阅[元关键字建议](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php)搜索引擎指南网站上。

这些新属性是一个简单的功能，但它们将保存你的要求，以将它们手动添加从或编写你自己的代码创建*元*标记。

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>对单个控件启用视图状态

默认情况下，为包含页上的每个控件可能存储视图状态，即使不需要应用程序的结果的页面，可启用视图状态。 一个页生成，并且会增加将页面发送到客户端和发回所需的时间量，在标记中包含视图状态数据。 存储比所需的详细视图状态可能会导致性能明显下降。 在 ASP.NET 的早期版本中，开发人员为了减少页大小，无法禁用单独的控件的视图状态，但出现了因此显式为单独的控件执行操作。 在 ASP.NET 4 中，Web 服务器控件包括*ViewStateMode*允许您默认情况下禁用视图状态，然后仅为需要它在页中的控件中启用它的属性。

*ViewStateMode*属性采用一个枚举，其中有三个值：*已启用*，*禁用*，和*继承*。 *启用*启用视图状态针对该控件和任何子控件的设置为*继承*或有执行任何操作设置。 *已禁用*禁用视图状态，和*继承*指定该控件使用*ViewStateMode*从父控件设置。

下面的示例演示如何*ViewStateMode*属性会发生作用。 标记和代码中的以下页面的控件包括值*ViewStateMode*属性：

[!code-aspx[Main](overview/samples/sample25.aspx)]

如你所见，代码将禁用 PlaceHolder1 控件的视图状态。 子 label1 控件继承此属性的值 (*继承*默认值为*ViewStateMode*控件。)，并因此将保存的视图状态。 在 PlaceHolder2 控件中， *ViewStateMode*设置为*已启用*，因此 label2 继承此属性并保存视图状态。 当首次加载页时，*文本*属性均*标签*控件设置为字符串"[DynamicValue]"。

这些设置的效果是，如果首次加载页面，下面的输出将显示在浏览器中：

已禁用`: [DynamicValue]`

启用：`[DynamicValue]`

回发后，但是，将显示以下输出：

已禁用`: [DeclaredValue]`

启用：`[DynamicValue]`

Label1 控件 (其*ViewStateMode*值设置为*禁用*) 具有不会保留在代码中设置为它的值。 但是，label2 控制 (其*ViewStateMode*值设置为*已启用*) 均保留其状态。

你还可以设置*ViewStateMode*中*@ Page*指令，如以下示例所示：

[!code-aspx[Main](overview/samples/sample26.aspx)]

*页*类是另一个控件; 它作为页中的所有其他控件的父控件。 默认值*ViewStateMode*是*已启用*有关的实例*页*。 因为默认为控件*继承*，控件将继承*已启用*属性值，除非你设置*ViewStateMode*页或控制级别。

值*ViewStateMode*属性确定仅在是否维护视图状态*应*属性设置为*true*。 如果*应*属性设置为*false*，将不保留视图状态即使*ViewStateMode*设置为*已启用*。

此功能很好的用途是与*ContentPlaceHolder*主页面，可以在其中设置中的控件*ViewStateMode*到*禁用*主数据页上，然后启用它为单独*ContentPlaceHolder*又可以包含需要的控件的控件视图状态。

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>与浏览器功能的更改

ASP.NET 确定用户用于浏览您的站点，通过使用一种称为功能的浏览器的功能*浏览器功能*。 浏览器功能由*HttpBrowserCapabilities*对象 (由公开*Request.Browser*属性)。 例如，你可以使用*HttpBrowserCapabilities*对象确定的类型和版本的当前浏览器是否支持 JavaScript 的特定版本。 或者，你可以使用*HttpBrowserCapabilities*对象确定请求是否来自移动设备。

*HttpBrowserCapabilities*对象都由一组的浏览器定义文件。 这些文件包含的特定浏览器的功能的信息。 在 ASP.NET 4 中，这些浏览器定义文件已更新为包含有关最近引入的浏览器和如 Google Chrome、 研究运动 BlackBerry smartphone 以及 Apple iPhone 中的设备的信息。

以下列表显示新的浏览器定义文件：

- *blackberry.browser*
- *chrome.browser*
- *Default.browser*
- *firefox.browser*
- *gateway.browser*
- *generic.browser*
- *ie.browser*
- *iemobile.browser*
- *iphone.browser*
- *opera.browser*
- *safari.browser*

#### <a name="using-browser-capabilities-providers"></a>使用浏览器功能提供程序

在 ASP.NET 3.5 Service Pack 1，你可以定义的功能的浏览器具有以下方式：

- 在计算机级别中，创建或更新`.browser`以下文件夹中的 XML 文件：

- [!code-console[Main](overview/samples/sample27.cmd)]

- 在定义的浏览器功能后，你运行以下命令从 Visual Studio 命令提示符以重新生成浏览器功能程序集并将其添加到 gac 中：

- [!code-console[Main](overview/samples/sample28.cmd)]

- 对于单个应用程序，您创建`.browser`中应用程序的文件`App_Browsers`文件夹。

这些方法要求你更改 XML 文件，但计算机级别更改，你必须重新启动应用程序后运行 aspnet\_regbrowsers.exe 过程。

ASP.NET 4 包含功能称为*浏览器功能提供程序*。 顾名思义，此允许您生成的提供程序在进而使你可以使用你自己的代码来确定浏览器功能。

在实践中，开发人员通常未定义自定义浏览器功能。 浏览器文件会进行硬若要更新，该过程相当复杂，对它们进行更新为和的 XML 语法`.browser`文件可以是复杂，无法使用和定义。 新增功能将使此过程更容易是如果没有常见的浏览器定义语法，或包含最新的浏览器的定义或甚至此类数据库的 Web 服务的数据库。 新的浏览器功能提供程序功能使得整个这些方案可能和实践，为第三方开发人员。

有两种主要方法为使用新的 ASP.NET 4 浏览器功能提供程序功能： 扩展 ASP.NET 浏览器功能定义功能，或完全替换它。 下列各节首先介绍如何替换功能，以及如何对其进行扩展。

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>替换 ASP.NET 浏览器功能的功能

若要完全替换 ASP.NET 浏览器功能定义功能，请按照下列步骤：

1. 创建派生自的提供程序类*HttpCapabilitiesProvider*它将替代*GetBrowserCapabilities*方法，如下面的示例所示： 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    此示例中的代码创建一个新*HttpBrowserCapabilities*对象，指定名为浏览器并将该功能设置为 MyCustomBrowser 功能。
2. 与应用程序注册提供程序。 

    为了与应用程序使用提供程序，您必须添加*提供程序*属性设为*browserCaps*主题中`Web.config`或`Machine.config`文件。 (你还可以定义中的提供程序属性*位置*目录应用程序，例如在特定的移动设备的文件夹中的特定文件的元素。)下面的示例演示如何设置*提供程序*配置文件中的属性：

    [!code-xml[Main](overview/samples/sample30.xml)]

    注册新的浏览器功能定义另一种方法是使用代码，如下面的示例中所示：

    [!code-csharp[Main](overview/samples/sample31.cs)]

    此代码必须在运行*应用程序\_启动*事件`Global.asax`文件。 任何将更改为*BrowserCapabilitiesProvider*执行应用程序中的任何代码，以确保缓存仍保留在已解析的有效状态之前，可能必须出现类*HttpCapabilitiesBase*对象。

#### <a name="caching-the-httpbrowsercapabilities-object"></a>缓存 HttpBrowserCapabilities 对象

前面的示例具有一个问题，这是代码将运行自定义提供程序调用以获取每次*HttpBrowserCapabilities*对象。 这可能在每个请求期间发生多次。 在示例中，提供程序的代码没有多大用处。 但是，如果自定义提供程序中的代码执行中的大量工作以便获得*HttpBrowserCapabilities*对象，这可能会影响性能。 若要防止此类情况发生，你可以缓存*HttpBrowserCapabilities*对象。 请执行这些步骤：

1. 创建一个类，派生自*HttpCapabilitiesProvider*，如下面的示例中： 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    在示例中，代码调用的自定义的 BuildCacheKey 方法，而生成的缓存密钥和它通过调用自定义的 GetCacheTime 方法获取的缓存的时间长度。 然后该代码将添加已解析*HttpBrowserCapabilities*到缓存的对象。 可以从缓存中检索并在上重复使用该对象进行的后续请求使用的自定义提供程序。
2. 注册提供程序与应用程序，如前面的过程中所述。

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>扩展 ASP.NET 浏览器功能的功能

上一节描述了如何创建一个新*HttpBrowserCapabilities* ASP.NET 4 中的对象。 你还可以通过将新的浏览器功能定义添加到已在 ASP.NET 中的那些扩展 ASP.NET 浏览器功能的功能。 你可以执行此操作而无需使用的 XML 浏览器定义。 下面的过程演示如何。

1. 创建一个类，派生自*HttpCapabilitiesEvaluator*它将替代*GetBrowserCapabilities*方法，如下面的示例中所示： 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    此代码首先使用 ASP.NET 浏览器功能的功能来尝试将浏览器标识。 但是，如果标识没有浏览器基于定义在请求中的信息 (即，如果*浏览器*属性*HttpBrowserCapabilities*对象是字符串"Unknown")，该代码调用自定义提供程序 (MyBrowserCapabilitiesEvaluator) 若要将浏览器标识。
2. 注册提供程序与应用程序，如前面的示例中所述。

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>通过将新功能添加到现有的功能定义扩展浏览器功能的功能

除了创建自定义浏览器定义提供程序并且动态创建新的浏览器定义中，你可以扩展具有附加功能的现有浏览器定义。 这允许你使用的定义，位于接近你想，但没有仅的一些功能。 为此，请执行下列步骤。

1. 创建一个类，派生自*HttpCapabilitiesEvaluator*它将替代*GetBrowserCapabilities*方法，如下面的示例中所示： 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    此代码示例将扩展现有的 ASP.NET *HttpCapabilitiesEvaluator*类，并获取*HttpBrowserCapabilities*与当前的请求定义通过使用下面的代码相匹配的对象:

    [!code-csharp[Main](overview/samples/sample35.cs)]

    然后，代码可以添加或修改此浏览器的功能。 有两种方法来指定新的浏览器功能：

    - 添加到的键/值对*IDictionary*由公开的对象*功能*属性*HttpCapabilitiesBase*对象。 在前面的示例中，该代码将添加名为的值的多点触控功能*true*。
    - 设置的现有属性*HttpCapabilitiesBase*对象。 在前面的示例中，代码会设置*帧*属性*true*。 此属性是只需访问器的*IDictionary*由公开的对象*功能*属性。 

        > [!NOTE]
        > 请注意此模型将应用到的任何属性*HttpBrowserCapabilities*，包括控件适配器。
2. 注册提供程序与应用程序，如前面的过程中所述。

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>ASP.NET 4 中的路由

ASP.NET 4 添加了对使用 Web 窗体使用的路由的内置支持。 路由使你能够配置应用程序可以接受请求不映射到物理文件的 Url。 相反，你可以使用路由定义 Url，并向用户意义，可帮助你的应用程序的搜索引擎搜索引擎优化 (SEO)。 例如，对于现有的应用程序中显示产品类别的页面的 URL 可能类似于下面的示例：

[!code-console[Main](overview/samples/sample36.cmd)]

通过使用路由，你可以配置应用程序接受要呈现的相同信息的以下 URL:

[!code-console[Main](overview/samples/sample37.cmd)]

路由已从 ASP.NET 3.5 SP1 开始提供。 (有关如何使用路由在 ASP.NET 3.5 SP1 中的示例，请参阅文章[路由使用与 WebForms](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "此项的标题。") 在 Phil Haack 博客。）但是，ASP.NET 4 包括一些功能，它可以更轻松地使用路由，其中包括：

- *PageRouteHandler*类，该类定义路由时，你使用的简单 HTTP 处理程序。 类将数据传递给请求路由到的页面。
- 新属性*HttpRequest.RequestContext*和*Page.RouteData* (即用于代理*HttpRequest.RequestContext.RouteData*对象)。 这些属性使其更轻松地从路由传递的访问信息。
- 下面的新的表达式生成器，在中定义*System.Web.Compilation.RouteUrlExpressionBuilder*和*System.Web.Compilation.RouteValueExpressionBuilder*:
- *RouteUrl*，该属性提供一种简单的方法来创建到 ASP.NET 服务器控件中的路由 URL 相对应的 URL。
- *RouteValue*，该属性提供一种简单的方法，以提取信息*RouteContext*对象。
- *RouteParameter*类，它可以更轻松地将中包含的数据传递*RouteContext*对数据源控件的查询的对象 (类似于[ *FormParameter*](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Web 窗体页的路由

下面的示例演示如何使用新的定义的 Web 窗体路由*MapPageRoute*方法*路由*类：

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 引入了*MapPageRoute*方法。 下面的示例与上一示例所示的 SearchRoute 定义是等效，但使用*PageRouteHandler*类。

[!code-csharp[Main](overview/samples/sample39.cs)]

示例中的代码映射到一个物理页的路由 (在第一个路由，到`~/search.aspx`)。 第一个的路由定义也指定应从 URL 中提取并传递到页面的参数名术语。

*MapPageRoute*方法支持以下的方法重载：

- *MapPageRoute （字符串 routeName、 字符串 routeUrl、 字符串 physicalFile、 bool checkPhysicalUrlAccess）*
- *MapPageRoute （字符串 routeName、 字符串 routeUrl、 字符串 physicalFile、 bool checkPhysicalUrlAccess、 RouteValueDictionary 默认值）*
- *MapPageRoute （字符串 routeName、 字符串 routeUrl、 字符串 physicalFile、 bool checkPhysicalUrlAccess、 RouteValueDictionary 默认值、 RouteValueDictionary 约束）*

*CheckPhysicalUrlAccess*参数指定该路由是否应检查路由到的物理页的安全权限 (在这种情况下，search.aspx) 以及针对传入 URL 的权限 （在这种情况下，搜索/ {术语})。 如果值*checkPhysicalUrlAccess*是*false*，将进行检查，仅传入 URL 的权限。 在中定义这些权限`Web.config`文件中使用类似如下设置：

[!code-xml[Main](overview/samples/sample40.xml)]

在示例配置中，访问被拒绝到物理页`search.aspx`除管理员角色中的所有用户。 当*checkPhysicalUrlAccess*参数设置为*true* （这是其默认值），只有管理员用户允许访问 URL /search/ {术语}，因为物理页 search.aspx 是限制为该角色中的用户。 如果*checkPhysicalUrlAccess*设置为*false*和站点配置中前面的示例所示，将允许所有经过身份验证的用户访问 URL /search/ {术语}。

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Web 窗体页中的路由信息的读取

在 Web 窗体物理页的代码中，你可以访问路由已从 URL 中提取的信息 (或另一个对象已添加到其他信息*RouteData*对象) 通过使用两个新属性： *HttpRequest.RequestContext*和*Page.RouteData*。 (*Page.RouteData*包装*HttpRequest.RequestContext.RouteData*。)下面的示例演示如何使用*Page.RouteData*。

[!code-csharp[Main](overview/samples/sample41.cs)]

代码中提取前面示例路由中定义为术语参数，而传递的值。 请考虑以下请求 URL:

[!code-console[Main](overview/samples/sample42.cmd)]

发出此请求时，"scott"将呈现在 word`search.aspx`页。

#### <a name="accessing-routing-information-in-markup"></a>访问在标记中的路由信息

上一节中所述的方法演示如何在 Web 窗体页中的代码中获取路由数据。 你还可以使用相同的信息有权访问在标记中的表达式。 表达式生成器是一种功能强大且简洁的方式来使用声明性代码。 (有关详细信息，请参阅文章[Express 自己与自定义表达式生成器](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx)Phil Haack 博客上。)

ASP.NET 4 包含两个新的表达式生成器用于 Web 窗体路由。 下面的示例演示如何使用它们。

[!code-aspx[Main](overview/samples/sample43.aspx)]

在示例中， *RouteUrl*表达式用于定义基于路由的参数的 URL。 这样可以节省你无需编写硬编码的完整 URL 向的标记，并可让你更高版本更改 URL 结构，而无需对此链接的任何更改。

根据前面定义的路由，此标记将生成以下 URL:

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET 自动工作出正确的路由 （即，它生成正确的 URL） 基于输入参数。 您还可以包含在表达式中，这样就可以指定要使用的路由的路由名称。

下面的示例演示如何使用*RouteValue*表达式。

[!code-aspx[Main](overview/samples/sample45.aspx)]

包含此控件的页在运行时，标签中显示的值"scott"。

*RouteValue*表达式可以很容易地在标记中，使用路由数据，而且它避免了无需使用更复杂的 Page.RouteData["x"] 在标记中的语法。

#### <a name="using-route-data-for-data-source-control-parameters"></a>使用适用于数据源控件参数路线数据

*RouteParameter*类可作为数据源控件中的查询的参数值指定路由数据。 它[工作方式非常类似于](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)类，如下面的示例中所示：

[!code-aspx[Main](overview/samples/sample46.aspx)]

在这种情况下，路由参数术语的值将用于@companyname中的参数*选择*语句。

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>设置客户端 Id

新*ClientIDMode*属性解决了长期存在的问题，在 ASP.NET 中，即如何控件创建*id*所呈现元素的属性。 了解*id*呈现元素的属性是重要信息： 如果你的应用程序包括引用这些元素的客户端脚本。

*Id*中呈现 Web 服务器控件的 HTML 特性基于生成*ClientID*的控件属性。 ASP.NET 4 中，生成的算法之前*id*属性从*ClientID*属性已要连接的命名容器 （如果有） 替换为 ID，并在重复 （作为中控件的情况下数据控件），以添加前缀和序列号。 虽然这始终有保证的页中的控件 Id 是唯一的该算法导致了控制未可预测，并且因此非常难以在客户端脚本中引用的 Id。

新*ClientIDMode*属性，允许您指定更确切地说如何的客户端 ID 生成的控件。 你可以设置*ClientIDMode*任何控件，包括页的属性。 可能的设置如下所示：

- *AutoID* – 这相当于生成的算法*ClientID*的 ASP.NET 的早期版本中使用的属性值。
- *静态*– 这是指定*ClientID*值将为而无需连接的命名容器的父 Id 的 ID 相同。 这可在 Web 用户控件中。 Web 用户控件可以位于不同页上和不同的容器控件中，因为它可能很难编写使用控件的客户端脚本*AutoID*算法因为你无法预测的 ID 值将是什么.
- *可预测*– 此选项主要用于在使用重复的模板的数据控件中使用。 它将连接在一起的控件的命名容器，但生成的 ID 属性*ClientID*值不包含字符串，例如"ctlxxx"。 此设置与结合工作*ClientIDRowSuffix*的控件属性。 你设置*ClientIDRowSuffix*生成使用的后缀数据字段中，名称和该字段的值的属性*ClientID*值。 通常使用作为数据记录的主键*ClientIDRowSuffix*值。
- *继承*– 此设置是控件的默认行为; 它指定控件的 ID 生成，并为其父项相同。

你可以设置*ClientIDMode*页级别的属性。 这定义了默认的*ClientIDMode*当前页中的所有控件的值。

默认值*ClientIDMode*页级别的值是*AutoID*，也是默认值*ClientIDMode*控件级别的值是*继承*. 因此，如果不执行在代码中任何位置设置此属性，则所有控件将都默认为*AutoID*算法。

在中设置的页级别值*@ Page*指令，如下面的示例中所示：

[!code-aspx[Main](overview/samples/sample47.aspx)]

你还可以设置*ClientIDMode*在配置文件中，在计算机级别或应用程序级别的值。 这定义了默认的*ClientIDMode*设置应用程序中的所有页中的所有控件。 如果在计算机级别设置的值，它定义了默认的*ClientIDMode*在该计算机上的所有 Web 站点设置。 下面的示例演示*ClientIDMode*在配置文件中设置：

[!code-xml[Main](overview/samples/sample48.xml)]

如前文所述的值*ClientID*属性从命名容器派生的控件的父级。 在某些情况下，例如，当你使用主页面，控件最终可能会具有 Id 类似于以下中呈现 HTML:

[!code-html[Main](overview/samples/sample49.html)]

即使*输入*显示在标记中的元素 (从*文本框中*控件) 是仅有两个命名的容器在页中的深度 (嵌套*ContentPlaceholder*控件)，由于处理母版页的方法，最终结果是如下所示的控件 ID:

[!code-console[Main](overview/samples/sample50.cmd)]

此 ID 保证在页上，必须唯一，但不必要地长的大多数情况下。 假设你想要减少的呈现 ID 的长度并更好地控制如何生成 ID。 （例如，你想要消除"ctlxxx"前缀。）实现此目的的最简单方法是通过设置*ClientIDMode*属性，如下面的示例中所示：

[!code-aspx[Main](overview/samples/sample51.aspx)]

在此示例中， *ClientIDMode*属性设置为*静态*的最外面*NamingPanel*元素，并设置为*可预测*为内部*NamingControl*元素。 这些设置会导致 （了页面和主页上的 rest 假定要如下所示前面的示例相同） 的以下标记：

[!code-html[Main](overview/samples/sample52.html)]

*静态*设置的效果的重置包含最外面的任何控件的命名层次结构*NamingPanel*元素，并消除*ContentPlaceHolder*和*母版页*Id 从生成的 id。 (*名称*呈现元素的属性不受影响，因此正常的 ASP.NET 功能保留的事件中，查看状态，依次类推。)重置命名层次结构会产生副作用是，即使移动的标记*NamingPanel*为另一种元素*ContentPlaceholder*控件，呈现客户端 Id 保持不变。

> [!NOTE]
> 请注意它是由您来确保呈现的控件 Id 是唯一的。 如果它们不是，它能中断为单个 HTML 元素，如客户端需要唯一的 Id 的任何功能*document.getElementById*函数。


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>在数据绑定控件中创建可预测的客户端 Id

*ClientID*为旧算法的数据绑定列表控件中的控件可以是生成的值长，不是真正可预测。 *ClientIDMode*功能可以帮助你进行更大的控制权生成通过这些 Id。

下面的示例中的标记包括*ListView*控件：

[!code-aspx[Main](overview/samples/sample53.aspx)]

在前面的示例中， *ClientIDMode*和*RowClientIDRowSuffix*标记中设置属性。 *ClientIDRowSuffix*属性可以使用仅在数据绑定控件中，并且其行为不同，具体取决于哪个控件使用。 差异如下：

- *GridView*控件 — 你可以在数据源中，在运行时创建客户端 Id 组合的指定一个或多个列的名称。 例如，如果你设置*RowClientIDRowSuffix*为"产品名称，ProductId"，呈现的元素将具有类似于下面的格式控件 Id:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView*控件 — 你可以指定追加到客户端 id。 的数据源中的单个列 例如，如果你设置*ClientIDRowSuffix*呈现的控件 Id 将为"产品名称"，具有一种格式如下所示：

- [!code-console[Main](overview/samples/sample55.cmd)]

- 在这种情况下尾随 1 派生自当前数据项的产品 ID。

- *转发器*控件 — 此控件不支持*ClientIDRowSuffix*属性。 在*中继器*使用控件，当前行的索引。 当你使用 ClientIDMode ="可预测"替换*中继器*控制，客户端 Id，将生成具有以下格式：

- [!code-console[Main](overview/samples/sample56.cmd)]

- 尾随 0 是当前行的索引。

*FormView*和*说明*控件不显示多个行，因此它们不支持*ClientIDRowSuffix*属性。

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>保持数据控件中的行选择

*GridView*和*ListView*控件可以让用户选择一行。 在以前版本的 ASP.NET，具有已选择基于在页上的行索引。 例如，如果你选择第 1 页上的第三个项，并且然后将其移动到第 2 页，选择该页面上的第三个项。

仅在.NET Framework 3.5 SP1 中的动态数据项目中，最初已支持持久化的选择。 启用此功能后，当前所选的项取决于项的数据键。 这意味着，如果您选择的第三个行，第 1 页上，并移动到第 2 页，则不选择在第 2 页。 当您移回到第 1 页时，第三行仍处于选中状态。 现在支持持久化的选择*GridView*和*ListView*使用的所有项目中的控件*EnablePersistedSelection*属性，如下所示下面的示例：

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>ASP.NET 图表控件

ASP.NET*图表*控件扩展.NET Framework 中的数据可视化产品。 使用*图表*控件，你可以创建具有直观和极具视觉表现力的图表进行复杂的统计或财务分析的 ASP.NET 页。 ASP.NET*图表*控件作为外接程序引入到.NET Framework 版本 3.5 SP1 版本，.NET Framework 4 版本的一部分。

控件包括以下功能：

- 35 种不同的图表类型。
- 无限的数量的图表区、 标题、 图例和批注。
- 所有图表元素的外观设置了多种不同。
- 对于大多数图表类型的三维支持。
- 可以自动调整数据点的智能数据标签。
- 条带线、 刻度分隔线和对数缩放。
- 包含 50 多个用于数据分析和转换的财务和统计公式。
- 简单绑定和操作中的图表数据。
- 对常见数据格式，例如日期、 时间和货币的支持。
- 交互性和事件驱动的自定义，包括客户端的支持，请单击使用 Ajax 事件。
- 状态管理。
- 二进制流。

下图显示由 ASP.NET 图表控件的财务图表的示例。

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

图 2: ASP.NET 图表控件示例

有关如何使用 ASP.NET 图表控件中的更多示例上下载的示例代码[Microsoft 图表控件的示例环境](https://go.microsoft.com/fwlink/?LinkId=128300)MSDN 网站上的页。 你可以找到更多示例的社区内容在[图表控件论坛](https://go.microsoft.com/fwlink/?LinkId=128713)。

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>将图表控件添加到 ASP.NET 页

下面的示例演示如何将添加*图表*到 ASP.NET 页使用标记的控件。 在示例中，*图表*控件生成静态数据点的柱形图。

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>使用三维图表

*图表*控件包含*ChartAreas*集合，其中可包含*ChartArea*定义图表区的特征的对象。 例如，若要使用三维图表区，请使用*Area3DStyle*属性，如以下示例所示：

[!code-aspx[Main](overview/samples/sample59.aspx)]

下图显示具有四个系列的三维图表*栏*图表类型。

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

图 3： 三维条形图

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>使用刻度分隔线和对数刻度

刻度分隔线和对数刻度是两种方法可以向图表中添加复杂。 这些功能是特定于每个轴在图表区。 例如，若要在图表区的主 Y 轴上使用这些功能，使用*AxisY.IsLogarithmic*和*ScaleBreakStyle*中的属性*ChartArea*对象。 下面的代码段演示如何使用刻度分隔线的主 Y 轴上。

[!code-aspx[Main](overview/samples/sample60.aspx)]

下图显示 Y 轴具有启用刻度分隔线。

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

图 4： 刻度分隔线

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>用 QueryExtender 控件筛选数据

创建数据驱动的网页的开发人员的非常常见任务是筛选数据。 这通常已执行通过构建*其中*子句在数据源控件。 此方法可能比较复杂，在某些情况下*其中*语法不允许您充分利用基础数据库的完整功能。

为简化筛选操作，新*QueryExtender*控件已添加 ASP.NET 4 中。 此控件可添加到*EntityDataSource*或*LinqDataSource*以便筛选由这些控件返回的数据的控件。 因为*QueryExtender*控件依赖于 LINQ、 应用筛选器是在数据库服务器上之前的数据发送到页上，这会导致非常高效的操作。

*QueryExtender*控件支持的广泛的筛选器选项。 以下部分介绍了这些选项，并提供如何使用它们的示例。

#### <a name="search"></a>搜索

搜索选项时， *QueryExtender*控件执行搜索指定字段中。 在下面的示例中，该控件使用输入中 TextBoxSearch 控件并将其内容搜索的文本`ProductName`和`Supplier.CompanyName`中从返回的数据的列*LinqDataSource*控件。

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>范围

范围选项类似于搜索选项，但指定要定义的范围的值对。 在下面的示例中， *QueryExtender*控制搜索`UnitPrice`中返回的数据列*LinqDataSource*控件。 从页上的 TextBoxFrom 和 TextBoxTo 控件中读取的范围。

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

属性表达式选项，可以定义为属性值的比较。 如果表达式计算结果为*true*，返回正在读取的数据。 在下面的示例中， *QueryExtender*控件通过比较中的数据来筛选数据`Discontinued`从 CheckBoxDiscontinued 控件在页上的值的列。

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

最后，可以指定要使用的自定义表达式*QueryExtender*控件。 此选项，你可以调用一个函数中定义自定义筛选器逻辑页。 下面的示例演示如何以声明方式指定的自定义表达式*QueryExtender*控件。

[!code-aspx[Main](overview/samples/sample64.aspx)]

下面的示例演示通过调用的自定义函数*QueryExtender*控件。 在这种情况下，而不是使用包含的数据库查询*其中*子句，代码使用 LINQ 查询来筛选的数据。

[!code-csharp[Main](overview/samples/sample65.cs)]

以下示例演示只有一个表达式中正在使用*QueryExtender*一次的控件。 但是，您可以包含多个表达式内的*QueryExtender*控件。

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>Html 编码的代码表达式

使用依赖于某些 ASP.NET 站点 （尤其是使用 ASP.NET MVC) `<%` =  `expression %>` （通常称为"代码问题"） 的语法为一些文本写入响应。 当你使用的代码表达式时，很容易忘记进行 HTML 编码的文本，如果文本来自用户输入，它可能会导致页打开与 XSS （跨站点脚本） 攻击。

ASP.NET 4 中引入的代码表达式的以下新语法：

[!code-aspx[Main](overview/samples/sample66.aspx)]

此语法使用写入到响应时，HTML 编码，默认情况下。 这个新的表达式有效地转换为以下：

[!code-aspx[Main](overview/samples/sample67.aspx)]

例如， &lt;%: 请求"UserInput"%&gt;执行 HTML 编码的值*请求"UserInput"*。

此功能旨在使将旧语法的所有实例替换为新的语法，以便你并不强制的每一个步骤决定使用哪一个。 但是，一些情况下在其中输出的文本要作为 HTML，或已进行编码时，在这种情况下这可能导致双重解码。

对于这些情况下，ASP.NET 4 中引入了新接口， *IHtmlString*，以及具体实现， *HtmlString*。 这些类型的实例，您可以指示，返回值是已正确编码 （或否则检查） 用于显示为 HTML，且，因此值不应为 HTML 编码试。 例如，以下不应为 （而不是） HTML 编码：

[!code-aspx[Main](overview/samples/sample68.aspx)]

如果您运行的 ASP.NET 4，已经过更新以使用此新语法，以便它们不双编码，但只有 ASP.NET MVC 2 帮助器方法。 当你运行使用 ASP.NET 3.5 SP1 的应用程序，则此新语法不起作用。

请记住这并不保证从 XSS 攻击的保护。 例如，使用不是用引号引起来的属性值的 HTML 可以包含仍容易的用户输入。 请注意，ASP.NET 控件和 ASP.NET MVC 帮助器的输出始终包含属性值用引号引起来，这是建议的方法。

同样，此语法不执行 JavaScript 编码，如当你创建 JavaScript 字符串根据用户输入。

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>项目模板更改

在早期版本的 ASP.NET，当你使用 Visual Studio 创建新网站项目或 Web 应用程序项目，生成的项目包含仅 Default.aspx 页上，默认`Web.config`文件，和`App_Data`文件夹，如以下所示图：

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio 也支持空网站项目类型，不包含任何文件根本下, 图中所示：

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

结果是，对于初学者，很少的指南如何生成生产型 Web 应用程序。 因此，ASP.NET 4 中引入了三个新模板，另一个为空的 Web 应用程序项目，每个 Web 应用程序和网站项目。

#### <a name="empty-web-application-template"></a>空 Web 应用程序模板

顾名思义，空 Web 应用程序模板是精简的 Web 应用程序项目。 下图中所示，可以从 Visual Studio 的新项目对话框中，选择此项目模板：

[![](overview/_static/image7.png)](overview/_static/image6.png)

([单击以查看实际尺寸的图像](overview/_static/image8.png))

在创建空的 ASP.NET Web 应用程序时，Visual Studio 将创建以下文件夹布局：

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

这是类似于空网站布局从早期版本的 ASP.NET，有一个例外。 在 Visual Studio 2010 中，空 Web 应用程序和空网站项目包含以下最小`Web.config`包含 Visual Studio 用于标识的项目所面向的 framework 的信息的文件：

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

如果没有此*targetFramework*属性，Visual Studio 默认值为面向.NET Framework 2.0，以打开较旧的应用程序时保持兼容性。

#### <a name="web-application-and-web-site-project-templates"></a>Web 应用程序和网站项目模板

其他两个新项目模板随附 Visual Studio 2010 包含重大更改。 下图显示在创建新的 Web 应用程序项目时创建的项目布局。 （网站项目中的布局是几乎相同的。）

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

该项目包括一个未在早期版本中创建的文件数。 此外，新的 Web 应用程序项目被配置为具有基本成员资格功能，这样就可以快速开始保护新的应用程序的访问。 由于此包含`Web.config`文件新的项目包括用于配置成员资格、 角色和配置文件的项。 下面的示例演示`Web.config`新的 Web 应用程序项目文件。 (在这种情况下， *roleManager*处于禁用状态。)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([单击以查看实际尺寸的图像](overview/_static/image14.png))

项目还包含第二个`Web.config`文件中`Account`目录。 第二个配置文件提供一种方法来确保安全访问的 ChangePassword.aspx 页非-已登录的用户。 下面的示例演示的第二个内容`Web.config`文件。

![](overview/_static/image15.png)

默认情况下，新的项目模板中创建的页还包含比在早期版本中的其他内容。 项目包含一个默认母版页和 CSS 文件，并且默认页 (Default.aspx) 配置为使用母版页默认情况下。 结果是，如果首次运行的 Web 应用程序或网站，默认 （主） 页将已正常运行。 事实上，它是类似于你启动新的 MVC 应用程序时，将显示默认页。

[![](overview/_static/image17.png)](overview/_static/image16.png)

([单击以查看实际尺寸的图像](overview/_static/image18.png))

这些更改的项目模板的目的是提供有关如何开始生成新的 Web 应用程序的指导。 在语义上正确，严格 XHTML 1.0 符合标记并且使用 CSS 指定的布局，在模板中的页体现生成 ASP.NET 4 Web 应用程序的最佳做法。 默认页还具有你可以轻松自定义的两列布局。

例如，假设，为新的 Web 应用程序想要更改某些颜色和插入我的 ASP.NET 应用程序徽标代替你公司的徽标。 若要执行此操作，你创建下的新建一个目录`Content`来存储你的徽标图像：

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

若要将映像添加到页面中，然后打开`Site.Master`文件中，查找其中我的 ASP.NET 应用程序文本定义，并将其替换*映像*元素其*src*属性设置为新的徽标映像，如以下示例所示：

[![](overview/_static/image21.png)](overview/_static/image20.png)

([单击以查看实际尺寸的图像](overview/_static/image22.png))

然后，可以进入 Site.css 文件和修改 CSS 类定义，以更改页的背景色以及的标头。

这些更改的结果是，你可以显示一个包含很少的工作量的自定义的主页：

[![](overview/_static/image24.png)](overview/_static/image23.png)

([单击以查看实际尺寸的图像](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>CSS 改进

ASP.NET 4 中主要区域之一是工作的帮助呈现符合最新的 HTML 标准的 HTML。 这包括对 ASP.NET Web 服务器控件如何使用 CSS 样式更改。

#### <a name="compatibility-setting-for-rendering"></a>呈现的兼容性设置

默认情况下，当 Web 应用程序或网站面向.NET Framework 4， *controlRenderingCompatibilityVersion*属性*页*元素设置为"4.0"。 在计算机级别中定义此元素`Web.config`文件和默认情况下适用于所有 ASP.NET 4 应用程序：

[!code-xml[Main](overview/samples/sample69.xml)]

值*controlRenderingCompatibility*是一个字符串，它允许潜在新版本定义在将来的版本。 在当前版本中，此属性支持以下值：

- "3.5". 此设置指示旧的呈现和标记。 标记由控件呈现为 100%向后兼容，以及设置的*xhtmlConformance*遵循属性。
- "4.0". 如果该属性具有此设置，ASP.NET Web 服务器控件将执行以下操作：
- *XhtmlConformance*属性始终被视为"Strict"。 因此，控件呈现 XHTML 1.0 Strict 标记。
- 禁用非输入控件不再呈现无效的样式。
- *div*围绕隐藏的字段的元素的现在风格，因此它们不会干扰用户创建 CSS 规则。
- 菜单控件呈现在语义上正确，并符合辅助功能准则的标记。
- 验证控件不会呈现内联样式。
- 控制以前呈现边框 ="0"(从 ASP.NET 派生的控件*表*控制和 ASP.NET*映像*控件) 不再呈现此属性。

#### <a name="disabling-controls"></a>禁用控件

在 ASP.NET 3.5 SP1 和更早版本中，框架呈现*禁用*特性的 HTML 标记中任何控制其*已启用*属性设置为*false*。 但是，根据 HTML 4.01 规范，仅*输入*元素应具有此属性。

在 ASP.NET 4 中，你可以设置*controlRenderingCompatabilityVersion*属性设置为"3.5"，如以下示例所示：

[!code-xml[Main](overview/samples/sample70.xml)]

你可能会创建标记*标签*如下所示，禁用控件的控件：

[!code-aspx[Main](overview/samples/sample71.aspx)]

*标签*控件将会设置以下 HTML 呈现：

[!code-html[Main](overview/samples/sample72.html)]

在 ASP.NET 4 中，你可以设置*controlRenderingCompatabilityVersion*为"4.0"。 在这种情况下，仅控制该呈现*输入*元素将呈现*禁用*属性时控件的*已启用*属性设置为*false*. 无法呈现 HTML 控件*输入*元素改为呈现*类*引用可用于定义控件的已禁用的面貌 CSS 类的属性。 例如，*标签*前面示例中所示的控件也会生成以下标记：

[!code-html[Main](overview/samples/sample73.html)]

指定对于此控件的类的默认值是"aspNetDisabled"。 但是，可以通过设置静态更改此默认值*DisabledCssClass*的静态属性*WebControl*类。 为控件开发人员要使用的特定控件的行为也可以定义使用*SupportsDisabledAttribute*属性。

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>隐藏 div 元素周围隐藏字段

ASP.NET 2.0 和更高版本呈现特定于系统的隐藏的字段 (如*隐藏*元素，用于存储视图状态信息) 内*div*为了遵守 XHTML 标准元素。 但是，这会导致问题，如果 CSS 规则影响*div*页上的元素。 例如，这会导致在页中显示一个像素行大约隐藏*div*元素。 在 ASP.NET 4 中， *div*将由 ASP.NET 生成的隐藏的字段括起来的元素添加 CSS 类引用，如以下示例所示：

[!code-html[Main](overview/samples/sample74.html)]

然后可以定义仅适用于的 CSS 类*隐藏*元素都由 ASP.NET，如以下示例所示：

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>模板化控件呈现的外部表

默认情况下，以下支持模板的 ASP.NET Web 服务器控件自动将封装在用于将内联样式应用的外部表：

- *FormView*
- *登录名*
- *说明*
- *ChangePassword*
- *向导*
- *CreateUserWizard*

名为的新属性*RenderOuterTable*已添加到允许的外部表，从标记要删除这些控件。 例如，考虑下面的示例对*FormView*控件：

[!code-aspx[Main](overview/samples/sample76.aspx)]

此标记将呈现到页上，其中包括 HTML 表的以下输出：

[!code-html[Main](overview/samples/sample77.html)]

若要防止呈现表，可以设置*FormView*控件的*RenderOuterTable*属性，如以下示例所示：

[!code-aspx[Main](overview/samples/sample78.aspx)]

前面的示例呈现以下输出，而不*表*， *tr*，和*td*元素：

> 内容


此增强功能可以更轻松到样式的控件内容的使用 CSS，因为无意外的标记所呈现控件。

> [!NOTE]
> 请注意此更改不支持 Visual Studio 2010 设计器中中的自动套用格式函数，因为不再有*表*元素可以承载生成的自动套用格式选项的样式特性。


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>ListView 控件增强功能

*ListView*控件已便于在 ASP.NET 4 中使用。 指定包含一个具有已知 ID 的服务器控件的布局模板所需的控件的早期版本 以下标记显示如何使用的典型示例*ListView* ASP.NET 3.5 中的控件。

[!code-aspx[Main](overview/samples/sample79.aspx)]

在 ASP.NET 4 中， *ListView*控件不需要的布局模板。 在前面的示例所示的标记可以替换为以下标记：

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>按和说明如何增强功能

可以在 ASP.NET 3.5 中，指定布局*按*和*说明如何*使用以下两个设置：

- *流*。 控件呈现*跨越*元素以包含其内容。
- *表*。 控件呈现*表*元素以包含其内容。

下面的示例演示上述每个控件的标记。

[!code-aspx[Main](overview/samples/sample81.aspx)]

默认情况下，控件将呈现 HTML 类似于以下：

[!code-html[Main](overview/samples/sample82.html)]

因为这些控件包含列表项呈现在语义上是正确的 HTML，它们应呈现其内容使用 HTML 列表 (*li*) 元素。 这简化它为读取网页使用辅助技术，并使控件能够更轻松地使用 CSS 样式的用户。

在 ASP.NET 4 中，*按*和*说明如何*控件支持的以下新值*适用*属性：

- *OrderedList* – 内容呈现为*li*中的元素*ol*元素。
- *UnorderedList* – 内容呈现为*li*中的元素*ul*元素。

下面的示例演示如何使用这些新值。

[!code-aspx[Main](overview/samples/sample83.aspx)]

前面的标记生成的以下 HTML:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> 请注意，是否你设置*适用*到*OrderedList*或*UnorderedList*、 *RepeatDirection*属性不再可用，并将如果已在标记或代码中设置的属性，则引发运行时异常。 属性将具有任何值，因为这些控件的可视布局定义改为使用 CSS。


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>菜单控件改进

ASP.NET 4 之前*菜单*控件呈现的 HTML 表的一系列。 这变得更难应用 CSS 样式外部设置内联属性并且也不符合辅助功能标准。

在 ASP.NET 4 中，则控件现在呈现使用未经排序的列表和列表元素组成的语义标记的 HTML。 下面的示例演示在 ASP.NET 页中标记*菜单*控件。

[!code-aspx[Main](overview/samples/sample85.aspx)]

如果页面呈现时，控件便会生成以下 HTML ( *onclick*代码已被省略为清楚起见):

[!code-html[Main](overview/samples/sample86.html)]

呈现除改进外，已使用焦点管理得到改善菜单的键盘导航。 当*菜单*控件获得焦点时，你可以使用箭头键导航元素。 *菜单*控件现在还将附加访问的丰富 internet 应用程序 (ARIA) 角色和属性如下[机翼](http://www.w3.org/TR/wai-aria-practices/#menu "菜单 ARIA 准则")以改进可访问性。

菜单控件的样式呈现的样式块中的顶部的页上，而不是根据呈现的 HTML 元素。 如果你想要对该控件的样式的完全控制，则可以设置新*IncludeStyleBlock*属性*false*，在这种情况下不发出样式块。 使用此属性的一种方法是使用中的 Visual Studio 设计器的自动套用格式功能来设置菜单的外观。 然后可以运行页上，打开页面源文件中，，然后将呈现的样式块复制到外部 CSS 文件。 在 Visual Studio 中，撤消的样式和集*IncludeStyleBlock*到*false*。 结果是使用外部样式表中的样式定义菜单外观。

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>向导和 CreateUserWizard 控件

ASP.NET*向导*和*CreateUserWizard*控件支持使你能够定义他们呈现的 HTML 的模板。 (*CreateUserWizard*派生自*向导*。)下面的示例演示的标记完全模板化*CreateUserWizard*控件：

[!code-aspx[Main](overview/samples/sample87.aspx)]

控件上呈现 HTML 类似如下：

[!code-html[Main](overview/samples/sample88.html)]

在 ASP.NET 3.5 SP1 中，尽管您可以更改的模板内容，但你仍具有有限的控制的输出*向导*控件。 在 ASP.NET 4 中，你可以创建*LayoutTemplate*模板和插入*占位符*控制 （使用保留的名称） 以指定你希望如何*向导控件*来呈现。 下面的示例演示了此过程：

[!code-aspx[Main](overview/samples/sample89.aspx)]

该示例包含以下名为中的占位符*LayoutTemplate*元素：

- *headerPlaceholder* – 在运行时，这替换的内容*HeaderTemplate*元素。
- *sideBarPlaceholder* – 在运行时，这替换的内容*SideBarTemplate*元素。
- *wizardStepPlaceHolder* – 在运行时，这替换的内容*WizardStepTemplate*元素。
- *navigationPlaceholder* – 在运行时，这替换为已定义的任何导航模板。

在以下示例中使用占位符的标记的以下 HTML 呈现 （而不实际的模板中定义的内容）：

[!code-html[Main](overview/samples/sample90.html)]

是现在不是用户定义的唯一 HTML 是*跨越*元素。 (我们预计这在将来版本中，即使*跨越*元素将不会呈现。)现在可以完全控制由生成的几乎所有内容*向导*控件。

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC 引入了作为外接程序框架到 ASP.NET 3.5 SP1 在 2009 年 3 月中。 Visual Studio 2010 包括 ASP.NET MVC 2，其中包括新特性和功能。

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>区域支持

区域，你可以组控制器和视图到相对独立于其他部分中的大型应用程序的部分。 每个区域可以作为一个单独的 ASP.NET MVC 项目，然后可以引用的主应用程序实现。 这可帮助管理复杂性，在生成大型应用程序时，并使其便于在单个应用程序协同工作的多个团队。

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>数据注释属性验证支持

*DataAnnotations*属性，可以通过使用元数据特性附加到模型的验证逻辑。 *DataAnnotations* ASP.NET 3.5 SP1 中的 ASP.NET 动态数据中引入了属性。 这些特性已集成到默认的模型联编程序，提供的元数据驱动方法验证用户输入。

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>模板化帮助器

模板化帮助器使你自动相关联的编辑和显示数据类型的模板。 例如，你可以使用模板帮助程序来指定自动为呈现日期选取器 UI 元素*System.DateTime*值。 这是类似于 ASP.NET 动态数据中的字段模板。

*Html.EditorFor*和*Html.DisplayFor*帮助器方法具有内置支持，对于呈现标准数据类型具有多个属性也为复杂对象。 他们还进行自定义呈现通过让你将应用这样的数据注释属性*DisplayName*和*ScaffoldColumn*到*ViewModel*对象。

通常，你想要自定义 UI 帮助器甚至可更进一步的输出，并对什么生成具有完全控制。 *Html.EditorFor*和*Html.DisplayFor*帮助器方法支持此使用模板化一种机制，你可以定义外部可以重写的模板和控件呈现的输出。 模板的类可以单独来呈现。

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dynamic Data — 动态数据

引入了动态数据中中旬 2008年中的.NET Framework 3.5 SP1 版本。 此功能提供了用于创建数据驱动的应用程序，其中包括许多增强功能：

- 快速生成数据驱动的网站提供 RAD 的体验。
- 基于数据模型中定义的约束的自动验证。
- 能够轻松地将更改为中的字段生成的标记*GridView*和*说明*使用是动态的数据项目的一部分的字段模板的控件。

> [!NOTE]
> 请注意有关详细信息，请参阅[动态数据文档](https://msdn.microsoft.com/library/cc488545.aspx)MSDN 库中。


为 ASP.NET 4 中，动态数据得到增强，为开发人员快速生成数据驱动的网站提供更强大。

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>启用动态数据的现有项目

在.NET Framework 3.5 SP1 中提供的动态数据功能引入新功能如下所示：

- 字段模板 – 这些将基于数据类型的模板提供的数据绑定控件。 字段模板提供更简单的方法，以自定义控件的外观的数据比使用为每个字段的模板字段。
- 验证 – 动态数据允许您在数据类中使用属性来指定为必填的字段、 范围检查、 类型检查、 模式匹配使用正则表达式，之类的常见方案的验证和自定义验证。 由数据控件强制执行验证。

但是，这些功能具有以下要求：

- 数据访问层必须基于实体框架或 LINQ to SQL。
- 唯一的数据源控件支持这些功能已*EntityDataSource*或*LinqDataSource*控件。
- 功能需要以具有以便具有需要支持功能的所有文件使用的动态数据或动态数据实体模板已创建的 Web 项目。

ASP.NET 4 中的动态数据支持的主要目标是能够对任何 ASP.NET 应用程序的新功能的动态数据。 下面的示例演示可以利用动态数据功能中的现有的页面的控件的标记。

[!code-aspx[Main](overview/samples/sample91.aspx)]

在代码页中，必须要使这些控件的动态数据支持添加以下代码：

[!code-csharp[Main](overview/samples/sample92.cs)]

当*GridView*控件处于编辑模式时，动态数据自动验证输入的数据将采用正确的格式。 如果不是这样，将显示一条错误消息。

此功能还提供其他好处，如能够指定默认值插入模式。 不包含动态数据，以实现一个字段，字段的默认值将附加到事件，因此必须查找控件 (使用*FindControl*)，并将其值设置。 在 ASP.NET 4 中， *EnableDynamicData*调用支持第二个参数，可让你将任何字段默认值传递对象，如本示例中所示：

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>声明性 DynamicDataManager 控件语法

*DynamicDataManager*管理已得到增强，以便可以将其配置以声明方式，与在 ASP.NET 中，而不是仅在代码的大多数控件一样。 标记*DynamicDataManager*控件的外观如下例所示：

[!code-aspx[Main](overview/samples/sample94.aspx)]

此标记启用动态数据行为中引用的 GridView1 控件*DataControls*部分*DynamicDataManager*控件。

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>实体模板

实体模板提供自定义而无需创建自定义页的布局的数据的新方法。 页上，模板使用*FormView*控件 (而不是*说明*控件，在早期版本的动态数据中的页模板中使用) 和*DynamicEntity*呈现实体模板的控件。 这样，可以更好地控制由动态数据呈现的标记。

以下列表显示包含实体模板的新项目目录布局：

[!code-console[Main](overview/samples/sample95.cmd)]

`EntityTemplate`目录包含有关如何显示数据模型对象的模板。 默认情况下，通过使用呈现对象`Default.ascx`模板，它提供类似的创建的标记的标记*说明*使用 ASP.NET 3.5 SP1 中的动态数据控件。 下面的示例演示的标记`Default.ascx`控件：

[!code-aspx[Main](overview/samples/sample96.aspx)]

可以编辑默认模板，以更改整个站点的外观和感觉。 有用于显示模板、 编辑和插入操作。 新的模板可以添加基于数据对象的名称以更改只是一种类型的对象的外观和感觉。 例如，你可以添加以下模板：

[!code-console[Main](overview/samples/sample97.cmd)]

该模板可能包含以下标记：

[!code-aspx[Main](overview/samples/sample98.aspx)]

在页面上显示新的实体模板，应使用新*DynamicEntity*控件。 在运行时，此控件将被替换实体模板的内容。 下面的标记演示*FormView*中控制`Detail.aspx`页使用实体模板的模板。 请注意*DynamicEntity*标记中的元素。

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>新的字段模板的 Url 和电子邮件地址

ASP.NET 4 引入了两个新的内置字段模板，`EmailAddress.ascx`和`Url.ascx`。 这些模板用于字段标记为*EmailAddress*或*Url*与*DataType*属性。 有关*EmailAddress*对象，该字段显示为超链接，通过创建*mailto:*协议。 当用户单击此链接时，打开用户的电子邮件客户端，并创建主干消息。 对象类型化为*Url*显示为普通的超链接。

下面的示例演示如何将标记字段。

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>创建与 DynamicHyperLink 控件的链接

动态数据使用.NET Framework 3.5 SP1，以控制用户访问网站时，请参阅最终用户的 Url 中添加新路由功能。 新*DynamicHyperLink*控件，可以轻松生成的动态数据站点中的页面链接。 下面的示例演示如何使用*DynamicHyperLink*控件：

[!code-aspx[Main](overview/samples/sample101.aspx)]

此标记创建一个链接，指向的列表页`Products`表基于在中定义的路由`Global.asax`文件。 该控件自动使用动态数据页基于默认表名称。

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>对数据模型中的继承支持

实体框架和 LINQ to SQL 可在其数据模型中支持继承。 此示例可能具有的数据库`InsurancePolicy`表。 它可能还包含`CarPolicy`和`HousePolicy`具有相同的字段的表`InsurancePolicy`，然后添加更多的字段。 动态数据已被修改，以了解数据模型中继承的对象，并可以支持继承的表中的基架。

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>对多对多关系 （仅用于实体框架） 的支持

实体框架具有丰富的支持供实现通过上公开为一个集合的关系的表之间的多对多关系*实体*对象。 新`ManyToMany.ascx`和`ManyToMany_Edit.ascx`字段模板已添加以用于显示和编辑涉及多对多关系的数据提供支持。

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>控件显示和支持枚举的新特性

*DisplayAttribute*已添加要向你提供对字段的显示方式的其他控制。 *DisplayName*在早期版本的动态数据允许你更改用作字段标题的名称的属性。 新*DisplayAttribute*类允许您指定用于显示字段，如顺序显示的字段以及字段是否将用作筛选器的更多选项。 该属性还提供了用于中的标签的名称的独立控制*GridView*控制中使用的名称*说明*控制，该字段的帮助文本和水印用于字段 （如果字段接受输入的文本）。

*EnumDataTypeAttribute*类已添加，以便你可以将字段映射到枚举。 当此属性应用于字段时，你将指定枚举类型。 动态数据使用新`Enumeration.ascx`字段模板来创建 UI，用于显示和编辑枚举值。 模板将映射到枚举中的名称来自数据库的值。

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>增强了对筛选器支持

动态数据 1.0 附带了内置布尔值列和外键列的筛选器。 筛选器不允许您指定是否显示它们，或以何种顺序显示它们。 新*DisplayAttribute*对列是否显示为筛选器控制属性解决了这两向您提供这些问题并在何种顺序显示。

其他增强功能就是确保筛选支持已重新[编写为使用新](#0.2__QueryExtender "_QueryExtender") Web 窗体功能。 这样可以创建筛选器，而无需将与用于筛选器数据源控件的知识。 这些扩展，以及筛选器也已转换为模板控件，该编辑器可以添加新的。 最后， *DisplayAttribute*前面所述的类允许默认筛选器中重写，在相同的方式*UIHint*允许列中被重写的默认字段模板。

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Visual Studio 2010 Web 开发改进

Visual Studio 2010 中的 web 开发已得到增强的更大 CSS 兼容性，通过 HTML 和 ASP.NET 标记代码段和新动态 IntelliSense JavaScript 提高工作效率。

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>改进了的 CSS 兼容性

在 Visual Studio 2010 的 Visual Web Developer 设计器已更新，提高 CSS 2.1 标准合规性。 在设计器更好地保留的 HTML 源文件的完整性和 Visual Studio 的早期版本比更加可靠。 实质上，体系结构改进还进行该更好地实现中呈现、 布局和可维护性将来增强功能。

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>HTML 和 JavaScript 代码段

在 HTML 编辑器中，IntelliSense 会自动完成标记名称。 IntelliSense 代码段功能整个标记以及更多设置为自动完成。 在 Visual Studio 2010 中，IntelliSense 代码段将支持 JavaScript，以及 C# 和 Visual Basic 中，这在 Visual Studio 的早期版本中受支持。

Visual Studio 2010 包括超过 200 个代码段，帮助你自动完成常见 ASP.NET 和 HTML 标记，包括必需的属性 (如 runat ="server") 和特定于标记的公共属性 (如*ID*， *DataSourceID*， *ControlToValidate*，和*文本*)。

你可以下载其他代码段，也可以编写自己的代码段，用于封装各种你或你的团队使用的常见任务的标记的块。

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>JavaScript IntelliSense 增强功能

在 Visual 2010 中，JavaScript IntelliSense 已经过重新设计提供甚至更丰富的编辑体验。 IntelliSense 现在识别对象具有已动态生成的方法如*registerNamespace*和其他 JavaScript 框架使用的类似技术。 若要分析的脚本的大型库，以及与很少或没有处理延迟一起显示 IntelliSense，性能得到了提高。 若要支持几乎所有第三方库并支持各种编码风格已大大增加兼容性。 现在其作为你键入并立即利用 IntelliSense 分析文档注释。

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>使用 Visual Studio 2010 的 web 应用程序部署

在 ASP.NET 开发人员部署 Web 应用程序时，它们通常发现他们遇到如下问题：

- 部署到共享的宿主站点需要技术如 FTP，可能会很慢。 此外，你必须手动执行任务，如运行 SQL 脚本，配置数据库，并且你必须更改 IIS 设置，例如为应用程序配置的虚拟目录文件夹。
- 在企业环境中，除了部署 Web 应用程序文件，管理员通常必须修改 ASP.NET 配置文件和 IIS 设置。 数据库管理员必须运行 SQL 脚本，以获得运行的应用程序数据库的一系列。 此类安装耗费大量精力，通常需要小时才能完成，并且必须仔细了介绍。

Visual Studio 2010 包括的技术，解决这些问题，能够让你无缝地部署 Web 应用程序。 这些技术之一是 IIS 的 Web 部署工具 (MsDeploy.exe)。

Visual Studio 2010 中的 web 部署功能包括以下主要方面：

- Web 打包
- Web.config 转换
- 数据库部署
- 单击发布为 Web 应用程序

下列各节提供有关这些功能的详细信息。

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Web 打包

Visual Studio 2010 使用 MSDeploy 工具来创建你的应用程序，即所谓的压缩 (.zip) 文件*Web 包*。 包文件包含有关你的应用程序以及以下内容的元数据：

- IIS 设置，包括应用程序池设置、 错误页设置等。
- 实际 Web 内容，其中包括网页、 用户控件、 静态内容 （映像和 HTML 文件） 和等等。
- SQL Server 数据库架构和数据。
- 安全证书，要安装在 GAC、 注册表设置等组件。

可以复制到任何服务器，然后通过使用 IIS 管理器手动安装 Web 包。 或者，用于自动部署，包可以安装通过使用命令行命令或通过使用部署 Api。

Visual Studio 2010 提供内置的 MSBuild 任务和用于创建 Web 包的目标。 有关详细信息，请参阅[ASP.NET Web 应用程序项目部署概述](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx)MSDN 网站上和[为什么应创建 Web 包的 10 + 20 原因](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html)Vishal Joshi 博客上。

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Web.config 转换

对于 Web 应用程序部署，Visual Studio 2010 引入了[XML 文档转换 (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)，这是一种功能，可以将转换`Web.config`文件从开发设置到生产环境设置。 转换设置在名为的转换文件中指定`web.debug.config`， `web.release.config`，依次类推。 （这些文件的名称匹配 MSBuild 配置。）转换文件包括只需更改，你需要对部署进行`Web.config`文件。 通过使用简单的语法指定所做的更改。

下面的示例显示的一部分`web.release.config`可能生成的发布配置的部署的文件。 该示例中的替换关键字在部署期间指定*connectionString*中的节点`Web.config`文件将替换为示例中列出的值。

[!code-xml[Main](overview/samples/sample102.xml)]

有关详细信息，请参阅[Web 应用程序项目部署的 Web.config 转换语法](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx)MSDN 上<a id="0.2_a"></a>网站和[Web 部署： Web.Config 转换](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)Vishal Joshi 博客上。

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>数据库部署

Visual Studio 2010 部署包可以包含 SQL Server 数据库上的依赖关系。 作为包定义的一部分，为源数据库提供的连接字符串。 当你创建的 Web 包时，Visual Studio 2010 创建 SQL 脚本的数据库架构和 （可选） 的数据，然后将它们添加到包。 你还可以提供自定义 SQL 脚本，并指定它们应运行在服务器的序列。 在部署时，你可以提供适合于目标服务器中; 的连接字符串部署过程然后使用此连接字符串来运行脚本，创建数据库架构并添加数据。

此外，通过使用一键式发布，您可以配置部署，以便应用程序发布到远程共享宿主站点时，直接发布你的数据库。 有关详细信息，请参阅[如何： 部署数据库与 Web 应用程序项目](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx)MSDN 网站上和[使用 VS 2010 数据库部署](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)Vishal Joshi 博客上。

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>单击发布为 Web 应用程序

Visual Studio 2010 还允许你使用 IIS 的远程管理服务发布到远程服务器的 Web 应用程序。 你可以为你的托管帐户或测试服务器或过渡服务器创建的发布配置文件。 每个配置文件可以安全地保存相应的凭据。 你可以随后部署到任何目标服务器通过使用 Web 一键式一次单击发布工具栏。 使用 Visual Studio 2010，你还可以通过使用 MSBuild 命令行发布。 这允许您配置您的团队生成环境，要包括在持续集成模型中的发布。

有关详细信息，请参阅[如何： 部署 Web 应用程序项目使用一键式发布和 Web 部署](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx)MSDN 网站上和[Web 1 单击发布使用 VS 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) Vishal Joshi 博客上。 若要在 Visual Studio 2010 中查看有关 Web 应用程序部署的视频演示文稿，请参阅[的 Web 开发人员预览版的 VS 2010](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) Vishal Joshi 博客上。

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>资源

以下网站提供有关 ASP.NET 4 和 Visual Studio 2010 的其他信息。

- [ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) -MSDN 网站上的 ASP.NET 4 的官方文档。
- [https://www.asp.net/](https://www.asp.net/) -ASP.NET 团队自己的网站。
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx)和[ASP.NET 动态数据内容映射](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx)-联机资源 ASP.NET 团队站点上和中的 ASP.NET 动态数据有关的正式文档。
- [https://www.asp.net/ajax/](../../ajax/index.md) -ASP.NET Ajax 开发主 Web 资源。
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) -Visual Web 开发人员团队博客，Visual Studio 2010 中包括有关功能的信息。
- [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) -ASP.NET 的预览版本的主 Web 资源。

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>免责声明

这是一份初稿，并可能在本文所述软件最终商业发布之前进行大幅更改。

本文所含信息代表 Microsoft Corporation 对截至发布之日所讨论问题持有的当前观点。 由于 Microsoft 必须对不断变化的市场情况作出响应，所以不应将本文解释为是 Microsoft 做出的承诺，Microsoft 并不保证所提供的任何信息在公布之日后的准确性。

本白皮书仅用于提供信息。 MICROSOFT 对本文档中的信息不做任何明示、暗示或法定的担保。

遵守所有适用的著作权法是用户的责任。 未经 Microsoft Corporation 明确的书面许可，不得出于任何目的或以任何形式或任何手段（电子、机械、影印、记录或其他方法）复制本文档的任何部分，或者将其存储或引入检索系统，或者将其进行传播。受版权法保护的权利不受此限制。

对于本文档中的主题，Microsoft 可能具有专利、专利申请、商标、版权或其他知识产权。 除非 Microsoft 的任何书面许可协议明确提出，否则，本文档的提供并不表示 Microsoft 已将这些专利、商标、版权或其他知识产权的任何许可权限授予您。

除非另行说明，示例公司、 组织、 产品、 域名、 电子邮件地址、 徽标、 人物、 地点和事件此处所述虚构，是的无意与任何真实的公司、 组织、 产品、 域名、 电子邮件地址、 徽标、 人员、 位置或事件旨在或妄加推断。

© 2009 Microsoft Corporation。 保留所有权利。

Microsoft 和 Windows 是 Microsoft Corporation 在美国和/或其他国家/地区的注册商标或商标。

此处提到的真实公司和产品的名称可能是其各自所有者的商标。
