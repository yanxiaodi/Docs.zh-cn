---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
title: "自定义的数据修改界面 (C#) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将查看如何通过替换标准的文本框中自定义的可编辑的 GridView，接口和复选框控件 alternati..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 22e99600-8d18-4a94-a20e-a3a62bb63798
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: fb0b20ab25d87eddc0b2f9da786db469b16f861a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="customizing-the-data-modification-interface-c"></a>自定义的数据修改界面 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_CS.exe)或[下载 PDF](customizing-the-data-modification-interface-cs/_static/datatutorial20cs1.pdf)

> 在本教程中我们将查看如何通过替换标准的文本框中自定义的可编辑的 GridView，接口和复选框控件与备用输入的 Web 控件。


## <a name="introduction"></a>介绍

BoundFields 和 CheckBoxFields GridView 和说明如何控件所使用的简化修改由于其能够呈现只读的、 可编辑，以及可插入接口的数据的过程。 无需添加任何附加的声明性标记或代码，可以呈现这些接口。 但是，BoundField 和 CheckBoxField 的接口缺少通常需要在实际方案中可定制性。 为了自定义可编辑或可插入界面 GridView 或说明中的，我们需要改用 TemplateField。

在[前面教程](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)我们已了解如何通过添加验证 Web 控件自定义的数据修改接口。 在本教程中我们将查看如何自定义的实际数据集合 Web 控件，替换 BoundField 和 CheckBoxField 的标准文本框和复选框控件与备用输入的 Web 控件。 具体而言，我们生成了允许的产品名称、 类别、 供应商和不再使用的状态要更新的可编辑 GridView。 在编辑的特定行时，类别和供应商字段将呈现为 DropDownLists 中,，包含的一套可用类别和供应商可供选择。 此外，我们会将提供两个选项的说明如何控件替换的 CheckBoxField 的默认复选框:"活动"和"停止"。


[![GridView 的编辑界面包括 DropDownLists 和单选按钮](customizing-the-data-modification-interface-cs/_static/image2.png)](customizing-the-data-modification-interface-cs/_static/image1.png)

**图 1**: GridView 编辑接口包括 DropDownLists 和单选按钮 ([单击以查看实际尺寸的图像](customizing-the-data-modification-interface-cs/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>步骤 1： 创建相应`UpdateProduct`重载

在本教程中我们将生成可编辑的 GridView，允许编辑的产品的名称、 类别、 供应商，并停止使用状态。 因此，我们需要`UpdateProduct`接受这些四个产品值由五个输入参数的重载加上`ProductID`。 我们前面的重载，这样一个中的类似内容：

1. 从指定的数据库中检索产品信息`ProductID`，
2. 更新`ProductName`， `CategoryID`， `SupplierID`，和`Discontinued`字段，并
3. 将更新请求发送到通过 TableAdapter 的 DAL`Update()`方法。

为简洁起见，此特定重载我已省略业务规则检查，以确保被标记为停止使用的产品不由其供应商提供的唯一产品。 随意添加中，如果你愿意，或者，理想情况下，出到单独的方法的逻辑的重构。

下面的代码演示新`UpdateProduct`重载中`ProductsBLL`类：


[!code-csharp[Main](customizing-the-data-modification-interface-cs/samples/sample1.cs)]

## <a name="step-2-crafting-the-editable-gridview"></a>步骤 2： 编写可编辑的 GridView

与`UpdateProduct`重载添加，我们已准备好创建我们可编辑的 GridView。 打开`CustomizedUI.aspx`页面`EditInsertDelete`文件夹并将 GridView 控件添加到设计器。 从 GridView 的智能标记，接下来，创建新对象数据源。 配置对象数据源以检索产品信息通过`ProductBLL`类的`GetProducts()`方法，并更新产品数据使用`UpdateProduct`我们刚刚创建的重载。 从的插入和删除选项卡，从下拉列表中选择 （无）。


[![配置对象数据源以使用刚刚创建的 UpdateProduct 重载](customizing-the-data-modification-interface-cs/_static/image5.png)](customizing-the-data-modification-interface-cs/_static/image4.png)

**图 2**： 配置使用 ObjectDataSource`UpdateProduct`重载刚刚创建 ([单击以查看实际尺寸的图像](customizing-the-data-modification-interface-cs/_static/image6.png))


如我们所见整个数据修改教程，Visual Studio 创建 ObjectDataSource 的声明性语法将分配`OldValuesParameterFormatString`属性`original_{0}`。 此操作，请当然，不会使用我们的业务逻辑层由于我们方法不需要原始`ProductID`要在中传递的值。 因此，随着我们在前面的教程中，需花费一段时间来删除此属性分配的声明性语法，或相反，将此属性的值设置为`{0}`。

此更改之后 ObjectDataSource 的声明性标记应如下所示：


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample2.aspx)]

请注意，`OldValuesParameterFormatString`属性已删除，以及是否有`Parameter`中`UpdateParameters`为每个所需的输入参数的集合我们`UpdateProduct`重载。

尽管对象数据源配置为更新产品值的一个子集，当前显示 GridView*所有*的产品字段。 花些时间编辑 GridView，以便：

- 它只包括`ProductName`， `SupplierName`， `CategoryName` BoundFields 和`Discontinued`CheckBoxField
- `CategoryName`和`SupplierName`字段 （左侧） 之前出现`Discontinued`CheckBoxField
- `CategoryName`和`SupplierName`BoundFields 的`HeaderText`属性分别设置为"类别"和"供应商"
- 启用编辑支持 （请检查 GridView 的智能标记中的启用编辑复选框）

这些更改后设计器将查找类似于图 3： 具有如下所示的 GridView 的声明性语法。


[![从 GridView 中删除不需要的字段](customizing-the-data-modification-interface-cs/_static/image8.png)](customizing-the-data-modification-interface-cs/_static/image7.png)

**图 3**： 从 GridView 中删除不需要的字段 ([单击以查看实际尺寸的图像](customizing-the-data-modification-interface-cs/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample3.aspx)]

此时 GridView 的只读的行为已完成。 查看数据时，每个产品呈现为 GridView，显示产品的名称、 类别、 供应商中的行，并停止使用状态。


[![GridView 的只读接口已完成](customizing-the-data-modification-interface-cs/_static/image11.png)](customizing-the-data-modification-interface-cs/_static/image10.png)

**图 4**: GridView 只读接口是否已完成 ([单击以查看实际尺寸的图像](customizing-the-data-modification-interface-cs/_static/image12.png))


> [!NOTE]
> 中所述[概述的插入、 更新和删除数据教程](an-overview-of-inserting-updating-and-deleting-data-cs.md)，它是非常重要的 GridView 的视图状态是启用 （默认行为）。 如果设置 GridView s`EnableViewState`属性`false`，运行出现无意中删除或编辑记录的并发用户的风险。 请参阅[警告： 禁用并发问题与 ASP.NET 2.0 GridViews/说明/FormViews 该支持编辑和/或删除和其视图状态](http://scottonwriting.net/sowblog/posts/10054.aspx)有关详细信息。


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>步骤 3： 使用 DropDownList 类别和供应商编辑接口

回想一下，`ProductsRow`对象包含`CategoryID`， `CategoryName`， `SupplierID`，和`SupplierName`属性，它们提供中的实际外键 ID 值`Products`数据库表和相应`Name`中的值`Categories`和`Suppliers`表。 `ProductRow`的`CategoryID`和`SupplierID`可以同时进行读取和写入，而`CategoryName`和`SupplierName`属性标记为只读。

由于的只读状态`CategoryName`和`SupplierName`相应 BoundFields 中获得了属性，其`ReadOnly`属性设置为`true`，防止这些值编辑行时被修改。 尽管我们可以设置`ReadOnly`属性`false`、 呈现`CategoryName`和`SupplierName`BoundFields 作为在编辑的文本框中，这种方法将导致异常时用户尝试将产品的更新，因为没有任何`UpdateProduct`中采用重载`CategoryName`和`SupplierName`输入。 事实上，我们不想创建此类重载有两个原因：

- `Products`表没有`SupplierName`或`CategoryName`字段，但`SupplierID`和`CategoryID`。 因此，我们希望我们方法传递这些特定的 ID 值，不是其查找表的值。
- 需要用户键入的供应商或类别的名称是小于理想，因为它要求用户了解可用的类别和供应商和其正确的拼写。

供应商和类别字段应显示该类别和在只读模式下的供应商的名称 （方式与其现在） 和适用的选项时正在编辑的下拉列表。 使用下拉列表，最终用户可以快速查看哪些类别和供应商有之间进行选择，并且多个可以轻松地使它们的选择。

若要提供此行为，我们需要将转换`SupplierName`和`CategoryName`到 TemplateFields BoundFields 其`ItemTemplate`发出`SupplierName`和`CategoryName`值并且其`EditItemTemplate`使用到列表的 DropDownList 控件可用类别和供应商。

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>添加`Categories`和`Suppliers`DropDownLists

首先转换`SupplierName`和`CategoryName`到通过 TemplateFields BoundFields： 单击从 GridView 的智能标记的编辑列链接; 从列表中的左下; 选择 BoundField 并单击"将此字段转换TemplateField"链接。 转换过程将创建具有同时为 TemplateField`ItemTemplate`和`EditItemTemplate`，如以下声明性语法中所示：


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample4.aspx)]

因为 BoundField 已标记为只读，同时`ItemTemplate`和`EditItemTemplate`包含标签 Web 控件`Text`属性绑定到相应的数据字段 (`CategoryName`，在上面的语法)。 我们需要修改`EditItemTemplate`，标签 Web 控件替换的 DropDownList 控件。

如我们所见前面的教程中，可以编辑该模板通过设计器或直接从声明性语法。 若要对其进行编辑通过设计器，单击从 GridView 的智能标记的编辑模板链接，然后选择可以使用类别字段`EditItemTemplate`。 删除标签 Web 控件并将其与 DropDownList 控件，将下拉列表的 ID 属性设置为`Categories`。


[![删除文本框中并向 EditItemTemplate 添加 DropDownList](customizing-the-data-modification-interface-cs/_static/image14.png)](customizing-the-data-modification-interface-cs/_static/image13.png)

**图 5**： 删除文本框中，并添加到 DropDownList `EditItemTemplate` ([单击以查看实际尺寸的图像](customizing-the-data-modification-interface-cs/_static/image15.png))


接下来，我们必须填充可用类别下拉列表。 单击下拉列表的智能标记中的选择数据源链接，然后选择创建名为新 ObjectDataSource `CategoriesDataSource`。


[![创建一个名为 CategoriesDataSource 的新 ObjectDataSource 控件](customizing-the-data-modification-interface-cs/_static/image17.png)](customizing-the-data-modification-interface-cs/_static/image16.png)

**图 6**： 创建新的 ObjectDataSource 控件命名`CategoriesDataSource`([单击以查看实际尺寸的图像](customizing-the-data-modification-interface-cs/_static/image18.png))


若要使此对象返回的所有类别的数据源，将其绑定到`CategoriesBLL`类的`GetCategories()`方法。


[![将对象数据源绑定到 CategoriesBLL GetCategories() 方法](customizing-the-data-modification-interface-cs/_static/image20.png)](customizing-the-data-modification-interface-cs/_static/image19.png)

**图 7**： 绑定到 ObjectDataSource`CategoriesBLL`的`GetCategories()`方法 ([单击以查看实际尺寸的图像](customizing-the-data-modification-interface-cs/_static/image21.png))


最后，配置的 DropDownList 设置以便`CategoryName`字段显示在每个 DropDownList`ListItem`与`CategoryID`用作值的字段。


[![具有类别名称字段显示和 CategoryID 用作的值](customizing-the-data-modification-interface-cs/_static/image23.png)](customizing-the-data-modification-interface-cs/_static/image22.png)

**图 8**： 具有`CategoryName`显示字段和`CategoryID`用作的值 ([单击以查看实际尺寸的图像](customizing-the-data-modification-interface-cs/_static/image24.png))


进行这些更改的声明性标记后`EditItemTemplate`中`CategoryName`TemplateField 中将包括 DropDownList 和对象数据源：


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> 在 DropDownList`EditItemTemplate`必须启用其视图状态。 我们很快会向数据绑定语法添加到下拉列表的声明性语法和数据绑定命令，如`Eval()`和`Bind()`只能出现在启用了其视图状态的控件。


重复上述步骤以添加名为 DropDownList`Suppliers`到`SupplierName`TemplateField 的`EditItemTemplate`。 这将涉及添加到 DropDownList`EditItemTemplate`和创建另一个对象数据源。 `Suppliers` DropDownList 的对象数据源，但是，应配置为调用`SuppliersBLL`类的`GetSuppliers()`方法。 此外，配置`Suppliers`DropDownList 以显示`CompanyName`字段并使用`SupplierID`字段的值作为其`ListItem`s。

添加到两个 DropDownLists 后`EditItemTemplate`s，加载浏览器中，然后单击编辑按钮 Chef Anton Cajun Seasoning 产品。 如图 9 所示，该产品的类别和供应商列呈现为包含可用类别和供应商可供选择的下拉列表。 但请注意，*第一个*这两个下拉列表中的项目默认选择 (Beverages 类别) 和尖端液体作为供应商，即使 Chef Anton Cajun Seasoning 是提供的新奥尔良 Cajun Condiment带来快乐。


[![默认为选中状态的下拉列表中的第一个项](customizing-the-data-modification-interface-cs/_static/image26.png)](customizing-the-data-modification-interface-cs/_static/image25.png)

**图 9**： 默认情况下选择下拉列表中的第一个项 ([单击以查看实际尺寸的图像](customizing-the-data-modification-interface-cs/_static/image27.png))


此外，如果单击更新时，你将找到的产品的`CategoryID`和`SupplierID`值设置为`NULL`。 这两种意外的行为由引起的因为在 DropDownLists `EditItemTemplate` s 从基础的产品数据未绑定到任何数据字段。

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>绑定到 DropDownLists`CategoryID`和`SupplierID`数据字段

为了具有编辑的产品类别和供应商下拉列表设置为适当的值，并具有发送回 BLL 的这些值`UpdateProduct`单击更新后的方法，我们需要将绑定 DropDownLists 的`SelectedValue`属性设置为`CategoryID`和`SupplierID`使用双向数据绑定的数据字段。 若要完成此操作与`Categories`DropDownList，你可以添加`SelectedValue='<%# Bind("CategoryID") %>'`直接到声明性语法。

或者，你可以通过编辑该模板通过设计器并单击从下拉列表的智能标记编辑数据绑定链接来设置数据绑定的 DropDownList。 接下来，指示`SelectedValue`属性应绑定到`CategoryID`字段使用双向数据绑定 （请参阅图 10）。 重复任一个声明性或设计器进程将绑定`SupplierID`数据字段`Suppliers`DropDownList。


[![将 CategoryID 绑定到使用双向数据绑定的 DropDownList SelectedValue 属性](customizing-the-data-modification-interface-cs/_static/image29.png)](customizing-the-data-modification-interface-cs/_static/image28.png)

**图 10**： 绑定`CategoryID`到 DropDownList`SelectedValue`属性使用双向数据绑定 ([单击以查看实际尺寸的图像](customizing-the-data-modification-interface-cs/_static/image30.png))


一旦绑定已应用于`SelectedValue`的两个 DropDownLists 属性中，编辑的产品类别和供应商列将默认为当前产品的值。 如果单击更新，`CategoryID`和`SupplierID`选择的下拉列表项的值将传递到`UpdateProduct`方法。 图 11 显示本教程后已添加的数据绑定语句;请注意如何 Chef Anton Cajun Seasoning 的所选的下拉列表项将正确 Condiment 和新奥尔良 Cajun 带来快乐。


[![默认情况下选择编辑产品的当前类别和供应商值](customizing-the-data-modification-interface-cs/_static/image32.png)](customizing-the-data-modification-interface-cs/_static/image31.png)

**图 11**： 默认情况下选择编辑产品当前类别和供应商值 ([单击以查看实际尺寸的图像](customizing-the-data-modification-interface-cs/_static/image33.png))


## <a name="handlingnullvalues"></a>处理`NULL`值

`CategoryID`和`SupplierID`中的列`Products`表可以是`NULL`，但在 DropDownLists `EditItemTemplate` s 不包含要表示的列表项`NULL`值。 这样做有以下两种结果：

- 用户不能使用我们的接口以从非更改为产品类别或供应商`NULL`值赋给`NULL`一个
- 如果产品具有`NULL``CategoryID`或`SupplierID`，单击编辑按钮将导致异常。 这是因为`NULL`返回值`CategoryID`(或`SupplierID`) 中`Bind()`语句未映射到下拉列表中的值 (DropDownList 引发异常时其`SelectedValue`属性设置为值*不*列表项其集合中)。

为了支持`NULL``CategoryID`和`SupplierID`我们需要添加另一个值，`ListItem`到来表示每个 DropDownList`NULL`值。 在[主/详细信息筛选与 DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)教程，我们已了解如何添加其他`ListItem`到数据绑定的 DropDownList，这涉及设置的 DropDownList`AppendDataBoundItems`属性`true`和手动添加其他`ListItem`。 在该前面的教程，但是，我们添加了`ListItem`与`Value`的`-1`。 数据绑定中的逻辑在 ASP.NET 中，但是，将自动转换到的空白字符串`NULL`值和相反的操作。 因此，本教程中我们想`ListItem`的`Value`为空字符串。

通过设置这两个 DropDownLists 的开始`AppendDataBoundItems`属性`true`。 接下来，添加`NULL``ListItem`通过添加以下`<asp:ListItem>`到每个 DropDownList 元素这样的声明性标记看起来就像：


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample6.aspx)]

我已选择使用"（无）"的文本值作为此`ListItem`，但可以将其以也是空白字符串，如果你想要进行更改。

> [!NOTE]
> 正如我们看到在*主/详细信息筛选与 DropDownList*教程中， `ListItem` s 可以通过单击的 DropDownList 添加到通过设计器 DropDownList`Items`属性窗口中的属性 (将显示`ListItem`集合编辑器)。 但是，请务必添加`NULL``ListItem`本教程中通过声明性语法。 如果你使用`ListItem`集合编辑器中，生成的声明性语法将省略`Value`完全设置时分配的空白字符串，创建类似的声明性标记： `<asp:ListItem>(None)</asp:ListItem>`。 尽管这可能看起来无害，缺失值会导致 DropDownList 使用`Text`在其位置的属性值。 这意味着，如果这`NULL``ListItem`是选择，"（无）"的值会尝试分配给`CategoryID`，这将导致异常。 通过将显式设置`Value=""`、`NULL`值将赋给`CategoryID`时`NULL``ListItem`选择。


对供应商 DropDownList 重复这些步骤。

与此其他`ListItem`，现在可以分配的编辑界面`NULL`值与产品的`CategoryID`和`SupplierID`字段，如图 12 中所示。


[![选择 （无） 的产品类别或供应商分配 NULL 值](customizing-the-data-modification-interface-cs/_static/image35.png)](customizing-the-data-modification-interface-cs/_static/image34.png)

**图 12**： 选择 （无） 分配到`NULL`为产品类别或供应商的值 ([单击以查看实际尺寸的图像](customizing-the-data-modification-interface-cs/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>步骤 4： 使用单选按钮为不再使用的状态

当前产品的`Discontinued`使用 CheckBoxField，呈现一个用于只读的行的已禁用的复选框和正在编辑的行启用复选框表示数据字段。 通常适用此用户界面时，我们可以根据需要使用为 TemplateField 定制它。 对于本教程中，让我们更改 CheckBoxField 转换的两个选项"活动"和"停止使用"使用说明如何控制用户可以从中指定产品的为 TemplateField`Discontinued`值。

通过将转换启动`Discontinued`转换为 TemplateField，将创建与 TemplateField CheckBoxField`ItemTemplate`和`EditItemTemplate`。 两个模板都包含一个具有复选框其`Checked`属性绑定到`Discontinued`数据字段，唯一的区别是，这两者之间`ItemTemplate`的复选框的`Enabled`属性设置为`false`。

替换中的复选框`ItemTemplate`和`EditItemTemplate`有一个说明如何控件设置这两个 RadioButtonLists 的`ID`属性设置为`DiscontinuedChoice`。 接下来，指示 RadioButtonLists 应每个包含两个单选按钮，一个标记为"活动"值为"False"，标记为"停用"的一个值为"True"。 若要完成此您可以输入`<asp:ListItem>`直接通过声明性语法或使用中的元素`ListItem`从设计器的集合编辑器。 图 13 显示`ListItem`集合编辑器后两个单选按钮选项已指定。


[![添加](customizing-the-data-modification-interface-cs/_static/image38.png)](customizing-the-data-modification-interface-cs/_static/image37.png)

**图 13**: 说明如何向中添加"活动"和"已中断"选项 ([单击以查看实际尺寸的图像](customizing-the-data-modification-interface-cs/_static/image39.png))


说明如何在自`ItemTemplate`不应可编辑，设置其`Enabled`属性`false`、 保留`Enabled`属性`true`（默认值） 中说明如何为`EditItemTemplate`。 这将使单选按钮以只读方式，非编辑行中，但将允许用户更改编辑过的行的单选按钮值。

我们仍需要分配的说明如何控件的`SelectedValue`属性，以便相应的单选按钮处于选中状态取决于产品的`Discontinued`数据字段。 直接在声明性的标记或 RadioButtonLists 的智能标记中的编辑数据绑定链接，也可以与本教程中前面检查 DropDownLists，添加此数据绑定语法。

在添加两个 RadioButtonLists 和配置它们后, `Discontinued` TemplateField 的声明性标记应如下所示：


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample7.aspx)]

进行这些更改后，`Discontinued`列的安全提供程序已从的复选框列表转换为单选按钮对 （请参阅图 14） 的列表。 在编辑产品时，相应的单选按钮处于选中状态，并可以通过选择其他单选按钮并单击更新更新产品的停用的状态。


[![单选按钮对已替换为已停止使用复选框](customizing-the-data-modification-interface-cs/_static/image41.png)](customizing-the-data-modification-interface-cs/_static/image40.png)

**图 14**: 停止使用复选框已替换为单选按钮对 ([单击以查看实际尺寸的图像](customizing-the-data-modification-interface-cs/_static/image42.png))


> [!NOTE]
> 由于`Discontinued`中的列`Products`数据库不能具有`NULL`值，我们不需要担心捕获`NULL`界面中的信息。 如果是，但是，`Discontinued`列无法包含`NULL`值我们想要添加第三个单选按钮与向列表其`Value`设置为空字符串 (`Value=""`)，就像使用的类别和供应商 DropDownLists。


## <a name="summary"></a>摘要

虽然 BoundField 和 CheckBoxField 自动呈现只读的编辑，并且他们插入接口，缺少自定义项的能力。 通常情况下，不过，我们将需要自定义编辑或将其插入接口，可能添加验证控件 （如在前面的教程，我们已了解） 或通过自定义数据收集用户界面 （如我们在本教程中看到）。 自定义与为 TemplateField 界面可以是计算总和中的以下步骤：

1. 添加为 TemplateField 或将现有 BoundField 或 CheckBoxField 转换转换为 TemplateField
2. 根据需要增加接口
3. 将适当的数据字段绑定到新添加的 Web 控件使用双向数据绑定

除了使用内置的 ASP.NET Web 控件，你还可以自定义为 TemplateField 与自定义的、 已编译的服务器控件和用户控件的模板。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一页](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
[下一页](implementing-optimistic-concurrency-cs.md)
