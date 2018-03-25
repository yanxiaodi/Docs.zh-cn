---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: "创建分级控件 (C#) |Microsoft 文档"
author: wenz
description: "很多网站中，从电子商务到社区站点，其为用户提供到速率文章或项目。 这通常需要某些编码工作，但是我们具有..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c004522ac72b848e42320862d77bced0c11ca15
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-rating-control-c"></a>创建分级控件 (C#)
====================
通过[Christian Wenz](https://github.com/wenz)

[下载代码](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip)或[下载 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)

> 很多网站中，从电子商务到社区站点，其为用户提供到速率文章或项目。 这通常需要某些编码工作，但我们尚未控件工具包到我们的处理方法。


## <a name="overview"></a>概述

很多网站中，从电子商务到社区站点，其为用户提供到速率文章或项目。 这通常需要某些编码工作，但我们尚未控件工具包到我们的处理方法。

## <a name="steps"></a>步骤

首先，你需要 （至少） 两种类型的映像： 一个用于填充出分级项和空分级项。 分级项通常是星型或笑脸。 对于此方案中，源代码下载本教程的一部分找到三个文件、 smiley.png 和 empty.png 和 smiley done.png。

然后，创建一个新的 ASP.NET 文件并开始添加`ScriptManager`对它控制：

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

然后，将添加`Rating`ASP.NET AJAX 控件工具包中的控件。 需要为此示例设置以下属性：

- `CurrentRating`要使用初始分级
- `MaxRating`最高评级
- `EmptyStarCssClass`要使用的分级项 （星号） 时的 CSS 类为空
- `FilledStarCssClass`填写要使用的分级项 （星号） 时的 CSS 类
- `StarCssClass`要用于可见 stat 的 CSS 类
- `WaitingStarCssClass`要使用星级发送回发到服务器时的 CSS 类

这是将创建具有五个级别控件的标记的其中一个填写最初的项 （表情符号）：

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

三个引用的 CSS 类现在需要显示相应的图像文件，这是很简单的操作使用 CSS:

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

请确保你提供的宽度和高度这三个图像，否则显示可能看起来有点 messed 向上。

最后，此级别的结果应是向用户显示 （或至少保存在数据库）。 因此，添加回发到服务器分级窗体的文本消息和提交按钮的输出的标签：

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

在服务器端代码中，访问级别控制通过其`ID`，然后访问其`CurrentRating`属性，它是所选的评级项，在我们的示例 0 和 5 之间的值的数目。

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

保存页，并将其加载到你的浏览器。 当鼠标悬停在 （最初为空） 评级项时，JavaScript 效果发生情况： 分级更改。 单击上的一套星，会保留当前的分级。 最后，在提交窗体时，服务器端代码将输出所选的评级。


[![使用最少的代码创建的分级系统](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)

使用最少的代码创建的分级系统 ([单击以查看实际尺寸的图像](creating-a-rating-control-cs/_static/image3.png))

>[!div class="step-by-step"]
[下一篇](creating-a-rating-control-vb.md)
