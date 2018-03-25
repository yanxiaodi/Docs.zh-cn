---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
title: "自定义格式设置取决于数据 (C#) |Microsoft 文档"
author: rick-anderson
description: "调整 GridView，说明如何或 FormView 取决于绑定到它的数据的格式可以采用多种方式来完成。 在本教程中我们将 l..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 871a4574-f89c-4214-b786-79253ed3653b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 606721b01fae34a7bce85d497a442cb110f1b51e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="custom-formatting-based-upon-data-c"></a>自定义格式设置取决于数据 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_11_CS.exe)或[下载 PDF](custom-formatting-based-upon-data-cs/_static/datatutorial11cs1.pdf)

> 调整 GridView，说明如何或 FormView 取决于绑定到它的数据的格式可以采用多种方式来完成。 在本教程中我们将查看如何完成数据绑定通过数据绑定和 RowDataBound 事件处理程序使用格式设置。


## <a name="introduction"></a>介绍

通过提供多种样式相关的属性，可以自定义 GridView、 说明如何和 FormView 控件的外观。 属性，例如`CssClass`， `Font`， `BorderWidth`， `BorderStyle`， `BorderColor`， `Width`，和`Height`，及其他规定呈现的控件的总体外观。 属性包括`HeaderStyle`， `RowStyle`， `AlternatingRowStyle`，和其他允许这些相同的样式设置应用于特定部分。 同样，可以在字段级别应用这些样式设置。

在许多情况下通过，格式设置的要求取决于显示的数据的值。 例如，若要绘制的关注，以外常用产品，列出产品信息的报表可能将背景颜色设置为黄色为这些产品其`UnitsInStock`和`UnitsOnOrder`字段都等于 0。 要突出显示的开销更大的产品，我们可能想要显示的多个用粗体 $75.00 的成本计算这些产品价格。

调整 GridView，说明如何或 FormView 取决于绑定到它的数据的格式可以采用多种方式来完成。 在本教程中我们将考察如何完成数据绑定的格式设置使用`DataBound`和`RowDataBound`事件处理程序。 在下一步的教程，我们将探讨另一种方法。

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>使用说明如何控制`DataBound`事件处理程序

当数据绑定到说明如何从数据源控件或通过以编程方式将数据分配给控件的`DataSource`属性和调用其`DataBind()`方法，下面的步骤顺序发生：

1. 数据的 Web 控件`DataBinding`事件激发。
2. 数据绑定到 Web 控件的数据。
3. 数据的 Web 控件`DataBound`事件激发。

可通过一个事件处理程序的步骤 1 和 3 后立即插入自定义逻辑。 通过创建的事件处理程序`DataBound`我们就能以编程方式确定已被数据绑定到数据 Web 控件，并调整格式根据需要的事件。 为了说明这一点让我们创建说明如何将列出有关产品的常规信息，但将显示`UnitPrice`中的值***加粗、 倾斜字体***如果它超过 $75.00。

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>步骤 1： 在说明中显示的产品信息

打开`CustomColors.aspx`页面`CustomFormatting`文件夹中，从工具箱中拖动到设计器中将说明如何控件、 设置其`ID`属性值设置为`ExpensiveProductsPriceInBoldItalic`，并将其绑定到一个新的 ObjectDataSource 控件调用`ProductsBLL`类的`GetProducts()`方法。 实现此操作的详细的步骤为简洁起见我们检查它们在详细信息在前面的教程中由于此处省略。

一旦你已绑定到说明的对象数据源，需要一段时间来修改的字段列表。 我已选择删除`ProductID`， `SupplierID`， `CategoryID`， `UnitsInStock`， `UnitsOnOrder`， `ReorderLevel`，和`Discontinued`BoundFields 和重命名并重新格式化剩余 BoundFields。 我也被清除`Width`和`Height`设置。 由于说明如何显示单个记录，我们需要启用分页，以便允许最终用户若要查看所有产品。 执行此操作通过检查中说明的智能标记的启用分页复选框。


[![检查中说明的智能标记的启用分页复选框](custom-formatting-based-upon-data-cs/_static/image2.png)](custom-formatting-based-upon-data-cs/_static/image1.png)

**图 1**： 检查启用分页中的复选框说明的智能标记 ([单击以查看实际尺寸的图像](custom-formatting-based-upon-data-cs/_static/image3.png))


这些更改后将来说明如何标记：


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample1.aspx)]

需要一段时间来测试你的浏览器中的此页。


[![说明如何控制一次显示一个产品](custom-formatting-based-upon-data-cs/_static/image5.png)](custom-formatting-based-upon-data-cs/_static/image4.png)

**图 2**: 说明如何控制显示一个产品一次 ([单击以查看实际尺寸的图像](custom-formatting-based-upon-data-cs/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>步骤 2： 以编程方式确定数据绑定事件处理程序中的数据的值

若要为这些产品用粗体、 斜体字体显示价格其`UnitPrice`值超过 $75.00，我们需要首先能够以编程方式确定`UnitPrice`值。 对于说明，这可以完成`DataBound`事件处理程序。 若要创建事件处理程序说明如何在设计器中单击，然后导航到属性窗口。 按 F4 以显示它，如果不可见，或转到视图菜单并选择属性窗口菜单选项。 从属性窗口中，单击闪电形图标，若要列出说明的事件。 接下来，双击`DataBound`事件或输入你想要创建的事件处理程序的名称。


![为数据绑定事件创建事件处理程序](custom-formatting-based-upon-data-cs/_static/image7.png)

**图 3**： 创建的事件处理程序`DataBound`事件


这样将自动创建事件处理程序，并将您带到的代码部分添加的位置。 此时你将看到：


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample2.cs)]

可以通过访问数据绑定到说明如何`DataItem`属性。 回想一下，我们将我们控件绑定到强类型 DataTable，组成的强类型 DataRow 实例的集合。 DataTable 绑定到说明如何，数据表中的第一个 DataRow 会分配给说明的`DataItem`属性。 具体而言，`DataItem`属性分配`DataRowView`对象。 我们可以使用`DataRowView`的`Row`属性以获取访问权限的基础的 DataRow 对象，这是实际`ProductsRow`实例。 一旦我们有这`ProductsRow`我们可以通过简单地检查对象的属性值，让我们决策的实例。

下面的代码演示如何确定是否`UnitPrice`绑定到说明控件的值是否大于 $75.00:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample3.cs)]

> [!NOTE]
> 由于`UnitPrice`可以`NULL`值在数据库中，我们首先检查以确保我们正在不处理与`NULL`前访问值`ProductsRow`的`UnitPrice`属性。 此检查是重要因为如果我们尝试访问`UnitPrice`属性时它具有`NULL`值`ProductsRow`对象将引发[StrongTypingException 异常](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx)。


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>步骤 3： 格式设置说明中的单价值

此时我们可以确定是否`UnitPrice`值绑定到说明具有值超过 $75.00，但我们尚未若要了解如何以编程方式调整说明的格式设置相应地。 若要修改的说明中的整个行格式设置，以编程方式访问行使用`DetailsViewID.Rows[index]`; 若要修改特定的单元格，访问，请使用`DetailsViewID.Rows[index].Cells[index]`。 一旦我们有了对我们然后可以通过设置其样式相关的属性来调整其外观的单元格的行的引用。

以编程方式访问行需要知道的行的索引，从 0 开始的。 `UnitPrice`行是说明如何，并向其提供的 4 索引并使其以编程方式访问中的第五个一行使用`ExpensiveProductsPriceInBoldItalic.Rows[4]`。 此时，我们可以通过使用下面的代码显示在加粗、 倾斜字体中的整个行的内容：


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample4.cs)]

但是，这将使*同时*标签 （价格） 和加粗和倾斜的值。 如果我们想要使只是价值加粗和倾斜我们需要将此应用到的第二个单元格的行，可以使用以下格式：


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample5.cs)]

由于我们的教程到目前为止已使用样式表来维护与样式有关的信息呈现的标记之间完全分离，而不是设置的特定样式属性，如上所示让我们改为使用 CSS 类。 打开`Styles.css`样式表并添加一个名为的新的 CSS 类`ExpensivePriceEmphasis`替换为以下定义：


[!code-css[Main](custom-formatting-based-upon-data-cs/samples/sample6.css)]

然后，在`DataBound`事件处理程序设置的单元格的`CssClass`属性`ExpensivePriceEmphasis`。 下面的代码演示`DataBound`其完整的事件处理程序：


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample7.cs)]

查看牛奶，成本小于 $75.00，价格将显示在普通字体 （请参见图 4）。 但是，当查看 Mishi Kobe Niku，具有指价格为 $97.00，价格会显示在加粗、 倾斜字体 （请参见图 5）。


[![价格低于 $75.00 将显示在普通字体](custom-formatting-based-upon-data-cs/_static/image9.png)](custom-formatting-based-upon-data-cs/_static/image8.png)

**图 4**： 价格低于 $75.00 将显示在普通字体 ([单击以查看实际尺寸的图像](custom-formatting-based-upon-data-cs/_static/image10.png))


[![昂贵产品的价格显示粗体、 斜体字体](custom-formatting-based-upon-data-cs/_static/image12.png)](custom-formatting-based-upon-data-cs/_static/image11.png)

**图 5**： 昂贵产品的价格显示粗体、 斜体字体 ([单击以查看实际尺寸的图像](custom-formatting-based-upon-data-cs/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>使用 FormView 控件的`DataBound`事件处理程序

确定基础数据绑定到 FormView 的步骤是相同的说明如何创建`DataBound`事件处理程序，强制转换`DataItem`到合适的对象类型的属性绑定到控件，并确定如何继续执行。 FormView 和说明不同，但是，在如何更新自己的用户界面的外观。

FormView 不包含任何 BoundFields 并因此缺少`Rows`集合。 相反，FormView 组成模板，其中可以包含多种静态 HTML，Web 控件，以及数据绑定语法。 调整 FormView 的样式通常涉及调整一个或多个 FormView 的模板中的 Web 控件的样式。

若要说明这一点，让我们为产品列表 FormView 中前面的示例中，但这次我们喜欢的使用显示只需的产品名称和单位使用以红色字体显示，如果它小于或等于 10 的库存量单位的库存量。

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>步骤 4： 在 FormView 中显示的产品信息

添加到 FormView`CustomColors.aspx`页下的说明和设置其`ID`属性`LowStockedProductsInRed`。 将 FormView 绑定到上一步中创建的 ObjectDataSource 控件。 这将创建`ItemTemplate`， `EditItemTemplate`，和`InsertItemTemplate`FormView 有关。 删除`EditItemTemplate`和`InsertItemTemplate`并简化`ItemTemplate`包括只需`ProductName`和`UnitsInStock`值，每个自己带有相应名称的标签控件。 与从之前的示例说明如何，还检查 FormView 的智能标记中的启用分页复选框。

后这些编辑你 FormView 标记应看起来类似于以下：


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample8.aspx)]

请注意，`ItemTemplate`包含：

- **静态 HTML**文本"产品:"和"库存量:"连同`<br />`和`<b>`元素。
- **Web 控件**两个标签控件，`ProductNameLabel`和`UnitsInStockLabel`。
- **数据绑定语法**`<%# Bind("ProductName") %>`和`<%# Bind("UnitsInStock") %>`语法，将从这些字段的值分配给的标签控件的`Text`属性。

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>步骤 5： 以编程方式确定数据绑定事件处理程序中的数据的值

与完整的 FormView 的标记下, 一步是以编程方式确定如果`UnitsInStock`值是否小于或等于 10。 可以做到这一点与 FormView 完全相同的方式按照原样使用说明。 首先为 FormView 创建事件处理程序`DataBound`事件。


![创建数据绑定事件处理程序](custom-formatting-based-upon-data-cs/_static/image14.png)

**图 6**： 创建`DataBound`事件处理程序


在事件处理程序强制转换 FormView`DataItem`属性`ProductsRow`实例，并确定是否`UnitsInPrice`值是，我们需要用红色字体显示。


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample9.cs)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>步骤 6： 格式设置在 FormView ItemTemplate UnitsInStockLabel 标签控件

最后一步是要设置格式显示`UnitsInStock`值以红色字体，如果值为 10 或更少。 若要完成此我们需要以编程方式访问`UnitsInStockLabel`中控制`ItemTemplate`并设置其样式属性，以便其文本显示为红色。 若要访问 Web 控件模板中的，使用`FindControl("controlID")`方法如下：


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample10.cs)]

对于我们的示例中我们想要访问一个标签来控制其`ID`值是`UnitsInStockLabel`，因此我们将使用：


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample11.cs)]

一旦我们有了对 Web 控件的编程引用，我们可以根据需要修改其与样式有关的属性。 如前面的示例中，我已创建中的 CSS 类`Styles.css`名为`LowUnitsInStockEmphasis`。 若要将此样式应用到标签 Web 控件，将设置其`CssClass`属性相应地。


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample12.cs)]

> [!NOTE]
> 格式设置以编程方式访问 Web 控件使用的模板的语法`FindControl("controlID")`，然后设置其样式相关的属性还可使用时[TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx)中的说明或 GridView控件。 在我们的下一步教程，我们将检查 TemplateFields。


图 7 显示 FormView 查看产品时其`UnitsInStock`图 8 中的产品获得它的值小于 10 时，值是大于 10。


[![对于产品与足够大 Units In Stock，无自定义格式设置将应用](custom-formatting-based-upon-data-cs/_static/image16.png)](custom-formatting-based-upon-data-cs/_static/image15.png)

**图 7**： 应用的产品与足够大 Units In Stock、 无自定义格式设置 ([单击以查看实际尺寸的图像](custom-formatting-based-upon-data-cs/_static/image17.png))


[![在库存数量的单位以红色显示的那些产品包含值 10 或更少的](custom-formatting-based-upon-data-cs/_static/image19.png)](custom-formatting-based-upon-data-cs/_static/image18.png)

**图 8**： 内库存数量的设备以红色显示的那些产品使用的值小于或等于 10 ([单击以查看实际尺寸的图像](custom-formatting-based-upon-data-cs/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>使用 GridView 的格式设置`RowDataBound`事件

前面我们检查的步骤说明如何顺序并 FormView 在数据绑定期间控制通过的进度。 让我们看通过这些步骤再一次作为刷新程序。

1. 数据的 Web 控件`DataBinding`事件激发。
2. 数据绑定到 Web 控件的数据。
3. 数据的 Web 控件`DataBound`事件激发。

这三个简单步骤的说明和 FormView 足以，因为它们将显示单个记录。 Gridview，它会显示*所有*记录绑定到它 （而不仅仅是第一个），步骤 2 非常有些复杂。

在步骤的 2 GridView 枚举数据源，并为每个记录，创建`GridViewRow`实例并将当前记录绑定到它。 每个`GridViewRow`添加到 GridView，会引发两个事件：

- **`RowCreated`**之后，将引发`GridViewRow`已创建
- **`RowDataBound`**当前记录已绑定到之后激发`GridViewRow`。

为 GridView，然后，数据绑定是更准确地由以下序列描述的步骤：

1. GridView`DataBinding`事件激发。
2. 将数据绑定到 GridView。   
  
 为数据源中的每个记录 

    1. 创建`GridViewRow`对象
    2. 激发`RowCreated`事件
    3. 将绑定到记录`GridViewRow`
    4. 激发`RowDataBound`事件
    5. 添加`GridViewRow`到`Rows`集合
3. GridView`DataBound`事件激发。

若要自定义的 GridView 的单个记录的格式，然后，我们需要创建的事件处理程序`RowDataBound`事件。 若要说明这一点，让我们将添加到 GridView`CustomColors.aspx`页，其中列出名称、 类别和突出显示其价格是小于用黄色背景色 $10.00 这些产品的每个产品的价格。

## <a name="step-7-displaying-product-information-in-a-gridview"></a>步骤 7： 在一个 GridView 中显示产品信息

添加上一示例中的下 FormView GridView，并设置其`ID`属性`HighlightCheapProducts`。 由于我们已有返回页的所有产品 ObjectDataSource，绑定到的 GridView。 最后，编辑 GridView BoundFields 包括只需产品的名称、 类别和价格。 在这些编辑后 GridView 标记应如下所示：


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample13.aspx)]

图 9 显示我们到目前为止，通过浏览器查看时的进度。


[![GridView 列出名称、 类别和每个产品的价格](custom-formatting-based-upon-data-cs/_static/image22.png)](custom-formatting-based-upon-data-cs/_static/image21.png)

**图 9**: GridView 列出名称、 类别和每个产品的价格 ([单击以查看实际尺寸的图像](custom-formatting-based-upon-data-cs/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>步骤 8： 以编程方式确定 RowDataBound 事件处理程序中的数据的值

当`ProductsDataTable`绑定到 GridView 其`ProductsRow`实例已枚举并为每个`ProductsRow``GridViewRow`创建。 `GridViewRow`的`DataItem`属性分配给特定`ProductRow`，此后 GridView`RowDataBound`事件处理程序引发。 若要确定`UnitPrice`为每个产品的值绑定到 GridView，然后，我们需要创建一个事件处理程序为 GridView`RowDataBound`事件。 此事件处理程序中，我们可以检查`UnitPrice`当前值`GridViewRow`并且格式设置为该行决定。

可以使用相同的一系列步骤作为不带 FormView 和说明如何创建此事件处理程序。


![GridView RowDataBound 事件创建事件处理程序](custom-formatting-based-upon-data-cs/_static/image24.png)

**图 10**： 为 GridView 创建事件处理程序`RowDataBound`事件


在这种方式中创建的事件处理程序将导致下面的代码要自动添加到 ASP.NET 页的代码部分：


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample14.cs)]

当`RowDataBound`事件激发事件处理程序作为其第二个参数传递类型的对象`GridViewRowEventArgs`，其中有一个名为`Row`。 此属性返回的引用`GridViewRow`已只是数据绑定。 访问`ProductsRow`实例绑定到`GridViewRow`我们使用`DataItem`属性如下所示：


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample15.cs)]

使用时`RowDataBound`务必要记住 GridView 组成的行的不同类型，此事件针对激发的事件处理程序*所有*行类型。 A`GridViewRow`的类型可以由其`RowType`属性，并且可以具有可能的值之一：

- `DataRow`从 GridView 的绑定到一个记录行`DataSource`
- `EmptyDataRow`如果显示的行 GridView`DataSource`为空
- `Footer`页脚行中;显示的如果 GridView`ShowFooter`属性设置为`true`
- `Header`标头行中;显示是否 GridView ShowHeader 属性设置为`true`（默认值）
- `Pager`GridView 的中，来实现分页，将显示分页接口的行
- `Separator`不用于 GridView，但使用`RowType`DataList 和转发器的属性控制，两个数据，我们将讨论在将来教程的 Web 控件

由于`EmptyDataRow`， `Header`， `Footer`，和`Pager`行不与关联`DataSource`记录，它们将始终具有`null`值，则为其`DataItem`属性。 出于此原因，然后再尝试处理当前`GridViewRow`的`DataItem`属性，我们首先必须确保我们正在处理与`DataRow`。 这可以通过检查来实现`GridViewRow`的`RowType`属性如下所示：


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample16.cs)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>步骤 9： 突出显示行黄色时 UnitPrice 值是小于 $10.00

最后一步是以编程方式突出显示整个`GridViewRow`如果`UnitPrice`值该行是小于 $10.00。 访问一个 GridView 行或单元格的语法是说明如何与相同`GridViewID.Rows[index]`访问整行，`GridViewID.Rows[index].Cells[index]`访问特定的单元格。 但是，当`RowDataBound`事件处理程序，将引发绑定数据`GridViewRow`中尚未添加到的 GridView`Rows`集合。 因此无法访问当前`GridViewRow`实例从`RowDataBound`事件处理程序使用的行集合。

而不是`GridViewID.Rows[index]`，我们可以引用当前`GridViewRow`实例中`RowDataBound`事件处理程序使用`e.Row`。 即，按顺序，以突出显示当前`GridViewRow`实例从`RowDataBound`我们将使用的事件处理程序：


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample17.cs)]

而不是设置`GridViewRow`的`BackColor`属性直接，让我们只讨论与使用 CSS 类。 我已创建一个名为的 CSS 类`AffordablePriceEmphasis`，将背景色设置为黄色。 已完成`RowDataBound`事件处理程序遵循：


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample18.cs)]


[![最经济的产品是黄色突出显示](custom-formatting-based-upon-data-cs/_static/image26.png)](custom-formatting-based-upon-data-cs/_static/image25.png)

**图 11**： 最经济的产品是突出显示黄色 ([单击以查看实际尺寸的图像](custom-formatting-based-upon-data-cs/_static/image27.png))


## <a name="summary"></a>摘要

在本教程中我们已了解如何设置 GridView，说明如何和 FormView 在基于绑定到控件的数据的格式。 若要完成此我们创建的事件处理程序`DataBound`或`RowDataBound`事件，如果需要格式设置的更改，以及检查基础数据已其中。 若要访问的数据绑定到一个说明如何或 FormView，我们使用`DataItem`中的属性`DataBound`事件处理程序; 对于一个 GridView，每个`GridViewRow`实例的`DataItem`属性包含绑定到该行，即在中可用的数据`RowDataBound`事件处理程序。

以编程方式调整数据 Web 控件的格式设置的语法取决于 Web 控件和要设置格式的数据的显示方式。 说明如何和 GridView 控件、 行和单元格可以访问由的序号索引。 有关说明，使用模板，`FindControl("controlID")`方法通常用于查找 Web 控件从模板中的。

在下一教程中我们将查看如何使用 GridView 和说明如何使用模板。 此外，我们将看到另一种自定义基于基础数据的格式设置的方法。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 E.R. Gilmore，Dennis Patterson 和 Dan Jagers。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[下一篇](using-templatefields-in-the-gridview-control-cs.md)
