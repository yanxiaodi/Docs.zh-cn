---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
title: "添加单选按钮 (C#) 的 GridView 列 |Microsoft 文档"
author: rick-anderson
description: "本教程讲述如何向用户提供更直观的方式选择的单个行的 GridView 控件添加单选按钮的列..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 32377145-ec25-4715-8370-a1c590a331d5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
msc.type: authoredcontent
ms.openlocfilehash: 386fcb1cef896edbb465ba36415712af70d916ec
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-gridview-column-of-radio-buttons-c"></a>添加 GridView 列的单选按钮 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_CS.exe)或[下载 PDF](adding-a-gridview-column-of-radio-buttons-cs/_static/datatutorial51cs1.pdf)

> 本教程讲述如何向用户提供更直观的方式选择的 GridView 单个行的 GridView 控件添加单选按钮的列。


## <a name="introduction"></a>介绍

GridView 控件提供了大量的内置功能。 它包括用于显示文本、 图像、 超链接和按钮的不同字段的数目。 它支持进一步的自定义的模板。 只需单击几下鼠标，它可能使其中每一行可以选择一个按钮，通过一个 GridView 或启用编辑或删除功能的 s。 尽管提供功能的过多，通常会在其中附加，不支持的功能将需要添加的情况。 在本教程下, 一步的两个，我们将查看如何增强以包括其他功能的 GridView 的功能。

本教程，并且对增强行选择过程的下一个重点关注。 在检查作为[母版/详细介绍使用详细信息 DetailView 可选择的主 GridView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)，我们可以将 CommandField 添加到包括选择按钮 GridView。 单击时，回发时，才会和 GridView 的`SelectedIndex`属性更新为其选择按钮被单击的行的索引。 在[母版/详细介绍使用详细信息 DetailView 可选择的主 GridView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)教程，我们已了解如何使用此功能以显示所选的 GridView 行的详细信息。

在选择按钮在许多情况下，它可能不适于以及其他。 而不是使用一个按钮，两个其他用户界面元素通常用于进行选择： 的单选按钮和复选框。 因此，而不是选择按钮，每一行都包含单选按钮或复选框，我们可以增加 GridView。 在其中用户可以仅选择其中一个 GridView 记录的情况下，单选按钮可能选择按钮上方首选。 在情况下可能选择用户可以如在基于 web 的电子邮件应用中，用户可能想要选择要删除的复选框的多个消息的其中的多个记录提供不是可从选择按钮或单选按钮的功能用户界面。

本教程讲述如何将单选按钮的列添加到 GridView。 继续本教程将探讨使用复选框。

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>步骤 1： 创建增强 GridView 网页

我们开始增强 GridView 可包含的单选按钮列之前，让我们来首先花一些时间，我们需要以及本教程后面的两个网站项目中创建 ASP.NET 页。 首先，通过添加一个名为的新文件夹`EnhancedGridView`。 接下来，添加到该文件夹，并确保将每个包含网页相关联的以下 ASP.NET 页面`Site.master`母版页：

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![SqlDataSource 相关教程添加的 ASP.NET 页面](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.gif)

**图 1**： 添加 ASP.NET 页 SqlDataSource 相关教程


在其他文件夹中，如`Default.aspx`中`EnhancedGridView`文件夹将在其部分中列出这些教程。 回想一下，`SectionLevelTutorialListing.ascx`用户控件提供此功能。 因此，此用户将控件添加到`Default.aspx`通过从解决方案资源管理器拖到页面上的设计视图拖动。


[![SectionLevelTutorialListing.ascx 用户控件添加到 Default.aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.png)

**图 2**： 添加`SectionLevelTutorialListing.ascx`用户控件`Default.aspx`([单击以查看实际尺寸的图像](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.png))


最后，将这些四个页面添加到的条目为`Web.sitemap`文件。 具体而言，在使用后添加以下标记 SqlDataSource 控件`<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample1.xml)]

在更新后`Web.sitemap`，花些时间查看通过浏览器网站的教程。 在左侧菜单现在包括用于编辑、 插入和删除教程的项。


![站点图现在包括项增强 GridView 教程](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.gif)

**图 3**： 站点图现在包括项增强 GridView 教程


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>步骤 2： 在一个 GridView 中显示供应商

本教程让 s 生成一个 GridView，其中列出每个提供一个单选按钮的 GridView 行从美国、 供应商。 选择通过单选按钮的供应商提供之后, 用户可以通过单击的按钮查看供应商的产品。 虽然此任务听起来可能很普通，有大量的细微的难度特别难的部分。 我们深入探讨这些细微部分之前，让我们来首先要获取列出供应商 GridView。

首先打开`RadioButtonField.aspx`页面`EnhancedGridView`通过从工具箱中拖动到设计器中拖动一个 GridView 的文件夹。 设置 GridView s`ID`到`Suppliers`并从其智能标记上，选择创建新的数据源。 具体而言，创建名为 ObjectDataSource`SuppliersDataSource`拉取从其数据`SuppliersBLL`对象。


[![创建名为 SuppliersDataSource 新对象数据源](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.png)

**图 4**： 创建新对象数据源命名`SuppliersDataSource`([单击以查看实际尺寸的图像](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.png))


[![配置对象数据源以使用 SuppliersBLL 类](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.png)

**图 5**： 配置使用 ObjectDataSource`SuppliersBLL`类 ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.png))


由于我们只想要列出这些供应商在美国，选择`GetSuppliersByCountry(country)`从下拉列表中选择的选项卡上的方法。


[![配置对象数据源以使用 SuppliersBLL 类](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.png)

**图 6**： 配置使用 ObjectDataSource`SuppliersBLL`类 ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.png))


从更新选项卡，选择 （无） 选项，然后单击下一步。


[![配置对象数据源以使用 SuppliersBLL 类](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.png)

**图 7**： 配置使用 ObjectDataSource`SuppliersBLL`类 ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.png))


由于`GetSuppliersByCountry(country)`方法接受参数，配置数据源向导将提示我们为该参数的源。 若要指定一个硬编码的值 （美国，在此示例中），将源下拉列表设置为 None，在文本框中输入的默认值的参数。 单击完成以完成向导。


[![用作美国国家/地区参数的默认值](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.png)

**图 8**： 作为的默认值的使用美国`country`参数 ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.png))


完成向导后，GridView 将为每个供应商数据字段包括 BoundField。 删除所有但`CompanyName`， `City`，和`Country`BoundFields，并重命名`CompanyName`BoundFields`HeaderText`到供应商的属性。 这么做，应该类似于以下的 GridView 和对象数据源的声明性语法。


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample2.aspx)]

对于本教程，让我们来允许用户查看所选供应商的产品供应商列表中，位于同一页上或另一页上。 为了适应这种情况，请向页面添加两个按钮 Web 控件。 我已集`ID`到这两个按钮的 s`ListProducts`和`SendToProducts`的想法，当`ListProducts`单击回发将发生，并且将在同一页上，而是何时列出所选供应商的产品`SendToProducts`单击后，用户将 whisked 到的另一个页面中列出的产品。

图 9 显示`Suppliers`GridView 和两个按钮 Web 控制通过浏览器查看时。


[![从美国这些供应商具有其名称、 城市和列出的国家/地区信息](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.png)

**图 9**： 那些供应商的美国具有与其名称、 市和列出国家/地区信息 ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>步骤 3： 添加单选按钮的列

此时`Suppliers`GridView 具有三个 BoundFields 美国显示公司名称、 城市和每个供应商的国家/地区。 它仍但是缺少单选按钮的列。 遗憾的是，GridView 不包括内置 RadioButtonField，否则为我们无法只需将它添加到网格和完成。 相反，我们可以添加为 TemplateField 和配置其`ItemTemplate`呈现单选按钮，从而导致每个 GridView 行的单选按钮。

最初，我们可能会假定是否可以通过添加单选按钮 Web 控件与实现所需的用户界面`ItemTemplate`TemplateField。 虽然这确实会将单个单选按钮添加到的 GridView 每一行，单选按钮不能进行组合，因此不互相排斥。 即，最终用户是能够从 GridView 同时选择多个单选按钮。

即使使用的单选按钮 Web 控件 TemplateField 不提供，我们需要的功能，让 s 实现此方法，因为它 s 值得检查生成的单选按钮未分组的原因。 首先，通过添加为 TemplateField 到供应商 GridView，使其最左边的字段。 接下来，从 GridView s 智能标记中，单击编辑模板链接并将单选按钮 Web 控件从工具箱拖到 TemplateField 的`ItemTemplate`（请参阅图 10）。 设置 RadioButton s`ID`属性`RowSelector`和`GroupName`属性`SuppliersGroup`。


[![将单选按钮 Web 控件添加到 ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.png)

**图 10**： 添加单选按钮 Web 控件与`ItemTemplate`([单击以查看实际尺寸的图像](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.png))


进行这些添加通过设计器之后, 你 GridView 的标记应如下类似如下：


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample3.aspx)]

单选按钮 s [ `GroupName`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx)用于组单选按钮的一系列内容。 具有相同的所有单选按钮控件`GroupName`值被视为分组; 只有一个单选按钮可以一次选择从组。 `GroupName`属性指定的值呈现的单选按钮的`name`属性。 浏览器检查的单选按钮`name`属性以确定单选按钮分组。

与 RadioButton Web 控件添加到`ItemTemplate`，访问此页面，通过浏览器，然后单击网格的行中的单选按钮。 请注意如何未分组的单选按钮，因而可能选择的所有行，作为图 11 显示。


[![GridView 的单选按钮是不分组在一起](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.png)

**图 11**: GridView 的单选按钮是不分组在一起 ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.png))


未分组的单选按钮的原因是因为其呈现`name`特性都相同，尽管具有相同`GroupName`属性设置。 若要查看这些差异，请查看/源执行从浏览器并检查单选按钮标记：


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample4.html)]

请注意这两种`name`和`id`属性不是在属性窗口中，指定精确值，但前面带有数目的其他`ID`值。 其他`ID`值添加到前面的呈现`id`和`name`属性是`ID`s 单选按钮父控件`GridViewRow`s `ID` s、 GridView 的`ID`，内容控件 s `ID`，和 Web 窗体的`ID`。 这些`ID`s 将添加，以便每个呈现在 GridView Web 控件具有一个唯一`id`和`name`值。

每个呈现控制需求不同`name`和`id`因为这是如何浏览器唯一标识客户端和如何它标识为 web 服务器的操作的每个控件或在回发时已发生更改。 例如，假设我们想要每次 RadioButton s 签状态发生了更改时运行某些服务器端代码。 我们无法完成此操作通过设置 RadioButton s`AutoPostBack`属性`true`和创建的事件处理程序`CheckChanged`事件。 但是，如果呈现`name`和`id`的所有单选按钮相同的在回发我们无法确定哪些特定值的单选按钮被单击。

它的短是我们无法在使用单选按钮 Web 控件 GridView 中创建的单选按钮的列。 相反，我们必须使用而是陈旧的技术以确保每个 GridView 行注入相应的标记。

> [!NOTE]
> 像 RadioButton Web 控件，单选按钮 HTML 控件时添加到模板，将包括唯一`name`属性，在未分组的网格中进行的单选按钮。 如果你不熟悉 HTML 控件，随意地忽略此说明，如 HTML 控件很少使用，特别是在 ASP.NET 2.0。 但如果你有兴趣了解详细信息，请参阅[k。 Scott Allen](http://odetocode.com/blogs/scott/default.aspx) s 博客文章[Web 控件和 HTML 控件](http://www.odetocode.com/Articles/348.aspx)。


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>使用文本控件将注入单选按钮标记

为了正确组所有 GridView 内单选按钮，我们需要手动将注入到的单选按钮标记`ItemTemplate`。 每个单选按钮需要相同`name`属性，但应具有唯一`id`属性 （如果我们想要访问通过客户端脚本的单选按钮）。 用户选择一个单选按钮和页回发后，浏览器将返回的值发送选定的单选按钮的`value`属性。 因此，每个单选按钮将需要一个唯一`value`属性。 最后，在回发时我们需要确保将添加`checked`属性与后选中了，否则用户所做选择和文章返回一个单选按钮，单选按钮将返回到其默认状态 （所有未选定）。

有两种方法可以执行以便将低级别的标记注入到模板。 之一是执行标记和为格式化方法的代码隐藏类中定义的调用的组合。 此方法首先中所述[GridView 控件中使用 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)教程。 在我们的示例中，它可能如下所示：


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample5.aspx)]

在这里，`GetUniqueRadioButton`和`GetRadioButtonValue`将返回相应的代码隐藏类中定义的方法`id`和`value`属性值为每个单选按钮。 此方法非常适用于分配`id`和`value`属性，但会短时无需指定`checked`属性值，因为数据绑定语法仅执行时数据首先绑定到 GridView。 因此，如果 GridView 具有启用视图状态，格式设置方法，将仅时，会激发首次加载页 （或当 GridView 显式重新绑定到数据源），因此该设置的函数`checked`上调用获胜 t 的属性回发。 它 s 而是细微问题和有点超出了本文中，因此，我将按此保留的范围。 我做、 但是，建议你尝试使用上面的方法和一同通过它在其中你将遇到困难的点。 尽管此类练习获胜不能获得您任何仔细的可行版本，它将帮助培养 GridView 和数据绑定生命周期的更深入地了解。

注入自定义其他方法，模板和我们将使用本教程中的方法中的低级标记是添加[文本控件](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx)到模板。 然后，在 GridView s`RowCreated`或`RowDataBound`事件处理程序，可以以编程方式访问文本控件并将其`Text`属性设置为标记发出。

通过从 TemplateField s 删除单选按钮来启动`ItemTemplate`，替换文本控件。 设置的文本控件，s`ID`到`RadioButtonMarkup`。


[![将文本控件添加到 ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.png)

**图 12**: 文本将控件添加到`ItemTemplate`([单击以查看实际尺寸的图像](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.png))


接下来，创建的事件处理程序 GridView 的`RowCreated`事件。 `RowCreated`后对于每个添加的行，无论数据正在重新绑定到 GridView，事件将激发。 这意味着，即使在回发时重新从视图状态加载数据，`RowCreated`仍激发事件，这是我们将使用它而不是原因`RowDataBound`（其中激发仅当数据显式绑定到 Web 控件的数据）。

在此事件处理程序中，我们只希望的情况下继续我们重新处理数据行。 我们想要以编程方式引用每个数据行`RadioButtonMarkup`文本控件并设置其`Text`到要发出的标记的属性。 如下面的代码所示，发出的标记创建单选按钮其`name`属性设置为`SuppliersGroup`、 其`id`属性设置为`RowSelectorX`，其中*X*是 GridView 行的索引并且其`value`属性设置为 GridView 行的索引。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample6.cs)]

当选择某个 GridView 行且回发发生时，我们感兴趣`SupplierID`的所选供应商。 因此，有人可能认为每个单选按钮的值应为实际`SupplierID`（而不是 GridView 行的索引）。 虽然这在某些情况下，它将会带来安全风险盲目地接受并处理`SupplierID`。 我们 GridView，例如，列出了仅在美国这些供应商。 但是，如果`SupplierID`直接从单选按钮的新增功能，若要停止有害用户从操作传递`SupplierID`发送开机回发值？ 通过使用作为的行索引`value`，，然后获取`SupplierID`在回发时从`DataKeys`集合中，我们可以确保用户仅使用其中一个`SupplierID`与 GridView 行之一相关联的值。

添加此事件处理程序代码后，花点时间查看浏览器中测试。 首先，请注意，只有一个单选按钮在网格中的可以一次选择。 但是，当选择一个单选按钮，然后单击一个按钮，产生的回发，并且所有的单选按钮恢复为其初始状态 （即，在回发时，选定的单选按钮不再处于选中状态）。 若要解决此问题，我们需要增加`RowCreated`事件处理程序，以便它检查发送的回发的所选的单选按钮索引添加`checked="checked"`属性设为发出的行索引匹配项的标记。

当发生回发时，浏览器发送回`name`和`value`的选定的单选按钮。 检索此值，可以以编程方式使用`Request.Form["name"]`。 [ `Request.Form`属性](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx)提供[ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx)表示窗体变量。 窗体变量的名称和 web 页中中的窗体字段的值也每当回发时，才会发送回的 web 浏览器。 因为呈现`name`GridView 中的单选按钮的属性`SuppliersGroup`，当浏览器将发送的回发 web 页面`SuppliersGroup=valueOfSelectedRadioButton`返回到 web 服务器 （以及其他窗体字段）。 然后可以从访问此信息`Request.Form`属性使用： `Request.Form["SuppliersGroup"]`。

因为我们将需要以确定选中的单选按钮索引不是仅在`RowCreated`事件处理程序，但在`Click`按钮 Web 控件的事件处理程序，让 s 添加`SuppliersSelectedIndex`属性返回的代码隐藏类`-1`如果选择没有单选按钮和所选的索引，如果一个单选按钮处于选中状态。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample7.cs)]

通过添加此属性，我们已知要添加`checked="checked"`中的标记`RowCreated`事件处理程序时`SuppliersSelectedIndex`等于`e.Row.RowIndex`。 更新事件处理程序，以包括此逻辑：


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample8.cs)]

进行此更改后，选中的单选按钮处于选定状态的回发之后。 现在，我们已经能够指定哪些单选按钮处于选中状态，我们无法更改行为，以便当首次访问页，选择第一个 GridView 行的单选按钮 （而不是具有默认情况下选择没有单选按钮，即当前行为）。 若要让第一个单选按钮选择默认情况下，只需更改`if (SuppliersSelectedIndex == e.Row.RowIndex)`与以下语句： `if (SuppliersSelectedIndex == e.Row.RowIndex || (!Page.IsPostBack && e.Row.RowIndex == 0))`。

此时我们已添加到允许单个 GridView 行选择和跨回发记住 GridView 的一列分组的单选按钮。 我们后续步骤是以显示所选供应商提供的产品。 在步骤 4 中我们将了解如何将用户重定向到另一页，沿所选发送`SupplierID`。 在步骤 5 中，我们将了解如何在同一页面上 GridView 中显示所选供应商的产品。

> [!NOTE]
> 而不是使用 TemplateField （此时间较长的步骤 3 的焦点），我们无法创建自定义`DataControlField`呈现合适的用户界面和功能的类。 [ `DataControlField`类](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx)是 BoundField、 CheckBoxField、 TemplateField 和其他内置的 GridView 和说明字段派生自的基类。 创建自定义`DataControlField`类将表示单选按钮的列无法仅使用声明性语法，添加，并且还会使复制其他网页和其他简单得多的 web 应用程序的功能。


如果你已创建自定义，编译在 ASP.NET 中的控件，但是，你知道，这样做需要数量相当大的先并携带的细微部分和必须进行仔细处理的边缘情况的主机。 因此，我们将放弃实现作为自定义的单选按钮的列`DataControlField`类现在并坚持使用 TemplateField 选项。 也许，我们将有机会浏览创建、 使用和部署自定义`DataControlField`将来的教程中的类 ！

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>步骤 4： 在单独的页面中显示所选供应商的产品

用户选定的 GridView 行后，我们需要显示所选供应商的产品。 在某些情况下，我们可能想要在单独的页上，在其他我们可能更愿意执行此操作在同一页面中显示这些产品。 让我们来首先检查如何在单独的页面; 中显示的产品步骤 5 中，我们将考察添加到 GridView`RadioButtonField.aspx`以显示所选供应商的产品。

目前有两个页面上的按钮 Web 控件`ListProducts`和`SendToProducts`。 当`SendToProducts`单击按钮时，我们想要发送到用户`~/Filtering/ProductsForSupplierDetails.aspx`。 此页中创建[主/详细信息筛选跨两个页](../masterdetail/master-detail-filtering-across-two-pages-cs.md)教程并显示供应商的产品其`SupplierID`通过名为的查询字符串字段传递`SupplierID`。

若要提供此功能，创建的事件处理程序`SendToProducts`按钮的`Click`事件。 在步骤 3 中，我们将添加`SuppliersSelectedIndex`选择属性，它将返回其单选按钮的行的索引。 相应`SupplierID`可检索来自 GridView s`DataKeys`集合和用户然后可以发送到`~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID`使用`Response.Redirect("url")`。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample9.cs)]

此代码非常有效，只要一个单选按钮处于选中状态从 GridView。 如果最初，GridView 不包含任何单选按钮选择，并且用户单击`SendToProducts`按钮，`SuppliersSelectedIndex`将`-1`，这将导致异常引发自`-1`超出的索引范围`DataKeys`集合。 这是一个问题，但是，如果您决定更新`RowCreated`事件处理程序以便在初始时会选中 GridView 中具有的第一个单选按钮的步骤 3 中所述。

若要容纳`SuppliersSelectedIndex`值`-1`，将标签 Web 控件添加到页面上方 GridView。 设置其`ID`属性`ChooseSupplierMsg`，将其`CssClass`属性`Warning`，将其`EnableViewState`和`Visible`属性设置为`false`，并将其`Text`请属性从网格中选择一个供应商。 CSS 类`Warning`以红色、 斜体、 粗体、 大字体显示文本，并在中定义`Styles.css`。 通过设置`EnableViewState`和`Visible`属性设置为`false`，标签不呈现除外，对于那些回发的位置控制 s`Visible`属性以编程方式设置为`true`。


[![添加标签 Web 控件上方 GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image21.png)

**图 13**： 添加标签 Web 控件上方 GridView ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-radio-buttons-cs/_static/image22.png))


接下来，增加`Click`事件处理程序来显示`ChooseSupplierMsg`标签如果`SuppliersSelectedIndex`是小于零，并将重定向到用户`~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID`否则为。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample10.cs)]

请访问中某个浏览器，然后单击页面`SendToProducts`之前从 GridView 选择供应商的按钮。 如图 14 所示，这将显示`ChooseSupplierMsg`标签。 接下来，选择一个供应商，然后单击`SendToProducts`按钮。 这将 whisk 你到页列出由所选供应商提供的产品。 图 15 所示`ProductsForSupplierDetails.aspx`页选择野人绑架 Breweries 供应商。


[![如果选择否供应商，显示 ChooseSupplierMsg 标签](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image23.png)

**图 14**:`ChooseSupplierMsg`才显示标签，如果选择否供应商 ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-radio-buttons-cs/_static/image24.png))


[![选择供应商的产品将显示在 ProductsForSupplierDetails.aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image25.png)

**图 15**: 选择供应商的产品显示在`ProductsForSupplierDetails.aspx`([单击以查看实际尺寸的图像](adding-a-gridview-column-of-radio-buttons-cs/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>步骤 5： 在同一页上显示所选供应商的产品

在步骤 4 中，我们已了解如何将用户发送到另一个 web 页面，以显示所选供应商的产品。 或者，可以在同一页上显示所选供应商的产品。 为了说明这一点，我们将添加到另一个 GridView`RadioButtonField.aspx`以显示所选供应商的产品。

由于我们只需要此 GridView 的产品以显示选定供应商后，将添加下方面板 Web 控件`Suppliers`GridView，设置其`ID`到`ProductsBySupplierPanel`及其`Visible`属性`false`。 在面板内将文本产品添加所选供应商后, 跟一个名为的 GridView `ProductsBySupplier`。 从 GridView s 智能标记中，选择要绑定到名为新 ObjectDataSource `ProductsBySupplierDataSource`。


[![将 ProductsBySupplier GridView 绑定到新对象数据源](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image27.png)

**图 16**： 绑定`ProductsBySupplier`到新对象数据源的 GridView ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-radio-buttons-cs/_static/image28.png))


接下来，配置对象数据源以使用`ProductsBLL`类。 由于我们只想要检索选定的供应商提供的这些产品，指定应调用 ObjectDataSource`GetProductsBySupplierID(supplierID)`方法来检索其数据。 从下拉列表中，在更新中，插入、 选择 （无） 并删除选项卡。


[![配置对象数据源以使用 GetProductsBySupplierID(supplierID) 方法](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image29.png)

**图 17**： 配置使用 ObjectDataSource`GetProductsBySupplierID(supplierID)`方法 ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-radio-buttons-cs/_static/image30.png))


[![在更新中，INSERT、 设置下拉列表中的，以便 （无） 和删除选项卡](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image31.png)

**图 18**： 设置中更新、 插入和删除选项卡的下拉列表将列出到 （无） ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-radio-buttons-cs/_static/image32.png))


配置选择之后, 更新、 插入和删除选项卡，请单击下一步。 由于`GetProductsBySupplierID(supplierID)`方法需要输入的参数，创建数据源向导将提示我们需要指定参数的值的源。

我们有几个此处在指定参数的值的源的选项。 我们无法使用默认参数对象，并以编程方式将分配的值`SuppliersSelectedIndex`属性参数 s`DefaultValue`中 ObjectDataSource 的属性`Selecting`事件处理程序。 将回指[以编程方式设置对象数据源的参数值](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)教程，以在以编程方式将值分配到 ObjectDataSource 的参数使用刷新程序。

或者，我们可以使用 ControlParameter，请参阅`Suppliers`GridView s [ `SelectedValue`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx)（请参阅图 19）。 GridView s`SelectedValue`属性返回`DataKey`值对应于[`SelectedIndex`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx)。 为了使此选项才能工作，我们需要以编程方式设置 GridView s`SelectedIndex`对所选的属性时行`ListProducts`单击按钮。 作为一项额外权益，通过设置`SelectedIndex`，将不执行所选的记录`SelectedRowStyle`中定义`DataWebControls`主题 （黄色背景）。


[![使用 ControlParameter GridView 的 SelectedValue 指定为参数源](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image33.png)

**图 19**： 使用 ControlParameter 指定为参数源的 GridView 的 SelectedValue ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-radio-buttons-cs/_static/image34.png))


完成向导之后，Visual Studio 将自动添加产品的数据字段的字段。 删除所有但`ProductName`， `CategoryName`，和`UnitPrice`BoundFields，并更改`HeaderText`到产品、 类别和价格的属性。 配置`UnitPrice`BoundField，以便其值设置为货币的格式。 进行这些更改后，面板、 GridView 和 ObjectDataSource s 声明性标记应如下所示：


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample11.aspx)]

若要完成本练习中，我们需要设置 GridView s`SelectedIndex`属性`SelectedSuppliersIndex`和`ProductsBySupplierPanel`面板 s`Visible`属性`true`时`ListProducts`单击按钮。 若要实现此目的，创建的事件处理程序`ListProducts`按钮 Web 控件的`Click`事件并添加以下代码：


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample12.cs)]

如果尚未选择 GridView，从供应商`ChooseSupplierMsg`显示标签和`ProductsBySupplierPanel`隐藏的面板。 否则为如果已选择了供应商，`ProductsBySupplierPanel`显示和 GridView 的`SelectedIndex`更新属性。

图 20 后已选择野人绑架 Breweries 供应商，并且单击页按钮上的显示产品后显示的结果。


[![由野人绑架 Breweries 产品提供的同一页面上列出](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image35.png)

**图 20**： 由野人绑架 Breweries 产品提供的同一页面上列出 ([单击以查看实际尺寸的图像](adding-a-gridview-column-of-radio-buttons-cs/_static/image36.png))


## <a name="summary"></a>摘要

中所述[母版/详细介绍使用详细信息 DetailView 可选择的主 GridView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)教程，则可以从使用 CommandField GridView 选择记录其`ShowSelectButton`属性设置为`true`。 但 CommandField 作为正则下压按钮、 链接或图像显示其按钮。 替代的行选择用户界面是提供的单选按钮或每个 GridView 行中的复选框。 在本教程中，我们将探讨如何添加单选按钮的列。

遗憾的，为简单或简单添加单选按钮并不是 t 的列，如您所料。 没有可以添加一个按钮的单击没有内置 RadioButtonField，使用单选按钮 Web 控件中为 TemplateField 引入了自己的问题集。 在结束时，若要提供此类接口我们也具有创建自定义`DataControlField`类或将适当的 HTML 注入转换期间为 TemplateField 采用`RowCreated`事件。

具有探讨了如何添加列的单选按钮，让我们着重于添加的列的复选框。 具有复选框的列，用户可以选择一个或多个 GridView 行，然后执行某项操作上的所有所选行 （如从基于 web 的电子邮件客户端，选择一组的电子邮件，然后选择要删除所有所选电子邮件）。 在下一步的教程，我们将了解如何添加这类列。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 David Suru。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[下一篇](adding-a-gridview-column-of-checkboxes-cs.md)
