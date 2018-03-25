---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: "创建单一实例 OData v4 中的使用 Web API 2.2 |Microsoft 文档"
author: rick-anderson
description: "本主题演示如何定义中的 Web API 2.2 OData 终结点中的单一实例。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 92c5056548b1e39defb28ac36f83b001dd108f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>创建单一实例 OData v4 中的使用 Web API 2.2
====================
通过 Zoe Luo

> 传统上，如果它已封装在实体集可以仅访问实体。 但在 OData v4 提供单一实例和包含，这两种 WebAPI 2.2 支持的两个附加选项。


这篇文章演示如何在中的 Web API 2.2 OData 终结点中定义单一实例。 有关哪些单一实例，你将可以从使用它中受益的信息，请参阅[使用单一实例定义特殊实体](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx)。 若要在 Web API 中创建 OData V4 终结点，请参阅[创建 OData v4 终结点使用 ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md)。 

Web API 项目使用以下数据模型中，我们将创建单一实例：

![数据模型](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

名为的单独`Umbrella`将基于类型定义`Company`，和的实体集命名`Employees`将基于类型定义`Employee`。

本教程中使用该解决方案可以从下载[CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)。

## <a name="define-the-data-model"></a>定义数据模型

1. 定义的 CLR 类型。

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. 生成基于 CLR 类型的 EDM 模型。

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    在这里，`builder.Singleton<Company>("Umbrella")`告知模型生成器来创建名为的单独`Umbrella`EDM 模型中。

    生成的元数据将如下所示：

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    从元数据中我们可以看到，导航属性`Company`中`Employees`实体集绑定到单一实例`Umbrella`。 会自动完成绑定`ODataConventionModelBuilder`，因为仅`Umbrella`具有`Company`类型。 如果模型中没有任何多义性，则可以使用`HasSingletonBinding`显式将导航属性绑定到单一实例;`HasSingletonBinding`具有相同的效果与使用`Singleton`CLR 类型定义中的属性：

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>定义的 singleton 控制器

EntitySet 控制器中，如继承自 singleton 控制器`ODataController`，和 singleton 控制器名称应为`[singletonName]Controller`。

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

若要处理不同类型的请求，操作都需要在控制器中预定义。 **属性路由**WebApi 2.2 中默认启用。 例如，若要定义要处理查询的操作`Revenue`从`Company`使用的属性路由，使用以下：

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

如果你不愿意来定义每个操作的属性，只需定义按照你操作[OData 路由约定](../odata-routing-conventions.md)。 由于不需要查询单一实例的密钥，则单独控制器中定义的操作中略有不同从 entityset 控制器中定义的操作。

作为参考，下面列出了在单独控制器中的每个操作定义的方法签名。

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

基本上，这是你需要在服务端上执行操作。 [示例项目](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)包含所有为解决方案和演示如何使用单独的 OData 客户端代码。 客户端生成的中的步骤[创建 OData v4 客户端应用](create-an-odata-v4-client-app.md)。

. 

*到 Leo Hu 感谢这篇文章的原始内容。*
