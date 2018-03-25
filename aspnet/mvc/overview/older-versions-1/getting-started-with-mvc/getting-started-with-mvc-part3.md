---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: "添加视图 |Microsoft 文档"
author: shanselman
description: "这是初学者本教程介绍 ASP.NET MVC 的基础知识。 创建一个简单的 web 应用程序读取和写入数据库中。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 8725d054861c857ceac10a42b0cc3f2afe056aea
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="adding-a-view"></a>添加视图
====================
通过[Scott Hanselman](https://github.com/shanselman)

> 这是初学者本教程介绍 ASP.NET MVC 的基础知识。 将创建一个简单的 web 应用程序读取和写入数据库中。 请访问[ASP.NET MVC 学习中心](../../../index.md)若要查找其他 ASP.NET MVC 教程和示例。


在本部分中我们将查看如何我们可以使用视图模板文件来完全封装回客户端生成的 HTML 响应我们 HelloWorldController 类。

让我们开始通过查看模板使用我们 Index 方法。 我们的方法中调用索引，它位于 HelloWorldController。 目前我们 index （） 方法返回使用的是控制器类中的硬编码的消息的字符串。

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

让我们现在更改索引方法，以便改用如下所示：

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

让我们现在添加到我们的项目，我们可以使用我们的 index （） 方法中的视图模板。 若要执行此操作，用某个位置的中间索引方法鼠标右键单击，然后单击添加视图...

![图像](getting-started-with-mvc-part3/_static/image1.png)

这将显示为我们提供一些用于我们想要创建可由我们的索引方法中的视图模板选项的"添加视图"对话框。 现在，不更改任何内容，并只需单击添加按钮。

[![添加视图对话框](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

单击添加后，新的文件夹和一个新文件将显示在解决方案文件夹中，如下所示。 我现在有在该文件夹的视图和 Index.aspx 文件下的一个 HelloWorld 文件夹。

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

新索引文件也是已打开的并且可供编辑。 添加一些文本下第一个&lt;h2&gt;索引&lt;/h2&gt;喜欢"Hello World"。

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

运行你的应用程序并访问[ `http://localhost:xx/HelloWorld` ](http://localhostxx)再次在浏览器中。 在此示例中我们控制器中的索引方法不执行任何工作，但未调用"返回 View()"，它表示我们想要使用的视图模板文件来呈现响应发回客户端。 因为我们未显式指定要使用的视图模板文件的名称，ASP.NET MVC 将默认为 \Views\HelloWorld 文件夹中使用 Index.aspx 视图文件中。 现在我们可以看到我们硬编码在我们的视图中的字符串。

[![索引的 Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

看起来一切都非常好。 但是，请注意，浏览器的标题显示"Index"，并在页上的大标题显示"My MVC Application。" 让我们更改那些。

### <a name="changing-views-and-master-pages"></a>更改视图和母版页

首先，让我们更改文本"My MVC Application。" 该文本共享，并显示在每一页。 它实际出现在只有一个位置在代码中，即使它位于我们的应用程序的每一页上。 转到在解决方案资源管理器的 /Views/Shared 文件夹并打开 Site.Master 文件。 此文件称为母版页，它是共享的"shell"我们其他所有页使用。

请注意此文件中，某些文本表明 ContentPlaceholder"主内容"。

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

该占位符为其中你创建的所有页将都显示，"包装"在母版页中。 尝试更改的标题，然后运行您的应用程序并访问多个页。 你会注意到，你一次更改出现在多个页上。

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

现在每一页将具有主标题-即 h 1-"我的 MVC 影片应用程序。" 用于处理在那里共享跨所有的页面顶部的白色文本。

下面是 Site.Master 我们已更改的标题与整个：

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

现在，让我们更改索引页的标题。

打开 /HelloWorld/Index.aspx。 没有两个位置更改。 首先，标题显示的浏览器中，然后辅助标头的也是 H2-标题中。 我将它们分别略有不同，以便你可以查看代码的哪些位更改应用程序的哪个部分。

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

运行你的应用并访问 /Movies。 请注意，已更改浏览器标题、 主标题和副标题。 很容易在使用很小的更改对应用程序中进行大更改，向你的视图。

[![影片列表-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

我们一小段 （在此情况下"Hello World ！"的"数据" 消息） 很难通过编码。 我们已有 V (Views)，并且我们具有 C （控制器），但尚没有的 M （模型）。 我们很快将演练如何创建数据库并从它检索模型数据。

## <a name="passing-a-viewmodel"></a>传递 ViewModel

我们转到数据库，并讨论模型之前，不过，让我们先介绍一下"Viewmodel。" 这些是表示视图模板所需呈现 HTML 响应回客户端的对象。 它们通常创建和由了控制器类传递给视图模板，并仅应包含视图模板所需的数据而没有其他。

以前与我们的 HelloWorld 示例，我们 Welcome() 操作方法采用一个名称和 numTimes 参数，并输出到浏览器。 让我们而不是必须继续直接呈现此响应的控制器，而是使一个小型类来存放该数据，然后将它传递到视图模板返回呈现 HTML 响应使用它。 这种方式控制器关心一件事和查看模板的另一个 – 使我们能够维护完全"分离关注点"我们的应用程序中。

返回到 HelloWorldController.cs 文件并添加一个新的"WelcomeViewModel"类并更改控制器内部的欢迎方法。 下面是完整 HelloWorldController.cs 与相同的文件中的新类。

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

即使它是在多行上，我们欢迎方法实际上是仅有两个代码语句。 第一个语句打包我们两个参数转换的 ViewModel 对象和第二个传递拖放到视图生成的对象。

现在，我们需要一个欢迎视图模板 ！ 欢迎使用的方法中右键单击并选择添加视图。 此时，我们将检查"创建强类型视图"，并且从下拉列表中选择我们 WelcomeViewModel 类。 此新视图将只知道 WelcomeViewModels 和任何其他类型的对象。

> *注意： 你将需要将已编译的添加有关你 WelcomeViewModel 才会显示在下拉列表中后，一次。*


下面是你添加视图的对话框应如下所示。 单击添加按钮。 ![添加视图带圆圈](getting-started-with-mvc-part3/_static/image10.png)

将在下，此代码添加&lt;h2&gt;新 Welcome.aspx 中。 我们将进行循环，并让用户说出我们应多次说 Hello ！

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

另请注意时你输入这些内容因为我们告知这有关 WelcomeViewModel 视图 （已婚，而记住？），我们可以获得有用的 Intellisense 每次我们引用我们模型对象作为下面的屏幕截图所示：

[![NumTime 源代码](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

运行你的应用程序并访问`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`试。 现在我们会从该 URL 采用数据，它自动传递到控制器，我们控制器打包数据插入视图模型，并将该对象拖放到我们视图传递。 不是向用户显示以 html 格式的数据视图。

[![欢迎-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

嗯，这是一种类型的"M"的模型，但不是数据库类型。 让我们我们已了解和创建数据库的影片。

>[!div class="step-by-step"]
[上一页](getting-started-with-mvc-part2.md)
[下一页](getting-started-with-mvc-part4.md)
