---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: "使用 ASP.NET MVC-第 1 部分中使用 HTML5 和 jQuery UI Datepicker 弹出日历 |Microsoft 文档"
author: Rick-Anderson
description: "本教程将教您如何使用编辑器模板、 显示模板和 jQuery UI datepicker 弹出日历，ASP.NET MV 中的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: 4b5507021af47d96c29809c9830d0558f5501f87
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>使用 ASP.NET MVC-第 1 部分中使用 HTML5 和 jQuery UI Datepicker 弹出日历
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> 本教程将教您如何使用编辑器模板、 显示模板和 jQuery UI datepicker 弹出日历，ASP.NET MVC Web 应用程序中的基础知识。


本教程将教您如何使用编辑器模板、 显示模板和 jQuery 的基础知识[UI datepicker 弹出日历](http://plugins.jquery.com/project/datepicker)ASP.NET MVC Web 应用程序中。 对于本教程中，你可以使用 Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer&quot;)，这是 Microsoft Visual Studio 的免费版，或者如果你已具有，则可以使用 Visual Studio 2010 SP1。

在开始之前，请确保已安装下面列出的先决条件。 你可以通过单击以下链接安装所有这些： [Web 平台安装程序](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以单独安装所需的软件，使用以下链接：

- [Visual Studio Web Developer Express SP1 系统必备](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools 更新](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（运行时 + 工具支持）

如果你使用 Visual Studio 2010 而不 Visual Web Developer 中，通过单击以下链接安装必备组件： [Visual Studio 2010 系统必备组件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。

本教程假定你已完成[Getting Started with MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)教程或你熟悉 ASP.NET MVC 开发。 本教程开头中完成的项目[Getting Started with MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)教程。

本教程演示 C# 中的代码。 但是，[初学者项目](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800)和已完成的项目也会出现在 Visual Basic。

与 C# 和 Visual Basic 源代码的 Visual Studio 项目是用本主题可以附带：[下载](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800)。

### <a name="what-youll-build"></a>你将生成

你将添加模板 （具体而言，编辑和显示模板） 的简单的影片列表应用程序中创建到[Getting Started with MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)教程。 你还将添加[jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)弹出日历，以简化输入日期的过程。 下面的屏幕截图使用显示 jQuery UI datepicker 弹出日历显示修改后的应用程序。

![完成的 jQuery](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>你将学习的技能

下面是你将要掌握的内容：

- 如何使用中的属性[DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空间以显示时会被控制数据的格式和处于编辑模式。
- 如何创建模板 （编辑和显示模板） 来控制数据的格式。
- 如何添加[jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)作为输入日期字段的方法。

### <a name="getting-started"></a>入门

如果你还没有从的初学者项目的影片列表应用，将其使用以下链接下载：[下载](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800)。 然后在 Windows 资源管理器，右键单击*MvcMovie.zip*文件，然后选择**属性**。 在**MvcMovie.zip 属性**对话框中，选择**解除阻止**。 (取消阻止一条安全警告，当你尝试使用*.zip*已从 web 下载的文件。)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

右键单击*MvcMovie.zip*文件，然后选择**提取所有**来解压缩该文件。 在 Visual Web Developer 或 Visual Studio 2010 中，打开*MvcMovieCS\_TU.sln*文件。

在**解决方案资源管理器**，双击*views/shared\\_Layout.cshtml*以将其打开。 更改`H1`标头**MVC 影片应用程序**到**电影 jQuery**。 按 CTRL + F5 运行应用程序并单击**主页**选项卡上，将跳转到`Index`电影控制器方法。 若要尝试应用程序，选择**编辑**链接和**详细信息**电影之一的链接。 请注意，在中的索引、 编辑和详细信息视图中，发布日期和价格的很好的名称格式：

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

日期和价格的格式是使用的结果[DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)上的属性的属性`Movie`类。

打开*Movie.cs*文件，并注释掉`DisplayFormat`属性`ReleaseDate`和`Price`属性。 生成`Movie`类类似如下所示：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

按 CTRL + F5 以运行应用程序并选择**主页**选项卡以查看影片列表。 这一次发布日期显示的日期和时间，并价格字段不再显示的货币符号。 在你更改`Movie`类撤消了 nice 格式更早版本，看到但来修复此问题在一段时间。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>使用 DataAnnotations 数据类型属性以指定数据类型

将注释掉`DisplayFormat`属性，则为`ReleaseDate`具有属性[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性，使用`Date`枚举。 替换`DisplayFormat`属性，则为`Price`具有属性[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)再次，属性这一次使用`Currency`枚举。 这是完整的代码如下所示：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

运行该应用程序。 现在发布日期和价格属性的格式设置正确 （即使用相应的日期和货币格式）。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性提供类型元数据的内置的 ASP.NET MVC 模板，以便以正确的格式呈现字段。 使用`DataType`属性优于使用`DisplayFormat`最初是在代码中，因为属性`DataType`特性使模型更简洁且更灵活的目的如国际化。

在下一部分中，你将看到如何使自定义模板，以显示日期字段。

>[!div class="step-by-step"]
[下一篇](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
