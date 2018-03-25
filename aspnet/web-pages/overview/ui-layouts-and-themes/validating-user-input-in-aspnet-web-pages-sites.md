---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: "验证用户输入，在 ASP.NET Web 页 (Razor) 站点 |Microsoft 文档"
author: tfitzmac
description: "此文章介绍了如何验证您获得用户的信息&mdash;也就是说，若要确保用户输入有效 HTML 中的信息在中窗体另存为..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 3bde2a4ea69577ebcbe3e9e89a7ee07e6ece8dd1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>验证在 ASP.NET Web 页 (Razor) 站点中的用户输入
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 此文章介绍了如何验证您获得用户的信息&mdash;也就是说，若要确保用户输入有效 HTML 中的信息窗体在 ASP.NET Web 页 (Razor) 站点。
> 
> 你将学习：
> 
> - 如何检查用户的输入与你定义的验证条件匹配。
> - 如何确定是否已通过所有验证测试。
> - 如何显示验证错误 （以及如何设置它们的格式）。
> - 如何验证不是直接来自用户的数据。
> 
> 这些是 ASP.NET 的文章中介绍的编程概念：
> 
> - `Validation`帮助器。
> - `Html.ValidationSummary`和`Html.ValidationMessage`方法。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET 网页 (Razor) 3
>   
> 
> 本教程还适用于 ASP.NET Web Pages 2。


本文包含以下各节：

- [用户输入验证的概述](#Overview_of_User_Input_Validation)
- [验证用户输入](#Validating_User_Input)
- [添加客户端验证](#Adding_Client-Side_Validation)
- [格式设置的验证错误](#Formatting_Validation_Errors)
- [验证不是直接来自用户的数据](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>用户输入验证的概述

如果你要求用户在页中输入信息 — 例如，转换为格式-务必要确保他们输入的值有效。 例如，你不想要处理缺少关键信息的窗体。

当用户向 HTML 窗体中输入值时，则他们输入的值是字符串。 在许多情况下，你需要的值是某些其他数据类型，如整数或日期。 因此，您还需要确保用户输入的值可以正确转换为适当的数据类型。

你还可能值具有某些限制。 即使用户正确输入一个整数，例如，你可能需要确保该值在某个特定范围。

![使用 CSS 样式类的验证错误](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **重要**验证用户输入也很重要的安全。 如果你限制用户可以在窗体中输入的值，则可以减少有人可以输入一个值，可能会危害你的站点的安全性的可能性。


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>验证用户输入

在 ASP.NET 网页 2 中，你可以使用`Validator`用于测试用户输入帮助程序。 基本的方法是执行以下操作：

1. 确定哪些输入你想要验证的元素 （字段）。

    通常验证中的值`<input>`窗体中的元素。 但是，它是一个好办法验证所有输入，即使输入来自如约束元素`<select>`列表。 这有助于确保用户不绕过页上的控件和提交窗体。
2. 为每个输入元素使用的方法，则在网页代码中，添加单独的验证检查`Validation`帮助器。

    若要检查的必填字段，请使用`Validation.RequireField(field, [error message])`（为单个字段） 或`Validation.RequireFields(field1, field2, ...))`（有关字段的列表）。 对于其他类型的验证，使用`Validation.Add(field, ValidationType)`。 有关`ValidationType`，你可以使用这些选项：

    `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField [, error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min, max [, error message])`  
`Validator.RegEx(pattern [, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`
3. 当提交页面时，检查是否通过检查已通过验证`Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    如果不存在任何验证错误，则跳过正常的页处理。 例如，如果该页面的用途是更新数据库，不执行操作，直到在修复所有验证错误。
4. 如果有验证错误，显示错误消息中页面的标记使用`Html.ValidationSummary`或`Html.ValidationMessage`，和/或文件名。

下面的示例演示一个页，演示了这些步骤。

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

若要查看验证的工作原理，运行此页，并有意犯的错误。 例如，下面是什么如果您忘记输入过程名称时，如果输入页的外观，并且如果输入无效的日期：

![呈现的页面中的验证错误](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>添加客户端验证

默认情况下，用户输入进行验证，在用户提交页后，即，在服务器代码中执行验证。 此方法的缺点是用户不知道他们进行后的出现错误直到它们提交页面。 如果窗体是长或者过于复杂，则报告错误，只有在提交页面后，才可以将向用户很不方便。

你可以添加支持，以在客户端脚本中执行验证。 在这种情况下，用户在浏览器中工作，则不执行验证。 例如，假设你指定的值应为整数。 如果用户输入一个非整数值，用户离开输入字段时，会报告错误。 用户获取将为它们提供方便的即时反馈。 基于客户端验证还可以减少的用户必须提交表单更正多个错误的次数。

> [!NOTE]
> 即使你使用客户端验证，始终也是在服务器代码中执行验证。 在服务器代码中执行验证是一种安全措施，以防用户跳过基于客户端的验证。


1. 在页中注册以下 JavaScript 库：  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

 两个库是内容交付网络 (CDN)，从加载的因此你不一定对您的计算机或服务器。 但是，你必须具有的本地副本*jquery.validate.unobtrusive.js*。 如果你不已正在使用 WebMatrix 模板 (如**入门站点**) 包含库中，创建基于一个网页站点**入门站点**。 然后将复制*.js*给当前站点的文件。
2. 标记，您要验证，每个元素中添加对的调用`Validation.For(field)`。 此方法会发出由客户端验证的属性。 (而不是发出实际的 JavaScript 代码时，该方法发出这样的属性`data-val-...`。 这些属性支持使用 jQuery 来执行工作的非介入式客户端验证）。

以下页面演示如何将客户端验证功能添加到前面所示的示例。

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

并非所有客户端上运行的验证检查。 具体而言，数据类型验证 （整数、 日期和等等） 不在客户端上运行。 在客户端和服务器上工作的以下检查：

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

在此示例中，是有效的日期的测试无法运行在客户端代码。 但是，测试将在服务器代码中执行。

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>格式设置的验证错误

你可以控制定义 CSS 类具有以下保留的名称显示验证错误：

- `field-validation-error`。 定义的输出`Html.ValidationMessage`方法时它显示一条错误。
- `field-validation-valid`。 定义的输出`Html.ValidationMessage`方法时没有错误。
- `input-validation-error`。 定义如何`<input>`元素呈现时出错。 (例如，你可以使用此类设置的背景色&lt;输入&gt;元素为不同的颜色，如果其值无效。)（在 ASP.NET 网页 2) 的客户端验证期间仅使用此 CSS 类。
- `input-validation-valid`。 定义的外观`<input>`元素时没有错误。
- `validation-summary-errors`。 定义的输出`Html.ValidationSummary`它显示的错误列表的方法。
- `validation-summary-valid`。 定义的输出`Html.ValidationSummary`方法时没有错误。

以下`<style>`块显示错误条件规则。

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

如果你在从与本文前面的示例页包含此样式块，错误显示的外观将类似于下图：

![使用 CSS 样式类的验证错误](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> 如果你不在 ASP.NET 网页 2 中使用客户端验证，CSS 类为`<input>`元素 (`input-validation-error`和`input-validation-valid`不产生任何影响。


### <a name="static-and-dynamic-error-display"></a>静态和动态错误显示

CSS 规则成对出现，如`validation-summary-errors`和`validation-summary-valid`。 这些对允许你定义这两个条件的规则： 一个错误条件和"正常"（非错误） 条件。 请务必了解，始终呈现错误显示的标记，即使没有错误。 例如，如果页具有`Html.ValidationSummary`标记中的方法，即使第一次请求页时，页面源文件将包含以下标记：

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

换而言之，`Html.ValidationSummary`方法始终呈现`<div>`元素和列表，即使错误列表为空。 同样，`Html.ValidationMessage`方法始终呈现`<span>`作为单个字段错误，即使没有错误占位符的元素。

在某些情况下，程序显示一条错误消息可能会导致页后，可以重新排列并可能导致在页上进行来回移动的元素。 以结尾的 CSS 规则`-valid`使你能够定义可以帮助防止此问题的布局。 例如，你可以定义`field-validation-error`和`field-validation-valid`两者都具有相同固定大小。 这样一来，该字段的显示区域是静态的不会显示一条错误消息的情况下更改页面流。

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>验证不是直接来自用户的数据

有时，你必须以验证不会直接从 HTML 窗体的信息。 一个典型示例是的单页其中传递值，则在查询字符串内，如以下示例所示：

`http://server/myapp/EditClassInformation?classid=1022`

在这种情况下，你想要确保传递到页的值 (这里，值的 1022年`classid`) 无效。 不能直接使用`Validation`帮助程序要执行此验证。 但是，你可以使用验证系统，如显示验证错误消息的功能的其他功能。

> [!NOTE] 
> 
> **重要**始终验证您获得的值*任何*源，包括窗体字段值、 查询字符串值和 cookie 值。 很容易让人有机会 （可能是出于恶意目的） 更改这些值。 因此，你必须为了保护你的应用程序中检查这些值。


下面的示例演示如何可能验证查询字符串中传递一个值。 代码测试的值不为空，以及它是一个整数。

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

请注意执行测试，如果请求不提交窗体 (`if(!IsPost)`)。 此测试将通过请求页时，第一次，但不是请求时提交窗体。

若要显示此错误，你可以将错误添加到验证错误的列表通过调用`Validation.AddFormError("message")`。 如果该页面包含调用`Html.ValidationSummary`方法，该错误会在其中显示一样用户输入验证错误。

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>其他资源

[使用 ASP.NET Web 页站点中的 HTML 窗体](https://go.microsoft.com/fwlink/?LinkID=202892)
