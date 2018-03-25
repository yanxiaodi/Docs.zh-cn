---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: "ASP.NET Web API 2 中的媒体格式化程序 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 9103574597df126a22e21a2f51815f608e46f47f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="media-formatters-in-aspnet-web-api-2"></a>ASP.NET Web API 2 中的媒体格式化程序
====================
通过[Mike Wasson](https://github.com/MikeWasson)

本教程将说明如何在 ASP.NET Web API 中支持的其他媒体格式。

## <a name="internet-media-types"></a>Internet 媒体类型

媒体类型，也称为 MIME 类型，标识一段数据的格式。 在 HTTP，媒体类型描述消息正文的格式。 媒体类型由两个字符串、 类型和子类型组成。 例如:

- text/html
- 图像/png
- application/json

当 HTTP 消息包含一个实体正文时，内容类型标头指定消息正文的格式。 这指示接收方如何分析消息正文的内容。

例如，如果 HTTP 响应包含 PNG 图像，响应可能包含以下标头。

[!code-console[Main](media-formatters/samples/sample1.cmd)]

当客户端发送请求消息时，它可以包括 Accept 标头。 Accept 标头指示的服务器的媒体类型的客户端想要从服务器。 例如:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

此标头指示服务器在客户端希望 HTML、 XHTML 或 XML。

媒体类型确定 Web API 如何序列化和反序列化 HTTP 消息正文。 Web API 有对 XML、 JSON、 BSON 和窗体编码形式数据的内置支持，并且你可以通过编写支持其他媒体类型*媒体格式化程序*。

若要创建媒体格式化程序，派生自这些类之一：

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). 此类使用的异步读取和写入方法。
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). 此类派生自**MediaTypeFormatter**但使用 sychronous 读/写方法。

派生自**BufferedMediaTypeFormatter**是更简单，因为没有异步代码，但它还意味着在 I/O 期间可以阻止调用线程。

## <a name="example-creating-a-csv-media-formatter"></a>示例： 创建 CSV 媒体格式化程序

下面的示例演示可以序列化到逗号分隔值 (CSV) 格式的产品对象的媒体类型格式化程序。 此示例使用在本教程中定义的产品类型[创建该支持 CRUD 操作的 Web API](../older-versions/creating-a-web-api-that-supports-crud-operations.md)。 下面是该对象定义的产品：

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

若要实现一个 CSV 格式化程序，定义派生自类**BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

在构造函数中，添加格式化程序支持的媒体类型。 在此示例中，格式化程序支持单个媒体类型，&quot;文本/csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

重写**CanWriteType**方法，以指示该类型的格式化程序可以序列化：

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

在此示例中，格式化程序可以序列化为单个`Product`对象的集合以及`Product`对象。

同样，重写**CanReadType**方法，以指示该类型的格式化程序可以反序列化。 在此示例中，格式化程序不支持反序列化，因此此方法只返回**false**。

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

最后，重写**WriteToStream**方法。 此方法序列化类型写入流。 如果你的格式化程序支持反序列化，还重写**ReadFromStream**方法。

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>将媒体格式化程序添加到 Web API 管道

若要添加的媒体类型到 Web API 管道的格式化程序，使用**格式化程序**属性**HttpConfiguration**对象。

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>字符编码

（可选） 媒体格式化程序可以支持多个的字符编码，例如 utf-8 或 ISO 8859-1。

在构造函数中，添加一个或多个[System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx)类型**SupportedEncodings**集合。 将编码第一个默认值。

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

在**WriteToStream**和**ReadFromStream**方法，调用[MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx)选择首选的字符编码。 此方法与匹配的请求标头针对支持编码的列表。 使用返回**编码**时，你可以读取或写入从流：

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
