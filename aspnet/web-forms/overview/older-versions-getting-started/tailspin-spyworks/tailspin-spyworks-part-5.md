---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: "第 5 部分： 业务逻辑 |Microsoft 文档"
author: JoeStagner
description: "本系列教程详细介绍所有生成 Tailspin Spyworks 示例应用程序所采取的步骤。 第 5 部分将添加一些业务逻辑。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e205788e05a2ad94d86d4847c11c40898b1c3113
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="part-5-business-logic"></a>第 5 部分： 业务逻辑
====================
通过[Joe stagner 将](https://github.com/JoeStagner)

> Tailspin Spyworks 演示创建在.NET 平台的功能强大、 可扩展应用程序是如何非常简单。 它演示如何使用 ASP.NET 4 中出色的新功能构建在线商店，包括购物、 结帐和管理关闭。
> 
> 本系列教程详细介绍所有生成 Tailspin Spyworks 示例应用程序所采取的步骤。 第 5 部分将添加一些业务逻辑。


## <a id="_Toc260221671"></a>添加一些业务逻辑

我们想要可用，每当有人访问我们的网站时我们购物体验。 访问者将能够浏览并将项添加到购物车，即使未注册或登录。 准备好签出时它们将其提供选项进行身份验证并且如果它们不是尚未成员他们将能创建帐户。

这意味着，我们将需要实现逻辑以将购物车从匿名状态转换为"已注册的用户"状态。

让我们创建一个名为"类"目录，然后右键单击文件夹并创建名为 MyShoppingCart.cs 的新"类"文件

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

如前面所提到，我们将扩展实现 MyShoppingCart.aspx 页的类和我们将使用执行此类情况的操作。NET 的强大"分部类"构造。

对我们 MyShoppingCart.aspx.cf 文件的生成的调用如下所示。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

请注意，使用"分部"关键字。

我们刚生成的类文件如下所示。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

我们将通过将 partial 关键字添加到此文件以及合并我们实现。

现在，我们的新类文件如下所示。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

我们将添加到类的第一个方法是"AddItem"方法。 这是当用户单击的产品列表和产品详细信息页上的"添加到画"链接时将最终调用的方法。

将以下内容追加到 using 语句页的顶部。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

并将此方法添加到 MyShoppingCart 类。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

我们将使用 LINQ to Entities 来查看项是否已在车。 如果这样，我们更新项的数量，否则我们创建的选定项的新项

若要调用此方法中，我们将实现类此方法不仅然后显示当前购物 = 车后已添加项的 AddToCart.aspx 页。

右键单击解决方案资源管理器中的解决方案名称，然后添加和命名 AddToCart.aspx，因为我们以前所做的新页。

尽管我们可以使用此页以显示中间结果类似于低股票问题等，我们实现中，页面将不实际呈现，而是调用"添加"逻辑和重定向。

若要完成此我们将以下代码添加到页面\_加载事件。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

请注意，我们在检索要添加到购物车中的查询字符串参数以及调用我们的类的 AddItem 方法的产品。

假设没有错误遇到控制权传递给我们将完全实现下一步的 SHoppingCart.aspx 页面。 如果应我们引发了异常错误。

目前我们没有实现全局错误处理程序以便此异常将进入未处理的我们的应用程序，但我们将很快修正此问题。

另请注意语句 Debug.Fail() （可通过使用`using System.Diagnostics;)`

是在调试器内部运行该应用程序时，此方法将显示包含的信息以及我们指定的错误消息的应用程序状态详细的对话框。

当运行在生产 Debug.Fail() 语句中的将被忽略。

你将注意到我们购物车类名"GetShoppingCartId"中的方法调用上方的代码中。

添加代码，如下所示实现方法。

请注意，我们还添加了更新和签出按钮和标签我们可以在其中显示购物车"总计"。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

现在，我们可以将项添加到我们的购物车，但我们尚未实现的逻辑，以添加产品后显示购物车。

因此，在 MyShoppingCart.aspx 页中我们将添加 EntityDataSource 控件和 GridVire 控件，如下所示。

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

调用的窗体设计器中，以便你可以双击更新购物车按钮和生成在标记中的声明中指定的 click 事件处理。

我们将更高版本实现的详细信息，但执行此操作将告诉我们生成并运行我们的应用程序且未发生错误。

当你运行应用程序，并且将项添加到购物车你将看到此。

![](tailspin-spyworks-part-5/_static/image2.jpg)

请注意，我们未遵从"默认"网格显示通过实现三个自定义列。

第一个是可编辑，按"绑定"字段的数量。

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

下一步是显示的行项总计 （项的成本的乘积的数量，才能进行排序） 的"计算"列：

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

最后，我们有包含用户将使用以指示应购物图表中移除的项的复选框控件的自定义列。

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

如你所见，因此，让我们为空总的行的顺序将添加一些逻辑来计算订单总计。

首先，我们将到 MyShoppingCart 类实现"GetTotal"方法。

MyShoppingCart.cs 文件中添加以下代码。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

然后在页\_我们将可以调用我们 GetTotal 方法的 Load 事件处理程序。 在同一时间，我们将添加一个测试以查看购物车是否为空并相应地调整显示，如果它是。

现在是否为空购物车我们得到如下：

![](tailspin-spyworks-part-5/_static/image4.jpg)

并且，如果不是，我们看到我们总共。

![](tailspin-spyworks-part-5/_static/image5.jpg)

但是，此页尚未完成。

我们将需要其他逻辑来重新计算购物车，通过删除标记为删除的项，如某些可能已更改网格中由用户确定新数量值。

允许添加到购物车类 MyShoppingCart.cs 来处理这种情况，当用户标记中删除的项目中的"RemoveItem"方法。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

现在让我们 ad 方法以处理情况，用户只需更改要按 GridView 排序的质量时。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

与就地基本的删除和更新功能，我们可以实现实际更新购物车中数据库的逻辑。 （在 MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

你将注意到此方法需要两个参数。 一个是购物车 Id，另一个是用户定义类型的对象的数组。

最小化用户界面细节我们逻辑的依赖关系，以便我们已定义一种数据结构，我们可以用来将购物车项传递给我们的代码，而无需我们无需直接访问 GridView 控件的方法。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

在我们的 MyShoppingCart.aspx.cs 文件我们可以使用此结构在我们的更新按钮单击事件处理程序中，如下所示。 请注意，除了更新购物车我们将更新购物车总计以及。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

请注意特别感兴趣使用此代码行：

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() 是一个特殊的 helper 函数，我们将在中实现 MyShoppingCart.aspx.cs，如下所示。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

这提供了用于访问我们的 GridView 控件中的绑定元素的值的全新方法。 由于我们"删除项"复选框控件未绑定我们将通过 FindControl() 方法来访问它。

在你的项目的开发中的此阶段中，我们正在准备实现在结帐过程。

让我们执行此操作之前使用 Visual Studio 生成成员资格数据库并将用户添加到成员身份存储库。

>[!div class="step-by-step"]
[上一页](tailspin-spyworks-part-4.md)
[下一页](tailspin-spyworks-part-6.md)
