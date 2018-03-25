---
title: "防止跨站点脚本 (XSS) 在 ASP.NET 核心"
author: rick-anderson
description: "了解有关跨站点脚本 (XSS) 和一些解决这一漏洞在 ASP.NET Core 应用程序技术。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cross-site-scripting
ms.openlocfilehash: 9e54ee0b1169c01629c3cd91a378509a73c53904
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="preventing-cross-site-scripting-xss-in-aspnet-core"></a>防止跨站点脚本 (XSS) 在 ASP.NET 核心

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

跨站点脚本 (XSS) 是一个安全漏洞，这会使攻击者将客户端脚本 (通常是 JavaScript) 放入网页。 当其他用户加载受影响的页的攻击者脚本将运行时，从而使攻击者窃取 cookie 和会话令牌更改通过 DOM 操作网页的内容或将浏览器重定向到另一页。 当应用程序接受用户输入，并输出它在页中而不验证、 编码或转义它时，通常会发生 XSS 漏洞。

## <a name="protecting-your-application-against-xss"></a>保护 XSS 针对应用程序

在通过诱使你的应用程序插入到工作原理的基本级别 XSS`<script>`标记到呈现页中，或通过插入`On*`插入元素的事件。 开发人员应使用以下的预防步骤以避免 XSS 引入其应用程序。

1. 永远不会，将不受信任的数据放入你的 HTML 输入中，除非按照下面的步骤的其余部分。 不受信任的数据是可能由攻击者、 HTML 窗体输入、 查询字符串、 HTTP 标头，甚至数据源从数据库中，攻击者可能能够破坏您的数据库，即使它们不能违反你的应用程序控制任何数据。

2. 将 HTML 元素中的不受信任的数据之前请确保它为 HTML 编码。 HTML 编码采取字符如&lt;和会变成安全的形式，例如&amp;lt;

3. 将不受信任的数据放入 HTML 特性之前请确保它是 HTML 编码的属性。 HTML 特性编码为 HTML 编码的超集，并将其他字符编码如"和。

4. 将不受信任的数据放入 JavaScript 之前将数据放在一个 HTML 元素在运行时检索其内容。 如果这不是可能的则确保数据被编码 JavaScript。 JavaScript 编码 javascript 采用危险的字符并将其替换为其十六进制，例如&lt;将编码为`\u003C`。

5. 将不受信任的数据放入一个 URL 查询字符串之前确保它是编码的 URL。

## <a name="html-encoding-using-razor"></a>使用 Razor 的 HTML 编码

自动使用 MVC Razor 引擎将所有编码输出源自变量，除非您真正努力工作以避免其执行此操作。 它使用编码规则，每当你使用的 HTML 特性 *@* 指令。 为 HTML 属性编码为 HTML 编码，这意味着你不必考虑自己是否应使用 HTML 编码或 HTML 特性编码的超集。 你必须确保您仅使用在 HTML 上下文中，不是在尝试将直接插入 JavaScript 不受信任的输入时。 标记帮助程序还会编码标记参数中使用的输入。

考虑以下 Razor 视图;

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

此视图输出的内容*untrustedInput*变量。 此变量包含在 XSS 攻击中，即使用某些字符&lt;，"和&gt;。 查看源显示呈现的输出编码为：

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> ASP.NET 核心 MVC 提供`HtmlString`在输出时不自动编码的类。 这应永远不会用于与不受信任的输入结合使用这会将公开 XSS 漏洞。

## <a name="javascript-encoding-using-razor"></a>使用 Razor Javascript 编码

可能有些时候你想要插入处理在视图中的 JavaScript 值。 有两种方法可以实现此目的。 插入简单值的最安全方法是将值放在一个标记的数据属性并检索你在 JavaScript。 例如:

```none
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

这将生成以下 HTML

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

其中，运行时，将呈现以下操作;

```none
<"123">
   <"123">
   ```

你还可以直接调用 JavaScript 编码器

```none
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

这将呈现在浏览器中，如下所示;

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> 不连接不受信任的输入，在 JavaScript 中创建 DOM 元素。 应使用`createElement()`并将属性值分配适当如`node.TextContent=`，或使用`element.SetAttribute()` / `element[attribute]=`否则向基于 DOM 的 XSS 公开自己。

## <a name="accessing-encoders-in-code"></a>访问代码中的编码器

HTML、 JavaScript 和 URL 编码器可供代码使用两种方式，可以将它们通过注入[依赖关系注入](../fundamentals/dependency-injection.md#fundamentals-dependency-injection)或者你可以使用中包含的默认编码器`System.Text.Encodings.Web`命名空间。 如果你使用的默认编码器，则你应用到任何要被视为为安全的字符范围不会生效-默认编码器使用的可能的安全编码规则。

若要使用可配置编码器通过你的构造函数应采用的 DI *HtmlEncoder*， *JavaScriptEncoder*和*UrlEncoder*作为适当的参数。 例如，

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a>编码的 URL 参数

如果你想要构建 URL 查询字符串以不受信任的输入作为值使用`UrlEncoder`值进行编码。 例如，应用于对象的

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

编码 encodedValue 后变量将包含`%22Quoted%20Value%20with%20spaces%20and%20%26%22`。 空格、 引号、 标点和其他不安全字符百分比编码为其十六进制值，例如空格字符将成为 %20。

>[!WARNING]
> 不要使用不受信任的输入的 URL 路径的一部分。 始终将不受信任的输入传递作为查询字符串值。

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>自定义编码器

默认情况下编码器使用安全列表限制为基本拉丁 Unicode 范围并为其等效的字符代码在该范围之外的所有字符进行都编码。 此行为还会影响 Razor TagHelper 和 HtmlHelper 呈现，因为它将使用编码器输出字符串。

这背后的原因是为了防止未知或将来的浏览器 bug （以前的浏览器 bug 具有触发向上分析基于非英语字符的处理）。 如果您的网站进行大量使用非拉丁字符，例如中文、 西里尔文或其他人这是可能不希望的行为。

你可以自定义编码器安全列表，以包括范围使用适合于在启动期间，你的应用程序中的 Unicode `ConfigureServices()`。

例如，使用默认配置可能会使用 Razor HtmlHelper 如下所示;

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

当您查看的网页的源时，你将看到已呈现，如下所示，使用中文文本编码;

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

若要扩大范围的字符视为安全编码器您将插入以下行到`ConfigureServices()`中的方法`startup.cs`;

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

此示例扩大安全的列表，使其包含 Unicode 范围 CjkUnifiedIdeographs。 呈现的输出将现在变为

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

安全列表范围被指定为 Unicode 代码图表、 不语言。 [Unicode 标准](http://unicode.org/)具有的列表[代码图表](http://www.unicode.org/charts/index.html)可用来查找包含你字符图表。 每个编码器，Html、 JavaScript 和 Url，必须单独配置。

> [!NOTE]
> 自定义安全列表仅影响通过 DI 来源的编码器。 如果你直接访问通过编码器`System.Text.Encodings.Web.*Encoder.Default`然后默认情况下，基本拉丁将使用仅安全列表。

## <a name="where-should-encoding-take-place"></a>编码的 take 应放置位置？

常规接受的做法是编码发生在输出和编码的值应永远不会存储在数据库中。 在输出编码，可将使用数据，例如，从 HTML 为查询字符串值更改。 它还使您可以轻松地搜索你的数据，而无需搜索前对值进行编码，并使您可以充分利用任何更改或对编码器所做的 bug 修复。

## <a name="validation-as-an-xss-prevention-technique"></a>验证在 XSS 预防方法

验证可以在限制 XSS 攻击的有用工具。 例如，一个简单的数值字符串只能包含字符 0-9 将不会触发 XSS 受到攻击。 验证变得更复杂应想要接受用户输入的中的 HTML HTML 输入内容分析为难以进行，即使不是不可能。 MarkDown 以及其他文本的格式将丰富的输入的更安全选项。 你应永远不会依赖于单独的验证。 始终对不受信任的输入，输出之前进行编码，无论何种验证已执行。
