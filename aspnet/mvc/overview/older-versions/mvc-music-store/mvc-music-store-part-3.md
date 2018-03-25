---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: "第 3 部分： 视图和 Viewmodel |Microsoft 文档"
author: jongalloway
description: "本系列教程详细介绍所有生成 ASP.NET MVC 音乐商店示例应用程序所采取的步骤。 第 3 部分介绍视图和 Viewmodel。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 5b38f88283c5d2d93f0bab283dbd9451855d95e3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="part-3-views-and-viewmodels"></a>第 3 部分： 视图和 Viewmodel
====================
通过[Jon Galloway](https://github.com/jongalloway)

> MVC 音乐商店是，从而引入并说明如何使用 ASP.NET MVC 和 Visual Studio 进行 web 开发分步教程应用程序。  
>   
> MVC 音乐商店是一个轻型示例存储区实现，该类销售音乐集联机，并实现基本的站点管理、 用户登录，和购物车功能。  
>   
> 本系列教程详细介绍所有生成 ASP.NET MVC 音乐商店示例应用程序所采取的步骤。 第 3 部分介绍视图和 Viewmodel。


到目前为止我们已刚刚已返回字符串的控制器操作。 可以很好地了解如何控制器工作，但不是如何你想要构建实际的 web 应用程序。 我们将需要一种更好的方式，来返回到浏览器访问我们的站点生成 HTML – 其中我们可以用模板文件要更轻松地自定义的 HTML 内容的一种发回。 这正是视图执行的操作。

## <a name="adding-a-view-template"></a>添加视图模板

若要使用视图模板，我们将更改 HomeController 索引方法，以便返回 ActionResult，并将其返回 View()，如下面：

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

上述更改表示而不是返回一个字符串，我们改为想要使用"视图"来生成结果返回。

我们现在将添加到我们的项目中的适当的视图模板。 若要这样做我们将在索引操作方法中，将文本光标的位置，然后右键单击并选择"添加视图"。 此时将显示添加视图对话框：

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

"添加视图"对话框，我们可以快速轻松地生成查看模板文件。 默认情况下，"添加视图"对话框将预先填充视图模板创建，以便它与匹配将使用它的操作方法的名称。 由于我们使用我们 HomeController index （） 操作方法内的"添加视图"上下文菜单，上面的"添加视图"对话框为默认情况下预填充的视图名称具有"索引"。 我们无需更改任何在该对话框中的选项，因此单击添加按钮。

当我们单击添加按钮时，Visual Web Developer 将创建新 Index.cshtml 为我们在 \Views\Home 目录中，如果创建文件夹的视图模板尚不存在。

![](mvc-music-store-part-3/_static/image2.png)

"Index.cshtml"文件的名称和文件夹位置是重要的是，并遵循的默认 ASP.NET MVC 命名约定。 目录名称、 \Views\Home，匹配的控制器的名为 HomeController。 视图模板名称，索引，匹配的控制器操作方法，这将会显示该视图。

ASP.NET MVC 允许我们以避免让我们使用此命名约定来返回的视图时显式指定的名称或视图模板的位置。 它会默认情况下呈现 \Views\Home\Index.cshtml 视图模板当我们在我们 HomeController 内编写类似下面的代码：

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer 中创建并打开"Index.cshtml"查看模板之后我们单击"添加视图"对话框中的"添加"按钮。 Index.cshtml 的内容如下所示。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

此视图使用 Razor 语法，这是比使用 ASP.NET Web 窗体和以前版本的 ASP.NET MVC 中的 Web 窗体视图引擎更加简洁。 Web 窗体视图引擎在 ASP.NET MVC 3 中仍然可用，但许多开发人员查找 Razor 视图引擎很好地符合 ASP.NET MVC 开发。

前三行设置使用 ViewBag.Title 页面标题。 我们将查看原理更详细地很快，但首先让我们更新文本标题文本，并查看该页面。 更新&lt;h2&gt;标记说"此是 Home Page"，如下所示。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

运行应用程序显示我们的新文本是在主页页上可见。

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>有关通用站点元素中使用的布局

大多数网站具有多个页间共享的内容： 导航、 页脚、 徽标图像、 样式表的引用，等等。Razor 视图引擎使此更容易地管理使用页调用\_Layout.cshtml 其中已自动为创建我们在/视图/共享文件夹。

![](mvc-music-store-part-3/_static/image4.png)

双击此文件夹以查看该内容，其如下所示。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

我们单个视图中的内容将显示的@RenderBody（） 的命令，并且我们想要显示之外的任何公共内容可以添加到\_Layout.cshtml 标记。 我们将需要拥有对我们主页上和存储的区域，在站点中，所有页面上的具有链接的常见标头，因此我们将添加到正上方的模板我们 MVC 音乐商店@RenderBody（） 语句。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>更新样式表

空项目模板包括非常简化的 CSS 文件，其中只包含用于显示验证消息的样式。 一些其他的 CSS 和映像，以定义我们的站点，外观和感觉，使我们将添加中现在提供了我们设计器。

这是在可用的 MvcMusicStore Assets.zip 的内容目录中包括的更新的 CSS 文件和映像[MVC 音乐商店](https://github.com/evilDave/MVC-Music-Store)。 我们将在 Windows 资源管理器中选择这两个，并将其放入在 Visual Web Developer 中，我们的解决方案的内容文件夹中，如下所示：

![](mvc-music-store-part-3/_static/image5.png)

你将需要确认你是否要覆盖现有的 Site.css 文件。 单击是。

![](mvc-music-store-part-3/_static/image6.png)

你的应用程序的内容文件夹将现在显示，如下所示：

![](mvc-music-store-part-3/_static/image7.png)

现在让我们运行应用程序并查看我们更改在主页上的效果。

![](mvc-music-store-part-3/_static/image8.png)

- 让我们回顾一下会发生什么变化： HomeController 的索引操作方法找到，并且显示 \Views\Home\Index.cshtmlView 模板中，即使我们的代码称为"返回 View()"，因为我们查看模板遵循标准的命名约定。
- 主页上显示在 \Views\Home\Index.cshtml 视图模板中定义的简单欢迎消息。
- 使用主页页面我们\_Layout.cshtml 模板，因此在欢迎消息包含在标准站点 HTML 布局。

## <a name="using-a-model-to-pass-information-to-our-view"></a>使用模型将信息传递给我们视图

只是显示了硬编码 HTML 视图模板并不会使很感兴趣的 web 站点。 若要创建动态网站，我们将改为想要将信息从我们的控制器操作传递到我们查看模板。

在模型-视图-控制器模式中，的术语模型是指对象表示应用程序中的数据。 通常情况下，模型对象对应于表在数据库中，但它们并非一定。

返回 ActionResult 控制器操作方法可以将模型对象传递给该视图。 这样，到完全包生成响应，，然后将此信息传递到视图模板所需的所有信息的控制器用来生成相应的 HTML 响应。 这是最简单方法是查看在操作中了解，因此让我们开始吧。

首先我们将创建用于在我们存储表示风格和专辑某些模型类。 让我们开始通过创建流派类。 右键单击你的项目中的"模型"文件夹中，选择"添加类"选项，并将文件"Genre.cs"。

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

然后添加到已创建的类的公共字符串名称属性：

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*注意： 在您想知道的情况下 {获取; 设置;} 表示法正在使用 C# 的自动实现的属性功能。这为我们提供一个属性的优点而无需我们可以声明一个后备字段。*

接下来，请按照相同的步骤来创建一个具有标题和风格属性的唱片集类 （名为 Album.cs）：

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

现在我们可以修改 StoreController 用于显示我们的模型中的动态信息的视图。 如果-出于演示目的现在-我们名为基于的请求 ID 我们专辑，我们无法显示该信息，如下面的视图中所示。

![](mvc-music-store-part-3/_static/image10.png)

我们将开始通过更改存储详细信息操作，以便其显示单个唱片集的信息。 将"using"语句添加到顶部**StoreControllers**类，以包含 MvcMusicStore.Models 命名空间，因此我们不需要键入 MvcMusicStore.Models.Album，每次我们想要使用唱片集类。 此类的"using"部分现在应显示如下。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

接下来，我们将更新的详细信息的控制器操作，以便它返回 ActionResult 而不是一个字符串，正如我们做与 HomeController 的 Index 方法。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

现在我们可以修改逻辑，以返回到视图的唱片集对象。 本教程中稍后我们将检索数据从数据库 – 但对于现在我们将使用"虚拟数据"以开始。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*注意： 如果你熟悉 C#，你可能会假定使用 var 意味着我们唱片集变量是后期绑定。不正确-C# 编译器使用类型推理基于什么我们要为变量分配以确定该唱片集属于类型唱片集和编译本地唱片集变量用作唱片集类型，因此我们获取编译时检查和 Visual Studio 代码编辑器支持。*

让我们现在创建一个视图模板，它使用我们唱片集来生成 HTML 响应。 我们执行此操作之前，我们需要生成项目，以便添加视图对话框就会了解有关我们新创建的唱片集类。 可以生成项目时通过选择 Debug⇨Build MvcMusicStore 菜单项 （对于额外信用，你可以使用 Ctrl Shift B 快捷方式以生成项目）。

![](mvc-music-store-part-3/_static/image11.png)

现在，我们已设置了我们支持的类，我们已准备好生成我们查看模板。 在详细信息方法内右键单击并选择"添加视图..." 从上下文菜单中。

![](mvc-music-store-part-3/_static/image12.png)

我们将创建一个新的视图模板像我们之前使用 HomeController。 因为我们将从 StoreController 创建它将默认情况下生成 \Views\Store\Index.cshtml 文件中。

与不同之前，我们将选中"创建强类型"视图复选框。 然后，我们将选择"视图数据的类"放-downlist 中我们"唱片集"的类。 这将导致创建唱片集将传递给其使用的对象的视图模板所需的"添加视图"对话框。

![](mvc-music-store-part-3/_static/image13.png)

当单击"添加"按钮时，我们将创建我们 \Views\Store\Details.cshtml 查看模板，包含以下代码。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

请注意第一行，该值指示此视图为强类型化为我们唱片集的类。 Razor 视图引擎能够理解，它已被传递唱片集对象，以便我们可以轻松地访问模型属性，甚至可以安排在 Visual Web Developer 编辑器中 IntelliSense 的好处。

更新&lt;h2&gt;标记以便它显示唱片集的 Title 属性通过修改该行起来，如下所示。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

请注意当你输入后的期间触发时 IntelliSense@Model关键字，显示的属性和唱片集类支持的方法。

让我们现在重新运行我们的项目，并访问应用商店/详细信息/5 URL。 我们将看到类似下面唱片集的详细信息。

![](mvc-music-store-part-3/_static/image14.png)

现在我们将使应用商店浏览操作方法类似的更新。 更新方法，使其返回 ActionResult 和修改的方法的逻辑，以使它创建一个新的流派对象并将其返回到视图。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

浏览方法中右键单击并选择"添加视图..." 从上下文菜单，然后添加是强类型的视图类中添加强类型化风格。

![](mvc-music-store-part-3/_static/image15.png)

更新&lt;h2&gt;视图中的元素内的代码 （/Views/Store/Browse.cshtml) 以显示流派信息。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

现在让我们重新运行我们的项目，并浏览到应用商店/浏览？流派 = Disco URL。 我们将看到浏览页显示如下所示。

![](mvc-music-store-part-3/_static/image16.png)

最后，让我们进行到稍微更复杂的更新**存储索引**操作方法以及要在我们的存储区中显示的所有风格列表视图。 我们将为我们的模型对象，而不是单个流派，只需使用风格列表来执行该操作。

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

存储索引操作方法中右键单击，然后选择添加视图之前，选择 Genre 作为模型类，以及按添加按钮。

![](mvc-music-store-part-3/_static/image17.png)

首先我们将更改@model声明，以指示该视图将需要几个流派对象而不是仅仅一个。 更改 /Store/Index.cshtml 读取，如下所示的第一行：

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

这将告知 Razor 视图引擎，它将使用可以容纳多个流派对象模型对象。 我们将使用 IEnumerable&lt;流派&gt;而不是列表&lt;流派&gt;由于这是更通用的使我们能够将我们的模型类型更高版本更改为支持 IEnumerable 接口的任何对象类型。

接下来，我们将循环访问中的模型，如下面的已完成的视图代码中所示的流派对象。

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

请注意，我们具有完整的 IntelliSense 支持我们输入此代码，以便当我们键入"@Model。" 我们看到所有方法和支持的类型流派 IEnumerable 的属性。

![](mvc-music-store-part-3/_static/image18.png)

在我们"foreach"循环中，Visual Web Developer 知道，每个项的类型是流派，我们看到 IntelliSence 每个风格类型。

![](mvc-music-store-part-3/_static/image19.png)

接下来，基架功能检查流派对象，并确定，每个将包含一个 Name 属性，因此它循环访问并写出。它还将生成的每个项的编辑、 详细信息，并删除链接。 我们将利用这一点，更高版本中我们存储管理器中，但现在我们想要改为具有一个简单的列表。

当我们运行该应用程序和浏览到 /Store 时，我们看到显示的计数和风格的列表。

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>页之间添加链接

当前列出风格我们 /Store URL 只需以纯文本形式列出流派名称。 让我们更改这，以便而不是纯文本我们改为具有流派名称链接到相应的应用商店/浏览 URL，以便单击音乐流派，如"Disco"将导航到 / 存储/浏览？ 流派 = Disco URL。 我们无法更新我们 \Views\Store\Index.cshtml 视图模板使用这些链接的代码类似下面的输出到**（不键入此中-我们将在其上改进）**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

操作成功，但它可能会导致更高版本问题，因为它依赖于硬编码字符串。 例如，如果我们想要重命名控制器，我们需要我们寻找需要更新的链接的代码中进行搜索。

我们可以使用另一种方法是利用一个 HTML 帮助器方法。 ASP.NET MVC 包括 HTML 帮助器方法，可从我们查看模板代码可执行各种就像下面这样的常见任务。 Html.ActionLink() 帮助器方法是一个特别有用，并可以轻松地生成 HTML &lt;&gt;链接，并负责令人讨厌的详细信息，如确保 URL 路径正确进行 URL 编码。

Html.ActionLink() 有几个不同的重载来允许指定尽可能多的信息，根据需要为你的链接。 在最简单的情况下，你将提供不仅仅是链接文本和客户端上单击超链接时转到的操作方法。 例如，我们可以将链接到"应用商店 /"链接文本"转到存储索引"使用以下调用存储详细信息页面上的 index （） 方法：

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*注意： 在这种情况下，我们不需要指定控制器名称，因为我们只需链接到同一个呈现当前视图的控制器中的另一个操作。*

我们指向浏览页上的将需要传递参数，不过，因此我们将使用另一个重载采用三个参数的次 Html.ActionLink 方法：

- 1. 链接文本，将显示流派名称
- 2. 控制器操作名称 （浏览）
- 3. 路由参数值，指定的名称 （流派） 和值 （流派名称）

将放在一起，此处我们如何将写存储索引视图这些链接：

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

现在我们再次运行我们的项目并访问 /Store/ URL 时我们将看到风格的列表。 单击时，每个风格是一个超链接 – 它将给我们讲到我们/存储/浏览？ 流派 =*[流派]* URL。

![](mvc-music-store-part-3/_static/image3.jpg)

有关风格列表 HTML 类似如下所示：

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


>[!div class="step-by-step"]
[上一页](mvc-music-store-part-2.md)
[下一页](mvc-music-store-part-4.md)
