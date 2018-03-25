---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: "使用控制器和视图来实现的详细信息列表/UI |Microsoft 文档"
author: microsoft
description: "步骤 4 演示了如何将控制器添加到的应用程序利用我们的模型来为用户提供数据的详细信息列表/导航体验..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 2f9148a2d419863229e2c5a2a0c98984001fcee5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>使用控制器和视图来实现详细信息列表/用户界面
====================
通过[Microsoft](https://github.com/microsoft)

[下载 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 这是一个免费的步骤 4 ["NerdDinner"应用程序教程](introducing-the-nerddinner-tutorial.md)，查找步程取如何生成一个较小，但完成，使用 ASP.NET MVC 1 web 应用程序。
> 
> 步骤 4 演示了如何充分利用我们的模型，以便用户提供数据的详细信息列表/导航的体验晚餐我们 NerdDinner 站点上的应用中添加一个控制器。
> 
> 如果你使用的 ASP.NET MVC 3，我们建议你遵循[获取启动使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC 音乐商店](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教程。


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner 步骤 4： 控制器和视图

与传统 web 框架 （经典 ASP、 PHP、 ASP.NET Web 窗体，等等），传入 Url 通常会映射到磁盘上的文件中。 例如： 如 URL 的请求"/ Products.aspx"或"/ Products.php"的"Products.aspx"或"Products.php"文件可能处理。

基于 web 的 MVC 框架将 Url 映射到服务器代码，以略有不同的方式。 而不是将传入 Url 映射到文件，它们改为映射到类的方法的 Url。 这些类称为"控制器"并负责处理传入的 HTTP 请求，处理用户输入，检索和保存数据，并确定要发送的响应返回给客户端 （显示 HTML，下载的文件，将重定向到不同URL，等等）。

现在，我们已为我们 NerdDinner 应用程序生成一个基本的模型，我们的下一步是将控制器添加到的应用程序充分利用它来为用户提供晚餐我们站点上提供数据的详细信息列表/导航体验。

### <a name="adding-a-dinnerscontroller-controller"></a>添加 DinnersController 控制器

我们将开始通过我们的 web 项目中的"控制器"文件夹中右键单击，然后选择**添加-&gt;控制器**菜单命令 （你还可以执行此命令通过键入 Ctrl M、 Ctrl + C）：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

此时将显示"添加控制器"对话框：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

我们会将新控制器"DinnersController"，然后单击"添加"按钮。 然后，visual Studio 将添加我们 \Controllers 目录下的 DinnersController.cs 文件：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

它还将打开代码编辑器中的新 DinnersController 类。

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>将 index （） 和 Details() 操作方法添加到 DinnersController 类

我们想要启用访客使用我们的应用程序以浏览即将到来晚餐的列表，使他们可以单击列表中的任何 Dinner，若要查看有关它的特定详细信息。 我们将通过发布从我们的应用程序的以下 Url 执行此操作：

| **URL** | **目的** |
| --- | --- |
| */Dinners/* | 显示即将到来的晚餐 HTML 列表 |
| */Dinners/详细信息 / [id]* | 显示有关由嵌入中的 URL – 将匹配的数据库中 dinner DinnerID"id"参数指示特定 dinner 详细信息。 例如： /Dinners/Details/2 将显示有关 Dinner DinnerID 值为 2 的详细信息的 HTML 页。 |

通过将两个公共"操作方法"添加到我们 DinnersController 类，如下面，我们将发布这些 Url 的初始的实现：

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

我们然后运行 NerdDinner 应用程序，使用我们的浏览器调用它们。 在中键入*"/ 晚餐 /"* URL 将导致我们*index （)*方法运行，然后它将发送回以下响应：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

在中键入*"/ 2-晚餐/详细信息"* URL 将导致我们*Details()*方法运行，并将发送回以下响应：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

你可能想知道-如何 ASP.NET MVC 知道创建我们 DinnersController 类和调用这些方法？ 若要了解，让我们快速了解路由的工作方式。

### <a name="understanding-aspnet-mvc-routing"></a>了解 ASP.NET MVC 路由

ASP.NET MVC 包括一个功能强大的 URL 路由引擎，提供了大量的灵活地控制如何将 Url 映射到控制器类。 使我们可以完全自定义 ASP.NET MVC 如何选择要创建哪种方法来调用它，以及配置不同的方式，变量可以是自动分析从 URL/查询字符串并传递给该方法作为参数哪些控制器类自变量。 它提供了灵活地完全针对 SEO （搜索引擎优化） 进行优化站点以及发布任何希望获得从应用程序的 URL 结构。

默认情况下，新的 ASP.NET MVC 项目附带了已注册的 URL 路由规则的预配置集。 这使我们能够轻松地开始对应用程序而无需显式配置的任何内容。 在我们的项目 – 我们可以通过双击"Global.asax"文件，我们的项目的根目录中打开"应用程序"类中找不到默认路由规则注册：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

此类的"RegisterRoutes"方法中注册的默认 ASP.NET MVC 路由规则：

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

"路由。MapRoute()"方法调用上述注册映射到控制器类使用的 URL 格式的传入 Url 的默认路由规则:"/ {controller} / {action} / {id}"– 其中"控制器，"是要实例化，控制器类的名称"action"是的名称公共方法来调用它，然后"id"是可以作为自变量传递给方法的 URL 中嵌入一个可选参数。 传递给"MapRoute()"方法调用的第三个参数是一套要用于控制器/操作/id 值的事件中不存在在 URL 中的默认值 (控制器 ="主页"，操作 ="Index"，Id ="")。

下面是演示如何各种 Url 的表映射使用默认值"*/ {控制器} / {action} / {id}"*路由规则：

| **URL** | **控制器类** | **操作方法** | **传递参数** |
| --- | --- | --- | --- |
| */ 晚餐/详细信息/2* | DinnersController | Details(id) | id = 2 |
| */ 晚餐/编辑/5* | DinnersController | Edit(id) | id = 5 |
| */ 创建晚餐 /* | DinnersController | Create （) | 不可用 |
| */ 晚餐* | DinnersController | Index （) | 不可用 |
| */ 主页* | HomeController | Index （) | 不可用 |
| */* | HomeController | Index （) | 不可用 |

最后三行显示的默认值 (控制器 = 主页，操作 = 索引，Id ="") 正在使用。 "索引"方法注册为默认操作名称，如果未指定，因为"/ 晚餐"和"/home"Url 原因 index （） 操作方法要对其控制器类调用。 因为如果未指定，将为默认控制器注册"主页"控制器，"/"URL 使 HomeController 要创建和 index （） 操作方法来调用它。

如果你不喜欢这些默认 URL 路由规则，好消息是它们可轻松地更改-只需在上面的 RegisterRoutes 方法中对其进行编辑。 我们 NerdDinner 为应用程序，不过，我们不会更改任何默认 URL 路由规则 – 我们将只需使用它们作为-是。

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>使用从我们 DinnersController DinnerRepository

让我们现在将 DinnersController 我们当前实现与使用我们的模型的实现的 index （） 和 Details() 操作方法。

我们将使用我们之前生成要实现该行为的 DinnerRepository 类。 我们首先，将添加引用"NerdDinner.Models"命名空间的"using"语句，然后在我们 DinnerController 类中声明为字段我们 DinnerRepository 的实例。

本章后面我们引入的"依赖关系注入"的概念，现在我们只需将创建我们 DinnerRepository 的实例显示另一种方法，若要获取对 DinnerRepository，更好的单元的引用我们控制器测试 – 但为权限如下面的内联。

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

现在我们已准备好生成返回使用我们检索到的数据模型对象的 HTML 响应。

### <a name="using-views-with-our-controller"></a>向我们控制器使用视图

可以用来编写组建 HTML，然后使用我们操作方法中的代码时*Response.Write()* helper 方法，以将其发送回客户端，该方法只有会变得相当快速。 较好的做法是为我们只对执行应用程序和数据逻辑内我们 DinnersController 的操作方法，而以将呈现到单独"视图"模板，它负责输出 HTML 形式呈现的 HTML 响应所需的数据它。 正如我们将在稍后看到的"查看"模板将是通常包含的 HTML 标记和嵌入的呈现代码组合的文本文件。

在我们视图呈现分隔我们控制器逻辑带来了几大好处。 具体而言，它有助于之间的应用程序代码和 UI 格式呈现代码强制"明显的问题"。 这很容易得多到单元测试中隔离的应用程序逻辑与 UI 呈现逻辑。 它便于以后修改 UI 呈现模板，而无需更改应用程序代码。 并且它可以使开发人员和设计器以协作项目上更容易。

我们可以更新我们 DinnersController 类，以指示我们想要查看模板用于通过将我们的两个操作方法的方法签名从具有返回类型为"void"改为具有返回类型为"ActionResult"更改发送回 HTML UI 响应。 然后，我们可以调用*View()*控制器基本类返回的帮助器方法"ViewResult"对象与下面类似：

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

签名*View()*下面，我们将使用上面的帮助器方法如下所示：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

第一个参数*View()*帮助器方法是我们想要用来呈现的 HTML 响应的视图模板文件的名称。 第二个参数是一个包含视图模板呈现的 HTML 响应所需的数据的模型对象。

我们正在呼叫我们 index （） 操作方法内*View()*帮助器方法并指示我们想要呈现晚餐使用"索引"视图模板的 HTML 列表。 我们要传递的视图模板以生成从列表的 Dinner 对象的序列：

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

在我们 Details() 操作方法中，我们尝试检索 Dinner 对象使用的 URL 中提供的 id。 如果我们调用找到有效 Dinner *View()* ，该值指示我们想要使用的"详细信息"视图模板来呈现检索到的 Dinner 对象的帮助器方法。 如果请求无效 dinner，则我们呈现一有用的错误消息，指示 Dinner 不存在使用"NotFound"视图模板 (和重载的版本*View()*帮助器方法，只需模板名称):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

让我们现在实现"NotFound"、"详细信息"和"索引"视图模板。

### <a name="implementing-the-notfound-view-template"></a>实现"NotFound"视图模板

我们将首先通过实现"NotFound"视图模板 – 其显示的友好错误消息，指出找不到请求的 dinner。

我们将通过在控制器操作方法中，我们文本光标定位来创建新视图模板，然后右键单击并选择"添加视图"菜单命令 （我们还可以执行此命令通过键入 Ctrl M、 Ctrl + V）：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

此时会弹出一个类似下面的"添加视图"对话框。 默认情况下，对话框将预填充要创建的视图名称的操作方法的名称相匹配光标时对话框已启动 （在本例中"的详细信息"）。 我们想要首先实现"NotFound"模板，因为我们将重写此视图名称，并将其设置改为"NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

当我们单击"添加"按钮时，Visual Studio 将创建一个新"NotFound.aspx"视图模板为我们内的"\Views\Dinners"目录 （它还将创建如果目录尚不存在）：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

它还将打开代码编辑器内我们新"NotFound.aspx"视图模板：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

默认情况下的视图模板具有两个"内容区域"我们可以在其中添加内容和代码。 第一种允许我们自定义的"标题"HTML 页发回。 第二个使得我们可以自定义"主要内容"的 HTML 页发回。

若要实现我们"NotFound"视图模板我们将添加一些基本的内容：

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

我们可以然后试试看以及浏览器中。 若要让我们执行此请求*"/ 晚餐/详细信息/9999"* URL。 这将参考 dinner，当前不在数据库中，存在且不会导致呈现我们"NotFound"视图模板我们 DinnersController.Details() 操作方法：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

你会注意到在上面的屏幕截图中的一件事情是我们基本视图的模板已继承了大量 HTML 环绕在屏幕上的主要内容。 这是因为我们的视图模板中使用"母版页"模板，使我们能够在站点上的所有视图上都应用一致的布局。 我们将讨论母版页详细在本教程稍后部分中的工作。

### <a name="implementing-the-details-view-template"></a>实现"详细信息"视图模板

让我们现在可实现的"详细信息"视图模板 – 将为单个 Dinner 模型生成 HTML。

我们将执行此操作通过定位在详细信息操作方法中，我们文本光标，然后右键单击并选择"添加视图"菜单命令 （或按 Ctrl M、 Ctrl + V）：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

这将显示"添加视图"对话框。 我们将保留默认的视图名称 （"详细信息"）。 我们还在对话框中选择"创建强类型视图"复选框，选择 （使用 combobox 下拉列表中） 我们通过从控制器向视图模型类型的名称。 此视图中，我们要传递 Dinner 对象 (这种类型的完全限定的名称是:"NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

与上一模板中，我们选择在其中创建"空视图"，的此时我们将自动选择"搭建基架"视图使用"详细信息"的模板。 我们可以通过更改"查看内容"下拉列表中上述对话框指示这一点。

"基架"将生成初始我们基于我们将传递给它的 Dinner 对象的详细信息视图模板的实现。 这可以轻松地为我们要快速开始我们视图模板实现。

当我们单击"添加"按钮时，Visual Studio 将创建一个新"Details.aspx"视图模板文件为我们在我们"\Views\Dinners"目录中：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

它还将打开代码编辑器内我们新"Details.aspx"查看模板。 它将包含基于 Dinner 模型的详细信息视图的初始基架实现。 基架引擎使用.NET 反射来看，传递的类上公开的公共属性，并将添加合适基于它找到每种类型的内容：

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

我们可以请求*"/ 1/晚餐/详细信息"* URL 以查看此"详细信息"基架实现在浏览器的外观。 使用此 URL 将显示我们手动添加到我们的数据库我们首次创建时晚餐之一：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

这将获取我们快速启动并运行，并为我们提供了我们 Details.aspx 视图的初始实现。 然后，我们可以转并调整其自定义用户界面，以我们的满意度。

当我们更紧密地看一下 Details.aspx 模板时，我们将找到，它包含静态 HTML 以及嵌入呈现代码。 &lt;%%&gt;代码问题执行代码，当查看模板呈现时，和&lt;%= %&gt;代码问题执行其中包含的代码，并随后呈现将结果发送到模板的输出流。

在此处访问从控制器使用强类型的"模型"属性已传递的"Dinner"模型对象的视图内，我们可以编写代码。 Visual Studio 为我们提供了完整的代码 intellisense 时访问此对编辑器内的"模型"属性：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

让我们进行一些优化，以便我们最终的详细信息视图模板的源类似于下面：

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

当我们访问*"/ 1/晚餐/详细信息"* URL 再次它将立即呈现与下面类似：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>实现"索引"视图模板

让我们现在实现"索引"视图模板-这会导致生成即将到来的晚餐的列表。 待办事项这我们在索引操作方法中，我们文本光标的位置，然后右键单击并选择"添加视图"菜单命令 （或按 Ctrl M、 Ctrl + V）。

在"添加视图"对话框中，我们将保持视图模板名为"Index"，然后选择"创建强类型视图"复选框。 此时，我们将选择自动生成"列表"视图模板，并选择"NerdDinner.Models.Dinner"作为模型类型传递给视图 (其中因为我们已表明我们正在创建"列表"基架将导致假定我们添加视图对话框传递 Dinner 对象的序列从我们控制器于视图）：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

当我们单击"添加"按钮时，Visual Studio 将创建新"Index.aspx"视图模板文件为我们在我们"\Views\Dinners"目录中。 它将"搭建基架"初始中的实现，提供我们将传递给视图晚餐 HTML 表列表。

当我们运行的应用程序和访问*"/ 晚餐 /"*它会呈现晚餐我们列表的 URL 如下所示：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

上述表解决方案为我们提供了我们的 Dinner 数据 – 这并不是非常我们想要的面向 Dinner 列表我们使用者的类似网格布局。 我们可以更新 Index.aspx 视图模板，修改它列出较少的列的数据，并使用&lt;ul&gt;元素来呈现它们而不是使用下面的代码的表：

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

因为我们循环访问每个 dinner 在我们的模型中，我们将使用在上面的 foreach 语句中的"var"关键字。 熟悉 C# 3.0 的那些可能认为，使用"var"意味着 dinner 对象是后期绑定。 而是表示编译器要使用针对的强类型的"模型"属性的类型推理 (它属于类型"IEnumerable&lt;Dinner&gt;") 和编译为 Dinner 类型 – 这意味着我们获取完整的本地"dinner"变量intellisense 和编译时在代码块内检查：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

当我们命中一次刷新*/Dinners*在我们更新的视图现在看起来像下面我们浏览器中的 URL:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

这查找更好，但尚未完全存在。 我们最后一步是启用最终用户单击列表中的单个晚餐和查看有关它们的详细信息。 我们将呈现的 HTML 超链接元素的链接到我们 DinnersController 上的详细信息操作方法进行实现。

我们可以在两种方式之一我们索引视图中生成这些超链接。 第一种是手动创建 HTML &lt;&gt;元素如下所示，我们将嵌入&lt;%%&gt;在内受阻&lt;&gt; HTML 元素：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

我们可以使用另一种方法是利用内置的"Html.ActionLink()"帮助程序方法内支持以编程方式创建 HTML 的 ASP.NET MVC &lt;&gt;链接到另一种操作方法的元素控制器：

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Html.ActionLink() 帮助器方法的第一个参数是要显示的链接文本 （在此情况下标题的 dinner），第二个参数是我们想要生成该链接可以 （在本例中的详细信息方法） 的控制器操作名称，第三个参数是要发送到 （实现为具有属性名称/值的匿名类型） 的操作的参数集。 在这种情况下，我们要指定的 dinner 我们想要链接到，并且由于在 ASP.NET MVC 中默认 URL 路由规则的"id"参数"{Controller} / {Action} / {id}"的 Html.ActionLink() 帮助器方法将生成以下输出：

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

我们 Index.aspx 视图中，我们将使用 Html.ActionLink() 帮助器方法方法，并具有列表链接到相应的详细信息的 URL 中的每个 dinner:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

现在我们命中条件和*/Dinners*我们 dinner 列表类似于下面的 URL:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

当我们单击任一列表中晚餐时我们将导航以查看有关它的详细信息：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>基于约定的命名和 \Views 目录结构

默认情况下的 ASP.NET MVC 应用程序使用基于约定的目录时解决查看模板命名结构。 这样，开发人员为了避免必须完全限定的位置路径引用从控制器类中的视图时。 默认情况下 ASP.NET MVC 将查找该视图模板文件内 * \Views\[ControllerName]\*目录下的应用程序。

例如，我们一直在 DinnersController 类 – 显式地引用三个视图模板:"索引"、"详细信息"和"NotFound"。 ASP.NET MVC 将默认情况下会查找中的这些视图*\Views\Dinners*目录下我们的应用程序根目录下：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

请注意如何在该处的上方是当前项目中的三个控制器类 (DinnersController、 HomeController 和 AccountController – 我们创建项目时，最后两个已添加默认情况下)，并且有三个子目录 （一个用于每个控制器） \Views 目录中。

从主页和帐户控制器中引用的视图将自动解决了相应的其视图模板*\Views\Home*和*\Views\Account*目录。 *\Views\Shared*子目录提供一种方法都是重复使用应用程序中的多个控制器的视图模板存储。 当 ASP.NET MVC 尝试解析查看模板时，它将首先检查内*\Views\[控制器]*特定目录中，如果它找不到视图模板存在它将查找内*\Views\共享*目录。

命名单独的视图模板时，推荐的指南，是能够共享相同的名称与操作方法导致它呈现的视图模板。 例如，高于我们"索引"操作方法使用"索引"视图呈现视图结果，而"详细信息"操作方法使用"详细信息"视图中呈现其结果。 这很容易地快速查看哪些模板是与每个操作相关联。

开发人员不需要显式指定的视图模板名称，当视图模板具有与从中调用在控制器上的操作方法相同的名称。 我们可以改为仅的模型对象传递给"View()"帮助器方法 （如果没有指定的视图名称），和 ASP.NET MVC 将自动推断我们想要使用*\Views\[ControllerName]\[ActionName]*磁盘来呈现不上的查看模板。

这使得我们可以进行稍微清理我们控制器代码并避免重复的名称在我们的代码中两次：

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

上面的代码是实现良好 Dinner 列表/详细信息所需的所有站点体验。

#### <a name="next-step"></a>下一步

我们现在有浏览体验生成 nice Dinner。

让我们现在启用编辑支持 CRUD （创建、 读取、 更新、 删除） 数据窗体。

>[!div class="step-by-step"]
[上一页](build-a-model-with-business-rule-validations.md)
[下一页](provide-crud-create-read-update-delete-data-form-entry-support.md)
