---
uid: mvc/overview/getting-started/introduction/adding-search
title: "搜索 |Microsoft 文档"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 116f681e14af0a09a4eb1502ef9f057c5db2f97d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="search"></a>搜索
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a>添加搜索方法和搜索视图

在本部分中，你将添加到搜索功能`Index`操作方法，可使你搜索电影按风格或名称。

## <a name="updating-the-index-form"></a>更新索引窗体

通过更新开始`Index`到现有的操作方法`MoviesController`类。 以下是代码：

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

第一行`Index`方法中创建以下[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)查询，以便选择电影：

[!code-csharp[Main](adding-search/samples/sample2.cs)]

查询在这种情况下，定义，但尚未尚未对数据库运行。

如果`searchString`参数包含一个字符串，将修改电影查询要作为筛选依据的搜索字符串，使用下面的代码的值：

[!code-csharp[Main](adding-search/samples/sample3.cs)]

上面的 `s => s.Title` 代码是 [Lambda 表达式](https://msdn.microsoft.com/library/bb397687.aspx)。 Lambda 在基于方法的使用[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)如查询中用作标准查询运算符方法的自变量[其中](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx)在上面的代码中使用的方法。 在定义后或通过调用方法，如对其进行修改时，将不会执行 LINQ 查询`Where`或`OrderBy`。 相反，延迟查询执行，这意味着表达式的计算延迟，直到它已实现的值实际上循环访问或[ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx)调用方法。 在`Search`示例中，在执行查询*Index.cshtml*视图。 有关延迟执行查询的详细信息，请参阅[Query Execution](https://msdn.microsoft.com/library/bb738633.aspx)（查询执行）。

> [!NOTE]
> [Contains](https://msdn.microsoft.com/library/bb155125.aspx)方法运行在数据库中，不是 c# 代码上面。 在数据库中， [Contains](https://msdn.microsoft.com/library/bb155125.aspx)映射到[SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx)，这是区分大小写。

现在你可以更新`Index`将向用户显示该窗体的视图。

运行应用程序并导航到*/电影/索引*。 将查询字符串（如 `?searchString=ghost`）追加到 URL。 筛选的电影将显示出来。

![SearchQryStr](adding-search/_static/image1.png)

如果你更改的签名`Index`方法都具有一个名为参数`id`、`id`参数将匹配`{id}`默认的占位符将路由中的组*应用\_用户RouteConfig.cs*文件。

[!code-json[Main](adding-search/samples/sample4.json)]

原始`Index`方法如下所示::

[!code-csharp[Main](adding-search/samples/sample5.cs)]

已修改`Index`方法如下，如下所示：

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

现可将搜索标题作为路由数据（ URL 段）而非查询字符串值进行传递。

![](adding-search/_static/image2.png)

但是，不能指望用户在每次要搜索电影时都修改 URL。 至此，你将添加 UI，以帮助它们筛选电影。 如果你更改的签名`Index`方法来测试如何将传递路由绑定 ID 参数，将其更改回来，以使你`Index`方法采用字符串参数名为`searchString`:

[!code-csharp[Main](adding-search/samples/sample7.cs)]

打开*Views\Movies\Index.cshtml*文件中，并紧后面`@Html.ActionLink("Create New", "Create")`，添加以下突出显示的窗体标记：

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

`Html.BeginForm`帮助器创建开始`<form>`标记。 `Html.BeginForm`帮助程序使窗体时在用户通过单击提交窗体发布到其自身**筛选器**按钮。

Visual Studio 2013 具有良好的改善时显示和编辑视图文件。 使用运行时应用程序视图文件打开，Visual Studio 2013 时，将调用以显示视图的正确的控制器操作方法。

![](adding-search/_static/image3.png)

索引视图 （如在上图中所示），在 Visual Studio 中打开，点击 Ctr F5 运行应用程序，然后再尝试搜索一部电影。

![](adding-search/_static/image4.png)

没有任何`HttpPost`重载`Index`方法。 你不需要它，因为该方法不更改状态的应用程序，只需筛选数据。

可添加以下 `HttpPost Index` 方法。 在这种情况下，将匹配操作调用程序`HttpPost Index`方法，与`HttpPost Index`方法将运行下图中所示。

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

但是，即使添加 `Index` 方法的 `HttpPost` 版本，其实现方式也受到限制。 假设你想要将特定搜索加入书签，或向朋友发送一个链接，让他们单击链接即可查看筛选出的相同电影列表。 请注意，HTTP POST 请求的 URL 是 GET 请求中 （localhost:xxxxx/电影/索引） 的 URL 相同-没有在 URL 本身搜索信息。 右现在，搜索字符串信息发送到服务器作为窗体字段值。 这意味着你无法捕获该搜索信息来添加书签或将发送到 URL 中的友元。

解决方案是使用的重载`BeginForm`指定 POST 请求，应将搜索信息添加到 URL，并且它应将路由到`HttpGet`版本`Index`方法。 将现有无参数`BeginForm`方法替换为以下标记：

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

现在当你提交搜索时，该 URL 将包含搜索查询字符串。 即使具备 `HttpPost Index` 方法，搜索也将转到 `HttpGet Index` 操作方法。

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>添加按风格的搜索

如果你添加`HttpPost`版本`Index`方法，现在将其删除。

接下来，你将添加一项功能，以便按风格搜索电影的用户。 将 `Index` 方法替换为以下代码：

[!code-csharp[Main](adding-search/samples/sample11.cs)]

此版本的`Index`方法采用附加参数，即`movieGenre`。 代码的前几行创建`List`对象以保存从数据库的电影风格。

下面的代码是一种 LINQ 查询，可从数据库中检索所有流派。

[!code-csharp[Main](adding-search/samples/sample12.cs)]

该代码使用`AddRange`方法的泛型`List`集合，以便添加到列表的所有非重复的风格。 (而无需`Distinct`修饰符，将添加重复的风格 — 例如，将在我们的示例中两次添加喜剧)。 代码然后将存储的列表中的风格`ViewBag.MovieGenre`对象。 存储类别数据 （此类电影风格的） 作为[此时](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx)对象在`ViewBag`，则访问一个下拉列表框中的类别数据是典型的 MVC 应用程序的方法。

下面的代码演示如何检查`movieGenre`参数。 如果不为空，则进一步对代码约束电影查询，以限制到指定的流派所选的影片。

[!code-csharp[Main](adding-search/samples/sample13.cs)]

如前面所述，运行查询时未对数据基础直到影片列表循环访问 (这发生在视图中之后,`Index`操作方法会返回)。

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>将标记添加到索引视图，用于支持按风格的搜索

添加`Html.DropDownList`到帮助程序*Views\Movies\Index.cshtml*文件，之前`TextBox`帮助器。 已完成的标记如下所示：

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

在下面的代码：

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

参数"MovieGenre"提供的密钥`DropDownList`用于查找帮助`IEnumerable<SelectListItem>`中`ViewBag`。 `ViewBag`已填充的操作方法中：

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

"全部"提供的选项标签参数。 如果您在浏览器中检查该选择，你将看到其"value"属性为空。 由于我们控制器仅筛选`if`字符串不是`null`或该表为空，提交空值的`movieGenre`显示所有风格。

你还可以设置一个选项以默认处于选中状态。 如果你想"喜剧"作为默认选项，则可以更改在控制器中的代码如下所示：

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

运行应用程序，并浏览到*/电影/索引*。 按风格、 电影名称，以及这两个条件，请尝试搜索。

![](adding-search/_static/image8.png)

在本部分中创建的搜索操作方法和视图，以让用户通过影片标题和风格搜索。 在下一步的部分中，你将了解如何将属性添加到`Movie`模型以及如何添加初始值设定项将自动创建一个测试数据库。

>[!div class="step-by-step"]
[上一页](examining-the-edit-methods-and-edit-view.md)
[下一页](adding-a-new-field.md)
