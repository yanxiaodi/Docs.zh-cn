---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: "提供 CRUD （创建、 读取、 更新、 删除） 数据窗体条目支持 |Microsoft 文档"
author: microsoft
description: "第 5 步演示如何通过启用用于编辑、 创建和删除晚餐也为它的支持，让我们进一步 DinnersController 类。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: 5a314a1761527d8a2273166a743e3deac012a557
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>提供 CRUD （创建、 读取、 更新、 删除） 数据窗体条目支持
====================
通过[Microsoft](https://github.com/microsoft)

[下载 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 这是一个免费的步骤 5 ["NerdDinner"应用程序教程](introducing-the-nerddinner-tutorial.md)，查找步程取如何生成一个较小，但完成，使用 ASP.NET MVC 1 web 应用程序。
> 
> 第 5 步演示如何通过启用用于编辑、 创建和删除晚餐也为它的支持，让我们进一步 DinnersController 类。
> 
> 如果你使用的 ASP.NET MVC 3，我们建议你遵循[获取启动使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC 音乐商店](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教程。


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner 步骤 5： 创建、 更新、 删除窗体方案

我们已引入控制器和视图，并介绍如何使用它们来在站点上实现晚餐的详细信息列表/体验。 我们的下一步将是采取进一步的我们 DinnersController 类和启用用于编辑、 创建和删除晚餐也为它的支持。

### <a name="urls-handled-by-dinnerscontroller"></a>Url 由 DinnersController 处理

我们之前添加到实现支持两个 Url 的 DinnersController 的操作方法： */Dinners*和*/Dinners/详细信息 / [id]*。

| **URL** | **谓词** | **目的** |
| --- | --- | --- |
| */Dinners/* | GET | 显示即将到来的晚餐 HTML 列表。 |
| */Dinners/详细信息 / [id]* | GET | 显示有关特定 dinner 详细信息。 |

我们现在将操作方法来实现三个其他 Url: */Dinners/编辑 / [id]、 / 晚餐/创建、*和*/Dinners/删除 / [id]*。 这些 Url 将启用对编辑现有晚餐，创建新晚餐和删除晚餐支持。

我们将支持使用这些新的 Url 的 HTTP GET 和 HTTP POST 谓词交互。 HTTP GET 请求到这些 Url 将显示数据 （使用在"编辑"的情况下的 Dinner 数据填充窗体，在"创建"的情况下的空白窗体和删除确认屏幕中，在"删除"的情况下） 的初始 HTML 视图。 对这些 Url 的 HTTP POST 请求将保存/更新/删除 Dinner 数据中我们 DinnerRepository （和从那里到数据库）。

| **URL** | **谓词** | **目的** |
| --- | --- | --- |
| */Dinners/编辑 / [id]* | GET | 显示可编辑 HTML 窗体使用 Dinner 数据进行填充。 |
| 发布 | 将窗体更改保存到数据库特定 Dinner 为。 |
| */ 创建晚餐 /* | GET | 显示允许用户定义新晚餐空 HTML 窗体。 |
| 发布 | 创建新的 Dinner 并将其保存在数据库中。 |
| */Dinners/删除 / [id]* | GET | 显示删除确认屏幕。 |
| 发布 | 从数据库中删除指定的 dinner。 |

### <a name="edit-support"></a>编辑支持

让我们首先将实现"编辑"方案。

#### <a name="the-http-get-edit-action-method"></a>HTTP GET 编辑操作方法

我们将开始通过实现我们编辑操作方法的 HTTP"GET"行为。 此方法将调用时*/Dinners/编辑 / [id]*请求 URL。 我们实现将如下所示：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

上面的代码使用 DinnerRepository 检索 Dinner 对象。 它随后将呈现使用 Dinner 对象的视图模板。 因为我们尚未显式传递到的模板名称*View()*帮助器方法，它将使用基于约定的默认路径来解析视图模板： /Views/Dinners/Edit.aspx。

让我们现在创建此视图模板。 我们将编辑方法内右键单击并选择"添加视图"上下文菜单命令来执行此操作：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

在"添加视图"对话框中，我们将指示我们 Dinner 对象传递给其模型中，我们查看模板和选择自动基架"编辑"的模板：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

当我们单击"添加"按钮时，Visual Studio 将在"\Views\Dinners"目录中为我们添加新的"Edit.aspx"视图模板文件。 它还将打开代码编辑器 – 使用初始"编辑"基架实现类似下面填充内部的新"Edit.aspx"视图模板：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

让我们进行到默认"的编辑"基架生成，一些更改，并将编辑视图模板更新为具有下列内容 （这将删除几个我们不需要公开的属性）：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

当我们运行的应用程序和请求*"/ 晚餐/编辑/1"*我们将看到以下页面的 URL:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

由我们视图生成的 HTML 标记类似于下面。 这一点与标准 HTML –&lt;窗体&gt;执行到 HTTP POST 的元素*/Dinners/Edit/1* URL 时"保存"&lt;输入类型 ="提交"/&gt;推送按钮。 对 HTML&lt;输入类型 ="text"/&gt;元素已被为每个可编辑属性的输出：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() 和 Html.TextBox() Html 帮助器方法

我们"Edit.aspx"视图模板正在使用的一些"Html 帮助程序"方法： Html.ValidationSummary()、 Html.BeginForm()、 Html.TextBox() 和 Html.ValidationMessage()。 除了为我们的 HTML 标记生成，这些帮助器方法提供了内置的错误处理和验证支持。

##### <a name="htmlbeginform-helper-method"></a>Html.BeginForm() 帮助器方法

Html.BeginForm() 帮助程序方法是 HTML 输出内容是什么&lt;窗体&gt;我们标记中的元素。 我们 Edit.aspx 视图模板中你将注意到，我们要将应用的 C#"using"语句时使用此方法。 左大括号指示的开头&lt;窗体&gt;内容和右大括号是什么指示的末尾&lt;窗体窗体&gt;元素：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

或者，如果你发现的"using"语句接近自然如下方案，你可以使用 Html.BeginForm() 和 Html.EndForm() 组合 （后者执行相同的操作）：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

不带任何参数调用 Html.BeginForm() 将导致输出对当前请求的 URL 执行 HTTP POST 的窗体元素。 它是为何我们编辑视图生成*&lt;形成操作 ="/ 晚餐/编辑/1"方法 ="post"&gt;* 元素。 我们无法具有或者传递显式参数给 Html.BeginForm() 如果我们想要发布到另一个 URL。

##### <a name="htmltextbox-helper-method"></a>Html.TextBox() 帮助器方法

我们 Edit.aspx 视图使用 Html.TextBox() 帮助器方法来输出&lt;输入类型 ="text"/&gt;元素：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

上面的 Html.TextBox() 方法将采用的单个参数 – 正在使用指定的 id/名称属性&lt;输入类型 ="text"/&gt;到输出，以及要导入从文本框中的值的模型属性的元素。 例如，我们传递到编辑视图的 Dinner 对象有".NET Future"，"Title"属性值，因此我们 Html.TextBox("Title") 方法调用输出： *&lt;输入的 id ="Title"名称 ="Title"type ="text"value =".NET Future"/&gt;*.

或者，我们可以使用第一个 Html.TextBox() 参数来指定 id/名称的元素，然后显式将传递中要用作第二个参数的值：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

通常，我们将需要执行的输出值的自定义格式设置。 内置于.NET string.format （) 静态方法可用于这些方案。 我们 Edit.aspx 视图模板使用此设置的格式 EventDate 值 （这是类型为 DateTime 的），因此它不会显示时间 （秒）：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Html.TextBox() 的第三个参数 （可选） 可用来输出 HTML 的其他属性。 的以下代码段演示如何呈现其他大小 ="30"的属性和类 ="mycssclass"属性上&lt;输入类型 ="text"/&gt;元素。 请注意，我们如何转义的类属性使用名称"@" character because "类"是保留的关键字在 C# 中：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>实现 HTTP POST 编辑操作方法

我们现在有了我们的编辑操作方法中实现的 HTTP GET 版本。 当用户请求*/Dinners/Edit/1*它们接收 HTML 页如下所示的 URL:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

按"保存"按钮会导致窗体发送到*/Dinners/Edit/1* URL，并在提交 HTML&lt;输入&gt;窗体中使用 HTTP POST 谓词的值。 让我们现在实现我们编辑操作方法 – 它将处理保存 Dinner 的 HTTP POST 行为。

我们将首先通过将重载"的编辑"操作方法添加到对其具有"AcceptVerbs"属性，该值指示它将处理 HTTP POST 方案我们 DinnersController:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

当 [AcceptVerbs] 特性应用于重载的操作方法中时，ASP.NET MVC 将自动处理调度到适当的操作方法，具体取决于传入的 HTTP 谓词的请求。 HTTP POST 请求到*/Dinners/编辑 / [id]* Url 将转到上述的编辑方法，对所有其他 HTTP 谓词请求时*/Dinners/编辑 / [id]*Url 将转到第一个的编辑方法我们实现 （其未不具有 [AcceptVerbs] 属性）。

| **通过 HTTP 谓词为什么区分端主题内容:？** |
| --- |
| 您可能会问 – 为何我们使用单一 URL 和区分的 HTTP 谓词通过其行为？ 为何不让两个单独的 Url 来处理加载和保存编辑的更改？ 例如： /Dinners/编辑 / [id] 以显示初始窗体和 /Dinners/保存 / [id] 来处理窗体发布请求，以将其保存？ 与发布的两个单独 Url 的缺点是在其中我们发布到 /Dinners/Save/2，，然后需要重新 HTML 窗体显示由于输入错误的情况下，最终的最终用户将得到 （因为这是在其浏览器的地址栏中具有晚餐/保存/2 URLURL 发布到窗体)。 如果最终用户创建一个书签，本页 redisplayed 到其浏览器收藏夹列表中，或复制/粘贴 URL 和电子邮件将其发送给友元，它们将结束保存将不起作用将来 （因为该 URL 取决于发送值） 的 URL。 通过公开单一 URL (例如： /Dinners/Edit/[id]) 并且区分处理它的 HTTP 谓词，则可以为最终用户若要编辑页面加入书签和/或将 URL 发送给他人安全。 |

#### <a name="retrieving-form-post-values"></a>检索窗体发送值

有多种方式我们就可以访问发布窗体中"的编辑"我们 HTTP POST 方法的参数。 一个简单方法是只使用控制器基类上请求属性来访问窗体集合和直接检索已发布的值：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

上面的方法是稍有详细信息; 不过，尤其是一旦我们添加错误处理逻辑。

一个更好的方法的这种情况下是利用内置*UpdateModel()*控制器基类上的帮助器方法。 它支持更新我们将它使用传入的窗体参数传递的对象的属性。 它使用反射来确定对象的属性名称然后自动将转换并向其客户端提交的输入值为基础的分配值。

我们无法使用 UpdateModel() 方法来简化使用此代码我们 HTTP POST 编辑操作：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

我们现在可以访问*/Dinners/Edit/1* URL，以及更改我们 Dinner 标题：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

当我们单击"保存"按钮时，我们使用以下方法将执行窗体发布到我们的编辑操作，并更新后的值将保留在数据库中。 我们将然后重定向到详细信息 URL 为 Dinner （这将显示新保存的值）：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>处理编辑错误

我们当前的 HTTP POST 实现能够正常微调 – 除非有错误。

当用户进行编辑窗体的一个错误时，我们需要确保窗体将重新显示使用的将引导他们修复此错误的详细的错误消息。 这包括最终用户，并且其中文章不正确的输入的情况下 (例如： 的格式不正确的日期字符串)，如以及情况的输入的格式是否有效，但没有业务规则冲突。 如果出现错误窗体应保留输入的数据最初输入，以便他们不需要手动重填其更改的用户。 窗体成功完成之前，应根据需要多次重复此过程。

ASP.NET MVC 包括一些很好的内置功能，用于简化错误处理和窗体重新显示。 若要查看操作中的这些功能，让我们更新我们的编辑操作方法替换为以下代码：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

上面的代码类似于我们以前的实现 – 只是我们现在我们工作周围包装 try/catch 错误处理块。 当调用 UpdateModel()，或者当我们尝试和保存 DinnerRepository （这将引发的异常，我们正在尝试进行保存的 Dinner 对象是否无效，由于我们的模型中的规则冲突），如果发生异常时将我们 catch 错误处理块执行。 在其中我们循环 Dinner 对象中存在任何规则冲突，并将它们添加到 ModelState 对象 （其中，我们将讨论很快）。 然后，我们重新显示视图。

若要查看此工作让我们重新运行该应用程序，编辑 Dinner，并将其更改为具有空标题，"BOGUS"EventDate 和美国国家/地区值中使用英国电话号码。 当我们按"保存"按钮时我们的 HTTP POST 编辑方法中将不能保存 Dinner （因为有错误） 并将重新显示窗体：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

我们的应用程序具有大错误体验。 具有无效的输入的文本元素将突出显示为红色，并验证错误消息显示给最终用户有关它们的信息。 窗体还保留用户最初输入 – 的输入的数据，以便它们无需重新填充任何内容。

如何操作，您可能会问，这发生？ 如何未标题、 EventDate，和 ContactPhone 文本框中突出显示本身以红色并知道要输出最初输入的用户值？ 和如何未错误消息显示在顶部列表中？ 好消息是这未发生魔法-而不是它是因为我们使用的一些内置的 ASP.NET MVC 功能用于简化输入的验证和错误处理方案。

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>了解 ModelState 和验证的 HTML 帮助器方法

控制器类具有一个"ModelState"属性集合，提供了方法，以指示错误存在与传递到视图的模型对象。 ModelState 集合中的错误项标识有问题的模型属性的名称 (例如:"标题"、"EventDate"或"ContactPhone")，并允许指定的用户友好错误消息 (例如:"标题是必需的")。

*UpdateModel()*时它在尝试将窗体值分配给模型对象的属性时遇到错误后，帮助器方法将自动填充 ModelState 集合。 例如，我们 Dinner 对象 EventDate 属性是类型为 DateTime。 无法在上述方案中向其分配的字符串值"BOGUS"UpdateModel() 方法时，UpdateModel() 方法添加到指示分配错误 ModelState 集合的项出现在与该属性。

开发人员还可以编写代码以显式添加到 ModelState 集合的错误条目，像我们正在下面我们"捕获"错误处理块内，这使用基于中活动的规则冲突的条目填充 ModelState 集合Dinner 对象：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>与 ModelState 的 html 帮助程序集成

呈现输出时，Html.TextBox()-等的 HTML 帮助器方法检查 ModelState 集合。 如果存在错误项，它们呈现的用户输入的值和 CSS 错误类。

例如，在我们"的编辑"视图中我们将使用 Html.TextBox() 帮助器方法来呈现我们 Dinner 对象 EventDate:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

当视图已呈现在错误的方案中时，Html.TextBox() 方法检查 ModelState 集合，以查看是否存在任何与我们 Dinner 对象的"EventDate"属性相关联的错误。 当时出现错误来确定它呈现提交的用户输入 ("BOGUS") 作为值，并添加到 css 错误类&lt;输入类型 ="textbox"/&gt;它生成的标记：

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

你可以自定义要查找但是希望 css 错误类的外观。 在中定义了默认的 CSS 错误类 –"输入的验证错误"– *\content\site.css*样式表，如下所示与下面类似：

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

此 CSS 规则是什么导致我们无效的输入的元素，如下面突出显示：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Html.ValidationMessage() 帮助器方法

Html.ValidationMessage() 帮助器方法可以用于输出与特定模型属性关联的 ModelState 错误消息：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

上面的代码将输出：  *&lt;class ="字段验证错误"&gt; BOGUS 的值无效&lt;/&gt;*

Html.ValidationMessage() 帮助器方法还支持允许开发人员重写将显示错误文本消息的第二个参数：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

上面的代码将输出：  *&lt;class ="字段验证错误"&gt;\*&lt;/&gt;*而不是为存在错误时的默认错误文本EventDate 属性。

##### <a name="htmlvalidationsummary-helper-method"></a>Html.ValidationSummary() 帮助器方法

Html.ValidationSummary() 帮助器方法可以用于呈现错误消息摘要，伴随&lt;ul&gt;&lt;li /&gt;&lt;u l&gt;消息中的所有详细的错误的列表ModelState 集合：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Html.ValidationSummary() 帮助程序方法采用一个可选的字符串参数 – 定义要显示详细的错误列表上方的摘要的错误消息：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

你可以选择使用 CSS 重写错误列表如下所示。

#### <a name="using-a-addruleviolations-helper-method"></a>使用一个 AddRuleViolations 帮助器方法

我们初始 HTTP POST 编辑实现使用其 catch 块中的 foreach 语句循环访问 Dinner 对象的规则冲突，并将其添加到控制器的 ModelState 集合：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

我们会使此代码通过添加"ControllerHelpers"少清洗磁带到 NerdDinner 项目中，类，实现"AddRuleViolations"扩展方法中，添加到 ASP.NET MVC ModelStateDictionary 类的一个帮助器方法。 此扩展方法可以封装填充 ModelStateDictionary RuleViolation 错误的列表所需的逻辑：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

然后，我们可以更新我们 HTTP POST 编辑操作方法以使用此扩展方法来填充具有我们 Dinner 规则冲突的 ModelState 集合。

#### <a name="complete-edit-action-method-implementations"></a>完成编辑操作方法实现

下面的代码实现了所有我们编辑的方案所需的控制器逻辑：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

有关我们编辑实现很棒是我们控制器类和我们的视图模板都不需一无所知特定验证或通过我们的 Dinner 模型强制实施业务规则。 我们可以将其他规则在将来添加到我们的模型，而不需要对我们控制器或以使其必须支持的视图进行任何代码更改。 这为我们提供了灵活地可以方便地改进我们应用程序要求在将来使用最少的代码更改。

### <a name="create-support"></a>创建支持

我们已经完成了实现我们 DinnersController 类的"编辑"行为。 让我们现在转上它 – 它将使用户能够添加新晚餐实现的"创建"支持。

#### <a name="the-http-get-create-action-method"></a>HTTP GET 创建操作方法

我们将首先通过实现的 HTTP"GET"行为我们创建操作方法。 在有人访问时，将调用此方法*/晚餐/创建*URL。 我们实现如下所示：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

上面的代码创建一个新的 Dinner 对象，并将分配其 EventDate 属性设置为在将来的一周。 它随后将呈现基于新的 Dinner 对象的视图。 因为我们尚未显式传递到名称*View()*帮助器方法，它将使用基于约定的默认路径来解析视图模板： /Views/Dinners/Create.aspx。

让我们现在创建此视图模板。 我们可以在创建操作方法内右键单击并选择"添加视图"上下文菜单命令来执行此操作。 在"添加视图"对话框将指示我们将 Dinner 对象传递给视图模板，并选择"创建"模板自动基架：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

当我们单击"添加"按钮时，会将新的基架基于"Create.aspx"视图保存到"\Views\Dinners"目录中，Visual Studio 并将其在 IDE 中打开它：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

让我们对我们而言，默认"的创建"基架已生成文件进行一些更改，并对其进行修改设置为类似于下面：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

现在当我们运行我们的应用程序和访问时和*"/ 晚餐/创建"*以及浏览器，它会从我们创建操作实现呈现类似下面的 UI 中的 URL:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>创建实现 HTTP POST 操作方法

我们有我们创建的操作方法中实现的 HTTP GET 版本。 当用户单击"保存"按钮时它会执行窗体发送到*/晚餐/创建*URL，并在提交 HTML&lt;输入&gt;窗体中使用 HTTP POST 谓词的值。

让我们现在实现的 HTTP POST 行为的我们创建操作方法。 我们将首先通过将重载的"创建"操作方法添加到对其具有"AcceptVerbs"属性，该值指示它将处理 HTTP POST 方案我们 DinnersController:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

有多种方式我们就可以在我们 HTTP POST 启用"创建"方法内访问的已发布的窗体参数。

一种方法是创建新的 Dinner 对象，然后使用*UpdateModel()*帮助器方法 （例如，我们使用编辑操作所做的那样） 使用已发布的窗体值其进行填充。 我们然后可以将其添加到我们 DinnerRepository、 将其保存到数据库，并将用户重定向到我们的详细信息操作以显示新创建的 Dinner 使用以下代码：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

或者，我们可以使用一种方法还有我们需要 Dinner 对象作为方法参数的 create （） 操作方法。 ASP.NET MVC 将然后自动实例化新 Dinner 对象，为我们、 填充其属性使用的窗体输入，并将其传递给我们操作方法：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

上述我们操作方法验证，Dinner 对象具有已成功的值填充填充窗体 post 通过检查 ModelState.IsValid 属性。 这将返回 false，如果没有输入转换问题 (例如： EventDate 属性的"BOGUS"的字符串)，如果有任何问题我们操作方法重新显示窗体和。

如果输入的值在有效时，操作方法就尝试添加并将新 Dinner 保存到 DinnerRepository。 它包装在一个 try/catch 块为此工作，并显示窗体，如果有任何业务规则冲突 （从而导致 dinnerRepository.Save() 方法来引发异常）。

若要查看此错误处理操作中的行为，我们可以请求*/晚餐/创建*URL 和新 Dinner 有关的详细信息，请填写。 输入不正确或值将导致创建窗体，以重新显示与下面类似突出显示的错误：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

请注意如何我们创建窗体遵守精确相同验证和业务规则为我们编辑窗体。 这是因为我们验证规则和业务规则已定义在模型中，并且未嵌入在 UI 或应用程序的控制器中。 这意味着我们可以稍后更改/发展我们验证或业务规则在单个放置，并将它们应用于整个应用程序。 我们将无需更改一个内部的任何代码我们编辑或创建操作的方法来自动接受任何新的规则或对现有文件的修改。

当我们解决的输入的值并单击"保存"按钮时试，我们添加到 DinnerRepository 将成功，并且新 Dinner 将添加到数据库。 我们然后定向到*/Dinners/详细信息 / [id]* URL – 其中我们将提供有关新创建的 Dinner 的详细信息：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Delete 支持

让我们现在添加到我们 DinnersController 的"删除"支持。

#### <a name="the-http-get-delete-action-method"></a>HTTP GET Delete 操作方法

我们将首先通过实现我们删除操作方法的 HTTP GET 行为。 在有人访问时，将会调用此方法*/Dinners/删除 / [id]* URL。 下面是实现：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

尝试检索 Dinner 要删除的操作方法。 如果 Dinner 存在呈现视图基于 Dinner 对象。 如果对象不存在 （或已删除） 它返回呈现"NotFound"视图查看我们在我们的"详细信息"操作方法前面创建的模板。

我们可以在删除操作方法内右键单击并选择"添加视图"上下文菜单命令来创建"删除"查看模板。 在"添加视图"对话框中，我们将指示我们 Dinner 对象传递给其模型中，我们查看模板，并选择创建一个空的模板：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

当我们单击"添加"按钮时，Visual Studio 将我们"\Views\Dinners"目录中为我们添加新的"Delete.aspx"视图模板文件。 我们将向模板来实现删除确认屏幕中添加一些 HTML 和代码与下面类似：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

上面的代码显示要删除的 Dinner 和输出的标题&lt;窗体&gt;对 /Dinners/删除 / [id] URL 执行 POST，如果最终用户单击"删除"按钮在其中的元素。

当我们运行我们的应用程序和访问*"/ 晚餐/删除 / [id]"* URL 有效 Dinner 对象它呈现用户界面与下面类似：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **端主题内容： 我们为什么做 POST？** |
| --- |
| 您可能会问 – 为何我们未向演示创建的工作量&lt;窗体&gt;我们删除确认屏幕中？ 为什么不直接使用标准的超链接来链接到操作方法，可执行实际的删除操作？ 原因是因为我们想要小心地将其避免出现爬网程序，并搜索引擎发现了 Url 和无意中导致数据被删除时它们遵循以下链接。 HTTP GET 基于 Url 被视为"安全"，以帮助用户访问/爬网，并且它们应该不遵循 HTTP POST 的。 种好的做法是确保你始终将放破坏性或 HTTP POST 请求背后的数据修改操作。 |

#### <a name="implementing-the-http-post-delete-action-method"></a>实现 HTTP POST Delete 操作方法

我们现在有实现我们 Delete 操作方法的 HTTP GET 版本后者将显示删除确认屏幕。 当最终用户单击"删除"按钮时它将执行到窗体发布请求*/Dinners/Dinner / [id]* URL。

让我们现在实现的 HTTP"发布"行为的使用下面的代码的删除操作方法：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

我们 Delete 操作方法的 HTTP POST 版本尝试检索要删除的 dinner 对象。 如果它找不到它 （因为它已被删除） 呈现我们"NotFound"的模板。 如果找到 Dinner，它可以从 DinnerRepository 删除它。 它随后将呈现已删除"模板。

若要实现我们将操作方法中右键单击并选择"添加视图"上下文菜单的"已删除"模板。 我们将我们的视图已删除"命名，使其成为空模板 （且不会强类型模型对象）。 然后，我们将向其添加一些 HTML 内容：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

现在当我们运行我们的应用程序和访问时和*"/ 晚餐/删除 / [id]"*类似下面的对象，它会呈现我们 Dinner 删除确认有效 Dinner URL 屏幕：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

单击"删除"按钮时它将执行到 HTTP POST */Dinners/删除 / [id]* URL，这将删除 Dinner 从我们的数据库，并显示我们已删除"查看模板：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>模型绑定安全性

我们已讨论了两种不同方式使用 ASP.NET MVC 的内置模型绑定功能。 首先使用 UpdateModel() 方法来更新现有模型对象的属性和第二个使用 ASP.NET MVC 将作为操作方法参数传递中的模型对象的支持。 这两种技术是非常强大和非常有用。

此 power 也会与其责任。 必须始终为需要保持警惕有关安全，如果接受任何用户输入，并也是如此时将对象绑定到窗体输入。 应注意到 HTML 始终对任何用户输入的值，以避免 HTML 和 JavaScript 注入攻击，并小心 SQL 注入式攻击进行编码 (请注意： 我们将使用 LINQ to SQL 对于我们的应用程序，它自动将编码参数以防止这些类型的攻击）。 永远不应依赖于客户端验证单独出现时，并始终使用服务器端验证若要防止黑客尝试发送伪造的值。

请确保你考虑使用 ASP.NET MVC 的绑定功能时的一个额外的安全项目是你正在绑定的对象的作用域。 具体而言，你想要确保你了解你允许将以可绑定，并确保仅允许那些实际上应为可更新最终用户要更新的属性的属性的安全含义。

默认情况下，UpdateModel() 方法将尝试更新模型对象传入的窗体参数值相匹配的所有属性。 同样，为操作方法参数也默认传递的对象会有所有窗体参数通过设置其属性。

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>锁定按每个使用量付费的绑定

您可以通过提供显式"包括列表"可更新的属性的锁定按每个使用量付费的绑定策略。 这可以通过将的额外字符串数组参数传递给 UpdateModel() 类似方法下：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

对象作为参数传递，操作方法还支持启用"包括列表"的允许属性，如下面指定的 [绑定] 特性：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>锁定基于类型的绑定

您可以锁定根据每种类型的绑定规则。 这允许您一次，指定绑定规则，然后将它们在所有方案 （包括 UpdateModel 和操作方法参数方案） 中应用所有控制器和操作方法。

通过添加到一种类型，[绑定] 属性或注册 （适用于您自己的类型的方案） 应用程序的 Global.asax 文件中，你可以自定义每种类型的绑定规则。 然后，你可以使用绑定属性的包含和排除属性来控制哪些属性是可绑定为特定类或接口。

我们将使用此方法来 Dinner 类进行中我们 NerdDinner 的应用程序，并将 [绑定] 特性添加到它的可绑定属性的列表限制到以下：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

请注意我们不允许 RSVPs 集合要操作通过绑定，也不我们允许 DinnerID 或 HostedBy 属性，以通过绑定设置。 出于安全原因我们将改为仅操作使用我们的操作方法中的显式代码这些特定属性。

### <a name="crud-wrap-up"></a>CRUD 总结

ASP.NET MVC 包括许多内置功能，可帮助完成实现发布方案的窗体。 我们使用了各种各样的这些功能在我们 DinnerRepository 之上提供 CRUD UI 支持。

我们将使用的模型已设定焦点的方法来实现我们的应用程序。 这意味着，我们的验证和业务规则在我们的模型层 – 以及不中我们控制器或视图定义逻辑。 我们控制器类和我们查看模板都不知道任何有关由我们 Dinner 模型类强制实施的特定业务规则。

这将使我们的应用程序体系结构清理并使它可以轻松地测试。 我们可以在将来向我们的模型层添加更多的业务规则和*无需进行任何代码更改*到我们的控制器或视图，以使其支持。 这将向我们提供了极大的灵活性发展和更改在将来，我们的应用程序。

我们 DinnersController 现在使 Dinner 列表/详细信息，以及创建、 编辑和 delete 支持。 类的完整代码可在下面找到：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>下一步

我们现在有我们 DinnersController 类中实现的基本 CRUD （创建、 读取、 更新和删除） 支持。

让我们现在看一下如何我们可以使用 ViewData 和 ViewModel 类我们窗体上启用甚至更丰富的 UI。

>[!div class="step-by-step"]
[上一页](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
[下一页](use-viewdata-and-implement-viewmodel-classes.md)
