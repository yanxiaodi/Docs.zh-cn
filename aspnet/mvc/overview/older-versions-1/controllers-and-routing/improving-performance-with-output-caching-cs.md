---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: "使用提高性能输出缓存的 (C#) |Microsoft 文档"
author: microsoft
description: "在本教程中，你学习如何极大地提高你的 ASP.NET MVC web 应用程序的性能通过利用的输出缓存。 你..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: 47f0aa976c5876991ccc2406fb8f7402e59ec556
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="improving-performance-with-output-caching-c"></a>使用输出缓存 (C#) 提高性能
====================
通过[Microsoft](https://github.com/microsoft)

> 在本教程中，你学习如何极大地提高你的 ASP.NET MVC web 应用程序的性能通过利用的输出缓存。 了解如何缓存，以便不需要创建新的用户调用该操作的每个时间相同的内容的控制器操作返回的结果。


本教程旨在说明如何你可以显著地提高性能的 ASP.NET MVC 应用程序利用输出缓存。 输出缓存可用于缓存的控制器操作返回的内容。 这样，相同的内容不需要生成每个调用的相同的控制器操作的时间。

例如，假设你的 ASP.NET MVC 应用程序名为索引视图中显示的数据库记录列表。 通常情况下，每个用户时，将调用返回的索引视图中，控制器操作的时间的数据库记录集必须是从数据库中检索通过执行数据库查询。

如果你充分利用输出缓存的手动，则可以避免执行数据库查询，每次任何用户调用相同的控制器操作。 可从缓存而不是正在重新生成的控制器操作检索视图。 你可以避免执行冗余缓存启用适用于 server。

## <a name="enabling-output-caching"></a>启用输出缓存

你启用输出缓存通过将 [OutputCache] 属性添加到各个控制器操作或整个控制器类。 例如，列表 1 中的控制器公开名为 index （） 的操作。 Index （） 操作的输出被缓存 10 秒。

**列表 1 – Controllers\HomeController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

在 Beta 版中的 ASP.NET MVC，输出缓存并不适用于的 URL，如[http://www.MySite.com/](http://www.mysite.com/)。 相反，您必须输入的 URL，如[http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index)。 

列出 1 中为 10 秒时，将缓存 index （） 操作的输出。 如果你愿意，你可以指定一个更长的时间的缓存持续时间。 例如，如果你想要缓存的输出为一天的控制器操作然后你可以指定缓存持续时间为 86400 秒 (60 秒\*60 分钟\*24 小时)。

没有该内容不能保证将缓存按你指定的时间量。 当内存资源变得低时，缓存将自动启动退出的内容。

列表 1 中的主页控制器返回列出 2 中的索引视图。 无需进行任何特殊有关此视图。 索引视图只显示的当前时间 （请参见图 1）。

**列出 2 – Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

**图 1 – 缓存索引视图**

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

如果多次通过在你的浏览器的地址栏中输入 URL /Home/索引调用 index （） 操作和重复达到你的浏览器中的刷新/重新加载按钮，然后通过索引视图显示的时间不会更改为 10 秒。 将显示在同一时间，因为视图缓存。

请务必了解为每个用户访问你的应用程序会缓存该视图的视图相同。 将调用该 index （） 操作的任何人将获得索引视图相同的缓存的的版本。 这意味着大幅减少的 web 服务器提供的索引视图时必须执行的工作量。

列出 2 中的视图碰巧执行某些非常简单。 该视图仅显示当前时间。 但是，你可以就像轻松缓存此视图显示一组数据库记录。 在这种情况下，不需要的数据库记录集从数据库中每次调用返回的视图控制器操作检索。 Caching 可以降低你的 web 服务器和数据库服务器必须执行的工作量。

不使用页&lt;%@ OutputCache %&gt;指令中的 MVC 视图。 此指令窜从 Web 窗体世界，并且不应使用 ASP.NET MVC 应用程序中。

## <a name="where-content-is-cached"></a>在其中缓存内容

默认情况下，当你使用 [OutputCache] 属性中，内容又缓存在三个位置： web 服务器、 任何代理服务器和 web 浏览器。 你可以控制完全其中缓存内容通过修改 [OutputCache] 特性的位置属性。

可以将位置属性设置为以下值之一：

> ·任何
> 
> ·客户端
> 
> ·下游
> 
> ·服务器
> 
> ·无
> 
> ·ServerAndClient


默认情况下，位置属性具有值任何。 但是，有一些你可能希望到仅在浏览器上或仅在服务器上缓存的情形。 例如，如果缓存个性化为每个用户的信息然后你不应缓存服务器上的信息。 如果您要为不同的用户显示不同的信息，你应该缓存仅在客户端上的信息。

例如，清单 3 中的控制器公开名为 GetName() 返回当前的用户名称的操作。 如果上插座登录到网站并调用 GetName() 操作然后操作返回字符串"Hi 上插座"。 如果以后，Jill 登录到网站并将调用该 GetName() 操作然后她还将获取字符串"Hi 上插座"。 上插座最初将调用该控制器操作后，该字符串缓存所有用户在 web 服务器上。

**列出 3 – Controllers\BadUserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

最有可能，列出 3 中的控制器工作不希望的方式。 你不想要向 Jill 显示消息"Hi 上插座"。

你应该永远不会缓存服务器缓存中的个性化的内容。 但是，你可能想要缓存在浏览器缓存以提高性能的个性化的内容。 如果缓存在浏览器中的内容，并且用户调用相同的控制器操作多次，然后可以从浏览器缓存中而不是服务器检索内容。

列出 4 中的已修改的控制器缓存 GetName() 操作的输出。 但是，该内容又缓存仅在浏览器和不在服务器上。 这样，多个用户调用 GetName() 方法时，每个人员获取其自己的用户名称和不能是其他用户的用户名。

**列出 4 – Controllers\UserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

请注意，列出 4 中的 [OutputCache] 属性包含一个设置为值 OutputCacheLocation.Client 的位置属性。 [OutputCache] 属性还包括 NoStore 属性。 NoStore 属性用来通知代理服务器和浏览器，它们不应存储缓存的内容的永久副本。

## <a name="varying-the-output-cache"></a>不同的输出缓存

在某些情况下，你可能想非常相同的内容的不同缓存的版本。 例如，假设你要创建主/详细信息页。 主控页显示电影的标题的列表。 当你单击标题时，为所选的电影获取详细信息。

如果缓存的详细信息页，然后将哪些影片无论您单击显示同一电影的详细信息。 第一个影片第一个用户选择将向所有将来的用户显示。

可以通过利用 [OutputCache] 特性的 VaryByParam 属性来解决此问题。 此属性使您能够创建不同的缓存的版本非常相同的内容时窗体参数或查询字符串参数而异。

例如，在列出 5 控制器公开名为 Master() 和 Details() 的两个操作。 Master() 操作返回电影的标题的列表以及 Details() 操作返回的所选的影片详细信息。

**列出 5 – Controllers\MoviesController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

Master() 操作包括 VaryByParam 属性的值"none"。 当调用操作，Master() 相同的缓存版本的主查看，则返回。 参数是任何形式的参数或查询字符串忽略 （请参见图 2）。

**图 2 – /Movies/Master 视图**

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

**图 3-/ 电影/详细信息视图**

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

Details() 操作包括具有"Id"的值的 VaryByParam 属性。 当不同的 Id 参数的值传递给控制器操作时，会生成不同的缓存的版本的详细信息视图。

它是一定要了解使用 VaryByParam 属性结果在多个缓存中，且不小于。 每个不同版本的 Id 参数创建的详细信息视图的不同缓存的版本。

你可以将 VaryByParam 属性设置为以下值：

> \*= 如果窗体或查询字符串参数变化，可以创建不同的缓存的版本。
> 
> none = Never 创建不同的缓存的版本
> 
> 以分号的参数列表 = 创建不同的缓存的版本，当任何列表中的窗体或查询字符串参数变化时


## <a name="creating-a-cache-profile"></a>创建缓存配置文件

作为配置输出缓存属性的修改 [OutputCache] 属性的属性的替代方法，你可以在 web 配置 (web.config) 文件中创建的缓存配置文件。 在 web 配置文件中创建的缓存配置文件提供了几个重要优势。

首先，通过配置输出缓存中的 web 配置文件，可以控制如何控制器操作缓存在一个中心位置的内容。 你可以创建一个缓存配置文件，并将配置文件应用到多个控制器或控制器操作。

其次，你可以修改 web 配置文件无需重新编译你的应用程序。 如果你需要禁用缓存的应用程序已部署到生产，则可以只需修改 web 配置文件中定义的缓存配置文件。 将自动检测并应用到的 web 配置文件的任何更改。

例如，&lt;缓存&gt;列表 6 中的 web 配置节定义名为 Cache1Hour 缓存配置文件。 &lt;缓存&gt;部分必须出现在&lt;system.web&gt; web 配置文件节。

**列出 6 – web.config 的 Caching 部分**

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

列出 7 中的控制器演示了如何将 Cache1Hour 配置文件应用到的控制器操作具有 [OutputCache] 属性。

**列出 7-Controllers\ProfileController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

如果调用公开的列出 7 中的控制器的 index （） 操作则将以 1 小时为单位返回相同的时间。

## <a name="summary"></a>摘要

输出缓存提供与极大地提高你的 ASP.NET MVC 应用程序的性能的轻松方法。 在本教程中，您学习了如何使用 [OutputCache] 属性来缓存输出的控制器操作。 你还了解了如何修改 [OutputCache] 属性，例如要修改如何获取缓存内容的持续时间和 VaryByParam 属性的属性。 最后，您学习了如何在 web 配置文件中定义缓存配置文件。

>[!div class="step-by-step"]
[上一页](understanding-action-filters-cs.md)
[下一页](adding-dynamic-content-to-a-cached-page-cs.md)
