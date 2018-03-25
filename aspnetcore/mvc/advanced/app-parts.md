---
title: "ASP.NET Core 中的应用程序部件"
author: ardalis
description: "了解如何使用应用程序部件（应用资源的抽象）将应用配置为从程序集中发现功能或避免加载功能。"
manager: wpickett
ms.author: riande
ms.date: 01/04/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 6b855f8725dacc89a7e0607224ef3c19ab9f5676
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="application-parts-in-aspnet-core"></a>ASP.NET Core 中的应用程序部件

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

*应用程序部件*是应用程序资源的一种抽象，可通过它发现控制器、视图组件或标记帮助程序等 MVC 功能。 AssemblyPart 就是一种应用程序部件，用于封装程序集引用以及公开类型和编译引用。 *功能提供程序*使用应用程序部件填充 ASP.NET Core MVC 应用的功能。 应用程序部件的主要用例是允许将应用配置为从程序集中发现（或避免加载）MVC 功能。

## <a name="introducing-application-parts"></a>应用程序部件简介

MVC 应用从[应用程序部件](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart)中加载其功能。 具体而言，[AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) 类表示受程序集支持的应用程序部件。 可以使用这些类发现和加载 MVC 功能，比如控制器、视图组件、标记帮助程序和 Razor 编译源。 [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) 负责跟踪可用于 MVC 应用的应用程序部件和功能提供程序。 配置 MVC 时，可以与 `Startup` 中的 `ApplicationPartManager` 交互：

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => p.ApplicationParts.Add(part));
```

默认情况下，MVC 将搜索依赖项树并查找控制器（甚至在其他程序集中）。 若要加载任意程序集（例如，从在编译时未引用的插件），可以使用应用程序部件。

可以使用应用程序部件*避免*查找特定程序集或位置中的控制器。 可以通过修改 `ApplicationPartManager` 的 `ApplicationParts` 集合，控制应用可用的部件（或程序集）。 `ApplicationParts` 集合中条目的顺序并不重要。 重要的是在使用 `ApplicationPartManager` 配置容器中的服务之前，对该类进行完全配置。 例如，应在调用 `AddControllersAsServices` 之前完全配置 `ApplicationPartManager`。 如果未进行完全配置，就意味着，在该方法调用之后添加的应用程序部件中的控制器不受影响（不注册为服务），这可能导致不正确的应用程序行为。

如果某个程序集包含你不想使用的控制器，则将该程序集从 `ApplicationPartManager` 中删除：

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p =>
    {
        var dependentLibrary = p.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

除了项目的程序集及其从属程序集，`ApplicationPartManager` 还默认包含 `Microsoft.AspNetCore.Mvc.TagHelpers` 和 `Microsoft.AspNetCore.Mvc.Razor` 的部件。

## <a name="application-feature-providers"></a>应用程序功能提供程序

应用程序功能提供程序用于检查应用程序部件，并为这些部件提供功能。 以下 MVC 功能有内置功能提供程序：

* [控制器](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [元数据引用](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [标记帮助程序](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [视图组件](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

功能提供程序从 `IApplicationFeatureProvider<T>` 继承，其中 `T` 是功能的类型。 你可以为上面列出的任意 MVC 功能类型实现自己的功能提供程序。 `ApplicationPartManager.FeatureProviders` 集合中功能提供程序的顺序可能很重要，因为靠后的提供程序可以对前面的提供程序所执行的操作作出反应。

### <a name="sample-generic-controller-feature"></a>示例：泛型控制器功能

默认情况下，ASP.NET Core MVC 会忽略泛型控制器（例如，`SomeController<T>`）。 此示例使用的控制器功能提供程序在默认提供程序后面运行并为指定的类型列表（在 `EntityTypes.Types` 中定义）添加泛型控制器实例：

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

实体类型：

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

将该功能提供程序添加到 `Startup` 中：

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

默认情况下，用于路由的泛型控制器名称的格式为 *GenericController`1[Widget]*，而不是 *Widget*。 以下属性用于修改该名称，以便与控制器使用的泛型类型对应：

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

`GenericController` 类：

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

当请求匹配的路由时，结果如下：

![示例应用的示例输出为“Hello from a generic Sproket controller”。](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>示例：显示可用功能

可循环访问可用于应用的已填充功能，方法为通过[依赖关系注入](../../fundamentals/dependency-injection.md)请求 `ApplicationPartManager`，并用它来填充相应功能的实例：

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

示例输出：

![示例应用的示例输出](app-parts/_static/available-features.png)
