---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
title: "自定义 DataList 的编辑界面 (VB) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将创建更丰富的编辑界面 DataList，一个包括 DropDownLists 和一个复选框。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 718628e2-224c-455f-b33a-a41efd48d5a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: bd31c18b9fced12feeda8ca8dea7dace0ef63573
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="customizing-the-datalists-editing-interface-vb"></a>自定义 DataList 的编辑界面 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_VB.exe)或[下载 PDF](customizing-the-datalist-s-editing-interface-vb/_static/datatutorial40vb1.pdf)

> 在本教程中我们将创建更丰富的编辑界面 DataList，一个包括 DropDownLists 和一个复选框。


## <a name="introduction"></a>介绍

标记和 DataList s 中的 Web 控件`EditItemTemplate`定义其可编辑的接口。 在所有可编辑的 DataList 示例中我们已检查到目前为止，可编辑的文本框中 Web 控件在撰写接口。 在[前面教程](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)我们改进通过添加验证控件的编辑时的用户体验。

`EditItemTemplate`可以进一步扩展以包含除文本框中，如 DropDownLists，RadioButtonLists，日历之外的 Web 控件等。 与文本框中，自定义以包括其他 Web 控件的编辑界面时，采用以下步骤：

1. 添加到 Web 控件`EditItemTemplate`。
2. 使用数据绑定语法将相应的数据字段值分配给相应的属性。
3. 在`UpdateCommand`事件处理程序，以编程方式访问 Web 控制值，并将它传递到相应的 BLL 方法。

在本教程中我们将创建更丰富的编辑界面 DataList，一个包括 DropDownLists 和一个复选框。 具体而言，我们将创建 DataList 列出产品信息并允许产品的名称、 供应商、 类别和不再使用的状态更新 （请参见图 1）。


[![编辑接口包含一个文本框、 两个 DropDownLists 和一个复选框](customizing-the-datalist-s-editing-interface-vb/_static/image2.png)](customizing-the-datalist-s-editing-interface-vb/_static/image1.png)

**图 1**： 编辑接口包含一个文本框、 两个 DropDownLists 和一个复选框 ([单击以查看实际尺寸的图像](customizing-the-datalist-s-editing-interface-vb/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>步骤 1： 显示产品信息

我们可以创建 DataList s 可编辑接口之前，我们首先需要构建只读的界面。 首先打开`CustomizedUI.aspx`来自页`EditDeleteDataList`文件夹并从设计器中，将 DataList 添加到页上，设置其`ID`属性`Products`。 从 DataList s 智能标记，创建新对象数据源。 命名此新 ObjectDataSource`ProductsDataSource`并将其从中检索数据配置`ProductsBLL`类的`GetProducts`方法。 作为与前面的可编辑 DataList 教程，我们将更新编辑的产品的信息通过直接访问业务逻辑层。 相应地，在更新中，INSERT、 设置的下拉列表，并删除选项卡添加到 （无）。


[![设置为 (None) 的更新、 插入和删除选项卡下拉列表](customizing-the-datalist-s-editing-interface-vb/_static/image5.png)](customizing-the-datalist-s-editing-interface-vb/_static/image4.png)

**图 2**： 将更新、 插入和删除选项卡下拉列表设置为 （无） ([单击以查看实际尺寸的图像](customizing-the-datalist-s-editing-interface-vb/_static/image6.png))


在配置对象数据源后，Visual Studio 将创建默认`ItemTemplate`列出每个数据字段的名称和值 DataList 返回。 修改`ItemTemplate`以便模板列出中的产品名称`<h4>`元素和类别名称、 供应商名称、 价格和停用的状态。 此外，添加一个编辑按钮，以确保其`CommandName`属性设置为编辑。 声明性标记我`ItemTemplate`遵循：


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

上面的标记是布局产品信息使用&lt;h4&gt;产品的名称和四个列标题`<table>`其余字段。 `ProductPropertyLabel`和`ProductPropertyValue`中定义的 CSS 类`Styles.css`，前面的教程讨论了。 图 3 显示我们通过浏览器查看时的进度。


[![显示名称、 供应商、 类别、 停用状态和每个产品的价格](customizing-the-datalist-s-editing-interface-vb/_static/image8.png)](customizing-the-datalist-s-editing-interface-vb/_static/image7.png)

**图 3**： 显示名称、 供应商、 类别、 停用状态和每个产品的价格 ([单击以查看实际尺寸的图像](customizing-the-datalist-s-editing-interface-vb/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>步骤 2： 将 Web 控件添加到编辑接口

生成自定义的 DataList 编辑界面是添加到所需的 Web 控件的第一步`EditItemTemplate`。 具体而言，我们需要 DropDownList 为类别，另一个用于供应商，和一个用于停用状态复选框。 由于产品的价格不可编辑在此示例中，我们可以继续使用标签 Web 控件中显示。

若要自定义编辑界面，请单击 DataList s 智能标记中的编辑模板链接并选择`EditItemTemplate`从下拉列表的选项。 添加到 DropDownList`EditItemTemplate`并设置其`ID`到`Categories`。


[![为类别添加 DropDownList](customizing-the-datalist-s-editing-interface-vb/_static/image11.png)](customizing-the-datalist-s-editing-interface-vb/_static/image10.png)

**图 4**： 类别添加 DropDownList ([单击以查看实际尺寸的图像](customizing-the-datalist-s-editing-interface-vb/_static/image12.png))


接下来，从 DropDownList s 智能标记，选择选择数据源选项，然后创建名为新 ObjectDataSource `CategoriesDataSource`。 配置要使用此 ObjectDataSource`CategoriesBLL`类的`GetCategories()`方法 （请参见图 5）。 接下来，DropDownList 的数据源配置向导提示输入要用于每个数据字段`ListItem`s`Text`和`Value`属性。 具有 DropDownList 显示`CategoryName`数据字段并使用`CategoryID`作为值，如图 6 中所示。


[![创建名为 CategoriesDataSource 新对象数据源](customizing-the-datalist-s-editing-interface-vb/_static/image14.png)](customizing-the-datalist-s-editing-interface-vb/_static/image13.png)

**图 5**： 创建新对象数据源命名`CategoriesDataSource`([单击以查看实际尺寸的图像](customizing-the-datalist-s-editing-interface-vb/_static/image15.png))


[![配置的 DropDownList 的显示和值字段](customizing-the-datalist-s-editing-interface-vb/_static/image17.png)](customizing-the-datalist-s-editing-interface-vb/_static/image16.png)

**图 6**： 配置 DropDownList 的显示和值字段 ([单击以查看实际尺寸的图像](customizing-the-datalist-s-editing-interface-vb/_static/image18.png))


重复这一系列的步骤来创建 DropDownList 供应商。 设置`ID`为到此 DropDownList`Suppliers`并命名为其 ObjectDataSource `SuppliersDataSource`。

添加两个 DropDownLists 后, 添加一个用于停用状态复选框和文本框的产品的名称。 设置`ID`复选框和文本框中，到`Discontinued`和`ProductName`分别。 添加 RequiredFieldValidator 以确保用户为产品的名称提供一个值。

最后，添加更新和取消按钮。 请记住，这两个按钮的命令性，其`CommandName`属性设置为更新以及取消，分别。

随意布局编辑界面你的喜好。 我已选择使用同一四列`<table>`从只读接口，下列声明性语法和屏幕截图所示的布局说明：


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample2.aspx)]


[![编辑接口熄灭放置如只读接口](customizing-the-datalist-s-editing-interface-vb/_static/image20.png)](customizing-the-datalist-s-editing-interface-vb/_static/image19.png)

**图 7**: 编辑接口是放置出如只读接口 ([单击以查看实际尺寸的图像](customizing-the-datalist-s-editing-interface-vb/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>步骤 3： 创建 EditCommand 和 CancelCommand 事件处理程序

目前，没有在数据绑定语法`EditItemTemplate`(除`UnitPriceLabel`，这通过按原义从复制`ItemTemplate`)。 我们将添加数据绑定语法短暂出现，但第一个可让的 DataList s 为创建的事件处理程序`EditCommand`和`CancelCommand`事件。 回想一下，责任`EditCommand`事件处理程序是用来呈现其编辑按钮被单击时，DataList 项的编辑界面，而`CancelCommand`的作业是 DataList 返回为其预编辑状态。

创建这些两个事件处理程序，并让他们使用下面的代码：


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample3.vb)]

使用这些两个事件处理程序就地情况下，单击编辑按钮可显示的编辑界面并单击取消按钮返回到其只读模式的编辑的项。 图 8 显示 DataList 后单击编辑按钮后 Chef Anton s 秋葵组合。 由于我们已有待向编辑的接口中，添加任何数据绑定语法`ProductName`文本框中为空白，`Discontinued`从中选择复选框未选中和的第一个项`Categories`和`Suppliers`DropDownLists。


[![单击编辑按钮显示编辑接口](customizing-the-datalist-s-editing-interface-vb/_static/image23.png)](customizing-the-datalist-s-editing-interface-vb/_static/image22.png)

**图 8**： 单击编辑按钮可显示的编辑界面 ([单击以查看实际尺寸的图像](customizing-the-datalist-s-editing-interface-vb/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>步骤 4： 将数据绑定语法添加到编辑接口

若要进行编辑界面中显示当前产品的值，我们需要使用数据绑定语法将数据字段值分配给适当的 Web 控件值。 可以通过转到编辑模板屏幕设计器通过应用数据绑定语法，并选择编辑数据绑定链接从 Web 控制智能标记。 或者，数据绑定语法可以直接添加到声明性的标记。

分配`ProductName`数据字段值`ProductName`文本框中 s`Text`属性，`CategoryID`和`SupplierID`数据字段值与`Categories`和`Suppliers`DropDownLists`SelectedValue`属性，与`Discontinued`数据字段值`Discontinued`复选框的`Checked`属性。 进行这些更改，通过设计器或直接通过声明性的标记之后, 重新访问通过浏览器页面并单击 Chef Anton 的秋葵组合的编辑按钮。 如图 9 所示，数据绑定语法已添加到文本框中、 DropDownLists 和复选框中的当前值。


[![单击编辑按钮显示编辑接口](customizing-the-datalist-s-editing-interface-vb/_static/image26.png)](customizing-the-datalist-s-editing-interface-vb/_static/image25.png)

**图 9**： 单击编辑按钮可显示的编辑界面 ([单击以查看实际尺寸的图像](customizing-the-datalist-s-editing-interface-vb/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>步骤 5： 在 UpdateCommand 事件处理程序中保存用户的更改

当用户编辑产品，并单击更新按钮，产生的回发和 DataList 的`UpdateCommand`事件激发。 在事件处理程序，我们需要读取的值中的 Web 控件从`EditItemTemplate`和与 BLL 更新的产品中数据库的接口。 正如我们已在前面的教程中看到`ProductID`通过访问的更新的产品已`DataKeys`集合。 用户输入字段访问通过以编程方式引用使用的 Web 控件`FindControl("controlID")`，如下面的代码所示：


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample4.vb)]

启动代码时通过参考`Page.IsValid`属性以确保页面上的所有验证控件都都有效。 如果`Page.IsValid`是`True`，编辑的产品 s`ProductID`从读取值`DataKeys`集合和 Web 控件中的数据条目`EditItemTemplate`以编程方式引用。 接下来，将从这些 Web 控件的值读入然后传递到相应的变量`UpdateProduct`重载。 更新后的数据，即会将 DataList 恢复到其预编辑状态。

> [!NOTE]
> 我已省略的异常处理逻辑中添加[处理 BLL-和 DAL 级别异常](handling-bll-and-dal-level-exceptions-vb.md)已设定焦点的为了使代码和此示例的教程。 作为练习，在完成本教程后添加此功能。


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>步骤 6： 处理 NULL CategoryID 和供应商 Id 值

Northwind 数据库允许`NULL`值`Products`表 s`CategoryID`和`SupplierID`列。 但是，我们编辑的接口不 t 当前容纳`NULL`值。 如果我们尝试编辑的产品的`NULL`值为其`CategoryID`或`SupplierID`列，我们将获取`ArgumentOutOfRangeException`并显示类似的错误消息：*类别具有无效，因为它的 SelectedValue中的项的列表不存在。* 此外，在该处 s 当前无法更改产品的类别或供应商值从非`NULL`值赋给`NULL`一个。

若要支持`NULL`类别和供应商 DropDownLists 的值，我们需要添加其他`ListItem`。 我已选择使用 （无） 作为`Text`值为`ListItem`，但如果 d 你喜欢，可以将其 （如空字符串） 的其他更改。 最后，记住设置 DropDownLists`AppendDataBoundItems`到`True`; 如果你忘记了为此，请将类别并绑定到的 DropDownList 的供应商将覆盖静态地添加`ListItem`。

进行这些更改，DataList s 中的 DropDownLists 标记之后`EditItemTemplate`外观应与以下类似：


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> 静态`ListItem`s 可以添加到 DropDownList 通过设计器或直接通过声明性语法。 添加的 DropDownList 项表示数据库时`NULL`值时，请务必添加`ListItem`通过声明性语法。 如果你使用`ListItem`集合编辑器在设计器中，生成的声明性语法将省略`Value`完全设置时分配的空白字符串，创建类似的声明性标记： `<asp:ListItem>(None)</asp:ListItem>`。 虽然这可能看起来无害，缺少`Value`导致 DropDownList 使用`Text`在其位置的属性值。 这意味着，如果这`NULL``ListItem`是选择，（无） 的值会尝试分配给产品数据字段 (`CategoryID`或`SupplierID`，在本教程中)，这将导致异常。 通过将显式设置`Value=""`、`NULL`值将分配给该产品数据字段时`NULL``ListItem`选择。


花些时间查看我们通过浏览器的进度。 在编辑产品时，请注意，`Categories`和`Suppliers`DropDownLists 这两个具有 （无） 开始时的下拉列表的选项。


[![类别和供应商 DropDownLists 包括 （无） 选项](customizing-the-datalist-s-editing-interface-vb/_static/image29.png)](customizing-the-datalist-s-editing-interface-vb/_static/image28.png)

**图 10**:`Categories`和`Suppliers`DropDownLists 包括 （无） 选项 ([单击以查看实际尺寸的图像](customizing-the-datalist-s-editing-interface-vb/_static/image30.png))


若要保存 （无） 选项作为数据库`NULL`值，我们需要返回到`UpdateCommand`事件处理程序。 更改`categoryIDValue`和`supplierIDValue`变量是可以为 null 的整数，并将它们分配一个值而`Nothing`才 DropDownList 的`SelectedValue`不为空字符串：


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample6.vb)]

进行此更改后，值为`Nothing`将传递给`UpdateProduct`BLL 方法如果用户选择 （无） 选项从下拉列表中，对应于`NULL`数据库值。

## <a name="summary"></a>摘要

在本教程中我们已了解如何创建包含三个不同的输入的 Web 控件，文本框中，两个 DropDownLists 和一个复选框，以及验证控件的更复杂 DataList 编辑界面。 在生成时编辑界面，步骤是相同的而不考虑所使用的 Web 控件： 首先，通过 Web 控件添加到 DataList 的`EditItemTemplate`; 使用数据绑定语法分配相应的数据字段值与相应的 Web控件属性;然后在`UpdateCommand`事件处理程序，以编程方式访问的 Web 控件和其相应的属性，将其值传递到 BLL。

时是否创建一个编辑的接口，它 s 组成刚文本框中或不同的 Web 控件的集合，请务必正确处理数据库`NULL`值。 时算`NULL`s，它是命令性，你不仅能正确显示现有`NULL`值中的编辑界面，但你提供用于标记形式的值的一种`NULL`。 对于在 DataLists DropDownLists，这通常意味着添加静态`ListItem`其`Value`属性显式设置为一个空字符串 (`Value=""`)，并将添加代码的位`UpdateCommand`事件处理程序来确定是否`NULL``ListItem`选择。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Dennis Patterson、 David Suru 和徐 Schmidt。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一篇](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
