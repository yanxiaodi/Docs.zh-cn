---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
title: "使用 TagBuilder 类来生成 HTML 帮助程序 (VB) |Microsoft 文档"
author: StephenWalther
description: "Stephen Walther 向你介绍 TagBuilder 类命名为 ASP.NET MVC framework 中的一个有用的实用工具类。 你可以轻松地使用 TagBuilder 类..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: ec26f264-d0ea-4031-9943-825505a3ac4b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 92c003cf929448d0b03f9de76330e9495ac51d20
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-vb"></a>使用 TagBuilder 类来生成 HTML 帮助程序 (VB)
====================
通过[Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther 向你介绍 TagBuilder 类命名为 ASP.NET MVC framework 中的一个有用的实用工具类。 TagBuilder 类可用于轻松生成 HTML 标记。


ASP.NET MVC framework 包括一个名为生成 HTML 帮助器时，可以使用 TagBuilder 类的有用的实用工具类。 TagBuilder 类，如所示的类名称，可以轻松地生成 HTML 标记。 此简易教程，你将获得 TagBuilder 类的概述并了解如何使用此类时生成简单的 HTML 帮助器上呈现 HTML &lt;img&gt;标记。

## <a name="overview-of-the-tagbuilder-class"></a>TagBuilder 类概述

TagBuilder 类包含在 System.Web.Mvc 命名空间。 它具有五个方法：

- AddCssClass() – 使您能够添加新*类 =""*属性设为标记。
- GenerateId() – 可用于向标记添加 id 属性。 此方法将自动替换 id 中的句点 （默认情况下，期间将被下划线取代）
- MergeAttribute() – 可以将属性添加到一个标记。 有多个重载此方法。
- SetInnerText() – 可设置内部标记的文本。 内部文本是 HTML 编码自动。
- Tostring （） – 使您能够呈现标记。 你可以指定是否想要创建的正常标记、 开始标记、 结束标记或自结束标记。
  

TagBuilder 类具有四个重要属性：

- 属性 – 表示的所有标记的属性。
- IdAttributeDotReplacement – 表示 GenerateId() 方法用于替换的句号 （默认值是一个下划线） 的字符。
- InnerHTML – 表示标记的内部内容。 将字符串分配给此属性*不*的字符串进行 HTML 编码。
- TagName – 表示标记的名称。

这些方法和属性为你提供的所有基本的方法和你需要生成的 HTML 标记的属性。 实际上，不需要使用 TagBuilder 类。 你可以改为使用 StringBuilder 类。 但是，TagBuilder 类使您的生活略微简化。

## <a name="creating-an-image-html-helper"></a>创建一个映像 HTML 帮助器

当你创建 TagBuilder 类的实例时，您传递你想要对 TagBuilder 构造函数生成的标记的名称。 接下来，你可以调用等 AddCssClass 和 MergeAttribute() 方法修改的属性标记的方法。 最后，你可以调用要呈现标记的 tostring （） 方法。

例如，清单 1 包含一个图像 HTML 帮助程序。 映像帮助程序通过实现的内部表示为 HTML TagBuilder &lt;img&gt;标记。

**列表 1 – Helpers\ImageHelper.vb**

[!code-vb[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample1.vb)]

列表 1 中的模块包含名为 Image() 的两个重载的方法。 当调用 Image() 方法时，您可以传递一个对象表示一组的 HTML 特性，或不该对象。

请注意如何 TagBuilder.MergeAttribute() 方法用于将单独的属性，如 src 属性添加到 TagBuilder。 请注意，此外，如何 TagBuilder.MergeAttributes() 方法用于将特性的集合添加到 TagBuilder。 MergeAttributes() 方法接受一个字典&lt;字符串、 对象&gt;参数。 RouteValueDictionary 类用于将转换表示的属性转换为字典的集合的对象&lt;字符串、 对象&gt;。

创建的映像帮助程序后，你可以在就像任何其他标准 HTML 帮助你 ASP.NET MVC 视图中使用的帮助器。 列出 2 中的视图使用 Image helper 两次显示 Xbox 同一个映像 （请参见图 1）。 Image() 帮助器称为使用或不 HTML 属性集合。

**列出 2 – Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample2.aspx)]


[![新项目对话框](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)

**图 01**： 使用 Image helper ([单击以查看实际尺寸的图像](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))


请注意，你必须导入与 Index.aspx 视图顶部的映像帮助器关联的命名空间。 使用以下指令，帮助器是导入：

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample3.aspx)]

在 Visual Basic 应用程序中，默认命名空间是应用程序的名称相同。

>[!div class="step-by-step"]
[上一页](creating-custom-html-helpers-vb.md)
[下一页](creating-page-layouts-with-view-master-pages-vb.md)
