---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: "添加控制器 |Microsoft 文档"
author: shanselman
description: "如果本教程可在此处使用 Visual Studio 2013 更新的版本。 新的教程使用 ASP.NET MVC 5，基础上 t 提供了许多改进..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: 93a362cf83d39b29fcba3f2dee0c28257805a89e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller"></a>添加控制器
====================
通过[Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > 本教程中是否可用的更新的版本[此处](../../getting-started/introduction/getting-started.md)使用[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)。 新的教程使用 ASP.NET MVC 5，通过本教程提供了许多改进。
> 
> 
> 这是初学者本教程介绍 ASP.NET MVC 的基础知识。 将创建一个简单的 web 应用程序读取和写入数据库中。 请访问[ASP.NET MVC 学习中心](../../../index.md)若要查找其他 ASP.NET MVC 教程和示例。


MVC 代表模型、 视图、 控制器。 MVC 是模式，用于开发应用程序，以便每个部分是不同于其他责任。

- 模型： 你的应用程序的数据
- 视图： 你的应用程序将使用动态生成 HTML 响应的模板文件。
- 控制器： 处理应用程序，传入 URL 请求的类中检索模型数据，，然后指定呈现响应发回客户端的视图模板

我们将涵盖所有这些概念，在本教程中，向您展示如何使用它们来生成应用程序。

右键单击解决方案资源管理器中的 controllers 文件夹并选择添加控制器，让我们创建一个新的控制器。

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

将你新控制器"HelloWorldController"，然后单击添加。

[![添加控制器对话框](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

请注意，在右侧，已为您调用 HelloWorldController.cs 创建了一个新的文件现在在中打开该文件的解决方案资源管理器**IDE**。

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

创建在您的新公共类 HelloWorldController 内部如下所示的两个新方法。 作为示例，我们将直接从我们控制器返回 HTML 的字符串。

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

你的控制器名为 HelloWorldController 且新方法调用索引。 你再次运行应用程序，像以前一样 （单击播放按钮或按 f5 键以执行此操作）。 一旦你的浏览器已启动，更改到的地址栏中的路径`http://localhost:xx/HelloWorld`其中，xx 是任何数字你的计算机已选。 现在你的浏览器应类似下面的屏幕截图。 我们在上面我们方法返回到一种称为"内容"。 方法传递的字符串 系统只返回一些 HTML，并且它未通知我们 ！

ASP.NET MVC 调用不同的控制器类 （和其中的不同操作方法），具体取决于传入的 URL。 使用 ASP.NET MVC 的默认映射逻辑使用如下格式来控制运行哪些代码：

/ [控制器] / [ActionName] / [参数]

URL 的第一部分确定要执行的控制器类。 因此 /HelloWorld 将映射到 HelloWorldController 类。 URL 的第二部分确定要执行的类上的操作方法。 因此 /HelloWorld/Index 可能会导致要执行的 HelloWorldcontroller 类的 index （） 方法。 请注意，我们仅必须访问 /HelloWorld 上述和索引已隐式的方法。 这是因为名为"Index"的方法是如果有一个未显式指定调用在控制器的默认方法。

[![这是我的默认操作](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

现在，让我们访问`http://localhost:xx/HelloWorld/Welcome.`现在我们欢迎方法已执行并返回其 HTML 字符串。

同样，/ [控制器] / [ActionName] / [参数] 因此控制器是 HelloWorld，欢迎在这种情况下是方法。 我们尚未将参数执行操作。

[![这是欢迎操作方法](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

让我们，以便我们可以将中的一些信息从 URL 中传递给我们控制器，例如如下所示稍微修改我们的示例: / HelloWorld/欢迎？ 名称 = Scott&amp;numtimes = 4。 更改欢迎方法，以包括两个参数和与下面类似的更新。 请注意，我们使用了 C# 可选参数功能来指示是否它未传入参数 numTimes 应默认为 1。

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

运行你的应用程序并访问`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`根据需要更改名称和 numtimes 的值。 系统会自动映射到你的方法中的参数来自你的地址栏中的查询字符串的命名的参数。

在这些示例中这两个控制器已被执行所有工作，并具有已直接返回 HTML。 通常，我们不希望我们控制器直接-返回 HTML，因为将非常难以代码结束。 而是我们通常将使用单独的视图模板文件来帮助生成 HTML 响应。 让我们看一下如何我们可以执行此操作。 关闭你的浏览器并返回到 IDE。

>[!div class="step-by-step"]
[上一页](getting-started-with-mvc-part1.md)
[下一页](getting-started-with-mvc-part3.md)
