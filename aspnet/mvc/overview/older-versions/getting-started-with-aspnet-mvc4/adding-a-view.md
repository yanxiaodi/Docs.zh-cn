---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: "添加视图 |Microsoft 文档"
author: Rick-Anderson
description: "注意： 本教程的更新的版本此处提供了使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全，请按照和演示要简单得多..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 60374ddc6754f7e312ad08b420268308a9935bb4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-view"></a>添加视图
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教程的更新的版本可[此处](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全，请按照要简单得多，并演示更多的功能。


在本节中你要修改`HelloWorldController`类以使用模板文件复制到完全封装生成 HTML 响应客户端的过程的视图。

你将创建视图模板文件使用[Razor 视图引擎](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)引入了 ASP.NET MVC 3。 基于 razor 视图模板具有*.cshtml*文件扩展名，并且提供一种简洁的方式来创建 HTML 输出使用 C#。 Razor 降至最低数量的字符时编写视图模板，所需的击键，并使快，流体编码工作流。

当前，`Index` 方法返回带有在控制器类中硬编码的消息的字符串。 更改`Index`方法以返回`View`对象，如下面的代码中所示：

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

`Index`上面方法使用视图模板来生成对浏览器的 HTML 响应。 控制器方法 (也称为[操作方法](http://rachelappel.com/asp.net-mvc-actionresults-explained))，如`Index`上面，方法通常返回[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (或从派生的类[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx))，不基元类型喜欢字符串。

在项目中，添加可与一个视图模板`Index`方法。 若要执行此操作，右键单击内`Index`方法，并单击**添加视图**。

![](adding-a-view/_static/image1.png)

**添加视图**对话框随即出现。 保留默认值的方式变，并单击**添加**按钮：

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld*文件夹和*MvcMovie\Views\HelloWorld\Index.cshtml*创建文件。 你可以看到在**解决方案资源管理器**:

![](adding-a-view/_static/image3.png)

如下所示*Index.cshtml*已创建的文件：

![HelloWorldIndex](adding-a-view/_static/image4.png)

添加下的以下 HTML`<h2>`标记。

[!code-html[Main](adding-a-view/samples/sample2.html)]

完整*MvcMovie\Views\HelloWorld\Index.cshtml*文件如下所示。

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

如果使用的 Visual Studio 2012 中，在解决方案资源管理器中，右键单击*Index.cshtml*文件，然后选择**在 Page Inspector 中的查看**。

![PI](adding-a-view/_static/image5.png)

[Page Inspector 教程](../../views/using-page-inspector-in-aspnet-mvc.md)提供了有关此新工具的详细信息。

或者，运行该应用程序，并浏览到`HelloWorld`控制器 (`http://localhost:xxxx/HelloWorld`)。 `Index`方法在你的控制器中未做大量的工作; 它只需运行该语句`return View()`，其指定该方法应使用的视图模板文件来呈现到浏览器的响应。 因为你未显式指定要使用的视图模板文件的名称，ASP.NET MVC 将默认为使用*Index.cshtml*视图文件中的*\Views\HelloWorld*文件夹。 下图显示字符串&quot;从我们的视图模板 Hello ！&quot;硬编码在视图中。

![](adding-a-view/_static/image6.png)

看起来一切都非常好。 但是，请注意，浏览器的标题栏显示&quot;索引我 ASP.NET A&quot; ，并在页面顶部的大链接显示&quot;此处你的徽标。&quot;下面&quot;你徽标此处。&quot;链接大约的注册和日志中的链接，或其下链接到主页，并向页。 让我们更改其中的某些类。

## <a name="changing-views-and-layout-pages"></a>更改视图和布局页

首先，你想要更改&quot;你徽标此处。&quot;页顶部的标题。 该文本是通用的每一页。 它是实际实现只能在一个位置，在项目中，即使在应用程序中的每一页上显示。 转到*/视图/共享*文件夹中的**解决方案资源管理器**并打开 *\_Layout.cshtml*文件。 此文件称为*布局页*共享它并且&quot;shell&quot;其他所有页使用。

![_LayoutCshtml](adding-a-view/_static/image7.png)

布局模板，可以在一个位置中指定你的站点的 HTML 容器布局，然后将它应用跨站点中的多个页面。 查找 `@RenderBody()` 行。 `RenderBody` 是显示创建的所有特定于视图的页面的占位符，已包装在布局页面中&quot;&quot;。 例如，如果选择关于链接， *Views\Home\About.cshtml*内呈现视图`RenderBody`方法。

更改从布局模板中的站点-标题标头&quot;此处你的徽标&quot;到&quot;MVC 影片&quot;。

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

标题元素的内容替换为以下标记：

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

运行应用程序，并请注意，它现在显示&quot;MVC 影片&quot;。 单击**有关**链接，你会看到该页面将显示如何&quot;MVC 影片&quot;也。 我们无法一次中的布局模板进行的更改，并具有站点上的所有页都反映新标题。

![](adding-a-view/_static/image8.png)

现在，让我们更改索引视图的标题。

打开*MvcMovie\Views\HelloWorld\Index.cshtml*。 有两个位置进行更改： 首先，文本显示的标题中的浏览器中，然后在辅助标头 (`<h2>`元素)。 稍稍对它们进行一些更改，这样可以看出哪一段代码更改了应用的哪部分。

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

指示要显示，集上面的代码的 HTML 标题`Title`属性`ViewBag`对象 (即在*Index.cshtml*视图模板)。 如果你查看返回的布局模板的源代码，你会注意到，该模板使用中的此值`<title>`作为的一部分的元素`<head>`一部分我们之前修改的 HTML。 使用此`ViewBag`方法中，你可以轻松地将传递其他参数视图模板与布局文件之间。

运行应用程序，并浏览到`http://localhost:xx/HelloWorld`。 请注意，浏览器标题、主标题和辅助标题已更改。 （如果没有在浏览器中看到更改，则可能正在查看缓存的内容。 在浏览器中按 Ctrl + F5 强制加载来自服务器的响应。）使用创建浏览器标题`ViewBag.Title`我们在中设置*Index.cshtml*查看模板和其他&quot;的电影应用&quot;在布局文件中添加。

另请注意如何在内容*Index.cshtml*视图模板与合并 *\_Layout.cshtml*视图模板和单个 HTML 响应发送到浏览器。 凭借布局模板可以很容易地对应用程序中所有页面进行更改。

![](adding-a-view/_static/image9.png)

我们一小段&quot;数据&quot;(在这种情况下&quot;从我们的视图模板 Hello ！&quot;消息) 但是硬编码。 MVC 应用程序具有&quot;V&quot; （视图） 并已获得了&quot;C&quot; （控制器），但不&quot;M&quot; （模型），尚未。 很快，我们将演练如何创建数据库并从它检索模型数据。

## <a name="passing-data-from-the-controller-to-the-view"></a>将数据从控制器传递给视图

我们转到数据库，并讨论模型之前，不过，让我们先介绍一下将从控制器的信息传递到一个视图。 在与传入 URL 请求的响应中将会调用控制器类。 控制器类是响应的在其中编写代码来处理传入的浏览器请求，从数据库检索数据并最终决定哪种类型将发送回浏览器。 查看模板随后可从控制器以生成并设置格式对浏览器的 HTML 响应。

控制器负责提供任何数据或对象所需顺序查看模板，以呈现到浏览器的响应。 一种最佳做法：**视图模板应永远不会执行业务逻辑或直接与数据库交互**。 相反，一个视图模板应只使用由控制器提供给它的数据。 维护这&quot;关注点分离&quot;干净、 可测试并且更易于维护，有助于保持你的代码。

目前，`Welcome`中的操作方法`HelloWorldController`类采用`name`和`numTimes`参数，然后输出直接向浏览器的值。 而不是有呈现为一个字符串，此响应的控制器，让我们更改控制器以改为使用视图模板。 视图模板将生成动态响应，这意味着你需要将适当的数据位从控制器传递给视图以生成响应。 你可以执行此操作通过让放在需要查看模板的动态数据 （参数） 的控制器`ViewBag`视图模板然后可以访问的对象。

返回到*HelloWorldController.cs*文件并将更改`Welcome`方法将添加`Message`和`NumTimes`值赋给`ViewBag`对象。 `ViewBag`是动态对象，这意味着你可以放置希望中包括的任何内容`ViewBag`对象具有未定义的属性，直到你输入其内部的一些内容。 [ASP.NET MVC 模型绑定系统](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)自动将映射的命名的参数 (`name`和`numTimes`) 从到你的方法中的参数的地址栏中的查询字符串。 完整的 HelloWorldController.cs 文件如下所示：

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

现在`ViewBag`对象包含将自动传递到视图的数据。

接下来，你需要一个欢迎视图模板 ！ 在**生成**菜单上，选择**生成 MvcMovie**若要确保编译项目。

然后右键单击内`Welcome`方法，并单击**添加视图**。

![](adding-a-view/_static/image10.png)

下面是什么**添加视图**对话框如下所示：

![](adding-a-view/_static/image11.png)

单击**添加**，然后添加以下代码在`<h2>`在新的元素*Welcome.cshtml*文件。 你将创建循环表明&quot;Hello&quot;让用户说出它应多次。 完整*Welcome.cshtml*文件如下所示。

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

运行应用程序，并浏览到以下 URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

现在是从 URL 中获取数据并将其传递给控制器使用[模型联编程序](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)。 控制器包数据插入`ViewBag`对象并将该对象传递到视图。 视图然后会以 html 格式的数据向用户显示。

![](adding-a-view/_static/image12.png)

在上面的示例中，我们使用`ViewBag`要将数据从控制器传递到视图对象。 后者在本教程中，我们将视图模型以将数据从控制器传递到视图。 视图模型种方法可以将数据传递是好得相比于视图包方法。 请参阅博客文章[动态 V 强类型化的视图](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx)有关详细信息。

当然，这是一种类型的&quot;M&quot;模型，但不数据库类型。 让我们用学到的内容来创建一个电影数据库。

>[!div class="step-by-step"]
[上一页](adding-a-controller.md)
[下一页](adding-a-model.md)
