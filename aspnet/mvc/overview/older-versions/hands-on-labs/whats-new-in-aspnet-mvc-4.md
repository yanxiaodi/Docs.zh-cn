---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: "什么是 ASP.NET MVC 4 中的新增功能 |Microsoft 文档"
author: rick-anderson
description: "ASP.NET MVC 4 是用于构建可缩放的、 基于标准的 web 应用程序使用成熟设计模式和 ASP.NET 的强大功能的框架和..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 35f9402ad6090c0441425a23b2b8063350892210
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="whats-new-in-aspnet-mvc-4"></a>什么是 ASP.NET MVC 4 中的新增功能

通过[Web 营地团队](https://twitter.com/webcamps)

[下载 Web 营地培训工具包](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 是一个框架，用于构建可缩放的、 基于标准的 web 应用程序使用成熟设计模式以及 ASP.NET 和.NET framework 的能力。 此新，第四个版本的 framework 侧重于使移动 web 应用程序开发更容易。

开始时，当你创建新的 ASP.NET MVC 4 项目现在有了你可以使用生成独立应用程序专用于移动设备的移动应用程序项目模板。 此外，与 jQuery Mobile jQuery.Mobile.MVC NuGet 程序包通过集成 ASP.NET MVC 4。 jQuery Mobile 是基于 HTML5 的开发与所有主流移动设备平台，包括 Windows Phone，iPhone、 Android 等兼容的 web 应用程序框架。 但是，如果你需要专用化，ASP.NET MVC 4 还使您能够服务适用于不同设备的不同视图，并提供特定于设备的优化。

在此动手实验中，你将使用 ASP.NET MVC 4 中启动&quot;Internet 应用程序&quot;项目模板创建照片库应用程序。 你将逐渐增强使用 jQuery Mobile 和 ASP.NET MVC 4 的新功能以使其与不同的移动设备和桌面 web 浏览器兼容的应用。 您还将了解代码生成和如何 ASP.NET MVC 4 使你更轻松地编写异步操作方法通过支持任务的新代码食谱&lt;ActionResult&gt;返回类型。

> [!NOTE]
> 在 Web 营地培训工具包中，在包括所有的示例代码和代码段[Microsoft 的 Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。 特定于此实验室项目位于[在 ASP.NET 4.5 Web 窗体中的新增](https://github.com/Microsoft-Web/HOL-ASPNETWebForms)。

<a id="Objectives"></a>
### <a name="objectives"></a>目标

在此动手实验中，你将了解如何：

- 充分利用 ASP.NET MVC 项目模板-包括新的移动应用程序项目模板的增强功能
- 使用 HTML5 视区属性和 CSS 媒体查询来改善在移动设备上的显示
- 使用 jQuery Mobile，来渐进式增强功能并构建触控优化 web UI
- 创建移动特定的视图
- 视图切换器组件用于应用程序中的移动和桌面视图之间切换
- 创建使用任务支持异步控制器

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>系统必备

你必须具有要完成本实验的以下项：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更高 (读取[附录 B](#AppendixB)有关如何安装它的说明)。
- [ASP.NET MVC 4](../../../mvc4.md) （Microsoft Visual Studio 2012 安装中包括）
- Windows Phone 仿真程序 (包括在[Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- 可选- [WebMatrix 2](https://www.microsoft.com/web/webmatrix/)与**Electric Plum iPhone 模拟器**（仅适用于用来与 iPhone 模拟器中浏览 web 应用程序的练习 3) 的扩展

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>安装

在整个实验文档中，你将指示要插入代码块。 为方便起见，该代码的大部分提供作为 Visual Studio 代码段，你可以使用从 Visual Studio 中为了避免必须手动添加它。

若要安装的代码段：

1. 打开 Windows 资源管理器窗口并浏览到本实验的**Source\Setup**文件夹。
2. 双击**Setup.cmd**文件置于此文件夹以安装的 Visual Studio 代码段。

如果你不熟悉 Visual Studio 代码段，并想要了解如何使用它们，你可以从该文档引用的附录&quot;[附录 a： 使用代码段](#AppendixA)&quot;。

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>练习

本动手实验包括以下练习：

1. [新的 ASP.NET MVC 4 项目模板](#Exercise1)
2. [创建照片库 Web 应用程序](#Exercise2)
3. [添加对移动设备的支持](#Exercise3)
4. [使用异步控制器](#Exercise4)

> [!NOTE]
> 每个练习均附带由**结束**包含生成您应该完成练习后获得的解决方案文件夹。 如果你需要通过在练习工作的更多帮助，可以使用此解决方案作为指南。


估计时间来完成该实验： **60 分钟**。

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>练习 1： 新的 ASP.NET MVC 4 项目模板

在此练习中，您将了解在 ASP.NET MVC 4 项目模板中的增强功能。 除了 Internet 应用程序模板中，在 MVC 3 中，已存在此版本现在包括用于移动应用程序的单独的模板。 首先，你将查看每个模板的某些相关功能。 然后，将处理呈现你的页正确在不同的平台上使用适当的方法。

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>任务 1-浏览 Internet 应用程序模板

1. 打开**Visual Studio**。
2. 选择**文件 |新 |项目**菜单命令。 在**新项目**对话框中，选择**Visual C# |Web**左窗格中的模板树，并选择**ASP.NET MVC 4 Web 应用程序。** 将项目**PhotoGallery**、 选择一个位置 （或保留默认值），然后单击**确定**。

    > [!NOTE]
    > 稍后将自定义图片库 ASP.NET MVC 4 解决方案现在要创建。

    ![创建新的项目](whats-new-in-aspnet-mvc-4/_static/image1.png "创建新项目")

    *创建新项目*
3. 在**新建 ASP.NET MVC 4 项目**对话框中，选择**Internet 应用程序**项目模板，然后单击**确定**。 请确保您已选择 Razor 作为视图引擎。

    ![新建 ASP.NET MVC 4 Internet 应用程序](whats-new-in-aspnet-mvc-4/_static/image2.png "新建 ASP.NET MVC 4 Internet 应用程序")

    *新建 ASP.NET MVC 4 Internet 应用程序*

    > [!NOTE]
    > ASP.NET MVC 3 中引入了 razor 语法。 其目标是字符和在文件中，需要启用快速和流体编码工作流的击键的数量降至最低。 Razor 利用现有的 C# / VB （或其他） 语言技能和提供使出色的 HTML 构造流的模板标记语法。
4. 按**F5**以运行该解决方案并查看续订的模板。 你可以查看以下功能：

    **现代样式模板**

    已续订的模板，提供更多的外观现代的样式。

    ![ASP.NET MVC 4 其样式模板](whats-new-in-aspnet-mvc-4/_static/image3.png "其样式模板的 MVC 4")

    *ASP.NET MVC 4 其样式模板*

    ![新联系人页](whats-new-in-aspnet-mvc-4/_static/image4.png "新联系人页")

    *新联系人页*

    **自适应呈现**

    签出调整浏览器窗口的大小，请注意如何页面布局可以动态地适应新的窗口大小。 这些模板使用的自适应呈现技术在没有任何自定义项的桌面和移动平台中正确呈现。

    ![在不同的浏览器大小中的 ASP.NET MVC 4 项目模板](whats-new-in-aspnet-mvc-4/_static/image5.png "中不同的浏览器大小的 ASP.NET MVC 4 项目模板")

    *在不同的浏览器大小中的 ASP.NET MVC 4 项目模板*

    **更丰富的 UI，使用 JavaScript**

    还有一项增强默认的项目模板是使用 JavaScript 来提供更交互式 JavaScript。 在模板中使用的登录和注册链接演示如何使用 jQuery 验证来验证从客户端的输入的字段。

    ![jQuery 验证](whats-new-in-aspnet-mvc-4/_static/image6.png)

    jQuery 验证

    > [!NOTE]
    > 请注意这两个日志中的第一节中的部分，你可以登录和你可以使用另一个身份验证服务，如 google （默认情况下禁用） 的 altenativelly 登录的第二部分中使用已注册帐户从站点。
5. 关闭浏览器以停止调试器并返回到 Visual Studio。
6. 打开文件**AuthConfig.cs**位于**应用\_启动**文件夹。
7. 从注册的 Google 客户端的最后一行中删除注释*OAuth*身份验证。


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > 请注意你可以轻松地实现使用如 Facebook、 Twitter、 Microsoft 等任何 OpenID 或 OAuth 服务身份验证。
8. 按**F5**若要运行解决方案，并导航到登录页。
9. 选择**Google**服务进行登录。

    ![在服务中选择日志](whats-new-in-aspnet-mvc-4/_static/image7.png)

    在服务中选择日志
10. 使用你的 Google 帐户登录。
11. 允许 Google 帐户中检索信息的站点 (localhost)。
12. 最后，你将需要在要将 Google 帐户相关联的站点中注册。

    ![将你的 Google 帐户相关联](whats-new-in-aspnet-mvc-4/_static/image8.png)

    *将你的 Google 帐户相关联*
13. 关闭浏览器以停止调试器并返回到 Visual Studio。
14. 现在浏览要签出项目模板中由 ASP.NET MVC 4 引入了一些其他新功能的解决方案。

    ![ASP.NET MVC 4 Internet 应用程序项目模板](whats-new-in-aspnet-mvc-4/_static/image9.png "ASP.NET MVC 4 Internet 应用程序项目模板")

    *ASP.NET MVC 4 Internet 应用程序项目模板*

    - **HTML 5 标记**

        浏览模板视图若要了解新的主题标记。

        ![使用 Razor 和 HTML5 标记 About.cshtml 新模板。](whats-new-in-aspnet-mvc-4/_static/image10.png "使用 Razor 和 HTML5 标记 About.cshtml 的新模板。")

        *使用 Razor 和 HTML5 标记 (About.cshtml) 新模板。*
    - **更新的 JavaScript 库**

        ASP.NET MVC 4 默认模板现在包括 KnockoutJS、 JavaScript MVVM 框架，它允许你创建丰富和响应度高的 web 应用程序使用 JavaScript 和 HTML。 如在 MVC3，jQuery 和 jQuery UI 库也包括在 ASP.NET MVC 4。

        > [!NOTE]
        > 你可以获取有关此链接中的 KnockOutJS 库的详细信息： [ [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/)。 此外，你可以了解有关 jQuery 和 jQuery UI 中[ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/)。

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>任务 2-浏览移动应用程序模板

ASP.NET MVC 4 便于移动的网站和平板电脑浏览器的开发。 此模板具有相同的应用程序结构与 Internet 应用程序模板 （注意控制器代码，并几乎完全相同），但其样式进行了修改以基于 touch 的移动设备中正确呈现。

1. 选择**文件 |新 |项目**菜单命令。 在**新项目**对话框中，选择**Visual C# |Web**左窗格中的模板树，并选择**ASP.NET MVC 4 Web 应用程序。** 将项目**PhotoGallery.Mobile**、 选择一个位置 （或保留默认值），选择&quot;将添加到解决方案&quot;单击**确定**。
2. 在**新建 ASP.NET MVC 4 项目**对话框中，选择**移动应用程序**项目模板，然后单击**确定**。 请确保您已选择 Razor 作为视图引擎。

    ![新建 ASP.NET MVC 4 移动应用程序](whats-new-in-aspnet-mvc-4/_static/image11.png "新建 ASP.NET MVC 4 移动应用程序")

    *新建 ASP.NET MVC 4 移动应用程序*
3. 现在，你将能够浏览解决方案和签出引入的一些新功能由移动的 ASP.NET MVC 4 解决方案模板：

    - **jQuery Mobile 库**

        移动应用程序项目模板包括 jQuery Mobile 库，这是移动浏览器兼容性的开放源代码库。 jQuery Mobile 适用于移动浏览器支持 CSS 和 JavaScript 渐进增强。 渐进增强允许所有浏览器显示网页的基本内容，虽然它仅可用于最强大的浏览器来显示丰富内容。 JavaScript 和 CSS 文件，包含在 jQuery 移动样式，帮助移动浏览器为适合在屏幕中的内容，而无需进行任何更改的页标记中。

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *模板中包含的 jQuery mobile 库*
    - **基于 HTML5 标记**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *使用 HTML5 标记、 （Login.cshtml 和 index.cshtml） 的移动应用程序模板*
4. 按**F5**运行该解决方案。
5. 打开**Windows Phone 7 仿真程序**。
6. 在 phone 开始屏幕中，打开 Internet Explorer。 签出桌面应用程序的起始位置的 URL 并通过手机浏览到该 URL (例如`http://localhost:[PortNumber]/`)。
7. 现在你已能够输入登录页或了解有关页面。 请注意到该网站的样式基于新 Metro 移动设备的应用程序。 ASP.NET MVC 4 项目模板正确显示在移动设备，并确保页面的所有元素都可见且已启用。 请注意，在标头的链接不够大，无法单击或点击。

    ![项目模板在移动设备中的页](whats-new-in-aspnet-mvc-4/_static/image14.png "项目在移动设备中的模板页")

    *在移动设备中的项目模板页*
8. 新模板还使用**视区元标记**。 大多数移动浏览器定义一个虚拟的浏览器窗口的宽度或&quot;视区&quot;，该值大于移动设备的实际宽度。 这使得移动浏览器以显示内部虚拟显示整个 web 页。 **视区元标记**允许 web 开发人员可以在移动设备上设置的宽度、 高度和浏览器区域的小数位数**。** 移动应用程序的 ASP.NET MVC 4 模板将视区设置为设备宽度 (&quot;宽度 = 设备宽度&quot;) 中的布局模板 (*views/shared\_Layout.cshtml*)，以便所有页将具有设置为设备屏幕宽度其视区。 请注意视区元标记不会更改默认浏览器视图。
9. 打开 **\_Layout.cshtml**为位于**视图 |共享**文件夹，并注释视区 meta 标记。 运行该应用程序，如果不是已打开，并且签出差异。


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

    ![注释视区元标记后的站点](whats-new-in-aspnet-mvc-4/_static/image15.png "后注释视区元标记站点")

    *站点后注释视区元标记*
10. 在 Visual Studio 中，按**SHIFT** + **F5**来停止调试应用程序。
11. 请取消注释视区 meta 标记。


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>任务 3-使用自适应呈现

在此任务中，你将学习另一种方法在没有任何自定义项的同一时间呈现在移动设备和 Web 浏览器上正常的网页。 你已具有类似用途使用视区 meta 标记。 现在就可以满足另一种功能强大的方法：*自适应呈现*。

自适应呈现是使用一种技术**CSS3 媒体查询**以自定义应用于页面的样式。 媒体查询定义分组特定条件下的 CSS 样式的样式表内的条件。 仅当条件为 true，则会将样式应用到已声明的对象。

提供的自适应呈现技术的灵活性允许用于在不同设备上显示站点的任何自定义。 你可以定义要在单个样式表而无需编写逻辑代码选择样式的所有样式。 因此，它是调整页样式的一种非常巧妙方法，因为这可以减少量重复的代码和呈现目的的逻辑。 另一方面，带宽消耗会增加，如 CSS 文件的大小会变得非常稍。

通过使用自适应呈现技术，你的站点将**正确显示，而不考虑浏览器。** 但是，你应考虑是否加载了额外的带宽是个问题。

> [!NOTE]
> 媒体查询的基本格式： @media \[作用域： 所有 | 手持 | 打印 | 投影 | 屏幕\]([属性： 值] 和...[属性： 值]）


媒体查询的示例： &gt;  **@media所有和 (最大宽度： 1000px) 和 (最小宽度： 700px) {}:**针对 700px 和 1000px 之间的所有分辨率。

> **@media 屏幕和 (最小宽度： 400px) 和 (最大宽度： 700px) {...}:**仅对屏幕。 分辨率必须为介于 400 和 700px。
> 
> **@media 手持和 (最小宽度： 20em)，屏幕和 (最小宽度： 20em) {...}:**手持设备 （移动设备和设备） 和屏幕。 最小宽度必须大于 20em。
> 
> 你可以在上找到有关此的详细信息[W3C 站点](http://www.w3.org/TR/css3-mediaqueries/)。


您现在将了解自适应呈现的工作原理、 提高其可读性的 ASP.NET MVC 4 默认网站模板。

1. 打开**PhotoGallery.sln**解决方案具有在任务 1 中创建和选择**PhotoGallery**项目。 按**F5**运行该解决方案。
2. 调整浏览器的宽度，设置 windows 到一半或小于其原始大小的四分之一。 请注意与标头中的项会发生什么情况： 某些元素不会出现在标头的可见区域。
3. 打开**Site.css**文件从 Visual Studio 解决方案资源管理器，位于**内容**项目文件夹。 按**CTRL + F**若要打开 Visual Studio 集成的搜索，并写入 **@media** 查找**CSS 媒体查询**。

    此模板中定义的媒体查询条件的工作原理以这种方式： 当浏览器的窗口大小低于**850 px**，应用的 CSS 规则是在此媒体块内定义的。

    ![定位到该媒体查询](whats-new-in-aspnet-mvc-4/_static/image16.png "定位到该媒体查询")

    *定位到该媒体查询*
4. 替换 850 中设置的最大宽度特性值与 px **10px**，禁用自适应呈现，以便按**CTRL + S**以保存所做的更改。 返回到浏览器和按**CTRL + F5**若要刷新的页中所做的更改。 在调整窗口的宽度，请注意这两个页中的差异。

    ![在左侧，应用页@media省略样式，请在右侧，样式](whats-new-in-aspnet-mvc-4/_static/image17.png "左侧，在应用页面@media省略样式，请在右侧，样式")

    *在左侧，应用页@media省略样式，请在右侧，样式*

    现在，让我们了解移动设备上会发生什么情况：

    ![在左侧，应用页@media省略样式，请在右侧，样式](whats-new-in-aspnet-mvc-4/_static/image18.png "左侧，在应用页面@media省略样式，请在右侧，样式")

    *在左侧，应用页@media省略样式，请在右侧，样式*

    尽管你会注意到，在 Web 浏览器中呈现页时的更改不非常重要，使用移动设备时的差异变得更加明显。 左侧的映像，我们可以看到的自定义样式改进可读性。

    可以在很多情况，从而更便于应用条件到网站样式和解决常见的样式问题，通过一种巧妙方法中使用自适应呈现。

    使你可以利用这些功能在任何 web 应用程序，并不特定于 ASP.NET MVC 4，视区元标记和 CSS 媒体查询。
5. 在 Visual Studio 中，按**SHIFT** + **F5**来停止调试应用程序。

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>练习 2： 创建照片库 Web 应用程序

在本练习中，你将处理要显示照片的照片库应用程序。 将开始使用 ASP.NET MVC 4 项目模板，，然后你将添加用于从服务中检索照片并将其显示在主页中的功能。

在以下练习中，你将更新此解决方案，以增强移动设备显示的方式。

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>任务 1-创建 Mock 照片服务

在此任务中，你将创建照片要检索服务将在库中显示的内容的模型。 若要执行此操作，将添加新的控制器，只需将返回一个 JSON 文件含有每张照片的数据。

1. 打开**Visual Studio**如果尚未打开。
2. 选择**文件 |新 |项目**菜单命令。 在**新项目**对话框中，选择**Visual C# |Web**左窗格中的模板树，并选择**ASP.NET MVC 4 Web 应用程序。** 将项目**PhotoGallery**、 选择一个位置 （或保留默认值），然后单击**确定**。 或者，您可以从现有的 ASP.NET MVC 4 继续工作**Internet 应用程序**解决方案从**练习 1**和跳过下一步。
3. 在**新建 ASP.NET MVC 4 项目**对话框中，选择**Internet 应用程序**项目模板，然后单击**确定**。 请确保你具有 Razor 作为视图引擎选择。
4. 在**解决方案资源管理器**，右键单击**应用\_数据**文件夹的项目，然后选择**添加 |现有项**。 浏览到**Source\Assets\App\_数据**这个实验室的文件夹并添加**Photos.json**文件。
5. 使用名称创建新的控制器**PhotoController**。 若要执行此操作，右键单击**控制器**文件夹中，转到**添加**和选择**控制器。** 完成该控制器的名称，保留**空 MVC 控制器**模板，然后单击**添加**。

    ![添加 PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "添加 PhotoController")

    *添加 PhotoController*
6. 替换**索引**方法替换为以下**库**操作，并从你最近添加到项目的 JSON 文件的返回内容。

    (代码段- *ASP.NET MVC 4 实验-Ex02-库操作*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. 按**F5**运行该解决方案，然后浏览到以下 URL 若要测试的模拟的照片服务： `http://localhost:[port]/photo/gallery` （[端口] 值依赖于已启动应用程序的当前端口）。 对此 URL 的请求应检索其内容的**Photos.json**文件。

    ![测试的模拟的照片服务](whats-new-in-aspnet-mvc-4/_static/image20.png "测试的模拟的照片服务")

    *测试的模拟的照片服务*

你可以使用在实际实现[ASP.NET Web API](../../../../web-api/index.md)若要实现照片库服务。 ASP.NET Web API 是一个框架，可以轻松地生成覆盖广泛的客户端，包括浏览器和移动设备的 HTTP 服务。 ASP.NET Web API 是用于在 .NET Framework 上生成 RESTful 应用程序的理想平台。

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>任务 2-显示照片库

在此任务中，你将更新主页页后，可以通过使用在本练习中的第一个任务中创建模拟的服务显示照片库。 你将添加模型文件和更新库视图。

1. 在 Visual Studio 中，按**SHIFT** + **F5**来停止调试应用程序。
2. 创建**照片**类**模型**文件夹。 若要执行此操作，右键单击**模型**文件夹，选择**添加**单击**类**。 然后，将名称设置为**Photo.cs**单击**添加**。
3. 添加到以下成员**照片**类。

    (代码段- *ASP.NET MVC 4 实验-Ex02-照片模型*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. 从“控制器”文件夹打开 HomeController.cs 文件。
5. 添加下面的 using 语句。

    (代码段- *ASP.NET MVC 4 实验-Ex02-HomeController Using*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. 更新**索引**操作使用**HttpClient**检索库数据，然后使用**JavaScriptSerializer**反其序列化到视图模型。

    (代码段- *ASP.NET MVC 4 实验-Ex02-索引操作*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. 打开**Index.cshtml**文件位于**views/home**文件夹，然后替换替换为以下代码的所有内容。

    此代码循环访问所有从服务检索的照片，并显示它们转换为无序的列表。

    (代码段- *ASP.NET MVC 4 实验-Ex02-照片列表*)


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. 在**解决方案资源管理器**，右键单击**内容**文件夹的项目，然后选择**添加 |现有项**。 浏览到**Source\Assets\Content**这个实验室的文件夹并添加**Site.css**文件。 你将需要确认其替换。 如果你有**Site.css**文件打开时，你将需要确认还重新加载该文件。
9. 打开文件资源管理器并将复制整个**照片**文件夹位于下**Source\Assets**的解决方案资源管理器中的项目的根文件夹到本实验的文件夹。
10. 运行该应用程序。 你现在应看到在库中显示照片的主页。

    ![照片库](whats-new-in-aspnet-mvc-4/_static/image21.png "照片库")

    *照片库*
11. 在 Visual Studio 中，按**SHIFT** + **F5**来停止调试应用程序。

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>练习 3： 添加对移动设备的支持

ASP.NET MVC 4 中的密钥更新之一是对移动开发的支持。 在此练习中，您将了解通过扩展 PhotoGallery 解决方案具有在上一练习中创建的 ASP.NET MVC 4 移动应用程序的新功能。

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>任务 1-ASP.NET MVC 4 应用程序中的安装 jQuery Mobile

1. 打开**开始**解决方案位于**源/Ex3-MobileSupport/开始/**文件夹。 否则，可能会继续使用**结束**解决方案获取通过完成上一练习。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
2. 打开**程序包管理器控制台**通过单击**工具** &gt; **库程序包管理器** &gt; **程序包管理器控制台**菜单选项。

    ![打开 NuGet 包管理器控制台](whats-new-in-aspnet-mvc-4/_static/image22.png "打开 NuGet 包管理器控制台")

    *打开 NuGet 包管理器控制台*
3. 在程序包管理器控制台中运行以下命令以安装**jQuery.Mobile.MVC**包。

    jQuery Mobile 是用于构建触控优化 web UI 一个开放源代码库。 JQuery.Mobile.MVC NuGet 程序包包括帮助器 jQuery Mobile 用于 ASP.NET MVC 4 应用程序。

    > [!NOTE]
    > 通过运行以下命令，你将下载 jQuery.Mobile.MVC 库从 Nuget。

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    此命令将安装 jQuery Mobile 和一些帮助程序文件，其中包括：

    - **视图/共享/\_Layout.Mobile.cshtml**： 是针对较小屏幕进行了优化的 jQuery 基于移动的布局。 当网站收到请求时从移动浏览器时，它将替换原始布局 (\_Layout.cshtml) 替换为此。
    - 视图切换器组件： 组成**视图/共享/\_ViewSwitcher.cshtml**分部视图和**ViewSwitcherController.cs**控制器。 此组件将在移动浏览器以使用户能够切换到桌面版本的页面上显示的链接。  
        ![移动支持照片库项目](whats-new-in-aspnet-mvc-4/_static/image23.png "移动支持照片库项目")

        *移动支持照片库项目*
4. 注册移动捆绑包。 若要执行此操作，打开**Global.asax.cs**文件并添加以下行。

    (代码段- *ASP.NET MVC 4 实验室-Ex03-注册移动捆绑*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. 运行使用桌面 web 浏览器的应用程序。
6. 打开**Windows Phone 7 仿真程序中，**位于**开始菜单 |所有程序 |Windows Phone SDK 7.1 |Windows Phone 仿真程序。**
7. 在 phone 开始屏幕中，打开 Internet Explorer。 签出启动该应用程序的 URL 并浏览到 phone 浏览器使用该 URL (例如`http://localhost:[PortNumber]/`)。

    你将注意到： 你的应用程序将看起来不同在 Windows Phone 模拟器中，如 jQuery.Mobile.MVC 具有显示针对移动设备进行了优化的视图在项目中创建新的资产。

    注意在手机上，显示的链接，切换到桌面视图顶部的消息。 此外，  **\_Layout.Mobile.cshtml**由已安装的包的布局应用程序中包括一个不同的布局。

    > [!NOTE]
    > 到目前为止，没有要返回到移动视图的链接。 它将包括在更高版本。

    ![照片库主页上的移动视图](whats-new-in-aspnet-mvc-4/_static/image24.png "移动视图的照片库主页")

    *移动视图的照片库主页*
8. 在 Visual Studio 中，按**SHIFT** + **F5**来停止调试应用程序。

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>任务 2-创建移动视图

在此任务中，将使用适用于移动设备中的更好 appareance 的内容创建索引视图的移动版本。

1. 复制**Views\Home\Index.cshtml**查看并将其创建的副本，请重命名新文件与粘贴**Index.Mobile.cshtml**。
2. 打开新创建**Index.Mobile.cshtml**查看和将现有&lt;ul&gt;标记替换为以下代码。 通过执行此操作，你将更新&lt;ul&gt;标记中使用 jQuery 移动数据注释，用于从 jQuery 移动的主题。


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > 请注意：
    > 
    > - **数据角色**属性设置为**listview**将呈现使用 listview 样式的列表。
    > 
    > - **数据内嵌**属性设置为 true 将显示该列表与圆角的边框和边距。
    > 
    > - **数据筛选器**属性设置为**true**将生成一个搜索框。
    > 
    > 你可以了解有关项目文档中的 jQuery 移动约定的详细信息： [ [http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. 按**CTRL + S**以保存所做的更改。
4. 切换到**Windows Phone 仿真程序**和刷新站点。 请注意新的外观和感觉的库列表中，以及新的搜索框位于顶部。 然后，在搜索框中键入一个词 (例如，**郁金香**) 照片库中开始搜索。

    ![使用 listview 样式筛选的库](whats-new-in-aspnet-mvc-4/_static/image25.png "库使用 listview 样式筛选")

    *使用 listview 样式筛选的库*

    总之，你使用的视图可持续配方创建索引视图中处理的副本&quot;移动&quot;后缀。 此后缀指示 ASP.NET MVC 4，从移动设备生成的每个请求将使用此副本的索引。 此外，您已更新要使用 jQuery Mobile 增强站点外观和感觉，移动设备中的索引视图的移动版本。
5. 返回到 Visual Studio 和打开**Site.Mobile.css**位于**内容**文件夹。
6. 修复该照片标题，以使其显示在右侧的映像的位置。 若要执行此操作，将添加到下面的代码**Site.Mobile.css**文件。

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. 按**CTRL + S**以保存所做的更改。
8. 切换回**Windows Phone 仿真程序**和刷新站点。 请注意照片标题正确现在定位。

    ![定位图像右侧标题](whats-new-in-aspnet-mvc-4/_static/image26.png "定位图像右侧的标题")

    *定位图像右侧的标题*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>任务 3-jQuery Mobile 主题

每个布局和小组件中 jQuery Mobile 在于，围绕设计就将完成统一的可视化设计主题应用于站点和应用程序新的面向对象的 CSS 框架。

jQuery Mobile 默认情况下主题包括 5 样本是否指定字母 (a、 b、 c、 d、 e） 供快速引用。

在此任务中，你将更新要使用不同的主题，而不是默认的移动布局。

1. 切换回 Visual Studio。
2. 打开 **\_Layout.Mobile.cshtml**文件位于**views/shared**。
3. 使用数据的角色设置为找到的 div 元素&quot;页&quot;和更新**数据主题**到&quot; **e**&quot;。


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. 按**CTRL + S**以保存所做的更改。
5. 刷新中的站点**Windows Phone 仿真程序**并留意新的颜色方案。

    ![具有不同的配色方案的移动布局](whats-new-in-aspnet-mvc-4/_static/image27.png "具有不同的配色方案的移动布局")

    *具有不同的配色方案的移动布局*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>任务 4-使用视图切换器组件和重写功能的浏览器

移动优化网页中的约定是将添加其文本是如桌面视图或完整的站点模式，从而让用户切换到该页面的桌面版本的链接。 JQuery.Mobile.MVC 包包括一个示例，**视图切换器**组件为用于此目的 **\_Layout.Mobile.cshtml**视图。

![链接以切换到桌面视图](whats-new-in-aspnet-mvc-4/_static/image28.png "链接以切换到桌面视图")

*用于切换到桌面视图的链接*

视图切换器使用称为的新增功能**浏览器重写**。 此功能允许你的应用程序处理请求，就像它们来自于它们实际来自其他浏览器 （用户代理）。

在此任务中，您将了解视图切换器添加 jQuery.Mobile.MVC 和重写 ASP.NET MVC 4 中的功能的新浏览器的示例实现。

1. 切换回 Visual Studio。
2. 打开 **\_Layout.Mobile.cshtml**视图位于**views/shared**文件夹，请注意为分部视图所引用的视图切换器组件。

    ![使用视图切换器组件的移动布局](whats-new-in-aspnet-mvc-4/_static/image29.png "使用视图切换器组件的移动布局")

    *使用视图切换器组件的移动布局*
3. 打开 **\_ViewSwitcher.cshtml**分部视图。

    分部视图使用的新方法**ViewContext.HttpContext.GetOverriddenBrowser()**确定 web 请求的源，并显示相应的链接可切换到桌面或移动视图。

    **GetOverridenBrowser**方法返回**HttpBrowserCapabilitiesBase**对应于当前为请求设置的用户代理的实例 （实际或重写）。 你可以使用此值以获取属性，如**IsMobileDevice**。

    ![ViewSwitcher 分部视图](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher 分部视图")

    *ViewSwitcher 分部视图*
4. 打开**ViewSwitcherController.cs**类位于**控制器**文件夹。 签出该 SwitchView 操作由 ViewSwitcher 组件中的链接，并注意新的 HttpContext 方法。

    - **HttpContext.ClearOverridenBrowser()**方法移除当前请求的任何重写的用户代理。
    - **HttpContext.SetOverridenBrowser()**方法重写请求的实际用户代理值使用指定的用户代理。  
        ![ViewSwitcher 控制器](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher 控制器")  
*ViewSwitcher 控制器*

        浏览器重写是一项核心功能的 ASP.NET MVC 4 中，即使你不安装 jQuery.Mobile.MVC 包，这是也可用。 但是，此功能会影响仅视图、 布局和分部视图，并且它不影响任何依赖于 Request.Browser 对象的功能。

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>任务 5-在桌面视图中添加视图切换器

在此任务中，你将更新桌面的布局，以包括视图切换器。 这将允许移动用户在浏览桌面视图时返回到移动视图。

1. 刷新中的站点**Windows Phone 仿真程序**。
2. 单击**桌面视图**顶部的库的链接。 请注意在桌面的视图，从而使你返回到移动视图中没有任何视图切换器。
3. 返回到 Visual Studio 和打开 **\_Layout.cshtml**视图。
4. 查找登录部分和插入的调用来呈现 **\_ViewSwitcher**下面的部分视图 **\_LogOnPartial**分部视图。 然后，按**CTRL + S**以保存所做的更改。


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. 按**CTRL + S**以保存所做的更改。
6. 刷新 Windows Phone 仿真程序中的页，然后双击以放大屏幕。 请注意，主页页面现在显示**移动视图**从移动切换到桌面视图的链接。

    ![查看在桌面视图中呈现的切换器](whats-new-in-aspnet-mvc-4/_static/image32.png "在桌面视图中呈现的视图切换器")

    *在桌面视图中呈现的视图切换器*
7. 重新切换到移动视图，并浏览到**有关**页 （http://localhost [端口] / Home/有关）。 请注意，即使你尚未创建 About.Mobile.cshtml 视图，关于页面显示使用移动布局 (\_Layout.Mobile.cshtml)。

    ![有关页面](whats-new-in-aspnet-mvc-4/_static/image33.png "有关页面")

    *有关页面*
8. 最后，在桌面的 Web 浏览器中打开网站。 请注意的任何以前的更新影响桌面视图。

    ![图片库桌面视图](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery 桌面视图")

    *图片库桌面视图*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>任务 6-创建新的显示模式

使用新的显示模式功能，应用程序选择具体取决于正在生成请求的浏览器的视图。 例如，如果桌面浏览器请求主页上，该应用程序将返回**Views\Home\Index.cshtml**模板。 然后，如果移动浏览器请求主页上，该应用程序将返回**Views\Home\Index.mobile.cshtml**模板。

在此任务中，你将创建的自定义的布局的 iPhone 设备，并必须以模拟 iPhone 设备发出的请求。 若要执行此操作，可以使用任一一个 iPhone 仿真程序/仿真程序 (如[Electric 移动模拟器](http://www.electricplum.com/)) 或外接程序中修改用户代理使用的浏览器。 有关如何在 Safari 浏览器中设置的用户代理字符串以模拟 iPhone 的说明，请参阅[如何让 Safari 模拟很 IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) David Alison 的博文中。

**请注意，此任务是可选的可以在实验室整个继续而不执行它。**

1. 在 Visual Studio 中，按**SHIFT** + **F5**来停止调试应用程序。
2. 打开**Global.asax.cs**并添加以下 using 语句。


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. 将以下突出显示的代码添加到应用程序\_Start 方法。

    (代码段- *ASP.NET MVC 4 实验室-Ex03-iPhone DisplayMode*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

    你注册一个新**DefaultDisplayMode 名为&quot;iPhone&quot;**，静态内**DisplayModeProvider.Instance.Modes**静态列表中，将与匹配每个传入请求。 如果传入的请求包含字符串&quot;iPhone&quot;，ASP.NET MVC 将查找其名称包含的视图&quot;iPhone&quot;后缀。 0 的参数指示如何特定是新的模式;例如，此视图是比常规更具体&quot;.mobile&quot;与移动设备发出的请求匹配的规则。

    此代码运行时为 iPhone 浏览器生成请求后，将使用你的应用程序**views/shared\\_Layout.iPhone.cshtml**将在后续步骤中创建的布局。

    > [!NOTE]
    > 这种测试请求的 iPhone 经过简化，出于演示目的，可能无法按预期进行每个 iPhone 用户代理字符串 （对应于示例测试是区分大小写） 的方式。
4. 创建一份 **\_Layout.Mobile.cshtml**文件中**views/shared**文件夹并将复制到重命名&quot;  **\_Layout.iPhone.csthml**&quot;.
5. 打开 **\_Layout.iPhone.csthml**你在上一步中创建。
6. 将数据角色属性设置为使用找到的 div 元素**页**和更改**数据主题**属性设为&quot; &quot;。


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

    现在你的 ASP.NET MVC 4 应用程序中有 3 布局：

    1. **\_Layout.cshtml**： 用于桌面浏览器的默认布局。
    2. **\_Layout.mobile.cshtml**： 用于移动设备的默认布局。
    3. **\_Layout.iPhone.cshtml**： 对于 iPhone 设备，使用不同的配色方案来区分开来的特定布局\_Layout.mobile.cshtml。
7. 按**F5**运行该应用程序和浏览中的站点**Windows Phone 仿真程序**。
8. 打开**iPhone 模拟器**(请参阅[附录 C](#AppendixC)有关如何安装和配置 iPhone 模拟器的说明)，和太浏览到该网站。 请注意，每个 phone 使用特定的模板。

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *对于每个移动设备使用不同的视图*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>练习 4： 使用异步控制器

Microsoft.NET Framework 4.5 引入了以 C# 和 Visual Basic 中的.NET 编程异步提供新的基础的新语言功能。 此新 foundation 能使异步编程，类似于-也大约像简单-同步编程。 你现可通过使用 ASP.NET MVC 4 中编写的异步操作方法**AsyncController**类。 可以使用的长时间运行的异步操作方法，非 CPU 绑定的请求。 这样可避免阻止 Web 服务器在处理请求时执行工作。 AsyncController 类通常用于长时间运行的 Web 服务调用。

本练习中说明 ASP.NET MVC 4 中的异步操作的基础的知识。 如果你希望更深入的了解，你可以查看以下文章： [ [https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>任务 1-实现异步控制器

1. 打开**开始**解决方案位于**源/Ex4-Async/开始/**文件夹。 否则，可能会继续使用**结束**解决方案获取通过完成上一练习。

    1. 如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
    2. 在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。
    3. 最后，通过单击生成解决方案**生成** | **生成解决方案**。

    > [!NOTE]
    > 使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。 使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。 这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。
2. 打开**HomeController.cs**类**控制器**文件夹。
3. 添加以下 using 语句。


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. 更新**HomeController**类继承自**AsyncController**。 从 AsyncController 派生的控制器使 ASP.NET 能够处理异步请求和它们仍可以服务的同步操作方法。


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. 添加**异步**关键字**索引**方法并使其返回类型**任务&lt;ActionResult&gt;**。


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > **异步**关键字是.NET Framework 4.5 提供的新关键字之一; 它会告知编译器，此方法包含异步代码。 A**任务**对象表示可能会在将来的某个时候完成一个异步操作。
6. 替换**客户端。GetAsync()**调用具有完整的异步版本使用 await 关键字，如下所示。

    (代码段- *ASP.NET MVC 4 实验室-Ex04-GetAsync*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > 在以前版本中，你已使用**结果**属性从**任务**要阻止线程，直至 （同步版本），则返回的结果对象。
    > 
    > 添加**await**关键字告知编译器以异步方式等待从方法调用返回的任务。 这意味着只有在处于等待状态的方法完成后将为回调执行代码的其余部分。 需要注意的另一件事情是你不需要更改才能使此工作的 try catch 块： 发生这种情况中背景或前景中的异常将仍然会捕获而无需使用处理程序框架提供的任何额外工作。
7. 更改代码以继续进行的异步实现通过将替换为新的代码行，如下所示

    (代码段- *ASP.NET MVC 4 实验室-Ex04-ReadAsStringAsync*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. 运行该应用程序。 你将注意到没有较大的变化，但你的代码不会阻止线程池中进行的服务器资源的更好的利用率并提高性能的线程。

    > [!NOTE]
    > 你可以了解有关在实验室中新的异步编程功能&quot;**使用 C# 和 Visual Basic.NET 4.5 中的异步编程**&quot;包含在 Visual Studio 培训工具包。

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>任务 2-处理的取消标记的超时时间

返回 Task 实例的异步操作方法还可以支持超时。 在此任务中，你将更新索引方法代码，来处理超时方案中使用取消标记。

1. 返回到 Visual Studio，然后按**SHIFT + F5**来停止调试。
2. 添加以下 using 语句到**HomeController.cs**文件。


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. 更新索引操作，即可接收**CancellationToken**自变量。


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. 更新**GetAsync**调用即可传递的取消标记。

    (代码段- *CancellationToken 与 ASP.NET MVC 4 实验室-Ex04-SendAsync*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. 修饰*索引*方法替换**AsyncTimeout**属性设置为 500 毫秒和**HandleError**属性配置为以处理**TaskCanceledException**通过重定向到**TimedOut**视图。

    (代码段- *ASP.NET MVC 4 实验室-Ex04-特性*)


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. 打开**PhotoController**类和更新**库**方法延迟执行 1000年毫秒 （1 秒），以模拟长时间运行的任务。


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. 打开**Web.config**文件，并通过添加下面的元素启用自定义错误。


    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. 创建新视图在**views/shared**名为**TimedOut**并使用默认的布局。 在解决方案资源管理器，右键单击**views/shared**文件夹，然后选择**添加 |视图**。

    ![对于每个移动设备使用不同的视图](whats-new-in-aspnet-mvc-4/_static/image36.png "为每个移动设备使用不同的视图")

    *对于每个移动设备使用不同的视图*
9. 更新**TimedOut**查看内容，如下所示。


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. 运行应用程序并导航到根 URL。 如已添加**Thread.Sleep**为 1000年毫秒，则会超时错误，生成的**AsyncTimeout**属性，并通过捕获**HandleError**属性。

    ![处理的超时异常](whats-new-in-aspnet-mvc-4/_static/image37.png "超时异常处理")

    *超时异常处理*

> [!NOTE]
> 此外，你可以部署此应用程序对 Windows Azure 网站以下[附录 d： 发布 ASP.NET MVC 4 应用程序使用 Web Deploy](#AppendixD)。


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>摘要

在此动手实验，您已观察到的一些新功能驻留在 ASP.NET MVC 4。 讨论了以下概念：

- 充分利用 ASP.NET MVC 项目模板-包括新的移动应用程序项目模板的增强功能
- 使用 HTML5 视区属性和 CSS 媒体查询来改善在移动设备上的显示
- 使用 jQuery Mobile，来渐进式增强功能并构建触控优化 web UI
- 创建移动特定的视图
- 视图切换器组件用于应用程序中的移动和桌面视图之间切换
- 创建使用任务支持异步控制器

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>附录 a： 使用代码片段

和代码片段，必须将你能够轻松获得所需的所有代码所示。 实验室文档将告诉您完全时你可以使用它们，如下图中所示。

![使用 Visual Studio 代码段将代码插入到你的项目](whats-new-in-aspnet-mvc-4/_static/image38.png "使用 Visual Studio 代码片段，可将代码插入到你的项目")

*使用 Visual Studio 代码段将代码插入到你的项目*

***若要添加代码片段使用键盘 (仅限 C#)***

1. 将光标置于想要插入代码。
2. 开始键入代码段名称 （不带空格或连字符）。
3. 观看作为 IntelliSense 显示匹配的代码段的名称。
4. 选择正确的代码段 （或继续键入直到选中整个代码段的名称）。
5. 按 Tab 键两次以光标位置处插入代码段。

![开始键入代码段名称](whats-new-in-aspnet-mvc-4/_static/image39.png "开始键入代码段名称")

*开始键入代码段名称*

![按 Tab 以选择突出显示代码段](whats-new-in-aspnet-mvc-4/_static/image40.png "按选项卡以选择突出显示代码段")

*按 Tab 以选择突出显示代码段*

![再次按 Tab 和代码段将会扩展](whats-new-in-aspnet-mvc-4/_static/image41.png "再次按 Tab 和代码段将会扩展")

*再次按 Tab 和代码段将会扩展*

***若要添加代码片段使用鼠标 （C#、 Visual Basic 和 XML）***

1. 右键单击你想要插入代码段。
2. 选择**插入代码段**跟**我的代码段**。
3. 选择相关的代码段从列表中，通过单击它。

![右键单击你想要插入代码段，并选择插入代码段](whats-new-in-aspnet-mvc-4/_static/image42.png "右键单击你想要插入代码段，并选择插入代码段")

*右键单击你想要插入代码段，并选择插入代码段*

![通过单击它选取列表中中的相关代码片段](whats-new-in-aspnet-mvc-4/_static/image43.png "选取相关的代码段从列表中，通过单击它")

*通过单击它选取从列表中，相关代码段*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>附录 b： 安装 Visual Studio Express 2012 for Web

你可以安装**Microsoft Visual Studio Express 2012 for Web**或另一个&quot;Express&quot;版本使用 **[Microsoft Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)**. 以下说明将指导你完成安装所需的步骤*Visual studio Express 2012 for Web*使用*Microsoft Web 平台安装程序*。

1. 转到[ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。 或者，如果你已安装 Web 平台安装程序，你可以打开它，并搜索产品&quot; *Visual Studio Express 2012 for Web 的 Windows Azure SDK*&quot;。
2. 单击**立即安装**。 如果你没有**Web 平台安装程序**将重定向以下载并请先安装它。
3. 一次**Web 平台安装程序**处于打开状态，单击**安装**以启动安装程序。

    ![安装 Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "安装 Visual Studio Express")

    *安装 Visual Studio Express*
4. 阅读所有产品的许可证和条款，然后单击**我接受**以继续。

    ![接受许可条款](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *接受许可条款*
5. 等待，直到下载和安装过程完成。

    ![安装进度](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *安装进度*
6. 当安装完成后时，单击**完成**。

    ![安装已完成](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *安装已完成*
7. 单击**退出**以关闭 Web 平台安装程序。
8. 若要打开 Visual Studio Express for Web，请转到**启动**屏幕并开始编写&quot; **VS Express**&quot;，然后单击**VS Express for Web**磁贴。

    ![Web 磁贴的 VS Express](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *Web 磁贴的 VS Express*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>附录 c： 安装 WebMatrix 2 和 iPhone 模拟器

若要模拟的 iPhone 设备中运行你的站点可以使用 WebMatrix 扩展&quot;适用于 iPhone 的电力移动模拟器&quot;。 此外，你可以配置要从 Visual Studio 2012 中运行模拟器的相同扩展。

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>任务 1-安装 WebMatrix 2

1. 转到[ [https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169)。 或者，如果你已安装 Web 平台安装程序，你可以打开它，并搜索产品&quot; *WebMatrix 2*&quot;。
2. 单击**立即安装**。 如果你没有**Web 平台安装程序**将重定向以下载并请先安装它。
3. 一次**Web 平台安装程序**处于打开状态，单击**安装**以启动安装程序。

    ![安装 WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "安装 WebMatrix 2")

    *安装 WebMatrix 2*
4. 阅读所有产品的许可证和条款，然后单击**我接受**以继续。

    ![接受许可条款](whats-new-in-aspnet-mvc-4/_static/image50.png "接受许可条款")

    *接受许可条款*
5. 等待，直到下载和安装过程完成。

    ![安装进度](whats-new-in-aspnet-mvc-4/_static/image51.png "安装进度")

    *安装进度*
6. 当安装完成后时，单击**完成**。

    ![安装已完成](whats-new-in-aspnet-mvc-4/_static/image52.png "安装已完成")

    *安装已完成*
7. 单击**退出**以关闭 Web 平台安装程序。

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>任务 2-安装 iPhone 模拟器扩展

1. 运行**WebMatrix**和打开任何现有的网站或创建一个新。
2. 单击**运行**按钮从**主页**功能区，然后选择**添加新**。

    ![添加新的 WebMatrix 扩展](whats-new-in-aspnet-mvc-4/_static/image53.png "添加新的 WebMatrix 扩展")

    *添加新的 WebMatrix 扩展*
3. 选择**iPhone 模拟器**单击**安装**。

    ![浏览 WebMatrix 扩展](whats-new-in-aspnet-mvc-4/_static/image54.png "浏览 WebMatrix 扩展")

    *浏览 WebMatrix 扩展*
4. 在包的详细信息，单击**安装**继续扩展安装。

    ![iPhone 模拟器扩展](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone 模拟器扩展")

    *iPhone 模拟器扩展*
5. 阅读并接受最终用户许可协议的扩展。

    ![WebMatrix 扩展 EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix 扩展 EULA")

    *WebMatrix 扩展 EULA*
6. 现在，可以从 WebMatrix 中运行您的网站使用 iPhone 模拟器选项。

    ![使用 iPhone 运行](whats-new-in-aspnet-mvc-4/_static/image57.png "使用 iPhone 运行")

    *使用 iPhone 运行*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>任务 3-配置 Visual Studio 2012 运行 iPhone 模拟器

1. 打开**Visual Studio 2012**和打开的任何网站或创建新项目。
2. 单击运行按钮从的向下箭头，然后选择**浏览与**。

    ![浏览与](whats-new-in-aspnet-mvc-4/_static/image58.png "浏览与")

    *浏览与*
3. 在&quot;浏览&quot;对话框中，单击**添加**。
4. 在&quot;添加程序&quot;对话框中，使用以下值：

    - **程序**: C:\Users\*{CurrentUser}*\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *（相应地更新的路径）*
    - **自变量**: &quot;1&quot;
    - **友好名称**: iPhone 模拟器

    ![添加程序](whats-new-in-aspnet-mvc-4/_static/image59.png "添加程序")

    *添加程序与浏览*
5. 单击**确定**并关闭对话框。
6. 现在，你就能够从 Visual Studio 2012 在 iPhone 模拟器中运行 Web 应用程序。

    ![浏览方式 iPhone 模拟器](whats-new-in-aspnet-mvc-4/_static/image60.png "浏览方式 iPhone 模拟器")

    *浏览方式 iPhone 模拟器*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>附录 d： 发布 ASP.NET MVC 4 应用程序使用 Web 部署

本附录将演示如何从 Windows Azure 管理门户创建新的网站和发布应用程序获取按照本实验中，利用 Windows Azure 提供的 Web 部署发布功能。

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>任务 1-创建新的 Web 站点从 Windows Azure 门户

1. 转到[Windows Azure 管理门户](https://manage.windowsazure.com/)并使用与你的订阅关联的 Microsoft 凭据登录。

    > [!NOTE]
    > 使用 Windows Azure 可以免费承载 10 个 ASP.NET 网站，然后随着流量增长而扩展。 你可以注册[此处](http://aka.ms/aspnet-hol-azure)。

    ![登录到 Windows Azure 门户](whats-new-in-aspnet-mvc-4/_static/image61.png "登录到 Windows Azure 门户")

    *登录到 Windows Azure 管理门户*
2. 单击**新建**命令栏上。

    ![创建新网站](whats-new-in-aspnet-mvc-4/_static/image62.png "创建新网站")

    *创建新网站*
3. 单击**计算** | **网站**。 然后选择**快速创建**选项。 为新网站提供可用的 URL，然后单击**创建网站**。

    > [!NOTE]
    > Windows Azure 网站是在云中，你可以控制和管理运行的 web 应用程序的宿主。 快速创建选项，可将已完成的 web 应用程序到 Windows Azure 网站从在门户外部部署。 它不包括用于设置数据库的步骤。

    ![创建新的网站使用快速创建](whats-new-in-aspnet-mvc-4/_static/image63.png "创建新的网站使用快速创建")

    *创建新的网站使用快速创建*
4. 等到新**网站**创建。
5. 创建网站后单击下的链接**URL**列。 检查新的 Web 站点工作。

    ![浏览到新的 web 站点](whats-new-in-aspnet-mvc-4/_static/image64.png "浏览到新的 web 站点")

    *浏览到新的 web 站点*

    ![运行网站](whats-new-in-aspnet-mvc-4/_static/image65.png "运行的网站")

    *运行的网站*
6. 返回到门户并单击在网站的名称**名称**列以显示的管理页。

    ![打开网站管理页](whats-new-in-aspnet-mvc-4/_static/image66.png "打开网站管理页")

    *打开网站管理页*
7. 在**仪表板**页上，在**速览**部分中，单击**下载发布配置文件**链接。

    > [!NOTE]
    > *发布配置文件*包含所有 web 应用程序发布到 Windows Azure 网站中的每个启用的发布方法所需的信息。 发布配置文件包含的 Url、 用户凭据和连接到并针对每个发布方法启用的终结点进行身份验证所需的数据库字符串。 **Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**和**Microsoft Visual Studio 2012**支持读取发布配置文件以自动执行的这些程序，以便配置web 应用程序发布到 Windows Azure 网站。

    ![下载网站发布配置文件](whats-new-in-aspnet-mvc-4/_static/image67.png "下载网站发布配置文件")

    *下载网站发布配置文件*
8. 到一个已知位置下载发布配置文件。 进一步在本练习中您将了解如何使用此文件来发布 Visual Studio 中的 web 应用程序到 Windows Azure 网站。

    ![保存发布配置文件](whats-new-in-aspnet-mvc-4/_static/image68.png "保存发布配置文件")

    *保存发布配置文件*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>任务 2-配置数据库服务器

如果你的应用程序将使用 SQL Server 数据库将需要创建 SQL 数据库服务器。 如果你想要部署的简单应用程序不使用 SQL Server 可能会跳过此任务。

1. 需要将用于存储应用程序数据库的 SQL 数据库服务器。 你可以从你的订阅在 Windows Azure 管理门户中查看 SQL 数据库服务器**Sql 数据库** | **服务器** | **服务器的仪表板**。 如果你没有创建的服务器，则可以创建一个使用**添加**命令栏上的按钮。 请记下的**服务器名称和 URL、 管理员登录名和密码**，如你将在接下来的任务中使用它们。 不创建数据库，它将在后面的阶段中创建。

    ![SQL 数据库服务器仪表板](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database 服务器仪表板")

    *SQL 数据库服务器仪表板*
2. 在下一个任务将测试从 Visual Studio 中，数据库连接，因此你需要在的服务器的列表中包括你的本地 IP 地址**允许的 IP 地址**。 若要做到这一点，请单击**配置**，选择的 IP 地址从**当前客户端 IP 地址**并将其粘贴在**起始 IP 地址**和**结束 IP 地址**文本框和单击![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png)按钮。

    ![添加客户端 IP 地址](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *添加客户端 IP 地址*
3. 一次**客户端 IP 地址**添加到允许的 IP 地址列表中，单击**保存**以确认所做的更改。

    ![确认做的更改](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *确认做的更改*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>任务 3-发布 ASP.NET MVC 4 应用程序使用 Web 部署

1. 返回到 ASP.NET MVC 4 解决方案。 在**解决方案资源管理器**，右键单击网站项目，然后选择**发布**。

    ![发布应用程序](whats-new-in-aspnet-mvc-4/_static/image73.png "发布应用程序")

    *发布此网站*
2. 导入发布配置文件保存在第一个任务。

    ![导入发布配置文件](whats-new-in-aspnet-mvc-4/_static/image74.png "导入发布配置文件")

    *导入发布配置文件*
3. 单击**验证连接**。 验证完成后单击**下一步**。

    > [!NOTE]
    > 请参阅验证连接按钮旁边将显示绿色的复选标记后，验证已完成。

    ![正在验证连接](whats-new-in-aspnet-mvc-4/_static/image75.png "验证连接")

    *正在验证连接*
4. 在**设置**页上，在**数据库**部分中，单击你的数据库连接的文本框旁边的按钮 (即**DefaultConnection**)。

    ![Web 部署配置](whats-new-in-aspnet-mvc-4/_static/image76.png "Web 部署配置")

    *Web 部署配置*
5. 配置数据库连接，如下所示：

    - 在**服务器名称**类型 SQL 数据库服务器 URL 使用*tcp:*前缀。
    - 在**用户名**键入您的服务器管理员登录名。
    - 在**密码**键入服务器管理员登录密码。
    - 键入新的数据库名称，例如： *MVC4SampleDB*。

    ![配置目标连接字符串](whats-new-in-aspnet-mvc-4/_static/image77.png "配置目标连接字符串")

    *配置目标连接字符串*
6. 然后单击“确定” 。 当系统提示创建数据库单击**是**。

    ![创建数据库](whats-new-in-aspnet-mvc-4/_static/image78.png "创建数据库字符串")

    *创建数据库*
7. 在默认连接文本框中显示了将用于连接到 Windows Azure 中的 SQL 数据库连接字符串。 然后，单击 **“下一步”**。

    ![连接字符串指向 SQL 数据库](whats-new-in-aspnet-mvc-4/_static/image79.png "指向 SQL 数据库的连接字符串")

    *连接字符串指向 SQL 数据库*
8. 在**预览**页上，单击**发布**。

    ![Web 应用程序发布](whats-new-in-aspnet-mvc-4/_static/image80.png "发布 web 应用程序")

    *发布 web 应用程序*
9. 完成发布过程后，默认浏览器将打开已发布的网站。

    ![应用程序发布到 Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "应用程序发布到 Windows Azure")

    *应用程序发布到 Windows Azure*
