---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
title: "显示每个行与 DataList 控件 (C#) 的多个记录 |Microsoft 文档"
author: rick-anderson
description: "在本简短的教程，我们将探讨如何自定义其 RepeatColumns 和 RepeatDirection 属性通过 DataList 的布局。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: cf5acaf5-d4f6-4957-badc-b89956b285f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e9f04089afdbeb1b13725536c9fe97951ee8ca5c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-c"></a>显示多个记录每行与 DataList 控件 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_CS.exe)或[下载 PDF](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/datatutorial31cs1.pdf)

> 在本简短的教程，我们将探讨如何自定义其 RepeatColumns 和 RepeatDirection 属性通过 DataList 的布局。


## <a name="introduction"></a>介绍

DataList 示例我们已在过去的两个教程中看到已作为某个单列 HTML 中的行呈现其数据源中的每个记录`<table>`。 虽然这是默认 DataList 行为，它是非常轻松地自定义 DataList 显示，以便数据源项分布在多列中，多行的表。 此外，它可能具有的所有数据源中的单行，多列 DataList 显示的项目。

我们可以自定义 DataList 的布局，通过其`RepeatColumns`和`RepeatDirection`属性，分别指示多少列将呈现出来并是否这些项垂直或水平的布局方式。 图 1 中，例如，显示具有三个列的表中显示产品信息 DataList。


[![DataList 显示每行的三个产品](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image1.png)

**图 1**: DataList 演示三种产品每行 ([单击以查看实际尺寸的图像](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image3.png))


通过显示每行的多个数据源项，DataList 可以更有效地利用水平屏幕空间。 在本简短的教程，我们将探讨这两个 DataList 属性。

## <a name="step-1-displaying-product-information-in-a-datalist"></a>步骤 1： 在 DataList 中显示产品信息

我们检查之前`RepeatColumns`和`RepeatDirection`属性，可让 s 首先 DataList 我们在页面上创建，其中列出了使用标准的单一列中，多行表布局的产品信息。 对于此示例，让我们来显示产品名称、 类别和价格使用以下标记：


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample1.html)]

我们已了解如何将数据绑定到在上一示例中，DataList，因此我将快速移动完成这些步骤。 首先打开`RepeatColumnAndDirection.aspx`页面`DataListRepeaterBasics`文件夹，然后拖动 DataList 从工具箱中拖动到设计器。 从 DataList s 智能标记中，选择创建新对象数据源并将其配置为请求从其数据`ProductsBLL`类的`GetProducts`方法，选择 （无） 选项从向导的插入、 更新和删除选项卡。

在创建并将新对象数据源绑定到 DataList 之后, Visual Studio 自动创建`ItemTemplate`为每个产品数据字段显示的名称和值。 调整`ItemTemplate`直接通过声明性标记或编辑模板从选项 DataList s 智能标记中，以使它使用上面所示，替换的标记*产品名称*，*类别名称*，和*价格*具有适当的数据绑定语法用于将值分配给的标签控件的文本其`Text`属性。 在更新后`ItemTemplate`，页面 s 声明性标记应类似于以下：


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample2.aspx)]

请注意，已包含中的格式说明符`Eval`数据绑定语法`UnitPrice`，格式设置为货币的返回的值`Eval("UnitPrice", "{0:C}").`

需要一段时间来访问你的浏览器中的页。 如图 2 所示，DataList 将呈现为一个产品中单个列中，多行的表。


[![默认情况下，为单个列中，多行表 DataList 呈现](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image4.png)

**图 2**： 默认情况下，为单个列，多行表的 DataList 将呈现 ([单击以查看实际尺寸的图像](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>步骤 2： 更改 DataList 的布局方向

默认行为时 DataList 为垂直在单个列中，多行表中，其项进行布局更改此行为可以轻松地通过 DataList s [ `RepeatDirection`属性](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx)。 `RepeatDirection`属性可以接受两个可能值之一：`Horizontal`或`Vertical`（默认值）。

通过更改`RepeatDirection`属性从`Vertical`到`Horizontal`，DataList 呈现其记录在单个行中，创建每个数据源项的一个列。 为了说明这种效果，设计器中 DataList 上单击，然后，从属性窗口中更改`RepeatDirection`属性从`Vertical`到`Horiztonal`。 立即在这样做，设计器调整 DataList 的布局，创建多列的单行更行接口 （请参见图 3）。


[![RepeatDirection 属性指示如何方向 DataList 的项是放置出](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image7.png)

**图 3**:`RepeatDirection`属性指示的方向 DataList 的项的放置出的方式 ([单击以查看实际尺寸的图像](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image9.png))


当显示少量数据，单行，多列的表可能是理想的方式，以最大化屏幕的实际空间。 但是，对于较大的数据的卷，单个行将需要多个列，这些项适合于屏幕关闭右侧该可以 t 的推送。 图 4 显示在单行 DataList 中呈现时的产品。 由于有许多产品 (超过 80)，用户将需要向下滚动向右侧以查看有关每个产品的信息。


[![对于足够大的数据源，单个列 DataList 需要水平滚动](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image10.png)

**图 4**： 为足够大数据源、 的单个列 DataList 将需要水平滚动 ([单击以查看实际尺寸的图像](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>步骤 3： 在多列中，多行表中显示数据

若要创建多列中，多行 DataList，我们需要设置[`RepeatColumns`属性](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx)到要显示的列数。 默认情况下，`RepeatColumns`属性设置为 0，这将导致 DataList 要在单个行或列中显示的所有项 (根据的值`RepeatDirection`属性)。

对于我们的示例，让我们来显示每个表行的三种产品。 因此，设置`RepeatColumns`为 3 的属性。 此更改后，需要一段时间来在浏览器中查看结果。 如图 5 所示，现在是三个列中，多行表中列出的产品。


[![每行显示三种产品](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image13.png)

**图 5**： 三种产品显示每个行 ([单击以查看实际尺寸的图像](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image15.png))


`RepeatDirection`属性会影响如何布局 DataList 中的项。图 5 显示的结果与`RepeatDirection`属性设置为`Horizontal`。 请注意，牛奶、 改动和茴香糖浆的前三个产品的布局方式从左到右，自上而下的顺序。 下方的前三个行中显示的下一步三种产品 （从 Chef Anton 的 Cajun Seasoning 开始）。 更改`RepeatDirection`属性改回`Vertical`，但是，从上到下这些产品进行布局，从左到右，如图 6 所示。


[![在这里，这些产品均放置出垂直](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image16.png)

**图 6**： 在这里，这些产品均放置出垂直 ([单击以查看实际尺寸的图像](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image18.png))


生成的表中显示的行数取决于绑定到 DataList 的总记录数。 确切地说，它的数据源项的总数目上限除以的 s`RepeatColumns`属性值。 由于`Products`表当前有 84 产品，这是可由 3 整除、 有 28 行。 如果数据源中的项的数目和`RepeatColumns`属性值不整除，则最后一个行或列将具有空白单元格。 如果`RepeatDirection`设置为`Vertical`，则最后一列将具有空单元格; 如果`RepeatDirection`是`Horizontal`，则最后一行将具有空单元格。

## <a name="summary"></a>摘要

DataList，默认情况下，列出其一个单一列中，多行的表，该表模仿与单个 TemplateField GridView 的布局中的项。 可接受此默认布局时，我们可以通过显示每行的多个数据源项最大化屏幕的实际空间。 实现此操作是只需设置 DataList 的`RepeatColumns`属性设置为要显示每行的列数。 此外，DataList 的`RepeatDirection`属性可以用于指示是否多列中，多行表的内容应将放置水平方向从左到右、 从上到下或垂直从上到下，从左到右。

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 John Suru。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
[下一页](nested-data-web-controls-cs.md)
