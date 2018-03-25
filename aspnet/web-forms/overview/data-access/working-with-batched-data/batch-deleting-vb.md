---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
title: "批处理删除 (VB) |Microsoft 文档"
author: rick-anderson
description: "了解如何删除单个操作中的多个数据库记录。 在用户界面层我们构建后在中创建早期 tut 增强 GridView..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 4fb72f75-32ab-4bf7-a764-be20367be726
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: cce3876ae0fab66f617d77abdb8777f9865c7514
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="batch-deleting-vb"></a>批处理删除 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_VB.zip)或[下载 PDF](batch-deleting-vb/_static/datatutorial65vb1.pdf)

> 了解如何删除单个操作中的多个数据库记录。 在用户界面层我们相互依赖在早期的教程中创建增强 GridView。 在数据访问层中我们将包装在以确保所有删除都成功或所有删除都回滚一个事务内的多个删除操作。


## <a name="introduction"></a>介绍

[前面教程](batch-updating-vb.md)探讨了如何创建一个批处理编辑接口使用完全可编辑的 GridView。 编辑接口一批将在其中用户通常编辑多个记录一次的情况下，需要数量少得多的回发和键盘鼠标上下文开关，从而提高最终用户的效率。 此方法十分同样适用于其中很常见的用户删除一个转到中的多个记录的页。

已使用的在线电子邮件客户端的任何人已熟悉最常见的批处理删除接口之一： 一个具有对应删除所有选中的项的网格中的每一行中的复选框按钮 （请参见图 1）。 本教程非常短因为我们已做的所有硬工作中前面的教程中创建基于 web 的界面以及要删除的记录作为单个原子操作的一系列方法。 在[添加 GridView 列的复选框](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md)教程，我们使用的列的复选框并在创建 GridView[在事务中包装数据库修改](wrapping-database-modifications-within-a-transaction-vb.md)教程中我们创建中的方法将使用事务来删除 BLL`List<T>`的`ProductID`值。 在本教程中，我们将基础之上构建，并合并我们以前的体验，以创建删除示例工作批次。


[![每个行包含一个复选框](batch-deleting-vb/_static/image1.gif)](batch-deleting-vb/_static/image1.png)

**图 1**： 每个行包含一个复选框 ([单击以查看实际尺寸的图像](batch-deleting-vb/_static/image2.png))


## <a name="step-1-creating-the-batch-deleting-interface"></a>步骤 1： 创建批处理删除接口

由于我们已创建的批处理删除中的接口[添加 GridView 列的复选框](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md)教程中，我们可以只需将其复制到`BatchDelete.aspx`而不是从头开始创建它。 首先打开`BatchDelete.aspx`页面`BatchData`文件夹和`CheckBoxField.aspx`页面`EnhancedGridView`文件夹。 从`CheckBoxField.aspx`页上，请转到源视图并复制之间的标记`<asp:Content>`标记图 2 中所示。


[![将 CheckBoxField.aspx 的声明性标记复制到剪贴板](batch-deleting-vb/_static/image2.gif)](batch-deleting-vb/_static/image3.png)

**图 2**： 复制的声明性标记`CheckBoxField.aspx`到剪贴板 ([单击以查看实际尺寸的图像](batch-deleting-vb/_static/image4.png))


接下来，请转到中的源视图`BatchDelete.aspx`粘贴剪贴板中的内容和`<asp:Content>`标记。 再复制和粘贴从中的代码中的代码隐藏类内`CheckBoxField.aspx.vb`到中的代码隐藏类内`BatchDelete.aspx.vb`(`DeleteSelectedProducts`按钮 s`Click`事件处理程序，`ToggleCheckState`方法，与`Click`事件处理程序有关`CheckAll`和`UncheckAll`按钮)。 此内容，请通过复制后`BatchDelete.aspx`页面的代码隐藏类应包含以下代码：


[!code-vb[Main](batch-deleting-vb/samples/sample1.vb)]

复制后通过声明性的标记和源代码，请花片刻时间来测试`BatchDelete.aspx`通过查看通过浏览器。 你应看到列出与每个行，其中列出产品的名称、 类别和一个复选框以及价格 GridView 中的前 10 个产品 GridView。 应该有三个按钮： 检查所有、 全部取消选中和删除所选产品。 时取消选中所有清除所有复选框，单击全部检查按钮选择所有复选框。 单击删除所选产品显示一条消息，其中列出了`ProductID`值的所选的产品，但不实际删除产品。


[![从 CheckBoxField.aspx 接口已移动到 BatchDeleting.aspx](batch-deleting-vb/_static/image3.gif)](batch-deleting-vb/_static/image5.png)

**图 3**： 从接口`CheckBoxField.aspx`已移至`BatchDeleting.aspx`([单击以查看实际尺寸的图像](batch-deleting-vb/_static/image6.png))


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>步骤 2： 删除使用事务检查的产品

与批处理删除接口成功复制到`BatchDeleting.aspx`，所有保持为更新代码，以便删除所选产品按钮会删除使用的检查的产品`DeleteProductsWithTransaction`中的方法`ProductsBLL`类。 此方法，在中添加[在事务中包装数据库修改](wrapping-database-modifications-within-a-transaction-vb.md)教程中，接受作为其输入`List(Of T)`的`ProductID`值并删除每个相应`ProductID`的作用域内事务。

`DeleteSelectedProducts`按钮 s`Click`事件处理程序当前使用以下`For Each`循环来循环访问每个 GridView 行：


[!code-vb[Main](batch-deleting-vb/samples/sample2.vb)]

对于每一行，`ProductSelector`复选框 Web 控件以编程方式引用。 如果选中，行 s`ProductID`从检索`DataKeys`集合和`DeleteResults`标签的`Text`属性将更新为包含一个消息，表明要删除所选行。

上面的代码不会实际为对调用删除任何记录`ProductsBLL`类的`Delete`方法已被注释掉。此删除逻辑打算应用，代码将删除产品但不是能在原子操作。 也就是说，如果序列中第一个的几个删除操作成功，但更高版本失败 （可能是由于外键约束冲突），将引发异常，但已删除这些产品将保持已删除。

为了确保原子性，我们需要改用`ProductsBLL`类的`DeleteProductsWithTransaction`方法。 此方法接受的列表，因为`ProductID`值，我们需要先编译此列表从网格和然后将其作为参数传递。 我们首先要创建的实例`List(Of T)`类型的`Integer`。 在`For Each`我们需要添加所选的产品的循环`ProductID`到此值`List(Of T)`。 循环后这`List(Of T)`必须传递给`ProductsBLL`类的`DeleteProductsWithTransaction`方法。 更新`DeleteSelectedProducts`按钮的`Click`事件处理程序替换为以下代码：


[!code-vb[Main](batch-deleting-vb/samples/sample3.vb)]

更新的代码创建`List(Of T)`类型的`Integer`(`productIDsToDelete`) 和填充其与`ProductID`若要删除的值。 后`For Each`循环中，如果没有选择的至少一个产品`ProductsBLL`类的`DeleteProductsWithTransaction`方法调用该方法并传递此列表。 `DeleteResults`还显示标签和数据重新绑定到 GridView，（以便为在网格中的行不会再出现新删除记录）。

图 4 显示 GridView 后删除所选的行数。 图 5 显示的屏幕，单击删除所选产品按钮后立即。 请注意，在图 5`ProductID`下 GridView 标签中显示已删除的记录的值，并且这些行不再 GridView。


[![将删除所选的产品](batch-deleting-vb/_static/image4.gif)](batch-deleting-vb/_static/image7.png)

**图 4**: 选择产品都将被删除 ([单击以查看实际尺寸的图像](batch-deleting-vb/_static/image8.png))


[![删除产品 ProductID 值是列出下方的 GridView](batch-deleting-vb/_static/image5.gif)](batch-deleting-vb/_static/image9.png)

**图 5**: 删除产品`ProductID`GridView 下方列出的有效值 ([单击以查看实际尺寸的图像](batch-deleting-vb/_static/image10.png))


> [!NOTE]
> 若要测试`DeleteProductsWithTransaction`方法 s 原子性，手动添加的条目中的产品`Order Details`表，然后尝试删除该产品 （以及其他）。 尝试删除的产品与关联的顺序，但请注意如何其他所选的产品删除都将回滚时，你将收到外键约束冲突。


## <a name="summary"></a>摘要

创建批删除接口涉及将添加一个 GridView 具有的列的复选框和按钮 Web 控制，如果单击，则将删除所有选定行的作为单个原子操作。 在本教程中我们生成此类接口进行汇总，在两个前面的教程中完成的工作[添加 GridView 列的复选框](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md)和[在事务中包装数据库修改](wrapping-database-modifications-within-a-transaction-vb.md)。 在第一个教程中我们 GridView 使用创建的列的复选框，并且我们在后一种实现中 BLL 的方法，传递时`List(Of T)`的`ProductID`值，已将其删除所有在事务范围内。

在下一教程中，我们将创建用于执行批处理插入的接口。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已希尔顿 Giesenow 和 Teresa 墨。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](batch-updating-vb.md)
[下一页](batch-inserting-vb.md)
