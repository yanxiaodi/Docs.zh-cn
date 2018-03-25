---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: "使用 ASP.NET MVC-第 4 部分中使用 HTML5 和 jQuery UI Datepicker 弹出日历 |Microsoft 文档"
author: Rick-Anderson
description: "本教程将教您如何使用编辑器模板、 显示模板和 jQuery UI datepicker 弹出日历，ASP.NET MV 中的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 32211465adeb1353908daa1014d188b84389e1a7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>使用 ASP.NET MVC-第 4 部分中使用 HTML5 和 jQuery UI Datepicker 弹出日历
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> 本教程将教您如何使用编辑器模板、 显示模板和 jQuery UI datepicker 弹出日历，ASP.NET MVC Web 应用程序中的基础知识。


### <a name="adding-a-template-for-editing-dates"></a>用于编辑日期添加模板

在本部分中，你将创建用于编辑日期的 ASP.NET MVC UI 显示用于编辑标记有的模型属性时将应用的模板**日期**的枚举[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性。 该模板将呈现仅显示日期;不会显示时间。 在模板中，你将使用[jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/)弹出日历，以提供一种方法编辑日期。

若要开始，打开*Movie.cs*文件并添加[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)特性与**日期**枚举`ReleaseDate`属性，如下面的代码中所示：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

此代码会导致`ReleaseDate`中的时间显示模板和编辑模板没有要显示的字段。 如果你的应用程序包含*date.cshtml*中的模板*Views\Shared\EditorTemplates*文件夹中或在*Views\Movies\EditorTemplates*文件夹中，该模板将用于呈现任何`DateTime`编辑时的属性。 否则内置 ASP.NET 模板化系统将显示为日期属性。

按 Ctrl+F5 运行应用程序。 选择的编辑链接，以验证发布日期的输入的字段的显示仅显示日期。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

在**解决方案资源管理器**，展开*视图*文件夹中，展开*共享*文件夹，，然后右键单击*Views\Shared\EditorTemplates*文件夹。

单击**添加**，然后单击**视图**。 **添加视图**对话框随即显示。

在**视图名称**框中，键入&quot;日期&quot;。

选择**创建为分部视图**复选框。 请确保**使用的布局或 master 页面**和**创建强类型化视图**不选中的复选框。

单击 **“添加”**。 *Views\Shared\EditorTemplates\Date.cshtml*创建模板。

以下代码添加到*Views\Shared\EditorTemplates\Date.cshtml*模板。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

第一行声明模型要`DateTime`类型。 尽管你不需要声明中编辑的模型类型和显示模板，但它是一种最佳做法，以便你可以获得编译时检查传递给视图的模型。 （另一个好处是您会收到 IntelliSense 对于 Visual Studio 中的视图中的模型。）ASP.NET MVC 如果未声明的模型类型，将其视为[动态](https://msdn.microsoft.com/library/dd264741.aspx)键入并且没有任何编译时类型检查。 如果你声明模型要`DateTime`类型，它将成为强类型。

第二行是仅文本显示的 HTML 标记&quot;使用日期模板&quot;之前的日期字段。 你将暂时使用此行以验证正在使用此日期模板。

下一行是[Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx)呈现的帮助器`input`字段的文本框。 帮助器的第三个参数使用一个匿名类型设置的文本设置为的类`datefield`和类型设置为`date`。 (因为`class`是保留在 C# 中，你需要使用`@`字符进行转义`class`C# 分析器中的属性。)

`date`类型是一个 HTML5 输入的类型，使感知 HTML5 浏览器呈现 HTML5 日历控件。 稍后你将添加一些 JavaScript 挂接到 jQuery datepicker`Html.TextBox`元素使用`datefield`类。

按 Ctrl+F5 运行应用程序。 你可以验证`ReleaseDate`编辑视图中的属性使用编辑模板，因为该模板显示&quot;使用日期模板&quot;之前`ReleaseDate`文本输入框中，此图中所示：

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

在浏览器中，查看的页面的源。 (例如，右击该页并选择**查看源**。)以下示例显示了一些页中，标记来说明`class`和`type`呈现的 HTML 中的属性。

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

返回到*Views\Shared\EditorTemplates\Date.cshtml*模板和删除&quot;使用日期模板&quot;标记。 现在已完成的模板类似如下所示：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>添加 jQuery UI Datepicker 弹出日历使用 NuGet

在本部分将添加[jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)弹出日历，日期编辑模板。 [JQuery UI](http://jqueryui.com/)库为动画，高级效果，以及可自定义小组件提供了支持。 它是基础的 jQuery JavaScript 库生成的。 包含 datepicker 弹出日历可以简单而自然以输入日期而不输入字符串中使用的日历。 弹出日历还限制到合法日期的用户-针对日期的普通文本条目会让你输入类似`2/33/1999`(年 2 月 33rd、 1999年)，但[jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)弹出日历不允许的。

首先，你必须安装 jQuery UI 库。 若要这样做，将使用 NuGet，是的 Visual Studio 2010 和 Visual Web Developer 的 SP1 版本中包括的包管理器。

在 Visual Web Developer 中，从**工具**菜单上，选择**库程序包管理器**，然后选择**管理 NuGet 包**。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

注意： 如果**工具**菜单中未显示**库程序包管理器**命令时，你需要按照的说明安装 NuGet[安装 NuGet](http://docs.nuget.org/docs/start-here/installing-nuget)页NuGet 网站。   
  
如果你使用 Visual Studio 而不是 Visual Web Developer 中，从**工具**菜单上，选择**库程序包管理器**，然后选择**添加库程序包引用**。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

在**MVCMovie-管理 NuGet 包**对话框中，单击**联机**在左侧选项卡，然后输入&quot;jQuery.UI&quot;的搜索框中。 选择 j**查询 UI 小组件： Datepicker**，然后选择**安装**按钮。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet 将这些调试版本和缩减的版本的 jQuery UI 核心和 jQuery UI 日期选取器添加到你的项目中：

- *jquery.ui.core.js*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.js*
- *jquery.ui.datepicker.min.js*

注意： 的调试版本 (不包含的文件*。 min.js*扩展) 可用于调试，但在生产站点，你会包括仅缩减的版本。

若要实际使用 jQuery 日期选取器，你需要创建将挂钩到编辑模板日历小组件的 jQuery 脚本。 在**解决方案资源管理器**，右键单击*脚本*文件夹，然后选择**添加**，然后**新项**，，然后**JScript文件**。 命名该文件*DatePickerReady.js*。

以下代码添加到*DatePickerReady.js*文件：

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

如果你不熟悉 jQuery，下面是这一功能的简要说明： 第一行是&quot;jQuery 准备&quot;函数，它在已加载页面中的所有 DOM 元素时调用。 第二行选择具有的类名称的所有 DOM 元素`datefield`，然后调用`datepicker`为每个函数。 (请记住你添加`datefield`类到*Views\Shared\EditorTemplates\Date.cshtml*在本教程前面的模板。)

接下来，打开*views/shared\\_Layout.cshtml*文件。 你需要添加对以下文件，以便你可以使用日期选取器将所有所需的引用：

- *Content/themes/base/jquery.ui.core.css*
- *Content/themes/base/jquery.ui.datepicker.css*
- *Content/themes/base/jquery.ui.theme.css*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.min.js*
- *DatePickerReady.js*

下面的示例演示实际代码，你应添加在底部`head`中的元素*views/shared\\_Layout.cshtml*文件。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

完整`head`部分如下所示：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

[URL 内容帮助器](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx)方法将资源路径转换为绝对路径。 必须使用`@URL.Content`正确引用这些资源，在 IIS 上运行应用程序时。

按 Ctrl+F5 运行应用程序。 选择的编辑链接，则将放到插入点**ReleaseDate**字段。 将显示 jQuery UI 弹出日历。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

大多数 jQuery 控件，如 datepicker 可以广泛地自定义。 有关信息，请参阅[Visual 自定义项： 设计 jQuery UI 主题](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme)上[jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/)站点。

### <a name="supporting-the-html5-date-input-control"></a>支持 HTML5 日期输入的控件

因为多个浏览器支持 HTML5，你将想要使用输入的信息，如本机 HTML5`date`输入元素，并使用 jQuery UI 日历。 可以将逻辑添加到你的应用程序，如果浏览器支持它们自动使用 HTML5 控件。 若要执行此操作，将内容*DatePickerReady.js*替换为以下文件：

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

此脚本的第一行使用 Modernizr 检验支持 HTML5 日期输入。 如果不支持，jQuery UI 日期选取器已改为挂钩。 ([Modernizr](http://www.modernizr.com/docs/)是检测到的可用性的 HTML5 和 CSS3 的本机实现一个开放源代码 JavaScript 库。 Modernizr 中包括你创建的任何新的 ASP.NET MVC 项目。）

已进行此更改后，可以通过使用支持 HTML5，如 Opera 11 的浏览器对其进行测试。 运行使用 HTML5 兼容浏览器的应用程序和编辑电影条目。 HTML5 日期控件代替 jQuery UI 弹出日历：

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

因为的浏览器的新版本以增量方式实现 HTML5，现在一个好方法是将代码添加到你还包含许多丰富的 HTML5 支持的网站。 例如，更可靠*DatePickerReady.js*脚本如下所示，从而使你站点支持的浏览器仅部分支持 HTML5 日期控件。

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

此脚本选择 HTML5`input`类型的元素`date`不完全支持 HTML5 日期控件。 对于这些元素，它挂钩 jQuery UI 弹出日历，然后更改`type`属性从`date`到`text`。 通过更改`type`属性从`date`到`text`，消除部分 HTML5 日期支持。 甚至更可靠*DatePickerReady.js*处找不到脚本[JSFIDDLE](http://jsfiddle.net/XSTK8/15/)。

### <a name="adding-nullable-dates-to-the-templates"></a>将为 Null 的日期添加到模板

如果你使用其中一个现有的日期模板并将传递 null 日期时，你将获得运行时错误。 若要使日期模板更可靠，你将更改它们以处理 null 值。 若要支持可以为 null 的日期，更改中的代码*Views\Shared\DisplayTemplates\DateTime.cshtml*所示：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

代码模型时也会返回空字符串**null**。

更改中的代码*Views\Shared\EditorTemplates\Date.cshtml*到以下文件：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

当此代码运行时，如果模型不为 null，该模型的`DateTime`使用值。 如果模型为 null，则改用当前的日期。

### <a name="wrapup"></a>便捷

本教程已覆盖 ASP.NET 模板化帮助器的基础知识，并向你显示如何在 ASP.NET MVC 应用程序中使用 jQuery UI datepicker 弹出日历。 有关详细信息，请尝试这些资源：

- 本地化的信息，请参阅 Rajeesh 的博客[ASP.NET mvc JQueryUI Datepicker](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/)。
- 有关 jQuery UI 的信息，请参阅[jQuery UI](http://docs.jquery.com/UI)。
- 有关如何本地化 datepicker 控件的信息，请参阅[UI/Datepicker/本地化](http://docs.jquery.com/UI/Datepicker/Localization)。
- 有关 ASP.NET MVC 模板的详细信息，请参阅 Brad wilson 制作的博客连载文章上[ASP.NET MVC 2 模板](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)。 序列为 ASP.NET MVC 2 编写的虽然材料将仍适用于当前版本的 ASP.NET MVC。

>[!div class="step-by-step"]
[上一篇](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
