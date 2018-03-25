---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
title: "设置 DataList 和转发器的格式取决于数据 (C#) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将逐步了解我们如何格式化 DataList 和转发器控件，通过使用格式设置的函数的外观的示例..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 83e3d759-82b8-41e6-8d62-f0f4b3edec41
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 604aa63919a881e828b6a3620360c3d1133c5830
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-c"></a>格式设置的 DataList 和转发器取决于数据 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_CS.exe)或[下载 PDF](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/datatutorial30cs1.pdf)

> 在本教程中我们将逐步了解我们如何格式化 DataList 和转发器控件，通过使用在模板中的格式设置函数或通过处理数据绑定事件的外观的示例。


## <a name="introduction"></a>介绍

正如我们看到在前面的教程，DataList 提供多个与样式有关的属性影响它的外观。 具体而言，我们已了解如何将默认 CSS 类分配给 DataList s `HeaderStyle`， `ItemStyle`， `AlternatingItemStyle`，和`SelectedItemStyle`属性。 除了这些四个属性，DataList 包括大量的其他样式相关的属性，如`Font`， `ForeColor`， `BackColor`，和`BorderWidth`，仅举几例。 转发器控件不包含任何与样式有关的属性。 必须直接在转发器的模板中的标记内进行任何此类样式设置。

通常情况下，不过，应如何格式化数据取决于数据本身。 例如，列出产品时我们可能想要在浅灰色字体颜色中显示的产品信息，如果它已停止使用，或者我们可能想要突出显示`UnitsInStock`时为零的值。 我们在前面的教程中看到，说明如何，GridView，FormView 则提供两种不同的方式来设置格式基于其数据的外观的顾客：

- **`DataBound`事件**创建的事件处理程序的相应`DataBound`触发数据已绑定到每个项之后的事件 (它是 gridview`RowDataBound`事件; 对于 DataList 和转发器中，它将是`ItemDataBound`事件)。 这种情况下处理程序中，绑定数据就可以进行检查和格式设置决策产生。 此方法中的，我们探讨[自定义格式设置基于时数据](../custom-formatting/custom-formatting-based-upon-data-cs.md)教程。
- **格式设置在模板中的函数**时的说明或 GridView 控件或 FormView 控件中的模板中使用 TemplateFields，我们可以将格式设置的函数添加到 ASP.NET 页的代码隐藏类、 业务逻辑层，或任何可从 web 应用程序访问其他类库。 此格式设置的函数可以接受任意数目的输入参数，但必须返回模板中呈现的 HTML。 格式设置的函数已在第一次检查[GridView 控件中使用 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)教程。

这两种格式设置的技巧是适用于 DataList 和转发器控件。 在本教程中我们将逐步了解两个控件使用这两种技术的示例。

## <a name="using-theitemdataboundevent-handler"></a>使用`ItemDataBound`事件处理程序

当数据绑定到 DataList，从数据源控件或通过以编程方式将数据分配给控件 s`DataSource`属性和调用其`DataBind()`方法，DataList 的`DataBinding`事件触发时，枚举，数据源并且每个数据记录绑定到 DataList。 为数据源中的每个记录，DataList 创建[ `DataListItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx)对象，该对象，然后绑定到当前记录。 在此过程中，DataList 引发两个事件：

- **`ItemCreated`**之后，将引发`DataListItem`已创建
- **`ItemDataBound`**当前记录已绑定到之后激发`DataListItem`

以下步骤概述 DataList 控件的数据绑定过程。

1. DataList s [ `DataBinding`事件](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx)激发
2. 将数据绑定到 DataList  
  
 为数据源中的每个记录 

    1. 创建`DataListItem`对象
    2. 激发[`ItemCreated`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. 将绑定到记录`DataListItem`
    4. 激发[`ItemDataBound`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. 添加`DataListItem`到`Items`集合

将数据绑定到转发器控件，当执行过程通过完全相同的步骤序列。 唯一的区别是而不是`DataListItem`正在创建的实例，中继器使用[ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s。

> [!NOTE]
> 敏锐读取器可能已注意到之间的经过 DataList 和转发器绑定到数据而不是当 GridView 绑定到数据时的步骤顺序略异常。 在数据绑定过程的结尾结束时，引发 GridView`DataBound`事件; 但是，无论是 DataList 还是转发器控件具有此类事件。 这是因为在复制前和后级别事件处理程序模式已成为公共之前 DataList 和转发器控件已回到中 ASP.NET 1.x 的时间范围内，创建。


格式设置在基于的数据的一个选项是创建的事件处理程序与 GridView，像`ItemDataBound`事件。 此事件处理程序将检查仅具有已绑定到的数据`DataListItem`或`RepeaterItem`而且影响格式化控件，根据需要。

DataList 控件设置格式的更改可以使用实现整个项`DataListItem`s 与样式有关的属性，其中包括标准`Font`， `ForeColor`， `BackColor`， `CssClass`，依次类推。 若要影响 DataList 的模板中的特定 Web 控件的格式设置，我们需要以编程方式访问和修改这些 Web 控件的样式。 我们已了解如何完成此后在*自定义格式设置基于时数据*教程。 与转发器控件中，相似`RepeaterItem`类具有不与样式有关的属性; 因此，所有与样式有关的更改`RepeaterItem`中`ItemDataBound`事件处理程序必须通过以编程方式访问和更新中的 Web 控件模板。

由于`ItemDataBound`DataList 和转发器的几乎相同，我们的示例将着重介绍如何使用 DataList 格式设置方法。

## <a name="step-1-displaying-product-information-in-the-datalist"></a>步骤 1： 在 DataList 中显示产品信息

我们担心的格式设置之前，让 s 将首先创建 DataList 用于显示产品信息的页。 在[以前一教程](displaying-data-with-the-datalist-and-repeater-controls-cs.md)我们创建 DataList 其`ItemTemplate`显示每个产品的名称、 类别、 供应商，每个单元和价格数量。 允许在本教程中重复此功能下面的 s。 若要实现此目的，你也可以重新创建 DataList 和从零开始，其 ObjectDataSource 或你可以通过这些控件从前面的教程中创建的页复制 (`Basics.aspx`) 和出于本教程将它们粘贴到页 (`Formatting.aspx`)。

一旦你已复制的 DataList 功能和 ObjectDataSource 功能从`Basics.aspx`到`Formatting.aspx`，花些时间更改 DataList s`ID`属性从`DataList1`到更具描述性`ItemDataBoundFormattingExample`。 接下来，在浏览器中查看 DataList。 如图 1 所示，每个产品之间的唯一格式设置区别是，交替的背景色。


[![DataList 控件中列出的产品](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image1.png)

**图 1**: DataList 控件中列出的产品 ([单击以查看实际尺寸的图像](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image3.png))


对于本教程，让我们来格式化 DataList 这样小于 $20.00 价格的任何产品将具有两个其名称，并且单元价格黄色突出显示。

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>步骤 2： 以编程方式确定 ItemDataBound 事件处理程序中的数据的值

由于仅在 $20.00 将价格与这些产品具有应用的自定义格式，因此我们必须能够确定每个产品的价格。 当将数据绑定到 DataList，DataList 枚举其数据源中的记录，并对于每个记录，请创建`DataListItem`实例，绑定到数据源记录`DataListItem`。 特定记录 s 数据已绑定到当前`DataListItem`对象，DataList 的`ItemDataBound`激发事件。 我们可以创建的事件处理程序此事件来检查当前的数据值`DataListItem`和基于这些值，进行必要的任何格式设置的更改。

创建`ItemDataBound`DataList 事件并添加以下代码：


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample1.cs)]

尽管的概念和语义后面 DataList s`ItemDataBound`事件处理程序都与使用的 GridView s 相同`RowDataBound`中的事件处理程序*自定义格式设置基于时数据*教程中，语法不同稍有。 当`ItemDataBound`事件激发`DataListItem`只绑定到数据传递到相应事件处理程序通过`e.Item`(而不是`e.Row`，如同处理 GridView 的`RowDataBound`事件处理程序)。 DataList s`ItemDataBound`个事件处理程序都会激发*每个*添加到 DataList，包括标题行、 页脚行和分隔符行的行。 但是，产品信息仅绑定到的数据行中。 因此，当使用`ItemDataBound`事件来检查的数据绑定到 DataList，我们需要首先确保我们重新使用的数据项。 这可以通过检查来实现`DataListItem`s [ `ItemType`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx)，它可以具有之一[以下八个值](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

同时`Item`和`AlternatingItem``DataListItem`的构成 DataList 的数据项。 假设我们重新使用`Item`或`AlternatingItem`，我们访问实际`ProductsRow`已绑定到当前的实例`DataListItem`。 `DataListItem` S [ `DataItem`属性](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx)包含对引用`DataRowView`对象，其`Row`属性提供对实际的引用`ProductsRow`对象。

接下来，我们检查`ProductsRow`实例的`UnitPrice`属性。 由于产品表 s`UnitPrice`字段允许`NULL`值，然后再尝试访问`UnitPrice`属性我们应首先检查以查看它是否具有`NULL`值使用`IsUnitPriceNull()`方法。 如果`UnitPrice`值不是`NULL`，我们然后，检查以查看是否它小于 $20.00 s。 如果它确实下为 $20.00，我们则需要应用自定义格式设置。

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>步骤 3： 突出显示的产品的名称和价格

一旦我们知道产品的价格为小于 $20.00，所有就是突出显示其名称和价格。 若要实现此目的，我们必须先以编程方式引用中的标签控件`ItemTemplate`，显示的产品的名称和价格。 接下来，我们需要将其显示黄色背景。 可以通过直接修改标签应用此格式设置信息`BackColor`属性 (`LabelID.BackColor = Color.Yellow`); 理想情况下，不过，应通过级联样式表中表示所有显示相关的问题。 实际上，我们已经有提供所需的格式中定义的样式表`Styles.css`  -  `AffordablePriceEmphasis`，其创建和中所述*自定义格式设置基于时数据*教程。

若要应用该格式，只需设置两个标签 Web 控件`CssClass`属性设置为`AffordablePriceEmphasis`，如下面的代码中所示：


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample2.cs)]

与`ItemDataBound`事件处理程序完成后，重新访问`Formatting.aspx`在浏览器中的页。 如图 2 所示，且在 $20.00 价格这些产品将具有其名称和突出显示的价格。


[![这些产品小于 $20.00 突出显示](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image4.png)

**图 2**： 那些产品小于 $20.00 突出显示 ([单击以查看实际尺寸的图像](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image6.png))


> [!NOTE]
> 由于 DataList 呈现为 HTML `<table>`，将其`DataListItem`实例具有与样式有关的属性，可设置为特定样式应用到整个项。 例如，如果我们想要突出显示*整个*项在其价格不小于 $20.00 黄色，我们无法具有替代引用标签的代码并设置其`CssClass`具有以下代码行的属性： `e.Item.CssClass = "AffordablePriceEmphasis"`（请参见图 3）。


`RepeaterItem`构成转发器控件，但是，don t s 提供此类样式级别属性。 因此，应用自定义格式设置为转发器需要在转发器的模板中的 Web 控件的样式属性的应用程序就像我们未在图 2 中一样。


[![有关产品在 $20.00 突出显示整个产品项](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image7.png)

**图 3**: 整个产品项被标记为产品下 $20.00 ([单击以查看实际尺寸的图像](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>使用从模板中的格式设置函数

在*GridView 控件中使用 TemplateFields*教程，我们已了解如何使用格式设置在函数内 GridView TemplateField 应用自定义格式取决于的数据绑定到 GridView 的行。 格式设置的函数是可以从模板调用并返回 HTML 中以在其位置中发出的方法。 格式设置的函数可以驻留在 ASP.NET 页的代码隐藏类或到中的类文件可以集中式`App_Code`文件夹或在单独的类库项目。 移动从 ASP.NET 页的代码隐藏类格式设置的函数是理想的如果你打算在其他 ASP.NET web 应用程序或多个 ASP.NET 页中使用相同的格式设置功能。

为了演示格式设置函数，让 s 的产品的名称旁边包含文本 [已中断]，如果产品信息它停止使用的 s。 此外，让 s 具有价格突出显示黄色如果它 s 小于 $20.00 (正如我们做`ItemDataBound`事件处理程序示例); 如果价格为 $20.00 或更高版本，但仍使 s 不显示实际的价格，但文本，请改为调用价格引用。 图 4 显示列出这些应用的格式设置规则的产品的屏幕截图。


[![对于昂贵的产品，价格替换的文本，请调用价格引用](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image10.png)

**图 4**： 对于昂贵的产品，价格替换的文本，请调用针对一个价格的报价 ([单击以查看实际尺寸的图像](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image12.png))


## <a name="step-1-create-the-formatting-functions"></a>步骤 1： 创建格式设置的函数

有关格式设置的两个函数、 一个显示产品名称以及文本 [已中断]，如果需要而另一个，我们需要此示例，显示突出显示的价格，如果它 s 小于 $20.00，还是的文本，否则为报价的请调用。 让我们来创建这些函数在 ASP.NET 页的代码隐藏类并将它们命名`DisplayProductNameAndDiscontinuedStatus`和`DisplayPrice`。 这两种方法需要返回 HTML 中以将呈现为一个字符串，同时需要标记`Protected`(或`Public`) 才能从 ASP.NET 页 s 声明性语法部分中进行调用。 这两种方法的代码如下所示：


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample3.cs)]

请注意，`DisplayProductNameAndDiscontinuedStatus`方法接受的值`productName`和`discontinued`而将数据字段为标量值、`DisplayPrice`方法接受`ProductsRow`实例 (而不是`unitPrice`标量值)。 将使用这两种方法; 如果但是，如果将格式设置的函数使用可以包含数据库的标量值`NULL`值 (如`UnitPrice`; 既不`ProductName`也不`Discontinued`允许`NULL`值)，格外小心，必须在处理这些标量输入。

具体而言，输入的参数必须为类型`Object`因为传入的值可能`DBNull`而不是预期的数据类型的实例。 此外，必须进行检查以确定传入的值是否为数据库`NULL`值。 也就是说，如果我们想`DisplayPrice`方法，以接受价格为标量值，d 我们必须使用下面的代码：


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample4.cs)]

请注意，`unitPrice`输入的参数属于类型`Object`且条件语句进行了修改，以确定如果`unitPrice`是`DBNull`与否。 此外，由于`unitPrice`作为输入的参数传递中`Object`，必须强制转换为十进制值。

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>步骤 2： 从 DataList 的 ItemTemplate 调用格式设置的函数

使用格式设置的函数添加到 ASP.NET 页的代码隐藏类，剩下的就是调用这些格式函数从 DataList 的`ItemTemplate`。 若要从模板调用的格式设置函数，将数据绑定语法中的函数调用：


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample5.aspx)]

在 DataList s `ItemTemplate` `ProductNameLabel`标签 Web 控件当前显示的产品的名称通过分配其`Text`属性结果的`<%# Eval("ProductName") %>`。 为了让其显示名称和文本 [已中断]，如果需要更新的声明性语法，以便它改为将分配`Text`属性值的`DisplayProductNameAndDiscontinuedStatus`方法。 时这样做，我们必须传递中的产品的名称和不再使用的值使用`Eval("columnName")`语法。 `Eval`返回类型的值`Object`，但`DisplayProductNameAndDiscontinuedStatus`方法需要输入的参数的类型`String`和`Boolean`; 因此，我们必须强制转换返回的值`Eval`到预期的输入的参数类型，方法如下所示：


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample6.aspx)]

若要显示的价格，我们只需可以设置`UnitPriceLabel`标签 s`Text`属性返回的值`DisplayPrice`方法，就像我们未用于显示产品的名称和 [停用] 文本。 但是，而不是传入`UnitPrice`作为标量的输入参数，我们改为传入整个`ProductsRow`实例：


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample7.aspx)]

与对就地格式设置的函数调用中，需要花费一段时间来在浏览器中查看我们的进度。 你的屏幕应类似于图 5： 使用停产的产品包括文本 [已中断]，多个 $20.00 具有其价格的成本计算这些产品替换文本请价格引号的调用。


[![对于昂贵的产品，价格替换的文本，请调用价格引用](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image13.png)

**图 5**： 对于昂贵的产品，价格替换的文本，请调用针对一个价格的报价 ([单击以查看实际尺寸的图像](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image15.png))


## <a name="summary"></a>摘要

可以使用两项技术实现格式设置取决于数据 DataList 或转发器控件的内容。 第一种方法是创建的事件处理程序`ItemDataBound`触发的事件，如数据源中的每个记录绑定到一个新`DataListItem`或`RepeaterItem`。 在`ItemDataBound`事件处理程序，可以检查当前的 s 项数据，然后格式设置可应用于的内容的模板，或者，对于`DataListItem`s，到整个项本身。

或者，完成格式化函数实现自定义格式设置。 格式设置函数是可以从 DataList 调用的方法或转发器的模板返回 HTML 中以在其位置中发出。 通常情况下，由绑定到当前项的值确定格式设置的函数返回的 HTML。 这些值可以传递到格式设置的函数、，用作的标量值或通过传入绑定到项的整个对象 (如`ProductsRow`实例)。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Yaakov Ellis、 徐 Schmidt 和沈 Shulok。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](displaying-data-with-the-datalist-and-repeater-controls-cs.md)
[下一页](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
