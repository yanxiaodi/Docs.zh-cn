---
title: "ASP.NET Core 中的图像标记帮助程序"
author: pkellner
description: "演示如何使用图像标记帮助程序"
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 75bddd01a95f3ae0b1ea19de0eb64ad3b9066319
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="imagetaghelper"></a>ImageTagHelper

作者：[Peter Kellner](http://peterkellner.net) 

图像标记帮助程序可增强 `img` (`<img>`) 标记。 它需要 `src` 标记以及 `boolean` 属性 `asp-append-version`。

如果图像源 (`src`) 是主机 Web 服务器上的静态文件，则会向图像源追加一个唯一缓存清除字符串作为查询参数。 这可确保，如果主机 Web 服务器上的文件发生更改，将生成包含已更新请求参数的唯一请求 URL。 缓存清除字符串是一个唯一值，表示静态图像文件的哈希。

如果图像源 (`src`) 不是静态文件（例如远程 URL，或者服务器上不存在该文件），则生成的 `<img>` 标记的 `src` 属性不带缓存清除查询字符串参数。

## <a name="image-tag-helper-attributes"></a>图像标记帮助程序属性


### <a name="asp-append-version"></a>asp-append-version

与 `src` 属性一起指定时，会调用图像标记帮助程序。

有效的 `img` 标记帮助程序示例为：

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

如果目录 *..wwwroot/images/asplogo.png* 中存在静态文件，生成的 html 与下面类似（哈希有所不同）：

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

分配给参数 `v` 的值是磁盘上的文件的哈希值。 如果 Web 服务器无法获取对所引用的静态文件的读取访问权限，则不会向 `src` 属性添加 `v` 参数。

- - -

### <a name="src"></a>src

若要激活图像标记帮助程序，`<img>` 元素需要有 src 属性。 

> [!NOTE]
> 图像标记帮助程序使用本地 Web 服务器上的 `Cache` 提供程序来存储给定文件的已计算 `Sha512`。 如果再次请求该文件，则不需要重新计算 `Sha512`。 当计算该文件的 `Sha512` 时，附加到该文件的文件观察程序会让 Cache 失效。

## <a name="additional-resources"></a>其他资源

* <xref:performance/caching/memory>
