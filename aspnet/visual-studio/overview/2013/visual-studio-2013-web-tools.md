---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: "动手实验： Visual Studio 2013 Web 工具 |Microsoft 文档"
author: rick-anderson
description: "Visual Studio 是用于的出色开发环境。基于网络的 Windows 和 web 项目。 它包括的功能强大的文本编辑器，可以轻松地使用到..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: ef8ab82f9043ef9da3a3e6a146a97f083149534d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>动手实验： Visual Studio 2013 Web 工具
====================
通过[Web 营地团队](https://twitter.com/webcamps)

[下载 Web 营地培训工具包](http://aka.ms/webcamps-training-kit)

> Visual Studio 是用于的出色开发环境。基于网络的 Windows 和 web 项目。 它包括的功能强大的文本编辑器，可以轻松地用于编辑不使用项目的独立文件。
> 
> 当您编辑每个文件，visual Studio 将维护齐全的分析树。 这使 Visual Studio 可以在作出更快、 更轻松的开发体验的同时提供无与伦比的自动完成和基于文档的操作。 这些功能是在 HTML 和 CSS 文档中特别强大。
> 
> 所有这种强大也是可用于扩展，因此很容易扩展了功能强大的新功能以满足你需求的编辑器。 Web Essentials 是一套 （通常） web 相关到 Visual Studio 的增强功能。 它包括大量 （特别是对于 CSS) 的新智能感知完成、 新的浏览器链接功能、 自动 JSHint 的 JavaScript 文件，新的警告，HTML 和 CSS 以及对现代 web 开发至关重要的许多其他功能。
> 
> 在 Web 营地培训工具包中，在包括所有的示例代码和代码段[http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)。


<a id="Overview"></a>
## <a name="overview"></a>概述

<a id="Objectives"></a>
### <a name="objectives"></a>目标

在此动手实验中，你将了解如何：

- 使用如丰富的 HTML5 代码段和 Zen 编码 Web Essentials 中包括的新 HTML 编辑器功能
- 使用新 Web Essentials 中包括如颜色选取器和浏览器矩阵工具提示的 CSS 编辑器功能
- 使用提取到文件和 IntelliSense 等的 Web Essentials 中包括的所有 HTML 元素的新 JavaScript 编辑器功能
- 你的浏览器和 Visual Studio 中使用浏览器链接之间交换数据

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>先决条件

完成本动手实验需要以下：

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/)或更高版本
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>安装

若要运行本动手实验中的练习，你将需要先设置你的环境。

1. 打开 Windows 资源管理器窗口并浏览到本实验的**源**文件夹。
2. 右键单击**Setup.cmd**和选择**以管理员身份运行**以启动安装过程将配置您的环境并安装本实验的 Visual Studio 代码段。
3. 如果显示用户帐户控制对话框中，确认操作以继续。

> [!NOTE]
> 请确保您已在运行安装程序之前检查本实验的所有依赖项。


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>使用代码片段

在整个实验文档中，你将指示要插入代码块。 为方便起见，大多数的此代码提供作为 Visual Studio 代码段，你可以从访问在 Visual Studio 2013，为了避免必须手动添加它。

> [!NOTE]
> 每个练习均附带的开始解决方案位于**开始**本练习，您可以完成相互独立地每个练习的文件夹。 请注意在练习过程中添加的代码段缺少这些开始解决方案中，并且可能不工作，直到完成练习。 在练习的源代码，你将找到**结束**包含具有完成相应练习中的步骤得到的代码的 Visual Studio 解决方案文件夹。 如果您在演练本动手实验需要其他帮助，你可以使用这些解决方案作为指南。


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>练习

本动手实验包括以下练习：

1. [使用浏览器链接和 Web Essentials](#Exercise1)
2. [利用代码段和 IntelliSense](#Exercise2)

> [!NOTE]
> 当你首次启动 Visual Studio 时，你必须选择一个预定义的设置集合。 每个预定义的集合用于匹配特定的开发风格和确定窗口布局、 编辑器行为、 IntelliSense 代码段和对话框选项。 在本实验中的过程描述完成给定的任务 Visual Studio 中使用时所需的操作**常规开发设置**集合。 如果您为开发环境选择不同的设置集合，则表明可能存在你应考虑到帐户的步骤中的差异。


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>练习 1： 使用浏览器链接和 Web Essentials

**Web Essentials**是一个 Visual Studio 扩展，将添加各种的现代 web 开发，主要侧重于使 web 开发体验更快、 更轻松的有用功能。 你可以从 Visual Studio 中的扩展库安装 Web Essentials。

**Browser Link**是提供你的 web 应用程序和 Visual Studio 之间交换数据的 Visual Studio IDE 和任何打开的浏览器之间的通道的 Visual Studio 2013 附带的新功能。 Web Essentials 用来操作 DOM 对象模型和直接通过浏览器在网页的 CSS 样式的工具扩展浏览器链接。

在此练习中，您将了解一些支持的功能**Web Essentials**和**Browser Link**来增强简单测验页。

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>任务 1-在多个浏览器中运行项目

在此任务中，你将配置 web 应用程序以在多个浏览器在运行一次，这是适用于跨浏览器测试。

1. 打开**Microsoft Visual Studio**。
2. 在**文件**菜单上，选择**打开 |项目/解决方案...**并浏览到**Ex1 WorkingwithBrowserLinkandWebEssentials\Begin**中**源**实验室 (C:\WebCampsTK\HOL\VSWebTooling\Source) 的文件夹。 选择**Begin.sln**单击**打开**。
3. 在 Visual Studio 工具栏上，展开浏览器菜单并选择**浏览...**.

    ![浏览与菜单选项](visual-studio-2013-web-tools/_static/image1.png "与...浏览在浏览器菜单")

    *浏览与菜单选项*
4. 在**浏览**对话框中，选择这两个**Google Chrome**和**Internet Explorer**通过按住**CTRL**密钥，然后单击**设为默认值**。

    ![浏览方式对话框中](visual-studio-2013-web-tools/_static/image2.png "浏览方式对话框")

    *选择多个默认浏览器*
5. Google Chrome 和 Internet 资源管理器现在应显示为默认浏览器。 单击**取消**以关闭对话框。

    ![Google Chrome 和 Internet Explorer 设置为默认浏览器](visual-studio-2013-web-tools/_static/image3.png "Google Chrome 和 Internet Explorer 默认浏览器")

    *Google Chrome 和 Internet Explorer 设置为默认浏览器*

    > [!NOTE]
    > 配置默认浏览器中之后,**多个浏览器**在浏览器的菜单中选择选项。
    > 
    > ![多个浏览器](visual-studio-2013-web-tools/_static/image4.png "多个浏览器")
6. 按**CTRL** + **F5**运行该应用程序而不进行调试。
7. 当这两个浏览器窗口打开时，将其中一个上下放以便同时在这两个浏览器中查看更新。 浏览器应显示具有浅蓝色矩形的 web 页。

    ![将上面另一个浏览器](visual-studio-2013-web-tools/_static/image5.png "放置上面另一个浏览器")

    *将上面另一个浏览器*
8. 不要关闭浏览器。 你将在下一个任务中使用它们。

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>任务 2-使用 Zen 编码来创建的 HTML 元素

**Zen 编码**高速 HTML、 XML、 XSL （或任何其他结构化的代码格式） 的插件编码和编辑是编辑器。 此插件的核心是一个功能强大的缩写引擎可用于扩展到 HTML 代码-类似于 CSS 选择器的表达式。 Zen 编码是一种编写 HTML 将 CSS 样式选择器语法的快速方法。

在此练习中，你将使用提供的 Web Essentials Zen 编码功能生成 HTML 表示的按钮，这一问题的选项。

1. 切换回 Visual Studio。
2. 打开**Index.cshtml**文件位于**视图** | **主页**文件夹。
3. 替换 **&lt;！-TODO： 添加选项此处-&gt;** 注释的以下代码，并按**选项卡**。

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. 应为 HTML 展开代码。

    ![展开 HTML](visual-studio-2013-web-tools/_static/image6.png "展开 HTML")

    *展开的 HTML*

    > [!NOTE]
    > 若要了解有关 Zen 编码语法的详细信息，请参阅以下[文章](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/)。
5. 单击**刷新链接的浏览器**按钮来更新这两个浏览器。

    ![刷新链接的浏览器](visual-studio-2013-web-tools/_static/image7.png "刷新链接的浏览器")

    *刷新链接的浏览器*

    ![带四个按钮的 Internet 资源管理器-页更新](visual-studio-2013-web-tools/_static/image8.png "Internet 资源管理器-页更新其中四个按钮")

    *带四个按钮的 Internet 资源管理器-页更新*

    ![带四个按钮的 Google Chrome 的页更新](visual-studio-2013-web-tools/_static/image9.png "Google Chrome 的页更新其中四个按钮")

    *带四个按钮的 Google Chrome 的页更新*
6. 切换回 Visual Studio。
7. 按钮添加到页中，但你仍需要添加模板问题。 若要执行此操作，将使用一项新功能中调用的 Web Essentials **Lorem Ipsum 生成器**。 找到**div**具有元素**类**属性**前面**。
8. 添加以下代码作为第一个子元素**div**，按**选项卡**。

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. 应为 HTML 展开代码。

    ![Lorem Ipsum 自动生成](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum 自动生成")

    *Lorem Ipsum 自动生成*

    > [!NOTE]
    > 作为 Zen 编码的一部分，你现在可以直接在 HTML 编辑器中生成 Lorem Ipsum 代码。 只需键入**lorem**并点击**选项卡**和 30 字 Lorem Ipsum 将插入文本。 例如 *lorem10*插入 10 位 Lorem Ipsum 的单词。
10. 将在问题的顶部添加徽标，通过在 Web Essentials 调用中使用另一项新功能**Lorem 像素生成器**。 添加以下代码作为第一个子元素**div**具有元素**容器**作为**类**值，然后按**选项卡**。

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. 代码应将扩展为 HTML。

    ![Lorem 像素自动生成](visual-studio-2013-web-tools/_static/image11.png "Lorem 像素自动生成")

    *Lorem 像素自动生成*

    > [!NOTE]
    > 作为 Zen 编码的一部分，你还可以直接在 HTML 编辑器中生成 Lorem 像素代码。 只需键入**pix 200 x 200 动物**并点击**选项卡**和**img**将插入标记使用的填充动物 200 x 200 映像。 有关详细信息，请参阅[Lorem 像素](http://www.lorempixel.com)。
12. 单击**刷新链接的浏览器**按钮来更新这两个浏览器。

    ![Internet Explorer 的自动生成图像和文本](visual-studio-2013-web-tools/_static/image12.png "Internet 资源管理器-自动生成图像和文本")

    *Internet 资源管理器-自动生成图像和文本*

    ![Google Chrome-自动生成图像和文本](visual-studio-2013-web-tools/_static/image13.png "Google Chrome-自动生成图像和文本")

    *Google Chrome-自动生成图像和文本*

    > [!NOTE]
    > 由于添加的代码段时，将随机选择映像，可能不同的浏览器中显示的图像。
13. 不要关闭浏览器。 你将在下一个任务中使用它们。

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>任务 3-更新样式属性

在此任务中，你将使用浏览器链接**检查模式**功能以检测其中不会生成特定的 DOM 元素的确切位置，然后更新使用颜色选取器提供 Web 该元素的颜色属性Essentials。

1. 在 Internet Explorer 浏览器中，按**CTRL** + **ALT** + **我**若要启用检查模式。
2. 将指针移浅蓝色边框并单击。

    ![将指针移动到浅蓝色边框上方](visual-studio-2013-web-tools/_static/image14.png "浅蓝色边框上移动指针")

    *浅蓝色边框上移动指针*
3. 切换回 Visual Studio。 请注意如何在 Visual Studio HTML 编辑器中还选择在浏览器中选择 HTML 元素。

    ![在 Visual Studio HTML 编辑器中选择 HTML 元素](visual-studio-2013-web-tools/_static/image15.png "在 Visual Studio HTML 编辑器中选择 HTML 元素")

    *在 Visual Studio HTML 编辑器中选择 HTML 元素*
4. 现在，你将更新**前面**CSS 类使用，以便更改所选元素的样式。 为此，请按**CTRL** + **，**以打开**导航到**搜索框。 类型**site.css**按**ENTER**打开文件。

    ![打开文件 Site.css](visual-studio-2013-web-tools/_static/image16.png "打开文件 Site.css")

    *打开文件 Site.css*
5. 按**CTRL** + **F**和类型**.flip 容器.front**若要查找的 CSS 选择器。
6. 单击要打开颜色选择器的类的边框属性中的浅蓝色方块。

    ![打开颜色选取器](visual-studio-2013-web-tools/_static/image17.png "打开颜色选取器")

    *打开颜色选取器*
7. 通过单击 v 形图标按钮展开颜色选取器并选择一种新颜色。

    ![展开颜色选取器](visual-studio-2013-web-tools/_static/image18.png "展开颜色选取器")

    *展开颜色选取器*
8. 按**CTRL** + **ALT** + **ENTER**若要刷新链接的浏览器。
9. 切换到 Internet 资源管理器，并请注意更改边框的颜色的方式。

    ![Internet Explorer 的更新的边框颜色](visual-studio-2013-web-tools/_static/image19.png "Internet 资源管理器-更新的边框颜色")

    *Internet 资源管理器-更新的边框颜色*
10. 切换到 Google Chrome，并请注意更改边框的颜色的方式。

    ![Google Chrome 的更新的边框颜色](visual-studio-2013-web-tools/_static/image20.png "Google Chrome 的更新的边框颜色")

    *Google Chrome 的更新的边框颜色*
11. 切换回 Visual Studio。
12. 转到末尾**Site.css**文件，然后按**CTRL** + **F**查找**.btn**选择器。
13. 请注意， **-webkit 边框 radius**属性下划线为绿色。

    ![为按钮选择器-webkit 边框 radius 属性](visual-studio-2013-web-tools/_static/image21.png "按钮选择器-webkit 边框 radius 属性")

    *-webkit 边框 radius 属性的按钮选择器*
14. 在将光标放置**-webkit 边框 radius**属性。 在属性的第一个单词的第一个字母下应显示一条蓝线。 这是**智能标记**。
15. 按**CTRL** + **。** 若要打开建议菜单上，然后单击**添加缺少标准属性 (边框 radius)**。

    ![添加缺少标准属性建议](visual-studio-2013-web-tools/_static/image22.png "添加缺少标准属性建议")

    *添加缺少标准属性建议*
16. **边框 radius**属性会自动添加到 CSS 规则。

    ![缺少标准属性添加](visual-studio-2013-web-tools/_static/image23.png "缺少添加的标准属性")

    *缺少添加的标准属性*
17. 将指针移**边框 radius**属性显示**浏览器矩阵工具提示**。 **浏览器矩阵工具提示**在每个浏览器中显示的属性的可用性。

    ![浏览器矩阵工具提示](visual-studio-2013-web-tools/_static/image24.png "浏览器矩阵工具提示")

    *浏览器矩阵工具提示*
18. 请注意，值**边框 radius**属性是否仍带有下划线。 将指针移若要查看警告消息的值。

    ![边框 radius 属性值警告](visual-studio-2013-web-tools/_static/image25.png "边框 radius 属性值警告")

    *边框 radius 属性值警告*
19. 删除的单元**边框 radius**根据由工具提示的建议的属性值。
20. 作为**边框 radius**是定义如何圆的角是，你可以删除的边框的标准属性**-webkit 边框 radius**属性和值从 CSS 规则。
21. 在将光标放置**自动换行**属性，请注意，下面还显示智能标记。
22. 打开菜单，然后单击**添加缺少的供应商细节**。

    ![添加缺少的供应商细节建议](visual-studio-2013-web-tools/_static/image26.png "添加缺少的供应商细节建议")

    *添加缺少的供应商细节建议*
23. **Ms 自动换行**属性会自动添加到 CSS 规则。

    ![供应商特定属性添加](visual-studio-2013-web-tools/_static/image27.png "供应商特定属性添加")

    *添加的供应商特定属性*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>任务 4-更新从浏览器的 HTML 代码

在此任务中，你将使用浏览器链接**设计模式下**功能来编辑浏览器中的 DOM 对象，并将所做的更改传输到 Visual Studio 中的 HTML 源文件。

1. 在 Google Chrome 中，按**CTRL** + **ALT** + **D**若要启用设计模式。
2. 将指针移**Lorem Ipsum dolor sit amet**标记，单击。

    ![编辑问题](visual-studio-2013-web-tools/_static/image28.png "编辑问题")

    *编辑问题*
3. 光标应出现。 将使用原始文本*什么它看起来像时编写更长的问题？*，然后按**ESC**以退出设计模式。

    ![编辑的问题](visual-studio-2013-web-tools/_static/image29.png "编辑的问题")

    *编辑的问题*
4. 切换回 Visual Studio 和打开**Index.cshtml**，如果尚未打开。 请注意，则内部文本的 **&lt;p&gt;** 元素已进行了更新。

    ![在 HTML 页中的已更新问题](visual-studio-2013-web-tools/_static/image30.png "在 HTML 页中的已更新问题")

    *在 HTML 页中的更新的问题*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>任务 5-查看 SEO 相关警告

**搜索引擎优化**(SEO) 是使搜索引擎结果列表中网站的排名更高版本的过程。 站点的排名更高版本并列出更加一致，站点将从该搜索引擎获取更多的访问者。 Web Essentials 合并检查 HTML 分析工具，报告问题，找到并提供帮助来修复它们。

1. 转到**视图**菜单，然后单击**错误列表**以打开**错误列表**窗口。

    ![错误列表视图中菜单](visual-studio-2013-web-tools/_static/image31.png "视图菜单中的错误列表")

    *错误列表视图中菜单*
2. 请注意，没有 SEO 警告，通知的**&lt;元&gt;**标记缺少页面描述为。 双击要修复此错误的 SEO 警告项。

    ![错误列表窗口](visual-studio-2013-web-tools/_static/image32.png "错误列表窗口")

    *错误列表窗口*
3. 在**Web Essentials**对话框中，单击**是**插入描述&lt;元&gt;标记。

    ![Web Essentials 对话框](visual-studio-2013-web-tools/_static/image33.png "Web Essentials 对话框")

    *Web Essentials 对话框*
4. 编辑器 **\_Layout.cshtml**打开和**&lt;元&gt;**标记都会自动添加到**头**部分HTML 文件。

    ![自动 _Layout 页中添加元标记](visual-studio-2013-web-tools/_static/image34.png "_Layout 页中自动添加元标记")

    *自动添加到元标记\_布局页*
5. 更改的值**内容**属性设为*GeekQuiz*并保存该文件。

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>练习 2： 利用代码段和 IntelliSense

与 Web Essentials HTML 编辑器已扩展具有额外功能。 在此练习中，你将看到一些新功能可帮助开发 web 应用程序。

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>任务 1-在 HTML 文档中使用 IntelliSense

你将看到在此任务中的第一个新功能称为**动态 IntelliSense**。 动态 IntelliSense 读取其他标记和特性来推断可能将使用的 id。

在此任务中，你将创建新的 HTML 窗体元素包含一个标签和输入的字段。 然后，将将添加**为**属性设为标签，以将其绑定到输入，并且你将看到 IntelliSense 建议基于作用域中的输入的 id。

1. 打开**Visual Studio Express 2013 for Web**和**Begin.sln**解决方案位于**源/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/开始**文件夹。 或者，继续与解决方案并在上一练习中获取。
2. 在**解决方案资源管理器**，打开**Index.cshtml**文件位于**视图** | **主页**文件夹。
3. 将以下表单内的添加**&lt;部分&gt;**元素。

    (代码段- *VisualStudio2013WebTooling* - *Ex2* - *窗体*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. 输入的标记之前应该具有某些说明的字段的标签。 添加下列标签之前输入的标记。

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. **为**属性**&lt;标签&gt;**指定标签的窗体元素绑定到。 该特性的值应设置为相关的元素的 id。 添加**为**属性设为**&lt;标签&gt;**元素。 下图中中, 所示&quot;名称&quot;值弹出 IntelliSense 框中，在基于相同的作用域内的元素的 id (封闭**&lt;窗体&gt;**)。

    ![在 IntelliSense 中显示 id](visual-studio-2013-web-tools/_static/image35.png "IntelliSense 中显示的 id")

    *在 IntelliSense 中显示的 id*
6. 删除最近添加**&lt;窗体&gt;**元素和其内容。

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>任务 2-使用 HTML 代码段

HTML5 引入超过 25 个新的语义标记。 Visual Studio 已有对这些标记，IntelliSense 支持，但是 Visual Studio 2013 可以更快、 更轻松地通过添加新的代码片段编写标记。 尽管这些标记不是复杂的但它们附带了几个小的细微部分，例如添加为将正确的编解码器回退字符*音频*标记。 在此任务中，你将看到音频标记的 HTML 代码段。

1. 在**Index.cshtml**文件，请键入 **&lt;aud**内**&lt;部分&gt;**元素下图中所示。

    ![插入音频元素](visual-studio-2013-web-tools/_static/image36.png "插入音频元素")

    *插入音频元素*
2. 按**选项卡**两次并请注意如何在页上添加下面的代码和在放置光标**src**的第一个源的属性。

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > 通过按**选项卡**键两次，插入代码段。 音频的代码段显示的标准用法*音频*标记，改进了支持的两个源文件。
3. 删除第二个行并使用以下链接 WebCampsTV Katana 显示更新的第一行的源： [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3)。 生成的代码所示。

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > 源文件*KatanaProject.mp3*用作示例。 如果你愿意，你可以使用另一个源。
4. 按**CTRL** + **S**保存文件。
5. 按**CTRL** + **F5**启动该应用程序。
6. 请注意音频播放机已添加到应用程序。

    ![在 Internet Explorer 中的音频播放器](visual-studio-2013-web-tools/_static/image37.png "在 Internet Explorer 中的音频播放机")

    *在 Internet Explorer 中的音频播放机*

    ![在 Google Chrome 的音频播放器](visual-studio-2013-web-tools/_static/image38.png "音频播放机在 Google Chrome")

    *在 Google Chrome 的音频播放机*
7. 不要关闭浏览器。 你将在下一个任务中使用它们。

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>任务 3-JavaScript 文档中使用 IntelliSense

与 Web Essentials 2013，样式表和 HTML 页生成的 Id 和类名称的列表。 在此任务中，你将学习如何这些列表改进 Web Essentials 2013 中的 JavaScript IntelliSense 支持。

1. 在**Index.cshtml**文件中，添加以下代码以定义**脚本**标记的 JavaScript 代码。

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. 添加以下代码**脚本**标记来定义准备就绪的回调函数。

    (代码段- *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. 在将光标放置**脚本**标记，然后按**CTRL** + **。** 若要打开建议菜单。
4. 单击**提取到文件**。

    ![JavaScript 提取到文件建议](visual-studio-2013-web-tools/_static/image39.png "JavaScript 提取到文件的建议")

    *JavaScript 提取到文件的建议*
5. 在**另存为**窗口中，选择**脚本**文件夹，将文件命名**init.js**单击**保存**。

    ![另存为窗口](visual-studio-2013-web-tools/_static/image40.png "另存为窗口")

    *另存为窗口*

    > [!NOTE]
    > **Init.js**创建文件和脚本的内容移动到文件。
    > 
    > ![创建包含的内容的 Init.js 文件](visual-studio-2013-web-tools/_static/image41.png "Init.js 文件创建包含的内容")
    > 
    > *创建包含的内容的 Init.js 文件*
6. 打开**Index.cshtml**文件，然后检查脚本标记已替换为引用**init.js**文件。

    ![Init.js html 引用](visual-studio-2013-web-tools/_static/image42.png "Init.js html 引用")

    *Init.js html 引用*
7. 转到**解决方案资源管理器**，请注意， **init.js**文件已自动包含在解决方案中。

    ![包含在解决方案中的 Init.js 文件](visual-studio-2013-web-tools/_static/image43.png "Init.js 文件包含在解决方案中")

    *包含在解决方案中的 Init.js 文件*
8. 切换回**init.js**文件以更新**准备**函数回调。
9. 在传递给函数回调定义*准备*，添加以下代码通过特定的类特性中获取所有元素。

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. 按**CTRL** + **空间**内引号之间**getElementsByClassName**函数调用。

    ![GetElementsByClassName 函数显示 IntelliSense](visual-studio-2013-web-tools/_static/image44.png "getElementsByClassName 函数显示 IntelliSense")

    *显示 IntelliSense getElementsByClassName 函数*

    > [!NOTE]
    > 请注意，IntelliSense 会显示项目样式表中定义的类。
11. 替换已替换为以下代码创建的行。

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. 将光标后**澳大利亚**在双引号内**getElementsByTagName**函数，并按**CTRL** + **空间**.

    ![GetElementByTagName 方法显示 IntelliSense](visual-studio-2013-web-tools/_static/image45.png "getElementByTagName 方法显示 IntelliSense")

    *GetElementsByTagName 方法显示 IntelliSense*
13. 选择**&quot;音频&quot;**从列表和按**ENTER**。 结果如下图所示。

    ![检索音频元素](visual-studio-2013-web-tools/_static/image46.png "检索音频元素")

    *检索音频元素*
14. 在**解决方案资源管理器**，右键单击**init.js**文件中**脚本**文件夹，然后选择**Minify JavaScript 文件**从**Web Essentials**菜单。

    ![Minify JavaScript 文件](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript 文件")

    *Minify JavaScript 文件*
15. 当系统提示时单击源文件更改启用自动缩减**是**。

    ![启用自动缩减警告](visual-studio-2013-web-tools/_static/image48.png "启用自动缩减警告")

    *启用自动缩减警告*

    > [!NOTE]
    > **Init.min.js**将创建并添加作为依赖项的**init.js**文件。
    > 
    > ![创建 Init.min.js 文件](visual-studio-2013-web-tools/_static/image49.png "Init.min.js 文件创建")
    > 
    > *创建 Init.min.js 文件*
16. 打开**init.min.js**文件，并请注意，缩减的文件。

    ![Init.min.js 文件内容](visual-studio-2013-web-tools/_static/image50.png "Init.min.js 文件内容")

    *Init.min.js 文件内容*
17. 在**init.js**文件中，添加下面的代码下面**getElementsByTagName**函数调用以播放音频的所有元素。

    (代码段- *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. 单击**CTRL** + **S**保存文件。 由于缩减的文件已打开，你将看到指示，源编辑器外部修改文件的对话框。 单击 **“是”**。

    ![Microsoft Visual Studio 警告](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio 警告")

    *Microsoft Visual Studio 警告*
19. 切换回**init.min.js**文件以验证该文件已更新使用新的代码。

    ![已更新的 Init.min.js 文件](visual-studio-2013-web-tools/_static/image52.png "Init.min.js 文件更新")

    *已更新的 Init.min.js 文件*
20. 单击**浏览器链接刷新**按钮。
21. 后刷新这两个浏览器将从你在上一任务中看到音频播放器开始自动播放。

    ![视图中包含的音频播放器](visual-studio-2013-web-tools/_static/image53.png "视图中包含的音频播放机")

    *视图中包含的音频播放机*

* * *

<a id="Summary"></a>
## <a name="summary"></a>摘要

通过完成本动手实验你已学习如何：

- 使用如丰富的 HTML5 代码段和 Zen 编码 Web Essentials 中包括的新 HTML 编辑器功能
- 使用新 Web Essentials 中包括如颜色选取器和浏览器矩阵工具提示的 CSS 编辑器功能
- 使用提取到文件和 IntelliSense 等的 Web Essentials 中包括的所有 HTML 元素的新 JavaScript 编辑器功能
- 你的浏览器和 Visual Studio 中使用浏览器链接之间交换数据
