---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: "创建一个基本的 ASP.NET 4.5 Web 窗体页在 Visual Studio 2013 |Microsoft 文档"
author: Erikre
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: 6b699cc939292b7ab0167dba7cfa6a00b681ef3a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-basic-aspnet-45-web-forms-page-in-visual-studio-2013"></a>创建一个基本的 ASP.NET 4.5 Web 窗体页在 Visual Studio 2013
====================
通过[艾力克 Reitan](https://github.com/Erikre)

本演练为您提供的简介中的 Web 开发环境[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)并在[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。 本演练指导您完成创建简单的 ASP.NET Web 窗体页，并阐释的创建一个新页面、 添加控件，以及编写代码的基本技术。

本演练涉及以下任务：

- 创建文件系统 Web 窗体应用程序项目。
- 您熟悉 Visual Studio。
- 创建 ASP.NET 页。
- 添加控件。
- 添加事件处理程序。
- 运行和测试页上，从 Visual Studio。

## <a name="prerequisites"></a>系统必备


若要完成本演练，你将需要：

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)或[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。 自动安装.NET Framework。 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 和 Microsoft Visual Studio Express 2013 for Web 将通常被称为 Visual Studio 在本教程系列整个。  
    >   
    > 如果你使用的 Visual Studio，本演练假定你所选**Web 开发**设置的集合，首次启动 Visual Studio。 有关详细信息，请参阅[如何： 选择 Web 开发环境设置](https://msdn.microsoft.com/library/ff521558.aspx)。


## <a name="creating-a-web-application-project-and-a-page"></a>创建 Web 应用程序项目和页面

<a id="sectionToggle0"></a>

在演练本部分，将创建一个 Web 应用程序项目，并向其添加一个新页。 你还将添加 HTML 文本，并在浏览器中运行页面。

### <a name="to-create-a-web-application-project"></a>创建 Web 应用程序项目

1. 打开 Microsoft Visual Studio。
2. 在“文件”菜单上，选择“新建项目”。  
    ![文件菜单](creating-a-basic-web-forms-page/_static/image1.png)

    此时将出现 “新建项目” 对话框。
3. 选择**模板** - &gt; **Visual C#**  - &gt; **Web**左侧的模板组。
4. 选择**ASP.NET Web 应用程序**中心列中的模板。
5. 命名你的项目***BasicWebApp***单击**确定**按钮。   
![新建项目对话框](creating-a-basic-web-forms-page/_static/image2.png)
6. 接下来，选择**Web 窗体**模板，然后单击**确定**按钮以创建该项目。  
![新建 ASP.NET 项目对话框](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio 创建新的项目包含在基于 Web 窗体模板的预构建的功能。 它不仅可为你提供*Home.aspx*页上， *About.aspx*页上， *Contact.aspx*页上，但还包括成员资格功能注册用户和保存其凭据，以便他们可以登录到你的网站。 创建一个新页后，默认情况下 Visual Studio 将显示中的页**源**视图中，可以看到此页的 HTML 元素的位置。 下图显示将在中看到的内容**源**查看如果创建一个名为的新 Web 页*BasicWebApp.aspx*。  
    ![源视图](creating-a-basic-web-forms-page/_static/image4.png)


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>介绍的 Visual Studio Web 开发环境


通过修改页在继续之前，它可用于熟悉 Visual Studio 开发环境。 下图显示了 windows 和 Visual Studio 和 Visual Studio Express for Web 中可用的工具。

> [!NOTE] 
> 
> 下图显示的默认窗口和窗口位置。 **视图**菜单，可以显示其他窗口和重新排列和调整大小以适合你的首选项。 如果已对窗口排列做出更改，你看到的内容将与此图不一致。


 在 Visual Studio 环境

![Visual Studio 环境](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>熟悉 Web 设计器

检查上图中，并与文本匹配的以下列表中，用于描述了最常使用的窗口和工具。 （不是所有 windows 和你看到此处列出的工具，只有都标记在上图中。）

- 工具栏。 提供用于格式化文本、 查找文本等命令。 仅当你处理时，某些工具栏才可用**设计**视图。
- **解决方案资源管理器**窗口。 Web 应用程序中显示的文件和文件夹。
- 文档窗口。 显示你正在使用选项卡式窗口的文档。 你可以通过单击选项卡的文档之间切换。
- **属性**窗口。 允许您更改页、 HTML 元素、 控件和其他对象的设置。
- 查看选项卡。 向您提供同一文档的不同视图。 **设计**视图是附近的所见即所得的编辑图面。 **源**视图是 HTML 编辑器中的为页。 **拆分**视图将同时显示**设计**视图和**源**文档视图。 您将使用**设计**和**源**稍后在本演练的视图。 如果想要打开中的 Web 页**设计**查看，请在**工具**菜单上，单击**选项**，选择**HTML 设计器**节点，然后更改**起始页位置**选项。
- **工具箱**。 提供控件和可以拖到你的页面上的 HTML 元素。 **工具箱**元素进行分组的公共函数。
- S **erver 资源管理器**。 显示数据库连接。 如果服务器资源管理器不可见，请在视图菜单中，单击服务器资源管理器。


### <a name="creating-a-new-aspnet-web-forms-page"></a>创建新的 ASP.NET Web 窗体页


当你创建新的 Web 窗体应用程序使用**ASP.NET Web 应用程序**项目模板中，Visual Studio 将添加一个名为的 ASP.NET 页 （Web 窗体页） *Default.aspx*，以及为多个其他文件和文件夹。 你可以使用*Default.aspx*页为 Web 应用程序主页的步骤。 但是，对于本演练，你将创建和使用新的页。

### <a name="to-add-a-page-to-the-web-application"></a>要添加到 Web 应用程序页


1. 关闭*Default.aspx*页。 若要执行此操作，单击显示的文件名称的选项卡，然后单击关闭选项。
2. 在**解决方案资源管理器**，右键单击 Web 应用程序名称 (在本教程中的应用程序名称**BasicWebSite**)，然后单击**添加** - &gt;**新项**。   
随即出现“添加新项”对话框。
3. 选择**Visual C#**  - &gt; **Web**左侧的模板组。 然后，选择**Web 窗体**从中间列表并将其命名*FirstWebPage.aspx*。   
    ![添加新项对话框](creating-a-basic-web-forms-page/_static/image6.png)
4. 单击**添加**将网页添加到你的项目。  
Visual Studio 将创建新的页，并将其打开。


### <a name="adding-html-to-the-page"></a>向页面添加 HTML


在演练本部分，你将添加一些静态文本页。

### <a name="to-add-text-to-the-page"></a>将文本添加到页面


1. 在文档窗口的底部，单击**设计**选项卡以切换到**设计**视图。

    设计视图显示类似所见即所得的方式显示当前页。 此时，你没有任何文本或控件在页上，因此页是空白概述了矩形的虚线除外。 将此矩形表示**div**页面上的元素。
2. 概述由虚线该矩形内单击。
3. 类型**欢迎使用 Visual Web Developer**按**ENTER**两次。

    下图显示中键入的文本**设计**视图。

    ![在设计视图中的欢迎文本](creating-a-basic-web-forms-page/_static/image7.png "在设计视图中的欢迎文本")
4. 切换到**源**视图。

    你可以看到在 HTML**源**框中键入时创建的视图**设计**视图。  
    ![包含静态文本的网页](creating-a-basic-web-forms-page/_static/image8.png)


### <a name="running-the-page"></a>运行页


通过将控件添加到页在继续之前，你可以先运行它。

### <a name="to-run-the-page"></a>若要运行页面


1. 在**解决方案资源管理器**，右键单击*FirstWebPage.aspx*和选择**设为起始页**。
2. 按**CTRL + F5**运行页面。

    在浏览器中显示页面。 尽管你创建的页面有的文件名称扩展*.aspx*，它当前运行类似任何 HTML 页。

    若要在浏览器中显示页你还可以右键单击中的页**解决方案资源管理器**和选择**用浏览器查看**。
3. 关闭浏览器来停止 Web 应用程序。


## <a name="adding-and-programming-controls"></a>添加和对控件进行编程


<a id="sectionToggle1"></a>

你现在将向页面添加服务器控件。 服务器控件，如按钮、 标签、 文本框中和其他熟悉的控件，提供 Web 窗体页面的典型窗体处理功能。 但是，你可以在服务器上，而不是客户端运行的代码与对控件进行编程。

你将添加[按钮](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控件，[文本框中](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)控件，和一个[标签](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控件到页，并编写代码来处理[单击](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)事件[按钮](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控件。

### <a name="to-add-controls-to-the-page"></a>若要将控件添加到页


1. 单击**设计**选项卡以切换到**设计**视图。
2. 将插入点放在末尾**欢迎使用 Visual Web Developer**文本并按**ENTER**五个或多个时间，以便在某些腾出空间**div**元素框。
3. 在**工具箱**，展开**标准**组如果尚未展开。  
请注意，你可能需要展开**工具箱**窗口在左侧以查看它。
4. 拖动[文本框中](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)控件拖到页面上并将它的之中放**div**元素框具有**欢迎使用 Visual Web Developer**在第一行中。
5. 拖动[按钮](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控件拖到页面上，并将其放到右侧[文本框中](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)控件。
6. 拖动[标签](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控件拖到页面上并将它放在单独的行下面[按钮](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控件。
7. 将插入点上述[文本框中](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)控制，，然后键入**输入您的姓名：** 。

    此静态 HTML 文本标题是[文本框中](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)控件。 可以混合使用静态 HTML 和同一页面上的服务器控件。 下图显示三个控件中的显示方式**设计**视图。

    ![在设计视图中的三个控件](creating-a-basic-web-forms-page/_static/image9.png "在设计视图中的三个控件")


### <a name="setting-control-properties"></a>设置控件属性


Visual Studio 提供了各种方式来设置页上的控件的属性。 在演练本部分，将属性设置中同时**设计**视图和**源**视图。

### <a name="to-set-control-properties"></a>若要设置控件属性


1. 第一次，显示**属性**通过从选择的 windows**视图**菜单-&gt; **其他窗口** - &gt; **属性窗口**。 或者，你可以选择**F4**以显示**属性**窗口。
2. 选择[按钮](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控件，然后在**属性**窗口中，设置的值**文本**到**显示名称**。 下图中所示，在设计器中，在按钮上显示您输入的文本。

    ![设置按钮文本](creating-a-basic-web-forms-page/_static/image10.png "设置按钮文本")
3. 切换到**源**视图。

    **源**视图显示在该 HTML 页上，包括 Visual Studio 已创建的服务器控件的元素。 控件使用 HTML 类似的语法声明，只不过标记使用前缀**asp:**和的属性包括在**runat =&quot;服务器&quot;**。

    控件属性被声明为属性。 例如，如果设置[文本](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx)属性[按钮](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控件中，在步骤 1 中，实际在设置**文本**在控件的标记中的属性。

    > [!NOTE] 
    > 
    > 所有这些控件都位于**窗体**元素，它还具有属性**runat =&quot;服务器&quot;**。 **Runat =&quot;服务器&quot;**属性和**asp:**前缀将对控件的标记控件标记，以便由进行处理 ASP.NET 在服务器上运行页面时。 外部的代码**&lt;形成 runat =&quot;服务器&quot;&gt;** 和**&lt;脚本 runat =&quot;服务器&quot;&gt;**元素原样发送到浏览器中，这正是 ASP.NET 代码必须在其开始标记中包含元素内**runat =&quot;服务器&quot;**属性。
4. 接下来，将添加到的其他属性[标签](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控件。 将插入点直接后的**asp： 标签**中 **&lt;asp： 标签&gt;**标记，然后按**空格键**。

    下拉列表将出现，显示可用的属性，你可以为设置列表[标签](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控件。 此功能称为**IntelliSense**，可帮助你在**源**服务器控件、 HTML 元素和其他项的语法页面上的视图。 下图显示**IntelliSense**下拉列表[标签](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控件。

    ![IntelliSense 特性](creating-a-basic-web-forms-page/_static/image11.png "IntelliSense 特性")
5. 选择**ForeColor**然后键入等号。

    IntelliSense 会显示颜色的列表。

    > [!NOTE] 
    > 
    > 可以显示**IntelliSense**随时通过按下的下拉列表**CTRL + J**查看代码时。
6. 为选择颜色**[标签](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)**控件的文本。 请确保选择是足够深，以便读取针对白色背景的颜色。

    **ForeColor**属性已完成并且你已选择，包括右引号的颜色。


### <a name="programming-the-button-control"></a>编程按钮控件


对于本演练，你将编写读取用户在文本框中输入，并随后显示中的名称的名称的代码[标签](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控件。

### <a name="add-a-default-button-event-handler"></a>添加一个默认按钮事件处理程序


1. 切换到**设计**视图。
2. 双击[按钮](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控件。

    默认情况下，Visual Studio 切换到代码隐藏文件，并创建主干事件处理程序[按钮](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控件的默认事件[单击](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)事件。 代码隐藏文件 （如 C# 中) 在服务器代码中分隔 UI 标记 （如 HTML)。   
游标位于添加为此事件处理程序的代码。

    > [!NOTE] 
    > 
    > 双击中的控件**设计**视图只是其中一个的几种方法可以创建事件处理程序。
3. 内部**Button1\_单击**事件处理程序中，键入**Label1**跟一个句点 (**。**)。

    当你键入后的期间**ID**的标签 (**Label1**)，Visual Studio 将显示为可用成员列表[标签](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控件，如下所示图。 成员通常的属性、 方法或事件。

    ![在代码视图中的 IntelliSense](creating-a-basic-web-forms-page/_static/image12.png "代码视图中的 IntelliSense")
4. 完成**单击**按钮以便它读取下面的代码示例中所示的事件处理程序。

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. 切换回查看**源**你的 HTML 标记，请右键单击视图*FirstWebPage.aspx*中**解决方案资源管理器**并选择**视图标记**。
6. 滚动到 **&lt;asp： 按钮&gt;**元素。 请注意，  **&lt;asp： 按钮&gt;**元素现在具有属性**onclick =&quot;Button1\_单击&quot;**。

    此属性将绑定按钮的[单击](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)到上一步中编码的处理程序方法的事件。

    事件处理程序方法可以具有任何名称;显示的名称是由 Visual Studio 创建的默认名称。 重要的一点是，该名称用于**OnClick** HTML 中的特性必须与匹配的代码隐藏文件中定义的方法的名称。


### <a name="running-the-page"></a>运行页


你现在可以测试页上的服务器控件。

### <a name="to-run-the-page"></a>若要运行页面


1. 按**CTRL + F5**浏览器中运行页面。 如果发生错误，重新检查上述步骤。
2. 在文本框中输入一个名称，然后单击**显示名称**按钮。

    您输入的名称显示在[标签](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控件。 请注意，当你单击按钮时，页面将被发送到 Web 服务器。 ASP.NET 然后重新创建页上，运行你的代码 (在这种情况下，[按钮](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控件的[单击](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)事件处理程序将运行)，然后将新的页发送到浏览器。 如果你观看浏览器中的状态栏，可以看到，页进行的往返过程到 Web 服务器每次你单击按钮。
3. 在浏览器中，查看你在正在运行的页上右键单击并选择的页面的源**查看源**。

    在页的源代码，而无需任何服务器代码中看到 HTML。 具体而言，你看不见 **&lt;asp:&gt;** 与你共事中的元素**源**视图。 页运行时，ASP.NET 将处理服务器控件，并呈现到页执行的函数，表示控件的 HTML 元素。 例如，  **&lt;asp： 按钮&gt;**控件呈现为 HTML **&lt;输入类型 =&quot;提交&quot;&gt;** 元素。
4. 关闭浏览器。


## <a name="working-with-additional-controls"></a>使用其他控件

<a id="sectionToggle2"></a>

在本部分演练中，您将可以使用[日历](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控件，一次显示一个月的日期。 [日历](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控件是一个更复杂比你一直在处理按钮、 文本框和标签控件，并阐明服务器控件的一些其他功能。

在本部分中，你将添加[System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制到页并设置其格式。

### <a name="to-add-a-calendar-control"></a>若要添加一个日历控件


1. 在 Visual Studio 中，切换到**设计**视图。
2. 从**标准**部分**工具箱**，拖动[日历](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控件拖到页面上并将它放下面**div**元素，包含的其他控件。

    将显示日历的智能标记面板。 该面板显示容易让你可以针对所选控件执行的最常见的任务的命令。 下图显示[日历](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制在呈现**设计**视图。

    ![月历控件在设计视图中](creating-a-basic-web-forms-page/_static/image13.png "月历控件在设计视图中")
3. 在智能标记面板中，选择**自动套用格式**。

    **自动套用格式**显示对话框，该编辑器可以为该日历选择一个格式设置方案。 下图显示**自动套用格式**对话框[日历](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控件。

    ![自动套用格式对话框 （Calendar 控件）](creating-a-basic-web-forms-page/_static/image14.png "自动套用格式对话框 （Calendar 控件）")
4. 从**选择一种方案**列表中，选择**简单**，然后单击**确定**。
5. 切换到**源**视图。

    你可以看到 **&lt;asp： 日历&gt;**元素。 此元素是远远超过前面创建的简单控件的元素。 它还包括子元素，如 **&lt;WeekEndDayStyle&gt;**，分别表示各种格式设置。 下图显示[日历](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)中控制**源**视图。 (请参阅中的确切标记**源**视图可能与该图略有不同。)

    ![月历控件在源视图中](creating-a-basic-web-forms-page/_static/image15.png "月历控件在源视图中")


### <a name="programming-the-calendar-control"></a>编程日历控件


在本部分中，您将编程[日历](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控件来显示当前选定的日期。

### <a name="to-program-the-calendar-control"></a>日历控件编程


1. 在**设计**视图中，双击[日历](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控件。

    创建一个新的事件处理程序并将其显示在代码隐藏文件中名为*FirstWebPage.aspx.cs*。
2. 完成[SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx)事件处理程序替换为以下代码。


    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

 上面的代码将标签控件的文本设置为所选日期的日历控件。


### <a name="running-the-page"></a>运行页


你现在可以测试日历。

### <a name="to-run-the-page"></a>若要运行页面


1. 按**CTRL + F5**浏览器中运行页面。
2. 单击日历中的日期。

    在显示你单击的日期[标签](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控件。
3. 在浏览器中，查看页的源代码。

    请注意，[日历](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控件呈现到页作为**表**，每一天都作为**td**元素。
4. 关闭浏览器。


## <a name="next-steps"></a>后续步骤


<a id="nextStepsToggle"></a>

本演练阐释了 Visual Studio 页设计器的基本功能。 现在，你已了解如何创建和编辑 Visual Studio 中的 Web 窗体页，你可能想要浏览其他功能。 例如，你可能想要执行以下操作：

- 了解有关 ASP.NET Web 窗体的详细信息按照分步教程系列[Getting Started with ASP.NET 4.5 Web 窗体和 Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)。
- 了解有关级联样式表 (CSS)。 有关详细信息，请参阅[使用 CSS 概述](https://msdn.microsoft.com/library/bb398931.aspx)。
