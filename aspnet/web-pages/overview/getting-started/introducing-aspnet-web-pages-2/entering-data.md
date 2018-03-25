---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: "引入的 ASP.NET Web Pages-通过使用窗体中输入数据库的数据 |Microsoft 文档"
author: tfitzmac
description: "本教程演示如何创建的输入表单，然后输入你从获取窗体插入数据库表时使用 ASP.NET Web Pages （...的数据"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: b74eecb16b2c4695bb417816b90f701f724cc9d0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>引入的 ASP.NET Web Pages-通过使用窗体中输入数据库的数据
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本教程演示如何创建的输入表单，然后输入你从获取窗体插入数据库表时使用 ASP.NET Web 页 (Razor) 的数据。 它假定你已完成通过系列[基础知识的 HTML 窗体中的 ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581)。
> 
> 你将学习：
> 
> - 有关如何处理输入窗体的详细信息。
> - 如何在数据库中添加 （插入） 数据。
> - 如何确保用户已在窗体 （如何验证用户输入） 中输入所需的值。
> - 如何显示验证错误。
> - 如何从当前页跳转到另一页。
>   
> 
> 功能/技术讨论：
> 
> - `Database.Execute` 方法。
> - SQL`Insert Into`语句
> - `Validation`帮助器。
> - `Response.Redirect` 方法。


## <a name="what-youll-build"></a>你将生成

通过编辑直接在 WebMatrix 中，在中工作的数据库的数据库数据输入在本教程之前，向您展示如何创建数据库，**数据库**工作区。 在大多数应用中，这不是将数据放入数据库，但切实可行的方法。 因此在本教程中，你将创建一个让您或任何人输入数据并将其保存到数据库的基于 web 的界面。

你将创建可以在其中输入新影片的页。 此页将包含具有的字段 （文本框） 你可以在其中输入影片标题、 genre 和年的输入表单。 页面将如下所示此页：

![在浏览器中的添加电影页](entering-data/_static/image1.png)

文本框将 HTML`<input>`之类此标记的外观的元素：

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>创建基本条目窗体

创建一个名为页*AddMovie.cshtml*。

将替换为以下标记文件中。 覆盖所有内容;你将很快在顶部添加代码块。

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

此示例显示创建窗体的典型的 HTML。 它使用`<input>`元素的文本框和提交按钮。 对文本框的标题由使用标准`<label>`元素。 `<fieldset>`和`<legend>`元素放置在窗体 nice 框的位置。

请注意，在此页上，`<form>`元素使用`post`的值作为`method`属性。 在前面的教程，你已创建一个窗体，`get`方法。 正是这样正确，因为虽然窗体提交到服务器的值，请求未做任何更改。 所有那样提取数据时不同的方式。 但是，在此页你*将*进行更改-你要添加新的数据库记录。 因此，应使用此窗体`post`方法。 (有关详细信息之间的差异`GET`和`POST`操作，请参阅[GET、 POST 和 HTTP 谓词安全](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety)边栏中前面的教程。)

请注意，每个文本框有`name`元素 (`title`， `genre`， `year`)。 如你在前面的教程中看到，这些名称很重要，因为您必须使用这些名称，因此你可以稍后获取用户的输入。 你可以使用任何名称。 将对有所帮助使用有意义名称帮助你记住你正在使用哪些数据。

`value`每个属性`<input>`元素包含的 Razor 代码 (例如， `Request.Form["title"]`)。 你已了解此操作的版本中以前的教程，若要保留 （如果有） 在文本框中输入的值后提交表单。

## <a name="getting-the-form-values"></a>获取窗体值

接下来，你添加处理窗体的代码。 在大纲，你将执行以下操作：

1. 检查是否正在发页面 （已提交）。 你想到你的代码仅在用户在单击此按钮时，不在页面首次运行时运行。
2. 获取用户输入的文本框中的值。 在这种情况下，因为使用该窗体`POST`谓词，你将获取窗体值从`Request.Form`集合。
3. 作为一条新记录中插入值*电影*数据库表。

在文件的顶部，添加以下代码：

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

前几行创建变量 (`title`， `genre`，和`year`) 以从文本框中保存的值。 行`if(IsPost)`可确保将设置的变量*仅*当用户单击**添加电影**按钮-即，当在窗体具有已发送。

正如你看到在较早的教程，你通过使用等表达式获取文本框中的值`Request.Form["name"]`，其中*名称*是的名称`<input>`元素。

变量的名称 (`title`， `genre`，和`year`) 是任意的。 如你将分配给名称`<input>`元素，则可以您喜欢的任何对其进行调用。 (变量的名称不必与匹配的名称属性`<input>`窗体上的元素。)但与`<input>`元素，它将使用反映它们所包含的数据的变量名称的一个好办法。 当你编写代码时，一致名称会使它可以更轻松地记住你正在使用哪些数据。

## <a name="adding-data-to-the-database"></a>向数据库添加数据

在代码中阻止您刚刚添加，请直接*内*右大括号 ( `}` ) 的`if`块 （而不仅仅是在代码块中） 中，添加以下代码：

[!code-csharp[Main](entering-data/samples/sample3.cs)]

此示例是类似于在提取和显示数据的上一个教程中使用的代码。 开头的行`db =`打开的数据库，如之前，和下一步的行定义 SQL 语句，再次为你之前看到。 但是，这次它定义 SQL`Insert Into`语句。 下面的示例演示的常规语法`Insert Into`语句：

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

换而言之，你指定的表将插入，然后列出的列将插入，并列出要插入的值。 （如前面提到的那样，SQL 不区分大小写，但一些人利用要更加轻松地阅读该命令的关键字。）

要插入的列已列在该命令- `(Title, Genre, Year)`。 有趣的部分是如何从到文本框中获取值`VALUES`命令的一部分。 而非实际值，你看到`@0`， `@1`，和`@2`，它们是当然的占位符。 当你运行该命令 (上`db.Execute`行)，传递从文本框中获取的值。

**重要 ！** 请记住，您应曾经包含数据输入的 SQL 语句中的用户的联机的唯一方法是使用占位符，如下所示 (`VALUES(@0, @1, @2)`)。 如果你连接到 SQL 语句的用户输入，则打开自己容易受到 SQL 注入攻击中, 所述[窗体基础知识中的 ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581) （前面的教程）。

仍内`if`块中，添加以下行后的`db.Execute`行：

[!code-css[Main](entering-data/samples/sample4.css)]

新的影片插入数据库后，此行将跳转 （重定向） 到*电影*页，以便你可以看到刚才输入的影片。 `~`运算符意味着"在网站的根目录。" (`~`运算符仅在 ASP.NET 页中，不在 HTML 中通常有效。)

完整的代码块的外观如下例所示：

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>测试插入命令 （为止）

你还未完成，但现在是测试的好时机。

在 WebMatrix 中的文件树视图中，右键单击*AddMovie.cshtml*页，然后单击**在浏览器中启动**。

![在浏览器中的添加电影页](entering-data/_static/image2.png)

(如果你最终浏览器中的其他页，请确保该 URL 是否`http://localhost:nnnnn/AddMovie`)，其中 *nnnnn* 是你使用的端口号。)

未获取了一个错误页面？ 如果是这样，仔细阅读它，并确保，代码看上去完全什么已前面列出。

在窗体中输入一部电影&mdash;例如，使用"公民 Kane"、"戏曲"和"1941"。 （或任何。）然后单击**添加电影**。

如果一切运行正常，你要重定向到*电影*页。 请确保列出了新的影片。

![显示新的电影页添加电影](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>验证用户输入

返回到*AddMovie*页上，或再次运行。 另一电影，但这次，请输入仅标题&mdash;例如，输入"上线 ' 中 Rain"。 然后单击**添加电影**。

要重定向到*电影*再次页。 你可以找到新的影片，但不完整。

![电影页上显示缺少某些值的新影片](entering-data/_static/image4.png)

你在创建时*电影*表，您显式所说的字段均不可以为 null。 此处你具有新影片的输入表单，并且你要将字段保留为空白。 则是错误。

在这种情况下，数据库实际上未引发 (或*引发*) 错误。 你未因此提供流派或年中的代码*AddMovie*页视为这些值所谓*空字符串*。 当 SQL`Insert Into`运行命令、 genre 和年份字段中，没有有用的数据，但它们并不为 null。

显然，你不想让用户半空影片信息输入到数据库。 该解决方案旨在验证用户的输入。 最初，验证将只需确保用户具有的所有字段中都输入的值 （即，其中未包含一个空字符串）。

> [!TIP] 
> 
> **Null 和空字符串**
> 
> 在编程中，没有"没有值。"的不同概念之间的差异 通常情况下，值是*null*如果它已永远不会设置或以任何方式初始化。 与此相反，需要字符数据 （字符串） 的变量可以设置为*空字符串*。 在这种情况下，值不为 null;它只是已显式设置为其长度为零的字符串。 这两个语句显示的差异：
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> 它具有较为复杂一些，但重要的一点是`null`表示一种不确定状态。
> 
> 现在，然后务必了解完全当一个值为 null 且时只是一个空字符串。 中的代码*AddMovie*页上，你通过使用获取的值的文本框中`Request.Form["title"]`，依此类推。 在页面首次运行 （在你单击的按钮） 之前、 的值`Request.Form["title"]`为 null。 但当你提交该表单，`Request.Form["title"]`获取的值`title`文本框。 它不是很明显，但的空文本框不为 null;它只包含一个空字符串。 因此当代码运行以响应按钮单击，`Request.Form["title"]`中有一个空字符串。
> 
> 为什么这一区别很重要？ 你在创建时*电影*表，您显式所说的字段均不可以为 null。 此处有新的影片的输入表单，但要将字段保留为空白。 规范的合理预期要抱怨当你尝试保存没有流派或年的值的新影片时的数据库。 但它是点&mdash;即使将这些文本框留空，值不为空; 它们是空字符串。 因此，你可以将保存到空这些列的数据库的新影片&mdash;不为 null，但 ！ &mdash; 值。 因此，你必须确保用户未提交空字符串，可以通过验证用户的输入来执行此操作。


### <a name="the-validation-helper"></a>验证帮助器

ASP.NET Web Pages 包括一个帮助程序&mdash;`Validation`帮助器&mdash;可用于确保用户输入满足你的要求的数据。 `Validation`帮助程序是一个内置到 ASP.NET Web 页中，因此无需通过使用 NuGet，在前面的教程安装 Gravatar 帮助器的方式将其安装作为包的帮助器。

若要验证用户的输入，将执行以下操作：

- 使用代码指定你希望要求在页面上的文本框中的值。
- 放在代码的测试，以便影片信息添加到数据库中，仅当所有内容正确验证。
- 将代码添加到的标记来显示错误消息。

中的代码块中*AddMovie*页上，向右向上在变量声明中之前, 的顶部添加以下代码：

[!code-csharp[Main](entering-data/samples/sample7.cs)]

你调用`Validation.RequireField`一次针对每个字段 (`<input>`元素) 想要要求一个条目。 你还可以添加每个调用，如你在此处看到自定义错误消息。 （我们不同的消息仅为演示您可以将放置在您喜欢的任何存在）。

如果没有问题，你想要防止新影片信息插入到数据库。 在`if(IsPost)`块中，使用`&&`（逻辑与） 添加另一个测试的条件`Validation.IsValid()`。 完成后，整个`if(IsPost)`块看上去类似此代码：

[!code-csharp[Main](entering-data/samples/sample8.cs)]

如果没有与通过使用已注册的字段的任意验证错误`Validation`帮助器，`Validation.IsValid`方法返回 false。 在这种情况下，无该块中的代码将运行，从而使没有无效电影项将插入到数据库。 和当然你在不重定向到*电影*页。

完整的代码块，包括验证代码，现在看起来如下例所示：

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>显示验证错误

最后一步是显示任何错误消息。 你可以显示有关每个验证错误时，单个消息，也可以显示摘要，或两个。 对于本教程，你将执行同时，以便你可以查看其工作方式。

每个旁边`<input>`要验证的元素，请调用`Html.ValidationMessage`方法并将其传递的名称`<input>`要验证的元素。 你将`Html.ValidationMessage`方法右想显示的错误消息。 当运行此页，则`Html.ValidationMessage`方法呈现`<span>`的目的地的验证错误的元素。 (如果没有错误，`<span>`元素呈现，但在其中没有任何文本。)

更改页面中的标记，以使其包括`Html.ValidationMessage`方法为每个的三个`<input>`元素在页面上，如下例所示：

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

若要查看汇总的工作原理，还添加以下标记和代码后`<h1>Add a Movie</h1>`页面上的元素：

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

默认情况下，`Html.ValidationSummary`方法列表中显示验证的所有消息 (`<ul>`内的元素`<div>`元素)。 与`Html.ValidationMessage`方法，始终呈现验证摘要的标记; 如果没有错误，没有列表项呈现。

该摘要可以是通过使用显示验证消息，而不是一种备用方法`Html.ValidationMessage`方法以显示每个字段特定的错误。 或者，你可以使用摘要和详细信息。 也可以使用`Html.ValidationSummary`方法显示一个一般性错误，然后使用单个`Html.ValidationMessage`调用以显示详细信息。

完成页现在看起来如下例所示：

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

介绍完毕。 你现在可以通过添加一部电影，但是省去一个或多个字段测试页。 执行操作时，你将看到显示以下错误：

![将添加影片页显示验证错误消息](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>样式的验证错误消息

你可以看到，错误消息，但它们不真正与众不同优良的性能。 没有一种样式的错误消息，但简便方法。

设置显示的单独错误消息的样式`Html.ValidationMessage`，创建一个名为的 CSS 样式类`field-validation-error`。 若要定义查找验证摘要，创建一个名为的 CSS 样式类`validation-summary-errors`。

若要查看此方法的工作原理，添加`<style>`内的元素`<head>`页的部分。 然后定义名为的样式类`field-validation-error`和`validation-summary-errors`包含以下规则：

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

通常情况下你可能会将放入一个单独的样式信息*.css*文件，但为简单起见你可以将它们放在页现在。 (更高版本中设置此教程，您就可以单独切换的 CSS 规则*.css*文件。)

如果没有验证错误，`Html.ValidationMessage`方法呈现`<span>`包含元素`class="field-validation-error"`。 通过添加为该类的样式定义，你可以配置消息如下所示。 如果存在错误，`ValidationSummary`方法同样动态呈现属性`class="validation-summary-errors"`。

再次运行此页并有意使出几个字段。 错误现在是更明显的。 （事实上，它们 overdone，但这是仅为演示你可以执行的操作。）

![将添加影片页显示了风格的验证错误](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>将链接添加到电影页

一个最后一步是为了方便可用于访问*AddMovie*页从原始的影片列表。

打开*电影*再次页。 之后，结束`</div>`标记后面`WebGrid`帮助器，添加以下标记：

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

正如你看到之前，ASP.NET 将解释`~`运算符用作在网站的根目录。 无需使用`~`运算符; 你可以使用标记`<a href="./AddMovie">Add a movie</a>`或某种其他方式来定义 HTML 理解的路径。 但是`~`运算符是一种好的常规方法时创建链接对于 Razor 页，因为它使站点更灵活-如果将当前页移到一个子文件夹中，将仍将链接转到*AddMovie*页。 (请记住，`~`运算符仅适用于*.cshtml*页。 ASP.NET 理解它，但它不是标准 HTML）。

完成后，运行*电影*页。 它将类似此页：

![指向添加电影页面链接的电影页](entering-data/_static/image7.png)

单击**添加影片**链接，以确保将转到*AddMovie*页。

## <a name="coming-up-next"></a>接下来

在下一步的教程中，你将了解如何以使用户可以编辑已在数据库中的数据。

## <a name="complete-listing-for-addmovie-page"></a>AddMovie 页的完整列表

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>其他资源

- [使用 Razor 语法的 ASP.NET Web 编程简介](https://go.microsoft.com/fwlink/?LinkID=202890)
- [INTO 语句中插入的 SQL](http://www.w3schools.com/sql/sql_insert.asp) W3Schools 站点上
- [验证用户输入，在 ASP.NET Web 页站点](https://go.microsoft.com/fwlink/?LinkId=253002)。 有关如何使用的详细信息`Validation`帮助器。

>[!div class="step-by-step"]
[上一页](form-basics.md)
[下一页](updating-data.md)
