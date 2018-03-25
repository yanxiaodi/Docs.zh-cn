---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
title: "嵌套的数据 Web 控件 (VB) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将探讨如何使用中继器嵌套在另一个转发器。 示例将演示如何以填充这两个 d 的内部中继器..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 8b7fcf7b-722b-498d-a4e4-7c93701e0c95
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 944f208d6fe4f9fde13b530fb236ecc69ff5e9cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="nested-data-web-controls-vb"></a>嵌套的数据 Web 控件 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_VB.exe)或[下载 PDF](nested-data-web-controls-vb/_static/datatutorial32vb1.pdf)

> 在本教程中我们将探讨如何使用中继器嵌套在另一个转发器。 示例将演示如何同时以声明方式和以编程方式填充内部的转发器。


## <a name="introduction"></a>介绍

除了静态 HTML 和数据绑定语法中，模板还可以包含 Web 控件和用户控件。 这些 Web 控件可以有其属性分配通过声明性、 数据绑定语法，或可在相应的服务器端事件处理程序以编程方式访问。

通过将嵌入在模板中的控件，可以自定义和改进后的外观和用户体验。 例如，在[GridView 控件中使用 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md)教程，我们已了解如何通过添加一个日历控件中以显示雇员雇佣日期为 TemplateField; 自定义 GridView 的显示[添加验证控件的编辑和插入接口到](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)和[自定义的数据修改界面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)教程，我们已了解如何自定义编辑和插入接口通过添加验证控件、 文本框、 DropDownLists 和其他 Web 控件。

模板还可以包含其他数据 Web 控件。 这就是，我们可以具有其模板中包含另一个 DataList （或转发器或 GridView 或说明，依次类推） DataList。 与此类接口的挑战适当的数据绑定到 Web 控件的内部数据。 有几种不同方法可用，范围从使用到编程的对象数据源的声明性选项。

在本教程中我们将探讨如何使用中继器嵌套在另一个转发器。 外部转发器将包含在数据库中，每个类别的项显示的类别的名称和描述。 每个类别项 s 内部转发器将显示属于该类别的每个产品信息 （请参见图 1） 中的项目符号列表。 我们的示例将说明如何同时以声明方式和以编程方式填充内部的转发器。


[![列出其产品，每个类别](nested-data-web-controls-vb/_static/image2.png)](nested-data-web-controls-vb/_static/image1.png)

**图 1**： 每个类别，以及其产品，列出 ([单击以查看实际尺寸的图像](nested-data-web-controls-vb/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>步骤 1： 创建类别列表

当生成使用的页嵌套数据 Web 控件时，我有所设计、 创建和测试最外面的数据的 Web 控件第一次，而无需甚至担心内部嵌套的控件。 因此，让我们来开始通过浏览到列出的名称和描述每个类别的页面添加中继器所必需的步骤。

首先打开`NestedControls.aspx`页面`DataListRepeaterBasics`文件夹并将重复器控件添加到页上，设置其`ID`属性`CategoryList`。 从转发器 s 智能标记中，选择创建名为新 ObjectDataSource `CategoriesDataSource`。


[![命名新 ObjectDataSource CategoriesDataSource](nested-data-web-controls-vb/_static/image5.png)](nested-data-web-controls-vb/_static/image4.png)

**图 2**： 命名新 ObjectDataSource `CategoriesDataSource` ([单击以查看实际尺寸的图像](nested-data-web-controls-vb/_static/image6.png))


配置对象数据源，以便它将从其数据`CategoriesBLL`类的`GetCategories`方法。


[![配置对象数据源以使用 CategoriesBLL 类的 GetCategories 方法](nested-data-web-controls-vb/_static/image8.png)](nested-data-web-controls-vb/_static/image7.png)

**图 3**： 配置使用 ObjectDataSource`CategoriesBLL`类 s`GetCategories`方法 ([单击以查看实际尺寸的图像](nested-data-web-controls-vb/_static/image9.png))


若要指定中继器的模板内容我们需要转至源视图，手动输入的声明性语法。 添加`ItemTemplate`显示中的类别的名称`<h4>`元素和段落元素中的类别的说明 (`<p>`)。 此外，让 s 分隔每个类别与水平规则 (`<hr>`)。 进行这些更改后你页面应类似于以下的 ObjectDataSource 转发器以及包含声明性语法：


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample1.aspx)]

图 4 显示我们通过浏览器查看时的进度。


[![每个类别名称和描述列出了，隔开水平标尺](nested-data-web-controls-vb/_static/image11.png)](nested-data-web-controls-vb/_static/image10.png)

**图 4**： 每个类别的名称和描述列出了，之间用水平标尺分隔 ([单击以查看实际尺寸的图像](nested-data-web-controls-vb/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>步骤 2： 添加嵌套的产品中继器

列出完成类别，我们的下一个任务是添加到中继器`CategoryList`s`ItemTemplate`显示有关这些产品属于相应的类别的信息。 有多种方式我们可以为此内部的转发器，其中两个我们很快就会检索数据。 现在，让 s 只需创建转发器的产品内`CategoryList`中继器的`ItemTemplate`。 具体来说，让具有产品符号列表中的每个产品与每个列表项包括产品的名称和价格的转发器显示的 s。

若要创建此转发器，我们需要手动输入内部转发器 s 声明性语法和模板到`CategoryList`s `ItemTemplate`。 添加以下标记内的`CategoryList`中继器的`ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>步骤 3： 绑定到 ProductsByCategoryList 转发器的特定于类别的产品

如果此时访问通过浏览器页面，你的屏幕将图与图 4 中的相同因为我们已尚未将任何数据绑定到转发器。 有几种方法，我们可以通过抓取适当的产品记录和将其绑定到转发器，它比其他一些更高效。 此处的主要难题获取相应的产品为指定类别。

要将绑定到内部的转发器控件的数据可以访问以声明方式，通过在 ObjectDataSource`CategoryList`中继器的`ItemTemplate`，或通过编程方式，从 ASP.NET 页的代码隐藏页。 同样，此数据可绑定到内部中继器或者以声明方式-通过内部中继器 s`DataSourceID`属性或通过声明性数据绑定语法或以编程方式通过引用在内部中继器`CategoryList`中继器 s`ItemDataBound`事件处理程序，以编程方式设置其`DataSource`属性，并调用其`DataBind()`方法。 让我们来探讨每一个这些方法。

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>使用访问数据以声明方式 ObjectDataSource 控件和`ItemDataBound`事件处理程序

因为我们已使用在本系列教程的此示例继续使用对象数据源访问数据的最简单选择整个广泛 ObjectDataSource。 `ProductsBLL`类具有`GetProductsByCategoryID(categoryID)`返回属于指定这些产品有关的信息的方法 *`categoryID`* 。 因此，我们可以将添加到 ObjectDataSource`CategoryList`中继器的`ItemTemplate`和将其配置为从此类的方法访问其数据。

遗憾的是，转发器不允许其模板编辑通过设计视图中，因此我们需要手动添加此 ObjectDataSource 控件的声明性语法。 下面的语法演示`CategoryList`中继器 s`ItemTemplate`添加此新对象数据源后 (`ProductsByCategoryDataSource`):


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample3.aspx)]

我们使用 ObjectDataSource 方法时需要设置`ProductsByCategoryList`中继器 s`DataSourceID`属性`ID`的 ObjectDataSource (`ProductsByCategoryDataSource`)。 此外，请注意，我们 ObjectDataSource 具有`<asp:Parameter>`指定的元素 *`categoryID`* 值将传递到`GetProductsByCategoryID(categoryID)`方法。 但是，我们如何指定此值？ 理想情况下，我们 d 能够只需设置`DefaultValue`属性`<asp:Parameter>`元素使用数据绑定的语法，如下所示：


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample4.aspx)]

遗憾的是，数据绑定语法才有效中具有的控件`DataBinding`事件。 `Parameter`的类没有此类事件，并因此上面的语法是非法的将导致运行时错误。

若要设置此值，我们需要创建的事件处理程序`CategoryList`中继器的`ItemDataBound`事件。 回想一下，`ItemDataBound`事件触发一次每个项绑定到转发器。 因此，对于外部转发器会激发此事件每次我们可以分配当前`CategoryID`值赋给`ProductsByCategoryDataSource`ObjectDataSource 的`CategoryID`参数。

创建的事件处理程序`CategoryList`中继器的`ItemDataBound`事件替换为以下代码：


[!code-vb[Main](nested-data-web-controls-vb/samples/sample5.vb)]

通过确保我们重新处理数据项而不是页眉、 页脚或分隔符项启动此事件处理程序。 接下来，我们引用的实际`CategoriesRow`刚刚已绑定到当前的实例`RepeaterItem`。 最后，我们引用中的 ObjectDataSource`ItemTemplate`并分配其`CategoryID`参数值以`CategoryID`的当前`RepeaterItem`。

与此事件处理程序中，`ProductsByCategoryList`在每个转发器`RepeaterItem`绑定到这些产品`RepeaterItem`的类别。 图 5 显示生成的输出的屏幕截图。


[![外部中继器列出每个类别;内部一个列出该类别的产品](nested-data-web-controls-vb/_static/image14.png)](nested-data-web-controls-vb/_static/image13.png)

**图 5**: 外部中继器列出每个类别; 内部一个列出该类别的产品 ([单击以查看实际尺寸的图像](nested-data-web-controls-vb/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>以编程方式访问通过类别数据的产品

而不是使用对象数据源检索当前的类别的产品，我们可以创建一种方法，在我们 ASP.NET 页的代码隐藏类 (或在`App_Code`文件夹或单独的类库项目中) 返回的相应的一组当传入的产品`CategoryID`。 假设我们在我们 ASP.NET 页的代码隐藏类有这样的方法，该主题名为`GetProductsInCategory(categoryID)`。 使用此方法就地我们无法将当前的类别的产品绑定到内部的转发器，使用下面的声明性语法：


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample6.aspx)]

转发器 s`DataSource`属性使用的数据绑定语法来指示其数据来自于`GetProductsInCategory(categoryID)`方法。 由于`Eval("CategoryID")`返回类型的值`Object`，我们将对象转换为`Integer`然后再将传递到`GetProductsInCategory(categoryID)`方法。 请注意，`CategoryID`访问下面的语法是通过数据绑定`CategoryID`中*外部*转发器 (`CategoryList`)，一个 s 绑定到的记录`Categories`表。 因此，我们知道`CategoryID`不能为数据库`NULL`值，该值是我们可以隐式强制转换的原因`Eval`方法而不会检查如果我们重新处理`DBNull`。

使用此方法，我们需要创建`GetProductsInCategory(categoryID)`方法并让它检索相应的一组给定所提供的产品 *`categoryID`* 。 我们可以执行此操作通过只需返回`ProductsDataTable`返回`ProductsBLL`类的`GetProductsByCategoryID(categoryID)`方法。 让我们来创建`GetProductsInCategory(categoryID)`方法中的代码隐藏类我们`NestedControls.aspx`页。 此使用下面的代码：


[!code-vb[Main](nested-data-web-controls-vb/samples/sample7.vb)]

此方法只需创建的一个实例`ProductsBLL`方法并返回的结果`GetProductsByCategoryID(categoryID)`方法。 请注意该方法必须标记为`Public`或`Protected`; 如果该方法标记为`Private`，它将不能从 ASP.NET 页 s 声明性标记访问。

进行这些更改，以使用此新方法后，花些时间查看通过浏览器的页。 使用对象数据源时，输出应为相同到输出和`ItemDataBound`事件处理程序方法 （回头参考图 5 显示屏幕截图中）。

> [!NOTE]
> 它可能看上去东西而加以排斥创建`GetProductsInCategory(categoryID)`在 ASP.NET 页的代码隐藏类的方法。 毕竟，此方法只需创建的一个实例`ProductsBLL`类，并返回的结果其`GetProductsByCategoryID(categoryID)`方法。 为什么不只需调用此方法直接从内部转发器中的数据绑定语法类似： `DataSource='<%# ProductsBLL.GetProductsByCategoryID(CType(Eval("CategoryID"), Integer)) %>'`？ 尽管此语法获胜 t 的当前实现使用`ProductsBLL`类 (由于`GetProductsByCategoryID(categoryID)`方法是实例方法)，你可以修改`ProductsBLL`包括一个静态`GetProductsByCategoryID(categoryID)`方法或具有包括静态类`Instance()`方法返回的新实例`ProductsBLL`类。


虽然此类修改将消除对需要`GetProductsInCategory(categoryID)`在 ASP.NET 页的代码隐藏类的方法，代码隐藏类方法会为我们提供更灵活地处理与数据检索，正如我们很快将看到。

## <a name="retrieving-all-of-the-product-information-at-once"></a>检索所有产品信息一次

两个早期技术我们检查已通过调用获取这些类别的产品的当前`ProductsBLL`类 s`GetProductsByCategoryID(categoryID)`方法 (第一种方法之所以这样做是通过对象数据源，通过第二个`GetProductsInCategory(categoryID)`中的方法代码隐藏类）。 每次调用此方法时，给数据访问层，业务逻辑层调用的查询返回中的行的 SQL 语句的数据库`Products`表`CategoryID`字段与匹配提供的输入的参数。

给定*N*系统中的类别，这种方法网*N* + 1 调用数据库一个数据库查询来获取所有类别，然后*N*调用以获取产品特定于每个类别。 但是，我们可以检索仅使用两个数据库调用一次调用，若要获取所有类别，另一个用于获取所有产品的所有所需的数据。 一旦我们拥有的所有产品，我们可以因此筛选这些产品仅匹配当前的产品`CategoryID`绑定到该类别 s 内部转发器。

若要提供此功能，我们只需进行需要稍做修改`GetProductsInCategory(categoryID)`在我们 ASP.NET 页的代码隐藏类的方法。 而不会盲目地返回的结果`ProductsBLL`类 s`GetProductsByCategoryID(categoryID)`方法，我们可以改为第一次访问*所有*的产品 (如果它们没有 t 已访问)，然后返回的只是筛选的视图产品基于传入的`CategoryID`。


[!code-vb[Main](nested-data-web-controls-vb/samples/sample8.vb)]

请注意页面级别变量中，添加`allProducts`。 这包含有关的所有产品的信息，而且填充第一次`GetProductsInCategory(categoryID)`调用方法。 后确保`allProducts`、 创建对象并将其填充方法选项 DataTable 的结果进行了筛选，以便只有行其`CategoryID`匹配所指定`CategoryID`可访问。 此方法可以降低从将访问该数据库的次数*N* + 1 到两个。

此增强功能不会引入到页上，呈现标记的任何更改也不会使其他方法相比，返回更少的记录。 它只是减少了对数据库的调用数。

> [!NOTE]
> 一个直观的方式可能原因减少的数据库访问次数会肯定提高性能。 但是，这可能不是这种情况。 如果你有大量的产品其`CategoryID`是`NULL`，示例中，则调用`GetProducts`方法返回的产品的永远不会显示一个数字。 此外，返回所有产品可以都是一种浪费如果你重新仅显示了类别，这可能都是这种情况，如果已实现分页的子集。


如往常一样，当涉及到分析的两种技术的性能，仅有效的度量值是运行的应用程序 s 常见案例方案而量身定制的受控的测试。

## <a name="summary"></a>摘要

在本教程中我们已了解如何嵌套一个数据在另一个，Web 控件专门检查如何让显示每个类别项列出符号列表中的每个类别的产品内部的转发器与外部转发器。 生成嵌套的用户界面中的主要挑战在于访问并将正确的数据绑定到内部数据 Web 控件。 有多种技术可用，我们在本教程中检查其中有 2 个。 检查第一种方法中的外部数据 Web 控件 s 使用 ObjectDataSource `ItemTemplate` ，已绑定到内部数据 Web 控制通过其`DataSourceID`属性。 第二种方法访问的数据通过在 ASP.NET 页的代码隐藏类的方法。 然后，此方法可以绑定到内部数据 Web 控件的`DataSource`通过数据绑定语法的属性。

尽管检查在本教程中的嵌套的用户接口使用中继器嵌套在中继器，这些技术可以扩展到其他数据 Web 控件。 可以嵌套在一个 GridView 或 DataList 内的 GridView 中继器等。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Zack Jones 和沈 Shulok。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一篇](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
