---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: "一部分 9： 注册和签出 |Microsoft 文档"
author: jongalloway
description: "本系列教程详细介绍所有生成 ASP.NET MVC 音乐商店示例应用程序所采取的步骤。 第 9 部分介绍如何注册和签出。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 1caf836f8c92cbc9ab95e0aa990f81493e577a27
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="part-9-registration-and-checkout"></a>一部分 9： 注册和签出
====================
通过[Jon Galloway](https://github.com/jongalloway)

> MVC 音乐商店是，从而引入并说明如何使用 ASP.NET MVC 和 Visual Studio 进行 web 开发分步教程应用程序。  
>   
> MVC 音乐商店是一个轻型示例存储区实现，该类销售音乐集联机，并实现基本的站点管理、 用户登录，和购物车功能。  
>   
> 本系列教程详细介绍所有生成 ASP.NET MVC 音乐商店示例应用程序所采取的步骤。 第 9 部分介绍如何注册和签出。


在此部分中，我们将创建其购物者的地址和付款信息将收集 CheckoutController。 我们将要求用户注册之前签出我们的站点，因此，此控制器将需要授权。

用户将转到结帐过程从其购物车通过单击"签出"按钮。

![](mvc-music-store-part-9/_static/image1.jpg)

如果用户未登录，将到提示他们。

![](mvc-music-store-part-9/_static/image1.png)

一旦成功登录，然后向用户显示的地址和付款视图。

![](mvc-music-store-part-9/_static/image2.png)

后它们已填充该窗体和提交订单，它们将显示订单确认屏幕。

![](mvc-music-store-part-9/_static/image3.png)

尝试查看不存在顺序或不属于你的订单将显示错误视图。

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>迁移购物车

尽管购物过程匿名的当用户单击签出按钮时，它们将需要注册和登录名。 用户将希望我们将维护之间访问，因此我们将需要在完成注册或登录名时将与用户关联的购物车信息其购物车信息。

这是实际为，非常简单，因为我们购物车类已经将与用户名关联当前购物车中的所有项的方法。 我们只需调用此方法，当用户完成注册或登录名。

打开**AccountController**时我们已设置成员身份和授权我们添加的类。 添加 using 语句引用 MvcMusicStore.Models，然后添加以下 MigrateShoppingCart 方法：

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

接下来，修改登录 post 操作，以调用 MigrateShoppingCart 后用户已进行验证，如下所示：

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

进行相同的更改注册后操作，用户帐户已成功创建后立即：

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

就这么简单-现在匿名的购物车将自动传输到在成功注册或登录名后的用户帐户。

## <a name="creating-the-checkoutcontroller"></a>创建 CheckoutController

右键单击 Controllers 文件夹，然后将新的控制器添加到名为 CheckoutController 使用空控制器模板的项目。

![](mvc-music-store-part-9/_static/image5.png)

首先，添加 Authorize 属性来要求用户注册之前签出该控制器类声明的上方：

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*注意： 这是类似于我们先前对 StoreManagerController，所做的更改，但在这种情况下 Authorize 属性所需的用户是管理员角色。在签出控制器中，我们要要求用户身份登录，但不要求，它们管理员。*

为简单起见，我们不会处理在本教程中的付款信息。 相反，我们允许用户签出使用促销代码。 我们将存储此促销代码使用名为 PromoCode 常量。

如下所示 StoreController，我们将声明某个字段包含名为 storeDB 的 MusicStoreEntities 类的实例。 为了使使用 MusicStoreEntities 类，我们将需要添加 using 语句 MvcMusicStore.Models 命名空间。 请查看控制器顶部如下所示。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

CheckoutController 将具有以下的控制器操作：

**AddressAndPayment （GET 方法）**将显示的窗体，以允许用户输入他们的信息。

**AddressAndPayment （POST 方法）**将验证输入和处理顺序。

**完成**将在用户成功完成结帐过程后显示。 此视图将包括用户的订单号作为确认。

首先，让我们将索引控制器操作 （其中我们创建控制器时生成） 重命名为 AddressAndPayment。 此控制器操作只是显示签出窗体中，因此它不需要任何模型信息。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

我们 AddressAndPayment POST 方法将遵循我们 StoreManagerController 中使用了相同的模式： 它将尝试接受表单提交并完成顺序，并将重新显示窗体，如果失败。

验证窗体输入满足订单我们验证要求后，我们将直接检查 PromoCode 窗体值。 假设所有内容正确无误，我们将与订单保存更新的信息，让 ShoppingCart 对象完成订单过程中，并将重定向到完成操作。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

在结帐过程成功完成，用户将重定向到完成控制器操作。 此操作将执行验证的顺序未确实属于登录的用户在显示订单号作为一条确认消息之前的简单检查。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*注意： 错误视图自动创建为我们 /Views/Shared 文件夹中我们开始项目时。*

完整的 CheckoutController 代码如下所示：

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>添加 AddressAndPayment 视图

现在，让我们创建 AddressAndPayment 视图。 右键单击其中一个 AddressAndPayment 控制器操作并添加名为 AddressAndPayment 强类型化为订单并使用编辑模板中，如下所示的视图。

![](mvc-music-store-part-9/_static/image6.png)

此视图将使用的两个生成 StoreManagerEdit 视图时在我们讨论的技术：

- 我们将使用 Html.EditorForModel() 以显示的顺序模型的窗体字段
- 我们将利用 Order 类中使用验证特性的验证规则

我们将开始通过更新窗体代码以使用 Html.EditorForModel()，对促销代码跟其他文本框。 AddressAndPayment 视图的完整代码所示。

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>为顺序定义验证规则

既然我们视图设置，我们将设置验证规则为我们的顺序模型我们像以前唱片集模型一样。 右键单击 Models 文件夹，然后添加名为顺序的类。 除了我们用于以前唱片集的验证属性，我们将还使用正则表达式来验证用户的电子邮件地址。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

试图提交窗体具有缺少或无效的信息现在将显示错误消息，使用客户端验证。

![](mvc-music-store-part-9/_static/image7.png)

好了，我们已完成大部分结帐过程中; 的复杂工作我们只具有少量机率 and 前端和后端完成。 我们需要添加两个简单的视图，并且我们需要负责的登录过程的购物车信息 handoff。

## <a name="adding-the-checkout-complete-view"></a>添加签出完整的视图

签出完整的视图是非常简单，因为它只需要显示订单 id。 右键单击完成控制器操作，然后添加名为强类型化为 int 的完成的视图

![](mvc-music-store-part-9/_static/image8.png)

现在我们将更新查看代码，以显示订单 ID，如下所示。

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>更新视图时出错

默认模板包括在共享 views 文件夹中的错误视图，以便它可以重复使用在其他位置在站点中。 此错误视图包含非常简单的错误，并且不使用我们的站点布局，因此我们将更新。

由于这是一般性错误页，内容是非常简单。 我们将包含一条消息和链接以导航到历史记录中前一页中，如果用户想要重试这些文件的操作。

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


>[!div class="step-by-step"]
[上一页](mvc-music-store-part-8.md)
[下一页](mvc-music-store-part-10.md)
