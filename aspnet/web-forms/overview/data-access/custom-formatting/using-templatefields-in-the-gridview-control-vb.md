---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
title: "在 GridView 控件 (VB) 中使用 TemplateFields |Microsoft 文档"
author: rick-anderson
description: "若要提供灵活性，GridView 提供 TemplateField，来呈现使用模板。 模板可以包括多种静态 HTML，Web 控件和..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: a92cd6ed-609a-4e40-ad23-004b54afd436
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 337765988cc6ec92384bec09a72fd00505d9a039
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="using-templatefields-in-the-gridview-control-vb"></a>在 GridView 控件 (VB) 中使用 TemplateFields
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_12_VB.exe)或[下载 PDF](using-templatefields-in-the-gridview-control-vb/_static/datatutorial12vb1.pdf)

> 若要提供灵活性，GridView 提供 TemplateField，来呈现使用模板。 模板中可包含混合的静态 HTML，Web 控件和数据绑定语法。 在本教程中我们将查看如何使用 TemplateField 来实现更大程度上与 GridView 控件的自定义。


## <a name="introduction"></a>介绍

GridView 组成的字段，指示哪些属性从一组`DataSource`要包括在呈现的输出以及数据的显示方式。 最简单的字段类型是 BoundField 后者将数据值显示为文本。 其他字段类型显示使用备用的 HTML 元素的数据。 CheckBoxField，例如，将呈现为一个复选框的选中的状态取决于指定的数据字段; 的值ImageField 呈现其图像源基于指定的数据字段的图像。 使用 HyperLinkField 和 ButtonField 字段类型可以呈现超链接和按钮的状态取决于基础数据字段值。

虽然 CheckBoxField、 ImageField、 HyperLinkField 和 ButtonField 字段类型允许的数据的替代视图，它们仍是相对于格式相当有限。 CheckBoxField 只能显示一个复选框，而 ImageField 只能显示单个映像。 如果特定字段需要显示一些文本，一个复选框，*和*映像，在不同的数据字段值的所有基础？ 或者，如果我们想要显示使用复选框、 图像、 超链接或按钮以外的 Web 控件的数据？ 此外，BoundField 限制到单个数据字段显示。 如果我们想要在单个的 GridView 列显示两个或多个数据字段值？

为了适应这一级别的灵活性 GridView 提供 TemplateField，来呈现使用*模板*。 模板中可包含混合的静态 HTML，Web 控件和数据绑定语法。 此外，TemplateField 有许多可以使用不同的情况下呈现进行自定义的模板。 例如，`ItemTemplate`默认情况下用于呈现的单元格的每一行，但`EditItemTemplate`模板可以用于编辑数据时，自定义界面。

在本教程中我们将查看如何使用 TemplateField 来实现更大程度上与 GridView 控件的自定义。 在[前面教程](custom-formatting-based-upon-data-vb.md)我们已了解如何自定义格式基于基础数据使用`DataBound`和`RowDataBound`事件处理程序。 若要自定义基于基础数据的格式设置的另一种方法通过调用格式设置方法从在模板中。 我们将考察以及本教程中此技术。

本教程中我们将使用 TemplateFields 自定义的外观的员工列表。 具体而言，我们将列出所有员工，但将显示该员工的一个列中，在日历控件和一个状态列指示多少天它们已被采用的公司中其雇佣日期中的第一个和最后一个名称。


[![三个 TemplateFields 用于自定义的显示](using-templatefields-in-the-gridview-control-vb/_static/image2.png)](using-templatefields-in-the-gridview-control-vb/_static/image1.png)

**图 1**： 三个 TemplateFields 用于自定义显示 ([单击以查看实际尺寸的图像](using-templatefields-in-the-gridview-control-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>步骤 1： 将数据绑定到 GridView

在报告中你需要使用 TemplateFields 自定义的外观的情况下，找到它最简单的方法首先创建第一次包含刚 BoundFields 的 GridView 控件，然后添加新 TemplateFields 或转换到现有 BoundFields根据需要 TemplateFields。 因此，通过逐页浏览设计器中添加一个 GridView，并将其绑定到返回的员工列表 ObjectDataSource 而让我们开始本教程。 这些步骤将为每个员工字段与 BoundFields 创建 GridView。

打开`GridViewTemplateField.aspx`页上，并从工具箱中拖动到设计器中拖动 GridView。 从 GridView 的智能标记选择添加一个新的 ObjectDataSource 控件调用`EmployeesBLL`类的`GetEmployees()`方法。


[![添加一个新的 ObjectDataSource 控件调用 GetEmployees() 方法](using-templatefields-in-the-gridview-control-vb/_static/image5.png)](using-templatefields-in-the-gridview-control-vb/_static/image4.png)

**图 2**： 添加新的 ObjectDataSource 控件该 Invoke`GetEmployees()`方法 ([单击以查看实际尺寸的图像](using-templatefields-in-the-gridview-control-vb/_static/image6.png))


以这种方式绑定 GridView 将自动添加 BoundField 为每个员工属性： `EmployeeID`， `LastName`， `FirstName`， `Title`， `HireDate`， `ReportsTo`，和`Country`。 为此报表让我们不涉及显示`EmployeeID`， `ReportsTo`，或`Country`属性。 若要删除这些 BoundFields 您可以：

- 使用字段对话框中，单击从 GridView 的智能标记的编辑列链接以打开此对话框。 接下来，选择从左下角 BoundFields 列表，然后单击红色的 X 按钮以删除 BoundField。
- 从源视图中手动编辑 GridView 的声明性语法，删除`<asp:BoundField>`BoundField 你想要删除的元素。

在移除之后`EmployeeID`， `ReportsTo`，和`Country`BoundFields，你 GridView 标记应如下所示：


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample1.aspx)]

需要一段时间来在浏览器中查看我们的进度。 此时你应看到一条记录的表的每个雇员，各四个列： 一个用于员工的姓氏，一个用于其第一个名称，一个用于其标题，一个用于其雇佣日期。


[![每个员工的显示 LastName、 FirstName、 标题和的 HireDate 字段](using-templatefields-in-the-gridview-control-vb/_static/image8.png)](using-templatefields-in-the-gridview-control-vb/_static/image7.png)

**图 3**: `LastName`， `FirstName`， `Title`，和`HireDate`字段显示为每个员工 ([单击以查看实际尺寸的图像](using-templatefields-in-the-gridview-control-vb/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>步骤 2： 在单个列中显示的第一个和最后一个名称

目前，每个员工的名字和姓氏显示在一个单独的列。 可能最好改为将它们组合成单个列。 若要实现此目的，我们需要使用为 TemplateField。 我们可以是添加新 TemplateField、 向其中添加所需的标记和数据绑定语法，然后删除`FirstName`和`LastName`BoundFields，或者我们可以将转换`FirstName`转换为 TemplateField BoundField 编辑 TemplateField 包括`LastName`值，，然后删除`LastName`BoundField。

这两种方法 net 相同的结果，但我喜欢将 BoundFields 转换为 TemplateFields 如果可能，因为转换会自动添加个人`ItemTemplate`和`EditItemTemplate`Web 控件与数据绑定语法来模拟外观和 BoundField 功能。 好处是，我们将需要执行工作较少使用 TemplateField 如转换过程将已执行的一些工作为我们。

若要将现有 BoundField 转换转换为 TemplateField，请单击从 GridView 的智能标记，提出字段对话框中的编辑列链接。 选择 BoundField 转换从列表中左下角，然后单击右下角中的"转换此字段转换为 TemplateField"链接。


[![将 BoundField 转换转换为 TemplateField 从字段对话框](using-templatefields-in-the-gridview-control-vb/_static/image11.png)](using-templatefields-in-the-gridview-control-vb/_static/image10.png)

**图 4**： 从字段对话框中转换为 BoundField 到 TemplateField ([单击以查看实际尺寸的图像](using-templatefields-in-the-gridview-control-vb/_static/image12.png))


请继续并转换`FirstName`转换为 TemplateField BoundField。 此更改之后在设计器中没有角度差异。 这是因为转换转换为 TemplateField BoundField，可以创建为 TemplateField 维护 BoundField 的外观和感觉。 尽管没有 visual 差异此时设计器中的存在，此转换过程已取代 BoundField 的声明性语法- `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` -具有以下 TemplateField 语法：


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample2.aspx)]

如你所见，TemplateField 包含两个模板`ItemTemplate`具有标签其`Text`属性设置为的值`FirstName`数据字段和`EditItemTemplate`与文本框控件`Text`也设置属性到`FirstName`数据字段。 数据绑定语法中- `<%# Bind("fieldName") %>` -指示数据字段 *`fieldName`* 绑定到指定的 Web 控件属性。

若要添加`LastName`数据字段值我们需要添加另一个标签 Web 控件中的此 TemplateField`ItemTemplate`并将绑定其`Text`属性`LastName`。 这可以手动或通过设计器实现。 若要手动执行它，只需将添加到合适的声明性语法`ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample3.aspx)]

若要将其添加通过设计器中，单击从 GridView 的智能标记的编辑模板链接。 这将显示 GridView 的模板编辑界面。 在此接口的智能标记是 GridView 中的模板的列表。 因为只有一个 TemplateField 此时，下拉列表中列出的唯一模板是这些模板`FirstName`连同 TemplateField`EmptyDataTemplate`和`PagerTemplate`。 `EmptyDataTemplate`模板，如果指定，用于呈现 GridView 的输出中是否存在任何结果的数据绑定到 GridView; `PagerTemplate`，如果指定，用于呈现一个 GridView，支持分页的分页界面。


[![可以通过设计器编辑 GridView 的模板](using-templatefields-in-the-gridview-control-vb/_static/image14.png)](using-templatefields-in-the-gridview-control-vb/_static/image13.png)

**图 5**: GridView 模板可以是编辑通过设计器 ([单击以查看实际尺寸的图像](using-templatefields-in-the-gridview-control-vb/_static/image15.png))


此外显示`LastName`中`FirstName`TemplateField 将 Label 控件拖动到工具箱从`FirstName`TemplateField 的`ItemTemplate`在 GridView 的模板编辑接口。


[![将标签 Web 控件添加到 FirstName TemplateField ItemTemplate](using-templatefields-in-the-gridview-control-vb/_static/image17.png)](using-templatefields-in-the-gridview-control-vb/_static/image16.png)

**图 6**： 添加标签 Web 控件与`FirstName`TemplateField 的 ItemTemplate ([单击以查看实际尺寸的图像](using-templatefields-in-the-gridview-control-vb/_static/image18.png))


此时标签 Web 控件添加到 TemplateField 具有其`Text`属性设置为"标签"。 我们需要更改此设置，以便此属性绑定到的值`LastName`数据字段。 若要完成此标签控件的智能标记，请单击并选择编辑数据绑定选项。


[![从标签的智能标记中选择编辑 DataBindings 选项](using-templatefields-in-the-gridview-control-vb/_static/image20.png)](using-templatefields-in-the-gridview-control-vb/_static/image19.png)

**图 7**： 编辑数据绑定选项从中选择标签的智能标记 ([单击以查看实际尺寸的图像](using-templatefields-in-the-gridview-control-vb/_static/image21.png))


这将显示数据绑定对话框。 从这里你可以选择要参与数据绑定，从左侧列表并选择要将数据绑定到从下拉列表右侧的字段的属性。 选择`Text`从左侧的属性和`LastName`字段从右侧，单击确定。


[![将文本属性绑定到 LastName 数据字段](using-templatefields-in-the-gridview-control-vb/_static/image23.png)](using-templatefields-in-the-gridview-control-vb/_static/image22.png)

**图 8**： 绑定`Text`属性`LastName`数据字段 ([单击以查看实际尺寸的图像](using-templatefields-in-the-gridview-control-vb/_static/image24.png))


> [!NOTE]
> 数据绑定对话框中，可指示是否执行双向数据绑定。 如果你将此取消选中后，数据绑定语法`<%# Eval("LastName")%>`将而不是使用`<%# Bind("LastName")%>`。 出于本教程，这两种方法是正常。 双向数据绑定变得重要插入和编辑数据。 对于只需显示数据，但是，这两种方法将同样能正常工作。 在将来的教程中，我们将讨论详细的双向数据绑定。


花些时间查看通过浏览器的此页。 如你所见，GridView 仍包含四列;但是，`FirstName`列现在列出*同时*`FirstName`和`LastName`数据字段值。


[![FirstName 和 LastName 值显示在单个列](using-templatefields-in-the-gridview-control-vb/_static/image26.png)](using-templatefields-in-the-gridview-control-vb/_static/image25.png)

**图 9**： 同时`FirstName`和`LastName`单个列中显示值 ([单击以查看实际尺寸的图像](using-templatefields-in-the-gridview-control-vb/_static/image27.png))


若要完成此第一步，删除`LastName`BoundField 和重命名`FirstName`TemplateField 的`HeaderText`为"Name"的属性。 这些更改后 GridView 的声明性标记应如下所示：


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample4.aspx)]


[![将一个列中显示每个员工的第一个和最后一个名称](using-templatefields-in-the-gridview-control-vb/_static/image29.png)](using-templatefields-in-the-gridview-control-vb/_static/image28.png)

**图 10**： 将一个列中显示每个员工的第一个和最后一个名称 ([单击以查看实际尺寸的图像](using-templatefields-in-the-gridview-control-vb/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>步骤 3： 使用日历控件显示`HiredDate`字段

在 GridView 中以文本显示数据字段值非常简单，使用 BoundField。 对于某些方案中，但是，数据是最表示而不只包含文本中使用特定的 Web 控件。 此类显示的数据的自定义是，可以使用 TemplateFields。 例如，而不是以文本形式显示该雇员雇佣日期，我们无法显示日历 (使用[日历控件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) 其雇用日期突出显示。

若要完成此操作，首先要将转换`HiredDate`转换为 TemplateField BoundField。 只需转到 GridView 的智能标记，并单击编辑列链接，提出字段对话框。 选择`HiredDate`BoundField，然后单击"转换此字段转换为 TemplateField。"


[![将 HiredDate BoundField 转换转换为 TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image32.png)](using-templatefields-in-the-gridview-control-vb/_static/image31.png)

**图 11**： 转换`HiredDate`BoundField 到 TemplateField ([单击以查看实际尺寸的图像](using-templatefields-in-the-gridview-control-vb/_static/image33.png))


正如我们在步骤 2 中看到的这将使用包含为 TemplateField 替换 BoundField`ItemTemplate`和`EditItemTemplate`与标签和文本框其`Text`属性绑定到`HiredDate`值使用数据绑定语法`<%# Bind("HiredDate")%>`.

若要与日历控件替换的文本，请通过删除标签，然后添加一个日历控件编辑的模板。 从设计器中，选择编辑模板从 GridView 的智能标记，并选择`HireDate`TemplateField 的`ItemTemplate`从下拉列表。 接下来，删除该标签控件，并将月历控件从工具箱拖到模板编辑界面。


[![添加到一个日历控件的 HireDate TemplateField 的 ItemTemplate](using-templatefields-in-the-gridview-control-vb/_static/image35.png)](using-templatefields-in-the-gridview-control-vb/_static/image34.png)

**图 12**： 添加到一个日历控件`HireDate`TemplateField 的`ItemTemplate`([单击以查看实际尺寸的图像](using-templatefields-in-the-gridview-control-vb/_static/image36.png))


此时 GridView 中的每一行将包含中的日历控件其`HiredDate`TemplateField。 但是，该员工的实际`HiredDate`不在日历控件，从而导致每个默认为显示的当前月份和日期的日历控件中任何位置设置值。 若要解决此问题，我们需要分配每个员工的`HiredDate`到日历控件的[SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx)和[VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx)属性。

从日历控件的智能标记上，选择编辑数据绑定。 接下来，将同时绑定`SelectedDate`和`VisibleDate`属性设置为`HiredDate`数据字段。


[![将 SelectedDate 和 VisibleDate 属性绑定到 HiredDate 数据字段](using-templatefields-in-the-gridview-control-vb/_static/image38.png)](using-templatefields-in-the-gridview-control-vb/_static/image37.png)

**图 13**： 绑定`SelectedDate`和`VisibleDate`属性设置为`HiredDate`数据字段 ([单击以查看实际尺寸的图像](using-templatefields-in-the-gridview-control-vb/_static/image39.png))


> [!NOTE]
> 月历控件所选的日期不一定需要可见。 例如，日历可能有 8 月 1 日<sup>st</sup>、 1999 为选定的日期，但在当前月份和年份显示。 日历控件的指定的所选的日期和可见日期`SelectedDate`和`VisibleDate`属性。 由于我们希望这两个选择员工的`HiredDate`并确保它将显示我们需要将这两种属性为绑定`HireDate`数据字段。


当在浏览器中查看的页面，日历将现在显示员工的雇用日期的月份，并选择该特定的日期。


[![日历控件中显示该员工的 HiredDate](using-templatefields-in-the-gridview-control-vb/_static/image41.png)](using-templatefields-in-the-gridview-control-vb/_static/image40.png)

**图 14**: 员工的`HiredDate`日历控件中所示 ([单击以查看实际尺寸的图像](using-templatefields-in-the-gridview-control-vb/_static/image42.png))


> [!NOTE]
> 是否所有的示例中，我们已了解到目前为止，违反本教程中我们未*不*设置`EnableViewState`属性`False`为此 GridView。 此决策的原因是因为单击日历控件的日期会导致回发时，将所选的日期设置为只需单击日期。 如果禁用了 GridView 的视图状态，但是，每次回发 GridView 的数据重新绑定到其基础数据源，这会导致日历的选定的日期设置*回*到员工的`HireDate`、 覆盖由用户选择日期。


本教程中这是一个毫无意义讨论因为用户不能更新该员工的`HireDate`。 它很可能是最佳配置日历控件，以便其日期不可选择。 无论如何，本教程演示，在某些情况下视图状态必须启用以便提供某些功能。

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>步骤 4： 显示员工的天数已在公司工作的

到目前为止，我们已了解 TemplateFields 的两个应用的程序：

- 将两个或多个数据字段值合并为一个列和
- 表示数据字段值而不文本中使用的 Web 控件

使用第三个 TemplateFields 中显示有关 GridView 的元数据为基础数据。 除了显示员工的雇佣日期，例如，我们可能还想要显示多少他们已经在作业的总天数的列。

尚未 TemplateFields 的另一个用途体现在方案，如果需要比数据库中存储格式，以不同方式显示在 web 页报表基础的数据。 假设，`Employees`表具有`Gender`存储字符的字段`M`或`F`以指示该雇员的性别。 在网页中显示此信息时我们可能想要显示性别，为"男性"或"女性"，而不是仅"M"或"F"。

这两种方案可以通过创建处理*格式设置方法*在 ASP.NET 页的代码隐藏类 (或在单独的类库，作为实现`Shared`方法) 调用从模板。 这样的格式设置方法是从使用相同的数据绑定语法前面看到的模板调用的。 格式设置方法可能需要在任意数量的参数，但必须返回一个字符串。 此返回的字符串是注入到模板的 HTML。

为了说明这一概念，让我们增加我们的教程中显示的列，其中列出了在作业已被某位员工总天数。 此格式设置方法将采用`Northwind.EmployeesRow`对象并返回的员工已被采用字符串形式的天数。 此方法可以添加到 ASP.NET 页的代码隐藏类，但*必须*标记为`Protected`或`Public`才能从该模板可访问。


[!code-vb[Main](using-templatefields-in-the-gridview-control-vb/samples/sample5.vb)]

由于`HiredDate`字段只能包含`NULL`数据库的值我们首先必须确保该值不是`NULL`然后继续进行计算。 如果`HiredDate`值是`NULL`，我们只需返回字符串"Unknown"; 如果还没有`NULL`，我们计算的当前时间之间的差异和`HiredDate`值和返回的天数。

若要利用此方法，我们需要调用中使用的数据绑定语法 GridView TemplateField 从。 通过将新 TemplateField 添加到 GridView，通过单击 GridView 的智能标记中的编辑列链接并添加新 TemplateField 启动。


[![将新 TemplateField 添加到 GridView](using-templatefields-in-the-gridview-control-vb/_static/image44.png)](using-templatefields-in-the-gridview-control-vb/_static/image43.png)

**图 15**： 将新 TemplateField 添加到 GridView ([单击以查看实际尺寸的图像](using-templatefields-in-the-gridview-control-vb/_static/image45.png))


设置此新 TemplateField`HeaderText`属性设置为"天上作业"并将其`ItemStyle`的`HorizontalAlign`属性`Center`。 若要调用`DisplayDaysOnJob`从模板中的方法添加`ItemTemplate`，使用以下数据绑定语法：


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample6.aspx)]

`Container.DataItem`返回`DataRowView`对象对应于`DataSource`记录绑定到`GridViewRow`。 其`Row`属性返回强类型`Northwind.EmployeesRow`，将传递给`DisplayDaysOnJob`方法。 此数据绑定语法可以显示直接在`ItemTemplate`（如下面的声明性语法中所示） 或可以分配给`Text`标签 Web 控件的属性。

> [!NOTE]
> 或者，而不是传入`EmployeesRow`实例，我们无法只需传递`HireDate`值使用`<%# DisplayDaysOnJob(Eval("HireDate")) %>`。 但是，`Eval`方法返回`Object`，因此我们必须要更改我们`DisplayDaysOnJob`方法签名，以接受输入的参数的类型`Object`，而不是。 我们不能隐式强制转换`Eval("HireDate")`调用`DateTime`因为`HireDate`中的列`Employees`表可以包含`NULL`值。 因此，我们将需要接受`Object`作为输入参数为`DisplayDaysOnJob`方法中，如果它有一个数据库检查`NULL`值 (可以使用完成其`Convert.IsDBNull(objectToCheck)`)，并相应地继续。


由于这些细微部分我已选择传入整个`EmployeesRow`实例。 在下一步的教程，我们将看到使用的多个调整示例`Eval("columnName")`将传入的格式设置方法的输入的参数的语法。

以下显示我们 GridView 的声明性语法在已添加 TemplateField 后和`DisplayDaysOnJob`从调用的方法`ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample7.aspx)]

图 16 显示已完成的教程中，通过浏览器查看时。


[![显示员工已在作业的天数](using-templatefields-in-the-gridview-control-vb/_static/image47.png)](using-templatefields-in-the-gridview-control-vb/_static/image46.png)

**图 16**: 天数员工已在作业显示 ([单击以查看实际尺寸的图像](using-templatefields-in-the-gridview-control-vb/_static/image48.png))


## <a name="summary"></a>摘要

在 GridView 控件 TemplateField 允许更高的灵活性比其他字段控件中显示数据。 TemplateFields 非常适合于情况其中：

- 多个数据字段都需要一个 GridView 列中显示
- 数据是最表示使用的 Web 控件，而不是纯文本
- 输出取决于基础数据，例如显示元数据或在重新格式化数据

除了自定义数据的显示，TemplateFields 还可用于自定义用于编辑和插入数据，正如我们将在将来看到教程的用户界面。

下面的两个教程继续浏览模板，在说明如何使用 TemplateFields 查看从开始。 接下来，我们将转到 FormView，而不是字段中使用模板来提供更好的灵活性的布局和数据结构。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Dan Jagers。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](custom-formatting-based-upon-data-vb.md)
[下一页](using-templatefields-in-the-detailsview-control-vb.md)
