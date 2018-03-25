---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: "路由和 ASP.NET Web API 中的操作选择 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 997582263bd48590b74434ee0ffc6be928fa1e08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>路由和 ASP.NET Web API 中的操作选择
====================
通过[Mike Wasson](https://github.com/MikeWasson)

本指南介绍了 ASP.NET Web API 将 HTTP 请求路由到在控制器上的特定操作的方式。

> [!NOTE]
> 有关路由的高级概述，请参阅[路由在 ASP.NET Web API 中](routing-in-aspnet-web-api.md)。


本文探讨路由过程的详细信息。 如果你创建的 Web API 项目，并且不会获得一些请求的查找路由按你期望的方式，希望本文将帮助。

路由具有三个主要阶段：

1. 到路由模板对 URI 进行匹配。
2. 选择一个控制器。
3. 选择操作。

可以使用你自己的自定义行为来替换该过程的某些部分。 在本文中，我将介绍的默认行为。 在结束时，我注意可以在其中自定义行为的位置。

## <a name="route-templates"></a>路由模板

路由模板看起来类似于 URI 路径，但它可以具有占位符值，指示用大括号：

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

当您创建一个路由时，你可以为某些或所有占位符提供默认值：

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

你还可以提供约束，限制 URI 段匹配占位符的方式：

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

框架会尝试匹配模板的 URI 路径段。 在模板中的文本必须完全匹配。 一个占位符匹配任何值，除非指定约束。 框架不匹配的 URI，如主机名或查询参数的其他部分。 框架将选择的第一个路由的 URI 相匹配的路由表中。

有两个特殊的占位符:"{controller}"和"{action}"。

- "{controller}"提供的控制器的名称。
- "{action}"提供的操作的名称。 在 Web API 的常用的约定是省略"{action}"。

### <a name="defaults"></a>默认值

如果你提供默认值，路由将与缺少这些段的 URI 匹配。 例如:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

URI"`http://localhost/api/products`"与此路由匹配。 "{类别}"段分配默认值"all"。

### <a name="route-dictionary"></a>路由字典

如果框架找到的匹配项的 uri，它将创建一个字典，其中对于每个占位符包含的值。 键是占位符名称，不包括大括号。 值都作为从 URI 路径或默认值。 字典存储在**IHttpRouteData**对象。

在此路由匹配的阶段，特殊"{controller}"和"{action}"占位符就像其他占位符一样对待。 它们只存储在字典中，其他值。

默认值可以具有的特殊值**RouteParameter.Optional**。 如果占位符获取分配此值的值不会添加到路由字典。 例如:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

对于 URI 路径"api/products"中，将包含路由字典：

- 控制器:"products"
- 类别:"全部"

对于"api/产品/toys/123"，但是，路由字典将包含：

- 控制器:"products"
- 类别:"toys"
- id:"123"

默认值还可以在路由模板中包含一个值，不会出现任何位置。 如果路由与匹配，则会将该值存储在字典中。 例如:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

如果该 URI 路径是"根/api/8"，字典将包含两个值：

- 控制器:"客户"
- id: "8"

## <a name="selecting-a-controller"></a>选择一个控制器

控制器所选内容由**IHttpControllerSelector.SelectController**方法。 此方法采用**HttpRequestMessage**实例并返回**HttpControllerDescriptor**。 由提供的默认实现**DefaultHttpControllerSelector**类。 此类使用简单的算法：

1. 在密钥"控制器"路由字典中查找。
2. 采用此键的值并将"控制器"获取控制器类型名称的字符串追加。
3. 查找具有此类型名称的 Web API 控制器。

例如，如果路由字典包含键 / 值对"控制器"="products"，则控制器类型是"ProductsController"。 如果没有任何匹配的类型或多个匹配项，框架会将错误返回到客户端。

步骤 3， **DefaultHttpControllerSelector**使用**IHttpControllerTypeResolver**接口来获取 Web API 控制器类型的列表。 默认实现**IHttpControllerTypeResolver**返回 （a） 实现的所有公共类**IHttpController**，（b） 是不抽象，而 （c） 有以"Controller"结尾的名称。

## <a name="action-selection"></a>操作选择

选择后控制器，则框架将选择该操作通过调用**IHttpActionSelector.SelectAction**方法。 此方法采用**HttpControllerContext**并返回**HttpActionDescriptor**。

由提供的默认实现**ApiControllerActionSelector**类。 若要选择一项操作，它会查看以下：

- 请求的 HTTP 方法。
- 在路由模板中，如果存在"{action}"占位符。
- 在控制器上的操作的参数。

之前查看选择算法，我们需要了解有关控制器操作的一些事项。

**在控制器上的方法被认为"操作"？** 当选择某个操作，框架仅查看公共实例方法在控制器上。 此外，它不包括["特殊 name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname)方法 （构造函数、 事件、 运算符重载等） 和从继承方法**ApiController**类。

**HTTP 方法。** 框架仅选择匹配的请求，按以下方式确定的 HTTP 方法的操作：

1. 你可以使用属性中指定的 HTTP 方法： **AcceptVerbs**， **HttpDelete**， **HttpGet**， **HttpHead**， **HttpOptions**， **HttpPatch**， **HttpPost**，或**HttpPut**。
2. 否则，如果控制器方法的名称以"Get"、"Post"、"Put"、"删除"，"Head"、"选项"或"补丁日"开头，然后按照约定操作支持该 HTTP 方法。
3. 如果上述任何，该方法支持 POST。

**参数绑定。** 参数绑定是如何 Web API 创建参数的值。 下面是参数绑定的默认规则：

- 从 URI 中获取简单类型。
- 从请求正文中获取复杂类型。

简单类型包括的所有[.NET Framework 基元类型](https://msdn.microsoft.com/library/system.type.isprimitive)，加上**DateTime**，**十进制**， **Guid**，**字符串**，和**TimeSpan**。 对于每个操作，最多一个参数可以读取请求正文。

> [!NOTE]
> 它是可以重写默认绑定规则。 请参阅[WebAPI 参数绑定实质上的](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)。


该背景，下面是操作选择算法。

1. 匹配的 HTTP 请求方法在控制器上创建的所有操作的列表。
2. 如果路由字典具有的"操作"条目，删除其名称与此值不匹配的操作。
3. 尝试匹配到 URI 的操作参数，如下所示： 

    1. 对于每个操作，可以获取是一种简单类型，其中绑定的 URI 中获取的参数的参数的列表。 排除可选参数。
    2. 从此列表中，尝试路由字典中或在 URI 查询字符串中查找每个参数名称的匹配项。 匹配项区分大小，并不依赖于参数顺序。
    3. 选择列表中的每个参数的 URI 中具有匹配的其中一项操作。
    4. 如果多该一个操作不符合这些标准，选取大多数的参数匹配的一个。
4. 忽略操作**[NonAction]**属性。

步骤 #3 是可能最容易混淆。 基本理念都是从 URI、 从请求正文中，或从自定义绑定，参数可以获取其值。 对于来自 URI 的参数，我们想要确保 URI 实际包含该参数，在 （通过路由字典） 的路径或查询字符串中的值。

例如，考虑以下操作：

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

*Id*参数绑定到的 URI。 因此，此操作只能匹配一个 URI，包含"id"，路由字典中或查询字符串中的值。

可选参数是一个例外，因为它们是可选。 对于一个可选参数，它将是确定，如果绑定无法从 URI 中获得的值。

复杂类型适用不同的原因的异常。 通过自定义绑定的复杂类型只能绑定到的 URI。 但在这种情况下，框架不能预先知道是否会将参数绑定到特定 URI。 若要找出，它将需要调用绑定。 选择算法的目的是从静态的说明，然后再调用任何绑定选择一项操作。 因此，从匹配算法会排除复杂类型。

选择操作后，会调用所有参数绑定。

摘要:

- Action 必须与匹配请求的 HTTP 的方法。
- 如果存在，操作名称必须与匹配路由字典中的"操作"条目。
- 对于操作的每个参数，如果参数则来自 URI，然后参数名称必须找路由字典中或在 URI 查询字符串。 （可选参数和复杂类型的参数被排除。）
- 尝试最大参数数量匹配。 最佳匹配可能是不带任何参数的方法。

## <a name="extended-example"></a>扩展的示例

路由：

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

控制器:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

HTTP 请求：

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>将路由匹配

URI 匹配名为"DefaultApi"的路由。 路由字典包含以下各项：

- 控制器:"products"
- id: "1"

路由字典不包含查询字符串参数、"版本"和"详细信息"，但这些仍会被视为期间操作选择。

### <a name="controller-selection"></a>控制器所选内容

从路由字典中的"控制器"条目，该控制器类型是`ProductsController`。

### <a name="action-selection"></a>操作选择

HTTP 请求是 GET 请求。 支持 GET 的控制器操作`GetAll`， `GetById`，和`FindProductsByName`。 路由字典不包含"操作"的条目，因此我们无需与操作名称匹配。

接下来，我们尝试匹配的参数名称用于操作，只查看的 GET 操作。

| 操作 | 到匹配项的参数 |
| --- | --- |
| `GetAll` | 无 |
| `GetById` | "id" |
| `FindProductsByName` | "name" |

请注意，*版本*参数`GetById`不被视为，因为它是一个可选参数。

`GetAll`方法完全匹配。 `GetById`方法也匹配项，因为路由字典包含"id"。 `FindProductsByName`方法不匹配。

`GetById`方法 wins，因为它与一个参数，而不是为指定参数匹配`GetAll`。 使用以下参数值调用的方法：

- *id* = 1
- *版本*= 1.5

请注意，即使*版本*不使用选择算法中参数的值来自 URI 查询字符串。

## <a name="extension-points"></a>扩展点

Web API 提供对路由过程的某些部分的扩展点。

| 接口 | 描述 |
| --- | --- |
| **IHttpControllerSelector** | 选择控制器。 |
| **IHttpControllerTypeResolver** | 获取控制器类型的列表。 **DefaultHttpControllerSelector**从此列表中选择控制器类型。 |
| **IAssembliesResolver** | 获取项目程序集的列表。 **IHttpControllerTypeResolver**接口使用此列表以找到控制器的类型。 |
| **IHttpControllerActivator** | 创建新的控制器实例。 |
| **IHttpActionSelector** | 选择的操作。 |
| **IHttpActionInvoker** | 将调用该操作。 |

若要为任何这些接口提供您自己的实现，请使用**服务**集合**HttpConfiguration**对象：

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
