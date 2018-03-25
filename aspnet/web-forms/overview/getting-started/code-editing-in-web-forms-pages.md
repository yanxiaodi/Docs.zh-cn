---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: "在 Visual Studio 2013 的 ASP.NET Web 窗体的代码编辑 |Microsoft 文档"
author: Erikre
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 8714f673cb0434189ca23d2dda14035d8652a051
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>在 Visual Studio 2013 中的代码编辑 ASP.NET Web 窗体
====================
通过[艾力克 Reitan](https://github.com/Erikre)

在许多 ASP.NET Web 窗体页中，你可以编写在 Visual Basic、 C# 中，或另一种语言的代码。 Visual Studio 中的代码编辑器可帮助您快速编写代码，同时帮助您避免错误。 此外，该编辑器还提供使你可以创建可重用的代码的方法来帮助减少你需要执行的工作量。

本演练阐释了 Visual Studio 代码编辑器的各种功能。

在本演练中，你将学会如何执行以下任务：

- 更正内联编码错误。
- 重构和重命名代码。
- 重命名的变量和对象。
- 插入代码段。

## <a name="prerequisites"></a>系统必备


若要完成本演练，你将需要：

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)或[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。 自动安装.NET Framework。 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 和 Microsoft Visual Studio Express 2013 for Web 将通常被称为 Visual Studio 在本教程系列整个。  
    >   
    > 如果你使用的 Visual Studio，本演练假定你所选**Web 开发**设置的集合，首次启动 Visual Studio。 有关详细信息，请参阅[如何： 选择 Web 开发环境设置](https://msdn.microsoft.com/library/ff521558.aspx)。

 Visual Studio 和 ASP.NET 的介绍，请参阅[在 Visual Studio 2013 中创建一个基本的 ASP.NET 4.5 Web 窗体页面](creating-a-basic-web-forms-page.md)。   
 

## <a name="creating-a-web-application-project-and-a-page"></a>创建 Web 应用程序项目和页面

<a id="sectionToggle0"></a>

在演练本部分，将创建一个 Web 应用程序项目，并向其添加一个新页。

### <a name="to-create-a-web-application-project"></a>创建 Web 应用程序项目

1. 打开 Microsoft Visual Studio。
2. 在“文件”菜单上，选择“新建项目”。  
    ![文件菜单](code-editing-in-web-forms-pages/_static/image1.png)

    此时将出现 “新建项目” 对话框。
3. 选择**模板** - &gt; **Visual C#**  - &gt; **Web**左侧的模板组。
4. 选择**ASP.NET Web 应用程序**中心列中的模板。
5. 命名你的项目***BasicWebApp***单击**确定**按钮。   
![新建项目对话框](code-editing-in-web-forms-pages/_static/image2.png)
6. 接下来，选择**Web 窗体**模板，然后单击**确定**按钮以创建该项目。  
![新建 ASP.NET 项目对话框](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio 创建新的项目包含在基于 Web 窗体模板的预构建的功能。


## <a name="creating-a-new-aspnet-web-forms-page"></a>创建新的 ASP.NET Web 窗体页


当你创建新的 Web 窗体应用程序使用**ASP.NET Web 应用程序**项目模板中，Visual Studio 将添加一个名为的 ASP.NET 页 （Web 窗体页） *Default.aspx*，以及为多个其他文件和文件夹。 你可以使用*Default.aspx*页为 Web 应用程序主页的步骤。 但是，对于本演练，你将创建和使用新的页。

### <a name="to-add-a-page-to-the-web-application"></a>要添加到 Web 应用程序页


1. 在**解决方案资源管理器**，右键单击 Web 应用程序名称 (在本教程中的应用程序名称**BasicWebSite**)，然后单击**添加** - &gt;**新项**。   
随即出现“添加新项”对话框。
2. 选择**Visual C#**  - &gt; **Web**左侧的模板组。 然后，选择**Web 窗体**从中间列表并将其命名*FirstWebPage.aspx*。   
    ![添加新项对话框](code-editing-in-web-forms-pages/_static/image4.png)
3. 单击**添加**将 Web 窗体页添加到你的项目。  
 Visual Studio 将创建新的页，并将其打开。
4. 接下来，将此新的页设置为默认启动页。 在**解决方案资源管理器**，右键单击名为的新页*FirstWebPage.aspx*和选择**设为起始页**。 在下次运行此应用程序以测试我们的进度，你将自动看到此浏览器中的新页。


## <a name="correcting-inline-coding-errors"></a>更正内联编码错误


Visual Studio 中的代码编辑器可帮助你避免错误，因为你编写代码，并且如果进行了错误，代码编辑器可帮助你更正此错误。 在演练本部分，你将编写说明在编辑器中的错误更正功能的代码的行。

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>若要更正 Visual Studio 中的简单编码错误


1. 在**设计**视图中，双击空白页后，可以创建的处理程序**负载**页的事件。   
你使用事件处理程序仅作为的位置来编写一些代码。
2. 内部处理程序中，键入以下行，其中包含了错误和按**ENTER**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

 当你按**ENTER**，代码编辑器中将绿色和红色下划线 (通常会调用&quot;波浪&quot;行) 下的代码有问题的区域。 绿色下划线指示警告。 红色下划线指示必须修复错误。 

    将鼠标指针停留在`myStr`若要查看工具提示，告诉你有关该警告。 此外，将鼠标指针悬停红色的下划线，以查看错误消息。

    下图显示带下划线的代码。

    ![在设计视图中的欢迎文本](code-editing-in-web-forms-pages/_static/image5.png "在设计视图中的欢迎文本")  
 必须通过添加分号修复错误`;`至行尾。 警告只会通知您，尚未使用`myStr`尚未变量。  

    > [!NOTE] 
    > 
    > 查看你当前的代码，通过选择格式 Visual Studio 中的设置**工具** - &gt; **选项** - &gt; **字体和颜色**。


## <a name="refactoring-and-renaming"></a>重构和重命名

重构是一种软件方法涉及重构代码以使其更易于理解和维护，同时保留其功能。 一个简单的示例可能是在从数据库中获取数据的事件处理程序中编写代码。 开发你的页面，你发现你需要从多个不同的处理程序访问数据。 因此，通过在页中创建数据访问方法，并在处理程序中插入对方法的调用重构页的代码。

代码编辑器中包括可帮助你执行各种重构任务的工具。 在本演练中，将使用两个重构的方法： 重命名变量以及提取方法。 其他重构选项包括封装字段、 局部变量提升为方法参数和管理方法参数。 这些重构选项的可用性取决于代码中的位置。

### <a name="refactoring-code"></a>重构代码

常见的重构方案是创建 （提取） 位于另一个成员，如方法的代码中的方法。 这会减少原始成员的大小，并使提取的代码可重用。

在演练本部分，你将编写一些简单的代码，然后从中提取方法。 重构支持 C# 中，因此你将创建使用 C# 作为编程语言的页面。

### <a name="to-extract-a-method-in-a-c-page"></a>若要提取的 C# 页中的方法

1. 切换到**设计**视图。
2. 在**工具箱**，从**标准**选项卡上，拖动[按钮](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)拖到页面上的控件。
3. 双击**按钮**控件创建的处理程序其[单击](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)事件，然后添加以下突出显示的代码：

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

 该代码创建**ArrayList**对象，使用循环包含值，加载它，然后使用另一个循环显示的内容**ArrayList**对象。
4. 按**CTRL + F5**以运行此页面，然后单击**按钮**若要确保你看到以下输出：   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. 返回到代码编辑器中，，然后选择事件处理程序中的的以下行。   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. 右键单击所选内容，单击**重构**，然后选择**提取方法**。 

    **提取方法**对话框随即出现。
7. 在**新方法名称**框中，键入**DisplayArray**，然后单击**确定**。 

    代码编辑器中创建一个名为的新方法`DisplayArray`，并将放到中的新方法的调用**单击**循环最初的处理程序。

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. 按**CTRL + F5**再次，运行此页并单击**按钮**。

    此页面的功能相同像以前一样。 `DisplayArray`方法现在可以从任何位置的调用中的页类。

## <a name="renaming-variables"></a>重命名变量

时要使用变量，以及对象时，你可能想要将其重命名后它们将已在你的代码中引用。 但是，重命名的变量和对象可以导致中断如果错过了重命名其中一个引用的代码。 因此，你可以使用重构来执行重命名。

### <a name="to-use-refactoring-to-rename-a-variable"></a>若要使用重构重命名变量


1. 在**单击**事件处理程序中，找到以下行：

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. 右键单击变量名称`alist`，选择**重构**，然后选择**重命名**。

    **重命名**对话框随即出现。
3. 在**新名称**框中，键入**ArrayList1**并确保**预览引用更改**选中复选框。 然后单击“确定” 。

    **预览更改**对话框出现，并显示一个树，它包含指向要重命名的变量的所有引用。
4. 单击**应用**关闭**预览更改**对话框。

    特定于所选实例引用的变量将重命名。 但是，请注意，变量`alist`以下行中不会重命名。

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    变量`alist`此行中不重命名，因为它不表示该变量的值相同`alist`你重命名。 变量`alist`中`DisplayArray`声明为该方法的局部变量。 这表明使用重构重命名变量是不同于只需在编辑器中; 执行查找和替换操作重构重命名变量的了解，它正在使用的变量的语义。


## <a name="inserting-snippets"></a>插入代码段

由于有很多 Web 窗体开发人员经常需要执行的编码任务，该代码编辑器还提供代码段或预编写的代码块的库。 你可以将这些代码段插入你的页面。

使用 Visual Studio 中的每种语言有中插入代码段的方式的细微差异。 有关插入代码段的信息，请参阅[Visual Basic IntelliSense 代码段](https://msdn.microsoft.com/library/18yz4be4.aspx)。 有关插入代码段 Visual C# 中的信息，请参阅[Visual C# 代码片段](https://msdn.microsoft.com/library/z41h7fat.aspx)。

## <a name="next-steps"></a>后续步骤

本演练为更正你的代码中的错误、 代码重构、 重命名变量，和将代码段插入到你的代码演示了 Visual Studio 2010 代码编辑器的基本功能。 在编辑器中的其他功能可以使快速而简单的应用程序开发。 例如，你可能希望：

- 了解有关 IntelliSense，如修改 IntelliSense 选项，管理代码段，以及搜索联机代码段的功能的详细信息。 有关详细信息，请参阅[使用 IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx)。
- 了解如何创建你自己的代码段。 有关详细信息，请参阅[创建和使用 IntelliSense 代码段](https://msdn.microsoft.com/library/ms165392.aspx)
- 了解有关 IntelliSense 代码段，如自定义代码段和故障排除的 Visual Basic 特定功能的详细信息。 有关详细信息，请参阅[Visual Basic IntelliSense 代码段](https://msdn.microsoft.com/library/18yz4be4.aspx)
- 了解有关 C# 的特定功能的 IntelliSense，例如重构和代码片段。 有关详细信息，请参阅[Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx)。
