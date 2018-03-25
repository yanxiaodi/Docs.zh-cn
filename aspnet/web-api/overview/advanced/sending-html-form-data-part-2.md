---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: "在 ASP.NET Web API 发送 HTML 窗体数据： 文件上载和多部分 MIME |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 331d0e520a1fd8ec84aecd09a9c9e6d286c5893b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>在 ASP.NET Web API 发送 HTML 窗体数据： 文件上载和多部分 MIME
====================
通过[Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>第 2 部分： 文件上载和多部分 MIME

本教程演示如何将文件上载到 web API。 它还描述如何处理多部分 MIME 数据。

> [!NOTE]
> [下载完成的项目](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d)。


下面是用于上载文件以 HTML 形式的示例：

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

此表单包含文本输入的控件和文件输入的控件。 当窗体包含文件的输入的控件， **enctype**属性应始终为&quot;multipart/窗体数据&quot;，它指定将作为多部分 MIME 消息发送的窗体。

多部分 MIME 消息的格式是最简单的方法了解通过查看请求的示例：

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

此消息划分为两个*部件*，一个用于每个窗体控件。 通过以短划线开头的行来指示零件边界。

> [!NOTE]
> 一部分边界包括随机组件 (&quot;41184676334&quot;) 以确保的边界字符串是否意外没有内的消息部分。


每个消息部分包含一个或多个标头，跟部件内容。

- Content-disposition 标头包括控件的名称。 对于文件，它还包含文件名称。
- 内容类型标头描述的部分中的数据。 如果省略此标头，则默认值为文本/无格式。

在前面的示例中，用户上载名 GrandCanyon.jpg，为具有内容类型映像/jpeg; 的文件输入文本的值，&quot;夏天休假&quot;。

## <a name="file-upload"></a>文件上载

现在让我们看一下从多部分 MIME 消息中读取文件的 Web API 控制器。 控制器将以异步方式读取文件。 Web API 支持异步操作，使用[基于任务的编程模型](https://msdn.microsoft.com/library/dd460693.aspx)。 如果你正面向.NET Framework 4.5，支持首先，以下是代码**异步**和**await**关键字。

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

请注意控制器操作不采用任何参数。 这是因为我们处理请求正文内操作，而无需调用媒体类型格式化程序。

**IsMultipartContent**方法检查请求是否包含多部分 MIME 消息。 如果没有，则控制器将返回 HTTP 状态代码 415 （不支持的媒体类型）。

**MultipartFormDataStreamProvider**类是一个分配为上载的文件的文件流的帮助器对象。 若要读取多部分 MIME 消息，请调用**ReadAsMultipartAsync**方法。 此方法提取所有的消息部分，并将其写入到流提供的**MultipartFormDataStreamProvider**。

方法完成时，你可以获取有关中的文件信息**数据**属性，这是集合的**MultipartFileData**对象。

- **MultipartFileData.FileName**是在服务器上，保存文件的本地文件名称。
- **MultipartFileData.Headers**包含部分标头 (*不*请求标头)。 可以使用此访问内容\_处置和内容类型标头。

顾名思义， **ReadAsMultipartAsync**是异步方法。 若要执行工作，该方法完成后，使用[延续任务](https://msdn.microsoft.com/library/ee372288.aspx)(.NET 4.0) 或**await**关键字 (.NET 4.5)。

下面是代码的前面的.NET Framework 4.0 版本：

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>读取窗体控件数据

我在前面显示的 HTML 窗体有一个文本输入的控件。

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

你可以从控件的值**FormData**属性**MultipartFormDataStreamProvider**。

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData**是**NameValueCollection**包含窗体控件的名称/值对。 集合可以包含重复键。 请考虑此窗体：

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

请求正文可能如下所示：

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

在这种情况下， **FormData**集合将包含以下键/值对：

- 行程： 往返
- 选项： nonstop
- 选项： 日期
- 座位： 窗口
