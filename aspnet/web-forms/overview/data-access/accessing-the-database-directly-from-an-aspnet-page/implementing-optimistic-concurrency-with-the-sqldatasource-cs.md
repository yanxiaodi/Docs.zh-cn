---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
title: "实现开放式并发使用 SqlDataSource (C#) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将查看的乐观并发控制 essentials，然后研究一下如何实现使用 SqlDataSource 控件。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: df999966-ac48-460e-b82b-4877a57d6ab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: b089a0b25aa5a520f3e20af8ec5212072ad7c7bf
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-c"></a>使用 SqlDataSource (C#) 的实现开放式并发
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_CS.exe)或[下载 PDF](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/datatutorial50cs1.pdf)

> 在本教程中我们将查看的乐观并发控制 essentials，然后研究一下如何实现使用 SqlDataSource 控件。


## <a name="introduction"></a>介绍

在前面的教程中，我们将探讨如何添加插入、 更新和删除 SqlDataSource 控件的功能。 简单地说，为了提供这些功能我们需要指定相应`INSERT`， `UPDATE`，或`DELETE`控件 s 中的 SQL 语句`InsertCommand`， `UpdateCommand`，或`DeleteCommand`以及相应的属性中的参数`InsertParameters`， `UpdateParameters`，和`DeleteParameters`集合。 尽管可以手动指定这些属性和集合，配置数据源向导 s 高级的按钮提供生成`INSERT`， `UPDATE`，和`DELETE`语句复选框，将自动创建这些语句基于`SELECT`语句。

以及生成`INSERT`， `UPDATE`，和`DELETE`语句复选框，高级 SQL 生成选项对话框中包括一个使用开放式并发选项 （请参见图 1）。 选中之后，`WHERE`中自动生成子句`UPDATE`和`DELETE`语句被修改，以仅执行更新或删除如果由于用户修改基础数据库数据做 t 上次加载数据到网格。


![你可以添加乐观并发支持从高级 SQL 生成选项对话框](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.gif)

**图 1**： 你可以添加乐观并发支持从高级 SQL 生成选项对话框


返回[实现开放式并发](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md)教程中，我们探讨乐观并发控制以及如何将其添加到对象数据源的基础知识。 在本教程中我们修饰的乐观并发控制 essentials 上，然后浏览如何实现使用 SqlDataSource。

## <a name="a-recap-of-optimistic-concurrency"></a>开放式并发扼要重述

Web 应用程序允许多个并发用户编辑或删除相同的数据，存在一个用户可能会意外地覆盖另一个 s 更改的可能性。 在[实现开放式并发](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md)我提供下面的示例的教程：

假设，两个用户，Jisun 和 Sam，同时也已访问的应用程序允许访问者更新和删除通过 GridView 控件的产品中的页。 同时时间大致相同牛奶单击编辑按钮。 Jisun 产品名称更改为牛奶 Tea，并单击更新按钮。 最终结果是`UPDATE`语句发送到数据库，设置*所有*产品 s 可更新字段的 (即使 Jisun 只更新一个字段， `ProductName`)。 在此时间点，数据库具有值牛奶 Tea，类别饮料，供应商尖端液体，此特定产品的依此类推。 但是，在 Sam 的屏幕上 GridView 仍将显示产品名称中可编辑的 GridView 行作为牛奶。 几秒后 Jisun s 已做的更改，Sam 到调味品更新类别并单击更新。 这会导致`UPDATE`语句发送到的数据库，将产品名称设置为牛奶，`CategoryID`到相应的调味品类别 ID，依次类推。 产品名称的 Jisun 的更改已被覆盖。

图 2 演示了这种交互。


[![当两个用户同时更新的记录存在一个用户 s 可能发生变化以覆盖其他 s](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.png)

**图 2**： 当两个用户同时更新记录存在 s 可能一个用户 s 将更改为覆盖其他 s ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.png))


若要防止这种情况下打开，一种形式的[并发控制](http://en.wikipedia.org/wiki/Concurrency_control)必须实现。 [开放式并发](http://en.wikipedia.org/wiki/Optimistic_concurrency_control)本教程重点介绍适用于可能会有并发冲突不时地，大多数情况下此类冲突获胜 t 出现发生的假设。 因此，如果确实会发生冲突，乐观并发控制只会通知用户由于另一个用户已修改相同的数据的情况下保存其更改可以 t。

> [!NOTE]
> 对于将成为许多并发冲突，或者如果此类冲突不是可容忍的假设其中的应用程序，然后保守式并发控制可以改用。 将回指[实现开放式并发](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md)教程有关保守式并发控制的更全面讨论。


通过确保正在更新或删除的记录具有相同的值，像更新或删除进程启动时适用乐观并发控制。 例如，当单击编辑按钮可编辑的 GridView 中，记录 s 从数据库读取和显示值在文本框中和其他 Web 控件。 GridView 保存这些原始值。 更高版本，则在用户进行她更改，并单击更新按钮后`UPDATE`使用语句必须考虑的原始值加上的新值并仅更新基础数据库记录，如果原始值在用户开始编辑是仍在数据库中的值相同。 图 3 描绘了这一序列的事件。


[![若要成功执行 Update 或 Delete，对于原始值必须等于当前的数据库值](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.png)

**图 3**: For Update 或 Delete succeed，原始值必须是等于当前的数据库值 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.png))


各种方法来实现开放式并发 (请参阅[Peter A.Bromberg](http://www.eggheadcafe.com/articles/pbrombergresume.asp) s [Optmistic 并发更新逻辑](http://www.eggheadcafe.com/articles/20050719.asp)简要查看多个选项)。 使用由 SqlDataSource （以及 ADO.NET 类型化数据集在我们的数据访问层中使用） 的方法增强了`WHERE`子句，以包括所有的原始值的比较。 以下`UPDATE`语句，例如，更新的名称和产品价格只有当前数据库值与最初检索到更新 GridView 中的记录时的值相等。 `@ProductName`和`@UnitPrice`参数包含由用户输入的新值，而`@original_ProductName`和`@original_UnitPrice`包含最初加载到了 GridView 时单击编辑按钮的值：


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample1.sql)]

正如我们将看到在本教程中，启用乐观并发控制使用 SqlDataSource 非常简单，选中复选框。

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>步骤 1： 创建 SqlDataSource 支持乐观并发

首先打开`OptimisticConcurrency.aspx`来自页`SqlDataSource`文件夹。 将一个 SqlDataSource 控件从工具箱中拖动到设计器中，设置其`ID`属性`ProductsDataSourceWithOptimisticConcurrency`。 接下来，单击从控件 s 智能标记的配置数据源链接。 从向导中的第一个屏幕，选择要处理`NORTHWINDConnectionString`单击下一步。


[![选择以使用 NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.png)

**图 4**： 选择工作与`NORTHWINDConnectionString`([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.png))


此示例中我们将添加一个 GridView，使用户能够编辑`Products`表。 因此，从配置 Select 语句屏幕中，选择`Products`表从下拉列表，然后选择`ProductID`， `ProductName`， `UnitPrice`，和`Discontinued`列，如图 5 中所示。


[![从产品表中，返回 ProductID、 ProductName、 UnitPrice 和已停止使用的列](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.png)

**图 5**： 从`Products`表，则返回`ProductID`， `ProductName`， `UnitPrice`，和`Discontinued`列 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.png))


列后，单击高级按钮以打开高级 SQL 生成选项对话框。 检查生成`INSERT`， `UPDATE`，和`DELETE`语句并使用开放式并发复选框，然后单击确定 （回头参考图 1 的屏幕快照）。 通过单击下一步，完成向导，然后完成。

完成配置数据源向导后，需要一段时间来检查生成`DeleteCommand`和`UpdateCommand`属性和`DeleteParameters`和`UpdateParameters`集合。 若要执行此操作的最简单方法是单击左下角以查看页面 s 声明性语法中的源选项卡上。 在此可以找到`UpdateCommand`的值：


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample2.sql)]

中的七个参数与`UpdateParameters`集合：


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample3.aspx)]

同样，`DeleteCommand`属性和`DeleteParameters`集合应如下所示：


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample5.aspx)]

除了补充`WHERE`子句`UpdateCommand`和`DeleteCommand`属性 （和相应的参数集合中添加其他参数），选择使用乐观并发选项调整其他两个属性：

- 更改[`ConflictDetection`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx)从`OverwriteChanges`（默认值） 到`CompareAllValues`
- 更改[`OldValuesParameterFormatString`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx)介于 {0} （默认值） 和原始\_{0}。

Web 控件的数据时调用 SqlDataSource s`Update()`或`Delete()`方法，它将传递的原始值。 如果 SqlDataSource s`ConflictDetection`属性设置为`CompareAllValues`，这些原始值添加到该命令。 `OldValuesParameterFormatString`属性提供了用于这些原始值参数的命名模式。 配置数据源向导使用原始\_{0} 并将其命名中的每个原始参数`UpdateCommand`和`DeleteCommand`属性和`UpdateParameters`和`DeleteParameters`集合相应地。

> [!NOTE]
> 因为我们不使用 SqlDataSource 控件 s 插入功能，请尝试删除`InsertCommand`属性并将其`InsertParameters`集合。


## <a name="correctly-handlingnullvalues"></a>正确处理`NULL`值

遗憾的是，增强`UPDATE`和`DELETE`语句自动生成的配置数据源向导时使用乐观并发执行*不*适用于包含的记录`NULL`值。 要了解原因，请考虑我们 SqlDataSource 的`UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample6.sql)]

`UnitPrice`中的列`Products`表可以有`NULL`值。 如果特定记录`NULL`值`UnitPrice`、`WHERE`子句部分`[UnitPrice] = @original_UnitPrice`将*始终*计算结果为 False，因为`NULL = NULL`始终返回 False。 因此，记录包含`NULL`值不能编辑或删除，作为`UPDATE`和`DELETE`语句`WHERE`子句获胜 t 返回要更新或删除任何行。

> [!NOTE]
> 此 bug 已第一次报告给 Microsoft 的 2004 年 6 月中在[SqlDataSource 生成不正确 SQL 语句](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937)，据说计划在 ASP.NET 的下一个版本中修复。


若要解决此问题，我们必须手动更新`WHERE`子句中同时`UpdateCommand`和`DeleteCommand`属性**所有**可以具有的列`NULL`值。 一般情况下，更改`[ColumnName] = @original_ColumnName`到：


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample7.sql)]

此修改可直接通过声明性的标记，通过从属性窗口中，UpdateQuery 或 DeleteQuery 选项或通过更新和删除在指定的选项卡，自定义 SQL 语句或存储的过程中配置数据的选项源向导。 同样，必须进行此修改*每个*中的列`UpdateCommand`和`DeleteCommand`s`WHERE`可以包含的子句`NULL`值。

将此应用到我们的示例会导致以下修改过`UpdateCommand`和`DeleteCommand`值：


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>步骤 2： 添加使用编辑和删除选项 GridView

使用 SqlDataSource，配置为支持乐观并发，那么保持是将数据 Web 控件添加到使用此并发控制页。 对于本教程，让我们来添加一个 GridView，提供这两个编辑和删除功能。 若要完成此操作，将从工具箱中拖动到设计器和组 GridView 其`ID`到`Products`。 从 GridView s 智能标记，将其绑定到`ProductsDataSourceWithOptimisticConcurrency`SqlDataSource 控件在步骤 1 中增加。 最后，检查的智能标记中的启用编辑和启用删除选项。


[![将 GridView 绑定到 SqlDataSource 并启用编辑和删除](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.png)

**图 6**： 将 GridView 绑定到 SqlDataSource 和启用编辑和删除 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image10.png))


添加 GridView 之后, 中配置其外观，通过删除`ProductID`更改 BoundField `ProductName` BoundField s`HeaderText`属性设置为产品和更新`UnitPrice`BoundField，以便其`HeaderText`属性是只需价格。 理想情况下，我们 d 增强以包括有关 RequiredFieldValidator 的编辑界面`ProductName`值和有关 CompareValidator`UnitPrice`值 （以确保它 s 格式正确的数字值）。 请参阅[自定义的数据修改界面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)教程，以在自定义编辑接口的 GridView s 的更深入探讨。

> [!NOTE]
> GridView 由于传递给 SqlDataSource 从 GridView 的原始值，则必须启用 s 视图状态存储在视图状态。


进行这些修改到 GridView 后, 的 GridView 和 SqlDataSource 的声明性标记应看起来类似于以下：


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample9.aspx)]

若要查看在操作中的乐观并发控制，请打开两个浏览器窗口并加载`OptimisticConcurrency.aspx`在这两页。 单击在这两个浏览器中的第一个产品的编辑按钮。 在一个浏览器中，更改产品名称，然后单击更新。 浏览器将回发并且 GridView 将恢复为其预编辑模式显示只需编辑的记录的新产品名称。

在第二个浏览器窗口中，更改的价格 （但保留其原始值形式的产品名称），然后单击更新。 在回发时，网格将返回到其预编辑模式，但不是会记录到价格更改。 第二个浏览器中显示相同的值的第一个新的产品名称与旧的价格。 在第二个浏览器窗口中所做的更改已丢失。 此外，所做的更改丢失而是安静模式，因为存在没有异常或消息，指出刚刚发生并发冲突。


[![第二个浏览器窗口中的更改都以无提示方式丢失](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image11.png)

**图 7**： 在第二个浏览器窗口已以无提示方式丢失的更改 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image12.png))


为什么所做的第二个浏览器的更改未提交的原因是的因为`UPDATE`语句的`WHERE`子句筛选出所有记录，并因此未影响任何行。 让我们来看一下`UPDATE`再次语句：


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample10.sql)]

当第二个浏览器窗口中更新的记录时，原始的产品名称中指定`WHERE`子句不 t 匹配向上 （因为它已更改的第一个浏览器） 的现有产品名称。 因此，该语句`[ProductName] = @original_ProductName`方法将返回 False，和`UPDATE`不会影响任何记录。

> [!NOTE]
> Delete 的工作方式相同的方式。 使用两个打开的浏览器窗口，通过编辑给定的产品使用其中一个，并保存其更改启动。 保存更改后在一个浏览器中，单击同一产品另一部分中的删除按钮。 由于原始值 don t 中匹配`DELETE`语句的`WHERE`子句，删除以静默方式失败。


从第二个浏览器窗口中，单击网格中的更新按钮返回到预先编辑模式，但其更改丢失后的最终用户的透视。 但是，此处 s 停在其更改没有 t 视觉反馈。 理想情况下，如果用户的更改都将丢失到并发冲突，我们 d 通知他们，并可能保留在编辑模式下的网格。 让我们来看一下如何实现此目的。

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>步骤 3： 确定当发生并发冲突

因为并发冲突拒绝了一个具有所做的更改，则可以 nice 并发冲突发生时向用户发出警报。 若要向用户发出警报，让的标签 Web 控件添加到名为的页的顶部`ConcurrencyViolationMessage`其`Text`属性将显示以下消息： 已尝试更新或删除已由另一个用户同时更新的记录。 请查看其他用户的更改张磁盘，然后重做更新或删除。 设置标签控件 s`CssClass`中定义的警告，这是 CSS 类的属性`Styles.css`以红色、 斜体、 粗体和大字体显示文本。 最后，设置标签 s`Visible`和`EnableViewState`属性设置为`false`。 这将隐藏除外仅其中我们显式设置这些回发标签其`Visible`属性`true`。


[![将一个标签控件添加到页后，可以显示警告](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image13.png)

**图 8**： 将标签控件添加到显示该警告的页 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image14.png))


当执行更新或删除、 GridView s`RowUpdated`和`RowDeleted`后其数据源控件具有执行请求的更新或删除触发的事件处理程序。 我们可以确定这些事件处理程序中的操作影响了多少行。 如果影响了零行，我们想要显示`ConcurrencyViolationMessage`标签。

创建两个事件处理程序`RowUpdated`和`RowDeleted`事件并添加以下代码：


[!code-csharp[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample11.cs)]

在这两个事件处理程序中，我们将检查`e.AffectedRows`属性，如果它等于 0，设置`ConcurrencyViolationMessage`标签 s`Visible`属性`true`。 在`RowUpdated`事件处理程序中，我们还指示 GridView 继续处于编辑模式，通过设置`KeepInEditMode`属性为 true。 在此情况下，我们需要重新绑定数据到网格，以便其他用户的数据加载到编辑界面。 这通过调用 GridView s 实现`DataBind()`方法。

如图 9 所示，使用这些两个事件处理程序中，每次发生并发冲突时，将显示一非常明显的消息。


[![在遇到并发冲突时显示一条消息](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image15.png)

**图 9**: 消息显示在遇到时并发冲突 ([单击以查看实际尺寸的图像](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image16.png))


## <a name="summary"></a>摘要

创建 web 应用程序时其中多个并发用户可以编辑相同的数据，务必要考虑并发控制选项。 默认情况下，Web 控件的 ASP.NET 数据和数据源控件不采用任何并发控制。 正如我们看到在本教程中，实现乐观并发控制使用 SqlDataSource 是相对快速而简单。 SqlDataSource 处理大部分先为你添加扩充`WHERE`到自动生成的子句`UPDATE`和`DELETE`但没有语句是在处理中的一些细微部分`NULL`值列中所述正确处理`NULL`值部分。

本教程到结束 SqlDataSource 我们检查。 我们剩余的教程将返回到使用使用 ObjectDataSource 和分层体系结构的数据。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一页](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
[下一页](querying-data-with-the-sqldatasource-control-vb.md)
