---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: "使用 Razor 语法 (C#) 的 ASP.NET Web 编程简介 |Microsoft 文档"
author: tfitzmac
description: "本章概述你的编程与 ASP.NET Web Pages 使用 Razor 语法。 ASP.NET 是 Microsoft 的技术，用于运行动态 web pa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: 32cdd8d524d783d7ccc3ab076de636ce4a868132
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>使用 Razor 语法 (C#) 的 ASP.NET Web 编程简介
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本文概述你的编程与 ASP.NET Web Pages 使用 Razor 语法。 ASP.NET 是用于在 web 服务器上运行动态 web 页的 Microsoft 的技术。 此文章重点使用 C# 编程语言。
> 
> **你将了解**:
> 
> - 编程提示的入门知识编程使用 Razor 语法的 ASP.NET Web Pages 顶部 8。
> - 你将需要的基本编程概念。
> - 哪些 ASP.NET 服务器代码和 Razor 语法与所有有关。
>   
> 
> ## <a name="software-versions"></a>软件版本
> 
> 
> - ASP.NET 网页 (Razor) 3
>   
> 
> 本教程还适用于 ASP.NET Web Pages 2。


## <a name="the-top-8-programming-tips"></a>最重要的 8 编程提示

本部分列出绝对需要知道当你开始编写使用 Razor 语法的 ASP.NET 服务器代码的一些提示。

> [!NOTE]
> Razor 语法基于 C# 编程语言中，这也最常使用的 ASP.NET Web Pages 使用的语言。 但是，Razor 语法还支持 Visual Basic 语言中，以及你看到你还可以执行在 Visual Basic 中的所有内容。 有关详细信息，请参阅附录[Visual Basic 语言和语法](https://go.microsoft.com/fwlink/?LinkId=202908)。


在本文的后面，你可以找到有关这些编程技术的大多数的更多详细信息。

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1.将代码添加到页使用 @ 字符

`@`字符开始内联表达式、 单个语句块和多语句块：

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

这是这些语句在页的浏览器中运行时的外观：

![Razor-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **HTML 编码**
> 
> 当你在页中使用显示内容`@`字符，如前面的示例中，ASP.NET 进行 HTML 编码输出。 这将替换保留的 HTML 字符 (如`<`和`>`和`&`) 与启用要显示为字符而不是被解释为 HTML 标记或实体的网页中的字符的代码。 HTML 编码，在服务器代码中的输出可能显示不正常，而无需无法公开安全风险的页。
> 
> 如果你的目标是输出将标记呈现为标记的 HTML 标记 (例如`<p></p>`，段落或`<em></em>`强调文本)，请参阅明[组合文本、 标记和代码块中的代码](#BM_CombiningTextMarkupAndCode)本文后续部分中。
> 
> 你可以阅读更多有关中的 HTML 编码[使用窗体](https://go.microsoft.com/fwlink/?LinkId=202892)。


### <a name="2-you-enclose-code-blocks-in-braces"></a>2.将代码块括在大括号中

A*代码块*包括一个或多个代码语句并括在大括号。

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

在浏览器中所显示的结果：

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3.在块中，你最终用分号每个代码语句

在代码块中，每个完整的代码语句必须以分号结尾。 内联表达式不以分号结尾。

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4.使用变量来存储值

你可以将值存储在*变量*，包括字符串、 数字和日期，等等。创建一个新的变量使用`var`关键字。 你可以直接在页中使用插入变量值`@`。

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

在浏览器中所显示的结果：

![Razor-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5.将文本字符串值括在双引号内

A*字符串*是作为文本处理的字符的序列。 若要指定一个字符串，则将它括在双引号中：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

如果你想要显示的字符串包含反斜杠字符 ( `\` ) 或双击引号引起来 ( `"` )，使用*原义字符串*且具有前缀与`@`运算符。 (在 C# 中，\ 字符具有特殊含义，除非你使用原义字符串。)

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

若要嵌入双引号括起来，使用原义字符串且不重复引号引起来：

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

下面是在页中使用的两个这些示例的结果：

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> 请注意，`@`标记在 C# 中逐字字符串文本和标记 ASP.NET 页中的代码使用字符。


### <a name="6-code-is-case-sensitive"></a>6.代码是区分大小写

在 C# 中，关键字 (如`var`， `true`，和`if`) 且变量名称区分大小写。 代码的以下行创建两个不同的变量，`lastName`和`LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

如果你声明一个变量，作为`var lastName = "Smith";`和如果你尝试引用该变量作为在页中的`@LastName`，产生错误，因为`LastName`将不会识别。

> [!NOTE]
> 在 Visual Basic 中，关键字和变量是*不*区分大小写。


### <a name="7-much-of-your-coding-involves-objects"></a>7.大部分代码涉及对象

*对象*表示，你可以使用编程件事情 &#8212; 页、 文本框、 文件、 映像、 web 请求、 电子邮件、 客户记录 （数据库行），等等。对象具有描述其特征的属性和，可读取或更改和 #8212;文本框对象具有`Text`（及其他） 的属性，请求对象具有`Url`属性，电子邮件具有`From`属性和客户对象都有`FirstName`属性。 对象还具有方法&quot;谓词&quot;他们可以执行。 示例包括文件对象的`Save`方法中，映像对象的`Rotate`方法和电子邮件对象的`Send`方法。

通常将使用`Request`对象，它会为你提供的文本框 （窗体字段） 的信息，如值在页上，浏览器的类型进行请求的 URL 的页面、 用户标识，等等。下面的示例演示如何访问属性的`Request`对象以及如何调用`MapPath`方法`Request`对象，这将使您在服务器上的页的绝对路径：

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

在浏览器中所显示的结果：

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8.你可以编写做出决策的代码

动态网页的一项重要功能是你可以确定要执行的操作根据条件。 若要执行此操作的最常见方法是使用`if`语句 (和可选`else`语句)。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

语句`if(IsPost)`是写入的速记方式`if(IsPost == true)`。 连同`if`语句，有各种方法来测试条件，重复代码块，并依此类推，它们进行了描述这篇文章中更高版本。

在浏览器中所显示的结果 (单击后**提交**):

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>HTTP GET 和 POST 方法以及 IsPost 属性
> 
> 用于网页 (HTTP) 的协议支持非常有限的数量的用于向服务器发出请求的方法 （谓词）。 两个最常见的是 GET，用于读取某页以及开机自检，用于提交的页面。 一般情况下，用户请求页面时，第一次请求页时使用 GET。 如果用户在填写窗体，然后单击提交按钮，浏览器向服务器发出的 POST 请求。
> 
> 在 web 编程中，它通常是有助于你了解是否请求该页为 GET 或 POST，以便你知道如何处理该页。 在 ASP.NET Web 页中，你可以使用`IsPost`属性以查看是否 GET 或 POST 请求。 如果请求是 POST，`IsPost`属性将返回 true，并且可执行诸如读取窗体上的文本框的值。 你将看到许多示例演示如何处理以不同的方式根据的值页`IsPost`。


## <a name="a-simple-code-example"></a>一个简单代码示例

此过程演示如何创建阐释基本的编程技术的页。 在示例中，你将创建一个允许用户输入两个数字，然后再将它们添加显示结果页面。

1. 在编辑器中创建一个新文件并将其命名*AddNumbers.cshtml*。
2. 将下面的代码和标记复制到页中，替换已在页中的任何内容。  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    下面是为你要注意一些事项：

    - `@`字符在页中，启动代码的第一个块，它位于之前`totalMessage`嵌入在页面底部附近的变量。
    - 在页面顶部块括在大括号中。
    - 在顶部块中，所有行以分号都结尾。
    - 变量`total`， `num1`， `num2`，和`totalMessage`存储多个数字和字符串。
    - 分配给的文字字符串值`totalMessage`变量是在双引号内。
    - 由于代码时区分大小写，因此`totalMessage`变量使用页面底部附近，其名称必须与在顶部变量完全匹配。
    - 表达式`num1.AsInt() + num2.AsInt()`演示如何使用对象和方法。 `AsInt`上每个变量的方法将转换到号 （整数） 输入的用户，因此，你可以在其上执行算术的字符串。
    - `<form>`标记包含`method="post"`属性。 此步骤指定当用户单击**添加**，页面将发送到服务器使用 HTTP POST 方法。 当提交页时，`if(IsPost)`测试的计算结果为 true，而条件性代码运行时，显示的添加数字结果。
3. 保存页并在浏览器中运行它。 (请确保页中选择**文件**工作区之前运行它。)输入两个整数，然后单击**添加**按钮。 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>基本编程概念

本文为你提供了 ASP.NET web 编程的概述。 并不详尽检查，只需通过将最常使用的编程概念的快速教程。 即便如此，它涵盖几乎所有操作都将需要若要开始使用 ASP.NET Web 页。

但首先，很少的技术背景。

### <a name="the-razor-syntax-server-code-and-aspnet"></a>Razor 语法、 服务器代码和 ASP.NET

Razor 语法是一种简单的编程语法，为基于服务器的代码嵌入在网页中。 在使用 Razor 语法的网页，有两种类型的内容： 客户端内容和服务器代码。 客户端的内容是用于在网页中的内容： HTML 标记 （元素） 的样式如 CSS 的信息可能某些客户端脚本，如 JavaScript 和纯文本。

Razor 语法允许您添加到此客户端内容的服务器代码。 如果存在服务器代码页中，服务器运行该代码第一次，再将页发送到浏览器。 通过在服务器上运行，代码可以执行任务，可以是更加复杂，无法使用客户端内容单独出现时，例如类似于访问基于服务器的数据库执行操作。 最重要的是，服务器代码可以动态创建客户端内容 （&） #8212;它可以生成 HTML 标记或在运行过程中的其他内容并将其发送到以及页可能会包含任何静态 HTML 浏览器。 从浏览器的角度来看，由服务器代码生成的客户端内容是比任何其他内容，客户端没有什么不同。 如你已经了解，具有所需的服务器代码是非常简单。

包含 Razor 语法的 ASP.NET web pages 具有特殊的文件扩展名 (*.cshtml*或*.vbhtml*)。 服务器可识别这些扩展，运行的代码，使用 Razor 语法标记，然后将页发送到浏览器。

### <a name="where-does-aspnet-fit-in"></a>ASP.NET 放置何处？

Razor 语法取决于从调用 ASP.NET，又基于 Microsoft.NET Framework 的 Microsoft 技术。 .Net Framework 开发几乎任何类型的计算机应用程序是一个大型的全面编程框架，从 Microsoft。 ASP.NET 是专门设计用于创建 web 应用程序的.NET framework 的一部分。 开发人员使用 ASP.NET 创建的最大值和最高流量网站许多世界。 (你看到的文件扩展名的任何时间*.aspx*作为站点中的 URL 的一部分，你将知道站点编写使用 ASP.NET。)

Razor 语法使您能够 ASP.NET，但使用可以更轻松地了解当你方面的专家，如果您是初学者，可将你提高工作效率的简化的语法的所有功能。 即使此语法是易于使用，它与 ASP.NET 和.NET Framework 的系列关系意味着，在您的网站变得越来越复杂，你会有更大的框架可供你的 power。

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **类和实例**
> 
> ASP.NET 服务器代码使用反过来生成类的理论基础的对象。 此类是定义或模板对象。 例如，应用程序可能包含`Customer`定义的属性和任何客户对象所需的方法的类。
> 
> 当应用程序需要处理的实际客户信息时，它创建的实例 (或*实例化*) 的客户对象。 每个单独的客户是一个单独的实例`Customer`类。 每个实例支持的相同的属性和方法，但每个实例的属性值通常不同，因为每个客户对象是唯一的。 一个客户对象中`LastName`属性可能是"Smith"; 在另一个客户对象，`LastName`属性可能是"Jones"。
> 
> 同样，在你的站点中任何单个 web 页是`Page`是的一个实例的对象`Page`类。 页上的按钮是`Button`是的一个实例的对象`Button`类中，依次类推。 每个实例都具有其自己的特征，但它们都基于对象的类定义中指定的内容。


## <a name="basic-syntax"></a>基本语法

前面你已了解如何创建一个 ASP.NET Web Pages 页面上，以及如何将服务器代码添加到 HTML 标记的一个基本示例。 此处将介绍编写使用 Razor 语法 &#8212; 的 ASP.NET 服务器代码的基础知识即，使用编程语言规则。

如果你有使用编程 （尤其是如果您使用过 C、 c + +、 C#、 Visual Basic 或 JavaScript） 的经验，此处读取大部分将熟悉。 你可能需要先熟悉一下仅如何服务器代码添加到标记中*.cshtml*文件。

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>组合文本、 标记和代码块中的代码

在服务器代码块内，你通常想输出文本或标记 （或两者） 页。 如果服务器代码块包含的文本，不是代码和，而是应呈现原样，ASP.NET 将需要能够将该文本与代码区分开来。 有若干方法可实现此操作。

- 将文本括在 HTML 元素类似`<p></p>`或`<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    文本、 其他 HTML 元素和服务器代码表达式，可以包括 HTML 元素。 当 ASP.NET 发现开始 HTML 标记 (例如， `<p>`)、 呈现包括元素的所有内容和浏览器中，解决服务器代码表达式，因为它将是作为其内容。
- 使用`@:`运算符或`<text>`元素。 `@:`输出单个行的内容包含纯文本或不匹配的 HTML 标记;`<text>`元素包含多行输出。 当你不想要呈现的 HTML 元素作为输出的一部分，这些选项非常有用。  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    如果你想要输出的文本或不匹配的 HTML 标记的多个行，你可以在前面的每一行`@:`，或可以放置在行外侧`<text>`元素。 如`@:`运算符，`<text>`标记 ASP.NET 用于识别文本内容，并且永远不会呈现在页输出中。

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    第一个示例重复前面的示例，但使用一对`<text>`标记括起要呈现的文本。 在第二个示例中，`<text>`和`</text>`标记括起三行，它们都有一些非包含的文本和不匹配的 HTML 标记的行 (`<br />`)，以及服务器代码和匹配的 HTML 标记。 另外无法再次，先完成每个行分别`@:`运算符; 它们的方式都有效。

    > [!NOTE]
    > 本部分 &#8212; 中所示，输出文本的时使用 HTML 元素，`@:`运算符，或`<text>`元素 &#8212;ASP.NET 不进行 HTML 编码输出。 (如前所述，ASP.NET 未编码的服务器的代码表达式和服务器代码块前面带有输出`@`，除非在本节中所述的特殊情况。)

### <a name="whitespace"></a>Whitespace

在语句中 （和外部字符串文本） 的额外空间不会影响该语句：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

在语句中的换行符的语句上无效，并可以将包装语句的可读性。 以下语句是相同的：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

但是，不能环绕文本字符串中间的行。 下面的示例不起作用：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

若要合并的长字符串换行到多个行与上面的代码一样，有两个选项。 可以使用串联运算符 (`+`)，您可以看到这篇文章中更高版本。 你还可以使用`@`创建原义字符串文本，如本文前面看到的字符。 你可以位于同一行中逐字字符串文本：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>代码 （和标记） 注释

注释可以为自己或他人留言。 它们还使您可以禁用 (*注释掉*) 部分的代码或不想运行但想要保留暂时在页中的标记。

没有不同注释语法 Razor 代码以及的 HTML 标记。 与所有 Razor 代码中，Razor 注释处理 （，然后删除） 页发送到浏览器之前在服务器上。 因此，Razor 注释语法可以放置时编辑文件，但用户看不到，即使在页面源文件中，可以看到的注释到代码 （或甚至向的标记）。

对于 ASP.NET Razor 注释你开始在批注`@*`，并与的结尾`*@`。 注释可以是多了一行或多个行：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

下面是在代码块的注释：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

下面的代码行的相同块的代码，注释掉，以便它不会运行：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

在代码块中，除了使用 Razor 注释语法，你可以使用你使用的如 C# 的编程语言的注释语法：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

C# 中的单行注释前面带有`//`字符和多行注释开头`/*`和以结尾`*/`。 （与 Razor 注释，C# 注释不会呈现到浏览器。）

对于标记，你可能知道，你可以创建的 HTML 注释：

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

HTML 注释开头`<!--`字符和以结尾`-->`。 可以使用 HTML 注释来包围不仅文本，而且还可能想要保留在页中但不想要呈现任何 HTML 标记。 此 HTML 注释将隐藏，标记，并且它们包含的文本的整个内容：

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

与不同的是 Razor 注释，HTML 注释*是*呈现到页和用户可以通过查看页面源文件来查看它们。

有关 C# 的嵌套块，razor 具有限制。 有关详细信息请参阅[名为 C# 变量和嵌套块生成中断代码](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>变量

变量是用于存储数据的已命名的对象。 您可以命名变量的任何内容，但名称必须以字母字符开头，不能包含空格或保留的字符。

### <a name="variables-and-data-types"></a>变量和数据类型

变量可以具有特定的数据类型，这表示什么类型的数据存储在变量中。 你可以存储字符串值的字符串变量 (如&quot;Hello world&quot;)，整数变量，其中存储整数值 （如 3 或 79） 和各种 （例如，2012 年 4 月 12 日或 2009 年 3 月的格式存储日期值的日期变量). 还有许多其他可以使用的数据类型。

但是，你通常不必指定类型的变量。 大多数情况下，ASP.NET 可以找出基于了如何使用变量中的数据的类型。 （有时必须指定一个类型; 你将看到示例其中是如此。）

声明变量的使用`var`关键字 （如果你不想指定类型） 或通过使用类型的名称：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

下面的示例演示在网页中的变量的一些典型用法：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

如果合并前面的示例在页中，你将看到此浏览器中显示：

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>转换和测试数据类型

尽管 ASP.NET 通常可以自动确定数据类型，有时它不能。 因此，你可能需要帮助解决 ASP.NET，通过执行显式转换。 即使你无需转换类型，有时会有帮助进行测试以查看哪种类型的数据你可能正在处理。

最常见的情况是，你必须将字符串转换为另一个类型，如整数或日期。 下面的示例演示一种典型情况其中必须将字符串转换为数字。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

一般来说，用户输入发送给您作为字符串。 即使已提示用户输入一个数字，并且即使在提交用户输入而读取在代码中所输入的一个数字，数据是字符串格式。 因此，你必须将字符串转换为数字。 在示例中，如果你尝试对值执行算术运算，而不转换它们，以下的错误结果，因为 ASP.NET 不能添加两个字符串：

*不能隐式转换为 int 的 string 类型。*

若要将值转换为整数，则调用`AsInt`方法。 如果转换成功，你可以然后添加数字。

下表列出了一些常见的转换和测试方法的变量。

| **方法** | **说明** | **示例** |
| --- | --- | --- |
| `AsInt(), IsInt()` | 将转换为整数表示整数数量 （如"593") 的字符串。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)] |
| `AsBool(), IsBool()` | 将转换字符串如下所示&quot;true&quot;或&quot;false&quot;到类型为 Boolean 类型。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)] |
| `AsFloat(), IsFloat()` | 将具有类似的十进制值的字符串转换&quot;1.3&quot;或&quot;7.439&quot;为浮点数。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)] |
| `AsDecimal(), IsDecimal()` | 将具有类似的十进制值的字符串转换&quot;1.3&quot;或&quot;7.439&quot;为十进制数。 （在 ASP.NET 中，十进制数是比浮点数更精确。） | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)] |
| `AsDateTime(), IsDateTime()` | 将对 ASP.NET 表示的日期和时间值的字符串转换`DateTime`类型。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)] |
| `ToString()` | 将任何其他数据类型转换为字符串。 | [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)] |

## <a name="operators"></a>运算符

运算符是命令的关键字或哪种类型的表达式中执行将告诉 ASP.NET 的字符。 C# 语言 （和基于它的 Razor 语法） 支持很多运算符，但你只需以识别一些吧。 下表总结了最常用的运算符。

| **Operator** | **说明** | **示例** |
| --- | --- | --- |
| `+` `-` `*` `/` | 在数值表达式中使用的数学运算符。 | [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)] |
| `=` | 赋值。 将一条语句右侧的值分配给左侧的对象。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)] |
| `==` | 相等。 返回`true`如果这些值是否相等。 (请注意之间的区别`=`运算符和`==`运算符。) | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)] |
| `!=` | 不相等。 返回`true`如果值不相等。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)] |
| `< > <= >=` | 小于-号、 大于-比小于-或-等于和大于或等于。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)] |
| `+` | 串联，用来联接字符串。 ASP.NET 就会知道此运算符和加法运算符基于表达式的数据类型之间的差异。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)] |
| `+=` `-=` | 递增和递减运算符，从而添加，并且从变量 （分别） 减去 1。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)] |
| `.` | 点。 用于区分对象及其属性和方法。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)] |
| `()` | 括号。 用于到组表达式并将参数传递给方法。 | [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)] |
| `[]` | 方括号。 用于访问数组或集合中的值。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)] |
| `!` | 不是。 反转`true`值赋给`false`，反之亦然。 通常用作要测试的速记方法`false`(即，为不`true`)。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)] |
| `&&` <code>&#124;&#124;</code> | 逻辑与和或用于链接条件组合在一起。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)] |

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a>使用文件和代码中的文件夹路径

将在代码中经常处理的文件和文件夹的路径。 可能显示的开发计算机上，下面是一个网站的物理文件夹结构的示例：

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

下面是一些有关 Url 和路径的基本详细信息：

- URL 开头的域名 (`http://www.example.com`) 或服务器名称 (`http://localhost`， `http://mycomputer`)。
- URL 对应于主机计算机上的物理路径。 例如，`http://myserver`可能对应于文件夹*C:\websites\mywebsite*服务器上。
- 虚拟路径是简写形式来表示在代码中的路径，而无需指定完整路径。 它包含后面的域或服务器名称的 URL 的部分。 当你使用虚拟路径时，可以无需更新的路径，将代码移动到不同的域或服务器。

此处是一个示例来帮助你了解的差异：

| 完整的 URL | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| 服务器名称 | *mycompanyserver* |
| 虚拟路径 | */humanresources/CompanyPolicy.htm* |
| 物理路径 | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

虚拟根目录为 /，驱动器是一样的 c： 根目录 \。 （虚拟文件夹路径始终使用正斜杠。）文件夹的虚拟路径不需要作为物理文件夹; 具有相同的名称它可以是一个别名。 （生产服务器上的虚拟路径很少的匹配确切的物理路径。）

当代码中的文件和文件夹时，有时你需要引用的物理路径和有时虚拟路径，具体取决于你正在使用哪些对象。 ASP.NET 为你提供这些工具用于处理在代码中的文件和文件夹路径：`Server.MapPath`方法，与`~`运算符和`Href`方法。

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>转换虚拟与物理路径： Server.MapPath 方法

`Server.MapPath`方法将转换的虚拟路径 (如*/default.cshtml*) 为绝对物理路径 (如*C:\WebSites\MyWebSiteFolder\default.cshtml*)。 需要完整的物理路径时使用此方法。 一个典型示例是要读取或写入文本文件或 web 服务器上的图像文件时。

你通常不知道你的站点托管站点的服务器上的绝对物理路径，因此此方法可以将路径转换你知道-的虚拟路径-到你的服务器上的相应路径。 将虚拟路径传递给文件或文件夹的方法，并返回物理路径：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>引用的虚拟根： ~ 运算符和 Href 方法

在*.cshtml*或*.vbhtml*文件，你可以引用虚拟根路径使用`~`运算符。 这是非常方便，因为您可以来回移动网页，在站点中，并且它们包含至其他页面的任何链接不会被破坏。 也很方便以防曾经将你的网站移动到其他位置。 下面是一些可能的恶意活动：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

如果网站`http://myserver/myapp`，下面是运行页面时 ASP.NET 将这些路径的方式：

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

（你实际上不会看到这些路径的值的变量，但 ASP.NET 将则将路径视为即它们是一样。）

你可以使用`~`运算符在 （如上所述） 的服务器代码和在标记中，如下：

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

在标记中，你使用`~`运算符来创建资源，如图像文件、 其他网页和 CSS 文件的路径。 ASP.NET 页运行时，（代码和标记） 页中查找和解析所有`~`与相应路径的引用。

## <a name="conditional-logic-and-loops"></a>条件逻辑，并循环

ASP.NET 服务器代码允许你执行基于条件的任务，编写特定次数的重复语句的代码 （即运行一个循环中的代码）。

### <a name="testing-conditions"></a>测试条件

你使用对简单条件进行测试`if`语句，返回 true 或 false 基于你指定的测试：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

`if`关键字开始块。 实际的测试 （条件） 位于括号中，并返回 true 或 false。 运行测试为 true 的语句括在大括号中。 `if`语句可以包含`else`指定语句在条件为 false 时要运行的块：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

你可以添加多个条件使用`else if`块：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

在此示例中，如果第一个条件在 if 块不为 true，`else if`才会检查条件。 如果满足该条件，则中的语句`else if`块执行。 如果不符合任何条件中的语句`else`块执行。 你可以添加任意数量的 else if 块，并与然后关闭`else`作为阻止&quot;其他一切&quot;条件。

若要测试大量的条件，使用`switch`块：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

要测试的值位于括号中 (在示例中，`weekday`变量)。 每个单独测试使用`case`结尾冒号 （:） 语句。 如果值`case`语句与匹配的测试的值，在执行中该事例的块的代码。 关闭与每个 case 语句`break`语句。 (如果你忘记了要包括在每个中断`case`阻止，请从下一个代码`case`语句还将运行。)A`switch`块通常有`default`语句的最后一种情况作为&quot;其他一切&quot;运行任何其他情况下是否的选项。

在浏览器中显示的最后两个条件块的结果：

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>循环的代码

你经常需要重复运行相同的语句。 通过循环执行此操作。 例如，你通常运行的相同语句的每个项集合中的数据。 如果你知道完全多少次想要循环，则可以使用`for`循环。 这种类型是循环的的向上计数或倒计时特别有用：

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

循环开头`for`每个以分号结尾的关键字后, 跟置于括号内，三个语句。

- 在括号内的第一个语句 (`var i=10;`) 创建一个计数器并将初始化为 10。 你不需要命名该计数器`i`&#8212; 你可以使用任何变量。 当`for`循环运行时，此计数器时自动递增。
- 第二个语句 (`i < 21;`) 设置为你想要计数的距离的条件。 在这种情况下，您希望其转到最多 20 个 （即，继续阅读计数器时小于 21）。
- 第三个语句 (`i++` ) 使用一个递增运算符，它只需指定计数器应具有 1 添加到其每次循环运行的时。

在括号内是循环的将每个迭代中运行的代码。 标记将创建一个新段落 (`<p>`元素) 每个时间，并将行添加到输出中，显示的值`i`（计数器）。 运行此页时，该示例将创建 11 行显示输出，与每个行，该值指示项数中的文本。

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

如果你正在使用集合或数组，则通常会使用`foreach`循环。 集合是一组类似对象和`foreach`循环的允许您执行的任务集合中的每个项。 这种类型的循环是方便对于集合，因为与不同`for`循环中，你无需递增计数器或设置一个限制。 相反，`foreach`循环代码只需继续访问该集合之前完成。

例如，以下代码将返回中的项`Request.ServerVariables`集合，这是一个对象，包含有关你的 web 服务器的信息。 它使用`foreac`h 循环，以显示每个项的名称，通过创建新`<li>`HTML 项目符号列表中的元素。

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

`foreach`关键字后跟圆括号其中声明表示单个项集合中的变量 (在示例中， `var item`) 后, 跟`in`关键字后, 跟你想要循环访问的集合。 正文中`foreach`循环中，你可以访问使用你之前声明的变量的当前项。

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

若要创建更通用的循环，使用`while`语句：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

A`while`循环开头`while`关键字后, 跟括号你在其中指定循环继续的多长时间 (在此处，供，只要`countNum`小于 50)，然后用于重复的块。 循环通常递增 （添加） 或递减 （从减） 变量或用于计数的对象。 在示例中，`+=`运算符将添加到 1`countNum`每次循环运行的时。 (要递减进行倒计时的循环中的变量，你可以使用递减运算符`-=`)。

## <a name="objects-and-collections"></a>对象和集合

在 ASP.NET 网站中几乎所有操作是一个对象，包括 web 页本身。 本部分讨论一些重要的对象，你将经常你在代码中使用。

### <a name="page-objects"></a>Page 对象

在 ASP.NET 中的最基本对象是的页。 你可以访问的直接不带任何合格的对象的页对象的属性。 以下代码将获取该页面的文件路径，使用`Request`页的对象：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

为了更加明确要引用属性和当前的页对象上的方法，你可以选择使用关键字`this`来表示你的代码中的页对象。 下面是与前面的代码示例，`this`添加以表示页：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

你可以使用属性的`Page`对象以获取大量的信息，如：

- `Request`。 如你已经了解，这是信息的有关当前请求，包括哪种类型的浏览器发出了请求、 页面、 用户标识，等等的 URL 的集合。
- `Response`。 这是信息的有关将发送到浏览器中，当服务器代码程序完成运行响应 （页） 的集合。 例如，此属性可用于将信息写入到响应中。 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>（数组和字典） 的集合对象

A*集合*是一组相同的类型，例如的集合对象`Customer`数据库中的对象。 ASP.NET 包含多个内置集合，如`Request.Files`集合。

通常，你将使用集合中的数据。 两个常见集合类型是*数组*和*字典*。 如果你想要存储的相似的项目集合，但不想创建一个单独的变量以保存每个项，则数组非常有用：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

数组，可声明特定的数据类型，如`string`， `int`，或`DateTime`。 要指明变量可以包含一个数组，可将括号添加到声明 (如`string[]`或`int[]`)。 你可以访问项数组使用它们的位置 （索引） 中或通过使用`foreach`语句。 数组索引从零开始的是 &#8212;也就是说，第一项是在位置 0，第二项是在位置 1，依此类推。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

你可以通过获取确定数组中的项的数目及其`Length`属性。 若要获取特定项 （要搜索数组） 的数组中的位置，请使用`Array.IndexOf`方法。 你还可以执行诸如反向数组的内容 (`Array.Reverse`方法) 或对内容进行排序 (`Array.Sort`方法)。

在浏览器中显示的字符串数组代码的输出：

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

字典是键/值对的集合，其中提供的密钥 （或名称） 来设置或检索相应的值：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

若要创建一个字典，请使用`new`关键字以指示你要创建新的字典对象。 你可以将字典分配给变量的使用`var`关键字。 指示在使用命令的尖括号字典中的项的数据类型 ( `< >` )。 在声明结束时，你必须添加一对括号，因为这是实际创建一个新的字典的方法。

若要将项添加到字典中，你可以调用`Add`字典变量或方法 (`myScores`在这种情况下)，然后指定一个键和值。 或者，可以使用方括号来指示键并执行简单的赋值，如以下示例所示：

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

若要从字典中获取一个值，请在括号中指定的密钥：

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>调用带参数的方法

阅读本文前面时，与程序的对象可以具有方法。 例如，`Database`对象可能具有`Database.Connect`方法。 许多方法还具有一个或多个参数。 A*参数*是向某个方法传递的值以使该方法以完成其任务。 例如，查看有关声明`Request.MapPath`方法，采用三个参数：

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

（已换行以使其更具可读性。 请记住，你可以插入的换行符几乎任何位置使用但内部字符串用引号引起来。)

此方法对应的服务器上的物理路径返回到指定的虚拟路径。 该方法的三个参数`virtualPath`， `baseVirtualDir`，和`allowCrossAppMapping`。 （请注意，在声明中，列出了参数将接受的数据的数据类型）。在调用此方法时，你必须提供所有三个参数的值。

Razor 语法为你提供了用于将参数传递给方法的两个选项：*位置参数*和*命名参数*。 若要调用的使用位置参数的方法，请在方法声明中指定严格顺序传递参数。 （你将通常知道此顺序通过阅读的方法的文档。）你必须遵循的顺序，并且你不能跳过任何参数 （&） #8212;如果有必要，你将传递一个空字符串 (`""`) 或`null`为不具有的值的位置参数。

下面的示例假定你有一个名为的文件夹*脚本*在网站上。 该代码调用`Request.MapPath`方法并传递正确的顺序中的三个参数的值。 然后，它将显示生成的映射的路径。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

当一个方法具有多个参数时，您可以保留你的代码更具可读性使用命名参数。 若要调用的方法使用命名的参数，你指定的参数名称后跟一个冒号 （:），然后选择值。 命名参数的优点是，你可以将它们传递你希望任何顺序。 （一个缺点是方法调用不是为 compact。）

下面的示例调用按上面所述的相同方法，但使用命名参数提供的值：

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

如你所见，参数进行传递以不同的顺序。 但是，如果你运行前面的示例和此示例，它们将返回相同的值。

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>处理错误

### <a name="try-catch-statements"></a>Try Catch 语句

通常将在代码中可能会失败的原因，在你的控制范围具有语句。 例如:

- 如果你的代码尝试创建或访问文件，可能会出现各种类型的错误。 所需的文件可能不存在，则可能锁定，代码可能不具有权限，依次类推。
- 同样，如果你的代码尝试更新数据库中的记录，可以有权限问题、 与数据库的连接可能会丢弃，要保存的数据可能无效，依次类推。

编程术语中，这些情况下调用*异常*。 如果你的代码遇到的异常，它会生成 （引发） 一条错误消息的在最好的情况，令人讨厌的用户：

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

在情况下，你的代码可能会遇到的异常，并且为了避免这种类型的错误消息，你可以使用`try/catch`语句。 在`try`语句，你将运行要检查的代码。 在一个或多个`catch`语句，则你可以查看为特定可能发生的错误 （特定类型的异常）。 可以包括任意多个`catch`语句作为你需要查找以及预期的错误。

> [!NOTE]
> 我们建议你避免使用`Response.Redirect`中的方法`try/catch`语句，因为它可以在页中导致异常。


下面的示例演示创建第一个请求上的文本文件，然后显示一个按钮，使用户能够打开文件的页。 该示例有意使用错误的文件名称，以便它会导致异常。 代码包含`catch`两个可能的异常的语句： `FileNotFoundException`，就会出现此错误，文件名称是否和`DirectoryNotFoundException`，如果 ASP.NET 甚至找不到文件夹时会出现此情况。 （你可以取消注释在示例中的语句才能看到它时一切运行正常的运行。）

如果你的代码未处理异常，你会看到一个错误页面，如前面的屏幕快照。 但是，`try/catch`部分可帮助防止用户看到这些类型的错误。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>其他资源

**使用 Visual Basic 编程**


[附录： Visual Basic 语言和语法](https://go.microsoft.com/fwlink/?LinkId=202908)


**参考文档**


[ASP.NET 2.0](https://msdn.microsoft.com/library/ee532866.aspx)

[C# 语言](https://msdn.microsoft.com/library/kx37x362.aspx)
