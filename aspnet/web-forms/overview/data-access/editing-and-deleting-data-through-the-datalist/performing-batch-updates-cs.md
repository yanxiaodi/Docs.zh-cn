---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
title: "执行批处理更新 (C#) |Microsoft 文档"
author: rick-anderson
description: "了解如何创建完全可编辑 DataList 的所有项处于编辑模式和其值可以通过单击全部更新按钮保存..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 57743ca7-5695-4e07-aed1-44b297f245a9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
msc.type: authoredcontent
ms.openlocfilehash: 46db3c5d733b9c8b6e749a9b8ff1aa9a061c36df
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="performing-batch-updates-c"></a>执行批处理更新 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_CS.exe)或[下载 PDF](performing-batch-updates-cs/_static/datatutorial37cs1.pdf)

> 了解如何创建完全可编辑 DataList 的所有项处于编辑模式和可以通过单击页面上的"全部更新"按钮保存其值。


## <a name="introduction"></a>介绍

在[前面教程](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)，我们探讨了如何创建项目级别 DataList。 像标准的可编辑 GridView DataList 中每一项包括编辑按钮，当单击时，会使项可编辑。 尽管这项级别编辑适用于仅偶尔更新的数据，某些用例方案都要求用户编辑多个记录。 如果用户需要编辑多个记录，并强制单击编辑，进行相应更改，然后单击为每个更新，则单击量会妨碍她的工作效率。 在这种情况下，更好的选择是提供完全可编辑 DataList，其中*所有*的其项处于编辑模式，其值可以进行编辑通过单击页面上的全部更新按钮 （请参见图 1）。


[![可以修改完全可编辑的 DataList 中每个项](performing-batch-updates-cs/_static/image2.png)](performing-batch-updates-cs/_static/image1.png)

**图 1**： 可以修改每个项目中完全可编辑的 DataList ([单击以查看实际尺寸的图像](performing-batch-updates-cs/_static/image3.png))


在本教程中我们将研究如何使用户能够更新使用完全可编辑 DataList 供应商地址信息。

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>步骤 1： 在 DataList 的 ItemTemplate 中创建的可编辑的用户界面

在前面的教程，其中我们创建标准的、 项目级别的可编辑 DataList，我们使用两个模板：

- `ItemTemplate`包含只读用户接口 （用于显示每个产品的名称和价格的标签 Web 控件）。
- `EditItemTemplate`包含编辑模式用户界面 （两个文本框中 Web 控件）。

DataList s`EditItemIndex`属性指示什么`DataListItem`（如果有） 使用呈现`EditItemTemplate`。 具体而言，`DataListItem`其`ItemIndex`值与匹配 DataList s`EditItemIndex`属性用来呈现使用`EditItemTemplate`。 此模型适用于创建完全可编辑 DataList 时，可以在时间，但回退拆分编辑只有一项。

为了完全可编辑 DataList，我们需要*所有*的`DataListItem`s 来呈现使用的可编辑的接口。 实现此目的的最简单方法是定义中的可编辑界面`ItemTemplate`。 修改的供应商地址信息，可编辑的接口包含地址、 市和国家/地区值作为文本，然后选择文本框中的供应商名称。

首先打开`BatchUpdate.aspx`页上，添加一个 DataList 控件中，并设置其`ID`属性`Suppliers`。 从 DataList s 智能标记中，选择要添加一个名为的新 ObjectDataSource 控件`SuppliersDataSource`。


[![创建名为 SuppliersDataSource 新对象数据源](performing-batch-updates-cs/_static/image5.png)](performing-batch-updates-cs/_static/image4.png)

**图 2**： 创建新对象数据源命名`SuppliersDataSource`([单击以查看实际尺寸的图像](performing-batch-updates-cs/_static/image6.png))


配置对象数据源检索数据使用`SuppliersBLL`类的`GetSuppliers()`方法 （请参见图 3）。 与前面的教程中，而不是更新的供应商信息通过对象数据源，我们将直接与业务逻辑层进行合作。 因此，在更新选项卡中设置为 (None) 下拉列表 （请参见图 4）。


[![检索使用 GetSuppliers() 方法的供应商信息](performing-batch-updates-cs/_static/image8.png)](performing-batch-updates-cs/_static/image7.png)

**图 3**： 检索供应商信息使用`GetSuppliers()`方法 ([单击以查看实际尺寸的图像](performing-batch-updates-cs/_static/image9.png))


[![在更新选项卡中设置为 (None) 下拉列表](performing-batch-updates-cs/_static/image11.png)](performing-batch-updates-cs/_static/image10.png)

**图 4**： 在更新选项卡中设置为 (None) 下拉列表 ([单击以查看实际尺寸的图像](performing-batch-updates-cs/_static/image12.png))


完成向导后，Visual Studio 自动生成 DataList 的`ItemTemplate`来显示标签 Web 控件中的数据源返回的每个数据字段。 我们需要修改此模板，以便它而是提供编辑界面。 `ItemTemplate`通过设计器使用从 DataList s 智能标记编辑模板选项或直接通过声明性语法可自定义。

花一些时间来创建将供应商的名称显示为文本，但包括用于供应商的地址、 市和国家/地区值的文本框中编辑界面。 进行这些更改后，你页 s 声明性语法应类似于以下：


[!code-aspx[Main](performing-batch-updates-cs/samples/sample1.aspx)]

> [!NOTE]
> 与前面的教程中，在本教程中 DataList 必须具有启用其视图状态。


在`ItemTemplate`我使用两个新的 CSS 类，m`SupplierPropertyLabel`和`SupplierPropertyValue`，其中已添加到`Styles.css`类并配置为使用相同的样式设置`ProductPropertyLabel`和`ProductPropertyValue`CSS 类。


[!code-css[Main](performing-batch-updates-cs/samples/sample2.css)]

进行这些更改后，请访问此页通过浏览器。 如图 5 所示，每个 DataList 项的供应商名称显示为文本，并使用文本框中显示的地址、 市和国家/地区。


[![DataList 中的每个供应商是可编辑](performing-batch-updates-cs/_static/image14.png)](performing-batch-updates-cs/_static/image13.png)

**图 5**: DataList 中的每个供应商是可编辑 ([单击以查看实际尺寸的图像](performing-batch-updates-cs/_static/image15.png))


## <a name="step-2-adding-an-update-all-button"></a>步骤 2： 添加更新全部按钮

图 5 中的每个供应商有其地址、 市和国家/地区字段显示在文本框中，当前没有任何更新按钮可用。 而不是使每个项的更新按钮，可完全编辑 DataLists 存在与通常的单个更新所有按钮在页上，单击时，更新*所有*DataList 中记录。 对于本教程，让我们来添加两个更新所有按钮-在页面顶部的一个，另一个底部 （尽管单击任一按钮将具有相同的效果）。

通过添加上方 DataList 和组的一个按钮 Web 控件的开始其`ID`属性`UpdateAll1`。 接下来，添加下 DataList，第二个按钮 Web 控件设置其`ID`到`UpdateAll2`。 设置`Text`到更新所有的两个按钮的属性。 最后，为两个按钮创建事件处理程序`Click`事件。 而不会复制每个事件处理程序中的更新逻辑，让 s 重构到第三个方法时，该逻辑`UpdateAllSupplierAddresses`，具有只需调用此第三个方法的事件处理程序。


[!code-csharp[Main](performing-batch-updates-cs/samples/sample3.cs)]

图 6 显示的页后已添加所有更新的按钮。


[![两个更新所有按钮已都添加到页面](performing-batch-updates-cs/_static/image17.png)](performing-batch-updates-cs/_static/image16.png)

**图 6**： 两个更新所有按钮已都添加到页 ([单击以查看实际尺寸的图像](performing-batch-updates-cs/_static/image18.png))


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>步骤 3： 更新的所有供应商地址信息

使用的所有 DataList 的项显示编辑界面和所有更新的按钮添加，所有这些剩下编写代码以执行批处理更新。 具体而言，我们需要循环访问 DataList 的项并调用`SuppliersBLL`类的`UpdateSupplierAddress`为每个方法。

集合`DataListItem`实例可通过 DataList s 访问 DataList 该构成[`Items`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx)。 具有对引用`DataListItem`，我们可以获取相应`SupplierID`从`DataKeys`集合并以编程方式引用文本框中 Web 控件中`ItemTemplate`如下面的代码所示：


[!code-csharp[Main](performing-batch-updates-cs/samples/sample4.cs)]

当用户单击其中一个更新所有按钮，`UpdateAllSupplierAddresses`方法循环访问每个`DataListItem`中`Suppliers`DataList 和调用`SuppliersBLL`类的`UpdateSupplierAddress`方法，以传递相应的值。 地址、 城市或国家/地区传递的非输入值是一个值的`Nothing`到`UpdateSupplierAddress`（而不是空字符串），这将导致数据库`NULL`基础记录 s 字段。

> [!NOTE]
> 作为一项增强功能，你可能想要将状态标签 Web 控件添加到提供一些确认消息，执行批处理更新后的页面。


## <a name="updating-only-those-addresses-that-have-been-modified"></a>更新已被修改的地址

用于此教程的调用的批处理更新算法`UpdateSupplierAddress`方法*每个*DataList，无论是否已更改其地址信息中的供应商。 虽然此类 blind 更新不 t 通常性能问题，它们将可能导致的多余的记录，如果你重新审核更改为数据库表。 例如，如果你使用触发器来记录所有`UPDATE`到`Suppliers`表与审核表，每次用户单击全部更新按钮将在系统中，而不管用户是否进行任何为每个供应商创建一个新的审核记录更改。

ADO.NET DataTable 和 DataAdapter 类旨在支持批处理更新只修改、 删除和新记录结果中的任何数据库通信的位置。 位于 DataTable 每行都有[`RowState`属性](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx)，该值指示是否已添加到数据表，从它，修改、 删除或保持不变的行。 当最初填充数据表时，所有的行已标记不变。 为已修改更改的任何行的列的值会将行的标记。

在`SuppliersBLL`我们以更新到单一供应商提供记录中的第一个读取指定供应商的地址信息的类`SuppliersDataTable`然后设置`Address`， `City`，和`Country`列的值使用以下代码：


[!code-csharp[Main](performing-batch-updates-cs/samples/sample5.cs)]

此代码将传入的地址、 城市、 和到国家/地区值 naively 分配`SuppliersRow`中`SuppliersDataTable`无论值没有更改。 这些修改会导致`SuppliersRow`s`RowState`将被标记为修改的属性。 当数据访问层 s`Update`方法调用，因此它发现`SupplierRow`已被修改，因此发送`UPDATE`命令到数据库。

但是，假设我们将代码添加到此方法以仅分配传入的地址、 市和国家/地区值，如果它们不同于`SuppliersRow`s 现有值。 将在其中地址、 市和国家/地区是相同的现有数据的情况下，进行任何更改和`SupplierRow`s`RowState`将保留标记为不变。 最终结果是，当 DAL s`Update`调用方法，将做出任何数据库调用，因为`SuppliersRow`尚未修改。

若要执行此更改，将隐式分配传入的地址、 市和国家/地区值替换为以下代码语句：


[!code-csharp[Main](performing-batch-updates-cs/samples/sample6.cs)]

与此添加了代码，DAL s`Update`方法发送`UPDATE`到其与地址相关的值已更改的那些记录的数据库的语句。

或者，我们无法跟踪是否有传入的地址字段和数据库数据之间的差异并，如果没有任何，只需跳过对 DAL 的调用`Update`方法。 此方法适用如果你重新使用 DB 直接方法，因为数据库直接方法仍未传递`SuppliersRow`实例，它的`RowState`可以对其进行检查，以确定是否实际需要的数据库调用。

> [!NOTE]
> 每次`UpdateSupplierAddress`调用方法，进行调用到数据库以检索有关已更新记录的信息。 然后，如果数据中有任何更改，另一个对数据库进行调用以更新表行。 此工作流无法优化通过创建`UpdateSupplierAddress`接受的方法重载`EmployeesDataTable`实例具有*所有*中的更改的`BatchUpdate.aspx`页。 然后，它无法进行一次调用到数据库，若要获取所有从记录`Suppliers`表。 然后可枚举两个结果集，无法更新已发生更改的那些记录。


## <a name="summary"></a>摘要

在本教程中，我们将了解如何创建完全可编辑 DataList，从而使用户可快速修改多个供应商的地址信息。 我们已开始将通过在 DataList s 中定义的供应商的地址、 市和国家/地区值的文本框中 Web 控件的编辑界面`ItemTemplate`。 接下来，我们添加更新所有按钮上方和下方 DataList。 用户已创建他的更改，并单击其中一个更新所有按钮之后,`DataListItem`枚举 s 和调用`SuppliersBLL`类的`UpdateSupplierAddress`方法由。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Zack Jones 和 Ken Pespisa。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)
[下一页](handling-bll-and-dal-level-exceptions-cs.md)
