---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: "安全指南用于 ASP.NET Web API 2 OData |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/06/2013
ms.topic: article
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 799e2a0c742b545acf3b5cd27531d734aa7def80
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a>安全指南用于 ASP.NET Web API 2 OData
====================
通过[Mike Wasson](https://github.com/MikeWasson)

本主题介绍一些公开通过 OData 的数据集时应考虑的安全问题。

## <a name="edm-security"></a>EDM 安全

查询语义基于实体数据模型 (EDM) 中，不是基础的模型类型。 您可以从 EDM 排除属性和它将看不到查询。 例如，假设您的模型包含具有薪金属性的员工类型。 你可能想要从 EDM 以隐藏它从客户端排除此属性。

有两种方法对排除来自 EDM 的属性。 你可以设置**[IgnoreDataMember]**模型类中的属性上的属性：

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

您还可以删除该属性从 EDM 以编程方式：

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>查询安全

恶意或 naïve 客户端可以来构造查询中需要很长时间才能执行。 在最坏情况下这可能会中断服务的访问权限。

**[Queryable]**属性是一个操作筛选器，用于分析、 验证，并适用查询。 筛选器将转换为 LINQ 表达式的查询选项。 当 OData 控制器返回**IQueryable**类型， **IQueryable** LINQ 提供程序将查询转换为 LINQ 表达式。 因此，性能取决于使用时，LINQ 提供程序和数据集或数据库架构的特定特征。

有关在 ASP.NET Web API 中使用 OData 查询选项的详细信息，请参阅[支持 OData 查询选项](supporting-odata-query-options.md)。

如果你知道所有客户端信任 （例如，在企业环境中），或如果你的数据集很小，查询性能可能不是问题。 否则，应考虑以下建议。

- 测试你的服务与各种查询和分析数据库。
- 启用服务器驱动的分页，若要避免在一个查询中返回大型数据集。 有关详细信息，请参阅[Server-Driven 分页](supporting-odata-query-options.md#server-paging)。 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- 你需要 $filter 和 $orderby 吗？ 某些应用程序可能允许客户端分页，使用 $top 和 $skip，但禁用其他查询选项。 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- 请考虑将 $orderby 限制为聚集索引中的属性。 对没有聚集索引的大型数据进行排序很慢。 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- 最大节点计数： **MaxNodeCount**属性**[Queryable]**设置 $filter 语法树中允许的最大数字节点。 默认值为 100，但你可能想要设置较低的值，因为大量的节点可能会很慢进行编译。 这是如果你使用 LINQ to Objects （即，在内存中，而不使用中间的 LINQ 提供程序的集合的 LINQ 查询） 尤其如此。 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- 考虑禁用了 any （） 和 all() 函数中，因为这些可能会很慢。 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- 如果任何字符串属性包含大的字符串 （#8212for 示例、 产品说明或博客文章） #8212consider 禁用字符串函数。 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- 请考虑不允许对导航属性筛选。 对导航属性筛选，可能会导致出现联接，它可能会比较慢，具体取决于你的数据库架构。 下面的代码演示可防止对导航属性筛选的查询验证程序。 有关查询验证程序的详细信息，请参阅[查询验证](supporting-odata-query-options.md#query-validation)。 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- 请考虑通过编写自定义你的数据库的一个验证程序限制 $filter 查询。 例如，考虑这两个查询： 

    - 具有 actors 最后一个名称以 A 开头的所有影片。
    - 1994 年发布的所有影片。

    除非电影参与者编制索引，第一个查询可能需要数据库引擎，扫描整个的影片列表。 而第二个查询可能是可接受的那么电影会按发行年份编制索引。

    下面的代码演示的验证程序，允许筛选上的"ReleaseYear"和"Title"属性但没有其他属性。

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- 一般情况下，请考虑你需要哪些 $filter 函数。 如果你的客户端不需要 $filter 完整表现力，你可以限制允许的函数。
