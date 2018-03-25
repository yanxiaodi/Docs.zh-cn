---
title: "ASP.NET Core 中的标记帮助程序"
author: rick-anderson
description: "了解标记帮助程序的定义及其在 ASP.NET Core 中的用法。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 7/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/intro
ms.openlocfilehash: 939eccd45ec437f379fb9349c24246cc0683528b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-tag-helpers-in-aspnet-core"></a>ASP.NET Core 中标记帮助程序的介绍 

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-are-tag-helpers"></a>什么是标记帮助程序？

标记帮助程序使服务器端代码可以在 Razor 文件中参与创建和呈现 HTML 元素。 例如，内置 `ImageTagHelper` 可以将版本号追加到映像名称。 每当映像发生变化时，服务器都会为映像生成一个新的唯一版本，因此客户端总能获得当前映像（而不是过时的缓存映像）。 有多种常见任务（例如创建窗体、链接，加载资产等）的内置标记帮助程序，公共 GitHub 存储库和 NuGet 包中甚至还有更多可用标记帮助程序。 标记帮助程序使用 C# 创建，基于元素名称、属性名称或父标记以 HTML 元素为目标。 例如，应用 `LabelTagHelper` 属性时，内置 `LabelTagHelper` 可以 HTML `<label>` 元素为目标。 如果熟悉 [ HTML 帮助程序](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers)，则标记帮助程序将减少 Razor 视图中 HTML 和 C# 之间的显式转换。 在很多情况下，HTML 帮助程序为特定标记帮助程序提供了一种替代方法，但标记帮助程序不会替代 HTML 帮助程序，且并非每个 HTML 帮助程序都有对应的标记帮助程序，认识到这点也很重要。 [标记帮助程序与 HTML 帮助程序的比较](#tag-helpers-compared-to-html-helpers)更详细地介绍了两者之间的差异。

## <a name="what-tag-helpers-provide"></a>标记帮助程序的功能

**HTML 友好开发体验** 在多数情况下，使用标记帮助程序的 Razor 标记看起来像是标准 HTML。 熟悉 HTML/CSS/JavaScript 的前端设计师，无需学习 C# Razor 语法即可编辑 Razor。

**用于创建 HTML 和 Razor 标记的丰富 IntelliSense 环境** 这与 HTML 帮助程序形成鲜明对比，HTML 帮助程序是 Razor 视图中标记的曾用服务器端创建方法。 [标记帮助程序与 HTML 帮助程序的比较](#tag-helpers-compared-to-html-helpers)更详细地介绍了两者之间的差异。 [标记帮助程序的 IntelliSense 支持](#intellisense-support-for-tag-helpers)解释了 IntelliSense 环境。 即使是熟悉 Razor C# 语法的开发人员，使用标记帮助程序也比编写 C# Razor 标记更高效。

**使用仅在服务器上可用的信息，可提高生产力，并能生成更稳定、可靠和可维护的代码** 例如，过去更新映像时，必须在更改映像时更改映像名称。 出于性能原因，要主动缓存映像，而若不更改映像的名称，客户端就可能获得过时的副本。 以前，编辑完映像后，必须更改名称，而且需要更新 Web 应用中对该映像的每个引用。 这不仅大费周章，还容易出错（可能会漏掉某个引用、意外输入错误的字符串等等）内置 `ImageTagHelper` 可自动执行此操作。 `ImageTagHelper` 可将版本号追加到映像名称，这样每当映像出现更改时，服务器都会自动为该映像生成新的唯一版本。 客户端总是能获得最新映像。 使用 `ImageTagHelper` 实质上是免费获得稳健性而节省劳动力。

大多数内置标记帮助程序以现有 HTML 元素为目标，为该元素提供服务器端属性。 例如，Views/Account 文件夹中的许多视图都使用的 `<input>` 元素包含 `asp-for` 特性，它会将指定模型属性的名称提取到呈现的 HTML 中。 以下 Razor 标记：

```cshtml
<label asp-for="Email"></label>
```

生成以下 HTML：

```cshtml
<label for="Email">Email</label>
```

通过 `LabelTagHelper` 中的 `For` 属性，可使用 `asp-for` 特性。 请参阅[创作标记帮助程序](authoring.md)，获取详细信息。

## <a name="managing-tag-helper-scope"></a>管理标记帮助程序作用域

标记帮助程序作用域由 `@addTagHelper`、`@removeTagHelper` 和“!”选择退出字符联合控制。

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a>使用 `@addTagHelper` 添加标记帮助程序

如果创建名为 AuthoringTagHelpers的新 ASP.NET Core Web 应用（无身份验证），将向项目添加以下 Views/_ViewImports.cshtml 文件：

[!code-cshtml[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

`@addTagHelper` 指令让视图可以使用标记帮助程序。 在此示例中，视图文件是 Views/_ViewImports.cshtml，“Views”文件夹及其子目录中的所有视图文件都会默认继承它，使得标记帮助程序可用。 上面的代码使用通配符语法（“\*”），指定程序集 (Microsoft.AspNetCore.Mvc.TagHelpers) 中的所有标记帮助程序对于 Views 目录或子目录中的所有视图文件可用。 `@addTagHelper` 后第一个参数指定要加载的标记帮助程序（我们使用“\*”指定加载所有标记帮助程序），第二个参数“Microsoft.AspNetCore.Mvc.TagHelpers”指定包含标记帮助程序的程序集。 Microsoft.AspNetCore.Mvc.TagHelpers 是内置 ASP.NET Core 标记帮助程序的程序集。

要公开此项目中的所有标记帮助程序（将创建名为 AuthoringTagHelpers 的程序集），可使用以下内容：

[!code-cshtml[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

如果项目包含具有默认命名空间 (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) 的 `EmailTagHelper`，则可提供标记帮助程序的完全限定名称 (FQN)：

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

若要使用 FQN 将标记帮助程序添加到视图，请首先添加 FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`)，然后添加程序集名称 (AuthoringTagHelpers)。 大多数开发人员更喜欢使用“\*”通配符语法。 使用通配符语法，可在 FQN 中插入通配符“\*”作为后缀。 例如，以下任何指令都将引入 `EmailTagHelper`：

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

如前所述，将 `@addTagHelper` 指令添加到 Views/_ViewImports.cshtml 文件，将使标记帮助程序对于 Views 目录及子目录中的所有视图文件可用。 如果想选择仅对特定视图公开标记帮助程序，可在这些视图文件中使用 `@addTagHelper` 指令。

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a>`@removeTagHelper` 删除标记帮助程序

`@removeTagHelper` 与 `@addTagHelper` 具有相同的两个参数，它会删除之前添加的标记帮助程序。 例如，应用于特定视图的 `@removeTagHelper` 会删除该视图中的指定标记帮助程序。 在 Views/Folder/_ViewImports.cshtml 文件中使用 `@removeTagHelper`，将从 Folder 中的所有视图删除指定的标记帮助程序。

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a>使用 _ViewImports.cshtml 文件控制标记帮助程序作用域

可将 _ViewImports.cshtml 添加到任何视图文件夹，视图引擎将同时应用该文件和 Views/_ViewImports.cshtml 文件中的指令。 如果为 Home 视图添加空的 Views/Home/_ViewImports.cshtml 文件，则不会发生任何更改，因为 _ViewImports.cshtml 文件是附加的。 添加到 Views/Home/_ViewImports.cshtml 文件（不在默认 Views/_ViewImports.cshtml 文件中）的任何 `@addTagHelper` 指令，都只会将这些标记帮助程序公开给 Home 文件夹中的视图。

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a>选择退出各个元素

使用标记帮助程序选择退出字符（“!”），可在元素级别禁用标记帮助程序。 例如，使用标记帮助程序选择退出字符在 `<span>` 中禁用 `Email` 验证：

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

须将标记帮助程序选择退出字符应用于开始和结束标记。 （将选择退出字符添加到开始标记时，Visual Studio 编辑器会自动为结束标记添加相应字符）。 添加选择退出字符后，元素和标记帮助程序属性不再以独特字体显示。

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a>使用 `@tagHelperPrefix` 阐明标记帮助程序用途

`@tagHelperPrefix` 指令可指定一个标记前缀字符串，以启用标记帮助程序支持并阐明标记帮助程序用途。 例如，可以将以下标记添加到 Views/_ViewImports.cshtml 文件：

```cshtml
@tagHelperPrefix th:
```
在以下代码图像中，标记帮助程序前缀设置为 `th:`，所以只有使用前缀 `th:` 的元素才支持标记帮助程序（可使用标记帮助程序的元素以独特字体显示）。 `<label>` 和 `<input>` 元素具有标记帮助程序前缀，可使用标记帮助程序，而 `<span>` 元素则相反。

![图像](intro/_static/thp.png)

适用于 `@addTagHelper` 的层次结构规则也适用于 `@tagHelperPrefix`。

## <a name="intellisense-support-for-tag-helpers"></a>标记帮助程序的 Intellisense 支持

在 Visual Studio 中创建新的 ASP.NET Web 应用时，会添加 NuGet 包“Microsoft.AspNetCore.Razor.Tools”。 这是添加标记帮助程序工具的包。

请考虑编写 HTML `<label>` 元素。 只要在 Visual Studio 编辑器中输入 `<l`，IntelliSense 就会显示匹配的元素：

![图像](intro/_static/label.png)

不仅会获得 HTML 帮助，还会有图标（下方带有“<>”的“@" symbol with "）

![图像](intro/_static/tagSym.png)

将该元素标识为标记帮助程序的目标。 纯 HTML 元素（如 `fieldset`）显示“<>”图标。

纯 HTML `<label>` 标记以棕色字体显示 HTML 标记（使用默认 Visual Studio 颜色主题时），以红色字体显示属性，并以蓝色字体显示属性值。

![图像](intro/_static/LableHtmlTag.png)

输入 `<label` 后，IntelliSense 会列出可用的 HTML/CSS 属性和以标记帮助程序为目标的属性：

![图像](intro/_static/labelattr.png)

通过 IntelliSense 语句完成功能，按 Tab 键即可用选择的值完成语句：

![图像](intro/_static/stmtcomplete.png)

只要输入标记帮助程序属性，标记和属性字体就会更改。 如果使用默认的 Visual Studio“蓝色”或“浅色”颜色主题，则字体是粗体紫色。 如果使用“深色”主题，则字体为粗体青色。 本文档中的图像在使用默认主题时截取的。

![图像](intro/_static/labelaspfor2.png)

可在双引号 ("") 内输入 Visual Studio CompleteWord 快捷方式（[默认值](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio)为 Ctrl+空格键），即可使用 C#，就像在 C# 类中一样。 IntelliSense 会显示页面模型上的所有方法和属性。 由于属性类型是 `ModelExpression`，所以这些方法和属性可用。 在下图中，我正在编辑 `Register` 视图，所以 `RegisterViewModel` 是可用的。

![图像](intro/_static/intellemail.png)

IntelliSense 会列出页面上模型可用的属性和方法。 丰富 IntelliSense 环境可帮助选择 CSS 类：

![图像](intro/_static/iclass.png)

![图像](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a>标记帮助程序与 HTML 帮助程序的比较

标记帮助程序附加到 Razor 视图中的 HTML 元素，[HTML 帮助程序](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers)是作为与 Razor 视图中 HTML 交织的方法被调用的。 请考虑下列 Razor 标记，它创建具有 CSS 类“caption”的 HTML 标签：

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

艾特 (`@`) 符号告诉 Razor 这是代码的开始。 接下来的两个参数（“FirstName”和“First Name:”）是字符串，所以 [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) 无法提供帮助。 最后一个参数：

```cshtml
new {@class="caption"}
```

是用于表示属性的匿名对象。 由于 class 是 C# 中的保留关键字，因此要使用 `@` 符号强制 C# 将“@class=”解释为符号（属性名称）。 对于前端设计师（熟悉 HTML/CSS/JavaScript 及其他客户端技术，但不熟悉 C# 和 Razor 的人），这行代码中大部分内容是陌生是。 必须在没有 IntelliSense 帮助的情况下编写整行代码。

使用 `LabelTagHelper`，相同标记可以编写为：

![图像](intro/_static/label2.png)

使用标记帮助程序版本，只要在 Visual Studio 编辑器中输入 `<l`，IntelliSense 就会显示匹配的元素：

![图像](intro/_static/label.png)

IntelliSense 可帮助编写整行。 `LabelTagHelper` 也默认将 `asp-for` 属性值（“FirstName”）的内容设置为“First Name”：即在属性值中每个大写字母前添加一个空格，将驼峰式大小写的属性转换为由属性名称组成的语句。 在下列标记中：

![图像](intro/_static/label2.png)

生成：

```cshtml
<label class="caption" for="FirstName">First Name</label>
```

如果将内容添加到 `<label>`，则不会使用由驼峰式大小写转换为语句式书写的内容。 例如:

![图像](intro/_static/1stName.png)

生成：

```cshtml
<label class="caption" for="FirstName">Name First</label>
```

以下代码图像显示了由 Visual Studio 2015 包含的旧版 ASP.NET 4.5.x MVC 模板生成的 Views/Account/Register.cshtml Razor 视图的 Form 部分。

![图像](intro/_static/regCS.png)

Visual Studio 编辑器以灰色背景显示 C# 代码。 例如，`AntiForgeryToken` HTML 帮助程序：

```cshtml
@Html.AntiForgeryToken()
```

以灰色背景显示。 Register 视图中的标记大部分是 C#。 将其与使用标记帮助程序的等效方法进行比较：

![图像](intro/_static/regTH.png)

与 HTML 帮助程序方法相比，此标记更清晰，更容易阅读、编辑和维护。 C# 代码会被减少至服务器需要知道的最小值。 Visual Studio 编辑器以独特的字体显示标记帮助程序的目标标记。

请考虑 Email 组：

[!code-csharp[Main](intro/sample/Register.cshtml?range=12-18)]

每个“asp-”属性都有一个“Email”值，但是“Email”不是字符串。 在此上下文中，“Email”是 `RegisterViewModel` 的 C# 模型表达式属性。

Visual Studio 编辑器可帮助编写注册窗体的标记帮助程序方法中的所有标记，而 Visual Studio 不会为 HTML 帮助程序方法中的大多数代码提供帮助。 [标记帮助程序的 IntelliSense 支持](#intellisense-support-for-tag-helpers)详细介绍了如何在 Visual Studio 编辑器中使用标记帮助程序。

## <a name="tag-helpers-compared-to-web-server-controls"></a>标记帮助程序与 Web 服务器控件的比较

* 标记帮助程序不拥有与其相关的元素：它们只是参与元素和内容的呈现。 ASP.NET [Web 服务器控件](https://msdn.microsoft.com/library/7698y1f0.aspx)在页面上进行声明和调用。

* [Web 服务器控件](https://msdn.microsoft.com/library/zsyt68f1.aspx)具有可观的生命周期，因而难以进行开发和调试。

* 通过 Web 服务器控件，可使用客户端控件向客户端文档对象模型 (DOM) 元素添加功能。 标记帮助程序没有 DOM。

* Web 服务器控件包括自动浏览器检测。 标记帮助程序不了解浏览器。

* 通常不能撰写 Web 服务器控件时，多个标记帮助程序可作用于同一元素（请参阅[避免标记帮助程序冲突](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts)）。

* 标记帮助程序可以修改其作用域内 HTML 元素的标记和内容，但不会直接修改页面上的其他内容。 Web 服务器控件的作用域较广，并且可以执行影响页面其他部分的操作，从而可能造成意想不到的副作用。

* Web 服务器控件使用类型转换器将字符串转换为对象。 使用标记帮助程序时，本身就用 C# 语言工作，因此无需进行类型转换。

* Web 服务器控件使用 [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) 实现组件和控件的运行时和设计时行为。 `System.ComponentModel` 包括用于属性和类型转换器的实现、数据源绑定和组件授权的基类和接口。 与通常派生自 `TagHelper` 的标记帮助程序相比，`TagHelper` 基类仅公开两个方法，即 `Process` 和 `ProcessAsync`。

## <a name="customizing-the-tag-helper-element-font"></a>自定义标记帮助程序元素字体

可以在“工具” > “选项” > “环境” > “字体和颜色”中自定义字体和着色：

![图像](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a>其他资源

* [创作标记帮助程序](xref:mvc/views/tag-helpers/authoring)
* [使用窗体](xref:mvc/views/working-with-forms)
* [GitHub 上的 TagHelperSamples](https://github.com/dpaquette/TagHelperSamples) 包含用于处理 [Bootstrap](http://getbootstrap.com/) 的标记帮助程序示例。
* [使用窗体](xref:mvc/views/working-with-forms)
