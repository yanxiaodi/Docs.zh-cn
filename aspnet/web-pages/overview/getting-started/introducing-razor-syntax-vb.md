---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: "使用 Razor 语法 (Visual Basic) 的 ASP.NET Web 编程简介 |Microsoft 文档"
author: tfitzmac
description: "本附录可与 ASP.NET 网页编程的概述在 Visual Basic 中，使用 Razor 语法。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 8f5d223a5944d8adb9fe65c89e87829d18d1c7ee
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>使用 Razor 语法 (Visual Basic) 的 ASP.NET Web 编程简介
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本文概述你的编程与 ASP.NET Web Pages 使用 Razor 语法和 Visual Basic。 ASP.NET 是用于在 web 服务器上运行动态 web 页的 Microsoft 的技术。
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


使用 Razor 语法中使用的 ASP.NET Web Pages 的大多数示例使用 C#。 但 Razor 语法也支持 Visual Basic。 要编制 ASP.NET 网页在 Visual Basic 中，你创建一个包含网页*.vbhtml*文件扩展名，然后添加 Visual Basic 代码。 本文提供使用 Visual Basic 语言和用于创建 ASP.NET 网页语法的概述。

> [!NOTE]
> Microsoft WebMatrix 的默认网站模板 (**面包店**，**照片库**，和**入门站点**等) 在 C# 和 Visual Basic 版本中可用。 你可以为 NuGet 包安装 Visual Basic 的模板。 名为的文件夹中的站点的根文件夹中安装的网站模板*Microsoft 模板*。


## <a name="the-top-8-programming-tips"></a>最重要的 8 编程提示

本部分列出绝对需要知道当你开始编写使用 Razor 语法的 ASP.NET 服务器代码的一些提示。

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1.将代码添加到页使用 @ 字符

`@`字符开始内联表达式、 单语句块和多语句块：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

在浏览器中所显示的结果：

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **HTML 编码**
> 
> 当你在页中使用显示内容`@`字符，如前面的示例中，ASP.NET 进行 HTML 编码输出。 这将替换保留的 HTML 字符 (如`<`和`>`和`&`) 与启用要显示为字符而不是被解释为 HTML 标记或实体的网页中的字符的代码。 HTML 编码，在服务器代码中的输出可能显示不正常，而无需无法公开安全风险的页。
> 
> 如果你的目标是输出将标记呈现为标记的 HTML 标记 (例如`<p></p>`，段落或`<em></em>`强调文本)，请参阅明[组合文本、 标记和代码块中的代码](#BM_CombiningTextMarkupAndCode)本文后续部分中。
> 
> 你可以阅读更多有关中的 HTML 编码[使用 ASP.NET Web Pages 站点中的 HTML 窗体](https://go.microsoft.com/fwlink/?LinkId=202892)。


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2.你将包含代码的代码块...端代码

代码块包含一个或多个代码语句，并且包括与关键字`Code`和`End Code`。 将打开`Code`关键字后立即`@`字符 &#8212; 它们之间不能有空格。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

在浏览器中所显示的结果：

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3.在块中，你最终通过行中断每个代码语句

在 Visual Basic 代码块中，每个语句以换行符结尾。 （在本文的后面将看到一种方法，如果需要包装为多行长代码语句。）

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4.使用变量来存储值

你可以将值存储在*变量*，包括字符串、 数字和日期，等等。创建一个新的变量使用`Dim`关键字。 你可以直接在页中使用插入变量值`@`。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

在浏览器中所显示的结果：

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5.将文本字符串值括在双引号内

A*字符串*是作为文本处理的字符的序列。 若要指定一个字符串，则将它括在双引号中：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

若要嵌入字符串值中的双引号括起来，插入两个双引号字符。 如果你想要一次出现在页面输出的双引号字符，则输入其作为`""`中带引号的字符串，并将其作为输入在您希望其出现两次，如果`""""`中带引号的字符串。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

在浏览器中所显示的结果：

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6.Visual Basic 代码不区分大小写

Visual Basic 语言不区分大小写。 编程关键字 (如`Dim`， `If`，和`True`) 和变量名 (如`myString`，或`subTotal`) 可以在任何情况下编写。

下面的代码行将值分配给该变量`lastname`使用小写名称，然后输出到使用的是大写名称的页变量的值。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

在浏览器中所显示的结果：

![vb 语法 5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7.大部分代码涉及对象的使用

对象表示，你可以使用编程件事情 &#8212;页、 文本框、 文件、 映像、 web 请求、 电子邮件、 客户记录 （数据库行），等等。对象具有描述其特征 &#8212; 的属性文本框对象具有`Text`属性，请求对象具有`Url`属性，电子邮件具有`From`属性和客户对象都有`FirstName`属性。 对象还具有方法&quot;谓词&quot;他们可以执行。 示例包括文件对象的`Save`方法中，映像对象的`Rotate`方法和电子邮件对象的`Send`方法。

通常将使用`Request`对象，这为你提供信息，如值形式的字段 （文本框中，等等），在页面上哪种类型的浏览器发出了请求、 页面、 用户标识，等等的 URL。此示例演示如何访问属性`Request`对象以及如何调用`MapPath`方法`Request`对象，这将使您在服务器上的页的绝对路径：

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

在浏览器中所显示的结果：

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8.你可以编写做出决策的代码

动态网页的一项重要功能是你可以确定要执行的操作根据条件。 若要执行此操作的最常见方法是使用`If`语句 (和可选`Else`语句)。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

语句`If IsPost`是写入的速记方式`If IsPost = True`。 连同`If`语句，有各种方法来测试条件，重复代码块，并依此类推，它们进行了描述这篇文章中更高版本。

在浏览器中所显示的结果 (单击后**提交**):

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP GET 和 POST 方法以及 IsPost 属性**
> 
> 用于网页 (HTTP) 的协议支持非常有限的数量的方法 (&quot;谓词&quot;)，用于向服务器发出请求。 两个最常见的是 GET，用于读取某页以及开机自检，用于提交的页面。 一般情况下，用户请求页面时，第一次请求页时使用 GET。 如果用户在填写窗体，然后单击**提交**，浏览器向服务器发出的 POST 请求。
> 
> 在 web 编程中，它通常是有助于你了解是否请求该页为 GET 或 POST，以便你知道如何处理该页。 在 ASP.NET Web 页中，你可以使用`IsPost`属性以查看是否 GET 或 POST 请求。 如果请求是 POST，`IsPost`属性将返回 true，并且可执行诸如读取窗体上的文本框的值。 你将看到许多示例演示如何处理以不同的方式根据的值页`IsPost`。


## <a name="a-simple-code-example"></a>一个简单代码示例

此过程演示如何创建阐释基本的编程技术的页。 在示例中，你将创建一个允许用户输入两个数字，然后再将它们添加显示结果页面。

1. 在编辑器中创建一个新文件并将其命名*AddNumbers.vbhtml*。
2. 将下面的代码和标记复制到页中，替换已在页中的任何内容。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    下面是为你要注意一些事项：

    - `@`字符在页中，启动代码的第一个块，它位于之前`totalMessage`变量嵌入底部附近。
    - 在页面顶部块括在`Code...End Code`。
    - 变量`total`， `num1`， `num2`，和`totalMessage`存储多个数字和字符串。
    - 分配给的文字字符串值`totalMessage`变量是在双引号内。
    - 因为 Visual Basic 代码时不区分大小写，`totalMessage`变量使用页面底部附近，其名称只需要匹配在页面顶部的变量声明的拼写是否正确。 大小写并不重要。
    - 表达式`num1.AsInt()`  +  `num2.AsInt()`演示如何使用对象和方法。 `AsInt`上每个变量的方法将转换为整数 （整数） 可添加到由用户输入的字符串。
    - `<form>`标记包含`method="post"`属性。 此步骤指定当用户单击**添加**，页面将发送到服务器使用 HTTP POST 方法。 当提交页面，则代码`If IsPost`计算结果为 true，条件的代码运行时，显示的添加数字结果。
3. 保存页并在浏览器中运行它。 (请确保页中选择**文件**工作区之前运行它。)输入两个整数，然后单击**添加**按钮。

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Visual Basic 语言和语法

前面你已了解如何创建 ASP.NET web 页中，以及如何将服务器代码添加到 HTML 标记的一个基本示例。 此处将介绍使用 Visual Basic 编写使用 Razor 语法 &#8212; 的 ASP.NET 服务器代码的基础知识即，使用编程语言规则。

如果你有使用编程 （尤其是如果您使用过 C、 c + +、 C#、 Visual Basic 或 JavaScript） 的经验，此处读取大部分将熟悉。 你可能需要先熟悉一下仅如何 WebMatrix 代码添加到标记中*.vbhtml*文件。

### <a id="BM_CombiningTextMarkupAndCode"></a>组合文本、 标记和代码块中的代码

在服务器代码块内，你将通常想输出文本和到页面的标记。 如果服务器代码块包含的文本，不是代码和，而是应呈现原样，ASP.NET 将需要能够将该文本与代码区分开来。 有若干方法可实现此操作。

- 将文本括在 HTML 块元素类似`<p></p>`或`<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    文本、 其他 HTML 元素和服务器代码表达式，可以包括 HTML 元素。 当 ASP.NET 发现开始 HTML 标记 (例如， `<p>`)，它会呈现的所有内容的元素，作为其内容是在浏览器 （和解析的服务器代码表达式）。

- 使用`@:`运算符或`<text>`元素。 `@:`输出单个行的内容包含纯文本或不匹配的 HTML 标记;`<text>`元素包含多行输出。 当你不想要呈现的 HTML 元素作为输出的一部分，这些选项非常有用。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    下面的示例重复前面的示例，但使用一对`<text>`标记括起要呈现的文本。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    在下面的示例中，`<text>`和`</text>`标记括起三行，它们都有一些非包含的文本和不匹配的 HTML 标记的行 (`<br />`)，以及服务器代码和匹配的 HTML 标记。 另外无法再次，先完成每个行分别`@:`运算符; 它们的方式都有效。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > 本部分 &#8212; 中所示，输出文本的时使用 HTML 元素，`@:`运算符，或`<text>`元素 &#8212;ASP.NET 不进行 HTML 编码输出。 (如前所述，ASP.NET 未编码的服务器的代码表达式和服务器代码块前面带有输出`@`，除非在本节中所述的特殊情况。)

### <a name="whitespace"></a>Whitespace

在语句中 （和外部字符串文本） 的额外空间不会影响该语句：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>长语句拆分为多行

你可以通过使用下划线字符长码语句拆分为多个行`_`(在 Visual Basic 中的这种行为称为*继续符*) 之后的每一行代码。 若要中断的语句到下一行，行末尾添加一个空格，然后继续符。 在下一行继续该语句。 你可以包装到以提高可读性所需的尽可能多行语句。 以下语句是相同的：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

但是，不能环绕文本字符串中间的行。 下面的示例不起作用：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

若要合并的长字符串换行到多个行与上面的代码一样，你将需要使用*串联运算符*(`&`)，您可以看到这篇文章中更高版本。

### <a name="code-comments"></a>代码注释

注释可以为自己或他人留言。 带有前缀 razor 语法注释`@*`和以结尾`*@`。

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

在代码块内，你可以使用 Razor 语法注释，或者可以使用普通的 Visual Basic 注释字符，这是单引号 (`'`) 每一行的前缀。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>变量

变量是用于存储数据的已命名的对象。 您可以命名变量的任何内容，但名称必须以字母字符开头，不能包含空格或保留的字符。 在 Visual Basic 中，正如你看到的更早版本，并不重要的变量名称中的字母大小写。

### <a name="variables-and-data-types"></a>变量和数据类型

变量可以具有特定的数据类型，这表示什么类型的数据存储在变量中。 你可以存储字符串值的字符串变量 (如&quot;Hello world&quot;)，整数变量，其中存储整数值 （如 3 或 79） 和各种 （例如，2012 年 4 月 12 日或 2009 年 3 月的格式存储日期值的日期变量). 还有许多其他可以使用的数据类型。

但是，你不必指定类型的变量。 在大多数情况下 ASP.NET 可以找出基于了如何使用变量中的数据的类型。 （有时必须指定一个类型; 你将看到示例其中是如此。）

若要声明变量时不指定类型的情况下，使用`Dim`加变量的名称 (例如， `Dim myVar`)。 若要使用的类型声明变量时，使用`Dim`加变量的名称后, 跟`As`，然后是类型名称 (例如， `Dim myVar As String`)。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

下面的示例演示某些内联表达式，在网页中使用变量。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

在浏览器中所显示的结果：

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>转换和测试数据类型

尽管 ASP.NET 通常可以自动确定数据类型，有时它不能。 因此，你可能需要帮助解决 ASP.NET，通过执行显式转换。 即使你无需转换类型，有时会有帮助进行测试以查看哪种类型的数据你可能正在处理。

最常见的情况是，你必须将字符串转换为另一个类型，如整数或日期。 下面的示例演示一种典型情况其中必须将字符串转换为数字。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

一般来说，用户输入发送给您作为字符串。 即使已提示用户输入一个数字，并且即使在提交用户输入而读取在代码中所输入的一个数字，数据是字符串格式。 因此，你必须将字符串转换为数字。 在示例中，如果你尝试对值执行算术运算，而不转换它们，以下的错误结果，因为 ASP.NET 不能添加两个字符串：

`Cannot implicitly convert type 'string' to 'int'.`

若要将值转换为整数，则调用`AsInt`方法。 如果转换成功，你可以然后添加数字。

下表列出了一些常见的转换和测试方法的变量。

| **方法** | **说明** | **示例** |
| --- | --- | --- |
| `AsInt(), IsInt()` | 将表示为整数的字符串转换 (如&quot;593&quot;) 为整数。 | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)] |
| `AsBool(), IsBool()` | 将转换字符串如下所示&quot;true&quot;或&quot;false&quot;到类型为 Boolean 类型。 | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)] |
| `AsFloat(), IsFloat()` | 将具有类似的十进制值的字符串转换&quot;1.3&quot;或&quot;7.439&quot;为浮点数。 | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)] |
| `AsDecimal(), IsDecimal()` | 将具有类似的十进制值的字符串转换&quot;1.3&quot;或&quot;7.439&quot;为十进制数。 （在 ASP.NET 中，十进制数是比浮点数更精确。） | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)] |
| `AsDateTime(), IsDateTime()` | 将对 ASP.NET 表示的日期和时间值的字符串转换`DateTime`类型。 | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)] |
| `ToString()` | 将任何其他数据类型转换为字符串。 | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)] |

## <a name="operators"></a>运算符

运算符是命令的关键字或哪种类型的表达式中执行将告诉 ASP.NET 的字符。 Visual Basic 支持许多运算符，但你只需以识别一些若要开始开发 ASP.NET web 页。 下表总结了最常用的运算符。

| **Operator** | **说明** | **示例** |
| --- | --- | --- |
| `+ - * /` | 在数值表达式中使用的数学运算符。 | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)] |
| `=` | 分配和相等性。 根据上下文，或者将语句右侧的值分配给左侧，对象，或检查值相等。 | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)] |
| `<>` | 不相等。 返回`True`如果值不相等。 | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)] |
| `< > <= >=` | 小于、 大于、 小于或等于、 和大于或等于。 | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)] |
| `&` | 串联，用来联接字符串。 | [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)] |
| `+= -=` | 递增和递减运算符，从而添加，并且从变量 （分别） 减去 1。 | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)] |
| `.` | 点。 用于区分对象及其属性和方法。 | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)] |
| `()` | 括号。 为组表达式，用于将参数传递到方法，并访问数组和集合的成员。 | [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)] |
| `Not` | 不是。 反转 true 值为 false，反之亦然。 通常用作要测试的速记方法`False`(即，为不`True`)。 | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)] |
| `AndAlso OrElse` | 逻辑与和或用于链接条件组合在一起。 | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)] |

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

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>引用的虚拟根： ~ 运算符和 Href 方法

在*.cshtml*或*.vbhtml*文件，你可以引用虚拟根路径使用`~`运算符。 这是非常方便，因为您可以来回移动网页，在站点中，并且它们包含至其他页面的任何链接不会被破坏。 也很方便以防曾经将你的网站移动到其他位置。 下面是一些可能的恶意活动：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

如果网站`http://myserver/myapp`，下面是运行页面时 ASP.NET 将这些路径的方式：

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

（你实际上不会看到这些路径的值的变量，但 ASP.NET 将则将路径视为即它们是一样。）

你可以使用`~`运算符在 （如上所述） 的服务器代码和在标记中，如下：

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

在标记中，你使用`~`运算符来创建资源，如图像文件、 其他网页和 CSS 文件的路径。 ASP.NET 页运行时，（代码和标记） 页中查找和解析所有`~`与相应路径的引用。

## <a name="conditional-logic-and-loops"></a>条件逻辑，并循环

ASP.NET 服务器代码允许你执行基于条件和重复特定次数，即代码的语句运行循环的编写代码的任务）。

### <a name="testing-conditions"></a>测试条件

你使用对简单条件进行测试`If...Then`语句，它返回`True`或`False`基于你指定的测试：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

`If`关键字开始块。 实际的测试 （条件） 遵循`If`关键字，并返回 true 或 false。 `If`语句结尾`Then`。 如果测试为 true，将运行的语句括通过`If`和`End If`。 `If`语句可以包含`Else`指定语句在条件为 false 时要运行的块：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

如果`If`语句启动代码块时，无需使用普通`Code...End Code`语句以包括的块。 可以仅添加`@`到块，以及它将工作。 此方法适用于`If`以及编程关键字跟代码块，包括其他 Visual Basic `For`， `For Each`， `Do While`，等等。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

你可以添加多个条件使用一个或多个`ElseIf`块：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

在此示例中，如果第一个条件中`If`块不为 true，`ElseIf`才会检查条件。 如果满足该条件，则中的语句`ElseIf`块执行。 如果不符合任何条件中的语句`Else`块执行。 你可以添加任意数量的`ElseIf`块，并与然后关闭`Else`作为阻止&quot;其他一切&quot;条件。

若要测试大量的条件，使用`Select Case`块：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

要测试的值位于 （在示例中，工作日变量） 的括号中。 每个单独测试使用`Case`列出一个值的语句。 如果值`Case`语句与匹配的测试的值的代码`Case`执行块。

在浏览器中显示的最后两个条件块的结果：

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>循环的代码

你经常需要重复运行相同的语句。 通过循环执行此操作。 例如，你通常运行的相同语句的每个项集合中的数据。 如果你知道完全多少次想要循环，则可以使用`For`循环。 这种类型是循环的的向上计数或倒计时特别有用：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

循环开头`For`关键字后, 跟三个元素：

- 后立即`For`语句，您将计数器变量声明 (无需使用`Dim`) 然后中表示范围， `i = 10 to 20`。 这意味着变量`i`将开始计数 10 并继续，直到它达到 20 （含）。
- 之间`For`和`Next`语句为块的内容。 这可以包含一个或多个代码语句，执行与每个循环。
- `Next i`语句结束循环。 它会递增的计数器，并启动循环的下一个迭代。

之间的代码行`For`和`Next`行包含每个迭代的循环中运行的代码。 标记将创建一个新段落 (`<p>`元素) 每个时间，并将行添加到输出中，显示的值 i （计数器）。 运行此页时，该示例将创建 11 行显示输出，与每个行，该值指示项数中的文本。

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

如果你正在使用集合或数组，则通常会使用`For Each`循环。 集合是一组类似对象和`For Each`循环的允许您执行的任务集合中的每个项。 这种类型的循环是方便对于集合，因为与不同`For`循环中，你无需递增计数器或设置一个限制。 相反，`For Each`循环代码只需继续访问该集合之前完成。

此示例将返回中的项`Request.ServerVariables`集合 （其中包含有关你的 web 服务器的信息）。 它使用`For Each`循环来显示每个项的名称，通过创建新`<li>`HTML 项目符号列表中的元素。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

`For Each`关键字后跟表示单个项集合中的变量 (在示例中， `myItem`) 后, 跟`In`关键字后, 跟你想要循环访问的集合。 正文中`For Each`循环中，你可以访问使用你之前声明的变量的当前项。

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

若要创建更通用的循环，使用`Do While`语句：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

此循环开头`Do While`关键字后, 跟一个条件后, 跟用于重复的块。 循环通常递增 （添加） 或递减 （从减） 变量或用于计数的对象。 在示例中，`+=`已将 1 运算符添加到变量的值每次循环运行的时。 (要递减进行倒计时的循环中的变量，你可以使用递减运算符`-=`。)

## <a name="objects-and-collections"></a>对象和集合

在 ASP.NET 网站中几乎所有操作是一个对象，包括 web 页本身。 本部分讨论一些重要的对象，你将经常你在代码中使用。

### <a name="page-objects"></a>Page 对象

在 ASP.NET 中的最基本对象是的页。 你可以访问的直接不带任何合格的对象的页对象的属性。 以下代码将获取该页面的文件路径，使用`Request`页的对象：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

你可以使用属性的`Page`对象以获取大量的信息，如：

- `Request`。 如你已经了解，这是信息的有关当前请求，包括哪种类型的浏览器发出了请求、 页面、 用户标识，等等的 URL 的集合。
- `Response`。 这是信息的有关将发送到浏览器中，当服务器代码程序完成运行响应 （页） 的集合。 例如，此属性可用于将信息写入到响应中。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>（数组和字典） 的集合对象

集合是一组相同的类型，例如的集合对象`Customer`数据库中的对象。 ASP.NET 包含多个内置集合，如`Request.Files`集合。

通常，你将使用集合中的数据。 两个常见集合类型是*数组*和*字典*。 如果你想要存储的相似的项目集合，但不想创建一个单独的变量以保存每个项，则数组非常有用：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

数组，可声明特定的数据类型，如`String`， `Integer`，或`DateTime`。 要指明变量可以包含一个数组，可将括号添加到声明中的变量名称 (如`Dim myVar() As String`)。 你可以访问项数组使用它们的位置 （索引） 中或通过使用`For Each`语句。 数组索引从零开始的是 &#8212;也就是说，第一项是在位置 0，第二项是在位置 1，依此类推。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

你可以通过获取确定数组中的项的数目及其`Length`属性。 若要获取数组中特定项的位置 (即，搜索数组)，使用`Array.IndexOf`方法。 你还可以执行诸如反向数组的内容 (`Array.Reverse`方法) 或对内容进行排序 (`Array.Sort`方法)。

在浏览器中显示的字符串数组代码的输出：

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

字典是键/值对的集合，其中提供的密钥 （或名称） 来设置或检索相应的值：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

若要创建一个字典，请使用`New`关键字以指示你要创建一个新`Dictionary`对象。 你可以将字典分配给变量的使用`Dim`关键字。 指示在使用括号字典中的项的数据类型 ( `( )` )。 在声明的末尾，你必须添加另一对括号，因为这是实际创建一个新的字典的方法。

若要将项添加到字典中，你可以调用`Add`字典变量或方法 (`myScores`在这种情况下)，然后指定一个键和值。 或者，可以使用括号以指示键和执行简单的赋值，如以下示例所示：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

若要从字典中获取一个值，请在括号中指定的密钥：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>调用带参数的方法

正如你看到这篇文章中的更早版本，与程序的对象具有方法。 例如，`Database`对象可能具有`Database.Connect`方法。 许多方法还具有一个或多个参数。 A*参数*是向某个方法传递的值以使该方法以完成其任务。 例如，查看有关声明`Request.MapPath`方法，采用三个参数：

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

此方法对应的服务器上的物理路径返回到指定的虚拟路径。 该方法的三个参数`virtualPath`， `baseVirtualDir`，和`allowCrossAppMapping`。 （请注意，在声明中，列出了参数将接受的数据的数据类型）。在调用此方法时，你必须提供所有三个参数的值。

在使用 Razor 语法，你使用 Visual Basic，你可以用于将参数传递给方法的两个选项：*位置参数*或*命名参数*。 若要调用的使用位置参数的方法，请在方法声明中指定严格顺序传递参数。 （你将通常知道此顺序通过阅读的方法的文档。）你必须遵循的顺序，并且你不能跳过任何参数 （&） #8212;如果有必要，你将传递一个空字符串 (`""`) 则不具有的值的位置参数为 null。

下面的示例假定你有一个名为的文件夹*脚本*在网站上。 该代码调用`Request.MapPath`方法并传递正确的顺序中的三个参数的值。 然后，它将显示生成的映射的路径。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

当存在多个参数的方法时，你可以将代码更简洁且更具可读性使用命名参数。 若要调用的方法使用命名的参数，指定参数名称后跟`:=`然后提供值。 命名参数的一个优点是，你可以按任何所需的顺序添加它们。 （一个缺点是方法调用不是为 compact。）

下面的示例调用按上面所述的相同方法，但使用命名参数提供的值：

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

如你所见，参数进行传递以不同的顺序。 但是，如果你运行前面的示例和此示例，它们将返回相同的值。

## <a name="handling-errors"></a>处理错误

### <a name="try-catch-statements"></a>Try Catch 语句

通常将在代码中可能会失败的原因，在你的控制范围具有语句。 例如:

- 如果你的代码尝试打开、 创建、 读取或写入文件时，可能会出现各种类型的错误。 所需的文件可能不存在，则可能锁定，代码可能不具有权限，依次类推。
- 同样，如果你的代码尝试更新数据库中的记录，可以有权限问题、 与数据库的连接可能会丢弃，要保存的数据可能无效，依次类推。

编程术语中，这些情况下调用*异常*。 如果你的代码遇到的异常，它会生成 （引发） 错误消息，它是，在最好的情况，令人讨厌的用户。

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

在情况下，你的代码可能会遇到的异常，并且为了避免这种类型的错误消息，你可以使用`Try/Catch`语句。 在`Try`语句，你将运行要检查的代码。 在一个或多个`Catch`语句，则你可以查看为特定可能发生的错误 （特定类型的异常）。 可以包括任意多个`Catch`语句作为你需要查找你要预测，将出现的错误。

> [!NOTE]
> 我们建议你避免使用`Response.Redirect`中的方法`Try/Catch`语句，因为它可以在页中导致异常。


下面的示例演示创建第一个请求上的文本文件，然后显示一个按钮，使用户能够打开文件的页。 该示例有意使用错误的文件名称，以便它会导致异常。 代码包含`Catch`两个可能的异常的语句： `FileNotFoundException`，就会出现此错误，文件名称是否和`DirectoryNotFoundException`，如果 ASP.NET 甚至找不到文件夹时会出现此情况。 （你可以取消注释在示例中的语句才能看到它时一切运行正常的运行。）

如果你的代码未处理异常，你会看到一个错误页面，如前面的屏幕快照。 但是，`Try/Catch`部分可帮助防止用户看到这些类型的错误。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>其他资源

### <a name="reference-documentation"></a>参考文档

- [ASP.NET 2.0](https://msdn.microsoft.com/library/ee532866.aspx)
- [Visual Basic 语言](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
