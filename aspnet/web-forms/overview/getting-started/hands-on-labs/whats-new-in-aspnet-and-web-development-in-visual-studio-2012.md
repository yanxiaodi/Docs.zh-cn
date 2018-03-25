---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: "ASP.NET 和 Visual Studio 2012 中的 Web 开发的新增功能 |Microsoft 文档"
author: rick-anderson
description: "Visual Studio 的新版本引入了一些增强功能集中于改进的体验和性能，使用 Web 技术时..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: f0818cce2a82ede80556b3471cec9d965c3e987f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>ASP.NET 和 Visual Studio 2012 中的 Web 开发的新增功能
====================
通过[Web 营地团队](https://twitter.com/webcamps)

> Visual Studio 的新版本引入了一些增强功能集中于改进的体验和性能，当使用 Web 技术。 CSS、 JavaScript 和 HTML 的 visual Studio 编辑器已彻底改观，以包括许多最需代码指导，例如智能感知和自动缩进。 至于性能方面，如内置功能，以便轻松地将减少页加载时间，则现已集成绑定和缩减。
> 
> Visual Studio，可使用最新的网站技术。 可以使用跨浏览器 CSS3 片段以确保你的站点工作不考虑客户端平台，同时还利用新的 HTML5 元素和功能。
> 
> 编写和分析 JavaScript 代码应为此 Visual Studio 版本，更容易。 智能感知列表集成的 XML 文档和导航功能现可用于 JavaScript 代码。 你现在可以在您能够轻松利用的 JavaScript 目录。 此外，你可以检查 ECMAScript5 符合你的脚本，并在早期阶段检测语法错误。
> 
> 上次值得一提的但此 Visual Studio 版本可实现内置绑定和缩减。 你的脚本文件和样式表将打包和压缩，以便站点执行速度更快。
> 
> 此实验室将引导你完成的增强功能和前面所述通过将细微的更改应用于源文件夹中提供的示例 Web 应用程序的新功能。
> 
> 在 Web 营地培训工具包中，在包括所有的示例代码和代码段[https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)。


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>目标

在此动手实验中，你将了解如何：

- 在 CSS 编辑器中使用的新功能和改进
- 在 HTML 编辑器中使用的新功能和改进
- 在 JavaScript 编辑器中使用的新功能和改进
- 配置和使用绑定和缩减

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>系统必备

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更高 (读取[附录 A](#AppendixA)有关如何安装它的说明)。
- [Windows PowerShell](https://support.microsoft.com/kb/968930/) （适用于安装程序脚本-已安装在 Windows 8 和 Windows Server 2008 R2）
- [Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) -或 HTML5 符合浏览器

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>练习

此动手实验包括以下练习：

1. [练习 1： 什么是 CSS 编辑器中的新增功能](#Exercise1)
2. [练习 2： 什么是新的 HTML 编辑器](#Exercise2)
3. [练习 3： 什么是 JavaScript 编辑器中的新增功能](#Exercise3)
4. [练习 4： 捆绑和缩减](#Exercise4)

估计时间来完成该实验： **60 分钟**。

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>练习 1： 什么是 CSS 编辑器中的新增功能

Web 开发人员应熟悉许多与 CSS 编辑相关的困难。 CSS 样式的最大问题之一是跨浏览器兼容性。 经常会出现，将样式应用到你的站点之后, 它看起来不同，如果你发现你在其他浏览器或设备中打开它。 因此，你可能在修复这些 visual 的问题，以实现，当你最后使其在浏览器，它会拆分中其他花费相当长的时间。

Visual Studio 现在包含功能，可帮助开发人员访问、 工作和有效地组织 CSS 样式表。 在本练习中，将满足的新功能为有效的组织和版本，以及跨浏览器兼容性 CSS3 代码段。

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>任务 1-新的编辑器功能

在此任务中，你将发现的新功能的 CSS 编辑器中。 此新的编辑器将帮助你提高工作效率，通过利用新的智能缩进、 改进的代码注释和增强的智能感知列表。

1. 启动**Visual Studio**并打开**WhatsNewASPNET.sln**解决方案位于**Source\WhatsNewASPNET**这个实验室的文件夹。
2. 在解决方案资源管理器，打开**Site.css**文件位于**样式**文件夹。 请确保**文本编辑器**工具是在工具栏上可见。 为此，请选择**视图** | **工具栏**菜单选项，并检查**文本编辑器**选项。 你将注意，此新版本，因为**注释**按钮 (![comment 按钮](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png)) 和**取消注释**按钮 (![取消注释按钮](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) 也已启用了 CSS 编辑器。

    ![启用编辑器和 CSS 工具](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "启用编辑器和 CSS 工具")

    *启用编辑器和 CSS 工具*
3. 滚动代码并选择任意 CSS 类定义。 单击**注释**(![comment 按钮](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png)) 按钮以注释所选的行。 然后，单击**取消注释**(![取消注释按钮](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png)) 按钮以还原所做的更改。
4. 单击**折叠**(![折叠](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png)) 和**展开**(![展开](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png)) 按钮位于文本的左边距。 请注意，您现在可以隐藏不使用具有更简洁的视图的样式。

    ![折叠 CSS 类](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "折叠 CSS 类")

    *折叠 CSS 类*
5. 请确保启用了智能缩进功能。 选择**工具** | **选项**菜单选项，然后选择**文本编辑器** | **CSS**  | **格式**屏幕的左窗格中的页。 检查**分层缩进**选项。

    ![启用分层缩进](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "启用分层缩进")

    *启用分层缩进*
6. 找到的主类定义 (.main) 并追加到的 div 元素的样式。 你将注意到代码将自动，对齐帮助用户查找父级类一目了然。

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![在 CSS 层次结构的对齐方式](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "CSS 中的层次结构对齐方式")

    *在 CSS 层次结构的对齐方式*
7. 内部**.main div**类，在末尾找到光标**边框： 0px;**按**Enter**以显示 IntelliSense 列表。 开始键入**顶部**，并注意你进行键入筛选列表的方式。 该列表将显示包含的元素**顶部**在 word 的任何部分 (在以前版本的 Visual Studio 中，对列表进行筛选的项，*开始*一词的)。

    ![在 CSS 中的 IntelliSense 增强功能](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "CSS 中的 IntelliSense 增强功能")

    *在 CSS 中的 IntelliSense 增强功能*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>任务 2-颜色选取器

在此任务中，你会发现新的 CSS 颜色选取器集成到 Visual Studio IntelliSense。

1. 在**Site.css，**找到标头类定义 (.header) 并将光标放在旁边**背景色**特性，之间&quot;:&quot;和&quot; #&quot;上的代码行的字符**。**

    ![定位光标](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "定位光标")

    *定位游标*
2. 删除**冒号**（:） 并将其再次以显示颜色选取器写入。 请注意，你将看到的第一个颜色是你的站点的最常见颜色。 如果你单击白色的颜色，其 HTML 颜色代码 (#fff) 会替换样式表中的当前颜色代码。

    ![颜色选取器](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "颜色选取器")

    *颜色选取器*
3. 按**展开**(![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) 以显示的颜色渐变，然后拖动要选择不同的颜色的渐变光标颜色选取器上的按钮。 之后，单击**取色器**按钮，然后从屏幕中选择任意颜色。 请注意，背景颜色值动态更改移动光标时。

    ![颜色选取器渐变](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "颜色选取器渐变")

    *颜色选取器渐变*
4. 在**不透明度**滑块，将选择器移到的栏，以减少不透明度的中心。 请注意，背景颜色值现在变为其刻度 RGBA。

    ![颜色选取器不透明度](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "颜色选取器不透明度")

    *颜色选取器不透明度*

    > [!NOTE]
    > CSS3 中的 RGBA （红色、 绿色，蓝色、 Alpha） 颜色定义可以定义单个项的颜色不透明度值。 与不同**不透明度-**类似的 CSS 属性 **-**  RGBA 颜色也是最新的浏览器与兼容。

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>任务 3-CSS 兼容的代码段

在此任务中，你将了解如何使用跨浏览器兼容 CSS3 代码段以实现你的网站中的某些功能。

1. 在**Site.css**文件中，找到**标头**CSS 类定义 (.header)，将以下光标 **/\*边框 radius\* /**占位符，以添加新的代码段。 按**Enter**以显示类型以及智能感知列表**radius**以筛选列表。 选择**边框 radius**从该列表与双击，选项，然后按**选项卡**键以插入代码段。 然后，键入像素为单位，然后按 radius 大小**Enter**。 例如，键入**15px**。

    添加的代码段 CSS3 属性将呈现在大多数的 HTML5 法规遵从性浏览器，包括 Mozilla 和基于 WebKit 浏览器中的圆角的边框。

    ![使用边框 radius 段](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "使用边框 radius 段")

    *使用边框 radius 段*
2. 将相同的应用**边框**页样式 (.page) 中的代码段。

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. 按**F5**运行该解决方案。 请注意，每个页面现在已舍入边框。

    ![圆角](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "圆角")

    *圆的角*
4. 关闭浏览器并返回到 Visual Studio。
5. 打开**Custom.css**文件位于**样式**文件夹并将光标置于**div.images ul li img**类定义。
6. 按 enter 以显示 IntelliSense 列表中，类型**框阴影**按**选项卡**键两次以插入的类定义中的默认卷影代码段。 将卷影值设置为**10px 10px 5px #888**。 然后，键入**边框 radius**和插入代码段。 类型**15px**设置 radius 大小和按**ENTER**。

    ![圆角与卷影](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "圆角与卷影")

    *使用卷影圆的角*

    > [!NOTE]
    > 此时，则使用相应的前缀 （moz、 易于使用的功能、 o） 以支持 Mozilla 和 Webkit (Chrome、 Safari，Konkeror) 浏览器中插入卷影属性。
7. 创建一个新类**div.images ul li img:hover**下面**div.images ul li img**类定义，并将光标置于括号**。**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. 类型**转换**按**选项卡**两次以插入转换代码段的密钥。 然后，输入**rotate(-15deg)**图像光标悬停上时更改旋转角度值。

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. 按**F5**若要运行解决方案并浏览到 CSS3 页。 请注意，映像有了圆形角并框阴影。 鼠标悬停的图像，并观察它们旋转。

    ![转换旋转图像的代码段](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "转换段旋转图像")

    *转换旋转图像的代码段*

    > [!NOTE]
    > 如果你使用 Internet Explorer 10，并且无法看到阴影，请确保文档模式设置为 IE10 标准。 按**F12**若要打开 Internet Explorer 开发人员工具，然后单击**文档模式**更改为 IE10 标准。

    ![about-us](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>练习 2： 什么是新的 HTML 编辑器

Visual Studio 提供改进的 HTML 编辑器。 某些包含在此版本中的增强功能是在 HTML 文档、 HTML5 代码段、 HTML 开始和结束标记匹配，和 HTML 验证智能缩进。 在本练习中，你将看到在网站标记中工作时，这些更改如何改进你流畅性。

CSS 编辑器中，如 HTML 编辑器也已得到改进。 这些改进大部分都是使 Web 开发人员更轻松的小的。 针对 HTML5，智能缩进匹配开始和结束标记时编辑和验证目标 HTML 文档 DOCTYPE 其中有些改善像多个代码段。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>任务 1-改进的 DOCTYPE 验证

HTML 编辑器现在具有能够检查你的页面，DOCTYPE，即使定义可能在母版页中。 具体取决于你的页面的 DOCTYPE，HTML 编辑器将验证与正确的规则集，并将筛选考虑 DOCTYPE 元素的智能感知列表。

在此任务中，你将更改页面的 DOCTYPE，若要了解如何 HTML 编辑器行为相应地改变。

1. 如果尚未打开，启动**Visual Studio**并打开**WhatsNewASPNET.sln**解决方案位于**Source\WhatsNewASPNET**这个实验室的文件夹。
2. 打开**Site.Master**页。
3. 验证工具栏，请注意目标架构。 HTML 编辑器的行为 （验证、 IntelliSense 等） 将正确更改以适应所选 Doctype。

    ![在 HTML 源编辑工具栏中使用 Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "使用 Doctype HTML 源编辑工具栏中")

    *使用 Doctype HTML 源编辑工具栏中*
4. 将目标架构更改为 HTML 4.01。

    ![在 HTML 源编辑工具栏中更改 Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "更改 Doctype HTML 源编辑工具栏中")

    *在 HTML 源编辑工具栏中更改 Doctype*
5. 将在光标**正文**元素，并开始键入 HTML5 元素的名称 (例如，**视频**)。 请注意，元素不存在的智能感知列表中。

    ![未列出的 HTML5 元素](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "未列出的 HTML5 元素")

    *未列出的 HTML5 元素*
6. 撤消对目标架构的更改的验证工具栏上，选择 DOCTYPE: XHTML5 从下拉列表。

    ![在 HTML 源编辑工具栏中使用 Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "使用 Doctype HTML 源编辑工具栏中")

    *重置 Doctype HTML 源编辑工具栏中*
7. 将在光标**正文**元素并开始再次键入 HTML5 元素 (例如，如**视频**)。 请注意 HTML5 元素现可用于智能感知列表。

    ![HTML5 元素会列出](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "正在列出的 HTML5 元素")

    *正在列出的 HTML5 元素*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>任务 2-开始/结束标记自动更新

Visual Studio 现在更新打开或关闭正在编辑相互匹配的元素的标记的 HTML。 在编辑 HTML 标记时，此新功能将提高工作效率。

1. 上**Default.aspx**页上，添加**H3**具有标题 （例如，Visual Studio 2012 岩石 ！） 的元素。


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. 更改**H3**标记和类型**H2**或**h 1。**

    请注意，自动更新的结束标记。 你还可以修改以查看，开始标记会相应地更新过的结束标记。

    ![结束标记的自动更新](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "的结束标记的自动更新")

    *结束标记的自动更新*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>任务 3-新的 HTML5 代码段

Visual Studio 现在包含几个 HTML5 代码段。 在此任务中，你将使用这些代码段的一些。

1. 添加一个名为的新文件夹**音频**到网站文件夹的根目录。 打开 Windows 资源管理器并将复制到任何音频文件**音频**文件夹**WhatsNewASPNET.sln**解决方案。
2. 在**Default.aspx**页上，找到下 Web11 岩石光标!! 标头。 类型**音频**，然后按 TAB 键。

    新的 HTML 编辑器包括 HTML5 内容的代码段。 请记住使用正确的 DOCTYPE 定义来启用 HTML5 代码段。

    ![插入 HTML5 代码代码段](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "插入 HTML5 代码代码段")

    *插入 HTML5 代码代码段*
3. 更新音频源以指向现有的音频文件。


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > 你将需要向解决方案中添加的音频文件。
4. 按**F5**来运行该站点和播放音频。

    ![运行音频控件](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "运行音频控件")

    *运行音频控件*

    > [!NOTE]
    > 你还可以尝试多个代码段包含在 Visual Studio 中，例如视频、 图，等等。
5. 现在，尝试将控件插入页的某些部分。 例如，尝试插入**GridView**控件，但而不是键入 **&lt;gri 可持续发展，**开始键入 **&lt;GV**。 请注意，IntelliSense 列表显示**asp: GridView**控件。

    IntelliSense 的 HTML 编辑器现在提供标题大小写搜索，以及部分匹配 （检索包含术语的所有元素）。

    ![插入与智能感知列表 GridView](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "插入与智能感知列表 GridView")

    *插入与智能感知列表 GridView*

    如果你键入**&lt;网格**会获得词，匹配的所有项而 Visual Studio 将建议**gridview**控件：

    ![插入与智能感知列表和部分匹配 GridView](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "插入与智能感知列表和部分匹配 GridView")

    *插入与智能感知列表和部分匹配 GridView*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>任务 4-HTML 编辑器智能标记

在 HTML 编辑器中的另一改进是智能标记功能。 智能标记使易于基于每个控件执行常见或重复的开发任务。 此功能已可用在 HTML 设计器中，但不是在 HTML 编辑器中。

1. 打开**Site.Master**并找到**asp： 菜单**元素。 将光标置于开始标记和元素的底部显示的小标志符号单击它以打开智能任务菜单的通知。 请注意，可以快速访问与菜单控件相关的一些任务。

    ![智能菜单控件任务的](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "智能菜单控件有关的任务")

    *菜单控件的智能任务*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>任务 5-智能缩进

HTML 中的最佳做法之一缩进的嵌套的元素，以使代码更具可读性。 在 Visual Studio 2012 中，你将注意到您在编写代码时，编辑器可以自动将元素。

> [!NOTE]
> 在以前版本的 Visual Studio 中，提供智能缩进的 XML 编辑器中，但不是在 HTML 编辑器中。


1. 请确保在 HTML 编辑器中的缩进配置设置为智能缩进。 为此，请选择**工具 |选项**菜单选项，然后选择**文本编辑器 |HTML |选项卡**屏幕的左窗格中的页。 选择智能缩进选项。

    ![HTML 编辑器设置](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML 编辑器设置")

    *HTML 编辑器设置*
2. 上**Default.aspx**页上，删除音频元素下的所有内容。
3. 将光标放在打开末尾**音频**元素然后点击**ENTER**。

    请注意，新光标的位置有一个其他的缩进级别。

    ![智能缩进的 HTML 编辑器](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "智能缩进的 HTML 编辑器")

    *智能缩进的 HTML 编辑器*
4. 还原已删除，或关闭的内容与音频标记**Default.aspx**而不保存所做的更改。

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>任务 6-提取到用户控件

包括在 Visual Studio 中，如提取部分、 指向函数的重构工具是代码的帮助改善和重构现有代码的强大功能。 ASP.NET 页面对应的将提取到用户控件的 HTML 代码。 不要手动将涉及几个步骤，如创建新的用户控件、 将代码部分移到用户控件、 注册用户控件标记前缀和，最后，实例化的页上该用户控件。 现在，新*提取到用户控件*工具会自动为你执行所有这些步骤。

在此任务中，你将使用新提取到用户控制上下文操作从所选的代码生成一个新的用户控件。

1. 上**Default.aspx**页上，选择**H2**和**音频**元素。
2. 右键单击并选择**提取到用户控件**。

    ![提取到用户控件菜单选项](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "提取到用户控件菜单选项")

    *提取到用户控件菜单选项*
3. 键入新的用户控件的名称。 例如， **Jukebox.ascx**，然后单击**确定**。

    ![保存提取的用户控件](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "保存提取的用户控件")

    *保存提取的用户控件*
4. 请注意，所选的代码提取到用户控件并与新的用户控件的实例替换为所选代码的原始位置。

    ![页自动更新，以使用新的用户控件](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "页将自动更新，以使用新的用户控件")

    *页自动更新，以使用新的用户控件*
5. 按**F5**运行页面，并验证该控件是否有效。

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>练习 3： 什么是 JavaScript 编辑器中的新增功能

编写或编辑 JavaScript 代码不是一个简单的任务，特别是在你的应用程序启动增大，并且你发现自己长文件处理和数百个函数。 脚本开发人员通常需要执行一些额外的工作以维护代码可读性并在文件之间导航。 如 jQuery JavaScript 库为包含，脚本导航变得艰巨本身由于代码长度。

Visual Studio 已续订承诺来使代码模式，可访问和组织的 JavaScript 编辑器。 已经存在于 C# 或 VB 编辑器中的许多 Visual Studio 功能现在已实现在 JavaScript 编辑器中： 转到定义、 自动缩进、 文档和当你正在编写时进行验证。 与续订的 IntelliSense 列表将在您能够轻松利用具有 JavaScript 函数目录。

在本练习中，你将学习的一些新功能和改进的 JavaScript 编辑器。 将浏览示例文件，并且发现每个将使在 JavaScript 编程更高效 Visual Studio 2012 中的新特征。

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>任务 1-JavaScript 编辑器新功能

此任务将向你介绍某些新的 JavaScript 编辑器功能，其中重点介绍组织你的代码并将更好的用户体验。

1. 如果尚未打开，启动**Visual Studio**并打开**WhatsNewASPNET.sln**解决方案位于**Source\WhatsNewASPNET**这个实验室的文件夹。
2. 按**F5**运行该应用程序，然后单击导航栏中的 JavaScript 链接。 刷新页面几次并检查如何此计数器递增。

    ![Page 计数器](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Page 计数器")

    *Page 计数器*
3. 关闭浏览器并返回到 Visual Studio。
4. 打开**JavaScript.aspx**页然后找到**&lt;脚本&gt;**块 （如下所示）。

    下面的代码使用 HTML5 本地存储来存储*pageLoadCount*变量，用于存储的当前用户访问页的次数。 本地存储是引入 HTML5 标准的客户端键值对的数据库。 在用户的浏览器内的本地计算机上保存的数据。

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > 确保 DOCTYPE 设置为 XHTML5 然后再继续进行下一步的步骤。
5. 编辑代码，并请注意，适用于 JavaScript IntelliSense 包含 HTML5 功能，如本地存储区和其内部的方法。

    ![在 JavaScript 中的 HTML5 JavaScript 功能](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "JavaScript 中的 HTML5 JavaScript 功能")

    *在 JavaScript 中的 HTML5 JavaScript 功能*
6. 单击任何左大括号 (**{**) 从脚本代码，并注意括号突出显示。

    ![括号突出显示](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "括号突出显示")

    *括号突出显示*
7. 取消注释函数**testAutoAlign()** (选择三个行，然后你可以使用**CTRL** + **K**;**CTRL** + **U**) 并找到光标置于函数代码。 按 enter 要追加第二行。 请注意，代码现在是**对齐**和**自动缩进**。

    ![JavaScript 代码是对齐的自动](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript 代码是自动的对齐")

    *JavaScript 代码是自动的对齐*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>任务 2-验证 JavaScript

在此任务中，你会发现 ECMAScript5 标准的新 JavaScript 验证。 此功能将帮助你编写兼容的 JavaScript 代码，在站点部署之前阻止脚本问题时。

> [!NOTE]
> Visual Studio 2010 实现 ECMAStript3 法规遵从性，而 Visual Studio 2012 提供 ECMAScript5 法规遵从性。


1. 打开**ECMA5script5.js**位于**Scripts\custom**项目文件夹。 现在，你将测试 ECMAScript5 标准的验证。

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    你可以查看&quot;**使用严格**&quot;方向在第一行中的文件，这样 ECMAScript5**严格模式下**。 此模式包含在阐明多义性从过去版本，并在对象属性上添加一些新功能，例如 getter 和 setter、 JSON 和更完整的反射的库支持的语言的子集。
2. 打开**错误列表**如果尚未打开 (**视图**菜单 |**错误列表**)。 请注意**函数**声明是否有下划线。 这是因为 ECMA5 标准函数不能嵌套在语言结构。 在错误列表下面你将看到警告详细信息。

    ![JavaScript 验证错误消息](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript 验证错误消息")

    *JavaScript 验证错误消息*
3. 注释掉**&quot;使用严格&quot;**方向和请注意，错误消失，但警告保持。
4. 在该文件的最后一行，编写任何字符串如下所示**&quot;测试&quot;** （包括引号引起来，以指示它是以字符串形式）。 编写一段时间的字符串可以显示智能感知列表中，并选择旁边**trim**选项。

    在 ECMAScript5 标准中，字符串值和变量也有定义，如 trim、 大写、 搜索和替换的字符串方法。

    ![在 JavaScript 中的智能感知列表](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "在 JavaScript 中的 IntelliSense 列表")

    *在 JavaScript 中的 IntelliSense 列表*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>任务 3-JavaScript XML 文档

在此任务中，您将了解有关在 JavaScript 中的 XML 文档的 Visual Studio 功能。 你将看到 JavaScript IntelliSense 列表现在显示每个函数的 XML 文档。 此外，你会发现在 JavaScript 中的导航功能。

1. 打开**XMLDoc.js**文件位于**自定义脚本/**项目文件夹。 此文件包含有关每个 JavaScript 函数的 XML 文档。

    ![JavaScript XML 文档集成到 IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML 文档集成到 IntelliSense")

    *集成到 IntelliSense 的 JavaScript XML 文档*
2. 下面**添加**函数中**XMLDoc.js**文件，创建一个名为的新函数**测试**。
3. 在**测试**函数中，调用**乘**接收两个参数的函数。 请注意显示的工具提示框**乘**函数文档。

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![对于 JavaScript 函数为 XML 文档](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "对于 JavaScript 函数为 XML 文档")

    *对于 JavaScript 函数为 XML 文档*
4. 完成的函数调用语句和类型*点*打开的智能感知列表上返回的值。 请注意，检测 Visual Studio**返回**在文档中，以便将该值视为一个数字的值。

    ![返回类型的 XML 文档](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "返回类型的 XML 文档")

    *返回类型的 XML 文档*
5. 现在，插入要添加函数的调用。 请注意 JavaScript 编辑器现在支持函数重载。 当你编写的函数名称时，你将能够选择任何可用的文档中指定的重载。

    ![重载的 XML 文档](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "重载的 XML 文档")

    *重载的 XML 文档*
6. 打开**GotoDefinition.js**文件并找到**$().html()**函数调用。 在上找到光标**html**。
7. 按**F12**和导航到的定义。 请注意，现在可以访问，并浏览你的 JavaScript 代码，而无需使用**查找**工具。
8. 在底部的代码文件的签名块之前的 jQuery 指令上查找光标。 按**F12**。 你将导航到 jQuery 库文件。 请注意你还可以使用 jQuery 文件中导航**F12**。

    ![导航到 jQuery 定义](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "导航到 jQuery 定义")

    *导航到 jQuery 定义*

> [!NOTE]
> 请确保 GotoDefinition.js 保存文件之前具有没有语法错误。


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>练习 4： 捆绑和缩减

您的网站多少次包括多个 JavaScript 或 CSS 文件？ 这是非常常见的方案，其中绑定和缩减可以帮助减少文件大小，而且使执行速度更快的站点。 ASP.NET 4.5 中的新捆绑功能插入单个元素，包一套 JS 或 CSS 文件，以及通过贴图层 （即删除不是必需的空白区域，删除注释，减少标识符） 的内容来减小其大小。

绑定和缩减 ASP.NET 4.5 中的执行在运行时，以便在过程可以标识用户代理 （例如 IE、 Mozilla，等等），并因此，通过针对用户的浏览器 （例如，删除内容的 Mozilla 特定来提高压缩当请求来自 IE）。

在此练习中，你将了解如何启用和使用 ASP.NET 4.5 中的不同类型的绑定和缩减。

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>任务 1-安装捆绑和缩减包从 NuGet

1. 如果尚未打开，启动**Visual Studio**并打开**WhatsNewASPNET.sln**解决方案位于**Source\WhatsNewASPNET**这个实验室的文件夹。
2. 打开 NuGet 包管理器控制台。 若要执行此操作，请使用菜单**视图** | **其他窗口** | **程序包管理器控制台**。

    ![打开程序包管理器 file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "打开包管理器控制台")

    *打开包管理器控制台*
3. 在**程序包管理器控制台中，**类型**安装包 Microsoft.Web.Optimization**按**ENTER**。

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>任务 2-默认捆绑包

使用绑定和缩减的最简单方法是启用的默认捆绑包。 此方法使用约定来让你引用 JS 和 CSS 文件的文件夹中的捆绑和缩减版本。

在此任务中，你将了解如何启用和引用捆绑和缩减型 JS 和 CSS 文件，查看生成的输出。

1. 如果尚未打开，启动**Visual Studio**并打开**WhatsNewASPNET.sln**解决方案位于**Source\WhatsNewASPNET**这个实验室的文件夹。
2. 在**解决方案资源管理器**，展开**样式**， **Scripts\custom**和**Scripts\bundle**文件夹。

    请注意应用程序正在使用多个 CSS 和 JS 文件。

    ![应用程序中的多个样式表和 JavaScript 文件](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "应用程序中的多个样式表和 JavaScript 文件")

    *应用程序中的多个样式表和 JavaScript 文件*
3. 打开**Global.asax.cs**文件。

    请注意，新**Microsoft.Web.Optimization**命名空间被注释掉该文件的开头。 取消注释使用指令以包含绑定和缩减功能。


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. 找到**应用程序\_启动**方法。

    在此方法中，取消注释 EnableDefaultBundles 调用，如下面的代码段中所示。 这使我们能够通过使用该文件夹的路径引用的文件夹中的 CSS 文件捆绑的集合加上&quot;CSS&quot;或&quot;JS&quot;后缀。


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. 打开**Optimization.aspx**文件并找到的内容控件**HeadContent**。

    请注意 CSS 文件和 JS 文件有一个引用的标记。


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > 此代码是出于演示目的。 理想情况下，将引用 Site.Master 文件中的捆绑包。 在此示例代码中，你会发现，某些捆绑文件也被引用 Site.Master 文件，使此最后一个引用冗余。
6. 请注意，使用链接中的捆绑约定**href**属性从样式和 Scripts\custom 获取所有 CSS 或 JS 文件文件夹分别。

    你可以使用路径**脚本/自定义/JS**如下所示捆绑和 minify 内的所有 JS 文件**自定义脚本/**文件夹。 这是默认捆绑包的默认行为。


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. 打开**Styles\Site.css**文件。

    请注意，原始的 CSS 文件包含缩进的代码、 空格和扩大文件的注释。 （还 JavaScript 文件包含空格和注释）。

    ![在脚本文件夹中的其中一个原始 CSS 文件](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "其中一个原始 CSS 文件的脚本文件夹中")

    *在脚本文件夹中的原始 CSS 文件之一*
8. 按**F5**运行该应用程序并导航到**优化**页。
9. 单击**CSS 捆绑包**链接以下载并打开文件。

    签出缩减捆绑文件。 你将注意到，已删除所有空格、 注释和缩进字符，生成较小的文件。

    ![捆绑 CSS 文件](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "捆绑 CSS 文件")

    *捆绑的 CSS 文件*
10. 现在，请单击**JS 捆绑**链接以打开捆绑在一起的 JavaScript 文件。 你可以放心地忽略警告的资源管理器。 请注意下的 JavaScript 文件**自定义**文件夹也将捆绑的和缩减。

    ![捆绑 JavaScript 文件](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "捆绑 JavaScript 文件")

    *捆绑的 JavaScript 文件*

    ASP.NET 的早期版本中，启用压缩，从而提供 CSS 或 JS 文件时非常复杂。 现在，正如你所看到的你只需添加中的一行*Global.asax*文件来启用绑定，，然后从你的站点引用捆绑的文件。

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>任务 3-静态捆绑包

静态捆绑方法允许你自定义捆绑、 引用和缩减方法将使用的文件组。

在此任务中，你将配置静态的捆绑包来定义一组特定的捆绑和 minify 的文件。

1. 关闭浏览器。
2. 打开**Global.asax.cs**文件并找到**应用程序\_启动**方法。
3. 取消注释下面的代码中所示的静态捆绑代码。

    定义将使用引用的静态捆绑&quot; **~/StaticBundle** &quot;虚拟路径和使用**JsMinify**与所有指定的文件的缩减为**AddFile**方法。 最后，要添加到此静态捆绑**BundleTable**和启用它。

    请注意，文件不位于同一位置;这是通过默认绑定的另一个优点。


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. 打开**Optimization.aspx**文件。

    请注意，链接到**静态 JS 捆绑**正在使用时的 Global.asax.cs 文件中配置静态捆绑已声明的路径： **/StaticBundle**。


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. 按**F5**以运行该应用程序，然后导航到**优化**页。
6. 单击**静态 JS 捆绑**链接以打开该文件。

    请注意缩减捆绑 JavaScript 文件是在路径下的静态捆绑文件中配置的所有 JavaScript 文件的输出&quot;/StaticBundle&quot;。

    ![静态 JavaScript 文件捆绑](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "静态 JavaScript 文件捆绑")

    *静态 JavaScript 文件捆绑在一起*
7. 关闭浏览器并返回到 Visual Studio。

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>任务 4-动态文件夹捆绑包

在此任务中，你将了解如何配置动态文件夹捆绑包。 可以包含静态 JavaScript，以及其他文件编译到 JavaScript 的语言中，因此，需要进行一些处理绑定执行前可以为动态绑定的权限。

在此示例中，您将学习如何使用**DynamicFolderBundle**类来创建一个动态绑定的 CofeeScript 中编写的文件。 CofeeScript 是一种编程语言，编译到 JavaScript 并为编写 JavaScript 代码，从而加强 JavaScript 的简洁起见和可读性提供更简单的语法。

1. 打开**Global.asax.cs**文件并找到**应用程序\_启动**方法。
2. 取消注释下面的代码中所示的动态捆绑代码。

    定义将使用的动态文件夹捆绑**CoffeeMinify**将仅应用到的文件的自定义缩减处理器&quot; **.coffee** &quot;扩展 (CoffeeScript 文件）。 请注意，你可以使用的搜索模式以选择要在文件夹中，如捆绑的文件\*.coffee。


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. 打开 NuGet 包管理器控制台。 若要执行此操作，请使用菜单**视图** | **其他窗口** | **程序包管理器控制台**。
4. 在**程序包管理器控制台中，**类型**安装包 CoffeeSharp**按**ENTER**。
5. 单击**显示所有文件**按钮**解决方案资源管理器**窗口

    ![显示的所有文件](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "都显示的所有文件")

    *显示的所有文件*
6. 右键单击**CoffeeMinify.cs**文件中**解决方案资源管理器**和选择**包括在项目中**

    ![在项目中包含 CoffeeMinify.cs 文件](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "在项目中包含 CoffeeMinify.cs 文件")

    *在项目中包含 CoffeeMinify.cs 文件*
7. 打开**CoffeeMinify.cs**文件。

    此类继承自 JsMinify 以 minify JavaScript 输出的 CoffeeScript 代码编译引起的。 它调用 CoffeeScript 编译器首先，生成的 JavaScript 代码，然后将它发送到 JsMinify.Process 方法以 minify 生成的代码。


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. 打开**Script1.coffee**和**Script2.coffee**文件从**脚本/捆绑**文件夹。

    这些文件将包括 CoffeScript 代码执行与 CoffeeMinify 类绑定时编译。

    为了简单起见，提供的 CoffeeScript 文件仅包含 CoffeeScript 示例代码。 注释排除的 JsMinify 过程。

    ![CoffeeScript 文件](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript 文件")

    *CoffeeScript 文件*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/)提供更简单的语法来编写 JavaScript 代码、 增强了 JavaScript 的简洁起见和可读性，以及添加其他功能，如数组理解和模式匹配。
9. 打开**Optimization.aspx**文件并找到的捆绑包链接。

    请注意，链接到**动态 JS 捆绑**正在引用**脚本/捆绑**文件夹使用**/咖啡**动态文件夹捆绑包配置的后缀。


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. 按**F5**以运行该应用程序，然后导航到**优化**页。
11. 单击**动态 JS 捆绑**链接以打开生成的文件。

    请注意，此包中包含的内容仅包含**.coffee**文件。 你还可以看到 CoffeeScript 代码已编译为 JavaScript 和注释掉的行已删除。

    ![动态 JS 文件捆绑](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "动态 JS 文件捆绑")

    *动态 JS 文件捆绑在一起*

> [!NOTE]
> 此外，你可以部署此应用程序对 Windows Azure 网站以下[附录 b： 发布 ASP.NET MVC 4 应用程序使用 Web Deploy](#AppendixB)。


<a id="Summary"></a>
## <a name="summary"></a>摘要

此实验室可帮助你了解 ASP.NET 中的新增功能和 Visual Studio 2012 中的 Web 开发是什么以及如何利用 Visual Studio 2012 中的许多增强功能。

通过完成本动手实验，您已了解到如何在 Visual Studio 2012 编辑器中使用的新功能和改进，CSS、 JavaScript 和 HTML。 此外，您已了解到 Visual Studio 2012 内置绑定和缩减的实现。

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附录 a： 安装 Visual Studio Express 2012 for Web

你可以安装**Microsoft Visual Studio Express 2012 for Web**或另一个&quot;Express&quot;版本使用 **[Microsoft Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)**. 以下说明将指导你完成安装所需的步骤*Visual studio Express 2012 for Web*使用*Microsoft Web 平台安装程序*。

1. 转到[ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。 或者，如果你已安装 Web 平台安装程序，你可以打开它，并搜索产品&quot; *Visual Studio Express 2012 for Web 的 Windows Azure SDK*&quot;。
2. 单击**立即安装**。 如果你没有**Web 平台安装程序**将重定向以下载并请先安装它。
3. 一次**Web 平台安装程序**处于打开状态，单击**安装**以启动安装程序。

    ![安装 Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "安装 Visual Studio Express")

    *安装 Visual Studio Express*
4. 阅读所有产品的许可证和条款，然后单击**我接受**以继续。

    ![接受许可条款](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *接受许可条款*
5. 等待，直到下载和安装过程完成。

    ![安装进度](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *安装进度*
6. 当安装完成后时，单击**完成**。

    ![安装已完成](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *安装已完成*
7. 单击**退出**以关闭 Web 平台安装程序。
8. 若要打开 Visual Studio Express for Web，请转到**启动**屏幕并开始编写&quot; **VS Express**&quot;，然后单击**VS Express for Web**磁贴。

    ![Web 磁贴的 VS Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *Web 磁贴的 VS Express*

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>附录 b： 发布 ASP.NET MVC 4 应用程序使用 Web 部署

本附录将演示如何从 Windows Azure 管理门户创建新的网站和发布应用程序获取按照本实验中，利用 Windows Azure 提供的 Web 部署发布功能。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>任务 1-创建新的 Web 站点从 Windows Azure 门户

1. 转到[Windows Azure 管理门户](https://manage.windowsazure.com/)并使用与你的订阅关联的 Microsoft 凭据登录。

    > [!NOTE]
    > 使用 Windows Azure 可以免费承载 10 个 ASP.NET 网站，然后随着流量增长而扩展。 你可以注册[此处](http://aka.ms/aspnet-hol-azure)。

    ![登录到 Windows Azure 门户](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "登录到 Windows Azure 门户")

    *登录到 Windows Azure 管理门户*
2. 单击**新建**命令栏上。

    ![创建新网站](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "创建新网站")

    *创建新网站*
3. 单击**计算** | **网站**。 然后选择**快速创建**选项。 为新网站提供可用的 URL，然后单击**创建网站**。

    > [!NOTE]
    > Windows Azure 网站是在云中，你可以控制和管理运行的 web 应用程序的宿主。 快速创建选项，可将已完成的 web 应用程序到 Windows Azure 网站从在门户外部部署。 它不包括用于设置数据库的步骤。

    ![创建新的网站使用快速创建](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "创建新的网站使用快速创建")

    *创建新的网站使用快速创建*
4. 等到新**网站**创建。
5. 创建网站后单击下的链接**URL**列。 检查新的 Web 站点工作。

    ![浏览到新的 web 站点](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "浏览到新的 web 站点")

    *浏览到新的 web 站点*

    ![运行网站](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "运行的网站")

    *运行的网站*
6. 返回到门户并单击在网站的名称**名称**列以显示的管理页。

    ![打开网站管理页](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "打开网站管理页")

    *打开网站管理页*
7. 在**仪表板**页上，在**速览**部分中，单击**下载发布配置文件**链接。

    > [!NOTE]
    > *发布配置文件*包含所有 web 应用程序发布到 Windows Azure 网站中的每个启用的发布方法所需的信息。 发布配置文件包含的 Url、 用户凭据和连接到并针对每个发布方法启用的终结点进行身份验证所需的数据库字符串。 **Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**和**Microsoft Visual Studio 2012**支持读取发布配置文件以自动执行的这些程序，以便配置web 应用程序发布到 Windows Azure 网站。

    ![下载网站发布配置文件](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "下载网站发布配置文件")

    *下载网站发布配置文件*
8. 到一个已知位置下载发布配置文件。 进一步在本练习中您将了解如何使用此文件来发布 Visual Studio 中的 web 应用程序到 Windows Azure 网站。

    ![保存发布配置文件](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "保存发布配置文件")

    *保存发布配置文件*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>任务 2-配置数据库服务器

如果你的应用程序将使用 SQL Server 数据库将需要创建 SQL 数据库服务器。 如果你想要部署的简单应用程序不使用 SQL Server 可能会跳过此任务。

1. 需要将用于存储应用程序数据库的 SQL 数据库服务器。 你可以从你的订阅在 Windows Azure 管理门户中查看 SQL 数据库服务器**Sql 数据库** | **服务器** | **服务器的仪表板**。 如果你没有创建的服务器，则可以创建一个使用**添加**命令栏上的按钮。 请记下的**服务器名称和 URL、 管理员登录名和密码**，如你将在接下来的任务中使用它们。 不创建数据库，它将在后面的阶段中创建。

    ![SQL 数据库服务器仪表板](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL Database 服务器仪表板")

    *SQL 数据库服务器仪表板*
2. 在下一个任务将测试从 Visual Studio 中，数据库连接，因此你需要在的服务器的列表中包括你的本地 IP 地址**允许的 IP 地址**。 若要做到这一点，请单击**配置**，选择的 IP 地址从**当前客户端 IP 地址**并将其粘贴在**起始 IP 地址**和**结束 IP 地址**文本框。 输入规则的名称，然后单击![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png)按钮。

    ![添加客户端 IP 地址](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *添加客户端 IP 地址*
3. 一次**客户端 IP 地址**添加到允许的 IP 地址列表中，单击**保存**以确认所做的更改。

    ![确认做的更改](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *确认做的更改*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>任务 3-发布 ASP.NET MVC 4 应用程序使用 Web 部署

1. 返回到 ASP.NET MVC 4 解决方案。 在**解决方案资源管理器**，右键单击网站项目，然后选择**发布**。

    ![发布应用程序](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "发布应用程序")

    *发布此网站*
2. 导入发布配置文件保存在第一个任务。

    ![导入发布配置文件](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "导入发布配置文件")

    *导入发布配置文件*
3. 单击**验证连接**。 验证完成后单击**下一步**。

    > [!NOTE]
    > 请参阅验证连接按钮旁边将显示绿色的复选标记后，验证已完成。

    ![正在验证连接](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "验证连接")

    *正在验证连接*
4. 在**设置**页上，在**数据库**部分中，单击你的数据库连接的文本框旁边的按钮 (即**DefaultConnection**)。

    ![Web 部署配置](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web 部署配置")

    *Web 部署配置*
5. 配置数据库连接，如下所示：

    - 在**服务器名称**类型 SQL 数据库服务器 URL 使用*tcp:*前缀。
    - 在**用户名**键入您的服务器管理员登录名。
    - 在**密码**键入服务器管理员登录密码。
    - 键入新的数据库名称，例如： *MVC4SampleDB*。

    ![配置目标连接字符串](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "配置目标连接字符串")

    *配置目标连接字符串*
6. 然后单击“确定” 。 当系统提示创建数据库单击**是**。

    ![创建数据库](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "创建数据库字符串")

    *创建数据库*
7. 在默认连接文本框中显示了将用于连接到 Windows Azure 中的 SQL 数据库连接字符串。 然后，单击 **“下一步”**。

    ![连接字符串指向 SQL 数据库](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "指向 SQL 数据库的连接字符串")

    *连接字符串指向 SQL 数据库*
8. 在**预览**页上，单击**发布**。

    ![Web 应用程序发布](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "发布 web 应用程序")

    *发布 web 应用程序*
9. 完成发布过程后，默认浏览器将打开已发布的网站。

    ![应用程序发布到 Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "应用程序发布到 Windows Azure")

    *应用程序发布到 Windows Azure*
