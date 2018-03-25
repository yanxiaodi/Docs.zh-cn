---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: "使用 ViewData 和实现 ViewModel 类 |Microsoft 文档"
author: microsoft
description: "步骤 6 显示如何启用对更丰富的形式编辑方案中的支持还介绍了可以用于将数据从控制器传递到视图的两种方法:..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 36b9e87cc24f74f7f2cc592afb5102709b598f74
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>使用 ViewData 和实现 ViewModel 类
====================
通过[Microsoft](https://github.com/microsoft)

[下载 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 这是一个免费的第 6 步["NerdDinner"应用程序教程](introducing-the-nerddinner-tutorial.md)，查找步程取如何生成一个较小，但完成，使用 ASP.NET MVC 1 web 应用程序。
> 
> 步骤 6 显示如何启用对更丰富的形式编辑方案中的支持还介绍了可以用于将数据从控制器传递到视图的两种方法： ViewData 和视图模型。
> 
> 如果你使用的 ASP.NET MVC 3，我们建议你遵循[获取启动使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC 音乐商店](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教程。


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner 步骤 6: ViewData 和视图模型

我们已介绍大量的窗体 post 方案，并讨论了如何实现创建、 更新和删除 (CRUD) 支持。 现在，我们将进一步采用我们 DinnersController 实现，并启用对更丰富的形式编辑方案的支持。 在完成此任务中，我们将讨论这两种方法可以用于将数据从控制器传递到视图： ViewData 和视图模型。

### <a name="passing-data-from-controllers-to-view-templates"></a>将从控制器的数据传递给视图模板

MVC 模式的定义特征之一是指严格"隔离的问题"它有助于强制应用程序在不同组件之间。 模型、 控制器和视图每个很好地定义角色和职责，并且它们之间相互进行通信以定义完善的方式。 这有助于提升可测试性和代码重用。

当控制器类决定呈现 HTML 响应回客户端时，它负责显式将传递到的所有数据所需呈现响应查看模板。 查看模板应永远不会执行任何数据检索或应用程序逻辑 –，并应改为将其本身为只包含从传递给它由控制器模型/数据驱动的呈现代码限制。

稍后再试模型数据按我们 DinnersController 传递到我们查看模板的类很简单，直截了当的 – 的 index （），对于 Dinner 对象的列表和单个 Dinner 对象对于 Details()、 edit （）、 create （） 和 delete （）。 我们将更多的用户界面功能添加到我们的应用程序时，我们通常将需要传递多个仅呈现在我们的视图模板中的 HTML 响应此数据。 例如，我们可能想要修改我们编辑中的"国家/地区"字段，从而防止到 dropdownlist HTML 文本框中创建视图。 而不是硬编码的视图模板中的国家/地区名称的下拉列表，我们可能想要生成从我们动态填充的受支持国家/地区的列表。 我们将需要一种方法将 Dinner 对象传递*和*支持国家/地区从我们控制器到我们查看模板的列表。

让我们看一下我们可以实现此目的的两种方法。

### <a name="using-the-viewdata-dictionary"></a>使用 ViewData 字典

控制器基该类会公开一个"ViewData"字典属性，可以用于将从控制器的其他数据项传递到视图。

例如，若要支持的方案，我们想要从正在到 dropdownlist HTML 文本框中更改"国家/地区"文本框中，我们编辑视图中的，我们可以更新我们 edit （） 的操作方法中将可以用作 m 此时对象传递 （除了一个 Dinner 对象）国家/地区 dropdownlist odel。

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

上述此时的构造函数将接受所在国家/地区来填充，放 downlist，以及当前选择的值的列表。

然后，我们可以更新我们 Edit.aspx 视图模板，而不是我们以前使用的 Html.TextBox() 帮助器方法使用 Html.DropDownList() 帮助器方法：

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

上面的 Html.DropDownList() 帮助程序方法采用两个参数。 第一个是要输出的 HTML 窗体元素的名称。 第二个是我们通过 ViewData 字典传递"此时"模型。 我们将使用 C#"as"关键字来强制转换为此时字典中的类型。

现在当我们运行我们的应用程序和访问时和*/Dinners/Edit/1* URL 我们浏览器中我们将看到我们编辑 UI 更新以显示国家/地区，而不是文本框中的说明：

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

因为我们也呈现编辑视图模板从 HTTP POST 编辑方法 （在方案中发生错误时），我们将需要确保我们还更新此方法可添加到 ViewData 此时，在错误情况中呈现的视图模板时：

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

并且，我们 DinnersController 的编辑方案现在支持 DropDownList。

### <a name="using-a-viewmodel-pattern"></a>使用 ViewModel 模式

ViewData 字典方法具有一个优势是相当快速且轻松地实现。 一些开发人员不喜欢使用基于字符串的字典，不过，由于拼写错误可能导致在编译时将不会捕获的错误。 非类型化的 ViewData 字典还需要使用"为"运算符或强制转换时使用 C# 之类视图模板中的强类型语言。

我们可以使用一种方法是一个通常称为"ViewModel"模式。 使用此模式时我们将创建强类型的类对于我们的特定视图方案，进行优化的和其公开的动态值/所需内容由我们视图模板的属性。 然后，我们控制器类可以填充，并将这些视图优化类传递给我们要使用的视图模板。 这样，类型安全，编译时检查，和编辑器 intellisense 中查看模板。

例如，若要启用 dinner 窗体类似下面的编辑方案我们可以创建"DinnerFormViewModel"类公开两个强类型属性： Dinner 对象和所需填充国家/地区的 dropdownlist 此时模型：

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

我们可以然后更新要创建使用从我们的存储库，我们检索 Dinner 对象 DinnerFormViewModel 我们 edit （） 操作方法，并将其传递给我们的视图模板：

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

我们将然后更新我们视图模板，因此它需要"DinnerFormViewModel"而不是"Dinner"对象通过更改 edit.aspx 页顶部的"继承"属性如下所示：

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

一旦我们执行此操作，我们视图模板中的"模型"属性的 intellisense 将更新以反映我们传递的 DinnerFormViewModel 类型的对象模型：

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

然后，我们可以更新我们查看代码，以便关闭它。 请注意如何我们未更改的输入元素的名称下面，我们将创建 （窗体元素将仍被命名为"标题"、"国家/地区"） – 但我们正在更新的 HTML 帮助器方法来检索使用 DinnerFormViewModel 类的值：

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

我们将更新时要使用 DinnerFormViewModel 类的呈现错误我们编辑 post 方法：

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

我们还可以更新我们的 create （） 的操作方法以重复使用的确切相同*DinnerFormViewModel*类启用的国家/地区的 DropDownList 中那些以及。 下面是 HTTP GET 实现：

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

下面是创建 HTTP POST 方法的实现：

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

和我们编辑和创建屏幕现在支持针对选取国家/地区的方法是下拉列表。

### <a name="custom-shaped-viewmodel-classes"></a>自定义形状 ViewModel 类

在上述方案中，我们 DinnerFormViewModel 类直接作为属性，以及支持此时模型属性公开 Dinner 模型对象。 此方法很好地适用于其中我们要在我们的视图模板创建的 HTML UI 方法相对较接近于我们域模型对象的方案。

对于方案这不是这种情况，你可以使用的一个选项是创建一个自定义形状的 ViewModel 类更视图 – 将其对象模型优化为消耗和可能看起来完全不同于基础的域模型对象。 例如，它可能无法公开不同的属性名称和/或从多个模型对象收集的聚合属性。

可以自定义形状 ViewModel 类用于同时将数据从控制器传递到呈现，以及以帮助处理回发到控制器的操作方法的窗体数据的视图。 对于此更高版本的情况下，你可能必须使用窗体发送数据，更新 ViewModel 对象，然后使用 ViewModel 实例映射或检索实际域模型对象的操作方法。

自定义形状 ViewModel 类可以提供极大的灵活性，并需要调查任何时间在你开始获取过于复杂的操作方法中的视图模板或窗体发布代码中查找呈现代码。 这通常是注册你的域模型不完全对应于你要生成的 UI 和中间的自定义形状 ViewModel 类可帮助。

### <a name="next-step"></a>下一步

让我们现在看一下我们可以如何使用它们和主控页重复使用并在我们的应用程序之间共享 UI。

>[!div class="step-by-step"]
[上一页](provide-crud-create-read-update-delete-data-form-entry-support.md)
[下一页](re-use-ui-using-master-pages-and-partials.md)
