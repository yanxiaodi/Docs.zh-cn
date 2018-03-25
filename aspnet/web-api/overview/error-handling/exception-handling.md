---
uid: web-api/overview/error-handling/exception-handling
title: "ASP.NET Web API 中的异常处理 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="exception-handling-in-aspnet-web-api"></a>ASP.NET Web API 中的异常处理
====================
通过[Mike Wasson](https://github.com/MikeWasson)

本指南介绍了错误和异常处理在 ASP.NET Web API 中。

- [HttpResponseException](#httpresponserexception)
- [异常筛选器](#exception_filters)
- [注册异常筛选器](#registering_exception_filters)
- [HttpError](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

如果 Web API 控制器引发未捕获的异常，则会发生什么情况？ 默认情况下，大多数异常被转换为状态代码 500 内部服务器错误的 HTTP 响应。

**HttpResponseException**类型是一种特殊情况。 此异常返回异常构造函数中指定任何 HTTP 状态代码。 例如，以下方法将返回 404，找不到，如果*id*参数无效。

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

更灵活地控制响应，还可以构造的整个响应消息，并将其与包含**HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>异常筛选器

你可以自定义 Web API 通过编写处理异常的方式*异常筛选器*。 异常筛选器时的控制器方法引发任何未处理的异常时执行*不* **HttpResponseException**异常。 **HttpResponseException**类型是一种特殊情况，因为它专为返回 HTTP 响应。

异常筛选器实现**System.Web.Http.Filters.IExceptionFilter**接口。 编写异常筛选器的最简单方法是为派生自**System.Web.Http.Filters.ExceptionFilterAttribute**类并重写**OnException**方法。

> [!NOTE]
> ASP.NET Web API 中的异常筛选器是 ASP.NET MVC 相似。 但是，声明它们在一个单独的命名空间和函数分开。 具体而言， **HandleErrorAttribute**在 MVC 中使用的类并不处理由 Web API 控制器引发的异常。


下面是将转换的筛选器**NotImplementedException**异常 HTTP 状态代码 501，未实现：

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

**响应**属性**HttpActionExecutedContext**对象包含将发送到客户端的 HTTP 响应消息。

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>注册异常筛选器

有多种，注册 Web API 异常筛选器：

- 由操作
- 由控制器
- 全局

若要将筛选器应用到特定的操作，请将作为属性的筛选器添加到操作：

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

将筛选器应用于所有在控制器上的操作，请将作为属性的筛选器添加到控制器类：

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

若要将筛选器全局应用于所有的 Web API 控制器，将添加到筛选器的实例**GlobalConfiguration.Configuration.Filters**集合。 在此集合中的异常筛选器适用于任何 Web API 控制器操作。

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

如果你使用"ASP.NET MVC 4 Web 应用程序"项目模板创建你的项目，将 Web API 配置代码内的放置`WebApiConfig`类，该类在应用程序位于\_开始文件夹：

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HttpError

**HttpError**对象提供一致的方法来响应正文中返回错误信息。 下面的示例演示如何返回 HTTP 状态代码 404 （未找到） 与**HttpError**响应正文中。

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse**中定义的扩展方法**System.Net.Http.HttpRequestMessageExtensions**类。 在内部， **CreateErrorResponse**创建**HttpError**实例，然后创建**HttpResponseMessage**包含**HttpError**.

在此示例中，如果此方法成功，它将返回 HTTP 响应中的产品。 但是，如果未找到请求的产品，包含 HTTP 响应**HttpError**请求正文中。 响应可能如下所示：

[!code-console[Main](exception-handling/samples/sample9.cmd)]

请注意， **HttpError**在此示例中，已将它们序列化为 JSON。 使用的一个优点**HttpError**是它将经历相同[内容协商](../formats-and-model-binding/content-negotiation.md)和序列化处理任何其他强类型的模型。

### <a name="httperror-and-model-validation"></a>HttpError 和模型验证

对于模型的验证，你可以将传递到在模型状态**CreateErrorResponse**，若要在响应中包含的验证错误：

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

此示例可能会返回以下响应：

[!code-console[Main](exception-handling/samples/sample11.cmd)]

有关模型验证的详细信息，请参阅[ASP.NET Web API 中的模型验证](../formats-and-model-binding/model-validation-in-aspnet-web-api.md)。

### <a name="using-httperror-with-httpresponseexception"></a>使用 HttpResponseException HttpError

前面的示例返回**HttpResponseMessage**消息从控制器操作，但你还可以使用**HttpResponseException**返回**HttpError**。 这样就可以时仍返回在正常成功情况下，返回强类型模型**HttpError**如果错误：

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
