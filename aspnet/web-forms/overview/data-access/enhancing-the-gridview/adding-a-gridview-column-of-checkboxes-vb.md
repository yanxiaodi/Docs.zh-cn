---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
title: "添加 GridView 列的复选框 (VB) |Microsoft 文档"
author: rick-anderson
description: "本教程讲述如何向 GridView 控件，以便用户提供选择 G.的多个行的一种直观方法中添加列的复选框..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 39253d05-75c0-41c7-b9d4-a6c58ecf69ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: 4468f7e0c142fa432e58d4c686dd79d3b38612ad
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-gridview-column-of-checkboxes-vb"></a>添加 GridView 列的复选框 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_VB.exe)或[下载 PDF](adding-a-gridview-column-of-checkboxes-vb/_static/datatutorial52vb1.pdf)

> 本教程讲述如何向 GridView 控件，以便用户提供选择多个行的 GridView 的一种直观方法中添加列的复选框。


## <a name="introduction"></a>介绍

在前面的教程中，我们将探讨如何将单选按钮的列添加到用于选择特定的记录 GridView。 单选按钮的列是合适的用户界面，当用户从网格中进行选择最多一个项受到限制。 有时，但是，我们可能想要允许用户选取任意数目的网格中的项。 基于 web 的电子邮件客户端，例如，通常显示的消息具有复选框的列的列表。 用户可以选择任意数目的消息，然后执行一些操作，如将电子邮件移动到另一个文件夹或删除它们。

在本教程中我们将了解如何添加复选框列以及如何确定哪些复选框已选中在回发时。 具体而言，我们将生成的示例，类似于基于 web 的电子邮件客户端用户界面。 我们的示例中将包括列出中的产品分页的 GridView`Products`数据库表具有一个复选框，在每个行 （请参见图 1）。 删除所选产品按钮，单击时，将删除所选的这些产品。


[![每个产品行包含一个复选框](adding-a-gridview-column-of-checkboxes-vb/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image1.png)

**图 1**： 每个产品行包含一个复选框 ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-checkboxes-vb/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>步骤 1： 添加一个分页的 GridView，列出产品信息

我们担心添加复选框列之前，让 s 第一个专注于列出一个 GridView，支持分页中的产品。 首先打开`CheckBoxField.aspx`页面`EnhancedGridView`文件夹，然后拖动一个 GridView 从工具箱中拖动到设计器中，设置其`ID`到`Products`。 接下来，选择要绑定到名为新对象数据源的 GridView `ProductsDataSource`。 配置对象数据源以使用`ProductsBLL`类，调用`GetProducts()`方法以返回数据。 由于此 GridView 将为只读的在更新中，INSERT、 设置的下拉列表，并删除选项卡添加到 （无）。


[![创建名为 ProductsDataSource 新对象数据源](adding-a-gridview-column-of-checkboxes-vb/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image3.png)

**图 2**： 创建新对象数据源命名`ProductsDataSource`([单击以查看实际尺寸的图像](adding-a-gridview-column-of-checkboxes-vb/_static/image4.png))


[![配置对象数据源检索数据使用 GetProducts() 方法](adding-a-gridview-column-of-checkboxes-vb/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image5.png)

**图 3**： 配置检索数据使用 ObjectDataSource`GetProducts()`方法 ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-checkboxes-vb/_static/image6.png))


[![在更新中，INSERT、 设置的下拉列表和删除选项卡到 （无）](adding-a-gridview-column-of-checkboxes-vb/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image7.png)

**图 4**： 设置的下拉列表中更新、 插入和删除选项卡为 （无） ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-checkboxes-vb/_static/image8.png))


完成配置数据源向导后，Visual Studio 将自动创建 BoundColumns 和 CheckBoxColumn 与产品相关的数据字段。 如我们未在前面的教程，请删除以外的所有`ProductName`， `CategoryName`，和`UnitPrice`BoundFields，并更改`HeaderText`到产品、 类别和价格的属性。 配置`UnitPrice`BoundField，以便其值设置为货币的格式。 此外配置 GridView 支持分页，通过检查从智能标记启用分页复选框。

允许 s 还添加删除所选的产品的用户界面。 添加按钮 Web 控件下 GridView，设置其`ID`到`DeleteSelectedProducts`及其`Text`删除所选产品的属性。 而不是真正从数据库中删除产品，此示例中我们将只显示一条消息将被删除的产品。 若要适应这种情况，请添加标签 Web 控件在按钮下方。 将其 ID 设置为`DeleteResults`，清除出其`Text`属性，并设置其`Visible`和`EnableViewState`属性设置为`False`。

进行这些更改后，应类似于以下的 GridView、 ObjectDataSource、 按钮和标签 s 声明性标记：


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample1.aspx)]

花些时间查看浏览器中的页 （请参见图 5）。 此时应看到名称、 类别和的前 10 个产品的价格。


[![列出名称、 类别和第十个产品价格](adding-a-gridview-column-of-checkboxes-vb/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image9.png)

**图 5**： 列出名称、 类别和第十个产品的价格 ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-checkboxes-vb/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>步骤 2： 添加列的复选框

由于 ASP.NET 2.0 包括 CheckBoxField，有人可能认为，它无法用于向一个 GridView 中添加列的复选框。 遗憾的是，程序不是这种情况，CheckBoxField 用于处理与布尔数据字段。 也就是说，为了使用 CheckBoxField 我们必须指定其值参考来确定是否呈现的复选框已选中的基础数据字段。 我们无法使用 CheckBoxField 只包含未选中的复选框的列。

相反，我们必须添加为 TemplateField 并添加复选框 Web 控件与其`ItemTemplate`。 请继续并添加到 TemplateField `Products` GridView 并使其第一个 （最左侧） 字段。 从 GridView s 智能标记中，单击编辑模板链接，然后将复选框 Web 控件拖到工具箱从`ItemTemplate`。 设置此复选框 s`ID`属性`ProductSelector`。


[![添加一个名为到 TemplateField 的 ItemTemplate ProductSelector 的复选框 Web 控件](adding-a-gridview-column-of-checkboxes-vb/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image11.png)

**图 6**： 添加复选框 Web 控件命名为`ProductSelector`到 TemplateField s `ItemTemplate` ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-checkboxes-vb/_static/image12.png))


与添加 TemplateField 和复选框 Web 控件，每个行现在包括一个复选框。 图 7 显示此页上，在添加的 TemplateField 和复选框后，通过浏览器，查看时。


[![每个产品行现在包含一个复选框](adding-a-gridview-column-of-checkboxes-vb/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image13.png)

**图 7**： 每个产品行现在包含一个复选框 ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-checkboxes-vb/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>步骤 3： 确定哪些复选框在回发时签入

此时我们具有的复选框，但无法确定哪些复选框已选中在回发时的列。 单击删除所选产品按钮后，不过，我们需要知道哪些复选框已选中状态，以便删除这些产品。

GridView s [ `Rows`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx)提供对 GridView 中的数据行的访问。 我们可以循环访问这些行，以编程方式访问复选框控件中，并请查阅其`Checked`属性来确定是否已选中的复选框。

创建的事件处理程序`DeleteSelectedProducts`按钮 Web 控件的`Click`事件并添加以下代码：


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample2.vb)]

`Rows`属性返回的集合`GridViewRow`实例该构成 GridView 的数据行。 `For Each`此处循环枚举此集合。 每个`GridViewRow`对象，以编程方式使用访问行 s 复选框`row.FindControl("controlID")`。 如果选中该复选框，则行 s 相应`ProductID`从检索值`DataKeys`集合。 在此练习中，我们只需显示一条信息性消息中的`DeleteResults`标签，尽管工作应用程序中我们 d 改为使调用`ProductsBLL`类的`DeleteProduct(productID)`方法。

此事件处理程序添加后，单击删除所选产品按钮现在将显示`ProductID`的所选产品的 s。


[![单击删除所选产品按钮时列出所选产品 Productid](adding-a-gridview-column-of-checkboxes-vb/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image15.png)

**图 8**： 时删除所选产品单击按钮选择产品`ProductID`列出 s ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-checkboxes-vb/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>步骤 4： 添加选中所有并取消选中所有按钮

如果用户想要删除当前页面上的所有产品，它们必须检查每十个复选框。 我们可以帮助加快此过程通过添加检查所有按钮，单击时，在网格中选择所有复选框。 取消选中所有按钮将都会同样有用。

将两个按钮 Web 控件添加到页上，将它们放置上方 GridView。 设置第一个 s`ID`到`CheckAll`及其`Text`检查所有属性; 设置第二个 s`ID`到`UncheckAll`并将其`Text`取消选中所有属性。


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample3.aspx)]

接下来，创建名为的代码隐藏类中的一种方法`ToggleCheckState(checkState)`，调用时，枚举`Products`GridView s`Rows`集合和设置每个复选框 s`Checked`属性与传递的值中*复选*参数。


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample4.vb)]

接下来，创建`Click`事件处理程序`CheckAll`和`UncheckAll`按钮。 在`CheckAll`s 事件处理程序，只需调用`ToggleCheckState(True)`; 在`UncheckAll`，调用`ToggleCheckState(False)`。


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample5.vb)]

使用此代码中，单击全部检查按钮导致回发，并且检查所有 GridView 中的复选框。 同样，单击取消选中所有，就会取消选择所有复选框。 在选中全部检查按钮后，图 9 显示的屏幕。


[![单击全部按钮的检查选择所有复选框](adding-a-gridview-column-of-checkboxes-vb/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image17.png)

**图 9**： 单击检查所有按钮选择所有复选框 ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-checkboxes-vb/_static/image18.png))


> [!NOTE]
> 当显示的列的复选框，选择或取消选择所有复选框的一种方法是通过一个标题行中的复选框。 此外，当前选中所有/取消选中所有实现都需要回发。 复选框可以选中或取消选中后，但是，完全通过客户端脚本，从而提供 snappier 的用户体验。 若要了解详细信息，以及使用客户端技术的讨论中使用的所有检查和取消选中所有的标头行复选框签出[检查 GridView 使用客户端脚本和选中所有复选框中的所有复选框](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx)。


## <a name="summary"></a>摘要

在需要使用户可以从在继续之前 GridView 选择任意数目的行的情况下，添加复选框列是一个选项。 正如我们在本教程中看到的包括在 GridView 的复选框的列时，需要添加与复选框 Web 控件 TemplateField。 通过使用 Web 控件 （而不是像前面的教程，请将标记注入直接到该模板后，） ASP.NET 自动会记住复选框已和未签跨回发。 我们可以以编程方式访问中的复选框代码来确定是否检查一个给定的复选框，或更改的选中的状态。

本教程和最后一个认为将行选择器列添加到 GridView。 在我们的下一步教程中，我们将查看如何操作，请通过执行少量工作，我们可以插入将功能添加到 GridView。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一页](adding-a-gridview-column-of-radio-buttons-vb.md)
[下一页](inserting-a-new-record-from-the-gridview-s-footer-vb.md)
