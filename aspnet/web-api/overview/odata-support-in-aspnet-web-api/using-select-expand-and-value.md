---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: "使用 $select，$expand、 和中 ASP.NET Web API 2 OData $value |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>使用 $select，$expand、 和中 ASP.NET Web API 2 OData $value
====================
通过[Mike Wasson](https://github.com/MikeWasson)

为 $expand、 $select，$value 中和选项 OData web API 2 添加了支持。 这些选项允许客户端控制从服务器重新获取的表示形式。

- **$expand**导致相关的实体以内联形式包含在响应中的。
- **$select**选择要在响应中包含的属性的子集。
- **$value**获取属性的原始值。

## <a name="example-schema"></a>示例架构

对于本文中，我将使用定义三个实体的 OData 服务： 产品、 供应商和类别。 每个产品均具有一个类别和一个供应商。

![](using-select-expand-and-value/_static/image1.png)

下面是定义实体模型的 C# 类：

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

请注意，`Product`类定义导航属性`Supplier`和`Category`。 `Category`类定义每个类别中的产品的导航属性。

若要创建此架构的 OData 终结点，使用 Visual Studio 2013 的基架中, 所述[在 ASP.NET Web API 中创建 OData 终结点](odata-v3/creating-an-odata-endpoint.md)。 为产品、 类别和供应商添加单独的控制器。

## <a name="enabling-expand-and-select"></a>启用 $展开和 $select

在 Visual Studio 2013 中，Web API OData 基架创建一个控制器，该自动支持 $expand 和 $select。 对于引用，这里有支持 $ 的要求可展开和 $select 控制器中的。

对于集合，控制器`Get`方法必须返回**IQueryable**。

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

对于单个实体，返回**SingleResult&lt;T&gt;**，其中 T 为**IQueryable** ，其中包含零个或一个实体。

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

此外，修饰你`Get`方法**[Queryable]**特性，如前面的代码段中所示。 或者，调用**EnableQuerySupport**上**HttpConfiguration**在启动时的对象。 (有关详细信息，请参阅[启用 OData 查询选项](supporting-odata-query-options.md#enable)。)

## <a name="using-expand"></a>使用 $展开

当查询 OData 实体或集合时，默认响应不包括相关的实体。 例如，以下是类别实体集的默认响应：

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

如你所见，响应将不包括任何产品，即使 Category 实体具有产品导航链接。 但是，客户端可以使用 $扩展以获取每个类别的产品的列表。 $Expand 选项将会填入请求的查询字符串：

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

现在服务器将包含每个类别，各类别的内联的产品。 下面是响应负载：

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

请注意，"value"数组中的每个条目包含产品列表。

$ Expand 选项的导航属性以展开的采用逗号分隔列表。 以下请求将展开类别和产品供应商。

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

下面是响应正文：

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

你可以展开导航属性的多个的级别。 下面的示例包含的类别的所有产品以及每个产品供应商。

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

下面是响应正文：

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

默认情况下，Web API 限制为 2 的最大扩展深度。 可阻止客户端发送类似的复杂请求`$expand=Orders/OrderDetails/Product/Supplier/Region`，这可能很低效查询并创建大型的响应。 若要覆盖默认值，设置**MaxExpansionDepth**属性**[Queryable]**属性。

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

有关详细信息，有关 $expand 选项，请参阅[展开 ($expand) 系统查询选项](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand)正式的 OData 文档中。

## <a name="using-select"></a>使用 $select

$Select 选项指定要包含在响应正文中的属性的子集。 例如，若要获取的名称和每个产品的价格，请使用以下查询：

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

下面是响应正文：

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

你可以组合 $select 和 $expand 在同一查询中。 请确保要包含在 $select 选项中的扩展的属性。 例如，以下请求将获取的产品名称和供应商。

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

下面是响应正文：

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

你还可以选择中的扩展属性的属性。 以下请求将展开产品，且选择类别名称和产品名称。

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

下面是响应正文：

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

有关 $select 选项的详细信息，请参阅[选择系统查询选项 ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select)正式的 OData 文档中。

## <a name="getting-individual-properties-of-an-entity-value"></a>获取单个实体 ($value) 属性

有两种方式的 OData 客户端可以从实体中获取的单个属性。 客户端可以获取 OData 格式中的值或获取属性的原始值。

以下请求 OData 格式中获取的属性。

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

下面是 JSON 格式的示例响应：

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

若要获取属性的原始值，请将 $value 追加到 URI:

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

下面是响应。 请注意，内容类型不"文本/plain"JSON。

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

若要在 OData 控制器中支持这些查询，将添加一个名为方法`GetProperty`，其中`Property`是属性的名称。 例如，要获取的 Name 属性的方法将被命名为`GetName`。 该方法应返回该属性的值：

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
