---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: "添加创建方法，并创建视图 |Microsoft 文档"
author: shanselman
description: "这是初学者本教程介绍 ASP.NET MVC 的基础知识。 创建一个简单的 web 应用程序读取和写入数据库中。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 36b3d6ef0432292f21ecd8f29ea2d88ee8867436
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="adding-a-create-method-and-create-view"></a>添加创建方法，并创建视图
====================
通过[Scott Hanselman](https://github.com/shanselman)

> 这是初学者本教程介绍 ASP.NET MVC 的基础知识。 将创建一个简单的 web 应用程序读取和写入数据库中。 请访问[ASP.NET MVC 学习中心](../../../index.md)若要查找其他 ASP.NET MVC 教程和示例。


在本部分中我们将实现使用户能够在我们的数据库中创建新影片所需的支持。 我们将实现电影/创建 URL 操作来执行此操作。

实现电影/创建 URL 是一个两步过程。 当用户第一次访问电影/创建 URL 我们想要显示这些通道他们可以填写输入新的影片的 HTML 窗体。 然后，当用户提交窗体和数据回服务器的文章，我们想要检索的已发布的内容并将其保存到我们的数据库。

我们将在我们 MoviesController 类中实现这两个 create （） 方法内的两个步骤。 一种方法将显示&lt;窗体&gt;用户应填写创建新的影片。 第二种方法将处理处理的已发布的数据，当用户提交&lt;窗体&gt;回服务器，并将保存在我们的数据库中新的影片。

下面是代码我们将添加到我们 MoviesController 类，以实现这一方案：

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

上面的代码包含所有我们需要我们控制器中的代码。

让我们现在实现我们将使用向用户显示的窗体的 Create View 模板。 我们将在第一种创建方法中右键单击，然后选择"添加视图"，以创建我们电影窗体视图模板。

我们将选择我们要将视图模板传递"电影"与它视图的数据类，并指示我们想要"搭建基架的"的"创建"模板。

[![添加视图](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

单击添加按钮后，将为你创建 \Movies\Create.aspx 视图模板。 由于我们从"查看内容"下拉列表中选择"创建"，添加视图对话框中自动"基架"一些默认内容为我们。 基架创建 HTML&lt;窗体&gt;、 验证错误的位置消息若要转，和基架知道电影，因为它创建标签和字段我们的类的每个属性。

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

由于我们的数据库自动使一部电影 ID，请让我们这些字段删除该引用模型。从我们创建视图的 id。 删除后的几行 7&lt;图例&gt;字段&lt;/legend&gt;如它们所示的 ID 字段，我们不希望。

让我们现在创建新的影片，并将其添加到数据库。 我们执行此操作通过运行一次，应用程序，请访问"/ 电影"URL，然后单击"创建"链接以添加新的影片。

[![创建的 Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

当我们单击创建按钮时，我们将为回 （通过 HTTP POST) 的数据发布到我们刚刚创建的 /Movies/Create 方法此窗体上。 一样在系统自动花费了超出 URL 的"numTimes"和"名称"参数，并映射到更早版本的方法上的参数，系统将自动将窗体字段带从文章，并将它们映射到对象。 在这种情况下，从 HTML 如"ReleaseDate"和"标题"中的字段的值将自动放入一部电影的新实例的正确属性中。

让我们看一下第二个的 Create 方法从我们 MoviesController 试。 请注意它采用"电影"对象用作参数的方式：

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

此电影对象然后传递给我们创建操作方法中，[HttpPost] 版本和我们将其保存在数据库中，然后被用户重定向回影片列表中将显示已保存的结果 index （） 的操作方法：

[![影片列表-Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

我们不检查是否我们电影是正确的不过，并且数据库不会使我们能够与无标题保存一部电影。 如果我们无法告知用户的数据库之前引发了错误，则可以很好。 我们将通过将验证支持添加到我们的应用程序来执行此下一步。

>[!div class="step-by-step"]
[上一页](getting-started-with-mvc-part5.md)
[下一页](getting-started-with-mvc-part7.md)
