---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: "创建和使用程序的帮助程序在 ASP.NET Web 页 (Razor) 站点 |Microsoft 文档"
author: tfitzmac
description: "本文介绍如何在 ASP.NET Web 页 (Razor) 网站中创建一个帮助程序。 帮助器是包含代码和性能标记是可重用组件..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5d0c1ae09d8fbc91ff76cd4045d439abafee7736
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>创建和使用 ASP.NET Web 页 (Razor) 站点中的一个帮助程序
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本文介绍如何在 ASP.NET Web 页 (Razor) 网站中创建一个帮助程序。 A*帮助器*包括代码和标记，以执行可能需要很长时间或复杂的任务是可重用组件。
> 
> **你将学习：** 
> 
> - 如何创建和使用简单的帮助器。
> 
> 这些是文章中引入的 ASP.NET 功能：
> 
> - `@helper`语法。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET 网页 (Razor) 3
>   
> 
> 本教程还适用于 ASP.NET Web Pages 2。


## <a name="overview-of-helpers"></a>帮助器概述

如果你需要在你的站点中的不同页上执行相同的任务，你可以使用程序的帮助。 ASP.NET Web Pages 包括大量的帮助器，并且有很多数据越多，你可以下载并安装。 (中列出的内置的帮助器中的 ASP.NET Web Pages 列表[ASP.NET API 快速参考](https://go.microsoft.com/fwlink/?LinkId=202907)。)如果没有任何现有的帮助器满足你的需求，你可以创建你自己的帮助器。

一个帮助程序允许你跨多个页面使用常见的代码块。 假设在页中你通常想要创建注意项的设置除了普通段落。 为可能创建了注意`<div>`元素，具有格式为包装盒边框。 而不是每次你想显示注释，请将此相同标记添加到页中，你可以标记打包为一个帮助程序。 然后，您可以插入一行代码具有注释需要的任何位置。

使用如下的帮助使每个页面中的代码，更简单且更易读。 它还使它成为易于维护你的站点，因为如果你需要更改说明的外观，你可以更改在一个位置的标记。

## <a name="creating-a-helper"></a>创建一个帮助程序

此过程演示如何创建创建的说明，如上所述的帮助程序。 这是一个简单的示例，但任何标记和你需要的 ASP.NET 代码，可以包括自定义帮助器。

1. 在网站的根文件夹中，创建名为的文件夹*应用\_代码*。 这是在 ASP.NET 中的保留的文件夹名称的组件，如帮助器代码的放置位置。
2. 在*应用\_代码*文件夹创建一个新*.cshtml*文件并将其命名*MyHelpers.cshtml*。
3. 将现有内容替换为以下：

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    该代码使用`@helper`语法来声明名为的新帮助`MakeNote`。 此特定的帮助器，可以将传递一个名为参数`content`可以包含文本和标记的组合。 帮助器将字符串插入到注意正文使用`@content`变量。

    请注意该文件名为*MyHelpers.cshtml*，但在名为帮助器`MakeNote`。 可以将多个自定义帮助器放入单个文件。
4. 保存并关闭文件。

## <a name="using-the-helper-in-a-page"></a>在页中使用的帮助器

1. 在根文件夹中，创建新的空白文件调用*TestHelper.cshtml*。
2. 向文件中添加以下代码：

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    若要调用你创建的帮助程序，使用`@`跟帮助器的工作所在，一个点的文件名称，然后选择帮助程序名称。 (如果在具有多个文件夹*应用\_代码*文件夹中，你可以使用语法`@FolderName.FileName.HelperName`调用中任何帮助程序嵌套文件夹级别)。 添加用引号引起来，在括号内的文本是说明的帮助器将显示为网页中的一部分的文本。
3. 保存页并在浏览器中运行它。 帮助器生成的注意项右中调用帮助器： 两个段落之间。

    ![显示在浏览器和帮助器如何生成将指定的文本围框内的标记中的页的屏幕截图。](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a>其他资源


[作为 Razor 助手水平菜单](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341)。 由 Mike Pope 此博客文章说明如何创建水平菜单方式使用标记、 CSS 和代码的帮助。

[利用在 ASP.NET 网页中的 HTML5 WebMatrix 和 ASP.NET MVC3 页帮助器](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx)。 通过 Sam 当年此博客文章说明的帮助程序呈现 HTML5`Canvas`元素。

[之间的差异@Helpers和@Functions在 WebMatrix 中](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix)。 由 Mike Brind 此博客条目描述`@helper`语法和`@function`语法以及何时使用每个。
