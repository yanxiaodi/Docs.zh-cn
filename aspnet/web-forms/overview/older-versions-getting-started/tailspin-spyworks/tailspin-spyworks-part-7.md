---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: "第 7 部分： 添加功能 |Microsoft 文档"
author: JoeStagner
description: "本系列教程详细介绍所有生成 Tailspin Spyworks 示例应用程序所采取的步骤。 第 7 部分添加其他功能，如帐户 revie..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 5280de44b3e75f9d1ae85e0248bc3ef6d5444f6d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="part-7-adding-features"></a>第 7 部分： 添加功能
====================
通过[Joe stagner 将](https://github.com/JoeStagner)

> Tailspin Spyworks 演示创建在.NET 平台的功能强大、 可扩展应用程序是如何非常简单。 它演示如何使用 ASP.NET 4 中出色的新功能构建在线商店，包括购物、 结帐和管理关闭。
> 
> 本系列教程详细介绍所有生成 Tailspin Spyworks 示例应用程序所采取的步骤。 第 7 部分添加了其他功能，如帐户评审、 产品评论和"常用项"和"还购买"用户控件。


## <a id="_Toc260221673"></a>添加功能

尽管用户可以浏览我们的目录，将项放在其购物车，和完成结帐过程，有大量的支持功能，我们将包含以改进我们的站点。

1. 帐户查看 （列表订单放置和查看详细信息。）
2. 将一些上下文特定内容添加到首页。
3. 在目录中添加一项功能以使用户可以查看的产品。
4. 创建用户控件在首页上显示控制的常用项和位置。
5. 创建"还购买"的用户控件并将其添加到产品详细信息页。
6. 添加联系人页。
7. 添加有关页面。
8. 全局错误

## <a id="_Toc260221674"></a>帐户检查

在"帐户"文件夹中创建一个命名的 OrderList.aspx 和其他命名的 OrderDetails.aspx 的两个.aspx 页

OrderList.aspx 将利用 GridView 和 EntityDataSoure 控件一样使用我们具有除前面。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

EntityDataSoure 从 Orders 表中在用户名上筛选选择记录 （请参阅 WhereParameter） 的用户登录时在会话变量中我们设置。

另请注意的 GridView HyperlinkField 中的这些参数：

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

这些名称指定为作为 OrderDetails.aspx 页的查询字符串参数中指定订单 id 字段的每个产品的订单详细信息视图的链接。

## <a id="_Toc260221675"></a>OrderDetails.aspx

我们将使用 EntityDataSource 控件访问订单和 FormView 以显示订单数据，另一个 EntityDataSource 与一个 GridView，若要显示的顺序的所有行项。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

在代码隐藏文件 (OrderDetails.aspx.cs) 我们具有两个小位的管理。

首先，我们需要确保 OrderDetails 始终获取 OrderId。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

我们还需要计算并显示总从行项的顺序。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>主页页面

让我们将一些静态内容添加到 Default.aspx 页上。

首先，我将创建的"内容"文件夹并在该映像文件夹 （并且将包含要在主页上使用的图像。）

到 Default.aspx 页的底部占位符，添加以下标记。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>产品评论

首先我们将向我们可以使用来输入产品评论的窗体添加一个按钮的链接。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

请注意我们在查询字符串中传递 ProductID

下一步让我们添加名为 ReviewAdd.aspx 页

此页将使用 ASP.NET AJAX 控件工具包。 如果你尚未做以便你可以下载它从[DevExpress](http://devexpress.com/act)和上设置适用于使用 Visual Studio 使用此处的工具包没有指南[https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

在设计模式下，从工具箱中拖动控件和验证程序和生成的窗体，如下所示。

![](tailspin-spyworks-part-7/_static/image2.jpg)

标记将如下所示。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

现在，我们可以输入评审，允许在产品页面上显示这些评审。

将此标记添加到 ProductDetails.aspx 页。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

运行应用程序现在和导航到产品显示产品信息，包括客户评审。

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>（创建用户控件） 的常用项控件

为了提高在网站上的销量我们将将几个功能添加到"暗示销售"常用或相关产品。

这些功能的第一个将在我们的产品目录中更受欢迎的产品的列表。

我们将创建一个"用户控件"来显示最畅销的主页上的我们的应用程序的项目。 因为这将为控件，我们可以使用它任何页上通过简单地拖放控件拖到我们喜欢的任何页面上的 Visual Studio 设计器中。

在 Visual Studio 的解决方案资源管理器中，右键单击解决方案名称，然后创建一个名为"控件"的新目录。 虽然不需要这样做，我们将帮助保持我们的项目中组织通过在"控件"目录中创建所有我们用户控件。

右键单击控件文件夹并选择"新项":

![](tailspin-spyworks-part-7/_static/image4.jpg)

指定"PopularItems"我们控件的名称。 请注意用户控件的文件扩展名是.ascx 不.aspx。

将按以下方式定义我们的常见项用户控制。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

此处我们将使用我们没有尚未使用此应用程序中的方法。 我们将使用转发器控件和而不是使用数据源控件我们正在将转发器控件绑定到 LINQ to Entities 查询的结果。

在我们的控制代码隐藏做到这一点，如下所示。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

另请注意在我们的控制标记顶部重要此行。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

因为所有最常用的项不会更改基于分钟我们可以添加一个 aching 指令，用于改进我们的应用程序的性能。 此指令将导致控件代码以缓存的输出的控件过期时只能执行。 否则，将使用的控件的输出缓存的版本。

现在我们需要做的就是在我们 Default.aspc 页中包含我们新的控制。

使用拖放在我们的默认窗体的打开列放置控件的实例。

![](tailspin-spyworks-part-7/_static/image5.jpg)

现在当我们运行我们的应用程序主页上显示的最常用的项。

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>"还购买"控制 （带参数的用户控件）

我们将创建第二个用户控件将会暗示通过添加上下文特殊性销售给下一个级别。

若要计算的顶部的"还购买"项的逻辑很重要。

我们"还购买"的控制将为当前所选的 ProductID 选择 （之前购买） 的 OrderDetails 记录，并获取每个唯一订单，它位于订单 id。

然后我们将选择 al 的产品从所有这些订单和购买数量的总和。 我们将按该数量之和的产品和显示的前五个项目。

考虑到此逻辑的复杂性，我们将为存储过程来实现此算法。

存储过程 T-SQL 的是，如下所示。

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

请注意此存储的过程 (SelectPurchasedWithProducts) 存在于数据库中，当我们在我们的应用程序，我们生成我们指定，除了的表和视图，我们需要实体数据模型的实体数据模型时包括应包括此存储的过程。

若要从实体数据模型中访问该存储的过程，我们需要将函数导入。

双击实体数据模型，在解决方案资源管理器，以在设计器中打开它并打开模型浏览器中，然后在设计器中右键单击并选择"添加函数导入"。

![](tailspin-spyworks-part-7/_static/image1.png)

这样将打开此对话框。

![](tailspin-spyworks-part-7/_static/image2.png)

正如你看到的更高版本，选择"SelectPurchasedWithProducts"填写字段并使用我们导入函数的名称的过程名称。

单击"确定"。

具有执行此我们可以针对存储过程只需编程，因为我们可能会在模型中的任何其他项。

因此，我们"控件"文件夹中创建一个名为 AlsoPurchased.ascx 的新用户控件。

对于此控件的标记看上去很眼熟 PopularItems 控制。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

显著的区别是，不缓存输出，由于要的项的呈现将按产品而异。

ProductId 将是到控件的"属性"。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

控件的 PreRender 事件处理程序中我们 eed 来做三件事。

1. 请确保设置 ProductID。
2. 查看是否存在与当前实例已购买任何产品。
3. 输出为在 #2 中确定某些项。

请注意调用存储的过程，通过模型是多么容易。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

确定的存在"还购买"后我们可以只需将绑定中继器到由查询返回的结果。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

如果不存在任何"还购买"项我们只需将从我们目录中显示其他常用的项。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

若要查看"还购买"项，请打开 ProductDetails.aspx 页，并将 AlsoPurchased 控件从解决方案资源管理器，以使其显示在标记中的此位置中。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

这样将在 ProductDetails 页的顶部创建对控件的引用。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

由于 AlsoPurchased 用户控件还需要大量 ProductId 我们将通过使用 Eval 语句的页的当前数据模型项针对我们的控制将 ProductID 属性设置。

![](tailspin-spyworks-part-7/_static/image3.png)

当我们构建并立即运行并浏览到产品我们将看到"还购买"的项。

![](tailspin-spyworks-part-7/_static/image7.jpg)

>[!div class="step-by-step"]
[上一页](tailspin-spyworks-part-6.md)
[下一页](tailspin-spyworks-part-8.md)
