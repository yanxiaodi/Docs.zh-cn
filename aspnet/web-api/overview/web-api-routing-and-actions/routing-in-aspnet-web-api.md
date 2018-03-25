---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: "ASP.NET Web API 中的路由 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: aa0ecc96029051fef6a81ac08f7fcf52a24de59c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="routing-in-aspnet-web-api"></a>ASP.NET Web API 中的路由
====================
通过[Mike Wasson](https://github.com/MikeWasson)

本指南介绍了 ASP.NET Web API 将 HTTP 请求路由到控制器的方式。

> [!NOTE]
> 如果你熟悉使用 ASP.NET MVC，Web API 路由是非常类似于 MVC 路由。 主要区别是 Web API 使用的 HTTP 方法，而不是 URI 路径，选择的操作。 你还可以使用 MVC 样式中 Web API 的路由。 本文不会采用 ASP.NET MVC 任何知识。


## <a name="routing-tables"></a>路由表

在 ASP.NET Web API 中，*控制器*是处理 HTTP 请求的类。 控制器的公共方法在调用*操作方法*或只需*操作*。 当 Web API 框架收到请求时，它将请求路由到的操作。

若要确定要调用的操作，框架将使用*路由表*。 Web API 的 Visual Studio 项目模板创建的默认路由：

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

此路由定义在 WebApiConfig.cs 文件中，放置在应用程序\_启动目录：

![](routing-in-aspnet-web-api/_static/image1.png)

有关详细信息**WebApiConfig**类，请参阅[配置 ASP.NET Web API](../advanced/configuring-aspnet-web-api.md)。

如果你自承载 Web API，则必须直接在上设置路由表**HttpSelfHostConfiguration**对象。 有关详细信息，请参阅[自承载 Web API](../older-versions/self-host-a-web-api.md)。

路由表中的每个条目包含*路由模板*。 Web API 的默认路由模板&quot;api / {controller} / {id}&quot;。 在此模板中， &quot;api&quot;是文本路径段和 {controller} 和 {id} 占位符的变量。

当 Web API 框架收到 HTTP 请求时，它会尝试匹配对路由表中的路由模板之一的 URI。 如果没有路由与匹配，则客户端收到 404 错误。 例如，下面的 Uri 匹配默认路由：

- / api/联系人
- /api/contacts/1
- /api/products/gizmo1

但是，下面的 URI 不匹配，因为它缺少&quot;api&quot;段：

- / 联系人/1

> [!NOTE]
> 在路由中使用"api"的原因是使用 ASP.NET MVC 路由避免冲突。 这种方式，可以进行&quot;/联系&quot;转到一个 MVC 控制器，和&quot;/api/联系人&quot;转到 Web API 控制器。 当然，如果你不喜欢此约定，你可以更改默认的路由表。

一旦找到匹配的路由，Web API 选择控制器和操作：

- 若要查找控制器，Web API 添加&quot;控制器&quot;为的值*{controller}*变量。
- 若要查找操作，Web API 的 HTTP 方法，查找并随后查找名称开头的 HTTP 方法名称的操作。 例如，使用 GET 请求，Web API 中寻找开头的操作&quot;获取...&quot;，如&quot;GetContact&quot;或&quot;GetAllContacts&quot;。 此约定仅适用于获取、 POST、 PUT 和删除方法。 可以通过在你的控制器上使用属性来启用其他 HTTP 方法。 我们将更高版本看到举例说明的。
- 在路由模板中，其他占位符变量如*{id}、*映射到操作参数。

让我们看一个示例。 假设定义以下控制器：

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

下面是一些可能的 HTTP 请求，以及每个调用的操作：

| HTTP 方法 | URI 路径 | 操作 | 参数 |
| --- | --- | --- | --- |
| GET | api/产品 | GetAllProducts | *（无）* |
| GET | api/产品/4 | GetProductById | 4 |
| DELETE | api/产品/4 | DeleteProduct | 4 |
| 发布 | api/产品 | *（没有匹配项）* |  |

请注意， *{id}*段的 URI，如果存在，将映射到*id*操作的参数。 在此示例中，控制器定义两个 GET 方法，其中一个有*id*参数，另一个不带任何参数。

此外，请注意，POST 请求将失败，因为控制器不会定义&quot;Post...&quot;方法。

## <a name="routing-variations"></a>路由的变体

上一节所述 ASP.NET Web API 的基本路由机制。 本部分介绍一些变体。

### <a name="http-methods"></a>HTTP 方法

而不是使用 HTTP 方法的命名约定，你可以显式指定操作的 HTTP 方法的修饰具有的操作方法**HttpGet**， **HttpPut**， **HttpPost**，或**HttpDelete**属性。

在下面的示例中，FindProduct 方法映射到 GET 请求：

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

若要允许多个 HTTP 方法对于一个操作，或允许 GET、 PUT、 POST 和 DELETE 以外的 HTTP 方法，使用**AcceptVerbs**属性，它采用 HTTP 方法的列表。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>路由由操作名称

与默认路由模板中，Web API 使用的 HTTP 方法来选择操作。 但是，你还可以创建一个路由，其中 URI 中包含的操作名称：

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

在此路由模板中， *{action}*参数名称在控制器上的操作方法。 这种样式的路由，与使用属性来指定允许的 HTTP 方法。 例如，假设你的控制器具有以下方法：

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

在这种情况下，"api/产品/详细信息/1"的 GET 请求将映射为详细信息方法。 这种样式的路由类似于 ASP.NET MVC，并且可能适合 RPC 样式 API。

你可以使用重写的操作名称**ActionName**属性。 在以下示例中，有两个操作映射到&quot;缩略图产品/api / /*id*。必须有一个支持 GET 和其他支持文章：

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>非操作

若要阻止用作一项操作正在调用的方法，使用**NonAction**属性。 这指示 framework 此方法不是操作，即使它否则将匹配的路由规则。

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>其他阅读材料

本主题提供路由的高级的视图。 有关详细信息，请参阅[路由和操作选择](routing-and-action-selection.md)，用于描述完全如何框架路由到的 URI 匹配、 选择一个控制器，然后选择要调用的操作。
