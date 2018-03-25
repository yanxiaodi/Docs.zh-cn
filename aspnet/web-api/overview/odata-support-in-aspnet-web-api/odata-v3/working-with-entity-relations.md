---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: "与 Web API 2 OData v3 支持实体关系 |Microsoft 文档"
author: MikeWasson
description: "大多数的数据集定义的实体之间的关系： 客户下了订单;书有作者;产品具有供应商。 使用 OData，可以通过导航客户端..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>与 Web API 2 OData v3 支持实体关系
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> 大多数的数据集定义的实体之间的关系： 客户下了订单;书有作者;产品具有供应商。 使用 OData，可以通过实体关系导航客户端。 提供一种产品，可以找到供应商。 你还可以创建或删除关系。 例如，你可以设置产品的供应商。
> 
> 本教程演示如何在 ASP.NET Web API 中支持这些操作。 本教程以教程为基础[创建与 Web API 2 OData v3 终结点](creating-an-odata-endpoint.md)。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - Web API 2
> - OData 版本 3
> - Entity Framework 6


## <a name="add-a-supplier-entity"></a>添加供应商实体

首先，我们需要将新的实体类型添加到我们 OData 数据源。 我们将添加`Supplier`类。

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

此类实体键使用的字符串。 在实践中，这可能是不太常见比使用整数键。 但值得看到 OData 处理整数除了其他密钥类型的方式。

接下来，我们将通过添加创建某一关系`Supplier`属性`Product`类：

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

添加新**DbSet**到`ProductServiceContext`类，以便将包括实体框架`Supplier`数据库表中的。

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

在 WebApiConfig.cs 中，将添加到 EDM 模型的"供应商"实体：

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>导航属性

若要获取产品供应商，客户端将发送 GET 请求：

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

此处"供应商"是导航属性上`Product`类型。 在这种情况下，`Supplier`引用为单个项，但一个导航属性还可以返回的集合 （一个一对多或多对多关系）。

若要支持此请求，添加以下方法`ProductsController`类：

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

*密钥*参数是该产品的键。 该方法将返回相关的实体与用于在此情况下，`Supplier`实例。 方法名称和参数名称都很重要。 一般情况下，如果导航属性名为"X"，你需要添加一个名为"GetX"方法。 方法必须采用名为的参数"*密钥*"的父项的数据类型匹配。

还有一点需要包括**[FromOdataUri]**属性中*密钥*参数。 此特性告知 Web API，若要在分析请求 URI 中的键时使用 OData 语法规则。

## <a name="creating-and-deleting-links"></a>创建和删除链接

OData 支持两个实体之间创建或删除关系。 在 OData 术语中，关系是"链接"。 每个链接都具有处理该窗体的 URI*实体*/$links /*实体*。 例如，供应商产品之间的链接如下所示：

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

若要创建新的链接，客户端将 POST 请求发送到链接 URI 中。 请求正文是目标实体的 URI。 例如，假定有与"CTSO"的键的供应商提供。 若要创建从"Product(1)"到"Supplier('CTSO')"的链接，客户端发送的请求如下所示：

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

若要删除的链接，客户端将 DELETE 请求发送到链接 URI 中。

**创建链接**

若要启用客户端来创建产品供应商的链接，添加以下代码到`ProductsController`类：

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

此方法采用三个参数：

- *密钥*： 到父实体 （产品） 的密钥
- *navigationProperty*： 导航属性的名称。 在此示例中，唯一有效的导航属性为"供应商"。
- *链接*： 相关实体的 OData URI。 此值是从请求正文中提取的。 例如，链接 URI 可能是"`http://localhost/odata/Suppliers('CTSO')`，这意味着供应商 id = CTSO。

该方法使用该链接来查找供应商。 如果找到匹配的供应商，则该方法将设置`Product.Supplier`属性，并将结果保存到数据库。

最困难的部分分析链接 URI。 基本上，你需要模拟与该 URI 发送 GET 请求的结果。 下面的 helper 方法演示如何执行此操作。 该方法调用 Web API 路由过程和获取返回**ODataPath**表示分析的 OData 路径的实例。 对于链接 URI，段之一应是实体键。 （如果没有，客户端发送了错误的 URI。）

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**删除链接**

若要删除的链接，将添加到下面的代码`ProductsController`类：

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

在此示例中，导航属性是单个`Supplier`实体。 如果导航属性是一个集合，要删除的链接的 URI 必须包括相关实体的键。 例如: 

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

此请求删除客户 1 的顺序为 1。 在这种情况下，DeleteLink 方法将具有以下签名：

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

*RelatedKey*参数指定了为相关实体的键。 因此，在你`DeleteLink`方法，查找由主实体*密钥*参数，查找相关的实体由*relatedKey*参数，并删除关联。 具体取决于你的数据模型，你可能需要实施这两个版本`DeleteLink`。 Web API 将调用基于请求 URI 上的正确版本。
