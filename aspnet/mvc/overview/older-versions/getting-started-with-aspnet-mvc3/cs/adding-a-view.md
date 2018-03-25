---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: "添加的视图 (C#) |Microsoft 文档"
author: Rick-Anderson
description: "本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是一个 ASP.NET MVC Web 应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 46d5494e668dfe156aeb6647ded83e6ce5366714
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-view-c"></a>添加的视图 (C#)
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教程的更新的版本可[此处](../../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全，请按照要简单得多，并演示更多的功能。
> 
> 
> 本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是 Microsoft Visual Studio 的免费版本的 ASP.NET MVC Web 应用程序的基础知识。 在开始之前，请确保已安装下面列出的先决条件。 你可以通过单击以下链接安装所有这些： [Web 平台安装程序](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，你可以单独安装系统必备组件，使用以下链接：
> 
> - [Visual Studio Web Developer Express SP1 系统必备](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools 更新](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（运行时 + 工具支持）
> 
> 如果你正在使用 Visual Studio 2010，而不 Visual Web Developer 2010，通过单击以下链接安装必备组件： [Visual Studio 2010 系统必备组件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用 C# 源代码的 Visual Web Developer 项目是可以附带本主题。 [下载的 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果你愿意 Visual Basic，切换到[Visual Basic 版本](../vb/intro-to-aspnet-mvc-3.md)本教程。


在本节中你要修改`HelloWorldController`类以使用模板文件复制到完全封装生成 HTML 响应客户端的过程的视图。

你将创建使用新的视图模板文件[Razor 视图引擎](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)引入了 ASP.NET MVC 3。 基于 razor 视图模板具有*.cshtml*文件扩展名，并且提供一种简洁的方式来创建 HTML 输出使用 C#。 Razor 降至最低数量的字符时编写视图模板，所需的击键，并使快，流体编码工作流。

使用具有的视图模板启动`Index`中的方法`HelloWorldController`类。 当前，`Index` 方法返回带有在控制器类中硬编码的消息的字符串。 更改`Index`方法以返回`View`对象，如在下面的示例所示：

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

此代码使用视图模板以生成对浏览器的 HTML 响应。 在项目中，添加可与一个视图模板`Index`方法。 若要执行此操作，右键单击内`Index`方法，并单击**添加视图**。

![](adding-a-view/_static/image1.png)

**添加视图**对话框随即出现。 保留默认值的方式变，并单击**添加**按钮：

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld*文件夹和*MvcMovie\Views\HelloWorld\Index.cshtml*创建文件。 你可以看到在**解决方案资源管理器**:

![](adding-a-view/_static/image3.png)

如下所示*Index.cshtml*已创建的文件：

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

添加一些 HTML 下的`<h2>`标记。 已修改*MvcMovie\Views\HelloWorld\Index.cshtml*文件如下所示。

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

运行应用程序，并浏览到`HelloWorld`控制器 (`http://localhost:xxxx/HelloWorld`)。 `Index`方法在你的控制器中未做大量的工作; 它只需运行该语句`return View()`，其指定该方法应使用的视图模板文件来呈现到浏览器的响应。 因为你未显式指定要使用的视图模板文件的名称，ASP.NET MVC 将默认为使用*Index.cshtml*视图文件中的*\Views\HelloWorld*文件夹。 下图显示了硬编码在视图中的字符串。

![](adding-a-view/_static/image6.png)

看起来一切都非常好。 但是，请注意，浏览器的标题栏将显示为"Index"，并在页上的大标题显示"My MVC Application。" 让我们更改那些。

## <a name="changing-views-and-layout-pages"></a>更改视图和布局页

首先，你想要更改页面顶部的"My MVC Application"标题。 该文本是通用的每一页。 它实际上被实现只能在一个位置，在项目中，即使在应用程序中的每一页上显示。 转到*/视图/共享*文件夹中的**解决方案资源管理器**并打开 *\_Layout.cshtml*文件。 此文件称为*布局页*且共享的"shell"其他所有页使用。

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

布局模板，可以在一个位置中指定你的站点的 HTML 容器布局，然后将它应用跨站点中的多个页面。 请注意`@RenderBody()`文件底部附近的行。 `RenderBody`是，你创建的所有特定于视图的页面都显示，"包装"在布局页中的占位符。 更改布局模板从"My MVC Application"到"MVC 影片应用程序"中的标题标头。

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

运行应用程序，并请注意它现在显示"MVC 影片应用程序"。 单击**有关**链接，你会看到如何该页面显示"MVC 影片应用程序，"过。 我们无法一次中的布局模板进行的更改，并具有站点上的所有页都反映新标题。

![](adding-a-view/_static/image9.png)

完整 *\_Layout.cshtml*文件如下所示：

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

现在，让我们更改索引页 （视图） 的标题。

打开*MvcMovie\Views\HelloWorld\Index.cshtml*。 有两个位置进行更改： 首先，文本显示的标题中的浏览器中，然后在辅助标头 (`<h2>`元素)。 稍稍对它们进行一些更改，这样可以看出哪一段代码更改了应用的哪部分。

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

指示要显示，集上面的代码的 HTML 标题`Title`属性`ViewBag`对象 (即在*Index.cshtml*视图模板)。 如果你查看返回的布局模板的源代码，你会注意到，该模板使用中的此值`<title>`作为的一部分的元素`<head>`HTML 的一部分。 使用此方法，可以查看模板和布局文件之间轻松地传递其他参数。

运行应用程序，并浏览到`http://localhost:xx/HelloWorld`。 请注意，浏览器标题、主标题和辅助标题已更改。 （如果没有在浏览器中看到更改，则可能正在查看缓存的内容。 在浏览器中按 Ctrl + F5 强制加载来自服务器的响应。）

另请注意如何在内容*Index.cshtml*视图模板与合并 *\_Layout.cshtml*视图模板和单个 HTML 响应发送到浏览器。 凭借布局模板可以很容易地对应用程序中所有页面进行更改。

![](adding-a-view/_static/image10.png)

但我们这一点点“数据”（在此示例中为“Hello from our View Template!” 消息）是硬编码的。 MVC 应用程序有一个“V”（视图），而你已有一个“C”（控制器），但还没有“M”（模型）。 很快，我们将演练如何创建数据库并从它检索模型数据。

## <a name="passing-data-from-the-controller-to-the-view"></a>将数据从控制器传递给视图

我们转到数据库，并讨论模型之前，不过，让我们先介绍一下将从控制器的信息传递到一个视图。 在与传入 URL 请求的响应中将会调用控制器类。 控制器类是响应的你在其中编写代码来处理传入的参数、 从数据库检索数据并最终决定哪种类型将发送回浏览器。 查看模板随后可从控制器以生成并设置格式对浏览器的 HTML 响应。

控制器负责提供任何数据或对象所需顺序查看模板，以呈现到浏览器的响应。 查看模板应永远不会执行业务逻辑或直接与数据库交互。 相反，它应只使用由控制器提供给它的数据。 维护此"分离问题"有助于使代码保持干净且更易于维护。

目前，`Welcome`中的操作方法`HelloWorldController`类采用`name`和`numTimes`参数，然后输出直接向浏览器的值。 而不是有呈现为一个字符串，此响应的控制器，让我们更改控制器以改为使用视图模板。 视图模板将生成动态响应，这意味着你需要将适当的数据位从控制器传递给视图以生成响应。 你可以执行此操作通过让将动态视图模板需要的数据控制器`ViewBag`视图模板然后可以访问的对象。

返回到*HelloWorldController.cs*文件并将更改`Welcome`方法将添加`Message`和`NumTimes`值赋给`ViewBag`对象。 `ViewBag`是动态对象，这意味着你可以放置希望中包括的任何内容`ViewBag`对象具有未定义的属性，直到你输入其内部的一些内容。 完整的 HelloWorldController.cs 文件如下所示：

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

现在`ViewBag`对象包含将自动传递到视图的数据。

接下来，你需要一个欢迎视图模板 ！ 在**调试**菜单上，选择**生成 MvcMovie**若要确保编译项目。

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

然后右键单击内`Welcome`方法，并单击**添加视图**。 下面是什么**添加视图**对话框如下所示：

![](adding-a-view/_static/image13.png)

单击**添加**，然后添加以下代码在`<h2>`在新的元素*Welcome.cshtml*文件。 你将创建让用户说出它应多次显示"Hello"的循环。 完整*Welcome.cshtml*文件如下所示。

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

运行应用程序，并浏览到以下 URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

现在数据是来自 URL，并自动传递到的控制器。 控制器包数据插入`ViewBag`对象并将该对象传递到视图。 视图然后会以 html 格式的数据向用户显示。

![](adding-a-view/_static/image14.png)

当然，这是模型的一种“M”类型，而不是数据库类。 让我们用学到的内容来创建一个电影数据库。

>[!div class="step-by-step"]
[上一页](adding-a-controller.md)
[下一页](adding-a-model.md)
