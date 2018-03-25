---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: "使用 AJAX 提供动态更新 |Microsoft 文档"
author: microsoft
description: "步骤 10 实现支持登录的用户的回复到其感兴趣的参加 dinner，使用基于 Ajax 的方法内 dinner 详细信息集成..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7b75f8c6cf08112eb77d1a9a40222ed1425ef3a7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>使用 AJAX 提供动态更新
====================
通过[Microsoft](https://github.com/microsoft)

[下载 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 这是一个免费的步骤 10 ["NerdDinner"应用程序教程](introducing-the-nerddinner-tutorial.md)，查找步程取如何生成一个较小，但完成，使用 ASP.NET MVC 1 web 应用程序。
> 
> 步骤 10 实现支持登录的用户的回复到其感兴趣的参加 dinner，使用集成在 dinner 详细信息页内基于 Ajax 的方法。
> 
> 如果你使用的 ASP.NET MVC 3，我们建议你遵循[获取启动使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC 音乐商店](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教程。


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner 步骤 10: AJAX 启用 RSVPs 接受

让我们现在可实现对登录的用户 RSVP 其感兴趣的参加 dinner 支持。 我们将使用支持此集成在 dinner 详细信息页内的基于 AJAX 的方法。

### <a name="indicating-whether-the-user-is-rsvpd"></a>该值指示用户是否答复活动

用户可以访问*/Dinners/详细信息 / [id*] URL 以查看有关特定 dinner 的详细信息：

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

操作方法实现 Details() 如下所示：

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

若要实现的回复支持我们第一步是将"IsUserRegistered(username)"帮助程序方法添加到我们 Dinner 对象 （在我们之前生成的 Dinner.cs 分部类）。 此帮助器方法返回 true 或 false，具体取决于是否用户当前答复活动为 Dinner:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

然后我们可以将下面的代码添加到我们 Details.aspx 视图模板，以显示相应的消息，指示是否注册用户或不事件：

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

然后，现在当用户访问为注册 Dinner 它们将看到以下消息：

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

和当他们访问他们未注册为他们将看到 Dinner 以下信息：

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>实现 Register 操作方法

让我们现在将添加必要的功能，使用户能够 dinner 的回复从详细信息页。

若要实现这一点，我们将创建一个新"RSVPController"类 \Controllers 目录上右键单击并选择添加-&gt;控制器菜单命令。

我们将实施的新的 RSVPController 类中作为自变量 Dinner 为采用 id，以检索相应的 Dinner 对象，将检查以确定如果登录的用户是当前在列表中的用户已注册的并且"注册"操作方法不将回复对象添加为它们：

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

请注意上面作为操作方法的输出，我们就会返回一个简单的字符串。 我们无法嵌入在视图模板 – 此消息，但因为它是太小只需将控制器基类和返回一个字符串消息类似上面使用的 Content() 帮助器方法。

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>调用使用 AJAX RSVPForEvent 操作方法

我们将使用 AJAX 调用 Register 操作方法从我们的详细信息视图。 实现这是这么简单。 首先我们将添加两个脚本库引用：

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

第一个库引用的核心 ASP.NET AJAX 客户端脚本库。 此文件为大约 24 k （压缩） 的大小，并包含核心客户端 AJAX 功能。 第二个库包含将与 ASP.NET MVC 的内置 AJAX 帮助器使用的方法 （我们将很快） 集成的实用工具函数。

我们可以更新我们前面添加，以便而不是 outputing"你未注册此事件"消息，我们改为呈现一个链接，推送时查看模板代码执行时，将调用我们的回复控制器上我们 RSVPForEvent 操作方法的 AJAX 调用和 RSVPs 用户：

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

上面使用的 Ajax.ActionLink() 帮助器方法是内置于 ASP.NET MVC，类似于 Html.ActionLink() 帮助器方法，只不过而不是执行标准导航它使 AJAX 调用对象的操作方法单击该链接时。 更高版本，我们已调用"回复"控制器"注册"操作方法，并将 DinnerID 作为"id"参数传递给它。 我们传递的最后一个 AjaxOptions 参数指示我们想要采取的操作方法从返回的内容并更新 HTML &lt;div&gt;其 id 是"rsvpmsg"页上的元素。

而且，现在当用户浏览到 dinner 它们不为注册尚他们将看到它的回复的链接：

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

如果他们单击"为此事件的回复"链接，它们将在回复控制器上，使 AJAX 调用对象的注册操作方法和在其完成后他们将看到更新的消息如下所示：

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

网络带宽和流量时执行此 AJAX 调用涉及是真正轻量。 当用户单击"为此事件的回复"链接时，对进行小的 HTTP POST 网络请求*/Dinners/Register/1*看起来与下面类似的网络的 URL:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

只需为我们 Register 操作方法的响应：

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

此轻型调用速度快，即使在慢速网络上将工作。

### <a name="adding-a-jquery-animation"></a>添加 jQuery 动画

我们实现的 AJAX 功能可以很好地进行快速进行。 有时也可能如此之快，不过，用户可能不到已替换的回复链接为使用新文本。 若要使结果更明显我们可以添加以吸引人们关注更新消息的一个简单动画。

默认情况下 ASP.NET MVC 项目模板包括 jQuery – Microsoft 也支持一个极好 （和非常受欢迎） 开源 JavaScript 库。 jQuery 提供了大量功能，包括良好的 HTML DOM 选择和效果库。

若要使用 jQuery 我们将先添加对它的脚本引用。 因为我们要在我们的站点内使用 jQuery 在各种位置中的，我们将在我们 Site.master 主控页文件内添加脚本引用，以便所有页面可以都使用它。

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*提示： 请确保你已经安装了使 JavaScript 文件 （包括 jQuery） 的更好地 intellisense 支持的 VS 2008 SP1 的 JavaScript intellisense 修补程序。你可以下载它从： http://tinyurl.com/vs2008javascripthotfix*

通常使用 JQuery 编写的代码使用全局"$ （）"JavaScript 方法可检索一个或多个使用 CSS 选择器的 HTML 元素。 例如， *$("#rsvpmsg")*选择 id 为 rsvpmsg，任何 HTML 元素时*$(".something")*将选择具有"内容"CSS 的所有元素类名。 你还可以编写类似"返回所有选中的单选按钮"更多高级的查询使用类似的选择器查询： *$("输入 [@type= 单选] [@checked]")*。

在你选择元素后，你可以对其执行操作，如隐藏它们调用方法： *$("#rsvpmsg").hide();*

对于我们的回复的方案，我们将定义一个名为"AnimateRSVPMessage"选择"rsvpmsg"的简单 JavaScript 函数&lt;div&gt;和进行动画处理其文本内容的大小。 下面的代码启动小写的文本，然后原因它增加一 400 毫秒的时间段：

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

我们可以然后网络型我们 AJAX 调用成功通过将其名称传递给我们 Ajax.ActionLink() 帮助器方法完成后调用此 JavaScript 函数 (通过 AjaxOptions"OnSuccess"事件属性):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

和现在时单击"为此事件的回复"链接和我们 AJAX 调用已成功完成，发送的内容消息后将进行动画处理，并会变得大：

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

除了提供"OnSuccess"事件，AjaxOptions 对象公开 OnBegin、 OnFailure 和 OnComplete 可以处理 （以及各种其他属性和有用的选项） 的事件。

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>清理-重构出 RSVP 分部视图

我们的详细信息视图模板正在开始获取有点长，哪些加班将使其有点难以理解。 为了帮助提高代码可读性，让我们最后创建封装了我们的详细信息页面的回复查看代码的所有分部视图 – RSVPStatus.ascx –。

我们可以执行此操作通过右键单击 \Views\Dinners 文件夹，然后选择添加-&gt;查看菜单命令。 我们将具有其采用将 Dinner 对象作为其强类型化视图模型。 我们可以然后复制/粘贴的回复内容从到其中我们 Details.aspx 视图。

一旦我们已完成该操作，让我们还创建另一个分部视图 – EditAndDeleteLinks.ascx-封装我们编辑和删除的链接查看代码。 我们还将拥有它采用将 Dinner 对象作为其强类型视图模型，并复制/粘贴到其中我们 Details.aspx 视图的编辑和删除逻辑。

我们详细信息查看模板，则只需包括在底部的两个 Html.RenderPartial() 方法调用：

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

这使代码更简洁读取和维护。

### <a name="next-step"></a>下一步

让我们现在看一下我们可以进一步使用 AJAX 和将交互式映射支持添加到我们的应用程序。

>[!div class="step-by-step"]
[上一页](secure-applications-using-authentication-and-authorization.md)
[下一页](use-ajax-to-implement-mapping-scenarios.md)
