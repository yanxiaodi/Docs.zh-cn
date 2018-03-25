---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: "使用 ASP.NET MVC 的 DropDownList 帮助器 |Microsoft 文档"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 278d04aec68e93f3ebfd12d06a96b59f3bcbef4b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>使用 ASP.NET MVC 的 DropDownList 帮助器
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

本教程将教您使用的基础知识[DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)帮助器和[ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) ASP.NET MVC Web 应用程序中的帮助器。 你可以使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是 Microsoft Visual Studio 中，若要遵循本教程的免费版。 在开始之前，请确保已安装下面列出的先决条件。 你可以通过单击以下链接安装所有这些： [Web 平台安装程序](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，你可以单独安装系统必备组件，使用以下链接：

- [Visual Studio Web Developer Express SP1 系统必备](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)<a id="post"></a>
- [ASP.NET MVC 3 Tools 更新](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（运行时 + 工具支持）

如果你正在使用 Visual Studio 2010，而不 Visual Web Developer 2010，通过单击以下链接安装必备组件： [Visual Studio 2010 系统必备组件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。 本教程假定你已完成[ASP.NET MVC 简介](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)教程或[ASP.NET MVC 音乐商店](../mvc-music-store/mvc-music-store-part-1.md)教程，或者你是熟悉 ASP.NET MVC 开发。 本教程开头已修改的项目，从[ASP.NET MVC 音乐商店](../mvc-music-store/mvc-music-store-part-1.md)教程。 你可以下载初学者项目使用以下链接[下载的 C# 版本](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829)。

具有已完成本教程 C# 源代码的 Visual Web Developer 项目是可以附带本主题。 [下载](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d)。

### <a name="what-youll-build"></a>你将生成

你将创建的操作方法和使用的视图[DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx)帮助器选择的类别。 你还将使用**jQuery**以添加新类别 （如流派或艺术家） 需要时，可以使用插入类别对话框。 下面是显示链接添加新流派并添加新艺术家的创建视图的屏幕快照。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>你将学习的技能

下面是你将要掌握的内容：

- 如何使用[DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx)帮助器来选择类别数据。
- 如何添加**jQuery**对话框以添加新类别。

### <a name="getting-started"></a>入门

通过下载初学者项目使用以下链接来启动[下载](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829)。 在 Windows 资源管理器，右键单击*DDL\_Starter.zip*文件，然后选择属性。 在**DDL\_Starter.zip 属性**对话框中，选择解除锁定。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

右键单击 DDL\_Starter.zip 文件并选择**提取所有**来解压缩该文件。 打开*StartMusicStore.sln* Visual Web Developer 2010 Express （"Visual Web Developer"或简称"VWD"） 或 Visual Studio 2010 的文件。

按 CTRL + F5 运行应用程序并单击**测试**链接。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

选择**选择电影类别 （简单）**链接。 电影类型选择列表是与喜剧选定的值一起显示。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

右键单击浏览器和选择查看源中。 显示的 HTML 页。 下面的代码演示选择的元素的 HTML。

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

你可以看到，选择列表中的每个项有一个值 （0 表示操作、 戏曲的 1、 2 喜剧和科学虚构的 3） 和 （操作、 戏曲、 喜剧和科学虚构） 的显示名称。 上面的代码是标准的 HTML，以便选择列表。

将选择列表更改为戏曲和命中**提交**按钮。 在浏览器中的 URL 是`http://localhost:2468/Home/CategoryChosen?MovieType=1`，页显示**选择： 1**。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

打开*Controllers\HomeController.cs*文件并检查`SelectCategory`方法。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

[DropDownList](https://msdn.microsoft.com/library/dd492738.aspx)帮助器用于创建 HTML 选择列表需要**IEnumerable&lt;SelectListItem &gt;** ，显式或隐式。 也就是说，你可以将传递**IEnumerable&lt;SelectListItem &gt;** 显式为[DropDownList](https://msdn.microsoft.com/library/dd492738.aspx)帮助器也可以添加**IEnumerable&lt;SelectListItem &gt;** 到[ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx)使用相同的名称用于**SelectListItem**作为模型属性。 传入**SelectListItem**本教程的下一部分中介绍了隐式和显式。 上面的代码演示创建可能的最简单方法**IEnumerable&lt;SelectListItem &gt;** 并使用文本和值填充它。 请注意`Comedy` [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)具有[选定](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx)属性设置为**true;**这将导致呈现选择列表，以显示**喜剧**与列表中所选的项目。

**IEnumerable&lt;SelectListItem &gt;** 创建更高版本添加到[ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) MovieType 同名。 这是我们将传递**IEnumerable&lt;SelectListItem &gt;** 隐式为[DropDownList](https://msdn.microsoft.com/library/dd492738.aspx)如下所示的帮助器。

打开*Views\Home\SelectCategory.cshtml*文件并检查标记。

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

在第三个行中，我们将布局设置为视图/共享/\_简单\_Layout.cshtml，这是标准布局文件的简化的版本。 我们执行此操作可使显示器保持打开，并呈现 HTML 简单。

在此示例中我们将不更改状态的应用程序，因此我们将提交数据使用**HTTP GET**，而不**HTTP POST**。 请参阅 W3C 部分[选择 HTTP GET 或 POST 的快速清单](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist)。 由于我们不更改应用程序和发布窗体，因此我们使用[Html.BeginForm](https://msdn.microsoft.com/library/dd460344.aspx)允许我们需要指定的操作方法、 控制器和窗体的方法的重载 (**HTTP POST**或**HTTP GET**)。 视图通常包含[Html.BeginForm](https://msdn.microsoft.com/library/dd505244.aspx)不带参数的重载。 没有参数版本默认为发布到相同的操作方法和控制器的 POST 版本的窗体数据。

下面的行

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

传递的字符串参数**DropDownList**帮助器。 此字符串中，在我们示例中，"MovieType"执行两项操作：

- 它提供的密钥**DropDownList**用于查找帮助**IEnumerable&lt;SelectListItem &gt;** 中**ViewBag**。
- 它被数据绑定到 MovieType 表单元素。 如果提交方法**HTTP GET**，`MovieType`将查询字符串。 如果提交方法**HTTP POST**，`MovieType`将在消息正文中添加。 下图显示值为 1 的查询字符串。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

下面的代码演示`CategoryChosen`窗体提交到的方法。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

向后定位到的测试页中，然后选择**HTML 此时**链接。 HTML 页呈现 select 元素类似于简单的 ASP.NET MVC 测试页。 右键单击浏览器窗口并选择**查看源**。 选择列表的 HTML 标记是实质上是相同的。 测试 HTML 页上，其工作原理类似的 ASP.NET MVC 操作方法和我们以前测试的视图。

### <a name="improving-the-movie-select-list-with-enums"></a>提高电影选择列表中的用枚举

如果你的应用程序中的类别固定的并且将不会更改，你可以利用枚举，以使代码更稳定、 更易于扩展。 在添加新类别时，将生成正确的类别值。 当您添加新类别，但忘了更新的类别值可避免复制和粘贴的错误。

打开*Controllers\HomeController.cs*文件并检查以下代码：

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

[枚举](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx)`eMovieCategories`捕获的四个电影类型。 `SetViewBagMovieType`方法创建**IEnumerable&lt;SelectListItem &gt;** 从`eMovieCategories`**枚举**，并将设置`Selected`从属性`selectedMovie`参数。 `SelectCategoryEnum`操作方法使用同一个视图作为`SelectCategory`操作方法。

导航到测试页并单击`Select Movie Category (Enum)`链接。 这次，而非显示的值 （数字），将显示一个字符串，表示枚举。

### <a name="posting-enum-values"></a>发布枚举值

HTML 窗体通常用于将数据发布到服务器。 下面的代码演示`HTTP GET`和`HTTP POST`版本`SelectCategoryEnumPost`方法。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

通过将传递`eMovieCategories`枚举到`POST`方法，我们可以提取的枚举值和枚举字符串。 运行示例，并导航到的测试页中。 单击`Select Movie Category(Enum Post)`链接。 选择电影类型，然后点击提交按钮。 屏幕将显示值和电影类型的名称。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>创建多个部分选择元素

[ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx)的 HTML 帮助器上呈现 HTML`<select>`具有元素`multiple`属性，它允许用户进行多选。 导航到测试链接，然后选择**多选择国家/地区**链接。 呈现用户界面，可选择多个国家/地区。 在下图中，选择加拿大和中国。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>检查 MultiSelectCountry 代码

检查下面的代码从*Controllers\HomeController.cs*文件。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

`GetCountries`方法将创建一份国家/地区，然后将其传递给`MultiSelectList`构造函数。 `MultiSelectList`中使用的构造函数重载`GetCountries`上面方法采用四个参数：

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *项*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx)包含列表中的项。 在国家/地区列表上方的示例。
2. *dataValueField*： 中的属性的名称**IEnumerable**包含的值的列表。 在上例中，`ID`属性。
3. *dataTextField*： 中的属性的名称**IEnumerable**列表，其中包含要显示的信息。 在上例中，`name`属性。
4. *selectedValues*： 选定值的列表。

在上例中，`MultiSelectCountry`方法传递`null`所选国家/地区的因此移动时显示的用户界面不选择任何国家/地区的值。 下面的代码演示用于呈现的 Razor 标记`MultiSelectCountry`视图。

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

HTML 帮助器[ListBox](https://msdn.microsoft.com/library/dd470200.aspx)方法采用上面两个参数，模型绑定到属性的名称使用与[MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx)包含的选择的选项和值。 `ViewBag.YouSelected`上面代码中用来显示你在提交表单时所选的国家/地区的值。 检查的 HTTP POST 重载`MultiSelectCountry`方法。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

`ViewBag.YouSelected`动态属性包含所选国家/地区，获得的`Countries`窗体集合中的条目。 在此版本 GetCountries 方法传递一份所选国家/地区，因此，在`MultiSelectCountry`显示视图，在 UI 中选择所选国家/地区。

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>进行选择元素友好搜集选 jquery 插件

收集[选](http://harvesthq.github.com/chosen/)jQuery 插件可以添加到 HTML&lt;选择&gt;元素来创建的用户友好的用户界面。 下图演示搜集[选](http://harvesthq.github.com/chosen/)jQuery 插件与`MultiSelectCountry`视图。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

在下面，将两个图像**加拿大**选择。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

在上图中，选择加拿大，并且它包含**x**可以单击要删除所选内容。 下图显示加拿大，中国，并且日本选择。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>挂接搜集选 jQuery 插件

下一节更为轻松地遵循如果你有一些使用 jQuery 的体验。 如果你从未使用过 jQuery 之前，你可能想要尝试以下 jQuery 教程之一。

- [JQuery 的工作原理](http://docs.jquery.com/Tutorials:How_jQuery_Works)通过[John Resig](http://ejohn.org/)
- [开始使用 jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery)通过[Jörn Zaefferer](http://bassistance.de/)
- [实时的 jQuery 示例](http://codylindley.com/blogstuff/js/jquery/#)通过[Cody Lindley](http://codylindley.com/)

选择插件包含在初学者和已完成的示例项目附带本教程。 对于本教程将仅需要使用 jQuery 来将它挂接到 UI。 若要在 ASP.NET MVC 项目中使用搜集选 jQuery 插件，你必须：

1. 下载所选择的插件， [github](https://github.com/harvesthq/chosen/)。 为你完成了此步骤。
2. 将所选文件夹添加到你的 ASP.NET MVC 项目。 将资源添加从你在上一步所选择的文件夹中下载所选择的插件。 为你完成了此步骤。
3. 挂钩到选插件**DropDownList**或**ListBox**的 HTML 帮助器。

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>挂接到 MultiSelectCountry 视图选择插件。

打开*Views\Home\MultiSelectCountry.cshtml*文件并添加`htmlAttributes`参数`Html.ListBox`。 你将添加的参数包含选择列表的类名称 (`@class = "chzn-select"`)。 已完成的代码所示：

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

在上面的代码中，我们要添加的 HTML 特性和特性值`class = "chzn-select"`。 @ 字符中前面的类具有执行任何操作与 Razor 视图引擎。 `class`是[C# 关键字](https://msdn.microsoft.com/library/x53a06bb.aspx)。 C# 关键字无法用作标识符，除非它们有作为前缀。 在上例中，`@class`是有效标识符但**类**不是因为**类**是一个关键字。

将引用添加到*Chosen/chosen.jquery.js*和*Chosen/chosen.css*文件。 *Chosen/chosen.jquery.js*并实现所选择的插件的功能。 *Chosen/chosen.css*文件提供样式。 添加到底部这些引用*Views\Home\MultiSelectCountry.cshtml*文件。 下面的代码演示如何引用所选择的插件。

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

激活使用中使用的类名称选择插件**Html.ListBox**代码。 在上面的示例中，类名是`chzn-select`。 将以下行添加到底部*Views\Home\MultiSelectCountry.cshtml*视图文件。 此行激活所选择的插件。

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

下面的行是用于调用 jQuery 准备函数，后者将选择具有类名称的 DOM 元素的语法`chzn-select`。

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

然后设置从上面的调用返回的已包装的应用所选的方法 (`.chosen();`)，其中挂钩选择插件。

下面的代码演示完成*Views\Home\MultiSelectCountry.cshtml*视图文件。

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

运行应用程序并导航到`MultiSelectCountry`视图。 请尝试添加和删除国家/地区。 提供示例下载还包含`MultiCountryVM`方法和实现使用视图的 MultiSelectCountry 功能的视图模型而不是**ViewBag**。

在下一部分中你将看到的 ASP.NET MVC 基架机制如何使用**DropDownList**帮助器。

>[!div class="step-by-step"]
[下一篇](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
