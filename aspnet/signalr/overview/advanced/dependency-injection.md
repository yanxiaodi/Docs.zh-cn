---
uid: signalr/overview/advanced/dependency-injection
title: "在 SignalR 的依赖关系注入 |Microsoft 文档"
author: MikeWasson
description: "Visual Studio 2013.NET 4.5 SignalR 本主题中使用软件版本的早期版本的信息的本主题的版本 2 早期版本..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-signalr"></a>在 SignalR 的依赖关系注入
====================
通过[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>本主题中使用的软件版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 版本 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>本主题的早期版本
> 
> 有关 SignalR 的早期版本的信息，请参阅[SignalR 较旧版本](../older-versions/index.md)。
> 
> ## <a name="questions-and-comments"></a>问题和意见
> 
> 请留下反馈上如何喜欢本教程的方式，我们可以提高在页面底部的注释中。 如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET SignalR 论坛](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


依赖关系注入是一种方法来删除硬编码对象，使其更方便，若要替换的对象的依赖项，为测试 （使用模拟对象） 或更改运行时行为之间的依赖关系。 本教程演示如何在 SignalR 集线器上执行依赖关系注入。 它还演示如何使用 SignalR IoC 容器。 IoC 容器是一个常规用于依赖关系注入框架。

## <a name="what-is-dependency-injection"></a>什么是依赖关系注入？

如果你已熟悉依赖关系注入，跳过此部分。

*依赖关系注入*(DI) 是一种模式不负责创建其自己的依赖项对象。 下面是一个简单的示例，以促使 DI。 假设你有需要记录消息的对象。 你可以定义日志记录接口：

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

在你的对象，你可以创建`ILogger`记录消息：

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

此方法有效，但它不是最佳设计。 如果你想要替换`FileLogger`与另一个`ILogger`实现中，你将需要修改`SomeComponent`。 Supposing 大量的其他对象使用`FileLogger`，你将需要更改所有这些。 或如果你决定进行`FileLogger`单一实例，你还需要在整个应用程序更改。

更好的方法是以"插入"`ILogger`到对象-例如，通过使用构造函数自变量：

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

现在的对象不是负责选择其`ILogger`使用。 你可以从交换机`ILogger`而无需更改依赖于它的对象的实现。

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

此模式称为[构造函数注入](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)。 另一种模式是 setter 注入，你在其中设置通过 setter 方法或属性的依赖关系。

## <a name="simple-dependency-injection-in-signalr"></a>在 SignalR 的简单的依赖关系注入

本教程中的聊天应用程序，请考虑[Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md)。 下面是从该应用程序的中心类：

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

假设你想要向用户发送之前存储在服务器上的聊天消息。 你可以定义一个接口，提取此功能，并使用 DI 注入到接口`ChatHub`类。

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

唯一的问题是 SignalR 应用程序不会直接创建中心;SignalR 为你创建它们。 默认情况下，SignalR 希望中心类，具有无参数构造函数。 但是，你可以轻松地注册创建中心实例的函数和使用此函数来执行 DI。 注册此函数通过调用**GlobalHost.DependencyResolver.Register**。

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

现在 SignalR 将调用此匿名函数，只要它需要创建`ChatHub`实例。

## <a name="ioc-containers"></a>IoC 容器

前面的代码是相当不错的简单的用例。 但你仍然必须编写此：

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

在具有许多依赖关系的复杂应用程序，你可能需要编写大量的此"连结"代码。 此代码可能难以维护，尤其是在嵌套依赖关系。 也很难进行单元测试。

一种解决方案是使用 IoC 容器。 IoC 容器是一个软件组件，负责管理依赖关系。你向容器中，注册类型，然后使用容器以创建对象。 容器自动计算出的依赖关系。 多个 IoC 容器还可用于控制对象生存期和作用域等事务。

> [!NOTE]
> "IoC"代表"反向的控件"，它是常规的模式其中一个框架，调入应用程序代码。 IoC 容器构造对象，其中"反转"常用控制流。


## <a name="using-ioc-containers-in-signalr"></a>使用 SignalR 在 IoC 容器

聊天应用程序可能是太简单，能够受益于 IoC 容器。 相反，让我们看一下[StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample)示例。

StockTicker 示例定义两个主要类：

- `StockTickerHub`： 管理客户端连接中心类。
- `StockTicker`： 包含股票价格并定期更新这些值单一实例。

`StockTickerHub`保存对`StockTicker`转变成 singleton，虽然`StockTicker`保存到的引用**IHubConnectionContext**为`StockTickerHub`。 使用此接口与进行通信`StockTickerHub`实例。 (有关详细信息，请参阅[服务器广播使用 ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md)。)

我们可以使用 IoC 容器有点理清这些依赖关系。 首先，让我们简化`StockTickerHub`和`StockTicker`类。 在下面的代码中，我已注释掉的部分，我们不需要。

移除无参数构造函数从`StockTickerHub`。 相反，我们将始终使用 DI 来创建中心。

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

对于 StockTicker，移除的单一实例。 更高版本，我们将使用 IoC 容器来控制 StockTicker 生存期。 此外，请公共构造函数。

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

接下来，我们可以通过创建一个接口，使重构代码`StockTicker`。 我们将使用此接口来分离`StockTickerHub`从`StockTicker`类。

Visual Studio 使此类型的重构变得简单。 打开文件 StockTicker.cs，右键单击`StockTicker`类声明，然后选择**重构**...**提取接口**。

![](dependency-injection/_static/image1.png)

在**提取接口**对话框中，单击**选择所有**。 保留其他默认值。 单击“确定”。

![](dependency-injection/_static/image2.png)

Visual Studio 将创建名为的新接口`IStockTicker`，也会更改和`StockTicker`为派生自`IStockTicker`。

打开文件 IStockTicker.cs 和更改的接口**公共**。

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

在`StockTickerHub`类中，更改的两个实例`StockTicker`到`IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

创建`IStockTicker`接口不是绝对必需的但我希望演示如何 DI 可帮助降低应用程序中的组件之间的耦合。

## <a name="add-the-ninject-library"></a>添加 Ninject 库

有多个适用于.NET 的开源 IoC 容器。 对于本教程中，我将使用[Ninject](http://www.ninject.org/)。 (其他常用库包括[城堡 Windsor](http://www.castleproject.org/)， [Spring.Net](http://www.springframework.net/)， [Autofac](https://code.google.com/p/autofac/)， [Unity](https://github.com/unitycontainer/unity)，和[StructureMap](http://docs.structuremap.net).)

使用 NuGet 包管理器安装[Ninject 库](https://nuget.org/packages/Ninject/3.0.1.10)。 在 Visual Studio 中，从**工具**菜单中选择**库程序包管理器** | **程序包管理器控制台**。 在 Package Manager Console 窗口中，输入以下命令：

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>替换 SignalR 依赖项解析程序

若要使用 Ninject SignalR 内的，创建派生自的类**DefaultDependencyResolver**。

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

此类重写**GetService**和**GetServices**方法**DefaultDependencyResolver**。 SignalR 调用这些方法来创建在运行时，包括中心实例，以及供内部使用 SignalR 的各种服务的各种对象。

- **GetService**方法创建一种类型的单个实例。 重写此方法以调用 Ninject 内核**TryGet**方法。 如果该方法将返回 null，回退到默认冲突解决程序。
- **GetServices**方法创建指定类型的对象的集合。 重写此方法以从 Ninject 的结果与默认冲突解决程序将结果串联起来。

## <a name="configure-ninject-bindings"></a>配置 Ninject 绑定

现在我们将使用 Ninject 声明类型绑定。

打开你的应用程序 Startup.cs 类 (，你可以手动创建中的包说明根据`readme.txt`，或通过将身份验证添加到你的项目创建)。 在`Startup.Configuration`方法，创建 Ninject 容器，Ninject 调用*内核*。

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

创建我们自定义的依赖项解析程序的实例：

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

为创建绑定`IStockTicker`，如下所示：

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

此代码表示以下两项操作。 首先，每当应用程序需要`IStockTicker`，内核应创建的实例`StockTicker`。 第二个，`StockTicker`类应创建为单一实例对象。 Ninject 将创建的对象的一个实例，并返回相同的实例为每个请求。

为创建绑定**IHubConnectionContext** ，如下所示：

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

此代码 creatres 返回一个匿名函数**IHubConnection**。 **WhenInjectedInto**方法告知 Ninject 若要使用此函数仅在创建时`IStockTicker`实例。 原因是 SignalR 创建**IHubConnectionContext**内部，实例，但我们不希望重写 SignalR 如何创建它们。 此函数仅适用于我们`StockTicker`类。

传递到依赖项解析程序**MapSignalR**通过添加中心配置的方法：

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

使用新的参数更新示例的启动类中的 Startup.ConfigureSignalR 方法：

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

现在 SignalR 会使用中指定的冲突解决程序**MapSignalR**，而不是默认冲突解决程序。

下面是完整代码清单`Startup.Configuration`。

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

若要在 Visual Studio 中运行 StockTicker 应用程序，按 F5。 在浏览器窗口中，导航到`http://localhost:*port*/SignalR.Sample/StockTicker.html`。

![](dependency-injection/_static/image3.png)

该应用程序功能和以前完全相同。 (有关的说明，请参阅[服务器广播使用 ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md)。)我们尚未更改的行为;刚才更轻松地测试、 维护和改进的代码。
