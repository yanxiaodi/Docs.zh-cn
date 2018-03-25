---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
title: "与来自母版页 (VB) 的内容页交互 |Microsoft 文档"
author: rick-anderson
description: "检查如何调用的方法，从母版页中的代码中设置属性的内容页等。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: a6e2e1a0-c925-43e9-b711-1f178fdd72d7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: a25f739e5d5717d275554909e1584bb7e7fed302
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="interacting-with-the-content-page-from-the-master-page-vb"></a>与来自母版页 (VB) 的内容页进行交互
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_VB.zip)或[下载 PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_VB.pdf)

> 检查如何调用的方法，从母版页中的代码中设置属性的内容页等。


## <a name="introduction"></a>介绍

前面的教程学习了如何具有内容页以编程方式与其主页面进行交互。 回想一下，我们更新主页后，可以包括列出五个最近的 GridView 控件添加产品。 然后，我们将创建内容页用户可以从中添加新产品。 在添加新的产品，需要指示主页面以刷新其 GridView，以便其将包括只添加产品内容页。 通过向母版页，刷新数据绑定到 GridView，添加一个公共方法，然后调用该方法从内容页，已实现此功能。

内容和母版页交互的最常见形式源自内容页。 但是，可能 master 页后，可以实现价值，rouse 当前内容页，并且如果母版页包含使用户能够修改数据，也会显示在内容页的用户界面元素，则可能需要这样的功能。 请考虑将内容页显示一个 GridView 中的产品信息控制和母版页包括一个按钮控件，单击时，两倍，所有产品的价格。 类似于前面的教程中的示例，GridView 需要刷新后，使其显示新的价格中，单击按钮的双精度价格，但在这种情况下很母版页需要转换为操作 rouse 内容页。

本教程将探讨如何让母版页调用内容页中定义的功能。

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>决定通过事件和事件处理程序的编程交互

调用从母版页的内容页面功能是比另一种方法解决更具挑战性。 由于内容页时决定从内容页的编程交互具有使用单个母版页，因此我们知道什么公共方法和属性都是我们的处理方法。 母版页，但是，可以有许多不同内容页中，每个都有自己的属性和方法集。 如何操作，然后，可以我们中编写代码时我们不知道哪些内容页将调用直到运行时在其内容页中执行某些操作的母版页？

请考虑的 ASP.NET Web 控件，如按钮控件。 按钮控件上任意数量的 ASP.NET 页可显示和需要依据它会发出警告页已被单击的机制。 这通过*事件*。 具体而言，该按钮控件引发其`Click`事件单击时其; 包含按钮的 ASP.NET 页 （可选） 可以响应通过该通知*事件处理程序*。

这一相同模式可以用于在其内容页中具有母版页触发器功能：

1. 将事件添加到母版页。
2. 引发事件时母版页需要其内容页与进行通信。 例如，如果母版页需要报警其内容页价格加倍用户后，其将会引发已翻倍价格后立即
3. 在需要采取某项操作这些内容页中创建事件处理程序。

本教程的此其余部分实现简介; 中所述的示例也就是说，内容页列出在数据库中的产品和包括一个按钮的母版页控制为双精度，最低价。

## <a name="step-1-displaying-products-in-a-content-page"></a>步骤 1： 在内容页上显示产品

我们第一个的顺序的业务就是创建内容页列出从 Northwind 数据库的产品。 (我们将 Northwind 数据库添加到项目中前面的教程中， [*与从内容页母版页交互*](interacting-with-the-master-page-from-the-content-page-vb.md)。)首先，通过添加到新的 ASP.NET 页`~/Admin`文件夹名为`Products.aspx`，这会让确保以将其绑定到`Site.master`母版页。 图 1 显示解决方案资源管理器后此页已添加到网站。


[![将新的 ASP.NET 页添加到管理员文件夹](interacting-with-the-content-page-from-the-master-page-vb/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image1.png)

**图 01**： 添加到新的 ASP.NET 页`Admin`文件夹 ([单击以查看实际尺寸的图像](interacting-with-the-content-page-from-the-master-page-vb/_static/image3.png))


回想一下，在[*母版页中指定的标题、 Meta 标记和其他 HTML 标头*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)教程中我们创建一个名为的自定义的基本页类`BasePage`，如果它不会生成页的标题显式设置。 转到`Products.aspx`页的代码隐藏类，并将其派生自`BasePage`(而不是从`System.Web.UI.Page`)。

最后，更新`Web.sitemap`文件以包含本课程的适当的项。 添加以下标记下的`<siteMapNode>`到母版页交互课程的内容：


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample1.xml)]

此添加`<siteMapNode>`元素将反映在这些课程列表 （请参见图 5）。

返回到`Products.aspx`。 在内容控件中为`MainContent`、 添加 GridView 控件并将其命名`ProductsGrid`。 将 GridView 绑定到一个名为的新 SqlDataSource 控件`ProductsDataSource`。


[![将 GridView 绑定到新的 SqlDataSource 控件](interacting-with-the-content-page-from-the-master-page-vb/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image4.png)

**图 02**： 将 GridView 绑定到新的 SqlDataSource 控件 ([单击以查看实际尺寸的图像](interacting-with-the-content-page-from-the-master-page-vb/_static/image6.png))


配置向导，以便它使用 Northwind 数据库。 如果你处理通过以前的教程，则你应该已拥有名为的连接字符串`NorthwindConnectionString`中`Web.config`。 从下拉列表中，选择此连接字符串，如图 3 中所示。


[![配置为使用 Northwind 数据库 SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image7.png)

**图 03**： 配置为使用 Northwind 数据库 SqlDataSource ([单击以查看实际尺寸的图像](interacting-with-the-content-page-from-the-master-page-vb/_static/image9.png))


接下来，指定的数据源控件`SELECT`通过从下拉列表中选择产品表并返回语句`ProductName`和`UnitPrice`（请参见图 4） 的列。 单击下一步并完成以完成配置数据源向导。


[![从产品表中返回的产品名称和单价字段](interacting-with-the-content-page-from-the-master-page-vb/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image10.png)

**图 04**： 返回`ProductName`和`UnitPrice`字段从`Products`表 ([单击以查看实际尺寸的图像](interacting-with-the-content-page-from-the-master-page-vb/_static/image12.png))


这就是所有到它 ！ 完成向导后 Visual Studio 将两个 BoundFields 添加到 GridView 镜像返回 SqlDataSource 控件的两个字段。 遵循的 GridView 和 SqlDataSource 控件的标记。 图 5 显示通过浏览器查看时的结果。


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample2.aspx)]


[![每个产品和其价格被列入 GridView](interacting-with-the-content-page-from-the-master-page-vb/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image13.png)

**图 05**： 每个产品和其价格被列入 GridView ([单击以查看实际尺寸的图像](interacting-with-the-content-page-from-the-master-page-vb/_static/image15.png))


> [!NOTE]
> 随意清理 GridView 的外观。 一些建议包括格式设置为货币显示的单价值和使用背景色和字体以改善网格的外观。 有关如何显示数据以及设置在 ASP.NET 中的数据格式的详细信息，请参阅我[使用数据教程系列](../../data-access/index.md)。


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>步骤 2： 将 Double 价格按钮添加到 Master 页

我们的下一个任务是要添加到母版按钮 Web 控件页上，单击时，将在数据库中的所有产品的价格加倍。 打开`Site.master`母版页和将按钮拖动从工具箱中拖动到设计器中，将其下方放置`RecentProductsDataSource`SqlDataSource 控件我们在前面的教程中添加。 设置按钮的`ID`属性`DoublePrice`及其`Text`属性设置为"Double 产品价格"。

接下来，将 SqlDataSource 控件添加到母版页，从而将其命名为`DoublePricesDataSource`。 此 SqlDataSource 将用于执行`UPDATE`语句为双精度，所有的价格。 具体而言，我们需要将设置其`ConnectionString`和`UpdateCommand`属性设置为适当的连接字符串和`UPDATE`语句。 然后我们需要调用此 SqlDataSource 控件`Update`方法时`DoublePrice`单击按钮。 若要设置`ConnectionString`和`UpdateCommand`属性，选择 SqlDataSource 控件，然后转到属性窗口。 `ConnectionString`属性列表已存储在这些连接字符串`Web.config`在下拉列表中; 选择`NorthwindConnectionString`选项图 6 中所示。


[![配置为使用 NorthwindConnectionString SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image16.png)

**图 06**： 配置使用 SqlDataSource `NorthwindConnectionString` ([单击以查看实际尺寸的图像](interacting-with-the-content-page-from-the-master-page-vb/_static/image18.png))


若要设置`UpdateCommand`属性，在属性窗口中找到 UpdateQuery 选项。 此属性，当选中时，将显示一个具有省略号; 的按钮单击此按钮将显示命令和参数编辑器对话框中图 7 所示。 键入以下`UPDATE`到对话框的文本框中的语句：


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample3.sql)]

此语句中，执行时，将会翻倍`UnitPrice`中每个记录的值`Products`表。


[![设置 SqlDataSource 的 UpdateCommand 属性](interacting-with-the-content-page-from-the-master-page-vb/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image19.png)

**图 07**： 设置 SqlDataSource`UpdateCommand`属性 ([单击以查看实际尺寸的图像](interacting-with-the-content-page-from-the-master-page-vb/_static/image21.png))


设置这些属性之后, 你按钮和 SqlDataSource 控件的声明性标记应如下类似如下：


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample4.aspx)]

剩下的就是调用其`Update`方法时`DoublePrice`单击按钮。 创建`Click`事件处理程序`DoublePrice`按钮并添加以下代码：


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample5.vb)]

若要测试此功能，请访问`~/Admin/Products.aspx`页面，我们在步骤 1 中创建并单击"双产品价格"按钮。 单击的按钮会导致回发并执行`DoublePrice`按钮的`Click`事件处理程序，所有产品将价格加倍。 然后重新呈现页面和标记是返回并重新显示在浏览器。 GridView 在内容页中，但是，列出"双产品价格"按钮被单击相同的价格。 这是因为在 GridView 最初加载的数据已存储视图状态中，因此它不在时重新加载回发除非规定，否则其状态。 如果访问其他页面，然后返回到`~/Admin/Products.aspx`页上，将看到更新的价格。

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>引发事件时的价格加倍步骤 3:

因为在 GridView`~/Admin/Products.aspx`页不会立即反映价格加倍，用户可能会不难理解认为它们未单击"双产品价格"按钮，或它不起作用。 它们可能会被单击的按钮几个更多时间，将价格反复加倍。 若要解决此我们需要将内容中的网格页新价格后立即显示它们翻倍。

本教程中前面所述，我们需要引发母版页中的事件，无论何时用户单击`DoublePrice`按钮。 事件是为一个类 （事件发布者） 的方法通知另一组的一些有趣的内容发生其他类 （事件订阅服务器）。 在此示例中，母版页是事件发布者;这些内容页关注时`DoublePrice`单击按钮是订阅服务器。

类通过创建订阅事件*事件处理程序*，这是在响应引发的事件中执行的方法。 发布服务器定义他通过定义引发的事件*事件委托*。 事件委托指定事件处理程序必须接受哪些输入的参数。 在.NET Framework 中，事件委托不返回任何值并接受两个输入的参数：

- `Object`，它标识事件源和
- 从派生的类`System.EventArgs`

传递给事件处理程序的第二个参数可能包括有关事件的其他信息。 虽然基`EventArgs`类不能通过任何信息、.NET Framework 包括了多个扩展的类`EventArgs`和包含其他属性。 例如，`CommandEventArgs`实例传递到响应的事件处理程序`Command`事件，并包括两个条信息性属性：`CommandArgument`和`CommandName`。

> [!NOTE]
> 有关创建的详细信息，引发，和处理事件，请参阅[事件和委托](https://msdn.microsoft.com/library/17sde2xt.aspx)和[中简单英语的事件委托](http://www.codeproject.com/KB/cs/eventdelegates.aspx)。


若要定义一个事件，请使用以下语法：


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample6.vb)]

因为我们只需警报内容页上，当用户单击`DoublePrice`按钮和不需要传递任何其他附加信息，我们可以使用事件委托`EventHandler`，其定义的事件处理程序接受为第二个操作数参数类型的对象`System.EventArgs`。 若要在母版页中创建事件，请到母版页的代码隐藏类添加以下代码行：


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample7.vb)]

上面的代码将公共事件添加到名为母版页`PricesDoubled`。 现在，我们需要加倍价格之后引发此事件。 若要引发事件，请使用以下语法：


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample8.vb)]

其中*发件人*和*eventArgs*是你想要传递到订阅服务器的事件处理程序的值。

更新`DoublePrice``Click`事件处理程序替换为以下代码：


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample9.vb)]

和前面一样，`Click`通过调用事件处理程序启动`DoublePricesDataSource`SqlDataSource 控件`Update`方法为双精度，所有产品的价格。 以下有两个添加到事件处理程序的操作。 首先， `RecentProducts` GridView 的数据刷新。 此 GridView 已添加到母版页中前面的教程并显示最近添加的五种产品。 我们需要刷新此网格，以使其显示这些五个产品的刚才翻倍价格。 以下，`PricesDoubled`引发事件。 对主控页本身的引用 (`Me`) 作为事件源和一个空发送到事件处理程序`EventArgs`对象发送作为事件自变量。

## <a name="step-4-handling-the-event-in-the-content-page"></a>步骤 4： 处理内容页中的事件

此时母版页引发其`PricesDoubled`事件每当`DoublePrice`单击按钮控件。 但是，这是成功了仅一半-我们仍需要处理订阅服务器中的事件。 这涉及到两个步骤： 创建事件处理程序，然后添加事件连结代码，以便当引发事件时执行的事件处理程序。

首先创建名为一个事件处理程序`Master_PricesDoubled`。 由于我们的定义方式`PricesDoubled`母版页中的事件必须是事件处理程序的两个输入的参数的类型`Object`和`EventArgs`分别。 在事件处理程序调用`ProductsGrid`GridView 的`DataBind`方法重新绑定数据到网格。


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample10.vb)]

事件处理程序的代码已完成，但我们尚未为连接母版页的`PricesDoubled`事件与此事件处理程序。 订阅服务器上条电源线条事件事件处理程序通过以下语法：


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample11.vb)]

*发布服务器*是对提供事件的对象的引用*eventName*，和*methodName*是在订阅服务器中定义的事件处理程序的名称。

此事件连结代码必须在第一次页访问和后续回发上执行，并应出现在之前可能引发事件时的页生命周期中的点。 添加事件连结代码的好时机是在 PreInit 阶段中，页生命周期中的非常早发生。

打开`~/Admin/Products.aspx`并创建`Page_PreInit`事件处理程序：


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample12.vb)]

若要完成此连结代码我们需要对母版页内容页中的编程引用。 如前面的教程中所述，有两种方式执行此操作：

- 通过强制转换为松散类型`Page.Master`属性设置为适当的母版页类型，或
- 通过添加`@MasterType`指令`.aspx`页，然后使用强类型`Master`属性。

让我们使用后一种方法。 添加以下`@MasterType`指令到页面的声明性标记的顶部：


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample13.aspx)]

然后添加中的以下事件连结代码`Page_PreInit`事件处理程序：


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample14.vb)]

使用此代码中的位置，刷新内容页中的 GridView 每当`DoublePrice`单击按钮。

图 8 和 9 说明了此行为。 图 8 显示页面在首次访问时。 请注意，在这两价格值`RecentProducts`（主控页左侧列） 中的 GridView 和`ProductsGrid`GridView （在内容页）。 图 9 显示相同屏幕后立即`DoublePrice`单击按钮。 如你所见，新价格将立即反映在这两个 GridViews。


[![初始的价格值](interacting-with-the-content-page-from-the-master-page-vb/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image22.png)

**图 08**: 初始的价格值 ([单击以查看实际尺寸的图像](interacting-with-the-content-page-from-the-master-page-vb/_static/image24.png))


[![Just-Doubled 价格显示在 GridViews](interacting-with-the-content-page-from-the-master-page-vb/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image25.png)

**图 09**: The Just-Doubled 价格显示在 GridViews ([单击以查看实际尺寸的图像](interacting-with-the-content-page-from-the-master-page-vb/_static/image27.png))


## <a name="summary"></a>摘要

理想情况下，母版页和其内容页是从另一个完全独立的不需要任何级别的交互。 但是，如果你有一个母版页或显示可以从主页或内容页面修改的数据的内容页，然后你可能需要具有母版页内容页 （或进行相反的转换） 发出警报，以便可以更新显示时修改数据。 我们已在前面的教程了解如何通过编程方式与其母版页; 交互的内容页在本教程中我们介绍了如何让母版页启动交互。

虽然编程交互的内容与母版页之间可以来自的内容或母版页，所使用的交互模式取决于来源。 差异是由于这样一个事实： 内容页具有使用单个母版页，但母版页可以具有许多不同的内容页。 而不是使母版页直接与内容页进行交互，更好的方法是让母版页引发事件以指示已发生某种操作。 有关操作处理这些内容页可以创建事件处理程序。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [访问和更新在 ASP.NET 中的数据](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [事件和委托](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [内容与母版页之间传递信息](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [在 ASP.NET 教程中使用数据](../../data-access/index.md)

### <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多个 ASP/ASP.NET 丛书和 4GuysFromRolla.com 创始人，具有已使用 Microsoft Web 技术自 1998 年。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 3.5 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)。 可以在达到 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Suchi Banerjee。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](interacting-with-the-master-page-from-the-content-page-vb.md)
[下一页](master-pages-and-asp-net-ajax-vb.md)
