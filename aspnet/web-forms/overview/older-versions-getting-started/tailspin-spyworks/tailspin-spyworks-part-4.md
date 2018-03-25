---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: "第 4 部分： 列出了所有产品 |Microsoft 文档"
author: JoeStagner
description: "本系列教程详细介绍所有生成 Tailspin Spyworks 示例应用程序所采取的步骤。 第 4 部分介绍的产品列表与 GridView contr...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 420cdbcc002bcbfff619d399a7a374009999d754
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="part-4-listing-products"></a>第 4 部分： 列表产品
====================
通过[Joe stagner 将](https://github.com/JoeStagner)

> Tailspin Spyworks 演示创建在.NET 平台的功能强大、 可扩展应用程序是如何非常简单。 它演示如何使用 ASP.NET 4 中出色的新功能构建在线商店，包括购物、 结帐和管理关闭。
> 
> 本系列教程详细介绍所有生成 Tailspin Spyworks 示例应用程序所采取的步骤。 第 4 部分介绍的产品列表与 GridView 控件。


## <a id="_Toc260221670"></a>列出的产品与 GridView 控件

让我们在我们的解决方案并选择"添加"和"新项"开始通过"右键单击"实现我们 ProductsList.aspx 页面。

![](tailspin-spyworks-part-4/_static/image1.jpg)

选择"Web 窗体使用母版页"并输入 ProductsList.aspx 一个页名称"。

单击"添加"。

![](tailspin-spyworks-part-4/_static/image2.jpg)

接下来选择我们放置 Site.Master 页的"样式"文件夹，并从"文件夹的内容"窗口中选择它。

![](tailspin-spyworks-part-4/_static/image3.jpg)

单击"确定"以创建页。

我们的数据库填充产品数据，如下所示。

![](tailspin-spyworks-part-4/_static/image4.jpg)

我们的页面因过期而我们再次将使用实体数据源来访问该产品的数据，但我们需要此实例中选择产品实体，我们需要限制返回到只有是所选类别的项。

若要实现此目的，我们将告诉到自动生成 EntityDataSource WHERE 子句，并且我们将指定 WhereParameter。

你将还记得，当我们在我们"产品类别菜单"中创建菜单项我们动态生成的链接通过将 CatagoryID 添加到每个链接查询字符串。 我们将告诉要从该查询字符串参数派生的位置参数的实体数据源。

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

接下来，我们将配置要显示的产品列表的 ListView 控件。 若要创建最佳购物体验我们将为我们 ListVew 中显示每种产品压缩几种简洁的功能。

- 产品名称将是该产品的详细信息视图的链接。
- 将显示产品的价格。
- 将显示产品的映像，并且我们将动态选择的映像从目录映像目录在我们的应用程序。
- 我们将包含一个用于立即将特定的产品添加到购物车链接。

下面是我们 ListView 控件实例的标记。

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

我们动态生成每个显示的产品的多个的链接。

此外，我们测试自己的新页之前我们需要创建目录结构的产品目录映像，如下所示。

![](tailspin-spyworks-part-4/_static/image1.png)

访问我们的产品图像后，我们可以测试我们的产品列表页面。

![](tailspin-spyworks-part-4/_static/image5.jpg)

从网站的主页上，单击其中一个类别列表链接。

![](tailspin-spyworks-part-4/_static/image6.jpg)

现在，我们需要实现 ProductDetials.apsx 页和 AddToCart 功能。

使用文件-&gt;新建以创建页名称 ProductDetails.aspx 我们像以前一样使用母版页的站点。

我们将再次使用 EntityDataSource 控件来访问数据库中的特定产品记录和我们将使用 ASP.NET FormView 控件来显示产品数据，如下所示。

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

如果格式一个看起来有点有趣，给你，请不要担心。 上面的标记留出空间显示布局中为多个我们将更高版本实现的功能。

购物车将表示在我们的应用程序更复杂的逻辑。 若要开始，使用文件-&gt;新建创建调用 MyShoppingCart.aspx 页。

请注意，我们不选择 ShoppingCart.aspx 的名称。

我们的数据库包含名为"ShoppingCart"的表。 我们生成实体数据模型时已为数据库中每个表创建一个类。 因此，实体数据模型生成名为"ShoppingCart"实体类。 我们无法编辑该模型，以便我们无法将该名称用于我们购物车实现或扩展针对我们的需求，但我们将选择而是只需选择将避免冲突的名称。

它也是值得注意的，我们将创建简单的购物车和具有购物车显示嵌入的购物车逻辑。 我们还可以选择在完全独立的业务层中实现我们购物车。

>[!div class="step-by-step"]
[上一页](tailspin-spyworks-part-3.md)
[下一页](tailspin-spyworks-part-5.md)
