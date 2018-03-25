---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: "引入的 ASP.NET Web Pages-HTML 窗体基础知识 |Microsoft 文档"
author: tfitzmac
description: "本教程演示如何创建的输入的窗体以及如何处理用户的输入，当你使用 ASP.NET Web 页 (Razor) 的基础知识。 和现在，您..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: 68056759b2e80230e5fd2c0f9b2d2a89b549cf37
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a>引入 ASP.NET Web 页的 HTML 窗体基础知识
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本教程演示如何创建的输入的窗体以及如何处理用户的输入，当你使用 ASP.NET Web 页 (Razor) 的基础知识。 现在，你已收到数据库，你将使用你窗体的技能让用户在数据库中查找特定电影。 它假定你已完成通过系列[简介到显示数据使用的 ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data)。
> 
> 你将学习：
> 
> - 如何通过使用标准的 HTML 元素创建的窗体。
> - 如何读取用户的窗体中输入。
> - 如何创建 SQL 查询，有选择地获取数据使用的搜索词的用户提供。
> - 如何在页中"记住"用户输入的字段。
>   
> 
> 功能/技术讨论：
> 
> - `Request` 对象。
> - SQL`Where`子句。


## <a name="what-youll-build"></a>你将生成

在以前的教程中，创建数据库，将数据添加到它，，然后使用`WebGrid`帮助器以显示数据。 在本教程中，你将添加一个搜索框，可让你查找的特定风格的影片或其标题中包含你输入任何字符。 （例如，你将能够查找所有电影其风格是"操作"或其标题包含"Harry"Adventure"。）

完成本教程后，将会得到如下所示的页面：

![电影 Genre 和标题搜索页](form-basics/_static/image1.png)

页的列表部分是与最后一个教程中的相同&mdash;网格。 将网格将显示仅电影您搜索的差异。

## <a name="about-html-forms"></a>有关 HTML 窗体

(如果你具有与创建 HTML 窗体和之间的差异的体验`GET`和`POST`，则可以跳过此部分。)

表单具有用户输入的元素&mdash;文本框、 按钮、 单选按钮、 复选框，下拉列表和等等。 用户填写这些控件或进行选择并单击一个按钮，然后提交窗体。

此示例中所示的窗体的基本 HTML 语法：

[!code-html[Main](form-basics/samples/sample1.html)]

在页中运行此标记，将创建一个简单窗体看上去像此图中：

![为呈现在浏览器中的基本 HTML 窗体](form-basics/_static/image2.png)

`<form>`元素包含要提交的 HTML 元素。 (若要使易犯错误是向页面添加元素，但然后忘记将它们放`<form>`元素。 在这种情况下，执行任何操作提交。）`method`属性告知浏览器如何提交用户输入。 将此设置为`post`如果要执行的更新服务器上或`get`如果你只需从服务器提取数据。

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **GET、 POST 和 HTTP 谓词安全性**
> 
> HTTP，浏览器和服务器使用交换信息，该协议是非常简单的其基本操作。 使用仅几个谓词的浏览器以向服务器发出请求。 当你编写适用于 web 的代码时，是有助于了解这些谓词和如何在浏览器和服务器使用它们。 距离遥远的最常使用的谓词是这些：
> 
> - `GET`。 浏览器使用此谓词来从服务器提取的内容。 例如，当你键入 URL 在浏览器，浏览器将执行`GET`操作请求所需的页。 如果该页面包括图形，浏览器将执行其他`GET`获取映像的操作。 如果`GET`操作具有要将信息传递给服务器，将信息传递作为查询字符串中的 URL 的一部分。
> - `POST`。 浏览器发送`POST`请求，以便提交要添加或更改服务器上的数据。 例如，`POST`谓词用于在数据库中创建记录或更改现有的。 大多数情况下，当您填写表单，并单击提交按钮，浏览器执行`POST`操作。 在`POST`操作，正在传递给服务器的数据位于页的正文。
> 
> 这些动词之间的一个重要区别在于，`GET`操作不应更改服务器上的任何内容，或将其放在稍微抽象的方式，`GET`操作未导致服务器上的状态的更改。 你可以执行`GET`多次您喜欢，并且不会更改这些资源的同一资源上的操作。 (A`GET`操作通常称为"安全，"或者可以使用技术术语，是*幂等*。)相反，当然，`POST`内容服务器上执行该操作每次时请求更改。
> 
> 两个示例将帮助阐释这一区别。 执行搜索时使用的引擎，如必应或 Google，填写包含一个文本框的窗体，然后单击搜索按钮。 浏览器执行`GET`操作，作为 URL 的一部分传递在框中输入的值。 使用`GET`操作为此类型的窗体不错，因为搜索操作不会更改服务器上的任何资源，它只提取信息。
> 
> 现在考虑排序联机的内容的过程。 你填写订单详细信息，然后单击提交按钮。 此操作将`POST`请求，因为该操作将导致在服务器上，如新的顺序记录、 你的帐户信息中的更改和可能是许多其他更改的更改。 与不同`GET`操作，不能重复你`POST`请求-如果你这样做，重新提交请求，每次会生成服务器上的新订单。 （在这种情况下，网站通常将警告你不要单击提交按钮一次以上，或将禁用提交按钮，以便不会意外地重新提交窗体。）
> 
> 在本教程的过程中你将使用同时`GET`操作和`POST`操作用于 HTML 窗体。 我们将介绍在每个事例的原因你使用的谓词是适合的选项。
> 
> (若要了解有关 HTTP 谓词的详细信息，请参阅[方法定义](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)W3C 网站上的文章。)


大多数用户输入的元素为 HTML`<input>`元素。 它们看起来像`<input type="type" name="name">,`其中*类型*指示所需的用户输入控件的种类。 这些元素包括的常见事件：

- 文本框中：`<input type="text">`
- 复选框：`<input type="check">`
- 单选按钮：`<input type="radio">`
- 按钮：`<input type="button">`
- 提交按钮：`<input type="submit">`

你还可以使用`<textarea>`元素以创建多行文本框和`<select>`元素来创建一个下拉列表或可滚动的列表。 (有关更多关于 HTML 窗体元素中，请参阅[HTML 窗体和输入](http://www.w3schools.com/html/html_forms.asp)W3Schools 站点上。)

`name`属性是非常重要，因为该名称是如何将获取更高版本，元素的值为很快就会看到。

有趣的一部分是你，即页开发人员，使用用户的输入所执行的操作。 没有与这些元素关联的任何内置行为。 相反，你需要获取用户输入或选择的值并使用它们执行某些操作。 这是在本教程中将要掌握的内容。

> [!TIP] 
> 
> **HTML5 和输入的窗体**
> 
> 你可能知道，如 HTML 处于过渡状态，最新版本 (HTML5) 包括对更直观的方式将用户输入信息的支持。 例如，在 HTML5，你 （即页开发人员） 可以告诉页你希望用户输入的日期。 然后可以自动显示浏览器，日历，而不是要求用户手动输入日期。 但是，HTML5 是新，并且尚不支持在所有浏览器中。
> 
> ASP.NET Web Pages 支持 HTML5 的范围内用户的浏览器未输入。 新特性的了解`<input>`元素 html5 格式，请参阅[HTML&lt;输入&gt;键入属性](http://www.w3schools.com/html/html_form_input_types.asp)W3Schools 站点上。


## <a name="creating-the-form"></a>创建窗体

在 WebMatrix 中，在**文件**工作区中，打开*Movies.cshtml*页。

之后，结束`</h1>`标记和在打开之前`<div>`标记`grid.GetHtml`调用，添加以下标记：

[!code-html[Main](form-basics/samples/sample2.html)]

此标记创建具有一个名为的文本框的窗体`searchGenre`和提交按钮。 文本框中，并提交按钮括在`<form>`元素其`method`属性设置为`get`。 (请记住，如果你没有将文本框中，然后提交按钮位于内的`<form>`元素中，单击按钮时将提交执行任何操作。)你使用`GET`谓词此处因为你要创建窗体，不进行任何更改在服务器上-它只需在搜索结果。 (在前面的教程，你使用了`post`方法，即如何提交到服务器的更改。 你将看到，在下一步教程再次。）

运行页面。 虽然你尚未定义窗体的任何行为，但是你可以看到如下所示：

![电影页流派的搜索框](form-basics/_static/image3.png)

输入一个值，在文本框中，如"喜剧。" 然后单击**搜索流派**。

记下页面的 URL。 由于已将设置`<form>`元素的`method`属性设为`get`，您输入的值现在是在 URL 中，如下查询字符串的一部分：

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>读取窗体值

此页已包含一些代码，获取数据库数据并在网格中显示结果。 现在，您需要添加一些代码，以便可以运行包括搜索词的 SQL 查询读取文本框的值。

因为窗体的方法设置为`get`，你可以读取已通过使用类似于下面的代码输入到文本框中的值：

`var searchTerm = Request.QueryString["searchGenre"];`

`Request.QueryString`对象 (`QueryString`属性`Request`对象) 包括作为的一部分提交的元素的值`GET`操作。 `Request.QueryString`属性包含*集合*（列表） 在窗体中提交的值。 若要获取任何单个值，你可以指定所需的元素的名称。 这就是为什么你必须拥有`name`属性`<input>`元素 (`searchTerm`) 创建文本框。 (有关详细信息`Request`对象，请参阅[边栏](#BKMK_TheRequestObject)更高版本。)

它非常简单，可以读取的文本框中的值。 但是，如果用户未输入任何内容根本在文本框中，但单击**搜索**，可以忽略该单击，因为无需进行任何搜索。

下面的代码是一个示例，演示如何实现这些条件。 （无需尚未添加此代码; 你将在稍后执行的。）

[!code-csharp[Main](form-basics/samples/sample3.cs)]

测试在这种方式中中断：

- 获取的值`Request.QueryString["searchGenre"]`，即已输入到的值`<input>`元素名为`searchGenre`。
- 了解它是否为空使用`IsEmpty`方法。 此方法是确定是否 （例如，窗体元素） 的内容包含值的标准方式。 但是你确实关心只有在它*不*为空，因此...
- 添加`!`前端中的运算符`IsEmpty`测试。 (`!`运算符意味着逻辑非)。

在简单地说，整个`if`条件将以下转换：*如果窗体的 searchGenre 元素不为空，然后...*

此块设置用于创建使用搜索词的查询的阶段。 在下一部分中，将执行该操作。

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **请求对象**
> 
> `Request`对象包含请求或提交页时，浏览器发送给你的应用程序的所有信息。 此对象包括用户提供，如文本框值或要上载的文件的任何信息。 它还包括各种类型的其他信息，如 cookie，URL 查询字符串 （如果有） 中的值正在运行，浏览器的用户正在使用，在浏览器中设置的语言的列表类型页的文件路径等等。
> 
> `Request`对象是*集合*（列表） 的值。 你可以通过指定其名称获取从该集合的单个值：
> 
> `var someValue = Request["name"];`
> 
> `Request`对象实际公开多个子集。 例如:
> 
> - `Request.Form`为你提供从内提交元素值`<form>`如果请求的元素`POST`请求。
> - `Request.QueryString`为你提供的值只是中的 URL 查询字符串。 (如 URL 中`http://mysite/myapp/page?searchGenre=action&page=2`、`?searchGenre=action&page=2`部分 URL 为查询字符串。)
> - `Request.Cookies`集合访问你的浏览器发送的 cookie。
> 
> 若要获取一个值，你知道是在提交窗体中，你可以使用`Request["name"]`。 或者，可以使用更具体版本`Request.Form["name"]`(有关`POST`请求) 或`Request.QueryString["name"]`(有关`GET`请求)。 当然，*名称*是要获取的项的名称。
> 
> 你想要获取的项的名称必须是唯一中正在使用的集合。 这就是为什么`Request`对象提供子集喜欢`Request.Form`和`Request.QueryString`。 假设你的页面包含名为窗体元素`userName`和*还*包含名为一个 cookie `userName`。 如果你收到`Request["userName"]`，它是不明确所需的窗体值或 cookie。 但是，如果你收到`Request.Form["userName"]`或`Request.Cookie["userName"]`，要被明确要获取的值。
> 
> 很好的做法进行特定并使用相同的子集`Request`你感兴趣，如`Request.Form`或`Request.QueryString`。 对于你要在本教程中创建的简单页面，它可能不真正带来任何差异。 但是，当您创建更复杂的页，使用的显式版本`Request.Form`或`Request.QueryString`可帮助你避免时的页面包含窗体 （或多个窗体），可能出现的问题 cookie、 查询字符串值和等等。


## <a name="creating-a-query-by-using-a-search-term"></a>通过使用的搜索词创建查询

既然你知道如何获取用户输入搜索词，可以创建使用它的查询。 请记住，若要获取出数据库的所有影片项，你正在使用如下所示此语句的 SQL 查询：

`SELECT * FROM Movies`

若要获取仅某些电影，你需要使用一个查询，包括`Where`子句。 此子句允许你设置由查询返回行的条件。 以下是一个示例：

`SELECT * FROM Movies WHERE Genre = 'Action'`

基本格式`WHERE column = value`。 除了只之外，可以使用不同运算符`=`、 like `>` （大于）、 `<` （小于）， `<>` （不等于）、 `<=` （小于或等于）、 等，具体取决于你正在寻找的内容。

如果您想知道，SQL 语句不区分大小写&mdash;`SELECT`相同`Select`(或甚至`select`)。 但是，用户通常利用关键字在 SQL 语句中，如`SELECT`和`WHERE`，以使其更易于阅读。

### <a name="passing-the-search-term-as-a-parameter"></a>作为参数传递的搜索词

搜索特定风格也很简单 (`WHERE Genre = 'Action'`)，但你想要能够搜索任何用户输入的风格。 若要做到这一点，创建时为 SQL 查询，其中包含要搜索的值的占位符。 它将类似此命令：

`SELECT * FROM Movies WHERE Genre = @0`

占位符所`@`字符跟零个。 您可能认为，查询可以包含多个占位符，它们将被命名为`@0`， `@1`， `@2`，等等。

若要设置的查询和实际传递的值，你可以使用代码如下所示：

[!code-sql[Main](form-basics/samples/sample4.sql)]

此代码将类似于所已做的网格中显示数据。 唯一的区别如下：

- 该查询包含一个占位符 (`WHERE Genre = @0"`)。
- 查询会被放置到一个变量 (`selectCommand`); 之前，你查询直接传递到`db.Query`方法。
- 当调用`db.Query`方法，需传递查询和要使用的占位符的值。 (如果查询具有多个占位符，会将它们传递所有项作为对方法的单独值。)

如果结合这些元素，将得到以下代码：

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **重要 ！** 使用占位符 (如`@0`) 若要将值传递给 SQL 命令*极其重要*的安全性。 你看到它在这里，带有占位符变量数据的方式是你应该构建 SQL 命令的唯一方法。
> 
> 永远不会通过将放在一起 （串联） 的文字文本和从用户获取的值来构造 SQL 语句。 连接到 SQL 语句的用户输入打开网站*SQL 注入式攻击*其中恶意用户提交到你的页 hack 你的数据库的值。 (你可以阅读更多的文章中[SQL 注入](https://msdn.microsoft.com/library/ms161953.aspx)MSDN 网站。)


## <a name="updating-the-movies-page-with-search-code"></a>使用搜索代码更新电影页

现在你可以更新中的代码*Movies.cshtml*文件。 若要开始，请将在页面顶部的代码块中的代码替换此代码：

[!code-csharp[Main](form-basics/samples/sample6.cs)]

此处的差异是你没有将放到查询`selectCommand`变量不同，后者将传递给`db.Query`更高版本。 将放置到一个变量中的 SQL 语句允许您更改语句，它将执行哪些操作来执行搜索。

你还删除了这两行，你将放回中更高版本：

[!code-csharp[Main](form-basics/samples/sample7.cs)]

你不想尚未运行的查询 (也就是说，调用`db.Query`) 并且你不想要初始化`WebGrid`帮助程序，但请。 你已理解的 SQL 语句都必须运行后，你将执行这些操作。

在此重写块后可以添加用于处理搜索新的逻辑。 已完成的代码将如下所示。 使其匹配此示例，请更新你的页面中的代码：

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

现在的页工作原理如下。 每次运行页面，代码将打开的数据库和`selectCommand`变量设置为获取中的所有记录的 SQL 语句`Movies`表。 此代码还初始化`searchTerm`变量。

但是，如果当前请求中包含的值`searchGenre`元素，该代码设置`selectCommand`到另一个查询 — 也就是说，为包含`Where`子句来搜索一种风格。 它还将设置`searchTerm`到任何传递的搜索框中 （这可能是执行任何操作）。

而不考虑哪些 SQL 语句不在`selectCommand`，然后，代码调用`db.Query`要运行查询，将其传递任何点在`searchTerm`。 如果中没有任何`searchTerm`，它并不重要，因为在这种情况下没有参数传递到值`selectCommand`仍。

最后，此代码初始化`WebGrid`帮助器通过使用查询结果，像之前一样。

你可以看到，通过将放在 SQL 语句和搜索词到变量中，你已向代码添加的灵活性。 正如您将看到更高版本在本教程中，你可以使用此基本框架，并且保留添加的搜索结果的不同类型的逻辑。

## <a name="testing-the-search-by-genre-feature"></a>测试通过流派搜索功能

在 WebMatrix 中，运行*Movies.cshtml*页。 你看到流派的文本框中的页。

输入已输入一个你测试的记录，然后单击一种风格**搜索**。 此时会显示仅匹配的影片电影的列表：

![列出搜索 genre Comedies 后的电影页](form-basics/_static/image4.png)

输入不同的流派，然后再次搜索。 请尝试使用所有小写字母或全部大写的字母，这样您可以看到搜索不区分大小写输入风格。

## <a name="remembering-what-the-user-entered"></a>"记住"用户输入

你可能已经注意到，之后您输入流派，并单击**搜索流派**，你看到的影片列表。 但是，搜索文本框中为空&mdash;换而言之，它未请记住您以前输入。

请务必了解为何发生这种情况。 当你提交的页面时，浏览器会将请求发送到 web 服务器。 时 ASP.NET 获取请求，它将创建页的全新实例、 运行的代码中，并随后将呈现到浏览器页面。 实际上，不过，页不知道你正在只需处理与自身的以前的版本。 所有它知道它有得到了一些的请求中它的窗体数据。

每次请求页&mdash;是第一次还是通过提交它&mdash;你获取一个新页。 Web 服务器包含您上次的请求没有内存。 不能用 ASP.NET，也不能用浏览器。 这些单独的页实例之间的唯一连接是它们之间传输任何数据。 如果提交页面，例如，新的页实例可以由早期实例发送的窗体数据。 （在页面之间传递数据另一种方法是使用 cookie。）

一种正式的方式来描述这种情况下是说，web 页中找到*无状态*。 Web 服务器和页面本身和页面中的元素则不会维护有关页面的前一状态的任何信息。 Web 旨在这种方式，因为维护的各个请求的状态将快速耗尽的资源的 web 服务器，通常可能处理数千，甚至几十万个情况下，每秒的请求。

因此，这就是为什么文本框为空。 提交页面后，ASP.NET 创建的页的新实例，并运行通过代码和标记。 没有执行任何操作在该代码中，是它告诉 ASP.NET，将值放在文本框中。 因此 ASP.NET 不执行任何操作，并且文本框呈现而无需在其中一个值。

没有实际的简单办法获取解决此问题。 在文本框中输入流派*是*可供你在代码中&mdash;在`Request.QueryString["searchGenre"]`。

更新在文本框中的标记，以便`value`属性获取其值从`searchTerm`，如下例所示：

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

在此页中，你也可以还设置`value`属性设为`searchTerm`输入变量，因为该变量还包含风格。 但使用`Request`对象以设置`value`属性如下所示下面是完成此任务的标准方式。 (假设即使想要执行此操作&mdash;在某些情况下，你可能想要呈现页*而无需*字段中的值。 其所有依赖于当前进行的操作与你的应用。）

> [!NOTE]
> 你不能"记住"用于密码的文本框的值。 它将安全漏洞，允许用户通过使用代码填写密码字段。


再次运行此页，输入一种风格，然后单击**搜索流派**。 这次不只执行您看到的搜索的结果，但文本框会记住你所输入上一次：

![页面显示，文本框中具有记住以前的条目](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>搜索标题中的任何词语

你现在可以搜索任何流派，但你可能也要搜索标题。 很难获取标题完全正确，在搜索时，因此，你可以搜索随即显示在标题的任意位置使用的字符。 若要在 SQL 中执行的操作，请使用`LIKE`运算符和语法如下：

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

此命令获取其标题包含"adventure"的所有影片。 当你使用`LIKE`运算符，你可以包括通配符`%`作为搜索词的一部分。 搜索`LIKE 'adventure%'`意味着"从 adventure 开始"。 （从技术上讲，它表示"string adventure 后面接任何内容。"）同样，搜索词`LIKE '%adventure'`意味着"的任何内容跟字符串 adventure"，这是另一种方法可以显示为"与 adventure 结束"。

搜索词`LIKE '%adventure%'`因此意味着"处理 adventure 标题中的任意位置。" （从技术上讲，"的任何内容在标题跟 adventure，后面接任何内容。"）

内部`<form>`元素，添加以下标记在结束之下`</div>`流派搜索标记 (只需在关闭前`</form>`元素):

[!code-html[Main](form-basics/samples/sample10.html)]

代码来处理此搜索是类似于流派搜索代码，只不过你必须以组合`LIKE`搜索。 在页顶部的代码块中，添加此`if`紧后面阻止`if`流派搜索的块：

[!code-csharp[Main](form-basics/samples/sample11.cs)]

此代码使用更早版本，看到的相同逻辑，只不过使用的搜索`LIKE`运算符和代码将"`%`"之前和之后的搜索词。

请注意如何很容易向页面添加另一次搜索。 你所要做时：

- 创建`if`测试以查看相关的搜索框中是否具有值的块。
- 设置`selectCommand`变量与新的 SQL 语句。
- 设置`searchTerm`变量要传递给查询的值。

下面是完整的代码块，其中包含为标题搜索新的逻辑：

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

下面是这段代码执行的摘要：

- 变量`searchTerm`和`selectCommand`顶部初始化。 用户在页面中的作用根据相应的 SQL 命令和你要将这些变量设置为相应的搜索词 （如果有）。 默认搜索是从数据库中获取所有电影的简单的情况。
- 中的测试`searchGenre`和`searchTitle`，代码集`searchTerm`到你想要搜索的值。 这些代码块还设置`selectCommand`对此搜索的相应 SQL 命令。
- `db.Query`方法只调用一次，使用任何 SQL 命令处于`selectedCommand`并且任何值在`searchTerm`。 如果没有任何搜索词 （没有 genre 和没有标题 word） 的值`searchTerm`为空字符串。 但是，，并不重要，因为在这种情况下查询不需要参数。

## <a name="testing-the-title-search-feature"></a>测试标题搜索功能

现在，你可以测试已完成的搜索页。 运行*Movies.cshtml*。

输入一种风格，然后单击**搜索流派**。 网格显示电影的影片，如之前。

输入标题词，然后单击**搜索标题**。 网格将显示在标题中包含该单词的电影。

![在标题中搜索有关 ' 后列出的电影页](form-basics/_static/image6.png)

将两个文本框内留空，单击任一按钮。 网格显示所有影片。

## <a name="combining-the-queries"></a>组合查询

你可能注意到，你可以执行的搜索并排斥。 你不能在同一时间，搜索标题和风格即使这两个搜索框中都有值。 例如，你不能搜索其标题包含"Adventure"的所有操作电影。 （如果值输入 genre 和标题，现在，编码页，如标题搜索获取优先。）若要创建组合条件搜索，你将必须创建具有如下所示的语法的 SQL 查询：

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

您必须使用类似于下面的语句运行查询 （大致上说）：

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

如你所见，创建逻辑，以允许多个屏蔽的搜索条件可以获取有点复杂。 因此，我们将停止此处。

## <a name="coming-up-next"></a>接下来

在下一步的教程中，你将创建使用窗体以使用户可以向数据库添加电影的页。

## <a name="complete-listing-for-movie-page-updated-with-search"></a>（使用搜索更新） 的电影页的完整列表

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>其他资源

- [使用 Razor 语法的 ASP.NET Web 编程简介](https://go.microsoft.com/fwlink/?LinkID=202890)
- [SQL WHERE 子句](http://www.w3schools.com/sql/sql_where.asp)W3Schools 站点上
- [方法定义](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)W3C 网站上的文章

>[!div class="step-by-step"]
[上一页](displaying-data.md)
[下一页](entering-data.md)
