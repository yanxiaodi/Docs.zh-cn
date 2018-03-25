---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: "支持 ASP.NET Web API 2 中的 OData 查询选项 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 004c029db6f01627f7cadff26aaf5554ce2b93a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>ASP.NET Web API 2 中支持的 OData 查询选项
====================
通过[Mike Wasson](https://github.com/MikeWasson)

OData 定义可以用于修改 OData 查询参数。 客户端的请求 URI 查询字符串中发送这些参数。 例如，若要对结果进行排序，客户端使用 $orderby 参数：

`http://localhost/Products?$orderby=Name`

这些参数调用 OData 规范*查询选项*。 你可以在你的项目 （&） #8212; 启用任何 Web API 控制器的 OData 查询选项控制器不必是一个 OData 终结点。 这样，可以方便地添加如筛选和排序的任何 Web API 应用程序的功能。

启用查询选项，请阅读主题之前[OData 安全指南](odata-security-guidance.md)。

- [启用 OData 查询选项](#enable)
- [示例查询](#examples)
- [服务器驱动的分页](#server-paging)
- [限制查询选项](#limiting_query_options)
- [直接调用查询选项](#ODataQueryOptions)
- [查询验证](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>启用 OData 查询选项

Web API 支持以下 OData 查询选项：

| 选项 | 描述 |
| --- | --- |
| $expand | 展开相关的实体内联。 |
| $filter | 筛选结果，并基于布尔条件。 |
| $inlinecount | 指示服务器在响应中包含的匹配的实体的总数。 （适用于服务器端分页。） |
| $orderby | 对结果进行排序。 |
| $select | 选择要在响应中包含哪些属性。 |
| $skip | 跳过前 n 个结果。 |
| $top | 返回仅的第一个 n 的结果。 |

若要使用 OData 查询选项，必须显式启用它们。 可以全局启用整个应用程序，或为特定控制器或特定操作启用它们。

若要全局启用 OData 查询选项，请调用**EnableQuerySupport**上**HttpConfiguration**在启动时的类：

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

**EnableQuerySupport**方法启用返回任何控制器操作为全局查询选项**IQueryable**类型。 如果你不希望对整个应用程序启用的查询选项，你可以启用它们特定控制器操作的添加**[Queryable]**属性设为的操作方法。

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>示例查询

此部分显示的可能使用的 OData 查询选项的查询的类型。 有关查询选项的特定详细信息，请参阅位于 OData 文档[www.odata.org](http://www.odata.org/)。

有关 $展开和 $select，请参阅[使用 $select，$expand、 和 ASP.NET Web API OData 中的 $value](using-select-expand-and-value.md)。

**客户端驱动的分页**

大型实体集的情况下，客户端可能会想要限制结果数。 例如，客户端可能在一次，以获取下一结果页的"下一步"链接显示 10 个条目。 若要执行此操作，客户端，请使用 $top 和 $skip 选项。

`http://localhost/Products?$top=10&$skip=20`

$Top 选项提供要返回的最大项数并 $skip 选项提供要跳过的项数。 前面的示例提取条目 21 到 30。

**筛选**

$Filter 选项允许客户端通过将应用的布尔表达式筛选结果。 筛选器表达式是功能非常强大;它们包括逻辑和算术运算符、 字符串函数和日期函数。

| 返回与类别的所有产品等于"Toys"。 | `http://localhost/Products?$filter=Category`eq 'Toys |
| --- | --- |
| 返回价格小于 10 的所有产品。 | `http://localhost/Products?$filter=Price`lt 10 |
| 逻辑运算符： 返回的所有产品其中价格 > = 5 和价格 < = 15。 | `http://localhost/Products?$filter=Price`ge 5 和价格 le 15 |
| 字符串函数： 返回名称中使用"zz"的所有产品。 | `http://localhost/Products?$filter=substringof('zz',Name)` |
| 日期函数： 返回 2005年之后的与 ReleaseDate 的所有产品。 | `http://localhost/Products?$filter=year(ReleaseDate)`gt 2005 |

**排序**

若要对结果进行排序，请使用 $orderby 筛选器。

| 价格按排序。 | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| 按降序排序 （最高到低） 的价格排序。 | `http://localhost/Products?$orderby=Price desc` |
| 按类别，进行排序，然后按降序排序类别中的价格进行排序。 | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>服务器驱动的分页

如果你的数据库包含数百万条记录，你不想将其发送全部位于一个负载。 为了防止此情况，服务器可能会限制单个响应中发送的条目数。 若要启用服务器分页，设置**PageSize**中的属性**Queryable**属性。 值为要返回的最大项数。

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

如果你的控制器返回 OData 格式，则响应正文将包含指向下一页数据的链接：

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

客户端可以使用此链接以提取下一步的页。 若要了解在结果集中的总项数，客户端可以设置 $inlinecount 查询选项的值"allpages"。

`http://localhost/Products?$inlinecount=allpages`

值"allpages"指示服务器在响应中包含的总计数：

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> 下一步页链接和内联计数都需要 OData 格式。 原因是 OData 响应正文以保留的链接和计数中定义特殊的字段。


对于非 OData 格式，它是支持通过包装中的查询结果的下一页链接和内联计数，仍可能**PageResult&lt;T&gt;** 对象。 但是，它需要更多代码。 下面是一个示例：

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

此处是一个示例 JSON 响应：

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>限制查询选项

查询选项为客户提供了大量的控制在服务器运行的查询。 在某些情况下，你可能想要限制可用于安全或性能原因的选项。 **[Queryable]**属性具有一些属性中为此生成的。 以下是一些示例。

允许使用仅 $skip 和 $top，以支持分页和其他任何内容：

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

允许排序只能由某些属性，以防止排序不在数据库中编制索引的属性：

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

允许使用"eq"逻辑函数，但没有其他逻辑函数：

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

不允许任何算术运算符：

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

你可以通过构造全局限制选项**QueryableAttribute**实例和将其传递给**EnableQuerySupport**函数：

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>直接调用查询选项

而不是使用**[Queryable]**属性中，你可以直接在你的控制器中调用的查询选项。 为此，请添加**ODataQueryOptions**控制器方法的参数。 在这种情况下，不需要**[Queryable]**属性。

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

Web API 填充**ODataQueryOptions**从 URI 查询字符串。 若要应用该查询，将传递**IQueryable**到**ApplyTo**方法。 该方法返回另一个**IQueryable**。

对于高级方案，如果你没有**IQueryable**查询提供程序，你可以检查**ODataQueryOptions** ，将转换到另一种形式的查询选项。 (有关示例，请参阅 RaghuRam Nadiminti 博客文章[HQL 转换 OData 查询](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx)，其中也包含[示例](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt)。)

<a id="query-validation"></a>
## <a name="query-validation"></a>查询验证

**[Queryable]**属性在执行前验证查询。 在中执行的验证步骤**QueryableAttribute.ValidateQuery**方法。 你还可以自定义验证过程。

另请参阅[OData 安全指南](odata-security-guidance.md)。

首先，在中定义一个验证程序，它是类的替代**Web.Http.OData.Query.Validators**命名空间。 例如，下面的验证程序类禁用 $orderby 选项的 desc 选项。

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

子类**[Queryable]**属性重写**ValidateQuery**方法。

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

然后设置你的自定义特性包括全局或每个控制器：

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

如果你使用**ODataQueryOptions**直接的选项中设置验证程序：

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
