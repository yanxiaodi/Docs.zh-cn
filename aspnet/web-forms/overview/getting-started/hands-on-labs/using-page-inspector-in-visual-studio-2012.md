---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: "使用 Page Inspector 在 Visual Studio 2012 |Microsoft 文档"
author: rick-anderson
description: "在此动手实验中，你会发现新增工具，可查找和修复 Visual Studio-Page Inspector 中的 web 页问题。 Page Inspector 是一个新的工具 b..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 1a9e093faae2cea1c27c582e22aebc908f78addb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="using-page-inspector-in-visual-studio-2012"></a>使用 Page Inspector 在 Visual Studio 2012
====================
通过[Web 营地团队](https://twitter.com/webcamps)

> 在此动手实验中，你会发现新增工具，可查找和修复 Visual Studio-Page Inspector 中的 web 页问题。
> 
> Page Inspector 是一个新的工具，将浏览器诊断工具引入到 Visual Studio，并提供在浏览器、 ASP.NET 和源代码之间的集成的体验。 它可以呈现网页 （HTML、 Web 窗体、 ASP.NET MVC 或网页） 直接在 Visual Studio IDE 内，并允许您检查源代码和生成的输出。 Page Inspector 可以轻松地将分解网站、 快速生成从零开始的页，以及快速诊断问题。
> 
> 现今，我们具有大量中及时，例如 ASP.NET MVC 和 WebForms 创建灵活、 可缩放的网站的 Web 框架。 另一方面，它可以获取更难发现的页上的问题，因为 IDE 不支持在基于模板的页面和动态内容的设计器视图。 因此，这些网站必须以查看它们如何向用户显示的浏览器中打开。
> 
> Web 开发人员使用外部工具来查找在浏览器中定期运行的问题。 然后，它们返回到 IDE，并开始修复。 这来回 IDE、 浏览器和分析工具之间的活动可能效率很低，而有时需要全新部署并清理的每当你想要重现某个问题的缓存。
> 
> Page Inspector 通过组合在一起使用的功能的组合的集这两个领域的最佳桥接客户端 （浏览器工具） 和服务器 （ASP.NET 和源代码） 之间的 Web 开发中存在间隔。
> 
> 使用 Page Inspector，你可以查看 （包括服务器端代码） 的源文件中的哪些元素具有生成要在浏览器中呈现的 HTML 标记。 Page Inspector 还可让你修改 CSS 属性和 DOM 元素特性，以查看立即反映在浏览器中的更改。
> 
> 本动手实验将引导你完成 Page Inspector 功能，并显示如何使用它们以在 Web 应用程序中修复问题。 **此实验室中包含两个练习使用类似的流，但目标不同的技术。如果是 ASP.NET MVC 开发人员，请按照练习一个;如果你是两个 WebForms 开发人员按照练习**。
> 
> 此实验室将引导你完成的增强功能和前面所述通过将细微的更改应用于源文件夹中提供的示例 Web 应用程序的新功能。
> 
> 在 Web 营地培训工具包中，在包括所有的示例代码和代码段[https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)。


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>目标

在此动手实验中，你将了解如何：

- 将分解使用 Page Inspector 的 Web 站点
- 检查和预览使用 Page Inspector 的 CSS 样式更改
- 检测和修复问题，在使用 Page Inspector 在网页中

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>先决条件

你必须具有要完成本实验的以下项：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更高 (读取[附录 A](#AppendixA)有关如何安装它的说明)。
- Internet Explorer 9 或更高版本

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>练习

本动手实验包括以下练习：

1. [练习 1： 在 ASP.NET MVC 项目中使用 Page Inspector](#Exercise1)
2. [练习 2： 使用 Page Inspector 在 WebForms 项目中](#Exercise2)

> [!NOTE]
> 每个练习均附带的开始解决方案，位于的练习中，可用于完成相互独立地每个练习的 Begin 文件夹中。 在练习的源代码，你还可以找到一个 End 文件夹，包含具有完成相应练习中的步骤得到的代码的 Visual Studio 解决方案。 如果您在演练本动手实验需要其他帮助，你可以使用这些解决方案作为指南。


估计时间来完成该实验： **30 分钟**。

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>练习 1： 在 ASP.NET MVC 项目中使用 Page Inspector

在本练习中，您将学习如何预览和调试**ASP.NET MVC 4**解决方案使用**Page Inspector**。 首先，您将执行该工具简要浏览若要了解促进 Web 调试进程的功能。 那么，你将包含样式问题的网页中正常工作。 你将学习如何使用 Page Inspector 找到生成问题的源代码并修复此错误。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>任务 1-浏览 Page Inspector

在此任务中，您将学习如何在显示照片库的 ASP.NET MVC 4 项目的上下文中使用 Page Inspector。

1. 打开**开始**解决方案位于**源/Ex1-MVC4/开始/**文件夹。

    1. 你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
2. 在解决方案资源管理器，找到**Index.cshtml**下查看**/视图/主页**项目文件夹，右键单击它，然后选择**在 Page Inspector 中的查看**。

    ![选择要在 Page Inspector 中预览的文件](using-page-inspector-in-visual-studio-2012/_static/image1.png "选择要在 Page Inspector 中预览的文件")

    *选择要在 Page Inspector 中预览的文件*
3. Page Inspector 窗口将显示*/Home/索引*映射到所选的视图的源的 URL。

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *使用 Page Inspector 第一个联系人*

    Page Inspector 工具中你的 Visual Studio 环境集成。 该检查器包含嵌入的浏览器，以及功能强大的 HTML 探查器。 请注意，则不需要运行解决方案，以查看您的网页的外观。

    > [!NOTE]
    > 当 Page Inspector 浏览器的宽度小于所打开的页的宽度时，则不会正确看到页。 如果发生这种情况，调整 Page Inspector 的宽度。
4. 单击**文件**在 Page Inspector 的选项卡。

    你将看到正在撰写索引页的所有源文件。 此功能可帮助标识一眼的所有元素，尤其是当你正在使用分部视图和模板。 请注意，你也可以打开每个文件是否你单击的链接。

    ![-文件的选项卡](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *文件选项卡*
5. 单击**切换检查模式下**按钮，位于左侧的选项卡。

    此工具会让你选择页上的任何元素，并查看其 HTML 和 Razor 代码。

    ![切换检查模式按钮](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *切换检查模式按钮*
6. Page Inspector 浏览器中，将鼠标指针移动到页元素。 将鼠标指针移过所呈现的任何的页面部分，而显示的元素类型，并在 Visual Studio 编辑器中突出显示相应的源标记或代码。

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *在操作中检查模式下*

    > [!NOTE]
    > 你将不能够看到显示的源代码预览选项卡或不最大化 Page Inspector 窗口。 如果它最大化时，请单击 Page Inspector 中的元素，将显示所选内容的源代码，但它将隐藏的页面检查器窗口。

    如果你注意到**Index.cshtml**文件，你将注意到，突出显示的源代码的生成所选的元素的部分。 此功能便于编辑长的源文件，提供用于访问代码的直接和快速方法。

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *检查元素*
7. 单击**切换检查模式下**按钮 (![选择 HTML 选项卡以显示在 Page Inspector 浏览器中呈现的 HTML 代码。](using-page-inspector-in-visual-studio-2012/_static/image7.png "选择 HTML 选项卡以显示在 Page Inspector 浏览器中呈现的 HTML 代码。") ) 若要禁用光标。
8. 选择**HTML**选项卡以显示在 Page Inspector 浏览器中呈现的 HTML 代码。
9. 在 HTML 标记中，找到包含考拉链接的列表项，然后选择它。

    请注意，当你选择的代码，相应的输出将自动突出显示在浏览器中。 此功能可用于查看 HTML 块的页上的呈现方式。

    ![在页中的选择 HTML 元素](using-page-inspector-in-visual-studio-2012/_static/image8.png "页中的选择 HTML 元素")

    *在页中选择 HTML 元素*
10. 单击**切换检查模式下**按钮以启用*检查模式下*单击导航栏。 在中样式窗格中，HTML 代码的右侧，你将看到列表与应用于所选元素的 CSS 样式。

    > [!NOTE]
    > 因为标头是站点布局的一部分，Page Inspector 还将打开\_Layout.cshtml 文件并突出显示代码段受影响。

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *发现样式和所选元素的源文件*
11. 启用切换检查指针，将下面的蓝色的特色栏鼠标指针移动，再单击一半的圆圈。

    ![选择元素](using-page-inspector-in-visual-studio-2012/_static/image10.png "选择元素")

    *选择元素*
12. 在样式窗格中，找到**背景图像**项下面**.main 内容**组。 **取消选中****背景图像**，看看效果。 你将注意到浏览器将立即反映所做的更改和隐藏圆。

    > [!NOTE]
    > 在页面检查器样式选项卡应用的更改不影响原始样式表。 你可以取消选中样式或更改它们所想，但它们将刷新页面后还原次数的值。

    ![启用和禁用 CSS 样式](using-page-inspector-in-visual-studio-2012/_static/image11.png "启用和禁用 CSS 样式")

    *启用和禁用 CSS 样式*
13. 现在，单击**此处你的徽标**上使用检查模式下的标头的文本。
14. 在**样式**选项卡上，找到**字体大小**CSS 属性下**.site 标题**组。 双击属性值，然后将 2.3 em 值替换**3 em**，然后按**ENTER**。 请注意标题看起来更大。

    ![更改在 Page Inspector 的 CSS 值](using-page-inspector-in-visual-studio-2012/_static/image12.png "Page Inspector 中的更改的 CSS 值")

    *更改在 Page Inspector 的 CSS 值*
15. 单击**跟踪样式**选项卡上，位于 Page Inspector 的右窗格。 这是一种替代方式来查看应用于所选内容，按属性名称排序的所有样式。

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *所选元素的 CSS 样式跟踪*
16. Page Inspector 的另一个功能是布局窗格。 使用检查模式下，选择导航栏，然后单击**布局**在右窗格中的选项卡。 你将看到所选元素的确切大小以及其偏移量、 边距、 填充和边框的大小。 请注意，你还可以修改此视图中的值。

    ![Page Inspector 中的元素布局](using-page-inspector-in-visual-studio-2012/_static/image14.png "Page Inspector 中的元素布局")

    *Page Inspector 中的元素布局*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>任务 2-查找和修复照片库中的样式问题

你将如何诊断与以前版本的 Visual Studio 的网页问题？ 是可能熟悉 web 调试工具，可在 Visual Studio IDE，如 Internet 资源管理器开发人员工具或 Firebug 之外运行。 浏览器仅了解 HTML、 脚本和样式，尽管基础框架生成将呈现的 HTML。 为此，通常需要部署整个站点，以查看如何网页如下所示。

当你想要检测和修复网站中的问题时，可能必须按照这些步骤：

1. 从 Visual Studio 中，运行该解决方案，或部署 web 服务器上的页面。
2. 在浏览器中，打开你使用，或只需打开源代码和样式并尝试匹配问题的开发人员工具。 若要查找所涉及的文件，你会使用&quot;搜索&quot;或&quot;文件中的搜索&quot;样式类同名的功能。
3. 一旦检测到错误，停止 Web 浏览器和服务器。
4. 清除浏览器缓存。
5. 返回到 Visual Studio 来应用修补程序。 重复测试的所有步骤。

因为 ASP.NET MVC 4 中没有任何实际的所见即所得，大部分样式问题上检测到的后面阶段，在运行或部署 web 应用程序。 现在，使用 Page Inspector，则可以预览任何页，而无需运行解决方案。

在此任务中，将使用 Page inspector 并修复照片库应用程序的一些问题。

1. 使用 Page Inspector 找到**注册**和**登录**标头的左侧的链接。

    请注意链接不会显示在右侧，预期的位置，并且这些更新会显示类似项目符号列表。 现在，您将对齐到右侧的链接，并可以相应地重新应用。

    ![在链接中查找的寄存器和日志](using-page-inspector-in-visual-studio-2012/_static/image15.png "链接中查找的寄存器和日志")

    *在链接中查找的寄存器和日志*
2. 选择切换检查模式下，单击要关闭，但不是在注册链接以打开其代码。

    请注意，链接的源代码位于 **\_LoginPartial.cshtml**文件，不 Index.cshtml 也不\_Layout.cshtml，可能会在第一个位置中查找位置。 你具有已直接放在正确的源文件。
3. 在**样式**选项卡上，找到并单击 **<section> #login</section>** 项，它是一个 HTML 容器，这些链接。

    请注意， **#login**样式自动位于**Site.css**单击后。 此外，代码现在已突出显示。

    ![选择的 CSS 样式](using-page-inspector-in-visual-studio-2012/_static/image16.png "选择的 CSS 样式")

    *选择的 CSS 样式*
4. 取消注释**文本对齐**属性中突出显示的代码通过删除开始标记和结束字符并保存**Site.css**文件。

    Page Inspector 是感知的所有不同文件构成当前页上，并且它可以检测这些文件的任何更改时。 它向你发出警报时在浏览器中的当前页面不是与源文件同步。
5. 在 Page Inspector 浏览器中，单击位于加载页面在地址栏下方的栏。

    ![重新加载该页面](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *重新加载该页面*

    链接现在位于右侧，但它们仍然看起来像项目符号列表。 现在，你将删除的项目符号和水平对齐链接。

    ![更新后的网页](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *更新后的网页*
6. 使用检查模式下，选择任一 **&lt;li&gt;** 包含的项&quot;注册&quot;和&quot;登录&quot;链接。 然后，单击**&lt;部分&gt;#login**项以访问**Styles.css**代码。

    ![查找样式](using-page-inspector-in-visual-studio-2012/_static/image19.png "查找样式")

    *查找样式*
7. 在**Style.css**，取消注释的代码**#login li**项。 要添加的样式将隐藏项目符号和水平显示的项目。

    ![右列的登录链接](using-page-inspector-in-visual-studio-2012/_static/image20.png "右列的登录链接")

    *右列的登录链接*
8. 保存**Style.css**文件，并位于以下地址重新加载网页的栏上单击一次。 请注意，正确显示的链接。

    ![链接的右侧对齐](using-page-inspector-in-visual-studio-2012/_static/image21.png "链接的右侧对齐")

    *链接的右侧对齐*
9. 最后，你将更改标头标题。 检查模式下用于单击**此处你的徽标**文本和 get 生成它的源代码。
10. 现在你已在 **\_Layout.cshtml**，替换**此处你的徽标**'与 text'**照片库**。 保存和更新 Page Inspector 浏览器。

    ![分配新的标题](using-page-inspector-in-visual-studio-2012/_static/image22.png "分配新的标题")

    *分配新的标题*

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *照片库页上更新*
11. 最后，请选择**PhotoGallery**项目，然后按**F5**运行应用程序。 签出所有按预期方式工作所做的更改。

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>练习 2： 使用 Page Inspector 在 WebForms 项目中

在本练习中，您将学习如何预览和调试使用 Page Inspector 的 WebForms 解决方案。 首先，您将执行该工具简要浏览若要了解促进调试进程 Web Page Inspector 功能。 那么，你将包含样式问题的网页中正常工作。 你将学习如何使用 Page Inspector 找到生成问题的源代码并修复此错误。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>任务 1-浏览 Page Inspector

在此任务中，您将学习如何在显示照片库 WebForms 项目的上下文中使用 Page Inspector 功能。

1. 打开**开始**解决方案位于**源/Ex2-WebForms/开始/**文件夹。

    1. 你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
2. 在解决方案资源管理器，找到**Default.aspx**页上，右键单击它并选择**在 Page Inspector 中的查看**。

    ![使用 Page Inspector 打开 Default.aspx](using-page-inspector-in-visual-studio-2012/_static/image24.png "使用 Page Inspector 打开 Default.aspx")

    *使用 Page Inspector 的打开 Default.aspx*
3. Page Inspector 窗口将显示 Default.aspx。

    ![在 Page Inspector 中查看 Default.aspx](using-page-inspector-in-visual-studio-2012/_static/image25.png "在 Page Inspector 中查看 Default.aspx")

    *在 Page Inspector 中查看 Default.aspx*

    Page Inspector 工具中你的 Visual Studio 环境集成。 该检查器包含嵌入的浏览器，以及功能强大的 HTML 探查器将显示所选的代码。 请注意，则不需要运行解决方案，以查看您的网页的外观。

    > [!NOTE]
    > 当 Page Inspector 浏览器的宽度小于所打开的页的宽度时，则不会正确看到页。 如果发生这种情况，调整 Page Inspector 的宽度。
4. 单击**文件**在 Page Inspector 的选项卡。

    你将看到正在撰写呈现的默认页面的所有源文件。 这是一个有用的功能来标识一眼的所有元素，尤其是当你正在使用用户控件和母版页。 请注意，你还可以导航到每个文件。

    ![文件选项卡](using-page-inspector-in-visual-studio-2012/_static/image26.png "文件选项卡")

    *文件选项卡*
5. 单击**切换检查模式下**按钮，位于左侧的选项卡。

    此工具会让你选择页上的任何元素，并查看其 HTML 代码和.aspx 源。

    ![切换检查模式按钮](using-page-inspector-in-visual-studio-2012/_static/image27.png "切换检查模式按钮")

    *切换检查模式按钮*
6. 在 Page Inspector 浏览器中，将鼠标移页元素。 将鼠标指针移过所呈现的任何的页面部分，而显示的元素类型，并在 Visual Studio 编辑器中突出显示相应的源标记或代码。

    ![在操作中检查模式下](using-page-inspector-in-visual-studio-2012/_static/image28.png "检查模式下操作中")

    *在操作中检查模式下*

    > [!NOTE]
    > 你将不能够看到显示的源代码预览选项卡或不最大化 Page Inspector 窗口。 如果它最大化时，请单击 Page Inspector 中的元素，将显示所选内容的源代码，但它将隐藏的页面检查器窗口。

    如果你注意到**Default.aspx**文件，你将注意到，突出显示的源代码的生成所选的元素的部分。 此功能便于长的源文件，从而提供一种直接和快速的途径，若要访问代码的版本。

    ![检查元素](using-page-inspector-in-visual-studio-2012/_static/image29.png "检查元素")

    *检查元素*
7. 单击**切换检查模式下**按钮 (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser。](using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser。") )，位于 Page Inspector 选项卡，以禁用光标。
8. 选择**HTML**选项卡以显示在 Page Inspector 浏览器中呈现的 HTML 代码。
9. 在 HTML 代码中，找到包含考拉链接的列表项，然后选择它。

    请注意，当你选择的代码，则对应的输出将是自动突出显示的浏览器。 此功能可用于查看 HTML 块的页上的呈现方式。

    ![在页中选择 HTML 元素](using-page-inspector-in-visual-studio-2012/_static/image31.png "在页中选择 HTML 元素")

    *在页中选择 HTML 元素*
10. 单击**切换检查模式下**按钮以启用*检查模式下*单击导航栏。 在中样式窗格中，HTML 代码的右侧，你将看到列表与应用于所选元素的 CSS 样式。

    > [!NOTE]
    > 因为标头是站点布局的一部分，Page Inspector 还将打开 Site.Master 文件，并突出显示受影响的代码段。

    ![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "发现样式和所选元素的源文件")

    *发现样式和所选元素的源文件*
11. 启用切换检查指针，移动鼠标指针的菜单栏下方，再单击空白半圆。

    ![选择元素](using-page-inspector-in-visual-studio-2012/_static/image33.png "选择元素")

    *选择元素*
12. 在样式窗格中，找到**背景图像**项下面**.main 内容**组。 **取消选中****背景图像**，看看效果。 你将注意到浏览器将立即反映所做的更改和隐藏圆。

    > [!NOTE]
    > 在页面检查器样式选项卡应用的更改不影响原始样式表。 你可以取消选中样式或更改它们所想，但它们将刷新页面后还原次数的值。

    ![启用和禁用 CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "启用和禁用 CSS 样式")

    *启用和禁用 CSS 样式*
13. 现在，单击**你****此处显示的徽标**上使用检查模式下的标头的文本。
14. 在**样式**选项卡上，找到**字体大小**CSS 属性下**.site 标题**组。 双击该属性一次以编辑它的值。 替换 2.3em 值与**3em**，然后按 ENTER。 请注意标题看起来更大。

    ![更改页 Inspector2 中的 CSS 值](using-page-inspector-in-visual-studio-2012/_static/image35.png "Page Inspector 中的更改的 CSS 值")

    *更改在 Page Inspector 的 CSS 值*
15. 单击**跟踪样式**选项卡上，位于 Page Inspector 的右窗格。 这是一种替代方式来查看应用于所选内容，按属性名称排序的所有样式。

    ![所选元素的 CSS 样式跟踪](using-page-inspector-in-visual-studio-2012/_static/image36.png "所选元素的 CSS 样式跟踪")

    *所选元素的 CSS 样式跟踪*
16. Page Inspector 的另一个功能是布局窗格。 使用检查模式下，选择导航栏，然后单击**布局**在右窗格中的选项卡。 你将看到所选元素的确切大小以及其偏移量、 边距、 填充和边框的大小。 请注意，你还可以修改此视图中的值。

    ![Page Inspector 中的元素布局](using-page-inspector-in-visual-studio-2012/_static/image37.png "Page Inspector 中的元素布局")

    *Page Inspector 中的元素布局*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>任务 2-查找和修复照片库中的样式问题

你将如何诊断与以前版本的 Visual Studio 的网页问题？ 是可能熟悉 web 调试工具，可在 Visual Studio IDE，如 Internet 资源管理器开发人员工具或 Firebug 之外运行。 浏览器仅了解 HTML、 脚本和样式，尽管基础框架生成将呈现的 HTML。 为此，通常需要部署整个站点，以查看如何网页如下所示。

当你想要检测和修复网站中的问题时，可能必须按照这些步骤：

1. 从 Visual Studio 中，运行该解决方案，或部署 web 服务器上的页面。
2. 在浏览器中，打开你使用，或只需打开源代码和样式并尝试匹配问题的开发人员工具。 若要查找所涉及的文件，你会使用&quot;搜索&quot;或&quot;文件中的搜索&quot;样式类同名的功能。
3. 一旦检测到错误，停止 Web 浏览器和服务器。
4. 清除浏览器缓存。
5. 返回到 Visual Studio 来应用修补程序。 重复测试的所有步骤。

因为没有不实际所见即所得中 ASP.NET WebForms，某些样式问题上检测到的后面阶段，在运行或部署之后。 现在，使用 Page Inspector，则可以预览任何页，而无需运行解决方案。

在此任务中，你将用于解决某些问题照片库应用程序使用 Page inspector。 在以下步骤中，将检测并快速修复标头中的一些简单样式问题。

1. 使用页检查找到**注册**和**Log In**标头的左侧的链接。

    请注意链接不会出现在右侧的预期位置。 现在，您将对齐到右侧的链接，并可以相应地重新应用。

    ![登录链接位于左侧](using-page-inspector-in-visual-studio-2012/_static/image38.png "登录位于左侧的链接")

    *登录链接，在左侧定位*
2. 选择切换检查模式下，选择登录链接以打开其代码。

    请注意，链接源代码位于**Site.Master**文件，不即替代的 Default.aspx 页中你可能看起来在第一个位置; 你具有已直接放在正确的源文件。
3. 在**样式**选项卡上，找到并单击**&lt;部分&gt;#login**项，它是一个 HTML 容器，这些链接。

    请注意， **#login**样式自动位于**Site.css**单击后。 此外，代码现在已突出显示。

    ![选择的 CSS 样式](using-page-inspector-in-visual-studio-2012/_static/image39.png "选择的 CSS 样式")

    *选择的 CSS 样式*
4. 取消注释**文本对齐**属性中突出显示的代码通过删除开始标记和结束字符并保存**Site.css**文件。

    Page Inspector 是感知的所有不同文件构成当前页上，并且它可以检测这些文件的任何更改时。 它向你发出警报时在浏览器中的当前页面不是与源文件同步。
5. 在 Page Inspector 浏览器中，单击位于保存所做的更改并重新加载该页面在地址栏下方的栏。

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *重新加载该页面*

    链接现在位于右侧，但它们仍然看起来像项目符号列表。 现在，你将删除的项目符号和水平对齐链接。

    ![更新后的网页](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *更新后的网页*
6. 使用检查模式下，选择任一 **&lt;li&gt;** 包含的项&quot;注册&quot;和&quot;登录&quot;链接。 然后，单击**&lt;部分&gt;#login**项以访问**Styles.css**代码。

    ![查找样式](using-page-inspector-in-visual-studio-2012/_static/image42.png "查找样式")

    *查找样式*
7. 在**Style.css**，取消注释的代码**#login li**项。 要添加的样式将隐藏项目符号和水平显示的项目。

    ![右列的登录链接](using-page-inspector-in-visual-studio-2012/_static/image43.png "右列的登录链接")

    *右列的登录链接*
8. 保存**Style.css**文件，并位于以下地址重新加载网页的栏上单击一次。 请注意，正确显示的链接。

    ![链接的右侧对齐](using-page-inspector-in-visual-studio-2012/_static/image44.png "链接的右侧对齐")

    *链接的右侧对齐*
9. 最后，你将更改标头标题。 而不是搜索**此处你的徽标**文本中所有文件，用于检查模式下单击相应的文本，并生成它的源代码获取。

    ![查找网站标题](using-page-inspector-in-visual-studio-2012/_static/image45.png "查找网站标题")

    *查找网站标题*
10. 现在你已在**Site.Master**，替换**此处你的徽标**'与 text'**照片库**。 保存和更新 Page Inspector 浏览器。

    ![照片库页上更新](using-page-inspector-in-visual-studio-2012/_static/image46.png "照片库页上更新")

    *照片库页上更新*
11. 最后按**F5**运行该签出所有按预期方式工作所做的更改的应用程序。

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>摘要

通过完成本动手实验，您已了解到如何使用 Page Inspector 而无需重新生成并运行网站的浏览器中预览 Web 应用程序。 此外，您已了解到如何快速查找和修复 bug 的源代码中呈现的输出直接访问。

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附录 a： 安装 Visual Studio Express 2012 for Web

你可以安装**Microsoft Visual Studio Express 2012 for Web**或另一个&quot;Express&quot;版本使用 **[Microsoft Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)**. 以下说明将指导你完成安装所需的步骤*Visual studio Express 2012 for Web*使用*Microsoft Web 平台安装程序*。

1. 转到[ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。 或者，如果你已安装 Web 平台安装程序，你可以打开它，并搜索产品&quot; *Visual Studio Express 2012 for Web 的 Windows Azure SDK*&quot;。
2. 单击**立即安装**。 如果你没有**Web 平台安装程序**将重定向以下载并请先安装它。
3. 一次**Web 平台安装程序**处于打开状态，单击**安装**以启动安装程序。

    ![安装 Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "安装 Visual Studio Express")

    *安装 Visual Studio Express*
4. 阅读所有产品的许可证和条款，然后单击**我接受**以继续。

    ![接受许可条款](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *接受许可条款*
5. 等待，直到下载和安装过程完成。

    ![安装进度](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *安装进度*
6. 当安装完成后时，单击**完成**。

    ![安装已完成](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *安装已完成*
7. 单击**退出**以关闭 Web 平台安装程序。
8. 若要打开 Visual Studio Express for Web，请转到**启动**屏幕并开始编写&quot; **VS Express**&quot;，然后单击**VS Express for Web**磁贴。

    ![Web 磁贴的 VS Express](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *Web 磁贴的 VS Express*
