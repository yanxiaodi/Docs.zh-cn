---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
title: "创建视图母版页 (C#) 中使用的页面布局 |Microsoft 文档"
author: microsoft
description: "在本教程中，您将学习如何在你的应用程序中创建多个页的常用页面布局，通过利用视图母版页。 你可以使用..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: dff54fcb-68b1-4488-89a2-ca97532d6a4c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 5d564b7e562435e8c6b1151287cbb1aec3d6bd10
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="creating-page-layouts-with-view-master-pages-c"></a>创建视图母版页 (C#) 中使用的页面布局
====================
通过[Microsoft](https://github.com/microsoft)

[下载 PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_CS.pdf)

> 在本教程中，您将学习如何在你的应用程序中创建多个页的常用页面布局，通过利用视图母版页。 你可用于视图的母版页，例如，定义两列的页面布局和为所有 web 应用程序中的页，都使用两列布局。


## <a name="creating-page-layouts-with-view-master-pages"></a>创建视图母版页中使用的页面布局

在本教程中，您将学习如何在你的应用程序中创建多个页的常用页面布局，通过利用视图母版页。 你可用于视图的母版页，例如，定义两列的页面布局和为所有 web 应用程序中的页，都使用两列布局。

你还可以利用视图的母版页跨应用程序中的多个页共享公共内容。 例如，你可以在视图的母版页中放置您的网站徽标、 导航链接和横幅播发。 这样一来，你的应用程序中每页将自动显示此内容。

在本教程中，您将学习如何创建新视图母版页和创建新视图内容页基于母版页。

### <a name="creating-a-view-master-page"></a>创建视图的母版页

让我们首先创建定义两列布局视图母版页。 你将添加新视图母版页到 MVC 项目，请右键单击 Views\Shared 文件夹中，选择菜单选项**添加、 新项**，并选择**MVC 视图母版页**模板 （请参见图 1）。


[![添加视图的母版页](creating-page-layouts-with-view-master-pages-cs/_static/image2.png)](creating-page-layouts-with-view-master-pages-cs/_static/image1.png)

**图 01**： 添加视图的母版页 ([单击以查看实际尺寸的图像](creating-page-layouts-with-view-master-pages-cs/_static/image3.png))


应用程序中，可以创建多个视图母版页。 每个视图的母版页可以定义不同的页面布局。 例如，您可能希望某些页具有两列布局和其他页面具有三列布局。

视图的母版页看起来非常相似的标准的 ASP.NET MVC 视图。 但是，与普通视图中，不同视图的母版页包含一个或多个`<asp:ContentPlaceHolder>`标记。 `<contentplaceholder>`使用标记来标记的区域的主控页可以在单个内容页面中重写。

例如，列表 1 中的视图母版页定义两列布局。 它包含两个`<contentplaceholder>`标记。 一个`<ContentPlaceHolder>`为每个列。

**列表 1 –`Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample1.aspx)]

列表 1 中的母版页包含两个视图的正文`<div>`对应于两个列的标记。 级联样式表列类应用于这两`<div>`标记。 此类声明顶部的主控页的样式表中定义。 你可以预览视图母版页通过切换到设计视图中的呈现方式。 单击左下角的源代码编辑器的设计选项卡 （请参见图 2）。


[![预览母版页设计器中](creating-page-layouts-with-view-master-pages-cs/_static/image5.png)](creating-page-layouts-with-view-master-pages-cs/_static/image4.png)

**图 02**： 预览设计器中的母版页 ([单击以查看实际尺寸的图像](creating-page-layouts-with-view-master-pages-cs/_static/image6.png))


### <a name="creating-a-view-content-page"></a>创建视图内容页

创建视图的母版页后，你可以创建一个或多个视图基于视图母版页的内容页。 例如，可以通过右键单击 Views\Home 文件夹中，创建主控制器的索引视图内容页选择**添加、 新项**，选择**MVC 视图内容页**模板中，输入名称 Index.aspx，并单击**添加**按钮 （请参见图 3）。


[![添加视图内容页](creating-page-layouts-with-view-master-pages-cs/_static/image8.png)](creating-page-layouts-with-view-master-pages-cs/_static/image7.png)

**图 03**： 添加视图内容页 ([单击以查看实际尺寸的图像](creating-page-layouts-with-view-master-pages-cs/_static/image9.png))


单击添加按钮后，新出现一个对话框，使您能够选择视图的母版页要视图内容页相关联 （请参见图 4）。 你可以导航到我们在上一节中创建 Site.master 视图母版页上。


[![选择母版页](creating-page-layouts-with-view-master-pages-cs/_static/image11.png)](creating-page-layouts-with-view-master-pages-cs/_static/image10.png)

**图 04**： 选择母版页 ([单击以查看实际尺寸的图像](creating-page-layouts-with-view-master-pages-cs/_static/image12.png))


创建新视图内容页基于 Site.master 主控页后，你将收到列出 2 中的文件。

**列出 2 –`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample2.aspx)]

请注意，此视图包含`<asp:Content>`对应于每个标记`<asp:ContentPlaceHolder>`视图母版页中的标记。 每个`<asp:Content>`标记包含指向特定 ContentPlaceHolderID 属性`<asp:ContentPlaceHolder>`，它将重写。

此外，请注意，列出 2 中的内容视图页不包含任何正常的开始和结束 HTML 标记。 例如，它不包含开始和结束`<html>`或`<head>`标记。 正常的开始和结束标记的所有包含在视图的母版页。

你想要在视图内容页中显示任何内容必须放在`<asp:Content>`标记。 如果将任何 HTML 或在这些标记之外的其他内容，然后将你尝试查看的页时收到错误。

无需重写每个`<asp:ContentPlaceHolder>`从母版页的内容视图页中的标记。 你只需重写`<asp:ContentPlaceHolder>`标记当你想要将标记替换为特定的内容。

例如，已修改的索引视图，列出 3 中仅包含两个`<asp:Content>`标记。 每个`<asp:Content>`标记包含一些文本。

**列出 3 –`Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample3.aspx)]

请求的视图中列出的 3 时，它将呈现图 5 中的页。 请注意视图呈现具有两个列的页。 此外，请注意，从视图内容页中的内容合并视图的母版页的内容


[![索引视图内容页](creating-page-layouts-with-view-master-pages-cs/_static/image14.png)](creating-page-layouts-with-view-master-pages-cs/_static/image13.png)

**图 05**: 索引视图内容页 ([单击以查看实际尺寸的图像](creating-page-layouts-with-view-master-pages-cs/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>修改视图母版页页内容

几乎遇到的一个问题立即时使用视图母版页是修改视图母版页内容，要求不同的视图内容页时的问题。 例如，你希望每个页中具有唯一的标题将 web 应用程序。 但是，在视图的母版页 （而不是视图内容页声明标题。 因此，执行自定义方式为每个视图内容页的页标题？

有两种方法，你可以进行修改的视图内容页显示的标题。 首先，将分配到的标题属性页面标题`<%@ page %>`指令声明视图内容页顶部。 例如，如果你想要将页标题"超级极佳 Website"分配给索引视图，然后你可以包括在索引视图的顶部的以下指令：

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample4.aspx)]

索引视图呈现到浏览器中，浏览器标题栏中会显示所需的标题：


[![浏览器标题栏](creating-page-layouts-with-view-master-pages-cs/_static/image17.png)](creating-page-layouts-with-view-master-pages-cs/_static/image16.png)


没有母版视图页中要工作的标题属性的顺序必须满足的一个重要要求。 必须包含视图的母版页`<head runat="server">`而不是常规的标记`<head>`标记其标头。 如果`<head>`标记不包括 runat ="server"属性，则不会显示标题。 母版页包含所需的默认视图`<head runat="server">`标记。

从单独的视图内容页修改母版页内容的备用方法是将你想要在修改的区域`<asp:ContentPlaceHolder>`标记。 例如，假设你想要更改不仅标题，而且还由母版视图页呈现的 meta 标记。 列出 4 中的母版视图页面包含`<asp:ContentPlaceHolder>`中标记其`<head>`标记。

**列出 4 –`Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample5.aspx)]

请注意，`<asp:ContentPlaceHolder>`列出 4 中的标记包含默认内容： 默认标题和默认 meta 标记。 如果你不替代这`<asp:ContentPlaceHolder>`标记在单独的视图内容页中，则将显示默认内容。

列出 5 中的内容视图页重写`<asp:ContentPlaceHolder>`为了显示自定义标题和自定义元标记的标记。

**列出 5-`Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample6.aspx)]

### <a name="summary"></a>摘要

本教程向您提供查看主控页并查看内容页的基本简介。 您学习了如何创建新视图的母版页和创建基于这些视图内容页。 我们还检查，您可以如何修改视图母版页从特定视图内容页中的内容。

>[!div class="step-by-step"]
[上一页](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
[下一页](passing-data-to-view-master-pages-cs.md)
