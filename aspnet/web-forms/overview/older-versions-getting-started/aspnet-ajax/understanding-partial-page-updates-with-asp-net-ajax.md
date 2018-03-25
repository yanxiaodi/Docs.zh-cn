---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: "了解分部页更新与 ASP.NET AJAX 一起 |Microsoft 文档"
author: scottcate
description: "可能是最明显的 ASP.NET AJAX Extensions 特色是能够执行部分或增量页更新操作而无需执行完整的回发到 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 1d8d3009df0a264e466d3f7decfb65978d8ae7a4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="understanding-partial-page-updates-with-aspnet-ajax"></a>了解分部页与 ASP.NET AJAX 一起更新
====================
通过[Scott 类别](https://github.com/scottcate)

[下载 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> 可能是最明显的 ASP.NET AJAX Extensions 特色是能够执行部分或增量页更新操作而无需执行完整的回发到服务器，与任何代码更改和最小标记更改。 优点是广泛 – 你多媒体内容 （例如 Adobe Flash 或 Windows Media） 的状态保持不变、 降低了带宽成本，，和客户端不会遇到通常与回发关联的闪烁。


## <a name="introduction"></a>介绍

Microsoft ASP.NET 技术将的面向对象及事件驱动的编程模型，并将它具有已编译代码的好处结合在一起。 但是，其服务器端处理模型已在技术中固有的几个缺点：

- 页更新需要一次往返行程到服务器，需要刷新页面。
- 往返操作，这不会保留任何影响生成的 Javascript 或其他客户端技术 （例如 Adobe Flash)
- 在回发时，过程以外 Microsoft Internet Explorer 浏览器不支持自动还原的滚动位置。 并且即使在 Internet Explorer 中，没有仍闪烁如刷新页面。
- 回发可能涉及大量带宽作为\_ \_VIEWSTATE 窗体字段可能会增长，尤其是在处理如 GridView 控件或中继器的控件。
- 没有用于访问 Web 服务通过 JavaScript 或其他客户端技术统一的模型。

输入 Microsoft 的 ASP.NET AJAX 扩展。 AJAX，代表**A**同步**J** avaScript **A** nd **X** ML，是用于提供增量页的集成的框架通过跨平台 JavaScript 更新组成，其中包含 Microsoft AJAX 框架中和名为 Microsoft AJAX 脚本库的脚本组件的服务器端代码。 使用 ASP.NET AJAX extensions 还提供用于访问 ASP.NET Web 服务通过 JavaScript 的跨平台支持。

本白皮书检查部分页面更新功能的 ASP.NET AJAX Extensions 中，其中包括 ScriptManager 组件、 UpdatePanel 控件和 UpdateProgress 控件，并考虑它们应或不应为的情况利用。

此白皮书基于 Visual Studio 2008 Beta 2 版本和.NET Framework 3.5，它将使用 ASP.NET AJAX Extensions 集成到基类库 （其中以前适用于 ASP.NET 2.0 中可用外接程序组件）。 此白皮书还假设你正在使用 Visual Studio 2008 和不 Visual Web Developer 速成版;引用的一些项目模板可能不能 Visual Web Developer Express 的用户。

## <a name="partial-page-updates"></a>分部页更新

可能是最明显的 ASP.NET AJAX Extensions 特色是能够执行部分或增量页更新操作而无需执行完整的回发到服务器，与任何代码更改和最小标记更改。 优点是广泛-你多媒体内容 （例如 Adobe Flash 或 Windows Media） 的状态保持不变、 降低了带宽成本，，和客户端不会遇到通常与回发关联的闪烁。

进行少量的更改到你的项目情况下，集成部分页呈现功能集成到 ASP.NET。

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>演练： 将部分呈现集成到现有项目


1. 在 Microsoft Visual Studio 2008 中，创建新的 ASP.NET 网站项目通过转到*文件* *- &gt;新建* *- &gt;网站*并从对话框中选择 ASP.NET Web 站点。 你可以将其任意命名，并可能到文件系统或到 Internet 信息服务 (IIS) 安装。
2. 你将看到基本 ASP.NET 标记的空默认页 (服务器端窗体和`@Page`指令)。 删除一个名为标签`Label1`和一个按钮调用`Button1`拖到窗体元素在页面。 你可能将其文本属性设置为希望的任何内容。
3. 在设计视图中，双击`Button1`以生成代码隐藏事件处理程序。 在此事件处理程序中，将设置`Label1.Text`到单击该按钮 ！ .

**列表 1: default.aspx 之前启用了部分呈现的标记**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**（剪裁） default.aspx.cs 中列出 2： 代码隐藏**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. 按 F5 以启动您的网站。 Visual Studio 将提示你添加 web.config 文件，以启用调试;这样做。 单击按钮时，请注意，刷新该页面，若要更改标签中的文本，并且如页重绘已简要闪烁。
2. 关闭浏览器窗口后，返回到 Visual Studio 和标记页面。 在 Visual Studio 工具箱中，向下滚动并查找标记为 AJAX Extensions 选项卡。 （如果你还没有此选项卡上因为您正在使用较旧版本的 AJAX 或地貌和城市等扩展，为更高版本在本白皮书中注册的 AJAX Extensions 工具箱项在本演练中，请参阅或使用 Windows Installer 可下载安装当前版本从网站）。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([单击以查看实际尺寸的图像](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))


1. *已知问题：*如果到已与 ASP.NET 2.0 AJAX Extensions 一起安装的 Visual Studio 2005 的计算机上安装 Visual Studio 2008，Visual Studio 2008 将导入 AJAX Extensions 工具箱项。 你可以确定这是否用例通过检查组件; 的工具提示它们应该显示版本 3.5.0.0。 如果他们说 2.0.0.0 版，然后导入你旧的工具箱项，并将需要手动使用 Visual Studio 中的选择工具箱项对话框中将它们导入。 你将无法添加通过设计器的版本 2 控件。

1. 之前`<asp:Label>`标记开始，创建线空格，然后双击工具箱中的 UpdatePanel 控件上。 请注意，新`@Register`指令是在页上，，该值指示应使用导 System.Web.UI 命名空间内的控件的顶部包括`asp:`前缀。
2. 拖动结束`</asp:UpdatePanel>`末尾的按钮元素中，添加标记以便元素包装的标签和按钮控件的格式正确。
3. 在起始`<asp:UpdatePanel>`标记中，开始打开新的标记。 请注意，IntelliSense 将提示两个选项。 在这种情况下，创建`<ContentTemplate>`标记。 请务必包装围绕标签和按钮此标记，以便标记的格式正确。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([单击以查看实际尺寸的图像](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))


1. 内的任何位置`<form>`元素，通过双击包含 ScriptManager 控件`ScriptManager`工具箱中的项。
2. 编辑`<asp:ScriptManager>`标记，以便它包含属性`EnablePartialRendering= true`。

**列出 3： 使用启用了部分呈现 default.aspx 的标记**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. 打开 web.config 文件。 请注意，Visual Studio 自动增加了对 System.Web.Extensions.dll 的编译引用。

1. 什么是 Visual Studio 2008 中的新增功能： 附带 ASP.NET 网站项目模板自动 web.config 包括到 ASP.NET AJAX Extensions 中，所有必要的引用并包括可配置信息的注释的部分取消注释以启用附加功能。 安装 ASP.NET 2.0 AJAX Extensions 时，visual Studio 2005 将具有类似的模板。 但是，在 Visual Studio 2008 中，使用 AJAX Extensions 是选择退出默认情况下 （即，它们在默认情况下，引用并且无法删除作为引用）。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([单击以查看实际尺寸的图像](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))


1. 按 F5 以启动你的网站。 请注意如何没有源代码更改均需要以支持部分呈现-仅标记已更改。

当启动你的网站时，你应看到现在启用，因为当你单击按钮有将无闪烁，也没有 （此示例没有演示的） 的页滚动位置中的任何更改了部分呈现。 如果你是在单击按钮后查看呈现的页面的源，它将确认事实上后备份后未发生-原始的标签文本仍是源标记的一部分并标签已更改通过 JavaScript。

Visual Studio 2008 似乎未附带一个支持 ASP.NET AJAX 的网站上的预定义模板。 但是，此类模板是在 Visual Studio 2005 内可用，如果已安装的 Visual Studio 2005 和 ASP.NET 2.0 AJAX Extensions。 因此，配置网站，然后使用 AJAX-Enabled 网站模板启动可能会更容易; 模板应包含完全配置的 web.config 文件 （支持所有 ASP.NET AJAX Extensions 中，包括 Web 服务的访问和 JSON 序列化的 JavaScript 对象表示法） 并且在主要的 Web 窗体页中包含一个 UpdatePanel 和 ContentTemplate，默认情况下。 启用的这一默认页的部分呈现非常简单，再次查看本演练的步骤 10 和放拖到页面上的控件。

## <a name="the-scriptmanager-control"></a>ScriptManager 控件

## <a name="scriptmanager-control-reference"></a>ScriptManager 控件引用

启用了标记的属性：

| **属性名称** | **类型** | **描述** |
| --- | --- | --- |
| AllowCustomErrors 重定向 | Bool | 指定是否使用 web.config 文件的自定义错误部分来处理错误。 |
| AsyncPostBackError 消息 | 字符串 | 获取或设置发送到客户端，如果引发错误的错误消息。 |
| AsyncPostBack 超时 | Int32 | 获取或设置默认的客户端应等待完成的异步请求的时间量。 |
| EnableScript 全球化 | Bool | 获取或设置是否启用脚本全球化。 |
| EnableScript 本地化 | Bool | 获取或设置是否启用脚本本地化。 |
| ScriptLoadTimeout | Int32 | 确定允许用于加载到客户端脚本的秒数 |
| ScriptMode | 枚举 （自动、 调试、 发布、 继承） | 获取或设置是否呈现脚本的发行版本 |
| ScriptPath | 字符串 | 获取或设置要发送到客户端的脚本文件的位置的根路径。 |

仅限代码的属性：

| **属性名称** | **类型** | **描述** |
| --- | --- | --- |
| 认证 | 认证管理器 | 获取有关将发送到客户端的 ASP.NET 身份验证服务代理的详细信息。 |
| IsDebuggingEnabled | Bool | 获取是否脚本并且代码调试已启用。 |
| IsInAsyncPostback | Bool | 获取当前的异步回发请求页是否。 |
| 页面 | 页面管理器 | 获取有关将发送到客户端的 ASP.NET 分析服务代理的详细信息。 |
| 脚本 | 集合&lt;脚本参考&gt; | 获取将发送到客户端的脚本引用的集合。 |
| 服务 | 集合&lt;服务引用&gt; | 获取将发送到客户端的 Web 服务代理引用的集合。 |
| SupportsPartialRendering | Bool | 获取指示当前的客户端支持部分呈现。 如果此属性返回**false**，则所有页面请求都将标准的回发。 |

公共代码的方法：

| **方法名称** | **类型** | **描述** |
| --- | --- | --- |
| SetFocus(string) | Void | 当请求已完成，请将客户端的焦点设置到特定控件。 |

标记后代：

| **标记** | **描述** |
| --- | --- |
| &lt;认证&gt; | 提供有关对 ASP.NET 身份验证服务代理的详细信息。 |
| &lt;页面&gt; | 对分析服务的 ASP.NET 提供有关代理的详细信息。 |
| &lt;脚本&gt; | 提供其他脚本引用。 |
| &lt;asp: ScriptReference&gt; | 表示特定的脚本引用。 |
| &lt;服务&gt; | 提供其他 Web 服务引用，这会生成的代理类。 |
| &lt;asp: ServiceReference&gt; | 表示特定的 Web 服务引用。 |

ScriptManager 控件是基本的核心，以便使用 ASP.NET AJAX Extensions。 它提供对 （包括大量客户端脚本类型系统） 的脚本库的访问、 支持部分呈现，并对其他 ASP.NET 服务 （例如身份验证和分析，但也有其他 Web 服务） 提供广泛支持。 ScriptManager 控件还提供了全球化和本地化的支持的客户端脚本。

## <a name="providing-alterative-and-supplemental-scripts"></a>提供种替代和补充脚本

在 Microsoft ASP.NET 2.0 AJAX Extensions 包括整个脚本代码，在这两个调试和发布版本作为资源嵌入中引用的程序集时，开发人员可自由将 ScriptManager 重定向到自定义的脚本文件，以及注册其他必要的脚本。

若要重写 （例如那些支持 Sys.WebForms 命名空间和自定义键入系统） 通常包含脚本的默认绑定，可以注册以进行`ResolveScriptReference`ScriptManager 类事件。 当调用此方法时，事件处理程序有机会将路径更改脚本所涉及的文件;脚本管理器将然后不同或自定义脚本的副本发送到客户端。

此外，脚本引用 (由表示`ScriptReference`类) 以编程方式或通过标记可以包含。 若要执行此操作，请以编程方式修改`ScriptManager.Scripts`集合，或包括`<asp:ScriptReference>`标记下`<Scripts>`标记，即第一级子 ScriptManager 控件。

## <a name="custom-error-handling-for-updatepanels"></a>自定义错误处理 UpdatePanels

尽管更新由触发器指定由 UpdatePanel 控件处理，对错误处理和自定义错误消息的支持的页面 ScriptManager 控件实例由处理。 这通过公开事件时， `AsyncPostBackError`，为页可可以然后提供自定义异常处理逻辑。

通过使用 AsyncPostBackError 事件，你可能指定`AsyncPostBackErrorMessage`属性，则可能导致一个警报框，以在回调完成时引发。

客户端自定义项也可能是而不是使用默认警报框;例如，你可能想要显示的自定义`<div>`元素而不是默认浏览器模式对话框。 在这种情况下，你可以处理客户端脚本中的错误：

**列出 5： 客户端脚本，以显示自定义错误**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

答案很简单，上述脚本注册一个回调与客户端 AJAX 运行时为时异步请求已完成。 然后，它将检查以确定是否报告了错误，并且如果是这样，处理的详细信息，最后，该值指示对运行时在自定义脚本已处理错误。

## <a name="globalization-and-localization-support"></a>全球化和本地化支持

ScriptManager 控件提供广泛支持用于本地化的脚本字符串和用户界面组件;但是，该主题是本白皮书的范围之外。 有关详细信息，请参阅白皮书，在 ASP.NET AJAX Extensions 中的全球化支持。

## <a name="the-updatepanel-control"></a>UpdatePanel 控件

## <a name="updatepanel-control-reference"></a>UpdatePanel 控件引用

启用了标记的属性：

| **属性名称** | **类型** | **描述** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | 指定子控件是否自动调用在回发时刷新。 |
| RenderMode | 枚举 （块、 内联） | 指定内容的方式将以可视方式显示。 |
| UpdateMode | 枚举 （始终、 条件） | 指定是否要在部分呈现器中始终刷新 UpdatePanel 或如果它才会刷新命中触发器时。 |

仅限代码的属性：

| **属性名称** | **类型** | **描述** |
| --- | --- | --- |
| IsInPartialRendering | bool | 获取是否 UpdatePanel 支持部分呈现为当前请求。 |
| ContentTemplate | ITemplate | 获取更新请求标记模板。 |
| ContentTemplateContainer | 控件 | 获取更新请求以编程方式的模板。 |
| 触发器 | UpdatePanel-TriggerCollection | 获取与当前 UpdatePanel 相关的触发器的列表。 |

公共代码的方法：

| **方法名称** | **类型** | **描述** |
| --- | --- | --- |
| Update （) | Void | 以编程方式更新指定的 UpdatePanel。 允许的服务器请求触发否则为存 UpdatePanel 部分呈现器。 |

标记后代：

| **标记** | **描述** |
| --- | --- |
| &lt;ContentTemplate&gt; | 指定要用于呈现部分呈现结果的标记。 子&lt;asp: UpdatePanel&gt;。 |
| &lt;触发器&gt; | 指定的集合 *n* 与更新此 UpdatePanel 关联的控件。 子&lt;asp: UpdatePanel&gt;。 |
| &lt;asp: AsyncPostBackTrigger&gt; | 指定触发器调用有给定 UpdatePanel 部分页呈现器。 这可能或可能不是作为问题 UpdatePanel 后代控件。 精确到事件名称。子&lt;触发器&gt;。 |
| &lt;asp: PostBackTrigger&gt; | 指定导致整个页面以刷新的控件。 这可能或可能不是作为问题 UpdatePanel 后代控件。 精确到对象。 子&lt;触发器&gt;。 |

`UpdatePanel`控件是分隔将参与了相应的 AJAX Extensions 部分呈现功能的服务器端内容控件。 没有为 UpdatePanel 控件可在页中，数没有限制，可以嵌套。 每个 UpdatePanel 是孤立的以便每个可以独立处理 （你可以在同一时间运行，为已呈现的页面的不同部分独立于页面的回发的两个 UpdatePanels）。

UpdatePanel 控制主要交易具有控制触发器-默认情况下，UpdatePanel 中包含任何控件`ContentTemplate`创建回发的 UpdatePanel 注册为触发器。 这意味着，UpdatePanel 可以处理与用户控件的默认数据绑定控件 （如 GridView)，而且可以在脚本中对它们进行编程。

默认情况下，当触发部分页呈现器时，将刷新该页上的所有 UpdatePanel 控件，UpdatePanel 控制定义此类操作的触发器。 例如，如果一个 UpdatePanel 定义按钮控件，并单击该按钮控件，默认情况下将会刷新该页面上的所有 UpdatePanel 控件。 这是因为，默认情况下， `UpdateMode` UpdatePanel 属性设置为`Always`。 或者，可能将 UpdateMode 属性设置为`Conditional`，这意味着如果达到特定触发器才会刷新 UpdatePanel。

## <a name="custom-control-notes"></a>自定义控件说明

UpdatePanel 可以添加到任何用户控件或自定义控件，则但是，这些控件在其是包含该页面还必须包含属性 EnablePartialRendering 设置为 ScriptManager 控件**true**。

一种方法在其中你可能考虑到这一点时使用 Web 自定义控件是重写受保护`CreateChildControls()`方法`CompositeControl`类。 通过这样做，可以插入控件的子级和外界之间 UpdatePanel 如果确定该页支持部分呈现;否则，只需到容器层的子控件`Control`实例。

## <a name="updatepanel-considerations"></a>UpdatePanel 注意事项

UpdatePanel 进行操作的黑色方框的东西包装 ASP.NET JavaScript XMLHttpRequest 的上下文中的回发。 但是，有显著的性能注意事项需要牢记，两个方面的行为及其速度。 若要了解如何 UpdatePanel 工作，以便你可以最决定其使用适合，你应该检查 AJAX exchange。 下面的示例使用了现有的站点和，Mozilla Firefox Firebug 扩展名 （Firebug 捕获 XMLHttpRequest 数据）。

请考虑具有的窗体，除了别的之外邮政编码文本框中，应该填充窗体或控件上的城市和状态字段。 此窗体最终收集成员身份信息，包括用户的名称、 地址和联系信息。 有许多设计注意事项，以考虑在内，根据特定项目的要求。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([单击以查看实际尺寸的图像](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([单击以查看实际尺寸的图像](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))


在此应用程序的原始迭代中，控件是生成合并整个用户注册数据，包括邮政编码、 城市和状态。 整个控件已内 UpdatePanel 自动换行和拖放到 Web 窗体。 当用户输入邮政编码时，UpdatePanel 检测到事件 （后端，通过指定触发器或通过使用 ChildrenAsTriggers 属性设置为 true 中的相应宽度事件）。 AJAX 文章的所有内 UpdatePanel，字段由 FireBug 捕获 （请参阅右侧的关系图）。

屏幕截图所示，传递从 UpdatePanel 中每个控件的值 （在这种情况下，它们是全部为空），以及视图状态字段。 所述，超过 9 kb 的数据时将发送，实际上只有五个字节的数据所需无法进行此特定的请求。 响应是更加臃肿:，总共 57 kb 发送到客户端，仅仅是为了更新文本字段和构造型字段。

它还可能感兴趣，若要查看 ASP.NET AJAX 更新演示文稿的方式。 UpdatePanel 的更新请求的响应部分显示在左侧; Firebug 控制台显示它是专门表述竖线分隔字符串是按客户端脚本，然后重新合并的页上。 具体而言，设置 ASP.NET AJAX *innerHTML*表示你 UpdatePanel 的客户端上的 HTML 元素的属性。 浏览器将重新生成 DOM，没有期间的轻微延迟，具体取决于需要处理的信息的量。

重新生成 DOM 触发更多问题的数：


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([单击以查看实际尺寸的图像](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))


- 如果 UpdatePanel 内的已设定焦点的 HTML 元素，则它将失去焦点。 因此，对于用户按下 Tab 键以退出邮政编码文本框中，其下一步的目标就已城市文本框。 但是，一旦 UpdatePanel 刷新显示，窗体将不再具有焦点，并且按 Tab 应开始突出显示的焦点元素 （例如链接）。
- 如果任何类型的自定义客户端脚本正在使用访问 DOM 元素，引用保留由函数可能会变得不起作用后部分的回发。

UpdatePanels 不应是全方位解决方案。 相反，它们提供快速解决方案对于某些情况下，包括原型制作小控件更新，并且对 ASP.NET 开发人员熟悉的.NET 对象模型，但以便小于与 DOM 一起使用可能提供熟悉的界面 有了一些可能会导致更好的性能，具体取决于应用程序方案的替代方法：

- 请考虑使用 PageMethods 和 JSON （JavaScript 对象表示法） 允许开发人员调用页上的静态方法，就像正在调用 web 服务调用。 无状态方法都是静态的因为是必需的;脚本调用方提供参数，并以异步方式返回的结果。
- 请考虑使用 Web 服务和 JSON，如果需要在多个位置中跨应用程序使用单个控件。 再次，这需要特殊工作很少，并以异步方式工作。

合并功能通过 Web 服务或页方法有缺点以及。 首先，最重要，ASP.NET 开发人员通常往往到用户控件 （.ascx 文件） 构建的功能的小组件。 不能在这些文件; 托管页方法它们必须承载于实际的.aspx 页类中。 Web 服务，同样，必须承载于.asmx 类中。 根据应用程序，此体系结构可能会违反单个责任原则在于单个组件的功能现在分布在两个或多个物理组件这可能造成很少或没有凝聚力等同值。

最后，如果应用程序需要使用 UpdatePanels，以下准则应帮助进行故障排除和维护。

- **嵌套 UpdatePanels 降至最低，不仅内 – 单元，但还跨个代码单元。** 例如上一个页，而该控件还包含 UpdatePanel，其中包含包含 UpdatePanel 的另一个控件，包装一个控件，, 具有 UpdatePanel 是跨单元嵌套。 这有助于确保让清除哪些元素应为刷新，并阻止对子 UpdatePanels 意外的刷新。
- **保留*ChildrenAsTriggers*属性设置为 false，并显式设置触发事件。** 利用`<Triggers>`集合是处理事件的得更清晰方法以及可能会阻止意外的行为，帮助进行维护任务和强制开发人员可以选择加入的事件。
- **可能的最小单位用于实现的功能。** 中所述的邮政编码服务，包装讨论仅最低将时间减少到的服务器、 总处理和客户端-服务器 exchange 的占用空间，提高性能。

## <a name="the-updateprogress-control"></a>UpdateProgress 控件

## <a name="updateprogress-control-reference"></a>UpdateProgress 控件引用

启用了标记的属性：

| **属性名称** | **类型** | **描述** |
| --- | --- | --- |
| AssociatedUpdate PanelID | 字符串 | 指定此 UpdateProgress 应报告 UpdatePanel 的 ID。 |
| DisplayAfter | Int | 在异步请求开始后显示此控件之前，请指定的超时以毫秒为单位。 |
| DynamicLayout | bool | 指定是否动态呈现进度。 |

标记后代：

| **标记** | **描述** |
| --- | --- |
| &lt;ProgressTemplate&gt; | 包含设置将与此控件显示的内容的控件模板。 |

UpdateProgress 控件使你的反馈，以便在执行必要的工作到传输到服务器时保留用户的相关度量值。 这可以帮助你知道，你要做的内容即使也可能不明显，特别是因为大多数用户习惯于刷新和看到状态栏突出显示的页的用户。

请注意，作为 UpdateProgress 控件可以出现的任何位置的页层次结构上。 但是，在部分的回发发自子 UpdatePanel （其中 UpdatePanel 嵌套在另一个 UpdatePanel） 的情况下，回发的触发器 UpdatePanel 将导致显示 UpdateProgress 模板为子子UpdatePanel 以及父 UpdatePanel。 但如果触发器为父 UpdatePanel 的直接子级，只有与父项关联的 UpdateProgress 模板将会显示。

## <a name="summary"></a>摘要

Microsoft ASP.NET AJAX 扩展是复杂的产品旨在以帮助使 web 内容更易于访问，从而提供对 web 应用程序提供更丰富的用户体验。 作为 ASP.NET AJAX Extensions 中，分部页呈现控件的一部分包括 ScriptManager、 UpdatePanel 和 UpdateProgress 控件是一些工具包的最明显的组件。

ScriptManager 组件集成有关扩展，客户端 JavaScript 的设置以及启用与一起使用很少的开发投资的各种服务器和客户端组件。

UpdatePanel 控件是明显的幻框-UpdatePanel 中的标记可以使服务器端代码隐藏文件，并不会触发刷新页面。 UpdatePanel 控件可以嵌套，并且可以依赖于其他 UpdatePanels 中的控件。 默认情况下，UpdatePanels 尽管此功能可以精细优化，以声明方式或以编程方式处理由其后代的控件，调用任何回发。

当使用 UpdatePanel 控件，开发人员应注意可能出现的性能影响。 潜在的备用方法包括 web 服务和页方法，但应考虑应用程序的设计。

UpdateProgress 控件允许用户知道她或他不会被忽略，并幕后的请求进行的操作时页否则不采取任何措施来响应用户输入。 它还包括中止部分呈现结果的能力。

在一起，这些工具有助于通过在服务器工作不太明显给用户，不会中断工作流创建丰富且无缝用户体验。

## <a name="bio"></a>简介

Scott 类别自 1997 年以来处理与 Microsoft Web 技术，并且是 myKB.com 总裁 ([www.myKB.com](http://www.myKB.com)) 其中他专注于编写 ASP.NET 基于侧重于知识库软件解决方案的应用程序。 可以通过在电子邮件联系 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或在其博客地址[ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[下一篇](understanding-asp-net-ajax-updatepanel-triggers.md)
