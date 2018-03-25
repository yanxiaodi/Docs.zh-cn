---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
title: "检查编辑方法和编辑视图 (C#) |Microsoft 文档"
author: Rick-Anderson
description: "本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是一个 ASP.NET MVC Web 应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 1d266bf0-a61e-423b-a3d2-13773d7dafe2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 137254be69344f31e65e1b4d1318a107ff9d6d47
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="examining-the-edit-methods-and-edit-view-c"></a>检查编辑方法和编辑视图 (C#)
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教程的更新的版本可[此处](../../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全，请按照要简单得多，并演示更多的功能。
> 
> 
> 本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是 Microsoft Visual Studio 的免费版本的 ASP.NET MVC Web 应用程序的基础知识。 在开始之前，请确保已安装下面列出的先决条件。 你可以通过单击以下链接安装所有这些： [Web 平台安装程序](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，你可以单独安装系统必备组件，使用以下链接：
> 
> - [Visual Studio Web Developer Express SP1 系统必备](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools 更新](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（运行时 + 工具支持）
> 
> 如果你正在使用 Visual Studio 2010，而不 Visual Web Developer 2010，通过单击以下链接安装必备组件： [Visual Studio 2010 系统必备组件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用 C# 源代码的 Visual Web Developer 项目是可以附带本主题。 [下载的 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果你愿意 Visual Basic，切换到[Visual Basic 版本](../vb/intro-to-aspnet-mvc-3.md)本教程。


在此部分中，你将检查生成的操作方法和电影控制器的视图。 然后你将添加自定义搜索页。

运行应用程序，并浏览到`Movies`控制器通过追加*/Movies*到你的浏览器的地址栏中的 URL。 将鼠标指针停留在**编辑**链接以查看链接到的 URL。

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image2.png)](examining-the-edit-methods-and-edit-view/_static/image1.png)

**编辑**链接通过生成`Html.ActionLink`中的方法*Views\Movies\Index.cshtml*视图：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image4.png)](examining-the-edit-methods-and-edit-view/_static/image3.png)

`Html`对象是使用上的属性公开一个帮助程序`WebViewPage`基类。 `ActionLink`的帮助器方法，可以轻松地动态生成 HTML 的超链接，链接到控制器上的操作方法。 第一个参数`ActionLink`方法是要呈现的链接文本 (例如， `<a>Edit Me</a>`)。 第二个参数是要调用的操作方法的名称。 最后一个参数是[匿名对象](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx)，用来生成路由数据 （在本例中为 4 的 ID）。

前面的图像中所示的生成的链接是`http://localhost:xxxxx/Movies/Edit/4`。 默认的路由采用的 URL 模式`{controller}/{action}/{id}`。 因此，ASP.NET 将转换`http://localhost:xxxxx/Movies/Edit/4`放置到请求`Edit`操作方法`Movies`与参数的控制器`ID`等于 4。

你还可以传递使用查询字符串的操作方法参数。 例如，URL`http://localhost:xxxxx/Movies/Edit?ID=4`还传递了参数`ID`4 至`Edit`操作方法`Movies`控制器。

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image6.png)](examining-the-edit-methods-and-edit-view/_static/image5.png)

打开`Movies`控制器。 这两个`Edit`操作方法如下所示。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

请注意第二个 `Edit` 操作方法的前面是 `HttpPost` 特性。 此属性指定的该重载`Edit`可以仅针对的 POST 请求调用方法。 你可以应用`HttpGet`属性设置为第一个编辑方法，但这它们是不必要，因为它是默认值。 (我们在指隐式分配的操作方法`HttpGet`属性为`HttpGet`方法。)

`HttpGet` `Edit`方法采用电影 ID 参数，以查找使用实体框架电影`Find`方法，并将所选的电影返回到编辑视图。 当基架系统创建“编辑”视图时，它会检查 `Movie` 类并创建代码为类的每个属性呈现 `<label>` 和 `<input>` 元素。 下面的示例显示生成的编辑视图：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

请注意如何查看模板`@model MvcMovie.Models.Movie`语句文件的顶部 — 这指定视图需要查看模板类型的模型`Movie`。

基架的代码使用了若干个*帮助器方法*来简化的 HTML 标记。 [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx)帮助器显示的字段 （"标题"、"ReleaseDate"、"Genre"或"Price"） 的名称。 [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx)帮助器将显示 HTML`<input>`元素。 [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx)帮助器显示与该属性相关联的任何验证消息。

运行应用程序并导航到*/Movies* URL。 点击“编辑”链接。 在浏览器中查看页面的源。 在页中的 HTML 类似于下面的示例。 （菜单标记被排除为清楚起见。）

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample4.html)]

`<input>`元素是在 HTML`<form>`元素其`action`特性设置为发布到*/电影/编辑*URL。 窗体数据将发送到服务器时**编辑**单击按钮。

## <a name="processing-the-post-request"></a>处理 POST 请求

以下列表显示了 `Edit` 操作方法的 `HttpPost` 版本。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs)]

ASP.NET framework 模型联编程序接受已发布的窗体值并创建`Movie`作为传递对象`movie`参数。 `ModelState.IsValid`在代码中检查会验证是否在窗体中提交的数据可以用于修改`Movie`对象。 如果数据有效，代码会将保存到影片数据`Movies`集合`MovieDBContext`实例。 代码然后将新的影片数据保存到数据库，通过调用`SaveChanges`方法`MovieDBContext`，这仍然存在对数据库的更改。 正在保存的数据之后, 该代码将用户重定向到`Index`操作方法`MoviesController`类，这会导致更新的电影的电影列表中显示。

如果发送的值不是有效的系统会将它们重新显示在窗体中。 `Html.ValidationMessageFor`中的帮助器*Edit.cshtml*视图模板负责显示相应的错误消息。

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image8.png)](examining-the-edit-methods-and-edit-view/_static/image7.png)

> **请注意关于区域设置**如果通常使用的非英语区域设置，请参阅[支持非英语区域设置使用 ASP.NET MVC 3 验证。](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>进行的编辑方法更可靠

`HttpGet` `Edit`基架系统生成的方法不会检查传递给它的 ID 无效。 从 URL 用户中删除 ID 段如果 (`http://localhost:xxxxx/Movies/Edit`)，显示以下错误：

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image10.png)](examining-the-edit-methods-and-edit-view/_static/image9.png)

用户可能还传递不存在在数据库中，如 ID `http://localhost:xxxxx/Movies/Edit/1234`。 你可以为两个更改`HttpGet``Edit`操作方法来解决此限制。 首先，更改`ID`参数有默认值为零，当未显式传递一个 ID。 你还可以检查`Find`方法实际上将影片对象返回到视图模板之前，找到的影片。 已更新`Edit`方法如下所示。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

如果不找到任何电影，`HttpNotFound`调用方法。

所有`HttpGet`方法遵循类似的模式。 它们获取电影对象 (或列表中的对象，这种情况的`Index`)，并将模型传递到该视图。 `Create`方法将空的电影对象传递给创建视图。 在方法的 `HttpPost` 重载中，创建、编辑、删除或以其他方式修改数据的所有方法都执行此操作。 博客文章中所述修改 HTTP GET 方法中的数据是安全风险， [ASP.NET MVC 提示 #46 – 不要使用删除的链接，因为他们创建的安全漏洞](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)。 在 GET 方法中修改数据也违反了 HTTP 最佳实践和体系结构的 REST 模式，它指定 GET 请求不应更改你的应用程序的状态。 换而言之，执行 GET 操作应为没有任何副作用的安全操作。

## <a name="adding-a-search-method-and-search-view"></a>添加搜索方法和搜索视图

在本部分将添加`SearchIndex`操作方法，可使你搜索电影按风格或名称。 这将是可通过*/电影/SearchIndex* URL。 请求将显示 HTML 窗体，其中包含用户可以填写以便搜索一部电影的输入的元素。 当用户提交表单时，操作方法将获取发布的用户的搜索值，并使用这些值来搜索数据库。

## <a name="displaying-the-searchindex-form"></a>显示 SearchIndex 窗体

首先，通过添加`SearchIndex`到现有的操作方法`MoviesController`类。 该方法将返回一个包含 HTML 窗体视图。 以下是代码：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cs)]

第一行`SearchIndex`方法中创建以下[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)查询，以便选择电影：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cs)]

查询在这种情况下，定义，但尚未尚未运行针对数据存储。

如果`searchString`参数包含一个字符串，将修改电影查询要作为筛选依据的搜索字符串，使用下面的代码的值：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

在定义后或通过调用方法，如对其进行修改时，将不会执行 LINQ 查询`Where`或`OrderBy`。 相反，延迟查询执行，这意味着表达式的计算延迟，直到它已实现的值实际上循环访问或[ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx)调用方法。 在`SearchIndex`示例 SearchIndex 视图中执行查询。 有关延迟执行查询的详细信息，请参阅[Query Execution](https://msdn.microsoft.com/library/bb738633.aspx)（查询执行）。

现在你可以实现`SearchIndex`将向用户显示该窗体的视图。 右键单击内部`SearchIndex`方法，然后单击**添加视图**。 在**添加视图**对话框框中，指定你要将传递`Movie`到其模型类的视图模板的对象。 在**基架模板**列表中，选择**列表**，然后单击**添加**。

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

当你单击**添加**按钮， *Views\Movies\SearchIndex.cshtml*创建视图模板。 由于你选择了**列表**中**基架模板**列表，Visual Web Developer 中自动生成 （基架） 在视图中的某些默认内容。 基架创建 HTML 窗体。 它检查`Movie`类和创建的代码来呈现`<label>`类的每个属性的元素。 下面的列表显示已生成的创建视图：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

运行应用程序并导航到*/电影/SearchIndex*。 将查询字符串（如 `?searchString=ghost`）追加到 URL。 筛选的电影将显示出来。

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

如果你更改的签名`SearchIndex`方法都具有一个名为参数`id`、`id`参数将匹配`{id}`默认的占位符将路由中的组*Global.asax*文件。

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

已修改`SearchIndex`方法如下，如下所示：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cs)]

现可将搜索标题作为路由数据（ URL 段）而非查询字符串值进行传递。

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

但是，不能指望用户在每次要搜索电影时都修改 URL。 至此，你将添加 UI，以帮助它们筛选电影。 如果你更改的签名`SearchIndex`方法来测试如何将传递路由绑定 ID 参数，将其更改回来，以使你`SearchIndex`方法采用字符串参数名为`searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample13.cs)]

打开*Views\Movies\SearchIndex.cshtml*文件中，并紧后面`@Html.ActionLink("Create New", "Create")`，添加以下：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cshtml)]

下面的示例显示的一部分*Views\Movies\SearchIndex.cshtml*文件替换为添加的筛选标记。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cshtml)]

`Html.BeginForm`帮助器创建开始`<form>`标记。 `Html.BeginForm`帮助程序使窗体时在用户通过单击提交窗体发布到其自身**筛选器**按钮。

运行应用程序并尝试搜索对于影片。

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

没有任何`HttpPost`重载`SearchIndex`方法。 你不需要它，因为该方法不更改状态的应用程序，只需筛选数据。

可添加以下 `HttpPost SearchIndex` 方法。 在这种情况下，将匹配操作调用程序`HttpPost SearchIndex`方法，与`HttpPost SearchIndex`方法将运行下图中所示。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

但是，即使添加 `SearchIndex` 方法的 `HttpPost` 版本，其实现方式也受到限制。 假设你想要将特定搜索加入书签，或向朋友发送一个链接，让他们单击链接即可查看筛选出的相同电影列表。 请注意，HTTP POST 请求的 URL 是 GET 请求中 （localhost:xxxxx/电影/SearchIndex） 的 URL 相同-没有在 URL 本身搜索信息。 右现在，搜索字符串信息发送到服务器作为窗体字段值。 这意味着你无法捕获该搜索信息来添加书签或将发送到 URL 中的友元。

解决方案是使用的重载`BeginForm`指定 POST 请求应将搜索信息添加到 URL，并且这是应为路由到的 HttpGet 版本`SearchIndex`方法。 将现有无参数`BeginForm`方法替换为以下：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml)]

[![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image22.png)](examining-the-edit-methods-and-edit-view/_static/image21.png)

现在当你提交搜索时，该 URL 将包含搜索查询字符串。 即使具备 `HttpPost SearchIndex` 方法，搜索也将转到 `HttpGet SearchIndex` 操作方法。

[![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image24.png)](examining-the-edit-methods-and-edit-view/_static/image23.png)

## <a name="adding-search-by-genre"></a>添加按风格的搜索

如果你添加`HttpPost`版本`SearchIndex`方法，现在将其删除。

接下来，你将添加一项功能，以便按风格搜索电影的用户。 将 `SearchIndex` 方法替换为以下代码：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cs)]

此版本的`SearchIndex`方法采用附加参数，即`movieGenre`。 代码的前几行创建`List`对象以保存从数据库的电影风格。

下面的代码是一种 LINQ 查询，可从数据库中检索所有流派。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

该代码使用`AddRange`方法的泛型`List`集合，以便添加到列表的所有非重复的风格。 (而无需`Distinct`修饰符，将添加重复的风格 — 例如，将在我们的示例中两次添加喜剧)。 代码然后将存储的列表中的风格`ViewBag`对象。

下面的代码演示如何检查`movieGenre`参数。 如果不为空代码进一步限制电影查询，以限制到指定的流派所选的影片。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>将标记添加到 SearchIndex 视图，以支持按风格的搜索

添加`Html.DropDownList`到帮助程序*Views\Movies\SearchIndex.cshtml*文件，之前`TextBox`帮助器。 已完成的标记如下所示：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cshtml)]

运行应用程序，并浏览到*/电影/SearchIndex*。 按风格、 电影名称，以及这两个条件，请尝试搜索。

在本部分中检查的 CRUD 操作方法和框架生成的视图。 你创建的搜索操作方法和视图，以让用户通过影片标题和风格搜索。 在下一步的部分中，你将了解如何将属性添加到`Movie`模型以及如何添加初始值设定项将自动创建一个测试数据库。

>[!div class="step-by-step"]
[上一页](accessing-your-models-data-from-a-controller.md)
[下一页](adding-a-new-field.md)
