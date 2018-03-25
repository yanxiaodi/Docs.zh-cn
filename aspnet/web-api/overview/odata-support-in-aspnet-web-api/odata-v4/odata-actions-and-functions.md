---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: "操作和函数在 OData v4 使用 ASP.NET Web API 2.2 |Microsoft 文档"
author: MikeWasson
description: "在 OData 中，操作和函数都可添加轻松未定义为对实体的 CRUD 操作的服务器端行为的方法。 本教程演示如何..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>操作和使用 ASP.NET Web API 2.2 中 OData v4 函数
====================
通过[Mike Wasson](https://github.com/MikeWasson)

> 在 OData 中，操作和函数都可添加轻松未定义为对实体的 CRUD 操作的服务器端行为的方法。 本教程演示如何向 OData v4 终结点，使用 Web API 2.2 添加操作和函数。 本教程以教程为基础[创建 OData v4 终结点使用 ASP.NET Web API 2](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - Web API 2.2
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>教程版本
> 
> 有关 OData 版本 3，请参阅[ASP.NET Web API 2 中的 OData 操作](../odata-v3/odata-actions.md)。


之间的差异*操作*和*函数*是操作可能会有副作用，并且函数不这样做。 操作和函数可返回数据。 操作的一些用途包括：

- 复杂的事务处理。
- 同时对多个实体进行操作。
- 允许仅对某些属性的实体的更新。
- 不是实体的发送数据。

函数可用于直接于实体或集合中返回不对应的信息。

操作 （或函数） 可以针对单个实体或集合。 在 OData 术语中，这是*绑定*。 此外可以让&quot;未绑定&quot;操作/函数，作为服务上的静态操作调用。

## <a name="example-adding-an-action"></a>示例： 添加操作

让我们定义要对产品评估的操作。

> [!NOTE]
> 本教程基于本教程[创建 OData v4 终结点使用 ASP.NET Web API 2](create-an-odata-v4-endpoint.md)


首先，添加`ProductRating`模型来表示分级。

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

此外添加**DbSet**到`ProductsContext`类，以便 EF 将在数据库中创建分级表。

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>将操作添加到 EDM

在 WebApiConfig.cs 中，添加以下代码：

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

**EntityTypeConfiguration.Action**方法将操作添加到实体数据模型 (EDM)。 **参数**方法指定操作的类型化的参数。

此代码还设置 EDM 的命名空间。 命名空间很重要，因为操作 URI 包含完全限定的操作名称：

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> 在典型的 IIS 配置中，此 URL 中的点则会导致 IIS 返回 404 错误。 可以通过将以下部分添加到你的 Web.Config 文件来解决此问题：

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>操作中添加一个控制器方法

若要启用&quot;速率&quot;操作，添加以下方法`ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

请注意的方法名称匹配的操作名称。 **[HttpPost]**属性指定该方法是 HTTP POST 方法。

若要调用的操作，客户端发送 HTTP POST 请求如下所示：

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

&quot;速率&quot;操作绑定到产品实例，因此操作 URI 是追加到 URI 的实体的完全限定的操作名称。 (回想一下，我们设置 EDM 命名空间为&quot;ProductService&quot;，因此完全限定的操作名称是&quot;ProductService.Rate&quot;。)

请求正文以 JSON 负载形式包含的操作参数。 Web API 会自动将转换为 JSON 负载**ODataActionParameters**对象，它是只是参数值的字典。 使用此字典访问控制器方法中的参数。

如果客户端发送在正确的操作参数的值格式化**ModelState.IsValid**为 false。 检查控制器方法中的此标志，如果返回错误**IsValid**为 false。

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>示例： 添加函数

现在让我们添加一个 OData 函数，返回成本最高的产品。 如之前，第一步将该函数添加到 EDM 中。 在 WebApiConfig.cs 中，添加以下代码。

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

在这种情况下，将函数绑定到产品集合，而不是各个实例的产品。 客户端通过发送 GET 请求调用此函数：

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

下面是此函数在控制器方法：

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

请注意的方法名称匹配的函数名称。 **[HttpGet]**属性指定方法是 HTTP GET 方法。

下面是 HTTP 响应：

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>示例： 添加未绑定的函数

前面的示例已绑定到集合的函数。 在下一个示例，我们将创建*未绑定*函数。 未绑定的函数是作为服务上的静态操作调用的。 此示例中的函数将返回的增值税给定邮政编码。

在 WebApiConfig 文件中，添加到 EDM 的函数：

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

请注意，我们正在呼叫**函数**直接基于**ODataModelBuilder**，而不是实体类型或集合。 这将告知模型生成器函数是未绑定。

以下是实现了该函数的控制器方法：

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

它并不重要放置在此方法的 Web API 控制器。 无法将其放入`ProductsController`，或定义单独的控制器。 **[ODataRoute]**属性定义用于函数的 URI 模板。

下面是一个示例客户端请求：

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

HTTP 响应：

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
