---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: "第 2 部分： 控制器 |Microsoft 文档"
author: jongalloway
description: "本系列教程详细介绍所有生成 ASP.NET MVC 音乐商店示例应用程序所采取的步骤。 第 2 部分介绍控制器。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: bdafd751e996e759d516d0fa25b09eff21241ed7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="part-2-controllers"></a>第 2 部分： 控制器
====================
通过[Jon Galloway](https://github.com/jongalloway)

> MVC 音乐商店是，从而引入并说明如何使用 ASP.NET MVC 和 Visual Studio 进行 web 开发分步教程应用程序。  
>   
> MVC 音乐商店是一个轻型示例存储区实现，该类销售音乐集联机，并实现基本的站点管理、 用户登录，和购物车功能。  
>   
> 本系列教程详细介绍所有生成 ASP.NET MVC 音乐商店示例应用程序所采取的步骤。 第 2 部分介绍控制器。


与传统 web 框架传入 Url 通常会映射到磁盘上的文件中。 例如： 如 URL 的请求"/ Products.aspx"或"/ Products.php"的"Products.aspx"或"Products.php"文件可能处理。

基于 web 的 MVC 框架将 Url 映射到服务器代码，以略有不同的方式。 而不是将传入 Url 映射到文件，它们改为映射到类的方法的 Url。 这些类称为"控制器"并负责处理传入的 HTTP 请求，处理用户输入，检索和保存数据，并确定要发送的响应返回给客户端 （显示 HTML，下载的文件，将重定向到不同URL、 等）。

## <a name="adding-a-homecontroller"></a>添加 HomeController

通过添加了控制器类将处理到我们的站点的主页的 Url，我们将首先我们 MVC 音乐商店的应用程序。 我们将遵循的 ASP.NET MVC 的默认命名约定，并调用它 HomeController。

右键单击解决方案资源管理器中的"控制器"文件夹中，然后选择"添加"，然后"控制器..." 命令：

![](mvc-music-store-part-2/_static/image1.jpg)

这将显示"添加控制器"对话框。 将命名控制器"HomeController"并按添加按钮。

![](mvc-music-store-part-2/_static/image1.png)

这将创建新文件中，命名为 HomeController.cs，替换为以下代码：

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

若要启动尽可能简单地，让我们使用简单方法只返回一个字符串替换 Index 方法。 我们将进行两个更改：

- 更改方法以返回而不是 ActionResult 字符串
- 更改返回语句，以返回"Hello 从主页"

该方法现在应如下所示：

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>运行应用程序

现在让我们运行该站点。 我们可以启动我们的 web 服务器，然后尝试登录网站，使用以下任一::

- 选择调试 ⇨ 启动调试菜单项
- 单击工具栏中的绿色箭头按钮![](mvc-music-store-part-2/_static/image2.jpg)
- 使用键盘快捷方式，F5。

使用任何上述步骤将编译我们项目中，并导致是内置于 Visual Web Developer 启动 ASP.NET 开发服务器。 通知会在以指示 ASP.NET 开发服务器已启动，屏幕的右下角中，将显示的端口号下运行。

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer 将自动打开浏览器窗口，其 URL 指向我们的 web 服务器。 这将使我们可以快速试用我们的 web 应用程序：

![](mvc-music-store-part-2/_static/image3.png)

那么，这是相当快速 – 我们创建的新网站，添加三个行函数，而且我们在浏览器中有文本。 不特别复杂，但它是一个开始。

*注意： Visual Web Developer 包括 ASP.NET 开发服务器，这将在一个随机可用"端口"号上运行你的网站。在上面的屏幕截图，该站点运行在`http://localhost:26641/`，因此它正在使用端口 26641。端口号将有所不同。当我们谈 URL 的 like /Store/Browse 在本教程中时，后者将更后的端口号。假设 26641 端口号，浏览到/存储/浏览将意味着浏览到`http://localhost:26641/Store/Browse`。*

## <a name="adding-a-storecontroller"></a>添加 StoreController

我们添加简单 HomeController 实现我们站点的主页。 让我们现在添加我们将使用来实现我们音乐商店浏览功能的另一个控制器。 存储控制器将支持三种方案：

- 在我们音乐商店音乐风格列表页面
- 列出所有在特定流派音乐集浏览页
- 显示有关特定音乐唱片集的信息的详细信息页

我们首先添加一个新的 StoreController 类... 如果你尚未这样做，则停止正在运行的应用程序或者通过关闭浏览器选择调试 ⇨ 停止调试菜单项。

现在，添加新 StoreController。 就像我们未与 HomeController，我们需要执行此操作通过右键单击解决方案资源管理器中的"控制器"文件夹，然后选择添加-&gt;控制器菜单项

![](mvc-music-store-part-2/_static/image4.png)

我们新 StoreController 已具有"Index"的方法。 我们将使用此"索引"方法来实现我们列表页，其中列出我们音乐商店中的所有风格。 我们还将添加两个其他的方法来实现两种情况下我们想要以处理我们 StoreController： 浏览和详细信息。

在我们控制器内的这些方法 （索引、 浏览和详细信息） 称为"控制器操作"，而你已经了解与 HomeController.Index （） 操作方法，如其作业是响应的 URL 请求并 （通常来说） 确定哪些内容应发送回浏览器或调用 URL 的用户。

我们将首先我们 StoreController 实现更改 theIndex() 方法以返回字符串"Hello 从 Store.Index()"和 browse （） 和 Details()，我们将添加类似的方法：

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

再次运行项目，并浏览以下 Url:

- / Store
- / 应用商店/浏览
- / 存储/详细信息

访问以下 Url 将调用我们控制器中的操作方法，并返回字符串的响应：

![](mvc-music-store-part-2/_static/image5.png)

这很不错，但它们是只是常量字符串。 让我们使这些动态的以便它们采用中 URL 的信息并将其显示在页面输出。

首先我们将更改浏览操作方法，以便从 URL 检索查询字符串值。 我们可以将"genre"参数添加到我们的操作方法来执行此操作。 在执行此操作时 ASP.NET MVC 会自动传入任何查询字符串或窗体发布参数名为"genre"到我们的操作方法中，当调用它。

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*注意： 我们将使用 HttpUtility.HtmlEncode 实用工具方法，整理用户输入。这可以防止用户将 Javascript 注入到我们的视图，如 /Store/Browse 的链接？流派 =&lt;脚本&gt;window.location= http://hackersite.com&lt;/&gt;。*

现在让我们浏览到/存储/浏览？流派 = Disco

![](mvc-music-store-part-2/_static/image6.png)

让我们下一次更改的详细信息操作读取，并显示输入的参数名为 id。 与不同的是我们前面的方法，我们不会为嵌入的 ID 值作为查询字符串参数。 而是我们将嵌入它直接在 URL 本身内。 例如： /Store/Details/5。

ASP.NET MVC 允许我们轻松执行此操作而无需进行任何配置。 ASP.NET MVC 的默认路由约定是在操作方法名称后面的 url 段视为一个名为"ID"参数。 如果您操作的方法有一个名为 ID 参数然后 ASP.NET MVC 将自动传递的 URL 段向你作为参数。

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

运行应用程序，并浏览到 /Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

让我们回顾一下我们到目前为止已完成：

- 我们已在 Visual Web Developer 中创建新的 ASP.NET MVC 项目
- 我们已讨论了 ASP.NET MVC 应用程序的基本文件夹结构
- 我们已经学习了如何运行我们的网站使用 ASP.NET 开发服务器
- 我们已创建了两个控制器类： HomeController 和 StoreController
- 我们已向我们控制器的响应的 URL 请求并返回到浏览器的文本添加操作方法


>[!div class="step-by-step"]
[上一页](mvc-music-store-part-1.md)
[下一页](mvc-music-store-part-3.md)
