---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
title: "缓存数据在应用程序启动 (C#) |Microsoft 文档"
author: rick-anderson
description: "在任何 Web 应用程序中的某些数据将经常使用，并将不常使用的某些数据。 我们可以改进我们的 ASP.NET 应用程序 b 的性能..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 22ca8efa-7cd1-45a7-b9ce-ce6eb3b3ff95
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
msc.type: authoredcontent
ms.openlocfilehash: 3a618ad702763a59b87336784afd1cb74de06d4c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="caching-data-at-application-startup-c"></a>缓存数据在应用程序启动 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载 PDF](caching-data-at-application-startup-cs/_static/datatutorial60cs1.pdf)

> 在任何 Web 应用程序中的某些数据将经常使用，并将不常使用的某些数据。 我们可以通过提前加载经常使用的数据，称为的方法来改进我们的 ASP.NET 应用程序的性能。 本教程演示主动加载，这是将数据加载到缓存在应用程序启动的一种方法。


## <a name="introduction"></a>介绍

缓存中的演示文稿和缓存层的数据看两个前面的教程。 在[缓存数据与 ObjectDataSource](caching-data-with-the-objectdatasource-cs.md)，我们看使用缓存在表示层中缓存数据的功能的 ObjectDataSource s。 [缓存体系结构中的数据](caching-data-in-the-architecture-cs.md)检查在缓存中新的、 独立缓存层。 这两个使用这些教程*反应加载*中使用数据缓存。 反应加载，每次请求数据时，系统会首先检查如果它在缓存中的 s。 如果没有，它获取原始的源，例如数据库中的数据，然后将其存储在缓存中。 反应加载的主要优点是它易于实现。 其缺点之一跨请求是其不均匀的性能。 假设使用前面教程中的缓存层来显示产品信息页。 当此页是首次访问或访问第一次缓存的数据已逐出由于内存约束或具有已达到指定的到期后时，必须从数据库检索的数据。 因此，这些用户请求将由缓存需要超过可提供的用户请求。

*主动加载*通过备用缓存管理策略性能平滑处理跨请求加载需要的才将其缓存的数据。 通常情况下，主动加载使用某些进程，可定期检查或时已对基础数据的更新通知。 然后，此进程会更新要保持最新的缓存。 主动加载是基础的数据来自慢速数据库连接、 Web 服务或某些其他特别缓慢的数据源尤其有用。 但主动加载此方法是更难实现，因为它需要创建、 管理和部署一个过程来检查更改并更新缓存。

另一个风格的主动加载和我们将在本教程中，浏览的类型将数据加载到缓存在应用程序启动。 此方法非常适合缓存静态数据，如数据库查找表中的记录。

> [!NOTE]
> 有关了解主动式和反应式加载以及专业人员、 利弊和实现建议的列表之间的差异的更深入信息，请参阅[管理缓存的内容](https://msdn.microsoft.com/library/ms978503.aspx)部分[缓存.NET Framework 应用程序的体系结构指南](https://msdn.microsoft.com/library/ms978498.aspx)。


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>步骤 1： 确定哪些数据缓存在应用程序启动

在将以前的两个教程工作很好地与数据，可能会定期更改才会 exorbitantly 长以便生成我们检查缓存的示例使用反应加载。 但如果缓存的数据永远不会更改，使用反应加载的到期时间是多余。 同样，如果正在缓存的数据所需生成极其长时间，则检索这些用户其请求查找缓存空需要 endure 基础数据时等待较长时间。 请考虑缓存静态数据和需要极长的时间才能生成在应用程序启动的数据。

虽然数据库有许多动态，频繁地更改值，但大多数还具有相当大的静态数据。 例如，几乎所有的数据模型具有包含一组固定的选择的特定值的一个或多个列。 A`Patients`数据库表中可能有`PrimaryLanguage`列，其组的值可能是英语、 西班牙语、 法语、 俄语、 日语和等等。 通常，使用实现这些类型的列*查找表*。 而不是存储的字符串英语或法语中的`Patients`，第二个表创建表，通常情况下，具有两个列中的唯一标识符和字符串说明-与每个可能值的记录。 `PrimaryLanguage`中的列`Patients`表查找表中存储的相应的唯一标识符。 图 1 中患者 John Doe s 主要语言是英语，Ed Johnson s 俄语时。


![语言表是通过 Patients 表使用查找表](caching-data-at-application-startup-cs/_static/image1.png)

**图 1**:`Languages`表是通过使用查找表`Patients`表


编辑或创建一个新患者的用户界面将包括允许的语言中的记录由填充的下拉列表`Languages`表。 不使用缓存功能，此接口是每次访问系统必须查询`Languages`表。 这是浪费和不必要因为查找表值进行更改很少，如果有。

我们无法缓存`Languages`使用相同反应加载技术来检查前面的教程中的数据。 反应加载，但是，使用基于时间的到期，就不必要的静态查找表数据。 虽然缓存使用反应加载将无缓存根本更好，最好的方法将主动将查找表数据加载到缓存在应用程序启动。

在本教程中我们将查看如何缓存查找表数据和其他静态信息。

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>第 2 步： 检查缓存数据的不同方式

可以使用各种方法的 ASP.NET 应用程序中以编程方式缓存信息。 我们已介绍了如何在前面的教程中使用数据缓存。 或者，对象可以以编程方式缓存使用*静态成员*或*应用程序状态*。

在使用类，通常必须首先为类实例化之前可以访问其成员。 例如，若要调用我们的业务逻辑层中的类的一个方法，我们必须先创建类的实例：


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample1.cs)]

我们可以调用之前*SomeMethod*或使用*SomeProperty*，我们必须先创建类使用的实例`new`关键字。 *SomeMethod*和*SomeProperty*与特定实例相关联。 这些成员的生存期取决于其关联的对象的生存期。 *静态成员*，另一方面，是变量、 属性和方法之间共享*所有*类的实例，因此，具有与类一样长的生存期。 静态成员表示由关键字`static`。

除了静态成员，可以使用应用程序状态缓存数据。 每个 ASP.NET 应用程序保持一个名称/值集合内所有用户和应用程序页共享该 s。 可以使用访问此集合[`HttpContext`类](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)s [ `Application`属性](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)，并且从 ASP.NET 页的代码隐藏类使用，如下所示：


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample2.cs)]

数据缓存的缓存数据，对于基于时间和依赖项的满、 缓存项属性的更多信息，其含义提供机制提供一个多更丰富的 API。 使用静态成员和应用程序状态，此类功能必须手动添加页面开发人员。 当应用程序的生存期内缓存在应用程序启动的数据，但是，数据缓存的优点是毫无意义。 在本教程中我们将查看有关缓存静态数据使用所有这三种技术的代码。

## <a name="step-3-caching-thesupplierstable-data"></a>步骤 3： 缓存`Suppliers`表数据

Northwind 数据库表我们已实施方法与日期不包括任何传统的查找表。 实现四个数据表中我们 DAL 其值是非静态的所有模型表。 而不是花费时间来将新数据表添加到 DAL，然后新类和方法为 BLL，本教程只需让 s 假设，`Suppliers`表的数据是静态的。 因此，我们无法缓存此数据在应用程序启动。

若要开始，创建一个名为的新类`StaticCache.cs`中`CL`文件夹。


![在 CL 文件夹中创建 StaticCache.cs 类](caching-data-at-application-startup-cs/_static/image2.png)

**图 2**： 创建`StaticCache.cs`类`CL`文件夹


我们需要添加将在启动数据加载到合适的缓存存储中，一个方法，以及从此缓存中返回数据的方法。


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample3.cs)]

上面的代码中使用静态成员变量， `suppliers`、 保存的结果从`SuppliersBLL`类 s`GetSuppliers()`方法，从调用`LoadStaticCache()`方法。 `LoadStaticCache()`方法旨在在应用程序的启动过程中调用。 需要使用供应商数据的任何页面后已在应用程序启动时加载此数据，可以调用`StaticCache`类的`GetSuppliers()`方法。 因此，对数据库的调用，以获取供应商只出现一次，启动应用程序时。

而不是作为缓存存储区中使用的静态成员变量，我们本来也可以或者使用应用程序状态或数据缓存。 下面的代码演示重组使用应用程序状态的类：


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample4.cs)]

在`LoadStaticCache()`，供应商信息存储到应用程序变量*密钥*。 它返回以适合的类型 (`Northwind.SuppliersDataTable`) 从`GetSuppliers()`。 虽然可在 ASP.NET 页使用的代码隐藏类访问应用程序状态`Application["key"]`，我们必须使用的体系结构中`HttpContext.Current.Application["key"]`以获取当前`HttpContext`。

同样，数据缓存可以用作缓存存储区中，如以下代码所示：


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample5.cs)]

若要将项添加到数据缓存的任何基于时间的到期，使用`System.Web.Caching.Cache.NoAbsoluteExpiration`和`System.Web.Caching.Cache.NoSlidingExpiration`作为输入参数的值。 数据缓存 s 此特定重载`Insert`已选择方法，因此，我们还可以指定*优先级*的缓存项。 优先级用于确定要清除从缓存中，当可用内存不足时哪些项。 我们在此处使用优先级`NotRemovable`，这可确保此缓存项获胜 t 清理。

> [!NOTE]
> 此教程的下载实现`StaticCache`类使用的静态成员变量的方法。 中的类文件中的注释提供了应用程序状态和数据缓存技术的代码。


## <a name="step-4-executing-code-at-application-startup"></a>步骤 4： 执行在应用程序启动代码

若要执行代码，web 应用程序首次启动时，我们需要创建一个名为的特殊文件`Global.asax`。 此文件可以包含事件处理程序应用程序-，会话，并且请求级事件，也是如此此处我们可在其中添加每当应用程序启动时将执行的代码。

添加`Global.asax`到 web 应用程序的根目录通过右键单击 Visual Studio 的解决方案资源管理器中的网站项目名称并选择添加新项的文件。 从添加新项对话框中，选择全局应用程序类项目类型，然后单击添加按钮。

> [!NOTE]
> 如果你已有`Global.asax`文件在项目中，全局项类型将不会列出在添加新项对话框中的应用程序类。


[![将 Global.asax 文件添加到 Web 应用程序的根目录](caching-data-at-application-startup-cs/_static/image4.png)](caching-data-at-application-startup-cs/_static/image3.png)

**图 3**： 添加`Global.asax`到你的 Web 应用程序 s 根目录的文件 ([单击以查看实际尺寸的图像](caching-data-at-application-startup-cs/_static/image5.png))


默认值`Global.asax`文件模板包括服务器端中的五个方法`<script>`标记：

- **`Application_Start`**在 web 应用程序第一次启动时执行
- **`Application_End`**在应用程序关闭时运行
- **`Application_Error`**执行时未处理的异常到达应用程序
- **`Session_Start`**在创建新的会话时执行
- **`Session_End`**运行时会话已过期或已放弃

`Application_Start` S 应用程序生命周期内仅一次调用事件处理程序。 在应用程序启动第一次 ASP.NET 资源从应用程序请求，继续运行，直到重新启动应用程序时，这可以通过修改内容的情况可能发生`/Bin`文件夹中，修改`Global.asax`，修改在内容`App_Code`文件夹，或修改`Web.config`文件，在其他原因。 请参阅[ASP.NET 应用程序生命周期概述](https://msdn.microsoft.com/library/ms178473.aspx)有关应用程序生命周期的更多详细讨论。

在本系列教程我们只需将代码添加到`Application_Start`方法，因此随意删除其他。 在`Application_Start`，只需调用`StaticCache`类的`LoadStaticCache()`方法，它将加载并缓存供应商信息：


[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample6.aspx)]

该 s 就是它 ！ 在应用程序启动`LoadStaticCache()`方法将获取供应商信息从 BLL，并将其存储在静态成员变量 (或任何缓存存储区中使用结束`StaticCache`类)。 若要验证此行为中, 设置断点`Application_Start`方法并运行应用程序。 请注意，在应用程序启动时命中断点。 后续请求，但是，不会导致`Application_Start`要执行的方法。


[![使用验证 Application_Start 事件处理程序正在执行的断点](caching-data-at-application-startup-cs/_static/image7.png)](caching-data-at-application-startup-cs/_static/image6.png)

**图 4**： 使用验证断点，`Application_Start`事件处理程序是正在执行 ([单击以查看实际尺寸的图像](caching-data-at-application-startup-cs/_static/image8.png))


> [!NOTE]
> 如果你不会命中`Application_Start`断点首次启动调试时，这是因为已经启动了你的应用程序。 强制应用程序重新启动通过修改你`Global.asax`或`Web.config`文件，然后重试。 你可以只需添加 （或删除） 空行末尾的这些文件之一来快速重新启动应用程序。


## <a name="step-5-displaying-the-cached-data"></a>步骤 5： 显示缓存的数据

此时`StaticCache`类具有在应用程序启动，可以通过访问缓存的供应商数据的版本其`GetSuppliers()`方法。 若要使用此数据与表示层，我们可以使用对象数据源，或以编程方式调用`StaticCache`类的`GetSuppliers()`从 ASP.NET 页的代码隐藏类的方法。 让我们来看如何使用对象数据源和 GridView 控件以显示缓存供应商信息。

首先打开`AtApplicationStartup.aspx`页面`Caching`文件夹。 将 GridView 拖动从工具箱中拖动到设计器中，设置其`ID`属性`Suppliers`。 接下来，从 GridView s 智能标记选择创建名为新 ObjectDataSource `SuppliersCachedDataSource`。 配置对象数据源以使用`StaticCache`类的`GetSuppliers()`方法。


[![配置对象数据源以使用 StaticCache 类](caching-data-at-application-startup-cs/_static/image10.png)](caching-data-at-application-startup-cs/_static/image9.png)

**图 5**： 配置对象数据源以使用`StaticCache`类 ([单击以查看实际尺寸的图像](caching-data-at-application-startup-cs/_static/image11.png))


[![使用 GetSuppliers() 方法来检索缓存的供应商数据](caching-data-at-application-startup-cs/_static/image13.png)](caching-data-at-application-startup-cs/_static/image12.png)

**图 6**： 使用`GetSuppliers()`方法来检索缓存供应商数据 ([单击以查看实际尺寸的图像](caching-data-at-application-startup-cs/_static/image14.png))


完成向导后，Visual Studio 将自动添加 BoundFields 中的数据字段的每个`SuppliersDataTable`。 GridView 和 ObjectDataSource s 声明性标记应类似于以下形式：


[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample7.aspx)]

图 7 显示通过浏览器查看时的页。 输出都是相同我们具有请求的数据来自 BLL s`SuppliersBLL`类，但使用`StaticCache`类返回作为缓存在应用程序启动的供应商数据。 你可以在设置断点`StaticCache`类的`GetSuppliers()`方法以验证此行为。


[![缓存供应商数据显示在一个 GridView](caching-data-at-application-startup-cs/_static/image16.png)](caching-data-at-application-startup-cs/_static/image15.png)

**图 7**: 缓存供应商数据将显示在一个 GridView ([单击以查看实际尺寸的图像](caching-data-at-application-startup-cs/_static/image17.png))


## <a name="summary"></a>摘要

大多数每个数据模型包含大量的静态数据，在查找表的形式中通常实现。 由于此信息是静态的那里 s 没有理由不断地访问数据库每次需要显示此信息。 此外，由于其静态性质，缓存数据时那里 s 无需到期时间。 在本教程中我们已了解如何执行此类数据并将数据缓存，应用程序状态，或通过静态成员变量对其进行缓存。 此信息缓存在应用程序启动，并保留在缓存在应用程序 s 整个生存期内。

在本教程和过去两个，我们已讨论过的应用程序 s 有效期限的持续时间内缓存数据，以及使用基于时间的满。 当缓存数据库的数据，不过，基于时间的到期可能并不理想。 而不是定期刷新缓存，则可以仅逐出缓存的项，修改基础数据库数据时的最佳。 这种理想状态是可能通过 SQL 缓存的依赖项，我们将查看我们的下一教程中使用。

尽情享受编程 ！

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Teresa 墨和 Zack Jones。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](caching-data-in-the-architecture-cs.md)
[下一页](using-sql-cache-dependencies-cs.md)
