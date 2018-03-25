---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: "使用 Web API 2.2 中 OData v4 的包含关系 |Microsoft 文档"
author: rick-anderson
description: "传统上，如果它已封装在实体集可以仅访问实体。 但 OData v4 提供两个其他选项，单一实例和 Con..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7d3c81bf3d2a43faa3e71155637e031f81143782
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="containment-in-odata-v4-using-web-api-22"></a>使用 Web API 2.2 中 OData v4 的包含关系
====================
通过 Jinfu Tan

> 传统上，如果它已封装在实体集可以仅访问实体。 但在 OData v4 提供单一实例和包含，这两种 WebAPI 2.2 支持的两个附加选项。


本主题演示如何在 WebApi 2.2 中的 OData 终结点中定义包含。 包含有关的详细信息，请参阅[包含即将与 OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx)。 若要在 Web API 中创建 OData V4 终结点，请参阅[创建 OData v4 终结点使用 ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md)。

首先，我们将在 OData 服务中，使用此数据模型创建的包含域模型：

![数据模型](odata-containment-in-web-api-22/_static/image1.png)

帐户包含许多 PaymentInstruments (PI)，但我们未定义实体集 PI。 相反，pi 均仅可以通过帐户访问。

你可以下载从本主题中使用的解决方案[CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/)。

## <a name="defining-the-data-model"></a>定义数据模型

1. 定义的 CLR 类型。

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    `Contained`属性用于包含导航属性。
2. 生成基于 CLR 类型的 EDM 模型。

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    `ODataConventionModelBuilder`将处理生成 EDM 模型，如果`Contained`属性会被添加到对应的导航属性。 如果属性是集合类型，`GetCount(string NameContains)`还将创建函数。

    生成的元数据将如下所示：

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    `ContainsTarget`属性指示的导航属性为包含。

## <a name="define-the-containing-entity-set-controller"></a>定义包含的实体集控制器

包含的实体没有其自己的控制器;包含的实体集控制器中定义该操作。 在此示例中，没有 AccountsController，但没有 PaymentInstrumentsController。

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

如果该 OData 路径是 4 个或多个段，仅属性路由工作原理，如`[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]`上述控制器中。 否则，属性和传统路由工作原理： 例如，`GetPayInPIs(int key)`匹配`GET ~/Accounts(1)/PayinPIs`。

*到 Leo Hu 感谢这篇文章的原始内容。*
