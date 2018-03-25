---
title: "ASP.NET 核心 mvc 视图基于授权"
author: rick-anderson
description: "本文档演示如何将注入和利用 ASP.NET 核心 Razor 视图内的授权服务。"
manager: wpickett
ms.author: riande
ms.date: 10/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/views
ms.openlocfilehash: 22754d07882cd704309a4e1a28ad0bf6f69432ea
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="view-based-authorization"></a>基于视图的授权

开发人员通常需要显示、 隐藏或修改基于当前的用户标识的用户界面。 你可以访问授权服务在服务内通过的 MVC 视图[依赖关系注入](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)。 若要将授权服务注入到 Razor 视图中，使用`@inject`指令：

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

如果希望每个视图中的授权服务，将放置`@inject`指令插入*_ViewImports.cshtml*文件*视图*目录。 有关详细信息，请参阅[到视图的依赖关系注入](xref:mvc/views/dependency-injection)。

使用插入的授权服务来调用`AuthorizeAsync`中完全相同的方式将检查期间[基于资源的授权](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

在某些情况下，资源将视图模型。 调用`AuthorizeAsync`中完全相同的方式将检查期间[基于资源的授权](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

在前面的代码中，该模型作为资源应采取的策略评估传递纳入考虑范围。

> [!WARNING]
> 不要依赖于与唯一的授权检查的应用程序的 UI 元素的切换可见性。 隐藏的 UI 元素可能无法完全阻止访问到其关联的控制器操作。 例如，考虑前面的代码段中的按钮。 用户可以调用`Edit`操作方法如果他或她知道的相对资源 URL 是*/Document/Edit/1*。 为此，`Edit`操作方法应执行其自己的授权检查。
