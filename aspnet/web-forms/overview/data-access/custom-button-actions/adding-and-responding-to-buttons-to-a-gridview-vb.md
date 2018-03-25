---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: "添加和响应的按钮添加到 GridView (VB) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将查看如何添加自定义按钮，同时向模板和 GridView 或说明控件的字段。 具体而言，我们将标示..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 8a642a9a8e25d64028df0b5d8741da3008700652
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>添加和响应的按钮添加到 GridView (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe)或[下载 PDF](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> 在本教程中我们将查看如何添加自定义按钮，同时向模板和 GridView 或说明控件的字段。 具体而言，我们将生成具有允许用户翻阅供应商 FormView 的接口。


## <a name="introduction"></a>介绍

虽然许多报告方案涉及只读访问报表数据，它用于报表包括执行操作的能力不罕见 s 取决于显示的数据。 这通常涉及与报表中显示每个记录中添加按钮、 LinkButton 或 ImageButton Web 控件，单击时，会导致回发，并调用某些服务器端代码。 编辑和删除的记录的基础上的数据是最常见的示例。 实际上，正如我们所看到开头[概述的插入、 更新和删除数据](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)教程中，编辑和删除非常常见的 GridView、 说明如何和 FormView 控件可以支持而无需此类功能需要编写任何代码。

此外进行编辑和删除按钮、 GridView，说明如何和 FormView 控件还可以包括按钮、 LinkButtons 或 ImageButtons，单击时，执行某些自定义服务器端逻辑。 在本教程中我们将查看如何添加自定义按钮，同时向模板和 GridView 或说明控件的字段。 具体而言，我们将生成具有允许用户翻阅供应商 FormView 的接口。 对于给定的供应商，FormView 将显示有关以及如果单击，会将标记所有其关联的产品为停止使用的按钮 Web 控件供应商的信息。 此外，一个 GridView 列出所选供应商提供的其中每一行包含增加价格和折扣价格的按钮，如果单击，引发或减少产品 s 这些产品`UnitPrice`（请参见图 1） 的 10%。


[![FormView 和 GridView 包含执行自定义操作的按钮](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**图 1**： 这两个 FormView 和 GridView 包含按钮，执行自定义操作 ([单击以查看实际尺寸的图像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>步骤 1： 添加按钮教程 Web 页

我们探讨如何添加自定义按钮之前，让 s 先花点时间在本教程中我们将需要我们网站项目中创建 ASP.NET 页。 首先，通过添加一个名为的新文件夹`CustomButtons`。 接下来，将以下两个 ASP.NET 页添加到该文件夹，并确保将每个包含网页相关联`Site.master`母版页：

- `Default.aspx`
- `CustomButtons.aspx`


![有关自定义的按钮相关教程添加的 ASP.NET 页面](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**图 2**： 添加 ASP.NET 页的自定义按钮相关教程


在其他文件夹中，如`Default.aspx`中`CustomButtons`文件夹将在其部分中列出这些教程。 回想一下，`SectionLevelTutorialListing.ascx`用户控件提供此功能。 因此，此用户将控件添加到`Default.aspx`通过从解决方案资源管理器拖到页面上的设计视图拖动。


[![SectionLevelTutorialListing.ascx 用户控件添加到 Default.aspx](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**图 3**： 添加`SectionLevelTutorialListing.ascx`用户控件`Default.aspx`([单击以查看实际尺寸的图像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))


最后，将页添加到的条目为`Web.sitemap`文件。 具体而言，在分页和排序后添加以下标记`<siteMapNode>`:


[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

在更新后`Web.sitemap`，花些时间查看通过浏览器网站的教程。 在左侧菜单现在包括用于编辑、 插入和删除教程的项。


![站点图现在包括自定义按钮教程的条目](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**图 4**： 站点图现在包括自定义按钮教程的条目


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>步骤 2： 添加列出供应商 FormView

让我们来开始使用本教程通过添加列出供应商 FormView。 如简介中所述，此 FormView 将允许用户逐页浏览供应商，显示在一个 GridView 供应商提供的产品。 此外，此 FormView 将包括一个按钮，单击时，会将标记所有供应商的产品为停止使用。 我们考虑自行向 FormView 添加自定义按钮之前，让我们来首先就可以创建 FormView，使它显示供应商信息。

首先打开`CustomButtons.aspx`页面`CustomButtons`文件夹。 通过从工具箱中拖动到设计器和组拖动到页面添加 FormView 其`ID`属性`Suppliers`。 从 FormView s 智能标记中，选择创建名为新 ObjectDataSource `SuppliersDataSource`。


[![创建名为 SuppliersDataSource 新对象数据源](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**图 5**： 创建新对象数据源命名`SuppliersDataSource`([单击以查看实际尺寸的图像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))


配置此新对象数据源，以便它从查询`SuppliersBLL`类的`GetSuppliers()`方法 （请参阅图 6）。 由于此 FormView 不提供用于更新供应商信息，请选择 (None) 选项从下拉列表中的更新选项卡中的接口。


[![配置数据源以使用 SuppliersBLL 类的 GetSuppliers() 方法](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**图 6**： 配置数据源以使用`SuppliersBLL`类 s`GetSuppliers()`方法 ([单击以查看实际尺寸的图像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))


配置之后 ObjectDataSource，Visual Studio 将生成`InsertItemTemplate`， `EditItemTemplate`，和`ItemTemplate`FormView 有关。 删除`InsertItemTemplate`和`EditItemTemplate`和修改`ItemTemplate`，使其显示仅供应商的公司名称和电话号码。 最后，通过检查从其智能标记启用分页复选框开启 FormView 的分页支持 (或通过设置其`AllowPaging`属性`True`)。 这些更改后你页面 s 声明性标记应类似于以下：


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

图 7 显示 CustomButtons.aspx 页查看通过浏览器时。


[![FormView 列出 CompanyName 和从当前所选供应商的电话号码字段](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**图 7**: FormView 列出`CompanyName`和`Phone`中当前选定供应商的字段 ([单击以查看实际尺寸的图像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>步骤 3： 添加一个 GridView，列出所选供应商的产品

我们将停止所有产品的按钮都添加到 FormView 的模板之前，让我们来首先都添加下方列出的所选供应商提供的产品 FormView GridView。 若要实现此目的，向页面添加一个 GridView，设置其`ID`属性`SuppliersProducts`，并添加名为新 ObjectDataSource `SuppliersProductsDataSource`。


[![创建名为 SuppliersProductsDataSource 新对象数据源](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**图 8**： 创建新对象数据源命名`SuppliersProductsDataSource`([单击以查看实际尺寸的图像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))


配置此对象数据源以使用 ProductsBLL 类的`GetProductsBySupplierID(supplierID)`方法 （请参阅图 9）。 它获胜时针对产品 s 的价格，以便可以调整将允许此 GridView，t 使用内置编辑或删除从 GridView 的功能。 因此，我们可以设置为 (None) 下拉列表 ObjectDataSource s 更新、 插入和删除选项卡。


[![配置数据源以使用 ProductsBLL 类的 GetProductsBySupplierID(supplierID) 方法](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**图 9**： 配置数据源以使用`ProductsBLL`类 s`GetProductsBySupplierID(supplierID)`方法 ([单击以查看实际尺寸的图像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))


由于`GetProductsBySupplierID(supplierID)`方法接受一个输入的参数，ObjectDataSource 向导将提示我们为此参数值的源。 若要传入`SupplierID`从 FormView 值时，请将参数源下拉列表设置为控制和到 ControlID 下拉列表`Suppliers`（在步骤 2 中创建的 FormView ID）。


[![指示，供应商 Id 参数应来自供应商 FormView 控件](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**图 10**： 指示 *`supplierID`* 参数应来自`Suppliers`FormView 控件 ([单击以查看实际尺寸的图像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))


完成对象数据源向导后，命令将 BoundField 或 CheckBoxField 为每个产品的数据字段包含 GridView。 允许 s 削减这以显示仅`ProductName`和`UnitPrice`连同 BoundFields `Discontinued` CheckBoxField; 此外，让 s 格式`UnitPrice`BoundField 以便其文本设置为货币的格式。 你 GridView 和`SuppliersProductsDataSource`ObjectDataSource s 声明性标记应类似于以下标记：


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

此时我们的教程中显示一个主/详细信息报告，允许用户从顶部 FormView 选取供应商，并查看通过底部 GridView 该供应商提供的产品。 图 11 显示此页的屏幕截图时从 FormView 选择东京 Traders 供应商。


[![选择供应商的产品将显示在 GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**图 11**: 选择供应商的产品将显示在 GridView ([单击以查看实际尺寸的图像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>步骤 4： 创建 DAL 和 BLL 方法停止所有产品的供应商

我们可以将按钮添加到 FormView 之前，单击时，终止所有的供应商的产品中，我们首先需要将方法添加到的 DAL 和执行此操作的 BLL。 具体而言，此方法将被命名为`DiscontinueAllProductsForSupplier(supplierID)`。 单击 FormView 的按钮后，我们将调用此方法在业务逻辑层，通过在所选供应商 s `SupplierID`; BLL 将然后向下调用到相应的数据访问层方法，将发出`UPDATE`语句终止指定供应商的产品数据库。

与我们在我们前面的教程，我们将使用自下而上的方法，启动与创建 DAL 方法，然后 BLL 方法，最后在 ASP.NET 页中实现功能。 打开`Northwind.xsd`中的类型化数据集`App_Code/DAL`文件夹并添加一个新方法`ProductsTableAdapter`(右键单击`ProductsTableAdapter`，然后选择添加查询)。 这样将显示 TableAdapter 查询配置向导中，我们将指导完成添加新方法的过程。 首先，该值指示我们 DAL 方法将使用的临时 SQL 语句。


[![创建使用临时 SQL 语句的 DAL 方法](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**图 12**： 创建 DAL 方法使用临时 SQL 语句 ([单击以查看实际尺寸的图像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))


接下来，向导会提示有关哪种类型的查询以创建我们。 由于`DiscontinueAllProductsForSupplier(supplierID)`方法将需要更新`Products`数据库表，设置`Discontinued`字段为 1。 对于指定提供的所有产品 *`supplierID`* ，我们需要创建更新数据的查询。


[![选择更新的查询类型](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**图 13**： 选择更新的查询类型 ([单击以查看实际尺寸的图像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))


下一步的向导屏幕提供 TableAdapter s 现有`UPDATE`语句，这将更新每个定义中的字段`Products`DataTable。 此查询文本替换为以下语句：


[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

输入此查询并单击下一步，最后一个向导屏幕要求提供的新方法的名称后使用`DiscontinueAllProductsForSupplier`。 通过单击完成按钮完成向导。 返回到数据集设计器时，您应看到中的新方法`ProductsTableAdapter`名为`DiscontinueAllProductsForSupplier(@SupplierID)`。


[![新的 DAL 方法 DiscontinueAllProductsForSupplier 名称](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**图 14**： 将新的 DAL 方法`DiscontinueAllProductsForSupplier`([单击以查看实际尺寸的图像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))


与`DiscontinueAllProductsForSupplier(supplierID)`在数据访问层中创建的方法，我们的下一个任务是创建`DiscontinueAllProductsForSupplier(supplierID)`业务逻辑层中的方法。 若要完成此操作，打开`ProductsBLL`类文件并添加以下：


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

此方法只需调用直至`DiscontinueAllProductsForSupplier(supplierID)`中 DAL，沿提供传递方法 *`supplierID`* 参数值。 如果没有任何要在某些情况下停用的 s 产品，只允许一个供应商的业务规则，这些规则应 BLL 在此处，实现。

> [!NOTE]
> 与不同`UpdateProduct`重载以`ProductsBLL`类，`DiscontinueAllProductsForSupplier(supplierID)`方法签名不包括`DataObjectMethodAttribute`属性 (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`)。 这将阻止`DiscontinueAllProductsForSupplier(supplierID)`从 ObjectDataSource s 配置数据源向导的下拉列表中更新选项卡的方法。我已省略此属性，因为我们将调用`DiscontinueAllProductsForSupplier(supplierID)`直接从我们 ASP.NET 页中的事件处理方法。


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>步骤 5： 添加停止 FormView 到的所有产品按钮

与`DiscontinueAllProductsForSupplier(supplierID)`中 BLL 和 DAL 方法完成，最后一个步骤添加为所选供应商是将按钮 Web 控件添加到 FormView s 停止所有产品的功能`ItemTemplate`。 允许 s 添加与按钮文本，停止所有产品的供应商的电话号码下面这样的按钮和`ID`属性值`DiscontinueAllProductsForSupplier`。 你可以通过单击 FormView s 智能标记中的编辑模板链接来添加此按钮 Web 控件通过设计器 （请参阅图 15），或直接通过声明性语法。


[![添加停止所有产品按钮 Web 控件到 FormView 的 ItemTemplate](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**图 15**： 将停止所有产品按钮 Web 控件都添加到 FormView s `ItemTemplate` ([单击以查看实际尺寸的图像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))


当通过页上，回发时，才会用户访问和 FormView s 单击该按钮[`ItemCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx)激发。 若要执行自定义代码以响应单击此按钮，我们可以创建的事件处理程序此事件。 理解，不过，这`ItemCommand`事件时将触发*任何*FormView 内单击按钮、 LinkButton 或 ImageButton Web 控件。 这意味着，当用户从一页移到另一种 FormView，`ItemCommand`事件激发; 当用户单击新建，编辑，或删除支持插入、 更新或删除 FormView 中的内容相同。

由于`ItemCommand`激发而不考虑哪些按钮后，事件处理程序我们需要一种方法，以确定被单击了停止所有的产品按钮，或者如果它已某些其他按钮。 若要实现此目的，我们可以设置按钮 Web 控件的`CommandName`为某个标识值的属性。 单击按钮时，这`CommandName`值传递给`ItemCommand`事件处理程序，使我们能够确定是否停止所有的产品按钮所单击的按钮。 将停止所有产品按钮 s 设置`CommandName`DiscontinueProducts 属性。

最后，让我们来使用客户端确认对话框中，以确保用户确实想要停止所选供应商的产品。 正如我们看到在[删除时添加客户端确认](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md)教程，则此操作可以稍加 JavaScript 完成。 具体而言，将按钮 Web 控件的 OnClientClick 属性设置为`return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

进行这些更改后，FormView s 声明性语法应如下所示：


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

接下来，创建的事件处理程序 FormView 的`ItemCommand`事件。 在此事件处理程序，我们需要首先确定是否已单击停止所有的产品按钮。 如果这样，我们想要创建的实例`ProductsBLL`类并调用其`DiscontinueAllProductsForSupplier(supplierID)`方法，同时传入`SupplierID`的所选 FormView:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

请注意， `SupplierID` FormView 中当前选定的供应商可以使用访问 FormView s [ `SelectedValue`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx)。 `SelectedValue`属性返回的第一个数据键显示在 FormView 该记录的值。 FormView s [ `DataKeyNames`属性](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx)，指示的数据字段的数据从其键值值来自，已自动设置为`SupplierID`由 Visual Studio 时，绑定到 FormView 后的对象数据源在步骤 2 中。

与`ItemCommand`事件处理程序创建，花些时间测试的页。 浏览到 Cooperativa de Quesos 的 Cabras 供应商 (它 s 中为我 FormView 第五个供应商)。 此供应商可提供两种产品，Queso Cabrales 和 Queso Manchego La Pastora，二者都是*不*停止使用。

假设 Cooperativa de Quesos 的 Cabras 已超出业务，因此要停用其产品。 单击停止所有产品按钮。 这将显示客户端确认对话框框中 （请参阅图 16）。


[![Cooperativa de Quesos 的 Cabras 提供两个活动产品](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**图 16**: Cooperativa de Quesos 的 Cabras 提供两个活动产品 ([单击以查看实际尺寸的图像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))


如果单击确定在客户端确认对话框中，表单提交将继续，在其中导致回发 FormView 的`ItemCommand`事件将激发。 我们创建的事件处理程序然后执行调用`DiscontinueAllProductsForSupplier(supplierID)`方法和中止 Queso Cabrales 和 Queso Manchego La Pastora 产品。

如果你已禁用的 GridView 的视图状态，GridView 是正在重新绑定的到基础数据存储在每个回发时，并因此将立即更新以反映，这两个产品现在不再 （请参阅图 17）。 如果，但是，不具有已禁用 GridView 中的视图状态，你将需要手动进行此更改后重新绑定到 GridView 的数据。 要完成此操作，只需进行到 GridView 的调用`DataBind()`方法调用后立即`DiscontinueAllProductsForSupplier(supplierID)`方法。


[![单击停止所有的产品按钮后，供应商的产品更新相应地受到](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**图 17**： 单击停止所有的产品按钮后，供应商的产品更新相应地受到 ([单击以查看实际尺寸的图像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>步骤 6： 在调整产品的价格的业务逻辑层中创建的 UpdateProduct 重载

如与停止所有产品中的按钮 FormView，以添加用于增加和减少 GridView 中的产品的价格按钮我们需要先添加适当的数据访问层和业务逻辑层方法。 由于我们已有更新 DAL 中的单个产品行的方法，我们可以通过创建新的重载，以提供此类功能`UpdateProduct`BLL 中的方法。

我们过去`UpdateProduct`重载取得产品字段的某种组合为标量的输入值，然后更新了指定的产品仅对这些字段。 此重载中，我们将略有不同于此标准然后改为将传入产品 s`ProductID`和用来调整百分比`UnitPrice`(而不是在新的传递，调整`UnitPrice`本身)。 此方法将简化我们需要在 ASP.NET 页的代码隐藏类中编写的代码，因为我们不需要涉及确定当前产品 s t `UnitPrice`。

`UpdateProduct`重载为本教程中所示：


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

此重载检索有关 DAL s 通过指定的产品信息`GetProductByProductID(productID)`方法。 然后，它将检查以确定是否产品 s`UnitPrice`分配数据库`NULL`值。 如果是，价格将保持不变。 如果，但是，没有非`NULL``UnitPrice`值，该方法更新产品 s`UnitPrice`由指定的百分比 (`unitPriceAdjustmentPercent`)。

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>步骤 7： 将增加和减少按钮添加到 GridView

GridView （和说明如何） 都组成的字段的集合。 除了 BoundFields、 CheckBoxFields 和 TemplateFields，ASP.NET 还包括 ButtonField，来顾名思义，呈现为带有按钮、 LinkButton 或 ImageButton 每一行的列。 类似于 FormView，单击*任何*GridView 分页按钮、 编辑或删除按钮、 排序按钮等中的按钮导致回发，并引发 GridView s [ `RowCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx)。

ButtonField 具有`CommandName`属性，将指定的值分配给每个其按钮`CommandName`属性。 FormView，与类似`CommandName`值由`RowCommand`事件处理程序来确定被单击的按钮。

允许 s 将两个新 ButtonFields 添加到 GridView，一个按钮文本价格 + 10%和另一种具有文本价格-10%。 若要添加这些 ButtonFields，单击从 GridView s 智能标记的编辑列链接，从左上方列表中选择 ButtonField 字段类型，然后单击添加按钮。


![将两个 ButtonFields 添加到 GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**图 18**： 将两个 ButtonFields 添加到 GridView


移动两个 ButtonFields，以便它们显示为前两个 GridView 字段。 接下来，设置`Text`这些以价格 + 10%的两个 ButtonFields 和价格-10 的属性 %和`CommandName`为 IncreasePrice 和 DecreasePrice，属性分别。 默认情况下，ButtonField 呈现 LinkButtons 按钮其列。 可进行更改，但是，通过 ButtonField s [ `ButtonType`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx)。 让具有呈现为常规下压按钮; 这些两个 ButtonFields 的 s因此，设置`ButtonType`属性`Button`。 图 19 显示的字段对话框中进行这些更改; 之后以下的是 GridView s 声明性标记。


![配置 ButtonFields 文本、 CommandName 和 ButtonType 属性](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**图 19**： 配置 ButtonFields `Text`， `CommandName`，和`ButtonType`属性


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

与创建这些 ButtonFields，最后一步是创建的事件处理程序 GridView 的`RowCommand`事件。 此事件处理程序，如果引发因为任一价格 + 10%或价格-10%按钮已单击，需要确定`ProductID`行其按钮被单击，然后调用`ProductsBLL`类的`UpdateProduct`传递在相应的方法`UnitPrice`连同百分比调整`ProductID`。 下面的代码执行这些任务：

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

若要确定`ProductID`行其价格 + 10%或价格-10%按钮被单击，我们需要参考 GridView 的`DataKeys`集合。 此集合保存中指定的字段的值`DataKeyNames`为每个 GridView 行的属性。 由于 GridView s`DataKeyNames`属性设置为 ProductID 由 Visual Studio 时，绑定到 GridView，ObjectDataSource`DataKeys(rowIndex).Value`提供`ProductID`指定*rowIndex*。

中会自动将传递 ButtonField *rowIndex*通过其按钮的行`e.CommandArgument`参数。 因此，若要确定`ProductID`行其价格 + 10%或价格-10%按钮被单击，我们使用： `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`。

使用停止所有产品按钮，如果你已禁用的 GridView 的视图状态，GridView 是正在重新绑定的到基础数据存储在每个回发时，以及因此将立即更新以反映，从单击发生的价格更改任一按钮。 如果，但是，不具有已禁用 GridView 中的视图状态，你将需要手动进行此更改后重新绑定到 GridView 的数据。 要完成此操作，只需进行到 GridView 的调用`DataBind()`方法调用后立即`UpdateProduct`方法。

图 20 查看祖母 Kelly 家居所提供的产品时显示的页。 图 21 显示结果后价格 + 10%按钮单击后两次以祖母梅分布和价格-10%按钮一次为 Cranberry 胡椒粉。


[![GridView 包括价格 + 10%和价格-10%按钮](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**图 20**: GridView 包括价格 + 10%和价格-10%按钮 ([单击以查看实际尺寸的图像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))


[![第一个和第三个产品的价格已更新通过价格 + 10%和价格-10%按钮](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**图 21**: 价格的第一个和第三个产品已更新通过价格 + 10%和价格-10%按钮 ([单击以查看实际尺寸的图像](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))


> [!NOTE]
> 按钮、 LinkButtons 或添加到其 TemplateFields ImageButtons 还可以具有的 GridView （和说明）。 如与 BoundField 这些按钮，单击时，将引入回发，则引发 GridView 的`RowCommand`事件。 当添加中的按钮为 TemplateField，但是，按钮的`CommandArgument`未自动设置为行的索引按原样使用 ButtonFields 时。 如果你需要确定被单击中的按钮的行索引`RowCommand`事件处理程序，你将需要手动设置按钮的`CommandArgument`TemplateField，使用如下代码在其声明性语法中的属性：  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`。


## <a name="summary"></a>摘要

所有的 GridView、 说明如何和 FormView 控制可以包括按钮、 LinkButtons 或 ImageButtons。 此类按钮，单击时，会导致回发和引发`ItemCommand`FormView 和说明如何控件中的事件和`RowCommand`GridView 中的事件。 这些数据 Web 控件具有内置功能来处理常见的命令相关的操作，如删除或编辑记录。 但是，我们可以还使用按钮，单击时，使用执行自己的自定义代码进行响应。

若要实现此目的，我们需要创建的事件处理程序`ItemCommand`或`RowCommand`事件。 此事件处理程序中我们首先检查传入`CommandName`值以确定被单击的按钮，然后采取相应的自定义操作。 在本教程中，我们将了解如何使用按钮和 ButtonFields 停止指定的供应商的所有产品或用于增加或减少 10%的特定产品的价格。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一篇](adding-and-responding-to-buttons-to-a-gridview-cs.md)
