---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
title: "批处理插入 (VB) |Microsoft 文档"
author: rick-anderson
description: "了解如何在单个操作中插入多个数据库记录。 在用户界面层中，我们将扩展以允许用户输入多个 n GridView..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 48e2a4ae-77ca-4208-a204-c38c690ffb59
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
msc.type: authoredcontent
ms.openlocfilehash: 3f09b5fd86c1cc6641fb42a466b07da161c1dd35
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="batch-inserting-vb"></a>批处理插入 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_VB.zip)或[下载 PDF](batch-inserting-vb/_static/datatutorial66vb1.pdf)

> 了解如何在单个操作中插入多个数据库记录。 在用户界面层我们扩展 GridView 以允许用户输入多个新记录。 在数据访问层中我们将包装在事务以确保所有插入都成功或回滚所有的插入中的多个插入操作。


## <a name="introduction"></a>介绍

在[批处理更新](batch-updating-vb.md)我们查看自定义要提供多个记录中可编辑的接口的 GridView 控件的教程。 用户访问的页面无法进行的一系列更改，然后，使用一次按钮单击，执行批处理更新。 对于用户通常更新一个定位中的多个记录的情况下，此类接口可以保存无数下鼠标和键盘鼠标上下文切换到默认值进行比较时每行都首先返回进行了研究的编辑功能[插入、 更新和删除数据的概述](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)教程。

添加记录时，还可以应用这一概念。 假设，此处 Northwind traders 公司我们通常会收到货运从供应商所包含的一种特定类别的产品数量。 例如，我们可能会从东京 Traders 收到一批的六个不同 tea 和 coffee 产品。 如果用户输入说明如何控制通过一次一个的六个产品，他们将需要反复选择相同的值的许多： 它们将需要选择的相同类别 （饮料），同一个供应商 (东京 Traders)、 相同废止的值 （False)，并对顺序的值 (0) 相同的单位。 重复数据该项不是仅耗时，但很容易导致错误。

我们可以轻而易举地创建批处理插入的界面，让用户选择的供应商和类别一次，输入一系列产品名称和单位价格，然后单击一个按钮以向数据库添加新的产品 （请参见图 1）。 添加每个产品时，其`ProductName`和`UnitPrice`数据字段被分配的文本框中输入的值时其`CategoryID`和`SupplierID`值从顶部 fo 窗体在 DropDownLists 分配值。 `Discontinued`和`UnitsOnOrder`值设置为的硬编码值`False`和 0 分别。


[![批处理插入接口](batch-inserting-vb/_static/image2.png)](batch-inserting-vb/_static/image1.png)

**图 1**: 批处理插入接口 ([单击以查看实际尺寸的图像](batch-inserting-vb/_static/image3.png))


在本教程中我们将创建实现批处理插入接口中图 1 所示的页。 作为与前面的两个教程，我们将自动换行插入事务以确保原子性的作用域内。 让我们来开始 ！

## <a name="step-1-creating-the-display-interface"></a>步骤 1： 创建显示接口

本教程将包含的是单个页面中划分为两个区域： 一个显示区域和插入的区域。 显示接口，我们将创建在此步骤中，在一个 GridView 显示产品，其中包括标题为进程，产品交付的按钮。 单击此按钮时，显示接口将被替换为插入界面、 中图 1 所示。 在添加产品后返回从装运的显示界面或单击取消按钮。 我们将在步骤 2 中创建插入接口。

当创建页有两个接口，一次只有其中一个是可见的每个接口通常放在[面板 Web 控件](http://www.w3schools.com/aspnet/control_panel.asp)，它用作其他控件的容器。 因此，我们的页面有两个面板控件一个，每个接口。

首先打开`BatchInsert.aspx`页面`BatchData`文件夹，然后拖动一个面板从工具箱中拖动到设计器 （请参见图 2）。 设置面板 s`ID`属性`DisplayInterface`。 当将面板添加到设计器中，其`Height`和`Width`属性设置为 50px 和 125px，分别。 清理这些属性值从属性窗口。


[![从工具箱中拖动到设计器中拖动面板](batch-inserting-vb/_static/image5.png)](batch-inserting-vb/_static/image4.png)

**图 2**： 从工具箱中拖动到设计器中拖动面板 ([单击以查看实际尺寸的图像](batch-inserting-vb/_static/image6.png))


接下来，将按钮和 GridView 控件拖动到面板。 设置按钮 s`ID`属性`ProcessShipment`及其`Text`处理产品装运到的属性。 设置 GridView s`ID`属性`ProductsGrid`和从其智能标记，请将其绑定到名为新 ObjectDataSource `ProductsDataSource`。 配置对象数据源将从其数据拉`ProductsBLL`类的`GetProducts`方法。 由于此 GridView 仅用于显示数据，在更新中，INSERT、 设置的下拉列表，删除选项卡添加到 （无）。 单击完成以完成配置数据源向导。


[![显示从 ProductsBLL 类的 GetProducts 方法返回的数据](batch-inserting-vb/_static/image8.png)](batch-inserting-vb/_static/image7.png)

**图 3**： 显示从数据返回`ProductsBLL`类 s`GetProducts`方法 ([单击以查看实际尺寸的图像](batch-inserting-vb/_static/image9.png))


[![在更新中，INSERT、 设置的下拉列表和删除选项卡到 （无）](batch-inserting-vb/_static/image11.png)](batch-inserting-vb/_static/image10.png)

**图 4**： 设置的下拉列表中更新、 插入和删除选项卡为 （无） ([单击以查看实际尺寸的图像](batch-inserting-vb/_static/image12.png))


完成对象数据源向导后，Visual Studio 将添加 BoundFields 和产品数据字段 CheckBoxField。 删除所有但`ProductName`， `CategoryName`， `SupplierName`， `UnitPrice`，和`Discontinued`字段。 随意调整美观的任何自定义。 我决定要设置格式`UnitPrice`作为货币值的字段重新排序字段，并且重命名的某些字段`HeaderText`值。 此外配置 GridView 包括分页和排序支持通过检查 GridView s 智能标记中的启用分页和启用排序复选框。

后添加面板、 按钮、 GridView 和 ObjectDataSource 控件和自定义的 GridView 的字段，你页面 s 声明性标记应看起来类似于以下：


[!code-aspx[Main](batch-inserting-vb/samples/sample1.aspx)]

请注意，在开始和结束中显示的标记的按钮和 GridView`<asp:Panel>`标记。 由于这些控件是在`DisplayInterface`面板中，我们可以隐藏起来，方法只需设置面板 s`Visible`属性`False`。 步骤 3 考察以编程方式更改面板的`Visible`至一个按钮的响应中的属性单击以显示同时隐藏了另一个接口。

花些时间查看我们通过浏览器的进度。 如图 5 所示，你应看到一个 GridView，一次列出的产品十个以上的进程，产品交付按钮。


[![GridView 列出的产品，并提供对进行排序和分页功能](batch-inserting-vb/_static/image14.png)](batch-inserting-vb/_static/image13.png)

**图 5**: GridView 列出的产品和提供排序和分页功能 ([单击以查看实际尺寸的图像](batch-inserting-vb/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>步骤 2： 创建插入接口

使用显示界面完成时，我们已准备好创建插入重新接口。 对于本教程，让我们来创建一个插入的接口来提示的单个的供应商和类别值，然后使用户输入最多五个产品名称和单位价格值。 通过此接口，用户可以添加一到五个新产品的所有共享相同的类别和供应商，但具有唯一的产品名称和产品价格。

通过从工具箱中拖动到设计器中，将其放在现有下面拖动面板`DisplayInterface`面板。 设置`ID`属性的新添加到面板`InsertingInterface`并设置其`Visible`属性`False`。 我们将添加代码设置`InsertingInterface`面板 s`Visible`属性`True`在步骤 3 中。 此外清除面板 s`Height`和`Width`属性值。

接下来，我们需要创建插入界面进来图 1 所示。 此接口可创建通过各种 HTML 的方法，但我们将使用一个非常简单： 一个四列七行的表。

> [!NOTE]
> 当输入 HTML 标记时，`<table>`元素，但我更喜欢使用的源视图。 Visual Studio 具有用于添加的工具时`<table>`通过设计器的元素，设计器似乎所有太愿意注入的未经请求`style`向的标记的设置。 一旦我已创建`<table>`标记中，我通常返回到设计器添加 Web 控件并设置其属性。 创建包含预先确定的列和行的表时我更喜欢使用静态 HTML 而不是[表 Web 控件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx)因为任何放置表 Web 控件内的 Web 控件仅可以使用`FindControl("controlID")`模式。 我，但是，使用表 Web 控件动态调整大小的表 （其行或列基于某些数据库或用户指定的条件的），因为表 Web 控件可以以编程方式构造。


输入中的以下标记`<asp:Panel>`标记`InsertingInterface`面板：


[!code-html[Main](batch-inserting-vb/samples/sample2.html)]

这`<table>`标记不包含任何 Web 控件，但我们将稍后添加它们。 请注意，每个`<tr>`元素包含特定的 CSS 类设置：`BatchInsertHeaderRow`标头行的供应商和类别 DropDownLists 的目的地;`BatchInsertFooterRow`的目的地装运和取消按钮中的添加产品; 的页脚行和交替`BatchInsertRow`和`BatchInsertAlternatingRow`的产品和单元将包含行的值价格 TextBox 控件。 我已创建中的相应 CSS 类`Styles.css`文件以便插入接口外观的外观类似于 GridView 和说明如何控制我们已在这些教程中使用。 下面显示了这些 CSS 类。


[!code-css[Main](batch-inserting-vb/samples/sample3.css)]

输入此标记，返回到设计视图。 这`<table>`应显示为一个四列七行的表在设计器中，如图 6 所示。


[![插入接口由四个列七行表的](batch-inserting-vb/_static/image17.png)](batch-inserting-vb/_static/image16.png)

**图 6**: 插入接口由四个列七行表的 ([单击以查看实际尺寸的图像](batch-inserting-vb/_static/image18.png))


我们现在重新准备好向插入接口添加 Web 控件。 将两个 DropDownLists 从工具箱拖到相应的单元格中的表中其中一个供应商，另一个用于类别。

设置供应商的 DropDownList s`ID`属性`Suppliers`并将其绑定到名为新 ObjectDataSource `SuppliersDataSource`。 配置新的对象数据源，以检索从其数据`SuppliersBLL`类的`GetSuppliers`方法和组更新选项卡上为 (None) s 下拉列表。 单击完成以完成向导。


[![配置对象数据源以使用 SuppliersBLL 类的 GetSuppliers 方法](batch-inserting-vb/_static/image20.png)](batch-inserting-vb/_static/image19.png)

**图 7**： 配置使用 ObjectDataSource`SuppliersBLL`类 s`GetSuppliers`方法 ([单击以查看实际尺寸的图像](batch-inserting-vb/_static/image21.png))


具有`Suppliers`DropDownList 显示`CompanyName`数据字段并使用`SupplierID`数据字段作为其`ListItem`的值。


[![显示 CompanyName 数据字段并使用供应商 Id 作为值](batch-inserting-vb/_static/image23.png)](batch-inserting-vb/_static/image22.png)

**图 8**： 显示`CompanyName`数据字段并使用`SupplierID`作为值 ([单击以查看实际尺寸的图像](batch-inserting-vb/_static/image24.png))


将第二个 DropDownList`Categories`并将其绑定到名为新 ObjectDataSource `CategoriesDataSource`。 配置`CategoriesDataSource`ObjectDataSource 使用`CategoriesBLL`类的`GetCategories`方法; 设置下拉列表中的更新和删除选项卡添加到 （无） 然后单击完成以完成向导。 最后，具有 DropDownList 显示`CategoryName`数据字段并使用`CategoryID`作为值。

已添加并绑定到适当配置 ObjectDataSources 这些两个 DropDownLists 后，你的屏幕应类似于图 9。


[![标头行现在包含的供应商和类别 DropDownLists](batch-inserting-vb/_static/image26.png)](batch-inserting-vb/_static/image25.png)

**图 9**: 标头行现在包含`Suppliers`和`Categories`DropDownLists ([单击以查看实际尺寸的图像](batch-inserting-vb/_static/image27.png))


现在，我们需要创建文本框中，若要收集的名称和每个新产品的价格。 将 TextBox 控件从工具箱中拖动到设计器为每个五个产品名称和价格行。 设置`ID`到文本框中的属性`ProductName1`， `UnitPrice1`， `ProductName2`， `UnitPrice2`， `ProductName3`， `UnitPrice3`，依次类推。

在每个单位价格文本框中，设置后添加 CompareValidator`ControlToValidate`到适当的属性`ID`。 此外设置`Operator`属性`GreaterThanEqual`，`ValueToCompare`为 0，和`Type`到`Currency`。 这些设置指示 CompareValidator 以确保价格，如果输入，将有效的货币值大于或等于零。 设置`Text`属性\*，和`ErrorMessage`价格必须是大于或等于零。 此外，请省略任何货币符号。

> [!NOTE]
> 插入的接口不包括任何 RequiredFieldValidator 控件，即使`ProductName`字段`Products`数据库表不允许`NULL`值。 这是因为我们想要让用户输入最多五个产品。 例如，如果用户已为前三个行中提供的产品名称和单位价格，将最后两行保留为空白，d 我们只需添加三个新产品到系统。 由于`ProductName`是必需的但是，我们将需要以编程方式检查以确保如果单价输入提供相应的产品名称值。 例如，我们将介绍在步骤 4 中的此检查。


当验证的用户的输入，CompareValidator 报告无效的数据，如果值包含货币符号。 添加每个单位价格文本框中以用作一个视觉提示，指示用户输入价格时忽略的货币符号的前面的 $。

最后，添加内的 ValidationSummary 控件`InsertingInterface`面板中，设置其`ShowMessageBox`属性`True`及其`ShowSummary`属性`False`。 使用这些设置，如果用户输入无效的单位价格值，有问题的 TextBox 控件旁边将出现一个星号，并且 ValidationSummary 将显示一个显示我们在前面指定的错误消息的客户端 messagebox。

此时，你的屏幕应类似于图 10。


[![插入的接口现在包含文本框中的产品名称和产品价格](batch-inserting-vb/_static/image29.png)](batch-inserting-vb/_static/image28.png)

**图 10**: 插入接口现在包含文本框中的产品名称和价格 ([单击以查看实际尺寸的图像](batch-inserting-vb/_static/image30.png))


接下来，我们需要将添加产品从装运和取消按钮添加到页脚行。 将两个按钮控件从工具箱插入的接口中，页脚设置按钮`ID`属性设置为`AddProducts`和`CancelButton`和`Text`属性以分别从装运和取消，添加产品。 此外，设置`CancelButton`控件 s`CausesValidation`属性`false`。

最后，我们需要添加标签 Web 控件将显示两个接口的状态消息。 例如，当用户成功添加一批新的产品，我们想要返回到显示接口并显示一条确认消息。 如果，但是，用户可为新产品但不关闭关闭产品名称中提供价格，我们需要显示一条警告消息，因为`ProductName`字段是必填。 因为我们需要此消息，以显示为这两个接口，则将其放置在外部面板页的顶部。

将标签 Web 控件从工具箱拖到设计器中页的顶部。 设置`ID`属性`StatusLabel`，清除出`Text`属性，同时设置`Visible`和`EnableViewState`属性设置为`False`。 如我们所看到在前面的教程中，设置`EnableViewState`属性`False`使我们能够以编程方式更改标签的属性值并将它们返回到各自的默认值在后续回发时自动还原。 这简化了显示状态消息以响应在后续回发时消失某些用户操作的代码。 最后，设置`StatusLabel`控件 s`CssClass`中定义的警告，这是 CSS 类的名称的属性`Styles.css`大型、 斜体、 粗体和红色字体中显示文本。

图 11 显示 Visual Studio 设计器添加标签并将其配置之后。


[![将上面两个面板控件 StatusLabel 控件放](batch-inserting-vb/_static/image32.png)](batch-inserting-vb/_static/image31.png)

**图 11**： 位置`StatusLabel`控件上面两个面板控件 ([单击以查看实际尺寸的图像](batch-inserting-vb/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>步骤 3： 显示和插入接口之间切换

此时我们已经完成标记用于我们显示和插入接口，但我们重新仍会余下两个任务：

- 在显示之间切换，并将插入接口
- 在运送到数据库中添加产品

目前，显示接口是可见，但插入接口处于隐藏状态。 这是因为`DisplayInterface`面板 s`Visible`属性设置为`True`（默认值），而`InsertingInterface`面板 s`Visible`属性设置为`False`。 我们只需切换 s 的每个控件的两个接口之间进行切换`Visible`属性值。

我们想要从移动显示接口到插入接口过程，产品交付按钮。 因此，为此按钮 s 创建事件处理程序`Click`包含下面的代码的事件：


[!code-vb[Main](batch-inserting-vb/samples/sample4.vb)]

此代码只需隐藏`DisplayInterface`面板并显示`InsertingInterface`面板。

接下来，为插入的界面中的装运和取消按钮控件中的添加产品创建事件处理程序。 单击其中任一个按钮时，我们需要还原到的显示接口。 创建`Click`两个事件处理程序按钮控件，以便它们调用`ReturnToDisplayInterface`，我们将稍后添加的方法。 除了隐藏`InsertingInterface`面板和显示`DisplayInterface`面板中，`ReturnToDisplayInterface`方法需要 Web 控件返回到其预编辑状态。 这涉及设置 DropDownLists`SelectedIndex`属性设置为 0 和清除`Text`文本框中控件的属性。

> [!NOTE]
> 请考虑可能会出现什么如果我们没有 t 返回到显示接口之前将控件恢复到其预编辑状态。 用户可能单击处理产品装运按钮，从装运，输入产品，然后单击添加产品装运。 这将添加的产品，并将用户返回到显示接口。 此时，用户可能想要添加另一个运送送达。 单击它们将返回到插入接口，但 DropDownList 处理产品装运按钮时文本框中的值，并选择仍将使用其以前的值填充。


[!code-vb[Main](batch-inserting-vb/samples/sample5.vb)]

同时`Click`事件处理程序只需调用`ReturnToDisplayInterface`方法，尽管我们将从装运返回到添加产品`Click`步骤 4 中的事件处理程序，然后添加代码来保存产品。 `ReturnToDisplayInterface`通过返回启动`Suppliers`和`Categories`DropDownLists 其第一个选项。 两个常量`firstControlID`和`lastControlID`标记的起始和结束命名接口中插入文本框中，并在的界限内使用的的产品名称和单价中使用的控件索引值`For`设置的循环`Text`文本框中控件的属性以返回一个空字符串。 最后，面板`Visible`属性被重置，以便插入界面隐藏和显示的显示界面。

需要一段时间来测试此页在浏览器。 首先访问页时你应看到显示接口，如图 5 中所示。 单击处理产品装运按钮。 将回发页面，你现在应该看到插入接口，如图 12 中所示。 单击任一添加中的产品装运或取消按钮返回到显示接口。

> [!NOTE]
> 在查看插入的接口时，需要一段时间来测试 CompareValidators 上单价文本框。 你应看到一个警告时单击具有无效的货币值的装运按钮或价格与小于零的值中的添加产品的客户端 messagebox。


[![单击处理产品装运按钮后显示的插入界面](batch-inserting-vb/_static/image35.png)](batch-inserting-vb/_static/image34.png)

**图 12**： 单击处理产品装运按钮后显示插入接口 ([单击以查看实际尺寸的图像](batch-inserting-vb/_static/image36.png))


## <a name="step-4-adding-the-products"></a>步骤 4： 添加产品

所有剩余本教程是将产品保存到的数据库中添加产品中，从装运按钮的`Click`事件处理程序。 这可以通过创建`ProductsDataTable`和添加`ProductsRow`为每个提供的产品名称的实例。 一次这些`ProductsRow`中添加了我们将调用`ProductsBLL`类 s`UpdateWithTransaction`方法并传入`ProductsDataTable`。 回想一下，`UpdateWithTransaction`方法，创建返回[在事务中包装数据库修改](wrapping-database-modifications-within-a-transaction-vb.md)教程，则传递`ProductsDataTable`到`ProductsTableAdapter`s`UpdateWithTransaction`方法。 在这里，ADO.NET 事务启动和 TableAdatper 问题`INSERT`到每个添加的数据库的语句`ProductsRow`数据表中。 假设所有产品都添加未生成错误，则提交此事务，否则它将回滚。

装运按钮 s 中的添加产品代码`Click`事件处理程序还需要执行一些错误检查。 因为在插入界面中使用任何 RequiredFieldValidators 不不存在，则用户可以输入价格的产品时省略其名称。 由于产品的名称是必需的如果此类条件展开我们需要向用户发出警报并不继续执行插入操作。 完整`Click`事件处理程序代码之后：


[!code-vb[Main](batch-inserting-vb/samples/sample6.vb)]

事件处理程序首先确保`Page.IsValid`属性返回的值`True`。 如果它返回`False`、 然后这意味着一个或多个 CompareValidators 是否正报告无效的数据; 在这种情况下我们不希望尝试插入的输入的产品或我们将得到的异常时尝试分配的用户输入的单位价格将值为`ProductsRow`s`UnitPrice`属性。

下一步、 新`ProductsDataTable`创建实例 (`products`)。 A`For`循环用于循环访问产品名称和单价文本框中和`Text`属性读取到的本地变量`productName`和`unitPrice`。 如果用户输入的值的单位价格而非相应的产品名称，`StatusLabel`显示消息，如果你提供一个单元价格你还必须包含产品的名称并退出事件处理程序。

如果已提供的产品名称，一个新`ProductsRow`使用创建实例`ProductsDataTable`s`NewProductsRow`方法。 此新`ProductsRow`实例 s`ProductName`属性设置为当前产品名称文本框中时`SupplierID`和`CategoryID`属性分配给`SelectedValue`DropDownLists 插入接口 s 标头中的属性。 如果用户输入的产品的价格一个值，则将它分配给`ProductsRow`实例 s`UnitPrice`属性; 否则，该属性是左未分配状态，这将导致`NULL`值`UnitPrice`数据库中。 最后，`Discontinued`和`UnitsOnOrder`属性分配给硬编码值`False`和 0 分别。

为分配了属性后`ProductsRow`实例添加到`ProductsDataTable`。

在完成`For`循环中，我们将检查是否已添加的任何产品。 用户可能，在所有情况下，单击在进入任何产品名称或价格之前的装运中的添加产品。 如果没有至少一个产品中的`ProductsDataTable`、`ProductsBLL`类的`UpdateWithTransaction`调用方法。 接下来，数据重新绑定到`ProductsGrid`GridView，以便在显示界面中将显示新添加的产品。 `StatusLabel`更新以显示一条确认消息和`ReturnToDisplayInterface`隐藏插入接口和显示的显示接口的调用。

如果输入的产品，插入的接口都显示但没有产品已添加的消息。 请输入产品名称和显示在文本框中的单位价格。

图 13、 14 中和 15 显示插入和在操作中显示接口。 在图 13 中，用户输入没有相应的产品名称的单位价格值。 图 14 显示了显示接口三次新产品已添加成功，而图 15 将显示两个新添加的产品中 GridView （位于前一页的第三个）。


[![产品名称是所需时输入的单位价格](batch-inserting-vb/_static/image38.png)](batch-inserting-vb/_static/image37.png)

**图 13**: 产品名称是所需时输入的单位价格 ([单击以查看实际尺寸的图像](batch-inserting-vb/_static/image39.png))


[![三个新 Veggies 中添加了供应商 Mayumi s](batch-inserting-vb/_static/image41.png)](batch-inserting-vb/_static/image40.png)

**图 14**： 三个新 Veggies 中添加了供应商 Mayumi s ([单击以查看实际尺寸的图像](batch-inserting-vb/_static/image42.png))


[![在 GridView 的最后一页中找不到新产品](batch-inserting-vb/_static/image44.png)](batch-inserting-vb/_static/image43.png)

**图 15**: 新产品可在 GridView 最后一页 ([单击以查看实际尺寸的图像](batch-inserting-vb/_static/image45.png))


> [!NOTE]
> 批处理插入本教程中使用的逻辑包装在事务范围内的插入操作。 若要验证这一点，有意引入一个数据库级错误。 例如，这些操作都而不必分配新`ProductsRow`实例 s`CategoryID`属性中的选定值`Categories`DropDownList，分配到一个值喜欢`i * 5`。 此处`i`是循环索引器和具有介于 1 到 5 的值。 因此，当添加两个或多个产品的批处理插入的第一个产品将有一个有效`CategoryID`值 (5)，但后续产品将具有`CategoryID`达不匹配的值`CategoryID`中值`Categories`表。 净效果是，同时第一`INSERT`将成功，但后续的具有外键约束冲突将失败。 批处理插入是原子操作，因为第一个`INSERT`将被回滚，返回到之前批插入过程的状态数据库开始。


## <a name="summary"></a>摘要

通过这和前面的两个教程中，我们已创建接口，便于更新，删除和插入的数据的批次，它们都使用我们将添加到数据访问层中的事务支持[包装数据库修改在事务中](wrapping-database-modifications-within-a-transaction-vb.md)教程。 对于某些情况下，此类批处理处理用户界面极大地提高最终用户效率通过减少点击次数，回发和键盘鼠标上下文切换，同时还能保留基础数据的完整性。

本教程中完成我们了解了如何使用批处理的数据。 教程的下一步一套探讨各种高级数据访问层方案，包括使用存储的过程在 TableAdapter 的方法中，在 DAL、 加密连接字符串和详细信息中配置连接和命令级别设置 ！

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 出于本教程已希尔顿 Giesenow 和 S ren 异世 Lauritsen 导致审阅者。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一篇](batch-deleting-vb.md)
