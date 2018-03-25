---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: "执行简单的验证 (C#) |Microsoft 文档"
author: StephenWalther
description: "了解如何在 ASP.NET MVC 应用程序中执行验证。 在本教程中，Stephen Walther 引入你为模型状态和验证 HTML 帮助程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 005872308d9d4d8ac7feb12dd5ab1fc463d0140e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="performing-simple-validation-c"></a>执行简单的验证 (C#)
====================
通过[Stephen Walther](https://github.com/StephenWalther)

> 了解如何在 ASP.NET MVC 应用程序中执行验证。 在本教程中，Stephen Walther 引入你为模型状态和验证 HTML 帮助器。


本教程旨在说明如何执行验证的 ASP.NET MVC 应用程序中。 例如，你将了解如何防止别人提交窗体不包含所需字段的值。 了解如何使用模型状态和验证 HTML 帮助器。

## <a name="understanding-model-state"></a>了解模型状态

使用模型状态-或更准确地说，模型状态字典-来表示验证错误。 例如，列表 1 中的 create （） 操作将产品类添加到数据库之前验证产品类的属性。


我不建议将你验证或数据库的逻辑添加到控制器。 控制器应包含仅与应用程序流控制相关的逻辑。 我们正在进行的快捷方式来为简单起见。


**列表 1-Controllers\ProductController.cs**

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

在列出 1 中，产品类的名称、 说明和 UnitsInStock 属性进行验证。 如果上述任一属性未通过验证测试然后会将错误添加到模型状态字典 （由控制器类的 ModelState 属性表示）。

如果模型状态中有任何错误 ModelState.IsValid 属性返回 false。 在这种情况下，创建新产品的 HTML 窗体将重新显示。 否则，如果不不存在任何验证错误，则新的产品添加到数据库中。

## <a name="using-the-validation-helpers"></a>使用的验证帮助器

ASP.NET MVC framework 包括两个验证帮助器： Html.ValidationMessage() 帮助器和 Html.ValidationSummary() 帮助器。 在视图中使用这些两个帮助器来显示验证错误消息。

通过 ASP.NET MVC 基架自动生成的创建和编辑视图中使用的 Html.ValidationMessage() 和 Html.ValidationSummary() 帮助器。 按照这些步骤生成创建视图：

1. 右键单击产品控制器中的 create （） 操作，然后选择菜单选项**添加视图**（请参见图 1）。
2. 在**添加视图**对话框中，选中复选框标记为**创建强类型化视图**（请参见图 2）。
3. 从**查看数据类**下拉列表中，选择产品类别。
4. 从**查看内容**下拉列表中，选择创建。
5. 单击“添加”按钮。


请确保生成你的应用程序，然后添加视图。 否则，类的列表不会显示在**查看数据类**下拉列表中。


[![新项目对话框](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)

**图 01**： 添加视图 ([单击以查看实际尺寸的图像](performing-simple-validation-cs/_static/image2.png))


[![新项目对话框](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)

**图 02**： 创建强类型化视图 ([单击以查看实际尺寸的图像](performing-simple-validation-cs/_static/image4.png))


完成这些步骤后，你将收到列出 2 中创建视图。

**列出 2-Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

在列出 2 中，Html.ValidationSummary() 帮助器称为立即上方的 HTML 窗体。 此帮助器用于显示验证错误消息的列表。 Html.ValidationSummary() 帮助器呈现项目符号列表中的错误。

每个 HTML 窗体字段旁边称为 Html.ValidationMessage() 帮助器。 此帮助器用于显示一条错误消息的表单域的右边。 对于列出 2 Html.ValidationMessage() 帮助程序错误时显示一个星号。

图 3 中的页说明了在具有缺失字段和无效值提交表单时，验证帮助器所呈现的错误消息。


[![新项目对话框](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)

**图 03**： 提交有问题的 Create view ([单击以查看实际尺寸的图像](performing-simple-validation-cs/_static/image6.png))


请注意的 html 外观输入验证错误时，还会修改字段。 Html.TextBox() 帮助器呈现*类 ="输入验证错误"*与呈现 Html.TextBox() 帮助程序属性关联的验证错误时的特性。

有用于控制验证错误的外观的三个级联样式表类：

- 输入验证错误的应用于&lt;输入&gt;Html.TextBox() 帮助程序呈现的标记。
- 字段验证错误的应用于&lt;跨越&gt;Html.ValidationMessage() 帮助程序呈现的标记。
- 验证摘要错误-应用于&lt;ul&gt; Html.ValidationSumamry() 帮助程序呈现的标记。

可以修改这些级联样式表类，并因此通过修改 Site.css 文件内容的文件夹中修改的验证错误的外观。

> [!NOTE] 
> 
> HtmlHelper 类包括只读的静态属性，检索的验证的名称与相关的 CSS 类。 ValidationInputCssClassName、 ValidationFieldCssClassName 和 ValidationSummaryCssClassName 命名这些静态属性。


## <a name="prebinding-validation-and-postbinding-validation"></a>Prebinding 验证和 Postbinding 验证

如果提交的 HTML 窗体创建产品，并且你输入的无效值 price 字段而没有值 UnitsInStock 字段，你将获得显示在图 4 中的验证消息。 这些验证错误消息来自何处呢？


[![新项目对话框](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)

**图 04**: Prebinding 验证错误 ([单击以查看实际尺寸的图像](performing-simple-validation-cs/_static/image8.png))


有两种类型实际的验证错误消息的这些生成的 HTML 窗体字段绑定到类和这些生成的窗体字段绑定到类之后之前。 换而言之，有 prebinding 验证错误和 postbinding 验证错误。

列表 1 中在产品控制器公开的 create （） 操作接受产品类的实例。 创建方法的签名如下所示：

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

创建窗体中的 HTML 窗体字段的值由称为模型联编程序绑定到 productToCreate 类。 默认的模型联编程序将添加一条错误消息到模型状态自动时它不能将窗体字段绑定到窗体属性。

默认的模型联编程序不能将字符串"apple"绑定到产品类的价格属性。 无法将字符串分配给十进制属性。 因此，模型联编程序将错误添加到模型状态。

此外，默认的模型联编程序不能将 null 值分配给不接受 null 值的属性。 具体而言，模型联编程序不能将 null 值分配给 UnitsInStock 属性。 同样，模型联编程序放弃，并将一条错误消息添加到模型状态。

如果你想要自定义这些的外观 prebinding 错误消息，然后需要创建这些消息的资源字符串。

## <a name="summary"></a>摘要

本教程的目标是验证的介绍用于在 ASP.NET MVC framework 中的基本机制。 您学习了如何使用模型状态和验证 HTML 帮助器。 我们还讨论了 prebinding 和 postbinding 验证之间的区别。 在其他教程中，我们将讨论各种移动你的控制器出来放入模型类验证代码的策略。

>[!div class="step-by-step"]
[上一页](displaying-a-table-of-database-data-cs.md)
[下一页](validating-with-the-idataerrorinfo-interface-cs.md)
