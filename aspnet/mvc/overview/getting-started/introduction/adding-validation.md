---
uid: mvc/overview/getting-started/introduction/adding-validation
title: "添加验证 |Microsoft 文档"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: 8d768727772738264d088315e605cca72db8de0a
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="adding-validation"></a>添加验证
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

在本部分中，你将添加到的验证逻辑`Movie`模型，且你将确保验证规则会实施任何时候用户试图创建或编辑使用应用程序的影片。

## <a name="keeping-things-dry"></a>保持操作干

ASP.NET MVC 的核心设计原则之一是[干](http://en.wikipedia.org/wiki/Don't_repeat_yourself)(&quot;不重复自己&quot;)。 ASP.NET MVC 鼓励您一次，指定功能或行为，然后使用它 everywhere 反映在应用程序。 这可减少所需编写的代码的数量，并使你确实要编写更少错误容易且更易于维护的代码。

提供 ASP.NET MVC 和 Entity Framework Code First 的验证支持是在操作中原则出色示例。 在一个位置 （在模型类中） 以声明方式指定验证规则和强制执行规则无处不在应用程序中。

让我们看一下影片应用程序中，你就可以充分利用此验证支持。

## <a name="adding-validation-rules-to-the-movie-model"></a>将验证规则添加到影片模型

将通过添加到某些验证逻辑开始`Movie`类。

打开 Movie.cs 文件。 请注意[ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空间不包含`System.Web`。 DataAnnotations 提供可以以声明方式应用于任何类或属性的验证属性一内置组。 (它还包含这样的格式设置属性[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) ，帮助进行格式设置，并且不提供任何验证。)

现在更新`Movie`类以利用内置[ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)， [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)，[正则表达式](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)，和[`Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)验证特性。 替换`Movie`替换为以下类：

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

[ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性设置为字符串，最大长度和它在数据库上设置此限制，因此将更改数据库架构。 右键单击**电影**表中**服务器资源管理器**单击**打开表定义**:

![](adding-validation/_static/image1.png)

在上图中，你可以看到所有字符串字段设置为[NVARCHAR (MAX)](https://technet.microsoft.com/library/ms186939.aspx)。 我们将使用迁移来更新架构。 生成解决方案，，然后打开**程序包管理器控制台**窗口并输入以下命令：

[!code-console[Main](adding-validation/samples/sample2.cmd)]

此命令完成时，Visual Studio 将打开定义新的类文件`DbMIgration`派生类具有指定的名称 (`DataAnnotations`)，然后在`Up`可以看到更新的架构约束的代码的方法：

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

`Genre`字段是将不再可以为 null （即，你必须输入一个值）。 `Rating`字段具有的最大长度为 5 和`Title`60 的最大长度。 3 上的最小长度`Title`和上的范围`Price`未创建架构更改。

检查电影架构：

![](adding-validation/_static/image2.png)

显示新的长度限制条件的字符串字段和`Genre`将不会选中为可以为 null。

验证特性指定要对应用这些特性的模型属性强制执行的行为。 `Required` 和 `MinimumLength` 特性表示属性必须有值；但用户可输入空格来满足此验证。 [正则表达式](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)属性用于限制字符可以是输入。 在上述代码中，`Genre` 和 `Rating` 仅可使用字母（禁用空格、数字和特殊字符）。 [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)属性限制到指定的范围中的值。 `StringLength` 特性使你能够设置字符串属性的最大长度，以及可选的最小长度。 值类型 (如`decimal, int, float, DateTime`) 是本质上是必需的不需要`Required`属性。

代码首先将确保： 在一个模型类指定的验证规则之前应用程序将更改保存在数据库会强制实施。 例如，下面的代码将引发[DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx)异常时`SaveChanges`调用方法，因为几个需要`Movie`是丢失属性值：

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

上面的代码将引发以下异常：

*一个或多个实体的验证失败。请参阅 EntityValidationErrors 属性的更多详细信息。*

具有自动由.NET Framework 强制执行的验证规则有助于使你的应用程序更可靠。 同时它能确保你无法忘记验证某些内容，并防止你无意中将错误数据导入数据库。

## <a name="validation-error-ui-in-aspnet-mvc"></a>验证错误在 ASP.NET MVC UI

运行应用程序并导航到*/Movies* URL。

单击**新建**链接添加新的影片。 使用无效值填写表单。 当 jQuery 客户端验证检测到错误时，会显示一条错误消息。

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> 若要为使用逗号的非英语区域设置支持 jQuery 验证 （"，"） 对于十进制点，必须包括 NuGet 全球化如在本教程中前面所述。


请注意如何窗体具有自动用红色边框颜色来突出显示的文本框中，它包含无效的数据，并且已发出旁边每个相应的验证错误消息。 客户端（使用 JavaScript 和 jQuery）和服务器端（若用户禁用 JavaScript）都必定会遇到这些错误。

实际的好处是，你不需要更改单个行中的代码`MoviesController`类或在*Create.cshtml*以启用此验证 UI 的视图。 在本教程前面创建的控制器和视图会自动选取验证规则，这些规则是通过在 `Movie` 模型类的属性上使用验证特性所指定的。 使用 `Edit` 操作方法测试验证后，即已应用相同的验证。

存在客户端验证错误时，不会将表单数据发送到服务器。 你可以通过将中断点放在 HTTP Post 方法中，在通过使用对此进行验证[fiddler 工具](http://fiddler2.com/fiddler2/)，或 IE [F12 开发人员工具](https://msdn.microsoft.com/ie/aa740478)。

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>如何验证发生在创建查看和创建操作方法

你可能想知道在不对控制器或视图中的代码进行任何更新的情况下，验证 UI 是如何生成的。 下一步的列表显示什么`Create`中的方法`MovieController`类如下所示。 这些更改将从你创建这些此前在本教程不变。

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

第一个 (HTTP GET) `Create` 操作方法显示初始的“创建”表单。 第二个 (`[HttpPost]`) 版本处理表单发布。 第二个 `Create` 方法（`HttpPost` 版本）调用 `ModelState.IsValid` 以检查电影是否有任何验证错误。 调用此方法将评估已应用于对象的任何验证特性。 如果对象有验证错误，则 `Create` 方法会重新显示此表单。 如果没有错误，此方法则将新电影保存在数据库中。 在我们电影的示例，**窗体不发布到服务器时有验证错误在客户端; 上检测到第二个** `Create`**方法在于： 绝不调用**。 如果在浏览器中禁用 JavaScript，禁用客户端验证和 HTTP POST`Create`方法调用`ModelState.IsValid`若要检查的影片是否有任何验证错误。

可以在 `HttpPost Create` 方法中设置断点，并验证方法从未被调用，客户端验证在检测到存在验证错误时不会提交表单数据。 如果在浏览器中禁用 JavaScript，然后提交错误的表单，将触发断点。 在没有 JavaScript 的情况下仍然可以进行完整的验证。 下图显示了如何禁用 Internet Explorer 中的 JavaScript。

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

以下图片显示如何在 FireFox 浏览器中禁用 JavaScript。

![](adding-validation/_static/image7.png)

以下图片显示如何在 Chrome 浏览器中禁用 JavaScript。

![](adding-validation/_static/image8.png)

下面是*Create.cshtml*了教程前面的基架的视图模板。 以上所示的操作方法使用它来显示初始表单，并在发生错误时重新显示此表单。

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

请注意代码如何使用`Html.EditorFor`帮助器输出`<input>`每个元素`Movie`属性。 此帮助器的旁边是调用`Html.ValidationMessageFor`帮助器方法。 这两个帮助器方法使用由控制器传递给视图的模型对象 (在这种情况下，`Movie`对象)。 它们在自动显示为相应的模型和显示错误消息上指定的验证特性。

此方法真正好的一点是：无论是控制器还是 `Create` 视图模板都不知道强制实施的实际验证规则或显示的特定错误消息。 仅可在 `Movie` 类中指定验证规则和错误字符串。 这些相同的验证规则自动应用于 `Edit` 视图和可能创建用于编辑模型的任何其他视图模板。

如果你想要以后更改验证逻辑，则可以在一个位置通过做到验证特性添加到模型 (在此示例中，`movie`类)。 无需担心对应用程序的不同部分所强制执行规则的方式不一致 - 所有验证逻辑都将定义在一个位置并用于整个应用程序。 这使代码非常简洁，并且更易于维护和改进。 以及它意味着，你将会完全遵循*干*原则。

## <a name="using-datatype-attributes"></a>使用 DataType 特性

打开 Movie.cs 文件并检查 `Movie` 类。 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空间提供内置集以及验证特性的格式设置属性。 我们已应用[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)枚举值到发布日期和价格字段。 下面的代码演示`ReleaseDate`和`Price`使用相应的属性[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性。

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性仅提供视图引擎对数据进行格式化的提示 (如提供属性和`<a>`url 的和`<a href="mailto:EmailAddress.com">`电子邮件。 你可以使用[正则表达式](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)要验证的数据格式属性。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性用于指定比数据库内部类型更具体的数据类型，它们是***不***验证特性。 在此示例中，我们只想跟踪日期，而不是日期和时间。 [DataType 枚举](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)提供多种数据类型，如*日期、 时间、 PhoneNumber、 货币、 电子邮件地址*和的详细信息。 应用程序还可通过 `DataType` 特性自动提供类型特定的功能。 例如，`mailto:`链接可以为创建[DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)，和日期选择器可提供用于[DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)中支持的浏览器[HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性发出 HTML 5[数据-](http://ejohn.org/blog/html-5-data-attributes/) (发音为*数据 dash*) HTML 5 浏览器可以理解的属性。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性不提供任何验证。

`DataType.Date` 不指定显示日期的格式。 默认情况下，数据字段显示根据基于服务器的默认格式[CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)。

`DisplayFormat` 特性用于显式指定日期格式：


[!code-csharp[Main](adding-validation/samples/sample8.cs)]


`ApplyFormatInEditMode`设置指定，指定的格式设置也应该将应用时的值显示在文本框中以进行编辑。 (你可能不想的某些字段 — 例如，货币值，你可能不希望在文本框中的货币符号以进行编辑。)

你可以使用[DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性通过本身，但它通常是使用一个好办法[数据类型](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)还属性。 `DataType`属性传达*语义*的数据作为，而不是如何呈现在屏幕上，然后提供就不会使用了以下好处`DisplayFormat`:

- 浏览器可以启用 HTML5 功能 （例如以显示一个日历控件、 区域设置相对应的货币符号、 电子邮件链接，等等）。
- 默认情况下，浏览器将呈现数据使用基于的正确格式你[区域设置](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx)。
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性可以启用 MVC 能够选择要呈现的数据的右侧字段模板 ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)本身如果由使用字符串模板)。 有关详细信息，请参阅 Brad Wilson[ASP.NET MVC 2 模板](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)。 （尽管为 MVC 2 编写的这篇文章仍适用于 ASP.NET MVC 的当前版本。）

如果你使用`DataType`属性与日期字段中，你必须指定`DisplayFormat`还以确保字段正确呈现在 Chrome 浏览器中的属性。 有关详细信息，请参阅[此 StackOverflow 线程](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie)。

> [!NOTE]
> jQuery 验证并不适用于[范围](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)属性和[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)。 例如，以下代码将始终显示客户端验证错误，即便日期在指定的范围内：
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> 你将需要禁用 jQuery 日期验证用于[范围](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)特性与[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)。 它通常不是一个好办法编译在模型中，因此使用硬日期[范围](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)属性和[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)不建议这样做。


以下代码显示组合在一行上的特性：

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=6,10)]

在本系列的下一部分中，我们将回顾应用程序，并对自动生成的 `Details` 和 `Delete` 方法进行一些改进。

>[!div class="step-by-step"]
[上一页](adding-a-new-field.md)
[下一页](examining-the-details-and-delete-methods.md)
