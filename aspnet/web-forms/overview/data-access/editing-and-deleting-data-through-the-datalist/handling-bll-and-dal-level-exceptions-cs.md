---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
title: "处理 BLL 和 DAL 级别异常 (C#) |Microsoft 文档"
author: rick-anderson
description: "在本教程中，我们将了解如何巧妙地处理可编辑 DataList 更新工作流过程中引发的异常。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: f8fd58e2-f932-4f08-ab3d-fbf8ff3295d2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: 284f3bebcd07c9cc70b3a7d645f353f5797889a7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="handling-bll--and-dal-level-exceptions-c"></a>处理 BLL 和 DAL 级别异常 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_CS.exe)或[下载 PDF](handling-bll-and-dal-level-exceptions-cs/_static/datatutorial38cs1.pdf)

> 在本教程中，我们将了解如何巧妙地处理可编辑 DataList 更新工作流过程中引发的异常。


## <a name="introduction"></a>介绍

在[概述的编辑和删除数据中 DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)教程中，我们创建 DataList 提供简单的编辑和删除功能。 当完全正常运行时，很难用户友好，为编辑过程中发生任何错误，或删除过程导致未经处理的异常。 例如，省略产品的名称或，在编辑产品时，输入价格值的十分合理 ！，将引发异常。 在代码中没有捕获此异常，因为它将冒泡到 ASP.NET 运行，然后在网页中显示的异常 s 详细信息。

正如我们看到在[处理 BLL-和 ASP.NET 页中的 DAL 级别异常](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)教程中，如果从深度的业务逻辑或数据访问层，将引发异常的异常详细信息将返回到 ObjectDataSource 和然后到 GridView。 我们已了解如何适当地处理这些异常，通过创建`Updated`或`RowUpdated`ObjectDataSource 或 GridView，检查异常，然后指示处理了该异常的事件处理程序。

我们 DataList 的教程，但是，不 t ObjectDataSource 用于更新和删除数据。 相反，我们正在直接针对 BLL。 为了检测源自 BLL 或 DAL 的异常，我们需要实现异常处理代码中我们 ASP.NET 页的代码隐藏。 在本教程中，我们将了解如何更巧妙地处理期间更新工作流可编辑 DataList s 引发的异常。

> [!NOTE]
> 在*概述的编辑和删除数据中 DataList*教程中我们讨论了不同的技术来编辑和删除数据从 DataList，一些技术涉及更新使用 ObjectDataSource 和正在删除。 如果你使用这些技术，你可以处理从 BLL 或 DAL 异常通过 ObjectDataSource s`Updated`或`Deleted`事件处理程序。


## <a name="step-1-creating-an-editable-datalist"></a>步骤 1： 创建可编辑 DataList

我们担心如何处理在更新的工作流过程中发生的异常之前，让我们来首先创建可编辑 DataList。 打开`ErrorHandling.aspx`页面`EditDeleteDataList`文件夹中，将 DataList 添加到设计器中，设置其`ID`属性`Products`，并添加名为新 ObjectDataSource `ProductsDataSource`。 配置对象数据源以使用`ProductsBLL`类的`GetProducts()`选择方法记录; 在 INSERT、 UPDATE、 中设置的下拉列表并删除选项卡添加到 （无）。


[![返回使用 GetProducts() 方法的产品信息](handling-bll-and-dal-level-exceptions-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-cs/_static/image1.png)

**图 1**： 返回产品信息使用`GetProducts()`方法 ([单击以查看实际尺寸的图像](handling-bll-and-dal-level-exceptions-cs/_static/image3.png))


完成对象数据源向导后，Visual Studio 自动创建`ItemTemplate`DataList 有关。 将此与替换`ItemTemplate`，显示每个产品 s name 和价格，并包括一个编辑按钮。 接下来，创建`EditItemTemplate`与名称和价格和更新和取消按钮的文本框中 Web 控件。 最后，设置 DataList 的`RepeatColumns`属性设置为 2。

这些更改后你页面 s 声明性标记应类似于以下。 编辑、 取消，请确保仔细检查和更新按钮具有其`CommandName`属性设置为编辑，请取消，并更新，分别。


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample1.aspx)]

> [!NOTE]
> 对于本教程 DataList 必须启用 s 视图状态。


花些时间查看我们通过浏览器的进度 （请参见图 2）。


[![每个产品包括一个编辑按钮](handling-bll-and-dal-level-exceptions-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-cs/_static/image4.png)

**图 2**： 每个产品包括一个编辑按钮 ([单击以查看实际尺寸的图像](handling-bll-and-dal-level-exceptions-cs/_static/image6.png))


目前，编辑按钮仅导致回发它没有 t 尚未使产品可编辑。 若要启用编辑，我们需要创建的事件处理程序 DataList s `EditCommand`， `CancelCommand`，和`UpdateCommand`事件。 `EditCommand`和`CancelCommand`事件只需更新 DataList 的`EditItemIndex`属性和重新绑定数据到 DataList:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample2.cs)]

`UpdateCommand`事件处理程序是有些复杂。 它需要编辑产品 s 中读取`ProductID`从`DataKeys`以及产品的名称和价格与中的文本框集合`EditItemTemplate`，然后调用`ProductsBLL`类的`UpdateProduct`之前返回 DataList 方法为其预编辑状态。

现在，让 s 只需使用完全相同的代码从`UpdateCommand`中的事件处理程序*概述的编辑和删除数据中 DataList*教程。 我们将添加代码以正常处理步骤 2 中的异常。


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample3.cs)]

遇到无效的输入可以是名称的形式的格式不正确的单位价格、 类似 5.00 美元，非法的单位价格值或省略产品，将引发异常。 由于`UpdateCommand`事件处理程序不包括在此时处理代码的任何异常，异常将向上冒泡到 ASP.NET 运行时，将在其中它显示给最终用户 （请参见图 3）。


![当未经处理的异常发生时，最终用户看到的错误页](handling-bll-and-dal-level-exceptions-cs/_static/image7.png)

**图 3**： 未经处理的异常时，最终用户将看到一个错误页面


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>步骤 2： 正常处理 UpdateCommand 事件处理程序中的异常

在更新工作流，异常可以会出现在`UpdateCommand`事件处理程序、 BLL 或 DAL。 例如，如果用户输入指价格为太占用大量资源，`Decimal.Parse`中的语句`UpdateCommand`事件处理程序将引发`FormatException`异常。 如果用户省略产品的名称或 DAL 如果价格具有负值，将引发的异常。

当发生异常时，我们想要显示一条信息性消息页本身中。 添加标签 Web 控件到页`ID`设置为`ExceptionDetails`。 配置要显示为红色，超大型，通过分配加粗和倾斜字体的标签的文本其`CssClass`属性`Warning`中定义的 CSS 类`Styles.css`文件。

发生错误时，我们只想要一次显示的标签。 也就是说，在后续回发上, 标签的警告消息将会消失。 这可以通过标签 s 任一清除`Text`属性或设置其`Visible`属性`False`中`Page_Load`事件处理程序 (正如我们做进来[处理 BLL-和 ASP DAL 级异常.NET 页](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)教程) 或通过禁用标签的视图状态支持。 让我们来使用后一个选项。


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample4.aspx)]

当引发异常时，我们将分配到异常的详细信息`ExceptionDetails`标签控件的`Text`属性。 由于其视图状态禁用的可以在后续回发`Text`属性 s 以编程方式更改都将丢失，恢复为默认文本 （空字符串），从而隐藏警告消息。

若要确定在已以有用的消息显示在页面上引发错误时，我们需要添加`Try ... Catch`阻止`UpdateCommand`事件处理程序。 `Try`部分包含代码，它可能会导致异常，而`Catch`块包含面临异常执行的代码。 签出[异常处理基础知识](https://msdn.microsoft.com/library/2w8f0bss.aspx)主题中的详细信息的.NET Framework 文档上`Try ... Catch`块。


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample5.cs)]

通过中的代码引发的任何类型异常`Try`块，`Catch`块的代码将开始执行。 引发异常的类型`DbException`， `NoNullAllowedException`， `ArgumentException`，并在完全匹配，依赖于什么，首先引发错误。 如果在数据库级别，那里的问题`DbException`将引发。 如果输入非法值`UnitPrice`， `UnitsInStock`， `UnitsOnOrder`，或`ReorderLevel`字段，`ArgumentException`会不引发，随着我们添加代码，以验证这些字段值`ProductsDataTable`类 (请参阅[创建业务逻辑层](../introduction/creating-a-business-logic-layer-cs.md)教程)。

我们可以通过使基于捕获到异常的类型的消息文本对最终用户提供的更有帮助的说明。 下面的代码时使用几乎完全相同的形式返回[处理 BLL-和 ASP.NET 页中的 DAL 级别异常](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)教程提供此级别的详细信息：


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample6.cs)]

若要完成本教程，只需调用`DisplayExceptionDetails`方法从`Catch`块已捕获传入`Exception`实例 (`ex`)。

与`Try ... Catch`就地块中，用户提供更详细的错误消息，显示为图 4 和 5 的显示。 请注意，在遇到异常 DataList 处于编辑模式。 这是因为一旦出现异常，控制流将立即重定向到`Catch`块，绕过 DataList 返回其预编辑状态代码。


[![如果用户省略必需的字段，显示错误消息](handling-bll-and-dal-level-exceptions-cs/_static/image9.png)](handling-bll-and-dal-level-exceptions-cs/_static/image8.png)

**图 4**： 如果用户省略必需的字段，将显示错误消息 ([单击以查看实际尺寸的图像](handling-bll-and-dal-level-exceptions-cs/_static/image10.png))


[![一条错误消息是显示时输入负价格](handling-bll-and-dal-level-exceptions-cs/_static/image12.png)](handling-bll-and-dal-level-exceptions-cs/_static/image11.png)

**图 5**: 错误消息是显示时输入负的价格 ([单击以查看实际尺寸的图像](handling-bll-and-dal-level-exceptions-cs/_static/image13.png))


## <a name="summary"></a>摘要

GridView 和 ObjectDataSource 提供后级别事件处理程序，包括有关任何更新和删除工作流期间引发的异常以及可以设置，以指示已被异常的属性信息处理。 这些功能，但是，不可用时将使用 DataList 和直接使用 BLL。 相反，我们是负责实现异常处理。

在本教程中我们已了解如何添加异常处理程序来更新工作流通过添加一个可编辑 DataList s`Try ... Catch`阻止`UpdateCommand`事件处理程序。 如果在更新工作流，将引发异常`Catch`块的代码的执行，显示中的有用信息`ExceptionDetails`标签。

此时，DataList 不会尝试防止从第一个位置发生的异常。 即使我们知道，负价格将导致异常，我们还 t 尚未添加任何功能，以主动防止用户输入此类无效输入。 在我们的下一步教程中，我们将了解如何帮助减少通过添加验证控件中的导致无效的用户输入的异常`EditItemTemplate`。

尽情享受编程 ！

## <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [异常的设计准则](https://msdn.microsoft.com/library/ms298399.aspx)
- [错误日志记录模块和处理程序 (ELMAH)](http://workspaces.gotdotnet.com/elmah) （记录错误的开放源代码库）
- [.NET Framework 2.0 的 Enterprise Library](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) （包括异常管理应用程序块）

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Ken Pespisa。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](performing-batch-updates-cs.md)
[下一页](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
