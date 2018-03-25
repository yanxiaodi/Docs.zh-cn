---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: "引入的 ASP.NET Web Pages-更新数据库数据 |Microsoft 文档"
author: tfitzmac
description: "本教程演示如何使用 ASP.NET Web 页 (Razor) 时 （更改） 的现有数据库条目更新。 它假定你已完成序列 th..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: b016231975bf8d359f4c390b0b478edc383117d4
ms.sourcegitcommit: df2157ae9aeea0075772719c29784425c783e82a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2018
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a>引入了 ASP.NET Web 页-更新数据库数据
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本教程演示如何使用 ASP.NET Web 页 (Razor) 时 （更改） 的现有数据库条目更新。 它假定你已完成通过系列[输入数据通过使用窗体使用的 ASP.NET Web Pages](entering-data.md)。
> 
> 你将学习：
> 
> - 如何选择中的单个记录`WebGrid`帮助器。
> - 如何从数据库中读取单个记录。
> - 如何预加载的窗体具有来自数据库记录的值。
> - 如何更新数据库中的现有记录。
> - 如何在页中存储的信息，而不显示它。
> - 如何使用隐藏的字段来存储信息。
>   
> 
> 功能/技术讨论：
> 
> - `WebGrid`帮助器。
> - SQL`Update`命令。
> - `Database.Execute` 方法。
> - 隐藏字段 (`<input type="hidden">`)。


## <a name="what-youll-build"></a>你将生成

在前面的教程，您学习了如何将记录添加到数据库。 在这里，您将学习如何显示一条记录以进行编辑。 在*电影*页上，你将更新`WebGrid`帮助器以便它显示**编辑**每个影片旁边的链接：

![WebGrid 显示包括每个影片的编辑链接](updating-data/_static/image1.png)

当你单击**编辑**链接，它将你带到不同的页的影片信息已在窗体：

![编辑显示要编辑的电影的电影页](updating-data/_static/image2.png)

你可以更改的任何值。 当提交所做的更改时，在页中的代码将更新数据库，并将影片列表返回。

此过程的一部分工作原理几乎完全与*AddMovie.cshtml*你创建在以前的教程中，因此，很多本教程将会熟悉页面。

有几种方法无法实现一种方法编辑单个影片。 因为它易于实施且易于理解，已选择显示的方法。

## <a name="adding-an-edit-link-to-the-movie-listing"></a>添加到影片列表的编辑链接

若要开始，你将更新*电影*页，以便还列出每个影片包含**编辑**链接。

打开*Movies.cshtml*文件。

在页的正文中，更改`WebGrid`通过添加的列的标记。 下面是已修改的标记：

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

新的列是这样一个：

[!code-html[Main](updating-data/samples/sample2.html)]

此列的点是显示的链接 (`<a>`元素) 的文本显示"编辑"。 我们后已是创建一个链接，以页运行时，如下所示使用`id`为每个电影不同的值：

[!code-css[Main](updating-data/samples/sample3.css)]

此链接将调用一个名为页*EditMovie*，它将通过查询字符串和`?id=7`到该页面。

新列的语法可能看起来有点复杂，但这只是因为它汇集了几个元素。 每个单独元素非常简单。 如果你专注于仅`<a>`元素，请参阅此标记：

[!code-html[Main](updating-data/samples/sample4.html)]

有关网格的工作方式一些背景信息： 在网格中显示行，分别针对每个数据库记录，而且它会显示数据库记录中的每个字段的列。 构造的每个网格行，而`item`对象包含该行的数据库记录 （项）。 这种安排为你提供一种方法中代码，使该行的数据。 你在此处看到的那样： 表达式`item.ID`获取当前数据库项的 ID 值。 你无法通过使用获取任何数据库的值 （标题、 genre 或年） 相同的方式`item.Title`， `item.Genre`，或`item.Year`。

表达式`"~/EditMovie?id=@item.ID`将合并目标 URL 的硬编码部分 (`~/EditMovie?id=`) 替换为此动态派生的 id。 (你已了解`~`运算符在以前的教程中; 它是一个表示当前的网站根目录的 ASP.NET 运算符。)

结果是标记的，这一部分的列中只需生成类似以下的标记在运行时：

[!code-xml[Main](updating-data/samples/sample5.xml)]

当然，实际值的`id`每一行都有所不同。

## <a name="creating-a-custom-display-for-a-grid-column"></a>创建自定义显示为网格列

现在回网格列。 三列最初已将在显示的网格数据值只有 （标题、 genre 和年）。 通过将数据库列的名称传递指定此显示&mdash;例如`grid.Column("Title")`。

此新**编辑**链接列是不同。 您传递指定列名称，而不`format`参数。 此参数，您可以定义标记，`WebGrid`帮助器将呈现连同`item`要显示为粗体或绿色的列数据或的任何格式所需值。 例如，如果你想要显示加粗的标题，你可以创建如下例所示的列：

[!code-html[Main](updating-data/samples/sample6.html)]

(各种`@`中看到的字符`format`属性标记的标记和代码值之间的转换。)

一旦你了解了有关`format`属性，它是更易于理解如何新**编辑**链接列结合在一起：

[!code-html[Main](updating-data/samples/sample7.html)]

列包含*仅*的呈现链接的标记，加上一些信息 (ID)，从提取数据库记录的行。

> [!TIP] 
> 
> **命名的参数和方法的位置参数**
> 
> 多次时调用方法并传递给它的参数后，你只需已列出并用逗号隔开的参数值。 以下为若干示例：
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> 当你第一次看到此代码中，但在每个情况下，你要将参数传递给以特定顺序的方法时，我们没有提到问题&mdash;也就是说，在该参数在该方法中定义的顺序。 有关`db.Execute`和`Validation.RequireFields`，如果你混合传递的值的顺序，你将收到一条错误消息页运行时或至少一些奇怪的结果。 显然，你必须知道传递中的参数顺序。 （在 WebMatrix 中，IntelliSense 可帮助你了解算出名称、 类型和参数的顺序。）
> 
> 作为按顺序传递值的替代方法，你可以使用*命名参数*。 (按顺序传递参数被称为使用*位置参数*。)对于命名参数，你可以将其值传递时显式包括到参数的名称。 你使用命名的参数已多次这些教程中。 例如:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> 和
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> 命名的参数是功能几个情况下，可以提供便利，尤其是在某方法采用多个参数。 其中一个是当你想要传递只能将一个或两个参数，但你想要传递的值不在参数列表中的第一个位置之间。 另一种情况是当你想要通过将参数传递给你最有利的顺序，使你的代码更具可读性。
> 
> 显然，若要使用命名的参数，你必须知道这些参数的名称。 WebMatrix IntelliSense 可以*显示*你名称，但它不能当前填充它们为你。


## <a name="creating-the-edit-page"></a>创建编辑页

现在，你可以创建*EditMovie*页。 当用户单击**编辑**链接，它们将显示此页上。

创建一个名为页*EditMovie.cshtml* ，并将替换为以下标记文件中：

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

此标记和代码是类似于中有*AddMovie*页。 没有略有不同的提交按钮的文本。 与*AddMovie*页上，没有`Html.ValidationSummary`将显示验证错误，如果有任何的调用。 此时我们要离开掉调用`Validation.Message`，因为错误将显示在摘要的验证。 如前面的教程中所述，你可以在各种组合中使用验证摘要和单独的错误消息。

再次请注意，`method`属性`<form>`元素设置为`post`。 与*AddMovie.cshtml*页上，此页进行更改到数据库。 因此，应执行此窗体`POST`操作。 (有关详细信息之间的差异`GET`和`POST`操作，请参阅[GET、 POST 和 HTTP 谓词安全](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety)边栏在教程中 HTML 窗体上。)

正如你在前面的教程中，看到`value`的文本框中的属性正在设置 Razor 代码使用了预加载它们。 这一次，不过，你使用的变量，如`title`和`genre`而不是该任务的`Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

为之前，此标记将预加载的影片值文本框值。 你将看到在稍后为方便起见，可以使用变量而不使用这一次`Request`对象。

此外，还有`<input type="hidden">`此页上的元素。 此元素而不使其可见的页上存储的电影 ID。 ID 最初传递到页上通过使用查询字符串值 (`?id=7`或类似的 URL 中)。 通过将 ID 值放入隐藏的字段，你可以确保便可提交表单时，即使你不再有权页使用调用的原始 URL。

与不同*AddMovie*页上的代码*EditMovie*页有两个不同的功能。 第一个函数是，如果首次显示的页面 (和*仅*然后)，此代码可查询字符串中获取的电影 ID。 然后，此代码使用 ID 来读取出数据库对应的电影和显示 （预加载） 它在文本框中。

第二个函数是，当用户单击**提交更改**按钮，代码必须读取的值的文本框中，并对其进行验证。 也必须对代码以使用新值更新的数据库项目。 这种技术很类似于添加记录，正如你在看到*AddMovie*。

## <a name="adding-code-to-read-a-single-movie"></a>添加代码以读取单个电影

若要执行的第一个函数，请将此代码添加到页的顶部：

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

此代码的大多数是在启动一个块内`if(!IsPost)`。 `!`运算符意味着"不，"因此表达式意味着*如果此请求不是 post 提交*，即，显示的间接方法*此请求如果是首次运行此页的*。 如前文所述，应运行此代码*仅*首次运行页面。 如果你未将包含中的代码`if(!IsPost)`，它将运行每次页调用时，是否第一次，或者在响应至一个按钮单击。

请注意，代码包含`else`阻止这一次。 正如我们所说当我们引入了`if`块，有时你想要运行替代代码，如果您要测试的条件不是如此。 这是这种情况。 如果该条件通过 （即，如果传递到页面的 ID 为确定），你可以从数据库读取行。 但是，如果不满足条件，`else`运行块和代码设置一条错误消息。

## <a name="validating-a-value-passed-to-the-page"></a>验证传递到页面的值

该代码使用`Request.QueryString["id"]`获取传递给页面的 ID。 代码可确保实际 id 传入的值 如果不传递任何值，该代码将设置验证错误。

此代码演示不同的方式，以验证信息。 在前面的教程，你使用过`Validation`帮助器。 你注册字段，若要验证，并 ASP.NET 自动未验证，并通过使用显示错误`Html.ValidationMessage`和`Html.ValidationSummary`。 在这种情况下，但是，你要实际上不验证用户输入。 相反，你要验证一个值，从其他位置传递到页。 `Validation`帮助器不会将出此为您。

因此，你自行检查值，通过测试它与`if(!Request.QueryString["ID"].IsEmpty()`)。 如果没有问题，你可以通过使用显示错误`Html.ValidationSummary`，就像处理`Validation`帮助器。 若要做到这一点，你调用`Validation.AddFormError`并将其传递要显示的消息。 `Validation.AddFormError`是，您可以定义自定义消息，同时结合使用你已经熟悉了验证系统的内置方法。 （本教程中稍后我们将讨论如何使此验证过程变得更加可靠。）

确保电影的 ID 后, 的代码读取数据库，为单个数据库项查找。 (你可能已经注意到数据库操作的常规模式： 打开数据库，定义 SQL 语句，然后运行该语句。)这一次，SQL`Select`语句包括`WHERE ID = @0`。 因为该 ID 是唯一的则可以返回只有一条记录。

查询执行使用`db.QuerySingle`(不`db.Query`，如用于影片列表)，并代码将放入结果`row`变量。 名称`row`是任意; 您喜欢的任何名称变量。 在顶部初始化的变量然后填入电影详细信息，以便可以在文本框中显示这些值。

## <a name="testing-the-edit-page-so-far"></a>测试的编辑页 （为止）

如果你想要测试你的页面，运行*电影*现在页上，单击**编辑**任何电影旁边的链接。 你将看到*EditMovie*具有详细信息页填充为所选的电影：

![编辑显示要编辑的电影的电影页](updating-data/_static/image3.png)

请注意，页面的 URL 将包含类似`?id=10`（或其他一些号码）。 到目前为止，你已测试的**编辑**链接*电影*页上工作、 从查询字符串中，你的页面进行读取 ID 和数据库查询以获取单个影片记录是否正常工作。

你可以更改影片信息中，但不执行任何操作时你单击**提交更改**。

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>添加代码以使用用户的更改更新电影

在*EditMovie.cshtml*文件中，若要实现 （保存更改） 的第二个函数，添加下面的代码内的右大括号`@`块。 (如果你不确定如何将代码放，你可以查看[完整代码清单编辑影片页](#Complete_Page_Listing_for_EditMovie)显示在本教程末尾上。)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

同样，此标记和代码是类似于中的代码*AddMovie*。 代码位于`if(IsPost)`阻止，因为此代码仅在用户单击时，才运行**提交更改**按钮&mdash;，即，当 （并仅当） 发送窗体。 在这种情况下，你不使用如测试`if(IsPost && Validation.IsValid())`— 也就是说，您将不进行合并两个测试均通过使用 and。 在此页上，你首先确定是否存在窗体提交 (`if(IsPost)`)，并仅然后注册以进行验证的字段。 然后你可以测试将验证结果 (`if(Validation.IsValid()`)。 流的方向是与中略有不同*AddMovie.cshtml*页上，但效果的方式相同。

使用获取的值的文本框中`Request.Form["title"]`和其他的类似代码`<input>`元素。 请注意，此时，代码获取的电影 ID 外的隐藏字段 (`<input type="hidden">`)。 当页上运行时第一次时，代码将收到查询字符串外的 ID。 从隐藏的字段，以确保您将获得的影片的最初所显示的 ID，以防从那时起以某种方式更改查询字符串中获取的值。

确实太重要区别*AddMovie*代码和此代码是在此代码中使用 SQL`Update`语句而不是`Insert Into`语句。 下面的示例演示的 sql 语法`Update`语句：

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

还可以按任意顺序指定任何列并不一定必须更新在每个列`Update`操作。 (因为，实际上将记录保存为一个新的记录，并且不允许的无法更新 ID 本身，`Update`操作。)

> [!NOTE] 
> 
> **重要**`Where`子句中的 ID 为非常重要，因为这是如何数据库知道哪个数据库记录你想要更新。 如果您离开`Where`子句，数据库将更新*每个*记录在数据库中。 在大多数情况下，这个位置是灾难。


在代码中，通过使用占位符要更新的值传递给 SQL 语句。 若要重复我们之前讲： 出于安全原因，*仅*占位符用于将值传递给 SQL 语句。

该代码使用后`db.Execute`运行`Update`语句，它将重定向回列表页上，你可以在其中看到更改。

> [!TIP] 
> 
> **不同的 SQL 语句，不同的方法**
> 
> 你可能已经注意到使用略有不同的方法运行不同的 SQL 语句。 若要运行`Select`查询，它可能返回多个记录，则使用`Query`方法。 若要运行`Select`你知道的查询将返回只有一个数据库项目，则使用`QuerySingle`方法。 若要运行的命令进行更改，但是，不返回数据库项目，你可以使用`Execute`方法。
> 
> 你必须具有不同的方法，因为每个返回不同的结果，正如你已看到之间的差异`Query`和`QuerySingle`。 (`Execute`方法实际上也返回一个值&mdash;即受该命令的数据库行数&mdash;但你已被忽略，到目前为止。)
> 
> 当然，`Query`方法可能返回只有一行的数据库。 但是，ASP.NET 始终会将结果`Query`作为集合的方法。 即使该方法返回一个行，必须从集合中提取该单个行。 因此，在情况下，你*知道*您就会得到只有一个行，它就会执行更方便地使用`QuerySingle`。
> 
> 有几个其他执行特定类型的数据库操作的方法。 你可以查找数据库中的方法的列表[ASP.NET 网页 API 快速参考](../../api-reference/asp-net-web-pages-api-reference.md#Data)。


## <a name="making-validation-for-the-id-more-robust"></a>使验证 ID 更可靠

页将运行，第一次你的电影 ID 从获取查询字符串，以便你可以从数据库中获取该影片。 做确保存在实际的值转查找，未使用此代码：

[!code-csharp[Main](updating-data/samples/sample13.cs)]

此代码用于可以确保，如果用户获取到*EditMovies*页，不需要先选择中的一个影片*电影*页上，页面将显示用户友好错误消息。 （否则，用户将看到一个错误，也许只需将它们混淆。）

但是，此验证不十分可靠。 此外可能出现这些错误调用页：

- ID 不是数字。 例如，页上，也可以调用，如将 URL `http://localhost:nnnnn/EditMovie?id=abc`。
- ID 是一个数字，但是它引用不存在一部电影 (例如， `http://localhost:nnnnn/EditMovie?id=100934`)。

如果你想要查看从这些 Url，运行产生的错误*电影*页。 选择一部电影，若要编辑，，然后更改的 URL *EditMovie*向包含字母的 URL page 的 ID 或不存在电影 ID。

因此，你该怎么办？ 第一个解决方法是确保，不仅能将 ID 传递到页中，但 ID 是一个整数。 更改的代码`!IsPost`测试看起来如下例所示：

[!code-csharp[Main](updating-data/samples/sample14.cs)]

你已添加到第二个条件`IsEmpty`测试，与链接`&&`（逻辑与）：

[!code-csharp[Main](updating-data/samples/sample15.cs)]

您可能还记得从[为 ASP.NET 网页编程的介绍](../introducing-razor-syntax-c.md)等方法的教程`AsBool``AsInt`将字符字符串转换为其他数据类型。 `IsInt`方法 (和等其他一些`IsBool`和`IsDateTime`) 类似。 但是，它们仅用于测试是否你*可以*转换为字符串，而不实际执行转换。 这里基本上就*如果查询字符串值可以转换为整数...*.

不存在一部电影查找其他潜在问题。 若要获取一部电影的代码看起来像此代码：

[!code-csharp[Main](updating-data/samples/sample16.cs)]

如果你通过`movieId`值赋给`QuerySingle`不对应于实际的电影的方法，未返回任何内容且符合语句 (例如， `title=row.Title`) 会产生错误。

同样是简单的解决方法。 如果`db.QuerySingle`方法会返回任何结果，`row`变量将为 null。 这样你就可以检查是否`row`变量为 null，再尝试从其获取值。 下面的代码添加`if`围绕获取外的值的语句块`row`对象：

[!code-csharp[Main](updating-data/samples/sample17.cs)]

利用这些两个其他验证测试时，页面将成为更防护。 完整代码`!IsPost`分支现在看起来如下例所示：

[!code-csharp[Main](updating-data/samples/sample18.cs)]

此任务是一个很好用法。 我们将需要再次注意`else`块。 如果测试未通过，`else`块设置错误消息。

## <a name="adding-a-link-to-return-to-the-movies-page"></a>添加的链接以返回电影页

最终且有帮助的详细信息是将链接添加回*电影*页。 在普通的事件流中，用户将开始在*电影*页上，单击**编辑**链接。 将用户带到*EditMovie*页上，它们可以在其中编辑电影并单击按钮。 代码处理此更改后，它将重定向回*电影*页。

但是：

- 用户可能会决定不更改任何内容。
- 用户可能收到此页面没有先单击**编辑**中链接*电影*页。

无论哪种方式，你想要使其可以方便地返回到主列表。 很容易修复&mdash;结束之后添加以下标记`</form>`标记中的标记：

[!code-html[Main](updating-data/samples/sample19.html)]

此标记使用的相同语法`<a>`在其他位置，你已了解的元素。 URL 包含`~`表示"在网站的根目录。"

## <a name="testing-the-movie-update-process"></a>测试电影更新过程

现在，你可以测试。 运行*电影*页，然后单击**编辑**旁边影片。 当*EditMovie*页出现时，更改到电影，然后单击**提交更改**。 影片列表出现时，请确保所做的更改，将显示。

若要确保验证正常工作，请单击**编辑**为另一个的电影。 当你到达*EditMovie*页上，清除**流派**字段 (或**年**字段，或两者)，然后尝试提交所做的更改。 如你所料，你将看到一个错误：

![编辑显示验证错误的影片页](updating-data/_static/image4.png)

单击**返回到影片列表**链接以放弃所做的更改并返回到*电影*页。

## <a name="coming-up-next"></a>接下来

在下一步的教程中，你将了解如何删除电影记录。

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>电影页 （经过更新带有编辑链接） 的完整列表

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>完成编辑影片页的页列表

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>其他资源

- [使用 Razor 语法的 ASP.NET Web 编程简介](../../getting-started/introducing-razor-syntax-c.md)
- [SQL UPDATE 语句](http://www.w3schools.com/sql/sql_update.asp)W3Schools 站点上

>[!div class="step-by-step"]
[上一页](entering-data.md)
[下一页](deleting-data.md)
