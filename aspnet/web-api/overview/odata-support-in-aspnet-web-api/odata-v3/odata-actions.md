---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: "支持在 ASP.NET Web API 2 OData 操作 |Microsoft 文档"
author: MikeWasson
description: "在 OData 中，操作是一种方法中添加不很容易定义为对实体的 CRUD 操作的服务器端行为。 操作的一些用途包括： 实现..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a>支持在 ASP.NET Web API 2 OData 操作
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> 在 OData 中，*操作*是一种方法中添加不很容易定义为对实体的 CRUD 操作的服务器端行为。 操作的一些用途包括：
> 
> - 实现复杂的事务处理。
> - 同时对多个实体进行操作。
> - 允许仅对某些属性的实体的更新。
> - 将信息发送到服务器未在实体中定义。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - Web API 2
> - OData 版本 3
> - Entity Framework 6


## <a name="example-rating-a-product"></a>示例： 评级产品

在此示例中，我们想要让用户速率产品，并为每个产品的平均级别，则公开。 在数据库中，我们将存储评级，与产品的列表。

下面是我们可能使用来表示实体框架中的级别的模型：

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

但是，我们不希望客户端为 POST`ProductRating`对象添加到"分级"收藏。 直观地说，分级是与产品集合中，关联，客户端应只需发布的分级值。

因此，而不是使用的普通 CRUD 操作，我们定义一个客户端可以调用的操作产品上。 在 OData 术语中，操作是*绑定*到产品实体。

>操作在服务器上产生负面影响。 因此，它们会调用使用 HTTP POST 请求。 操作可以具有参数和返回类型，所述的服务元数据。 客户端在请求正文中发送参数和服务器发送响应正文中返回的值。 若要调用的"速率产品"操作，客户端，将发送到 URI 如下所示的 POST:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

POST 请求中的数据是只需产品级别：

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>声明实体数据模型中的操作

在 Web API 配置中，将添加到实体数据模型 (EDM) 的操作：

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

此代码定义"RateProduct"为可以在产品实体执行一个操作。 它还声明了操作将**int**参数名为"分级"，并返回**int**值。

## <a name="add-the-action-to-the-controller"></a>将操作添加到控制器

"RateProduct"操作绑定到产品实体中。 若要实现此操作，添加一个名为方法`RateProduct`到产品控制器：

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

请注意的方法名称匹配 EDM 中操作的名称。 该方法具有两个参数：

- *密钥*： 速率的产品密钥。
- *参数*： 操作参数值的字典。

如果你使用的默认路由约定，按键参数必须命名为"关键"。 还有一点需要包括**[FromOdataUri]**特性，如所示。 此特性告知 Web API，若要在分析请求 URI 中的键时使用 OData 语法规则。

使用*参数*字典，以获取操作参数：

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

如果客户端发送的操作参数中正确设置格式的值**ModelState.IsValid**为 true。 在这种情况下，你可以使用**ODataActionParameters**为了获取参数值的字典。 在此示例中，`RateProduct`操作采用一个名为"评级"的单个参数。

## <a name="action-metadata"></a>操作元数据

若要查看的服务元数据，将 GET 请求发送到 /odata/$ 元数据。 下面是声明的元数据的部分`RateProduct`操作：

[!code-xml[Main](odata-actions/samples/sample7.xml)]

**FunctionImport**元素声明的操作。 大多数字段都一目了然，但两个数据类型值得注意：

- **IsBindable**意味着操作可以调用针对目标实体至少一些时间。
- **IsAlwaysBindable**意味着始终可以针对目标实体调用的操作。

区别是，某些操作始终可供客户端，但其他操作可能依赖于该实体的状态。 例如，假设你定义的"购买"操作。 你仅可以购买的项时库存量。 如果该项是 stock 外，客户端不能调用该操作。

当定义 EDM，**操作**方法创建一个始终可绑定的操作：

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

我将讨论 not 的始终-可绑定的操作 (也称为*暂时性*操作) 本主题中更高版本。

## <a name="invoking-the-action"></a>调用操作

现在让我们看看客户端将如何调用此操作。 假设在客户端需要向产品具有 ID 2 的分级 = 4。 下面是一个示例请求消息，使用 JSON 格式的请求正文：

[!code-console[Main](odata-actions/samples/sample9.cmd)]

下面是响应消息：

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>绑定到实体集的操作

在前面的示例中，操作绑定到单个实体： 客户端进行分级单个产品。 你还可以将操作绑定到实体的集合。 只需进行以下更改：

在 EDM 中，将操作添加到实体的**集合**属性。

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

在控制器方法中，省略*密钥*参数。

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

现在客户端时，将调用产品的实体集上的操作：

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>使用集合参数的操作

操作可以接受值的集合的参数。 在 EDM 中，使用**CollectionParameter&lt;T&gt;** 若要将参数声明。

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

这会将声明一个名为"分级"采用的集合参数**int**值。 在控制器方法中，你仍获取参数值**ODataActionParameters**对象，但现在的值是**ICollection&lt;int&gt;** 值：

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>暂时性的操作

在"RateProduct"示例中，用户可以始终对产品评估，因此操作将始终可用。 但某些操作依赖于该实体的状态。 例如，在视频租赁服务中，"签出"操作并不总是可用。 （它取决于一份该视频是否可用。）此类型的操作称为*暂时性*操作。

在服务元数据，暂时性操作具有**IsAlwaysBindable**等于 false。 即实际默认值，因此元数据将如下所示：

[!code-xml[Main](odata-actions/samples/sample16.xml)]

此处为什么这很重要： 如果操作是暂时性的服务器需要告诉客户端操作时可用。 通过在实体中包括指向操作的执行此操作。 下面是一个示例，用于电影实体：

[!code-console[Main](odata-actions/samples/sample17.cmd)]

"#CheckOut"属性包含到签出操作的链接。 如果操作不可用，服务器会忽略链接。

若要声明 EDM 中的暂时性操作，请调用**TransientAction**方法：

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

此外，你必须提供一个函数，返回为给定实体的操作链接。 设置此函数通过调用**HasActionLink**。 您可以编写函数作为 lambda 表达式：

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

可用的操作，如果 lambda 表达式将返回到操作的链接。 OData 序列化程序包括此链接时其序列化实体。 当操作不可用时，函数将返回`null`。

## <a name="additional-resources"></a>其他资源

[OData 操作示例](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
