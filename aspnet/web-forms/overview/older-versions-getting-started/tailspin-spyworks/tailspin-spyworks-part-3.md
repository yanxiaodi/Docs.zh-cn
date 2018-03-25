---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: "第 3 部分： 布局和类别菜单 |Microsoft 文档"
author: JoeStagner
description: "本系列教程详细介绍所有生成 Tailspin Spyworks 示例应用程序所采取的步骤。 第 3 部分介绍如何添加布局和类别菜单。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 57c0342efb67b94a0d8c8b06dc13a727e7184db8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="part-3-layout-and-category-menu"></a>第 3 部分: 布局和类别菜单
====================
通过[Joe stagner 将](https://github.com/JoeStagner)

> Tailspin Spyworks 演示创建在.NET 平台的功能强大、 可扩展应用程序是如何非常简单。 它演示如何使用 ASP.NET 4 中出色的新功能构建在线商店，包括购物、 结帐和管理关闭。
> 
> 本系列教程详细介绍所有生成 Tailspin Spyworks 示例应用程序所采取的步骤。 第 3 部分介绍如何添加布局和类别菜单。


## <a id="_Toc260221669"></a>添加一些布局和类别菜单

在我们站点的主页面中，我们将添加将包含我们的产品类别菜单的左侧列 div。

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

请注意，将由我们添加到我们的 Style.css 文件的 CSS 类提供所需对齐和其他格式。

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

产品类别菜单将动态创建在运行时通过查询商务数据库，对于现有产品类别以及创建菜单项和相应链接。

若要实现此目的我们将使用两个 ASP。NET 的功能强大的数据控件。 "实体数据源"控件和"ListView"控件。

![](tailspin-spyworks-part-3/_static/image1.jpg)

让我们切换到"设计视图"，并使用以下帮助器来配置我们的控件。

![](tailspin-spyworks-part-3/_static/image2.jpg)

让我们将 EntityDataSource ID 属性设置为费用分配表\_类别\_菜单，然后单击"配置数据源"。

![](tailspin-spyworks-part-3/_static/image3.jpg)

选择已创建为我们为我们的商务数据库创建实体数据源模型时的 CommerceEntities 连接，然后单击"下一步"。

![](tailspin-spyworks-part-3/_static/image4.jpg)

选择"类别"实体集名称，并将其余的选项为默认值。 单击"完成"。

现在让我们设置我们将为 ListView 我们页面放置的 ListView 控件实例的 ID 属性\_ProductsMenu 并激活其帮助器。

![](tailspin-spyworks-part-3/_static/image5.jpg)

但我们无法使用控件选项来设置数据的项显示的格式和格式设置，我们菜单创建将只需要简单标记因此我们将在源视图中输入的代码。

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

请注意"Eval"语句： &lt;%# Eval("CategoryName") %&gt;

ASP.NET 语法&lt;%# %&gt;是指示运行时执行任何包含在中，"行"中的将结果输出的速记约定。

语句 Eval("CategoryName") 指示，当前项的数据项时，绑定的集合中提取的实体模型项名称的值"CatagoryName"。 这是简洁语法非常强大的功能。

我们来运行应用程序现在。

![](tailspin-spyworks-part-3/_static/image6.jpg)

请注意，现在显示我们的产品类别菜单和时我们将鼠标悬停我们可以看到我们必须尚未实现的页的菜单项链接点的类别菜单项的通过一个名为 ProductsList.aspx 并且，我们已生成动态查询字符串自变量，包含 类别 id。

>[!div class="step-by-step"]
[上一页](tailspin-spyworks-part-2.md)
[下一页](tailspin-spyworks-part-4.md)
