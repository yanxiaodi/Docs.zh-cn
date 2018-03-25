---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: "添加控制器 |Microsoft 文档"
author: Rick-Anderson
description: "注意： 本教程的更新的版本此处提供了使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全，请按照和演示要简单得多..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 69af91401e51470fbc0b67103345325201b06723
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller"></a>添加控制器
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教程的更新的版本可[此处](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全，请按照要简单得多，并演示更多的功能。


MVC 代表*模型-视图-控制器*。 MVC 是模式，用于开发的应用程序很好地设计、 可测试且易于维护。 基于 MVC 的应用程序包含：

- **M** odels： 类： 表示应用程序的数据和使用验证逻辑以强制实施针对这些数据的业务规则。
- **V** iews： 应用程序使用动态生成 HTML 响应的模板文件。
- **C** ontrollers： 处理传入的浏览器请求，类检索模型数据，然后指定将响应返回到浏览器的视图模板。

我们将涵盖所有这些概念进行了本系列教程，向您展示如何使用它们来生成应用程序。

让我们首先创建了控制器类。 在**解决方案资源管理器**，右键单击*控制器*文件夹，然后选择**添加控制器**。

![](adding-a-controller/_static/image1.png)

新控制器命名&quot;HelloWorldController&quot;。 保留的默认模板**空 MVC 控制器**单击**添加**。

![添加控制器](adding-a-controller/_static/image2.png)

请注意，在**解决方案资源管理器**，新的文件具有已创建的名为*HelloWorldController.cs*。 在 IDE 中打开该文件。

![](adding-a-controller/_static/image3.png)

文件的内容替换为以下代码。

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

例如，控制器方法将返回 HTML 的字符串。 控制器命名为`HelloWorldController`和名为上面的第一个方法`Index`。 让我们从浏览器调用它。 运行应用程序 （按 F5 或 Ctrl + F5）。 在浏览器中，追加&quot;HelloWorld&quot;地址栏中的路径。 (例如，在下面的图`http://localhost:1234/HelloWorld.`) 在浏览器页面将如下所示的以下屏幕截图。 在上面的方法中，代码直接返回一个字符串。 告知系统以仅返回一些 HTML，并且它未 ！

![](adding-a-controller/_static/image4.png)

ASP.NET MVC 调用另一个控制器类 （和其中的不同的操作方法），具体取决于传入的 URL。 使用 ASP.NET MVC 的默认 URL 路由逻辑使用如下格式来确定哪些代码调用：

`/[Controller]/[ActionName]/[Parameters]`

URL 的第一部分确定要执行的控制器类。 因此*/HelloWorld*映射到`HelloWorldController`类。 URL 的第二部分确定要执行的类上的操作方法。 因此*/HelloWorld/索引*将导致`Index`方法`HelloWorldController`类执行。 请注意，我们仅必须浏览到*/HelloWorld*和`Index`时默认情况下使用方法。 这是因为方法名为`Index`是如果有一个未显式指定调用在控制器的默认方法。

浏览到 `http://localhost:xxxx/HelloWorld/Welcome`。 `Welcome`方法将运行并返回字符串&quot;这是欢迎操作方法...&quot;. 默认 MVC 映射是`/[Controller]/[ActionName]/[Parameters]`。 对于此 URL，采用 `HelloWorld` 控制器和 `Welcome` 操作方法。 目前尚未使用 URL 的 `[Parameters]` 部分。

![](adding-a-controller/_static/image5.png)

让我们修改该示例略有，以便你可以将某些参数信息从 URL 中传递到控制器 (例如， */HelloWorld/欢迎？ 名称 = Scott&amp;numtimes = 4*)。 更改你`Welcome`方法以包括两个参数，如下所示。 请注意，代码将使用 C# 可选参数的功能，则指示`numTimes`参数应默认为 1，如果为该参数不传递任何值。

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

运行你的应用程序并浏览到示例 URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`。 你可以尝试不同的值`name`和`numtimes`在 URL 中。 [ASP.NET MVC 模型绑定系统](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)自动将映射到你的方法中的参数的查询字符串中的地址栏中的命名的参数。

![](adding-a-controller/_static/image6.png)

在这些示例中这两个控制器就已开始使用&quot;VC&quot; MVC 部分 — 也就是说，视图和控制器工作。 控制器直接返回 HTML。 通常，你不希望直接返回 HTML，因为该按钮将变为非常麻烦的代码的控制器。 而是我们通常将使用单独的视图模板文件来帮助生成 HTML 响应。 让我们来看如何我们可以执行此操作在下一步。

>[!div class="step-by-step"]
[上一页](intro-to-aspnet-mvc-4.md)
[下一页](adding-a-view.md)
