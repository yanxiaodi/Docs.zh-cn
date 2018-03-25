---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: "引入的 ASP.NET Web Pages-编程基础知识 |Microsoft 文档"
author: tfitzmac
description: "本教程提供你的概述如何给程序中的 ASP.NET Web Pages 使用 Razor 语法。 你将了解： 基本 Razor 语法用于 pr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/17/2015
ms.topic: article
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: eed07f4f8a13ea9082ab3aad3e3db24febff8ef6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---programming-basics"></a>引入了 ASP.NET Web 页-编程基础知识
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本教程提供你的概述如何给程序中的 ASP.NET Web Pages 使用 Razor 语法。
> 
> 你将学习：
> 
> - 将用于编程中的 ASP.NET Web Pages 基本"Razor"语法。
> - 一些基本 C# 中，这是你将使用的编程语言。
> - 网页的某些基本的编程概念。
> - 如何安装程序包 （包含预构建的代码的组件），以使用你的网站。
> - 如何使用*帮助器*以执行常见编程任务。
>   
> 
> 功能/技术讨论：
> 
> - NuGet 和程序包管理器中。
> - `Gravatar`帮助器。


本教程是主要中引入的编程语法，你将使用 ASP.NET Web 页的一个过程。 你将了解*Razor 语法*和用 C# 编写的代码编程语言。 你在前面的教程中; 此语法 glimpse在本教程中我们将介绍更多的语法。

我们保证本教程，包括编程你会看到在单个教程中，以及它是否是唯一教程最*仅*有关编程。 在此组中的其余教程，你将实际创建有趣事情的页。

你还将了解有关*帮助器*。 一个帮助程序是一个组件-打包向上的一段代码-，可以添加到页。 帮助器执行工作，否则可能会需要很长时间或太复杂，手动执行。

## <a name="creating-a-page-to-play-with-razor"></a>创建页以玩 Razor

在本节中你将玩有点 Razor 以便您可以获取的意义上的基本语法。

如果它尚未运行，请启动 WebMatrix。 你将使用你在前面的教程中创建的网站 ([获取启动与网页](https://go.microsoft.com/fwlink/?LinkId=251578))。 若要重新打开它，请单击**我的网站**选择**WebPageMovies**:

![WebMatrix 开始屏幕显示打开的站点选项和我突出显示的网站](intro-to-web-pages-programming/_static/image1.png)

选择**文件**工作区。

在功能区中，单击**新建**来创建一个页面。 选择**CSHTML**并命名新页*TestRazor.cshtml*。

单击“确定”。

将以下内容复制到文件中，完全替换什么已存在。

> [!NOTE]
> 如果你放入页中，将示例中复制代码或标记，缩进和对齐方式可能不与本教程中的相同。 代码的运行方式，但并不影响缩进和对齐方式。


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>检查示例页面

你所看到的大多数是普通的 HTML。 但是，在顶部没有此代码块：

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

请注意有关此代码块的以下内容：

- @ 字符是告诉 ASP.NET 的以下内容是 Razor 代码中，不 HTML。 ASP.NET 将处理后的所有内容 @ 字符如下代码，直至它再次运行到一些 HTML。 (在这种情况下的&lt;！DOCTYPE&gt;元素。
- 大括号 （{和}） 括起的 Razor 代码块，如果代码有多个行。 大括号告诉 ASP.NET，该块中的代码位置开始和结束。
- / / 字符标记注释-即，不会执行的代码的一部分。
- 每个语句必须以分号 （;） 结尾。 （不注释，但。）
- 你可以将值存储在*变量*，创建的 (*声明*) 与关键字差异 在创建变量时，你赋予它一个名称，可以包含字母、 数字和下划线 (\_)。 变量名不能以数字开头，并且无法使用 （如 var) 编程关键字的名称。
- 将字符字符串 （例如"ASP.NET"和"网页"） 括在双引号中。 （它们必须是两个双引号）。数字不是用引号引起来。
- 外部引号引起来的空白并不重要。 分行符主要不重要;例外情况是，不能将用引号引起来的字符串拆分到行。 缩进和对齐方式不重要。

这一点不明显来自此示例是所有代码都是区分大小写。 这意味着变量的参数是一个不同的变量比可能名参数的变量。 同样，var 是一个关键字，但 Var 不是。

### <a name="objects-and-properties-and-methods"></a>对象和属性和方法

然后是 DateTime.Now 的表达式。 简单地说，DateTime 是*对象*。 一个对象是，你可以使用编程 — 页、 文本框、 文件、 映像、 web 请求、 电子邮件、 客户记录，等等。对象具有一个或多个*属性*描述其特征。 文本框对象具有文本属性 （及其他）、 请求对象具有 Url 属性 （及其他）、 电子邮件，并且在 From 属性的收件人属性中，依次类推。 对象还具有*方法*是他们可以执行的"谓词"。 你将对象大量使用。

如你所见示例中，日期时间将是可以程序日期和时间的对象。 它具有名为现在将返回当前日期和时间的属性。

### <a name="using-code-to-render-markup-in-the-page"></a>使用代码来呈现页中标记

在页的正文中，注意以下事项：

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

同样，@ 字符是告诉 ASP.NET 的以下该内容是代码，不 HTML。 在标记中你可以添加跟代码表达式，并且 ASP.NET 将在该点呈现该表达式的值。 在示例中，@a将呈现命名的变量的值是任何内容，@product呈现任何内容是在变量的命名产品中，依此类推。

你可以不局限于变量，但。 在某些情况下在这里，@ 字符之前表达式：

- @(\*b） 呈现变量中的所有内容的产品和 b。 (\*运算符意味着乘法。)
- @(技术 +""+ 产品) 将它们连接起来并之间添加空格之后呈现变量技术和产品中的值。 为连接字符串的运算符 （+） 是与运算符相同的给出数字相加。 ASP.NET 通常可以判断计算机是否正在与数字或字符串并执行相应的操作使用 + 运算符。
- @Request.Url呈现请求对象的 Url 属性。 请求对象包含有关浏览器中，从当前请求的信息，这是当然的 Url 属性包含该当前请求的 URL。

该示例还旨在演示你可以执行不同的方式工作。 你可以执行代码块顶部中的计算、 将结果放入变量中，和随后呈现标记中的变量。 也可以执行在标记中的表达式权限中的计算。 你使用的方法取决于你要执行的操作和，在某种程度上，在您自己的首选项。

### <a name="seeing-the-code-in-action"></a>看到操作中的代码

右键单击该文件的名称，然后选择**在浏览器中启动**。 你看到浏览器中使用的所有值和解决在页中的表达式中的页。

![浏览器中运行的 TestRazor 页](intro-to-web-pages-programming/_static/image2.png)

查看浏览器中的源。

![在浏览器中的测试 Razor 页面源文件](intro-to-web-pages-programming/_static/image3.png)

正如预期从你在前面的教程中的体验，没有 Razor 代码是在页中。 你看到是实际显示的值。 运行页面时，你实际上正在对内置 WebMatrix 的 web 服务器进行请求。 当收到请求时，ASP.NET 解析所有的值和表达式，并将其值呈现到页。 它随后发送至浏览器的页面。

> [!TIP] 
> 
> **Razor 和 C#**
> 
> 到目前为止我们讲到你正在使用 Razor 语法。 这是为 true，但它不是完成的情景。 要使用的实际编程语言称为*C#*。 C# 通过十年前创建由 Microsoft 和已成为一种用于创建 Windows 应用程序的主编程语言。 你已了解有关如何命名变量以及如何创建语句的所有规则都都实际的 C# 语言的所有规则。
> 
> Razor 更具体地说就是指如何将此代码嵌入到页面的约定的少部分。 例如，使用标记在页中的代码并使用的约定 @ {} 嵌入代码块是单页的 Razor 方面。 帮助器也被视为是 Razor 的一部分。 在只需在 ASP.NET Web Pages 比多个位置使用 razor 语法。 （例如，它用于 ASP.NET MVC 视图也。）
> 
> 我们提到这是因为如果你寻找的编程 ASP.NET Web Pages 有关的信息时，你将找到大量对 Razor 的引用。 但是，大量的这些引用不会应用到你要执行此操作并因此可能会造成混淆。 和中的事实，许多编程问题实际上会成为有关使用 C# 或使用 ASP.NET。 因此如果你查看针对 Razor 有关的信息，您可能无法找到所需的答案。


## <a name="adding-some-conditional-logic"></a>添加一些条件逻辑

有关在页中使用代码的强大功能之一是，您可以更改时会发生什么情况基于各种条件。 在本教程的此部分中，你将要尝试一下更改页面中显示的内容的一些方法。

该示例将简单和专门设计，以便我们可以专注于条件逻辑的某种程度上。 你将创建的页将执行此操作：

- 根据是否第一次显示的页面或是否已单击按钮以提交页的页上显示不同的文本。 将第一个条件的测试。
- 仅当某个值传递的 URL (http://...?show=true) 的查询字符串中显示消息。 将第二个条件的测试。

在 WebMatrix 中，创建一个页面，并将其命名*TestRazorPart2.cshtml*。 (在功能区中，单击**新建**，选择**CSHTML**，作为文件名，然后单击**确定**。)

该页面的内容替换为以下：

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

在顶部的代码块初始化名为带有某些文本的消息的变量。 在页的正文中，消息变量的内容显示在&lt;p&gt;元素。 标记还包含&lt;输入&gt;元素以创建**提交**按钮。

运行要查看其工作原理现在的页。 现在，它基本上是静态的页上，即使你单击**提交**按钮。

返回到 WebMatrix。 在代码块中，添加以下突出显示的代码*后*初始化消息的行：

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>如果 {} 块

你刚添加了 if 条件。 在代码中，如果条件具有如下结构：

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

要测试的条件位于括号中。 它必须是一个值或一个表达式，返回 true 或 false。 如果条件为 true，ASP.NET 将运行的语句或大括号内的语句。 (这种*然后*属于*如果那么*逻辑。)如果条件为 false，则跳过的代码块。

下面是可在 if 中测试的条件的几个示例语句：

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

你可以通过使用测试变量对值或表达式*逻辑运算符*或*比较运算符*： 等于 （= =） 大于 (&gt;)，小于 (&lt;)，大于或等于 (&gt;=)，并且小于或等于 (&lt;=)。 ！ = 运算符表示不等于-例如，如果 (！ = 0) 意味着*如果* *是否不等于 0*。

> [!NOTE]
> 请确保你注意到等于 （= =） 比较运算符不是 = 的相同。 = 运算符仅用于将值分配 (var = 2)。 如果混合这些运算符，您可以将要获取错误或你将获得一些奇怪的结果。


若要测试是否内容为 true，完整的语法是 if(IsDone == true)。 但你也可以使用快捷 if(IsDone)。 如果没有任何比较运算符，ASP.NET 将假定您要测试为 true。

！ 运算符本身意味着逻辑非。 例如，条件 if （！IsPost) 意味着*若 IsPost 不为 true*。

你可以通过使用逻辑 AND 组合条件 (&amp; &amp;运算符) 或逻辑 OR (| | 运算符)。 例如，如果的最后一个条件在前面的示例表示*如果 FileProcessingIsDone 未设置为 true AND displayMessage 设置为 false*。

### <a name="the-else-block"></a>Else 块

一个有关如果的最后一件事情块： 如果块可以跟到 else 块中。 到 else 块中非常有用，您需要在条件为 false 时执行不同的代码。 下面是一个简单的示例：

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

在更高版本，使用到 else 块中也是有用本系列教程中，你将看到一些示例。

### <a name="testing-whether-the-request-is-a-submit-post"></a>测试是否请求是提交 (post)

还有更多，但我们回过头的示例中，它具有条件 if(IsPost) {...}。 IsPost 是实际当前页的属性。 第一次请求页时，IsPost 返回 false。 但是，如果你单击一个按钮，或以其他方式提交页 — 你发布的即-IsPost，则返回 true。 因此 IsPost 使你能够确定是否正在处理与表单提交。 （根据 HTTP 谓词 GET 操作时，该请求是否 IsPost 返回 false。 如果请求是 POST 操作，IsPost 返回 true。）在后面的教程中，您将可以使用输入的表单，此测试变得特别有用。

运行页面。 因为这是首次在请求页中，你将看到"这是在首次已请求页"。 该字符串将是初始化消息变量的值。 没有一 if(IsPost) 项测试，但，返回 false 时间，因此块置于 if 的代码不运行。

单击**提交**按钮。 再次请求该页。 为之前，消息变量设置为"这是首次..."。 但这一次，测试 if(IsPost)，则返回 true，因此如果内的代码块将运行。 代码更改为不同的值，这是在标记中呈现的内容的消息变量的值。

现在添加 if 标记中的条件。 下面&lt;p&gt;包含的元素**提交**按钮，添加以下标记：

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

你要添加标记内的代码，因此你必须首先@。 If 则类似于你前面添加的代码块中的测试。 在大括号，不过，你要添加普通 HTML-至少，它是普通，直到到达@DateTime.Now。 这是另一小段 Razor 代码，因此再次你需要添加前面遮挡它。

这里的要点是，如果你可以添加在条件块的代码在顶部和标记中。 如果使用 if 页上，在该块内的行的正文中的条件可以是标记或代码。 在这种情况下，并且因为每当混合使用标记和代码，则为 true，你必须使用 @ 以使它清楚地了解 ASP.NET 代码。

运行页面并单击**提交**。 这次你不仅看到不同的消息时提交 （"现在你已提交..."），但看到列出的日期和时间的新消息。

![提交后显示的时间戳与浏览器中运行的测试 Razor 2 页](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>测试查询字符串的值

一个多个测试。 此时，你将添加 if 测试某个值的块名为可能传递给查询字符串中的显示。 (如下所示: ' http://localhost:43097/TestRazorPart2.cshtml`?show=true`)，以便消息你已显示，将更改的页 （"这是首次..."，等等） 如果显示的值为 true，则仅显示。

在底部 （但内部） 在代码块中的页上，顶部添加以下代码：

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

完整的代码块现在看起来像下面的示例。 （请记住，当你将代码复制到你的页面，缩进显示可能会有所不同。 但是，不会影响代码的运行方式。）

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

新代码块中的初始化一个名为分隔开为 false 的多个变量。 它，则执行 if 测试查找查询字符串中的值。 当您第一次请求的页面时，它具有与此类似的 URL:

`http://localhost:43097/TestRazorPart2.cshtml`

代码将确定 URL 是否包含名为查询字符串，如 URL 的此版本中显示的变量：

`http://localhost:43097/TestRazorPart2.cshtml`？ 显示 = true

测试本身来看待请求对象的查询字符串属性。 如果查询字符串包含项命名的显示，并且该项设置为 true，如果块运行，并将分隔开多个变量设置为 true。

没有了技巧，可以在这里，你可以看到。 规定的名称，查询字符串是一个字符串。 但是，你可以仅测试为 true 和 false 如果您要测试的值是一个布尔 (true/false) 值。 你可以测试查询字符串中的显示变量的值之前，必须将其转换为一个布尔值。 这就是 AsBool 方法作用 — 它采用字符串作为输入，并将其转换为布尔值。 很明显，如果字符串为"true"，AsBool 方法会将该值转换为 true。 如果任何其他内容字符串的值，则 AsBool 返回 false。

> [!TIP] 
> 
> **数据类型和 as （） 方法**
> 
> 我们仅讲到目前为止，当创建变量时，你会使用关键字差异 这不是整篇文章，不过。 为了操作值-若要添加数字，或连接字符串，或比较日期，或测试 true/false-C# 必须使用适当的值的内部表示形式。 C# 可以*通常*找出该表示形式中应该是什么 (即，什么*类型*数据) 基于您所做的值。 现在，然后，不过，它无法做到这一点。 如果没有，你需要通过显式，该值指示如何 C# 应表示的数据帮助解决问题。 AsBool 方法执行的 — 它告诉 C#"true"或"false"的字符串值应视为一个布尔值。 存在类似的方法无法表示为其他类型，如 AsInt (treat 整数形式)、 AsDateTime （视为日期/时间）、 AsFloat （视为浮点数） 和等等的字符串。 当你使用这些项目称为 （） 方法，如果 C# 不能表示的字符串值的要求时，你将看到一个错误。


在页面的标记中，删除或注释掉此元素 （此处显示注释掉）：

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

右删除或注释掉该文本的位置添加以下代码：

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

如果测试表明，是否分隔开多个变量为 true，呈现&lt;p&gt;具有消息变量的值的元素。

### <a name="summary-of-your-conditional-logic"></a>条件逻辑的摘要

如果你不能完全确定只需完成的新增功能，这里只是摘要。

- 消息变量初始化为默认字符串 （"这是首次..."）。
- 如果页面请求是提交 (post) 的结果，消息的值更改为"现在你已提交..."
- 分隔开多个变量初始化为 false。
- 如果查询字符串包含？ 显示 = 分隔开多个变量设置为 true，则为 true。
- 在标记中，如果分隔开多个为 true， &lt;p&gt;元素呈现显示消息的值。 （如果分隔开多个为 false，执行任何操作将呈现在该点标记中。）
- 在标记中，如果请求是 post， &lt;p&gt;元素呈现，显示的日期和时间。

运行页面。 没有任何消息，因为分隔开多个为 false，因此在标记中 if(showMessage) 测试返回 false。

单击“提交”。 你看到该日期和时间，但仍任何消息。

在浏览器中，转到 URL 框中，并将以下代码添加到 URL 的末尾:？ 显示为 true，然后按 Enter。

![在浏览器显示查询字符串中的测试 Razor 2 页](intro-to-web-pages-programming/_static/image5.png)

将再次显示的页。 （由于更改 URL，这是新请求，不提交。）单击**提交**试。 原样的日期和时间，将再次显示消息。

![查询字符串时，将提交后的测试 Razor 2 页](intro-to-web-pages-programming/_static/image6.png)

在 URL 更改？ 显示为真 =？ 显示 = false 然后按 Enter。 重新提交页。 该页是返回到你的启动方式如何-任何消息。

如前文所述，在此示例中的逻辑是一个小虚构浓。 但是，如果要在很多的页，提出，需要一个或多个窗体，你已了解此处。

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>安装程序 （显示 Gravatar 图像） 的帮助程序

用户通常想要在网页上执行某些任务需要大量的代码，或者需要额外的知识。 示例： 显示的图表数据;将 Facebook"Like"按钮放置在页;从您的网站; 发送电子邮件裁剪或调整图像; 的大小为您的网站使用 PayPal。 为了更加轻松地完成这些类型的操作，ASP.NET 网页，你可以使用*帮助器*。 帮助器是为站点安装，并能够让你通过使用 Razor 代码只需几行执行典型的任务的组件。

ASP.NET Web 页具有内置的几个帮助器。 但是，使用 NuGet 包管理器提供的包 （外接程序） 中提供了许多的帮助器。 NuGet 允许你选择要安装的程序包，然后它就会完成安装的所有详细信息。

在本教程的此部分中，你将安装的帮助程序便可以显示 Gravatar （"全局识别虚拟形象"） 映像。 你将学习以下两项操作。 一个是如何查找和安装程序的帮助。 你还将了解如何帮助程序可以轻松地执行某些否则需要使用大量您将不得不自己编写的代码执行操作。

你可以注册在 Gravatar 网站在自己 Gravatar [http://www.gravatar.com/](http://www.gravatar.com/)，但它不是创建 Gravatar 帐户执行本教程的此部分非常重要。

在 WebMatrix 中，单击**NuGet**按钮。

![在 WebMatrix 的 NuGet 库对话框](intro-to-web-pages-programming/_static/image7.png)

这将启动 NuGet 包管理器，并显示可用的包。 (不是所有包都帮助器; 一些将功能添加到 WebMatrix 中本身，一些是其他模板，依次类推。)你可能会收到有关版本不兼容的错误消息。 你可以忽略此错误消息，通过单击**确定**和继续执行本教程。

![在 WebMatrix 的 NuGet 库对话框](intro-to-web-pages-programming/_static/image8.png)

在搜索框中，输入"asp.net 帮助器"。 NuGet 显示与搜索词匹配的程序包。

![在 WebMatrix 中显示包的 NuGet 库](intro-to-web-pages-programming/_static/image9.png)

ASP.NET Web 帮助程序库包含代码以简化许多常见任务，包括使用 Gravatar 图像。 选择**ASP.NET Web 帮助程序库**包，然后单击**安装**以启动安装程序。 选择**是**当系统询问你是否想要安装包，并接受条款以完成安装。

介绍完毕。 NuGet 下载并安装所有内容，包括可能需要的任何其他组件 (*依赖关系*)。

如果出于某种原因你必须卸载程序的帮助程序，此过程是非常类似。 单击**NuGet**菜单上，单击**已安装**选项卡，然后选择你想要卸载的程序包。

## <a name="using-a-helper-in-a-page"></a>在页中使用程序的帮助程序

现在，你将使用你刚安装的帮助程序。 将程序的帮助程序添加到页过程是类似的大多数帮助器。

在 WebMatrix 中，创建一个页面，并将其命名*GravatarTest.cshml*。 （你要创建特殊页后，可以测试帮助器，但你可以使用在你的站点中的任意页中的帮助器。）

内部&lt;正文&gt;元素中，添加&lt;div&gt;元素。 内部&lt;div&gt;元素中，键入：

@Gravatar。

@ 字符是你所用来标记 Razor 代码的相同字符。 **Gravatar**即你正在使用的帮助程序对象。

WebMatrix 只要键入句点 （.），显示的列表*方法*（函数） 的 Gravatar 帮助程序使可：

![Gravatar 帮助器 IntelliSense 下拉列表](intro-to-web-pages-programming/_static/image10.png)

此功能被称为*IntelliSense*。 它通过提供上下文相对应的选项帮助你进行编码。 IntelliSense 适用于 HTML、 CSS、 ASP.NET 代码、 JavaScript 和其他语言，支持在 WebMatrix 中。 它是另一个功能，它可以更轻松地开发在 WebMatrix 中的网页。

按 G 上键盘，请参阅 IntelliSense 查找 GetHtml 方法。 按 Tab。IntelliSense 为您插入所选的方法 (GetHtml)。 键入左括号，请注意，自动添加右括号。 用引号括起来的两个括号之间键入你的电子邮件地址。 如果你有 Gravatar 帐户，将返回你的个人资料图片。 如果你没有 Gravatar 帐户，则返回默认图像。 完成后，行如下所示：

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

现在在浏览器中查看的页面。 你的图片或默认图像将显示，具体取决于是否有 Gravatar 帐户。

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![默认映像](intro-to-web-pages-programming/_static/image12.png)

若要了解的内容的帮助器为你做，在浏览器中查看的页面的源。 必须在页中的 HTML，以及你将看到包含的标识符的图像元素。 这是帮助器呈现到页，其中 d 的位置的代码@Gravatar.GetHtml。 帮助器所花费的提供和生成交谈直接 Gravatar 若要提供帐户的取回了正确的图像的代码的信息。

GetHtml 方法还可通过提供其他参数自定义映像。 下面的代码演示如何请求映像具有宽度和高度 40 个像素，并使用名为指定的默认映像**wavatar**如果指定的帐户不存在。

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

此代码生成以下 （默认映像随机有所不同） 的结果类似。

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>接下来

若要使此教程短，我们不得不专注于仅几个基础知识。 当然，没有*很多*更多的 Razor 和 C#。 你将了解更多其他你经历这些教程。 如果你感兴趣稍后再试学习有关 Razor 和 C# 的编程方面的详细信息，你可以阅读的更全面的介绍： [ASP.NET Web 编程使用 Razor 语法的简介](https://go.microsoft.com/fwlink/?LinkID=202890)。

下一步教程将介绍如何使用的数据库。 在该教程中，你将开始创建的示例应用程序允许你列出您最喜爱的电影。

## <a name="complete-listing-for-testrazor-page"></a>TestRazor 页的完整列表

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>TestRazorPart2 页的完整列表

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>GravatarTest 页的完整列表

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>其他资源

- [使用 Razor 语法的 ASP.NET Web 编程简介](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Twitter 帮助器](../../ui-layouts-and-themes/twitter-helper.md)

>[!div class="step-by-step"]
[上一页](getting-started.md)
[下一页](displaying-data.md)
