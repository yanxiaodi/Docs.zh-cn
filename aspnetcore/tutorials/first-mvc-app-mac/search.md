---
title: "将搜索添加到 ASP.NET Core MVC 应用"
author: rick-anderson
description: "演示如何将搜索添加到简单的 ASP.NET Core MVC 应用"
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: d0e42242fd8c05b538e5e2e2686b77c95e9b90f4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

请注意：SQLlite 区分大小写，因此需搜索“Ghost”而非“ghost”。

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

在“Views\movie\Index.cshtml”Razor 视图中更改 `<form>` 标记以指定 `method="get"`：

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
[上一篇 - 控制器方法和视图](controller-methods-views.md)
[下一篇 - 添加字段](new-field.md)
