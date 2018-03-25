---
uid: web-api/overview/advanced/httpclient-message-handlers
title: "在 ASP.NET Web API 中的 HttpClient 消息处理程序 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>在 ASP.NET Web API 中的 HttpClient 消息处理程序
====================
通过[Mike Wasson](https://github.com/MikeWasson)

A*消息处理程序*是接收 HTTP 请求，并返回 HTTP 响应的类。

通常情况下，消息处理程序的一系列链接在一起。 第一个处理程序接收的 HTTP 请求，会处理，并提供请求下一个处理程序。 在某些时候，响应将创建并将会再次回升链。 此模式称为*委派*处理程序。

![](httpclient-message-handlers/_static/image1.png)

在客户端， **HttpClient**类使用消息处理程序来处理请求。 默认处理程序是**HttpClientHandler**，其中通过网络发送请求，并从服务器获取的响应。 你可以将自定义消息处理程序插入客户端管道：

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> ASP.NET Web API 还使用在服务器端的消息处理程序。 有关详细信息，请参阅[HTTP 消息处理程序](http-message-handlers.md)。


## <a name="custom-message-handlers"></a>自定义消息处理程序

若要编写自定义消息处理程序，派生自**System.Net.Http.DelegatingHandler** ，并重写**SendAsync**方法。 下面是方法签名：

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

该方法采用**HttpRequestMessage**作为输入并以异步方式返回**HttpResponseMessage**。 一个典型的实现将执行以下操作：

1. 处理请求消息。
2. 调用`base.SendAsync`将请求发送到内部处理程序。
3. 内部处理程序返回的响应消息。 （此步骤是异步的。）
4. 处理响应，并将其返回给调用方。

下面的示例显示的消息处理程序将自定义标头添加到传出的请求：

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

调用`base.SendAsync`是异步的。 如果处理程序此调用后执行任何工作，使用**await**关键字在方法完成之后继续执行。 下面的示例显示的处理程序日志错误代码。 本身的日志记录不是很有趣，但此示例演示如何获取在处理程序内响应。

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>将消息处理程序添加到客户端管道

若要添加到自定义处理程序**HttpClient**，使用**HttpClientFactory.Create**方法：

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

消息处理程序调用的顺序，将它们到传递**创建**方法。 由于嵌套处理程序的响应消息旅行反方向。 即，最后一个处理程序是以获取响应消息的第一个。
