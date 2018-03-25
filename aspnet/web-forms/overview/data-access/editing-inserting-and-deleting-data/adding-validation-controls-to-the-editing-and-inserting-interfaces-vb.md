---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: "将验证控件添加到的编辑，并将插入接口 (VB) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们会将验证控件添加到 EditItemTemplate 和 InsertItemTemplate 数据 Web 控件，以提供更是多么容易..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: 53414aa17514d07083fe05b8c2abcba10a01cf98
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>将验证控件添加到的编辑，并将插入接口 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe)或[下载 PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> 在本教程中我们会将验证控件添加到 EditItemTemplate 和 InsertItemTemplate 数据 Web 控件，以提供更安全的用户界面是多么容易。


## <a name="introduction"></a>介绍

在示例中的 GridView 和说明如何控制我们已介绍了在过去三个教程具有所有已 BoundFields 和组成 CheckBoxFields （字段类型将 GridView 或说明如何绑定到数据源时自动添加由 Visual Studio控制通过智能标记）。 当编辑 GridView 或说明中的行，不是只读的这些 BoundFields 会转换在文本框中中,，最终用户可以在其中修改的现有数据。 同样，当插入新记录到说明如何控制，这些 BoundFields 其`InsertVisible`属性设置为`True`（默认值） 呈现为空的文本框中，用户可以在其中提供新记录的字段值。 同样，CheckBoxFields，已被禁用标准的、 只读的界面中，将转换为中的编辑和插入接口启用复选框。

尽管默认的编辑并插入 BoundField 和 CheckBoxField 接口可能很有用，接口中没有任何种类的验证。 如果用户是犯了数据条目-如省略`ProductName`字段或输入的值无效`UnitsInStock`（如-50) 将会引发异常从内的应用程序体系结构的深度。 此异常可以正常处理中所示时[以前一教程](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)，理想情况下编辑或将其插入用户界面将包括验证控制，以防止用户输入中的此类数据无效第一个位置。

为了提供自定义的编辑或插入的接口，我们需要替换为 TemplateField BoundField 或 CheckBoxField。 TemplateFields，所讨论的主题中[GridView 控件中使用 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)和[中说明使用 TemplateFields](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md)教程中，可以包含多个模板定义分隔为不同的行状态的接口。 TemplateField`ItemTemplate`而是用于在只读字段或说明或 GridView 控件中的行进行呈现时`EditItemTemplate`和`InsertItemTemplate`指示这些接口，以便进行编辑，然后分别插入模式下，使用。

在本教程中我们将看到将验证控件添加到 TemplateField 是多么容易`EditItemTemplate`和`InsertItemTemplate`以提供更安全的用户界面。 具体而言，本教程将创建中的示例[检查与插入、 更新和删除的事件相关联](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)教程和补充了编辑和插入接口以包含相应的验证。

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-vbmd"></a>步骤 1： 复制中的示例[与插入、 更新和删除检查事件相关联](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

在[检查与插入、 更新和删除的事件相关联](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)我们创建一个页，列出的名称和价格中可编辑的 GridView 的产品的教程。 此外，页面包含说明其`DefaultMode`属性设置为`Insert`，从而始终呈现在插入模式下。 此说明如何从用户无法输入新产品的名称和价格，单击插入，且具有它添加到系统中 （请参见图 1）。


[![前面的示例允许用户添加新产品和编辑现有的](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**图 1**: 上一示例允许用户添加新产品和编辑现有的 ([单击以查看实际尺寸的图像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))


我们在本教程的目标是增加要提供验证控件的说明和 GridView。 具体而言，我们验证逻辑将：

- 需要在插入或编辑产品时的情况下提供的名称
- 要求时插入记录; 提供的价格当编辑记录，我们仍将需要使用价格，但将在 GridView 的中使用的编程逻辑`RowUpdating`前面教程中已存在的事件处理程序
- 确保输入价格的值是一种有效的货币格式

我们可以看看补充前面的示例，以包含验证之前，我们首先需要复制中的示例`DataModificationEvents.aspx`到本教程中的页的页`UIValidation.aspx`。 若要完成此，我们需要通过同时复制`DataModificationEvents.aspx`页面的声明性标记和其源代码。 首先通过声明性标记复制通过执行以下步骤：

1. 打开`DataModificationEvents.aspx`Visual Studio 中的页
2. 请转到页面的声明性标记 （单击页面底部的源按钮）
3. 复制文本在内的`<asp:Content>`和`</asp:Content>`标记 （3 至 44 行），如图 2 中所示。


[![复制文本内&lt;asp： 内容&gt;控件](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**图 2**： 文本中复制`<asp:Content>`控件 ([单击以查看实际尺寸的图像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))


1. 打开`UIValidation.aspx`页
2. 请转到页面的声明性标记
3. 将在文本粘贴`<asp:Content>`控件。

若要复制的源代码，请打开`DataModificationEvents.aspx.vb`页上，并将复制不仅仅是文本*内*`EditInsertDelete_DataModificationEvents`类。 将复制三个事件处理程序 (`Page_Load`， `GridView1_RowUpdating`，和`ObjectDataSource1_Inserting`)，但不要**不**复制类声明。 粘贴复制的文本*内*`EditInsertDelete_UIValidation`类`UIValidation.aspx.vb`。

之后将移动到的内容和从代码上方`DataModificationEvents.aspx`到`UIValidation.aspx`，花些时间来测试你的浏览器中的进度。 你应看到相同输出，并且遇到每个这些两个页面中相同的功能 (有关的屏幕截图回头参考图 1`DataModificationEvents.aspx`操作中)。

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>步骤 2： 将 BoundFields 转换为 TemplateFields

若要将验证控件添加到的编辑和插入接口，使用说明和 GridView 控件 BoundFields 需要将其转换为 TemplateFields。 若要实现此目的，请的 GridView 和说明的智能标记中的编辑列和编辑字段链接中分别单击。 那里出现，请选择每个 BoundFields，然后单击"将此字段转换为 TemplateField"链接。


[![将每个说明的和 GridView 的 BoundFields 转换为 TemplateFields](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**图 3**： 转换每个说明的和 GridView 的 BoundFields 到 TemplateFields ([单击以查看实际尺寸的图像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))


将 BoundField 转换为通过字段对话框中为 TemplateField 生成 TemplateField 展现作为 BoundField 本身相同的只读的、 编辑以及插入接口。 以下标记显示的声明性语法`ProductName`说明后已转换转换为 TemplateField 字段：

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

请注意，此 TemplateField 了自动创建的三个模板`ItemTemplate`， `EditItemTemplate`，和`InsertItemTemplate`。 `ItemTemplate`显示单个数据字段值 (`ProductName`) 使用标签 Web 控件，而`EditItemTemplate`和`InsertItemTemplate`将数据字段与文本框的相关联的文本框中 Web 控件中显示数据字段值`Text`使用双向数据绑定的属性。 由于我们只使用说明如何在此页中插入，你可能会删除`ItemTemplate`和`EditItemTemplate`从两个 TemplateFields，尽管不没有保留任何影响。

因为 GridView 不支持内置插入功能的说明，将转换 GridView`ProductName`字段转换为 TemplateField 仅导致`ItemTemplate`和`EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

通过单击"转换此字段转换为 TemplateField"，Visual Studio 创建了其模板模拟的用户界面的转换后的 BoundField TemplateField。 可以通过访问此页通过浏览器对此进行验证。 你将找到的外观和行为的 TemplateFields 都等于体验时 BoundFields 只能改为使用。

> [!NOTE]
> 随意自定义中的模板，根据需要编辑的接口。 例如，我们可能想要文本框中`UnitPrice`TemplateFields 呈现为一个较小的文本框，比`ProductName`文本框。 若要完成此操作可以设置文本框的`Columns`为适当的值的属性，或者提供通过绝对宽度`Width`属性。 在下一教程中我们将了解如何以完全自定义编辑界面，通过将替换为一个备用数据条目 Web 控件的文本框。


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>步骤 3： 将验证控件添加到的 GridView`EditItemTemplate` s

构造数据输入窗体时, 很重要，则用户输入所有必填的字段和所有提供的输入是合法的格式正确的值。 为了帮助确保用户的输入有效，ASP.NET 提供了五个内置的验证控件，旨在用于验证的单个输入控件的值：

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx)可确保已提供的值
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx)验证值与另一个 Web 控件值或一个常量值，或确保值的格式是合法的指定的数据类型
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx)确保值是值的范围之内
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx)验证值与[正则表达式](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx)验证值与自定义、 用户定义的方法

有关这些五个控件的详细信息，请参阅[验证控件部分](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx)的[ASP.NET 快速入门教程](https://asp.net/QuickStart/aspnet/)。

我们的教程，我们将需要使用中的说明和 GridView 的 RequiredFieldValidator `ProductName` TemplateFields 和中说明的 RequiredFieldValidator `UnitPrice` TemplateField。 此外，我们将需要向这两个控件的添加 CompareValidator `UnitPrice` TemplateFields，以确保输入的价格大于或等于 0 的值而有效的货币格式提供。

> [!NOTE]
> 尽管 ASP.NET 1.x 具有这些相同的五个验证控件、 ASP.NET 2.0 已添加了大量改进、 Internet Explorer 以外的浏览器以及到分区到页面上的验证控件的功能的两个正在客户端脚本支持主验证组。 有关 2.0 中的新验证控件功能的详细信息，请参阅[将 ASP.NET 2.0 中的验证控件](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)。


让我们首先，通过添加必要的验证控件到`EditItemTemplate`GridView TemplateFields。 若要完成此操作，请单击上 GridView 的智能标记上以打开该模板编辑接口中的编辑模板链接。 从这里，你可以选择要从下拉列表编辑的模板。 由于我们想要增加编辑界面，我们需要验证将控件添加到`ProductName`和`UnitPrice`的`EditItemTemplate`s。


[![我们需要将产品名称和单价的 EditItemTemplates 扩展](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**图 4**： 我们需要扩展`ProductName`和`UnitPrice`的`EditItemTemplate`s ([单击以查看实际尺寸的图像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))


在`ProductName` `EditItemTemplate`，通过从工具箱拖动到模板编辑界面中，添加 RequiredFieldValidator 将置于之后文本框。


[![将 RequiredFieldValidator 添加到 ProductName EditItemTemplate](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**图 5**： 添加到 RequiredFieldValidator `ProductName` `EditItemTemplate` ([单击以查看实际尺寸的图像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))


所有验证控件验证单个 ASP.NET Web 控件的输入都工作。 因此，我们需要指示我们刚添加 RequiredFieldValidator 应验证在文本框中`EditItemTemplate`; 这通过设置验证控件的实现[ControlToValidate 属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx)到`ID`的适当的 Web 控件。 文本框中当前具有而是 nondescript`ID`的`TextBox1`，但让我们将其更改为更合适。 单击该模板中的文本框，然后，从属性窗口中，更改`ID`从`TextBox1`到`EditProductName`。


[![到 EditProductName 更改文本框的 ID](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**图 6**： 更改文本框的`ID`到`EditProductName`([单击以查看实际尺寸的图像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))


接下来，设置 RequiredFieldValidator`ControlToValidate`属性`EditProductName`。 最后，设置[ErrorMessage 属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx)到"你必须提供该产品的名称"和[文本属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx)到"\*"。 `Text`属性值，如果提供，则如果验证失败，通过该验证控件显示的文本。 `ErrorMessage`属性值，该值是必需的由 ValidationSummary 控件; 如果`Text`省略属性值，`ErrorMessage`属性值也是由上输入无效的验证控件显示的文本。

在设置后 RequiredFieldValidator 这三个属性，你的屏幕应类似于图 7。


[![设置 RequiredFieldValidator ControlToValidate、 ErrorMessage 和文本属性](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**图 7**： 设置 RequiredFieldValidator `ControlToValidate`， `ErrorMessage`，和`Text`属性 ([单击以查看实际尺寸的图像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))


使用添加到 RequiredFieldValidator `ProductName` `EditItemTemplate`，所有剩下是添加到必要的验证`UnitPrice` `EditItemTemplate`。 由于我们已为此页上，确定，`UnitPrice`可选的在编辑记录，我们无需添加 RequiredFieldValidator。 我们执行此操作，但是，需要添加 CompareValidator 以确保`UnitPrice`，如果提供，作为一种货币格式正确，大于或等于 0。

我们将添加到 CompareValidator 之前`UnitPrice` `EditItemTemplate`，让我们首先更改文本框中 Web 控件的 ID 从`TextBox2`到`EditUnitPrice`。 此更改后，添加 CompareValidator，设置其`ControlToValidate`属性`EditUnitPrice`，将其`ErrorMessage`属性设置为"价格必须大于或等于零并且不能包括货币符号"，并将其`Text`属性"\*".

若要指示`UnitPrice`值必须是大于或等于 0，设置 CompareValidator[运算符属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx)到`GreaterThanEqual`，将其[ValueToCompare 属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx)为"0"，并其[键入属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx)到`Currency`。 下面的声明性语法演示`UnitPrice`TemplateField 的`EditItemTemplate`进行了这些更改后：

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

进行这些更改后，在浏览器中打开页面。 如果你尝试省略名称或输入无效的价格值编辑产品时，文本框旁边将出现一个星号。 如图 8 所示，包含如 19.95 美元的货币符号的价格值将被视为无效。 CompareValidator `Currency` `Type` （如逗号或句号，具体取决于的区域性设置） 的数字分隔符的前导加号或减号，允许而不是*不*允许货币符号。 此行为可能 perplex 用户，如编辑接口当前呈现`UnitPrice`使用货币格式。

> [!NOTE]
> 回想一下，在*插入、 更新和删除与关联事件*教程，我们将设置 BoundField`DataFormatString`属性`{0:c}`以便其设置为货币格式。 此外，我们设置`ApplyFormatInEditMode`属性为 true，导致 GridView 的编辑界面来设置格式`UnitPrice`作为一种货币。 Visual Studio 转换时 BoundField 转换为 TemplateField，记下这些设置，并格式化文本框的`Text`属性作为一种货币使用数据绑定语法`<%# Bind("UnitPrice", "{0:c}") %>`。


[![无效的输入的文本框旁边显示了星号](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**图 8**: 星号显示下一步到无效的输入的文本框 ([单击以查看实际尺寸的图像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))


尽管验证可充当的是，用户必须手动编辑一个记录，不是可接受时删除的货币符号。 若要解决此问题，我们具有三个选项：

1. 配置`EditItemTemplate`以便`UnitPrice`格式值不为货币。
2. 允许用户通过删除 CompareValidator 并将它替换为正确的格式正确的货币值检查 RegularExpressionValidator 输入货币符号。 此处问题是要验证货币值的正则表达式不是美观，并且如果我们想要合并的区域性设置需要编写代码。
3. 完全删除验证控件和依赖于在 GridView 的服务器端验证逻辑`RowUpdating`事件处理程序。

让我们使用选项 #1 对于本练习。 当前`UnitPrice`设置为在文本框中的数据绑定表达式由于货币格式`EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`。 绑定将语句更改为`Bind("UnitPrice", "{0:n2}")`，其中将结果格式化为大量具有两位数的精度。 这可以通过声明性语法直接或通过单击从编辑数据绑定链接`EditUnitPrice`中的文本框中`UnitPrice`TemplateField 的`EditItemTemplate`（请参阅图 9 和 10）。


[![单击文本框的编辑数据绑定链接](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**图 9**： 单击文本框的编辑数据绑定链接 ([单击以查看实际尺寸的图像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))


[![在绑定语句中指定的格式说明符](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**图 10**： 指定中的格式说明符`Bind`语句 ([单击以查看实际尺寸的图像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))


进行此更改后，编辑界面中的格式化的价格包括作为组分隔符的逗号和句点作为小数分隔符，但保留关闭的货币符号。

> [!NOTE]
> `UnitPrice` `EditItemTemplate`不包括 RequiredFieldValidator，它使回发随之发生，更新的逻辑，开始。 但是，`RowUpdating`复制过来的事件处理程序*检查与插入、 更新和删除的事件相关联*教程包括可确保以编程方式检查`UnitPrice`提供。 请尝试删除此逻辑，请将其作为保留的或添加到 RequiredFieldValidator `UnitPrice` `EditItemTemplate`。


## <a name="step-4-summarizing-data-entry-problems"></a>步骤 4： 汇总数据条目问题

除了五个验证控件中，包括 ASP.NET [ValidationSummary 控件](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx)，其中显示`ErrorMessage`s 检测到无效的数据的那些验证控件。 此摘要数据可以显示为文本在网页上或通过模式，客户端的消息框。 让我们增强以包括一个汇总任何验证问题的客户端 messagebox 本教程。

若要完成此操作，请从工具箱中拖动到设计器拖动 ValidationSummary 控件。 验证控件的位置不真正有意义，因为我们要将其配置为仅显示摘要作为消息框。 在添加控件后, 设置其[ShowSummary 属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx)到`False`及其[ShowMessageBox 属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx)到`True`。 添加此元素后，客户端 messagebox 概述了任何验证错误。


[![客户端 Messagebox 中总结了验证错误](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**图 11**： 一个客户端的 Messagebox 中总结了验证错误 ([单击以查看实际尺寸的图像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>步骤 5： 将验证控件添加到说明`InsertItemTemplate`

所有这些对于本教程的余下是将验证控件添加到说明如何插入接口。 将验证控件添加到说明的模板的过程等同于步骤 3; 在读取器因此，我们将轻松处理通过此步骤中的任务。 正如我们做与 GridView `EditItemTemplate` s，我建议你重命名`ID`nondescript 从文本框中的 s`TextBox1`和`TextBox2`到`InsertProductName`和`InsertUnitPrice`。

添加到 RequiredFieldValidator `ProductName` `InsertItemTemplate`。 设置`ControlToValidate`到`ID`在模板中，文本框中的其`Text`属性设置为"\*"并将其`ErrorMessage`属性设置为"你必须提供该产品的名称"。

由于`UnitPrice`是添加新记录时，所需的此页，添加到 RequiredFieldValidator `UnitPrice` `InsertItemTemplate`，设置其`ControlToValidate`， `Text`，和`ErrorMessage`属性正确。 最后，将添加到 CompareValidator `UnitPrice` `InsertItemTemplate` ，配置其`ControlToValidate`， `Text`， `ErrorMessage`， `Type`， `Operator`，和`ValueToCompare`属性就像我们像使用`UnitPrice`的在 GridView 的 CompareValidator `EditItemTemplate`。

在添加这些验证控件之后, 无法添加到系统，如果未提供其名称或其的价格为负数或非法格式化新产品。


[![验证逻辑已添加到说明如何插入接口](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**图 12**： 验证逻辑已添加到说明如何插入接口 ([单击以查看实际尺寸的图像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>步骤 6： 分区到验证组的验证控件

我们的页面包含的验证控件的两个逻辑上不同集： 那些 GridView 对应于用于编辑的接口和说明所对应的那些的插入接口。 默认情况下，当发生回发*所有*检查了该页上的验证控件。 但是，编辑记录时我们不想要验证的说明的插入接口的验证控件。 图 13 展示我们当前境地，当用户编辑具有完全合法的值的产品时，单击更新将导致验证错误，因为插入接口中的名称和价格值为空。


[![更新产品会插入接口的验证控件导致到激发](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**图 13**： 更新产品会插入接口的验证控件导致到激发 ([单击以查看实际尺寸的图像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))


ASP.NET 2.0 中的验证控件可以分成验证组通过其`ValidationGroup`属性。 若要将组中的验证控件的一组相关联，只需设置其`ValidationGroup`为相同的值的属性。 对于我们的教程中，设置`ValidationGroup`到的 GridView TemplateFields 中的验证控件的属性`EditValidationControls`和`ValidationGroup`属性的说明的 TemplateFields 到`InsertValidationControls`。 这些更改可以直接在声明性的标记中，或通过属性窗口时使用设计器的编辑模板接口。

以及验证控件、 按钮和按钮相关控件在 ASP.NET 2.0 中还包括`ValidationGroup`属性。 验证组的验证程序检查的有效性仅回发引起的具有相同按钮`ValidationGroup`属性设置。 例如，顺序说明的插入按钮来触发`InsertValidationControls`我们需要将设置 CommandField 的验证组`ValidationGroup`属性`InsertValidationControls`（请参阅图 14）。 此外，设置 GridView CommandField 的`ValidationGroup`属性`EditValidationControls`。


[![说明如何的 InsertValidationControls CommandField 的 ValidationGroup 属性](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**图 14**： 设置说明的 CommandField 的`ValidationGroup`属性`InsertValidationControls`([单击以查看实际尺寸的图像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))


这些更改后的说明和 GridView 的 TemplateFields 和命令应显示类似于以下：

说明如何的 TemplateFields 和 CommandField

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

GridView CommandField 和 TemplateFields

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

此时编辑特定的验证控件仅在单击 GridView 的更新按钮时以及触发插入特定的验证控件激发仅当单击说明的插入按钮时，解决该问题由图 13 突出显示时。 但是，进行此更改我们 ValidationSummary 控件不再时显示输入无效数据。 ValidationSummary 控件还包含`ValidationGroup`属性和仅显示在其验证组这些验证控件的摘要信息。 因此，我们需要在此页中，一个用于将两个验证控件`InsertValidationControls`验证组，一个用于`EditValidationControls`。

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

与此添加我们的教程中已完成 ！

## <a name="summary"></a>摘要

BoundFields 可以同时插入和编辑接口提供的而不是可自定义接口。 通常情况下，我们想要添加到的编辑和插入界面可确保用户在合法格式中输入必需的输入的验证控件。 若要实现此目的，我们必须 BoundFields 转换 TemplateFields 并将验证控件添加到合适的模板。 在本教程中我们扩展中的示例*检查与插入、 更新和删除的事件相关联*教程中，向这两个说明如何添加验证控件的插入接口和 GridView编辑接口。 此外，我们已了解如何显示摘要验证信息使用 ValidationSummary 控件以及如何划分为不同的验证组页上的验证控件。

正如我们在本教程中看到的 TemplateFields 允许进行扩充以包括验证控件的编辑和插入的接口。 TemplateFields 还可以扩展以包括其他输入的 Web 控件，使文本框中，将替换为更适合的 Web 控件。 在我们的下一步教程，我们将了解如何将文本框控件替换数据绑定的 DropDownList 控件，这是理想时编辑外键 (如`CategoryID`或`SupplierID`中`Products`表)。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已沈 Shulok 和 Zack Jones。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
[下一页](customizing-the-data-modification-interface-vb.md)
