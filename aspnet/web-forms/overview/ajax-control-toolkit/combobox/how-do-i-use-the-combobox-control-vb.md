---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: "如何使用 ComboBox 控件？ (VB) |Microsoft 文档"
author: microsoft
description: "组合框是一个文本框中的灵活性结合的用户可以从中选择的选项列表的 ASP.NET AJAX 控件。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 54e36cf275dcc4b85253dc3b8bb5b0dbb027af77
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-use-the-combobox-control-vb"></a>如何使用 ComboBox 控件？ (VB)
====================
通过[Microsoft](https://github.com/microsoft)

> 组合框是一个文本框中的灵活性结合的用户可以从中选择的选项列表的 ASP.NET AJAX 控件。


本教程旨在说明 AJAX 控件工具包 ComboBox 控件。 组合框的工作原理类似之间标准 ASP.NET DropDownList 控件和文本框控件的组合。 你可以从预先存在的项列表中选择或输入新的项目。

组合框类似于记忆式键入功能控件扩展程序，但在不同的方案中使用的控件。 记忆式键入功能扩展程序查询 web 服务以获取匹配项。 与此相反，组合框控件中，使用一组项的初始化。 使用自动完成扩展程序使意义时你正在使用大量数据 （数以百万计的汽车部件） 时使用组合框控件时有意义使用一小部分的数据 （数十个汽车部分）。

## <a name="selecting-from-a-static-list-of-items"></a>从静态的项列表中选择

允许 s 开头使用组合框控件的一个简单的示例。 假设你想要在下拉列表中显示的项的静态列表。 但是，你想要使保持打开状态的列表并不完整的可能性。 你想要允许用户输入到列表的自定义值。

我们将创建新的 ASP.NET Web 窗体页并在页中使用 ComboBox 控件。 将新的 ASP.NET 页添加到你的项目，并切换到设计视图。

如果你想要在页中使用的组合框控件必须将 ScriptManager 控件添加到页。 从 AJAX Extensions 选项卡将 ScriptManager 控件拖动到设计器图面。 应在页; 的顶部添加 ScriptManager 控件将其添加下面打开服务器端的立即&lt;窗体&gt;标记。

接下来，将 ComboBox 控件拖到该页面。 与其他 AJAX 控件工具包控件和控件扩展程序 （请参见图 1） 工具箱中，可以找到 ComboBox 控件。


[![用于创建名片简单窗体](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)

**图 01**： 从工具箱中选择的组合框控件 ([单击以查看实际尺寸的图像](how-do-i-use-the-combobox-control-vb/_static/image2.png))


我们将使用 ComboBox 控件以显示静态列表的选择。 用户可以从三个选项的列表中选择特定级别的其食品 spiciness： 轻微、 中型和作用 （请参见图 2）。


[![从静态的项列表中选择](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)

**图 02**： 从静态的项列表中选择 ([单击以查看实际尺寸的图像](how-do-i-use-the-combobox-control-vb/_static/image4.png))


有两种方法，可以将这些选项添加到 ComboBox 控件。 首先，选择任务编辑选项选项，当鼠标悬停在设计视图中的控件和打开项编辑器 （请参见图 3）。


[![编辑组合框项](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)

**图 03**： 编辑组合框项 ([单击以查看实际尺寸的图像](how-do-i-use-the-combobox-control-vb/_static/image6.png))


第二个选项是添加到开始和结束之间的项列表&lt;asp: ComboBox&gt;源视图中的标记。 列表 1 中的页面包含更新组合框具有的项的列表。

**列表 1-Static.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

列出 1 中打开页面时，你可以从组合框中选择预先存在的选项之一。 换而言之，组合框的工作方式与 DropDownList 控件。

但是，还必须输入 （例如，超级辣味） 现有列表中不是一个新选择的选项。 因此，组合框还工作原理类似文本框控件。

无论是否选择预先存在的项，或输入自定义项，在提交窗体中，你的选择会显示标签控件中时。 当你将提交窗体上，btnSubmit\_单击处理程序执行，并更新标签 （请参见图 4）。


[![显示选定的项](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)

**图 04**： 显示所选的项 ([单击以查看实际尺寸的图像](how-do-i-use-the-combobox-control-vb/_static/image8.png))


组合框用于检索选定的项，在提交窗体之后、 支持与 DropDownList 控件相同的属性：

- SelectedItem.Text-显示选定项的文本属性的值。
- SelectedItem.Value-显示选定项的 Value 属性的值或显示到组合框中键入的文本。
- SelectedValue-与 SelectedItem.Value 相同，只不过此属性使您能够指定默认值 （初始） 选定的项。

如果你键入自定义然后自定义选项在组合框选择分配给 SelectedItem.Text 和 SelectedItem.Value 属性。

## <a name="selecting-the-list-of-items-from-the-database"></a>从数据库中选择的项的列表

你可以从数据库中检索的组合框显示的项的列表。 例如，你可以将组合框绑定到 SqlDataSource 控件、 ObjectDataSource 控件、 LinqDataSource 或 EntityDataSource。

假设你想要在组合框中显示影片列表。 你想要从电影数据库表中检索的影片列表。 请执行这些步骤：

1. 创建一个名为 Movies.aspx 页
2. 将 ScriptManager 控件添加到页面中，通过从 AJAX Extensions 选项卡下 ScriptManager 拖动工具箱拖到页中。
3. 将 ComboBox 控件添加到页面中，通过拖动到该页面上的组合框。
4. 在设计视图中，请将鼠标悬停 ComboBox 控件并选择**选择数据源**任务选项 （请参见图 5）。 数据源配置向导将启动。
5. 在**选择数据源**步骤中，选择&lt;新数据源&gt;选项。
6. 在**选择数据源类型**步骤中，选择数据库。
7. 在**选择你的数据连接**步骤中，选择你的数据库 (例如，MoviesDB.mdf)。
8. 在**将连接字符串保存到应用程序配置文件**步骤中，选择此选项以保存你的连接字符串。
9. 在**配置 Select 语句**步骤，选择电影数据库表并选择所有列。
10. 在**测试查询**步骤中，单击完成按钮。
11. 返回**选择数据源**步骤中，选择要显示的字段的标题列和数据的 Id 列字段 （请参见图）。
12. 单击确定按钮以关闭向导。


[![选择数据源](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)

**图 05**： 选择数据源 ([单击以查看实际尺寸的图像](how-do-i-use-the-combobox-control-vb/_static/image10.png))


[![选择数据的文本和值字段](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)

**图 06**： 选择数据的文本和值字段 ([单击以查看实际尺寸的图像](how-do-i-use-the-combobox-control-vb/_static/image12.png))


完成上述步骤后，组合框绑定到影片数据库表中表示电影 SqlDataSource 控件。 页的源如下所示列出 2 （我清理格式设置得有点）。

**列出 2-Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

请注意，ComboBox 控件 SqlDataSource 控件所指向的 DataSourceID 属性。 当您在浏览器中打开页时，显示从数据库的影片列表 （请参阅图 7）。 你可以任一选取一部电影，从列表或通过键入电影到组合框中输入新的影片。


[![显示影片列表](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)

**图 07**： 显示影片列表 ([单击以查看实际尺寸的图像](how-do-i-use-the-combobox-control-vb/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>设置 DropDownStyle

组合框 DropDownStyle 属性可用于更改组合框的行为。 此属性接受那里可能的值：

- 下拉列表中的 （默认值） 的组合框显示一个下拉列表列出当您单击的箭头，您可以输入自定义值。
- 简单的组合框会自动显示一个下拉列表，你可以输入自定义值。
- DropDownList-ComboBox 作用一样的 DropDownList 控件。

不同的下拉列表中之间和简单时显示的项的列表。 对于简单，立即时将焦点移到组合框显示的列表。 如果下拉列表中，必须单击该箭头可查看的项的列表。

DropDownList 值将导致 ComboBox 控件，就像标准的 DropDownList 控件一样工作。 但是，是一个重要的区别。 控件将显示于前面遮挡它施加任何控制，因此，较旧版本的 Internet Explorer 将显示具有无限 z 索引的 DropDownList 控件。 由于组合框上呈现 HTML &lt;div&gt;而不是 HTML 标记&lt;选择&gt;标记，组合框正确遵循 z 顺序。

## <a name="setting-the-autocompletemode"></a>设置 AutoCompleteMode

组合框 AutoCompleteMode 属性用于指定当有人到组合框中键入文本时，会发生什么情况。 此属性接受以下可能值：

- None-（默认值） 的组合框不提供任何自动完成行为。
- 建议的组合框显示的列表，并会突出显示列表中的匹配项 （请参阅图 8）。
- 追加-组合框不会显示列表和追加的列表拖到键入的内容 （请参阅图 9） 的匹配项。
- SuggestAppend-ComboBox 同时显示的列表，并追加的列表拖到键入的内容 （请参阅图 10） 的匹配项。


[![组合框使建议](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)

**图 08**: ComboBox 使建议 ([单击以查看实际尺寸的图像](how-do-i-use-the-combobox-control-vb/_static/image16.png))


[![组合框将匹配的文本追加](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)

**图 09**: ComboBox 将匹配的文本追加 ([单击以查看实际尺寸的图像](how-do-i-use-the-combobox-control-vb/_static/image18.png))


[![组合框提供的建议，并追加](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)

**图 10**: 组合框提供的建议，并追加 ([单击以查看实际尺寸的图像](how-do-i-use-the-combobox-control-vb/_static/image20.png))


## <a name="summary"></a>摘要

在本教程中，您学习了如何使用 ComboBox 控件来显示一组固定的项。 我们将这两种到的项设置静态和到数据库表该组合框控件绑定。 最后，您学习了如何通过设置其 DropDownStyle 和 AutoCompleteMode 属性修改组合框的行为。

>[!div class="step-by-step"]
[上一篇](how-do-i-use-the-combobox-control-cs.md)
