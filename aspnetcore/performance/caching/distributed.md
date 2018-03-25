---
title: "使用 ASP.NET Core 中分布式缓存"
author: ardalis
description: "了解如何使用 ASP.NET Core 分布式缓存以提高应用性能和可伸缩性，尤其是在云或服务器场环境。"
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/distributed
ms.openlocfilehash: 635c61cbb72a6a9eb822307bbc80936ee73bedc8
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="working-with-a-distributed-cache-in-aspnet-core"></a>使用 ASP.NET Core 中分布式缓存

作者：[Steve Smith](https://ardalis.com/)

分布式的缓存可以提高性能和可伸缩性的 ASP.NET Core 应用，尤其是在托管在云中或服务器场环境中。 此文章介绍了如何使用 ASP.NET 核心内置的分布式的缓存抽象和实现。

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

## <a name="what-is-a-distributed-cache"></a>什么是分布式的缓存

分布式的缓存共享由多个应用程序服务器 (请参阅[缓存的基础知识](memory.md#caching-basics))。 在缓存中的信息不存储在单独的 web 服务器的内存和缓存的数据可供所有应用程序的服务器。 这提供了几个优点：

1. 缓存的数据是所有 web 服务器上一致。 用户看不到不同的结果，具体取决于哪个 web 服务器处理他们的请求

2. 缓存的数据在 web 服务器重新启动和部署能存在。 单独的 web 服务器可以删除或添加而不会影响缓存。

3. 源数据存储区具有对它 （不是使用多个内存中缓存或否缓存根本） 发出的少数几个请求。

> [!NOTE]
> 如果使用 SQL Server 分发的缓存，但某些带来如下好处是仅当对应用程序的源数据的缓存比使用单独的数据库实例，则返回 true。

任何缓存，如分布式的缓存可以显著提高应用的响应能力，因为通常可以从比快得多的关系数据库 （或 web 服务） 从缓存中检索数据。

缓存配置是特定的实现。 本文介绍如何配置同时 Redis 和 SQL Server 分布式缓存。 无论选择哪一种实现，则应用程序与使用一组公共缓存交互`IDistributedCache`接口。

## <a name="the-idistributedcache-interface"></a>IDistributedCache 接口

`IDistributedCache`接口包含同步和异步方法。 接口允许添加、 检索和删除从分布式的缓存实现的项。 `IDistributedCache`接口包括以下方法：

**Get、 GetAsync**

采用字符串键和检索缓存的项作为`byte[]`如果缓存中找到。

**集 SetAsync**

添加一项 (作为`byte[]`) 到使用字符串键的缓存。

**刷新 RefreshAsync**

刷新缓存基于其密钥，重置其滑动到期超时值 （如果有） 中的项。

**删除 RemoveAsync**

删除基于其键在某个缓存项。

若要使用`IDistributedCache`接口：

   1. 将所需的 NuGet 包添加到你的项目文件。

   2. 配置的特定实现`IDistributedCache`中你`Startup`类的`ConfigureServices`方法，并将其添加到容器存在。

   3. 从应用程序的[中间件](xref:fundamentals/middleware/index)MVC 控制器类，请求的实例或`IDistributedCache`从构造函数。 实例将由提供[依赖关系注入](../../fundamentals/dependency-injection.md)(DI)。

> [!NOTE]
> 没有无需使用的单一实例或作用域生存期`IDistributedCache`实例 (至少对于内置实现)。 你还可以创建一个实例，只要你可能需要一个 (而不是使用[依赖关系注入](../../fundamentals/dependency-injection.md))，但这会导致你的代码更难若要测试，并且与冲突[显式依赖关系原则](http://deviq.com/explicit-dependencies-principle/)。

下面的示例演示如何使用的实例`IDistributedCache`简单中间件组件中：

[!code-csharp[](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]

在上面的代码中，缓存的值是读取，但永远不会写入。 在此示例中，值只能设置当服务器启动，并不会更改。 在多服务器方案中，最新的服务器以开始将覆盖任何以前的值已设置的其他服务器。 `Get`和`Set`方法使用`byte[]`类型。 因此，字符串必须将值转换使用`Encoding.UTF8.GetString`(有关`Get`) 和`Encoding.UTF8.GetBytes`(有关`Set`)。

下面的代码从*Startup.cs*显示设置的值：

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]

> [!NOTE]
> 由于`IDistributedCache`中配置`ConfigureServices`方法，它可供`Configure`作为参数的方法。 将其添加作为参数将允许通过 DI 提供配置的实例。

## <a name="using-a-redis-distributed-cache"></a>使用分布式的 Redis 缓存

[Redis](https://redis.io/)是一个开放源代码内存中数据存储区，通常用来作为分布式缓存。 你可以在本地，使用它，并且你可以配置[Azure Redis 缓存](https://azure.microsoft.com/services/cache/)为你的 Azure 托管的 ASP.NET Core 应用。 ASP.NET Core 应用程序将配置为使用缓存实现`RedisDistributedCache`实例。

配置中的 Redis 实现`ConfigureServices`和通过请求的实例应用程序代码中访问它`IDistributedCache`（请参阅上面的代码）。

在示例代码中，`RedisCache`时服务器配置为使用实现`Staging`环境。 因此`ConfigureStagingServices`方法配置`RedisCache`:

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]

> [!NOTE]
> 若要在本地计算机上安装 Redis，安装 chocolatey 程序包[https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/)并运行`redis-server`从命令提示符。

## <a name="using-a-sql-server-distributed-cache"></a>使用 SQL Server 分布式缓存

SqlServerCache 实现允许分布式的缓存使用作为其后备存储的 SQL Server 数据库。 若要创建 SQL Server 表中，可以使用 sql 缓存工具，该工具，请与你指定的名称和架构中创建一个表。

若要使用 sql 缓存工具，添加`SqlConfig.Tools`到`<ItemGroup>`元素*.csproj*文件，运行 dotnet 还原。

[!code-xml[](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]

通过运行以下命令来测试 SqlConfig.Tools

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create --help
   ```

现在，你可以运行"创建 sql 缓存"命令入 sql server，创建表，sql 缓存工具将显示使用情况、 选项和命令的帮助：

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
   info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
       Table and index were created successfully.
   ```

创建的表具有以下架构：

![Sql Server 缓存表](distributed/_static/SqlServerCacheTable.png)

所有的缓存实现，如你的应用程序应获取和设置缓存值，使用的实例`IDistributedCache`，而不`SqlServerCache`。 此示例实现`SqlServerCache`中`Production`环境 (以便在配置`ConfigureProductionServices`)。

[!code-csharp[](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]

> [!NOTE]
> `ConnectionString` (和 （可选）`SchemaName`和`TableName`) 通常应在因为它们可能包含凭据存储在源控件 （如 UserSecrets)，之外。

## <a name="recommendations"></a>建议

在决定的哪一种实现`IDistributedCache`是适合你的应用，选择 Redis 之间，SQL Server 基于现有基础结构和环境、 性能要求和你的团队的体验。 如果你的团队更喜欢使用 Redis，它是一个理想的选择。 如果你的团队倾向于 SQL Server，您可以确信中以及该实现。 请注意，传统的缓存解决方案将存储数据的内存中用于进行快速检索的数据。 你应在缓存中存储常用的数据并将整个数据存储在 SQL Server 或 Azure 存储空间等后端持久存储区中。 Redis 缓存是为你提供高吞吐量和低延迟相比 SQL 缓存的缓存解决方案。

## <a name="additional-resources"></a>其他资源

* [Redis 在 Azure 上的缓存](https://azure.microsoft.com/documentation/services/redis-cache/)
* [在 Azure 上的 SQL 数据库](https://azure.microsoft.com/documentation/services/sql-database/)
* [内存中缓存](xref:performance/caching/memory)
* [使用更改令牌检测更改](xref:fundamentals/primitives/change-tokens)
* [响应缓存](xref:performance/caching/response)
* [响应缓存中间件](xref:performance/caching/middleware)
* [缓存标记帮助程序](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分布式缓存标记帮助程序](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
