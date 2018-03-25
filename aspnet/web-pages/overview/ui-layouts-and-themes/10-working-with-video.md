---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: "在 ASP.NET 网页中显示视频页 (Razor) 站点 |Microsoft 文档"
author: tfitzmac
description: "本章介绍如何显示在带有 Razor 语法页 ASP.NET Web Pages 的视频。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 4e7dc50fb60546d1e1f10a16ed863c0b812ec82b
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>在 ASP.NET 网站页 (Razor) 中显示视频
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 此文章介绍了如何在 ASP.NET Web 页 (Razor) 网站中使用的视频 （媒体） 播放器以使用户可以查看存储在站点的视频。 使用 Razor 语法的 ASP.NET 网页，您可以播放 Flash (*.swf*)，Media Player (*.wmv*)，和 Silverlight (*.xap*) 视频。
> 
> 你将学习：
> 
> - 如何选择视频播放器。
> - 如何添加到网页上的视频。
> - 如何设置视频播放器特性。
> 
> 这些是 ASP.NET Razor 页文章中引入的功能：
> 
> - `Video`帮助器。
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


## <a name="introduction"></a>介绍

你可能想要显示在你的站点上的视频。 若要这样做的一种方法是链接到网站已视频中的，如 YouTube。 如果你想要直接在您自己的页面中嵌入这些站点中的视频，可以从站点通常获取 HTML 标记，然后将其复制到你的页面。 例如，下面的示例演示如何嵌入了 YouTube 视频：

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

如果你想要播放的视频，位于您自己的网站 （不在公共视频共享站点），你无法直接链接到它使用类似此嵌入的标记。 但是，通过使用，从您的网站播放视频`Video`帮助器，呈现直接在页中的媒体播放器。

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>选择视频播放器

有大量的视频文件格式，并且每种格式通常需要使用不同的玩家和另一种方式配置播放器。 在 ASP.NET Razor 页中，也可以在网页中使用的视频`Video`帮助器。 `Video`帮助程序简化的网页中嵌入视频，因为它自动生成过程`object`和`embed`通常用于视频添加到页面的 HTML 元素。

`Video`帮助器支持以下媒体播放器：

- Adobe Flash
- Windows MediaPlayer
- Microsoft Silverlight

### <a name="the-flash-player"></a>Flash Player

`Flash`播放器的`Video`帮助器，可以播放闪存视频 (*.swf*文件) 网页中。 至少，你必须提供视频文件的路径。 如果指定了任何内容但路径，播放器将使用设置 Flash 的当前版本的默认值。 典型的默认设置如下：

- 使用其默认宽度和高度显示视频和不使用背景颜色。
- 加载页面时，会自动播放视频。
- 视频循环显式停止之前一直。
- 视频扩展以显示所有的视频中，而不是裁剪视频，以适应特定大小。
- 在窗口播放视频。

### <a name="the-mediaplayer-player"></a>MediaPlayer 播放器

`MediaPlayer`播放器的`Video`帮助器，您可以播放 Windows Media 视频 (*.wmv*文件)，Windows Media 音频 (*.wma*文件)，和 MP3 (*.mp3*文件） 网页中。 必须包含的要播放; 的媒体文件的路径所有其他参数是可选的。 如果你仅指定一个路径，播放器将使用默认设置设置 MediaPlayer 的当前版本，如：

- 在视频显示时使用其默认宽度和高度。
- 加载页面时，会自动播放视频。
- 视频播放一次 （它不会循环的）。
- 播放器用户界面中显示的完整控件集。
- 在窗口播放视频。

### <a name="the-silverlight-player"></a>Silverlight 播放器

`Silverlight`播放器的`Video`帮助器，您可以播放 Windows Media 视频 (*.wmv*文件)，Windows Media 音频 (*.wma*文件)，和 MP3 (*.mp3*文件）。 必须设置 path 参数，以指向基于 Silverlight 的应用程序包 (*.xap*文件)。 此外必须设置宽度和高度参数。 其他所有参数都是可选参数。 当你使用 Silverlight 播放器有关视频，如果设置所需的参数时，Silverlight 播放器显示不带背景颜色的视频。

> [!NOTE]
> 如果您尚不知道 Silverlight: *.xap*文件是一个压缩的文件，其中包含中的布局说明*.xaml*文件，程序集和可选资源中的托管的代码。 你可以创建*.xap*为 Silverlight 应用程序项目的 Visual Studio 中的文件。


`Silverlight`视频播放器使用两个播放器提供的设置和中提供的设置*.xap*文件。

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>MIME 类型
> 
> 当浏览器下载一个文件时，则浏览器可确保文件类型匹配的文档的呈现为指定的 MIME 类型。 MIME 类型是一个文件的内容类型或媒体类型。 `Video`帮助器使用以下 MIME 类型：
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>播放 Flash (.swf) 视频

此过程演示如何播放闪存视频名为*sample.swf*。 此过程假设您有一个名为文件夹*媒体*你的站点和上*.swf*文件是在该文件夹中。

1. 将 ASP.NET Web 帮助程序库添加到你的网站中所述[安装 ASP.NET 网页站点中的帮助器](https://go.microsoft.com/fwlink/?LinkId=252372)，如果你尚未添加它。
2. 在网站中，添加一个页面，并将其命名*FlashVideo.cshtml*。
3. 向页面添加以下标记： 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. 在浏览器中运行页面。 (请确保页中选择**文件**工作区之前运行它。)将显示的页和自动播放视频。 

    ![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")

你可以设置`quality`参数到闪存视频`low`， `autolow`， `autohigh`， `medium`， `high`，和`best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

你可以更改闪存的视频，以播放特定大小使用`scale`参数，您可以对以下设置：

- `showall`。 这将使整个视频可见同时保持原始纵横比。 但是，你可能会得到与每个边的边框。
- `noorder`。 这同时保持原始纵横比，缩放视频，但它可能会裁剪。
- `exactfit`。 这将使整个视频可见而无需保留原始的纵横比，但可能会发生扭曲。

如果没有指定`scale`参数，整个视频可以可见，而无需任何裁剪将保留原始的纵横比。 下面的示例演示如何使用`scale`参数：

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Flash player 支持的视频模式设置命名`windowMode`。 你可以将此设置为`window`， `opaque`，和`transparent`。 默认情况下，`windowMode`设置为`window`，它会在网页上的单独窗口中显示视频。 `opaque`设置隐藏在网页上的视频后面的所有内容。 `transparent`设置可让 web 页的背景图像显示视频中，通过假设透明的视频的任何部分。

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>播放 MediaPlayer (*.wmv*) 视频

下面的过程演示如何播放窗口媒体视频名为*sample.wmv* ，位于*媒体*文件夹。

1. 将 ASP.NET Web 帮助程序库添加到你的网站中所述[安装 ASP.NET 网页站点中的帮助器](https://go.microsoft.com/fwlink/?LinkId=252372)，如果你尚未。
2. 创建一个名为的新页*MediaPlayerVideo.cshtml*。
3. 向页面添加以下标记： 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. 在浏览器中运行页面。 视频加载，并会自动播放。 

    ![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")

你可以设置`playCount`为整数类型的值，该值指示多少次，以自动播放视频：

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

`uiMode`参数允许你指定哪些控件显示在用户界面。 你可以设置`uiMode`到`invisible`， `none`， `mini`，或`full`。 如果没有指定`uiMode`参数，视频将显示与状态窗口，查找条形图、 控制按钮和音量控制，除了视频窗口。 如果你使用播放器播放音频文件，也会显示这些控件。 下面是如何使用的一个示例`uiMode`参数：

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

默认情况下，音频已启用并时播放视频。 你可以通过设置静音音频`mute`参数为 true:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

你可以通过设置控制 MediaPlayer 视频的音频级别`volume`到 0 和 100 之间的值的参数。 默认值为 50。 以下是一个示例：

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>播放 Silverlight 视频

此过程演示如何播放视频 Silverlight 中包含*.xap*页的文件夹中名为*媒体*。

1. 将 ASP.NET Web 帮助程序库添加到你的网站中所述[安装 ASP.NET 网页站点中的帮助器](https://go.microsoft.com/fwlink/?LinkId=252372)，如果你尚未。
2. 创建一个名为的新页*SilverlightVideo.cshtml*。
3. 向页面添加以下标记： 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. 在浏览器中运行页面。 

    ![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他资源


[Silverlight 概述](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Flash 对象和嵌入标记特性](http://kb2.adobe.com/cps/127/tn_12701.html)

[Windows Media Player 11 SDK PARAM 标记](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
