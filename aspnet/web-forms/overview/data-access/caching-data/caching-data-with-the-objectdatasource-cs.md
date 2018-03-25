---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
title: "缓存数据与 ObjectDataSource (C#) |Microsoft 文档"
author: rick-anderson
description: "缓存可能意味着速度较慢和快速的 Web 应用程序之间的差异。 本教程是 4 的倍数详细一下缓存在 ASP.NET 中的第一个..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: bd87413c-8160-4520-a8a2-43b555c4183a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 8cd4fd2afb16772baf45618ccee2c3c3caea5b64
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="caching-data-with-the-objectdatasource-c"></a>缓存数据与 ObjectDataSource (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_CS.exe)或[下载 PDF](caching-data-with-the-objectdatasource-cs/_static/datatutorial58cs1.pdf)

> 缓存可能意味着速度较慢和快速的 Web 应用程序之间的差异。 本教程是 4 的倍数详细一下缓存在 ASP.NET 中的第一个。 了解缓存的关键概念以及如何将应用到通过 ObjectDataSource 控件表示层缓存。


## <a name="introduction"></a>介绍

在计算机科学中*缓存*是接受的数据或将占用大量资源，若要获取的信息并将它的副本存储在访问速度更快的位置的过程。 对于数据驱动的应用程序，大型和复杂查询通常使用大多数的应用程序的执行时间。 此类应用程序的性能，然后，通常可以通过将昂贵的数据库查询的结果存储在应用程序的内存改进。

ASP.NET 2.0 提供了各种各样的缓存选项。 可以通过缓存的整个网页或用户控件呈现的 s 标记*输出缓存*。 ObjectDataSource 和 SqlDataSource 控件提供缓存功能以及，从而允许数据缓存的控制级别。 和 ASP.NET s*数据缓存*提供一个丰富的缓存 API，用于以编程方式缓存对象的页开发人员。 本教程中的下一步的三个，我们将检查使用 ObjectDataSource s 缓存功能，以及为数据缓存。 我们将探讨如何缓存在启动应用程序级数据以及如何使缓存的数据保持最新通过使用 SQL 缓存依赖项。 这些教程不了解输出缓存。 在输出缓存了解详细信息，请参阅[ASP.NET 2.0 中的输出缓存](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)。

缓存可以在该体系结构中的任何位置从数据访问层向上通过应用表示层。 在本教程中我们将查看应用到通过 ObjectDataSource 控件表示层缓存。 在下一步教程中我们将检查缓存在业务逻辑层的数据。

## <a name="key-caching-concepts"></a>密钥缓存概念

缓存可以极大地提高应用程序 s 整体性能和可伸缩性通过采用将占用大量资源来生成的数据，并将它的副本存储在可以更有效地访问的位置。 因为缓存容纳只是实际的基础数据的副本，它将变得过期，或*陈旧*，如果基础数据发生更改。 为了应对这种情况，页开发人员可以指示缓存项将依据的条件*逐出*从缓存中，使用：

- **基于时间的条件**可能将项添加到缓存中用于绝对或滑动持续时间。 例如，页开发人员可能指示持续时间为说，60 秒。 绝对的持续时间，缓存的项将被逐出 60 秒后它已添加到缓存，而不考虑访问频率。 用一个滑动持续时间，缓存的项将被逐出的上次访问后 60 秒。
- **基于依赖项的条件**依赖项可以是与时添加到缓存的项相关联。 S 项依赖项发生更改时它是从缓存中逐出。 依赖项可能是文件、 另一个缓存项或两者的组合。 ASP.NET 2.0 还允许 SQL 缓存依赖关系，可让开发人员将项添加到缓存并让它逐出基础数据库数据发生更改时。 我们将检查 SQL 缓存依赖项中即将推出的[使用 SQL 缓存依赖项](using-sql-cache-dependencies-cs.md)教程。

指定的逐出条件，无论缓存中的项可能*清理*满足基于时间的或基于依赖项的条件之前。 如果缓存已达到其容量，则必须先删除现有项，然后才能添加新的。 因此，以编程方式使用缓存数据时它 s 至关重要，你始终假定，则缓存的数据可能不存在。 我们将查看要使用在访问数据时从缓存以编程方式在我们的下一教程中的模式*体系结构中缓存数据*。

缓存提供了一种经济的方法挤压从应用程序的详细性能。 作为[Steven Smith](http://aspadvice.com/blogs/ssmith/)说明了在他的文章[ASP.NET 缓存： 方法和最佳实践](https://msdn.microsoft.com/library/aa478965.aspx):

缓存可以获得良好足够性能而不需要大量时间和分析的一种好方法。 内存是比较便宜，因此如果你可以获取所需的缓存的输出，而不是一天或每周尝试优化你的代码或数据库的花费 30 秒的性能，请执行缓存的解决方案 （假定为第二个旧-30 数据是确定），然后移。 最后，较差的设计将可能赶上到你，因此当然，你应尝试正确设计应用程序。 但如果你只需获得良好今天的足够性能，缓存可能是一个极好 [方法]，购买时间重构你的应用程序在以后当必须进行这些操作的时间。

虽然缓存可以提供明显的性能增强功能，它并不适用于在所有情况下，例如，使用经常更新的实时数据，或甚至很快留存陈旧数据是不可接受的应用程序。 但对于大多数应用程序的情况下，应使用缓存。 有关在 ASP.NET 2.0 中缓存的更多背景，请参阅[缓存以提高性能](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx)部分[ASP.NET 2.0 快速入门教程](https://quickstarts.asp.net/QuickStartv20/aspnet/)。

## <a name="step-1-creating-the-caching-web-pages"></a>步骤 1： 创建缓存的 Web 页

我们开始我们浏览 ObjectDataSource s 缓存功能之前，让我们来首先花一些时间，我们需要以及本教程的下一步的三个网站项目中创建 ASP.NET 页。 首先，通过添加一个名为的新文件夹`Caching`。 接下来，添加到该文件夹，并确保将每个包含网页相关联的以下 ASP.NET 页面`Site.master`母版页：

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![添加 ASP.NET 页的缓存相关的教程](caching-data-with-the-objectdatasource-cs/_static/image1.png)

**图 1**： 添加 ASP.NET 页的缓存相关的教程


在其他文件夹中，如`Default.aspx`中`Caching`文件夹将在其部分中列出这些教程。 回想一下，`SectionLevelTutorialListing.ascx`用户控件提供此功能。 因此，此用户将控件添加到`Default.aspx`通过从解决方案资源管理器拖到页面上的设计视图拖动。


[![图 2： 将 SectionLevelTutorialListing.ascx 用户控件添加到 Default.aspx](caching-data-with-the-objectdatasource-cs/_static/image3.png)](caching-data-with-the-objectdatasource-cs/_static/image2.png)

**图 2**： 图 2： 添加`SectionLevelTutorialListing.ascx`用户控件`Default.aspx`([单击以查看实际尺寸的图像](caching-data-with-the-objectdatasource-cs/_static/image4.png))


最后，将这些页面添加到的条目为`Web.sitemap`文件。 具体而言，在处理二进制数据后添加以下标记`<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-cs/samples/sample1.xml)]

在更新后`Web.sitemap`，花些时间查看通过浏览器网站的教程。 现在，在左侧菜单在缓存教程包括项。


![站点图现在包括缓存的教程的条目](caching-data-with-the-objectdatasource-cs/_static/image5.png)

**图 3**： 站点图现在包括缓存的教程的条目


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>步骤 2： 在 Web 页上显示产品的列表

本教程将探讨如何使用 ObjectDataSource 控件 s 内置缓存功能。 我们可以看看这些功能之前，不过，我们首先需要页后，可以从工作。 允许 s 创建一个网页，使用列出产品信息检索从 ObjectDataSource GridView`ProductsBLL`类。

首先打开`ObjectDataSource.aspx`页面`Caching`文件夹。 从工具箱拖动到设计器中拖动 GridView，设置其`ID`属性`Products`，和从其智能标记上，选择要绑定到一个名为的新 ObjectDataSource 控件`ProductsDataSource`。 配置要使用 ObjectDataSource`ProductsBLL`类。


[![配置对象数据源以使用 ProductsBLL 类](caching-data-with-the-objectdatasource-cs/_static/image7.png)](caching-data-with-the-objectdatasource-cs/_static/image6.png)

**图 4**： 配置使用 ObjectDataSource`ProductsBLL`类 ([单击以查看实际尺寸的图像](caching-data-with-the-objectdatasource-cs/_static/image8.png))


有关此页上，让我们来创建可编辑的 GridView，以便我们可以检查在 ObjectDataSource 中缓存数据修改通过 GridView 的界面时，会发生什么情况。 在选择选项卡中设置为其默认值，将下拉列表`GetProducts()`，但更改到更新选项卡中的选定的项`UpdateProduct`接受的重载`productName`， `unitPrice`，和`productID`作为其输入参数。


[![将更新选项卡的下拉列表设置为相应 UpdateProduct 重载](caching-data-with-the-objectdatasource-cs/_static/image10.png)](caching-data-with-the-objectdatasource-cs/_static/image9.png)

**图 5**： 将更新选项卡的下拉列表设置为适合`UpdateProduct`重载 ([单击以查看实际尺寸的图像](caching-data-with-the-objectdatasource-cs/_static/image11.png))


最后，将下拉列表设置到 （无） 插入和删除选项卡中，单击完成。 Visual Studio 将在完成配置数据源向导，请设置 ObjectDataSource s`OldValuesParameterFormatString`属性`original_{0}`。 中所述[概述的插入、 更新和删除数据](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)教程中，此属性需要从声明性语法中删除或重新设置为其默认值， `{0}`，以便我们更新工作流保存到继续执行而未出现错误。

此外，在向导完成 Visual Studio 将字段添加到 GridView 为每个产品数据字段。 删除所有但`ProductName`， `CategoryName`，和`UnitPrice`BoundFields。 接下来，更新`HeaderText`的每个产品、 类别和价格，到这些 BoundFields 属性分别。 由于`ProductName`字段是必填，将 BoundField 转换转换为 TemplateField 并添加到 RequiredFieldValidator `EditItemTemplate`。 同样，将转换`UnitPrice`转换为 TemplateField BoundField 和添加 CompareValidator 以确保用户输入的值为有效的货币值 s 大于或等于零。 除了这些修改，请尝试执行任何美观的更改，如右对齐`UnitPrice`值，或指定的格式设置`UnitPrice`其只读和编辑接口中的文本。

使 GridView 通过检查 GridView s 智能标记中的启用编辑复选框可编辑。 此外请检查启用分页和启用排序复选框。

> [!NOTE]
> 需要评审如何自定义 GridView s 编辑界面？ 如果是这样，回头参考[自定义的数据修改界面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)教程。


[![用于编辑、 排序和分页启用 GridView 支持](caching-data-with-the-objectdatasource-cs/_static/image13.png)](caching-data-with-the-objectdatasource-cs/_static/image12.png)

**图 6**： 启用对编辑，排序和分页的 GridView 支持 ([单击以查看实际尺寸的图像](caching-data-with-the-objectdatasource-cs/_static/image14.png))


进行这些 GridView 修改后, 的 GridView 和 ObjectDataSource s 声明性标记应看起来类似于以下：


[!code-aspx[Main](caching-data-with-the-objectdatasource-cs/samples/sample2.aspx)]

如图 7 所示，可编辑的 GridView 将列出名称、 类别和每个数据库中的产品的价格。 花一些时间来测试页的功能排序结果中，逐页查看它们，并编辑记录。


[![每个产品名称、 类别和价格被列入 Sortable、 Pageable、 可编辑的 GridView](caching-data-with-the-objectdatasource-cs/_static/image16.png)](caching-data-with-the-objectdatasource-cs/_static/image15.png)

**图 7**： 每个产品的名称、 类别和价格被列入 Sortable、 Pageable、 可编辑的 GridView ([单击以查看实际尺寸的图像](caching-data-with-the-objectdatasource-cs/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>步骤 3： 检查时 ObjectDataSource 是请求数据

`Products` GridView 检索要显示通过调用其数据`Select`方法`ProductsDataSource`对象数据源。 此对象数据源创建的业务逻辑层的实例`ProductsBLL`类并调用其`GetProducts()`方法，后者反过来调用数据访问层 s `ProductsTableAdapter` s`GetProducts()`方法。 DAL 方法连接到 Northwind 数据库，并发出配置`SELECT`查询。 此数据随后会返回与 DAL，它打包在`NorthwindDataTable`。 DataTable 对象就会归还 BLL，将其返回给对象数据源，将其返回给 GridView。 然后创建 GridView`GridViewRow`每个对象`DataRow`位于 DataTable，和每个`GridViewRow`最终呈现之后，可返回到客户端的访问者 s 浏览器上显示的 HTML。

这一序列的事件发生 GridView 需要绑定到其基础数据的每个时间。 当首次访问页上，当一个各页之间移动数据的另一个，排序 GridView，或修改通过其内置编辑或删除接口的 GridView 的数据时，将发生这种情况。 如果禁用 GridView 的视图状态，则将在每个回发时以及重新绑定 GridView。 GridView 可以还显式重新绑定到其数据通过调用其`DataBind()`方法。

为了充分认识与其从数据库检索的数据的频率，让我们来显示一条指示正在重新检索数据时消息。 添加名为 GridView 上方的一个标签 Web 控件`ODSEvents`。 清除其`Text`属性并设置其`EnableViewState`属性`false`。 下方标签，添加按钮 Web 控件并设置其`Text`回发到的属性。


[![将标签和按钮添加到页面上方 GridView](caching-data-with-the-objectdatasource-cs/_static/image19.png)](caching-data-with-the-objectdatasource-cs/_static/image18.png)

**图 8**： 将标签和按钮添加到 GridView 页上面 ([单击以查看实际尺寸的图像](caching-data-with-the-objectdatasource-cs/_static/image20.png))


在数据访问工作流，ObjectDataSource 的`Selecting`之前创建的基础对象的事件触发和其配置的方法调用。 创建此事件的事件处理，并添加以下代码：


[!code-csharp[Main](caching-data-with-the-objectdatasource-cs/samples/sample3.cs)]

ObjectDataSource 发出请求时，数据体系结构每次该标签将显示文本选择事件激发。

访问此页面的浏览器中。 当首次访问页时，将显示文本选择事件激发。 单击回发的按钮，并记下的文本会消失 (假设 GridView s`EnableViewState`属性设置为`true`，默认值)。 这是因为在回发时，从其视图状态重构 GridView，因此，不会变为其数据 ObjectDataSource。 排序、 分页，或编辑数据，但是，导致 GridView 重新绑定到数据源，并因此选择事件激发文本随即再次显示。


[![每当 GridView 其数据源重新绑定，显示选择事件激发](caching-data-with-the-objectdatasource-cs/_static/image22.png)](caching-data-with-the-objectdatasource-cs/_static/image21.png)

**图 9**： 每当 GridView 重新绑定到其数据源，将显示选择事件激发 ([单击以查看实际尺寸的图像](caching-data-with-the-objectdatasource-cs/_static/image23.png))


[![单击回发按钮会导致 GridView 必须从其视图状态重新构建](caching-data-with-the-objectdatasource-cs/_static/image25.png)](caching-data-with-the-objectdatasource-cs/_static/image24.png)

**图 10**： 单击回发按钮会导致 GridView 以从其视图状态进行重建 ([单击以查看实际尺寸的图像](caching-data-with-the-objectdatasource-cs/_static/image26.png))


这可能看起来浪费检索每次通过分页或排序数据的数据库数据。 毕竟，因为我们重新使用默认分页，ObjectDataSource 已检索的所有记录时显示的第一页。 即使 GridView 不提供排序和分页支持，每个页面首次访问的任何用户 （以及每次回发，如果禁用了视图状态） 的时间必须检索的数据从数据库。 但如果 GridView 向所有用户显示相同的数据，这些额外数据库请求是多余的。 为什么不缓存从返回的结果`GetProducts()`方法和绑定到 GridView 缓存结果？

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>步骤 4： 缓存使用对象数据源的数据

通过只需设置的几个属性，可以将对象数据源配置为自动缓存其 ASP.NET 数据缓存中检索到的数据。 以下列表总结了 ObjectDataSource 的缓存相关的属性：

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx)必须设置为`true`若要启用缓存。 默认值为 `false`。
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx)的时间，以秒为单位，缓存数据量。 默认值为 0。 ObjectDataSource 仅缓存数据，如果`EnableCaching`是`true`和`CacheDuration`设置为值大于零。
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx)可以将设置为`Absolute`或`Sliding`。 如果`Absolute`，ObjectDataSource 缓存为其检索到的数据`CacheDuration`秒; 如果`Sliding`的数据过期仅后未存取过`CacheDuration`秒。 默认值为 `Absolute`。
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx)使用此属性与现有的缓存依赖项关联的 ObjectDataSource 的缓存项。 ObjectDataSource 的数据条目可以被提前逐出从缓存过期及其关联`CacheKeyDependency`。 此属性通常用于将 SQL 缓存依赖项与 ObjectDataSource 的缓存相关联，主题我们将探讨在将来[使用 SQL 缓存依赖项](using-sql-cache-dependencies-cs.md)教程。

让我们来配置`ProductsDataSource`ObjectDataSource 30 秒内对绝对刻度缓存其数据。 设置 ObjectDataSource s`EnableCaching`属性`true`及其`CacheDuration`为 30 的属性。 保留`CacheExpirationPolicy`属性设置为其默认值， `Absolute`。


[![配置对象数据源在 30 秒内缓存其数据](caching-data-with-the-objectdatasource-cs/_static/image28.png)](caching-data-with-the-objectdatasource-cs/_static/image27.png)

**图 11**： 配置对象数据源在 30 秒内缓存其数据 ([单击以查看实际尺寸的图像](caching-data-with-the-objectdatasource-cs/_static/image29.png))


保存所做的更改并重新访问此页在浏览器。 选择触发的事件文本将显示当你首次访问页上，为最初的数据不在缓存中。 而不是后续回发通过单击回发的按钮，排序，触发的分页，或单击编辑或取消按钮*不*重新显示选择事件激发文本。 这是因为`Selecting`事件才会触发时 ObjectDataSource 获取其数据从其基础的对象;`Selecting`如果从数据缓存中提取数据，不会不会触发事件。

30 秒后，将从缓存逐出数据。 此外可从缓存逐出数据 ObjectDataSource s `Insert`， `Update`，或`Delete`调用方法。 因此，过了 30 秒的更新按钮单击后，排序、 分页，或单击编辑或取消按钮将导致 ObjectDataSource 若要从其基础对象中获取其数据后，显示选择事件激发文本时`Selecting`事件激发。 这些返回的结果放回数据缓存中。

> [!NOTE]
> 如果经常查看选择激发事件文本，即使你预期对象数据源以使用缓存的数据，它可能由于内存约束。 如果没有足够的可用内存，通过对象数据源添加到缓存的数据可能已清理。 如果 ObjectDataSource 不 t 看上去正确缓存的数据或仅缓存数据将个别情况下，关闭一些应用程序以释放内存，然后重试。


图 12 显示了缓存工作流的 ObjectDataSource s。 选择事件激发时文本显示在屏幕上，这是因为数据未在缓存中且必须从基础对象中检索。 此文本时缺失，但是，它 s 因为从缓存的数据。 当从缓存返回的数据执行不需要调用的基础对象和任何数据库查询，因此，该处 s。


![ObjectDataSource 存储和检索其数据从数据缓存](caching-data-with-the-objectdatasource-cs/_static/image30.png)

**图 12**: ObjectDataSource 存储和从数据缓存中检索其数据


每个 ASP.NET 应用程序实例在所有页和访问者之间共享该 s 自己数据缓存。 这意味着跨所有用户访问页同样共享数据缓存内存储的对象数据源的数据。 若要验证这一点，打开`ObjectDataSource.aspx`在浏览器中的页。 当第一次访问该页面，选择激发事件文本将显示 （假设由以前的测试添加到缓存的数据已，到目前为止，退出）。 打开第二个浏览器实例并复制和粘贴到第二个第一个浏览器实例的 URL。 在第二个浏览器实例中，选择激发的事件文本未显示，因为它使用相同的 s 缓存与第一个的数据。

ObjectDataSource 在其检索到的数据插入到缓存时，使用包含的缓存密钥值：`CacheDuration`和`CacheExpirationPolicy`属性值; 对象数据源，指定正在使用的基础业务对象的类型通过[`TypeName`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx)(`ProductsBLL`，在此示例中); 的值`SelectMethod`属性的名称和值中的参数的`SelectParameters`集合; 以及其的值`StartRowIndex`和`MaximumRows`属性，在实现时使用[自定义分页。](../paging-and-sorting/paging-and-sorting-report-data-cs.md)

为这些属性的组合创建的缓存密钥值可确保唯一缓存项，因为这些值发生变化。 例如，在过去教程我们已了解了使用`ProductsBLL`类的`GetProductsByCategoryID(categoryID)`，这将返回为指定类别的所有产品。 一个用户可能要到页并查看饮料具有`CategoryID`为 1。 如果 ObjectDataSource 缓存而不考虑其结果`SelectParameters`值，在另一个用户提供到页以查看调味品时饮料产品已在缓存中时，他们 d 看到缓存的饮料产品而不是调味品。 通过按这些属性进行区分的缓存密钥，其中包括的值`SelectParameters`，ObjectDataSource 维护计算的 beverages 和调味品的单独的缓存项。

## <a name="stale-data-concerns"></a>陈旧数据问题

ObjectDataSource 自动逐出从缓存时的任何一个其项其`Insert`， `Update`，或`Delete`调用方法。 这有助于保护计算机免受陈旧数据完成的页修改的数据时清除缓存条目。 但是，它有可能使用缓存来仍显示过时的数据对象数据源。 在最简单的情况下，它可能是由于直接在数据库内更改的数据。 可能是数据库管理员只需运行修改的某些数据库中记录的脚本。

这种情况下也可能更难以捉摸的方式展开。 尽管 ObjectDataSource 逐出缓存中的其项目数据修改方法之一调用时，删除的缓存的项是属性值的 ObjectDataSource s 特定组合 (`CacheDuration`， `TypeName`， `SelectMethod`，依此类推）。 如果必须使用不同的两个 ObjectDataSources`SelectMethods`或`SelectParameters`，但仍可以中更新同一数据，然后一个 ObjectDataSource 可能更新的行，并使其自己的缓存条目，但相应的行无效的第二个对象数据源仍将提供从已缓存。 我建议你创建页面来展示此功能。 创建显示可编辑的 GridView，从 ObjectDataSource 使用缓存并配置为从获取数据提取其数据的页`ProductsBLL`类的`GetProducts()`方法。 添加另一个可编辑 GridView 和 ObjectDataSource 此页 （或另一个），但可用于此第二个 ObjectDataSource 让它使用`GetProductsByCategoryID(categoryID)`方法。 由于两个 ObjectDataSources`SelectMethod`属性不同，它们将每个具有它们自己的缓存的值。 如果你编辑一个网格中的产品下一次将数据返回到其他网格绑定 （由分页、 排序和等），将仍为旧的、 缓存数据提供服务并不反映已从其他网格所做的更改。

如果您愿意为陈旧的数据，可能和数据的新鲜度是重要的方案使用短满简单地说，只能使用基于时间的满。 如果过时的数据不是可接受的则放弃缓存或使用 SQL 缓存依赖项 (假设它为数据库数据所缓存)。 在将来的教程中，我们将探讨 SQL 缓存依赖项。

## <a name="summary"></a>摘要

在本教程中，我们将探讨 ObjectDataSource s 内置缓存功能。 通过只需设置的几个属性，我们可以指示 ObjectDataSource 缓存返回从指定的结果`SelectMethod`到 ASP.NET 数据缓存。 `CacheDuration`和`CacheExpirationPolicy`属性指示缓存项的持续时间以及它是否是绝对或相对过期机制。 `CacheKeyDependency`属性将与现有的缓存依赖项关联的所有对象数据源的缓存条目。 这可用来收回 ObjectDataSource 的项目从缓存，然后基于时间的过期为止，并通常用于 SQL 缓存依赖项。

由于 ObjectDataSource 只需缓存数据缓存到其值，我们无法以编程方式复制 ObjectDataSource s 内置功能。 它不是 t 意义执行此操作在表示层上，因为 ObjectDataSource 提供在初始状态下，此功能，但我们可以在体系结构的一个单独的层中实现缓存功能。 若要执行此操作，我们将需要重复 ObjectDataSource 所使用的相同逻辑。 我们将探讨如何以编程方式使用数据缓存从体系结构中，在我们的下一教程。

尽情享受编程 ！

## <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [ASP.NET 缓存： 方法和最佳实践](https://msdn.microsoft.com/library/aa478965.aspx)
- [.NET Framework 应用程序的缓存体系结构指南](https://msdn.microsoft.com/library/ee817645.aspx)
- [在 ASP.NET 2.0 中的输出缓存](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Teresa 墨。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[下一篇](caching-data-in-the-architecture-cs.md)
