---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: "使用 ASP.NET MVC-第 2 部分中使用 HTML5 和 jQuery UI Datepicker 弹出日历 |Microsoft 文档"
author: Rick-Anderson
description: "本教程将教您如何使用编辑器模板、 显示模板和 jQuery UI datepicker 弹出日历，ASP.NET MV 中的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 69fbaa7761c97895ffee770f6feb9ce6b745d186
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>使用 ASP.NET MVC-第 2 部分中使用 HTML5 和 jQuery UI Datepicker 弹出日历
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> 本教程将教您如何使用编辑器模板、 显示模板和 jQuery UI datepicker 弹出日历，ASP.NET MVC Web 应用程序中的基础知识。


## <a name="adding-an-automatic-datetime-template"></a>添加自动的 DateTime 模板

在本教程的第一部分，您将了解如何将属性添加到模型后，若要显式指定格式设置，以及如何显式指定用于对模型进行渲染的模板。 例如， [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)中下面的代码显式属性指定的格式设置`ReleaseDate`属性。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

在下面的示例中， [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性，使用`Date`枚举，指定日期模板应该用于对模型进行渲染。 如果你的项目中没有日期的模板，则使用内置日期模板。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

但是，ASP。MVC，可以执行类型匹配通过查找与一种类型的名称匹配的模板来使用约定转移配置。 这允许您创建的模板，自动设置数据格式不使用任何属性或代码在所有情况下。 对于本教程的此部分，你将创建的模板，将自动应用于类型的模型属性[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)。 无需使用属性或其他配置来指定应使用的模板来呈现的类型的所有模型属性[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)。

你还将了解了如何自定义单独的属性或甚至是单个字段的显示。

若要开始，让我们删除现有的格式设置信息，并在应用程序中显示完整的日期。

打开*Movie.cs*文件，并注释掉`DataType`属性`ReleaseDate`属性：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

按 Ctrl+F5 运行应用程序。

请注意，`ReleaseDate`属性现在显示的日期和时间，因为这是默认值时未不提供任何格式设置信息。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>为测试新模板添加 CSS 样式

在创建用于设置日期格式的模板之前，你将添加一些 CSS 样式规则可应用于新的模板。 这些将帮助您验证在呈现的页面使用的新模板。

打开*Content\Site.cs*s 文件并将以下 CSS 规则添加到文件的底部：

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>添加日期时间的显示模板

现在你可以创建新的模板。 在*Views\Movies*文件夹中，创建*DisplayTemplates*文件夹。

在*views/shared*文件夹中，创建*DisplayTemplates*文件夹和*EditorTemplates*文件夹。

中的显示模板*Views\Shared\DisplayTemplates*文件夹将由所有控制器。 中的显示模板*Views\Movie\DisplayTemplates*将仅可由使用文件夹`Movie`控制器。 (如果具有相同名称的模板出现在这两个文件夹中中的模板*Views\Movie\DisplayTemplates*文件夹 — 也就是说，更具体的模板-优先视图返回的`Movie`控制器。)

在**解决方案资源管理器**，展开*视图*文件夹中，展开*共享*文件夹，，然后右键单击*Views\Shared\DisplayTemplates*文件夹。

单击**添加**，然后单击**视图**。 **添加视图**对话框随即显示。

在**视图名称**框中，键入`DateTime`。 （你必须使用此名称以便匹配类型的名称。）

选择**创建为分部视图**复选框。 请确保**使用的布局或 master 页面**和**创建强类型化视图**不选中的复选框。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

单击 **“添加”**。 A *DateTime.cshtml*中创建模板*Views\Shared\DisplayTemplates*。

下图显示*视图*文件夹中的**解决方案资源管理器**后`DateTime`创建显示和编辑器模板。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

打开*Views\Shared\DisplayTemplates\DateTime.cshtml*文件并添加以下标记，使用[String.Format](https://msdn.microsoft.com/library/system.string.format.aspx)方法来设置该属性为无时间日期格式。 (`{0:d}`格式指定短日期格式。)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

重复此步骤以创建`DateTime`中的模板*Views\Movie\DisplayTemplates*文件夹。 使用下面的代码*Views\Movie\DisplayTemplates\DateTime.cshtml*文件。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

`loud-1` CSS 类将导致为红色粗体文本显示的日期。 你添加`loud-1`一样临时度量值，以便你可以轻松地查看此特定模板中使用时的 CSS 类。

已执行哪些操作将创建并自定义 ASP.NET 将用来显示日期的模板。 更多常规模板 (在*Views\Shared\DisplayTemplates*文件夹) 显示简单的短日期。 专用于模板`Movie`控制器 (在*Views\Movies\DisplayTemplates*文件夹) 将也格式化短日期显示为红色粗体文本。

按 Ctrl+F5 运行应用程序。 浏览器呈现应用程序的索引视图。

`ReleaseDate`属性现在以红色粗体无时间中显示的日期。这可帮助你确认`DateTime`中的模板化帮助器*Views\Movies\DisplayTemplates*文件夹所选的转移`DateTime`共享文件夹中的模板化帮助器 (*Views\Shared\DisplayTemplates*)。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

现在重命名*Views\Movies\DisplayTemplates\DateTime.cshtml*文件为*Views\Movies\DisplayTemplates\LoudDateTime.cshtml*。

按 Ctrl+F5 运行应用程序。

这一次`ReleaseDate`属性显示日期时间而不以粗体显示的红色字体。 这表明，具有的数据的名称的模板类型 (在这种情况下`DateTime`) 会自动使用来显示该类型的所有模型属性。 重命名检查完*DateTime.cshtml*文件为*LoudDateTime.cshtml*，找不到 ASP.NET 中的模板*Views\Movies\DisplayTemplates*文件夹，所以只有在使用*DateTime.cshtml*从模板 * Views\Movies\Shared\*文件夹。

（模板匹配是区分大小写，因此你可以使用任何大小写创建模板文件的名称。 例如， *DATETIME.chstml、 datetime.cshtml*，和*DaTeTiMe.cshtml*将所有匹配`DateTime`类型。)

若要查看： 此时，`ReleaseDate`字段显示的是使用*Views\Movies\DisplayTemplates\DateTime.cshtml*模板，但显示的数据使用短日期格式，否则将添加没有特殊格式。

### <a name="using-uihint-to-specify-a-display-template"></a>使用 UIHint 若要指定显示模板

如果 web 应用程序的许多`DateTime`字段并且默认情况下你想要仅日期格式显示全部或大部分它们*DateTime.cshtml*模板是一种好方法。 但是，如果您有几个日期想要显示的完整的日期和时间？ 没问题。 你可以创建其他模板，并使用[UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)特性以指定完整的日期和时间格式设置。 然后，你可以有选择地应用该模板。 你可以使用[UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)在模型级别或你的特性可以指定视图内的模板。 在此部分中，你将了解如何使用`UIHint`特性有选择地更改某些情况下的日期时间字段的格式。

打开*Views\Movies\DisplayTemplates\LoudDateTime.cshtml*文件并将现有代码替换为以下：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

这会导致要显示的完整的日期和时间，并将添加 CSS 类，使该文本，绿色和大型。

打开*Movie.cs*文件并添加[UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)属性设为`ReleaseDate`属性，如下面的示例中所示：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

这将告知 ASP.NET MVC，在显示时`ReleaseDate`属性 (具体而言，并不只是任何`DateTime`对象)，它应使用*LoudDateTime.cshtml*模板。

按 Ctrl+F5 运行应用程序。

请注意，`ReleaseDate`属性现在显示的日期和时间，大的绿色字体。

返回到`UIHint`属性中*Movie.cs*文件，并注释掉因此*LoudDateTime.cshtml*不会使用模板。 再次运行该应用程序。 发布日期不会显示大型和绿色。 此验证*Views\Shared\DisplayTemplates\DateTime.cshtml*索引和详细信息视图中使用模板。

如前所述，你还可以应用在视图中，这样就可以将模板应用到一个单独的实例的某些数据模板。 打开*Views\Movies\Details.cshtml*视图。 添加`"LoudDateTime"`的第二个参数[Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx)寻求`ReleaseDate`字段。 完整代码如下所示：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

此步骤指定`LoudDateTime`应使用模板来显示模型属性，而不考虑哪些属性应用于模型。

按 Ctrl+F5 运行应用程序。

验证是否正在使用影片索引页*Views\Shared\DisplayTemplates\DateTime.cshtml*模板 （红色粗体） 和*Movie\Details*网页使用的*Views\Movies\DisplayTemplates\LoudDateTime.cshtml*模板 （大型和绿色）。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

在下一步的部分中，你将创建用于复杂类型的模板。

>[!div class="step-by-step"]
[上一页](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
[下一页](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
