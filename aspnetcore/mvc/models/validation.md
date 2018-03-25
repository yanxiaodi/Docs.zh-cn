---
title: "ASP.NET Core MVC 中的模型验证"
author: rachelappel
description: "了解 ASP.NET Core MVC 中的模型验证。"
manager: wpickett
ms.author: riande
ms.date: 12/18/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/validation
ms.openlocfilehash: dfb24a4c72b15737295b7aea406be24160fc6674
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-model-validation-in-aspnet-core-mvc"></a>ASP.NET Core MVC 模型验证简介

作者：[Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-validation"></a>模型验证简介

在将数据存储到数据库之前，应用必须先验证数据。 必须检查数据是否存在潜在的安全威胁，确保数据已设置适当的类型和大小格式，并且必须符合相关规则。 实施验证的过程可能有些单调乏味，但却必不可少。 在 MVC 中，验证发生在客户端和服务器上。

幸运的是，.NET 已将验证抽象化为验证属性。 这些属性包含验证代码，从而减少了所需编写的代码量。

[查看或下载 GitHub 中的示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample)。

## <a name="validation-attributes"></a>验证属性

验证属性用于配置模型验证，因此，在概念上类似于数据库表中字段上的验证。 它包括诸如分配数据类型或必填字段之类的约束。 其他类型的验证包括将向数据应用模式以强制实施业务规则，比如信用卡、电话号码或电子邮件地址。 验证属性更易使用，并使这些要求的实施变得更简单。

下面是一个应用的已批注 `Movie` 模型，该应用用于存储电影和电视节目的相关信息。 大多数属性都是必需属性，多个字符串属性具有长度要求。 此外，还有一个针对·`Price` 属性设置的从 0 到 $999.99 的数值范围限制，以及一个自定义验证特性。

[!code-csharp[Main](validation/sample/Movie.cs?range=6-29)]

通过读取整个模型即可显示有关此应用的数据的规则，从而使代码维护变得更轻松。 下面是几个常用的内置验证属性：

* `[CreditCard]`：验证属性是否具有信用卡格式。

* `[Compare]`：验证某个模型中的两个属性是否匹配。

* `[EmailAddress]`：验证属性是否具有电子邮件格式。

* `[Phone]`：验证属性是否具有电话格式。

* `[Range]`：验证属性值是否落在给定范围内。

* `[RegularExpression]`：验证数据是否与指定的正则表达式匹配。

* `[Required]`：将属性设置为必需属性。

* `[StringLength]`：验证字符串属性是否最多具有给定的最大长度。

* `[Url]`：验证属性是否具有 URL 格式。

MVC 支持从 `ValidationAttribute` 派生的所有用于验证的属性。 在 [System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations) 命名空间中可找到许多有用的验证属性。

在某些情况下，内置属性可能无法提供所需的功能。 这时，就可以通过从 `ValidationAttribute` 派生或将模型更改为实现 `IValidatableObject`，来创建自定义验证属性。

## <a name="notes-on-the-use-of-the-required-attribute"></a>必需属性的使用说明

从本质上来说，需要不可以为 null 的[值类型](/dotnet/csharp/language-reference/keywords/value-types)（如 `decimal`、`int`、`float` 和 `DateTime`），但不需要 `Required` 特性。 应用不会对标记为 `Required` 的不可为 null 的类型执行任何服务器端验证检查。

对于不可为 null 的类型，MVC 模型绑定（与验证和验证属性无关）会拒绝包含缺失值或空白的表单域提交。 如果目标属性上缺少 `BindRequired` 特性，模型绑定会忽略不可为 null 的类型的缺失数据，导致传入表单数据中缺少表单域。

[BindRequired 属性](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute)（另请参阅[使用属性自定义模型绑定行为](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)）可用于确保表单数据的完整性。 当应用于某个属性时，模型绑定系统要求该属性具有值。 当应用于某个类型时，模型绑定系统要求该类型的所有属性都具有值。

使用 [Nullable\<T> 类型](/dotnet/csharp/programming-guide/nullable-types/)（例如，`decimal?` 或 `System.Nullable<decimal>`）并将其标记为 `Required` 时，将执行服务器端验证检查，就像该属性是标准的可以为 null 的类型（例如，`string`）一样。

客户端验证要求与标记为 `Required` 的模型属性对应的表单域以及未标记为 `Required` 的不可为 null 的类型属性具有值。 `Required` 可用于控制客户端验证错误消息。

## <a name="model-state"></a>模型状态

模型状态表示已提交的 HTML 表单值中的验证错误。

MVC 将继续验证字段，直至达到错误数上限（默认为 200 个）。 通过向 *Startup.cs* 文件中的 `ConfigureServices` 方法插入以下代码，可配置该数字：

[!code-csharp[Main](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a>处理模型状态错误

在调用每个控制器操作之前都会执行模型验证，将由操作方法负责检查 `ModelState.IsValid` 并作出正确反应。 在许多情况下，正确的反应是返回错误响应，理想状况下会详细说明模型验证失败的原因。

某些应用会选择遵循标准约定来处理模型验证错误，在这种情况下，可以在筛选器中实现此类策略。 应测试操作在有效模型状态和无效模型状态下的行为方式。

## <a name="manual-validation"></a>手动验证

完成模型绑定和验证后，可能需要重复其中的某些步骤。 例如，用户可能在应输入整数的字段中输入了文本，或者你可能需要计算模型的某个属性的值。

你可能需要手动运行验证。 为此，请调用 `TryValidateModel` 方法，如下所示：

[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>自定义验证

验证属性适用于大多数验证需求。 但是，某些验证规则特定于你的业务。 你的规则可能不是常见的数据验证技术，比如确保字段是必填字段或符合一系列值。 在这些情况下，自定义验证属性是一种不错的解决方案。 在 MVC 中创建你自己的自定义验证属性很简单。 只需从 `ValidationAttribute` 继承并重写 `IsValid` 方法。 `IsValid` 方法采用两个参数，第一个是名为 *value* 的对象，第二个是名为 *validationContext* 的 `ValidationContext` 对象。 *Value* 引用自定义验证程序要验证的字段中的实际值。

在下面的示例中，一项业务规则规定，用户不能将 1960 年以后发行的电影的流派设置为 *Classic*。 `[ClassicMovie]` 属性会先检查流派，如果是经典流派，则查看发行日期是否晚于 1960 年。 如果晚于 1960 年，则验证失败。 此属性采用一个表示年份的整数参数，可用于验证数据。 可以在该属性的构造函数中捕获该参数的值，如下所示：

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-29)]

上面的 `movie` 变量表示一个 `Movie` 对象，其中包含要验证的表单提交中的数据。 在此例中，验证代码会根据规则检查 `ClassicMovieAttribute` 类的 `IsValid` 方法中的日期和流派。 如果验证成功，`IsValid` 会返回 `ValidationResult.Success` 代码；如果验证失败，则返回包含错误消息的 `ValidationResult`。 当用户修改 `Genre` 字段并提交表单时，`ClassicMovieAttribute` 的 `IsValid` 方法将验证该电影是否为经典电影。 将 `ClassicMovieAttribute` 像所有内置特性一样应用于属性（如 `ReleaseDate`）以确保执行验证，如前面的代码示例所示。 由于此示例仅适用于 `Movie` 类型，因此建议使用 `IValidatableObject`，如下一段中所示。

也可以通过实现 `IValidatableObject` 接口上的 `Validate` 方法，将这段代码直接放入模型中。 如果自定义验证特性可用于验证各个属性，则可使用 `IValidatableObject` 来实现类级别的验证，如下所示。

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a>客户端验证

客户端验证极大地方便了用户。 它节省了时间，让用户不必浪费时间等待服务器往返。 从商业角度而言，即使每次只有几分之一秒，但如果每天有几百次，也会耗费大量的时间和成本，带来很多不必要的烦恼。 简单直接的验证能够提高用户的工作效率和投入产出比。

你必须有一个包含适当的 JavaScript 脚本引用的视图，才能让客户端验证正常工作，如下所示。

[!code-cshtml[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

[jQuery 非介入式验证](https://github.com/aspnet/jquery-validation-unobtrusive)脚本是一个基于热门 [jQuery Validate](https://jqueryvalidation.org/) 插件的自定义 Microsoft 前端库。 如果没有 jQuery 非介入式验证，则必须在两个位置编码相同的验证逻辑：一次是在模型属性上的服务器端验证特性中，一次是在客户端脚本中（jQuery Validate 的 [`validate()`](https://jqueryvalidation.org/validate/) 方法示例展示了这种情况可能的复杂程度）。 MVC 的[标记帮助程序](xref:mvc/views/tag-helpers/intro)和 [HTML 帮助程序](xref:mvc/views/overview)则能够使用模型属性中的验证特性和类型元数据，呈现需要验证的表单元素中的 HTML 5 [data- 特性](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes)。 MVC 为内置属性和自定义属性生成 `data-` 属性。 然后，jQuery 非介入式验证分析这些 `data-` 属性并将逻辑传递给 jQuery Validate，从而将服务器端验证逻辑有效地“复制”到客户端。 可以使用相关标记帮助程序在客户端上显示验证错误，如下所示：

[!code-cshtml[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

上面的标记帮助程序将呈现以下 HTML。 请注意，HTML 输出中的 `data-` 特性与 `ReleaseDate` 属性的验证特性相对应。 下面的 `data-val-required` 属性包含在用户未填写发行日期字段时将显示的错误消息。 jQuery 非介入式验证将此值传递给 jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) 方法，该方法随后在随附的 **\<span>** 元素中显示该消息。

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="" />
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

客户端验证将阻止提交，直到表单变为有效为止。 “提交”按钮运行 JavaScript：要么提交表单要么显示错误消息。

MVC 基于属性的 .NET 数据类型确定类型特性值（有可能使用 `[DataType]` 特性进行重写）。 `[DataType]` 基本特性不执行真正的服务器端验证。 浏览器选择自己的错误消息，并根据需要显示这些错误，不过，jQuery 非介入式验证包可以重写消息，并使它们与其他消息的显示保持一致。 当用户应用 `[DataType]` 子类（比如 `[EmailAddress]`）时，最常发生这种情况。

### <a name="add-validation-to-dynamic-forms"></a>向动态表单添加验证

由于 jQuery 非介入式验证会在第一次加载页面时将验证逻辑和参数传递到 jQuery Validate，因此，动态生成的表单不会自动展示验证。 你必须指示 jQuery 非介入式验证在创建动态表单后立即对其进行分析。 例如，下面的代码展示如何对通过 AJAX 添加的表单设置客户端验证。

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

`$.validator.unobtrusive.parse()` 方法采用 jQuery 选择器作为它的一个参数。 此方法指示 jQuery 非介入式验证分析该选择器内表单的 `data-` 属性。 这些属性的值随后传递到 jQuery Validate 插件中，以便表单展示所需的客户端验证规则。

### <a name="add-validation-to-dynamic-controls"></a>向动态控件添加验证

也可以在动态生成各个控件（比如 `<input/>` 和 `<select/>`）时，更新表单上的验证规则。 不能将用于这些元素的选择器直接传递到 `parse()` 方法，因为周围表单已进行分析并且不会更新。 应当先删除现有的验证数据，然后重新分析整个表单，如下所示：

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        form.removeData("validator")    // Added by the raw jQuery Validate
            .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="iclientmodelvalidator"></a>IClientModelValidator

你可以为自定义属性创建客户端逻辑，[非介入式验证](http://jqueryvalidation.org/documentation/)将在验证过程中，在客户端上自动为你执行此逻辑。 第一步是通过实现 `IClientModelValidator` 接口来控制要添加哪些 data- 属性，如下所示：

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

实现此接口的属性可以将 HTML 属性添加到生成的字段。 检查 `ReleaseDate` 元素的输出时，将显示与上一示例类似的 HTML，唯一不同的是，此示例包含一个已在 `IClientModelValidator` 的 `AddValidation` 方法中定义的 `data-val-classicmovie` 属性。

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

非介入式验证使用 `data-` 属性中的数据来显示错误消息。 不过，除非将规则或消息添加到 jQuery 的 `validator` 对象，否则 jQuery 并不知道它们的存在。 以下示例对此进行了说明，该示例向 jQuery `validator` 对象添加一个包含自定义客户端验证代码的名为 `classicmovie` 的方法。

[!code-javascript[Main](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

现在，jQuery 包含用于执行自定义 JavaScript 验证的信息，以及该验证代码返回 false 时将要显示的错误消息。

## <a name="remote-validation"></a>远程验证

远程验证是一项非常不错的功能，可在需要根据服务器上的数据验证客户端上的数据时使用。 例如，应用可能需要验证某个电子邮件或用户名是否已被使用，并且它必须为此查询大量数据。 为验证一个或几个字段而下载大量数据会占用过多资源。 它还有可能暴露敏感信息。 一种替代方法是发出往返请求来验证字段。

可以分两步实现远程验证。 首先，必须使用 `[Remote]` 属性为模型添加批注。 `[Remote]` 属性采用多个重载，可用于将客户端 JavaScript 定向到要调用的相应代码。 下面的示例指向 `Users` 控制器的 `VerifyEmail` 操作方法。

[!code-csharp[Main](validation/sample/User.cs?range=7-8)]

第二步是按照 `[Remote]` 属性中的定义，将验证代码放入相应的操作方法。 根据 jQuery Validate [`remote()`](https://jqueryvalidation.org/remote-method/) 方法文档：

> 服务器端响应必须是使用默认错误消息的 JSON 字符串，对于有效的元素必须为 `"true"`，对于无效元素则可以为 `"false"`、`undefined` 或 `null`。 如果服务器端响应是一个字符串，例如， `"That name is already taken, try peter123 instead"`，则此字符串将显示为自定义错误消息，以取代默认错误消息。

`VerifyEmail()` 方法的定义遵循这些规则，如下所示。 如果电子邮件已被占用，它会返回验证错误消息；如果电子邮件可用，则返回 `true`，并将结果包装在 `JsonResult` 对象中。 然后，客户端可以使用返回的值，继续进行下一步操作或根据需要显示错误。

[!code-csharp[Main](validation/sample/UsersController.cs?range=19-28)]

现在，当用户输入电子邮件时，视图中的 JavaScript 会发出远程调用，以了解该电子邮件是否已被占用，如果是，则显示错误消息。 如果不是，用户就可以像往常一样提交表单。

`[Remote]` 特性的 `AdditionalFields` 属性可用于根据服务器上的数据验证字段组合。 例如，如果上面的 `User` 模型具有两个附加属性，名为 `FirstName` 和 `LastName`，你可能想要验证该名称对尚未被现有用户占用。 按以下代码所示定义新属性：

[!code-csharp[Main](validation/sample/User.cs?range=10-13)]

`AdditionalFields` 可能已显式设置为字符串 `"FirstName"` 和 `"LastName"`，但使用 [`nameof`](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) 这样的操作符可简化稍后的重构过程。 然后，用于执行验证的操作方法必须采用两个参数，一个用于 `FirstName` 的值，一个用于 `LastName` 的值。

[!code-csharp[Main](validation/sample/UsersController.cs?range=30-39)]

现在，当用户输入名和姓时，JavaScript 会:

* 发出远程调用，以了解该名称对是否已被占用。
* 如果被占用，则显示一条错误消息。 
* 如果未被占用，则用户可以提交表单。

如果需要使用 `[Remote]` 特性验证两个或更多附加字段，可将其以逗号分隔的列表形式列出。 例如，若要向模型中添加 `MiddleName` 属性，可按以下代码所示设置 `[Remote]` 特性：

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields` 与所有属性参数一样，必须是常量表达式。 因此，不能使用[内插字符串](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings)或调用 [`string.Join()`](https://msdn.microsoft.com/library/system.string.join(v=vs.110).aspx) 来初始化 `AdditionalFields`。 对于添加到 `[Remote]` 特性的每个附加字段，都必须向相应的控制器操作方法另外添加一个参数。
