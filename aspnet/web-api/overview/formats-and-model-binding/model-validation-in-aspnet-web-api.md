---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: "模型 ASP.NET Web API 中的验证 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 45b519af4073b62c8be1ca8951e44d6cf3cbe075
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="model-validation-in-aspnet-web-api"></a>ASP.NET Web API 中的模型验证
====================
通过[Mike Wasson](https://github.com/MikeWasson)

当客户端将数据发送到你的 web API 时，通常你想要在进行任何处理之前验证数据。 这篇文章演示如何添加批注您的模型、 批注用于数据验证和处理你的 web API 中的验证错误。

## <a name="data-annotations"></a>数据注释

在 ASP.NET Web API，你可以使用中的属性[System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空间在您的模型上设置属性的验证规则。 请考虑以下模型：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

如果在 ASP.NET MVC 中使用了模型验证，这应该熟悉。 **所需**属性告知`Name`属性不得为空。 **范围**属性告知`Weight`必须是介于 0 和 999 之间。

假设客户端发送 POST 请求的以下 JSON 表示形式：

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

你可以看到客户端未包括`Name`属性，它被标记为必需。 当 Web API 将转换为 JSON`Product`实例，它会验证`Product`针对验证属性。 在你的控制器操作，你可以检查模型是否有效：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

模型验证不保证客户端数据的安全。 应用程序的其他层中，可能需要其他验证。 （例如，数据层可能会强制执行外键约束。）本教程[使用实体框架使用 Web API](../data/using-web-api-with-entity-framework/part-1.md)探讨一些这些问题。

**"未得到充分发布"**： 客户端省略了某些属性时发生未得到充分发布。 例如，假设客户端发送以下：

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

在这里，客户端未指定值`Price`或`Weight`。 JSON 格式化程序将分配默认值为 0 到缺少的属性。

![](model-validation-in-aspnet-web-api/_static/image1.png)

模型状态无效，因为这些属性的有效值是零。 这是否是一个问题取决于你的方案。 例如，在更新操作中，你可能想要区分"零"和"未设置。" 若要强制客户端可以设置一个值，将该属性可以为 null，并且已设置**所需**属性：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"过多发布"**： 客户端还可以发送*详细*比你预期的数据。 例如:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

在这里，JSON 包括中不存在的属性 ("Color")`Product`模型。 在这种情况下，JSON 格式化程序只需将忽略此值。 （XML 格式化程序执行相同的操作。）如果你的模型具有你想要为只读的属性，过度发布会导致问题。 例如:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

你不希望用户更新`IsAdmin`属性并将自身提升至管理员 ！ 安全策略是使用完全匹配什么则允许客户端发送一个模型类：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Brad wilson 制作的博客文章"[输入验证 vs。模型中的 ASP.NET MVC 的验证](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"具有未得到充分发布和过度发布的良好讨论。 尽管有关 ASP.NET MVC 2 是 post，但问题仍然是相关到 Web API。


## <a name="handling-validation-errors"></a>处理验证错误

Web API 不会自动返回错误客户端验证失败时。 负责检查模型状态和做出相应响应的控制器操作。

你还可以创建一个可调用的控制器操作之前检查模型状态的操作筛选器。 下面的代码演示一个示例：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

如果模型验证失败，则此筛选器将返回包含验证错误的 HTTP 响应。 在这种情况下，不调用控制器操作。

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

若要将此筛选器应用于所有的 Web API 控制器，将添加到筛选器的实例**HttpConfiguration.Filters**在配置期间的集合：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

另一个选项是在各个控制器或控制器操作上设置作为特性的筛选器：

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
