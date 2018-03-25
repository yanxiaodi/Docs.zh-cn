---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
title: "说明如何控件 (C#) 中使用 TemplateFields |Microsoft 文档"
author: rick-anderson
description: "提供的 GridView 相同 TemplateFields 功能，还提供个说明。 在本教程中我们将显示一个产品..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 83efb21f-b231-446e-9356-f4c6cbcc6713
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 8004937b758ee1bdb2a2df84c5ea40d47e89dd1c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="using-templatefields-in-the-detailsview-control-c"></a>说明如何控件 (C#) 中使用 TemplateFields
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_13_CS.exe)或[下载 PDF](using-templatefields-in-the-detailsview-control-cs/_static/datatutorial13cs1.pdf)

> 提供的 GridView 相同 TemplateFields 功能，还提供个说明。 在本教程中我们将使用包含 TemplateFields 说明一次显示一个产品。


## <a name="introduction"></a>介绍

TemplateField 提供更高的灵活地呈现数据比 BoundField、 CheckBoxField、 HyperLinkField，以及其他数据字段控件。 在[以前一教程](using-templatefields-in-the-gridview-control-cs.md)我们讨论在使用中 GridView TemplateField:

- 将一个列中显示多个数据字段值。 具体而言，同时`FirstName`和`LastName`字段已合并为一个 GridView 列。
- 使用备用的 Web 控件表示数据字段值。 我们已了解如何显示`HiredDate`值使用日历控件。
- 显示基于基础数据的状态信息。 虽然`Employees`表不包含的列，返回的员工已在作业的天数，可在使用 TemplateField 和格式设置方法与前面的教程中在 GridView 的示例中显示此类信息。

提供的 GridView 相同 TemplateFields 功能，还提供个说明。 在本教程中我们将使用包含两个 TemplateFields 说明一次显示一个产品。 第一个 TemplateField 将合并`UnitPrice`， `UnitsInStock`，和`UnitsOnOrder`到一个说明如何行的数据字段。 第二个 TemplateField 将显示的值`Discontinued`字段，但将使用格式设置方法来显示"是"如果`Discontinued`是`true`，，否则为"否"。


[![两个 TemplateFields 用于自定义的显示](using-templatefields-in-the-detailsview-control-cs/_static/image2.png)](using-templatefields-in-the-detailsview-control-cs/_static/image1.png)

**图 1**： 两个 TemplateFields 用于自定义显示 ([单击以查看实际尺寸的图像](using-templatefields-in-the-detailsview-control-cs/_static/image3.png))


让我们进入正题！

## <a name="step-1-binding-the-data-to-the-detailsview"></a>步骤 1： 将数据绑定到说明

如前面的教程中, 所述，使用的 TemplateFields 通常很容易通过创建包含刚 BoundFields 说明如何控制开始然后添加新 TemplateFields 或将现有 BoundFields 转换为作为 TemplateFields 时需要. 因此，通过将说明如何添加到设计器中翻页，并将其绑定到返回的产品列表 ObjectDataSource 开始本教程。 这些步骤将为每个产品的非布尔值字段和一个布尔值字段 （中止） CheckBoxField 与 BoundFields 创建说明。

打开`DetailsViewTemplateField.aspx`页上，并将说明如何拖到设计器工具箱。 从说明的智能标记选择添加一个新的 ObjectDataSource 控件调用`ProductsBLL`类的`GetProducts()`方法。


[![添加一个新的 ObjectDataSource 控件调用 GetProducts() 方法](using-templatefields-in-the-detailsview-control-cs/_static/image5.png)](using-templatefields-in-the-detailsview-control-cs/_static/image4.png)

**图 2**： 添加新的 ObjectDataSource 控件该 Invoke`GetProducts()`方法 ([单击以查看实际尺寸的图像](using-templatefields-in-the-detailsview-control-cs/_static/image6.png))


为此报表中删除`ProductID`， `SupplierID`， `CategoryID`，和`ReorderLevel`BoundFields。 接下来，重新排序 BoundFields 以便`CategoryName`和`SupplierName`BoundFields 出现后立即`ProductName`BoundField。 随意调整`HeaderText`属性和格式设置属性作为你 BoundFields 认为适合。 如与 GridView，通过字段对话框 （可通过单击中说明的智能标记的编辑字段链接访问） 或通过声明性语法，可以执行这些 BoundField 级别编辑。 最后，清除说明的`Height`和`Width`以便允许说明的属性值控件扩展基于显示的数据，并检查的智能标记中的启用分页复选框。

进行这些更改后，说明如何控制的声明性标记应类似于以下：


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample1.aspx)]

花些时间查看通过浏览器的页。 此时你应看到列出的单个产品 （牛奶） 显示产品的名称、 类别、 供应商、 价格、 库存量、 订购，量和其停用的状态的行。


[![显示使用 BoundFields 一系列的产品的详细信息](using-templatefields-in-the-detailsview-control-cs/_static/image8.png)](using-templatefields-in-the-detailsview-control-cs/_static/image7.png)

**图 3**： 显示使用 BoundFields 系列的产品的详细信息 ([单击以查看实际尺寸的图像](using-templatefields-in-the-detailsview-control-cs/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>步骤 2： 将价格、 库存，量和订购量合并为一个行

说明如何具有对应的行`UnitPrice`， `UnitsInStock`，和`UnitsOnOrder`字段。 我们可以这些数据字段组合成单个行与为 TemplateField 通过添加新 TemplateField 或转换其中一个现有`UnitPrice`， `UnitsInStock`，和`UnitsOnOrder`转换为 TemplateField BoundFields。 虽然我个人更喜欢转换现有 BoundFields，让我们通过添加新 TemplateField 练习。

通过单击编辑字段链接中说明的智能标记，以打开字段对话框中启动。 接下来，添加新 TemplateField 并设置其`HeaderText`属性和移动新 TemplateField 以便它位于上面的"价格和清单" `UnitPrice` BoundField。


[![将新 TemplateField 添加到说明如何控制](using-templatefields-in-the-detailsview-control-cs/_static/image11.png)](using-templatefields-in-the-detailsview-control-cs/_static/image10.png)

**图 4**： 将新 TemplateField 添加到说明如何控制 ([单击以查看实际尺寸的图像](using-templatefields-in-the-detailsview-control-cs/_static/image12.png))


由于此新 TemplateField 将包含在当前显示的值`UnitPrice`， `UnitsInStock`，和`UnitsOnOrder`BoundFields，让我们将其删除。

此步骤的最后一个任务是定义`ItemTemplate`的价格和清单 TemplateField，可以标记来完成通过接口在设计器或手动编辑通过控件的声明性语法的说明的模板。 与 GridView，可以通过单击智能标记中的编辑模板链接访问说明的模板编辑接口。 从此处你可以选择要从下拉列表编辑然后从工具箱中添加任何 Web 控件模板。

本教程中，启动通过将一个标签控件添加到的价格和清单 TemplateField `ItemTemplate`。 接下来，单击从标签 Web 控件的智能标记的编辑数据绑定链接并将绑定`Text`属性`UnitPrice`字段。


[![将标签的文本属性绑定到单价数据字段](using-templatefields-in-the-detailsview-control-cs/_static/image14.png)](using-templatefields-in-the-detailsview-control-cs/_static/image13.png)

**图 5**： 绑定的标签`Text`属性`UnitPrice`数据字段 ([单击以查看实际尺寸的图像](using-templatefields-in-the-detailsview-control-cs/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>作为一种货币格式价格

添加此元素后，标签 Web 控件价格和清单 TemplateField 现在将显示只包括所选产品的价格。 图 6 显示我们的进度的屏幕截图为止通过浏览器查看时。


[![价格和清单 TemplateField 显示价格](using-templatefields-in-the-detailsview-control-cs/_static/image17.png)](using-templatefields-in-the-detailsview-control-cs/_static/image16.png)

**图 6**: 价格和清单 TemplateField 显示价格 ([单击以查看实际尺寸的图像](using-templatefields-in-the-detailsview-control-cs/_static/image18.png))


请注意该产品的价格未格式化为货币。 使用 BoundField 格式设置有可能通过设置`HtmlEncode`属性`false`和`DataFormatString`属性`{0:formatSpecifier}`。 对于为 TemplateField，但是，格式设置的所有说明操作必须指定在数据绑定的语法中或通过在应用程序的代码 （如 ASP.NET 页的代码隐藏类） 中的某一位置定义的格式设置方法使用。

若要指定标签 Web 控件中使用的数据绑定语法的格式，返回到数据绑定对话框中，通过单击标签的智能标记中的编辑数据绑定链接。 可以在格式下拉列表中直接键入的格式设置的说明进行操作，也可以选择定义的格式字符串之一。 与 BoundField 的类似`DataFormatString`属性，格式使用指定的`{0:formatSpecifier}`。

有关`UnitPrice`字段的使用货币格式指定通过选择相应的下拉列表值或通过键入`{0:C}`手动。


[![设置价格作为一种货币的格式](using-templatefields-in-the-detailsview-control-cs/_static/image20.png)](using-templatefields-in-the-detailsview-control-cs/_static/image19.png)

**图 7**： 设置为货币价格的格式 ([单击以查看实际尺寸的图像](using-templatefields-in-the-detailsview-control-cs/_static/image21.png))


以声明方式，到第二个参数指示的格式规范`Bind`或`Eval`方法。 通过声明性的标记中的以下数据绑定表达式中的设计器结果只需做的设置：


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>将剩余的数据字段添加到 TemplateField

此时我们已在其中显示和格式化`UnitPrice`数据字段中的价格和清单 TemplateField，但仍需要显示`UnitsInStock`和`UnitsOnOrder`字段。 让我们来显示这些节点在行下方价格，并在括号中。 从设计器中的模板编辑界面，可以通过在模板中将光标并只需键入要显示的文本添加这种标记。 或者，可以直接在声明性语法中输入此标记。

添加静态标记、 标签 Web 控件和数据绑定语法，以便的价格和清单 TemplateField 显示的价格和清单信息如下所示：

*单价*  
(**库存量 / Order:** *UnitsInStock* / *UnitsOnOrder*)

执行此任务后你说明如何声明性标记应看起来类似于以下：


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample3.aspx)]

使用这些更改，我们已合并为单个说明行的价格和清单信息。


[![单个行中显示的价格和清单信息](using-templatefields-in-the-detailsview-control-cs/_static/image23.png)](using-templatefields-in-the-detailsview-control-cs/_static/image22.png)

**图 8**: 价格和清单信息显示在单个行 ([单击以查看实际尺寸的图像](using-templatefields-in-the-detailsview-control-cs/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>步骤 3： 自定义的中断的字段信息

`Products`表的`Discontinued`列是一个位值，指示产品是否已被删除。 布尔值字段中，当绑定到数据源控件的说明 （或 GridView），如`Discontinued`，而非布尔值字段，如作为 CheckBoxFields 实现`ProductID`， `ProductName`，依此类推，作为实现BoundFields。 CheckBoxField 将呈现为一个已禁用的复选框，如果数据字段的值可以为 True，并且未选中否则下将选中。

而不是显示 CheckBoxField 我们可能想要改为显示文本，该值指示已停止使用该产品。 若要完成此我们无法从说明如何删除 CheckBoxField，然后再添加 BoundField 其`DataField`属性设置为`Discontinued`。 需要一段时间来执行此操作。 此更改之后说明如何显示文本"True"停产的产品和"False"仍处于活动状态的产品。


[![字符串 True 和 False 用于显示已停止使用的状态](using-templatefields-in-the-detailsview-control-cs/_static/image26.png)](using-templatefields-in-the-detailsview-control-cs/_static/image25.png)

**图 9**: 字符串 True 和使用 False 以显示已中断状态 ([单击以查看实际尺寸的图像](using-templatefields-in-the-detailsview-control-cs/_static/image27.png))


假设我们不想要的字符串"True"或"False"改为使用，但"是"和"否"。 可以使用为 TemplateField 和格式设置方法的帮助执行这种自定义。 格式设置方法可能需要在任意数量的输入参数，但必须返回 HTML （作为字符串） 将插入该模板。

添加到一个格式设置方法`DetailsViewTemplateField.aspx`页的代码隐藏类名为`DisplayDiscontinuedAsYESorNO`用于接受一个布尔值作为输入参数并返回一个字符串。 如前面的教程中，此方法中所述*必须*标记为`protected`或`public`才能从该模板可访问。


[!code-csharp[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample4.cs)]

此方法检查输入的参数 (`discontinued`) 并返回"是"，如果它是`true`，否则为"否"。

> [!NOTE]
> 在检查中以前的教程回想一下，我们已传递中可能包含的数据字段的格式设置方法`NULL`s，因此需要检查如果员工的`HiredDate`属性值有一个数据库`NULL`值之前访问`EmployeesRow`的`HiredDate`属性。 此类检查，则不需要此处自`Discontinued`列永远不会有数据库`NULL`分配的值。 此外，这就是原因的方法可以接受一个布尔值输入参数，而不是无需接受`ProductsRow`实例或类型的参数`object`。


使用此完整的格式设置方法，剩下的就是无法调用它从 TemplateField `ItemTemplate`。 若要创建删除的 TemplateField `Discontinued` BoundField 和添加新 TemplateField 或转换`Discontinued`转换为 TemplateField BoundField。 然后，从声明性标记视图中，编辑的 TemplateField 以使其包含只需调用 ItemTemplate`DisplayDiscontinuedAsYESorNO`方法，同时传入的当前值`ProductRow`实例的`Discontinued`属性。 这可以通过访问`Eval`方法。 具体而言，TemplateField 标记应如下所示：


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample5.aspx)]

这将导致`DisplayDiscontinuedAsYESorNO`说明，在呈现时要调用方法并传入`ProductRow`实例的`Discontinued`值。 由于`Eval`方法返回类型的值`object`，但`DisplayDiscontinuedAsYESorNO`方法需要输入的参数的类型`bool`，我们强制转换`Eval`方法返回值到`bool`。 `DisplayDiscontinuedAsYESorNO`方法然后将返回"是"或"否"值根据它接收。 返回的值是此说明中显示的内容行 （请参阅图 10）。


[![是或否的值为现在已中断行中显示](using-templatefields-in-the-detailsview-control-cs/_static/image29.png)](using-templatefields-in-the-detailsview-control-cs/_static/image28.png)

**图 10**: 是或否的值为现在已中断行中所示 ([单击以查看实际尺寸的图像](using-templatefields-in-the-detailsview-control-cs/_static/image30.png))


## <a name="summary"></a>摘要

说明如何控件中的 TemplateField 允许灵活地显示数据，同时提供的其他字段控件而适合的情况下使用更高其中：

- 多个数据字段都需要一个 GridView 列中显示
- 数据是最表示使用的 Web 控件，而不是纯文本
- 输出取决于基础数据，例如显示元数据或在重新格式化数据

虽然 TemplateFields 允许的更高程度的灵活性在呈现的说明的基础数据，说明如何输出仍感觉有点 boxy 每个字段呈现为 HTML 中的一行`<table>`。

FormView 控制提供更高程度的灵活地配置呈现的输出。 FormView 不包含字段，但而是仅一系列模板 (`ItemTemplate`， `EditItemTemplate`， `HeaderTemplate`，依次类推)。 我们将了解如何使用 FormView 在我们的下一教程中实现的呈现布局的更多控制。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Dan Jagers。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](using-templatefields-in-the-gridview-control-cs.md)
[下一页](using-the-formview-s-templates-cs.md)
