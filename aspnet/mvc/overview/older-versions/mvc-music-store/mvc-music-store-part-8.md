---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: "第 8 部分： 购物车与 Ajax 更新 |Microsoft 文档"
author: jongalloway
description: "本系列教程详细介绍所有生成 ASP.NET MVC 音乐商店示例应用程序所采取的步骤。 第 8 部分介绍如何使用 Ajax 更新购物车。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 75e1dff96f8b56d74c28ff9d522f4766fbad669f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>第 8 部分： 购物车与 Ajax 更新
====================
通过[Jon Galloway](https://github.com/jongalloway)

> MVC 音乐商店是，从而引入并说明如何使用 ASP.NET MVC 和 Visual Studio 进行 web 开发分步教程应用程序。  
>   
> MVC 音乐商店是一个轻型示例存储区实现，该类销售音乐集联机，并实现基本的站点管理、 用户登录，和购物车功能。  
>   
> 本系列教程详细介绍所有生成 ASP.NET MVC 音乐商店示例应用程序所采取的步骤。 第 8 部分介绍如何使用 Ajax 更新购物车。


我们将允许用户将唱片集放在其购物车中，如果没有注册，但它们需要作为来宾要签出完成注册。 在购物和签出过程将分为两个控制器： 允许匿名将项添加到购物车，购物车控制器和签出控制器用于处理在结帐过程。 我们将使用在本部分中，购物车启动，然后生成下列部分中的在结帐过程。

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>添加车、 顺序和 OrderDetail 模型类

我们购物车和签出的过程将使用的某些新的类。 右键单击 Models 文件夹，然后添加车类 (Cart.cs) 替换为以下代码。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

此类是非常类似于其他我们使用了目前为止，除了 RecordId 属性 [键] 属性。 我们车项将具有名为 CartID 以允许匿名购物的字符串标识符，但表包括名为 RecordId 整数主键。 按照约定，实体框架代码的第一个需要名为购物车的表的主键将 CartId 或 ID，但我们可以轻松地重写通过批注或代码如果我们想。 这是一个示例我们可以如何使用简单的约定的实体框架代码先进先时它们满足 us，但我们正在不受它们时它们不会。

接下来，添加 Order 类 (Order.cs) 替换为以下代码。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

此类跟踪订单的摘要和传递信息。 **它不会尚未编译**，因为它具有一个 OrderDetails 的导航属性，具体取决于我们尚未创建的类。 让我们来修复现在通过添加一个名为 OrderDetail.cs，添加以下代码。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

我们将使一个上次更新到我们 MusicStoreEntities 类，以包括 Dbset 该类将这些新的模型类，还包括 DbSet&lt;艺术家&gt;。 更新后的 MusicStoreEntities 类显示为下面。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>管理购物车业务逻辑

接下来，我们将在模型文件夹中创建的购物车类。 购物车模型处理对车表的数据访问。 此外，它将处理到添加和从购物车中移除项的业务逻辑。

由于我们不希望要求用户注册的帐户，只是为了向其购物车中添加项，我们会将用户分配一个临时的唯一标识符 （使用 GUID 或全局唯一标识符） 时，他们访问购物车。 我们将存储此 ID 使用 ASP.NET 会话类。

*注意： ASP.NET 会话是方便的位置来存储特定于用户的信息将过期后他们离开该站点。虽然不正确使用了会话状态都有较大的站点上的性能影响，我们轻度使用将适用于演示目的。*

购物车类公开以下方法：

**AddToCart**采用唱片集作为参数并将其添加到用户的购物车。 由于车表跟踪每个唱片集的数量，它包括逻辑来创建一个新行，如果需要或只是递增数量，如果用户具有已排序的唱片集的一个副本。

**RemoveFromCart**使用唱片集 ID，并将其从用户的购物车中删除。 如果用户仅在其购物车具有唱片集的一个副本，删除行。

**EmptyCart**从用户的购物车中移除所有项。

**GetCartItems**检索进行显示或处理 CartItems 的列表。

**GetCount**检索唱片集用户具有在其购物车中的总次数。

**GetTotal**计算购物车中的所有项的总成本。

**CreateOrder**签出阶段将购物车转换为顺序。

**GetCart**是一种静态方法，这样我们控制器以获取车对象。 它使用**GetCartId**方法以处理 CartId 读取用户的会话。 GetCartId 方法需要 HttpContextBase，以便它可以从用户的会话中读取用户的 CartId。

下面是完整**购物车类**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>Viewmodel

我们购物车控制器需要进行一些复杂将信息传递给其视图不会完全映射到我们的模型对象。 我们不需要修改以满足我们的视图; 我们模型模型的类应表示我们的域，不是用户界面。 一种解决方案是将信息传递给使用 ViewBag 类中，我们使用存储管理器下拉列表中信息，但通过 ViewBag 传递的大量信息变得非常艰难来管理我们的视图。

到此解决方案是使用*ViewModel*模式。 使用此模式时我们将创建强类型的类对于我们的特定视图方案，进行优化的和其公开的动态值/所需内容由我们视图模板的属性。 然后，我们控制器类可以填充，并将这些视图优化类传递给我们要使用的视图模板。 这样，类型安全，编译时检查，和编辑器 IntelliSense 中查看模板。

我们将在购物车控制器中创建两个视图模型，以用于： ShoppingCartViewModel 将保留用户的购物车的内容和 ShoppingCartRemoveViewModel 将用于显示确认信息，当用户删除内容从购物车中。

让我们在我们要将组织的事物保存的项目的根目录中创建新的 Viewmodel 文件夹。 右键单击项目，选择添加 / 新文件夹。

![](mvc-music-store-part-8/_static/image1.jpg)

该文件夹 Viewmodel 命名。

![](mvc-music-store-part-8/_static/image1.png)

接下来，Viewmodel 文件夹中添加 ShoppingCartViewModel 类。 它具有两个属性： 车项，以及用于在购物车中保存的所有项的总价格的十进制值的列表。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

现在将 ShoppingCartRemoveViewModel 添加到具有以下四个属性的 Viewmodel 文件夹中。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>购物车控制器

购物车控制器有三个主要用途： 将项添加到购物车、 从购物车，删除项和查看购物车的项目。 它将使的三个类使用我们刚刚创建： ShoppingCartViewModel、 ShoppingCartRemoveViewModel 和购物车。 如下所示的 StoreController 和 StoreManagerController，我们将添加一个字段以保存 MusicStoreEntities 的实例。

使用空控制器模板向项目中添加一个新的购物车控制器。

![](mvc-music-store-part-8/_static/image2.png)

下面是完成的购物车控制器。 索引和添加控制器操作应看上去很眼熟。 删除和 CartSummary 控制器操作处理两个的特殊情况下，我们将在下一节中讨论。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>使用 jQuery 的 Ajax 更新

接下来，我们将创建强类型化为 ShoppingCartViewModel 并使用列表视图模板使用相同的方法为之前的购物车索引页。

![](mvc-music-store-part-8/_static/image3.png)

但是，而不是使用次 Html.ActionLink 从购物车中删除项目，我们将使用 jQuery 来"连接"在此视图中的所有链接，其具有 HTML 类 RemoveLink 的 click 事件。 而不是发布窗体，此 click 事件处理程序只需将 AJAX 回调我们 RemoveFromCart 控制器操作。 RemoveFromCart 返回 JSON 序列化结果，其中我们 jQuery 回调然后分析并执行四个快速更新为使用 jQuery 的页：

- 1. 从列表中移除已删除的唱片集
- 2. 更新标头中的购物车计数
- 3. 向用户显示一更新消息
- 4. 更新购物车总价格

因为索引视图中的 Ajax 回调处理删除方案，我们不需要 RemoveFromCart 操作的其他视图。 下面是 /ShoppingCart/Index 视图的完整代码：

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

若要测试此项，我们需要能够将项目添加到我们的购物车。 我们将更新我们**存储详细信息**视图包括"添加到购物车"按钮。 当我们解决一下时，我们可以包括一些我们已添加了唱片集其他信息由于我们上次更新此视图： 流派、 艺术家、 价格和唱片集画面。 此时将显示更新的存储详细信息查看代码，如下面所示。

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

现在我们可以单击通过应用商店和测试中添加和删除专辑与我们的购物车。 运行应用程序，并浏览到存储索引。

![](mvc-music-store-part-8/_static/image4.png)

接下来，单击一种风格，若要查看的唱片集的列表。

![](mvc-music-store-part-8/_static/image5.png)

现在单击唱片集标题显示我们已更新的唱片集详细信息视图，包括"添加到购物车"按钮。

![](mvc-music-store-part-8/_static/image6.png)

单击"添加到购物车"按钮显示我们购物车索引视图中的，购物车摘要列表。

![](mvc-music-store-part-8/_static/image7.png)

在加载了您的购物车之后, 你可以单击删除购物车链接以查看您的购物车的 Ajax 更新。

![](mvc-music-store-part-8/_static/image8.png)

我们已构建了一个有效的购物车这样未注册的用户将项添加到购物车。 在以下部分中，我们将允许用户注册并完成结帐过程。


>[!div class="step-by-step"]
[上一页](mvc-music-store-part-7.md)
[下一页](mvc-music-store-part-9.md)
