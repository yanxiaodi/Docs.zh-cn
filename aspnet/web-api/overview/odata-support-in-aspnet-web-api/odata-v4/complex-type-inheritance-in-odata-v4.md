---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: "与 ASP.NET Web API OData v4 中的复杂类型继承 |Microsoft 文档"
author: microsoft
description: "OData v4 规范中，根据复杂类型可以继承另一种复杂类型。 （复杂类型是结构化的类型没有键。）Web API..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>与 ASP.NET Web API OData v4 中的复杂类型继承
====================
通过[Microsoft](https://github.com/microsoft)

> 根据 OData v4[规范](http://www.odata.org/documentation/odata-version-4-0/)，复杂类型可以继承另一种复杂类型。 (A*复杂*类型是结构化的类型没有键。)Web API OData 5.3 支持复杂类型继承。
> 
> 本主题演示如何生成具有复杂的继承类型的实体数据模型 (EDM)。 有关完整的源代码，请参阅[OData 复杂类型继承示例](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt)。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - Web API OData 5.3
> - OData v4


## <a name="model-hierarchy"></a>模型层次结构

为了说明复杂类型继承，我们将使用以下类层次结构。

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape`是抽象的复杂类型。 `Rectangle``Triangle`，和`Circle`复杂类型派生自`Shape`，和`RoundRectangle`派生自`Rectangle`。 `Window`是一个实体类型，包含`Shape`实例。

以下是定义这些类型的 CLR 类。

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>生成 EDM 模型

若要创建 EDM，可以使用**ODataConventionModelBuilder**，，推断从 CLR 类型的继承关系。

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

你也可以生成 EDM 显式使用**ODataModelBuilder**。 这将更多代码，但使你更好地控制 EDM。

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

这两个示例创建相同的 EDM 架构。

## <a name="metadata-document"></a>元数据文档

此处是 OData 元数据文档时，显示复杂类型继承。

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

从元数据文档中，你可以看到:

- `Shape`复杂类型为抽象。
- `Rectangle`， `Triangle`，和`Circle`复杂类型具有的基类型`Shape`。
- `RoundRectangle`类型具有基类型`Rectangle`。

## <a name="casting-complex-types"></a>强制转换复杂类型

现在支持复杂类型上进行转换。 例如，下面的查询强制转换`Shape`到`Rectangle`。

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

下面是响应负载：

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
