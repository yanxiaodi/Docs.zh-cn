---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
title: "插入、 更新和删除数据 (VB) 的概述 |Microsoft 文档"
author: rick-anderson
description: "在本教程中，我们将了解如何映射 ObjectDataSource Insert()，update （)，以及对方法的 BLL delete （） 方法类，以及如何 configu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 35b40b8f-2ca8-4ab3-9c19-f361a91a3647
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
msc.type: authoredcontent
ms.openlocfilehash: e7552abb30aa26d3aaceb3312c00661c6d4d6cf8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="an-overview-of-inserting-updating-and-deleting-data-vb"></a>插入、 更新和删除数据 (VB) 的概述
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_VB.exe)或[下载 PDF](an-overview-of-inserting-updating-and-deleting-data-vb/_static/datatutorial16vb1.pdf)

> 在本教程中，我们将了解如何映射 ObjectDataSource Insert()，update （)，并于 BLL 的方法的 delete （） 方法的类，以及如何配置提供数据修改功能的 GridView、 说明如何和 FormView 控件。


## <a name="introduction"></a>介绍

通过过去的几个教程，我们探讨了如何在 ASP.NET 页使用 GridView、 说明，和 FormView 控件中显示数据。 这些控件只需处理提供给它们的数据。 通常情况下，这些控件访问通过使用数据源控件，如对象数据源的数据。 我们已了解如何 ObjectDataSource 充当 ASP.NET 页和基础数据之间的代理。 当一个 GridView 需要显示数据时，将调用其 ObjectDataSource`Select()`方法，反过来调用的方法从我们业务逻辑层 (BLL)，该调用的相应数据访问层的 (DAL) 的方法 TableAdapter，后者反过来将发送`SELECT`包含到 Northwind 数据库的查询。

回想一下，我们在 DAL 在中创建 Tableadapter 时[我们第一个教程](../introduction/creating-a-data-access-layer-cs.md)、 Visual Studio 自动添加用于插入，方法更新和删除数据从基础数据库表。 此外，在[创建业务逻辑层](../introduction/creating-a-business-logic-layer-vb.md)我们设计调用 BLL 中的方法应用到这些数据修改 DAL 方法。

除了其`Select()`方法，ObjectDataSource 还有`Insert()`， `Update()`，和`Delete()`方法。 如`Select()`方法，这三种方法可以映射到基础对象中的方法。 配置为插入、 更新或删除数据时，GridView、 说明，和 FormView 控件用于修改基础数据提供一个用户界面。 此用户界面调用`Insert()`， `Update()`，和`Delete()`方法的对象数据源，然后调用基础对象的关联的方法 （请参见图 1）。


[![ObjectDataSource Insert()、 update （） 和 delete （） 方法用作代理到 BLL](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image1.png)

**图 1**: ObjectDataSource `Insert()`， `Update()`，和`Delete()`方法用作代理，BLL ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image3.png))


在本教程中，我们将了解如何映射 ObjectDataSource `Insert()`， `Update()`，和`Delete()`BLL，以及如何配置 GridView、 说明如何和 FormView 控件，以提供数据修改中类方法的方法功能。

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>步骤 1： 创建插入、 更新和删除教程网页

我们开始浏览如何插入、 更新和删除数据之前，让我们先花一些时间，我们需要用于本教程和下一步的几个的网站项目中创建 ASP.NET 页。 首先，通过添加一个名为的新文件夹`EditInsertDelete`。 接下来，添加到该文件夹，并确保将每个包含网页相关联的以下 ASP.NET 页面`Site.master`母版页：

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![有关数据修改相关教程添加的 ASP.NET 页面](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image4.png)

**图 2**： 添加 ASP.NET 页的数据修改相关教程


在其他文件夹中，如`Default.aspx`中`EditInsertDelete`文件夹将在其部分中列出这些教程。 回想一下，`SectionLevelTutorialListing.ascx`用户控件提供此功能。 因此，此用户将控件添加到`Default.aspx`通过从解决方案资源管理器拖放到页面的设计视图拖动。


[![SectionLevelTutorialListing.ascx 用户控件添加到 Default.aspx](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image5.png)

**图 3**： 添加`SectionLevelTutorialListing.ascx`用户控件`Default.aspx`([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image7.png))


最后，将页添加到的条目为`Web.sitemap`文件。 具体而言，在自定义格式化后添加以下标记`<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample1.xml)]

在更新后`Web.sitemap`，花些时间查看通过浏览器网站的教程。 在左侧菜单现在包括用于编辑、 插入和删除教程的项。


![站点图现在包括用于编辑、 插入和删除教程项](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image8.png)

**图 4**： 站点图现在包括用于编辑、 插入和删除教程项


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>步骤 2： 添加和配置 ObjectDataSource 控件

由于 GridView，说明如何和在其数据修改功能和布局每个不同的 FormView，让我们分别检查每个。 而不是将每个控件使用其自己的对象数据源，但是，让我们就可以创建所有三个控件示例可以共享单个对象数据源。

打开`Basics.aspx`页上，将 ObjectDataSource 拖动从工具箱中拖动到设计器中，并单击其智能标记中的配置数据源链接。 由于`ProductsBLL`是提供编辑、 插入和删除方法，配置对象数据源以使用此类的唯一 BLL 类。


[![配置对象数据源以使用 ProductsBLL 类](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image9.png)

**图 5**： 配置使用 ObjectDataSource`ProductsBLL`类 ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image11.png))


我们可以在下一个屏幕中指定的哪些方法`ProductsBLL`类映射到 ObjectDataSource `Select()`， `Insert()`， `Update()`，和`Delete()`通过选择相应的选项卡，然后从下拉列表中选择该方法。 图 6 中，应熟悉到目前为止，映射 ObjectDataSource`Select()`方法`ProductsBLL`类的`GetProducts()`方法。 `Insert()`， `Update()`，和`Delete()`方法可以通过从顶部列表中选择相应的选项卡来配置。


[![具有 ObjectDataSource 返回所有产品](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image12.png)

**图 6**： 具有 ObjectDataSource 返回所有产品的 ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image14.png))


图 7、 8 和 9 显示对象数据源的更新、 插入和删除选项卡。 配置这些选项卡，以便`Insert()`， `Update()`，和`Delete()`方法调用`ProductsBLL`类的`UpdateProduct`， `AddProduct`，和`DeleteProduct`方法，分别。


[![将对象数据源的 update （） 方法映射到 ProductBLL 类 UpdateProduct 方法](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image15.png)

**图 7**： 映射 ObjectDataSource`Update()`方法`ProductBLL`类的`UpdateProduct`方法 ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image17.png))


[![将对象数据源的 Insert() 方法映射到 ProductBLL 类 AddProduct 方法](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image18.png)

**图 8**： 映射 ObjectDataSource`Insert()`方法`ProductBLL`类的添加`Product`方法 ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image20.png))


[![将对象数据源的 delete （） 方法映射到 ProductBLL 类 DeleteProduct 方法](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image21.png)

**图 9**： 映射 ObjectDataSource`Delete()`方法`ProductBLL`类的`DeleteProduct`方法 ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image23.png))


你可能已经注意到的下拉列表中的更新、 INSERT 和删除选项卡已经有选择这些方法。 这是我们的使用感谢`DataObjectMethodAttribute`修饰的方法`ProducstBLL`。 例如，DeleteProduct 方法具有以下签名：


[!code-vb[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample2.vb)]

`DataObjectMethodAttribute`属性指示每个方法的目的，它是用于选择、 插入、 更新或删除和它是否是默认值。 如果你省略这些属性创建 BLL 类时，你将需要为手动从更新中，选择的方法插入和删除选项卡。

后确保相应`ProductsBLL`方法映射到 ObjectDataSource `Insert()`， `Update()`，和`Delete()`方法，单击完成以完成向导。

## <a name="examining-the-objectdatasources-markup"></a>检查对象数据源的标记

在配置后通过其向导 ObjectDataSource，转到源视图以检查生成的声明性标记。 `<asp:ObjectDataSource>`标记指定的基础对象和要调用的方法。 此外，还有`DeleteParameters`， `UpdateParameters`，和`InsertParameters`，它将映射到的输入参数`ProductsBLL`类的`AddProduct`， `UpdateProduct`，和`DeleteProduct`方法：


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample3.aspx)]

ObjectDataSource 包含一个参数为每个输入参数的关联方法，就像一份`SelectParameter`s 时的存在时配置 ObjectDataSource 调用要求输入的参数的选择方法 (如`GetProductsByCategoryID(categoryID)`). 正如我们将很快，看到这些值`DeleteParameters`， `UpdateParameters`，和`InsertParameters`自动和设置的 GridView，说明如何，FormView 之前调用 ObjectDataSource `Insert()`， `Update()`，或`Delete()`方法。 这些值在为在将来的教程，我们将讨论还可以根据需要以编程方式设置。

使用向导将配置为对象数据源的一个副作用是 Visual Studio 将设置[OldValuesParameterFormatString 属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx)到`original_{0}`。 此属性的值可用于包括正在编辑的数据的原始值，并在两种方案中非常有用：

- 如果在编辑记录时，用户就能够更改的主键值。 在这种情况下，新的主键值和原始的主键值必须提供，以便原始主键值的记录可以找到并将相应地更新其值。
- 当使用开放式并发。 乐观并发是一种技术，以确保两个同时进行的用户不会覆盖彼此的所做更改，并且是将来的教程主题。

`OldValuesParameterFormatString`属性指示的基础对象的更新和删除方法的原始值中的输入参数的名称。 我们探讨乐观并发，我们将讨论此属性，并且更详细介绍其用途。 我显示它现在，但是，因为我们 BLL 方法不需要原始值，因此很重要，我们删除此属性。 离开`OldValuesParameterFormatString`属性设置为默认值之外的任何内容 (`{0}`) 会导致错误，当尝试调用对象数据源的数据 Web 控件`Update()`或`Delete()`方法因为 ObjectDataSource 将尝试同时传入`UpdateParameters`或`DeleteParameters`以及原始值参数指定。

如果不是特别清除此时，别担心，我们将在将来的教程中检查此属性和它的实用。 现在，只是某些声明性语法中完全移除此属性声明或将值设置为默认值 （{0}）。

> [!NOTE]
> 如果你只需清除`OldValuesParameterFormatString`于在设计视图中，该属性的属性窗口的属性值将仍然存在于声明性语法，但将设置为空字符串。 此操作，请遗憾的是，仍会导致上述相同问题。 因此，删除属性完全声明性语法，或从属性窗口中，将值设置为默认情况下， `{0}`。


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>步骤 3： 添加数据 Web 控件，并将其配置针对数据修改

添加到页面并配置对象数据源后，我们已准备好将数据 Web 控件添加到页后，可以同时显示数据，并提供最终用户对其进行修改的一种方法。 我们将考察的 GridView，说明如何和 FormView 单独，因为这些数据 Web 控件在其数据修改功能和配置都不同。

我们将看到本文的其余部分中添加非常基本编辑、 插入和删除通过 GridView，说明如何，支持以及 FormView 控制是真正简单，只检查几个复选框。 有许多细微部分和在现实世界的边缘情况，以提供更多地涉及不是只需点，然后单击此类功能。 本教程中，但是，着重依赖证明非常简单的数据修改功能。 将来的教程将检查必定会在实际设置中出现的问题。

## <a name="deleting-data-from-the-gridview"></a>从 GridView 删除数据

通过从工具箱中拖动到设计器中拖动一个 GridView 启动。 接下来，将对象数据源绑定到 GridView，通过从 GridView 的智能标记中的下拉列表中选择它。 此时将为 GridView 的声明性标记：


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample4.aspx)]

绑定到通过其智能标记 ObjectDataSource 的 GridView 有两个好处：

- 自动为每个对象数据源返回的字段创建 BoundFields 和 CheckBoxFields。 此外，BoundField 和 CheckBoxField 的属性是根据设置的基础字段元数据。 例如， `ProductID`， `CategoryName`，和`SupplierName`字段都标记为只读中`ProductsDataTable`，因此不应是可更新在编辑时。 若要容纳此，则这些 BoundFields [ReadOnly 属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx)设置为`True`。
- [将字段名](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx)分配给基础对象的主键字段。 这是必不可少的时使用 GridView 进行编辑或删除数据，此属性指示的字段 （或集的字段），它唯一标识每个记录。 有关详细信息`DataKeyNames`属性，回头参考[母版/详细介绍使用详细信息 DetailView 可选择的主 GridView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)教程。

虽然 GridView 可以绑定到通过属性窗口或声明性语法，ObjectDataSource，执行此操作需要你手动添加相应的 BoundField 和`DataKeyNames`标记。

GridView 控件提供对行级别编辑和删除的内置支持。 配置以支持删除一个 GridView 添加一列删除按钮。 当最终用户单击特定行的删除按钮时，回发时，才会和 GridView 执行以下步骤：

1. ObjectDataSource`DeleteParameters`分配值
2. ObjectDataSource`Delete()`调用方法时，删除指定的记录
3. GridView 重新绑定次数本身 ObjectDataSource 到通过调用其`Select()`方法

分配给的值`DeleteParameters`的值`DataKeyNames`其删除按钮被单击的行的字段。 因此，它是重要的 GridView`DataKeyNames`正确设置属性。 如果缺少，`DeleteParameters`将被赋的值`Nothing`在步骤 1 中，这反过来不会导致在步骤 2 中任何已删除的记录。

> [!NOTE]
> `DataKeys`集合存储在 GridView 的控件状态，这意味着，`DataKeys`值将保存在回发，即使已禁用的 GridView 的视图状态。 但是，它是非常重要的视图状态的支持编辑或删除 （默认行为） 的 GridViews 将保持启用状态。 如果设置 GridView s`EnableViewState`属性`false`、 编辑和删除行为将效果不错，适用于单个用户，但如果没有删除数据的并发用户，存在这些并发的用户可能意外的可能性删除或编辑记录他们没有 t 想。 请参阅我博客文章[警告： 禁用并发问题与 ASP.NET 2.0 GridViews/说明/FormViews 该支持编辑和/或删除和其视图状态](http://scottonwriting.net/sowblog/posts/10054.aspx)，有关详细信息。


此相同警告也适用于 DetailsViews 和 FormViews。

若要将删除的功能添加到一个 GridView，只需转到其智能标记，并选中启用删除复选框。


![检查删除复选框启用](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image24.png)

**图 10**： 检查删除复选框启用


检查从智能标记启用删除复选框将 CommandField 添加到 GridView。 CommandField 呈现其中包括用于执行一个或多个以下任务按钮 GridView 中的列： 选择记录，编辑记录，并删除一条记录。 我们以前看到的那样操作选择中的记录中 CommandField[母版/详细介绍使用详细信息 DetailView 可选择的主 GridView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)教程。

CommandField 包含大量`ShowXButton`指示哪些系列按钮显示 CommandField 中的属性。 通过检查启用删除复选框 CommandField 其`ShowDeleteButton`属性是`True`已添加到 GridView 的列集合。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample5.aspx)]

此时，无论您相信与否，我们已经完成了将删除的支持添加到 GridView ！ 如图 11 所示，当访问此页通过浏览器的删除按钮列不存在。


[![CommandField 添加一列删除按钮](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image25.png)

**图 11**: CommandField 添加了列的删除按钮 ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image27.png))


如果您生成此教程组成的基本组件上你自己，测试单击此页时删除按钮将引发异常。 继续阅读以了解有关为什么引发这些异常以及如何修复它们。

> [!NOTE]
> 如果您一直使用下载随附本教程中，具有已说明这些问题。 但是，我建议你通读下面列出以帮助识别可能出现的问题和合适的解决方法的详细信息。


如果尝试删除产品时, 获取的异常的消息是类似于"*ObjectDataSource ObjectDataSource1 找不到非泛型方法 DeleteProduct 具有参数： productID，原始\_ProductID*，"你可能忘记删除`OldValuesParameterFormatString`从 ObjectDataSource 的属性。 与`OldValuesParameterFormatString`指定属性，ObjectDataSource 尝试传递中都`productID`和`original_ProductID`输入参数为`DeleteProduct`方法。 `DeleteProduct`但是，仅接受一个输入的参数，因此异常。 删除`OldValuesParameterFormatString`属性 (或将它设置为`{0}`) 指示 ObjectDataSource 不要尝试在原始的输入参数中传递。


[![确保 OldValuesParameterFormatString 属性已被清除](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image28.png)

**图 12**： 确保`OldValuesParameterFormatString`属性已被清除出 ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image30.png))


即使您已经删除`OldValuesParameterFormatString`属性，你仍将发生异常时尝试删除具有消息的产品:"*DELETE 语句与的引用约束冲突 FK\_顺序\_详细信息\_产品的*。"Northwind 数据库包含之间的外键约束`Order Details`和`Products`表，这意味着在有一个或多个记录中为其无法从系统删除一种产品`Order Details`表。 由于 Northwind 数据库中的每个产品具有至少一个记录`Order Details`，我们首先删除该产品的关联的订单详细信息记录之前，我们无法删除任何产品。


[![外键约束禁止将删除产品](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image31.png)

**图 13**： 外键约束禁止删除的产品 ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image33.png))


我们的教程，让我们只需删除所有从记录`Order Details`表。 在实际应用程序中，我们将需要为：

- 具有管理订单详细信息信息的另一个屏幕
- 增加`DeleteProduct`方法以包括逻辑来删除指定的产品的订单详细信息
- 修改 TableAdapter 用于包括删除指定的产品的订单详细信息的 SQL 查询

让我们只需删除所有从记录`Order Details`若要避免的外键约束的表。 请转到 Visual Studio 中的服务器资源管理器，右键单击`NORTHWND.MDF`节点，然后选择新查询。 然后，在查询窗口中，运行以下 SQL 语句：`DELETE FROM [Order Details]`


[![从详细信息表中删除所有记录](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image34.png)

**图 14**： 删除所有记录`Order Details`表 ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image36.png))


后清除`Order Details`表，单击删除按钮将删除产品而未出现错误。 如果单击删除按钮不会删除该产品，请检查以确保 GridView`DataKeyNames`属性设置为的主键字段 (`ProductID`)。

> [!NOTE]
> 单击删除按钮时回发时，才会并删除该记录。 这可能会十分危险，因为很容易意外单击错误行删除按钮。 在将来的教程中我们将了解如何添加客户端确认删除一条记录时。


## <a name="editing-data-with-the-gridview"></a>编辑数据与 GridView

以及删除 GridView 控件还提供内置的行级编辑支持。 配置一个 GridView，若要支持编辑添加编辑按钮的列。 从最终用户的角度来看，单击某一行的编辑按钮，将使该行成为可编辑，到文本框中包含的现有值并将与更新的编辑按钮和取消按钮启用单元格。 进行其所需的更改后，最终用户可以单击更新按钮以提交更改或丢弃这些取消按钮。 在任一情况下，单击更新或取消按钮后 GridView 恢复为其预编辑状态。

从作为页开发人员，我们透视时最终用户单击特定行的编辑按钮时，回发时，才会和 GridView 执行以下步骤：

1. GridView`EditItemIndex`属性分配给其编辑按钮被单击的行的索引
2. GridView 重新绑定次数本身 ObjectDataSource 到通过调用其`Select()`方法
3. 与匹配的行索引`EditItemIndex`呈现在"编辑模式。 在此模式下，编辑按钮替换为更新和取消按钮和 BoundFields 其`ReadOnly`属性为 False （默认值） 作为文本框中 Web 控件的呈现`Text`属性分配给数据字段的值。

在此点标记返回到浏览器中，这样就允许最终用户对行的数据进行任何更改。 当用户单击更新按钮时，产生的回发，GridView 执行以下步骤：

1. ObjectDataSource`UpdateParameters`值分配到 GridView 的编辑界面的最终用户输入的值
2. ObjectDataSource`Update()`调用方法时，更新指定的记录
3. GridView 重新绑定次数本身 ObjectDataSource 到通过调用其`Select()`方法

分配给主键值`UpdateParameters`在步骤 1 中来自中指定的值`DataKeyNames`属性，而非主键值来自中编辑过的行的文本框中 Web 控件的文本。 如与删除，它是至关重要的 GridView`DataKeyNames`正确设置属性。 如果缺少，`UpdateParameters`主键值将为其赋值的`Nothing`在步骤 1 中，这反过来不会导致在步骤 2 中的任何更新的记录。

可以通过只需检查 GridView 的智能标记中的启用编辑复选框激活编辑功能。


![检查编辑复选框启用](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image37.png)

**图 15**： 检查编辑复选框启用


检查启用编辑复选框将添加 CommandField （是否需要） 并设置其`ShowEditButton`属性`True`。 CommandField 前面看到的如包含大量的`ShowXButton`指示哪些系列按钮显示 CommandField 中的属性。 检查启用编辑复选框添加`ShowEditButton`现有 CommandField 属性：


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample6.aspx)]

这就是所有到添加基本的编辑支持。 编辑接口是相当不成熟如 Figure16 所示，每个 BoundField 其`ReadOnly`属性设置为`False`（默认值） 呈现为文本框。 这包括字段如`CategoryID`和`SupplierID`，这是与其他表的键。


[![单击牛奶的编辑按钮以编辑模式显示该行](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image38.png)

**图 16**： 单击牛奶 s 编辑按钮以编辑模式显示该行 ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image40.png))


除了要求用户直接编辑的外键值，通过以下方式缺少编辑接口的接口：

- 如果用户输入`CategoryID`或`SupplierID`不在数据库中，存在`UPDATE`将违反外键约束，会导致异常引发。
- 编辑接口不包括任何验证。 如果你未提供所需的值 (如`ProductName`)，或输入一个字符串值的数字值 （例如，输入"过多 ！"的地方 到`UnitPrice`文本框中)，将引发异常。 将来的教程将说明如何将验证控件添加到编辑的用户界面。
- 目前，*所有*不是只读的产品字段必须包含在 GridView。 如果我们 GridView 从中删除字段，说`UnitPrice`，当更新数据 GridView 将未设置`UnitPrice``UpdateParameters`值，该值更改的数据库记录`UnitPrice`到`NULL`值。 同样，如果必填字段，如`ProductName`，删除从 GridView，更新将具有相同失败"*列 ProductName 不允许使用 null*"上面提到的异常。
- 编辑接口格式使许多需要改进。 `UnitPrice`具有四个十进制点所示。 理想情况下`CategoryID`和`SupplierID`值将包含在系统中列出的类别和供应商的 DropDownLists。

这些是所有的不足之处，我们将需要进行实时现在，但将在将来的教程中得以解决。

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>插入、 编辑和删除说明如何使用数据

如我们所见在早期教程中，控件一次显示一条记录，并如 GridView，允许用于编辑和删除当前所显示的记录的说明。 进行编辑和删除说明和从 ASP.NET 端工作流中的项目的这两个最终用户的体验都是相同的 GridView。 说明如何 GridView 与不同的是它还提供了内置插入支持。

若要演示数据修改功能的 GridView，首先要添加到说明`Basics.aspx`上方现有 GridView 页上，并将其绑定到现有 ObjectDataSource 通过说明的智能标记。 下一步，清理说明的`Height`和`Width`属性，并检查通过智能标记启用分页选项。 若要启用编辑，插入和删除的支持，只需选中的智能标记中的启用编辑、 启用插入和启用删除复选框。


![配置为支持编辑、 插入和删除说明](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image41.png)

**图 17**： 配置为支持编辑、 插入和删除说明


作为 GridView，添加编辑，插入或删除操作支持将添加 CommandField 到说明，如下面的声明性语法所示：


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample7.aspx)]

请注意，对于说明 CommandField 默认情况下，将显示在列集合的末尾。 由于说明的字段将呈现为行，CommandField 显示为行与 Insert、 编辑和删除说明底部的按钮。


[![配置为支持编辑、 插入和删除说明](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image42.png)

**图 18**： 配置为支持编辑，插入和删除说明 ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image44.png))


单击删除按钮启动与 GridView 相同的事件序列： 的回发，则跟填充其 ObjectDataSource 说明`DeleteParameters`基于`DataKeyNames`值; 并已完成，但调用其 ObjectDataSource`Delete()`方法，实际上从数据库中删除产品。 说明如何在编辑还在以与 GridView 相同的方式工作。

用于插入，最终用户会看到其中新建按钮，单击时，呈现在"插入模式。 说明 "插入模式"与新建按钮替换为 Insert 和取消按钮和仅这些 BoundFields 其`InsertVisible`属性设置为`True`（默认值） 显示。 如标识为自动递增字段，这些数据字段`ProductID`，具有其[InsertVisible 属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx)设置为`False`时将说明如何绑定到数据源通过智能标记。

Visual Studio 时将数据源绑定到说明如何通过智能标记，将设置`InsertVisible`属性`False`仅用于自动递增字段。 只读字段，如`CategoryName`和`SupplierName`，除非将在"插入模式"用户界面中显示其`InsertVisible`属性显式设置为`False`。 花些时间设置这两个字段的`InsertVisible`属性设置为`False`，通过说明的声明性语法，或者通过编辑字段中的智能标记链接。 图 19 显示设置`InsertVisible`属性设置为`False`编辑字段上单击链接。


[![Northwind Traders 现在提供 acme 开发 Tea](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image45.png)

**图 19**: Northwind Traders 现在提供 acme 开发 Tea ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image47.png))


设置后`InsertVisible`属性、 视图`Basics.aspx`页在浏览器，然后单击新建按钮。 图 20 显示说明如何添加新饮料时, acme 开发 Tea，到我们的产品线。


[![Northwind Traders 现在提供 acme 开发 Tea](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image48.png)

**图 20**: Northwind Traders 现在提供 acme 开发 Tea ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image50.png))


为 acme 开发 Tea 输入详细信息，并单击插入按钮后, 回发时，才会和新的记录添加到`Products`数据库表。 由于此说明如何列出的顺序与它们存在于数据库表中的产品，我们必须页上的最后一个产品才能看到新的产品。


[![Acme 开发 Tea 的详细信息](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image51.png)

**图 21**: acme 开发 Tea 的详细信息 ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image53.png))


> [!NOTE]
> 说明如何的[CurrentMode 属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx)表示正在显示的界面，可以是以下值之一： `Edit`， `Insert`，或`ReadOnly`。 [DefaultMode 属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx)指示的模式说明如何在编辑后返回到或插入已完成并且对显示说明的永久处于编辑模式或插入模式很有用。


该点并单击插入和编辑功能的说明会受到与 GridView 相同的限制： 用户必须输入现有`CategoryID`和`SupplierID`通过文本框中的值; 接口缺少任何验证逻辑; 它的所有不允许的产品字段`NULL`值或不具有默认值在数据库级别指定的值必须包括在插入的接口，依次类推。

技术我们将检查扩展和增强 GridView 的编辑界面在将来的文章可应用于说明控件的编辑和插入以及接口。

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>FormView 用于更灵活的数据修改用户界面

FormView 提供内置支持用于插入、 编辑和删除数据，因为它使用模板而不是字段，但添加 BoundFields 或 CommandField GridView 和说明控件用于提供数据的位置修改接口。 相反，此接口用于收集用户的 Web 控件添加新项时输入或编辑一个现有新建，以及编辑、 删除、 插入、 更新和取消按钮必须手动添加到适当的模板。 幸运的是，Visual Studio 将自动创建所需的接口绑定到数据源通过其智能标记中的下拉列表 FormView 时。

若要演示了这些技术，首先要添加到 FormView`Basics.aspx`页上，并从 FormView 的智能标记，请将其绑定到已创建对象数据源。 这将生成`EditItemTemplate`， `InsertItemTemplate`，和`ItemTemplate`为包含用于收集用户的输入和按钮 Web 控件新建文本框中 Web 控件 FormView，编辑、 删除、 插入、 更新和取消按钮。 此外，FormView 的`DataKeyNames`属性设置为的主键字段 (`ProductID`) 的对象数据源返回的对象。 最后，检查 FormView 的智能标记中的启用分页选项。

下面显示了声明性标记为 FormView `ItemTemplate` FormView 已绑定到 ObjectDataSource 后。 默认情况下，每个非布尔值产品字段绑定到`Text`时每个布尔值字段标签 Web 控件的属性 (`Discontinued`) 绑定到`Checked`已禁用的复选框 Web 控件的属性。 为了使新建、 编辑和删除按钮，以触发某些 FormView 行为单击时，它是命令性，其`CommandName`值设置为`New`， `Edit`，和`Delete`分别。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample8.aspx)]

图 22 显示 FormView`ItemTemplate`通过浏览器查看时。 每个产品字段列在一起在底部的新建、 编辑和删除按钮。


[![情况下 FormView ItemTemplate 列出每个产品字段以及新、 编辑和删除按钮](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image54.png)

**图 22**: 的情况下 FormView`ItemTemplate`列出每个产品字段沿具有新建、 编辑和删除按钮 ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image56.png))


与的 GridView 和说明，单击删除按钮或任何按钮、 LinkButton 或 ImageButton 类似其`CommandName`属性设置为删除会导致回发，填充 ObjectDataSource`DeleteParameters`根据 FormView 的`DataKeyNames`值，并调用 ObjectDataSource`Delete()`方法。

单击编辑按钮时回发时，才会和数据重新绑定到`EditItemTemplate`，即负责呈现编辑界面。 此接口包括编辑的更新和取消按钮以及数据的 Web 控件。 默认值`EditItemTemplate`生成的 Visual Studio 包含的任何自动递增字段的标签 (`ProductID`)、 一个文本框中的每个非布尔值字段，以及一为每个布尔值字段的复选框。 此行为是非常类似于 GridView 和说明控件中的自动生成 BoundFields。

> [!NOTE]
> FormView 的自动生成的一个小问题`EditItemTemplate`是，它会呈现文本框中 Web 控件是只读的如这些字段`CategoryName`和`SupplierName`。 我们将了解如何考虑到这一点很快。


控制中的文本框中`EditItemTemplate`具有其`Text`属性绑定到其相应的数据字段使用的值*双向数据绑定*。 双向数据绑定，由表示`<%# Bind("dataField") %>`，数据绑定到该模板时和填充插入或编辑记录的对象数据源的参数时执行这两个数据绑定。 也就是说，当用户单击编辑按钮从`ItemTemplate`、`Bind()`方法返回指定的数据字段值。 用户进行相应更改，并单击更新后，值将对应的回发到使用指定的数据字段`Bind()`应用于 ObjectDataSource `UpdateParameters`。 单向数据绑定，或者，由表示`<%# Eval("dataField") %>`，仅当数据绑定到模板中检索数据字段值并未*不*上回发到数据源的参数返回的用户输入的值。

下面的声明性标记显示 FormView `EditItemTemplate`。 请注意，`Bind()`方法可在数据绑定的语法，并更新和取消按钮 Web 控件具有其`CommandName`相应地进行设置的属性。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample9.aspx)]

我们`EditItemTemplate`，在这点，将导致如果我们尝试使用它引发异常。 问题在于`CategoryName`和`SupplierName`作为文本框中 Web 控件中呈现字段`EditItemTemplate`。 我们需要将这些文本框中更改为标签或删除它们。 让我们只需将其从完全删除`EditItemTemplate`。

图 23 在浏览器中显示 FormView 后单击编辑按钮后牛奶。 请注意，`SupplierName`和`CategoryName`中所示的字段`ItemTemplate`不再存在，我们只需删除从`EditItemTemplate`。 单击更新按钮时 FormView 继续通过 GridView 和说明控件相同的步骤序列。


[![默认情况下 EditItemTemplate 显示文本框中或复选框为每个可编辑产品字段](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image57.png)

**图 23**： 默认情况下`EditItemTemplate`显示每个可编辑产品字段为文本框中或复选框 ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image59.png))


插入按钮时单击 FormView`ItemTemplate`回发时，才会。 但是，任何数据不绑定到 FormView 因为一条新记录将被添加。 `InsertItemTemplate`接口包含 Web 控件以添加新记录以及插入和取消按钮。 默认值`InsertItemTemplate`生成的 Visual Studio 包含每个非布尔值字段一个文本框和一个用于每个布尔值字段，类似于自动生成的复选框`EditItemTemplate`的接口。 TextBox 控件具有其`Text`属性绑定到其使用双向数据绑定的相应数据字段的值。

下面的声明性标记显示 FormView `InsertItemTemplate`。 请注意，`Bind()`方法使用在数据绑定的语法，插入和取消按钮 Web 控件需要其`CommandName`相应地进行设置的属性。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample10.aspx)]

没有与 FormView 的自动生成的种微妙`InsertItemTemplate`。 具体而言，即使为是只读的如这些字段创建文本框中 Web 控件`CategoryName`和`SupplierName`。 与类似`EditItemTemplate`，我们需要先删除这些文本框中，从`InsertItemTemplate`。

图 24 显示 FormView 浏览器中，添加一个新的产品，acme 开发 Coffee 时。 请注意，`SupplierName`和`CategoryName`中所示的字段`ItemTemplate`不再存在，我们只需删除它们。 插入按钮单击后通过说明相同的步骤序列 FormView 继续进行，添加一条新记录到`Products`表。 插入它后，图 25 FormView 中显示 acme 开发 Coffee 产品的详细信息。


[![InsertItemTemplate 指示 FormView 插入接口](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image60.png)

**图 24**:`InsertItemTemplate`指示 FormView 插入接口 ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image62.png))


[![对于新的产品，acme 开发 Coffee 细节将显示在 FormView](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image63.png)

**图 25**： 新产品，acme 开发 Coffee 的详细信息显示在 FormView ([单击以查看实际尺寸的图像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image65.png))


通过将分离出只读的编辑，并插入三个单独的模板，接口 FormView 允许比的说明和 GridView 更精细的控制这些接口。

> [!NOTE]
> 如说明如何，FormView 的`CurrentMode`属性指示要显示的接口并将其`DefaultMode`属性指示的模式 FormView 将返回到在编辑后，要么已完成插入。


## <a name="summary"></a>摘要

在本教程中，我们将探讨插入、 编辑和删除使用 GridView，说明如何和 FormView 数据的基础知识。 这些控件的所有三个提供一定程度的可以利用而无需编写一行代码感谢数据 Web 控件和 ObjectDataSource ASP.NET 页中的内置数据修改功能。 但是，简单点，然后单击技术呈现相当 frail 和 naïve 数据修改用户界面。 若要提供验证，注入编程值、 适当地处理异常、 自定义用户界面中，和依此类推，我们将需要依赖于将在下一步的几个教程讨论的技术 bevy。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一页](limiting-data-modification-functionality-based-on-the-user-cs.md)
[下一页](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
