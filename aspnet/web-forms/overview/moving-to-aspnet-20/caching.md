---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: "缓存 |Microsoft 文档"
author: microsoft
description: "了解缓存很重要的性能良好的 ASP.NET 应用程序。 ASP.NET 1.x 提供缓存; 的三个不同的选项输出缓存..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 9b229de60e09b94189f62a6bb6fa61a9973d637b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="caching"></a>缓存
====================
by [Microsoft](https://github.com/microsoft)

> 了解缓存很重要的性能良好的 ASP.NET 应用程序。 ASP.NET 1.x 提供缓存; 的三个不同的选项输出缓存、 片段缓存和缓存 API。


了解缓存很重要的性能良好的 ASP.NET 应用程序。 ASP.NET 1.x 提供缓存; 的三个不同的选项输出缓存、 片段缓存和缓存 API。 ASP.NET 2.0 提供所有这三种方法，但它添加了一些重要的其他功能。 有几个新的缓存依赖项和开发人员现在可以选择创建自定义缓存依赖项。 缓存的配置具有也已得到明显改进在 ASP.NET 2.0。

## <a name="new-features"></a>新增功能

## <a name="cache-profiles"></a>缓存配置文件

缓存配置文件允许开发人员定义特定的缓存设置，然后应用于各个页面。 例如，如果必须在 12 小时后应从缓存过期某些页，你可以轻松创建可应用于这些网页的缓存配置文件。 若要添加新的缓存配置文件，使用&lt;outputCacheSettings&gt;配置文件中的部分。 例如，下面是名为缓存配置文件的配置*twoday*配置缓存持续时间为 12 小时。

[!code-xml[Main](caching/samples/sample1.xml)]

若要将此缓存配置文件应用于特定页，请使用 @ OutputCache 指令的 CacheProfile 属性，如下所示：

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>自定义缓存依赖项

自定义缓存依赖项的情况下，ASP.NET 1.x 开发人员 cried 状态。 在 ASP.NET 中 1.x，CacheDependency 类已密封的哪些不的开发人员从其派生各自的类。 在 ASP.NET 2.0 中，已取消该限制，而且开发人员可用来开发其自己的自定义缓存依赖项。 CacheDependency 类可以创建基于文件、 目录或缓存密钥的自定义缓存依赖项。

例如，下面的代码创建新的自定义缓存依赖项基于调用 stuff.xml 位于 Web 应用程序的根目录中的文件：

[!code-csharp[Main](caching/samples/sample3.cs)]

在此方案中，当 stuff.xml 文件发生更改，缓存的项会失效。

还有可能要创建使用缓存密钥的自定义缓存依赖项。 使用此方法，则将使缓存键删除失效的缓存的数据。 下面的示例阐释了这一点：

[!code-csharp[Main](caching/samples/sample4.cs)]

要使之无效的上方已插入的项，只需删除已插入缓存，以充当缓存键的项。

[!code-csharp[Main](caching/samples/sample5.cs)]

请注意，用作缓存键的项键必须添加到缓存密钥的数组的值相同。

## <a name="polling-based-sql-cache-dependenciesalso-called-table-based-dependencies"></a>轮询的基于 SQL 缓存依赖项*（也称为基于表的依赖项）*

SQL Server 7 和 2000年将基于轮询的模型用于 SQL 缓存依赖项。 基于轮询的模型使用的数据库表的表中的数据更改时触发的触发器。 将触发更新**changeId** ASP.NET 会定期检查通知表中的字段。 如果**changeId**已更新字段，ASP.NET 知道数据已更改，并且它的缓存的数据失效。

> [!NOTE]
> SQL Server 2005 还可以使用基于轮询的模型中，但由于基于轮询的模型不是最有效的模型，最好使用与 SQL Server 2005 （稍后讨论） 的基于查询的模型。


为了使 SQL 缓存依赖项使用轮询基于模型正常工作，这些表必须具有启用的通知。 这可以实现使用 SqlCacheDependencyAdmin 类以编程方式或通过使用 aspnet\_regsql.exe 实用程序。

下面的命令行位于名为的 SQL Server 实例上的 Northwind 数据库中注册产品表*dbase* sql 缓存依赖项。

[!code-console[Main](caching/samples/sample6.cmd)]

下面是上述命令中使用的命令行开关的说明：

| **命令行开关** | **目的** |
| --- | --- |
| -S *server* | 指定服务器名称。 |
| -ed | 指定数据库，应启用 SQL 缓存依赖项。 |
| -d*数据库\_名称* | 指定应为 SQL 缓存依赖项启用的数据库名称。 |
| -E | 指定该 aspnet\_regsql 连接到数据库时应使用 Windows 身份验证。 |
| -et | 指定我们要启用 SQL 缓存依赖项的数据库表。 |
| -t*表\_名称* | 指定要为 SQL 缓存依赖项启用的数据库表的名称。 |

> [!NOTE]
> 没有其他开关可用于 aspnet\_regsql.exe。 有关完整列表，运行 aspnet\_regsql.exe-？ 从命令行。


此命令运行时对 SQL Server 数据库进行以下更改：

- **AspNet\_SqlCacheTablesForChangeNotification**添加表。 此表包含每个表为其启用 SQL 缓存依赖项的数据库中的一行。
- 在数据库内创建以下存储的过程：


| AspNet\_SqlCachePollingStoredProcedure | 查询 AspNet\_SqlCacheTablesForChangeNotification 表并返回的所有表都启用了 SQL 缓存依赖项和每个表的 changeId 的值。 此存储的过程进行轮询用于确定数据是否已更改。 |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | 返回的所有表为 SQL 缓存依赖项启用通过查询 AspNet\_SqlCacheTablesForChangeNotification 表并返回所有表都启用 sql 缓存依赖项。 |
| AspNet\_SqlCacheRegisterTableStoredProcedure | 通过在通知表中添加必要的入口注册 SQL 缓存依赖项的表，并将触发器添加。 |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | 通过通知表中删除的项注销表以获取 SQL 缓存依赖项并删除该触发器。 |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | 通过将递增已更改的表 changeId 更新通知表。 ASP.NET 使用此值以确定数据是否已更改。 如下所示，通过启用表时创建的触发器执行此存储的过程。 |


- SQL Server 触发器调用 ***表\_名称 *\_AspNet\_SqlCacheNotification\_触发器**为该表创建。 此触发器将执行 AspNet\_SqlCacheUpdateChangeIdStoredProcedure 对表执行插入、 更新或删除时。
- SQL Server 角色称为**aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess**添加到数据库。

**Aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** SQL Server 角色具有 EXEC 权限到 AspNet\_SqlCachePollingStoredProcedure。 为了使轮询模型正常工作，必须将进程帐户添加到 aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess 角色。 Aspnet\_regsql.exe 工具将执行此操作为你。

### <a name="configuring-polling-based-sql-cache-dependencies"></a>配置轮询基于 SQL 缓存依赖项

有几个步骤所需的配置的基于轮询的 SQL 缓存依赖项。 第一步是启用数据库和表，如上文所述。 该步骤完成后，配置的其余部分是，如下所示：

- 配置 ASP.NET 配置文件。
- 配置 SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>配置 ASP.NET 配置文件

除了在前一模块中所述，将添加连接字符串，还必须配置&lt;缓存&gt;具有元素&lt;sqlCacheDependency&gt;元素如下所示：

[!code-xml[Main](caching/samples/sample7.xml)]

此配置在上启用 SQL 缓存依赖项*pubs*数据库。 请注意，pollTime 属性中&lt;sqlCacheDependency&gt;元素默认值为 60000 毫秒或 1 分钟。 （此值不能少于 500 毫秒）。在此示例中，&lt;添加&gt;元素将添加一个新的数据库和替代 pollTime，将其设置为 9000000 毫秒。

#### <a name="configuring-the-sqlcachedependency"></a>配置 SqlCacheDependency

下一步是配置 SqlCacheDependency。 完成此操作的最简单方法是，如下所示在 @ Outcache 指令中指定 SqlDependency 属性的值：

[!code-aspx[Main](caching/samples/sample8.aspx)]

在上面的 @ OutputCache 指令，SQL 缓存依赖项配置为*作者*表中*pubs*数据库。 可以通过用分号分隔配置多个依赖关系如下所示：

[!code-aspx[Main](caching/samples/sample9.aspx)]

配置 SqlCacheDependency 的另一种方法是以编程方式执行操作。 下面的代码上创建新的 SQL 缓存依赖项*作者*表中*pubs*数据库。

[!code-csharp[Main](caching/samples/sample10.cs)]

以编程方式定义 SQL 缓存依赖项的优势之一在于，你可以处理可能会发生任何异常。 例如，如果你尝试定义尚未为通知，启用数据库的 SQL 缓存依赖项**DatabaseNotEnabledForNotificationException**将引发异常。 在这种情况下，你可以尝试通过调用启用通知的数据库**SqlCacheDependencyAdmin.EnableNotifications**方法并将其传递的数据库名称。

同样，如果你尝试定义尚未为通知，启用表的 SQL 缓存依赖项**TableNotEnabledForNotificationException**将引发。 然后，你可以调用**SqlCacheDependencyAdmin.EnableTableForNotifications**方法将其传递的数据库名称和表名称。

下面的代码示例演示如何配置 SQL 缓存依赖项时，正确配置异常处理。

[!code-csharp[Main](caching/samples/sample11.cs)]

详细信息： [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>基于查询的 SQL 缓存依赖项 (仅 SQL Server 2005)

在使用 SQL Server 2005 的 SQL 缓存依赖项时，基于轮询的模型不是必需的。 SQL 缓存依赖项时与 SQL Server 2005 一起使用，通过 SQL 连接到 SQL Server 实例 （不需要任何进一步的配置） 直接通信使用 SQL Server 2005 查询通知。

若要启用基于查询的通知的最简单方法是以声明方式设置进行**SqlCacheDependency**到的数据源对象属性**CommandNotification**并设置**EnableCaching**属性设为**true**。 使用此方法，则不不需要的任何代码。 如果命令的结果执行针对数据源更改，它将使缓存数据。

下面的示例将配置数据源控件为 SQL 缓存依赖项：

[!code-aspx[Main](caching/samples/sample12.aspx)]

在此情况下，如果在指定的查询**SelectCommand**返回不同的结果比最初，将缓存的结果会失效。

你还可以指定所有数据源通过设置启用了 SQL 缓存依赖项**SqlDependency**属性**@ OutputCache**指令至**CommandNotification**. 下面的示例阐释了这一点。

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> 有关 SQL Server 2005 中的查询通知的详细信息，请参阅 SQL Server 联机丛书。


配置基于查询的 SQL 缓存依赖项的另一种方法是为此，以编程方式使用 SqlCacheDependency 类。 下面的代码示例演示如何完成此操作。

[!code-csharp[Main](caching/samples/sample14.cs)]

详细信息： [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>缓存后替换

对页面进行缓存可以大幅提高 Web 应用程序的性能。 但是，在某些情况下你需要在页后，可以是动态的页面将缓存的大部分内容和某些片段。 例如，如果你创建的新闻故事完全是静态的设定时间段内的页，你可以设置整个页后，可以缓存。 如果你想要包括在每个页请求更改旋转 ad 标题，包含播发的页面的部分将需要是动态的。 若要允许缓存某个页面，但动态替换一些内容，可以使用 ASP.NET 缓存后替换。 使用缓存后替换整个页面进行缓存，并将标记为从缓存中免除的特定部分的输出。 在广告横幅示例中，如何可以充分利用缓存后替换，以便为每个用户和每次刷新页时动态创建广告。

有三种方法可以实现缓存后替换：

- 以声明方式，使用 Substitution 控件。
- 以编程方式，使用 Substitution 控件 API。
- 隐式地使用。

### <a name="substitution-control"></a>Substitution 控件

ASP.NET Substitution 控件指定缓存的页面，它是在可动态创建而非缓存的部分。 你想要显示的动态内容的位置的页上放置 Substitution 控件。 在运行时，Substitution 控件调用的 MethodName 属性与指定的方法。 该方法必须返回一个字符串，然后替换 Substitution 控件的内容。 方法必须是包含的页或 UserControl 控件上的静态方法。 使用 substitution 控件将导致客户端可缓存性更改为服务器可缓存性，以便不将客户端上缓存的页。 这可确保以后对页的请求调用该方法再次以生成动态内容。

### <a name="substitution-api"></a>替换 API

若要以编程方式创建动态内容缓存的页面，你可以调用[WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx)在网页代码中，将其作为参数传递的方法名称的方法。 处理动态内容的创建方法采用单个[HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)参数并返回一个字符串。 返回的字符串是在给定的位置将替换的内容。 调用 WriteSubstitution 方法而不是以声明方式使用 Substitution 控件的一个优点是，你可以调用任意对象，而不是调用的页或 UserControl 对象的静态方法的方法。

WriteSubstitution 方法调用时，客户端可缓存性更改为服务器可缓存性，以便不将客户端上缓存的页。 这可确保以后对页的请求调用该方法再次以生成动态内容。

### <a name="adrotator-control"></a>如何

服务器控件实现 AdRotator 内部支持缓存后替换。 如果你将放置在页面上如何，它会呈现每个请求，而不考虑是否缓存父页上的唯一播发。 因此，包括如何一个页是仅缓存的服务器端。

## <a name="controlcachepolicy-class"></a>ControlCachePolicy 类

ControlCachePolicy 类允许片段缓存使用用户控件的编程控件。 ASP.NET 嵌入内的用户控件[BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx)实例。 BasePartialCachingControl 类表示启用了输出缓存的用户控件。

访问时[BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx)属性[PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx)控件，你始终将收到一个有效的 ControlCachePolicy 对象。 但是，如果你访问[UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx)属性[UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx)控件，你可以收到有效的 ControlCachePolicy 对象，仅当已包装的用户控件BasePartialCachingControl 控件。 如果不换行，该属性返回的 ControlCachePolicy 对象将引发异常，当你尝试对其进行操作，因为它不具有关联的 BasePartialCachingControl。 若要确定 UserControl 实例是否支持缓存不会生成异常，请检查[SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx)属性。

使用 ControlCachePolicy 类是你可以启用输出缓存的几种方式之一。 以下列表描述可用于启用输出缓存的方法：

- 使用[@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx)指令来启用输出缓存中声明性方案。
- 使用[PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx)属性，以便启用缓存的代码隐藏文件中的用户控件。
- ControlCachePolicy 类用于在其中使用 BasePartialCachingControl 实例已启用缓存的使用上述方法之一并使用动态加载的编程方案中指定要缓存设置[System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx)方法。

ControlCachePolicy 实例可以成功操作仅之间控件生命周期的 Init 和 PreRender 阶段。 如果 PreRender 阶段完成之后修改 ControlCachePolicy 对象，ASP.NET 将引发异常，因为任何之后时呈现控件所做的更改实际上不会影响缓存设置 （在呈现器阶段期间缓存控件）。 最后，用户控件实例 （并因此其 ControlCachePolicy 对象） 才可用于以编程方式操作实际呈现时。

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>更改为缓存配置-&lt;缓存&gt;元素

有几个 ASP.NET 2.0 中的缓存配置更改。 &lt;缓存&gt;元素是 ASP.NET 2.0 中的新增功能，允许你在配置文件中进行缓存的配置更改。 以下属性将可用。

| **元素** | **说明** |
| --- | --- |
| **cache** | 可选元素。 定义全局应用程序缓存设置。 |
| **outputCache** | 可选元素。 指定应用程序级输出缓存设置。 |
| **outputCacheSettings** | 可选元素。 指定可应用于应用程序中的页的输出缓存设置。 |
| **sqlCacheDependency** | 可选元素。 为 ASP.NET 应用程序配置 SQL 缓存依赖项。 |

### <a name="the-ltcachegt-element"></a>&lt;缓存&gt;元素

以下属性位于&lt;缓存&gt;元素：

| **特性** | **说明** |
| --- | --- |
| **disableMemoryCollection** | 可选**布尔**属性。 获取或设置一个值，该值指示是否禁用缓存内存集合时发生计算机处于内存压力下。 |
| **disableExpiration** | 可选**布尔**属性。 获取或设置一个值，该值指示是否禁用缓存过期时间。 禁用时，缓存的项不会过期和后台清理的过期的缓存项不会发生。 |
| **privateBytesLimit** | 可选**Int64**属性。 获取或设置一个值，该值指示应用程序的专用字节之前在缓存开始刷新的最大大小过期项并尝试进行回收内存。 此限制包括使用的缓存内存以及正常内存开销从运行的应用程序。 零设置指示，ASP.NET 将使用自己的试探法，用于确定何时开始回收内存。 |
| **percentagePhysicalMemoryUsedLimit** | 可选**Int32**属性。 获取或设置一个值，该值指示在缓存开始刷新之前可由应用程序使用的计算机的物理内存的最大百分比过期项并尝试进行回收此内存使用量包括缓存也使用这两个内存的内存为运行的应用程序的正常内存使用量。 零设置指示，ASP.NET 将使用自己的试探法，用于确定何时开始回收内存。 |
| **privateBytesPollTime** | 可选**TimeSpan**属性。 获取或设置一个值，该值轮询应用程序的专用字节内存使用情况之间的时间间隔。 |

### <a name="the-ltoutputcachegt-element"></a>&lt;OutputCache&gt;元素

以下属性将可用的&lt;outputCache&gt;元素。

| **特性** | **说明** |
| --- | --- |
| **enableOutputCache** | 可选**布尔**属性。 启用/禁用页面输出缓存。 如果禁用，不会缓存页而不考虑以编程方式或声明性设置。 默认值是**true**。 |
| **enableFragmentCache** | 可选**布尔**属性。 启用/禁用应用程序片段缓存。 如果禁用，不会缓存页而不考虑[@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx)指令或缓存使用的配置文件。 包括一个缓存控制标头，该值，上游代理服务器，以及浏览器客户端不应尝试缓存页面输出。 默认值是**false**。 |
| **sendCacheControlHeader** | 可选**布尔**属性。 获取或设置一个值，该值指示是否**缓存的控件： 私有**标头默认情况下发送该输出缓存模块。 默认值是**false**。 |
| **omitVaryStar** | 可选**布尔**属性。 启用/禁用发送 Http"**Vary: \*** "在响应中的标头。 如果使用默认设置为 false，"**Vary: \*** "标头发送为输出缓存页。 当发送 Vary 标头时，它允许不同版本要缓存基于 Vary 标头中指定的内容。 例如， *Vary： 用户-代理*将存储的基于用户代理发出请求的页面的不同版本。 默认值是**false**。 |

### <a name="the-ltoutputcachesettingsgt-element"></a>&lt;OutputCacheSettings&gt;元素

&lt;OutputCacheSettings&gt;元素可以按前面所述的缓存配置文件的创建。 唯一子级元素&lt;outputCacheSettings&gt;元素是&lt;outputCacheProfiles&gt;元素来配置缓存配置文件。

### <a name="the-ltsqlcachedependencygt-element"></a>&lt;SqlCacheDependency&gt;元素

以下属性将可用的&lt;sqlCacheDependency&gt;元素。

| **特性** | **说明** |
| --- | --- |
| **enabled** | 所需**布尔**属性。 指示轮询更改。 |
| **pollTime** | 可选**Int32**属性。 设置与其 SqlCacheDependency 轮询更改的数据库表的频率。 此值对应于连续两次轮询之间的毫秒数。 它不能设置为小于 500 毫秒。 默认值为 1 分钟。 |

### <a name="more-information"></a>详细信息

没有应该注意的有关缓存配置一些其他信息。

- 如果未设置辅助进程专用字节限制，缓存将使用一个以下限制： 

    - x86 2 GB: 800 MB 或 60%的物理 RAM，小者为准
    - x86 3 GB: 1800 MB 或 60%的物理 RAM，小者为准
    - x 64: 1 terabyte 或 60%的物理 RAM，小者为准
- 如果这两个工作进程专用字节限制和&lt;缓存 privateBytesLimit /&gt;的设置，缓存将使用的两个最小值。
- 就像在 1.x，我们删除缓存条目并调用 GC。收集以下两个原因： 

    - 我们会非常接近的专用字节限制
    - 可用内存较接近或低于 10%
- 可以有效地禁用剪裁并可通过设置缓存的可用内存不足条件&lt;缓存 percentagePhysicalMemoryUseLimit /&gt;为 100。
- 与 1.x 不同 2.0 将会挂起剪裁并收集调用如果上一次 GC。收集未减小专用字节或多个 1%（缓存） 内存限制的托管堆的大小。

## <a name="lab1-custom-cache-dependencies"></a>Lab1： 自定义缓存依赖项

1. 创建一个新的 Web 站点。
2. 添加一个名 cache.xml 的新 XML 文件并将其保存到 Web 应用程序的根目录。
3. 将以下代码添加到页\_加载 default.aspx 的代码隐藏文件中的方法： 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. 将以下代码添加到在源视图中的 default.aspx 的顶部： 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. 浏览 Default.aspx。 时间的说是什么？
6. 刷新浏览器。 时间的说是什么？
7. 打开 cache.xml 并添加以下代码： 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. 保存 cache.xml。
9. 刷新浏览器。 时间的说是什么？
10. 说明而不是显示以前缓存的值更新的时间的原因：

## <a name="lab-2-using-polling-based-cache-dependencies"></a>实验室 2： 使用基于轮询的缓存依赖项

此实验室将使用在前一模块，可用于编辑通过 GridView 和说明如何控制 Northwind 数据库中的数据中创建的项目。

1. 在 Visual Studio 2005 中打开项目。
2. 运行 aspnet\_regsql 实用程序，针对要启用数据库和产品表的 Northwind 数据库。 使用以下命令从 Visual Studio 命令提示符： 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. 将以下代码添加到 web.config 文件： 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. 添加调用 showdata.aspx 的新 web 窗体。
5. 将 @ outputcache 指令以下代码添加到 showdata.aspx 页： 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. 将以下代码添加到页\_showdata.aspx 负载： 

    [!code-html[Main](caching/samples/sample21.html)]
7. 将新的 SqlDataSource 控件添加到 showdata.aspx 并将其配置为使用 Northwind 数据库连接。 单击下一步。
8. 选择产品名称和产品 id 复选框，然后单击下一步。
9. 单击完成。
10. 向 showdata.aspx 页中添加一个新的 GridView。
11. 从下拉列表中选择 SqlDataSource1。
12. 保存并浏览 showdata.aspx。 记下显示的时间。
