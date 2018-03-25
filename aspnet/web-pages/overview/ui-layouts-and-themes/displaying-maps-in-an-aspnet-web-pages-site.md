---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: "在 ASP.NET 网页中显示地图页 (Razor) 站点 |Microsoft 文档"
author: tfitzmac
description: "此文章介绍了如何在基于映射提供的必应、 Google、 Ma 服务的 ASP.NET Web 页 (Razor) 网站网页上显示交互式的地图..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 6f3e6a0cfb8c08cd971e88986d0f059dd8237aab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>在 ASP.NET 网站页 (Razor) 中显示地图
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 此文章介绍了如何在基于映射通过必应、 Google、 MapQuest 和 Yahoo 提供服务的 ASP.NET Web 页 (Razor) 网站网页上显示交互式的地图。
> 
> 你将学习：
> 
> - 如何生成基于地址的映射。
> - 如何生成基于纬度和经度坐标的映射。
> - 如何注册必应地图开发人员帐户，并获取用于 Bing 地图密钥。
> 
> 这是文章中引入的 ASP.NET 功能：
> 
> - `Maps`帮助器。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> 本教程还适用于 WebMatrix 3。


在 Web 页中，你可以映射页面上显示通过使用`Maps`帮助器。 你可以生成基于地址或一组的经度和纬度坐标的地图。 `Maps`类，可以调入常用映射引擎包括必应、 Google、 MapQuest 和 Yahoo。

将映射添加到页的步骤都相同的调用而与所映射引擎。 你只需添加使可用的方法，以显示地图的 JavaScript 文件引用，然后调用的方法`Maps`帮助器。

选择在其上基于的映射服务`Maps`你使用的帮助器方法。 你可以使用下列任一：

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>安装所需的组件

若要显示地图，您需要这些部分：

- `Maps`帮助器。 此帮助程序是版本 2 中的 ASP.NET Web 帮助程序库。 如果你尚未添加到库，你可以安装它在你的网站作为 NuGet 程序包。 有关详细信息，请参阅[安装 ASP.NET 网页站点中的帮助器](https://go.microsoft.com/fwlink/?LinkId=252372)。 (在库，搜索`microsoft-web-helpers`包。)
- JQuery 库中。 WebMatrix 网站模板的几个已包含中的 jQuery 库其*脚本*文件夹。 如果你没有这些库，你可以下载最新的 jQuery 库直接从[jQuery.org](http://jQuery.org)站点。 也可以创建新站点使用模板 (例如，**入门站点**模板) 然后将 jQuery 文件从该站点复制到当前站点。

最后，如果你想要使用必应地图，必须先创建一个 （免费） 帐户，并获取密钥。 获取密钥，请执行以下步骤：

1. 在创建帐户[必应地图开发人员帐户](https://www.microsoft.com/maps/developers/web.aspx)。 你必须具有 Microsoft 帐户 (Windows Live ID) 以及。

    你可以指定你想要使用的键**评估/测试**。 如果你要使用 WebMatrix 和 IIS Express 自己计算机上测试映射函数，请转到**站点**工作区并记下你的站点的 URL (例如， `http://localhost:50408`，尽管端口号可能不同)。 你可以使用此*localhost*作为注册时的站点的地址。
2. 你的帐户注册后，请转到必应地图帐户中心，然后单击**创建或视图键**:

    ![mapping-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. 记录必应创建的密钥。

## <a name="creating-a-map-based-on-an-address-using-google"></a>创建基于 （使用 Google） 的地址映射

下面的示例演示如何创建将基于地址结构图呈现的页。 此示例演示如何使用 Google 映射。

1. 创建名为的文件*MapAddress.cshtml*站点的根目录中。 此页将生成基于传递给它的地址映射。
2. 将以下代码复制到文件中，覆盖现有的内容。

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    请注意页上的以下功能：

    - `<script>`中的元素`<head>`元素。 在示例中，`<script>`元素引用*jquery 1.6.4.min.js*文件，它是 jQuery 库，版本 1.6.4 的缩减 （压缩） 版本。 请注意引用假定*.js*文件位于*脚本*网站文件夹。 

        > [!NOTE]
        > 如果你正在使用 jQuery 库的不同版本，只需确保的你要指向该版本正确。
    - 调用`@Maps.GetGoogleHtml`页的正文中。 若要将地址映射，你必须传递地址字符串。 对于其他映射引擎方法的工作原理类似的方式 (`@Maps.GetYahooHtml`， `@Maps.GetMapQuestHtml`)。
- 运行页面，并输入一个地址。 该页面显示地图中，基于 Google 地图，显示你指定的位置。

    ![mapping-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>创建基于纬度和经度地图坐标 （使用必应）

此示例演示如何创建基于坐标的映射。 此示例演示如何使用 Bing 地图以及如何包括你必应的密钥。 （可以创建基于使用的其他映射引擎还，而不使用必应密钥的坐标的映射。）

1. 创建名为的文件*MapCoordinates.cshtml*根目录中的站点和替换下面的代码和标记的现有内容：

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. 替换`your-key-here`替换先前生成的 Bing 地图密钥。
3. 运行*MapCoordinates.cshtml*页上，输入纬度和经度坐标，然后单击**Map It ！** 按钮。 （如果你不知道任何坐标，请尝试以下。 这是 Microsoft 雷蒙德办公区上的位置）。

    - 纬度： 47.6781005859375
    - 经度：-122.158317565918

    将显示的页，使用你指定的坐标。

    ![mapping-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他资源


[Microsoft.Maps API 参考](https://msdn.microsoft.com/library/gg427611.aspx)
