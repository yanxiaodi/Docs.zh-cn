---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: "添加控制器 |Microsoft 文档"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: c8f317b2ac133f560461917af1588b7a1fa51c4f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-controller"></a>添加控制器
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

MVC 代表*模型-视图-控制器*。 MVC 是模式，用于开发的应用程序很好地设计、 可测试且易于维护。 基于 MVC 的应用程序包含：

- **M** odels： 类： 表示应用程序的数据和使用验证逻辑以强制实施针对这些数据的业务规则。
- **V** iews： 应用程序使用动态生成 HTML 响应的模板文件。
- **C** ontrollers： 处理传入的浏览器请求，类检索模型数据，然后指定将响应返回到浏览器的视图模板。

我们将涵盖所有这些概念进行了本系列教程，向您展示如何使用它们来生成应用程序。

> [!NOTE]
> 在上一步默认 MVC 模板被选中。 这将创建主页，帐户和默认情况下管理控制器。

让我们首先创建了控制器类。 在**解决方案资源管理器**，右键单击*控制器*文件夹，然后单击**添加**，然后**控制器**。


![](adding-a-controller/_static/image1.png)

在**添加基架**对话框中，单击**MVC 5 控制器-空**，然后单击**添加**。

![](adding-a-controller/_static/image2.png)  
 

将新控制器"HelloWorldController"，然后单击**添加**。

![添加控制器](adding-a-controller/_static/image3.png)

请注意，在**解决方案资源管理器**，新的文件具有已创建的名为*HelloWorldController.cs*和一个新文件夹*Views\HelloWorld*。 在 IDE 中打开控制器。

![](adding-a-controller/_static/image4.png)

文件的内容替换为以下代码。

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

例如，控制器方法将返回 HTML 的字符串。 控制器命名为`HelloWorldController`和名为第一种方法`Index`。 让我们从浏览器调用它。 运行应用程序 （按 F5 或 Ctrl + F5）。 在浏览器中，追加&quot;HelloWorld&quot;地址栏中的路径。 (例如，在下面的图`http://localhost:1234/HelloWorld.`) 在浏览器页面将如下所示的以下屏幕截图。 在上面的方法中，代码直接返回一个字符串。 告知系统以仅返回一些 HTML，并且它未 ！

![](adding-a-controller/_static/image5.png)

ASP.NET MVC 调用另一个控制器类 （和其中的不同的操作方法），具体取决于传入的 URL。 使用 ASP.NET MVC 的默认 URL 路由逻辑使用如下格式来确定哪些代码调用：

`/[Controller]/[ActionName]/[Parameters]`

设置中的路由的格式*应用\_Start/RouteConfig.cs*文件。

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

当你运行应用程序，并且不提供任何 URL 段时，它默认为"Home"控制器和"索引"操作方法的上面的代码中的默认值部分中指定。

URL 的第一部分确定要执行的控制器类。 因此*/HelloWorld*映射到`HelloWorldController`类。 URL 的第二部分确定要执行的类上的操作方法。 因此*/HelloWorld/索引*将导致`Index`方法`HelloWorldController`类执行。 请注意，我们仅必须浏览到*/HelloWorld*和`Index`时默认情况下使用方法。 这是因为方法名为`Index`是如果有一个未显式指定调用在控制器的默认方法。 URL 段的第三部分 (`Parameters`) 针对的是路由数据。 在本教程中，我们将更高版本上看到路由数据。

浏览到 `http://localhost:xxxx/HelloWorld/Welcome`。 `Welcome`方法将运行并返回字符串&quot;这是欢迎操作方法...&quot;. 默认 MVC 映射是`/[Controller]/[ActionName]/[Parameters]`。 对于此 URL，采用 `HelloWorld` 控制器和 `Welcome` 操作方法。 目前尚未使用 URL 的 `[Parameters]` 部分。

![](adding-a-controller/_static/image6.png)

让我们修改该示例略有，以便你可以将某些参数信息从 URL 中传递到控制器 (例如， */HelloWorld/欢迎？ 名称 = Scott&amp;numtimes = 4*)。 更改你`Welcome`方法以包括两个参数，如下所示。 请注意，代码将使用 C# 可选参数的功能，则指示`numTimes`参数应默认为 1，如果为该参数不传递任何值。

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> 安全说明： 使用上面的代码[HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx)以防止恶意输入 (即 JavaScript) 的应用程序。 有关详细信息请参阅[如何： 保护对脚本利用在 Web 应用程序中进行应用 HTML 编码为字符串](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx)。


 运行你的应用程序并浏览到示例 URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`)。 你可以尝试不同的值`name`和`numtimes`在 URL 中。 [ASP.NET MVC 模型绑定系统](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)自动将映射到你的方法中的参数的查询字符串中的地址栏中的命名的参数。

![](adding-a-controller/_static/image7.png)

在上面的 URL 段的示例 ( `Parameters`) 未使用，`name`和`numTimes`参数作为进行传递[的查询字符串](http://en.wikipedia.org/wiki/Query_string)。 ?  （问号） 在上述 URL 是一个分隔符，并且遵循一定的查询字符串。 &amp; 字符用于分隔查询字符串。

欢迎方法替换为以下代码：

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

运行应用程序并输入以下 URL:`http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

这一次的第三个 URL 段匹配的路由参数`ID.``Welcome`操作方法包含的参数 (`ID`) 匹配中的 URL 规范`RegisterRoutes`方法。

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

在 ASP.NET MVC 应用程序，它是更常见的做法传入作为路由数据 （例如，我们 id 上面所做的那样） 比将它们作为查询字符串中传递的参数。 您也可以添加一个路由，都传递`name`和`numtimes`在参数中为在 URL 中的路由数据。 在*应用\_Start\RouteConfig.cs*文件中，添加"Hello"路由：

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

运行应用程序，并浏览到`/localhost:XXX/HelloWorld/Welcome/Scott/3`。

![](adding-a-controller/_static/image9.png)

对于许多 MVC 应用程序，默认路由工作正常。 你将了解将数据使用模型联编程序，本教程中稍后和无需修改该默认路由。

已在这些示例中执行操作控制器&quot;VC&quot; MVC 部分 — 也就是说，视图和控制器工作。 控制器直接返回 HTML。 通常，你不希望直接返回 HTML，因为该按钮将变为非常麻烦的代码的控制器。 而是我们通常将使用单独的视图模板文件来帮助生成 HTML 响应。 让我们来看如何我们可以执行此操作在下一步。

>[!div class="step-by-step"]
[上一页](getting-started.md)
[下一页](adding-a-view.md)
