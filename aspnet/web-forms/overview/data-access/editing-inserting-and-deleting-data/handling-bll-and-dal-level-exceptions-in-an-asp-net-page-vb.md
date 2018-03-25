---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
title: "处理 ASP.NET 页 (VB) 中的 BLL 和 DAL 级别异常 |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将了解如何显示友好、 带有提示性错误消息应在插入、 更新或删除操作的过程可能出现异常..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 129d4338-1315-4f40-89b5-2b84b807707d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 2269458cbc41fd3a483aaade0f07288ee805bdd1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-vb"></a>处理 ASP.NET 页 (VB) 中的 BLL 和 DAL 级别异常
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_VB.exe)或[下载 PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/datatutorial18vb1.pdf)

> 在本教程中我们将了解如何以显示友好、 带有提示性错误消息应在插入、 更新或删除操作的 Web 控件的 ASP.NET 数据期间可能出现异常。


## <a name="introduction"></a>介绍

使用 ASP.NET web 应用程序使用分层应用程序体系结构的数据涉及以下三个常规步骤：

1. 确定哪种业务逻辑层方法需要调用，以及要向其传递哪些参数值。 参数值可以硬编码，以编程方式分配或由用户输入的输入。
2. 调用方法。
3. 处理的结果。 当调用某个用于返回数据的 BLL 方法，这可能涉及到将数据绑定到数据 Web 控件。 对于 BLL 修改数据的方法，这可能包括在执行某些操作基于返回值或正常处理在步骤 2 中出现的任何异常。

正如我们看到在[以前一教程](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)，ObjectDataSource 和 Web 控件的数据为步骤 1 和 3 提供扩展点。 GridView，例如，触发其`RowUpdating`事件之前将其字段值分配到其 ObjectDataSource`UpdateParameters`集合; 其`RowUpdated`ObjectDataSource 完成该操作后，将引发事件。

我们已检查在步骤 1 期间激发并已了解如何使用它们可以自定义输入的参数或取消操作的事件。 在本教程中我们将着重完成操作后激发的事件。 与我们可以除了别的之外这些后级别事件处理程序确定操作过程中是否发生异常，并在屏幕上显示的友好、 带有提示性错误消息，而不是默认为标准 ASP.NET 正常处理异常页。

为了说明使用这些后级别的事件，让我们创建的页面列出中可编辑的 GridView 的产品。 更新某一产品，如果引发异常引发时我们 ASP.NET 页将显示一条短消息上面 GridView 说明发生问题。 让我们进入正题！

## <a name="step-1-creating-an-editable-gridview-of-products"></a>步骤 1： 创建产品可编辑 GridView

在以前的教程，我们使用创建的可编辑的 GridView 仅使用两个字段，`ProductName`和`UnitPrice`。 这需要创建的其他重载`ProductsBLL`类的`UpdateProduct`方法，仅接受三个输入的参数 （该产品的名称、 单价和 ID），而非每个产品字段的参数的一个。 对于本教程，让我们练习此技术同样，创建可编辑的 GridView 股价图、 显示产品的名称，每个单元、 单价和单位数量，但只允许的名称、 单价和单位，要编辑的库存量。

若要适应这种情况下，我们将需要的另一个重载`UpdateProduct`方法、 一个接受四个参数： 产品的名称、 单价、 单元以股价图、 和 id。 添加以下方法`ProductsBLL`类：


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample1.vb)]

使用此方法完成的情况下，我们已准备好创建 ASP.NET 页，可用于编辑这些四个特定产品字段。 打开`ErrorHandling.aspx`页面`EditInsertDelete`文件夹并将一个 GridView 添加到设计器中翻页。 将 GridView 绑定到新的对象数据源，映射`Select()`方法`ProductsBLL`类的`GetProducts()`方法和`Update()`方法`UpdateProduct`刚创建的重载。


[![请使用接受四个输入的参数的 UpdateProduct 方法重载](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image1.png)

**图 1**： 使用`UpdateProduct`方法重载，接受四个输入参数 ([单击以查看实际尺寸的图像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image3.png))


这将创建与 ObjectDataSource`UpdateParameters`具有四个参数和与字段 GridView 为每个产品字段的集合。 ObjectDataSource 的声明性标记分配`OldValuesParameterFormatString`属性值`original_{0}`，这将导致异常，因为我们 BLL 类不预期名为的输入的参数`original_productID`要在中传递。 别忘了删除此设置完全声明性语法 (或将其设置为默认值， `{0}`)。

接下来，削减 GridView 仅包含`ProductName`， `QuantityPerUnit`， `UnitPrice`，和`UnitsInStock`BoundFields。 此外可随意应用任何字段级别格式设置你认为所必要 (例如，更改`HeaderText`属性)。

在以前的教程，我们讨论在设置格式的方式`UnitPrice`BoundField 作为一种货币同时处于只读模式和编辑模式。 让我们执行相同的此处。 回想一下，这需要设置 BoundField`DataFormatString`属性`{0:c}`，将其`HtmlEncode`属性`false`，并将其`ApplyFormatInEditMode`到`true`，如图 2 所示。


[![将显示 UnitPrice BoundField 配置为一种货币](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image4.png)

**图 2**： 配置`UnitPrice`BoundField 显示为一种货币 ([单击以查看实际尺寸的图像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image6.png))


格式设置`UnitPrice`因为编辑界面中的货币需要为 GridView 创建事件处理程序`RowUpdating`货币格式字符串分析成的事件`decimal`值。 回想一下，`RowUpdating`最后一个教程中的事件处理程序还检查以确保用户提供`UnitPrice`值。 但是，对于本教程让我们允许用户以省略价格。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample2.vb)]

我们 GridView 包括`QuantityPerUnit`BoundField 但此 BoundField 应仅用于显示目的，不应由用户可编辑。 若要这样安排，只需设置 BoundFields 的`ReadOnly`属性`true`。


[![使 QuantityPerUnit BoundField 只读](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image7.png)

**图 3**： 请`QuantityPerUnit`BoundField 只读 ([单击以查看实际尺寸的图像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image9.png))


最后，选中从 GridView 的智能标记启用编辑复选框。 完成这些步骤之后`ErrorHandling.aspx`页面的设计器应类似于图 4。


[![移除所有元素，保留所需 BoundFields 并检查编辑复选框启用](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image10.png)

**图 4**： 删除以外的所有需要 BoundFields 并选中启用编辑复选框 ([单击以查看实际尺寸的图像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image12.png))


此时我们拥有的所有产品的列表`ProductName`， `QuantityPerUnit`， `UnitPrice`，和`UnitsInStock`字段; 但是，仅`ProductName`， `UnitPrice`，和`UnitsInStock`可编辑字段。


[![用户现在可以轻松地编辑产品的名称、 价格和常用的字段中的单位](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image13.png)

**图 5**： 用户可以现在轻松编辑产品的名称、 价格和单位中 Stock 字段 ([单击以查看实际尺寸的图像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>步骤 2： 正常处理 DAL 级别异常

尽管我们可编辑的 GridView 非常有效用户时输入用于编辑的产品的名称、 价格和库存量的合法值，输入非法值将引发异常。 例如，省略`ProductName`值使[NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp)以来引发`ProductName`中的属性`ProdcutsRow`类具有其`AllowDBNull`属性设置为`false`; 如果数据库已关闭，`SqlException`尝试连接到数据库时，将由 TableAdapter 引发。 不执行任何操作，这些异常向上冒泡从数据访问层到业务逻辑层，再到 ASP.NET 页中，依次向 ASP.NET 运行时。

具体取决于如何配置 web 应用程序以及是否正在访问应用程序与`localhost`，未经处理的异常会导致一个泛型的服务器错误页、 详细的错误报表或用户友好的网页。 请参阅[Web 应用程序错误处理在 ASP.NET 中](http://www.15seconds.com/issue/030102.htm)和[customErrors 元素](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx)有关 ASP.NET 运行时如何响应未捕获的异常的详细信息。

图 6 显示了尝试将产品更新而无需指定时遇到的屏幕`ProductName`值。 这是默认值时在随后显示详细的错误报告`localhost`。


[![省略该产品的名称将显示异常详细信息](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image16.png)

**图 6**： 省略该产品的名称将显示异常详细信息 ([单击以查看实际尺寸的图像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image18.png))


虽然测试的应用时，此类异常详细信息很有用，向最终用户提供此类屏幕在遇到异常时也是不理想。 最终用户可能不知道什么`NoNullAllowedException`是或已出现此错误的原因。 更好的方法是向用户提供一个更加友好的用户的消息，解释了尝试更新产品的问题。

如果在执行该操作时，则会发生异常，ObjectDataSource 和数据 Web 控件中的后级别事件提供对其进行检测和取消异常向上冒泡到 ASP.NET 运行时的方法。 对于我们的示例，让我们将创建为 GridView 的事件处理程序`RowUpdated`确定异常激发，如果是这样，标签 Web 控件中显示的异常详细信息的事件。

通过将标签添加到 ASP.NET 页中，设置启动其`ID`属性`ExceptionDetails`和清除其`Text`属性。 若要绘制到此消息的用户的眼睛，设置其`CssClass`属性`Warning`，我们将添加到的 CSS 类`Styles.css`以前一教程中的文件。 回想一下，此 CSS 类会导致将标签的文本中的红色、 斜体、 加粗和特大型字体显示。


[![向页面添加标签 Web 控件](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image19.png)

**图 7**： 将标签 Web 控件添加到页 ([单击以查看实际尺寸的图像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image21.png))


由于我们希望此标签 Web 控制选项可立即可见仅后发生了异常，设置其`Visible`属性设置为 false 在`Page_Load`事件处理程序：


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample3.vb)]

使用此代码中，第一次页访问和后续回发`ExceptionDetails`控件将具有其`Visible`属性设置为`false`。 在遇到时 DAL 或 BLL 级别的异常，我们可以检测在 GridView 的`RowUpdated`事件处理程序，我们将设置`ExceptionDetails`控件的`Visible`属性为 true。 由于 Web 控件事件处理程序之后发生`Page_Load`页面生命周期中的事件处理程序，将显示标签。 但是，在下一步的回发，`Page_Load`事件处理程序将恢复`Visible`属性改回`false`，再次隐藏从视图。

> [!NOTE]
> 或者，我们无法删除设置必要条件`ExceptionDetails`控件的`Visible`中的属性`Page_Load`通过分配其`Visible`属性`false`中的声明性语法和禁用 （设置其其视图状态`EnableViewState`属性`false`)。 在将来的教程中，我们将使用此备用方法。


与添加的标签控件，我们的下一步是创建 GridView 的事件处理程序`RowUpdated`事件。 在设计器中选择 GridView，转到属性窗口中，然后单击闪电形图标，列出 GridView 的事件。 应该已经有一个条目 gridview 的`RowUpdating`事件，我们在本教程前面创建的事件处理程序此事件。 创建的事件处理程序`RowUpdated`以及事件。


![GridView RowUpdated 事件创建事件处理程序](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image22.png)

**图 8**： 为 GridView 创建事件处理程序`RowUpdated`事件


> [!NOTE]
> 你还可以在代码隐藏类文件的顶部创建事件处理程序通过下拉列表。 从左侧的下拉列表中选择 GridView 和`RowUpdated`从右侧的一个事件。


创建此事件处理程序后，将下面的代码添加到 ASP.NET 页的代码隐藏类中：


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample4.vb)]

此事件处理程序的第二个输入的参数是类型的对象[GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx)，它具有用于处理的异常感兴趣的三个属性：

- `Exception`对引发的异常; 的引用如果已不引发任何异常，此属性将具有的值`null`
- `ExceptionHandled`一个布尔值，该值指示是否在处理了该异常`RowUpdated`事件处理程序; 如果`false`（默认值），例外情况是重新引发，percolating 到 ASP.NET 运行时
- `KeepInEditMode`如果设置为`true`编辑过的 GridView 行处于编辑模式; 如果`false`（默认值）、 GridView 行将恢复到其只读模式

我们的代码，然后，应检查以查看是否`Exception`不`null`，这意味着执行操作时引发了一个异常。 如果出现这种情况，我们希望：

- 显示中的用户友好消息`ExceptionDetails`标签
- 指示处理了异常
- 保持 GridView 行处于编辑模式

下面的代码来实现这些目标：


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample5.vb)]

此事件处理程序首先检查是否`e.Exception`是`null`。 如果不是，`ExceptionDetails`标签的`Visible`属性设置为`true`及其`Text`属性设置为"时出现问题更新产品。" 实际引发的异常的详细信息位于`e.Exception`对象的`InnerException`属性。 此内部异常进行检查; 如果它是特定类型，其他、 有用的消息会追加到`ExceptionDetails`标签的`Text`属性。 最后，`ExceptionHandled`和`KeepInEditMode`属性都设置为`true`。

图 9 显示此页的屏幕截图时省略产品; 的名称图 10 显示的结果时输入了非法`UnitPrice`值 (-50)。


[![ProductName BoundField 必须包含值](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image23.png)

**图 9**: `ProductName` BoundField 必须包含值 ([单击以查看实际尺寸的图像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image25.png))


[![负 UnitPrice 值是不允许](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image26.png)

**图 10**： 负`UnitPrice`值是不允许 ([单击以查看实际尺寸的图像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image28.png))


通过设置`e.ExceptionHandled`属性`true`、`RowUpdated`事件处理程序已指示它已处理的异常。 因此，该异常不会向上传播到 ASP.NET 运行时。

> [!NOTE]
> 图 9 和 10 显示的正常方式来处理由于无效的用户输入而引发的异常。 理想情况下，不过，此类输入无效将永远不会到达业务逻辑层在第一个位置，如 ASP.NET 页应确保在调用之前的用户的输入是否有效`ProductsBLL`类的`UpdateProduct`方法。 在我们下一步的教程，我们将了解如何将验证控件添加到这些编辑和插入接口，以便确保提交到业务逻辑层的数据符合业务规则。 验证控件不只是防止调用`UpdateProduct`方法，直到用户提供的数据有效，但还提供用于标识数据条目问题的信息更丰富的用户体验。


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>步骤 3： 正常处理 BLL 级别异常

在插入时，更新，或删除数据，数据访问层可能引发遇到与数据相关的错误异常。 数据库可能处于脱机状态、 所需的数据库表列可能不具有指定值或表级约束可能违反了。 除了严格与数据相关的异常，业务逻辑层可以使用异常来指示何时违反了业务规则。 在[创建业务逻辑层](../introduction/creating-a-business-logic-layer-vb.md)教程中，例如，我们将业务规则检查添加到原始`UpdateProduct`重载。 具体而言，如果用户已将产品标记为停止使用，我们要求产品不应只有一个其供应商提供的。 如果违反此条件，`ApplicationException`抛出。

有关`UpdateProduct`本教程中创建的重载，让我们添加业务规则禁止`UnitPrice`字段从设置为新值的两倍以上原始`UnitPrice`值。 若要完成此操作，调整`UpdateProduct`重载，以便它执行此检查，并引发`ApplicationException`如果违反规则。 更新的方法如下所示：


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample6.vb)]

进行此更改后，将导致任何价格所更新的两倍以上的现有价格`ApplicationException`引发。 就像从 DAL，这 BLL 引发引发的异常`ApplicationException`可以检测到并且在 GridView 的中处理`RowUpdated`事件处理程序。 事实上，`RowUpdated`事件处理程序的代码，顾名思义，将正确检测此异常并显示`ApplicationException`的`Message`属性值。 图 11 显示屏幕截图当用户尝试更新牛奶的价格为 $50.00，即多个双精度 $19.95 其当前价格。


[![业务规则不允许多个双击产品的价格的价格增加](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image29.png)

**图 11**： 业务规则不允许多个双击产品的价格的价格增加 ([单击以查看实际尺寸的图像](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image31.png))


> [!NOTE]
> 理想情况下我们业务逻辑规则要重构外`UpdateProduct`方法重载，并放入一种常用方法。 这是为读取器作为练习保留您的问题。


## <a name="summary"></a>摘要

在插入、 更新和删除操作，数据 Web 控件和涉及的 ObjectDataSource 激发前和后级别事件该书挡实际操作。 正如我们看到在本教程和前一次，使用可编辑的 GridView 时 GridView`RowUpdating`事件激发，跟 ObjectDataSource`Updating`事件，此时更新命令供 ObjectDataSource基础对象。 完成该操作后，ObjectDataSource`Updated`事件激发，跟 GridView`RowUpdated`事件。

我们可以创建自定义输入的参数才能预级别的事件，或者针对以便检查和响应为操作的结果后级别事件的事件处理程序。 后级别事件处理程序通常用于检测是否在操作期间发生了异常。 遇到异常，这些后级别事件处理程序可以根据需要处理自己的异常。 在本教程中，我们将了解如何通过显示友好错误消息来处理此异常。

在下一步的教程，我们将了解如何降低引发异常时产生的数据格式设置问题的可能性 (例如，输入一个负数`UnitPrice`)。 具体而言，我们将了解如何将验证控件添加到的编辑和插入接口。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已沈 Shulok。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
[下一页](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
