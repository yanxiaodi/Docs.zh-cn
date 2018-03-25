---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: "ASP.NET Web API 2.1 的新增功能 |Microsoft 文档"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: cc5dc111d88cc7dae6a4a93203317fa0769d5427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 的新增功能
====================
通过[Microsoft](https://github.com/microsoft)

本主题介绍什么是用于 ASP.NET Web API 2.1 的新功能。

- [下载](#download)
- [文档](#documentation)
- [ASP.NET Web API 2.1 中的新增功能](#new-features)

    - [全局错误处理](#global-error)
    - [属性路由改进](#attribute-routing)
    - [帮助页的改进](#help-page)
    - [IgnoreRoute 支持](#ignoreroute)
    - [BSON 媒体类型格式化程序](#bson)
    - [更好地支持异步筛选器](#async-filters)
    - [查询分析格式库客户端](#query-parsing)
- [已知的问题和重大更改](#known-issues)
- [Bug 修复](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>下载

运行时功能将发布为 NuGet 库上的 NuGet 包。 所有运行时包按照[语义版本控制](http://semver.org/)规范。 最新的 ASP.NET Web API 2.1 RTM 包具有以下版本:"5.1.2"。 你可以安装或更新这些包通过[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/)。 版本还包括在 NuGet 上的相应本地化的包。

你可以安装或使用 NuGet 程序包管理器控制台更新到已发布的 NuGet 程序包：

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>文档

教程和有关 ASP.NET Web API 2.1 RTM 的其他信息可以从 ASP.NET web 站点 ([https://www.asp.net/web-api](../../index.md))。

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 中的新增功能

<a id="global-error"></a>
### <a name="global-error-handling"></a>全局错误处理

未经处理的异常的行为可以自定义和通过一个中央机制，现在会记录所有未经处理的异常。

框架支持多个异常记录器，全都请参阅的未经处理的异常和在其中发生，如时要处理的请求的上下文信息。

例如，下面的代码使用 System.Diagnostics.TraceSource 记录所有未经处理的异常：

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

你也可以替换默认异常处理程序，以便你可以完全自定义当未经处理的异常时发送的 HTTP 响应消息时发生。

我们提供了[示例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt)记录所有未经处理的异常，通过流行的 ELMAH framework。

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>属性路由改进

现在的属性路由支持约束，启用版本控制和基于标头的路由选择。 此外，许多方面的属性路由是现在可通过自定义**IDirectRouteFactory**接口和**RouteFactoryAttribute**类。 路由前缀现在是可扩展的通过**IRoutePrefix**接口和**RoutePrefixAttribute**类。

我们提供了[示例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt)，它使用约束动态筛选通过 api 版本 HTTP 标头的控制器。

<a id="help-page"></a>
### <a name="help-page-improvements"></a>帮助页的改进

Web API 2.1 包括以下增强功能到[API 帮助页](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- 参数或返回类型的操作的各个属性的文档。
- 数据模型批注的文档。

帮助页的 UI 设计也已更新，以适应这些更改。

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>IgnoreRoute 支持

Web API 2.1 支持忽略 Web API 路由，通过一组中的 URL 模式**IgnoreRoute**上的扩展方法**HttpRouteCollection**。 这些方法会导致 Web API，若要忽略任何与匹配的指定的模板的 Url，使该主机可应用其他处理，如果适用。

下面的示例将忽略以开头的 Uri&quot;内容&quot;段：

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>BSON 媒体类型格式化程序

Web API 现在支持[BSON](http://bsonspec.org/)连网格式，客户端和服务器。

若要在服务器端上启用 BSON，添加**BsonMediaTypeFormatter**到格式化程序集合：

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

下面是如何.NET 客户端可以使用 BSON 格式：

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

我们提供了[示例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt)演示客户端和服务器端。

有关详细信息，请参阅[BSON 支持在 Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>更好地支持异步筛选器

Web API 现在支持轻松创建异步执行的筛选器。 此功能是有用的是你的筛选器需要执行一个异步操作，例如数据库的访问。 以前，若要创建一个异步筛选器，你必须实现筛选器接口你自己，因为筛选器的基类仅公开同步方法。 现在你可以重写虚拟`On*Async`方法筛选器的基类。

例如: 

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

**AuthorizationFilterAttribute**， **ActionFilterAttribute**，和**ExceptionFilterAttribute**类都支持异步中 Web API 2.1。

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>查询分析格式库客户端

以前， **System.Net.Http.Formatting**支持分析和更新 URI 查询对于服务器端代码，但等效的可移植库缺少此功能。 在 Web API 2.1 中，客户端应用程序可以现在轻松分析和更新查询字符串。

下面的示例演示如何分析、 修改和生成 URI 查询。 （示例中显示为简单起见一个控制台应用程序。）

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的问题和重大更改

本部分介绍已知的问题和 ASP.NET Web API 2.1 RTM 中的重大更改。

### <a name="attribute-routing"></a>属性路由

在属性路由匹配项中的多义性现在还报告错误，而不是无需选择第一个匹配项。

属性路由禁止使用*{controller}*参数，并从使用*{action}*路由的参数放置在操作上。 这些参数将很有可能会导致多义性。

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>到 5.1 包在中使用结果尚不存在项目中的那些 5.0 包项目的基架 MVC/Web API

在更新 NuGet 包适用于 ASP.NET Web API 2.1 RTM 不更新的 Visual Studio 工具，如 ASP.NET 基架或 ASP.NET Web 应用程序项目模板。 它们使用以前版本的 ASP.NET 运行时包 (5.0.0.0)。 因此，ASP.NET 基架将安装所需的包的以前版本 (5.0.0.0)，如果它们尚不存在在你项目中可用。 但是，在 Visual Studio 2013 RTM 或 Update 1 中的 ASP.NET 基架不会覆盖你的项目中的最新包。

如果您使用 ASP.NET Web API 2.1 或 ASP.NET MVC 5.1 在更新包后的基架，请确保 Web API 和 MVC 的版本都一致。

### <a name="type-renames"></a>类型重命名

从 RC 到 2.1 RTM 一些用于属性路由扩展的类型已重命名。

| 旧的类型名称 (2.1 RC) | 新的类型名称 (2.1 RTM) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>异常筛选器不 unwrap 聚合异步操作中引发的异常

以前，如果异步操作引发了**AggregateException**，异常筛选器将解包该异常，和**OnException**将获取使用基异常。 在 2.1 中，异常筛选器不会不 unwrap，和**OnException**获取原始**AggregateException**。

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Bug 修复

此版本还包括多项 bug 修复。 您可以找到完整的列表：

- [5.1.0 包](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 包](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

5.1.2 包包含 IntelliSense 更新但没有 bug 修复。
