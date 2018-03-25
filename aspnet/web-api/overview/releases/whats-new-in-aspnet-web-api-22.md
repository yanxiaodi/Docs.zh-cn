---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: "什么是 ASP.NET Web API 2.2 中的新增功能 |Microsoft 文档"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 400329dd852ca3c527387ee45e3e902b725e771b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-22"></a>什么是 ASP.NET Web API 2.2 中的新增功能
====================
通过[Microsoft](https://github.com/microsoft)

本主题介绍什么是用于 ASP.NET Web API 2.2 的新功能。

- [下载](#download)
- [文档](#documentation)
- [ASP.NET Web API 2.2 中的新增功能](#newf)

    - [OData v4](#OData)
    - [属性路由改进](#ARI)
    - [Web API 客户端支持 Windows Phone 8.1](#phone)
- [已知的问题和重大更改](#known-issues)
- [Bug 修复](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1 章](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Microsoft.AspNet.WebAPI 5.2.3 Beta](#523)

<a id="download"></a>
## <a name="download"></a>下载

运行时功能将发布为 NuGet 库上的 NuGet 包。 所有运行时包按照[语义版本控制](http://semver.org/)规范。 最新的 ASP.NET Web API 2.2 包具有以下版本:"5.2.0"。 你可以安装或更新这些包通过[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/)。 版本还包括在 NuGet 上的相应本地化的包。

你可以安装或使用 NuGet 程序包管理器控制台更新到已发布的 NuGet 程序包：

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>文档

教程和有关 ASP.NET Web API 2.2 的其他信息可以从 ASP.NET web 站点 ([https://www.asp.net/web-api](../../index.md))。

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>ASP.NET Web API 2.2 中的新增功能

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

此版本添加了对 OData v4 协议的支持。 有关详细信息，请参阅[Web API OData v4 文档。](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

下面是一些主要功能和 OData v4 的更改：

- [对 OData 模型中的别名属性的支持](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [支持 ComplexTypeAttribute、 AssociationAttribute、 TimesTampAttribute 和 ConcurrencyCheckAttribute ODataConventionModelBuilder 中](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [提供能够提供有关操作的友好标题](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- 将与 ODL UriParser 集成
- 支持[枚举](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/)，[包含](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/)和[单一实例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- 强制转换为基元类型的支持
- [添加了 OData 函数的支持](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [支持的函数调用的参数的别名](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [在模型中支持 camel 大小写的命名约定](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- 对在 $filter cast() 的支持
- 打开的复杂类型的支持
- 删除的 EntitySetController 和 AsyncEntitySetController
- [更改到 $ref $link](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [添加的属性路由支持](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- 使用 OData 核心库 6.4.0

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>属性路由改进

现在的属性路由提供调用 IDirectRouteProvider，允许完全控制属性路由如何发现和配置一个扩展点。 IDirectRouteProvider 负责提供的操作和以及关联的路由信息以指定哪些路由配置所需的这些操作完全的控制器的列表。 调用 MapAttributes/MapHttpAttributeRoutes 时，可能指定 IDirectRouteProvider 实现。

自定义 IDirectRouteProvider 是最简单的扩展了默认实现，DefaultDirectRouteProvider。 此类提供单独可重写虚拟方法，若要更改用于发现属性、 创建的路由项和发现的路由前缀和区域前缀的逻辑。

以下是一些示例上你可以使用此新的扩展点执行的操作：

1. 支持路由属性的继承

    示例:

    此处 like"/ 10/api/值"的请求已成功将返回"成功： 10"

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. 按照您喜欢的某些约定提供默认路由名称的属性路由。 默认情况下，属性路由不会自动创建的属性路由的名称。
3. 路由表中的它们结束之前，请修改在一个中心位置的属性路由的路由模板。

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Web API Client Support for Windows Phone 8.1

现在可以使用 Web API 客户端 NuGet 包来实现你的 Web API 客户端逻辑，如果目标 Windows Phone 8.1 或通用应用程序中。

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的问题和重大更改

本部分介绍已知的问题和 ASP.NET Web API 2.2 中的重大更改。

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>模型生成器

问题： 重载的函数可能不会公开为 FunctionImport

如果有两个重载的函数，并且它们还 FunctionImport 然后请求 ~/GetAllConventionCustomers(CustomerName={customerName}) 导致 System.InvalidOperationException 如下所示。

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

解决方法： 此问题的解决方法是作为 FunctionImports 添加这两个函数重载。

#### <a name="odata-routing"></a>OData 路由

包括 URL 的字符串文本编码斜杠 (%2f) 和 backslash(%5C) 导致 404 错误时的 OData 资源路径中使用它们。

例如，字符串文本可以用作 OData 资源路径中的函数的参数或实体集的密钥值。

/Employees/Total.GetCount(Name='Name%2F')

/Employees('Name%5C')

如果服务收到此类请求主机将取消转义那些转义序列，然后再将它们传递到 Web API 运行时。 这样可防止攻击，如下所示：  
  
 http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:

这会导致 Web API OData 堆栈，以将返回 404 错误 （未找到）。 若要防止出现此错误，你的客户端应使用斜杠 （%252f) 的双重转义序列和反斜杠 （%255 C)。 这不会发生的查询字符串，例如，/Employees？ $filter = Name eq '名称 %2f

**请注意取消转义的斜杠 （'/') 和反斜杠 （'） 不是合法中的 OData 资源路径字符串文字。斜杠应显示仅为路径分隔符和反斜杠应根本不显示在 OData 资源路径中。（都在 OData 查询字符串的某些部分中可用。）**

解决方法： 你无法重写 DefaultODataPathHandler 进行实际分析它们之前转义的斜杠和字符串文本中的反斜杠的分析方法。 你可以找到这种方法的示例。

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[查询]

[Queryable] 属性已弃用。 新的 OData v3 应用程序应使用**System.Web.Http.OData.EnableQueryAttribute**。

**ODataHttpConfigurationExtensions.EnableQuerySupport**扩展方法现在将添加**EnableQueryAttribute**到全局筛选器集合。 如果任何控制器有**[Queryable]**属性，调用`config.EnableQuerySupport()`将导致**[Queryable]**属性失败

若要解决此问题的建议的方法是替换的所有实例**QueryableAttribute**与**System.Web.Http.OData.EnableQueryAttribute**。

备用解决方法是使用你的 Web API 配置中的以下代码：

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>属性路由

问题： 模型绑定的复杂类型也使用 FromUri 特性修饰的行为有所不同时使用的属性路由。

以下链接在跟踪问题，并且也有一种解决方法有关的详细信息。  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

问题： 到 5.2.0 与项目的基架 MVC/Web API 包导致 5.1.2 打包以一种是在项目中尚不存在

有关 ASP.NET MVC 5.2 更新 NuGet 程序包不会更新 Visual Studio 工具，例如 ASP.NET 基架或 ASP.NET Web 应用程序项目模板。 它们使用以前版本的 ASP.NET 运行时包 (例如 5.1.2 Update 2 中)。 因此，ASP.NET 基架将安装所需的包的以前版本 (例如 5.1.2 Update 2 中)，如果它们尚不存在在你项目中可用。 但是，在 Visual Studio 2013 RTM 或 Update 1 中的 ASP.NET 基架不会覆盖你的项目中的最新包。 如果您使用 ASP.NET Web API 2.2 或 ASP.NET MVC 5.2 到更新你的项目的包之后的基架，请确保 Web API 和 ASP.NET MVC 的版本都一致。

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Bug 修复和细微的功能更新

此版本还包括几个 bug 修复和细微的功能更新。 您可以找到完整的列表：

- [5.2 包](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1 章

Microsoft.AspNet.OData 5.2.1 章程序包包含 NuGet 依赖项更新，但是没有 bug 修复。 利用此更新，是不再严格的依赖项的 Microsoft.OData.Core 6.4.0，但是其中一个可以升级到 6.4.0 和 7.0.0 之间的任何版本。

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

在此版本中，我们已创建依赖关系更改为`Json.Net 6.0.4`。 有关详细信息中的此版本的新增`Json.NET`，请参阅[Json.NET 6.0 版本 4-JSON 合并，依赖关系注入](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection)。 此版本不具有任何其他新功能或 bug 修复的 Web API。 随后，我们已更新我们拥有取决于此新版本的 Web API 的所有其他从属包。

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Microsoft.AspNet.WebAPI 5.2.3 Beta

你可以阅读有关版本[此处](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)。 此版本包含仅 bug 修复。 你可以使用[此查询](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)若要查看此版本中修复的问题的列表。
