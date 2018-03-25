---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: "呈现 ASP.NET Web 页 (Razor) 站点对于移动设备 |Microsoft 文档"
author: tfitzmac
description: "本文介绍如何在将相应地呈现在移动设备的 ASP.NET Web 页 (Razor) 站点中创建页面。 你将了解： 你如何..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 899bbdef82d689be81cd77ea6805e0484fb614aa
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>移动设备的的呈现 ASP.NET Web 页 (Razor) 站点
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本文介绍如何在将相应地呈现在移动设备的 ASP.NET Web 页 (Razor) 站点中创建页面。
> 
> 你将学习：
> 
> - 如何使用命名约定指定页专为移动设备设计。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET 网页 (Razor) 3
>   
> 
> 本教程还适用于 ASP.NET Web Pages 2。


ASP.NET Web 页中，可以在移动或其他设备上创建自定义呈现内容的显示。

在 ASP.NET Web Pages 站点中创建设备特定的页上的最简单方法是使用如下的文件命名模式： *FileName。**移动**.cshtml*。 你可以创建两个版本的页面 (例如，一个名为*MyFile.cshtml*和一个名为*MyFile.Mobile.cshtml*)。 在运行的时，当移动设备请求*MyFile.cshtml*，ASP.NET 将从内容呈现*MyFile.Mobile.cshtml*。 否则为*MyFile.cshtml*呈现。

下面的示例演示如何通过添加移动设备的内容页中启用移动呈现。 *Page1.cshtml*包含内容以及导航侧边栏。 *Page1.Mobile.cshtml*包含相同的内容，但省略侧栏。

1. 在 ASP.NET Web Pages 站点中，创建名为的文件*Page1.cshtml*并将当前的内容替换为以下标记。

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. 创建名为的文件*Page1.Mobile.cshtml*并将现有内容替换为以下标记。 请注意该页面的移动版本省略更好地呈现较小屏幕上的导航部分。

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. 运行桌面浏览器并浏览到*Page1.cshtml*。 ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. 运行移动浏览器 （或移动设备仿真程序） 并浏览到*Page1.cshtml*。 (请注意，不包括*.mobile。* 作为 URL 的一部分。）即使该请求是*Page1.cshtml*，ASP.NET 呈现*Page1.Mobile.cshtml*。

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> 若要测试移动页，可以使用在台式计算机运行的移动设备模拟器。 此工具允许你测试 web 页面，因为它们将显示在移动设备上 （即，通常通过小得多显示区域）。 模拟器的一个示例是[用户代理切换器外接程序](http://addons.mozilla.org/firefox/addon/user-agent-switcher/)对 Mozilla Firefox，它可让你模拟从桌面版本的 Firefox 的各种移动浏览器。


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他资源


[Windows Phone 仿真程序](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
