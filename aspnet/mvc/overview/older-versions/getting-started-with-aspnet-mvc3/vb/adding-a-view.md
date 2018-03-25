---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: "添加的视图 (VB) |Microsoft 文档"
author: Rick-Anderson
description: "本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是一个 ASP.NET MVC Web 应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 7e8564c743510780b93d56bc1215f4c5b1faeb43
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-view-vb"></a>添加的视图 (VB)
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
> Visual Web Developer 项目与 VB.NET 的源代码是可以附带本主题。 [下载 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果你愿意 C#，切换到[C# 版本](../cs/adding-a-view.md)本教程。


在本部分中我们将修改`HelloWorldController`类使用的视图模板文件，以便完全封装生成 HTML 响应客户端的过程。

让我们开始使用具有的视图模板`Index`中的方法`HelloWorldController`类。 当前`Index`方法返回使用的是硬编码在控制器类中的消息的字符串。 更改`Index`方法以返回`View`对象，如在下面的示例所示：

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

让我们现在将视图模板添加到我们可以调用与我们项目`Index`方法。 若要执行此操作，右键单击内`Index`方法，并单击**添加视图**。

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

**添加视图**对话框随即出现。 保留默认条目，然后单击**添加**按钮。

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

*MvcMovie\Views\HelloWorld*文件夹和*MvcMovie\Views\HelloWorld\Index.vbhtml*创建文件。 你可以看到在**解决方案资源管理器**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

添加一些 HTML 下的`<h2>`标记。 已修改*MvcMovie\Views\HelloWorld\Index.vbhtml*文件如下所示。

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

运行应用程序，并浏览到&quot;你好 world&quot;控制器 (`http://localhost:xxxx/HelloWorld`)。 `Index`方法在你的控制器中未做大量的工作; 它只需运行该语句`return View()`，这指示我们想要使用的视图模板文件来呈现对客户端的响应。 ASP.NET MVC 因为我们未显式指定要使用的视图模板文件的名称，默认为使用*Index.vbhtml*视图文件中的*\Views\HelloWorld*文件夹。 下图显示了硬编码在视图中的字符串。

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

看起来一切都非常好。 但请注意浏览器的标题栏将显示&quot;索引&quot;，并在页上的大标题显示&quot;我的 MVC 应用程序。&quot;让我们更改那些。

## <a name="changing-views-and-layout-pages"></a>更改视图和布局页面

首先，让我们更改文本&quot;我的 MVC 应用程序。&quot;该文本共享，并显示在每一页。 它实际出现在只有一个位置中我们项目中，即使它位于我们的应用程序中每页上。 转到*/视图/共享*文件夹中的**解决方案资源管理器**并打开 *\_Layout.vbhtml*文件。 此文件称为布局页，它是共享&quot;shell&quot;其他所有页使用。

请注意`@RenderBody()`的文件的底部附近的代码行。 `RenderBody`是你创建的所有页面，都会都显示其中一个占位符&quot;包装&quot;布局页中。 更改`<h1>`标题从 **&quot;**  My MVC Application&quot;到&quot;MVC 影片应用程序&quot;。

[!code-html[Main](adding-a-view/samples/sample3.html)]

运行应用程序，并记下它现在显示&quot;MVC 影片应用程序&quot;。 单击**有关**链接，并且页面显示了&quot;MVC 影片应用程序&quot;也。

完整 *\_Layout.vbhtml*文件如下所示：

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

现在，让我们更改索引页 （视图） 的标题。

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

打开*MvcMovie\Views\HelloWorld\Index.vbhtml*。 有两个位置进行更改： 首先，文本显示的标题中的浏览器中，然后在辅助标头 (`<h2>`元素)。 我们将使它们略有不同以便你可以查看代码的哪些位更改应用程序的哪个部分。

运行应用程序，并浏览到`http://localhost:xx/HelloWorld`。 请注意，浏览器标题、主标题和辅助标题已更改。 很容易地对视图进行少量更改将应用程序中的大更改。 （如果没有在浏览器中看到更改，则可能正在查看缓存的内容。 在浏览器中按 Ctrl + F5 强制加载来自服务器的响应。）

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

我们一小段&quot;数据&quot;(在这种情况下&quot;Hello World ！&quot;消息) 但是硬编码。 我们 MVC 应用程序具有 V (views)，我们已经有了 C （控制器），但尚没有的 M （模型）。 很快，我们将演练如何创建数据库并从它检索模型数据。

## <a name="passing-data-from-the-controller-to-the-view"></a>将数据从控制器传递给视图

我们转到数据库，并讨论模型之前，不过，让我们先介绍一下将从控制器的信息传递给视图。 我们要将传递以呈现给客户端 HTML 响应所需的视图模板。 这些对象是通常创建了控制器类由传递给视图模板和它们应包含视图模板所需的数据-不多。

以前与`HelloWorldController`类，`Welcome`操作方法所花`name`和`numTimes`参数，然后使用参数值，向浏览器的输出。 而是必须继续直接呈现此响应的控制器，让我们改为我们将将放该数据在一个包视图。 可以使用控制器和视图`ViewBag`对象来存放该数据。 将传递到一个视图模板自动，并用于呈现 HTML 响应作为数据使用的包内容。 控制器致力于一件事和与另一个的视图模板通过这种方式 — 使我们能够维护干净&quot;关注点分离&quot;应用程序中。

或者，我们无法定义自定义的类、 然后我们自己在创建该对象的实例、 填充数据和将其传递给该视图。 情况通常称作视图模型，因为它是自定义模型的视图。 对于少量的数据，但是，ViewBag 非常适用。

返回到*HelloWorldController.vb*文件更改`Welcome`中将该消息和 NumTimes 放入 ViewBag 控制器方法。 ViewBag 是动态对象。 这意味着你可以任何你想要将放置到它。 ViewBag 一直输入其内部的一些内容，将任何定义的属性。

完整`HelloWorldController.vb`与相同的文件中的新类。

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

现在我们 ViewBag 包含将通过传递到视图自动的数据。 同样，或者我们无法具有传递如下我们自己对象中如果我们喜欢：

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

现在我们需要`WelcomeView`模板 ！ 运行应用程序，因此编译新代码。 关闭浏览器中，右键单击内`Welcome`方法，，然后单击**添加视图**。

下面是你**添加视图**对话框如下所示。

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

添加以下代码在`<h2>`在新的元素*欢迎。*vbhtml 文件。 我们将进行循环，说&quot;Hello&quot;让用户说出我们应多次 ！

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

运行应用程序，并浏览到`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

现在数据是来自 URL，并自动传递到的控制器。 控制器包将数据分成`Model`对象并将该对象传递到视图。 不是向用户显示以 html 格式的数据视图。

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

当然，这是一种类型的&quot;M&quot;模型，但不数据库类型。 让我们用学到的内容来创建一个电影数据库。

>[!div class="step-by-step"]
[上一页](adding-a-controller.md)
[下一页](adding-a-model.md)
