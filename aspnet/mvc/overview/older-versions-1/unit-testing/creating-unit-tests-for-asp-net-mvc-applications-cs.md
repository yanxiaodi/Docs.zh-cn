---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
title: "为 ASP.NET MVC 应用程序 (C#) 中创建单元测试 |Microsoft 文档"
author: StephenWalther
description: "了解如何创建单元测试的控制器操作。 在本教程中，Stephen Walther 演示如何测试是否控制器操作返回 parti..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: d3a270b9-d7b1-47f2-8775-fc3beb518b5c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
msc.type: authoredcontent
ms.openlocfilehash: 56c981363f1905c1c9869dbaf2adb6b5ac1c28a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-c"></a>为 ASP.NET MVC 应用程序 (C#) 中创建单元测试
====================
通过[Stephen Walther](https://github.com/StephenWalther)

[下载 PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_CS.pdf)

> 了解如何创建单元测试的控制器操作。 在本教程中，Stephen Walther 演示如何测试是否控制器操作返回的特定视图、 返回一组特定的数据，或返回不同类型的操作结果。


本教程的目标是演示如何编写单元测试控制器的 ASP.NET mvc 应用程序。 我们讨论如何生成三个不同类型的单元测试。 你了解如何测试控制器操作返回的视图、 如何测试的控制器操作，返回的视图数据以及如何测试一个控制器操作将你重定向到第二个控制器操作。

## <a name="creating-the-controller-under-test"></a>创建测试控制器

让我们首先创建我们想要测试的控制器。 控制器上，名为`ProductController`，包含在清单 1。

**列表 1 –`ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample1.cs)]

`ProductController`包含名为的两个操作方法`Index()`和`Details()`。 这两种操作方法返回的视图。 请注意，`Details()`操作接受一个参数，名为 id。

## <a name="testing-the-view-returned-by-a-controller"></a>测试视图返回的控制器

假设我们想要测试是否`ProductController`返回右视图。 我们想要确保当`ProductController.Details()`调用操作，返回的详细信息视图。 列出 2 中的测试类包含用于测试返回的视图的单元测试`ProductController.Details()`操作。

**列出 2 –`ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample2.cs)]

列出 2 中的类包括一个名为的测试方法`TestDetailsView()`。 此方法将包含三行代码。 第一行代码创建的新实例`ProductController`类。 代码的第二行时，将调用控制器的`Details()`操作方法。 最后，代码检查是否视图返回的最后一行`Details()`操作为详细信息视图。

`ViewResult.ViewName`属性表示由控制器返回视图的名称。 有关测试此属性的一个大警告。 有两个控制器可以返回视图的方法。 控制器可以显式返回的视图如下：

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample3.cs)]

或者，可以如下的控制器操作的名称从推断出的视图名称：

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample4.cs)]

此控制器操作还返回名为的视图`Details`。 但是，根据相应的操作名称进行推断的视图的名称。 如果你想要测试的视图名称，则必须从控制器操作显式返回视图名称。

可以列出 2 中运行单元测试，通过输入键盘组合**Ctrl R、 A**或通过单击**运行解决方案中的所有测试**按钮 （请参见图 1）。 如果测试通过，你将看到图 2 中的测试结果窗口。


[![在解决方案中运行所有测试](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)

**图 01**： 在解决方案中运行所有测试 ([单击以查看实际尺寸的图像](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))


[![成功 ！](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)

**图 02**： 成功 ！ ([单击以查看实际尺寸的图像](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>测试视图数据返回的控制器

一个 MVC 控制器，将数据传递给视图通过使用称为 *`View Data`* 。 例如，假设你想要显示特定产品的详细信息，在调用时`ProductController Details()`操作。 在这种情况下，你可以创建的实例`Product`类 （在你的模型中定义） 并将传递到实例`Details`视图，通过利用`View Data`。

已修改`ProductController`列出 3 中包括更新`Details()`返回产品的操作。

**列出 3 –`ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample5.cs)]

首先，`Details()`操作创建的新实例`Product`表示便携式计算机的类。 接下来，实例`Product`类作为第二个参数传递`View()`方法。

你可以编写单元测试来测试所需的数据是否包含在视图中的数据。 单元测试中列出 4 测试是否在调用时返回表示一台便携式计算机的产品`ProductController Details()`操作方法。

**列出 4 –`ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample6.cs)]

在列出 4 中，`TestDetailsView()`方法测试通过调用返回的视图数据`Details()`方法。 `ViewData`上作为属性公开`ViewResult`返回通过调用`Details()`方法。 `ViewData.Model`属性包含传递给视图的产品。 测试只需验证视图数据中包含的产品具有便携式计算机的名称。

## <a name="testing-the-action-result-returned-by-a-controller"></a>测试操作结果返回的控制器

更复杂的控制器操作可能会返回不同类型的操作结果具体取决于值传递到的控制器操作的参数。 控制器操作可以返回的类型的操作的结果，包括各种`ViewResult`， `RedirectToRouteResult`，或`JsonResult`。

例如，修改`Details()`列出 5 中的操作返回`Details`查看有效的产品 Id 传递给操作时。 如果传递无效的产品 Id-Id 与一个值小于 1--则您将重定向到`Index()`操作。

**列出 5-`ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample7.cs)]

你可以测试的行为`Details()`列出 6 的单元测试的操作。 列出 6 中的单元测试验证你将重定向到`Index`查看时使用值-1 的 Id 传递给`Details()`方法。

**列出 6-`ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample8.cs)]

当调用`RedirectToAction()`方法的控制器操作中，控制器操作返回`RedirectToRouteResult`。 测试检查是否`RedirectToRouteResult`将用户重定向到名为的控制器操作`Index`。

## <a name="summary"></a>摘要

在本教程中，您学习了如何生成 MVC 控制器操作的单元测试。 首先，您学习了如何验证是否由控制器操作返回与右视图。 你已了解如何使用`ViewResult.ViewName`属性以验证视图的名称。

接下来，我们探讨了如何测试的内容`View Data`。 您学习了如何检查是否在返回适当的产品`View Data`后调用的控制器操作。

最后，我们讨论如何可以测试是否将不同类型的操作结果返回的控制器操作。 你已了解如何测试控制器将返回是否`ViewResult`或`RedirectToRouteResult`。

>[!div class="step-by-step"]
[下一篇](creating-unit-tests-for-asp-net-mvc-applications-vb.md)
