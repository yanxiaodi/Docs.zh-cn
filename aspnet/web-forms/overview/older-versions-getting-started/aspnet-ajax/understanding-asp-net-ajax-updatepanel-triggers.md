---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: "了解 ASP.NET AJAX UpdatePanel 触发器 |Microsoft 文档"
author: scottcate
description: "当使用 Visual Studio 中的标记编辑器，你可能注意到 （从 IntelliSense) 有两个 UpdatePanel 控件的子元素。 疑问词之一..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2008
ms.topic: article
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: 1338ef0763d9bfab451bc30cafa39f715200153d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>了解 ASP.NET AJAX UpdatePanel 触发器
====================
通过[Scott 类别](https://github.com/scottcate)

[下载 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> 当使用 Visual Studio 中的标记编辑器，你可能注意到 （从 IntelliSense) 有两个 UpdatePanel 控件的子元素。 其中之一是触发器元素，它指定页 （或用户控件，如果你正在使用它） 上的控件将会触发 UpdatePanel 控件元素驻留在其中部分呈现器。


## <a name="introduction"></a>介绍

Microsoft ASP.NET 技术将的面向对象及事件驱动的编程模型，并将它具有已编译代码的好处结合在一起。 但是，其服务器端处理模型有很多这样的可以通过 Microsoft ASP.NET 3.5 AJAX Extensions 中包含的新功能进行寻址的技术中的固有的几个缺点。 这些扩展使许多新的丰富的客户端功能，包括部分呈现页，而无需完整的页面刷新，能够通过 （包括 ASP.NET 分析 API） 的客户端脚本和广泛的客户端 API 访问 Web 服务用于镜像许多 ASP.NET 服务器端控件集中所示的控件方案。

本白皮书检查 ASP.NET AJAX 的 XML 触发器功能`UpdatePanel`组件。 XML 触发器使精细控制可能会导致部分呈现特定 UpdatePanel 控件的组件。

此白皮书基于 Beta 2 版本的.NET Framework 3.5 和 Visual Studio 2008。 ASP.NET AJAX Extensions 中，以前针对 ASP.NET 2.0 中，外接程序程序集现已集成到.NET Framework 基类库中。 此白皮书还假定你将使用 Visual Studio 2008 中，不 Visual Web Developer Express，并将提供根据 Visual Studio 的用户界面的演练 （尽管将完全兼容而不考虑代码列表开发环境）。

## <a name="triggers"></a>*触发器*

为给定 UpdatePanel，默认情况下，触发器自动包括调用回发，包括 （例如） TextBox 控件具有任何子控件其`AutoPostBack`属性设置为**true**。 但是，触发器还可包含使用标记; 以声明方式在完成此操作`<triggers>`UpdatePanel 控件声明的一部分。 尽管可以通过访问触发器`Triggers`集合属性，它建议 （例如，如果控件在设计时不可用） 注册任何部分呈现触发器在运行时通过使用`RegisterAsyncPostBackControl(Control)`方法ScriptManager 页，在对象`Page_Load`事件。 请记住，页是无状态，因此你应重新注册这些控件每次创建它们。

自动子触发器包含也可禁用 （以便创建回发的子控件不自动触发部分呈现） 通过设置`ChildrenAsTriggers`属性**false**。 这使得您最大的灵活性分配了哪些特定控件可能会调用页呈现，并建议，以便开发人员将选择加入以响应某个事件，而不是处理可能出现的任何事件。

请注意，当嵌套 UpdatePanel 控件时，当 UpdateMode 设置为**条件**，如果子 UpdatePanel 触发，但父无效，则仅将刷新 UpdatePanel 子。 但是，如果父 UpdatePanel 处于刷新，然后子 UpdatePanel 也将刷新。

## <a name="the-lttriggersgt-element"></a>*&lt;触发器&gt;元素*

当使用 Visual Studio 中的标记编辑器，你可能注意到 （从 IntelliSense) 有两个子元素的`UpdatePanel`控件。 最常见的元素是`<ContentTemplate>`元素，它实质上是封装将被更新面板的内容 （我们要为其启用部分呈现的内容）。 其他元素是`<Triggers>`元素，它指定页 （或用户控件，如果你正在使用它） 上的控件将会触发 UpdatePanel 控件中的部分呈现&lt;触发器&gt;元素驻留。

`<Triggers>`元素可以包含任意数量每个的两个子节点：`<asp:AsyncPostBackTrigger>`和`<asp:PostBackTrigger>`。 它们都接受两个属性，`ControlID`和`EventName`，并可以指定在当前的单元中封装任何控件 （例如，如果 UpdatePanel 控件包含在 Web 用户控件中，你不应尝试引用上的控件用户控件将在其驻留的页确定）。

`<asp:AsyncPostBackTrigger>`元素是特别有用，因为它可以指定目标中存在的子控件的任何事件*任何*UpdatePanel 封装，而不仅仅是在其下此触发器是子级 UpdatePanel 的单元中的控件. 因此，任何控件可用于触发分部页更新。

同样，`<asp:PostBackTrigger>`元素可用于的触发器部分页呈现，但其中一个需要完整往返到服务器。 此触发器元素还可以用于强制完整的页面呈现控件通常会否则触发部分页呈现器时 (例如，当`Button`中存在控件`<ContentTemplate>`UpdatePanel 控件元素)。 同样，PostBackTrigger 元素可以指定是封装的当前单元中任何 UpdatePanel 控件的子级的任何控件。

## <a name="lttriggersgt-element-reference"></a>*&lt;触发器&gt;元素引用*

*标记后代：*

| **标记** | **描述** |
| --- | --- |
| &lt;asp: AsyncPostBackTrigger&gt; | 指定的控件和将会包含此触发器引用 UpdatePanel 导致分部页更新的事件。 |
| &lt;asp: PostBackTrigger&gt; | 指定的控件和将导致整页更新 （完整的页面刷新） 的事件。 此标记可用于强制完全刷新时控件否则会触发部分呈现。 |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*演练： 跨 UpdatePanel 触发器*

1. 创建新的 ASP.NET 页的设置以启用部分呈现一个 ScriptManager 对象。 将两个 UpdatePanels 添加到此页-在第一个，包括一个标签控件 (Label1) 和两个按钮控件 （Button1 和 Button2）。 应该在 Button1 单击此项可同时更新和 Button2 应该显示单击此项可更新，或沿这些行的内容。 在第二个 UpdatePanel，包括仅的标签控件 (Label2)，但将其前景色属性设置为默认值对其进行区分以外的项目。
2. 设置到这两个 UpdatePanel 标记的 UpdateMode 属性**条件**。

**列表 1: default.aspx 标记：** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. 在 Button1 单击事件处理程序，设置 Label1.Text 和 Label2.Text 某事时间相关 （例如 DateTime.Now.ToLongTimeString())。 对于 Button2 的单击事件处理程序，设置为依赖于时间值的仅 Label1.Text。

**列出 2： 代码隐藏文件 default.aspx.cs 中 （剪裁）：** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. 按 F5 生成并运行项目。 请注意，当你单击更新同时面板，这两个标签更改文本;但是，当你单击更新此面板，仅 Label1 更新。


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([单击以查看实际尺寸的图像](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*揭秘*

利用我们刚构建的示例，我们可以看看 ASP.NET AJAX 正在做什么以及我们 UpdatePanel 跨面板触发器的工作方式。 若要执行此操作，我们会使用生成的页面源文件 HTML，以及使用它，调用 FireBug-Mozilla Firefox 扩展我们可以轻松地检查 AJAX 回发。 我们还将使用通过 Lutz Roeder.NET 反射器工具。 这两种工具，请随意联机，并可以使用 internet 搜索找到。

对页源代码的检查显示不正常; 几乎任何内容UpdatePanel 控件呈现为`<div>`容器以及我们可以看到了脚本资源包括提供通过`<asp:ScriptManager>`。 此外，还存在一些新特定于 AJAX 调用到 PageRequestManager 的内部 AJAX 客户端脚本库。 最后，我们看到两个 UpdatePanel 容器-一个具有呈现`<input>`带两个按钮`<asp:Label>`控件呈现为`<span>`容器。 （如果您检查的 DOM 树中 FireBug，你将注意到标签灰显以表示它们不能发出可见内容）。

单击更新此面板按钮，并注意顶部 UpdatePanel 将更新与当前服务器的时间。 因此，你可以检查请求，请在 FireBug，选择控制台选项卡。 首先检查 POST 请求参数：


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([单击以查看实际尺寸的图像](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


请注意，UpdatePanel 表明对服务器端 AJAX 代码精确的控制树触发通过 ScriptManager1 参数：`Button1`的`UpdatePanel1`控件。 现在，单击更新两个面板按钮。 然后，检查响应，我们请参阅竖线分隔的一系列变量字符串; 中的设置具体而言，我们看到顶部 UpdatePanel， `UpdatePanel1`，已发送到浏览器其 HTML 整个。 AJAX 客户端脚本库替换 UpdatePanel 的原始 HTML 内容使用新的内容通过`.innerHTML`属性，因此服务器从服务器以 HTML 的形式发送已更改的内容。

现在，单击更新两个面板按钮，并检查从服务器的结果。 结果将是非常类似-这两个 UpdatePanels 从服务器接收新的 html 代码。 与以前的回调中，将发送其他页面状态。

我们可以看到，因为没有特殊的代码用于执行 AJAX 回发，则 AJAX 客户端脚本库就能够截获无需任何其他代码的窗体回发。 服务器控件自动利用 JavaScript，以便它们不会自动提交表单-ASP.NET 自动插入代码窗体验证和状态，主要通过自动脚本资源包含，PostBackOptions 类和 ClientScriptManager 类。

例如，假设复选框控件，则检查在.NET 反射器类反汇编。 为此，请确保你 System.Web 程序集处于打开状态，并导航到`System.Web.UI.WebControls.CheckBox`类中，打开`RenderInputTag`方法。 检查的条件查找`AutoPostBack`属性：


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([单击以查看实际尺寸的图像](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


当在上启用自动回发`CheckBox`控制 （通过有些属性是否均为 true），所产生的`<input>`标记因此呈现与 ASP.NET 事件处理中的脚本其`onclick`属性。 窗体的提交，然后，截获允许 ASP.NET AJAX，若要插入到页 nonintrusively，有助于避免任何重大更改，可能出现的使用可能不精确的字符串替换的可能性。 此外，这使*任何*自定义 ASP.NET 控件，若要利用的功能的 ASP.NET AJAX 无需任何其他代码以支持其使用 UpdatePanel 容器中的。

`<triggers>`功能对应于 PageRequestManager 调用中初始化的值\_updateControls （请注意 ASP.NET AJAX 客户端脚本库利用约定，该方法、 事件和开始的字段名称下划线开头标记为内部，而不适合在库自身之外的使用）。 通过该对话框，我们可以看到哪些控件旨在导致 AJAX 回发。

例如，让我们添加到页中，完全，离开 UpdatePanels 之外的一个控件并离开 UpdatePanel 中的两个其他控件。 我们将添加内上部 UpdatePanel，复选框控件拖 DropDownList 包含列表中定义的颜色数。 下面是新的标记：

**列出 3： 新的标记**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

而以下是新的代码隐藏：

**列出 4： 代码隐藏**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

此页背后的理念是，下拉列表中选择一个三种颜色以显示第二个标签、 复选框确定是否显示为粗体，又标签显示日期，以及时间。 复选框不应导致 AJAX 更新，但应下拉列表，即使不将其存放在 UpdatePanel 也是如此。


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([单击以查看实际尺寸的图像](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


很明显上述的屏幕截图中，最新的按钮被单击时右侧的按钮更新此面板中，更新前的时间独立于的底部时间。 日期也关闭了点击，之间因为日期是在底部标签中可见。 最后需要注意的是底部标签的颜色： 它更新的时间比标签的文本，其演示了控件状态很重要，并且用户希望它将保留通过 AJAX 回发。 *但是*，不更新的时间。 时间已自动重填的持久性通过\_\_控件在服务器上重新呈现时，ASP.NET 运行时被解释的页面的视图状态字段。 ASP.NET AJAX 服务器代码不能识别控件的方法在其中更改状态;它只需重新填充从视图状态，然后运行相应的事件。

它应被指出，但是，，具有我初始化页内的时间\_负载事件时间将具有已正确增加。 因此，开发人员应应谨慎，相应的代码运行期间相应的事件处理程序，并避免使用页\_加载时控件的事件处理程序将适用。

## <a name="summary"></a>摘要

ASP.NET AJAX 扩展 UpdatePanel 控件是通用的并且可以利用大量用于标识应将导致其要更新的控件事件的方法。 它支持正由子控件，自动更新，但还可以响应在其他位置的页上的控件事件。

若要减少潜在的服务器处理负载，建议`ChildrenAsTriggers`UpdatePanel 属性设置为`false`，事件会选择到而不是默认情况下包括和。 此外可防止不需要的任何事件造成可能不需要的影响，包括验证和更改输入字段。 这些类型的 bug 可能很难隔离，因为页更新以透明方式对用户来说，并且可能的原因可能因此不会即时显现内容。

通过检查 ASP.NET AJAX 窗体的内部工作情况截获模型，我们无法确定它利用已经提供的 ASP.NET 框架。 在此情况下，它会保留与使用同一个框架，设计控件的最大兼容性，按最小方式侵入页写入任何其他 javascript。

## <a name="bio"></a>简介

Rob Paveza 是在 Terralever 高级.NET 应用程序开发人员 ([www.terralever.com](http://www.terralever.com))，在 Tempe，AZ.前导交互式市场营销公司 他可以达到在[ robpaveza@gmail.com ](mailto:robpaveza@gmail.com)，且其博客地址位于处[http://geekswithblogs.net/robp/](http://geekswithblogs.net/robp/)。

Scott 类别自 1997 年以来处理与 Microsoft Web 技术，并且是 myKB.com 总裁 ([www.myKB.com](http://www.myKB.com)) 其中他专注于编写 ASP.NET 基于侧重于知识库软件解决方案的应用程序。 可以通过在电子邮件联系 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或在其博客地址[ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[上一页](understanding-partial-page-updates-with-asp-net-ajax.md)
[下一页](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
