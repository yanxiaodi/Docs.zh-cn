---
title: "将视图添加到 MVC 应用程序"
author: Rick-Anderson
description: "将视图添加到 MVC 应用程序"
ms.author: riande
manager: wpickett
ms.date: 09/1721/2017
ms.topic: article
ms.technology: dotnet-mvc
ms.prod: .net-framework
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: 86887f0dafa31ff3eb6597284c469c4b3053b6b7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-view"></a>添加视图
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

在本节中你要修改`HelloWorldController`类以使用模板文件复制到完全封装生成 HTML 响应客户端的过程的视图。 

你将创建视图模板文件使用[Razor 视图引擎](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md)。 基于 razor 视图模板具有*.cshtml*文件扩展名，并且提供一种简洁的方式来创建 HTML 输出使用 C#。 Razor 降至最低数量的字符时编写视图模板，所需的击键，并使快，流体编码工作流。

当前，`Index` 方法返回带有在控制器类中硬编码的消息的字符串。 更改`Index`方法以返回`View`对象，如下面的代码中所示：

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

`Index`上面方法使用视图模板来生成对浏览器的 HTML 响应。 控制器方法 (也称为[操作方法](http://rachelappel.com/asp.net-mvc-actionresults-explained))，如`Index`上面，方法通常返回[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (或从派生的类[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx))，不基元类型喜欢字符串。

右键单击*Views\HelloWorld*文件夹，然后单击**添加**，然后单击**布局 (Razor) 使用的 MVC 5 视图页**。
  
![](adding-a-view/_static/image1.png)   
  
在**指定项名称**对话框框中，输入*索引*，然后单击**确定**。  
  
![](adding-a-view/_static/image2.png)  
  
在**选择布局页**对话框中，接受默认值 **\_Layout.cshtml**单击**确定**。  
  
![](adding-a-view/_static/image3.png)  
  
在更高版本，对话框*views/shared*的左窗格中选择文件夹。 如果另一个文件夹中有自定义布局文件，你可以选择它。 我们将讨论的布局文件在本教程的更高版本

*MvcMovie\Views\HelloWorld\Index.cshtml*创建文件。

![](adding-a-view/_static/image4.png)

添加以下突出显示的标记。

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

右键单击*Index.cshtml*文件，然后选择**用浏览器查看**。

![PI](adding-a-view/_static/image5.png)

你还可以右键单击*Index.cshtml*文件，然后选择**Page Inspector 中的视图。** 请参阅[Page Inspector 教程](../../views/using-page-inspector-in-aspnet-mvc.md)有关详细信息。

或者，运行该应用程序，并浏览到`HelloWorld`控制器 (`http://localhost:xxxx/HelloWorld`)。 `Index`方法在你的控制器中未做大量的工作; 它只需运行该语句`return View()`，其指定该方法应使用的视图模板文件来呈现到浏览器的响应。 因为你未显式指定要使用的视图模板文件的名称，ASP.NET MVC 将默认为使用*Index.cshtml*视图文件中的*\Views\HelloWorld*文件夹。 下图显示字符串&quot;从我们的视图模板 Hello ！&quot;硬编码在视图中。

![](adding-a-view/_static/image6.png)

看起来一切都非常好。 但是，请注意，浏览器的标题栏显示&quot;索引-我的 ASP.NET 应用程序"，并在页面顶部的大链接显示"应用程序名称"。 具体取决于如何小进行浏览器窗口，你可能需要单击右上角，以查看中的三个栏到**主页**，**有关**，**联系人**， **注册**和**登录**链接。

## <a name="changing-views-and-layout-pages"></a>更改视图和布局页

首先，你想要更改&quot;应用程序名称&quot;页顶部的链接。 该文本是通用的每一页。 它是实际实现只能在一个位置，在项目中，即使在应用程序中的每一页上显示。 转到*/视图/共享*文件夹中的**解决方案资源管理器**并打开 *\_Layout.cshtml*文件。 此文件称为*布局页*它可在其他所有页都使用的共享文件夹中。

![_LayoutCshtml](adding-a-view/_static/image7.png)

布局模板，可以在一个位置中指定你的站点的 HTML 容器布局，然后将它应用跨站点中的多个页面。 查找 `@RenderBody()` 行。 `RenderBody` 是显示创建的所有特定于视图的页面的占位符，已包装在布局页面中&quot;&quot;。 例如，如果你选择**有关**链接， *Views\Home\About.cshtml*内呈现视图`RenderBody`方法。

更改标题元素的内容。 更改[ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx)中布局模板从&quot;应用程序名称&quot;到&quot;MVC 影片&quot;和从控制器`Home`到`Movies`。 完成布局文件如下所示：

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

运行应用程序，并请注意，它现在显示&quot;MVC 影片&quot;。 单击**有关**链接，你会看到该页面将显示如何&quot;MVC 影片&quot;也。 我们无法一次中的布局模板进行的更改，并具有站点上的所有页都反映新标题。

![](adding-a-view/_static/image8.png)

当我们首次创建*Views\HelloWorld\Index.cshtml*文件，它包含以下代码：

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

上面的 Razor 代码显式设置布局页。 检查*视图\\_ViewStart.cshtml*文件，它包含的确切的相同 Razor 标记。 *[视图\\_ViewStart.cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* 文件定义所有视图将都使用通用布局，因此你可以注释掉或删除从该代码*Views\HelloWorld\Index.cshtml*文件。

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

可以使用 `Layout` 属性设置不同的布局视图，或将它设置为 `null`，这样将不会使用任何布局文件。

现在，让我们更改索引视图的标题。

打开*MvcMovie\Views\HelloWorld\Index.cshtml*。 有两个位置进行更改： 首先，文本显示的标题中的浏览器中，然后在辅助标头 (`<h2>`元素)。 稍稍对它们进行一些更改，这样可以看出哪一段代码更改了应用的哪部分。

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

指示要显示，集上面的代码的 HTML 标题`Title`属性`ViewBag`对象 (即在*Index.cshtml*视图模板)。 请注意，布局模板 ( *views/shared\\_Layout.cshtml* ) 使用此值在`<title>`作为的一部分的元素`<head>`一部分我们之前修改的 HTML。

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

使用此`ViewBag`方法中，你可以轻松地将传递其他参数视图模板与布局文件之间。

运行该应用程序。 请注意，浏览器标题、主标题和辅助标题已更改。 （如果没有在浏览器中看到更改，则可能正在查看缓存的内容。 在浏览器中按 Ctrl + F5 强制加载来自服务器的响应。）使用创建浏览器标题`ViewBag.Title`我们在中设置*Index.cshtml*查看模板和其他&quot;的电影应用&quot;在布局文件中添加。

另请注意如何在内容*Index.cshtml*视图模板与合并 *\_Layout.cshtml*视图模板和单个 HTML 响应发送到浏览器。 凭借布局模板可以很容易地对应用程序中所有页面进行更改。

![](adding-a-view/_static/image9.png)

我们一小段&quot;数据&quot;(在这种情况下&quot;从我们的视图模板 Hello ！&quot;消息) 但是硬编码。 MVC 应用程序具有&quot;V&quot; （视图） 并已获得了&quot;C&quot; （控制器），但不&quot;M&quot; （模型），尚未。 很快，我们将演练如何创建数据库并从它检索模型数据。

## <a name="passing-data-from-the-controller-to-the-view"></a>将数据从控制器传递给视图

我们转到数据库，并讨论模型之前，不过，让我们先介绍一下将从控制器的信息传递到一个视图。 在与传入 URL 请求的响应中将会调用控制器类。 控制器类是响应的在其中编写代码来处理传入的浏览器请求，从数据库检索数据并最终决定哪种类型将发送回浏览器。 查看模板随后可从控制器以生成并设置格式对浏览器的 HTML 响应。

控制器负责提供任何数据或对象所需顺序查看模板，以呈现到浏览器的响应。 一种最佳做法：**视图模板应永远不会执行业务逻辑或直接与数据库交互**。 相反，一个视图模板应只使用由控制器提供给它的数据。 维护这&quot;关注点分离&quot;干净、 可测试并且更易于维护，有助于保持你的代码。

目前，`Welcome`中的操作方法`HelloWorldController`类采用`name`和`numTimes`参数，然后输出直接向浏览器的值。 而不是有呈现为一个字符串，此响应的控制器，让我们更改控制器以改为使用视图模板。 视图模板将生成动态响应，这意味着你需要将适当的数据位从控制器传递给视图以生成响应。 你可以执行此操作通过让放在需要查看模板的动态数据 （参数） 的控制器`ViewBag`视图模板然后可以访问的对象。

返回到*HelloWorldController.cs*文件并将更改`Welcome`方法将添加`Message`和`NumTimes`值赋给`ViewBag`对象。 `ViewBag`是动态对象，这意味着你可以放置希望中包括的任何内容`ViewBag`对象具有未定义的属性，直到你输入其内部的一些内容。 [ASP.NET MVC 模型绑定系统](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)自动将映射的命名的参数 (`name`和`numTimes`) 从到你的方法中的参数的地址栏中的查询字符串。 完整的 HelloWorldController.cs 文件如下所示：

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

现在`ViewBag`对象包含将自动传递到视图的数据。 接下来，你需要一个欢迎视图模板 ！ 在**生成**菜单上，选择**生成解决方案**（或 Ctrl + Shift + B） 以确保编译项目。 右键单击*Views\HelloWorld*文件夹，然后单击**添加**，然后单击**布局 (Razor) 使用的 MVC 5 视图页**。
  
![](adding-a-view/_static/image10.png)   
  
在**指定项名称**对话框框中，输入*欢迎*，然后单击**确定**。   
  
在**选择布局页**对话框中，接受默认值 **\_Layout.cshtml**单击**确定**。  
  
![](adding-a-view/_static/image11.png)   

*MvcMovie\Views\HelloWorld\Welcome.cshtml*创建文件。

替换中的标记*Welcome.cshtml*文件。 你将创建循环表明&quot;Hello&quot;让用户说出它应多次。 完整*Welcome.cshtml*文件如下所示。

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

运行应用程序，并浏览到以下 URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

现在是从 URL 中获取数据并将其传递给控制器使用[模型联编程序](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)。 控制器包数据插入`ViewBag`对象并将该对象传递到视图。 视图然后会以 html 格式的数据向用户显示。

![](adding-a-view/_static/image12.png)

在上面的示例中，我们使用`ViewBag`要将数据从控制器传递到视图对象。 稍后在本教程中，我们将使用视图模型将数据从控制器传递给视图。 视图模型种方法可以将数据传递是好得相比于视图包方法。 请参阅博客文章[动态 V 强类型化的视图](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx)有关详细信息。 

当然，这是一种类型的&quot;M&quot;模型，但不数据库类型。 让我们用学到的内容来创建一个电影数据库。

>[!div class="step-by-step"]
[上一页](adding-a-controller.md)
[下一页](adding-a-model.md)
