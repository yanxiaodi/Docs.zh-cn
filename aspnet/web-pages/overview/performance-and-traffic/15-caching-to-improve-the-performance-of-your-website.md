---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: "在 ASP.NET Web 中缓存数据页 (Razor) 站点的更好的性能 |Microsoft 文档"
author: tfitzmac
description: "你可以提高网站的速度进行存储的即缓存-通常会需要很长时间来检索或处理的数据的结果..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2014
ms.topic: article
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 742409219bd3b05f8ddf2c0d5034919fc9bf1d26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>缓存 ASP.NET Web 页 (Razor) 站点中的数据以便更好的性能
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 此文章介绍了如何为更快的性能，在 ASP.NET Web 页 (Razor) 网站中使用的缓存信息的帮助。 可以通过应用商店和 #8212; 让它来加快你的网站也就是说，缓存和 #8212;通常将需要相当长的时间来检索或处理和，通常不会更改的数据结果。
> 
> **你将学习：** 
> 
> - 如何使用缓存来改进你的网站的响应能力。
> 
> 这些是文章中引入的 ASP.NET 功能：
> 
> - `WebCache`帮助器。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET 网页 (Razor) 3
>   
> 
> 本教程还适用于 ASP.NET Web Pages 2。


每次有人请求页从您的网站时，web 服务器必须执行某项操作以便满足请求。 对于某些页面中，服务器可能需要执行需要 （相对较） 长时间，例如从数据库检索数据的任务。 即使这些任务会长时间采用绝对字词，如果您的网站遇到大量流量，导致对 web 服务器来执行复杂的或速度缓慢任务的各个请求的完整的一系列可针对大量工作。 最终，这可能会影响站点的性能。

提高你在如下情况下的性能的一种方法是网站的缓存数据。 如果你的站点获取重复的请求相同的信息，和信息不需要修改的每个人，并且它不是时间敏感的而不是重新提取或重新计算它，你可以一次提取数据，然后存储结果。 下一次请求传入该信息，您只会收到其缓存。

一般情况下，你的缓存不经常更改的信息。 当你将信息放在缓存中时，它存储在 web 服务器上的内存中。 你可以指定多长时间它应缓存，从几秒到天。 当缓存期到期时，信息是自动从缓存中删除。

> [!NOTE]
> 缓存中的条目可能会删除原因以外，它们已过期。 例如，web 服务器可能会暂时运行低内存，并且它可以回收的内存的一种方法是通过引发缓存条目。 正如你将看到的即使已将信息放入缓存，你必须以确保它仍存在时检查你需要它。


假设您的网站已显示的当前温度和天气预报的页面。 若要获取此类型的信息，可能会将请求发送到外部服务中。 由于此信息不会更改 （在两小时时间段，例如） 的很多，因为外部调用需要时间和带宽，很好的候选用于缓存。

## <a name="adding-caching-to-a-page"></a>添加到页缓存

ASP.NET 包括`WebCache`帮助程序可以轻松地向网站添加缓存并将数据添加到缓存。 在此过程中，你将创建缓存的当前时间的页。 这不是现实世界的示例中，由于当前时间，通常情况下，会更改并此外不是计算复杂。 但是，它是一种好方法，用于说明中操作的缓存。

1. 添加一个名为的新页*WebCache.cshtml*到网站。
2. 将下面的代码和标记添加到页中：

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    当缓存数据时，你将其放入缓存使用名称这是唯一的网站。 在这种情况下，你将使用名为某个缓存项`CachedTime`。 这是`cacheItemKey`代码示例中所示。

    该代码第一次读取`CachedTime`缓存条目。 如果 （即，如果缓存条目不为 null），则返回值，则代码只需将时间变量的值设置为缓存数据。

    但是，如果该缓存条目不存在 （即，它为 null），代码会设置的时间值，将其添加到缓存中，并将过期记录的值设置为一分钟。 在一分钟后该缓存条目将被丢弃。 （缓存中的项的默认过期值为 20 分钟。）该命令`WebCache.Set(cacheItemKey, time, 1, false)`演示如何向缓存添加当前的时间值并将其过期设置为 1 分钟。 设置*slidingExpiration*参数`false`意味着的到期时间不续订它请求每次。 将过期完全 1 分钟之后它最初添加到缓存。 如果将此值设置为`true`1 分钟到期时间从缓存请求值每次重置。

    此代码演示在缓存数据时应始终使用的模式。 您将得到缓存之前，始终首先检查是否`WebCache.Get`方法已返回 null。 请记住的缓存项可能已到期或可能被删除，由于某种其他原因，因此永远不会保证任何给定的项目是在缓存中。
3. 运行*WebCache.cshtml*在浏览器。 (请确保页中选择**文件**工作区之前运行它。)第一次请求页上，时间数据不在缓存中，并且代码具有要添加到缓存的时间值。

    ![cache-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. 刷新*WebCache.cshtml*浏览器中。 这一次，时间数据是在缓存中。 请注意自上次查看页面以来未更改的时间。

    ![cache-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. 等待在清空缓存一分钟，然后刷新页面。 页再次指示在缓存中，时间数据找不到和更新的时间被添加到缓存。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他资源


- [在图表中显示数据](https://go.microsoft.com/fwlink/?LinkId=202895)
- [WebCache API 参考](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx)(MSDN)
