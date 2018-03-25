---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: "使用 Page Inspector 在 ASP.NET 网页中的 Visual Studio 2012 窗体 |Microsoft 文档"
author: rick-anderson
description: "Visual Studio 2012 的 Page Inspector 是 web 开发工具，具备集成浏览器。 选择任何元素中的集成浏览器和 Page Inspector..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: ca8a3c194577766e56d0604323fef567d539316c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>使用 Page Inspector 在 ASP.NET Web 窗体中的 Visual Studio 2012
====================
通过 Tim Ammann

> Visual Studio 2012 的 Page Inspector 是 web 开发工具，具备集成浏览器。 在集成浏览器中，选择任何元素，并立即 Page Inspector 突出显示，元素的源和 CSS。 可以浏览应用程序中的任何页，快速找到呈现标记源和使用权限在 Visual Studio 环境中的浏览器工具。
> 
> 此教程 shwos 如何启用检查模式下和随后快速查找和编辑 CSS 规则和你的 web 项目中的文本。 本教程将使用 Web 窗体应用程序项目中，但你也可以通过以下方式使用 Page Inspector 适用于网站项目和[MVC](https://go.microsoft.com/?linkid=9802002)应用程序。
> 
> 本教程包含以下部分：
> 
> [先决条件](#_1_prerequisites)
> 
> [创建 Web 应用程序](#_2_creating_a)
> 
> [使用 Page Inspector 查看该应用程序](#_3_using_page)
> 
> [启用检查模式](#_4_inspection_mode)
> 
> [使用 Page Inspector 对标记进行的更改](#_5_using_page)
> 
> [检查模式下和 HTML 窗口](#_6_inspection_mode)
> 
> [在样式窗口中预览 CSS 更改](#_7_previewing_css)
> 
> [CSS 自动同步](#css_auto_sync)
> 
> [使用 CSS 颜色选取器](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>系统必备

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11)或[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。

> [!NOTE]
> 若要获取 Page Inspector 的最新版本，请使用[Web 平台安装程序](https://go.microsoft.com/fwlink/?LinkId=255386)要安装 Azure SDK for.NET 2.0。


Page Inspector 绑定了 Microsoft Web 开发人员工具。 最新版本是 1.3。 若要检查的版本，运行 Visual Studio 并选择**有关 Microsoft Visual Studio**从**帮助**菜单。

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>创建 Web 应用程序

首先，你将创建的 web 应用程序将使用与 Page Inspector。 在 Visual Studio 中，选择**文件** &gt; **新项目**。 在左侧，展开**Visual C#**，选择**Web**，然后选择**ASP.NET Web 窗体应用程序**。

![新的 Web 窗体应用程序](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

单击 **“确定”**。

应用程序将在中打开**源**视图。

![在源视图中的新的 Web 窗体应用程序](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

现在，你拥有的应用程序与工作，你可以使用 Page Inspector 检查并对其进行修改。

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>使用 Page Inspector 查看该应用程序

接下来，你将查看 Page Inspector 的应用程序。 在**解决方案资源管理器**，右击该项目，然后选择**在 Page Inspector 中的查看**。

![在 Page Inspector 的视图](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

默认情况下，Page Inspector 首次启动时它是作为窄的窗口停靠在 Visual Studio 环境的左侧。 将其停靠在左侧，并将其设置宽度都能轻松地为你，或将它工具区域之一中停靠在顶部、 底部或右保留：

![Page Inspector 停靠位置](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

如果你取消停靠该页面检查器窗口，则可以它放外部 Visual Studio 中，或者甚至上第二个监视器如果你有一个。 但是，按 ALT + TAB Page Inspector 和 Visual Studio 之间的顺序在 Page Inspector 窗口停靠时，请转到**工具** &gt; **选项** &gt; **环境** &gt; **选项卡和窗口**，并在列表视图**选项卡也**，清除复选框调用**浮动工具窗口始终保持的顶部主窗口**:

![清除到 Visual Studio 和停靠的 Page Inspector 窗口之间的 ALT + 选项卡的浮动工具 windows 复选框](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

页面检查器窗口的顶部窗格会在浏览器窗口显示当前页。 底部窗格中显示的页中的 HTML 标记在左侧，并让你右侧某些选项卡检查页的不同方面。 在底部窗格中，类似于[F12 开发人员工具](https://msdn.microsoft.com/ie/aa740478)在 Internet Explorer 中。 （但是，与不同的开发人员工具，你可以使用 Page Inspector 在 Visual Studio 中的权限。）

![Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

在本教程中，你将使用 Page Inspector 浏览器窗格中，与**HTML**和**样式**选项卡来帮助你快速导航，并对应用程序进行更改。

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>启用检查模式

接下来，你将看到 Page Inspector 的检查模式下的工作原理。 在 Page Inspector 窗口中，单击**检查**按钮。

![检查元素](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

若要查看在操作中的检查模式下，请将鼠标移 page 在 Page Inspector 浏览器窗口中的不同部分。 一样，鼠标指针更改为一个大型的加号，并突出显示下的元素：

![将鼠标悬停在 div.content 包装](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

移动鼠标指针时，请注意，

- 中的内容**源**查看更改，以显示对应于页面上所选元素的标记。 突出显示相关的标记。 如果源是在另一个文件，该文件将在源视图中打开与突出显示的相关标记中。

- 中显示的标记**HTML**在 Page Inspector 的选项卡，还更改以与在页面上所选元素相对应。 在**HTML**选项卡上，列出了相关的标记。

- **样式**选项卡显示与当前所选内容相关的 CSS 规则。

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>使用 Page Inspector 对标记进行的更改

现在，你将看到如何使用 Page Inspector 以查找并对标记或其位置可能不会即时显现的文本进行更改。

Page Inspector 置于检查模式下，然后向下滚动到主页页面的底部。

一旦你输入的页脚区域，Page Inspector 将打开*Site.Master*中的布局文件**源**另右侧的临时选项卡中的视图选项卡，并突出显示的主要部分页上，您已选择。 这向你展示如何 Page Inspector 可以查找和标识可能实际上来自于与最初打开另一个文件的页面上显示的内容。

![在检查模式下的页脚突出显示](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

在 Page Inspector 浏览器窗口中，将鼠标指针移到行的版权<a id="a"></a>请注意。

在*Site.Master*突出显示页面上，相应的行。

![页脚版权所有行突出显示](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

中的行的末尾添加一些文本*Site.Master*文件。

&lt;p&gt;&amp;复制;&lt;%: DateTime.Now.Year %&gt; -我的 ASP.NET 应用程序岩石 ！&lt;/ p&gt;

现在，按 Ctrl + Alt + Enter 或单击更新条以查看在 Page Inspector 浏览器窗口的结果。

![我的 ASP.NET 应用程序岩石 ！](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

你可能具有认为页脚是上*Default.aspx*页上，但它令牌是在母版布局页中，并为您的 Page Inspector 找到它。

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>检查模式下和 HTML 窗口

接下来，您可以快速了解一下 HTML 窗口以及它如何为你映射元素。

Page Inspector 置于检查模式。

![检查元素](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

单击显示"your logo here"页的顶部。 要检查的详细信息，因此在浏览器窗口显示无法再更改您将鼠标指针移动中的特定元素。

现在移动鼠标指针指向**HTML**窗口。 移动鼠标指针时，Page Inspector 概述了中的元素**HTML**窗口并突出显示的浏览器窗口中的相应元素。

![HTML 窗口](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

如之前，Page Inspector 将打开*Site.Master*为你的临时选项卡中的文件。单击 Site.Master 选项卡，然后以突出显示相应的标记&lt;标头&gt;部分：

![突出显示的标记](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>在样式窗口中预览 CSS 更改

接下来，你将看到如何使用 Page Inspector**样式**窗口来预览对 CSS 的更改。

单击**检查**Page Inspector 置于检查模式下的按钮。

在 Page Inspector 浏览器窗口中，将鼠标指针移之前的"主页"部分**div.content 包装**标签出现。

![将鼠标悬停在元素](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

一次，在 div.content 包装部分内单击，然后移动鼠标指针指向**样式**窗口。 下.featured.content 包装类选择器中，清除并选择背景色属性的复选框。

![清除背景色](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

请注意如何更改预览立即在 Page Inspector 浏览器窗口。

再次选中的复选框，然后双击属性值并将其更改为`red`。 更改立即显示：

![红色背景色](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

**样式**窗口使它更容易测试和预览 CSS 更改之前将更改提交到样式表本身。

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>CSS 自动同步

> [!NOTE]
> 此功能需要版本 1.3 的 Page Inspector。


CSS 自动同步功能，可直接编辑 CSS 文件，并查看立即在 Page Inspector 浏览器中的更改。

单击**检查**Page Inspector 置于检查模式。

在 Page Inspector 浏览器中，将鼠标指针移之前的"主页"部分**div.content 包装**标签出现。 单击一次以选择此元素。

**Syles**窗口将显示所有此元素的 CSS 规则。 向下滚动到查找.featured.content 包装器类选择器。 单击"包装器.featured.content"。 Page Inspector 将打开定义此样式 (Site.css) 并突出显示相应的 CSS 样式的 CSS 文件。

![CSS 文件](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

现在更改的值`background-color`为"红色"。 在 Page Inspector 浏览器中，更改会立即出现。

![Page Inspector 浏览器](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>使用 CSS 颜色选取器

接下来，你将了解如何使用 Page Inspector 快速查找和更改的 CSS，在默认应用程序中的突出显示文本。 在此示例中，您已决定你不喜欢的蓝色突出显示并想要将其更改为另一种颜色。

单击**检查**按钮。

![检查元素](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

在 Page Inspector 浏览器窗口中，将鼠标指针移突出显示"视频、 教程和示例"，以便 CSS"标记"标签将显示文本。

![将鼠标悬停在标记元素](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

单击以选中它的文本。 底部将显示相应的 CSS 标记选择器**样式**窗口。

![样式窗口中的标记选择器](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

单击标记选择器。 这将打开*Site.css* web 应用程序文件。 单击 Site.css 选项卡，并突出显示相应的 CSS 选择器：

![标记样式表中的选择器](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

选择并删除在行的背景色属性。

您现在将使用新的 Visual Studio 2012 CSS 颜色选取器选择的新颜色**标记**背景色属性。

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>使用 Visual Studio 2012 CSS 颜色选取器

Visual Studio 2012 中的 CSS 编辑器具有可以轻松地选择和插入颜色颜色选取器。 它具有一个简单的颜色栏和提供更好的控制"pop 停机"选取器。

颜色选取器包括一个标准的调色板，支持标准颜色名称、 哈希代码、 RGB、 RGBA、 HSL 和 HSLA 颜色，并维护你已在文档中最近使用的颜色的列表。

在背景色属性所在的行中，键入"bc"，然后按向下箭头一次。

当你键入连字符分隔的属性，如"背景色"中的每个单词的第一个字符时，IntelliSense 将筛选你可以显示仅相匹配的属性的列表：

![Intellisense 筛选值](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

现在，键入一个冒号。 执行操作时，将插入完整的背景色属性名称。 类型 **#** 或**rgb (**，和颜色选取器栏将显示：

![CSS 颜色选取器栏](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

若要查看的颜色选取器栏的工作原理，请单击其颜色与将鼠标指针或按向下箭头键，然后使用左和向右箭头键遍历颜色。 当访问一种颜色时，预览对应的背景色属性值：

![预览的背景色属性值](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

此时，无法按 enter 键以选择值，然后输入分号 （;） 以完成 CSS 条目。 现在，继续到下一节，以便你可以查看颜色选取器 pop 向下的工作原理。

#### <a name="using-the-color-picker-pop-down"></a>使用颜色选取器 Pop 列表

当在颜色栏不具有完全你正在寻找的颜色时，可以使用颜色选取器 pop。

若要打开它，请单击颜色栏右侧的双精度的 v 形图标或按键盘上一次或两次的向下箭头。

![CSS 颜色选取器 Pop 列表](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

单击右侧的竖线中的某种颜色。 这将在主窗口中显示该颜色的渐变。 通过按 Enter，直接从垂直条中选择一种颜色，或单击以选择而且更精确的主窗口中的任何点。

在你想要使用的计算机屏幕上是否有一种颜色 （它不必是在 Visual Studio 用户界面内），你可以通过使用取色器工具右下角捕获其值。

你还可以更改通过移动滑块的颜色选取器底部的一种颜色的不透明度。 这样做的更改颜色为 RGBA 值的值，因为 RGBA 格式可以代表不透明度。

您已选择一种颜色后，按 Enter，，然后键入分号完成中的背景色条目*Site.css*文件。

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>页面检查器更新条

Page Inspector 将立即检测到更改*Site.css*文件 （或应用程序中的任何文件） 和更新栏中显示警报。

![更新条](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

若要保存所有文件并刷新 Page Inspector 浏览器，请按 Ctrl + Alt + Enter 或单击更新条。 中的突出显示颜色的更改显示在浏览器：

![突出显示颜色更改](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>请注意你方便地刷新 Page Inspector 浏览器直接从 Visual Studio 环境中。 而不外部浏览器使用 Page Inspector 可让你保持在编辑器中，当你开发 web 应用程序时。

## <a name="see-also"></a>请参阅

[引入 Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) （频道 9 视频）

[Page Inspector 错误信息](https://go.microsoft.com/?linkid=9813062)(MSDN)
