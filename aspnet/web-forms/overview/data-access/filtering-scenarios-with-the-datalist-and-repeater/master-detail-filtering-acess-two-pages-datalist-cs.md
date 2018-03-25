---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
title: "跨两个页 (C#) 筛选主/详细信息 |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们了解如何在两个页上分隔母版/详细信息报告。 在 'master' 页我们使用重复器控件来呈现的 categ 列表..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2010
ms.topic: article
ms.assetid: 68b8c023-92fa-4df6-9563-1764e16e4b04
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: bb86db509ca26dde0c24341dee402e7af4355507
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-across-two-pages-c"></a>大纲/细节筛选跨两个页 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_CS.exe)或[下载 PDF](master-detail-filtering-acess-two-pages-datalist-cs/_static/datatutorial34cs1.pdf)

> 在本教程中我们了解如何在两个页上分隔母版/详细信息报告。 在"母版页"我们可以使用重复器控件来呈现类别的列表，当单击，会使用户进入"详细信息"页其中两个列 DataList 显示属于所选类别这些产品。


## <a name="introduction"></a>介绍

在[前面教程](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)我们已了解如何在使用 DropDownLists 以显示"主"的记录和显示的"详细信息。"DataList 单个 web 页中显示主/详细信息报表 用于主/详细信息报表的另一种常见模式是具有一个网页上的主记录和在另一台的详细信息。 在早期版本[主/详细信息筛选跨两个页](../masterdetail/master-detail-filtering-across-two-pages-cs.md)教程中，我们将检查此模式使用一个 GridView 显示系统中所有供应商。 此 GridView 包含 HyperLinkField，呈现为指向第二页，传递的`SupplierID`中查询字符串。 使用一个 GridView，第二个页列出所选供应商提供的这些产品。

可以使用 DataList 和转发器控件来完成此类的两页主/详细信息报表。 唯一的区别在于，DataList 和转发器都不提供支持 HyperLinkField 控件。 相反，我们必须添加超链接 Web 控件或定位 HTML 元素 (`<a>`) 中控件的`ItemTemplate`。 超链接的`NavigateUrl`属性或定位点的`href`属性可以然后使用自定义声明性或以编程方式的方法。

在本教程中我们将探讨一个示例，其中列出了使用转发器控件的一个页上的项目符号列表中的类别。 每个列表项将包括该类别的名称和描述，具有到第二个页面的链接的形式显示的类别名称。 单击此链接将 whisk 到第二个页面中，用户 DataList 其中将显示属于所选类别这些产品。

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>步骤 1： 在项目符号列表中显示的类别

创建任何主/详细信息报表的第一步是首先显示的"主"的记录。 因此，我们第一个任务是在"主"页上显示类别。 打开`CategoryListMaster.aspx`页面`DataListRepeaterFiltering`文件夹中，添加转发器控件，，和从智能标记中，选择要添加新对象数据源。 配置新对象数据源，以便访问其数据从`CategoriesBLL`类的`GetCategories`方法 （请参见图 1）。


[![配置对象数据源以使用 CategoriesBLL 类 GetCategories 方法](master-detail-filtering-acess-two-pages-datalist-cs/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image1.png)

**图 1**： 配置使用 ObjectDataSource`CategoriesBLL`类的`GetCategories`方法 ([单击以查看实际尺寸的图像](master-detail-filtering-acess-two-pages-datalist-cs/_static/image3.png))


接下来，定义转发器的模板，以便它将显示每个类别名称和描述，并为项目符号列表中的项。 让我们尚不担心具有每个类别的详细信息页的链接。 下面显示的转发器和对象数据源中的声明性的标记：

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample1.aspx)]

完成此标记中，与需要花费一段时间来查看我们通过浏览器的进度。 如图 2 所示，转发器将呈现为一个项目符号列表，显示每个类别的名称和描述。


[![每个类别显示为项目符号列表项](master-detail-filtering-acess-two-pages-datalist-cs/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image4.png)

**图 2**： 每个类别显示为项目符号列表项 ([单击以查看实际尺寸的图像](master-detail-filtering-acess-two-pages-datalist-cs/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>步骤 2： 将转换的详细信息页的链接的类别名称

若要允许的用户以显示给定的类别中的"详细信息"信息，我们需要添加到每个项目符号列表项，如果单击，则将需要对用户发出的第二页的链接 (`ProductsForCategoryDetails.aspx`)。 然后，此第二个页面将显示使用 DataList 所选类别的产品。 为了确定被单击其链接的类别，我们需要通过单击的类别`CategoryID`通过某种机制的第二页。 将标量数据从一页传输到另一个的最简单、 最简单方法是通过查询字符串，我们将在本教程中使用此选项。 具体而言，`ProductsForCategoryDetails.aspx`页应所选 *`categoryID`* 要先通过名为的查询字符串字段值`CategoryID`。 例如，若要查看饮料类别的产品具有`CategoryID`为 1，用户需要访问`ProductsForCategoryDetails.aspx?CategoryID=1`。

若要在转发器，我们需要添加超链接 Web 控件或 HTML 定位点元素中创建每个项目符号列表项的超链接 (`<a>`) 到`ItemTemplate`。 中的超链接的方案显示为每个行相同，这两种方法就足够了。 为重复字符，但我更喜欢使用定位元素。 若要使用的定位点元素，请更新到的转发器的 ItemTemplate:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample2.aspx)]

请注意，`CategoryID`可直接在定位元素内插入`href`属性; 不过，若要执行因此请务必分隔`href`属性的值，该值撇号 （，注意引号），因为`Eval`方法在`href`属性分隔其字符串 (`"CategoryID"`) 用引号引起来。 或者，可以改为使用超链接 Web 控件：

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample3.aspx)]

请注意如何 URL 的静态部分 — `ProductsForCategoryDetails.aspx?CategoryID` -追加到的结果`Eval("CategoryID")`直接内使用字符串串联的数据绑定语法。

使用超链接控件的一个优点是，它可以以编程方式访问从转发器的`ItemDataBound`事件处理程序，如果需要。 例如，你可能想要为文本而不是与任何关联的产品类别的链接的形式显示的类别名称。 无法在以编程方式执行此类检查`ItemDataBound`事件处理程序; 对于不带类别关联的产品，超链接`NavigateUrl`属性无法设置为空字符串，从而导致该特定的类别名称呈现为纯文本 （而不是一个链接）。 将回指[格式设置的 DataList 和数据后转发器基于](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md)有关格式设置 DataList 和转发器的内容的详细信息的教程基于通过编程逻辑`ItemDataBound`事件处理程序。

如果内容的观众，随意使用你在页面中的定位元素或超链接控件方法。 无论方法中，查看通过浏览器的每个类别名称应呈现为链接到网页时`ProductsForCategoryDetails.aspx`，并传入适用`CategoryID`值 （请参见图 3）。


[![类别名称现在将链接到 ProductsForCategoryDetails.aspx](master-detail-filtering-acess-two-pages-datalist-cs/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image7.png)

**图 3**: 类别名称现在指向`ProductsForCategoryDetails.aspx`([单击以查看实际尺寸的图像](master-detail-filtering-acess-two-pages-datalist-cs/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>步骤 3： 列出属于所选类别的产品

与`CategoryListMaster.aspx`完成的页上，我们已准备好将实现"详细信息"页中，我们工作重心`ProductsForCategoryDetails.aspx`。 打开此页，将 DataList 拖动从工具箱中拖动到设计器中，并设置其`ID`属性`ProductsInCategory`。 接下来，从 DataList 的智能标记选择将新对象数据源添加到页上，其命名为`ProductsInCategoryDataSource`。 将其配置，以便它调用`ProductsBLL`类的`GetProductsByCategoryID(categoryID)`方法; 设置为 （无） INSERT、 UPDATE 和 DELETE 选项卡中列出的下拉列表。


[![配置对象数据源以使用 ProductsBLL 类 GetProductsByCategoryID(categoryID) 方法](master-detail-filtering-acess-two-pages-datalist-cs/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image10.png)

**图 4**： 配置使用 ObjectDataSource`ProductsBLL`类的`GetProductsByCategoryID(categoryID)`方法 ([单击以查看实际尺寸的图像](master-detail-filtering-acess-two-pages-datalist-cs/_static/image12.png))


由于`GetProductsByCategoryID(categoryID)`方法接受一个输入的参数 (*`categoryID`*)，选择数据源向导为我们提供了机会指定参数的源。 将参数源设置为查询字符串使用 QueryStringField `CategoryID`。


[![使用作为参数的源的查询字符串字段 CategoryID](master-detail-filtering-acess-two-pages-datalist-cs/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image13.png)

**图 5**： 使用查询字符串字段`CategoryID`作为参数的源 ([单击以查看实际尺寸的图像](master-detail-filtering-acess-two-pages-datalist-cs/_static/image15.png))


正如我们已完成选择数据源向导后看到在上一个教程中，Visual Studio 将自动创建`ItemTemplate`为 DataList，其中列出了每个数据字段名称和值。 替换此模板，其中列出了仅产品的名称、 供应商和价格。 此外，还要设置 DataList`RepeatColumns`属性设置为 2。 这些更改后 DataList 和对象数据源的声明性标记应看起来类似以下：

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample4.aspx)]

若要在操作中查看该页，从启动`CategoryListMaster.aspx`页; 接下来，单击类别项目符号列表中的链接。 这样你将转到`ProductsForCategoryDetails.aspx`，并传递沿`CategoryID`通过查询字符串。 `ProductsInCategoryDataSource`中的 ObjectDataSource`ProductsForCategoryDetails.aspx`将仅对这些产品获取指定的类别并在 DataList，这使这两种产品每行中显示它们。 图 6 显示的屏幕截图`ProductsForCategoryDetails.aspx`查看饮料时。


[![显示饮料，每行的两个](master-detail-filtering-acess-two-pages-datalist-cs/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image16.png)

**图 6**： 显示饮料，每行的两个 ([单击以查看实际尺寸的图像](master-detail-filtering-acess-two-pages-datalist-cs/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>步骤 4： 在 ProductsForCategoryDetails.aspx 上显示类别信息

当用户单击中的类别`CategoryListMaster.aspx`，就会进入`ProductsForCategoryDetails.aspx`和显示属于所选类别的产品。 但是，在`ProductsForCategoryDetails.aspx`有关于选择哪种类别没有视觉提示。 用户应单击饮料，但会意外地单击的调味品还没有的方法的意识到他们的错误操作，一旦达到`ProductsForCategoryDetails.aspx`。 若要缓解此潜在问题，我们可以显示有关所选类别的信息 — 其名称和说明-顶部`ProductsForCategoryDetails.aspx`页。

若要完成此操作，将添加在转发器控件中的上方 FormView `ProductsForCategoryDetails.aspx`。 接下来，从名为 FormView 的智能标记将新对象数据源添加到页`CategoryDataSource`和将其配置为使用`CategoriesBLL`类的`GetCategoryByCategoryID(categoryID)`方法。


[![有关通过 CategoriesBLL 类 GetCategoryByCategoryID(categoryID) 方法的类别的访问信息](master-detail-filtering-acess-two-pages-datalist-cs/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image19.png)

**图 7**： 访问有关通过类别信息`CategoriesBLL`类的`GetCategoryByCategoryID(categoryID)`方法 ([单击以查看实际尺寸的图像](master-detail-filtering-acess-two-pages-datalist-cs/_static/image21.png))


与`ProductsInCategoryDataSource`ObjectDataSource 添加在步骤 3 中，`CategoryDataSource`的配置数据源向导将提示我们提供的源`GetCategoryByCategoryID(categoryID)`方法的输入参数。 使用完全相同的设置为之前，将参数源设置为查询字符串和 QueryStringField 值赋给`CategoryID`（回头参考图 5 中）。

完成向导后，Visual Studio 自动创建`ItemTemplate`， `EditItemTemplate`，和`InsertItemTemplate`FormView 有关。 由于我们提供只读接口，请尝试删除`EditItemTemplate`和`InsertItemTemplate`。 此外，随意自定义 FormView `ItemTemplate`。 删除多余的模板和自定义 ItemTemplate 之后, 你 FormView 和对象数据源的声明性标记应如下类似于以下：

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample5.aspx)]

图 8 显示屏幕截图时查看通过浏览器的此页。

> [!NOTE]
> 除了 FormView，我还添加了上方 FormView 需要用户定向回到类别列表中的一个超链接控件 (`CategoryListMaster.aspx`)。 随意放置此链接在其他位置，或完全忽略。


[![类别信息是现在显示在页面顶部](master-detail-filtering-acess-two-pages-datalist-cs/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image22.png)

**图 8**： 类别信息是现在显示在页面顶部 ([单击以查看实际尺寸的图像](master-detail-filtering-acess-two-pages-datalist-cs/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>步骤 5： 将显示一条消息，如果要安装的产品属于所选类别

`CategoryListMaster.aspx`页列出所有类别在系统中，而不考虑是否有任何关联的产品。 如果用户单击与任何关联的产品，在 DataList 类别`ProductsForCategoryDetails.aspx`将不会呈现，因为其数据源将不具有任何项。 GridView 如我们所见过去教程中，提供`EmptyDataText`属性，可以用于指定要显示在其数据源中不有任何记录的文本消息。 遗憾的是，DataList 和转发器都不具有此类的属性。

为了显示消息，告知用户，所选类别不匹配的产品，我们需要添加一个标签控件到页`Text`属性分配要显示的事件中有不匹配的产品的消息。 我们然后需要以编程方式设置其`Visible`属性基于 DataList 是否包含任何项。

若要完成此操作，首先添加下 DataList 标签。 设置其`ID`属性`NoProductsMessage`及其`Text`属性设置为"There are 不到所选的类别的产品" 接下来，我们需要以编程方式设置此标签的`Visible`属性基于是否任何数据被绑定到`ProductsInCategory`DataList。 数据已绑定到 DataList 之后，必须进行此分配。 对于 GridView，说明如何和 FormView，我们无法创建控件的事件处理程序`DataBound`触发后数据绑定已完成的事件。 但是，DataList 和转发器都不具有`DataBound`事件可用。

对于此特定示例中，我们可以分配标签的`Visible`中的属性`Page_Load`事件处理程序，因为数据将已分配给页面的之前 DataList`Load`事件。 但是，此方法将无法在一般情况下，按工作对象数据源中的数据可能会绑定到更高版本中页面的生命周期 DataList。 例如，如果显示的数据基于另一个控件中的值，例如显示主/详细信息报表使用的 DropDownList 来保存的"主"的记录时，数据可能不重新绑定到数据的 Web 控件之前`PreRender`阶段中页面的生命周期。

该值将适用于所有情况下的一种解决方案是将分配`Visible`属性`False`中 DataList `ItemDataBound` (或`ItemCreated`) 事件处理程序绑定的项类型时`Item`或`AlternatingItem`。 在这种情况下我们知道是否有至少一个数据在数据源中项，并因此可以隐藏`NoProductsMessage`标签。 除了此事件处理程序中，我们还需要一个事件处理程序用于 DataList`DataBinding`事件，其中我们初始化标签的`Visible`属性`True`。 由于`DataBinding`之前激发事件`ItemDataBound`事件，标签`Visible`属性最初将设置为`True`; 如果没有任何数据项目，但是，它将设置为`False`。 下面的代码实现此逻辑：

[!code-csharp[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample6.cs)]

所有 Northwind 数据库中的类别对一个或多个产品均相关联。 若要测试此功能，我已手动调整 Northwind 数据库为本教程中，重新分配与生成类别关联的所有产品 (`CategoryID` = 7) 到数据项类别 (`CategoryID` = 8)。 这可以实现从服务器资源管理器通过选择新查询，并使用以下`UPDATE`语句：

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample7.sql)]

相应地更新数据库之后, 返回到`CategoryListMaster.aspx`页上，单击生成链接。 因为不再有任何属于生成类别的产品，你应该会看到"There are 不到所选的类别的产品" 消息，如图 9 中所示。


[![如果有任何产品属于所选分类，显示一条消息](master-detail-filtering-acess-two-pages-datalist-cs/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image25.png)

**图 9**： 如果有任何产品属于所选分类将显示消息 ([单击以查看实际尺寸的图像](master-detail-filtering-acess-two-pages-datalist-cs/_static/image27.png))


## <a name="summary"></a>摘要

尽管主/详细信息报表可以显示 master 和详细信息记录在一页，在很多网站中它们被分隔跨两个 web 页。 在本教程中我们介绍了如何通过将"主"网页中使用中继器的项目符号列表中列出的类别和"详细信息"页中列出的关联的产品实现主/详细信息报表。 母板网页中的每个列表项包含指向传递的行的详细信息页面的`CategoryID`值。

在详细信息页中指定的提供程序检索这些产品完成通过`ProductsBLL`类的`GetProductsByCategoryID(categoryID)`方法。  *`categoryID`* 使用以声明方式指定参数值`CategoryID`作为参数源的查询字符串值。 我们还了解了如何在使用 FormView 的详细信息页中显示类别详细信息以及如何显示一条消息，如果没有不属于所选类别的产品。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢...

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Zack Jones 和沈 Shulok。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)
[下一页](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
