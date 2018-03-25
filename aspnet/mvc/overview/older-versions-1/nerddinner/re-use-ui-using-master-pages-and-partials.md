---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: "重用使用母版页和它们的 UI |Microsoft 文档"
author: microsoft
description: "在我们查看模板，以消除代码重复，使用分部视图模板和主控页内，第 7 步考察我们可以将干原则应用的方式。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: c42cd6aca40b08a9f8461532fbfd0589901b64ad
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="re-use-ui-using-master-pages-and-partials"></a>重用使用母版页和它们的 UI
====================
通过[Microsoft](https://github.com/microsoft)

[下载 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 这是一个免费的步骤 7 ["NerdDinner"应用程序教程](introducing-the-nerddinner-tutorial.md)，查找步程取如何生成一个较小，但完成，使用 ASP.NET MVC 1 web 应用程序。
> 
> 在我们查看模板，以消除代码重复，使用分部视图模板和主控页内，第 7 步考察我们可以将"干原则"应用的方式。
> 
> 如果你使用的 ASP.NET MVC 3，我们建议你遵循[获取启动使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC 音乐商店](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教程。


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner 步骤 7： 它们并母版页

ASP.NET MVC 涵盖的设计理念之一是"执行不重复自己"原则 （通常称为"模拟"）。 干设计可帮助消除重复代码和逻辑，最终会使应用程序来生成更快且更易维护。

我们已经了解应用在了几个我们 NerdDinner 方案原则。 几个示例： 我们验证逻辑实现在我们的模型层，这使它能够跨这两个编辑会强制执行并在我们控制器; 中创建方案我们将编辑、 详细信息和删除操作方法; 跨重新使用"NotFound"视图模板我们将通过我们的视图模板，从而无需显式指定的名称，当我们调用 View() 帮助器方法; 时使用的约定的命名模式我们重新将 DinnerFormViewModel 类用于这两个编辑和创建操作方案。

让我们现在看一下我们可以将"干原则"应用的方式在我们查看模板以及消除存在代码重复。

### <a name="re-visiting-our-edit-and-create-view-templates"></a>重新访问我们的编辑和创建视图模板

当前，我们将使用两个不同的视图模板 –"Edit.aspx"和"Create.aspx"– 来显示我们 Dinner 窗体 UI。 突出显示了其中的快速 visual 比较，它们是如何类似。 下面是创建窗体如下所示：

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

和下面是我们"的编辑"窗体的外观：

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

有了不大部分差异？ 以外的标题和标头的文本，窗体布局和输入控件是相同的。

如果我们打开"Edit.aspx"和"Create.aspx"查看模板我们就会发现它们包含相同的窗体的布局和输入控件的代码。 这种重复意味着我们结束，无需进行更改的任何我们引入或更改的是一个新的 Dinner 属性-是不好的时候两次。

### <a name="using-partial-view-templates"></a>使用分部视图模板

ASP.NET MVC 支持能够定义可以用于封装视图呈现逻辑的页面的一个子部分的"分部视图"模板。 "它们"提供有用的方式来一次，定义视图呈现逻辑，然后重新使用它在多个位置跨应用程序。

为了帮助"干型"我们 Edit.aspx 和 Create.aspx 视图模板重复，我们可以创建一个名为"DinnerForm.ascx"，用于封装窗体布局和公用的输入的元素的分部视图模板。 我们需要执行此操作我们/视图/晚餐目录上右键单击并选择"添加-&gt;视图"菜单命令：

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

这将显示"添加视图"对话框。 我们将新视图命名为我们想要创建"DinnerForm"，选中"创建了分部视图"复选框在对话框中，并指示，我们会将它传递 DinnerFormViewModel 类：

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

当我们单击"添加"按钮时，Visual Studio 将创建新"DinnerForm.ascx"视图模板为我们"\Views\Dinners"目录中。

我们可以然后复制/粘贴重复窗体布局 / 我们新的"DinnerForm.ascx"分部视图模板中输入从我们 Edit.aspx/ Create.aspx 查看模板的控件代码：

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

然后，我们可以更新我们编辑和创建视图模板，以调用 DinnerForm 部分模板并消除窗体重复。 我们可以通过调用 Html.RenderPartial("DinnerForm") 实现此我们视图模板中：

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

你可以显式限定调用 Html.RenderPartial 时所需的部分模板的路径 (例如: ~ Views/Dinners/DinnerForm.ascx")。 在上面代码中，不过，我们将利用基于约定的命名模式在 ASP.NET MVC 并只需指定"DinnerForm"作为分部呈现的名称。 我们执行此操作 ASP.NET MVC 将查找基于约定的 views 目录中的第一行 （这将是/视图/晚餐 DinnersController)。 如果找不到的部分的模板存在它随后将查找为其在 /Views/Shared 目录中。

当只分部视图的名称与调用 Html.RenderPartial() 时，ASP.NET MVC 将传递给分部视图相同的模型和 ViewData 字典的对象由调用的视图模板。 或者，有的 Html.RenderPartial() 重载使您能够通过将其他模型对象和/或要使用的分部视图的 ViewData 字典的版本。 这非常有用的情况下，你仅希望传递完整模型/视图模型的子集。

| **端主题内容： 为什么&lt;%%&gt;而不是&lt;%= %&gt;？** |
| --- |
| 你可能已经注意到使用上面的代码的细微项目之一是，我们将使用&lt;%%&gt;阻止而不是&lt;%= %&gt;阻止调用 Html.RenderPartial() 时。 &lt;%= %&gt; ASP.NET 中的块指示开发人员想要呈现指定的值 (例如： &lt;%="Hello"%&gt;将会呈现"Hello")。 &lt;%%&gt;块而是指示开发人员想要执行的代码，并且必须显式完成任何呈现其中的输出 (例如： &lt;%response.write("hello")%&gt;。 我们将使用的原因&lt;%%&gt;代码块替换上述我们 Html.RenderPartial 代码是因为 Html.RenderPartial() 方法不返回一个字符串，并改为输出调用的视图模板直接将内容的输出流。 这是为了效率提高性能，并通过执行操作，因此它可以避免需要创建一个 （可能非常大） 的临时字符串对象。 这可减少内存使用量并提高应用程序的总体吞吐量。 在使用 Html.RenderPartial() 即将忘记中时调用的末尾添加分号时的一种常见错误&lt;%%&gt;块。 例如，此代码将导致编译器错误： &lt;%html.renderpartial("dinnerform")%&gt;需要改为编写： &lt;%html.renderpartial("dinnerform"); %&gt;这是因为&lt;%%&gt;块都是自包含的代码语句，并使用 C# 代码语句需要使用分号终止时。 |

### <a name="using-partial-view-templates-to-clarify-code"></a>使用分部视图模板以阐明代码

我们创建了"DinnerForm"分部视图模板，以避免重复多个位置中的视图呈现逻辑。 这是创建分部视图模板的最常见原因。

有时，仍有必要创建分部视图，即使它们正在单个位置被调用。 非常复杂的视图模板通常可能变得更易读在提取、 分区到一个或多也名为模板的部分其视图呈现逻辑。

例如，考虑下面我们项目 （该我们将很快考察） 中的 Site.master 文件中的代码段。 代码是比较直接的操作读取 – 部分原因逻辑来显示登录/注销链接顶部屏幕的右侧封装在"LogOnUserControl"部分：

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

每当你发现自己感到迷惑尝试了解视图模板中的 html/代码标记，请考虑是否它不是清楚地了解如果部分内容成功提取并重构为正确命名分部视图。

### <a name="master-pages"></a>母版页

除了支持分部视图，ASP.NET MVC 还支持能够创建用于定义常用布局和顶级 html 的站点的"母版页"模板。 内容的占位符控件然后，可以添加到主页后，可以确定可替换区域可以重写或"已填写"视图。 这提供了将通用布局应用跨应用程序的非常有效 （和干） 方法。

默认情况下，新的 ASP.NET MVC 项目具有母版页模板自动添加到它们。 此母版页是名为"Site.master"和生活 \Views\Shared\ 文件夹中：

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

默认 Site.master 文件类似于下面。 它定义的站点，以及在顶部导航菜单的外部 html。 它包含两个可替换内容占位符控件 – 一个用于标题、，另一个用于应替换为主要内容的页面：

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

我们已为我们 NerdDinner 的应用程序 （"列表"、"详细信息"、"编辑"、"创建"、"NotFound"等） 创建的视图模板的所有具有已在基于此 Site.master 模板。 这通过已按默认添加到顶部的"MasterPageFile"属性指示&lt;%@ 页 %&gt;指令我们创建我们视图使用"添加视图"对话框时：

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

这意味着，我们可以更改 Site.master 内容，并且具有所做的更改自动被应用，并且使用当我们呈现任何我们查看模板。

让我们更新我们 Site.master 标头部分，以便我们的应用程序的标头"NerdDinner"而不是"My MVC Application"。 让我们还更新我们导航菜单使第一个选项卡为"查找 Dinner"（由 HomeController 的 index （） 操作方法），并让我们添加名为"主机 Dinner"（由 DinnersController 的 create （） 操作方法） 的新选项卡：

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

当我们保存 Site.master 文件并刷新我们浏览器，我们将看到我们标头更改显示在我们的应用程序中的所有视图。 例如: 

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

与*/Dinners/编辑 / [id]* URL:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>下一步

它们和主控页提供了非常灵活的选项，您可以完全组织视图。 你将找到它们可以帮助你避免重复视图内容 / 编码，并使你的视图模板更易于读取和维护。

让我们现在重新考虑我们之前生成的列表方案，并启用可缩放的分页支持。

>[!div class="step-by-step"]
[上一页](use-viewdata-and-implement-viewmodel-classes.md)
[下一页](implement-efficient-data-paging.md)
