---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
title: "检查编辑方法和编辑视图 |Microsoft 文档"
author: Rick-Anderson
description: "注意： 本教程的更新的版本此处提供了使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全，请按照和演示要简单得多..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 41eb99ca-e88f-4720-ae6d-49a958da8116
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 315914056c0a666fdf23cf82a314a999e03114b6
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="examining-the-edit-methods-and-edit-view"></a>检查编辑方法和编辑视图
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教程的更新的版本可[此处](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全，请按照要简单得多，并演示更多的功能。


在此部分中，你将检查生成的操作方法和电影控制器的视图。 然后你将添加自定义搜索页。

运行应用程序，并浏览到`Movies`控制器通过追加*/Movies*到你的浏览器的地址栏中的 URL。 将鼠标指针停留在**编辑**链接以查看链接到的 URL。

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**编辑**链接通过生成`Html.ActionLink`中的方法*Views\Movies\Index.cshtml*视图：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html`对象是使用上的属性公开一个帮助程序[System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx)基类。 `ActionLink`的帮助器方法，可以轻松地动态生成 HTML 的超链接，链接到控制器上的操作方法。 第一个参数`ActionLink`方法是要呈现的链接文本 (例如， `<a>Edit Me</a>`)。 第二个参数是要调用的操作方法的名称。 最后一个参数是[匿名对象](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx)，用来生成路由数据 （在本例中为 4 的 ID）。

前面的图像中所示的生成的链接是`http://localhost:xxxxx/Movies/Edit/4`。 默认路由 (在中建立*应用\_Start\RouteConfig.cs*) 采用的 URL 模式`{controller}/{action}/{id}`。 因此，ASP.NET 将转换`http://localhost:xxxxx/Movies/Edit/4`放置到请求`Edit`操作方法`Movies`与参数的控制器`ID`等于 4。 检查下面的代码从*应用\_Start\RouteConfig.cs*文件。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

你还可以传递使用查询字符串的操作方法参数。 例如，URL`http://localhost:xxxxx/Movies/Edit?ID=4`还传递了参数`ID`4 至`Edit`操作方法`Movies`控制器。

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

打开`Movies`控制器。 这两个`Edit`操作方法如下所示。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cs)]

请注意第二个 `Edit` 操作方法的前面是 `HttpPost` 特性。 此属性指定的该重载`Edit`可以仅针对的 POST 请求调用方法。 你可以应用`HttpGet`属性设置为第一个编辑方法，但这它们是不必要，因为它是默认值。 (我们在指隐式分配的操作方法`HttpGet`属性为`HttpGet`方法。)

`HttpGet` `Edit`方法采用电影 ID 参数，以查找使用实体框架电影`Find`方法，并将所选的电影返回到编辑视图。 ID 参数指定[默认值](https://msdn.microsoft.com/library/dd264739.aspx)为零的`Edit`方法调用不带参数。 如果找不到一部电影， [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx)返回。 当基架系统创建“编辑”视图时，它会检查 `Movie` 类并创建代码为类的每个属性呈现 `<label>` 和 `<input>` 元素。 下面的示例显示生成的编辑视图：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cshtml)]

请注意如何查看模板`@model MvcMovie.Models.Movie`语句文件的顶部 — 这指定视图需要查看模板类型的模型`Movie`。

基架的代码使用了若干个*帮助器方法*来简化的 HTML 标记。 [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx)帮助器显示的字段的名称 (&quot;标题&quot;， &quot;ReleaseDate&quot;，&quot;流派&quot;，或&quot;价格&quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx)帮助器上呈现 HTML`<input>`元素。 [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx)帮助器显示与该属性相关联的任何验证消息。

运行应用程序并导航到*/Movies* URL。 点击“编辑”链接。 在浏览器中查看页面的源。 下面显示了窗体元素的 HTML。

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html?highlight=7,10-11)]

`<input>`元素是在 HTML`<form>`元素其`action`特性设置为发布到*/电影/编辑*URL。 窗体数据将发送到服务器时**编辑**单击按钮。

## <a name="processing-the-post-request"></a>处理 POST 请求

以下列表显示了 `Edit` 操作方法的 `HttpPost` 版本。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

[ASP.NET MVC 模型联编程序](https://msdn.microsoft.com/magazine/hh781022.aspx)采用已发布的窗体值，并创建`Movie`作为传递对象`movie`参数。 `ModelState.IsValid` 方法验证表单中提交的数据是否可以用于修改（编辑或更新）`Movie` 对象。 如果数据有效，影片数据保存到`Movies`集合`db(MovieDBContext`实例)。 新的影片数据保存到数据库上，通过调用`SaveChanges`方法`MovieDBContext`。 正在保存的数据之后, 该代码将用户重定向到`Index`操作方法`MoviesController`类，其中显示的电影收藏，包括刚才所做的更改。

如果发送的值不是有效的系统会将它们重新显示在窗体中。 `Html.ValidationMessageFor`中的帮助器*Edit.cshtml*视图模板负责显示相应的错误消息。

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

> [!NOTE]
> 若要为使用逗号的非英语区域设置支持 jQuery 验证 (&quot;，&quot;) 必须包含一个小数点， *globalize.js*和您的特定*cultures/globalize.cultures.js*文件 (从[ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) 和 JavaScript 使用`Globalize.parseFloat`。 下面的代码演示要处理的 Views\Movies\Edit.cshtml 文件修改&quot;FR-FR&quot;区域性：


[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

十进制字段可能需要一个逗号，而不是十进制点。 作为临时解决问题，可以将全球化元素添加到项目根 web.config 文件。 下面的代码演示与设置为英语 （美国） 区域性的全球化元素。

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.xml)]

所有`HttpGet`方法遵循类似的模式。 它们获取电影对象 (或列表中的对象，这种情况的`Index`)，并将模型传递到该视图。 `Create`方法将空的电影对象传递给创建视图。 在方法的 `HttpPost` 重载中，创建、编辑、删除或以其他方式修改数据的所有方法都执行此操作。 博客文章中所述修改 HTTP GET 方法中的数据是安全风险， [ASP.NET MVC 提示 #46 – 不要使用删除的链接，因为他们创建的安全漏洞](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)。 HTTP 最佳实践和体系结构在 GET 方法中修改数据也违反了[REST](http://en.wikipedia.org/wiki/Representational_State_Transfer)模式，它指定 GET 请求不应更改你的应用程序的状态。 换句话说，执行 GET 操作应是没有任何隐患的安全操作，也不会修改持久数据。

## <a name="adding-a-search-method-and-search-view"></a>添加搜索方法和搜索视图

在本部分将添加`SearchIndex`操作方法，可使你搜索电影按风格或名称。 这将是可通过*/电影/SearchIndex* URL。 请求将显示 HTML 窗体，其中包含用户可以输入以便搜索一部电影的输入的元素。 当用户提交表单时，操作方法将获取发布的用户的搜索值，并使用这些值来搜索数据库。

## <a name="displaying-the-searchindex-form"></a>显示 SearchIndex 窗体

首先，通过添加`SearchIndex`到现有的操作方法`MoviesController`类。 该方法将返回一个包含 HTML 窗体视图。 以下是代码：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

第一行`SearchIndex`方法中创建以下[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)查询，以便选择电影：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cs)]

查询在这种情况下，定义，但尚未尚未运行针对数据存储。

如果`searchString`参数包含一个字符串，将修改电影查询要作为筛选依据的搜索字符串，使用下面的代码的值：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample11.cs)]

上面的 `s => s.Title` 代码是 [Lambda 表达式](https://msdn.microsoft.com/library/bb397687.aspx)。 Lambda 在基于方法的使用[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)如查询中用作标准查询运算符方法的自变量[其中](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx)在上面的代码中使用的方法。 在定义后或通过调用方法，如对其进行修改时，将不会执行 LINQ 查询`Where`或`OrderBy`。 相反，延迟查询执行，这意味着表达式的计算延迟，直到它已实现的值实际上循环访问或[ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx)调用方法。 在`SearchIndex`示例 SearchIndex 视图中执行查询。 有关延迟执行查询的详细信息，请参阅[Query Execution](https://msdn.microsoft.com/library/bb738633.aspx)（查询执行）。

现在你可以实现`SearchIndex`将向用户显示该窗体的视图。 右键单击内部`SearchIndex`方法，然后单击**添加视图**。 在**添加视图**对话框框中，指定你要将传递`Movie`到其模型类的视图模板的对象。 在**基架模板**列表中，选择**列表**，然后单击**添加**。

![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image5.png)

当你单击**添加**按钮， *Views\Movies\SearchIndex.cshtml*创建视图模板。 由于你选择了**列表**中**基架模板**列表，自动生成的 Visual Studio （基架） 在视图中的一些默认标记。 基架创建 HTML 窗体。 它检查`Movie`类和创建的代码来呈现`<label>`类的每个属性的元素。 下面的列表显示已生成的创建视图：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cshtml)]

运行应用程序并导航到*/电影/SearchIndex*。 将查询字符串（如 `?searchString=ghost`）追加到 URL。 筛选的电影将显示出来。

![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image6.png)

如果你更改的签名`SearchIndex`方法都具有一个名为参数`id`、`id`参数将匹配`{id}`默认的占位符将路由中的组*Global.asax*文件。

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample13.json)]

原始`SearchIndex`方法如下所示::

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cs)]

已修改`SearchIndex`方法如下，如下所示：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cs?highlight=1,3)]

现可将搜索标题作为路由数据（ URL 段）而非查询字符串值进行传递。

![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image7.png)

但是，不能指望用户在每次要搜索电影时都修改 URL。 至此，你将添加 UI，以帮助它们筛选电影。 如果你更改的签名`SearchIndex`方法来测试如何将传递路由绑定 ID 参数，将其更改回来，以使你`SearchIndex`方法采用字符串参数名为`searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

打开*Views\Movies\SearchIndex.cshtml*文件中，并紧后面`@Html.ActionLink("Create New", "Create")`，添加以下：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml?highlight=2)]

下面的示例显示的一部分*Views\Movies\SearchIndex.cshtml*文件替换为添加的筛选标记。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cshtml?highlight=12-15)]

`Html.BeginForm`帮助器创建开始`<form>`标记。 `Html.BeginForm`帮助程序使窗体时在用户通过单击提交窗体发布到其自身**筛选器**按钮。

运行应用程序并尝试搜索对于影片。

![](examining-the-edit-methods-and-edit-view/_static/image8.png)

没有任何`HttpPost`重载`SearchIndex`方法。 你不需要它，因为该方法不更改状态的应用程序，只需筛选数据。

可添加以下 `HttpPost SearchIndex` 方法。 在这种情况下，将匹配操作调用程序`HttpPost SearchIndex`方法，与`HttpPost SearchIndex`方法将运行下图中所示。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image9.png)

但是，即使添加 `SearchIndex` 方法的 `HttpPost` 版本，其实现方式也受到限制。 假设你想要将特定搜索加入书签，或向朋友发送一个链接，让他们单击链接即可查看筛选出的相同电影列表。 请注意，HTTP POST 请求的 URL 是 GET 请求中 （localhost:xxxxx/电影/SearchIndex） 的 URL 相同-没有在 URL 本身搜索信息。 右现在，搜索字符串信息发送到服务器作为窗体字段值。 这意味着你无法捕获该搜索信息来添加书签或将发送到 URL 中的友元。

解决方案是使用的重载`BeginForm`指定 POST 请求，应将搜索信息添加到 URL，并且它应将路由到的 HttpGet 版本`SearchIndex`方法。 将现有无参数`BeginForm`方法替换为以下：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cshtml)]

![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

现在当你提交搜索时，该 URL 将包含搜索查询字符串。 即使具备 `HttpPost SearchIndex` 方法，搜索也将转到 `HttpGet SearchIndex` 操作方法。

![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image11.png)

## <a name="adding-search-by-genre"></a>添加按风格的搜索

如果你添加`HttpPost`版本`SearchIndex`方法，现在将其删除。

接下来，你将添加一项功能，以便按风格搜索电影的用户。 将 `SearchIndex` 方法替换为以下代码：

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cs)]

此版本的`SearchIndex`方法采用附加参数，即`movieGenre`。 代码的前几行创建`List`对象以保存从数据库的电影风格。

下面的代码是一种 LINQ 查询，可从数据库中检索所有流派。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample22.cs)]

该代码使用`AddRange`方法的泛型`List`集合，以便添加到列表的所有非重复的风格。 (而无需`Distinct`修饰符，将添加重复的风格 — 例如，将在我们的示例中两次添加喜剧)。 代码然后将存储的列表中的风格`ViewBag`对象。

下面的代码演示如何检查`movieGenre`参数。 如果不为空，则进一步对代码约束电影查询，以限制到指定的流派所选的影片。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample23.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>将标记添加到 SearchIndex 视图，以支持按风格的搜索

添加`Html.DropDownList`到帮助程序*Views\Movies\SearchIndex.cshtml*文件，之前`TextBox`帮助器。 已完成的标记如下所示：

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample24.cshtml?highlight=4)]

运行应用程序，并浏览到*/电影/SearchIndex*。 按风格、 电影名称，以及这两个条件，请尝试搜索。

![](examining-the-edit-methods-and-edit-view/_static/image12.png)

在本部分中检查的 CRUD 操作方法和框架生成的视图。 你创建的搜索操作方法和视图，以让用户通过影片标题和风格搜索。 在下一步的部分中，你将了解如何将属性添加到`Movie`模型以及如何添加初始值设定项将自动创建一个测试数据库。

>[!div class="step-by-step"]
[上一页](accessing-your-models-data-from-a-controller.md)
[下一页](adding-a-new-field-to-the-movie-model-and-table.md)
