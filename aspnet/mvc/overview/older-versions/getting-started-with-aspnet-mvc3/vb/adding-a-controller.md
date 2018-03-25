---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: "添加控制器 (VB) |Microsoft 文档"
author: Rick-Anderson
description: "本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是一个 ASP.NET MVC Web 应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 74113d76a74b1da27a7f9a33a13038a0c36ad036
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller-vb"></a>添加控制器 (VB)
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> 本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是 Microsoft Visual Studio 的免费版本的 ASP.NET MVC Web 应用程序的基础知识。 在开始之前，请确保已安装下面列出的先决条件。 你可以通过单击以下链接安装所有这些： [Web 平台安装程序](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，你可以单独安装系统必备组件，使用以下链接：
> 
> - [Visual Studio Web Developer Express SP1 系统必备](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools 更新](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（运行时 + 工具支持）
> 
> 如果你正在使用 Visual Studio 2010，而不 Visual Web Developer 2010，通过单击以下链接安装必备组件： [Visual Studio 2010 系统必备组件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> Visual Web Developer 项目与 VB.NET 的源代码是可以附带本主题。 [下载 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果你愿意 C#，切换到[C# 版本](../cs/adding-a-controller.md)本教程。


MVC 代表*模型-视图-控制器*。 MVC 是用于开发应用程序，以便每个部分具有单独的责任的模式：

- 模型： 你的应用程序数据。
- 视图： 你的应用程序将使用动态生成 HTML 响应的模板文件。
- 控制器： 处理应用程序，传入 URL 请求的类中检索模型数据，然后指定呈现对客户端的响应的视图模板。

我们将涵盖所有这些概念，在本教程中，向您展示如何使用它们来生成应用程序。

通过右键单击创建新的控制器*控制器*文件夹中的**解决方案资源管理器**，然后选择**添加控制器**。

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

新控制器命名&quot;HelloWorldController&quot;单击**添加**。

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

请注意，在**解决方案资源管理器**右侧，已为你创建一个新文件命名为*HelloWorldController.cs* ，以及是否在 IDE 中打开该文件。

在新`public class HelloWorldController`块中，创建类似于下面的代码的两个新方法。 作为示例，我们将直接从控制器返回 HTML 的字符串。

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

名为你的控制器`HelloWorldController`和新方法命名为`Index`。 运行应用程序 （按 F5 或 Ctrl + F5）。 一旦你的浏览器已启动，则追加&quot;HelloWorld&quot;地址栏中的路径。 (在我的计算机，它具有`http://localhost:43246/HelloWorld`) 你的浏览器将类似于下面的屏幕截图。 在上面的方法中，代码直接返回一个字符串。 我们告知系统以仅返回一些 HTML，并且它未 ！

![](adding-a-controller/_static/image5.png)

ASP.NET MVC 调用另一个控制器类 （和其中的不同的操作方法），具体取决于传入的 URL。 使用 ASP.NET MVC 的默认映射逻辑使用如下格式来控制调用哪些代码：

`/[Controller]/[ActionName]/[Parameters]`

URL 的第一部分确定要执行的控制器类。 因此*/HelloWorld*映射到`HelloWorldController`类。 URL 的第二部分确定要执行的类上的操作方法。 因此*/HelloWorld/索引*将导致`Index`方法`HelloWorldController`类执行。 请注意，我们仅必须访问*/HelloWorld*上面和`Index`时默认情况下使用方法。 这是因为方法名为`Index`是如果有一个未显式指定调用在控制器的默认方法。

浏览到 `http://localhost:xxxx/HelloWorld/Welcome`。 `Welcome`方法将运行并返回字符串&quot;这是欢迎操作方法...&quot;. 默认 MVC 映射是`/[Controller]/[ActionName]/[Parameters]`。 对于此 URL，该控制器是`HelloWorld`和`Welcome`是方法。 我们尚未使用`[Parameters]`尚未的 URL 的一部分。

![](adding-a-controller/_static/image6.png)

让我们修改该示例略有，以便我们可以将中的某些参数信息从 URL 中传递给控制器 (例如， */HelloWorld/欢迎？ 名称 = Scott&amp;numtimes = 4*)。 更改你`Welcome`方法以包括两个参数，如下所示。 请注意，我们已使用 VB 可选参数功能，则指示`numTimes`参数应默认为 1，如果为该参数不传递任何值。

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

运行你的应用程序，并浏览到`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **。** 你可以尝试不同的值`name`和`numtimes`。 系统会自动映射到你的方法中的参数来自你的地址栏中的查询字符串的命名的参数。

![](adding-a-controller/_static/image7.png)

已在这些示例中这两个控制器执行 MVC 的 VC 部分 — 即视图和控制器工作。 控制器直接返回 HTML。 通常，我们不希望直接返回 HTML，因为该按钮将变为非常麻烦的代码的控制器。 而是我们通常将使用单独的视图模板文件来帮助生成 HTML 响应。 让我们看一下如何我们可以执行此操作。

>[!div class="step-by-step"]
[上一页](intro-to-aspnet-mvc-3.md)
[下一页](adding-a-view.md)
