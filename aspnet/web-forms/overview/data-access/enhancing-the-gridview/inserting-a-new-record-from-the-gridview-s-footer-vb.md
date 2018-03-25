---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
title: "将新记录插入从 GridView 的页脚 (VB) |Microsoft 文档"
author: rick-anderson
description: "虽然 GridView 控件不提供内置支持用于插入的数据的新记录，本教程演示如何增加 GridView 包括..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 528acc48-f20c-4b4e-aa16-4cc02f068ebb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: 4d452e15ced52fd9dcac8201598146cb9ef38d7b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="inserting-a-new-record-from-the-gridviews-footer-vb"></a>将新记录插入从 GridView 的页脚 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_VB.exe)或[下载 PDF](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/datatutorial53vb1.pdf)

> 虽然 GridView 控件不提供内置支持用于插入的数据的新记录，本教程将演示如何增加 GridView 以包括插入的接口。


## <a name="introduction"></a>介绍

中所述[概述的插入、 更新和删除数据](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)教程，GridView，说明如何和 FormView Web 控制每个包括内置的数据修改功能。 如果使用声明性数据源控件，这三个 Web 控件可以快速轻松地配置来修改数据的和无需编写一行代码的方案中。 遗憾的是，仅说明和 FormView 控件提供内置的插入、 编辑和删除功能。 GridView 只提供编辑和删除的支持。 但是，有少量弯头硅脂，我们可以增加 GridView 以包括插入的接口。

在将插入功能添加到 GridView，我们是负责确定将添加新记录、 创建插入的接口，和编写代码以插入新记录。 在本教程中我们将着眼于将插入接口添加到的 GridView 的页脚行 （请参见图 1）。 每个列的页脚单元格包括适当的数据集合用户界面元素 （产品 s 名称文本框中，用于供应商，依次类推 DropDownList）。 我们还需要一个列的添加按钮，单击时，将导致回发并插入新记录到`Products`表使用页脚行中提供的值。


[![页脚行提供用于添加新的产品的接口](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.png)

**图 1**： 页脚行提供用于添加新产品的接口 ([单击以查看实际尺寸的图像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>步骤 1： 在一个 GridView 中显示产品信息

我们考虑自行创建 GridView 的脚注中的插入接口之前，让 s 第一个专注于向列出数据库中的产品页中添加一个 GridView。 首先打开`InsertThroughFooter.aspx`页面`EnhancedGridView`文件夹，然后拖动一个 GridView 从工具箱中拖动到设计器中，设置 GridView s`ID`属性`Products`。 接下来，使用 GridView s 智能标记将其绑定到名为新 ObjectDataSource `ProductsDataSource`。


[![创建名为 ProductsDataSource 新对象数据源](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.png)

**图 2**： 创建新对象数据源命名`ProductsDataSource`([单击以查看实际尺寸的图像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.png))


配置对象数据源以使用`ProductsBLL`类的`GetProducts()`方法以检索产品信息。 对于本教程，允许 s 焦点严格上添加插入功能测试和无需担心编辑和删除。 因此，请确保插入选项卡中的下拉列表设置为`AddProduct()`并更新和删除选项卡中的下拉列表被设置为 （无）。


[![将 AddProduct 方法映射到 ObjectDataSource 的 Insert() 方法](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.png)

**图 3**： 映射`AddProduct`ObjectDataSource 的方法`Insert()`方法 ([单击以查看实际尺寸的图像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.png))


[![设置为 （无） 更新和删除选项卡下拉列表](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.png)

**图 4**： 将更新和删除选项卡下拉列表设置为 （无） ([单击以查看实际尺寸的图像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.png))


完成 ObjectDataSource s 配置数据源向导后，Visual Studio 将自动将字段添加到 GridView 为每个相应的数据字段。 现在，将保留所有由 Visual Studio 添加的字段。 稍后在本教程中我们将返回并删除其中一些字段，其值不需要添加一条新记录时指定。

由于存在接近 80 产品在数据库中，用户将需要向下滚动一直到 web 页的底部才能添加新的记录。 因此，让我们来启用分页更可见且可访问使插入的接口。 若要打开分页，只需选中从 GridView s 智能标记启用分页复选框。

此时，GridView 和 ObjectDataSource s 声明性标记应类似以下：


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample1.aspx)]


[![所有产品数据字段都显示在分页的 GridView](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.png)

**图 5**： 所有产品数据字段都显示在分页 GridView ([单击以查看实际尺寸的图像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.png))


## <a name="step-2-adding-a-footer-row"></a>步骤 2： 添加页脚行

其标头和数据行，以及 GridView 包括的页脚行。 根据 GridView s 的值显示页眉和页脚行[ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx)和[ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx)属性。 若要显示页脚行，只需将`ShowFooter`属性`True`。 如图 6 所示，设置`ShowFooter`属性`True`将的页脚行添加到网格。


[![若要显示页脚行，请将 ShowFooter 设置为 True](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.png)

**图 6**： 到显示页脚行中，设置`ShowFooter`到`True`([单击以查看实际尺寸的图像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.png))


请注意，页脚行深红色背景颜色。 这是由于 DataWebControls 主题我们创建并应用于所有页进来[将数据显示与 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md)教程。 具体而言，`GridView.skin`文件将配置`FooterStyle`属性此类用于`FooterStyle`CSS 类。 `FooterStyle`类中定义`Styles.css`，如下所示：


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample2.css)]

> [!NOTE]
> 我们已介绍了在前面的教程中使用的 GridView 的页脚行。 如果需要回头参考[GridView 的页脚中显示摘要信息](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md)复习教程。


设置后`ShowFooter`属性`True`，花一些时间才能在浏览器中查看的输出。 当前页脚行不包含任何文本或 Web 控件。 在步骤 3 中我们将修改每个 GridView 字段的页脚，以使其包括适当的插入接口。


[![空的页脚行是显示上面分页接口控制](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image13.png)

**图 7**: 空页脚行是显示上面分页接口控制 ([单击以查看实际尺寸的图像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>步骤 3： 自定义的页脚行

返回[GridView 控件中使用 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md)教程，我们已了解如何极大地自定义使用 TemplateFields （而不是 BoundFields 或 CheckBoxFields） 的特定 GridView 列的显示; 在[自定义的数据修改界面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)我们看使用 TemplateFields 自定义在 GridView 的编辑界面。 回想一下 TemplateField 组成了许多的模板定义的标记、 Web 控件和数据绑定语法的组合，用于某些类型的行。 `ItemTemplate`，例如，指定用于只读的行的模板时`EditItemTemplate`定义可编辑的行的模板。

连同`ItemTemplate`和`EditItemTemplate`，TemplateField 还包括`FooterTemplate`，它指定页脚行的内容。 因此，我们可以将添加所需的每个字段 s 插入到接口的 Web 控件`FooterTemplate`。 若要开始，请将所有 GridView 中的字段转换为 TemplateFields。 这可以单击 GridView s 智能标记，在左下角，选择每个字段和 TemplateField 链接到单击转换此字段中的编辑列链接。


![将每个字段转换为 TemplateField](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.gif)

**图 8**： 将每个字段转换为 TemplateField


单击转换转换为 TemplateField 此字段将变为等效 TemplateField 到当前的字段类型。 例如，每个 BoundField 替换为 TemplateField`ItemTemplate`包含显示相应的数据字段的标签和`EditItemTemplate`文本框中显示数据字段。 `ProductName` BoundField 已转换为以下 TemplateField 标记：


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample3.aspx)]

同样， `Discontinued` CheckBoxField 已转换为 TemplateField 其`ItemTemplate`和`EditItemTemplate`包含复选框 Web 控件 (使用`ItemTemplate`s 禁用的复选框)。 只读`ProductID`BoundField 已转换为与标签控件中都为 TemplateField`ItemTemplate`和`EditItemTemplate`。 字段转换为 TemplateField 转换现有的 GridView 简单地说，是功能的一种在切换到更容易自定义 TemplateField 而不丢失任何现有字段的快速而简单的方法。

自 GridView 我们重新使用没有 t 支持编辑，请尝试删除`EditItemTemplate`只需从每个 TemplateField 离开`ItemTemplate`。 完成后，你 GridView s 声明性标记应如下所示：


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample4.aspx)]

现在，已转换为 TemplateField 转换每个 GridView 字段，我们可以向每个字段 s 输入适当的插入接口`FooterTemplate`。 某些字段不会插入接口 (`ProductID`，例如); 其他会随之用于收集新的产品的信息的 Web 控件。

若要创建的编辑界面，请从 GridView s 智能标记中选择编辑模板链接。 然后，从下拉列表中，选择相应的字段的`FooterTemplate`并将相应的控件拖到设计器工具箱。


[![将相应的插入接口添加到每个字段的要](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image15.png)

**图 9**： 将相应的插入接口添加到每个字段 s `FooterTemplate` ([单击以查看实际尺寸的图像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image16.png))


以下项目符号列表枚举 GridView 字段，指定要添加的插入接口：

- `ProductID`无。
- `ProductName`添加一个文本框，并设置其`ID`到`NewProductName`。 添加 RequiredFieldValidator 控件以确保用户输入一个值，新的产品 s 名称。
- `SupplierID`无。
- `CategoryID`无。
- `QuantityPerUnit`添加文本框中，设置其`ID`到`NewQuantityPerUnit`。
- `UnitPrice`添加名为的文本框`NewUnitPrice`和 CompareValidator，以确保输入的值是大于或等于零的货币值。
- `UnitsInStock`使用文本框中其`ID`设置为`NewUnitsInStock`。 包括 CompareValidator，以确保输入的值是大于或等于零的整数值。
- `UnitsOnOrder`使用文本框中其`ID`设置为`NewUnitsOnOrder`。 包括 CompareValidator，以确保输入的值是大于或等于零的整数值。
- `ReorderLevel`使用文本框中其`ID`设置为`NewReorderLevel`。 包括 CompareValidator，以确保输入的值是大于或等于零的整数值。
- `Discontinued`添加一个复选框，设置其`ID`到`NewDiscontinued`。
- `CategoryName`添加的 DropDownList 并设置其`ID`到`NewCategoryID`。 将其绑定到名为新 ObjectDataSource`CategoriesDataSource`和将其配置为使用`CategoriesBLL`类的`GetCategories()`方法。 具有 DropDownList s `ListItem` s 显示`CategoryName`数据字段中，使用`CategoryID`作为其值的数据字段。
- `SupplierName`添加的 DropDownList 并设置其`ID`到`NewSupplierID`。 将其绑定到名为新 ObjectDataSource`SuppliersDataSource`和将其配置为使用`SuppliersBLL`类的`GetSuppliers()`方法。 具有 DropDownList s `ListItem` s 显示`CompanyName`数据字段中，使用`SupplierID`作为其值的数据字段。

对于每个验证控件中，清除`ForeColor`属性以便`FooterStyle`CSS 类 s 白色前景颜色将代替红色默认值。 此外使用`ErrorMessage`属性的详细说明，但设置`Text`为星号的属性。 若要防止验证控件的文本导致要换到两个行的插入接口，设置`FooterStyle`s`Wrap`属性设置为 false 的每个`FooterTemplate`使用验证控件的 s。 最后，添加 ValidationSummary 控件下 GridView 并设置其`ShowMessageBox`属性`True`及其`ShowSummary`属性`False`。

在添加新产品时，我们需要提供`CategoryID`和`SupplierID`。 通过中的页脚单元格 DropDownLists 捕获此信息`CategoryName`和`SupplierName`字段。 我选择了使用这些字段相对于`CategoryID`和`SupplierID`TemplateFields 因为网格的数据行用户很可能更感兴趣的类别和供应商名称而不是其 ID 值。 由于`CategoryID`和`SupplierID`值现在正在以捕获`CategoryName`和`SupplierName`字段 s 插入接口，我们可以删除`CategoryID`和`SupplierID`TemplateFields 从 GridView。

同样，`ProductID`未使用时添加一个新的产品，因此`ProductID`TemplateField 可以以及删除。 但是，让我们来保留`ProductID`字段在网格中。 除了文本框中、 DropDownLists、 复选框，和构成插入接口的验证控件中，我们还需要添加按钮，单击时，执行逻辑，以向数据库添加新的产品。 在步骤 4 中，我们将包含插入接口中添加按钮`ProductID`TemplateField 的`FooterTemplate`。

随意提高各种 GridView 字段的外观。 例如，你可能想要设置格式`UnitPrice`值作为货币、 右对齐`UnitsInStock`， `UnitsOnOrder`，和`ReorderLevel`字段和更新`HeaderText`TemplateFields 的值。

创建插入接口中的许多之后`FooterTemplate`s，删除`SupplierID`，和`CategoryID`TemplateFields，并提高的格式和对齐方式 TemplateFields，声明性你 GridView s 通过网格美感标记应类似于以下形式：


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample5.aspx)]

查看通过浏览器时, 的 GridView 的页脚行现在包含完成插入接口 （请参阅图 10）。 此时，插入的接口不包括用户指示该她 s 为新产品输入数据，并且想要将一个新记录插入数据库的一种方法。 此外，我们已有待解决如何插入页脚输入的数据将转换到一条新记录中`Products`数据库。 在步骤 4 中我们将查看如何包含添加按钮对插入的接口以及如何在执行代码回发时它 s 单击。 第 5 步演示如何插入新记录使用页脚中的数据。


[![GridView 页脚提供用于添加新记录的接口](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image17.png)

**图 10**: GridView 页脚提供用于添加新记录的接口 ([单击以查看实际尺寸的图像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>步骤 4： 在插入界面中包括的添加按钮

我们需要添加按钮某个位置在接口中包含插入因为当前插入接口的页脚行 s 中没有用于用户指示他们已完成输入新的产品的信息的方法。 这无法放入一个现有`FooterTemplate`s，或者我们无法添加一个新列在网格为此目的。 对于本教程中，让 s 将放在添加按钮`ProductID`TemplateField 的`FooterTemplate`。

从设计器中，单击 GridView s 智能标记中的编辑模板链接，然后选择`ProductID`字段的`FooterTemplate`从下拉列表。 添加到模板，将其 ID 设置为的 Button Web 控件 （或 LinkButton 或 ImageButton，如果你愿意） `AddProduct`，将其`CommandName`插入，并且其`Text`添加图 11 中所示的属性。


[![添加按钮置于 ProductID TemplateField 的要](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image19.png)

**图 11**： 放置在添加按钮`ProductID`TemplateField s `FooterTemplate` ([单击以查看实际尺寸的图像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image20.png))


一旦你已包含添加按钮，测试浏览器中的页。 请注意，当单击添加按钮使用插入的接口中的数据无效，简单地说 circuited 回发 ValidationSummary 控件指示无效的数据 （请参阅图 12）。 输入适当的数据，单击添加按钮会导致回发。 没有记录添加到数据库，但是。 我们将需要编写的代码来实际执行插入。


[![添加按钮 s 回发，则短 Circuited 如果插入界面中存在无效的数据](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image21.png)

**图 12**: 添加按钮 s 回发是短 Circuited 如果插入界面中存在无效的数据 ([单击以查看实际尺寸的图像](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image22.png))


> [!NOTE]
> 插入的接口中的验证控件不会将分配给验证组。 这很好地配合只要插入接口是唯一的页面上的验证控件集。 如果，但是，有其他验证控件 （如网格 s 编辑界面中的验证控件） 页上，验证控件中插入接口，并添加按钮的`ValidationGroup`属性应分配相同的值作为 so 到将这些控件与特定的验证组相关联。 请参阅[将 ASP.NET 2.0 中的验证控件](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)有关分区的验证控件和在页面上的按钮为验证组的详细信息。


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>步骤 5： 插入一条新记录到`Products`表

当利用内置的编辑功能的 GridView，GridView 自动处理所有所需执行更新的工作。 具体而言，单击更新按钮时复制从编辑接口输入到 ObjectDataSource s 中的参数的值`UpdateParameters`集合和关闭通过调用 ObjectDataSource 的更新的启动`Update()`方法。 由于 GridView 不提供此类用于插入的内置功能，我们必须实现调用 ObjectDataSource 的代码`Insert()`方法和 ObjectDataSource s 接口中插入的值的副本`InsertParameters`集合.

后单击添加按钮后，应执行此插入逻辑。 中所述[添加和响应中 GridView 按钮](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb.md)教程中，每当按钮、 LinkButton 或在 GridView ImageButton 单击后，GridView 的`RowCommand`在回发事件激发。 是否按钮、 LinkButton 或 ImageButton 已添加显式如页脚行中的添加按钮，或如果它已自动添加通过 GridView 会激发此事件 (如选中启用排序后，每个列顶部 LinkButtons 或在分页界面当选中启用分页时 LinkButtons)。

因此，若要响应用户单击添加按钮，我们需要创建的事件处理程序 GridView 的`RowCommand`事件。 由于此事件时将触发*任何*单击按钮、 LinkButton 或在 GridView ImageButton 后，它 s 重要，我们仅使用插入的逻辑的情况下继续`CommandName`属性到传递到事件处理程序映射`CommandName`的添加按钮 (Insert) 的值。 此外，我们还仅在才继续验证控件报告有效的数据。 为此，创建的事件处理程序`RowCommand`事件替换为以下代码：


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample6.vb)]

> [!NOTE]
> 你可能想知道为什么事件处理程序厌烦检查`Page.IsValid`属性。 在所有情况中插入的接口提供无效的数据，被抑制获胜的 t 回发？ 此假设是正确的只要用户具有未禁用 JavaScript 或执行了步骤，若要避免客户端验证逻辑。 简单地说，一个应永远不会完全依赖于客户端验证，则为始终应使用的数据之前执行服务器端检查的有效性。


在步骤 1 中，我们创建`ProductsDataSource`ObjectDataSource 以便其`Insert()`方法映射到`ProductsBLL`类的`AddProduct`方法。 用于插入新记录到`Products`表，我们可以只需调用 ObjectDataSource 的`Insert()`方法：


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample7.vb)]

现在，`Insert()`调用方法时，所有剩下就是进行插入的接口的值复制到的参数传递到`ProductsBLL`类的`AddProduct`方法。 正如我们所看到的进来[检查与插入、 更新和删除的事件相关联](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)教程，则这可以通过 ObjectDataSource s 来实现`Inserting`事件。 在`Inserting`我们需要以编程方式引用中的所有控件的事件`Products`GridView 的页脚行并将分配到其值`e.InputParameters`集合。 如果用户省略一个值，如离开`ReorderLevel`文本框中为空白，我们需要指定插入到数据库中的值应为`NULL`。 由于`AddProducts`方法接受可以为 null 的数据库字段可以为 null 的类型，只需使用可以为 null 的类型，并将其值设置为`Nothing`在其中省略用户输入的情况下。


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample8.vb)]

与`Inserting`完成的事件处理程序，新的记录可以添加到`Products`通过 GridView 的页脚行的数据库表。 请继续并尝试添加几个新产品。

## <a name="enhancing-and-customizing-the-add-operation"></a>增强和自定义添加的操作

目前，单击添加按钮将一条新记录添加到数据库表，但不提供任何种类的可视反馈，已成功添加记录。 理想情况下，标签 Web 控件或客户端警报框将通知用户他们插入已成功完成。 我将此作为练习为读取器。

本教程中使用 GridView 不适用于任何排序顺序列出的产品中，也不允许最终用户对数据进行排序。 因此，记录进行排序，因为它们是由其主键的主键字段数据库中。 由于每个新的记录具有`ProductID`大于最后一个，每次新产品添加到网格的末尾附加在值。 因此，你可能想要自动将用户发送到的 GridView 的最后一页，添加一条新记录后。 这可以通过在调用后添加以下代码行来实现`ProductsDataSource.Insert()`中`RowCommand`事件处理程序，以指示用户需要发送的最后一页数据绑定到 GridView 后：


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample9.vb)]

`SendUserToLastPage`最初的页级别布尔变量分配值`False`。 在 GridView`DataBound`事件处理程序，如果`SendUserToLastPage`为 false，`PageIndex`属性进行更新，向用户发送的最后一页。


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample10.vb)]

原因`PageIndex`属性在中设置`DataBound`事件处理程序 (而非`RowCommand`事件处理程序) 是因为当`RowCommand`事件处理程序，将引发我们遇到有待向其中添加新记录`Products`数据库表。 因此，在`RowCommand`事件处理程序的最后一个的页索引 (`PageCount - 1`) 表示最后一个的页索引*之前*已添加新的产品。 对于所添加的产品的大多数，最后一个的页索引是相同的后添加新的产品。 但是当添加一项产品会导致新的最后一页索引，如果我们将错误地更新`PageIndex`中`RowCommand`事件处理程序然后我们将转到第二个到最后一页 （在添加新产品之前的最后一页索引） 而不是新的最后一页 index。 由于`DataBound`之后添加了新的产品，通过设置，数据重新绑定到网格中，将触发的事件处理程序`PageIndex`属性存在我们知道我们重新获取正确的最后一页索引。

最后，本教程中使用 GridView 是相当宽由于必须为添加新产品收集的字段数。 由于此宽度，说明如何 s 垂直布局可能是首选。 GridView s 可通过收集较少输入减少总体宽度。 可能是我们不收集 t 需要`UnitsOnOrder`， `UnitsInStock`，和`ReorderLevel`字段时添加一个新的产品，在这种情况下可能从 GridView 中删除这些字段。

若要调整收集的数据，我们可以使用两种方法之一：

- 继续使用`AddProduct`需要为值的方法`UnitsOnOrder`， `UnitsInStock`，和`ReorderLevel`字段。 在`Inserting`事件处理程序，提供硬编码，默认值用于已从插入的接口中删除这些输入。
- 创建的新重载`AddProduct`中的方法`ProductsBLL`不接受输入的类`UnitsOnOrder`， `UnitsInStock`，和`ReorderLevel`字段。 然后，在 ASP.NET 页中，配置对象数据源以使用此新的重载。

这两个选项同样也将工作。 在过去的教程我们使用后一种方式，创建多个重载`ProductsBLL`类的`UpdateProduct`方法。

## <a name="summary"></a>摘要

GridView 缺少内置插入中的功能之外的说明和 FormView，但进行少量的工作量插入接口可以添加到页脚行。 若要只需在一个 GridView 中显示的页脚行设置其`ShowFooter`属性`True`。 由字段转换为 TemplateField，可以为每个字段定制的页脚行内容，并添加插入接口`FooterTemplate`。 正如我们在本教程中，看到`FooterTemplate`只能包含按钮、 文本框中，DropDownLists、 复选框，用于填充数据驱动的 Web 控件 （如 DropDownLists)，数据源控件和验证控件。 用于收集用户的输入控件，以及需要添加按钮、 LinkButton 或 ImageButton。

单击添加按钮后，ObjectDataSource 的`Insert()`方法调用以启动插入工作流。 ObjectDataSource 然后调用配置的 insert 方法 (`ProductsBLL`类的`AddProduct`方法，在本教程中)。 我们必须将值插入到 ObjectDataSource s 的接口的 GridView s 从复制`InsertParameters`之前正在调用的插入方法的集合。 这可以通过以编程方式引用 ObjectDataSource s 中的插入接口 Web 控件来实现`Inserting`事件处理程序。

本教程中完成我们看增强 GridView 的外观的技术。 教程的下一组将检查如何处理二进制数据，如图像、 Pdf、 Word 文档中，依次类推和 Web 控件的数据。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已伯纳黛特 Leigh。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一篇](adding-a-gridview-column-of-checkboxes-vb.md)
