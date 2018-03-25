---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
title: "批更新 (VB) |Microsoft 文档"
author: rick-anderson
description: "了解如何更新单个操作中的多个数据库记录。 在用户界面层中，我们将生成一个 GridView，其中每一行都是可编辑。 数据中..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: d191a204-d7ea-458d-b81c-0b9049ecb55f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
msc.type: authoredcontent
ms.openlocfilehash: bcfdf734de0b4a4aa0a11f35bd6e40d6b97719cf
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="batch-updating-vb"></a>批更新 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_VB.zip)或[下载 PDF](batch-updating-vb/_static/datatutorial64vb1.pdf)

> 了解如何更新单个操作中的多个数据库记录。 在用户界面层中，我们将生成一个 GridView，其中每一行都是可编辑。 在数据访问层中我们将包装在以确保所有更新都成功，或所有更新都将都回滚一个事务内的多个更新操作。


## <a name="introduction"></a>介绍

在[前面教程](wrapping-database-modifications-within-a-transaction-vb.md)我们已了解如何扩展数据访问层，若要添加的数据库事务的支持。 数据库事务保证一系列的数据修改语句将被视为一个原子操作，这将确保所有将成功或所有修改将都失败。 使用此低级 DAL 功能开，我们重新准备好着重于创建数据修改接口的批处理。

在本教程中我们将生成一个 GridView，其中每一行都是可编辑 （请参阅图 1）。 由于每个行在其编辑界面，那里 s 中的编辑列不需要呈现，更新和取消按钮。 相反，有两个更新产品按钮在页上，单击时，枚举 GridView 行并更新数据库。


[![在 GridView 的每一行都是可编辑](batch-updating-vb/_static/image1.gif)](batch-updating-vb/_static/image1.png)

**图 1**: GridView 中的每一行都是可编辑 ([单击以查看实际尺寸的图像](batch-updating-vb/_static/image2.png))


让我们来开始 ！

> [!NOTE]
> 在[执行批处理更新](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md)教程中我们创建一个批处理编辑接口使用 DataList 控件。 本教程不同从以前的是使用一个 GridView，且在事务范围内执行批处理更新。 完成本教程后我鼓励你返回到之前的教程并将其更新为使用在前面的教程添加的数据库与事务相关功能。


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>检查建立所有 GridView 行可编辑的步骤

中所述[概述的插入、 更新和删除数据](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)教程中，GridView 提供对编辑的行数基于其基础数据的内置支持。 在内部，GridView 说明哪些行是通过可编辑其[`EditIndex`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx)。 如 GridView 正在绑定到其数据源，它会检查每个行，若要查看的行索引是否等于的值`EditIndex`。 如果是这样，该行 s 使用其编辑呈现字段的接口。 对于 BoundFields，编辑界面是文本框中其`Text`属性分配指定 BoundField s 的数据字段的值`DataField`属性。 有关 TemplateFields，`EditItemTemplate`代替使用`ItemTemplate`。

回想一下，编辑工作流将启动当用户单击行的编辑按钮。 这将导致回发，设置 GridView 的`EditIndex`到的被单击的行的索引和重新绑定数据到网格的属性。 当单击行 s 取消按钮，在回发时`EditIndex`设置的值为`-1`之前重新绑定数据到网格。 由于 GridView 的行开始从零开始的索引，因此设置`EditIndex`到`-1`在只读模式中显示 GridView 的效果。

`EditIndex`属性非常适用于每个行编辑，但不是用于批处理编辑。 若要使整个 GridView 可编辑，我们需要使用其编辑界面设置呈现在每个行。 实现此目的的最简单方法是创建具有其编辑界面 TemplateField 中定义每个可编辑字段的实现了`ItemTemplate`。

在接下来的几个步骤我们将创建一个完全可编辑的 GridView。 在步骤 1 中我们将首先创建 GridView 和其对象数据源并将其 BoundFields 和 CheckBoxField 转换成 TemplateFields。 步骤 2 和 3 中，我们将从 TemplateFields 移动编辑接口`EditItemTemplate`到其`ItemTemplate`s。

## <a name="step-1-displaying-product-information"></a>步骤 1： 显示产品信息

有关创建 GridView 我们担心之前所在的行都是可编辑，但让 s 入手，只显示的产品信息。 打开`BatchUpdate.aspx`页面`BatchData`文件夹，然后拖动一个 GridView 从工具箱中拖动到设计器。 设置 GridView s`ID`到`ProductsGrid`和从其智能标记上，选择要绑定到名为新 ObjectDataSource `ProductsDataSource`。 配置对象数据源中检索其数据从`ProductsBLL`类的`GetProducts`方法。


[![配置对象数据源以使用 ProductsBLL 类](batch-updating-vb/_static/image2.gif)](batch-updating-vb/_static/image3.png)

**图 2**： 配置使用 ObjectDataSource`ProductsBLL`类 ([单击以查看实际尺寸的图像](batch-updating-vb/_static/image4.png))


[![检索产品数据使用 GetProducts 方法](batch-updating-vb/_static/image3.gif)](batch-updating-vb/_static/image5.png)

**图 3**： 检索产品数据使用`GetProducts`方法 ([单击以查看实际尺寸的图像](batch-updating-vb/_static/image6.png))


如 GridView，ObjectDataSource 的修改功能旨在针对每个行而工作。 若要更新的一组记录，我们将需要在批处理数据并将它传递给 BLL 的 ASP.NET 页的代码隐藏类中编写的代码。 因此，下拉列表中设置 ObjectDataSource 的更新、 插入和删除选项卡添加到 （无）。 单击完成以完成向导。


[![在更新中，INSERT、 设置的下拉列表和删除选项卡到 （无）](batch-updating-vb/_static/image4.gif)](batch-updating-vb/_static/image7.png)

**图 4**： 设置的下拉列表中更新、 插入和删除选项卡为 （无） ([单击以查看实际尺寸的图像](batch-updating-vb/_static/image8.png))


完成配置数据源向导后，ObjectDataSource s 声明性标记应如下所示：


[!code-aspx[Main](batch-updating-vb/samples/sample1.aspx)]

完成配置数据源向导也会导致 Visual Studio 创建 BoundFields 和产品数据字段 CheckBoxField 中 GridView。 对于本教程，让我们来仅允许用户查看和编辑产品的名称、 类别、 价格和停用的状态。 删除所有但`ProductName`， `CategoryName`， `UnitPrice`，和`Discontinued`字段和重命名`HeaderText`的前三个属性字段设置为产品、 类别和价格，分别。 最后，检查 GridView s 智能标记中的启用分页和启用排序复选框。

此时 GridView 具有三个 BoundFields (`ProductName`， `CategoryName`，和`UnitPrice`) 和 CheckBoxField (`Discontinued`)。 我们需要将这四个字段转换为 TemplateFields，然后将编辑界面转移从 TemplateField s`EditItemTemplate`到其`ItemTemplate`。

> [!NOTE]
> 我们探讨了创建和自定义在 TemplateFields[自定义的数据修改界面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)教程。 我们将指导您完成将 BoundFields 和 CheckBoxField 转换到 TemplateFields 的步骤和定义其编辑接口中其`ItemTemplate`s，但如果你遇到困难或需要刷新程序，don t 对于有所顾虑回头参考此早期的教程。


从 GridView s 智能标记中，单击编辑列链接以打开字段对话框。 接下来，选择每个字段，然后单击 TemplateField 链接到的转换此字段。


![现有 BoundFields 和 CheckBoxField 转换为 TemplateFields](batch-updating-vb/_static/image5.gif)

**图 5**： 现有 BoundFields 和 CheckBoxField 转换为 TemplateFields


现在，每个字段都为 TemplateField，我们已准备好移动编辑重新接口从`EditItemTemplate`到`ItemTemplate`s。

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>步骤 2： 创建`ProductName`，`UnitPrice`，和`Discontinued`编辑接口

创建`ProductName`， `UnitPrice`，和`Discontinued`编辑接口是此步骤的主题和 TemplateField s 中已定义的每个接口都可以很简单， `EditItemTemplate`。 创建`CategoryName`编辑接口是有些复杂，因为我们需要创建适用的类别的说明。 这`CategoryName`编辑接口在步骤 3 中处理。

允许 s 开头`ProductName`TemplateField。 单击从 GridView s 智能标记的编辑模板链接和深化至`ProductName`TemplateField 的`EditItemTemplate`。 选择文本框中，将其复制到剪贴板，然后将其粘贴到`ProductName`TemplateField 的`ItemTemplate`。 更改文本框中 s`ID`属性`ProductName`。

接下来，添加到 RequiredFieldValidator`ItemTemplate`以确保用户为每个产品的名称提供一个值。 设置`ControlToValidate`属性设置为产品名称`ErrorMessage`给您的属性必须提供该产品的名称。 与`Text`属性\*。 建立到这些添加后`ItemTemplate`，你的屏幕应类似于图 6。


[![ProductName TemplateField 现在包括一个文本框和 RequiredFieldValidator](batch-updating-vb/_static/image6.gif)](batch-updating-vb/_static/image9.png)

**图 6**: `ProductName` TemplateField 现在包含一个文本框中的和一 RequiredFieldValidator ([单击以查看实际尺寸的图像](batch-updating-vb/_static/image10.png))


有关`UnitPrice`编辑接口，通过将复制从文本框中开始`EditItemTemplate`到`ItemTemplate`。 接下来，将文本框中和组的前面的 $ 其`ID`UnitPrice 属性并将其`Columns`属性设置为 8。

此外将添加到 CompareValidator `UnitPrice` s`ItemTemplate`以确保用户输入的值是有效的货币值大于或等于 0.00 美元。 设置验证程序 s`ControlToValidate`属性设置为单价，其`ErrorMessage`属性设置为你必须输入一个有效的货币值。 请省略任何货币符号。，其`Text`属性\*，将其`Type`属性`Currency`，将其`Operator`属性`GreaterThanEqual`，并将其`ValueToCompare`属性设为 0。


[![添加 CompareValidator 以确保价格输入是一个非负货币值](batch-updating-vb/_static/image7.gif)](batch-updating-vb/_static/image11.png)

**图 7**： 添加 CompareValidator 以确保价格输入是一个非负货币值 ([单击以查看实际尺寸的图像](batch-updating-vb/_static/image12.png))


有关`Discontinued`TemplateField 可以使用已在中定义的复选框`ItemTemplate`。 只需将设置其`ID`到已中断并将其`Enabled`属性`True`。

## <a name="step-3-creating-thecategorynameediting-interface"></a>步骤 3： 创建`CategoryName`编辑接口

中的编辑界面`CategoryName`TemplateField s`EditItemTemplate`包含一个文本框，用于显示的值`CategoryName`数据字段。 我们需要将其替换为列出可能的类别的 DropDownList。

> [!NOTE]
> [自定义的数据修改界面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)教程包含自定义模板以包括而不是一个文本框 DropDownList 的更全面且完整讨论。 此处的步骤完成时，会向他们显示 tersely。 有关创建和配置类别的 DropDownList 更深入探讨，回头参考[自定义的数据修改界面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)教程。


从工具箱拖到拖动 DropDownList `CategoryName` TemplateField s `ItemTemplate`，设置其`ID`到`Categories`。 此时我们将通常定义 DropDownLists 的数据源通过其智能标记，创建新对象数据源。 但是，这会将添加在 ObjectDataSource `ItemTemplate`，这将导致为每个 GridView 行创建 ObjectDataSource 实例。 相反，让我们来创建外部 GridView 的 TemplateFields ObjectDataSource。 结束模板编辑，并从工具箱拖到设计器下拖动 ObjectDataSource`ProductsDataSource`对象数据源。 命名新 ObjectDataSource`CategoriesDataSource`和将其配置为使用`CategoriesBLL`类的`GetCategories`方法。


[![配置对象数据源以使用 CategoriesBLL 类](batch-updating-vb/_static/image8.gif)](batch-updating-vb/_static/image13.png)

**图 8**： 配置使用 ObjectDataSource`CategoriesBLL`类 ([单击以查看实际尺寸的图像](batch-updating-vb/_static/image14.png))


[![检索类别数据使用 GetCategories 方法](batch-updating-vb/_static/image9.gif)](batch-updating-vb/_static/image15.png)

**图 9**： 检索类别的数据使用`GetCategories`方法 ([单击以查看实际尺寸的图像](batch-updating-vb/_static/image16.png))


由于此 ObjectDataSource 仅用于检索数据，到 （无） 更新和删除选项卡中设置的下拉列表。 单击完成以完成向导。


[![组的下拉列表中的更新和删除选项卡添加到 （无）](batch-updating-vb/_static/image10.gif)](batch-updating-vb/_static/image17.png)

**图 10**： 设置的下拉列表中的更新和删除选项卡为 （无） ([单击以查看实际尺寸的图像](batch-updating-vb/_static/image18.png))


完成向导后, `CategoriesDataSource` s 声明性标记应如下所示：


[!code-aspx[Main](batch-updating-vb/samples/sample2.aspx)]

与`CategoriesDataSource`创建并配置，返回到`CategoryName`TemplateField 的`ItemTemplate`，再从 DropDownList s 智能标记中，单击选择数据源链接。 在数据源配置向导中，选择`CategoriesDataSource`选项从第一个下拉列表中，选择具有`CategoryName`用于显示和`CategoryID`作为值。


[![绑定的 DropDownList 到 CategoriesDataSource](batch-updating-vb/_static/image11.gif)](batch-updating-vb/_static/image19.png)

**图 11**： 绑定到 DropDownList `CategoriesDataSource` ([单击以查看实际尺寸的图像](batch-updating-vb/_static/image20.png))


此时`Categories`DropDownList 列出的所有类别，但它不会尚未自动选择相应绑定到 GridView 行的产品类别。 若要完成此我们需要将设置`Categories`DropDownList s`SelectedValue`到产品的`CategoryID`值。 单击从 DropDownList s 智能标记的编辑数据绑定链接并将关联`SelectedValue`具有属性`CategoryID`数据字段图 12 中所示。


![将产品的 CategoryID 值绑定到的 DropDownList 的 SelectedValue 属性](batch-updating-vb/_static/image12.gif)

**图 12**： 绑定产品 s`CategoryID`值到 DropDownList 的`SelectedValue`属性


一个最后一个问题保持： 如果产品不具有`CategoryID`上指定值然后数据绑定语句`SelectedValue`将导致异常。 这是因为下拉列表包含仅为类别的项，不提供的这些产品选项`NULL`数据库值`CategoryID`。 若要解决此问题，将设置的 DropDownList s`AppendDataBoundItems`属性`True`并将新项添加到下拉列表中，省略`Value`属性的声明性语法。 也就是说，请确保`Categories`DropDownList s 声明性语法，如下所示：


[!code-aspx[Main](batch-updating-vb/samples/sample3.aspx)]

请注意如何`<asp:ListItem Value="">`-一个选择-具有其`Value`属性显式设置为一个空字符串。 将回指[自定义的数据修改界面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)教程有关为什么需要此附加的 DropDownList 项处理的更全面讨论`NULL`用例和原因分配`Value`属性为空字符串至关重要。

> [!NOTE]
> 没有的潜在性能和可伸缩性问题此处值得一提的是。 因为每行都有使用 DropDownList`CategoriesDataSource`作为数据源，`CategoriesBLL`类 s`GetCategories`将调用方法 *n* 每页的次数，请访问，其中 *n*是 GridView 中的行数。 这些 *n* 调用`GetCategories`导致 *n* 到数据库的查询。 此对数据库的影响可能会降低通过缓存每个请求缓存中或通过使用 SQL 缓存依赖关系或非常短时间基于过期的缓存层，返回的类别。 有关每个请求的详细信息缓存选项，请参阅[`HttpContext.Items`每个请求缓存存储](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx)。


## <a name="step-4-completing-the-editing-interface"></a>步骤 4： 完成编辑接口

我们进行了大量更改到的 GridView 的模板而不暂停以查看我们的进度。 花些时间查看我们通过浏览器的进度。 如图 13 所示，每个行使用呈现其`ItemTemplate`，其中包含编辑界面单元格 s。


[![每 GridView 一行都是可编辑](batch-updating-vb/_static/image13.gif)](batch-updating-vb/_static/image21.png)

**图 13**： 每 GridView 一行都是可编辑 ([单击以查看实际尺寸的图像](batch-updating-vb/_static/image22.png))


有几个我们应负责在此时次要格式设置问题。 首先，请注意，`UnitPrice`值包含四个十进制点。 若要解决此问题，返回到`UnitPrice`TemplateField 的`ItemTemplate`，再从文本框中的 s 智能标记，单击编辑数据绑定链接。 接下来，指定`Text`属性的格式应为一个数字。


![格式文本属性设置为数字](batch-updating-vb/_static/image14.gif)

**图 14**： 格式`Text`数字形式的属性


其次，让 s 中心中的复选框`Discontinued`列 （而不是让它左对齐）。 单击编辑列的 GridView s 智能标记，然后选择`Discontinued`TemplateField 从左下角中的字段列表。 向下钻取`ItemStyle`并设置`HorizontalAlign`到中心图 15 中所示的属性。


![Center 废止的复选框](batch-updating-vb/_static/image15.gif)

**图 15**: Center`Discontinued`复选框


接下来，向页面添加 ValidationSummary 控件并设置其`ShowMessageBox`属性`True`及其`ShowSummary`属性`False`。 此外添加按钮 Web 控件，单击时，将会更新用户的更改。 具体而言，添加两个按钮 Web 控件，一个上面 GridView，它下面的一个设置两个控件`Text`更新产品的属性。

自 GridView s 编辑接口中定义其 TemplateFields `ItemTemplate` s， `EditItemTemplate` s 是不必要的并且可能会删除。

进行以上所述的格式设置的更改后，将按钮控件，添加和删除不必要`EditItemTemplate`s，你的页面 s 声明性语法应如下所示：


[!code-aspx[Main](batch-updating-vb/samples/sample4.aspx)]

图 16 显示此页时后已添加按钮 Web 控件，通过浏览器查看和格式设置所做的更改。


[![页现在包括两个更新产品按钮](batch-updating-vb/_static/image16.gif)](batch-updating-vb/_static/image23.png)

**图 16**: 页现在包含两个更新产品按钮 ([单击以查看实际尺寸的图像](batch-updating-vb/_static/image24.png))


## <a name="step-5-updating-the-products"></a>步骤 5： 更新产品

当用户访问此页它们将使其修改，然后单击两个更新产品按钮之一。 此时，我们需要以某种方式保存到每个行的用户输入值`ProductsDataTable`实例，然后将其传递到 BLL 方法，然后将传递`ProductsDataTable`实例与 DAL 的`UpdateWithTransaction`方法。 `UpdateWithTransaction`方法，我们在中创建[前面教程](wrapping-database-modifications-within-a-transaction-vb.md)，可确保成批的更改，将更新作为一个原子操作。

创建一个名为方法`BatchUpdate`中`BatchUpdate.aspx.vb`并添加以下代码：


[!code-vb[Main](batch-updating-vb/samples/sample5.vb)]

此方法会启动通过获取所有产品进来`ProductsDataTable`通过 BLL 的调用`GetProducts`方法。 然后枚举`ProductGrid`GridView s [ `Rows`集合](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx)。 `Rows`集合包含[`GridViewRow`实例](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx)GridView 中显示每个行。 由于我们展示最多十个行，每页、 GridView 的`Rows`集合将有不能超过 10 个项。

为每个行`ProductID`从只张开`DataKeys`集合和相应`ProductsRow`从选择`ProductsDataTable`。 以编程方式引用四个 TemplateField 输入的控件和它们的值分配给`ProductsRow`实例 s 属性。 在每个 GridView 后行的值已用于更新`ProductsDataTable`，它 s 传递给 BLL s`UpdateWithTransaction`方法，正如我们看到在前面的教程中，只需调用该向下到 DAL 的`UpdateWithTransaction`方法。

本教程使用的批处理更新算法更新中的每一行`ProductsDataTable`相对应的 GridView，无论是否已更改的产品的信息中的行。 虽然此类 blind 更新不 t 通常性能问题，它们将可能导致的多余的记录，如果你重新审核更改为数据库表。 返回[执行批处理更新](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md)教程，我们介绍了使用 DataList 更新接口一批，并添加了代码，只会更新已被用户实际修改的那些记录。 请尝试使用从技术[执行批处理更新](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md)在本教程中，更新代码，如果需要。

> [!NOTE]
> 当数据源绑定到 GridView 通过其智能标记时，Visual Studio 将自动分配数据源 s 主值给 GridView 的`DataKeyNames`属性。 如果你未绑定 ObjectDataSource 到通过 GridView s 智能标记 GridView 步骤 1 中所述，则将需要手动设置 GridView s`DataKeyNames`属性才能访问 ProductID`ProductID`通过每行的值`DataKeys`集合。


中使用的代码`BatchUpdate`类似于在 BLL s 中使用`UpdateProduct`方法，主要区别在于，在`UpdateProduct`方法只有一个`ProductRow`从体系结构检索实例。 将分配的属性的代码`ProductRow`之间相同`UpdateProducts`方法和中的代码内`For Each`循环中`BatchUpdate`，如是整体的模式。

若要完成本教程，我们需要将`BatchUpdate`方法时调用的任一更新产品按钮单击。 创建事件处理程序`Click`事件的这两个按钮控件，然后在事件处理程序添加以下代码：


[!code-vb[Main](batch-updating-vb/samples/sample6.vb)]

第一次调用`BatchUpdate`。 接下来， [ `ClientScript`属性](https://msdn.microsoft.com/library/system.web.ui.page.clientscript(VS.80).aspx)用于插入将显示已更新的产品中读取一个消息框的 JavaScript。

花一些时间来测试此代码。 请访问`BatchUpdate.aspx`通过浏览器中，编辑的行数，然后单击更新产品按钮之一。 假设没有输入的验证错误，你应看到已更新的产品中读取一个消息框。 若要验证更新的原子性，请考虑添加随机`CHECK`约束，如其中一个不允许`UnitPrice`1234.56 的值。 然后从`BatchUpdate.aspx`，编辑的记录，并确保设置一个产品的数`UnitPrice`禁止使用的值 (1234.56) 的值。 在该批处理操作期间所做的更改与单击更新产品回滚到其原始值时，应导致错误。

## <a name="an-alternativebatchupdatemethod"></a>一种替代方法`BatchUpdate`方法

`BatchUpdate`刚刚的方法检查检索*所有*BLL s 中的产品的`GetProducts`方法，然后更新这些记录中 GridView 出现。 如果 GridView 不使用分页，但如果是这样，可能有数百、 千或成千上万的产品，但在 GridView 的十个行，此方法很理想。 在这种情况下，从仅要修改其中的 10 的数据库中获取所有产品都是小于理想。

对于这些类型的情况下，请考虑使用以下`BatchUpdateAlternate`方法改为：


[!code-vb[Main](batch-updating-vb/samples/sample7.vb)]

`BatchMethodAlternate`通过创建新的空启动`ProductsDataTable`名为`products`。 然后，它通过 GridView 的步骤`Rows`集合并为每个行获取使用 BLL s 的特定产品信息`GetProductByProductID(productID)`方法。 检索`ProductsRow`实例具有与相同的方式更新其属性`BatchUpdate`，但在更新导入到的行后`products``ProductsDataTable`通过 DataTable s [ `ImportRow(DataRow)`方法](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

后`For Each`循环完成后，`products`包含一个`ProductsRow`GridView 中的每一行的实例。 由于每个的`ProductsRow`实例已添加到`products`（而不是更新），如果我们盲目地将其传递到`UpdateWithTransaction`方法`ProductsTableAdatper`将尝试将每个记录插入数据库。 相反，我们需要指定，每个这些行已被修改 （未添加）。

这可以通过将新方法添加到名为 BLL `UpdateProductsWithTransaction`。 `UpdateProductsWithTransaction`下面，显示的集`RowState`的每个`ProductsRow`实例`ProductsDataTable`到`Modified`然后传递`ProductsDataTable`到 DAL 的`UpdateWithTransaction`方法。


[!code-vb[Main](batch-updating-vb/samples/sample8.vb)]

## <a name="summary"></a>摘要

GridView 提供内置的每个行的编辑功能，但缺少对创建完全可编辑的接口的支持。 正如我们看到在本教程中，此类接口是可能的但需要不少的工作。 若要创建的每个行是可编辑一个 GridView，我们需要将 GridView 的字段转换为 TemplateFields 和定义中的编辑界面`ItemTemplate`s。 此外，更新所有的按钮 Web 控件的类型必须添加到页上，独立于 GridView。 这些按钮`Click`事件处理程序需要枚举 GridView s`Rows`集合，存储中的更改`ProductsDataTable`，并将更新的信息传递到相应的 BLL 方法。

在下一步的教程，我们将了解如何创建用于批处理删除的接口。 具体而言，每个 GridView 行将包括一个复选框，而不是更新所有-键入按钮，我们必须删除所选行按钮。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Teresa 墨和 David Suru。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](wrapping-database-modifications-within-a-transaction-vb.md)
[下一页](batch-deleting-vb.md)
