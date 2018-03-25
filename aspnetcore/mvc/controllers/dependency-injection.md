---
title: "控制器中的依赖关系注入"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 118f504311b58258b5a0510477280505135dd2d9
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="dependency-injection-into-controllers"></a>控制器中的依赖关系注入

<a name="dependency-injection-controllers"></a>

作者：[Steve Smith](https://ardalis.com/)

ASP.NET Core MVC 控制器应该通过构造函数显式请求其依赖关系。 在某些情况下，单独的控制器操作可能需要服务，但在控制器级别上请求可能没有意义。 在此情况下，也可在操作方法上选择将服务作为参数注入。

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

## <a name="dependency-injection"></a>依赖关系注入

依赖关系注入是遵循 [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/)（依赖关系反向原则）的方法，允许应用程序由松散耦合的模块组成。 ASP.NET Core 具有对[依赖关系注入](../../fundamentals/dependency-injection.md)的内置支持，能更易于测试和维护应用程序。

## <a name="constructor-injection"></a>构造函数注入

ASP.NET Core 对基于构造函数的依赖关系注入的内置支持扩展到 MVC 控制器。 通过将服务类型作为构造函数参数添加到控制器中，ASP.NET Core 将尝试使用其内置的服务容器来解析该类型。 通常（但不总是）使用接口来定义服务。 例如，如果应用程序具有依赖于当前时间的业务逻辑，则可以注入一个检索时间（而不是对其进行硬编码）的服务，这将允许测试进入使用设置时间的实现。

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


实现这样一个接口，以便在运行时使用系统时钟，此操作很简单：

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


准备就绪后，我们可在控制器中使用服务。 在此情况下，我们在 `HomeController` `Index` 方法中添加了一些逻辑，根据每天的时间向用户显示问候语。

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

如果现在运行应用程序，很可能会遇到错误：

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

当我们尚未在 `Startup` 类的 `ConfigureServices` 方法中配置服务时，会发生此错误。 若要指定应使用 `SystemDateTime` 的实例解析 `IDateTime` 的请求，请将下面列表中突出显示的行添加到 `ConfigureServices` 方法中：

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> 可以使用几种不同的生存期选项（`Transient``Scoped` 或 `Singleton`）中的任意一个来实现此特定服务。 请参阅[依赖关系注入](../../fundamentals/dependency-injection.md)，了解每个作用域选项将如何影响服务的行为。

一旦服务配置完毕，运行应用程序并导航到主页应按预期显示基于时间的消息：

![服务器问候语](dependency-injection/_static/server-greeting.png)

>[!TIP]
> 请参阅[测试控制器逻辑](testing.md)，了解如何显式请求依赖关系，控制器中的 [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) 可轻松测试代码。

ASP.NET Core 的内置依赖关系注入支持请求服务的类只拥有一个构造函数。 如果有多个构造函数，可能会收到如下异常消息：

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

如错误消息所述，仅拥有一个构造函数可更正此问题。 还可[将默认依赖关系注入支持替换为第三方实现](../../fundamentals/dependency-injection.md#replacing-the-default-services-container)，其中许多可支持多个构造函数。

## <a name="action-injection-with-fromservices"></a>FromServices 的操作注入

有时，控制器中不需要多个操作的服务。 在此情况下，将服务作为参数注入操作方法可能是有意义的。 通过标记具有属性 `[FromServices]` 的参数来完成此操作，如下所示：

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a>从控制器访问设置

从控制器中访问应用程序或配置设置是一种常见模式。 此访问应使用[配置](xref:fundamentals/configuration/index)中所述的选项模式。 通常不应使用依赖关系注入直接从控制器请求设置。 最好请求 `IOptions<T>` 实例，其中 `T` 是所需的配置类。

若要使用选项模式，需要创建一个表示选项的类，例如：

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

然后，需要将应用程序配置为使用选项模型，并将配置类添加到 `ConfigureServices` 中的服务集合：

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> 在上面的列表中，我们要将应用程序配置为从 JSON 格式的文件中读取设置。 也可以完全使用代码配置设置，如上面注释的代码所示。 有关进一步的配置选项，请参阅[配置](xref:fundamentals/configuration/index)。

一旦指定了强类型的配置对象（在本例中为 `SampleWebSettings`）并将其添加到服务集合中，就可通过请求 `IOptions<T>` 的实例，从任何 Controller 或 Action 方法请求它（在本例中为 `IOptions<SampleWebSettings>`）。 以下代码显示了如何从控制器请求设置：

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

遵循选项模式，可将设置和配置相互分离，并确保控制器遵循 [separation of concerns](http://deviq.com/separation-of-concerns/)（问题分离），因为它不需要知道如何或在哪里找到设置信息。 由于控制器类中没有 [static cling](http://deviq.com/static-cling/)（静态粘附）或设置类的直接实例化，因此控制器更易于对[测试控制器逻辑](testing.md)进行单元测试。
