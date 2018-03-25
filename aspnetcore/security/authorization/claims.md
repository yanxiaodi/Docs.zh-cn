---
title: "基于声明的授权"
author: rick-anderson
description: "本文档说明如何在 ASP.NET Core 应用中添加声明授权检查。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/claims
ms.openlocfilehash: 608aaa469c5ca20fab8250025804e28e7808122d
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2018
---
# <a name="claims-based-authorization"></a>基于声明的授权

<a name="security-authorization-claims-based"></a>

创建一个标识时它可能会分配一个或多个由受信任方发出的声明。 声明是表示哪些使用者名称值对，可以不哪些的主题。 例如，你可能具有驱动程序的许可证，本地驱动的许可证颁发机构签发。 驱动程序的许可证对其具有你的出生日期。 在这种情况下将声明名称`DateOfBirth`，声明值将是你为出生日期，例如`8th June 1970`和颁发者是驱动的许可证颁发机构。 基于声明的授权，简单地说，将检查声明的值，并允许对基于该值资源的访问。 例如，如果你想夜间俱乐部访问授权过程可能是：

门安全负责人将评估你的出生声明，并且它们是否在授予你访问之前信任颁发者 （驱动许可证机构） 的日期的值。

标识可以包含具有多个值的多个声明，并且可以包含多个相同类型的声明。

## <a name="adding-claims-checks"></a>添加声明检查

声明基于的授权检查声明性-开发人员将它们嵌入在其代码中，针对控制器或者控制器中的某个操作指定当前用户必须拥有，并选择性地声明的值必须保留访问的声明请求的资源。 要求是基于策略的声明，开发人员必须生成并注册表达声明要求的策略。

最简单类型的声明策略如下所示的声明存在，并且不会检查值。

首先，你需要生成和注册策略。 这是作为授权服务配置的一部分，这通常需要一部分花在`ConfigureServices()`中你*Startup.cs*文件。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

在这种情况下`EmployeeOnly`策略将检查是否存在`EmployeeNumber`于当前的标识声明。

你随后可应用策略使用`Policy`属性`AuthorizeAttribute`特性以指定策略名称;

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

`AuthorizeAttribute`特性可以应用于整个控制器，在这种情况，仅匹配策略的标识将在控制器上允许向任何操作的访问。

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

如果您有一个控制器受`AuthorizeAttribute`属性，但想要允许匿名访问你应用的特定操作`AllowAnonymousAttribute`属性。

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }

    [AllowAnonymous]
    public ActionResult VacationPolicy()
    {
    }
}
```

大多数声明附带了一个值。 创建策略时，你可以指定允许的值的列表。 下面的示例将仅成功执行员工的员工数是 1、 2、 3、 4 或 5。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

## <a name="multiple-policy-evaluation"></a>多个策略评估

如果将多个策略应用到的控制器或操作，然后授予访问权限之前也必须传递所有策略。 例如:

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}
```

在上例中任何标识了满足`EmployeeOnly`策略可以访问`Payslip`在控制器上强制执行该策略的操作。 但是为了调用`UpdateSalary`标识必须满足的操作*同时*`EmployeeOnly`策略和`HumanResources`策略。

如果你希望更复杂的策略，如将出生声明的日期，计算年龄字段从它，然后检查年龄为 21 或更低版本，则需要编写[自定义策略处理程序](policies.md)。
