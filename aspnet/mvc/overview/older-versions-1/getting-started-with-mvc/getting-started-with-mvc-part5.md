---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: "从控制器访问您的模型的数据 |Microsoft 文档"
author: shanselman
description: "这是初学者本教程介绍 ASP.NET MVC 的基础知识。 创建一个简单的 web 应用程序读取和写入数据库中。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: cf81d351aef45af3640f5d113eb3619911e03606
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="accessing-your-models-data-from-a-controller"></a>从控制器访问您的模型的数据
====================
通过[Scott Hanselman](https://github.com/shanselman)

> 这是初学者本教程介绍 ASP.NET MVC 的基础知识。 将创建一个简单的 web 应用程序读取和写入数据库中。 请访问[ASP.NET MVC 学习中心](../../../index.md)若要查找其他 ASP.NET MVC 教程和示例。


在本部分中我们将创建新的 MoviesController 类，并编写一些代码来检索我们影片数据并将其显示回浏览器使用视图模板。

右键单击 Controllers 文件夹，并使新 MoviesController。

[![添加控制器](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

这将创建我们项目中我们 \Controllers 文件夹下的新"MoviesController.cs"文件。 让我们更新 MovieController 从我们新填充的数据库中检索的影片列表。

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

我们正在执行 LINQ 查询，以便我们只能检索发布后 1984年夏天的电影。 我们将需要一个视图模板来呈现电影返回此列表，因此在方法中右键单击，然后选择添加视图，以创建它。

在添加视图对话框将指示我们会将一个列表传递&lt;Movies.Models.Movie&gt;到我们查看模板。 与不同的是我们使用添加视图对话框并选择"空"模板创建以前的时间，我们将指示这次我们希望 Visual Studio 自动"搭建基架"视图模板为我们具有一些默认内容。 我们需要执行此操作通过选择"列表"项中的"视图内容的下拉列表菜单。

请记住，你可以创建一个新的类，你将需要编译为其显示在添加的视图对话框的应用程序。

![添加视图](getting-started-with-mvc-part5/_static/image3.png)

单击添加和系统将自动生成的代码用于视图为我们显示我们的影片列表。 这是更改的好时机&lt;h2&gt;像我们前面与 Hello World 视图标题为"我的电影列表"类似。

[![电影-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

运行你的应用程序，并在地址栏中访问 /Movies。 现在我们已使用控制器中的基本查询从数据库检索数据并将数据返回到知道电影的视图。 该视图然后旋转通过影片列表，并为我们创建数据的表。

[![影片列表-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

因此我们不需要基架模板创建为我们的默认链接，我们不会将实现与此应用程序-编辑、 详细信息和删除功能。 打开 /Movies/Index.aspx 文件并将其删除。

下面是什么我们更新的视图模板应如下所示我们进行这些更改后的源代码：

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

它创建我们不需要的链接，因此我们将此示例删除它们。 不过，根据下一步，我们会保持我们创建新的链接 ！ 下面是我们的应用程序与删除此列的外观。

[![影片列表-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

我们现在有了影片数据的简单列表。 但是，如果我们单击"新建"链接，我们将收到错误，因为它未挂钩 ！ 让我们实现创建的操作方法，并使用户能够在我们的数据库中输入新影片。

>[!div class="step-by-step"]
[上一页](getting-started-with-mvc-part4.md)
[下一页](getting-started-with-mvc-part6.md)
