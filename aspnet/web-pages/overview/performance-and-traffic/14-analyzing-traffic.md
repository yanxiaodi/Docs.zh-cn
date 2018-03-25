---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: "跟踪 ASP.NET 网页 (Razor) 站点访问者信息 （分析） |Microsoft 文档"
author: tfitzmac
description: "你已收到将你的网站后，你可能想要分析你的网站流量。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>跟踪一个 ASP.NET Web 页 (Razor) 站点的访问者信息 （分析）
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本文介绍如何使用程序的帮助程序将网站分析添加到在 ASP.NET Web 页 (Razor) 网站中的页。
> 
> 你将学习：
> 
> - 如何将有关你的网站流量的信息发送到的分析提供程序。
> 
> 这些是 ASP.NET 编程文章中引入的功能：
> 
> - `Analytics`帮助器。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET 网页 (Razor) 2
> - ASP.NET Web 帮助程序库 （NuGet 包）


分析是用于测量你的网站上的流量，这样你可以了解人们如何使用站点的技术的常用术语。 许多分析服务都可用，包括从 Google、 Yahoo、 StatCounter，和其他服务。

工作原理是，你注册的帐户分析提供程序，你在其中注册站点与你的方式分析想要跟踪。提供程序发送 JavaScript 代码，其中包括 ID 或跟踪你的帐户的代码的段。 将 JavaScript 代码段添加到您想要跟踪的站点上的网页。（你通常分析将段添加到页脚或布局页或其他站点中每一页显示的 HTML 标记。）当用户请求包含这些 JavaScript 代码段之一的页时，代码段会将有关当前页的信息发送到分析提供商联系，记录页有关的各种详细信息。

如果你想要看一下你站点的统计信息，你将登录到分析提供程序的网站。 然后，可以查看各种类型的报表有关您的网站，如上：

- 各个页的页视图的数。 这将告知你 （大约） 有多少人访问站点，并且在你的站点上的页都是最常见。
- 人员花费的时间上的特定页。 这可以告知您的主页上是否保持人的兴趣等。
- 哪些站点人已在之前它们访问您的站点。 这有助于你了解你的流量来自链接，从搜索中，依次类推。
- 当用户访问你的网站并它们保持多长时间。
- 哪些国家/地区距离访客都是从。
- 哪些浏览器和操作系统距离访客使用。

    ![Ch14traffic 1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>使用程序的帮助程序添加到页面的分析

ASP.NET Web Pages 包括多个分析帮助器 (`Analytics.GetGoogleHtml`， `Analytics.GetYahooHtml`，和`Analytics.GetStatCounterHtml`)，使其更容易地管理用于分析的 JavaScript 代码段。 而不是了解如何以及在何处将 JavaScript 代码，放入所要做是添加到页面的帮助器。 你需要提供的唯一信息是帐户名称、 ID 或跟踪代码。 （有关 StatCounter，还必须提供了几个其他值。）

在此过程中，你将创建一个使用的布局页`GetGoogleHtml`帮助器。 如果您已具有包含其他分析提供程序之一的帐户，你可以改为使用该帐户，并根据需要进行细微调整。

> [!NOTE]
> 当你创建的分析帐户时，你注册你想要跟踪的站点的 URL。 如果你正在本地计算机上测试所有内容，不会跟踪 （仅发送的流量是你） 的实际流量，因此将看不到记录和查看站点统计信息。 但此过程演示如何向页中添加一个分析帮助程序。 当你发布你的站点时，实时站点会将信息发送给你的分析提供商。


1. 将 ASP.NET Web 帮助程序库添加到你的网站中所述[安装 ASP.NET 网页站点中的帮助器](https://go.microsoft.com/fwlink/?LinkId=252372)，如果你尚未添加它。
2. 创建具有 Google Analytics 帐户，并记录帐户名称。
3. 创建一个名为的布局页*Analytics.cshtml*并添加以下标记：

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > 你必须将放置到调用`Analytics`网页的正文中的帮助器 (之前`</body>`标记)。 否则，浏览器将不运行该脚本。

    如果你使用不同的分析提供程序，请改为使用以下帮助器之一：

    - (Yahoo)`@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`
4. 替换`myaccount`同名的帐户、 ID 或你在步骤 1 中创建的跟踪代码。
5. 在浏览器中运行页面。 (请确保页中选择**文件**工作区之前运行它。)
6. 在浏览器中查看页面源文件。 你将能够查看呈现的分析代码：

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. 登录到 Google Analytics 站点，并检查你的站点的统计信息。 如果您在实时站点上运行页上，你会看到日志访问页面的项。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他资源

- [Google Analytics 站点](https://www.google.com/analytics/)
- [Yahoo ！网站分析](http://help.yahoo.com/l/us/yahoo/ywa/)
- [StatCounter 站点](http://statcounter.com/)
