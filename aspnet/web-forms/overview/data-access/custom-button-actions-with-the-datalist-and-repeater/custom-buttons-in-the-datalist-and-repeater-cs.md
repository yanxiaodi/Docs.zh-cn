---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
title: "DataList 和转发器 (C#) 中的自定义按钮 |Microsoft 文档"
author: rick-anderson
description: "在本教程中，我们将生成一个接口，使用中继器要列出各个类别在系统中，每个类别都提供一个按钮以显示其 associ..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 1f42e332-78dc-438b-9e35-0c97aa0ad929
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 9a072ae18bbb19d086eb825c6e72b68d40b2e429
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="custom-buttons-in-the-datalist-and-repeater-c"></a>DataList 和转发器 (C#) 中的自定义按钮
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_CS.exe)或[下载 PDF](custom-buttons-in-the-datalist-and-repeater-cs/_static/datatutorial46cs1.pdf)

> 在本教程中我们将生成一个接口，使用中继器要列出各个类别在系统中，每个类别都提供一个按钮以显示如何使用其相关的产品。


## <a name="introduction"></a>介绍

在过去的十七 DataList 和转发器教程，我们制作了只读示例和编辑和删除示例。 为了便于编辑和删除于 DataList 中的功能，我们将按钮添加到 DataList s `ItemTemplate` ，单击时，导致回发，并引发与按钮 s 对应 DataList 事件`CommandName`属性。 例如，添加到按钮`ItemTemplate`与`CommandName`编辑属性值将使 DataList s`EditCommand`为回发，则对触发另一个使用`CommandName`删除引发`DeleteCommand`。

除了要编辑和删除按钮，DataList 和转发器控件可以还包括按钮、 LinkButtons 或 ImageButtons，单击时，执行某些自定义服务器端逻辑。 在本教程中我们将生成一个接口，使用中继器列出系统中的类别。 转发器将针对每个类别，包括一个按钮以显示使用如何的产品的类别 （请参见图 1）。


[![单击显示产品链接显示项目符号列表中的类别 s 产品](custom-buttons-in-the-datalist-and-repeater-cs/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image1.png)

**图 1**： 单击显示的产品链接显示项目符号列表中的 s 产品类别 ([单击以查看实际尺寸的图像](custom-buttons-in-the-datalist-and-repeater-cs/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>步骤 1： 添加自定义按钮教程 Web 页

我们探讨如何添加自定义按钮之前，让 s 先花点时间在本教程中我们将需要我们网站项目中创建 ASP.NET 页。 首先，通过添加一个名为的新文件夹`CustomButtonsDataListRepeater`。 接下来，将以下两个 ASP.NET 页添加到该文件夹，并确保将每个包含网页相关联`Site.master`母版页：

- `Default.aspx`
- `CustomButtons.aspx`


![有关自定义的按钮相关教程添加的 ASP.NET 页面](custom-buttons-in-the-datalist-and-repeater-cs/_static/image4.png)

**图 2**： 添加 ASP.NET 页的自定义按钮相关教程


在其他文件夹中，如`Default.aspx`中`CustomButtonsDataListRepeater`文件夹将在其部分中列出这些教程。 回想一下，`SectionLevelTutorialListing.ascx`用户控件提供此功能。 此用户将控件添加到`Default.aspx`通过从解决方案资源管理器拖到页面上的设计视图拖动。


[![SectionLevelTutorialListing.ascx 用户控件添加到 Default.aspx](custom-buttons-in-the-datalist-and-repeater-cs/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image5.png)

**图 3**： 添加`SectionLevelTutorialListing.ascx`用户控件`Default.aspx`([单击以查看实际尺寸的图像](custom-buttons-in-the-datalist-and-repeater-cs/_static/image7.png))


最后，将页添加到的条目为`Web.sitemap`文件。 具体而言，分页和排序后添加以下标记，使用 DataList 和转发器`<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample1.xml)]

在更新后`Web.sitemap`，花些时间查看通过浏览器网站的教程。 在左侧菜单现在包括用于编辑、 插入和删除教程的项。


![站点图现在包括自定义按钮教程的条目](custom-buttons-in-the-datalist-and-repeater-cs/_static/image8.png)

**图 4**： 站点图现在包括自定义按钮教程的条目


## <a name="step-2-adding-the-list-of-categories"></a>步骤 2： 添加类别的列表

我们需要创建中继器，其中列出了所有类别以及显示的产品 LinkButton 本教程中，单击时，项目符号列表中显示的关联的类别的产品。 让我们来首先创建中继器系统中列出的类别的简单。 首先打开`CustomButtons.aspx`页面`CustomButtonsDataListRepeater`文件夹。 从工具箱中拖动到设计器和组拖动中继器其`ID`属性`Categories`。 从转发器 s 智能标记，接下来，创建一个新的数据源控件。 具体而言，创建一个名为的新 ObjectDataSource 控件`CategoriesDataSource`选择从其数据`CategoriesBLL`类的`GetCategories()`方法。


[![配置对象数据源以使用 CategoriesBLL 类的 GetCategories() 方法](custom-buttons-in-the-datalist-and-repeater-cs/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image9.png)

**图 5**： 配置使用 ObjectDataSource`CategoriesBLL`类 s`GetCategories()`方法 ([单击以查看实际尺寸的图像](custom-buttons-in-the-datalist-and-repeater-cs/_static/image11.png))


与 Visual Studio 将为其创建默认的 DataList 控件不同`ItemTemplate`基于数据源，中继器的模板必须手动定义。 此外，必须创建并以声明方式编辑转发器的模板 （即，在该处 s 没有编辑的模板选项中继器 s 智能标记中）。

单击左下角的源选项卡上，然后添加`ItemTemplate`显示中的类别的名称`<h3>`元素和段落中的其描述标记; 包括`SeparatorTemplate`显示水平规则 (`<hr />`) 之间类别。 此外将添加与 LinkButton 其`Text`属性设置为显示产品。 完成这些步骤后，你页面 s 声明性标记应如下所示：


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample2.aspx)]

图 6 显示的页时查看通过浏览器。 列出每个类别名称和描述。 显示产品按钮，单击时，会导致回发，但不执行任何操作。


[![每个类别名称和说明显示，并显示产品 LinkButton](custom-buttons-in-the-datalist-and-repeater-cs/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image12.png)

**图 6**： 每个类别的名称和描述显示，并显示产品 LinkButton ([单击以查看实际尺寸的图像](custom-buttons-in-the-datalist-and-repeater-cs/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>执行服务器端逻辑时显示产品 LinkButton 单击步骤 3:

单击按钮、 LinkButton 或 DataList 或转发器中的模板内的 ImageButton 后，每当产生的回发和 DataList 或转发器的`ItemCommand`事件激发。 除了`ItemCommand`事件，控制可能也会引发其他、 更具针对性的事件，如果 DataList 按钮 s`CommandName`属性设置为的一个保留的字符串 （删除，编辑、 取消、 更新或选择），但`ItemCommand`事件是*始终*激发。

DataList 或转发器中单击按钮时，通常我们需要 （如传递 （在这种情况，可能有多个在控件中，如这两个的编辑按钮和删除按钮） 时单击的按钮和可能的一些其他信息主键值其按钮被单击的项）。 按钮、 LinkButton 和 ImageButton 提供两个属性，其值传递给`ItemCommand`事件处理程序：

- `CommandName`通常用于标识在模板中的每个按钮的字符串
- `CommandArgument`通常用来保存某些数据字段，如的主键值的值

对于此示例中，设置 LinkButton s `CommandName` ShowProducts 和绑定的当前记录 s 主键值的属性`CategoryID`到`CommandArgument`属性使用的数据绑定语法`CategoryArgument='<%# Eval("CategoryID") %>'`。 指定这两个属性后, LinkButton s 声明性语法应如下所示：


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample3.aspx)]

当单击该按钮，产生的回发和 DataList 或转发器的`ItemCommand`事件激发。 事件处理程序传递按钮 s`CommandName`和`CommandArgument`值。

为转发器 s 创建事件处理程序`ItemCommand`事件并记下第二个参数传递到事件处理程序 (名为`e`)。 此第二个参数属于类型[ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx)并具有以下四个属性：

- `CommandArgument`值被单击的按钮的`CommandArgument`属性
- `CommandName`按钮的值`CommandName`属性
- `CommandSource`对被单击的按钮控件的引用
- `Item`对引用[ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx)包含被单击的按钮; 绑定到中继器每个记录显示为`RepeaterItem`

由于所选类别 s`CategoryID`通过传入`CommandArgument`属性，我们可以获取与所选类别中关联的产品集`ItemCommand`事件处理程序。 然后，这些产品可以绑定到中的如何控件`ItemTemplate`(哪些我们遇到有待添加)。 所有保持，，然后是添加如何，都引用在`ItemCommand`事件处理程序，并将绑定到它的一套所选类别，在步骤 4 中，我们将介绍的产品。

> [!NOTE]
> DataList s`ItemCommand`事件处理程序传递的类型对象[ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx)，它提供与相同的四个属性`RepeaterCommandEventArgs`类。


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>步骤 4： 在项目符号列表中显示所选的类别的产品

所选的类别的产品可能显示在转发器的`ItemTemplate`使用任意数量的控件。 我们无法添加另一个嵌套中继器、 DataList、 DropDownList、 GridView，等等。 由于我们想要为项目符号列表中显示的产品，不过，我们将使用如何控制。 返回到`CustomButtons.aspx`页上 s 声明性标记，如何将控件添加到`ItemTemplate`后显示产品 LinkButton。 设置 BulletedLists s`ID`到`ProductsInCategory`。 如何显示通过指定数据字段的值`DataTextField`属性; 因为此控件将有产品信息绑定到它时，请设置`DataTextField`属性`ProductName`。


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample4.aspx)]

在`ItemCommand`事件处理程序中，引用此控件使用`e.Item.FindControl("ProductsInCategory")`并将其绑定到与所选类别关联的产品集。


[!code-csharp[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample5.cs)]

在执行中的任何操作之前`ItemCommand`事件处理程序，它首先检查传入的值比较明智的做法 s `CommandName`。 由于`ItemCommand`事件处理程序，则会激发*任何*单击按钮时，如果模板使用中有多个按钮`CommandName`值以便辨别要执行的操作。 检查`CommandName`此处毫无意义，因为只有一个按钮，但它是一个好习惯到窗体。 接下来，`CategoryID`检索所选类别的从`CommandArgument`属性。 在模板中的如何控件然后引用并绑定到的结果`ProductsBLL`类的`GetProductsByCategoryID(categoryID)`方法。

在前面的教程，如使用内 DataList 按钮[概述的编辑和删除数据中 DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)，我们确定通过给定项的主键值`DataKeys`集合。 虽然此方法适用于 DataList，中继器没有`DataKeys`属性。 相反，我们必须使用另一种方法提供的主键值，如通过按钮的`CommandArgument`属性或将主键值分配到隐藏标签 Web 控件模板内，并读取其值进来`ItemCommand`事件处理程序使用`e.Item.FindControl("LabelID")`。

完成后`ItemCommand`事件处理程序，需要一段时间来测试此页在浏览器。 如图 7 所示，单击显示产品链接导致回发并中如何显示所选类别的产品。 此外，请注意，此产品信息将保持，即使在单击其他类别显示产品链接。

> [!NOTE]
> 如果你想要修改此报表的行为，这样只有一个类别的产品列出一次，只需设置如何控制 s`EnableViewState`属性`False`。


[![如何用来显示所选分类的产品](custom-buttons-in-the-datalist-and-repeater-cs/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image15.png)

**图 7**: 如何用来显示所选分类的产品 ([单击以查看实际尺寸的图像](custom-buttons-in-the-datalist-and-repeater-cs/_static/image17.png))


## <a name="summary"></a>摘要

DataList 和转发器控件可以包含在其模板中任意数量的按钮、 LinkButtons 或 ImageButtons。 此类按钮，单击时，会导致回发和引发`ItemCommand`事件。 若要将自定义服务器端操作相关联与所单击的按钮，创建的事件处理程序`ItemCommand`事件。 在此事件处理程序首先检查传入`CommandName`值以确定被单击的按钮。 其他信息 （可选） 可以提供通过按钮的`CommandArgument`属性。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Dennis Patterson。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[下一篇](custom-buttons-in-the-datalist-and-repeater-vb.md)
