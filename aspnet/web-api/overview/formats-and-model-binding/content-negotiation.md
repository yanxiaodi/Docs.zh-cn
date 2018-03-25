---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: "内容在 ASP.NET Web API 的协商 |Microsoft 文档"
author: MikeWasson
description: "描述如何 ASP.NET Web API 实现 HTTP 内容协商。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/20/2012
ms.topic: article
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: ca373af6754e82889dc100b63f73b76aaa4e4f27
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="content-negotiation-in-aspnet-web-api"></a>ASP.NET Web API 中的内容协商
====================
通过[Mike Wasson](https://github.com/MikeWasson)

本指南介绍了如何 ASP.NET Web API 实现内容协商。

HTTP 规范 (RFC 2616) 定义为"有多个表示形式之间实现可用时选择的最佳表示对于给定的响应的处理。"的内容协商 用于在 HTTP 中的内容协商的主要机制是这些请求标头：

- **接受：**哪些媒体类型都可接受的响应，如"application/json;"的"application/xml"或自定义媒体类型，如&quot;application/vnd.example+xml&quot;
- **Accept-charset:**哪些字符集是可以接受的例如 utf-8 或 ISO 8859-1。
- **Accept-encoding:**是可接受的例如 gzip 的哪种内容编码。
- **接受语言：**首选的自然语言，例如"en-我们"。

服务器还可以查看在 HTTP 请求的其他部分。 例如，如果请求包含 X-请求的带有标头，指示 AJAX 请求，服务器可能默认为 JSON 如果没有任何 Accept 标头。

在本文中，我们将了解如何 Web API 使用接受和 Accept-charset 标头。 （在此时，没有 Accept-encoding 或接受语言内置支持。）

## <a name="serialization"></a>序列化

如果 Web API 控制器返回作为 CLR 类型的资源，管道将序列化的返回值，并将其写入 HTTP 响应正文。

例如，考虑以下的控制器操作：

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

客户端可能会发送此 HTTP 请求：

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

在响应中，服务器可能会发送：

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

在此示例中，客户端请求 JSON、 Javascript，或"任何"(\*/\*)。 服务器响应的 JSON 表示形式的`Product`对象。 请注意，在响应中的内容类型标头设置为&quot;应用程序/json&quot;。

控制器还可以返回**HttpResponseMessage**对象。 若要指定响应正文的 CLR 对象，调用**CreateResponse**扩展方法：

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

此选项可更好地控制响应的详细信息。 你可以设置的状态代码、 添加 HTTP 标头和等等。

序列化资源的对象称为*媒体格式化程序*。 媒体格式化程序派生自**MediaTypeFormatter**类。 Web API 提供媒体格式化程序的 XML 和 JSON，并且可以创建自定义格式化程序来支持其他媒体类型。 有关编写自定义格式化程序的信息，请参阅[媒体格式化程序](media-formatters.md)。

## <a name="how-content-negotiation-works"></a>如何在内容协商工作原理

首先，管道获取**IContentNegotiator**服务中，从**HttpConfiguration**对象。 它还可以获取的从媒体格式化程序列表**HttpConfiguration.Formatters**集合。

接下来，调用管道**IContentNegotiatior.Negotiate**，并传入：

- 要序列化的对象类型
- 媒体格式化程序的集合
- HTTP 请求

**Negotiate**方法返回两条信息：

- 若要使用的格式化程序
- 响应的媒体类型

如果不找到任何格式化程序，则**Negotiate**方法返回**null**，和客户端收到 HTTP 错误 406 （不可接受）。

下面的代码演示如何控制器可以直接调用内容协商：

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

此代码是等效的针对管道自动执行。

## <a name="default-content-negotiator"></a>默认内容 Negotiator

**DefaultContentNegotiator**类提供的默认实现**IContentNegotiator**。 它使用多个条件来选择格式化程序。

首先，格式化程序必须能序列化的类型。 这可以通过调用验证**MediaTypeFormatter.CanWriteType**。

接下来，内容 negotiator 查找每个格式化程序，并计算结果 HTTP 请求与匹配的程度。 若要评估匹配，内容 negotiator 会在以下两项操作在格式化程序：

- **SupportedMediaTypes**集合，其中包含的受支持的媒体类型的列表。 内容 negotiator 尝试匹配此列表针对请求的 Accept 标头。 请注意，Accept 标头可以包含范围。 例如，"文本/plain"是文本的匹配项 /\*或\* / \*。
- **MediaTypeMappings**集合，其中包含一份**MediaTypeMapping**对象。 **MediaTypeMapping**类提供一种来匹配 HTTP 请求和媒体类型的泛型方法。 例如，它无法映射到某个特定媒体类型的自定义的 HTTP 标头。

如果有多个匹配，在最高的质量因素 wins 的匹配项。 例如: 

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

在此示例中，应用程序/json 具有的隐含的质量因子为 1.0，因此它是首选对应用程序中的 xml。

如果不找到任何匹配项，内容 negotiator 尝试匹配媒体类型的请求正文中，如果有的话。 例如，如果该请求包含 JSON 数据，内容 negotiator 查找 JSON 格式化程序。

如果仍没有匹配项，则内容 negotiator 只会选取的第一个格式化程序可以序列化的类型。

## <a name="selecting-a-character-encoding"></a>选择一种字符编码

选择一个格式化程序后，内容 negotiator 选择最佳字符编码通过查看**SupportedEncodings**上格式化程序，并将它与请求中的 Accept-charset 标头匹配，（如果有） 的属性。
