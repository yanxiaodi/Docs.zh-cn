---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
title: "使用 FormView 的模板 (VB) |Microsoft 文档"
author: rick-anderson
description: "说明如何，与 FormView 不组成字段。 相反，使用模板呈现 FormView。 在本教程中，我们将检查使用 F...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 67b25f4c-2823-42b6-b07d-1d650b3fd711
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
msc.type: authoredcontent
ms.openlocfilehash: 05e97ce5efeaf72192ed294b946e2249c60007d1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="using-the-formviews-templates-vb"></a>使用 FormView 的模板 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_14_VB.exe)或[下载 PDF](using-the-formview-s-templates-vb/_static/datatutorial14vb1.pdf)

> 说明如何，与 FormView 不组成字段。 相反，使用模板呈现 FormView。 在本教程中我们将检查使用 FormView 控件呈现数据的不太严格显示。


## <a name="introduction"></a>介绍

最后两个教程中我们已了解如何自定义使用 TemplateFields 的 GridView 和说明控件的输出。 TemplateFields 允许高度自定义的特定字段的内容，但最后的 GridView 和说明具有而是 boxy、 类似网格的外观。 很多情况下，此类的类似网格布局是理想的但有时需要更流畅、 不太严格的显示。 在显示一条记录，请使用 FormView 控件可能会出现此类的流畅的布局。

说明如何，与 FormView 不组成字段。 无法将 BoundField 或 TemplateField 添加到 FormView。 相反，使用模板呈现 FormView。 FormView 看作包含单个 TemplateField 说明如何控制。 FormView 支持以下模板：

- `ItemTemplate`用于呈现特定的记录显示在 FormView
- `HeaderTemplate`用于指定一个可选标头行
- `FooterTemplate`用于指定一个可选的页脚行
- `EmptyDataTemplate`当 FormView`DataSource`缺少任何记录，`EmptyDataTemplate`代替使用`ItemTemplate`来呈现控件的标记
- `PagerTemplate`可用于为已启用的分页的 FormViews 自定义分页接口
- `EditItemTemplate` / `InsertItemTemplate`用于为支持此类功能的 FormViews 自定义的编辑界面或插入接口

在本教程中我们将检查使用 FormView 控件呈现以不太严格显示的产品。 而不是使字段的名称、 类别、 供应商和等等，FormView 的`ItemTemplate`将显示使用的标头元素组合这些值和`<table>`（请参见图 1）。


[![FormView 中断的说明中所示类似网格布局](using-the-formview-s-templates-vb/_static/image2.png)](using-the-formview-s-templates-vb/_static/image1.png)

**图 1**: FormView 跳出 Grid-Like 布局看到说明如何在 ([单击以查看实际尺寸的图像](using-the-formview-s-templates-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>步骤 1： 将数据绑定到 FormView

打开`FormView.aspx`页上，并将 FormView 拖到设计器工具箱。 第一次添加 FormView 时它都显示为灰色的框中，指示我们的`ItemTemplate`需要。


[![要在设计器中呈现 FormView，直到提供 ItemTemplate](using-the-formview-s-templates-vb/_static/image5.png)](using-the-formview-s-templates-vb/_static/image4.png)

**图 2**: FormView 无法在设计器直到呈现`ItemTemplate`提供 ([单击以查看实际尺寸的图像](using-the-formview-s-templates-vb/_static/image6.png))


`ItemTemplate`可以 （通过声明性语法） 手动创建，也可以自动创建通过将 FormView 绑定到数据源控件通过设计器。 此自动创建`ItemTemplate`包含，列表的名称的每个字段和标签控件的 HTML`Text`属性绑定到字段的值。 此方法还自动-创建`InsertItemTemplate`和`EditItemTemplate`，这两种使用输入控件填充每个返回的数据源控件的数据字段。

如果你想要自动创建模板，从 FormView 的智能标记添加时，将调用新 ObjectDataSource 控件`ProductsBLL`类的`GetProducts()`方法。 这将创建与 FormView `ItemTemplate`， `InsertItemTemplate`，和`EditItemTemplate`。 从源视图中，删除`InsertItemTemplate`和`EditItemTemplate`编辑或尚未插入，因为我们不感兴趣创建 FormView 支持。 下一步，清理中的标记`ItemTemplate`，因此，我们必须干净状态进行工作。

如果中而不是所生成的生成`ItemTemplate`手动，你可以在其中添加和配置通过从工具箱中拖动到设计器拖动的对象数据源。 但是，不设置 FormView 的数据源从设计器。 相反，请转到源视图，并手动设置 FormView`DataSourceID`属性`ID`ObjectDataSource 的值。 接下来，手动添加`ItemTemplate`。

无论使用哪种方法，您决定要执行，此时你 FormView 声明性标记应看到如下：


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample1.aspx)]

花一些时间来检查 FormView 的智能标记; 中的启用分页复选框这将添加`AllowPaging="True"`属性设为 FormView 的声明性语法。 此外，还要设置`EnableViewState`属性设置为 False。

## <a name="step-2-defining-theitemtemplates-markup"></a>步骤 2： 定义`ItemTemplate`的标记

与 FormView 绑定到 ObjectDataSource 控件以及配置以支持分页，我们已准备好指定的内容`ItemTemplate`。 对于本教程，让我们具有该产品的名称显示在`<h3>`标题。 接下来，让我们使用 HTML`<table>`在四个列表，其中第一个和第三个列则列出的属性名称，第二个和第四个列表及其值中显示剩余的产品属性。

可以通过在设计器中的 FormView 的模板编辑界面中输入或通过声明性语法手动输入此标记。 使用模板时通常找到它直接使用声明性语法，但可随意使用你最熟悉的无论技术更快。

以下标记显示 FormView 声明性标记后的`ItemTemplate`的结构已完成：


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample2.aspx)]

请注意，数据绑定语法中- `<%# Eval("ProductName") %>`，对于示例可以直接注入模板的输出。 也就是说，它需要将分配给的标签控件`Text`属性。 例如，我们具有`ProductName`中显示值`<h3>`元素使用`<h3><%# Eval("ProductName") %></h3>`，这对于产品牛奶将呈现为`<h3>Chai</h3>`。

`ProductPropertyLabel`和`ProductPropertyValue`CSS 类用于指定样式的产品属性名称和中的值`<table>`。 在中定义这些 CSS 类`Styles.css`和会导致属性名称是加粗和右对齐，并添加到属性值填充的权限。

因为不不存在任何 CheckBoxFields 附带 FormView，为了显示`Discontinued`值作为一个复选框中，我们必须添加自己的复选框控件。 `Enabled`属性设置为 False，使其成为只读的并复选框的`Checked`属性绑定到的值`Discontinued`数据字段。

与`ItemTemplate`完成后，产品信息将显示在更流畅的方式。 说明如何将输出从最后一个教程 (图 3) 进行比较与 FormView 在本教程 (图 4) 中生成的输出。


[![刚性说明如何输出](using-the-formview-s-templates-vb/_static/image8.png)](using-the-formview-s-templates-vb/_static/image7.png)

**图 3**: 刚性说明如何输出 ([单击以查看实际尺寸的图像](using-the-formview-s-templates-vb/_static/image9.png))


[![流畅的 FormView 输出](using-the-formview-s-templates-vb/_static/image11.png)](using-the-formview-s-templates-vb/_static/image10.png)

**图 4**: 流体 FormView 输出 ([单击以查看实际尺寸的图像](using-the-formview-s-templates-vb/_static/image12.png))


## <a name="summary"></a>摘要

虽然 GridView 和说明控件都有使用 TemplateFields 自定义其输出，同时仍以一种类似网格、 boxy 格式显示其数据。 当需要显示单个记录这些时间使用不太严格的布局，FormView 是理想的选择。 说明如何，如 FormView 呈现来自的单个记录其`DataSource`，但与说明如何将只需组成模板且不支持字段。

正如我们在本教程中看到的 FormView 时，可以更灵活的布局显示单个记录。 在将来我们将检查 DataList 和转发器控件，请提供相同级别的灵活性以作为 FormsView，但可以显示多个记录 （如 GridView) 的教程。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 E.R. Gilmore。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](using-templatefields-in-the-detailsview-control-vb.md)
[下一页](displaying-summary-information-in-the-gridview-s-footer-vb.md)
