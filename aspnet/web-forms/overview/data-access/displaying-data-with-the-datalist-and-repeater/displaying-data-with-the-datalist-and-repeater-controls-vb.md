---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
title: "使用 DataList 和转发器控件 (VB) 中显示数据 |Microsoft 文档"
author: rick-anderson
description: "前面的教程中我们具有用于 GridView 控件以显示数据。 从开始本教程，我们看一下生成使用的常见报表模式..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 58618954-a9ed-4ca0-8c2d-95a5ffd9c03e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 20a092ee2886932664705c22c3aa88d8a2f7f0ef
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-vb"></a>使用 DataList 和转发器控件 (VB) 中显示数据
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_VB.exe)或[下载 PDF](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/datatutorial29vb1.pdf)

> 前面的教程中我们具有用于 GridView 控件以显示数据。 从开始本教程，我们看看 DataList 和转发器控件中，使用的常见报表模式生成开头显示与这些控件的数据的基础知识。


## <a name="introduction"></a>介绍

在过去的所有示例中的所有 28 教程，如果我们需要显示我们到 GridView 控件打开数据源的多个记录。 GridView 呈现为数据源中的每个记录的行在列中显示的记录的数据字段。 虽然 GridView 使变得很容易到显示，通过排序、 编辑和删除数据的页，其外观会有点 boxy。 此外，负责标记为 GridView 的结构固定包括 HTML`<table>`与表行 (`<tr>`) 针对每个记录和表格单元格 (`<td>`) 的每个字段。

若要显示多个记录时，请提供更高程度的自定义项中的外观和呈现的标记，ASP.NET 2.0 提供 DataList 和转发器控件 (也 ASP.NET 版本中提供，这两种了 1.x)。 DataList 和转发器控件呈现其内容使用的模板，而不是 BoundFields，CheckBoxFields，ButtonFields，依次类推。 GridView，如 DataList 呈现为 HTML `<table>`，但可实现多个数据源记录要显示每个表行。 转发器，另一方面，呈现任何其他标记，而不仅仅什么你显式指定，并需要精确地控制发出的标记时的理想候选项。

通过下一步几十个左右教程中，我们将查看生成 DataList 和转发器控件中，使用的常见报表模式开头使用这些控件模板显示数据的基础知识。 我们将了解如何设置这些控件的格式如何更改数据源中的记录 DataList、 常见母版/详细方案、 方式进行编辑和删除数据，布局如何通过记录页上，依次类推。

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>步骤 1： 添加 DataList 和转发器教程网页

在开始本教程之前，让 s 先花点时间以添加我们需要用于本教程和显示使用 DataList 和转发器的数据处理的下一步几个教程的 ASP.NET 页面。 首先创建一个新文件夹中名为的项目`DataListRepeaterBasics`。 接下来，将以下五个 ASP.NET 页添加到此文件夹中，让所有的它们配置为使用母版页`Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![创建 DataListRepeaterBasics 文件夹并添加教程 ASP.NET 页](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image1.png)

**图 1**： 创建`DataListRepeaterBasics`文件夹并添加教程 ASP.NET 页


打开`Default.aspx`页上，并将其拖`SectionLevelTutorialListing.ascx`从用户控件`UserControls`到设计图面上的文件夹。 用户控件中，我们在中创建的[母版页和网站的导航](../introduction/master-pages-and-site-navigation-vb.md)教程中，枚举站点图，并会显示项目符号列表中的当前节中的教程。


[![SectionLevelTutorialListing.ascx 用户控件添加到 Default.aspx](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image2.png)

**图 2**： 添加`SectionLevelTutorialListing.ascx`用户控件`Default.aspx`([单击以查看实际尺寸的图像](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image4.png))


为了使项目符号列表显示 DataList 和转发器教程中我们将创建，我们需要将它们添加到站点映射。 打开`Web.sitemap`文件并添加自定义按钮站点映射节点标记之后添加以下标记：


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample1.xml)]


![更新包括新的 ASP.NET 页的站点图](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image5.png)

**图 3**： 更新包括新的 ASP.NET 页的站点图


## <a name="step-2-displaying-product-information-with-the-datalist"></a>步骤 2： 显示与 DataList 的产品信息

类似于 FormView，DataList 控件呈现输出的 s 取决于模板而不是 BoundFields、 CheckBoxFields，依次类推。 与 FormView，不同 DataList 旨在显示一组记录而不是一个作对。 让我们来开始本教程中看一下绑定到 DataList 的产品信息。 首先打开`Basics.aspx`页面`DataListRepeaterBasics`文件夹。 接下来，将从工具箱拖动到设计器中 DataList。 如图 4 所示之前指定 DataList 的模板, 设计器会将其显示为灰色的框。


[![从工具箱拖动到设计器中拖动 DataList](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image6.png)

**图 4**： 将 DataList 从工具箱拖到设计器 ([单击以查看实际尺寸的图像](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image8.png))


从 DataList s 智能标记，添加新对象数据源并将其配置为使用`ProductsBLL`类的`GetProducts`方法。 由于我们在本教程中，创建只读 DataList 重新设置为 (None) 下拉列表中向导的插入，更新和删除选项卡。


[![选择创建新对象数据源](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image9.png)

**图 5**： 选择创建新对象数据源 ([单击以查看实际尺寸的图像](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image11.png))


[![配置对象数据源以使用 ProductsBLL 类](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image12.png)

**图 6**： 配置使用 ObjectDataSource`ProductsBLL`类 ([单击以查看实际尺寸的图像](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image14.png))


[![检索有关所有使用 GetProducts 方法的产品信息](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image15.png)

**图 7**： 检索信息有关的所有产品使用`GetProducts`方法 ([单击以查看实际尺寸的图像](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image17.png))


配置对象数据源并将其与通过其智能标记 DataList 关联之后, Visual Studio 自动创建`ItemTemplate`中显示的名称和值的数据源返回每个数据字段 DataList (请参阅标记下）。 此默认设置`ItemTemplate`的外观等同于将数据源绑定到通过设计器 FormView 时自动创建的模板。


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample2.aspx)]

> [!NOTE]
> 回想一下，在将数据源绑定到 FormView 控件通过 FormView s 智能标记，Visual Studio 创建`ItemTemplate`， `InsertItemTemplate`，和`EditItemTemplate`。 DataList，但是，与仅`ItemTemplate`创建。 这是因为 DataList 不具有相同的内置编辑和插入 FormView 所提供的支持。 DataList 确实包含编辑和删除相关的事件，并且编辑和删除支持可添加进行少量的代码，但存在 s 作为与 FormView 不简单的全新支持。 我们将了解如何以包括编辑和删除的 DataList 的支持，在将来的教程。


允许 s 花一些时间来提高此模板的外观。 而不是显示的所有数据字段，让我们来仅显示产品名称、 供应商、 类别、 每个单元和单价数量。 此外，让 s 显示中的名称`<h4>`标题，并使用其余字段的布局`<table>`标题的下面。

若要进行这些更改，你可以是使用模板编辑从 DataList s 智能标记单击编辑模板链接，或者你可以修改通过页 s 声明性语法手动模板设计器中的功能。 如果在设计器中使用编辑模板选项，你得到的标记可能不完全匹配，匹配的以下标记，但通过查看浏览器看起来应非常相似的屏幕快照中图 8 所示。


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample3.aspx)]

> [!NOTE]
> 上面的示例将标签 Web 控制其`Text`属性分配数据绑定语法的值。 或者，我们无法省略标签一起，只是数据绑定语法中键入。 也就是说，而不是使用`<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />`我们本来也可以改为使用声明性语法`<%# Eval("CategoryName") %>`。


在标签 Web 控件内保留，但是，提供两大优势。 首先，它提供一种方便的方法的格式设置在基于的数据的数据，正如我们看到在下一步的教程。 其次，设计器不 t 显示声明性数据绑定语法中的编辑模板选项显示在某些 Web 控件外。 相反，编辑模板接口旨在为使用静态标记提供方便，Web 控件，并假定任何数据绑定将通过编辑数据绑定对话框中，可从 Web 控件的智能标记进行访问。

因此，当使用 DataList，提供了编辑通过设计器的模板的选项，我愿意使用标签 Web 控件，因此内容是可通过编辑模板接口访问。 正如我们很快将看到，转发器需要从源视图可编辑的模板的内容。 因此，如果我不知道需要格式编写中继器的模板通常，我将省略标签 Web 控件时数据的外观绑定文本基于编程逻辑。


[![每个产品的输出是呈现使用 DataList 的 ItemTemplate](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image18.png)

**图 8**： 每个产品的输出是呈现使用 DataList s `ItemTemplate` ([单击以查看实际尺寸的图像](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>步骤 3： 改善 DataList 的外观

GridView，如 DataList 提供了多个与样式有关的属性，如`Font`， `ForeColor`， `BackColor`， `CssClass`， `ItemStyle`， `AlternatingItemStyle`， `SelectedItemStyle`，依次类推。 在使用 GridView 和说明控件，我们创建中的外观文件`DataWebControls`预定义的主题`CssClass`这两个控件的属性和`CssClass`为多个其子属性的属性 (`RowStyle``HeaderStyle`，依次类推)。 允许为 DataList 执行同样的 s。

中所述[将数据显示与 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md)教程，外观文件指定的默认 Web 控件与外观相关属性; 主题是定义的外观、 CSS、 映像和 JavaScript 文件的集合网站获得特定外观和感觉。 在*将数据显示与 ObjectDataSource*教程中，我们创建`DataWebControls`主题 (实现为一个文件夹内`App_Themes`文件夹)，其，目前，两个外观文件-`GridView.skin`和`DetailsView.skin`. 让我们来添加三个外观文件指定 DataList 的预定义的样式设置。

若要添加的外观文件，右键单击`App_Themes/DataWebControls`文件夹，选择添加新项，并从列表中选择外观文件选项。 为 `DataList.skin` 文件命名。


[![创建一个名为 DataList.skin 的新外观文件](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image21.png)

**图 9**： 创建新的外观文件命名`DataList.skin`([单击以查看实际尺寸的图像](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image23.png))


使用的以下标记`DataList.skin`文件：


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample4.aspx)]

这些设置将相同的 CSS 类分配给适当的 DataList 属性，如用于 GridView 和说明控件。 此处使用的 CSS 类`DataWebControlStyle`， `AlternatingRowStyle`，`RowStyle`等中定义`Styles.css`文件，并在前面的教程中添加了。

添加此外观文件后，在设计器中 （可能需要刷新设计器视图以查看新的外观文件; 从视图菜单上的效果，请选择刷新） 更新 DataList 的外观。 如图 10 所示，每个交替产品均具有一种浅色粉红色背景色。


[![创建一个名为 DataList.skin 的新外观文件](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image24.png)

**图 10**： 创建新的外观文件命名`DataList.skin`([单击以查看实际尺寸的图像](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>步骤 4： 浏览 DataList s 其他模板

除了`ItemTemplate`，DataList 支持六个其他可选模板：

- `HeaderTemplate`如果提供，将标题行添加到输出，并用于呈现此行
- `AlternatingItemTemplate`用于呈现交替项
- `SelectedItemTemplate`用于呈现所选的项;所选的项目属于其索引对应于 DataList s 项[`SelectedIndex`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate`用于呈现所编辑的项
- `SeparatorTemplate`如果提供，将添加每个项之间的分隔符和用于呈现此分隔符
- `FooterTemplate`-如果提供，将的页脚行添加到输出，并用于呈现此行

指定时`HeaderTemplate`或`FooterTemplate`，DataList 将一个附加的页眉或页脚行添加到呈现的输出。 如 GridView 的页眉和页脚行、 页眉和页脚中 DataList 不绑定到数据。 因此中的任何数据绑定语法`HeaderTemplate`或`FooterTemplate`尝试访问绑定的数据都将返回空白字符串。

> [!NOTE]
> 正如我们看到在[GridView 的页脚中显示摘要信息](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md)教程中，虽然页眉和页脚行不 t 支持数据绑定语法，特定于数据的信息可以直接注入从这些行GridView 的`RowDataBound`事件处理程序。 此方法可用于同时计算正在运行的总计或其他信息从数据绑定到控件，以及将该信息分配给页脚。 此相同的概念可应用于 DataList 和转发器控件中;唯一的区别是 DataList 和转发器创建的事件处理程序`ItemDataBound`事件 (而不是为`RowDataBound`事件)。


对于我们的示例，让 s 具有标题中的 DataList s 结果顶部显示的产品信息`<h3>`标题。 若要完成此操作，将添加`HeaderTemplate`包含相应的标记。 从设计器中，这可通过单击 DataList s 智能标记中的编辑模板链接，选择从下拉列表中，标头的模板并在之后选取样式下拉列表中的标题 3 选项的文本中键入列表 （请参阅图 11）。


[![添加与文本产品信息 HeaderTemplate](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image27.png)

**图 11**： 添加`HeaderTemplate`与文本产品信息 ([单击以查看实际尺寸的图像](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image29.png))


或者，这可以添加以声明方式通过输入中的以下标记`<asp:DataList>`标记：


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample5.html)]

若要添加的每个产品列表之间的空间位，让我们来添加`SeparatorTemplate`包括每个部分之间的连线。 水平标尺标记 (`<hr>`)，将添加一个此类分隔线。 创建`SeparatorTemplate`，使其具有以下标记：


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample6.html)]

> [!NOTE]
> 如`HeaderTemplate`和`FooterTemplates`、`SeparatorTemplate`未绑定到任何记录从数据源并因此不能直接访问的数据源绑定到 DataList 的记录。


建立此添加后, 查看通过浏览器页面时其外观应类似于图 12。 请注意标头行和每个产品列表之间的行。


[![DataList 包括标头行和每个产品列表之间的水平规则](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image30.png)

**图 12**: DataList 包括标头行和水平规则之间每个产品列出 ([单击以查看实际尺寸的图像](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>步骤 5： 呈现与转发器控件的特定标记

如果访问图 12 的 DataList 示例时，可从你的浏览器中执行视图/源，你将看到 DataList 发出 HTML`<table>`包含的表行 (`<tr>`) 使用单个表单元格 (`<td>`) 为每个项绑定到DataList。 此输出，事实上，等同于什么会发出从与单个 TemplateField GridView。 我们将在将来的教程中看到的 DataList 允许进一步自定义输出中，使我们能够显示每个表行的多个数据源记录。

如果您不想发出 HTML `<table>`，不过？ 进行生成的数据的 Web 控件的标记的总计和完全控制，我们必须使用转发器控件。 DataList，如基于模板构造转发器。 转发器，但是，仅提供以下五个模板：

- `HeaderTemplate`如果提供，将添加项之前指定的标记
- `ItemTemplate`用于呈现项
- `AlternatingItemTemplate`如果提供，，用于呈现交替项
- `SeparatorTemplate`如果提供，将添加每个项之间的指定的标记
- `FooterTemplate`-如果提供，将指定的标记添加的项目之后

在 ASP.NET 中 1.x，控制通常用于显示其数据来自某些数据源的项目符号列表转发器。 在这种情况下，`HeaderTemplate`和`FooterTemplates`将包含到开始和结束`<ul>`标记，分别而`ItemTemplate`将包含`<li>`使用数据绑定语法的元素。 仍这种方法可以使用在 ASP.NET 2.0 中，正如我们所看到的两个示例中[母版页和网站的导航](../introduction/master-pages-and-site-navigation-vb.md)教程：

- 在`Site.master`母版页，中继器用于显示的顶层站点映射的内容 （基本报告、 筛选报表，自定义格式设置等） 的项目符号列表; 另一个嵌套中继器用于显示的子部分顶级部分
- 在`SectionLevelTutorialListing.ascx`，中继器用于显示当前站点映射节的子部分的项目符号列表

> [!NOTE]
> ASP.NET 2.0 引入了新[如何](https://msdn.microsoft.com/library/ms228101.aspx)，这可以绑定到数据源控件为了显示简单的项目符号列表。 如何控制与我们不需要指定任何列表相关 HTML 中;相反，我们只用于表示要显示为每个列表项的文本的数据字段。


作为 catch 的转发器服务，Web 控件的所有数据。 如果不是生成所需的标记的现有控件，可以使用转发器控件。 为了说明使用转发器，让 s 具有如上所示产品信息 DataList 在步骤 2 中创建的类别的列表。 具体而言，让 s 具有类别显示在单行 HTML`<table>`每个类别都显示为表中的列。

若要完成此操作，首先从工具箱中拖动到设计器中，上面产品信息 DataList 拖动重复器控件。 与 DataList，转发器开始将显示为灰色的框在定义它的模板之前。


[![将转发器添加到设计器](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image33.png)

**图 13**: 转发器添加到设计器 ([单击以查看实际尺寸的图像](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image35.png))


此处的中继器 s 智能标记中只有一个选项： 选择数据源。 选择创建新对象数据源并将其配置为使用`CategoriesBLL`类的`GetCategories`方法。


[![创建新对象数据源](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image36.png)

**图 14**： 创建新对象数据源 ([单击以查看实际尺寸的图像](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image38.png))


[![配置对象数据源以使用 CategoriesBLL 类](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image39.png)

**图 15**： 配置使用 ObjectDataSource`CategoriesBLL`类 ([单击以查看实际尺寸的图像](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image41.png))


[![检索有关使用 GetCategories 方法的类别的所有信息](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image42.png)

**图 16**： 检索信息有关的所有类别使用`GetCategories`方法 ([单击以查看实际尺寸的图像](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image44.png))


DataList，与 Visual Studio 不会自动创建 ItemTemplate 为转发器后将其绑定到数据源。 此外，转发器的模板不能通过设计器配置，并且必须以声明方式指定。

显示类别为单行`<table>`具有每个类别的列，我们需要转发器发出类似于以下标记：


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample7.html)]

由于`<td>Category X</td>`文本是重复发生，将出现在转发器的 ItemTemplate 的部分。 显示在它的前面的标记`<table><tr>`-将被放入`HeaderTemplate`时的结束标记的`</tr></table>`-将放入`FooterTemplate`。 若要输入的这些模板设置，请转到 ASP.NET 页的声明性部分通过单击左下角中的源按钮和以下语法中的类型：


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample8.aspx)]

转发器发出的精确指定它的模板、 只、 执行任何操作较少的标记。 图 17 显示中继器的输出查看通过浏览器时。


[![单行 HTML&lt;表&gt;列出一个单独的列中的每个类别](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image45.png)

**图 17**: 单行 HTML`<table>`列出一个单独的列中的每个类别 ([单击以查看实际尺寸的图像](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>步骤 6： 改善转发器的外观

由于中继器发出精确地指定由其模板的标记，它应该为不足为奇存在中继器没有与样式有关的属性。 若要更改的转发器生成的内容的外观，我们必须手动直接向中继器的模板中添加所需的 HTML 或 CSS 内容。

对于我们的示例，让我们来使用 DataList 中的交替行类似备用，背景颜色的类别列。 若要实现此目的，我们需要分配`RowStyle`对每个转发器项目的 CSS 类和`AlternatingRowStyle`对通过每个交替转发器项目的 CSS 类`ItemTemplate`和`AlternatingItemTemplate`模板，如下所示：


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample9.aspx)]

允许 s 还将标头行添加到与产品类别的文本输出。 由于我们不 t 知道多少列我们导致`<table>`将组成，生成保证跨所有列标题行的最简单方法是使用*两个* `<table>` s。 第一个`<table>`标头行和将包含第二个的单行更行的行，将包含两行`<table>`的系统中有一列用于每个类别。 也就是说，我们想要发出以下标记：


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample10.html)]

以下`HeaderTemplate`和`FooterTemplate`产生所需的标记：


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample11.aspx)]

进行这些更改之后，图 18 显示转发器。


[![类别列交替效果的背景色和包含一个标题行](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image48.png)

**图 18**: 类别列备用背景色和包括标题行中 ([单击以查看实际尺寸的图像](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image50.png))


## <a name="summary"></a>摘要

虽然 GridView 控件更便于显示，编辑、 删除、 排序和分页浏览数据，外观是 boxy 和类似网格。 更灵活地控制外观，我们需要打开到 DataList 或转发器控件。 这两种控件显示一组记录使用模板而不 BoundFields、 CheckBoxFields，依次类推。

DataList 呈现为 HTML `<table>` ，默认情况下，每个数据源记录中显示单个表行，就像与单个 TemplateField GridView 一样。 我们将会在将来的教程中看到的但是，DataList 确实允许多个记录，以显示每个表行。 转发器，另一方面，严格发出其模板; 中指定的标记它不会添加任何附加标记，并因此通常用于除 HTML 元素中显示数据`<table>`（如项目符号列表）。

DataList 和转发器提供了更灵活地其呈现输出，它们没有很多 GridView 中的内置功能。 因为我们将查看即将到来的教程中，这些功能的一些可以返回插入，而无需太多工作，但不要继续记住，使用 DataList 或转发器的替代 GridView，不会限制你可以使用而无需实现这些功能的功能自己。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Yaakov Ellis、 沈 Shulok、 徐 Schmidt 和 Stacy Park。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](nested-data-web-controls-cs.md)
[下一页](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
