---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
title: "缓存的体系结构 (C#) 中的数据 |Microsoft 文档"
author: rick-anderson
description: "在以前的教程，我们学习了如何应用缓存，在表示层上。 在本教程中，我们将了解如何充分利用我们分层 architectu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: d29a7c41-0628-4a23-9dfc-bfea9c6c1054
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
msc.type: authoredcontent
ms.openlocfilehash: 0b068b3020b5c454519950e436115a7efa044fb4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="caching-data-in-the-architecture-c"></a>缓存的体系结构 (C#) 中的数据
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载示例应用程序](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_CS.exe)或[下载 PDF](caching-data-in-the-architecture-cs/_static/datatutorial59cs1.pdf)

> 在以前的教程，我们学习了如何应用缓存，在表示层上。 在本教程中，我们将了解如何利用我们分层体系结构在业务逻辑层的缓存数据。 我们执行此操作通过扩展体系结构来容纳缓存层。


## <a name="introduction"></a>介绍

正如我们看到在前面的教程，缓存 ObjectDataSource 的数据非常简单，设置几个属性。 遗憾的是，ObjectDataSource 适用缓存，在表示层，这紧密的缓存策略与 ASP.NET 页上。 创建分层体系结构的原因之一是允许此类 couplings 中断。 数据访问层分离数据访问的详细信息时，业务逻辑层，例如，将从 ASP.NET 页的业务逻辑脱耦。 业务逻辑和数据访问详细信息这种分离是首选方法，在某种，因为它使系统，可读性更强、 更易于维护，并更灵活地更改。 它还允许域知识和的表示层不 t 上的开发人员需要是熟悉数据库 s 详细信息，才能完成其工作的工作划分。 分离与表示层的缓存策略提供类似的好处。

在本教程中，我们将增加我们体系结构来容纳*缓存层*（或简称 CL） 使用我们的缓存策略。 缓存层将包括`ProductsCL`提供等方法访问产品信息的类`GetProducts()`， `GetProductsByCategoryID(categoryID)`，依此类推，调用时，该，将从缓存中检索数据的第一次尝试。 如果缓存为空，则这些方法将调用相应`ProductsBLL`BLL，反过来将从 DAL 获取数据中的方法。 `ProductsCL`方法缓存然后再返回它从 BLL 检索的数据。

如图 1 所示，CL 驻留的演示文稿和业务逻辑层之间。


![缓存层 (CL) 是我们的体系结构中的另一个层](caching-data-in-the-architecture-cs/_static/image1.png)

**图 1**: 缓存层 (CL) 是我们的体系结构中的另一个层


## <a name="step-1-creating-the-caching-layer-classes"></a>步骤 1： 创建缓存层类

在本教程中我们将创建单个类具有非常简单 CL`ProductsCL`具有只有几种方法。 为整个应用程序将需要创建生成完整的缓存层`CategoriesCL`， `EmployeesCL`，和`SuppliersCL`类，并提供每个数据访问或修改方法中 BLL 这些缓存层类中的方法。 与 BLL 和 DAL 中，缓存层应理想情况下实现为单独的类库项目;但是，我们将它作为实现中的类`App_Code`文件夹。

从 DAL 和 BLL 类的多个完全单独 CL 类，让 s 创建新的子文件夹中`App_Code`文件夹。 右键单击`App_Code`文件夹在解决方案资源管理器，选择新文件夹，并将新文件夹命名`CL`。 创建此文件夹后, 添加到该新类名为`ProductsCL.cs`。


![添加新的文件夹名为 CL 和一个名为 ProductsCL.cs 类](caching-data-in-the-architecture-cs/_static/image2.png)

**图 2**： 添加一个名为的新文件夹`CL`和一个名为类`ProductsCL.cs`


`ProductsCL`类应包含相同的数据访问和修改方法，如在其相应的业务逻辑层类集 (`ProductsBLL`)。 而不是创建所有这些方法，让的 s 只需几个此处以针对的模式感受使用的生成按 CL。 具体而言，我们将添加`GetProducts()`和`GetProductsByCategoryID(categoryID)`步骤 3 中的方法和`UpdateProduct`重载在步骤 4 中。 你可以添加剩余`ProductsCL`方法和`CategoriesCL`， `EmployeesCL`，和`SuppliersCL`类在方便的时候。

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>步骤 2： 读取和写入数据缓存

缓存功能内部探讨在前面的教程，ObjectDataSource 使用 ASP.NET 数据缓存来存储从 BLL 检索的数据。 数据缓存在从 ASP.NET 页代码隐藏类或从 web 应用程序 s 体系结构中的类也可以以编程方式访问。 若要对读取和写入数据缓存从 ASP.NET 页的代码隐藏类，使用以下模式：


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample1.cs)]

[ `Cache`类](https://msdn.microsoft.com/library/system.web.caching.cache.aspx)s [ `Insert`方法](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx)具有大量重载。 `Cache["key"] = value`和`Cache.Insert(key, value)`是同义词并同时添加到缓存中使用指定的密钥，而无需定义到期的一个项。 通常情况下，我们想要时将项添加到缓存中，作为依赖关系和 / 或基于时间的过期时间指定到期时间。 使用的其他`Insert`方法的重载，可提供基于依赖项或时间的过期信息。

S 方法需要先检查所请求的数据是否在缓存中，如果是，缓存层会将其返回从该处中。 如果请求的数据不在缓存中，相应的 BLL 方法需要调用。 其返回值应缓存，则返回的值，如下面的序列图所示。


![缓存层的方法在如果从缓存返回数据它 s 可用](caching-data-in-the-architecture-cs/_static/image3.png)

**图 3**: 缓存层的方法在如果从缓存返回数据它 s 可用


图 3 所示的顺序是使用以下模式 CL 类中实现的：


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample2.cs)]

在这里，*类型*是存储在缓存中的数据类型`Northwind.ProductsDataTable`，例如时*密钥*是用于唯一标识的缓存项键。 如果具有指定的项*密钥*不在缓存中，然后*实例*将`null`同时将从相应的 BLL 方法检索的数据，并将其添加到缓存。 按时间`return instance`为止，*实例*包含数据，从缓存的引用，或从 BLL 请求。

请务必从缓存访问数据时使用上面的模式。 下面的模式，该从表面看，链接的外观等效，包含引入了争用条件细微差别。 争用条件很难调试，因为它们偶尔揭示本身，并且难以重现。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample3.cs)]

第二个中的差异，不正确的代码段是，而不是在本地变量中存储对缓存项的引用，在条件语句中直接访问数据缓存*和*中`return`。 当达到此代码时，假设，`Cache["key"]`为非`null`，但之前`return`达到语句、 系统逐出*密钥*从缓存。 在此罕见情况下，代码将返回`null`值而不是预期类型的对象。

> [!NOTE]
> 数据缓存是线程安全的因此您不需要访问进行同步线程对于简单的读 / 写。 但是，如果需要多个对数据执行操作需要以保证不可分割的缓存中，你负责实现锁或某种其他机制来确保线程安全。 请参阅[同步访问 ASP.NET 缓存](http://www.ddj.com/184406369)有关详细信息。


项可以以编程方式从使用数据缓存逐出[`Remove`方法](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx)如下所示：


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample4.cs)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>步骤 3： 返回产品信息从`ProductsCL`类

本教程让实现两种方法来返回中的产品信息的 s`ProductsCL`类：`GetProducts()`和`GetProductsByCategoryID(categoryID)`。 与类似`ProductsBL`业务逻辑层中的类`GetProducts()`中 CL 方法返回有关所有作为产品信息`Northwind.ProductsDataTable`对象，而`GetProductsByCategoryID(categoryID)`返回指定类别中所有产品。

下面的代码演示中的方法的一部分`ProductsCL`类：


[!code-vb[Main](caching-data-in-the-architecture-cs/samples/sample5.vb)]

首先，请注意`DataObject`和`DataObjectMethodAttribute`特性应用于类和方法。 这些特性向 ObjectDataSource 的向导中，指示类和方法应显示的内容在向导的 s 步骤中提供信息。 因为将从 ObjectDataSource 在表示层访问 CL 类和方法，所以我中添加这些属性以增强的设计时体验。 将回指[创建业务逻辑层](../introduction/creating-a-business-logic-layer-cs.md)教程有关这些属性和其效果的更全面说明。

在`GetProducts()`和`GetProductsByCategoryID(categoryID)`方法、 从返回的数据`GetCacheItem(key)`方法分配给本地变量。 `GetCacheItem(key)`方法，我们将很快检查，从缓存根据指定返回特定项*密钥*。 如果在缓存中未不找到任何此类数据，则将它检索从相应`ProductsBLL`类方法，并随后添加到缓存使用`AddCacheItem(key, value)`方法。

`GetCacheItem(key)`和`AddCacheItem(key, value)`方法分别接口使用数据缓存、 读取和写入值。 `GetCacheItem(key)`方法是更简单的两个。 它只需返回的值从传入的使用缓存的类*密钥*:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample6.cs)]

`GetCacheItem(key)`不使用*密钥*值提供，而是调用`GetCacheKey(key)`方法，它返回*密钥*前面带有 ProductsCache-。 `MasterCacheKeyArray`，其中包含的字符串 ProductsCache，也可由`AddCacheItem(key, value)`方法，如我们所见短暂出现。

从 ASP.NET 页的代码隐藏类，则数据可以访问缓存使用`Page`类 s [ `Cache`属性](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)，并允许进行这样的语法`Cache["key"] = value`，在步骤 2 中所述。 从体系结构中的类，则数据可以访问缓存使用`HttpRuntime.Cache`或`HttpContext.Current.Cache`。 [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx)的博客文章[HttpRuntime.Cache vs。HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache)说明中使用的略微的性能优点`HttpRuntime`而不是`HttpContext.Current`; 因此，`ProductsCL`使用`HttpRuntime`。

> [!NOTE]
> 如果使用类库项目实现你的体系结构，则将需要添加对的引用`System.Web`若要使用的程序集[HttpRuntime](https://msdn.microsoft.com/library/system.web.httpruntime.aspx)和[HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)类。


如果在缓存中，未找到该项目`ProductsCL`类的方法从 BLL 获取数据并将其添加到缓存使用`AddCacheItem(key, value)`方法。 若要添加*值*到缓存中，我们可以使用下面的代码，使用 60 秒的时间到期：


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample7.cs)]

`DateTime.Now.AddSeconds(CacheDuration)`在将来的段中指定的基于时间的到期时间 60 秒[ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx)指示没有 s 不会滑动过期。 虽然这`Insert`方法重载具有输入参数的这两个绝对，滑动到期，你可以仅提供两种状态之一。 如果你尝试指定一个绝对时间和时间跨度，`Insert`方法会引发`ArgumentException`异常。

> [!NOTE]
> 此实现的`AddCacheItem(key, value)`方法当前存在一些不足。 我们解决，克服在步骤 4 中的这些问题。


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>步骤 4： 失效缓存时数据是修改通过体系结构

数据检索方法，以及缓存层需要提供相同的方法中，同时作为 BLL 插入、 更新和删除数据。 CL 的数据修改方法不需要修改缓存的数据，而是调用 BLL s 相应数据修改方法但然后使缓存失效的。 正如我们看到在前面的教程，这是相同的行为时启用其缓存功能 ObjectDataSource 应用并将其`Insert`， `Update`，或`Delete`调用方法。

以下`UpdateProduct`重载演示如何在 CL 中实现数据修改方法：


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample8.cs)]

调用适当的数据修改业务逻辑层方法时，但返回其响应之前，我们需要使缓存失效的。 遗憾的是，从而导致无效缓存不是简单直接因为`ProductsCL`类 s`GetProducts()`和`GetProductsByCategoryID(categoryID)`方法每个项向缓存添加具有不同键和`GetProductsByCategoryID(categoryID)`方法将不同的缓存项添加每个唯一*categoryID*。

当使缓存失效，我们需要删除*所有*已通过添加的项`ProductsCL`类。 这可以通过将相关联来实现*缓存依赖项*与每个项添加到缓存中`AddCacheItem(key, value)`方法。 通常情况下，缓存依赖项可以是缓存、 文件系统或 Microsoft SQL Server 数据库中的数据上的文件中的另一个项。 依赖项更改或是时从缓存中删除，与之关联的缓存项将自动从缓存中逐出。 对于本教程，我们想要创建可用作所有项的缓存依赖项添加到缓存中的附加项`ProductsCL`类。 这样一来，所有这些项可以是从缓存中删除通过只需删除缓存依赖项。

让的 s 更新`AddCacheItem(key, value)`方法，以便每个项通过此方法添加到缓存是单个缓存依赖项与相关联：


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample9.cs)]

`MasterCacheKeyArray`是一个字符串数组，包含单个值，ProductsCache。 首先，缓存项添加到缓存中并分配的当前日期和时间。 如果缓存项已存在，则它更新内容。 接下来，创建缓存依赖项。 [ `CacheDependency`类](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx)s 构造函数具有大量的重载，但在此处中正在使用需要两个`string`数组输入。 第一个指定要用作依赖项的文件集。 由于我们不希望使用任何基于文件的依赖关系，值为 t`null`用于第一个输入参数。 第二个输入的参数指定缓存密钥要用作依赖项的集。 在这里，我们指定我们依赖项， `MasterCacheKeyArray`。 `CacheDependency`然后传递到`Insert`方法。

在此修改`AddCacheItem(key, value)`、 invaliding 缓存非常简单，只删除的依赖关系。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample10.cs)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>步骤 5： 从表示层调用缓存层

缓存层的类和方法可以用于处理数据使用的技术我们已检查整个这些教程。 为了说明使用缓存数据，你将更改保存至`ProductsCL`类，然后打开`FromTheArchitecture.aspx`页面`Caching`文件夹并添加一个 GridView。 从 GridView s 智能标记，创建新对象数据源。 在向导 s 第一步，你应看到`ProductsCL`类作为一个下拉列表中的选项。


[![ProductsCL 类包含在业务对象下拉列表](caching-data-in-the-architecture-cs/_static/image5.png)](caching-data-in-the-architecture-cs/_static/image4.png)

**图 4**:`ProductsCL`类包括在业务对象下拉列表中 ([单击以查看实际尺寸的图像](caching-data-in-the-architecture-cs/_static/image6.png))


选择后`ProductsCL`，单击下一步。 下拉列表中选择的选项卡上有两项-`GetProducts()`和`GetProductsByCategoryID(categoryID)`和更新选项卡具有唯一`UpdateProduct`重载。 选择`GetProducts()`从选择的选项卡上的方法和`UpdateProducts`方法从该更新选项卡，然后单击完成。


[![下拉列表中列出了 ProductsCL 类的方法](caching-data-in-the-architecture-cs/_static/image8.png)](caching-data-in-the-architecture-cs/_static/image7.png)

**图 5**:`ProductsCL`下拉列表中列出了类的方法 ([单击以查看实际尺寸的图像](caching-data-in-the-architecture-cs/_static/image9.png))


完成向导后，Visual Studio 将设置 ObjectDataSource s`OldValuesParameterFormatString`属性`original_{0}`并将相应的字段添加到 GridView。 更改`OldValuesParameterFormatString`回其默认值，属性`{0}`，和配置以支持分页、 排序和编辑 GridView。 由于`UploadProducts`重载使用 CL 接受编辑的产品的名称和价格，限制 GridView，以便仅这些字段是可编辑。

在前面的教程，我们定义一个 GridView，若要包含的字段`ProductName`， `CategoryName`，和`UnitPrice`字段。 请尝试复制此格式设置和结构，在这种情况下声明性你 GridView 和 ObjectDataSource 的标记应类似于以下：


[!code-aspx[Main](caching-data-in-the-architecture-cs/samples/sample11.aspx)]

此时我们有一个使用缓存层的页面。 若要查看操作中的缓存，在中设置断点`ProductsCL`类 s`GetProducts()`和`UpdateProduct`方法。 当从缓存中提取排序和分页才能查看的数据时，请访问浏览器和逐句通过代码中的页。 然后更新的记录，并请注意，缓存失效，因此，它从 BLL 时检索数据重新绑定到 GridView。

> [!NOTE]
> 下载伴随这篇文章中提供了缓存层不完整。 它包含只有一个类， `ProductsCL`，其中仅运动少量的方法。 此外，仅将一个 ASP.NET 页面使用 CL (`~/Caching/FromTheArchitecture.aspx`) 所有其他用户仍 BLL 直接引用。 如果你计划在你的应用程序中使用 CL，与表示层的所有调用都应都发送到 CL，将要求 CL 的类和方法涵盖这些类和 BLL 当前使用的表示层中的方法。


## <a name="summary"></a>摘要

时可在表示层为 ASP.NET 2.0 的 SqlDataSource 和 ObjectDataSource 控件应用缓存，理想情况下缓存职责会将委派给一个单独的层体系结构中。 在本教程中我们创建一个表示层和业务逻辑层之间驻留的缓存层。 需要提供一组相同的类和方法中 BLL 存在并从表示层调用缓存层。

我们探讨了在此示例和前面教程的缓存层示例表现出*反应加载*。 反应加载数据加载到缓存，仅当发出数据请求，该数据是从缓存缺少时。 也可以是数据*主动加载*到缓存中，一种技术，将数据加载到缓存之前实际需要。 在下一教程中我们将看到举例说明主动加载我们查看如何将静态值存储到缓存在应用程序启动时。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Teresa Murph。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](caching-data-with-the-objectdatasource-cs.md)
[下一页](caching-data-at-application-startup-cs.md)
