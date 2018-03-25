---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
title: "与从内容页 (VB) 母版页交互 |Microsoft 文档"
author: rick-anderson
description: "检查如何调用的方法，从内容页中的代码中设置属性的母版页等。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 081fe010-ba0f-4e7d-b4ba-774840b601c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
msc.type: authoredcontent
ms.openlocfilehash: a9020115e6348d30350f8fff493efe88bd0d4c37
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="interacting-with-the-master-page-from-the-content-page-vb"></a>与从内容页 (VB) 母版页交互
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_VB.zip)或[下载 PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_VB.pdf)

> 检查如何调用的方法，从内容页中的代码中设置属性的母版页等。


## <a name="introduction"></a>介绍

在我们讨论如何创建母版页过过去五个教程的过程中，定义内容区域，将 ASP.NET 页绑定到母版页，并定义特定于页的内容。 当访问者请求特定的内容页时，内容和母版页的标记 fused 在运行时，导致统一的控件层次结构的呈现。 因此，我们已经看到母版页，其内容页中的另一个可以进行交互的一种方式： 拼写 transfuse 到母版页的 ContentPlaceHolder 控件的标记为内容页。

什么我们尚未检查是如何母版页和内容页可以以编程方式交互。 除了定义母版页的 ContentPlaceHolder 控件的标记，内容页可以将值分配给其母版页的公共属性和调用其公共方法。 同样，母版页可能与其内容页进行交互。 不太常见比其声明性标记之间的交互之间 master 和内容页编程交互时，有很多情况下需要此类编程交互的位置。

在本教程中我们检查如何内容页可以以编程方式与页面进行交互其主;在下一教程中我们将查看如何母版页同样可以与其内容的网页交互。

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>内容页和其母版页之间编程交互的示例

当需要按页基于配置页的特定区域时，我们将使用 ContentPlaceHolder 控件。 但呢页大部分需要发出某些输出，但较小数量的页的情况下需要自定义它以显示其他内容？ 这样一个我们探讨了中的示例[*多个 ContentPlaceHolders 和默认内容*](multiple-contentplaceholders-and-default-content-vb.md)教程中，包括每一页上显示的登录名接口。 虽然大多数页应包含登录名接口，它应取消的少量的页面，如： 主登录页 (`Login.aspx`); 创建帐户页中; 以及才可供经过身份验证的用户访问的其他页。 [*多个 ContentPlaceHolders 和默认内容*](multiple-contentplaceholders-and-default-content-vb.md)教程介绍了如何为 ContentPlaceHolder 母版页中定义的默认内容和如何重写该中那些然后页的位置不需要默认内容。

另一种方法是创建的公共属性或母版页，该值指示是否显示或隐藏登录界面中的方法。 例如，母版页可能包含名为的公共属性`ShowLoginUI`其值用于设置`Visible`母版页中的登录控件的属性。 然后可以以编程方式设置这些内容页应取消显示登录用户界面`ShowLoginUI`属性`False`。

可能需要在某些操作已经在内容页中产生后刷新该母版页中显示数据时发生最常见的内容和母版页交互示例。 考虑包括一个 GridView，显示 5 个最新的主页面添加记录从特定数据库表，并且其内容页中的一个包括用于将新记录添加到该相同的表的接口。

当用户访问页后，可以添加新的记录时，她看到的五个最近添加的记录显示在母版页。 后填写新记录的列的值，她将提交窗体。 假定在母版页中的 GridView 具有其`EnableViewState`属性设置为 True （默认值），从视图状态重新加载其内容，，并因此，虽然的较新记录刚才添加到该数据库显示五个记录相同的。 这可能会将用户相混淆。

> [!NOTE]
> 即使你禁用 GridView 的视图状态，以便它将在每个回发时其基础数据源重新绑定，它仍不会显示刚才添加记录因为数据绑定到 GridView 前面的页面生命周期比新的记录添加到 datab 时ase。


要解决此问题，以便只添加记录显示在母版页的 GridView 在回发时，我们需要指示其数据源重新绑定的 GridView*后*已添加到数据库中的新记录。 这需要的内容与母版页之间的交互，因为用于添加新的记录 （和其事件处理程序） 均在内容页，但需要刷新 GridView 接口是在母版页中。

由于刷新从事件处理程序在内容页的主控页的显示内容和母版页交互的最常见需求之一，让我们浏览本主题中更多详细信息。 本教程中下载内容还包括名为的 Microsoft SQL Server 2005 Express Edition 数据库`NORTHWIND.MDF`在网站的`App_Data`文件夹。 Northwind 数据库存储产品、 员工和针对虚构公司，Northwind Traders 的销售信息。

显示的五个最近将步骤 1 演练在母版页中 GridView 中添加产品。 步骤 2 创建一个内容页面添加新产品。 步骤 3 着眼于如何在主页上中, 创建公共属性和方法，并第 4 步演示如何以编程方式与这些属性和内容页中的方法之间的接口。

> [!NOTE]
> 本教程不深入探讨在 ASP.NET 中使用数据的详细信息。 有关设置主控页以显示数据和插入数据的内容页的步骤是完成的、 尚未 breezy。 如在显示和插入数据以及使用 SqlDataSource 和 GridView 控件更深入地了解，在本教程末尾查阅进一步读数部分中的资源。


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>步骤 1： 显示五个最近母版页中添加产品

打开 Site.master 母版页并添加一个标签和 GridView 控件与`leftContent` `<div>`。 清理标签的`Text`属性，设置其`EnableViewState`属性`False`，并将其`ID`属性`GridMessage`; 设置 GridView`ID`属性`RecentProducts`。 接下来，从设计器中，展开 GridView 的智能标记，并选择将其绑定到新的数据源。 这将启动数据源配置向导。 因为 Northwind 数据库中`App_Data`文件夹是 Microsoft SQL Server 数据库，选择通过选择 （参见图 1） 创建 SqlDataSource; SqlDataSource `RecentProductsDataSource`。


[![将 GridView 绑定到名为 RecentProductsDataSource SqlDataSource 控件](interacting-with-the-master-page-from-the-content-page-vb/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image1.png)

**图 01**： 绑定到 SqlDataSource 控件命名为 GridView `RecentProductsDataSource` ([单击以查看实际尺寸的图像](interacting-with-the-master-page-from-the-content-page-vb/_static/image3.png))


下一步让我们指定什么数据库以连接到。 选择`NORTHWIND.MDF`数据库文件从下拉列表，然后单击下一步。 由于这是首次我们已使用此数据库，该向导将提供的用于存储中的连接字符串`Web.config`。 使其使用的名称将连接字符串存储`NorthwindConnectionString`。


[![连接到 Northwind 数据库](interacting-with-the-master-page-from-the-content-page-vb/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image4.png)

**图 02**： 连接到 Northwind 数据库 ([单击以查看实际尺寸的图像](interacting-with-the-master-page-from-the-content-page-vb/_static/image6.png))


配置数据源向导提供了通过其中我们可以指定用于检索数据的查询的两个方法：

- 通过指定自定义 SQL 语句或存储的过程，或
- 通过选择表或视图，然后指定要返回的列

由于我们要返回 5 个最近添加产品，我们需要指定自定义 SQL 语句。 使用以下`SELECT`查询：


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample1.sql)]

`TOP 5`关键字从查询返回仅前五个记录。 `Products`表的主键， `ProductID`，是`IDENTITY`列，将确保我们，每个新的产品添加到表将具有更大的值比以前的条目。 因此，对通过结果进行排序`ProductID`按降序返回开头最近创建的产品。


[![返回五个最近添加的产品](interacting-with-the-master-page-from-the-content-page-vb/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image7.png)

**图 03**： 返回五个最最近添加产品 ([单击以查看实际尺寸的图像](interacting-with-the-master-page-from-the-content-page-vb/_static/image9.png))


完成向导后，Visual Studio 将生成两个 BoundFields gridview 以显示`ProductName`和`UnitPrice`从数据库返回的字段。 此时你母版页的声明性标记应包含类似于以下标记：


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample2.aspx)]

如你所见，标记包含： 标签 Web 控件 (`GridMessage`); GridView `RecentProducts`、 两个 BoundFields; 与返回的五个最近添加产品 SqlDataSource 控件。

与此 GridView 创建和配置，其 SqlDataSource 控件访问网站，通过浏览器。 如图 4 所示，你将看到的网格中，其中列出了五个最近左下角添加产品。


[![GridView 显示五个最近添加的产品](interacting-with-the-master-page-from-the-content-page-vb/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image10.png)

**图 04**: GridView 显示五个最最近添加产品 ([单击以查看实际尺寸的图像](interacting-with-the-master-page-from-the-content-page-vb/_static/image12.png))


> [!NOTE]
> 随意清理 GridView 的外观。 一些建议包括格式设置显示`UnitPrice`值作为货币和使用背景色和字体提高网格的外观。


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>步骤 2： 创建内容页后，可以添加新产品

我们的下一个任务是创建用户可以从其添加到新的产品内容页`Products`表。 添加到新的内容页`Admin`文件夹名为`AddProduct.aspx`，这会让确保以将其绑定到`Site.master`母版页。 图 5 显示解决方案资源管理器后此页已添加到网站。


[![将新的 ASP.NET 页添加到管理员文件夹](interacting-with-the-master-page-from-the-content-page-vb/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image13.png)

**图 05**： 添加到新的 ASP.NET 页`Admin`文件夹 ([单击以查看实际尺寸的图像](interacting-with-the-master-page-from-the-content-page-vb/_static/image15.png))


回想一下，在[*母版页中指定的标题、 Meta 标记和其他 HTML 标头*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)教程中我们创建一个名为的自定义的基本页类`BasePage`如果它已生成了该页面的标题未显式设置。 转到`AddProduct.aspx`页的代码隐藏类，并将其派生自`BasePage`(而不是从`System.Web.UI.Page`)。

最后，更新`Web.sitemap`文件以包含本课程的适当的项。 添加以下标记下的`<siteMapNode>`控制 ID 命名问题课后生成：


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample3.xml)]

图 6 中，此加法中所示`<siteMapNode>`课程列表中反映元素。

返回到`AddProduct.aspx`。 在内容控件中为`MainContent`ContentPlaceHolder，添加说明控件并将其命名`NewProduct`。 将说明如何绑定到一个名为的新 SqlDataSource 控件`NewProductDataSource`。 例如，使其使用 Northwind 数据库，则使用在步骤 1 中 SqlDataSource，配置向导，选择来指定自定义 SQL 语句。 说明如何将用于将项添加到数据库，因为我们需要同时指定两个`SELECT`语句和`INSERT`语句。 使用以下`SELECT`查询：


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample4.sql)]

然后，从插入选项卡，添加以下`INSERT`语句：


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample5.sql)]

完成向导后转到说明的智能标记，并选中"启用插入"复选框。 这将向与说明如何添加 CommandField 其`ShowInsertButton`属性设置为 True。 此说明如何将仅用于插入数据使用，因为设置说明的`DefaultMode`属性`Insert`。

这就是所有到它 ！ 让我们测试此页。 请访问`AddProduct.aspx`通过浏览器中，输入名称和价格 （请参见图 6）。


[![向数据库添加新产品](interacting-with-the-master-page-from-the-content-page-vb/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image16.png)

**图 06**： 向数据库添加新的产品 ([单击以查看实际尺寸的图像](interacting-with-the-master-page-from-the-content-page-vb/_static/image18.png))


键入后的名称和新产品的价格中，单击插入按钮。 这将导致窗体回发。 在回发时，SqlDataSource 控件的`INSERT`执行语句; 其两个参数填充说明的两个文本框控件中的用户输入值。 遗憾的是，没有可视反馈插入已发生。 最好能够显示，确认已添加一条新记录的消息。 我将此作为练习为读取器。 此外，从说明如何添加一条新记录后在母版页中的 GridView 仍将显示相同的五个记录作为之前;它不包括刚才添加的记录。 我们将研究如何解决此问题在即将执行的步骤。

> [!NOTE]
> 除了添加某种形式的已成功插入的可视反馈，我将鼓励你还更新说明的插入接口，以包含验证。 目前，没有任何验证。 如果用户输入的值无效`UnitPrice`字段，如"太占用大量资源，"时系统会尝试将该字符串转换为十进制，将在回发时引发异常。 有关详细信息自定义插入接口，请参阅[*自定义的数据修改界面*教程](https://asp.net/learn/data-access/tutorial-20-vb.aspx)从我[使用数据教程系列](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>步骤 3： 在母版页中创建公共属性和方法

在步骤 1 中，我们将添加一个名为的标签 Web 控件`GridMessage`上面母版页中 GridView。 此标签用于根据需要显示一条消息。 例如，在添加新记录到`Products`表，我们可能想要显示读取一条消息:"*ProductName*已添加到数据库。" 而不是硬编码的母版页中的此标签的文本，我们可能需要是可通过内容页自定义的消息。

由于标签控件它不能直接从内容页访问的母版页中的受保护的成员变量作为实现的。 若要使用母版页从内容页 （或者，为该很重要，母版页中的任何 Web 控件） 中的标签我们需要在母版页公开 Web 控件或作为代理所依据的一个属性可以是服务中创建的公共属性 访问。 将以下语法添加到母版页的代码隐藏类公开的标签`Text`属性：


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample6.vb)]

当一条新记录添加到`Products`从内容页表`RecentProducts`母版页中的 GridView 需要重新绑定到其基础数据源。 若要重新绑定的 GridView 调用其`DataBind`方法。 由于在母版页中的 GridView 不是以编程方式访问内容页，我们需要创建一个公共方法在母版页中，调用时，将重新绑定到 GridView 的数据。 将以下方法添加到母版页的代码隐藏类：


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample7.vb)]

与`GridMessageText`属性和`RefreshRecentProductsGrid`方法中的位置，任何内容页可以以编程方式设置或读取的值`GridMessage`标签的`Text`属性或重新绑定数据到`RecentProducts`GridView。 步骤 4 检查如何从内容页访问母版页的公共属性和方法。

> [!NOTE]
> 不要忘记将标记母版页的属性和方法作为`Public`。 如果你不显式表示这些属性和方法作为`Public`，它们将不能从内容页访问。


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>步骤 4： 从内容页调用母版页的公共成员

现在，主控页具有必要的公共属性和方法，我们已准备好调用这些属性和方法从`AddProduct.aspx`内容页。 具体而言，我们需要将设置母版页的`GridMessageText`属性并调用其`RefreshRecentProductsGrid`方法后已添加到数据库中新的产品。 立即之前和之后完成各种任务，这样可以方便页开发人员采取某种编程操作之前或之后任务，则所有 ASP.NET 数据 Web 控件都触发事件。 例如，当最终用户单击时说明的插入按钮，在回发说明如何引发其`ItemInserting`开始插入的工作流之前的事件。 然后，它将记录插入到数据库中。 接下来，说明如何引发其`ItemInserted`事件。 因此，为了使用母版页添加新产品后，为说明如何创建事件处理程序`ItemInserted`事件。

有两种方法供内容页以编程方式可与其母版页：

- 使用`Page.Master`属性，它返回到母版页的松散类型化引用，或
- 指定通过该页面的母版页类型或文件路径`@MasterType`指令; 这会自动将强类型的属性添加到名为的页`Master`。

让我们看一下这两种方法。

### <a name="using-the-loosely-typedpagemasterproperty"></a>使用松散类型`Page.Master`属性

所有的 ASP.NET web pages 必须派生自`Page`类，该类位于`System.Web.UI`命名空间。 `Page`类包括[`Master`属性](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx)返回到该页面的主控页的引用。 如果页面没有母版页`Master`返回`Nothing`。

`Master`属性返回类型的对象[ `MasterPage` ](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (也位于`System.Web.UI`命名空间) 是从其所有母版页都派生自基类型。 因此，若要使用的公共属性或方法定义中我们网站的母版页我们必须强制转换`MasterPage`从返回的对象`Master`为适当的类型的属性。 因为我们命名为我们的主控页文件`Site.master`，代码隐藏类名为`Site`。 因此，下面的代码的强制转换`Page.Master`指向的实例的属性`Site`类。


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample8.vb)]

现在，我们已转换松散类型`Page.Master`属性，我们可以向网站类型引用的属性和方法特定于站点。 如图 7 所示的公共属性`GridMessageText`出现在 IntelliSense 下拉列表。


[![IntelliSense 会显示我们母版页的公共属性和方法](interacting-with-the-master-page-from-the-content-page-vb/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image19.png)

**图 07**: IntelliSense 会显示我们母版页的公共属性和方法 ([单击以查看实际尺寸的图像](interacting-with-the-master-page-from-the-content-page-vb/_static/image21.png))


> [!NOTE]
> 如果名为你的主控页文件`MasterPage.master`主控页的代码隐藏类名称则`MasterPage`。 这可能会导致不明确的代码，当从类型强制转换时`System.Web.UI.MasterPage`到你`MasterPage`类。 简单地说，您需要完全限定的类型进行强制转换，这一点可能会稍有很棘手，在使用网站项目模型时。 我的建议可以确保，如果要创建主页面名称为其命名以外`MasterPage.master`或甚至更好地创建强类型引用的主控页。


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>创建一个强类型具有引用`@MasterType`指令

如果您仔细查看您可以看到 ASP.NET 页的代码隐藏类是分部类 (请注意`Partial`类定义中的关键字)。 分部类 C# 和 Visual Basic with.NET Framework 2.0 中引入，并简单地说，允许进行跨多个文件定义的类的成员。 代码隐藏类文件- `AddProduct.aspx.vb`，例如-包含我们页开发人员创建的代码。 除了下面代码中，ASP.NET 引擎会自动创建单独的类文件具有属性和事件处理程序中，将声明性标记转换为页的类层次结构。

只要访问 ASP.NET 页就会发生自动代码生成为某些而有趣且有用的可能性铺平。 对于主页面，如果我们将告诉我们内容页正在使用哪些母版页 ASP.NET 引擎就会生成一个强类型`Master`为我们的属性。

使用[`@MasterType`指令](https://msdn.microsoft.com/library/ms228274.aspx)以通知内容页面的母版页类型 ASP.NET 引擎。 `@MasterType`指令可以接受主控页的类型名称，或者其文件路径。 若要指定`AddProduct.aspx`页上使用`Site.master`作为其主页上，添加以下指令到顶部`AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample9.aspx)]

此指令将指示 ASP.NET 引擎添加到母版页通过名为的属性的强类型引用`Master`。 与`@MasterType`就地指令，我们可以调用`Site.master`主页面的公共属性和方法直接通过`Master`不带任何强制转换的属性。

> [!NOTE]
> 如果省略`@MasterType`指令，语法`Page.Master`和`Master`返回相同的操作： 为页的母版页的松散类型化对象。 如果包含`@MasterType`指令然后`Master`返回指定的主控页的强类型引用。 `Page.Master`但是，仍会返回松散类型化的引用。 如原因更全面地了解这种情况以及如何`Master`构造属性时`@MasterType`指令是包括在内，请参阅[k。 Scott Allen](http://odetocode.com/blogs/scott/default.aspx)的博客文章[`@MasterType`在 ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>添加新产品后更新母版页

现在，我们知道如何调用母版页的公共属性和内容页中的方法，我们已准备好更新`AddProduct.aspx`页，以便在添加新产品后刷新母版页。 步骤 4 开头我们创建的事件处理程序说明如何控制`ItemInserting`事件，新产品已添加到数据库后立即执行。 将以下代码添加到该事件处理程序：


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample10.vb)]

上述代码使用这两个松散类型化`Page.Master`属性和强类型`Master`属性。 请注意，`GridMessageText`属性设置为"*ProductName*添加到网格..."只添加产品的值是可通过访问`e.Values`集合; 如你所见，只-添加`ProductName`值访问通过`e.Values("ProductName")`。

图 8 显示`AddProduct.aspx`立即后新产品-Scott 的 Soda 的页已添加到数据库。 请注意，只添加产品名称将其记录在母版页的标签和 GridView 刷新以包括产品和其价格。


[![母版页的标签和 GridView 显示刚才添加产品](interacting-with-the-master-page-from-the-content-page-vb/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image22.png)

**图 08**: 母版页的标签和 GridView 显示 Just-Added 产品 ([单击以查看实际尺寸的图像](interacting-with-the-master-page-from-the-content-page-vb/_static/image24.png))


## <a name="summary"></a>摘要

理想情况下，母版页和其内容页是从另一个完全独立的不需要任何级别的交互。 虽然母版页和内容页应设计为具有记住该目标，有大量的顺序内容页必须与其母版页的常见方案。 最常见的原因之一是围绕更新母版页显示在内容页中发生的情况的某种操作所基于的特定部分。

好消息是相对比较简单，能够以编程方式与其主页面进行交互的内容页。 通过在母版页封装的功能，需要调用的内容页中创建公共属性或方法启动。 然后，在内容页，访问母版页的属性和方法通过松散类型`Page.Master`属性或使用`@MasterType`指令来创建到母版页的强类型引用。

在下一教程中，我们将查看如何通过编程方式与一个其内容页中的交互的母版页。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [访问和更新在 ASP.NET 中的数据](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET 母版页： 提示、 技巧和陷阱](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType`在 ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [内容与母版页之间传递信息](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [在 ASP.NET 教程中使用数据](../../data-access/index.md)

### <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多个 ASP/ASP.NET 丛书和 4GuysFromRolla.com 创始人，具有已使用 Microsoft Web 技术自 1998 年。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 3.5 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)。 可以在达到 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Zack Jones。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](control-id-naming-in-content-pages-vb.md)
[下一页](interacting-with-the-content-page-from-the-master-page-vb.md)
