---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
title: "分页和排序报告数据 (VB) |Microsoft 文档"
author: rick-anderson
description: "分页和排序是两个非常常见功能，当在联机应用程序中显示数据。 在本教程中我们将首先查看一下添加排序和..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: b895e37e-0e69-45cc-a7e4-17ddd2e1b38d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 23dbd63110092b2e91b7f3f9f6b602ef917c5527
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="paging-and-sorting-report-data-vb"></a>分页和排序报表数据 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_VB.exe)或[下载 PDF](paging-and-sorting-report-data-vb/_static/datatutorial24vb1.pdf)

> 分页和排序是两个非常常见功能，当在联机应用程序中显示数据。 在本教程中我们将花首先查看一下添加排序和分页至我们的报表，我们将然后相互依赖在将来的教程。


## <a name="introduction"></a>介绍

分页和排序是两个非常常见功能，当在联机应用程序中显示数据。 例如，当搜索时在联机书店 ASP.NET 丛书，可能有数百个此类丛书，但此报表列出搜索结果将列出每页的仅十个匹配项。 此外，可以按标题、 价格、 页计数、 作者姓名等排序结果。 而的过去 23 教程已学习了如何生成各种报表，包括接口，它允许添加、 编辑和删除数据，我们看不到如何进行排序数据和唯一的已分页示例我们看到已被使用说明和 FormView控件。

在本教程中我们将了解如何添加排序和分页至我们的报表，这可以通过只需检查几个复选框完成。 遗憾的是，这个简单的实施有排序接口离开一个位需要改进其缺点和分页例程不用于有效地进行分页大型结果集。 将来的教程将探索如何克服的-现成分页和排序解决方案的限制。

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>步骤 1： 添加分页和排序教程网页

在开始本教程之前，让 s 先花点时间添加我们需要以及本教程的下一步的三个 ASP.NET 页。 首先创建一个新文件夹中名为的项目`PagingAndSorting`。 接下来，将以下五个 ASP.NET 页添加到此文件夹中，让所有的它们配置为使用母版页`Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![创建 PagingAndSorting 文件夹并添加教程 ASP.NET 页](paging-and-sorting-report-data-vb/_static/image1.png)

**图 1**： 创建 PagingAndSorting 文件夹并添加教程 ASP.NET 页


接下来，打开`Default.aspx`页上，并将其拖`SectionLevelTutorialListing.ascx`从用户控件`UserControls`到设计图面上的文件夹。 用户控件中，我们在中创建的[母版页和网站的导航](../introduction/master-pages-and-site-navigation-vb.md)教程中，枚举站点图，并在项目符号列表中的当前节中显示这些教程。


![SectionLevelTutorialListing.ascx 用户控件添加到 Default.aspx](paging-and-sorting-report-data-vb/_static/image2.png)

**图 2**: SectionLevelTutorialListing.ascx 用户控件添加到 Default.aspx


为了具有显示分页和排序的教程中我们将创建的项目符号列表，我们需要将它们添加到站点映射。 打开`Web.sitemap`文件，并在编辑、 插入、 和删除站点映射节点标记后添加以下标记：


[!code-xml[Main](paging-and-sorting-report-data-vb/samples/sample1.xml)]


![更新包括新的 ASP.NET 页的站点图](paging-and-sorting-report-data-vb/_static/image3.png)

**图 3**： 更新包括新的 ASP.NET 页的站点图


## <a name="step-2-displaying-product-information-in-a-gridview"></a>步骤 2： 在一个 GridView 中显示产品信息

我们实际上实现分页和排序功能之前，让我们来首先创建标准非-srotable，列出产品信息的不可分页 GridView。 这是一项任务我们已多次在此之前完成本教程系列中因此这些步骤应该会熟悉。 首先打开`SimplePagingSorting.aspx`页上，并将一个 GridView 控件从工具箱中拖动到设计器中，设置其`ID`属性`Products`。 接下来，创建新对象数据源使用的 ProductsBLL 类的`GetProducts()`方法以返回所有产品信息。


![检索有关所有使用 GetProducts() 方法的产品信息](paging-and-sorting-report-data-vb/_static/image4.png)

**图 4**： 检索有关所有使用 GetProducts() 方法的产品信息


由于此报表不是只读的报表、 在该处 s 需要映射 ObjectDataSource s `Insert()`， `Update()`，或`Delete()`到对应的方法`ProductsBLL`方法; 因此，请 （无） 从下拉列表中选择更新，INSERT、和删除选项卡。


![选择 （无） 选项的下拉列表中插入、 更新和删除选项卡](paging-and-sorting-report-data-vb/_static/image5.png)

**图 5**： 选择 （无） 选项的下拉列表中插入、 更新和删除选项卡


接下来，让 s 自定义的 GridView 的字段，以便显示仅产品名称、 供应商、 类别、 价格和停用的状态。 此外，随意使任何字段级别格式设置发生更改，例如调整`HeaderText`属性或格式设置为货币的价格。 这些更改后 GridView s 声明性标记应看起来类似以下：


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample2.aspx)]

通过浏览器查看时为止图 6 显示我们的进度。 请注意页列出所有在一个屏幕中，显示每个产品的名称、 类别、 供应商、 价格、 产品和停用状态。


[![每个产品列出](paging-and-sorting-report-data-vb/_static/image7.png)](paging-and-sorting-report-data-vb/_static/image6.png)

**图 6**： 每个产品列出 ([单击以查看实际尺寸的图像](paging-and-sorting-report-data-vb/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>步骤 3： 添加分页支持

列出*所有*在一个屏幕上的产品可能会导致用户浏览数据的信息重载。 为了帮助使结果更易于管理，我们可以中断将数据分成较小的数据页，并允许用户一次单步执行一页数据。 若要完成这只需选中从 GridView s 智能标记启用分页复选框 (这将设置 GridView s [ `AllowPaging`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx)到`true`)。


[![选中启用分页复选框可添加分页支持](paging-and-sorting-report-data-vb/_static/image10.png)](paging-and-sorting-report-data-vb/_static/image9.png)

**图 7**： 检查启用分页所对应复选框添加分页支持 ([单击以查看实际尺寸的图像](paging-and-sorting-report-data-vb/_static/image11.png))


启用分页限制每页显示的记录数，并且将添加*分页接口*到 GridView。 图 7 所示的默认分页界面是页码，允许用户从一页数据快速导航到另一系列。 此分页接口应看起来很熟悉，正如我们已在过去教程说明如何和 FormView 控件添加分页支持时看到它。

说明和 FormView 控件只显示每页的单个记录。 GridView，但是，向其[`PageSize`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx)以确定每页显示的记录数量 （此属性默认值为 10）。

可以使用以下属性自定义此 GridView、 说明如何和 FormView 的分页接口：

- `PagerStyle`指示的分页接口; 的样式信息可以指定设置，例如`BackColor`， `ForeColor`， `CssClass`， `HorizontalAlign`，依次类推。
- `PagerSettings`包含可以自定义分页接口中; 的功能的属性 bevy`PageButtonCount`指示数值页码 （默认值为 10） 的分页界面中显示的最大数目; [ `Mode`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx)指示如何分页界面运行，并可以将设置为： 

    - `NextPrevious`显示允许用户在单步向前或向后一页时间下一步和上一步按钮
    - `NextPreviousFirstLast`下一步和上一步按钮，除了第一个和最后一个按钮，还提供，允许用户以快速移动到第一个或最后一个数据页
    - `Numeric`显示页码，使用户可立即跳转到任何页的系列
    - `NumericFirstLast`除了页码，包括第一个和最后一个按钮，从而允许用户以快速移动到第一个或最后一页的数据;第一个/最后一个按钮才会显示是否所有数值的页码无法容纳

此外，GridView、 说明如何和所有提供的 FormView`PageIndex`和`PageCount`属性，分别指示当前正在查看的页和数据，页的总数。 `PageIndex`属性编制了索引从 0，表示该参数时查看数据的第一页开始`PageIndex`将等于 0。 `PageCount`另一方面，启动计数 1，这意味着，`PageIndex`限制为的值介于 0 和`PageCount - 1`。

允许花一些时间来提高我们 GridView 的分页接口的默认外观的 s。 具体来说，让 s 具有分页接口与浅灰色背景右对齐。 而不是设置这些属性直接通过 GridView s`PagerStyle`属性，可让 s 创建中的 CSS 类`Styles.css`名为`PagerRowStyle`然后分配`PagerStyle`s`CssClass`通过我们的主题的属性。 首先打开`Styles.css`并添加以下 CSS 类定义：


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample3.css)]

接下来，打开`GridView.skin`文件中`DataWebControls`文件夹内的`App_Themes`文件夹。 如我们所述*母版页和网站的导航*教程，外观文件可以用于指定的 Web 控件的默认属性值。 因此，增加现有的设置，以包含设置`PagerStyle`s`CssClass`属性`PagerRowStyle`。 此外，让 s 配置要显示最多五个数字寻呼按钮使用的分页界面`NumericFirstLast`分页接口。


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>分页用户体验

图 8 显示网页时在选中的 GridView s 启用分页复选框后，通过浏览器访问和`PagerStyle`和`PagerSettings`配置已通过`GridView.skin`文件。 显示注意仅十个记录，并且分页接口指示我们正在查看数据的第一页。


[![与分页启用，仅记录的子集显示一次](paging-and-sorting-report-data-vb/_static/image13.png)](paging-and-sorting-report-data-vb/_static/image12.png)

**图 8**： 与分页已启用，仅记录的子集显示一次 ([单击以查看实际尺寸的图像](paging-and-sorting-report-data-vb/_static/image14.png))


当用户单击分页接口中的页码之一上时，回发时，才会和页重新加载请求的页面的记录的显示。 选择要查看的数据的最后一页后，图 9 显示的结果。 请注意最后一页上，仅有一条记录;这是因为，从而导致与唯一记录每个页面以及一页的 10 条记录的八个页总共有 81 记录。


[![单击页号导致回发，并演示记录的相应的子集](paging-and-sorting-report-data-vb/_static/image16.png)](paging-and-sorting-report-data-vb/_static/image15.png)

**图 9**： 对页号单击导致回发并显示相应记录子集 ([单击以查看实际尺寸的图像](paging-and-sorting-report-data-vb/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>分页的服务器端工作流

当最终用户单击分页接口中的按钮上时，回发时，才会和以下服务器端工作流一开始：

1. GridView s （或说明或 FormView）`PageIndexChanging`事件激发
2. ObjectDataSource 重新请求*所有*BLL; 中的数据的 GridView s`PageIndex`和`PageSize`属性值用于确定记录从 BLL 返回需要显示在 GridView
3. GridView 的`PageIndexChanged`事件激发

在步骤 2 中，ObjectDataSource 重新请求所有来自其数据源的数据。 这种样式的分页通常称为*默认分页*，因为它分页行为，使用默认设置时`AllowPaging`属性`true`。 使用默认 naively 分页 Web 控件的数据检索的数据，每一页的所有记录，即使仅记录的子集实际呈现到 HTML 发送到浏览器 s。 数据库数据由 BLL 或 ObjectDataSource 缓存，则默认分页是不适用于为足够大的结果集或包含许多并发用户的 web 应用程序。

在下一教程中，我们将查看如何实现*自定义分页*。 使用自定义分页专门可以指示 ObjectDataSource 以仅检索精确的所需的请求的数据页的记录集。 如你所想象的自定义分页大幅提高大型结果集进行分页的效率。

> [!NOTE]
> 默认分页，尽管不是适合时访问足够大的结果集或站点分页与许多并发用户发现自定义分页需要更多的更改和工作量才能实施，且不是像 （就是默认选中复选框一样简单分页）。 因此，默认分页可能会小、 低流量网站或相对较小的结果进行分页的设置时，作为它的理想选择 s 更轻松、 更加快速地实现。


例如，如果我们知道我们将在我们的数据库中永远不会有 100 个以上的产品，通过实现它所需的工作量可能偏移享受自定义分页的最小的性能提升。 如果，但是，我们可能一天有数千或成千上万的产品，*不*实现自定义分页将极大地影响我们的应用程序的可伸缩性。

## <a name="step-4-customizing-the-paging-experience"></a>步骤 4： 自定义分页体验

数据 Web 控件提供多个可用于增强用户的分页体验的属性。 `PageCount`属性，例如，指示有多少总页数，虽然`PageIndex`属性指示要访问当前页，可以对其进行设置，以快速移动到特定页的用户。 说明如何使用这些属性来改进了用户的分页体验，让我们来添加标签 Web 控制转移到我们向用户通知页面的页面它们重新当前访问，以及使他们能够快速跳转至任何给定页的 DropDownList 控件.

首先，将标签 Web 控件添加到你的页面，设置其`ID`属性`PagingInformation`，并将清空其`Text`属性。 接下来，创建的事件处理程序 GridView 的`DataBound`事件并添加以下代码：


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample5.vb)]

此事件处理程序分配`PagingInformation`标签 s`Text`条消息，告知用户页当前访问属性`Products.PageIndex + 1`外多少总页数`Products.PageCount`(我们将添加到 1`Products.PageIndex`属性因为`PageIndex`编制了索引从 0 开始)。 分配选择此标签 s`Text`中的属性`DataBound`事件处理程序，而不是`PageIndexChanged`事件处理程序因为`DataBound`事件将激发每次数据绑定到 GridView，而对于`PageIndexChanged`事件处理程序更改页索引时激发。 当 GridView 最初被数据绑定的第一页访问`PageIndexChanging`事件不 t 激发 (而`DataBound`事件)。

添加此元素后，用户现在显示一条消息指出他们访问的页面，并且存在数据的多少总页数。


[![显示的当前页码和总页数](paging-and-sorting-report-data-vb/_static/image19.png)](paging-and-sorting-report-data-vb/_static/image18.png)

**图 10**： 显示当前页码和总页数 ([单击以查看实际尺寸的图像](paging-and-sorting-report-data-vb/_static/image20.png))


除了标签控件，让我们来还添加 DropDownList 控件，其中列出与当前正在查看的页面选择了 GridView 中的页码。 其目的是，用户可以快速跳转从当前页到另一个只需从下拉列表中选择新的页索引。 通过将 DropDownList 添加到设计器中，设置启动其`ID`属性`PageList`并检查其智能标记的启用有些选项。

接下来，返回到`DataBound`事件处理程序并添加以下代码：


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample6.vb)]

此代码先清除中的项`PageList`DropDownList。 这看起来似乎多余的因为一个对 t 预期的页数，若要更改，但其他用户可能同时使用系统、 添加或删除记录从`Products`表。 此类插入或删除无法更改的数据页数。

接下来，我们需要再次创建页码，并且有一个映射到当前的 GridView`PageIndex`默认选择。 我们实现此目的从 0 到存在循环的`PageCount - 1`，添加一个新`ListItem`在每个迭代和设置其`Selected`属性设置为 true，如果当前迭代索引等于 GridView 的`PageIndex`属性。

最后，我们需要创建的事件处理程序的 DropDownList s`SelectedIndexChanged`触发的每当用户选择不同的项从列表中的事件。 若要创建此事件处理程序，只需双击设计器中，在下拉列表，然后添加以下代码：


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample7.vb)]

如图 11 所示，只更改 GridView 的`PageIndex`属性将导致数据重新绑定到 GridView。 在 GridView`DataBound`事件处理程序，相应的 DropDownList`ListItem`选择。


[![用户将自动转到第六个页时选择页 6 下拉列表项](paging-and-sorting-report-data-vb/_static/image22.png)](paging-and-sorting-report-data-vb/_static/image21.png)

**图 11**： 的用户将自动转到第六个页时选择页 6 下拉列表项 ([单击以查看实际尺寸的图像](paging-and-sorting-report-data-vb/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>步骤 5： 添加双向语言排序支持

添加双向语言排序支持非常简单，只添加分页支持只需选中 GridView s 智能标记的启用排序选项 (这会设置 GridView s [ `AllowSorting`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx)到`true`)。 此呈现每个标头的 GridView 的字段作为 LinkButtons，单击时，导致回发，并返回按所单击的列以升序排序的数据。 再次单击同一标题 LinkButton 重新对数据进行排序以降序顺序。

> [!NOTE]
> 如果你使用自定义的数据访问层，而不是类型化数据集，你可能没有启用排序选项 GridView s 智能标记中。 仅 GridViews 绑定到数据源以本机方式支持排序具有可用此复选框。 类型化数据集提供现成可用排序支持由于 ADO.NET DataTable 提供`Sort`方法，调用时，对 DataTable s 使用指定的条件的数据行进行排序。


如果你 DAL 不返回由 DAL 的本机支持排序将需要配置对象数据源将排序的信息传递给业务逻辑层，以对数据进行排序或具有的数据排序的对象。 我们将探讨如何进行排序在将来的教程中的数据在业务逻辑和数据访问层。

排序 LinkButtons 呈现为 HTML 超链接，其当前的颜色 （蓝色未访问的链接并访问过的链接深红色表示） 与标头行的背景色冲突。 相反，让 s 必须中显示为空白，而不管是否的所有标头行链接它们已被访问或不。 这可以通过添加以下实现`Styles.css`类：


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample8.css)]

此语法指示要显示这些元素使用的 HeaderStyle 类中的超链接时使用白色文本。

此 CSS 添加后, 访问通过浏览器的页面屏幕看起来应类似于图 12。 具体而言，图 12 显示的结果后单击价格字段 s 标头链接。


[![具有已按升序排序单价排序结果](paging-and-sorting-report-data-vb/_static/image25.png)](paging-and-sorting-report-data-vb/_static/image24.png)

**图 12**: 结果排过序升序顺序 UnitPrice ([单击以查看实际尺寸的图像](paging-and-sorting-report-data-vb/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>检查排序工作流

所有 GridView 都字段 BoundField、 CheckBoxField、 TemplateField，和等等具有`SortExpression`属性，指示应使用对数据进行排序时单击该字段 s 排序标头链接的表达式。 GridView 还有`SortExpression`属性。 GridView 时单击 LinkButton 排序标头，指定该字段 s`SortExpression`值赋给其`SortExpression`属性。 接下来，并将数据从 ObjectDataSource 重新检索根据 GridView s 排序`SortExpression`属性。 下表详细描述调查时最终用户对数据进行 GridView 的步骤顺序：

1. GridView s [Sorting 事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx)激发
2. GridView s [ `SortExpression`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx)设置为`SortExpression`的字段 LinkButton 被单击其排序标头
3. ObjectDataSource 重新检索所有 BLL 中的数据，然后对使用 GridView s 的数据进行排序`SortExpression`
4. GridView 的`PageIndex`属性重置为 0，这意味着，排序用户时返回到 （假设已实现分页支持） 的数据的第一页
5. GridView s [ `Sorted`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx)激发

像默认分页，请使用默认排序选项重新检索*所有*从 BLL 的记录。 使用不分页的情况下排序时，或者使用使用进行排序时默认分页，那里 s 没有办法避开此性能下降 （缺少缓存的数据库数据）。 但是，正如在将来的教程，我们看到它可以有效地对数据进行排序时使用自定义分页的 s。

当对象数据源绑定到 GridView 通过 GridView s 智能标记中的下拉列表时，每个 GridView 字段会自动拥有其`SortExpression`属性分配给中的数据字段的名称`ProductsRow`类。 例如， `ProductName` BoundField s`SortExpression`设置为`ProductName`，以下声明标记中所示：


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample9.aspx)]

可以配置一个字段，以便它可通过清除不排序 s 其`SortExpression`（将其分配为空字符串） 的属性。 为了说明这一点，我们假设，我们没有 t 想要让我们按价格排序我们的产品的客户。 `UnitPrice` BoundField 的`SortExpression`通过声明性的标记或通过字段对话框 （这是通过单击 GridView s 智能标记中的编辑列链接的访问） 可以删除此属性。


![具有已按升序排序单价排序结果](paging-and-sorting-report-data-vb/_static/image27.png)

**图 13**： 结果已按升序排序单价进行了排序


一次`SortExpression`属性已被撤除`UnitPrice`BoundField 页眉呈现为文本而不是一个链接，从而阻止用户对数据进行排序的价格。


[![通过删除 SortExpression 属性，用户可以不再对价格按产品](paging-and-sorting-report-data-vb/_static/image29.png)](paging-and-sorting-report-data-vb/_static/image28.png)

**图 14**： 通过删除 SortExpression 属性，用户可以不再对产品的价格 ([单击以查看实际尺寸的图像](paging-and-sorting-report-data-vb/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>以编程方式排序 GridView

你也可以进行排序的 GridView 内容以编程方式使用 GridView s [ `Sort`方法](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx)。 只需传入`SortExpression`值作为排序依据以及[ `SortDirection` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending`或`Descending`)，并且 GridView 的数据将会重新排序。

假设，原因我们关闭排序`UnitPrice`是的因为我们只需担心我们的客户只需将购买仅价格最低的产品。 但是，我们想要鼓励他们购买的成本最高的产品，因此我们 d 诸如它们才能进行排序的价格，但只能从最高价到最少的产品。

若要完成这将 Button Web 控件添加到页中，设置其`ID`属性`SortPriceDescending`，并将其`Text`价格按排序的属性。 接下来，为 s 按钮创建一个事件处理程序`Click`通过双击该按钮控件设计器中的事件。 将以下代码添加到此事件处理程序：


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample10.vb)]

单击此按钮使用户返回到的第一页与按定价从成本最高到最便宜 （请参阅图 15） 产品。


[![单击按钮订单中成本最高的产品到最少](paging-and-sorting-report-data-vb/_static/image32.png)](paging-and-sorting-report-data-vb/_static/image31.png)

**图 15**： 单击按钮订单产品从成本最高到最少 ([单击以查看实际尺寸的图像](paging-and-sorting-report-data-vb/_static/image33.png))


## <a name="summary"></a>摘要

在本教程中我们已了解如何实现分页和排序功能的默认值，这两种都是简单地检查一个复选框的 ！ 当用户对进行排序或数据进行分页时，则展开类似的工作流：

1. 回发时，才会
2. Web 控件 s 的数据预先级别事件将触发 (`PageIndexChanging`或`Sorting`)
3. 所有数据将重新检索的对象数据源
4. Web 控件 s 的数据后级别事件将触发 (`PageIndexChanged`或`Sorted`)

尽管实现基本的分页和排序是轻松，以利用更高效的自定义分页或来进一步增强分页或排序接口必须造成更多的工作。 将来的教程将探索这些主题。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一页](creating-a-customized-sorting-user-interface-cs.md)
[下一页](efficiently-paging-through-large-amounts-of-data-vb.md)
