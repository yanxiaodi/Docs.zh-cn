---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
title: "检查事件与插入、 更新和删除 (VB) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将检查使用发生之前、 之中以及之后插入的事件，更新或删除操作的 Web 控件的 ASP.NET 数据。 W..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: c9bd10a7-eff8-4d8c-bec9-963c2aef2d6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 88f6beb3f3514c6a9784d4cb936a5b779ce75ae1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-vb"></a>检查与插入、 更新和删除 (VB) 关联的事件
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_VB.exe)或[下载 PDF](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/datatutorial17vb1.pdf)

> 在本教程中我们将检查使用发生之前、 之中以及之后插入的事件，更新或删除操作的 Web 控件的 ASP.NET 数据。 我们将了解如何自定义编辑界面只更新产品字段的子集。


## <a name="introduction"></a>介绍

在使用内置插入、 编辑或删除功能的 GridView，说明如何或 FormView 控件时，各种步骤经过最终用户完成添加新记录或更新或删除现有记录的过程。 如我们所述[以前一教程](an-overview-of-inserting-updating-and-deleting-data-vb.md)，当在编辑按钮替换为更新和取消按钮并在文本框中 BoundFields 打开 GridView 中编辑行时。 最终用户更新的数据，并单击更新后，在回发时执行以下步骤：

1. GridView 填充其 ObjectDataSource`UpdateParameters`与已编辑的记录的唯一标识字段的记录 (通过`DataKeyNames`属性) 以及用户输入的值
2. GridView 调用其 ObjectDataSource`Update()`方法，反过来调用基础对象中的相应方法 (`ProductsDAL.UpdateProduct`，我们以前一教程中)
3. 基础数据，现在包含已更新的更改，是重新绑定到 GridView

在这一序列的步骤，过程的数目的事件激发，使我们能够创建事件处理程序来添加自定义逻辑在需要时。 例如，在步骤 1，GridView 之前`RowUpdating`事件激发。 如果没有某些验证错误，我们此时，可以取消更新请求。 当`Update()`调用方法时，ObjectDataSource`Updating`事件激发，提供机会添加或自定义的任一值`UpdateParameters`。 对象的方法的基本 ObjectDataSource 后已完成执行，ObjectDataSource`Updated`引发事件。 事件处理程序`Updated`事件可以查看有关更新操作，例如多少行受到影响，以及是否发生了异常详细信息。 最后，在步骤 2，GridView 后`RowUpdated`事件激发; 检查执行只需更新操作的其他信息的事件处理程序会此事件。

图 1 所示更新 GridView 时这一系列的事件和步骤。 图 1 中的事件模式不是唯一的 GridView 使用更新的。 插入、 更新或删除数据从 GridView，说明如何或 FormView precipitates 相同的数据的 Web 控件和对象数据源的复制前和后级别事件序列。


[![A 系列的预和后期事件激发时在 GridView 中更新数据](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image1.png)

**图 1**: A 系列的复制前和后事件激发时更新中的数据 GridView ([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image3.png))


在本教程中我们将检查使用这些事件来扩展内置的插入、 更新和删除功能的 ASP.NET 数据 Web 控件。 我们将了解如何自定义编辑界面只更新产品字段的子集。

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>步骤 1： 更新产品的`ProductName`和`UnitPrice`字段

在从以前的教程，编辑接口*所有*产品不是只读的具有要包含的字段。 如果要 GridView-从中删除字段说`QuantityPerUnit`-当更新数据 Web 控件的数据不会设置 ObjectDataSource `QuantityPerUnit` `UpdateParameters`值。 中的一个值然后传递 ObjectDataSource`Nothing`到`UpdateProduct`业务逻辑层 (BLL) 方法，更改编辑的数据库记录`QuantityPerUnit`列`NULL`值。 同样，如果必填字段，如`ProductName`，删除在编辑界面上，更新将失败并"*列 ProductName 不允许使用 null*"异常。 此行为的原因是因为配置 ObjectDataSource 调用`ProductsBLL`类的`UpdateProduct`方法，应为每个产品字段输入的参数。 因此，ObjectDataSource 的`UpdateParameters`集合包含一个参数，为每个方法的输入参数。

如果我们想要提供数据 Web 控件，最终用户可以仅更新字段的子集，则我们需要以编程方式设置缺失`UpdateParameters`ObjectDataSource 的中值`Updating`事件处理程序或创建和调用 BLL 方法需要字段的一个子集。 让我们了解此后一种方法。

具体而言，让我们创建显示的页面仅`ProductName`和`UnitPrice`中可编辑的 GridView 的字段。 此 GridView 编辑界面将只允许要更新两个显示的字段的用户`ProductName`和`UnitPrice`。 由于此编辑接口仅提供产品的字段的子集，我们需要创建使用现有 BLL ObjectDataSource`UpdateProduct`方法和具有缺少产品字段值设置以编程方式在其`Updating`事件处理程序，或者我们需要创建一个新的 BLL 方法需要仅在 GridView 中定义的字段的子集。 对于本教程，让我们使用后一个选项，创建的重载`UpdateProduct`方法，只需三个输入参数采用的一个： `productName`， `unitPrice`，和`productID`:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample1.vb)]

如原始`UpdateProduct`方法，此重载会启动通过检查具有指定数据库中是否存在产品`ProductID`。 如果不是，它返回`False`，即指示该更新的产品信息的请求失败。 否则它会更新现有产品记录的`ProductName`和`UnitPrice`相应字段，并通过调用 TableAdpater 的提交更新`Update()`方法，同时传入`ProductsRow`实例。

添加到此元素我们`ProductsBLL`类，我们已准备好创建简化的 GridView 接口。 打开`DataModificationEvents.aspx`中`EditInsertDelete`文件夹和向页面添加一个 GridView。 创建新对象数据源并将其配置为使用`ProductsBLL`类，该类具有其`Select()`方法映射到`GetProducts`及其`Update()`方法映射到`UpdateProduct`重载只使用在`productName`， `unitPrice`，和`productID`输入参数。 图 2 显示了创建数据源向导，映射 ObjectDataSource 时`Update()`方法`ProductsBLL`类的新`UpdateProduct`方法重载。


[![将对象数据源的 update （） 方法映射到新的 UpdateProduct 重载](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image4.png)

**图 2**： 映射 ObjectDataSource`Update()`方法新建`UpdateProduct`重载 ([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image6.png))


由于我们的示例最初只需要编辑数据，但不是能插入或删除记录的功能，花一些时间，则显式指示 ObjectDataSource`Insert()`和`Delete()`方法不应映射到任何`ProductsBLL`通过转到插入和删除选项卡并从下拉列表中选择 （无） 的类的方法。


[![从的插入和删除选项卡的下拉列表中选择 （无）](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image7.png)

**图 3**： 选择 （无） 从下拉列表中对插入和删除选项卡 ([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image9.png))


完成此向导后检查从 GridView 的智能标记启用编辑复选框。

创建数据源向导和绑定到 GridView 完成时，Visual Studio 创建了两个控件的声明性语法。 请转到源视图以检查对象数据源的声明性标记，其如下所示：


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample2.aspx)]

因为不不存在的对象数据源的任何映射`Insert()`和`Delete()`方法，有没有`InsertParameters`或`DeleteParameters`部分。 此外，由于`Update()`方法映射到`UpdateProduct`仅接受三个输入的参数的方法重载`UpdateParameters`部分具有只使用三`Parameter`实例。

请注意，ObjectDataSource`OldValuesParameterFormatString`属性设置为`original_{0}`。 使用配置数据源向导时，此属性将由 Visual Studio 自动进行设置。 但是，由于我们 BLL 方法不需要原始`ProductID`要中传递，从 ObjectDataSource 的声明性语法中完全删除此属性分配值。

> [!NOTE]
> 如果你只需清除`OldValuesParameterFormatString`于在设计视图中，该属性的属性窗口的属性值将仍然存在于声明性语法，但将设置为一个空字符串。 删除属性完全声明性语法，或从属性窗口中，将值设置为默认情况下， `{0}`。


虽然 ObjectDataSource 只有`UpdateParameters`有关产品的名称、 价格和 ID，Visual Studio 添加了 BoundField 或 CheckBoxField GridView 中用于每个产品的字段。


[![GridView 对于每个产品的字段包含 BoundField 或 CheckBoxField](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image10.png)

**图 4**: GridView 对于每个产品的字段包含 BoundField 或 CheckBoxField ([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image12.png))


当最终用户编辑产品，并单击其更新按钮时，GridView 枚举这些字段不是只读的。 然后将相应的参数的值设置 ObjectDataSource`UpdateParameters`为用户输入的值的集合。 如果没有相应的参数，GridView 将添加到集合的一个。 因此，如果我们 GridView 包含 BoundFields 和 CheckBoxFields 所有产品的字段，ObjectDataSource 将结束调用`UpdateProduct`重载采用在所有这些参数的情况下，ObjectDataSource声明性标记指定 （请参见图 5） 的仅三个输入的参数。 同样，如果没有非只读的某种组合产品字段中都不对应的输入参数 GridView`UpdateProduct`超负荷运转，当尝试进行更新时，将引发异常。


[![GridView 将将参数添加到对象数据源的 UpdateParameters 集合](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image13.png)

**图 5**: GridView 将添加参数以 ObjectDataSource`UpdateParameters`集合 ([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image15.png))


若要确保 ObjectDataSource 调用`UpdateProduct`中只是该产品的名称、 价格和 ID，采用重载我们需要使其可编辑字段限制 GridView 仅`ProductName`和`UnitPrice`。 这可以通过删除其他 BoundFields 和 CheckBoxFields，通过设置这些其他字段的`ReadOnly`属性`True`，或通过两个的某种组合。 对于本教程让我们只需删除之外的所有 GridView 字段`ProductName`和`UnitPrice`BoundFields，此后 GridView 的声明性标记将如下所示：


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample3.aspx)]

即使`UpdateProduct`重载需要三个输入的参数，我们在我们 GridView 中只能有两个 BoundFields。 这是因为`productID`输入的参数是主键值，在传递的值`DataKeyNames`编辑过的行的属性。

我们 GridView，连同`UpdateProduct`重载，允许用户而不丢失任何其他产品字段的编辑仅的名称和产品价格。


[![接口允许编辑只是该产品的名称和价格](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image16.png)

**图 6**： 编辑只是该产品的名称和价格接口允许 ([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image18.png))


> [!NOTE]
> 如前面的教程中所述，是极其重要的 GridView 的视图状态是启用 （默认行为）。 如果设置 GridView s`EnableViewState`属性`false`，运行出现无意中删除或编辑记录的并发用户的风险。 请参阅[警告： 禁用并发问题与 ASP.NET 2.0 GridViews/说明/FormViews 该支持编辑和/或删除和其视图状态](http://scottonwriting.net/sowblog/posts/10054.aspx)有关详细信息。


## <a name="improving-theunitpriceformatting"></a>提高`UnitPrice`格式设置

尽管图 6 工作原理中, 所示的 GridView 示例`UnitPrice`字段的格式不根本，导致缺少任何货币价格显示符号和具有四个小数位。 若要应用一种货币格式不可编辑的行，只需设置`UnitPrice`BoundField 的`DataFormatString`属性`{0:c}`及其`HtmlEncode`属性`False`。


[![相应地设置单价的数据和 HtmlEncode 属性](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image19.png)

**图 7**： 设置`UnitPrice`的`DataFormatString`和`HtmlEncode`相应属性 ([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image21.png))


进行此更改后，不可编辑的行设置价格的格式为货币;编辑过的行，但是，仍会显示的货币符号而无需有四个小数位数的值。


[![非可编辑的行是现进行格式设置为货币值](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image22.png)

**图 8**: 非可编辑的行是现在格式化为货币值 ([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image24.png))


中指定的格式设置说明`DataFormatString`属性可以通过设置 BoundField 应用于编辑接口`ApplyFormatInEditMode`属性`True`(默认值是`False`)。 花些时间将此属性设置为`True`。


[![UnitPrice BoundField ApplyFormatInEditMode 属性设置为 True](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image25.png)

**图 9**： 设置`UnitPrice`BoundField 的`ApplyFormatInEditMode`属性`True`([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image27.png))


进行此更改后，值`UnitPrice`显示在编辑行也设置为货币的格式。


[![编辑行的单价值是现在格式的作为一种货币](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image28.png)

**图 10**: 编辑行`UnitPrice`值现在格式化为货币 ([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image30.png))


但是，在文本框中的货币符号与更新产品，如 $19.00 引发`FormatException`。 当尝试将用户提供的值分配给对象数据源的 GridView`UpdateParameters`集合无法转换`UnitPrice`到字符串"$19.00"`Decimal`为参数要求 （请参阅图 11）。 若要纠正此我们可以为 GridView 创建事件处理程序`RowUpdating`事件，并将其分析用户提供`UnitPrice`作为货币格式`Decimal`。

GridView`RowUpdating`接受作为其第二个参数类型的对象的事件[GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx)，其中包括`NewValues`作为包含的用户提供的值可供其属性之一的字典分配给 ObjectDataSource`UpdateParameters`集合。 我们可以覆盖现有`UnitPrice`中的值`NewValues`十进制值集合分析用以下行中的代码使用货币格式`RowUpdating`事件处理程序：


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample4.vb)]

如果用户已提供`UnitPrice`值 （如"$19.00") 与计算出的十进制值会覆盖了此值[Decimal.Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx)，分析作为一种货币值。 这将正确分析小数点发生任何货币符号、 逗号、 小数点、 和等等，并使用[NumberStyles 枚举](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx)中[System.Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx)命名空间。

图 11 显示这两个问题引起的中的用户提供的货币符号`UnitPrice`，以及如何 GridView`RowUpdating`事件处理程序可以利用为了正确分析这类输入。


[![编辑行的单价值是现在格式的作为一种货币](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image31.png)

**图 11**: 编辑行`UnitPrice`值现在格式化为货币 ([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>步骤 2： 禁止`NULL UnitPrices`

尽管数据库配置为允许`NULL`中值`Products`表的`UnitPrice`列中，我们可能想要阻止用户访问此特定页，指定从`NULL``UnitPrice`值。 也就是说，如果用户无法输入`UnitPrice`时编辑产品行的值，而不是将结果保存到数据库我们想要显示一条消息，通知用户，通过此页上，编辑的任何产品必须具有指定的价格。

`GridViewUpdateEventArgs`对象传递到的 GridView`RowUpdating`事件处理程序包含`Cancel`属性，如果设置为`True`，终止更新程序。 让我们扩展`RowUpdating`事件处理程序来设置`e.Cancel`到`True`并显示一条消息说明原因，如果`UnitPrice`中的值`NewValues`集合包含的值`Nothing`。

通过将标签 Web 控件添加到名为的页启动`MustProvideUnitPriceMessage`。 如果用户无法指定，将会显示此标签控件`UnitPrice`值更新产品时。 设置标签的`Text`属性设置为"你必须提供价格的产品。" 我还创建了新 CSS 类中的`Styles.css`名为`Warning`替换为以下定义：


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample5.css)]

最后，设置标签的`CssClass`属性`Warning`。 此时设计器应显示以红色、 粗体、 斜体显示的警告消息、 特大型字体大小超出 GridView，如图 12 中所示。


[![上面 GridView 中添加标签](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image34.png)

**图 12**: 标签具有已添加上面 GridView ([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image36.png))


默认情况下，此标签应被隐藏，因此设置其`Visible`属性`False`中`Page_Load`事件处理程序：


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample6.vb)]

如果用户尝试将产品更新而无需指定`UnitPrice`，我们想要取消更新并显示警告标签。 增加 GridView`RowUpdating`事件处理程序，如下所示：


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample7.vb)]

如果用户尝试保存产品而无需指定价格，更新会取消并会显示很有帮助的消息。 而在数据库 （和业务逻辑） 允许`NULL` `UnitPrice` s，此特定的 ASP.NET 页却没有。


[![用户不能离开单价保留为空白](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image37.png)

**图 13**: 用户不能离开`UnitPrice`空白 ([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image39.png))


到目前为止，我们已了解如何使用 GridView`RowUpdating`事件以编程方式更改分配给对象数据源的参数值`UpdateParameters`集合以及如何取消更新处理完全。 这些概念会延续到说明和 FormView 控件和也适用于插入和删除。

也可以在对象数据源级别事件处理程序通过执行这些任务其`Inserting`， `Updating`，和`Deleting`事件。 这些事件激发之前调用的基础对象的关联的方法，并提供上一次机会机会来修改输入的参数集合，或者取消迫切地操作。 这三个事件的事件处理程序传递的类型对象[ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx)具有两个相关属性：

- [取消](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx)，后者，如果设置为`True`，取消正在执行的操作
- [输入参数](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx)，这是集合的`InsertParameters`， `UpdateParameters`，或`DeleteParameters`，具体取决于事件处理程序是否为`Inserting`， `Updating`，或`Deleting`事件

为了说明使用对象数据源级别的参数值，让我们在我们的页面，用户可添加新的产品中包含说明如何。 此说明如何将用于提供用于将新产品快速添加到数据库的接口。 若要在添加新的产品，让我们允许用户只需输入的值时保持一致的用户界面`ProductName`和`UnitPrice`字段。 默认情况下，在说明的插入接口中不提供这些值将设置为`NULL`数据库值。 但是，我们可以使用对象数据源的`Inserting`以插入不同的默认值，正如我们很快将看到的事件。

## <a name="step-3-providing-an-interface-to-add-new-products"></a>步骤 3： 提供一个接口，以便添加新产品

将说明如何从工具箱中拖动到设计器上面 GridView，拖动清除其`Height`和`Width`属性，并绑定到 ObjectDataSource 页已存在。 这将为每个产品的字段添加 BoundField 或 CheckBoxField。 由于我们想要使用此说明如何添加新的产品，我们需要检查从智能标记; 启用插入选项但是，没有此类选项因为 ObjectDataSource`Insert()`方法未映射到中的方法`ProductsBLL`类 （回想一下，我们设置此映射到 （无） 时配置数据源，请参见图 3）。

若要配置对象数据源，请从其智能标记，启动该向导中选择的配置数据源链接。 第一个屏幕可让你可以更改基础对象数据源绑定到; 的对象保留`ProductsBLL`。 下一个屏幕中列出的映射与从 ObjectDataSource 的方法的基础对象。 即使显式指出`Insert()`和`Delete()`方法不应映射到任何方法，如果你转到插入和删除选项卡将会看到映射是否存在。 这是因为`ProductsBLL`的`AddProduct`和`DeleteProduct`方法使用`DataObjectMethodAttribute`特性以指示它们是默认方法`Insert()`和`Delete()`分别。 因此，ObjectDataSource 向导会选择这些每次你运行向导，除非显式指定的一些其他值。

保留`Insert()`方法指向`AddProduct`方法，但重新设置为 （无） 删除选项卡的下拉列表。


[![将插入选项卡的下拉列表设置为 AddProduct 方法](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image40.png)

**图 14**： 将插入选项卡的下拉列表设置为`AddProduct`方法 ([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image42.png))


[![设置为 （无） 删除选项卡的下拉列表](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image43.png)

**图 15**： 设置为 (None) 的删除选项卡的下拉列表 ([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image45.png))


进行这些更改后，ObjectDataSource 的声明性语法，将会展开以包括`InsertParameters`集合，如下所示：


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample8.aspx)]

重新运行该向导添加回`OldValuesParameterFormatString`属性。 花一些时间来清除此属性，将其设置为默认值 (`{0}`) 或将其删除的声明性语法一起删除。

使用对象数据源提供插入功能，说明如何的智能标记现在将包括启用插入的复选框。返回到设计器，并选中此选项。 接下来，削减说明，以便它仅有两个 BoundFields-`ProductName`和`UnitPrice`-和 CommandField。 在此点说明的声明性语法，应如下所示：


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample9.aspx)]

图 16 显示此页，此时查看通过浏览器时。 如你所见，说明如何列出的名称和 （牛奶） 的第一个产品价格。 但是，我们希望是插入接口，提供用户快速将新产品添加到数据库的一种方法。


[![说明如何为当前呈现在只读模式下](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image46.png)

**图 16**: 说明如何为当前呈现在只读模式下 ([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image48.png))


为了显示说明如何在插入模式下我们需要将设置`DefaultMode`属性`Inserting`。 此呈现在插入模式下时首次访问说明，并阻止插入一条新记录后对其存在。 如图 17 所示，此类说明提供快速接口添加新记录。


[![说明如何提供一个接口，以便将新产品快速添加](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image49.png)

**图 17**: 说明如何提供一个接口以便将快速添加新的产品 ([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image51.png))


当用户输入的产品名称和价格 （如"Acme Water"和 1.99，如下所示图 17），并单击插入时，回发时，才会和插入工作流开始，新产品记录添加到数据库中结束。 说明如何维护其插入接口和 GridView 自动重新绑定到其数据源以便包含新的产品，如图 18 中所示。


![产品](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image52.png)

**图 18**： 产品"Acme Water"已添加到数据库


虽然图 18 中的 GridView 不显示它，说明如何接口从缺少产品字段`CategoryID`， `SupplierID`， `QuantityPerUnit`，依次类推分配`NULL`数据库值。 你可以看到此通过执行以下步骤：

1. 转到 Visual Studio 中的服务器资源管理器
2. 展开`NORTHWND.MDF`数据库节点
3. 右键单击`Products`数据库表节点
4. 选择显示表数据

此命令将列出所有中的记录`Products`表。 如图 19 所示，我们新产品的列的所有以外`ProductID`， `ProductName`，和`UnitPrice`具有`NULL`值。


[![说明如何在未提供产品字段是分配 NULL 值](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image53.png)

**图 19**: 产品字段中未提供说明如何分配`NULL`值 ([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image55.png))


我们可能想要提供默认值以外`NULL`的一个或多个列值，或者因为`NULL`不是最佳的默认选项或因为本身的数据库列不允许`NULL`s。 若要实现此目的我们可以以编程方式设置的说明的参数的值`InputParameters`集合。 此分配可能会出于或者在事件处理程序说明如何的`ItemInserting`事件或 ObjectDataSource`Inserting`事件。 由于我们已介绍在使用预和后级别事件的数据 Web 控制级别，让我们研究一下这一次使用对象数据源的事件。

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>步骤 4： 将值赋给`CategoryID`和`SupplierID`参数

对于本教程让我们想象，对于我们的应用程序中添加新产品通过此接口时它应分配`CategoryID`和`SupplierID`值为 1。 如前所述，ObjectDataSource 有一对预和后级别事件触发数据修改过程。 当其`Insert()`调用方法，ObjectDataSource 首先引发其`Inserting`事件，然后调用的方法，其`Insert()`方法映射到，并最后引发`Inserted`事件。 `Inserting`事件处理程序为我们提供了一个最后一次机会调整的输入的参数，或者取消迫切地操作。

> [!NOTE]
> 在实际应用程序中你可能想让用户指定的类别和供应商，或将它们根据某些标准选取此值或业务逻辑 （而不是隐式选择 ID 为 1）。 无论如何，该示例说明如何以编程方式设置从 ObjectDataSource 预级别事件的输入参数的值。


花一些时间创建对象数据源的事件处理程序`Inserting`事件。 请注意，事件处理程序的第二个输入的参数是类型的对象`ObjectDataSourceMethodEventArgs`，它具有要对参数集合的访问的属性 (`InputParameters`) 和要取消操作的属性 (`Cancel`)。


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample10.vb)]

此时，`InputParameters`属性包含 ObjectDataSource`InsertParameters`与说明如何从所赋的值的集合。 若要更改的其中一个参数的值，只需使用： `e.InputParameters("paramName") = value`。 因此，若要设置`CategoryID`和`SupplierID`为 1 的值，调整`Inserting`事件处理程序如下所示：


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample11.vb)]

此时间添加新产品 （如 acme 开发 Soda) 时`CategoryID`和`SupplierID`新产品的列将设置为 1 （请参阅图 20）。


[![新的产品现在具有其 CategoryID 和供应商 Id 值设置为 1](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image56.png)

**图 20**： 新产品现在拥有他们`CategoryID`和`SupplierID`值设置为 1 ([单击以查看实际尺寸的图像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image58.png))


## <a name="summary"></a>摘要

在编辑、 插入和删除过程，过程数据 Web 控件和 ObjectDataSource 继续完成复制前和后级别的事件数。 在本教程中我们将检查预级别的事件，并已了解如何使用这些自定义输入的参数或取消数据修改操作完全同时从数据 Web 控件和对象数据源的事件。 在下一教程中我们将查看创建和使用针对后级别事件的事件处理程序。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Jackie Goor 和沈 Shulok。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](an-overview-of-inserting-updating-and-deleting-data-vb.md)
[下一页](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
