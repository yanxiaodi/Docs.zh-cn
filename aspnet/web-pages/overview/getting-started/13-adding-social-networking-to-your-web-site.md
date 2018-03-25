---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: "将社交网络添加到 ASP.NET Web 页 (Razor) 站点 |Microsoft 文档"
author: tfitzmac
description: "本章介绍如何将您的网站与社交网络服务集成。 在本章中，你将了解如何允许用户书签/链接你的网站..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2c43fa7d286e43f3a4581662ce421c7435e1871f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>添加社交网络到 ASP.NET 网页 (Razor) 站点
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 此文章介绍了如何将 Facebook、 Twitter、 Reddit，和 Digg 的社交网络链接添加到在 ASP.NET Web 页 (Razor) 网站中的页以及如何包含 Twitter 源、 Xbox 游戏卡和 Gravatar 映像。
> 
> 你将学习：
> 
> - 如何使用户可以书签/链接你的站点。
> - 如何添加 Twitter 数据源。
> - 如何将 Facebook 添加**如**到页的按钮。
> - 如何呈现 Gravatar.com 图像。
> - 如何在你的站点上显示 Xbox 玩家卡。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET 网页 (Razor) 2
> - ASP.NET Web 帮助程序库 （NuGet 包）
>   
> 
> 本教程还适用于 ASP.NET Web Pages 3，但使用 ASP.NET Web 帮助程序库的部分除外。


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>链接您的网站上社交网络站点

如果用户喜欢上你的站点的某些内容，他们通常想要与朋友共享。 你可以通过使其轻松显示的标志符号 （图标），用户可以单击共享 Digg、 Reddit、 Facebook、 Twitter 或类似的网站上的页面。

若要显示这些标志符号，添加`LinkSharecode`到页的帮助器。 请访问页面的人员可以单击各个标志符号。 如果他们具有与该社交网络站点的帐户，它们可以在该站点上中发布你的页面的链接。

![图 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. 将 ASP.NET Web 帮助程序库添加到你的网站中所述[安装 ASP.NET 网页站点中的帮助器](https://go.microsoft.com/fwlink/?LinkId=252372)，如果你尚未添加它。-创建一个名为页*ListLinkShare.cshtml*并添加以下标记：

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    在此示例中，当`LinkShare`帮助程序运行，页面标题将作为一个参数，它又将传递到社交网络站点的页标题。 但是，你无法传递所需的任何字符串。 此示例还指定在列表中包括哪些社交网络站点。 你可以指定与您的网站相关的社交网络站点。
- 运行*ListLinkShare.cshtml*在浏览器中的页。 (请确保页中选择**文件**工作区之前运行它。)
- 单击其中一个要注册的站点的字形。 链接将你带到页上所选的社交网络站点在你可以共享的链接。 例如，如果你单击 Reddit 链接，你将会转到`submit to reddit`Reddit 网站上的页面。

    ![图 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>添加 Twitter 源

有关使用与 Twitter API 的当前版本兼容的 Twitter 帮助的信息，请参阅[Twitter 帮助器](../ui-layouts-and-themes/twitter-helper.md)。 此示例演示如何编写你自己的帮助器，因此你可以轻松地重复使用很多页中的代码。

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>显示是 Facebook&quot;如&quot;按钮

在某些情况下，最好的选择是直接从社交网络提供程序，而不是依赖于一个帮助程序获取代码。 这是如果社交网络提供商不是更新的帮助程序更快地更新其选项尤其如此。

若要将 Facebook 功能 （如 Like 按钮） 添加到你的站点，你可以检索从代码段[developers.facebook.com](https://developers.facebook.com/)站点。 在 Facebook 站点上，你可以使用其工具生成了与你的站点相关的代码段。

以下突出显示的代码是 developers.facebook.com 站点上的喜欢按钮工具中检索到的代码。 必须提供你自己的应用程序 id。

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>呈现 Gravatar 映像

A *Gravatar* (&quot;全局识别虚拟形象&quot;) 的映像，可在多个网站上作为用户标识和 #8212; 也就是说，表示你的映像。 例如，Gravatar 可以识别的人员在论坛帖子，在博客注释中，依次类推。 (你可以注册在 Gravatar 网站在自己 Gravatar [http://www.gravatar.com/](http://www.gravatar.com/)。)如果你想要显示在网站上的人的名称或电子邮件地址旁边的图像，你可以使用 Gravatar 帮助器。

在此示例中，你正在使用表示自己单个 Gravatar。 使用 Gravatar 另一种是允许用户指定其 Gravatar 地址时它们在你的站点上注册。 (你可以了解如何使用户可以注册在[添加安全和 ASP.NET Web Pages 站点的成员资格](https://go.microsoft.com/fwlink/?LinkId=202904)。)每当显示为该用户的信息，你可以只需添加 Gravatar 到其中显示用户的名称。

1. 将 ASP.NET Web 帮助程序库添加到你的网站中所述[安装 ASP.NET 网页站点中的帮助器](https://go.microsoft.com/fwlink/?LinkId=252372)，如果你尚未。
2. 创建一个名为的新 web 页*Gravatar.cshtml*。
3. 将以下标记添加到文件： 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    `Gravatar.GetHtml`方法在页上显示 Gravatar 的图像。 若要更改图像的大小，可以作为第二个参数包含一个数字。 默认大小为 80。 数字小于 80 使图像更小。 大于 80 使图像更大的数字。
4. 在`Gravatar.GetHtml`方法，将替换`<Your Gravatar account here>`用于 Gravatar 帐户的电子邮件地址。 （如果你没有 Gravatar 帐户，你可以使用人员合作的电子邮件的地址）。
5. 在浏览器中运行页面。 该页面显示您指定的电子邮件地址的两个 Gravatar 映像。 第二个图像是比第一个较小。 

    ![图 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>显示 Xbox 玩家卡

每个用户时人游戏 Microsoft Xbox 联机，具有唯一的 id。 每个玩家游戏卡中，这显示其信誉、 游戏的分数，以及最近播放游戏形式保持统计信息。 如果你 Xbox 游戏，你可以显示玩家卡你的站点中的网页上使用`GamerCard`帮助器。

1. 将 ASP.NET Web 帮助程序库添加到你的网站中所述[安装 ASP.NET 网页站点中的帮助器](https://go.microsoft.com/fwlink/?LinkId=252372)，如果你尚未。
2. 创建一个名为的新页*XboxGamer.cshtml*并添加以下标记。

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    你使用`GamerCard.GetHtml`属性来指定要显示玩家卡的别名。
3. 在浏览器中运行页面。 该页面显示您指定 Xbox 玩家卡。

    ![图 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
