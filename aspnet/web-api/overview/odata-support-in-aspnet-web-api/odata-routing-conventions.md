---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: "ASP.NET Web API 2 中的路由约定 Odata |Microsoft 文档"
author: MikeWasson
description: "本指南介绍了 Web API 使用 OData 终结点的路由约定。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/31/2013
ms.topic: article
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 0ab99dd443040b90ffefd2f5b9261a63b91e9463
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>ASP.NET Web API 2 中的路由约定 Odata
====================
通过[Mike Wasson](https://github.com/MikeWasson)

> 本指南介绍了 Web API 使用 OData 终结点的路由约定。


当 Web API 获取 OData 请求时，它将请求映射到的控制器名称和操作名称。 映射基于 HTTP 方法和 URI。 例如，`GET /odata/Products(1)`映射到`ProductsController.GetProduct`。

在本文的第 1 部分中，我介绍了内置的 OData 路由约定。 这些约定专为 OData 终结点，并且它们将默认的 Web API 路由系统。 (在调用时，会发生情况替换**MapODataRoute**。)

在第 2 部分，我将介绍如何添加自定义路由的约定。 当前的内置约定未涵盖整个范围的 OData Uri，但你可以扩展它们以处理其他情况。

- [内置路由约定](#conventions)
- [自定义路由约定](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>内置路由约定

我介绍了 Web API 中的 OData 路由约定之前，最好先了解 OData Uri。 [OData URI](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/)组成：

- 服务根
- 资源路径
- 查询选项

![](odata-routing-conventions/_static/image1.png)

用于路由，重要的部分是资源路径。 资源路径分为段。 例如，`/Products(1)/Supplier`具有三个段：

- `Products`引用名为"产品"实体集。
- `1`是否为实体键，从组中选择单个实体。
- `Supplier`是一个导航属性，选择相关的实体。

因此出供应商产品 1 会选取此路径。

> [!NOTE]
> OData 路径段始终不对应 URI 段。 例如，"1"被视为路径段。


**控制器名称。** 控制器名称始终被派生自的实体集的资源路径的根目录。 例如，如果资源路径是`/Products(1)/Supplier`，Web API 查找名为的控制器`ProductsController`。

**操作名称。** 操作名称派生自的路径段以及实体数据模型 (EDM) 中，为以下表格中列出。 在某些情况下，你具有的操作名称的两种方法。 例如，"Get"或&quot;GetProducts&quot;。

**查询实体**

| 请求 | 示例 URI | 操作名称 | 示例操作 |
| --- | --- | --- | --- |
| 获取 /entityset | / 产品 | GetEntitySet 或 Get | GetProducts |
| 获取 /entityset(key) | /Products(1) | GetEntityType 或 Get | GetProduct |
| 获取 /entityset （密钥）/强制转换 | /Products(1)/Models.Book | GetEntityType 或 Get | GetBook |

有关详细信息，请参阅[创建只读 OData 终结点](odata-v3/creating-an-odata-endpoint.md)。

**创建、 更新和删除实体**

| 请求 | 示例 URI | 操作名称 | 示例操作 |
| --- | --- | --- | --- |
| 文章 /entityset | / 产品 | PostEntityType 或 Post | PostProduct |
| PUT /entityset(key) | /Products(1) | PutEntityType 或 Put | PutProduct |
| PUT /entityset （密钥）/强制转换 | /Products(1)/Models.Book | PutEntityType 或 Put | PutBook |
| 修补程序 /entityset(key) | /Products(1) | PatchEntityType 或修补程序 | PatchProduct |
| 修补程序 /entityset （密钥）/强制转换 | /Products(1)/Models.Book | PatchEntityType 或修补程序 | PatchBook |
| DELETE /entityset(key) | /Products(1) | DeleteEntityType 或删除 | DeleteProduct |
| DELETE /entityset(key)/cast | /Products(1)/Models.Book | DeleteEntityType 或删除 | DeleteBook |

**查询的导航属性**

| 请求 | 示例 URI | 操作名称 | 示例操作 |
| --- | --- | --- | --- |
| GET /entityset （密钥） / 导航 | /Products(1)/Supplier | GetNavigationFromEntityType or GetNavigation | GetSupplierFromProduct |
| 获取 /entityset （密钥）/转换/导航 | /Products(1)/Models.Book/Author | GetNavigationFromEntityType or GetNavigation | GetAuthorFromBook |

有关详细信息，请参阅[使用实体关系](odata-v3/working-with-entity-relations.md)。

**创建和删除链接**

| 请求 | 示例 URI | 操作名称 |
| --- | --- | --- |
| POST /entityset(key)/$links/navigation | /Products(1)/$links/Supplier | CreateLink |
| PUT 的 /entityset （密钥） / $links/导航 | /Products(1)/$links/Supplier | CreateLink |
| DELETE /entityset(key)/$links/navigation | /Products(1)/$links/Supplier | DeleteLink |
| DELETE /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$links/Suppliers(1) | DeleteLink |

有关详细信息，请参阅[使用实体关系](odata-v3/working-with-entity-relations.md)。

**属性**

*需要 Web API 2*

| 请求 | 示例 URI | 操作名称 | 示例操作 |
| --- | --- | --- | --- |
| GET /entityset （密钥） / 属性 | /Products(1)/Name | GetPropertyFromEntityType 或 GetProperty | GetNameFromProduct |
| 获取 /entityset （密钥）/转换/属性 | /Products(1)/Models.Book/Author | GetPropertyFromEntityType 或 GetProperty | GetTitleFromBook |

**操作**

| 请求 | 示例 URI | 操作名称 | 示例操作 |
| --- | --- | --- | --- |
| POST /entityset(key)/action | /Products(1)/Rate | ActionNameOnEntityType 或 ActionName | RateOnProduct |
| POST /entityset(key)/cast/action | /Products(1)/Models.Book/CheckOut | ActionNameOnEntityType 或 ActionName | CheckOutOnBook |

有关详细信息，请参阅[OData 操作](odata-v3/odata-actions.md)。

**方法签名**

下面是方法签名的一些规则：

- 如果路径包含一个密钥，该操作应具有一个名为参数*密钥*。
- 如果路径包含转换的导航属性的密钥，该操作应具有一个名为参数*relatedKey*。
- 修饰*密钥*和*relatedKey*参数**[FromODataUri]**参数。
- POST 和 PUT 请求需要实体类型的参数。
- PATCH 请求需要类型的参数**增量&lt;T&gt;**，其中*T*是实体类型。

作为参考，此处是显示每个内置的 OData 路由约定的方法签名的示例。

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>自定义路由约定

当前的内置约定并未涵盖所有可能的 OData Uri。 你可以通过实现来添加新约定**IODataRoutingConvention**接口。 此接口有两种方法：

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController**返回控制器的名称。
- **SelectAction**返回操作的名称。

对于这两种方法，如果约定不适用于该请求，该方法应返回 null。

**ODataPath**参数表示的已分析的 OData 资源路径。 它包含的列表 **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** 实例，每个段的资源路径的一个。 **ODataPathSegment**是一个抽象类; 每个段类型由派生自的类**ODataPathSegment**。

**ODataPath.TemplatePath**属性是一个字符串，表示串联所有的路径段。 例如，如果的 URI 是`/Products(1)/Supplier`，路径模板是&quot;~/entityset/key/navigation&quot;。 请注意，段不直接对应 URI 段。 例如，实体键 (1) 表示为其自身**ODataPathSegment**。

通常，实现**IODataRoutingConvention**执行下列任务：

1. 比较路径模板以查看是否此约定适用于当前的请求。 如果不适用，返回 null。
2. 如果要将约定应用，使用属性**ODataPathSegment**控制器和操作名称派生的实例。
3. 对于操作，将添加到路由字典，其中应绑定到操作参数 （通常实体键） 的任何值。

我们来看具体示例的信息。 内置路由约定不支持导航集合索引。 换而言之，没有 uri 约定如下所示：

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

下面是查询的自定义的路由约定来处理这种类型。

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

注意：

1. 我派生自**EntitySetRoutingConvention**，这是因为**SelectController**在该类的方法适合于此新的路由约定。 这意味着我不需要重新实现**SelectController**。
2. 约定适用仅对 GET 请求中，并且仅当路径模板时&quot;~/entityset/key/navigation/key&quot;。
3. 操作名称是&quot;获取 {EntityType}&quot;，其中*{EntityType}*是导航集合的类型。 例如， &quot;GetSupplier&quot;。 您可以使用任何您喜欢的命名约定和 #8212;只需确保你与匹配的控制器操作。
4. 将名为的两个参数的操作*密钥*和*relatedKey*。 (有关某些预定义的参数名称的列表，请参阅[ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx)。)

下一步向路由约定的列表添加新的约定。 发生这种情况在配置期间，如下面的代码中所示：

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

下面是一些其他示例路由约定的有用来研究：

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

当然 Web API 本身是开放源代码，以便你可以查看[源代码](http://aspnetwebstack.codeplex.com/)的内置路由约定。 这些定义在**System.Web.Http.OData.Routing.Conventions**命名空间。
