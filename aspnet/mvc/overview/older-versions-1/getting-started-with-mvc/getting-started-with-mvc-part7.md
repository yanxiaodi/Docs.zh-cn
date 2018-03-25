---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: "将验证添加到模型 |Microsoft 文档"
author: shanselman
description: "这是初学者本教程介绍 ASP.NET MVC 的基础知识。 创建一个简单的 web 应用程序读取和写入数据库中。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 5616c3c3bc77be0a770540d04cc2ae48ba9eedff
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="adding-validation-to-the-model"></a>添加到模型的验证
====================
通过[Scott Hanselman](https://github.com/shanselman)

> 这是初学者本教程介绍 ASP.NET MVC 的基础知识。 将创建一个简单的 web 应用程序读取和写入数据库中。 请访问[ASP.NET MVC 学习中心](../../../index.md)若要查找其他 ASP.NET MVC 教程和示例。


在本部分中我们将实现启用我们的应用程序中的输入的验证所需的支持。 我们将确保我们数据库内容始终是正确的并且在它们尝试并输入影片数据，这是无效时向最终用户提供有用的错误消息。 我们将首先通过将一些验证逻辑添加到影片类。

右键单击模型文件夹，然后选择添加类。 将你类影片。

当我们在前面创建电影实体模型时，IDE 将创建电影类。 事实上，电影类的一部分，可以在一个文件，部分位于另一个。 这称为分部类。 我们将从另一个文件扩展电影类。

我们将使用某些属性，将为系统验证提示创建部分电影类，指向"合作者类"。 我们将标题和价格为必选、 标记，还部署价格是在某个特定范围内。 右键单击 Models 文件夹，然后选择添加类。 将你的类电影，然后单击确定按钮。 下面是我们部分的电影类看起来类似。

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

重新运行你的应用程序并尝试输入制定价格超过 100 影片。 已提交了表单之后，你将收到错误。 错误将捕获在服务器端，并发送窗体后发生。 请注意如何 ASP.NET MVC 的内置 HTML 帮助器已足够智能，可显示错误消息和维护为我们在文本框中元素的值：

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

这非常适用，但如果我们无法将告诉用户在客户端立即之前服务器获取涉及, 那就太好。

让我们启用使用 JavaScript 某些客户端验证。

## <a name="adding-client-side-validation"></a>添加客户端验证

由于我们电影类已有一些验证特性，我们将只需将几个 JavaScript 文件添加到我们 Create.aspx 视图模板和添加一行代码以启用客户端验证做好准备。

在中 VWD 转我们视图/电影文件夹，并打开 Create.aspx。

打开解决方案资源管理器中的脚本文件夹并将其拖到中的以下三个脚本&lt;头&gt;标记。

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

你想要按此顺序显示这些脚本文件。

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

此外，将添加 Html.BeginForm 上面这一行：

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

下面是在 IDE 中所示的代码。

[![电影-Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

运行你的应用程序和再次访问 /Movies/Create 并单击创建而无需输入任何数据。 错误消息显示立即不带闪存，我们将关联与发送数据的页面一直退回到服务器。 这是因为 ASP.NET MVC 现在验证上同时的输入 （使用 JavaScript） 客户端和服务器上。

[![创建的 Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

这看起来很好 ！ 让我们现在添加到数据库的一个其他列。

>[!div class="step-by-step"]
[上一页](getting-started-with-mvc-part6.md)
[下一页](getting-started-with-mvc-part8.md)
