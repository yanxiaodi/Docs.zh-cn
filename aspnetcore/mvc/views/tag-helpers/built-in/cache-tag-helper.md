---
title: "ASP.NET Core MVC 中的缓存标记帮助程序"
author: pkellner
description: "演示如何使用缓存标记帮助程序"
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 51811ee1669a24a0fc4ce9bc67e782b61bff655c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>ASP.NET Core MVC 中的缓存标记帮助程序

作者：[Peter Kellner](http://peterkellner.net) 

缓存标记帮助程序通过将其内容缓存到内部 ASP.NET Core 缓存提供程序中，可极大地提高 ASP.NET Core 应用的性能。

Razor 视图引擎将默认的 `expires-after` 设置为 20 分钟。

以下 Razor 标记将缓存日期/时间：

```cshtml
<cache>@DateTime.Now</cache>
```

针对包含 `CacheTagHelper` 页面的第一个请求将显示当前的日期/时间。 其他请求将显示已缓存的值，直到缓存过期（默认 20 分钟）或被内存压力逐出。

可使用以下属性设置缓存持续时间：

## <a name="cache-tag-helper-attributes"></a>缓存标记帮助程序属性

- - -

### <a name="enabled"></a>enabled    


| 属性类型    | 有效值      |
|----------------   |----------------   |
| boolean           | “true”（默认值）  |
|                   | “false”   |


确定是否缓存了缓存标记帮助程序所包含的内容。 默认值为 `true`。  如果设置为 `false`，则此缓存标记帮助程序对于呈现的输出没有缓存效果。

示例:

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a>expires-on 

| 属性类型    | 示例值     |
|----------------   |----------------   |
| DateTimeOffset    | "@new DateTime(2025,1,29,17,02,0)"    |


设置一个绝对到期日期。 以下示例将在 2025 年 1 月 29 日下午 5:02 之前缓存缓存标记帮助程序的内容。

示例:

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a>expires-after

| 属性类型    | 示例值     |
|----------------   |----------------   |
| TimeSpan    | "@TimeSpan.FromSeconds(120)"    |


设置从第一个请求时间到缓存内容的时间长度。 

示例:

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a>expires-sliding

| 属性类型    | 示例值     |
|----------------   |----------------   |
| TimeSpan    | "@TimeSpan.FromSeconds(60)"     |


设置缓存条目在未被访问时应被逐出的时间。

示例:

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a>vary-by-header

| 属性类型    | 示例值                |
|----------------   |----------------               |
| String            | "User-Agent"                  |
|                   | "User-Agent,content-encoding" |

接受单个标头值或逗号分隔的标头值列表，在更改时触发缓存刷新。 以下示例监视标头值 `User-Agent`。 该示例将缓存提供给 Web 服务器的每个不同 `User-Agent` 的内容。

示例:

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a>vary-by-query

| 属性类型    | 示例值                |
|----------------   |----------------               |
| String            | "Make"                |
|                   | "Make,Model" |

接受单个标头值或逗号分隔的标头值列表，当标头值更改时触发缓存刷新。 以下示例查看 `Make` 和 `Model` 的值。

示例:

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a>vary-by-route

| 属性类型    | 示例值                |
|----------------   |----------------               |
| String            | "Make"                |
|                   | "Make,Model" |

接受单个标头值或逗号分隔的标头值列表，当路由数据参数值更改时触发缓存刷新。 示例:

*Startup.cs* 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
Index.cshtml

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a>vary-by-cookie

| 属性类型    | 示例值                |
|----------------   |----------------               |
| String            | ".AspNetCore.Identity.Application"                |
|                   | ".AspNetCore.Identity.Application,HairColor" |

接受单个标头值或逗号分隔的标头值列表，当标头值更改时触发缓存刷新。 以下示例查看与ASP.NET Identity 相关联的 cookie。 当用户经过身份验证时，要设置的请求 cookie 将触发缓存刷新。

示例:

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a>vary-by-user

| 属性类型    | 示例值                |
|----------------   |----------------               |
| Boolean             | “true”                  |
|                     | “false”（默认值） |

指定当已登录用户（或上下文主体）更改时是否应该重置缓存。 当前用户也称为请求上下文主体，可通过引用 `@User.Identity.Name` 在 Razor 视图中查看。

以下示例查看当前登录的用户。  

示例:

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

通过登录和注销周期，使用此属性将内容维护在缓存中。  使用 `vary-by-user="true"` 时，登录和注销操作会使经过身份验证的用户的缓存无效。  缓存无效，因为登录时会生成一个新的唯一 cookie 值。 当 cookie 不存在或已过期时，则维持缓存以呈现匿名状态。 这意味着如果没有用户登录，将维持缓存。

- - -

### <a name="vary-by"></a>vary-by

| 属性类型    | 示例值                |
|----------------   |----------------               |
| String             | “@Model”                 |


允许自定义缓存的数据。 当属性的字符串值引用的对象发生更改时，会更新缓存标记帮助程序的内容。 通常将模型值的字符串串联分配给此属性。  从效果上看，这意味着更新任何已连接的值都会使缓存无效。

以下示例假定视图的控制器方法将两个路由参数 `myParam1` 和 `myParam2` 的整数值相加，并将其作为单个模型属性返回。 当此总和更改时，会再次呈现并缓存缓存标记帮助程序的内容。  

示例:

操作：

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

Index.cshtml

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a>priority

| 属性类型    | 示例值                |
|----------------   |----------------               |
| CacheItemPriority  | "High"                   |
|                    | "Low" |
|                    | "NeverRemove" |
|                    | "Normal" |

为内置缓存提供程序提供缓存逐出指导。 在内存压力下，Web 服务器将首先逐出 `Low` 缓存条目。

示例:

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

`priority` 属性并不能保证特定级别的缓存保留。 `CacheItemPriority` 仅供参考。 将此属性设置为 `NeverRemove` 并不能保证缓存将始终保留。 有关详细信息，请参阅[其他资源](#additional-resources)。

缓存标记帮助程序依赖于[内存缓存服务](xref:performance/caching/memory)。 如果尚未添加该服务，缓存标记帮助程序将添加。

## <a name="additional-resources"></a>其他资源

* [内存中缓存](xref:performance/caching/memory)
* [标识简介](xref:security/authentication/identity)
