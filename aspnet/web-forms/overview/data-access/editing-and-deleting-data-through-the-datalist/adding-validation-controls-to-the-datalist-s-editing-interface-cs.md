---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
title: "将验证控件添加到 DataList 的编辑接口 (C#) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们会将验证控件添加到 DataList EditItemTemplate，以提供更安全的编辑用户 int。 是多么容易..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 3ecc21c5-da0e-40ab-abb4-fac1e47398ad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: b80b25933679d5c5b465af24cf6ff5d3b824b401
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-c"></a>将验证控件添加到 DataList 的编辑界面 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_CS.exe)或[下载 PDF](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/datatutorial39cs1.pdf)

> 在本教程中我们会将验证控件添加到 DataList EditItemTemplate，以提供更安全编辑的用户界面是多么容易。


## <a name="introduction"></a>介绍

在编辑教程为止 DataList，编辑接口 DataLists 没有包括任何主动的用户输入的验证，即使用户无效输入如缺少的产品名称或负的价格会引发异常。 在[前面教程](handling-bll-and-dal-level-exceptions-cs.md)，我们探讨了如何添加异常处理代码，以便 DataList 的`UpdateCommand`事件处理程序以捕获和正常显示有关已引发任何异常的信息。 但是，理想情况下，编辑界面应包括验证控制，以防止用户在第一个位置中输入此类无效数据。

在本教程中我们将看到将验证控件添加到 DataList s 是多么容易`EditItemTemplate`以便提供更安全编辑的用户界面。 具体而言，本教程采用在前面的教程创建的示例，并增强了编辑界面以包含相应的验证。

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-csmd"></a>步骤 1： 复制中的示例[处理 BLL 和 DAL 级别异常](handling-bll-and-dal-level-exceptions-cs.md)

在[处理 BLL-和 DAL 级别异常](handling-bll-and-dal-level-exceptions-cs.md)教程中我们创建列出的名称和两列，可编辑 DataList 中的产品的价格的页。 我们在本教程的目标是增加 DataList s 编辑界面，若要包括验证控件。 具体而言，我们验证逻辑将：

- 要求提供产品的名称
- 确保输入价格的值是一种有效的货币格式
- 确保输入的价格是大于或等于零，因为负数的值`UnitPrice`是非法的值

我们可以看看补充前面的示例，以包含验证之前，我们首先需要复制中的示例`ErrorHandling.aspx`页面`EditDeleteDataList`到本教程中的页的文件夹`UIValidation.aspx`。 若要实现此，我们需要通过同时复制`ErrorHandling.aspx`页面 s 声明性标记和其源代码。 首先通过声明性标记复制通过执行以下步骤：

1. 打开`ErrorHandling.aspx`Visual Studio 中的页
2. 请转到页面 s 声明性标记 （单击页面底部的源按钮）
3. 复制文本在内的`<asp:Content>`和`</asp:Content>`标记 （3 至 32 行），作为图 1 所示。


[![复制文本内&lt;asp： 内容&gt;控件](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image1.png)

**图 1**： 文本中复制`<asp:Content>`控件 ([单击以查看实际尺寸的图像](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image3.png))


1. 打开`UIValidation.aspx`页
2. 请转到页面 s 声明性标记
3. 将在文本粘贴`<asp:Content>`控件。

若要复制的源代码，请打开`ErrorHandling.aspx.vb`页上，并将复制不仅仅是文本*内*`EditDeleteDataList_ErrorHandling`类。 将复制三个事件处理程序 (`Products_EditCommand`， `Products_CancelCommand`，和`Products_UpdateCommand`) 连同`DisplayExceptionDetails`方法，但不是**不**复制类声明或`using`语句。 粘贴复制的文本*内*`EditDeleteDataList_UIValidation`类`UIValidation.aspx.vb`。

之后将移动到的内容和从代码上方`ErrorHandling.aspx`到`UIValidation.aspx`，花些时间测试浏览器中的页。 你应看到相同的输出和遇到每个 （请参见图 2） 这些两个页面中相同的功能。


[![UIValidation.aspx 页模仿 ErrorHandling.aspx 中的功能](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image4.png)

**图 2**:`UIValidation.aspx`页模仿中的功能`ErrorHandling.aspx`([单击以查看实际尺寸的图像](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>步骤 2： 将验证控件添加到 DataList 的 EditItemTemplate

构造数据输入窗体时, 很重要，则用户输入所有必填的字段和所有其提供的输入是合法的格式正确的值。 为了帮助确保用户的输入有效，ASP.NET 提供专门用于验证的单个输入 Web 控件的值的五个内置的验证控件：

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx)可确保已提供的值
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx)验证值与另一个 Web 控件值或一个常量值，或确保值的格式是合法的指定的数据类型
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx)确保值是值的范围之内
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx)验证值与[正则表达式](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx)验证值与自定义、 用户定义的方法

有关详细信息这些五个控件回头参考[将验证控件添加到的编辑和插入接口](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)教程或签出[验证控件部分](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx)的[ASP.NET 快速入门教程](https://quickstarts.asp.net)。

我们的教程，我们将需要使用 RequiredFieldValidator 以确保已提供的产品名称的值和 CompareValidator 以确保输入的价格大于或等于 0 的值而有效的货币格式提供。

> [!NOTE]
> 尽管 ASP.NET 1.x 具有这些相同的五个验证控件、 ASP.NET 2.0 已添加了大量改进，包括 Internet Explorer 浏览器以及到分区到页面上的验证控件的功能的两个正在客户端脚本支持主验证组。 有关 2.0 中的新验证控件功能的详细信息，请参阅[将 ASP.NET 2.0 中的验证控件](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)。


让我们来开始通过将必要的验证控件添加到 DataList 的`EditItemTemplate`。 通过设计器通过单击从 DataList s 智能标记，编辑模板链接或通过声明性语法，可以执行此任务。 允许 s 逐句通过使用设计视图中的编辑模板选项的过程。 选择要编辑 DataList s 后`EditItemTemplate`之后, 通过从工具箱拖动到模板编辑界面中，添加 RequiredFieldValidator 放`ProductName`文本框。


[![产品名称文本框中的后面添加 EditItemTemplate RequiredFieldValidator](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image7.png)

**图 3**： 添加到 RequiredFieldValidator `EditItemTemplate After` `ProductName`文本框中 ([单击以查看实际尺寸的图像](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image9.png))


所有验证控件验证单个 ASP.NET Web 控件的输入都工作。 因此，我们需要指示我们刚添加 RequiredFieldValidator 应验证`ProductName`文本框中; 这可通过设置验证控件 s [ `ControlToValidate`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx)到`ID`的相应的 Web 控件 (`ProductName`，在这种情况)。 接下来，设置[`ErrorMessage`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx)必须到提供的产品的名称和[`Text`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx)到\*。 `Text`属性值，如果提供，则如果验证失败，通过该验证控件显示的文本。 `ErrorMessage`属性值，该值是必需的由 ValidationSummary 控件; 如果`Text`省略属性值，`ErrorMessage`上输入无效的验证控件的显示属性值。

在设置后 RequiredFieldValidator 这三个属性，你的屏幕应类似于图 4。


[![设置 RequiredFieldValidator 的 ControlToValidate、 ErrorMessage 和文本属性](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image10.png)

**图 4**： 设置 RequiredFieldValidator s `ControlToValidate`， `ErrorMessage`，和`Text`属性 ([单击以查看实际尺寸的图像](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image12.png))


使用添加到 RequiredFieldValidator `EditItemTemplate`，所有保持是添加产品的价格文本框中的必要验证。 由于`UnitPrice`是可选时编辑记录，我们不需要添加 RequiredFieldValidator。 我们执行此操作，但是，需要添加 CompareValidator 以确保`UnitPrice`，如果提供，作为一种货币格式正确，大于或等于 0。

添加到 CompareValidator`EditItemTemplate`并设置其`ControlToValidate`属性`UnitPrice`，将其`ErrorMessage`价格属性必须大于或等于零，并且不能包括货币符号，并将其`Text`属性\*. 若要指示`UnitPrice`值必须是大于或等于 0，设置 CompareValidator s [ `Operator`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx)到`GreaterThanEqual`，将其[`ValueToCompare`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx)为 0，并其[`Type`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx)到`Currency`。

添加这些两个验证控件，DataList s 后`EditItemTemplate`s 声明性语法外观应与以下类似：


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

进行这些更改后，在浏览器中打开页面。 如果你尝试省略名称或输入无效的价格值编辑产品时，文本框旁边将出现一个星号。 如图 5 所示，包含如 19.95 美元的货币符号的价格值将被视为无效。 CompareValidator s `Currency` `Type` （如逗号或句号，具体取决于的区域性设置） 的数字分隔符的前导加号或减号，允许而不是*不*允许货币符号。 此行为可能 perplex 用户，如编辑接口当前呈现`UnitPrice`使用货币格式。


[![无效的输入的文本框旁边显示了星号](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image13.png)

**图 5**: 星号显示下一步到无效的输入的文本框 ([单击以查看实际尺寸的图像](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image15.png))


尽管验证可充当的是，用户必须手动编辑一个记录，不是可接受时删除的货币符号。 此外，如果有无效输入中编辑接口既不更新，也不取消按钮，当单击时，将调用回发。 理想情况下，取消按钮将返回 DataList 为预先编辑状态，而不考虑用户的输入的有效性。 此外，我们需要确保更新 DataList s 中的产品信息之前是否有效，s 的页面数据`UpdateCommand`事件处理程序，其浏览器不支持 JavaScript 或具有的用户可以跳过客户端逻辑的验证控件禁用其支持。

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>从 EditItemTemplate s 单价文本框中删除的货币符号

当使用 CompareValidator 的`Currency``Type`，正被验证的输入不能包含任何货币符号。 此类符号存在导致 CompareValidator 将标记为无效的输入。 但是，我们的编辑界面当前包括中的货币符号`UnitPrice`文本框中，这意味着用户必须在保存其更改之前显式删除的货币符号。 要解决此问题，我们有三个选项：

1. 配置`EditItemTemplate`以便`UnitPrice`文本框值未设置为货币的格式。
2. 允许用户通过删除 CompareValidator 并将它替换为格式正确的货币值检查 RegularExpressionValidator 输入货币符号。 这里的挑战是要验证货币值的正则表达式不是像 CompareValidator 一样简单，并且如果我们想要合并的区域性设置需要编写代码。
3. 完全删除验证控件和依赖于 GridView s 中的自定义服务器端验证逻辑`RowUpdating`事件处理程序。

让我们来使用本教程中的选项 1。 当前`UnitPrice`格式化为货币值中文本框中的数据绑定表达式由于`EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`。 更改`Eval`语句`Eval("UnitPrice", "{0:n2}")`，其中将结果格式化为大量具有两位数的精度。 这可以通过声明性语法直接或通过单击从编辑数据绑定链接`UnitPrice`DataList s 中的文本框中`EditItemTemplate`。

进行此更改后，编辑界面中的格式化的价格包括作为组分隔符的逗号和句点作为小数分隔符，但保留关闭的货币符号。

> [!NOTE]
> 可编辑的接口在从删除货币格式时, 找到它有助于将作为外部文本框中的文本放在货币符号。 作为对它们不需要提供的货币符号的用户的提示，此选项。


## <a name="fixing-the-cancel-button"></a>修复取消按钮

默认情况下，验证 Web 控件发出 JavaScript 客户端上执行验证。 单击按钮、 LinkButton 或 ImageButton 时，回发发生之前针对客户端检查页上的验证控件。 如果没有任何无效的数据，将被取消回发。 对于某些按钮，不过，数据的有效性可能重要;在这种情况下，无回发由于无效的数据已被取消是一种干扰。

取消按钮是此类示例。 假设用户输入无效数据，如省略产品的名称，，然后决定她没有 t 想要在所有保存产品并触发取消按钮。 目前，取消按钮将触发验证控件在页上，该报告的产品名称缺失，防止回发。 我们的用户必须键入一些文本到`ProductName`文本框中，只是为了取消退出编辑过程。

幸运的是，按钮、 LinkButton 和 ImageButton 具有[`CausesValidation`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx)可以指示是否单击按钮应启动的验证逻辑 (默认为`True`)。 设置取消按钮 s`CausesValidation`属性`False`。

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>确保输入是限于 UpdateCommand 事件处理程序中的有效

由于发出的验证控件中，客户端脚本如果用户输入无效的输入验证控件取消按钮，LinkButton，由启动任何回发 ImageButton 控件或其`CausesValidation`属性`True`(默认值）。 但是，如果用户访问过时的浏览器或其中一个已禁用其 JavaScript 支持中，将不会执行客户端验证检查。

ASP.NET 验证控件的所有重复回发时将立即对其验证逻辑并报告通过页的输入的总体有效性[`Page.IsValid`属性](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx)。 但是，页面流未中断或停止任何方式基于的值中`Page.IsValid`。 作为开发人员而言，是，我们有责任确保`Page.IsValid`属性具有的值`True`假定有效的代码继续输入数据之前。

如果用户具有禁用的 JavaScript，访问我们的页面、 编辑产品、 进入太价格值占用大量资源，并单击更新按钮，将绕过客户端验证和回发将随之发生。 在回发时，ASP.NET 页 s`UpdateCommand`事件处理程序执行，并在尝试分析太时引发异常的成本较低`Decimal`。 由于我们有异常处理，将处理此类异常正常，但我们可以通过第一个位置通过仅继续执行操作滑动阻止无效的数据`UpdateCommand`事件处理程序如果`Page.IsValid`的值为`True`。

将以下代码添加到开始`UpdateCommand`事件处理程序中，紧靠之前`Try`块：


[!code-csharp[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample2.cs)]

添加此元素后，产品将尝试更新仅当已提交的数据有效。 获胜 t 的大多数用户能够回发无效数据由于验证控件客户端脚本，但用户的浏览器不支持 JavaScript 或具有支持的 JavaScript 禁用，可以绕过客户端检查和提交的数据无效。

> [!NOTE]
> 敏锐读取器使用 GridView，更新数据时将记住，我们没有 t 需要显式检查`Page.IsValid`我们页面的代码隐藏类中的属性。 这是因为 GridView 咨询`Page.IsValid`属性对于我们和仅与更新仅当它返回的值时，才会继续进行`True`。


## <a name="step-3-summarizing-data-entry-problems"></a>步骤 3： 汇总数据条目问题

除了五个验证控件中，包括 ASP.NET [ValidationSummary 控件](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx)，其中显示`ErrorMessage`s 检测到无效的数据的那些验证控件。 此摘要数据可以显示为文本在网页上或通过模式，客户端的消息框。 让我们来增强此教程，以包括客户端的消息框汇总任何验证问题。

若要完成此操作，请从工具箱中拖动到设计器拖动 ValidationSummary 控件。 位置 ValidationSummary 控件不真正有意义，因为我们重新要将其配置为仅显示摘要作为消息框。 在添加控件后, 设置其[`ShowSummary`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx)到`False`及其[`ShowMessageBox`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx)到`True`。 添加此元素后，任何验证错误汇总中一个客户端的 messagebox （请参阅图 6）。


[![客户端 Messagebox 中总结了验证错误](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image16.png)

**图 6**： 一个客户端的 Messagebox 中总结了验证错误 ([单击以查看实际尺寸的图像](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image18.png))


## <a name="summary"></a>摘要

在本教程中我们已了解如何通过使用验证控件主动确保我们的用户输入有效在之前尝试更新工作流中使用它们减少引发异常的可能性。 ASP.NET 提供五个验证 Web 控件，旨在检查特定 Web 控制 s 输入和报告回输入的 s 有效性。 在本教程中我们使用这些五个控件的两个 RequiredFieldValidator 和 CompareValidator 以确保提供了产品的名称和价格，必须使用大于或等于零的值以货币格式。

将验证控件添加到编辑接口 DataList s 非常简单，拖动到`EditItemTemplate`从工具箱和设置少量的属性。 默认情况下，验证控件自动发出客户端验证脚本;它们还提供服务器端验证回发时，存储中的累积结果`Page.IsValid`属性。 若要在单击按钮、 LinkButton 或 ImageButton 时绕过客户端验证，请设置按钮 s`CausesValidation`属性`False`。 此外，使用在回发时提交的数据执行任何任务之前, 确保`Page.IsValid`属性返回`True`。

所有编辑教程 DataList 我们的产品 s 名称文本框中，另一个用于价格中检查到目前为止已获得了非常简单的编辑界面。 编辑接口，但是，可以包含多种不同的 Web 控件，如 DropDownLists、 日历、 单选按钮、 复选框，依次类推。 在我们的下一步教程中我们将查看生成一个接口，使用各种 Web 控件。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Dennis Patterson、 Ken Pespisa 和沈 Shulok。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](handling-bll-and-dal-level-exceptions-cs.md)
[下一页](customizing-the-datalist-s-editing-interface-cs.md)
