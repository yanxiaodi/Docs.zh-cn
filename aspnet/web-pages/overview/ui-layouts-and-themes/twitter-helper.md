---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: "Twitter 与 ASP.NET Web 页的帮助器 |Microsoft 文档"
author: tfitzmac
description: "此主题和应用程序演示如何将 Twitter 的帮助器添加到您的 WebMatrix 3 项目。 它包含的 Twitter 帮助器代码，并演示如何调用帮助器..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 07d9c4d485c42b78a42c54c9740af5f67cb44763
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="twitter-helper-with-aspnet-web-pages"></a>Twitter 与 ASP.NET Web 页的帮助器
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 此主题和应用程序演示如何将 Twitter 的帮助器添加到您的 WebMatrix 3 项目。 它包含的 Twitter 帮助器代码，并演示如何调用的帮助器方法。
> 
> 这段代码用于 Twitter.cshtml 文件由开发**天平移**的 Microsoft。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET 网页 (Razor) 3
>   
> 
> 本教程还适用于 ASP.NET Web Pages 2。


## <a name="introduction"></a>介绍

本主题演示如何将 Twitter 的帮助器添加到你的应用程序并使用 Razor 语法来调用的帮助器方法。 Twitter 帮助器，可以轻松合并 Twitter 按钮和你的应用程序中的小组件。 若要使用 Twitter 小组件，如用户的时间线或哈希标记的搜索结果必须先创建[Twitter 上的小组件](https://twitter.com/settings/widgets)。 在创建后你小组件，您将收到一个小组件 id。调用显示小组件的帮助器方法时，你将作为参数传递此小组件 id。

本主题是版本 1.1 的 Twitter API 编写的。 通过直接将 Twitter 帮助程序代码添加到你的项目，你可以更新的帮助器代码，如果 Twitter API 更改。

有关安装 WebMatrix 的信息，请参阅[简介 ASP.NET 网页 2-入门](../getting-started/introducing-aspnet-web-pages-2/getting-started.md)。

## <a name="add-twitter-helper-to-your-project"></a>将 Twitter 帮助器添加到你的项目

若要添加 Twitter 帮助程序，首先，添加名为的文件夹**应用\_代码**到你的项目。 然后，创建名为的文件**Twitter.cshtml**。

![App_Code 文件夹](twitter-helper/_static/image1.png)

Twitter.cshtml 中的默认代码替换为以下代码。

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>从 web 页面调用 Twitter 方法

下面的示例演示如何在你的项目中使用从页的 Twitter 帮助器方法。 在你的项目，你将想要将参数值替换为与你的需求相关的值。 你可以使用提供的小组件 id 来浏览方法的工作方式，但你将想要生成你自己的小组件为你的项目。

不是所有下面所示的参数都是必需的。 可选的参数用于自定义按钮或小组件的显示方式。 例如，请按照按钮仅要求用户名称遵循，但该示例演示如何包含数量的追随者的用户，以及如何指定按钮和语言的大小。

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>查看结果

上面的代码生成以下按钮和小组件。 这些按钮和小组件是功能完整的不屏幕快照。 请按照按钮将显示西班牙语，因为语言参数设置为**es**。

### <a name="follow-button"></a>请按照按钮

[请按照@aspnet)](https://twitter.com/aspnet)<script>！ 函数 （d、 s、 id） {var js，fjs = d.getElementsByTagName(s) [0]，p = /^http:/.test(d.location)？http: https;如果 (！ d.getElementById(id)) {js = d.createElement(s); js.id = id; js.src = p +: / / platform.twitter.com/widgets.js 的; fjs.parentNode.insertBefore （js，fjs）;}}（文档、 脚本、 twitter wjs）;</script>

### <a name="tweet-button"></a>推文按钮

[推文](https://twitter.com/share)<script>！ 函数 （d、 s、 id） {var js，fjs = d.getElementsByTagName(s) [0]，p = /^http:/.test(d.location)？http: https;如果 (！ d.getElementById(id)) {js = d.createElement(s); js.id = id; js.src = p +: / / platform.twitter.com/widgets.js 的; fjs.parentNode.insertBefore （js，fjs）;}}（文档、 脚本、 twitter wjs）;</script>

### <a name="user-timeline-profile"></a>用户时间线 （配置文件）

[通过推文@aspnet ](https://twitter.com/aspnet) <script>！ 函数 （d、 s、 id） {var js，fjs = d.getElementsByTagName(s) [0]，p = /^http:/.test(d.location)？http: https;如果 (！ d.getElementById(id)) {js = d.createElement(s); js.id = id; js.src = p +": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore （js，fjs）;}}（文档、"脚本"、"twitter wjs"）;</script>

### <a name="favorites"></a>收藏夹

[收藏夹的推文通过@Microsoft ](https://twitter.com/Microsoft/favorites) <script>！ 函数 （d、 s、 id） {var js，fjs = d.getElementsByTagName(s) [0]，p = /^http:/.test(d.location)？http: https;如果 (！ d.getElementById(id)) {js = d.createElement(s); js.id = id; js.src = p +": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore （js，fjs）;}}（文档、"脚本"、"twitter wjs"）;</script>

### <a name="list"></a>列表

[从推文@Microsoft/MS\_使用者\_带区](https://twitter.com/microsoft/ms-consumer-brands/)<script>！ 函数 （d、 s、 id） {var js，fjs = d.getElementsByTagName(s) [0]，p = /^http:/.test(d.location)？http: https;如果 (！ d.getElementById(id)) {js = d.createElement(s); js.id = id; js.src = p +": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore （js，fjs）;}}（文档、"脚本"、"twitter wjs"）;</script>

### <a name="search"></a>搜索

[有关推文&quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>！ 函数 （d、 s、 id） {var js，fjs = d.getElementsByTagName(s) [0]，p = /^http:/.test(d.location)？http: https;如果 (！ d.getElementById(id)) {js = d.createElement(s); js.id = id; js.src = p +": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore （js，fjs）;}}（文档、"脚本"、"twitter wjs"）;</script>
