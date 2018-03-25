---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
title: "迭代 #7-添加 Ajax 功能 (C#) |Microsoft 文档"
author: microsoft
description: "在第七个迭代中，我们通过添加对 Ajax 的支持提高响应能力和我们的应用程序的性能。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1b0809e-8909-444e-b6bb-a5cd1dea3f72
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
msc.type: authoredcontent
ms.openlocfilehash: db313d12dfd6a146347f253dc3a1f4a889bee780
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="iteration-7--add-ajax-functionality-c"></a>迭代 #7-添加 Ajax 功能 (C#)
====================
通过[Microsoft](https://github.com/microsoft)

[下载代码](iteration-7-add-ajax-functionality-cs/_static/contactmanager_7_cs1.zip)

> 在第七个迭代中，我们通过添加对 Ajax 的支持提高响应能力和我们的应用程序的性能。


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>生成联系人管理 ASP.NET MVC 应用程序 (C#)

在这一系列的教程，我们生成整个联系人管理应用程序从头到尾完成。 联系人管理器应用程序，可存储的名称，电话号码和电子邮件地址的联系人信息有关的人员列表。

我们在多次迭代中生成应用程序。 每次迭代时，我们逐渐提高应用程序。 此多个迭代方法旨在使您能够了解每个更改的原因。

- 迭代 #1-创建应用程序。 在第一次迭代中，我们创建联系人管理器中的最简单方法可能。 我们将添加对基本数据库操作的支持： 创建、 读取、 更新和删除 (CRUD)。

- 迭代 #2-使应用程序，看上去很好。 在此迭代中，我们通过修改默认 ASP.NET MVC 视图母版页和级联样式表改进应用程序的外观。

- 迭代 #3-添加窗体验证。 在第三个迭代中，我们将添加基本窗体验证。 我们可以防止人员提交窗体，但不完成需要的表单域。 我们还验证电子邮件地址和电话号码。

- 迭代 #4-请松散耦合的应用程序。 在此第三个迭代中，我们利用多个软件设计模式以使其更轻松地监视和修改联系人管理器应用程序。 例如，我们将重构应用程序以使用存储库模式和依赖关系注入模式。

- 迭代 #5-创建单元测试。 在第五个迭代中，我们使我们的应用程序更轻松地监视和修改通过添加单元测试。 我们模拟我们数据模型类，并生成单元测试控制器和验证逻辑。

- 迭代 #6-使用测试驱动开发。 在此第六个迭代中，我们将添加新功能到我们的应用程序通过首先编写单元测试和针对单元测试编写代码。 在此迭代中，我们添加联系人的组。

- 迭代 #7-添加 Ajax 功能。 在第七个迭代中，我们通过添加对 Ajax 的支持提高响应能力和我们的应用程序的性能。

## <a name="this-iteration"></a>此迭代

此迭代中的联系人管理器应用程序，我们将重构应用程序以利用 Ajax。 通过利用 Ajax，我们使我们的应用程序更快地响应。 我们可以避免当我们需要更新某些区域在页面呈现整个页面。

以便我们不需要以重新显示整个页面，每当有人选择新的联系人组时，我们将重构我们索引视图。 相反，当有人单击联系人的组时，我们将只需更新联系人的列表并保留单独网页的其余内容。

我们还将更改我们删除链接的工作原理的方式。 而不是显示一个单独的确认页面，我们将显示一个 JavaScript 确认对话框。 如果你确认你想要删除联系人，针对要从数据库中删除联系人记录的服务器执行 HTTP DELETE 操作。

此外，我们将利用 jQuery 将动画效果添加到我们的索引视图。 正在从服务器提取新的联系人列表时，我们将显示一种动画效果。

最后，我们将利用 ASP.NET AJAX 框架支持用于管理浏览器历史记录。 每当我们执行通过 Ajax 调用才能更新联系人的列表，我们将创建历史时间点。 这样一来，浏览器向后和向前按钮将工作。

## <a name="why-use-ajax"></a>为何使用 Ajax？

使用 Ajax 具有诸多优点。 首先，将 Ajax 功能添加到应用程序将导致更好的用户体验。 在普通的 web 应用程序中，整个页面必须要回发到服务器每个用户执行的操作的时间。 每当执行操作时，浏览器锁和用户必须等待，直到提取和重新显示整个页面。

这将是不可接受的体验，对于桌面应用程序。 但是，传统上，我们留存与在 web 应用程序的情况下此不良用户体验因为我们不知道，我们无法执行任何好。 我们认为它是 web 应用程序的限制时，在现实中，它只是我们想象力的限制。

在 Ajax 应用程序，您不需要以使异常终止只是为了将更新页面的用户体验。 相反，您可以在后台以更新的页面中执行的异步请求。 您不 t 强制用户等待获取更新页面的一部分时。

通过利用 Ajax，您还可以改善你的应用程序的性能。 请考虑联系人管理器应用程序工作原理稍后再试而无需将 Ajax 功能。 单击联系人的组时，整个索引视图必须重新显示。 必须从数据库服务器检索的联系人列表和联系人的组的列表。 所有这些数据必须传递跨网络从 web 服务器为 web 浏览器。

我们将 Ajax 功能添加到我们的应用程序之后，但是，我们可以避免重新显示整个页面，当用户单击联系人的组。 我们不再需要获取从数据库联系人的组。 我们还不 t 需要将整个索引视图推送到网络。 通过利用 Ajax，我们减少的我们的数据库服务器必须执行的工作和我们减少所需的我们的应用程序的网络流量的量。

## <a name="don-t-be-afraid-of-ajax"></a>不要将担心的 Ajax

一些开发人员避免使用 Ajax，因为他们担心下层浏览器。 他们想要确保其 web 应用程序由不支持 JavaScript 的浏览器访问时仍将起作用。 由于 Ajax 依赖于 JavaScript，一些开发人员应避免使用 Ajax。

但是，如果你非常小心有关如何实现 Ajax 然后可以生成使用上层和下层浏览器应用程序。 我们的联系人管理器应用程序会使用支持 JavaScript 的浏览器和浏览器不这样做。

如果支持 JavaScript 的浏览器中使用联系人管理器的应用程序，然后将具有更好的用户体验。 例如，当你单击联系人的组，则将更新仅显示联系人的页的区域。

如果使用浏览器不支持 JavaScript （或已禁用 JavaScript） 使用联系人管理器应用程序在另一方面，你将具有略有不太理想的用户体验。 例如，当你单击联系人的组，整个索引视图必须将发送回浏览器以显示匹配的联系人列表。

## <a name="adding-the-required-javascript-files"></a>添加所需的 JavaScript 文件

我们将需要使用三个 JavaScript 文件将 Ajax 功能添加到我们的应用程序。 新的 ASP.NET MVC 应用程序的脚本文件夹中包含这些文件的所有三个。

如果你计划在你的应用程序在多个页面中使用 Ajax 然后其意义要包含在你应用程序的视图母版页中的所需的 JavaScript 文件。 这样一来，JavaScript 文件都将包括所有在你的应用程序页中自动。

将添加以下 JavaScript 包含内部&lt;头&gt;视图母版页的标记：

[!code-html[Main](iteration-7-add-ajax-functionality-cs/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>重构要使用 Ajax 的索引视图

让我们来首先修改我们索引视图，以便显示联系人的视图的区域，仅单击联系人的组更新。 图 1 中的红色框包含我们想要更新的区域。


[![更新仅联系人](iteration-7-add-ajax-functionality-cs/_static/image1.jpg)](iteration-7-add-ajax-functionality-cs/_static/image1.png)

**图 01**： 更新仅联系人 ([单击以查看实际尺寸的图像](iteration-7-add-ajax-functionality-cs/_static/image2.png))


第一步是视图的将我们想要以异步方式更新到单独的部分 （视图用户控件） 的一部分。 显示的联系人的表的索引视图的部分已移动到列表 1 中部分。

**列表 1-Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample2.aspx)]

请注意，列表 1 中的部分不同于索引视图的模型。 *Inherits*属性中&lt;%@ 页 %&gt;指令指定分部继承 ViewUserControl&lt;组&gt;类。

更新的索引视图包含在清单 2。

**列出 2-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample3.aspx)]

有您应注意到了更新的视图，列出 2 中的两个项。 首先，请注意，则将所有内容已移动到分部替换通过 Html.RenderPartial() 调用。 为了显示联系人的初始设置首次请求的索引视图时调用 Html.RenderPartial() 方法。

其次，请注意，用来显示联系人组 Html.ActionLink() 已替换为 Ajax.ActionLink()。 使用以下参数调用 Ajax.ActionLink():

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample4.aspx)]

第一个参数表示要显示的链接的文本、 第二个参数表示路由值和第三个参数表示 Ajax 的选项。 在这种情况下，我们使用 UpdateTargetId Ajax 选项指向 HTML &lt;div&gt;我们想要更新在 Ajax 请求完成之后的标记。 我们想要更新&lt;div&gt;标记中使用新的联系人列表。

列出 3 中包含的联系人控制器更新的 index （） 方法。

**列出 3-Controllers\ContactController.cs （Index 方法）**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample5.cs)]

更新后的 index （） 操作有条件地返回以下两种含义。 如果通过 Ajax 请求调用 index （） 操作则控制器将返回部分。 否则，index （） 操作返回的整个视图。

请注意 index （） 操作不需要返回多调用 Ajax 请求时的数据。 在正常的请求的上下文中，索引操作返回的所有联系人的组和所选联系人的组的列表。 Ajax 请求的上下文，在 index （） 操作将返回选定的组。 Ajax 意味着在你的数据库服务器上的工作较少。

我们已修改的索引视图的工作原理在上层和下层浏览器的情况下。 如果您单击某个联系人的组，并且你的浏览器支持 JavaScript，则更新仅包含联系人列表中的视图的区域。 如果在另一方面，你的浏览器不支持 JavaScript，则更新整个视图。


我们已更新的索引视图都有一个问题。 单击联系人的组时，所选的组不会突出显示。 在更新期间 Ajax 请求的区域之外显示的组列表，因为不获取突出显示正确的组。 我们将在下一部分中修复此问题。


## <a name="adding-jquery-animation-effects"></a>添加 jQuery 动画效果

通常情况下，当您单击网页中的链接，你可以使用浏览器进度栏检测浏览器主动提取更新的内容。 在执行 Ajax 请求时，另一方面，浏览器进度栏不显示任何进度。 这可以使用户紧张。 如何知道是否已冻结浏览器？

有多种方法，你可以向表明用户需在执行 Ajax 请求时执行工作。 一种方法是显示一个简单动画。 例如，你可以淡入淡出出区域 Ajax 请求开始和淡出该区域，而在请求完成时。

我们将使用 jQuery 库包含在 Microsoft ASP.NET MVC framework 中，若要创建的动画效果。 更新的索引视图包含在清单 4。

**列出 4-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample6.aspx)]

请注意，更新的索引视图包含三个新的 JavaScript 函数。 前两个函数使用 jQuery 淡出和淡入淡出的联系人列表中单击新的联系人组时。 第三个函数中出现错误 （例如，网络超时） 显示一条错误消息时 Ajax 请求结果。

第一个函数还负责的突出显示所选的组。 一个类 = 所选的属性添加到单击的元素的父元素 （LI 元素）。 同样，jQuery 可以轻松选择正确元素并添加 CSS 类。

这些脚本被绑定到 Ajax.ActionLink() AjaxOptions 参数的帮助的组链接。 更新的 Ajax.ActionLink() 方法调用类似如下所示：

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>添加浏览器历史记录支持

通常情况下，单击更新页的链接时，将更新你的浏览器历史记录。 这样一来，你可以单击浏览器后退按钮以将移回及时页的前一状态。 例如，如果您单击的好友联系人组，然后单击业务联系人组，你可以单击浏览器后退按钮导航回页的状态时未选择的好友联系人组。

遗憾的是，执行 Ajax 请求不浏览器历史记录自动更新。 如果您单击某个联系人的组，并且使用 Ajax 请求检索匹配联系人的列表，则不会更新的浏览器历史记录。 浏览器的后退按钮不能用于在选择新的联系人组后导航回联系人的组。

如果你希望用户能够使用浏览器返回按钮执行 Ajax 请求后然后你需要执行一些工作。 你需要充分利用 ASP.NET AJAX 框架中内置的浏览器历史记录管理功能。

ASP.NET AJAX 浏览器历史记录，你需要做三件事：

1. 通过将 enableBrowserHistory 属性设置为 true 来启用浏览器历史记录。
2. 通过调用 addHistoryPoint() 方法的视图状态更改时，请保存历史时间点。
3. 引发导航事件时，重新构造的视图状态。

更新的索引视图包含在列出 5。

**列出 5-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample8.aspx)]

在列出 5 中，pageInit() 函数中启用了浏览器历史记录。 PageInit() 函数还用于设置导航事件的事件处理程序。 转发的浏览器或后退按钮会导致页后，可以更改的状态时，将引发导航事件。

单击联系人的组时，被调用 beginContactList() 方法。 此方法通过调用 addHistoryPoint() 方法创建新的历史时间点。 单击联系人组的 id 添加到历史记录。

从联系人组链接的 expando 属性检索组 id。 则链接呈现与对 Ajax.ActionLink() 的以下调用。

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample9.aspx)]

传递给 Ajax.ActionLink() 的最后一个参数将添加一个名为 groupid （XHTML 兼容性为小写） 链接到的 expando 特性。

当用户点击回浏览器或前进按钮时，将引发导航事件和调用 navigate() 方法。 此方法将更新以匹配到浏览器历史记录点传递给 navigate 方法对应的页的状态的页中显示的联系人。

## <a name="performing-ajax-deletes"></a>执行 Ajax 删除

目前，若要删除联系人，你需要单击删除链接，然后单击删除确认页中显示删除按钮 （请参见图 2）。 这看起来像大量的页请求来完成更简单如删除的数据库记录。


[![删除确认页](iteration-7-add-ajax-functionality-cs/_static/image2.jpg)](iteration-7-add-ajax-functionality-cs/_static/image3.png)

**图 02**: 删除确认页 ([单击以查看实际尺寸的图像](iteration-7-add-ajax-functionality-cs/_static/image4.png))


它很容易跳过删除确认页并直接从索引视图中删除联系人。 应当避免此倾向做法，因为采用此方法将打开应用程序以便对安全漏洞。 一般情况下，don t 想要调用的操作修改 web 应用程序的状态时执行一个 HTTP GET 操作。 当执行 delete，你想要执行 HTTP POST，或者更好的尚未，HTTP DELETE 操作。

删除链接包含在部分联系人列表。 在列出 6 中包含部分 ContactList 的更新的版本。

**列出 6-Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample10.aspx)]

删除链接来呈现与对 Ajax.ImageActionLink() 方法的以下调用：

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample11.aspx)]

> [!NOTE] 
> 
> Ajax.ImageActionLink() 不是 ASP.NET MVC framework 的标准部分。 Ajax.ImageActionLink() 是 Contact Manager 项目中包含自定义帮助器方法。


AjaxOptions 参数具有两个属性。 首先，确认属性用于显示一个弹出窗口 JavaScript 确认对话框。 其次，HttpMethod 属性用于执行 HTTP DELETE 操作。

列出 7 包含新 AjaxDelete() 操作已添加到联系人控制器。

**列出 7-Controllers\ContactController.cs (AjaxDelete)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample12.cs)]

AjaxDelete() 操作是使用 AcceptVerbs 属性进行修饰。 此属性可防止调用除 HTTP DELETE 操作以外的任何 HTTP 操作的操作。 具体而言，不能调用此操作使用 HTTP GET。

删除数据库记录后，你需要显示的联系人不包含已删除的记录更新的列表。 AjaxDelete() 方法返回部分 ContactList 和更新的联系人的列表。

## <a name="summary"></a>摘要

在此迭代中，我们添加到我们的联系人管理器应用程序 Ajax 功能。 我们使用 Ajax 提高响应能力和我们的应用程序的性能。

首先，我们将重构索引视图，以便单击联系人的组不会更新整个视图。 相反，单击所需联系组只会更新联系人的列表。

接下来，我们使用 jQuery 动画效果淡出和淡入联系人的列表。 将动画添加到 Ajax 应用程序可以用于提供与浏览器进度栏的等效项应用程序的用户。

我们还添加到我们 Ajax 应用程序的浏览器历史记录支持。 我们已启用的用户单击浏览器返回并将转发按钮，以更改索引视图的状态。

最后，我们创建一个删除链接，以支持 HTTP DELETE 操作。 通过执行 Ajax 删除，我们使用户能够删除数据库记录，而无需用户请求其他删除确认页。

>[!div class="step-by-step"]
[上一页](iteration-6-use-test-driven-development-cs.md)
[下一页](iteration-1-create-the-application-vb.md)
