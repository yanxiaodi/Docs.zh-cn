---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
title: "删除 (C#) 时添加客户端确认 |Microsoft 文档"
author: rick-anderson
description: "在接口中我们已创建了到目前为止，用户可以意外当它们应单击编辑按钮时，单击删除按钮中删除数据。 在此 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: f6e2a12a-2b5e-48fd-8db3-1e94a500c19a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: c5e8ee76224a48d3132597016b81099bd70a1776
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="adding-client-side-confirmation-when-deleting-c"></a>添加客户端确认当删除 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_CS.exe)或[下载 PDF](adding-client-side-confirmation-when-deleting-cs/_static/datatutorial22cs1.pdf)

> 在接口中我们已创建了到目前为止，用户可以意外当它们应单击编辑按钮时，单击删除按钮中删除数据。 在本教程中我们将添加客户端确认对话框中，单击删除按钮时出现。


## <a name="introduction"></a>介绍

通过过去的几个教程我们已了解如何使用我们的应用程序体系结构、 对象数据源和 Web 控件的数据一起提供插入、 编辑和删除功能。 删除接口我们已检查到目前为止已被组成删除按钮，在单击时，会导致回发并且调用 ObjectDataSource 的`Delete()`方法。 `Delete()`方法然后调用传播给发出实际的数据访问层调用业务逻辑层中的配置的方法`DELETE`到数据库的语句。

虽然此用户界面可用于访问者来删除通过 GridView，说明如何或 FormView 控件的记录，但它缺少任何种类的确认当用户单击删除按钮。 如果用户意外单击删除按钮时它们应单击编辑，则将改为删除它们应更新的记录。 若要帮助防止这一点，在本教程中我们将添加客户端确认对话框中，单击删除按钮时出现。

JavaScript`confirm(string)`函数将其字符串输入的参数显示为模式对话框，配备两个按钮的确定和取消 （请参见图 1） 内的文本。 `confirm(string)`函数返回一个布尔值，具体取决于哪些按钮 (`true`，如果用户单击确定，和`false`如果他们单击取消)。


![JavaScript confirm(string) 方法显示在安装结束时，客户端的 Messagebox](adding-client-side-confirmation-when-deleting-cs/_static/image1.png)

**图 1**: JavaScript`confirm(string)`方法显示一个模式，客户端的 Messagebox


在窗体提交，如果值为`false`然后取消提交窗体从客户端事件处理程序返回。 使用此功能，我们可能会删除按钮的客户端`onclick`事件处理程序返回的值对的调用`confirm("Are you sure you want to delete this product?")`。 如果用户单击取消，`confirm(string)`将返回 false，从而导致窗体提交以取消。 无回发时，删除其删除按钮被单击的获胜 t 的产品。 如果，但是，用户单击确定，在确认对话框中，在回发将继续全速，将删除该产品。 请查阅[使用 JavaScript s`confirm()`方法控制窗体提交到](http://www.webreference.com/programming/javascript/confirm/)有关此技术的详细信息。

会稍有添加所需的客户端脚本不同，如果使用比时使用 CommandField 的模板。 因此，在本教程中我们将看一个 FormView 和 GridView 示例。

> [!NOTE]
> 使用客户端确认技术，类似的讨论在本教程中，假定你的用户访问与支持 JavaScript 的浏览器，它们具有启用 JavaScript。 如果这些假设任一未满足特定用户，则单击删除按钮将立即导致回发 （不显示确认消息框）。


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>步骤 1： 创建 FormView 支持删除

首先，通过添加到 FormView`ConfirmationOnDelete.aspx`页面`EditInsertDelete`文件夹中，将其绑定到新对象数据源返回拉取通过的产品信息`ProductsBLL`类的`GetProducts()`方法。 此外将配置对象数据源，以便`ProductsBLL`类 s`DeleteProduct(productID)`方法映射到 ObjectDataSource 的`Delete()`方法; 确保下拉列表设置为 （无） INSERT 和 UPDATE 选项卡。 最后，检查 FormView s 智能标记中的启用分页复选框。

在这些步骤后的新的 ObjectDataSource s 声明性标记将如下所示：


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample1.aspx)]

如我们未使用开放式并发的过去示例，花些时间来清除 ObjectDataSource 的`OldValuesParameterFormatString`属性。

因为它已绑定到仅支持删除 FormView s ObjectDataSource 控件`ItemTemplate`提供仅删除按钮，缺少的新建和更新按钮。 FormView s 声明性的标记，但是，包含多余`EditItemTemplate`和`InsertItemTemplate`，可以删除的。 花些时间自定义`ItemTemplate`因此，它是显示数据字段的产品的一个子集。 我已配置挖掘以显示中的产品的名称`<h3>`标题上面 （以及删除按钮） 其供应商和类别名称。


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample2.aspx)]

进行这些更改后，我们具有完全正常运行的网页，用户可以一次通过产品一个切换能够只需单击删除按钮删除产品。 图 2 显示我们的进度的屏幕截图为止通过浏览器查看时。


[![FormView 显示有关单个产品的信息](adding-client-side-confirmation-when-deleting-cs/_static/image3.png)](adding-client-side-confirmation-when-deleting-cs/_static/image2.png)

**图 2**: FormView 显示信息有关单个产品 ([单击以查看实际尺寸的图像](adding-client-side-confirmation-when-deleting-cs/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>步骤 2： 从删除按钮客户端 onclick 事件调用 confirm(string) 函数

与创建 FormView，最后一步是配置删除按钮此类，当它用在距访客，JavaScript 单击 s`confirm(string)`调用函数。 将客户端脚本添加到按钮、 LinkButton 或 ImageButton 的客户端`onclick`事件，可使用`OnClientClick property`，这是新为 ASP.NET 2.0。 由于我们想要使数值`confirm(string)`函数返回，只需将此属性设置为：`return confirm('Are you certain that you want to delete this product?');`

此更改之后删除 LinkButton s 声明性语法应如下所示：


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample3.aspx)]

该 s 就是它 ！ 图 3 所示为正在运行的这种确认屏幕截图。 单击删除按钮将显示确认对话框。 如果用户单击取消，将取消回发，并不会删除该产品。 如果，但是，用户单击确定，回发继续和 ObjectDataSource 的`Delete()`调用方法时，最后完成要删除的数据库记录。

> [!NOTE]
> 将字符串传递到`confirm(string)`用撇号 （而不是引号引起来） 分隔的 JavaScript 函数。 在 JavaScript 中，可以使用任一字符分隔字符串。 我们使用撇号此处以便的分隔符的字符串传递给`confirm(string)`不使用分隔符用于引入多义性`OnClientClick`属性值。


[![一条确认消息是现在显示时单击删除按钮](adding-client-side-confirmation-when-deleting-cs/_static/image6.png)](adding-client-side-confirmation-when-deleting-cs/_static/image5.png)

**图 3**: 确认信息是现在显示时单击删除按钮 ([单击以查看实际尺寸的图像](adding-client-side-confirmation-when-deleting-cs/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>步骤 3： 在 CommandField 中配置删除按钮 OnClientClick 属性

确认对话框中使用时的按钮、 LinkButton 或 ImageButton 直接在模板中，可以通过只需配置是与之关联其`OnClientClick`属性来返回结果的 JavaScript`confirm(string)`函数。 但是，将删除按钮的字段添加到的 GridView 或说明-的 CommandField 没有`OnClientClick`可以以声明方式设置的属性。 相反，我们必须以编程方式引用适当的 GridView 或说明 s 中的删除按钮`DataBound`事件处理程序，然后将设置其`OnClientClick`那里属性。

> [!NOTE]
> 设置删除按钮 s 时`OnClientClick`在相应的属性`DataBound`事件处理程序中，我们有权访问数据被绑定到当前记录。 这意味着我们可以扩展的确认消息包括特定的记录，有关详细信息，例如，"确实要删除牛奶产品？" 此类自定义项，还可以使用数据绑定语法的模板中。


为做法设置`OnClientClick`属性中 CommandField，让 s 删除按钮向页面添加一个 GridView。 配置为使用相同 FormView 使用的 ObjectDataSource 控件此 GridView。 此外限制 GridView 的 BoundFields 使其仅包含产品的名称、 类别和供应商。 最后，选中从 GridView s 智能标记启用删除复选框。 这将向 GridView s 添加 CommandField`Columns`具有集合其`ShowDeleteButton`属性设置为`true`。

进行这些更改后，你 GridView s 声明性标记应如下所示：


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample4.aspx)]

CommandField 包含可以从 GridView s 以编程方式访问的单个删除 LinkButton 实例`RowDataBound`事件处理程序。 后引用，我们可以设置其`OnClientClick`属性相应地。 创建的事件处理程序`RowDataBound`事件使用以下代码：


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample5.cs)]

此事件处理程序处理数据行 （那些将具有删除按钮），并且通过以编程方式引用删除按钮开始。 中通常使用以下模式：


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample6.cs)]

*ButtonType*是一种由 CommandField-按钮、 LinkButton 或 ImageButton 的按钮。 默认情况下，CommandField 使用 LinkButtons，但这可以通过 CommandField s 自定义`ButtonType property`。 *CommandFieldIndex*是内 GridView 的 CommandField 的序号索引`Columns`集合，而*controlIndex*是中 CommandField s 中的删除按钮的索引`Controls`集合。 *ControlIndex*值取决于在 CommandField 按钮 s 位置相对于其他按钮。 例如，如果唯一的按钮显示在 CommandField 删除按钮，请使用索引为 0。 如果，但是，在该处 s 之前删除按钮，编辑按钮使用的索引 2。 通过在删除按钮之前 CommandField 添加两个控件使用的 2 的索引的原因是: 编辑按钮和 LiteralControl s 用来添加编辑和删除按钮之间的一些空间。

对于本特定示例中，CommandField 使用 LinkButtons 和，正在最左侧的字段中，具有*commandFieldIndex*为 0。 由于没有其他按钮但 CommandField 中的删除按钮，我们使用*controlIndex*为 0。

在引用 CommandField 中的删除按钮之后, 我们下一步是抓住绑定到当前的 GridView 行产品有关的信息。 最后，我们设置删除按钮的`OnClientClick`适当的 javascript 中，其中包括产品的名称的属性。 由于 JavaScript 字符串传递给`confirm(string)`使用的撇号我们必须转义的产品的名称中出现任何撇号分隔函数。 任何撇号产品的名称中使用的转义具体而言，"`\'`"。

完成这些更改中，单击 GridView 显示一个自定义的确认对话框 （请参见图 4） 中的删除按钮。 为具有从 FormView，确认 messagebox 如果用户单击取消回发被取消时，从而防止在出现删除。

> [!NOTE]
> 此外可以使用此方法以编程方式访问说明如何在 CommandField 中的删除按钮。 对于说明，但是，d 你创建的事件处理程序`DataBound`事件，因为说明没有`RowDataBound`事件。


[![单击 GridView s 删除按钮会显示一个自定义的确认对话框](adding-client-side-confirmation-when-deleting-cs/_static/image9.png)](adding-client-side-confirmation-when-deleting-cs/_static/image8.png)

**图 4**： 单击删除按钮的 GridView s 显示自定义的确认对话框中 ([单击以查看实际尺寸的图像](adding-client-side-confirmation-when-deleting-cs/_static/image10.png))


## <a name="using-templatefields"></a>使用 TemplateFields

CommandField 的缺点之一是其按钮必须通过索引来访问和生成的对象，必须强制转换为相应的按钮类型 （按钮、 LinkButton 或 ImageButton）。 使用"幻数"和硬编码类型诚邀直到运行时将不能发现的问题。 例如，如果您或其他开发人员，将新按钮添加到在某些点 （例如一个编辑按钮） 在将来或更改 CommandField`ButtonType`属性，则现有的代码仍将编译未生成错误，但访问的页可能会引发异常或意外的行为，具体取决于你的代码编写方式和所做的更改。

另一种方法是将 GridView 和说明如何的命令转换为 TemplateFields。 这将生成与 TemplateField `ItemTemplate` CommandField 中每个按钮具有 LinkButton （或按钮或 ImageButton）。 这些按钮`OnClientClick`属性可以以声明方式分配，如我们看到 FormView，或可以以编程方式访问在相应`DataBound`事件处理程序使用以下模式：


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample7.cs)]

其中*controlID*是按钮的值`ID`属性。 虽然此模式仍需要用于强制转换的硬编码类型，它无需索引，从而使布局以更改不会导致运行时错误。

## <a name="summary"></a>摘要

JavaScript`confirm(string)`函数是一种用于控制窗体提交工作流的常用的技术。 在执行时，该函数将显示模式，客户端的对话框，其中包含两个按钮，确定和取消。 如果用户单击确定，`confirm(string)`函数返回`true`; 单击取消返回`false`。 此功能，结合了取消提交窗体，如果在提交过程中的事件处理程序返回一个浏览器的行为`false`，可以用于删除一条记录时显示确认消息框。

`confirm(string)`函数可以是与一个按钮 Web 控件的客户端相关联`onclick`事件处理程序通过控件的`OnClientClick`属性。 使用模板的其中一个 FormView 的模板中或者在中的说明或 GridView-TemplateField 中的删除按钮时可以设置此属性是以声明方式或以编程方式，正如我们在本教程中看到。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一页](implementing-optimistic-concurrency-cs.md)
[下一页](limiting-data-modification-functionality-based-on-the-user-cs.md)
