---
uid: web-api/overview/advanced/dependency-injection
title: "ASP.NET Web API 2 中的依赖关系注入 |Microsoft 文档"
author: MikeWasson
description: "本教程演示如何将依赖关系注入到你的 ASP.NET Web API 控制器。 在教程的 Web API 2 Unity 应用程序块中使用的软件版本..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 7f64cc83e36c80b0ffd53edfc629557c0847b200
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="dependency-injection-in-aspnet-web-api-2"></a>ASP.NET Web API 2 中的依赖关系注入
====================
通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> 本教程演示如何将依赖关系注入到你的 ASP.NET Web API 控制器。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - Web API 2
> - [Unity 应用程序块](https://www.nuget.org/packages/Unity/)
> - 实体框架 6 （也适用版本 5）


## <a name="what-is-dependency-injection"></a>什么是依赖关系注入？

A*依赖*是另一个对象需要的任何对象。 例如，很容易定义[存储库](http://martinfowler.com/eaaCatalog/repository.html)用于处理数据访问。 让我们举例说明了。 首先，我们将定义域模型：

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

此处是一个简单的存储库类，将项存储在数据库中，使用 Entity Framework。

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

现在让我们定义支持 GET 请求的一个 Web API 控制器`Product`实体。 （我已经将出 POST 和为简单起见其他方法。）下面是第一次尝试：

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

请注意，取决于控制器类`ProductRepository`，我们会让创建控制器和`ProductRepository`实例。 但是，它的原因是为硬编码的依赖关系，这种方式，是个好主意。

- 如果你想要替换`ProductRepository`使用不同的实现，你还需要修改控制器类。
- 如果`ProductRepository`具有依赖关系，你必须配置这些控制器中。 对于具有多个控制器的大型项目，你的配置代码变得分散在你的项目。
- 很难进行单元测试，因为控制器是硬编码来查询数据库。 对于单元测试，则应使用一个模型或存根 （stub） 存储库，这是不可能与当前设计。

我们可以解决这些问题的*将注入*插入控制器的存储库。 首先，重构`ProductRepository`到接口的类：

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

然后提供`IProductRepository`作为构造函数参数：

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

此示例使用[构造函数注入](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)。 你还可以使用*setter 注入*，则将通过 setter 方法或属性的依赖关系的设置。

但现在没有问题，因为你的应用程序不直接创建控制器。 Web API 可创建控制器，当它将路由请求，并且 Web API 不知道任何有关`IProductRepository`。 这是 Web API 依赖项解析程序传入的位置。

## <a name="the-web-api-dependency-resolver"></a>Web API 依赖项解析程序

Web API 定义**IDependencyResolver**用于解析的依赖关系接口。 下面是接口的定义：

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

**IDependencyScope**接口有两种方法：

- **GetService**创建类型的一个实例。
- **GetServices**创建指定类型的对象的集合。

**IDependencyResolver**方法继承**IDependencyScope**并添加**如何**方法。 我将在本教程后面讨论的作用域。

当 Web API 创建的控制器实例时，首先调用**IDependencyResolver.GetService**，并传入控制器类型。 此扩展性挂钩，可用于创建控制器上，解决任何依赖关系。 如果**GetService** ，则返回 null，Web API 控制器类上的无参数构造函数的如下所示。

## <a name="dependency-resolution-with-the-unity-container"></a>依赖项解析与 Unity 容器

尽管你可以编写的完整**IDependencyResolver**从零开始，该接口的实现真正旨在作为 Web API 和现有 IoC 容器之间的桥。

IoC 容器是一个软件组件，负责管理依赖关系。 你向容器中，注册类型，然后使用容器以创建对象。 容器自动计算出的依赖关系。 多个 IoC 容器还可用于控制对象生存期和作用域等事务。

> [!NOTE]
> "IoC"代表"反向的控件"，它是常规的模式其中一个框架，调入应用程序代码。 IoC 容器构造对象，其中"反转"常用控制流。


对于本教程中，我们将使用[Unity](https://msdn.microsoft.com/library/ff647202.aspx)从 Microsoft 模式&amp;做法。 (其他常用库包括[城堡 Windsor](http://www.castleproject.org/)， [Spring.Net](http://www.springframework.net/)， [Autofac](https://code.google.com/p/autofac/)， [Ninject](http://www.ninject.org/)，和[StructureMap](http://docs.structuremap.net/).)NuGet 包管理器可用于安装 Unity。 从**工具**菜单在 Visual Studio 中，选择**库程序包管理器**，然后选择**程序包管理器控制台**。 在 Package Manager Console 窗口中，键入以下命令：

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

下面是实现**IDependencyResolver**包装 Unity 容器。

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> 如果**GetService**方法不能解析类型，它应返回**null**。 如果**GetServices**方法不能解析类型，则它应返回一个空集合对象。 不引发异常的未知类型。


## <a name="configuring-the-dependency-resolver"></a>配置依赖项解析程序

设置依赖项解析程序上**DependencyResolver**的全局属性**HttpConfiguration**对象。

下面的代码注册`IProductRepository`与 Unity 之间的接口，然后创建`UnityResolver`。

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>依赖项作用域和控制器生存期

每个请求创建控制器。 若要管理对象生存期**IDependencyResolver**使用的概念*作用域*。

依赖项解析程序附加到**HttpConfiguration**对象具有全局作用域。 当 Web API 可创建控制器时，它将调用**如何**。 此方法返回**IDependencyScope**表示子范围。

然后 web API 调用**GetService**在子范围，可创建控制器上。 完成请求时，Web API 调用**释放**在子作用域上。 使用**释放**方法来释放控制器的依赖关系。

如何实现**如何**取决于 IoC 容器。 For Unity，作用域对应于子容器：

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

大多数 IoC 容器具有类似的等效项。
