---
title: "ASP.NET Core 的 Razor 语法参考"
author: rick-anderson
description: "了解 Razor 标记语法，该语法用于将基于服务器的代码嵌入网页中。"
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/razor
ms.openlocfilehash: 98021cc76555f0c1402764c845471a4730b01b20
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="razor-syntax-for-aspnet-core"></a>ASP.NET Core 的 Razor 语法

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Luke Latham](https://github.com/guardrex)、[Taylor Mullen](https://twitter.com/ntaylormullen) 和 [Dan Vicarel](https://github.com/Rabadash8820)

Razor 是一种标记语法，用于将基于服务器的代码嵌入网页中。 Razor 语法由 Razor 标记、C# 和 HTML 组成。 包含 Razor 的文件通常具有 *.cshtml* 文件扩展名。

## <a name="rendering-html"></a>呈现 HTML

默认 Razor 语言为 HTML。 从 Razor 标记呈现 HTML 与从 HTML 文件呈现 HTML 并没有什么不同。 服务器会按原样呈现 *.cshtml* Razor 文件中的 HTML 标记。

## <a name="razor-syntax"></a>Razor 语法

Razor 支持 C#，并使用 `@` 符号从 HTML 转换为 C#。 Razor 计算 C# 表达式，并将它们呈现在 HTML 输出中。

当 `@` 符号后跟 [Razor 保留关键字](#razor-reserved-keywords)时，它会转换为 Razor 特定标记。 否则会转换为纯 C#。

若要对 Razor 标记中的 `@` 符号进行转义，请使用另一个 `@` 符号：

```cshtml
<p>@@Username</p>
```

该代码在 HTML 中使用单个 `@` 符号呈现：

```html
<p>@Username</p>
```

包含电子邮件地址的 HTML 属性和内容不将 `@` 符号视为转换字符。 Razor 分析不会处理以下示例中的电子邮件地址：

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>隐式 Razor 表达式

隐式 Razor 表达式以 `@` 开头，后跟 C# 代码：

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

隐式表达式不能包含空格，但 C# `await` 关键字除外。 如果该 C# 语句具有明确的结束标记，则可以混用空格：

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

隐式表达式**不能**包含 C# 泛型，因为括号 (`<>`) 内的字符会被解释为 HTML 标记。 以下代码**无**效：

```cshtml
<p>@GenericMethod<int>()</p>
```

上述代码生成与以下错误之一类似的编译器错误：

 * "int" 元素未结束。 所有元素都必须自结束或具有匹配的结束标记。
 *  无法将方法组 "GenericMethod" 转换为非委托类型 "object"。 是否希望调用此方法?` 
 
泛型方法调用必须包装在[显式 Razor 表达式](#explicit-razor-expressions)或 [Razor 代码块](#razor-code-blocks)中。

## <a name="explicit-razor-expressions"></a>显式 Razor 表达式

显式 Razor 表达式由 `@` 符号和平衡圆括号组成。 若要呈现上一周的时间，可使用以下 Razor 标记：

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

将计算 `@()` 括号中的所有内容，并将其呈现到输出中。

前面部分中所述的隐式表达式通常不能包含空格。 在下面的代码中，不会从当前时间减去一周：

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

该代码呈现以下 HTML：

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

可以使用显式表达式将文本与表达式结果串联起来：

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

如果不使用显式表达式，`<p>Age@joe.Age</p>` 会被视为电子邮件地址，因此会呈现 `<p>Age@joe.Age</p>`。 如果编写为显式表达式，则呈现 `<p>Age33</p>`。


显式表达式可用于从 *.cshtml* 文件中的泛型方法呈现输出。 在隐式表达式中，括号 (`<>`) 内的字符被解释为 HTML 标记。 以下标记**不**是有效的 Razor：

```cshtml
<p>@GenericMethod<int>()</p>
```

上述代码生成与以下错误之一类似的编译器错误：

 * "int" 元素未结束。 所有元素都必须自结束或具有匹配的结束标记。
 *  无法将方法组 "GenericMethod" 转换为非委托类型 "object"。 是否希望调用此方法?` 
 
 以下标记展示了此代码的正确编写方式。 此代码以显式表达式的形式编写：

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a>表达式编码

计算结果为字符串的 C# 表达式采用 HTML 编码。 计算结果为 `IHtmlContent` 的 C# 表达式直接通过 `IHtmlContent.WriteTo` 呈现。 计算结果不为 `IHtmlContent` 的 C# 表达式通过 `ToString` 转换为字符串，并在呈现前进行编码。

```cshtml
@("<span>Hello World</span>")
```

该代码呈现以下 HTML：

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

该 HTML 在浏览器中显示为：

```
<span>Hello World</span>
```

`HtmlHelper.Raw` 输出不进行编码，但呈现为 HTML 标记。

> [!WARNING]
> 对未经审查的用户输入使用 `HtmlHelper.Raw` 会带来安全风险。 用户输入可能包含恶意的 JavaScript 或其他攻击。 审查用户输入比较困难。 应避免对用户输入使用 `HtmlHelper.Raw`。

```cshtml
@Html.Raw("<span>Hello World</span>")
```

该代码呈现以下 HTML：

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Razor 代码块

Razor 代码块以 `@` 开头，并括在 `{}` 中。 代码块内的 C# 代码不会呈现，这点与表达式不同。 一个视图中的代码块和表达式共享相同的作用域并按顺序进行定义：

```cshtml
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```

该代码呈现以下 HTML：

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a>隐式转换

代码块中的默认语言为 C#，不过，Razor 页面可以转换回 HTML：

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a>带分隔符的显式转换

若要定义应呈现 HTML 的代码块子节，请使用 Razor **\<text>** 标记将要呈现的字符括起来：

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

使用此方法可呈现未被 HTML 标记括起来的 HTML。 如果没有 HTML 或 Razor 标记，会发生 Razor 运行时错误。

**\<text>** 标记可用于在呈现内容时控制空格：

* 仅呈现 **\<text>** 标记之间的内容。 
* **\<text>** 标记之前或之后的空格不会显示在 HTML 输出中。

### <a name="explicit-line-transition-with-"></a>使用 @ 的显式行转换：

若要在代码块内以 HTML 的形式呈现整个行的其余内容，请使用 `@:` 语法：

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

如果代码中没有 `@:`，会生成 Razor 运行时错误。

警告：Razor 文件中多余的 `@` 字符可能会导致代码块中后面的语句发生编译器错误。 这些编译器错误可能难以理解，因为实际错误发生在报告的错误之前。 将多个隐式/显式表达式合并到单个代码块以后，经常会发生此错误。

## <a name="control-structures"></a>控制结构

控制结构是对代码块的扩展。 代码块的各个方面（转换为标记、内联 C#）同样适用于以下结构：

### <a name="conditionals-if-else-if-else-and-switch"></a>条件语句 @if、else if、else 和 @switch

`@if` 控制何时运行代码：

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

`else` 和 `else if` 不需要 `@` 符号：

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```

以下标记展示如何使用 switch 语句：

```cshtml
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number wasn't 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a>循环语句 @for、@foreach、@while 和 @do while

可以使用循环控制语句呈现模板化 HTML。 若要呈现一组人员：

```cshtml
@{
    var people = new Person[]
    {
          new Person("Weston", 33),
          new Person("Johnathon", 41),
          ...
    };
}
```

支持以下循环语句：

`@for`

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```cshtml
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```cshtml
@{ var i = 0; }
@while (i < people.Length)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
}
```

`@do while`

```cshtml
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a>复合语句 @using

在 C# 中，`using` 语句用于确保释放对象。 在 Razor 中，可使用相同的机制来创建包含附加内容的 HTML 帮助程序。 在下面的代码中，HTML 帮助程序使用 `@using` 语句呈现表单标记：


```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

可以使用[标记帮助程序](xref:mvc/views/tag-helpers/intro)执行作用域级别的操作。

### <a name="try-catch-finally"></a>@try、catch、finally

异常处理与 C# 类似：

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

Razor 可以使用 lock 语句来保护关键节：

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>注释

Razor 支持 C# 和 HTML 注释：

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

该代码呈现以下 HTML：

```html
<!-- HTML comment -->
```

在呈现网页之前，服务器会删除 Razor 注释。 Razor 使用 `@*  *@` 来分隔注释。 以下代码已被注释禁止，因此服务器不呈现任何标记：

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a>指令

Razor 指令由隐式表达式表示：`@` 符号后跟保留关键字。 指令通常用于更改视图分析方式或启用不同的功能。

通过了解 Razor 如何为视图生成代码，更易理解指令的工作原理。

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

该代码生成与下面类似的类：

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Getting old ain't for wimps! - Anonymous";

        WriteLiteral("/r/n<div>Quote of the Day: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

本文后面的[查看为视图生成的 Razor C# 类](#viewing-the-razor-c-class-generated-for-a-view)部分说明了如何查看此生成的类。

### <a name="using"></a>@using

`@using` 指令用于向生成的视图添加 C# `using` 指令：

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

`@model` 指令指定传递到视图的模型类型：

```cshtml
@model TypeNameOfModel
```

在使用个人用户帐户创建的 ASP.NET Core MVC 应用中，*Views/Account/Login.cshtml* 视图包含以下模型声明：

```cshtml
@model LoginViewModel
```

生成的类继承自 `RazorPage<dynamic>`：

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Razor 公开了 `Model` 属性，用于访问传递到视图的模型：

```cshtml
<div>The Login Email: @Model.Email</div>
```

`@model` 指令指定此属性的类型。 该指令将 `RazorPage<T>` 中的 `T` 指定为生成的类，视图便派生自该类。 如果未指定 `@model` 指令，则 `Model` 属性的类型为 `dynamic`。 模型的值会从控制器传递到视图。 有关详细信息，请参阅[强类型模型和 @model 关键字。

### <a name="inherits"></a>@inherits

`@inherits` 指令对视图继承的类提供完全控制：

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

下面的代码是一种自定义 Razor 页面类型：

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

`CustomText` 显示在视图中：

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

该代码呈现以下 HTML：

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 `@model` 和 `@inherits` 可在同一视图中使用。 `@inherits` 可位于视图导入的 *_ViewImports.cshtml* 文件中：

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

下面的代码是一种强类型视图：

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

如果在模型中传递“rick@contoso.com”，视图将生成以下 HTML 标记：

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


`@inject` 指令允许 Razor 页面将服务从[服务容器](xref:fundamentals/dependency-injection)注入到视图。 有关详细信息，请参阅[视图中的依赖关系注入](xref:mvc/views/dependency-injection)。

### <a name="functions"></a>@functions

`@functions` 指令允许 Razor 页面将函数级别的内容添加到视图：

```cshtml
@functions { // C# Code }
```

例如:

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

该代码生成以下 HTML 标记：

```html
<div>From method: Hello</div>
```

以下代码是生成的 Razor C# 类：

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

`@section` 指令与[布局](xref:mvc/views/layout)结合使用，允许视图将内容呈现在 HTML 页面的不同部分。 有关详细信息，请参阅[部分](xref:mvc/views/layout#layout-sections-label)。

## <a name="tag-helpers"></a>标记帮助程序

[标记帮助程序](xref:mvc/views/tag-helpers/intro)有三个相关指令。

| 指令 | 函数 |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | 向视图提供标记帮助程序。 |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | 从视图中删除以前添加的标记帮助程序。 |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | 指定标记前缀，以启用标记帮助程序支持并阐明标记帮助程序的用法。 |

## <a name="razor-reserved-keywords"></a>Razor 保留关键字

### <a name="razor-keywords"></a>Razor 关键字

* page（需要 ASP.NET Core 2.0 及更高版本）
* 函数
* 继承
* 模型
* section
* helper（ASP.NET Core 当前不支持）

Razor 关键字使用 `@(Razor Keyword)` 进行转义（例如，`@(functions)`）。

### <a name="c-razor-keywords"></a>C# Razor 关键字

* 大小写
* do
* default
* for
* foreach
* if
* else
* 锁定
* switch
* try
* catch
* finally
* using
* while

C# Razor 关键字必须使用 `@(@C# Razor Keyword)` 进行双转义（例如，`@(@case)`）。 第一个 `@` 对 Razor 分析器转义。 第二个 `@` 对 C# 分析器转义。

### <a name="reserved-keywords-not-used-by-razor"></a>Razor 不使用的保留关键字

* namespace
* 类

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>查看为视图生成的 Razor C# 类

将下面的类添加到 ASP.NET Core MVC 项目：

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

使用 `CustomTemplateEngine` 类替代 MVC 添加的 `RazorTemplateEngine`：

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

在 `CustomTemplateEngine` 的 `return csharpDocument` 语句上设置断点。 当程序执行在断点处停止时，查看 `generatedCode` 的值。

![generatedCode 的文本可视化工具视图](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a>视图查找和区分大小写

Razor 视图引擎为视图执行区分大小写的查找。 但是，实际查找取决于基础文件系统：

* 基于文件的源： 
  * 在使用不区分大小写的文件系统的操作系统（例如，Windows）上，物理文件提供程序查找不区分大小写。 例如，`return View("Test")` 可匹配 */Views/Home/Test.cshtml*、*/Views/home/test.cshtml* 以及任何其他大小写变体。
  * 在区分大小写的文件系统（例如，Linux、OSX 以及使用 `EmbeddedFileProvider` 构建的文件系统）上，查找区分大小写。 例如，`return View("Test")` 专门匹配 */Views/Home/Test.cshtml*。
* 预编译视图：在 ASP.NET Core 2.0 及更高版本中，预编译视图查找在所有操作系统上均不区分大小写。 该行为与 Windows 上物理文件提供程序的行为相同。 如果两个预编译视图仅大小写不同，则查找的结果具有不确定性。

建议开发人员将文件和目录名称的大小写与以下项的大小写匹配：

    * 区域、控制器和操作名称。 
    * Razor 页面。
    
匹配大小写可确保无论使用哪种基础文件系统，部署都能找到其视图。
