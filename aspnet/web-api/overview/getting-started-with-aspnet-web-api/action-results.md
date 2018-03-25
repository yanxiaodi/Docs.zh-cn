---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: "操作会导致 Web API 2 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: d0db5c6d45020861d7295ab1db989caee525fff9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="action-results-in-web-api-2"></a>Web API 2 中的操作结果
====================
通过[Mike Wasson](https://github.com/MikeWasson)

本主题介绍如何 ASP.NET Web API 将返回的值转换成一条 HTTP 响应消息的控制器操作从。

Web API 控制器操作可以返回以下任一项：

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. 某些其他类型

具体取决于以下哪种返回时，Web API 使用另一种机制来创建 HTTP 响应。

| 返回类型 | Web API 如何创建响应 |
| --- | --- |
| void | 返回空 204 （无内容） |
| **HttpResponseMessage** | 将转换直接为 HTTP 响应消息。 |
| **IHttpActionResult** | 调用**ExecuteAsync**创建**HttpResponseMessage**，然后将转换为 HTTP 响应消息。 |
| 其他类型 | 将序列化的返回值写入到响应正文中;返回 200 （正常）。 |

本主题的其余部分介绍更多详细信息中的每个选项。

## <a name="void"></a>void

如果返回类型为`void`，Web API 只返回状态代码 204 （无内容） 的空 HTTP 响应。

示例控制器：

[!code-csharp[Main](action-results/samples/sample1.cs)]

HTTP 响应：

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>HttpResponseMessage

如果操作返回[HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx)，Web API 将返回的值转换为 HTTP 响应消息中，直接使用的属性**HttpResponseMessage**要填充对象响应。

此选项提供了大量的响应消息上的控件。 例如，以下的控制器操作设置的缓存控制标头。

[!code-csharp[Main](action-results/samples/sample3.cs)]

响应：

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

如果传递到的域模型**CreateResponse**方法时，Web API 使用[媒体格式化程序](../formats-and-model-binding/media-formatters.md)要序列化的模型写入响应正文。

[!code-csharp[Main](action-results/samples/sample5.cs)]

Web API 使用 Accept 标头在请求中选择格式化程序。 有关详细信息，请参阅[内容协商](../formats-and-model-binding/content-negotiation.md)。

## <a name="ihttpactionresult"></a>IHttpActionResult

**IHttpActionResult** Web API 2 中引入了接口。 从根本上来说，它定义**HttpResponseMessage**工厂。 以下是使用的某些优点**IHttpActionResult**接口：

- 简化了[单元测试](../testing-and-debugging/unit-testing-controllers-in-web-api.md)你的控制器。
- 将移动到单独的类创建 HTTP 响应的常见逻辑。
- 通过隐藏构造响应的低级别的详细信息会更清晰、 控制器操作的意图。

**IHttpActionResult**包含单个方法**ExecuteAsync**，这将以异步方式创建**HttpResponseMessage**实例。

[!code-csharp[Main](action-results/samples/sample6.cs)]

如果控制器操作返回**IHttpActionResult**，Web API 调用**ExecuteAsync**方法来创建**HttpResponseMessage**。 然后它将转换**HttpResponseMessage**到 HTTP 响应消息。

下面是的简单实现**IHttpActionResult**创建纯文本响应：

[!code-csharp[Main](action-results/samples/sample7.cs)]

示例控制器操作：

[!code-csharp[Main](action-results/samples/sample8.cs)]

响应：

[!code-console[Main](action-results/samples/sample9.cmd)]

更多时候，你将使用**IHttpActionResult**中定义的实现 **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** 命名空间。 **ApiController**类定义返回这些内置操作结果的帮助器方法。

在下面的示例中，如果请求不匹配现有产品 ID，控制器调用[ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx)创建 404 （未找到） 响应。 否则，控制器会调用[ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx)，这将创建一个 200 (OK) 响应，包含产品。

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>其他返回类型

对于所有其他返回类型，Web API 使用[媒体格式化程序](../formats-and-model-binding/media-formatters.md)序列化的返回值。 Web API 将序列化的值写入到响应正文。 响应状态代码为 200 （正常）。

[!code-csharp[Main](action-results/samples/sample11.cs)]

此方法的缺点是，不能直接返回错误代码，如 404。 但是，你可以引发**HttpResponseException**有关错误代码。 有关详细信息，请参阅[ASP.NET Web API 中的异常处理](../error-handling/exception-handling.md)。

Web API 使用 Accept 标头在请求中选择格式化程序。 有关详细信息，请参阅[内容协商](../formats-and-model-binding/content-negotiation.md)。

示例请求

[!code-console[Main](action-results/samples/sample12.cmd)]

示例响应：

[!code-console[Main](action-results/samples/sample13.cmd)]
