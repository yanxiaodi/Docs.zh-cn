---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: "在 ASP.NET Web API 发送 HTML 窗体数据： 窗体编码形式数据 |Microsoft 文档"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0ed339c4f9d5854ab5a21cdd077a4d494987101f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>在 ASP.NET Web API 发送 HTML 窗体数据： 窗体编码形式的数据
====================
通过[Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>第 1 部分： 窗体编码形式数据

这篇文章演示如何窗体编码形式将数据发布到 Web API 控制器。

- [HTML 窗体概述](#overview_of_html_forms)
- [发送的复杂类型](#sending_complex_types)
- [通过 AJAX 发送的窗体数据](#sending_form_data_via_ajax)
- [发送的简单类型](#sending_simple_types)

> [!NOTE]
> [下载完成的项目](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007)。


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>HTML 窗体概述

HTML 窗体使用获取，或者发布将数据发送到服务器。 **方法**属性**窗体**元素提供的 HTTP 方法：

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

默认方法为 GET。 如果窗体使用 GET，窗体的数据被编码为查询字符串的 URI 中。 如果窗体使用 POST，表单数据都放在请求正文中。 已过帐数据**enctype**属性指定请求正文的格式：

| enctype | 描述 |
| --- | --- |
| application/x-www-form-urlencoded | 窗体数据被编码为名称/值对，类似于 URI 查询字符串。 这是默认格式为 POST。 |
| multipart/窗体的数据 | 窗体数据被编码为多部分 MIME 消息。 如果将文件上载到服务器，请使用此格式。 |

本文的第 1 部分考察 x-响应客户-窗体-urlencoded 格式。 [第 2 部分](sending-html-form-data-part-2.md)介绍多部分 MIME。

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>发送的复杂类型

通常情况下，你将发送一个复杂类型，由多个窗体控件中获取的值构成。 请考虑以下模型表示的状态更新：

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

下面是接受一个 Web API 控制器`Update`通过 POST 的对象。

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> 此控制器使用[基于操作的路由](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name)，因此路由模板是&quot;api / {controller} / {action} / {id}&quot;。 客户端会将发布到数据&quot;/api/updates/complex&quot;。


现在让我们来编写用户提交状态更新以 HTML 形式。

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

请注意，**操作**窗体上的属性是我们控制器操作的 URI。 下面是具有某些值中输入的窗体：

![](sending-html-form-data-part-1/_static/image1.png)

当用户单击提交时，则浏览器将发送 HTTP 请求与以下类似：

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

请注意，请求正文包含格式为名称/值对的窗体数据。 Web API 会自动将名称/值对转换到的实例`Update`类。

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>通过 AJAX 发送的窗体数据

当用户提交表单时，浏览器导航离开当前页，并呈现响应消息的正文。 这是确定时响应是 HTML 页。 与 web API，但是，响应正文是通常为空或包含结构化的数据，例如 JSON。 在这种情况下，它更有意义发送的窗体数据使用 AJAX 请求，以便该页可以处理响应。

下面的代码演示如何发布使用 jQuery 的窗体数据。

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

JQuery**提交**函数窗体操作替换为新的函数。 此设置将替代提交按钮的默认行为。 **序列化**函数名称/值对序列化为窗体数据。 若要向服务器发送的窗体数据，调用`$.post()`。

完成请求后，`.success()`或`.error()`处理程序为用户显示相应的消息。

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>发送的简单类型

在前面的部分中，我们将发送一个复杂类型，Web API 反序列化到模型类的实例。 你还可以发送简单类型，例如字符串。

> [!NOTE]
> 在发送之前简单类型，请考虑改为在复杂类型中包装值。 这使您在服务器端的模型验证的优点，并轻松地根据需要扩展您的模型。


要发送的简单类型的基本步骤都相同，但有两个细微的差异。 首先，在控制器中，必须修饰参数名称后的加**FromBody**属性。

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

默认情况下，Web API 尝试于请求 URI 中获取简单类型。 **FromBody**属性告知 Web API 以从请求正文中读取值。

> [!NOTE]
> Web API 读取响应正文最多一次，以便只有一个操作的参数可来自于请求正文。 如果你需要从请求正文中获取多个值，定义复杂类型。


其次，客户端需要发送具有以下格式的值：

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

具体而言，名称/值对的名称部分必须是简单类型为空。 并非所有浏览器都支持这对于 HTML 窗体，但你创建此格式，在脚本中，如下所示：

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

下面是一个示例表单：

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

和下面是用于将此窗体值提交脚本。 与前一个脚本中的唯一差异是自变量传递给**文章**函数。

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

可以使用相同的方法将发送一个简单的类型数组：

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>其他资源

[第 2 部分： 文件上载和多部分 MIME](sending-html-form-data-part-2.md)
