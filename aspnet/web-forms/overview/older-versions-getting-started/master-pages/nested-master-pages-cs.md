---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
title: "嵌套母版页 (C#) |Microsoft 文档"
author: rick-anderson
description: "演示如何嵌套在另一个母版页。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 32b7fb6e-d74b-4048-91f8-70631b2523ee
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 97513a5a6ac7a958a03626f16a328ecb0b85c03f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="nested-master-pages-c"></a>嵌套的母版页 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.zip)或[下载 PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.pdf)

> 演示如何嵌套在另一个母版页。


## <a name="introduction"></a>介绍

在过去的九个教程的过程中，我们已了解如何实现使用母版页的站点范围布局。 简而言之，母版页允许我们，页开发人员，可以定义以及可以基于内容的页面的内容页自定义的特定区域母版页中的常见标记。 ContentPlaceHolder 控件在母版页中的指示的可自定义区域;ContentPlaceHolder 控件的自定义的标记在内容控件通过内容页中定义。

如果你有一个跨整个站点使用的布局，则我们到目前为止已探讨了的母版页技术非常用。 但是，许多大型网站跨多个部分具有自定义站点布局。 例如，考虑医院员工用于管理患者信息、 活动和计费卫生保健应用程序。 可能有三种类型的此应用程序中的网页：

- 人员成员特定页面员工可以在其中更新可用性，查看计划，或请求休假时间。
- 员工在其中查看或编辑特定位患者的信息特定于患者的页面。
- 名会计师查看当前的计费特定页面声明状态和财务报告。

每一页可能共享通用的布局，如跨顶部和底部的频繁使用链接的一系列菜单。 但人员、 患者和计费特定页可能需要自定义此通用的布局。 例如，可能是所有的特定人员的页面应包括显示当前登录的用户的可用性和每日计划的日历和任务列表。 可能需要显示名称、 地址和保险信息正在编辑其信息位患者的所有患者特定页。

可以通过创建此类自定义的布局*嵌套母版页*。 若要实现上述方案，我们将通过创建使用 ContentPlaceHolders 定义可自定义区域定义的站点范围的布局、 菜单和页脚内容的母版页开始。 然后，我们将创建三个嵌套的母版页，一个用于每种类型的网页。 每个嵌套的主页面将定义之间的一种使用母版页的内容页的内容。 换而言之，患者特定内容页嵌套的母版页会包括标记和用于显示与正在编辑的患者相关的信息的编程逻辑。 创建一个新的患者特定页时我们会将其绑定到此嵌套母版页。

本教程开始通过突出显示嵌套的母版页的好处。 然后，它演示如何创建和使用嵌套的母版页。

> [!NOTE]
> .NET Framework 2.0 版后，嵌套的母版页已可能。 但是，Visual Studio 2005 中不包括嵌套的母版页的设计时支持。 好消息是 Visual Studio 2008 提供嵌套母版页提供丰富的设计时体验。 如果你感兴趣使用嵌套的主控页，但仍在使用 Visual Studio 2005，需签出[Scott Guthrie](https://weblogs.asp.net/scottgu/)的博客文章[嵌套母版中的页面 VS 2005 设计时提示](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)。


## <a name="the-benefits-of-nested-master-pages"></a>嵌套的母版页的优点

很多网站中有总体的站点设计，以及自定义的设计更特定于某些类型的页。 例如，我们演示 web 应用程序中我们已创建基本的管理部分 (中的页`~/Admin`文件夹)。 当前中的 web 页`~/Admin`文件夹以这些页面不在管理部分中使用相同的母版页 (也就是说，`Site.master`或`Alternate.master`，取决于用户的选择)。

> [!NOTE]
> 现在，假设我们的站点具有一个母版页`Site.master`。 我们将解决本教程中稍后使用嵌套的母版页利用两个 （或多个）"使用嵌套 Master 页的管理部分"开头的主页面。


假设我们被要求以自定义布局的管理页，以包含其他信息或将否则不存在于站点中的其他页面的链接。 有以下四种技术来实现这一要求：

1. 手动将管理特定的信息和链接添加到每个内容页`~/Admin`文件夹。
2. 更新`Site.master`是否管理页面之一正在访问基于母版页要包含的管理部分特定信息和链接，然后将代码添加到主页后，可以显示或隐藏这些部分。
3. 创建新的母版页专门为管理部分中，通过从标记的复制`Site.master`、 添加的管理部分特定信息和链接，，然后更新中的内容页`~/Admin`要使用此新的主文件夹页。
4. 创建将绑定到一个嵌套的母版页`Site.master`中, 具有内容页`~/Admin`文件夹使用此新嵌套母版页。 此嵌套的主控页将包括只需的其他信息和特定于管理页的链接和不需要重复已在中定义的标记`Site.master`。

第一个选项是至少 palatable。 使用母版页的整个点是要将移开无需手动复制并粘贴到新的 ASP.NET 页的常见标记。 第二个选项都可以接受，但会导致应用程序不太容易维护，因为它 bulks 向上标记，只是偶尔显示并需要开发人员可以编辑母版页若要解决此标记并必须记住时，包含的主页面完全匹配，某些标记将显示与当其处于隐藏状态。 此方法是将较低 tenable 作为自定义项从越来越多类型的此单个母版页适合所需的 web 页。

第三个选项中移除混乱和复杂性问题出现的第二个选项。 但是，选项三个的主要缺点是，它需要我们可以复制并粘贴从通用布局`Site.master`到新的管理特定部分的主页面。 如果我们稍后决定更改站点范围布局我们需要记住若要更改在两个位置。

第四个选项中，嵌套母版页，向我们提供的第二个和第三个选项的最佳。 特定于特定区域的内容被分隔到不同的文件时，会将站点范围布局信息保留在一个文件的顶级的主控页的中。

本教程开头创建和使用简单的嵌套的母版页一看。 我们创建全新的顶级母版页、 两个嵌套的主页面和两个内容页。 从"使用嵌套 Master 页的管理部分"开始，我们看看更新我们现有的母版页体系结构，以包括嵌套的母版页的使用。 具体而言，我们创建嵌套的母版页并使用它来包括其他自定义内容中的内容页`~/Admin`文件夹。

## <a name="step-1-creating-a-simple-top-level-master-page"></a>步骤 1： 创建简单的顶级母版页

创建嵌套的 master 基于现有的主机之一上页，然后更新现有的内容页，以便使用此新的嵌套母版页上而不是顶级母版页需要某种程度的复杂性，因为现有的内容页已需要某些ContentPlaceHolder 顶级母版页中定义控件。 因此，嵌套的主控页还必须包括具有相同名称的相同 ContentPlaceHolder 控制。 此外，我们特定演示应用程序可以有两个母版页 (`Site.master`和`Alternate.master`)，动态分配给内容页基于用户的首选项，它会进一步将添加到此复杂性。 我们将着眼于更新现有的应用程序，以在本教程后面使用嵌套的主控页，但让我们第一个专注于一个简单嵌套母版页示例。

创建一个名为的新文件夹`NestedMasterPages`然后将新的主控页文件添加到名为该文件夹`Simple.master`。 （请参阅图 1 解决方案资源管理器的屏幕截图后已添加此文件夹和文件。）拖动`AlternateStyles.css`从解决方案资源管理器中拖动到设计器的样式表文件。 这将添加`<link>`元素中的样式表文件`<head>`元素，此后母版页的`<head>`元素的标记应如下所示：


[!code-aspx[Main](nested-master-pages-cs/samples/sample1.aspx)]

接下来，添加以下标记内的 Web 窗体`Simple.master`:


[!code-aspx[Main](nested-master-pages-cs/samples/sample2.aspx)]

此标记中深蓝色背景上大白色字体的页面顶部显示标题为"嵌套 Master 页 （简单）"的链接。 下面，都有`MainContent`ContentPlaceHolder。 图 1 显示`Simple.master`母版页时在 Visual Studio 设计器中加载。


[![嵌套的母版页定义特定内容所致的管理部分中的页](nested-master-pages-cs/_static/image2.png)](nested-master-pages-cs/_static/image1.png)

**图 01**: 嵌套 Master 页定义内容特定于管理部分中的页 ([单击以查看实际尺寸的图像](nested-master-pages-cs/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>步骤 2： 创建简单的嵌套的母版页

`Simple.master`包含两个 ContentPlaceHolder 控件：`MainContent`连同 Web 窗体中添加的我们的 ContentPlaceHolder`head`中的 ContentPlaceHolder`<head>`元素。 如果要创建内容页，并将其绑定到`Simple.master`内容页将具有两个引用两个 ContentPlaceHolders 的内容控件。 同样，如果我们将创建嵌套的母版页并将其绑定到`Simple.master`然后嵌套的主控页将具有两个内容控件。

让我们添加到新的嵌套母版页上`NestedMasterPages`文件夹名为`SimpleNested.master`。 右键单击`NestedMasterPages`文件夹，然后选择添加新项。 这会打开添加新项对话框，在图 2 所示。 选择母版页模板类型和输入新的主控页的名称。 若要指示新的主控页应嵌套母板页，请检查"选择母版页"复选框。

接下来，单击添加按钮。 这将显示相同的 Select 绑定到母版页的内容页时，将显示一个母版页对话框 （请参见图 3）。 选择`Simple.master`母版页中的`NestedMasterPages`文件夹，然后单击确定。

> [!NOTE]
> 如果你创建 ASP.NET 网站而不网站项目模型使用的 Web 应用程序项目模型则将看不图 2 所示的添加新项对话框中的"选择母版页"复选框。 若要创建嵌套的母版页，在使用 Web 应用程序项目模型时必须选择嵌套母版页模板 （而不是母版页模板中）。 选择嵌套母板页模板并单击添加后, 相同选择 Master 页将出现图 3 所示的对话框。


[![检查&quot;选择母版页&quot;复选框以添加嵌套母版页](nested-master-pages-cs/_static/image5.png)](nested-master-pages-cs/_static/image4.png)

**图 02**： 选中"选择母版页"复选框可添加嵌套的母版页 ([单击以查看实际尺寸的图像](nested-master-pages-cs/_static/image6.png))


[![将嵌套的母版页绑定到 Simple.master 母版页](nested-master-pages-cs/_static/image8.png)](nested-master-pages-cs/_static/image7.png)

**图 03**： 绑定到嵌套母版页`Simple.master`母版页 ([单击以查看实际尺寸的图像](nested-master-pages-cs/_static/image9.png))


嵌套的主控页的声明性标记，如下所示，包含两个引用顶级母版页的两个 ContentPlaceHolder 控件的内容控件。


[!code-aspx[Main](nested-master-pages-cs/samples/sample3.aspx)]

除`<%@ Master %>`指令，嵌套的主控页的初始声明性标记等同于最初生成绑定到相同的顶级母版页的内容页时的标记。 像内容页面的`<%@ Page %>`指令，`<%@ Master %>`指令此处包括`MasterPageFile`指定嵌套的母版页的父母版页的属性。 嵌套的主控页和绑定到相同的顶级母版页的内容页之间的主要区别是嵌套的主控页可以包括 ContentPlaceHolder 控件。 嵌套的主控页的 ContentPlaceHolder 控件定义其中内容页可以自定义标记的区域。

更新此嵌套的母版页，使其显示文本"Hello，从 SimpleNested ！" 内容控件中对应于`MainContent`ContentPlaceHolder 控件。


[!code-aspx[Main](nested-master-pages-cs/samples/sample4.aspx)]

建立此添加后, 保存嵌套的主控页，然后添加到新的内容页`NestedMasterPages`文件夹名为`Default.aspx`，并将其绑定到`SimpleNested.master`母版页。 在添加此页时，你可能感到很惊奇，它不包含任何内容的控件 （请参见图 4） ！ 内容页只能访问其*父*主页面的 ContentPlaceHolders。 `SimpleNested.master`不包含任何 ContentPlaceHolder 控件;因此，所有绑定到此母版页的内容页不能包含任何内容控件。


[![新内容页不包含任何内容控件](nested-master-pages-cs/_static/image11.png)](nested-master-pages-cs/_static/image10.png)

**图 04**: 新内容页包含无内容控件 ([单击以查看实际尺寸的图像](nested-master-pages-cs/_static/image12.png))


我们需要做什么是更新嵌套的母版页 (`SimpleNested.master`) 若要包括 ContentPlaceHolder 控件。 通常需要你嵌套的主控页，以便将包含在由其父母版页，从而允许其子母版页或内容页后，可以使用任何顶级母版页的 ContentPlaceHolder 每个 ContentPlaceHolder ContentPlaceHolder控件。

更新`SimpleNested.master`母版页 ContentPlaceHolder 纳入其两个内容控件。 为 ContentPlaceHolder 控件作为其内容的控件是指 ContentPlaceHolder 控件相同的名称。 也就是说，添加一个名为的 ContentPlaceHolder 控件`MainContent`到内容控件中`SimpleNested.master`引用`MainContent`中的 ContentPlaceHolder `Simple.master`。 执行同一操作中引用的内容控件`head`ContentPlaceHolder。

> [!NOTE]
> 虽然我建议命名中嵌套的主控页的 ContentPlaceHolder 控件顶层的母版页中 ContentPlaceHolders 相同，则不需要此命名对称性。 你可以为 ContentPlaceHolder 控件在嵌套母版页你喜欢的任何名称。 但是，我发现更容易记住 ContentPlaceHolders 与哪些区域页上，如果我的顶级母版页和嵌套的母版页使用相同的名称。


建立这些添加后你`SimpleNested.master`母版页的声明性标记的外观应与以下类似：


[!code-aspx[Main](nested-master-pages-cs/samples/sample5.aspx)]

删除`Default.aspx`内容我们刚刚创建的页，然后再重新添加，将其绑定到`SimpleNested.master`母版页。 此时 Visual Studio 将添加两个内容控件添加到`Default.aspx`，现在引用 ContentPlaceHolders 中定义`SimpleNested.master`（请参阅图 6）。 添加文本"Hello，从 Default.aspx ！" 内容中控制引用`MainContent`。

图 5 所示此处-涉及三个实体`Simple.master`， `SimpleNested.master`，和`Default.aspx`-相互之间的关系。 如图所示，嵌套的主控页为其父级的 ContentPlaceHolder 实现内容控件。 如果这些区域需要可以访问内容页，嵌套的主控页必须将其自身 ContentPlaceHolders 添加到内容控件。


[![顶级和嵌套的母版页规定内容页的布局](nested-master-pages-cs/_static/image14.png)](nested-master-pages-cs/_static/image13.png)

**图 05**： 顶级和嵌套母版页规定内容页面布局 ([单击以查看实际尺寸的图像](nested-master-pages-cs/_static/image15.png))


此行为演示内容页或母版页才认识到其父母版页的方式。 Visual Studio 设计器也可指示此行为。 图 6 显示的设计器`Default.aspx`。 虽然设计器会清楚地显示哪些区域都是从内容页可编辑和部分不，它不消除歧义非可编辑区域是从嵌套母版页和区域是从顶级主页面。


[![内容页现在包括嵌套的母版页的 ContentPlaceHolders 内容控件](nested-master-pages-cs/_static/image17.png)](nested-master-pages-cs/_static/image16.png)

**图 06**: 内容页现在包含内容控件的嵌套母版页的 ContentPlaceHolders ([单击以查看实际尺寸的图像](nested-master-pages-cs/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>步骤 3： 添加第二个简单嵌套的母版页

有多个嵌套的主控页时，嵌套的母版页的好处会更明显。 为了说明这一优势，创建中的另一个嵌套的母版页`NestedMasterPages`文件夹; 名称此新嵌套母版页`SimpleNestedAlternate.master`和将其绑定到`Simple.master`母版页。 像我们在步骤 2 中，请在嵌套的主控页的两个内容控件中添加 ContentPlaceHolder 控件。 此外添加文本"Hello，从 SimpleNestedAlternate ！" 对应于顶级母版页的内容控件中`MainContent`ContentPlaceHolder。 进行这些更改后新嵌套的母版页的声明性标记应类似于以下：


[!code-aspx[Main](nested-master-pages-cs/samples/sample6.aspx)]

创建一个名为的内容页`Alternate.aspx`中`NestedMasterPages`文件夹并将其绑定到`SimpleNestedAlternate.master`嵌套的母版页。 添加文本"Hello，从备用 ！" 内容控件中对应于`MainContent`。 图 7 显示了`Alternate.aspx`查看通过 Visual Studio 设计器时。


[![Alternate.aspx 绑定到 SimpleNestedAlternate.master 母版页](nested-master-pages-cs/_static/image20.png)](nested-master-pages-cs/_static/image19.png)

**图 07**:`Alternate.aspx`绑定到`SimpleNestedAlternate.master`母版页 ([单击以查看实际尺寸的图像](nested-master-pages-cs/_static/image21.png))


比较图 7 到图 6 中的设计器中的设计器。 这两个内容页共享顶级母版页中定义的布局相同 (`Simple.master`)，即"嵌套 Master 页教程 （简单）"标题。 尚未都具有不同的内容在其父母版页-中定义的文本"Hello，从 SimpleNested ！" 图 6 中的"Hello，从 SimpleNestedAlternate ！" 在图 7。 授予，这些差异此处进行了一些无关紧要，但你无法扩展此示例，以便包含更有意义的差异。 例如，`SimpleNested.master`页可能包括具有特定于其内容页选项的菜单，而`SimpleNestedAlternate.master`可能必须将绑定到它的内容页与相关的信息。

现在，假设我们需要对总体站点布局进行更改。 例如，假设我们想要添加到内容的所有页面的常见链接列表。 若要完成此我们更新顶级母版页`Simple.master`。 存在的任何更改将立即反映在其嵌套的主控页上，以及通过扩展，其内容页。

若要演示我们可以将总体站点布局更改与其易用性，请打开`Simple.master`母版页并添加以下标记之间`topContent`和`mainContent``<div>`元素：


[!code-aspx[Main](nested-master-pages-cs/samples/sample7.aspx)]

这会将两个链接添加到将绑定到每一页的顶部`Simple.master`， `SimpleNested.master`，或`SimpleNestedAlternate.master`; 这些更改将立即应用于所有嵌套的母版页和其内容页。 图 8 显示`Alternate.aspx`通过浏览器查看时。 请注意添加 （与比较图 7） 页顶部的链接。


[![更改为顶级母版页会立即反映在其嵌套母版页和他们的内容页](nested-master-pages-cs/_static/image23.png)](nested-master-pages-cs/_static/image22.png)

**图 08**： 更改为顶级母版页会立即反映在其嵌套母版页和他们的内容页 ([单击以查看实际尺寸的图像](nested-master-pages-cs/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>为管理部分中使用嵌套的母版页

此时我们讨论过的嵌套 master 优点页，并已了解如何创建和 ASP.NET 应用程序中使用它们。 但是，在步骤 1、 2 和 3 中的示例涉及到创建新的顶级母版页、 新嵌套的主页面和新的内容页。 呢添加新嵌套的母版页到网站与现有顶级母版页和内容页？

将嵌套的母版页集成到现有网站并将其关联与现有的内容页需要比从零开始的更多工作。 步骤 4、 5、 6 和 7 浏览这些难题，因为我们增加演示应用程序以包括新嵌套的母版页，名为`AdminNested.master`，其中包含管理员的说明，使用由 ASP.NET 页`~/Admin`文件夹。

将嵌套的母版页集成到我们演示应用程序引入了以下障碍：

- 中的现有内容页面`~/Admin`文件夹具有其母版页上的某些预期。 对于初学者，他们期望存在某些 ContentPlaceHolder 控件。 此外，`~/Admin/AddProduct.aspx`和`~/Admin/Products.aspx`页调用母版页的公共`RefreshRecentProductsGrid`方法，设置其`GridMessageText`属性，或具有事件处理程序为其`PricesDoubled`事件。 因此，我们嵌套的母版页必须提供相同 ContentPlaceHolders 和公共成员。
- 在前面的教程中，我们将增强`BasePage`类来动态设置`Page`对象的`MasterPageFile`属性基于会话变量。 我们如何为支持动态主控页时使用嵌套的主控页？

随着我们生成嵌套的主控页并使用它从我们现有内容页面上，将呈现这些两个难题。 我们调查，出现克服这些问题。

## <a name="step-4-creating-the-nested-master-page"></a>步骤 4： 创建嵌套的母版页

我们第一项任务是创建嵌套的主控页用于管理部分中的页。 正如我们看到在步骤 2 中，当添加新嵌套母版页我们需要指定嵌套的母版页的父母版页。 但我们有两个顶级母版页：`Site.master`和`Alternate.master`。 回想一下，我们创建`Alternate.master`在前面的教程编写代码和`BasePage`设置的页对象的类`MasterPageFile`属性在运行时为`Site.master`或`Alternate.master`根据的值`MyMasterPage`会话变量。

我们如何配置我们嵌套的主页面，使其使用适当的顶级母版页？ 我们有两个选项：

- 创建两个嵌套的主控页`AdminNestedSite.master`和`AdminNestedAlternate.master`，并将它们绑定到顶级母版页`Site.master`和`Alternate.master`分别。 在`BasePage`，然后，我们将设置`Page`对象的`MasterPageFile`到适当的嵌套母版页。
- 创建单个嵌套的母版页并且具有使用此特定的母版页的内容页。 然后，在运行时，我们将需要设置嵌套的主控页的`MasterPageFile`适当在运行时的顶级母版页的属性。 (如你可能会找到到目前为止，也有母版页`MasterPageFile`属性。)

我们将使用第二个选项。 创建单个嵌套母版页文件中的`~/Admin`文件夹名为`AdminNested.master`。 因为同时`Site.master`和`Alternate.master`具有相同的 ContentPlaceHolder 控件集，并不重要哪些母版页你将其绑定到，虽然我建议你将其绑定到`Site.master`为一致性的起见。


[![将嵌套的母版页添加到 ~/Admin 文件夹中。](nested-master-pages-cs/_static/image26.png)](nested-master-pages-cs/_static/image25.png)

**图 09**： 添加到嵌套的母版页`~/Admin`文件夹。 ([单击以查看实际尺寸的图像](nested-master-pages-cs/_static/image27.png))


嵌套的母版页绑定到母版页与四个 ContentPlaceHolder 控件，因为 Visual Studio 将添加四个内容控件添加到新嵌套的主控页文件的初始标记。 像我们在步骤 2 和 3，在每个内容控件中，添加 ContentPlaceHolder 控件并向其提供与顶级母版页的 ContentPlaceHolder 控件相同的名称。 此外将以下标记添加到对应于的内容控件`MainContent`ContentPlaceHolder:


[!code-html[Main](nested-master-pages-cs/samples/sample8.html)]

接下来，定义`instructions`CSS 类`Styles.css`和`AlternateStyles.css`CSS 文件。 以下的 CSS 规则将导致与样式的 HTML 元素`instructions`类来显示时带有浅黄色背景色，黑色、 实心边框：


[!code-css[Main](nested-master-pages-cs/samples/sample9.css)]

因为嵌套的母版页中添加了此标记，它才会在使用此嵌套的母版页 （也就是说，管理部分中的页） 这些页中显示。

进行这些添加嵌套母版页后, 其声明性标记应看起来类似以下：


[!code-aspx[Main](nested-master-pages-cs/samples/sample10.aspx)]

请注意，每个内容控件有 ContentPlaceHolder 控制的和的 ContentPlaceHolder 控件的`ID`属性分配与顶级母版页中的相应 ContentPlaceHolder 控件相同的值。 此外，管理特定部分的标记将出现在`MainContent`ContentPlaceHolder。

图 10 显示`AdminNested.master`嵌套的母版页查看通过 Visual Studio 设计器时。 你可以看到在顶部的黄色框中的说明进行操作`MainContent`内容控件。


[![嵌套的母版页扩展顶层的主页面，以包括有关管理员的说明。](nested-master-pages-cs/_static/image29.png)](nested-master-pages-cs/_static/image28.png)

**图 10**： 嵌套的母版页扩展顶层的主页面，以包括有关管理员的说明。 ([单击以查看实际尺寸的图像](nested-master-pages-cs/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>步骤 5： 更新现有的内容页，以使用新的嵌套的母版页

我们将新的内容页添加到管理部分我们需要将其绑定到的任何时候`AdminNested.master`我们刚刚创建的母版页。 但是，有关现有哪些内容页？ 目前，站点中的所有内容页派生自`BasePage`类，该类在运行时以编程方式设置内容页的母版页。 这不是我们想要在管理部分中的内容页面的行为。 相反，我们希望始终使用这些内容页`AdminNested.master`页。 它将嵌套 master 页后，可以选择在运行时右顶级内容页面的责任。

若要实现的最佳方式这所需的行为是创建一个名为的新自定义的基本页类`AdminBasePage`扩展`BasePage`类。 `AdminBasePage`然后可以重写`SetMasterPageFile`并设置`Page`对象的`MasterPageFile`为硬编码值"~ / Admin/AdminNested.master"。 这种方式，任何页，派生自`AdminBasePage`将使用`AdminNested.master`，而派生自的任何页面`BasePage`将具有其`MasterPageFile`属性设置为动态"~ / Site.master"或"~ / Alternate.master"的值`MyMasterPage`会话变量。

首先，通过添加新的类文件与`App_Code`文件夹名为`AdminBasePage.cs`。 具有`AdminBasePage`扩展`BasePage`，然后重写`SetMasterPageFile`方法。 在该方法将`MasterPageFile`值"~ / Admin/AdminNested.master"。 你的类进行这些更改后文件应类似于以下：


[!code-csharp[Main](nested-master-pages-cs/samples/sample11.cs)]

我们现在需要将现有内容页中的管理部分派生自`AdminBasePage`而不是`BasePage`。 转到每个内容页中的代码隐藏类文件`~/Admin`文件夹和进行此更改。 例如，在`~/Admin/Default.aspx`将更改从代码隐藏类声明：


[!code-csharp[Main](nested-master-pages-cs/samples/sample12.cs)]

到:


[!code-csharp[Main](nested-master-pages-cs/samples/sample13.cs)]

图 11 描述了如何顶级母版页 (`Site.master`或`Alternate.master`)，嵌套的母版页 (`AdminNested.master`)，以及管理部分内容页与另一个。


[![嵌套的母版页定义特定内容所致的管理部分中的页](nested-master-pages-cs/_static/image32.png)](nested-master-pages-cs/_static/image31.png)

**图 11**: 嵌套 Master 页定义内容特定于管理部分中的页 ([单击以查看实际尺寸的图像](nested-master-pages-cs/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>步骤 6： 镜像母版页的公共方法和属性

回想一下，`~/Admin/AddProduct.aspx`和`~/Admin/Products.aspx`页与母版页以编程方式进行交互：`~/Admin/AddProduct.aspx`调用母版页的公共`RefreshRecentProductsGrid`方法并设置其`GridMessageText`属性;`~/Admin/Products.aspx`具有的事件处理程序`PricesDoubled`事件。 在前面的教程中，我们将创建一个抽象`BaseMasterPage`定义这些公共成员的类。

`~/Admin/AddProduct.aspx`和`~/Admin/Products.aspx`页假定其母版页派生自`BaseMasterPage`类。 `AdminNested.master`页上，但是，当前扩展`System.Web.UI.MasterPage`类。 因此，当访问`~/Admin/Products.aspx``InvalidCastException`引发并显示消息:"找不到类型的对象强制转换 ASP.admin\_adminnested\_master 类型 BaseMasterPage。"

若要解决此我们需要将`AdminNested.master`代码隐藏类扩展`BaseMasterPage`。 更新从嵌套的主控页的代码隐藏类声明：


[!code-csharp[Main](nested-master-pages-cs/samples/sample14.cs)]

到:


[!code-csharp[Main](nested-master-pages-cs/samples/sample15.cs)]

我们还未完成。 因为`BaseMasterPage`类为抽象类，我们需要重写`abstract`成员，`RefreshRecentProductsGrid`和`GridMessageText`。 这些成员的顶级母版页中用于更新其用户界面。 (事实上，仅`Site.master`母版页使用这些方法，虽然这两个顶级母版页实现这些方法，因为同时扩展`BaseMasterPage`。)

尽管我们需要在这些成员以实现`AdminNested.master`，这些实现需要做的就是只需在顶级母版页由嵌套母版页中调用相同的成员。 例如，当管理部分中的内容页调用嵌套的母版页的`RefreshRecentProductsGrid`方法，所有嵌套的主控页需要执行操作，反过来，则调用`Site.master`或`Alternate.master`的`RefreshRecentProductsGrid`方法。

若要实现此目的，首先将以下内容添加`@MasterType`指令到顶部`AdminNested.master`:


[!code-aspx[Main](nested-master-pages-cs/samples/sample16.aspx)]

回想一下，`@MasterType`指令将强类型的属性添加到名为的代码隐藏类`Master`。 然后，重写`RefreshRecentProductsGrid`和`GridMessageText`成员，并只需委托调用`Master`相应的方法：


[!code-csharp[Main](nested-master-pages-cs/samples/sample17.cs)]

此代码中的位置，你应能够访问并在管理部分中使用内容的页。 图 12 显示`~/Admin/Products.aspx`页上查看通过浏览器时。 如你所见，该页面将包括管理说明框中，嵌套的母版页中定义。


[![管理部分中的内容页包括在每个页面顶部的说明](nested-master-pages-cs/_static/image35.png)](nested-master-pages-cs/_static/image34.png)

**图 12**： 在每个页顶部的管理部分包括说明中的内容页 ([单击以查看实际尺寸的图像](nested-master-pages-cs/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>步骤 7： 在运行时使用相应的顶级母版页

完全正常运行管理部分中的所有内容页时，它们全都使用相同的顶级母版页并忽略母版页在用户选择`ChooseMasterPage.aspx`。 此行为是因为，嵌套的主控页具有其`MasterPageFile`属性静态设置为`Site.master`中其`<%@ Master %>`指令。

若要使用我们需要将设置的最终用户选择的顶级 master 页`AdminNested.master`的`MasterPageFile`属性中的值`MyMasterPage`会话变量。 因为我们设置内容页的`MasterPageFile`中的属性`BasePage`，您可能会认为我们将设置嵌套的主控页的`MasterPageFile`中的属性`BaseMasterPage`或`AdminNested.master`的代码隐藏类。 这将不起作用，但是，因为我们需要设置`MasterPageFile`PreInit 阶段结束属性。 我们可以以编程方式点击到母版页上的页面生命周期的最早时间是 Init 舞台 （它在 PreInit 阶段后出现）。

因此，我们需要将设置嵌套的主控页的`MasterPageFile`从内容页的属性。 只有内容页使用`AdminNested.master`母版页派生自`AdminBasePage`。 因此，我们可以那里放置此逻辑。 在步骤 5 中我们重写`SetMasterPageFile`方法，设置`Page`对象的`MasterPageFile`属性设置为"~ / Admin/AdminNested.master"。 更新`SetMasterPageFile`还要设置母版页的`MasterPageFile`属性存储在会话中的结果：


[!code-csharp[Main](nested-master-pages-cs/samples/sample18.cs)]

`GetMasterPageFileFromSession`方法，我们将添加到`BasePage`在前面的教程中，返回的适当的母版页文件路径基于会话的变量值的类。

进行此更改到位后，用户的母版页选择将被传递到管理部分。 图 13 显示为图 12，但用户更改其母版页选定范围向后的同一页面`Alternate.master`。


[![嵌套的管理页使用用户选定的顶级母版页](nested-master-pages-cs/_static/image38.png)](nested-master-pages-cs/_static/image37.png)

**图 13**： 嵌套管理页使用顶层 Master 页中选择用户 ([单击以查看实际尺寸的图像](nested-master-pages-cs/_static/image39.png))


## <a name="summary"></a>摘要

多 like 如何在内容页可以将绑定到母版页，它是可以创建嵌套具有子级的母版页的主控页将绑定到一个父级母版页。 子母版页可能定义的其父级的 ContentPlaceHolders; 每个内容控件它然后可以将自己 ContentPlaceHolder 控件 （以及其他标记） 添加到这些内容的控件。 嵌套的母版页是其中所有页都共享总体的外观和感觉，但站点某些部分需要唯一的自定义项的大型 web 应用程序中非常有用。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [嵌套的 ASP.NET 母版页](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [嵌套的母版页和 VS 2005 设计时的提示](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [VS 2008 嵌套主控页支持](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多个 ASP/ASP.NET 丛书和 4GuysFromRolla.com 创始人，具有已使用 Microsoft Web 技术自 1998 年。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 3.5 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)。 可以在达到 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](specifying-the-master-page-programmatically-cs.md)
[下一页](creating-a-site-wide-layout-using-master-pages-vb.md)
