---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: "使用 ASP.NET MVC-第 3 部分中使用 HTML5 和 jQuery UI Datepicker 弹出日历 |Microsoft 文档"
author: Rick-Anderson
description: "本教程将教您如何使用编辑器模板、 显示模板和 jQuery UI datepicker 弹出日历，ASP.NET MV 中的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: 7d4ed67254c2b0fc2aef748cfab1c8f628b25641
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>使用 ASP.NET MVC-第 3 部分中使用 HTML5 和 jQuery UI Datepicker 弹出日历
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> 本教程将教您如何使用编辑器模板、 显示模板和 jQuery UI datepicker 弹出日历，ASP.NET MVC Web 应用程序中的基础知识。


## <a name="working-with-complex-types"></a>使用复杂类型

在本节中你将创建一个地址类，并了解如何创建模板，以显示它。

在*模型*文件夹中，创建名为的新类文件*Person.cs*将放置两种类型的位置：`Person`类和`Address`类。 `Person`类将包含一个属性，被类型化为`Address`。 `Address`类型是复杂类型，这意味着不内置类型，如之一`int`， `string`，或`double`。 相反，它具有多个属性。 新的类的代码如下所示：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

在`Movie`控制器上，添加以下`PersonDetail`操作以显示 person 实例：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

然后添加以下代码到`Movie`控制器来填充`Person`用一些示例数据模型：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

打开*Views\Movies\PersonDetail.cshtml*文件并添加以下标记`PersonDetail`视图。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

按 Ctrl + F5 运行应用程序并导航到*电影/PersonDetail*。

`PersonDetail`视图不包含`Address`复杂类型，可以看出，在此屏幕截图。 （不显示为任何地址。）

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

`Address`模型数据将不显示，因为它是一个复杂类型。 若要显示的地址信息，请打开*Views\Movies\PersonDetail.cshtml*再次文件并添加以下标记。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

完整标记`PersonDetail`现在视图如下所示：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

运行一次，应用程序并显示`PersonDetail`视图。 现在显示的地址信息：

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>创建用于复杂类型的模板

在本部分中，你将创建将用于呈现模板`Address`复杂类型。 当你创建的模板`Address`类型，ASP.NET MVC 可以自动使用它来设置应用程序中的任意位置的地址模型的格式。 这使你能够控制的呈现`Address`从一个地方即可应用程序中的类型。

在*Views\Shared\DisplayTemplates*文件夹中，创建名为强类型的分部视图**地址**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

单击**添加**，然后打开新*Views\Shared\DisplayTemplates\Address.cshtml*文件。 新视图包含以下生成的标记：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

运行应用程序并显示`PersonDetail`视图。 此时，`Address`刚创建的模板用于显示`Address`复杂类型，以便显示如下所示：

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>摘要： 方式可以指定的模型显示格式和模板

你已了解你可以通过使用以下方法指定的格式或模型属性的模板：

- 应用`DisplayFormat`属性设为模型中的属性。 例如，下面的代码会导致没有时间的情况下显示的日期：

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- 应用[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性中的模型和指定的数据类型的属性。 例如，下面的代码会导致没有时间的情况下显示的日期。

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    如果应用程序包含*date.cshtml*中的模板*Views\Shared\DisplayTemplates*文件夹或*Views\Movies\DisplayTemplates*文件夹中，该模板将用于呈现`DateTime`属性。 否则内置 ASP.NET 模板化系统将显示为日期属性。
- 创建中的显示模板*Views\Shared\DisplayTemplates*文件夹或*Views\Movies\DisplayTemplates*其名称与你想要设置格式的数据类型相匹配的文件夹。 例如，您会看到*Views\Shared\DisplayTemplates\DateTime.cshtml*用于呈现`DateTime`模型，而无需添加到模型的属性，而无需任何标记添加到视图中的属性。
- 使用[UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)模型上要指定要显示的模型属性的模板的属性。
- 显式添加到的显示模板名称[Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx)调用在视图中。

你使用的方法取决于你需要在你的应用程序中执行操作。 并不少见混合使用这些方法以获取恰恰格式设置所需。

在下一步的部分中，你将切换项有点内容，将从自定义如何到自定义输入方式显示的数据移动。 您将挂钩到应用程序，以便提供指定日期的巧妙方法中的编辑视图 jQuery 包含 datepicker。

>[!div class="step-by-step"]
[上一页](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[下一页](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
