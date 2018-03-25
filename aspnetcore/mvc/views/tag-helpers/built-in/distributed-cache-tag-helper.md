---
title: "ASP.NET Core 中的分布式缓存标记帮助程序"
author: pkellner
description: "演示如何使用缓存标记帮助程序"
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 710477732b865e2e3821102d34545bbd4e0a5919
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="distributed-cache-tag-helper"></a>分布式缓存标记帮助程序

作者：[Peter Kellner](http://peterkellner.net) 


分布式缓存标记帮助程序将其内容缓存到分布式缓存源，从而大幅提高 ASP.NET Core 应用的性能。

分布式缓存标记帮助程序与缓存标记帮助程序继承自相同的基类。  与缓存标记帮助程序相关的所有属性也将适用于分布式标记帮助程序。


分布式缓存标记帮助程序遵循被称为“构造函数注入”的显式依赖关系原则。  具体而言，将 `IDistributedCache` 接口容器传递给分布式缓存标记帮助程序的构造函数。  如果未在 `ConfigureServices` 中创建 `IDistributedCache` 的特定具体实现（通常在 startup.cs 中找到），则分布式缓存标记帮助程序将使用与基本缓存标记帮助程序所用相同的内存中提供程序来存储缓存数据。

## <a name="distributed-cache-tag-helper-attributes"></a>分布式缓存标记帮助程序属性

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority

有关定义，请参阅缓存标记帮助程序。 分布式缓存标记帮助程序与缓存标记帮助程序继承自相同的类，因此其属性都是缓存标记帮助程序中的常见属性。

- - -

### <a name="name-required"></a>name（必需）

| 属性类型    | 示例值     |
|----------------   |----------------   |
| 字符串    | "my-distributed-cache-unique-key-101"     |

必需的 `name` 属性用作为分布式缓存标记帮助程序的每个实例存储的缓存的键。  分布式缓存标记帮助程序与基础缓存标记帮助程序不同：后者基于 Razor 页面名称和 Razor 页面中标记帮助程序的位置，向每个缓存标记帮助程序实例分配一个键；前者的键仅基于属性 `name`

用法示例：

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>分布式缓存标记帮助程序 IDistributedCache 实现

ASP.NET Core 中内置了 `IDistributedCache` 的两个实现。  一个基于 Sql Server，另一个基于 Redis。 有关这些实现的详细信息，请参阅下方引用的名为“使用分布式缓存”的资源。 这两个实现都涉及在 ASP.NET Core 的 startup.cs 中设置 `IDistributedCache` 的实例。

没有专门与使用 `IDistributedCache` 的任何特定实现相关的标记属性。



- - -



## <a name="additional-resources"></a>其他资源

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
