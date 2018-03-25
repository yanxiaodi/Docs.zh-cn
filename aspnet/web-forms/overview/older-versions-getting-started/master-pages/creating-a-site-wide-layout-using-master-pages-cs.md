---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-cs
title: "创建使用母版页 (C#) 的站点范围布局 |Microsoft 文档"
author: rick-anderson
description: "本教程将介绍母版页基础知识。 也就是说，什么是母版页，如何执行一个创建母版页、 什么是内容的占位符，如何执行一个 cr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 78f8d194-03b9-44a5-8255-90e7cd1c2ee1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 8061a2aff318d397116cbc0bc0a8ce24ef35c7fb
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-site-wide-layout-using-master-pages-c"></a>创建站点范围布局使用母版页 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_CS.zip)或[下载 PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_CS.pdf)

> 本教程将介绍母版页基础知识。 也就是说，什么是母版页、 一个会如何创建母版页，什么是内容的占位符，如何会一个创建 ASP.NET 页使用母版页，如何修改母版页将自动反映在其关联的内容页中，依次类推。


## <a name="introduction"></a>介绍

设计良好的一个特性是网站的一致的站点范围页面布局。 以 www.asp.net 网站中，为例。 在撰写本文时，每个页都有相同的内容的顶部和页的底部。 如图 1 所示，每个页面的最顶部将显示带有 Microsoft 社区的列表的灰色栏。 下，它是网站徽标、 向其中转换站点，语言和核心部分的列表： 主页、 开始、 了解、 下载和等。 同样，页面底部包括有关广告 www.asp.net、 版权声明和的隐私声明的链接上的信息。


[![Www.asp.net 网站在所有页面采用一致的外观和感觉](creating-a-site-wide-layout-using-master-pages-cs/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image1.png)

**图 01**: www.asp.net 网站采用一致的外观和感觉跨所有的页面 ([单击以查看实际尺寸的图像](creating-a-site-wide-layout-using-master-pages-cs/_static/image3.png))


设计良好的另一个特性是站点的站点的外观更改的方便。 图 1 显示 www.asp.net 主页截至 2008 年 3 月，但现在，本教程的发布，可能已更改外观和感觉。 可能是沿顶部的菜单项将展开以包括一个新的节，用于 MVC 框架。 否则可能使用不同的颜色、 字体和布局的全新设计将活跃。 将此类更改应用于整个站点应是快速而简单的过程，不需要修改将组成该网站的 web 页的数千个。

在 ASP.NET 中创建站点范围页模板是可能使用*主页*。 简而言之，母版页是一种特殊类型的定义是在所有常见的标记的 ASP.NET 页*内容页*以及是在内容页面的内容页基础上可自定义的区域。 （内容页是绑定到母版页的 ASP.NET 页。）每当更改母版页的布局或格式设置时，则这使得应用轻松更新和部署单个文件 （即，主控页） 的站点范围外观更改，对所有其内容页的输出同样立即更新。

这是一系列浏览使用母版页的教程中的第一个教程。 在本系列教程的过程中我们：

- 检查创建母版页和其关联的内容页
- 讨论各种提示、 技巧和陷阱，
- 确定母版页的常见问题和浏览解决方法，
- 请参阅如何从内容页和相反的操作访问母版页
- 了解如何指定在运行时，内容页面的母版页和
- 其他高级母版页主题。

这些教程旨在是简洁并提供引导你完成此过程以可视方式并提供充足的屏幕截图的分步说明。 每个教程是在 C# 和 Visual Basic 版本中可用，包括使用的完整代码下载。

此篇教程开头看母版页基础知识。 我们讨论如何母版页的工作原理、 查看创建母版页和使用 Visual Web Developer 中，关联的内容页，请参阅如何对母版页的更改将立即反映在其内容页中。 让我们进入正题！

## <a name="understanding-how-master-pages-work"></a>了解如何母版页的工作原理

构建一个具有一致的站点范围页面布局的网站需要每个网页发出除了其自定义内容的常见格式设置标记。 例如，每个教程或论坛上的发布帖子 www.asp.net 有自己唯一的内容，而每个这些页面还呈现了常见的一系列`<div>`显示顶级部分链接的元素： 主页、 开始、 了解，和等等。

有一系列用于创建具有一致的外观和感觉的网页的技术。 Naïve 方法是只需复制并粘贴到所有网页的常见的布局标记，但这种方法具有大量弊端。 对于初学者，每次创建一个新页时，你必须记得复制并粘贴到页的共享的内容。 此类复制和粘贴操作是这个错误意外，您可以将共享标记的一个子集复制到新页。 并为了彻底，这种方法使现有的站点范围外观将替换为一个新实际困难，因为每个站点中的单页必须编辑才能使用新的外观和感觉。

在 ASP.NET 2.0 版中之前, 页上通常放置中的常见标记的开发人员[用户控件](https://msdn.microsoft.com/library/y6wb1a0e.aspx)并随后添加到每个页的这些用户控件。 此方法所需页开发人员记住手动将用户控件添加到每个新的页上，而允许的更容易覆盖整个站点修改，因为更新的常见标记时仅用户控件所需修改。 遗憾的是，Visual Studio.NET 2002年和 2003-Visual Studio 版本的用于创建 ASP.NET 1.x 应用程序-在设计视图中的用户控件呈现为灰色的框。 因此，使用此方法的页开发人员未不喜欢所见即所得的设计时环境。

使用用户控件的不足之处已解决在 ASP.NET 2.0 版和 Visual Studio 2005 中，通过引入*主页*。 母版页是一种特殊类型的定义这两个站点范围标记的 ASP.NET 页和*区域*其中关联*内容页*定义其自定义标记。 我们将会看到在步骤 1 中，这些区域定义由 ContentPlaceHolder 控件。 ContentPlaceHolder 控件只是表示内容页可以其中插入自定义内容的主页面控件层次结构中的位置。

> [!NOTE]
> ASP.NET 版本 2.0 以来未更改的核心概念和功能的主控页。 但是，Visual Studio 2008 提供用于嵌套母版页的设计时支持已在 Visual Studio 2005 中缺少的功能。 我们将了解在将来的教程中使用嵌套的母版页。


图 2 显示的 www.asp.net 母版页可能如下所示。 请注意主控页以及中间方，每个单独的 web 页的唯一内容所在 ContentPlaceHolder 定义常见的站点范围布局的在顶部、 底部和每一页的右侧的标记的。


![母版页内容页面的内容页基于定义的站点范围布局和可编辑区域](creating-a-site-wide-layout-using-master-pages-cs/_static/image4.png)

**图 02**： 母版页内容页面的内容页基于定义的站点范围布局和可编辑区域


在定义母版页后它可以绑定到新的 ASP.NET 页，通过一个复选框的刻度线。 -调用内容页-这些 ASP.NET 页面包括用于每个母版页的 ContentPlaceHolder 控件内容控件。 时通过浏览器访问内容页 ASP.NET 引擎创建主页面控件层次结构，并将内容页控件层次结构注入到相应的位置。 呈现此组合的控件层次结构，生成的 HTML 返回到最终用户的浏览器。 因此，内容页会发出 ContentPlaceHolder 控件的外部其母版页中定义的常见标记和在其自己的内容控件中定义的特定于页的标记。 图 3 说明了这一概念。


[![请求的页面的标记 Fused 到母版页](creating-a-site-wide-layout-using-master-pages-cs/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image5.png)

**图 03**: 请求页面标记 Fused 到母版页 ([单击以查看实际尺寸的图像](creating-a-site-wide-layout-using-master-pages-cs/_static/image7.png))


现在，我们已经讨论了如何母版页的工作原理，让我们看看创建母版页和使用 Visual Web Developer 的关联内容页。

> [!NOTE]
> 若要访问尽可能多的用户，我们在本教程系列整个生成 ASP.NET 网站将创建与 Microsoft 的免费版本的 Visual Studio 2008 中，使用 ASP.NET 3.5 [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/)。 如果你尚未升级到 ASP.NET 3.5 中，不要担心-在这些教程工作中所述的概念同样有效的 ASP.NET 2.0 和 Visual Studio 2005。 但是，某些演示应用程序可能使用的.NET framework 版本 3.5; 新增功能当使用 3.5 特定功能时，包括请注意，讨论如何在 2.0 版中实现类似的功能。 执行请记住，可用于演示应用程序下载每个教程的目标从.NET Framework 3.5 版中，这会导致`Web.config`包括 3.5 特定配置元素和到 3.5 特定命名空间中的引用的文件`using` ASP.NET 页的代码隐藏类中的语句。 将无法正常长情景短，如果你尚未在您的计算机然后可下载的 web 应用程序上安装.NET 3.5 第一个删除从 3.5 特定标记`Web.config`。 请参阅[将 ASP.NET 3.5 版的`Web.config`文件](http://www.4guysfromrolla.com/articles/121207-1.aspx)有关本主题的详细信息。 你还需要删除`using`引用 3.5 特定命名空间的语句。


## <a name="step-1-creating-a-master-page"></a>步骤 1： 创建母版页

我们可以浏览创建和使用母版页和内容页之前，我们首先需要 ASP.NET 网站。 首先，创建新的文件系统基于 ASP.NET 网站。 若要完成此操作，启动 Visual Web Developer 中，然后转到文件菜单并选择新的网站，显示新建网站对话框框中 （请参见图 4）。 选择 ASP.NET 网站模板，将位置下拉列表设置为文件系统、 选择一个文件夹，以将 web 站点，并将语言设置为 C#。 这将创建新的网站与`Default.aspx`ASP.NET 页中，`App_Data`文件夹中，和一个`Web.config`文件。

> [!NOTE]
> Visual Studio 支持两种项目管理模式： 网站项目和 Web 应用程序项目。 网站项目缺少项目文件中，而 Web 应用程序项目模拟项目体系结构在 Visual Studio.NET 2002年/2003年-它们包括项目文件并将项目的源代码编译到单个程序集，并置于`/bin`文件夹。 Visual Studio 2005 最初仅支持的网站项目，尽管[Web 应用程序项目模型](https://msdn.microsoft.com/library/aa730880(vs.80).aspx)已重新引入 Service Pack 1;Visual Studio 2008 提供这两种项目模型。 Visual Web Developer 2005 和 2008年版，但是，仅支持网站项目。 我在本教程系列我演示使用网站项目模型。 如果你在使用非 Express edition，并想要改为使用 Web 应用程序项目模型，请尝试这样做，但请注意，可能会出现某些差异之间在你的屏幕和必须与所示的屏幕截图与 instructio 采取的步骤上看到的内容这些教程中提供的 ns。


[![创建新的文件系统基于 Web 站点](creating-a-site-wide-layout-using-master-pages-cs/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image8.png)

**图 04**： 创建 New File System-Based 网站 ([单击以查看实际尺寸的图像](creating-a-site-wide-layout-using-master-pages-cs/_static/image10.png))


接下来，添加母版页到站点的根目录中，方法是通过右键单击项目名称，选择添加新项，选择母版页模板。 请注意以扩展名结尾母版页`.master`。 命名此新的母版页`Site.master`并单击添加。


[![将主页面添加名为 Site.master 到网站](creating-a-site-wide-layout-using-master-pages-cs/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image11.png)

**图 05**： 添加 Master 页命名`Site.master`到网站 ([单击以查看实际尺寸的图像](creating-a-site-wide-layout-using-master-pages-cs/_static/image13.png))


添加 Visual Web Developer 通过新的主控页文件创建一个主页面，替换为以下的声明性标记：

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample1.aspx)]

声明性的标记中的第一行是[`@Master`指令](https://msdn.microsoft.com/library/ms228176.aspx)。 `@Master`指令是类似于[`@Page`指令](https://msdn.microsoft.com/library/ydy4x04a.aspx)在 ASP.NET 页中显示。 它定义的服务器端语言 (C#) 和信息的位置和母版页的代码隐藏类的继承。

`DOCTYPE`和页面的声明性标记显示在下方`@Master`指令。 该页面包括四个服务器端控件以及静态 HTML:

- **Web 窗体 ( `<form runat="server">`)** -因为通常有一个 Web 窗体中的所有 ASP.NET 页面而且，因为主控页可能包括 Web 控件，它们必须出现在 Web 窗体-会确保将 Web 窗体添加到你的母版页 （而不是将 Web 窗体添加到 e位内容页）。
- **一个名为的 ContentPlaceHolder 控件`ContentPlaceHolder1`**  -此 ContentPlaceHolder 控件将显示在 Web 窗体，并作为内容页的用户界面的区域。
- **服务器端`<head>`元素**-`<head>`元素具有`runat="server"`属性，使其可以通过服务器端代码访问。 `<head>`元素实现这种方式，以便页的标题和其他`<head>`-相关标记可能添加或以编程方式调整。 例如，设置 ASP.NET 页的`Title`属性更改`<title>`元素呈现通过`<head>`服务器控件。
- **一个名为的 ContentPlaceHolder 控件`head`**  -此 ContentPlaceHolder 控件出现在`<head>`服务器控制和可用于以声明方式将内容添加到`<head>`元素。

此默认母版页声明性标记用作设计你自己的母版页的起点。 随意编辑 HTML 或将其他 Web 控件或 ContentPlaceHolders 添加到母版页。

> [!NOTE]
> 设计母版页请确保母版页包含 Web 窗体，且此 Web 窗体中显示该至少一个 ContentPlaceHolder 控件。


### <a name="creating-a-simple-site-layout"></a>创建简单的站点布局

让我们展开`Site.master`的默认声明性标记以创建所有页都共享的其中一个站点布局： 常见标头; 与导航、 新闻和其他站点范围的内容; 以及页脚显示的"支持的 Microsoft ASP.NET"图标左侧的列。 图 6 显示主控页的最终结果，通过浏览器查看其内容页之一时。 在图 6 中的红色圆圈的区域是特定于正在访问过的网页 (`Default.aspx`); 其他内容是在所有内容页面的在母版页中定义并因此一致。


[![母版页上边框、 左侧和底部部分定义标记](creating-a-site-wide-layout-using-master-pages-cs/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image14.png)

**图 06**: 母版页上边框、 左侧和底部部分定义标记 ([单击以查看实际尺寸的图像](creating-a-site-wide-layout-using-master-pages-cs/_static/image16.png))


若要实现图 6 所示的站点布局，首先要更新`Site.master`母版页以使其包含以下声明性标记：

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample2.aspx)]

使用一系列定义母版页的布局`<div>`HTML 元素。 `topContent` `<div>`包含每个页上，顶部会显示的标记时`mainContent`， `leftContent`，和`footerContent` `<div>` s 用于显示页面的内容、 左侧的列中和"支持由 MicrosoftASP.NET"图标，分别。 除了添加这些`<div>`元素，我也已重命名`ID`从主 ContentPlaceHolder 控件属性`ContentPlaceHolder1`到`MainContent`。

这些各种类型的格式设置和布局规则`<div>`元素拼写是否在[级联样式表 (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets)文件`Styles.css`，这通过指定&lt;链接&gt;中的元素母版页的&lt;头&gt;元素。 这些内容的各种规则定义的每个外观和感觉`<div>`元素如上所示。 例如， `topContent` `<div>`元素，后者将显示"Master 页教程"文本和链接，具有中指定其格式设置规则`Styles.css`，如下所示：

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample3.css)]

如果你是内容的观众在你的计算机，你将需要下载本教程随附的代码并将添加`Styles.css`到你的项目文件。 同样，你还需要创建一个名为映像文件夹并将"支持的 Microsoft ASP.NET"图标从下载的演示网站复制到你的项目。

> [!NOTE]
> CSS 和 web 页格式的讨论不在本文的范围。 有关 CSS 的详细信息，请查看[CSS 教程](http://www.w3schools.com/css/default.asp)在[W3Schools.com](http://www.w3schools.com/)。我还鼓励你要下载本教程随附的代码并玩中的 CSS 设置`Styles.css`若要查看不同的格式设置规则的效果。


### <a name="creating-a-master-page-using-an-existing-design-template"></a>创建使用现有设计模板母版页

多年来，我已生成大量的 ASP.NET web 应用程序对于小型到中型公司。 我的客户端的一些具有想要使用; 的现有站点布局其他人的租期功能完善图形设计器。 一些委托我设计网站布局。 图 6 中可以看出，任务程序员能够设计网站的布局通常为明智的做法是为具有执行 open-heart surgery，而你 doctor 却你的税额你会计。

幸运的是，存在 innumerous 提供免费 HTML 设计模板的网站-Google 返回六个 100 万个结果的搜索术语"免费网站模板"。 我最喜欢的之一是[OpenDesigns.org](http://opendesigns.org/)。一旦找到您喜欢的网站模板，将 CSS 文件和图像添加到你的网站项目，并将模板的 HTML 集成到你母版页。

> [!NOTE]
> Microsoft 还提供了多个[免费 ASP.NET 设计启动工具包模板](https://msdn.microsoft.com/asp.net/aa336613.aspx)，将集成到 Visual Studio 中的新建网站对话框。


## <a name="step-2-creating-associated-content-pages"></a>步骤 2： 创建关联内容页

创建的主页面，我们现在可以开始创建绑定到母版页的 ASP.NET 页。 此类页被称为*内容页*。

让我们向项目添加新的 ASP.NET 页，并将其绑定到`Site.master`母版页。 右键单击解决方案资源管理器中的项目名称，然后选择添加新项选项。 选择 Web 窗体模板，输入名称`About.aspx`，然后选中"选择母版页"复选框，如图 7 所示。 这样将显示选择母版页对话框框中，你可以选择主页后，可以使用从 （请参阅图 8）。

> [!NOTE]
> 如果你创建 ASP.NET 网站而不网站项目模型使用的 Web 应用程序项目模型则将看不图 7 所示的添加新项对话框中的"选择母版页"复选框。 若要创建内容页时使用 Web 应用程序项目模型你必须选择而不是 Web 窗体模板的 Web 内容窗体模板。 在选择内容的 Web 窗体模板，然后单击添加后, 相同选择 Master 页将出现图 8 所示的对话框。


[![添加新的内容页](creating-a-site-wide-layout-using-master-pages-cs/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image17.png)

**图 07**： 添加新的内容页 ([单击以查看实际尺寸的图像](creating-a-site-wide-layout-using-master-pages-cs/_static/image19.png))


[![选择 Site.master 母版页](creating-a-site-wide-layout-using-master-pages-cs/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image20.png)

**图 08**： 选择`Site.master`母版页 ([单击以查看实际尺寸的图像](creating-a-site-wide-layout-using-master-pages-cs/_static/image22.png))


如下面的声明性标记所示，新的内容页包含`@Page`，指回其主页和内容控件的每个母版页的 ContentPlaceHolder 控件的指令。

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample4.aspx)]

> [!NOTE]
> 在步骤 1 中的"创建简单站点布局"一节中我重命名`ContentPlaceHolder1`到`MainContent`。 如果你没有重命名此 ContentPlaceHolder 控件`ID`相同的方式，你内容页面的声明性标记略有不同，根据上面所示的标记。 也就是说，第二个内容控件的`ContentPlaceHolderID`将反映`ID`相应 ContentPlaceHolder 控制你母版页中。


当呈现内容页、 ASP.NET 引擎必须保险丝页面的内容与其母版页 ContentPlaceHolder 控件的控件。 ASP.NET 引擎确定内容页的母版页从`@Page`指令的`MasterPageFile`属性。 如上面的标记所示，此内容页绑定到`~/Site.master`。

因为主控页具有两个 ContentPlaceHolder 控件-`head`和`MainContent`-Visual Web Developer 生成两个内容控件。 每个内容控件引用通过特定 ContentPlaceHolder 其`ContentPlaceHolderID`属性。

其中母版页增加亮点过上一个站点范围模板方法是使用它们的设计时支持。 图 9 显示`About.aspx`内容页查看通过 Visual Web Developer 设计视图时。 请注意，尽管母版页内容是可见的它将灰显且不能修改。 内容控件与母版页的 ContentPlaceHolders 相对应的但是，可编辑。 并且，就像使用任何其他 ASP.NET 页中，你可以创建内容页的接口通过添加通过源或设计视图的 Web 控件一样。


[![内容页的设计视图显示这两个特定于页的和主页面内容](creating-a-site-wide-layout-using-master-pages-cs/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image23.png)

**图 09**: 内容页面的设计视图显示两个特定于页的和 Master 页面内容 ([单击以查看实际尺寸的图像](creating-a-site-wide-layout-using-master-pages-cs/_static/image25.png))


### <a name="adding-markup-and-web-controls-to-the-content-page"></a>将标记和 Web 控件添加到内容页

花一些时间创建的某些内容`About.aspx`页。 如你所见图 10 中，我输入"有关作者"标题和几个段落的文本，但随意太添加 Web 控件。 在创建之后此接口，请访问`About.aspx`通过浏览器的页。


[![请访问 About.aspx 页通过浏览器](creating-a-site-wide-layout-using-master-pages-cs/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image26.png)

**图 10**： 访问`About.aspx`通过浏览器页面 ([单击以查看实际尺寸的图像](creating-a-site-wide-layout-using-master-pages-cs/_static/image28.png))


请务必了解请求的内容页和其关联的母版页是 fused，作为一个整体的 web 服务器上完全呈现。 最终用户的浏览器然后会发送的生成、 融合 HTML。 若要验证这一点，请查看浏览器通过转到视图菜单并选择数据源收到的 HTML。 请注意，有不帧或任何其他专用的技术，用于在单个窗口中显示两个不同的 web 页。

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>绑定到现有的 ASP.NET 页的母版页

正如我们看到在此步骤中，将新的内容页添加到 ASP.NET web 应用程序非常简单，只需选中"选择母版页"复选框和选取母版页。 遗憾的是，将现有的 ASP.NET 页转换为母版页不一样简单。

若要绑定到现有的 ASP.NET 页的母版页需要执行以下步骤：

1. 添加`MasterPageFile`属性设为 ASP.NET 页`@Page`指令，其指向适当的母版页。
2. 为每个 ContentPlaceHolders 母版页中添加内容控件。
3. 有选择地剪切并粘贴到相应的内容控件的 ASP.NET 页的现有内容。 我说"有选择地"此处因为 ASP.NET 页上可能会包含标记为已由表示主页上，如`DOCTYPE`、`<html>`元素和 Web 窗体。

屏幕快照以及此过程的分步说明，请查看[Scott Guthrie](https://weblogs.asp.net/scottgu/)的[使用母版页和网站的导航](http://webproject.scottgu.com/CSharp/MasterPages/MasterPages.aspx)教程。 "更新`Default.aspx`和`DataSample.aspx`使用母版页"部分将详细介绍这些步骤。

因为它是可以更轻松地创建新的内容页不是它是将现有的 ASP.NET 页转换为内容页，建议，每当你创建新的 ASP.NET 网站添加母版页到站点。 将所有新的 ASP.NET 页面绑定到此母版页。 如果初始母版页是非常简单或普通; 请不要担心你可以稍后更新母版页。

> [!NOTE]
> 在创建新的 ASP.NET 应用程序时，Visual Web Developer 将添加`Default.aspx`没有绑定到母板页的页。 如果你要将现有的 ASP.NET 页转换为内容页的做法，请继续并完成此操作`Default.aspx`。 或者，可以删除`Default.aspx`并随后重新添加它，但这一次检查"选择母版页"复选框。


## <a name="step-3-updating-the-master-pages-markup"></a>步骤 3： 更新母版页的标记

为母版页的主要优点之一是使用单个母版页可能用于定义在站点上的多页的总体布局。 因此，在更新站点的外观和感觉需要更新单个文件的主控页。

为了说明此行为，让我们更新我们的主页面，以包含在顶部左侧列中的当前日期。 添加一个名为标签`DateDisplay`到`leftContent` `<div>`。

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample5.aspx)]

接下来，创建`Page_Load`事件处理程序主页上，添加以下代码：

[!code-csharp[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample6.cs)]

上面的代码中设置的标签`Text`属性的当前日期和时间格式化为日期是星期几、 月和两位数日期的名称 （请参阅图 11）。 进行此更改后，重新访问您的内容页。 图 11 显示，生成的标记立即更新以包括对主页上的更改。


[![对 Master 页上的更改会反映时查看内容页](creating-a-site-wide-layout-using-master-pages-cs/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image29.png)

**图 11**: 母版页的更改都反映时查看内容页 ([单击以查看实际尺寸的图像](creating-a-site-wide-layout-using-master-pages-cs/_static/image31.png))


> [!NOTE]
> 如本示例所示，母版页可能包含服务器端 Web 控件、 代码和事件处理程序。


## <a name="summary"></a>摘要

主控页启用 ASP.NET 开发人员设计一致的站点范围布局是可轻松地更新。 Visual Web Developer 提供丰富的设计时支持，则创建母版页和其关联的内容页非常简单，创建标准 ASP.NET 页。

我们在本教程中创建的母版页示例有两个 ContentPlaceHolder 控件，`head`和`MainContent`。 我们仅指定标记`MainContent`ContentPlaceHolder 在内容页中，但是控制。 在下一教程我们查看使用多个内容控件在内容页中的。 我们还将了解如何定义默认标记为内容控件中主页上，以及如何使用默认值之间进行切换标记中定义主页面并提供从内容页自定义标记。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [设计器的 ASP.NET： 上生成 ASP.NET 网站使用 Web 标准释放设计模板和指南](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [ASP.NET Master 页概述](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [级联样式表 (CSS) 教程](http://www.w3schools.com/css/default.asp)
- [动态设置页的标题](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [在 ASP.NET 母版页](http://www.odetocode.com/articles/419.aspx)
- [母版页的快速入门教程](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多个 ASP/ASP.NET 丛书和 4GuysFromRolla.com 创始人，具有已使用 Microsoft Web 技术自 1998 年。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 3.5 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 可以在达到 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

>[!div class="step-by-step"]
[下一篇](multiple-contentplaceholders-and-default-content-cs.md)
