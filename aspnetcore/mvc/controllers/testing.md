---
title: "测试 ASP.NET Core 中的控制器逻辑"
author: ardalis
description: "了解如何使用 Moq 和 xUnit 测试 ASP.NET Core 中的控制器逻辑。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/testing
ms.openlocfilehash: cabb1d2498e6c993b327c2fb9719525ec2181f9e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="testing-controller-logic-in-aspnet-core"></a>测试 ASP.NET Core 中的控制器逻辑

作者：[Steve Smith](https://ardalis.com/)

ASP.NET MVC 应用的控制器应该是小型的，且重点应对用户界面问题。 处理非 UI 问题的大型控制器更难以测试和维护。

[查看或下载 GitHub 中的示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)

## <a name="testing-controllers"></a>测试控制器

控制器是任意 ASP.NET Core MVC 应用程序的核心部分。 因此，应该对它们按应用计划表现有信心。 自动测试可以给你这种信心，还能在生成前检测错误。 避免在控制器中安置不必要的职责并确保测试仅专注于控制器职责非常重要。

控制器应采用最简单的逻辑，而不应关注业务逻辑或基础结构问题（例如数据访问）。 测试控制器逻辑，而不是框架。 测试控制器基于有效或无效输入的行为。 测试控制器如何相应其所执行业务操作的结果。

典型控制器职责：

* 验证 `ModelState.IsValid`。
* 如果 `ModelState` 无效，则返回错误响应。
* 从持久性检索业务实体。
* 对业务实体执行操作。
* 将业务实体保存到持久性。
* 返回相应的 `IActionResult`。

## <a name="unit-testing"></a>单元测试

[单元测试](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)涉及在测试应用程序部分时，使之与它的基础结构和依赖关系相隔离。 单元测试控制器逻辑时，仅测试单个操作的内容，不测试其依赖项或框架自身的行为。 单元测试控制器操作时，请确保仅关注其行为。 控制器单元测试将避开[筛选器](filters.md)、[路由](../../fundamentals/routing.md)或[模型绑定](../models/model-binding.md)等。 仅专注测试一项内容，单位测试通常编写简单、运行迅速。 正确编写的单位测试集无需过多开销即可经常运行。 但是，单位测试不检测部件间交互的问题，这是[集成测试](xref:mvc/controllers/testing#integration-testing)的用途。

如果编写自定义筛选器、路由等，应对其进行单位测试，而不是作为特定控制器操作测试的一部分进行。 应该对其进行隔离测试。

> [!TIP]
> [使用 Visual Studio 创建和运行单位测试](https://docs.microsoft.com/visualstudio/test/unit-test-your-code)。

若要演示单位测试，请查看以下控制器。 它显示集体讨论会话的列表并允许使用 POST 创建新的集体讨论会话：

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

控制器遵循[显式依赖关系原则](http://deviq.com/explicit-dependencies-principle/)，需要依赖关系注入提供 `IBrainstormSessionRepository` 实例。 因此使用 mock 对象框架（例如 [Moq](https://www.nuget.org/packages/Moq/)）进行测试很简单。 `HTTP GET Index` 方法没有循环或分支，且仅调用一个方法。 若要测试此 `Index` 方法，我们需要验证返回了 `ViewResult`，附带来自存储库 `List` 方法的 `ViewModel`。

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

`HomeController` `HTTP POST Index` 方法（如上所示）应该验证：

* 操作方法在 `ModelState.IsValid` 是 `false` 时返回有相应数据的错误请求 `ViewResult`

* `ModelState.IsValid` 为 true 时，调用存储库上的 `Add` 方法并返回有正确参数的 `RedirectToActionResult`。

通过使用 `AddModelError` 添加错误，可以测试无效模型状态，如下第一个测试所示。

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

第一个测试确认在 `ModelState` 无效时，返回与 `GET` 请求相同的 `ViewResult`。 请注意，测试不会尝试传入无效模型。 即使传入也无济于事，因为模型绑定不会运行（虽然[集成测试](xref:mvc/controllers/testing#integration-testing)将使用练习模型绑定）。 在本例中，不测试模型绑定。 这些单位测试仅测试操作方法中代码的行为。

第二个测试验证 `ModelState` 有效时，（通过存储库）添加了新的 `BrainstormSession`，且该方法返回有所需属性的 `RedirectToActionResult`。 通常会忽略未调用的模拟调用，但在设置调用末尾调用 `Verifiable` 就可以在测试中对其进行验证。 这通过对 `mockRepo.Verify` 的调用完成，进行这种调用时，如果未调用所需方法，则测试将失败。

> [!NOTE]
> 通过此示例中使用的 Moq 库，可轻松混合可验证（或称“严格”）mock 和非可验证 mock（也称为“宽松”mock 或存根）。 详细了解[使用 Moq 自定义 Mock 行为](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior)。

应用中的另一个控制器显示与特定集体讨论会话相关的信息。 它包括用于处理无效 ID 值的逻辑：

[!code-csharp[Main](./testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

控制器操作有三个要测试的事例，每个 `return` 语句对应一个：

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

应用将功能公开为 Web API（与集体讨论会话相关的想法的列表，以及将新想法添加到会话的方法）：

<a name="ideas-controller"></a>

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

`ForSession` 方法返回 `IdeaDTO` 类型的列表。 避免直接通过 API 调用返回业务域实体，因为它们常包含 API 客户端所需以外的数据，且它们会不必要地将应用的内部域模型与外部公开的 API 配对。 域实体和通过网络返回的类型之间的映射可以手动（使用 LINQ `Select`，如此处所示）或使用 [AutoMapper](https://github.com/AutoMapper/AutoMapper) 等库完成

`Create` 和 `ForSession` API 方法的单位测试：

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

如前所述，若要测试方法在 `ModelState` 无效时的行为，请在测试zho 将模型错误添加到控制器。 请勿在单位测试中尝试测试模型有效性或模型绑定 - 仅测试操作方法在遇到特定 `ModelState` 值时的行为。

第二个测试依赖存储库返回 null，所以 mock 存储库配置为返回 null。 无需创建测试数据库（在内存中或其他位置）并构建将返回此结果的查询 - 它可以在单个语句中完成，如下所示。

最后一个测试验证调用了存储库的 `Update` 方法。 正如我们之前所做，使用 `Verifiable` 调用 mock，然后调用模拟存储库的 `Verify` 方法，以确认执行了可验证方法。 确保 `Update` 方法保存数据不是单位测试的职责，这可以通过集成测试完成。

## <a name="integration-testing"></a>集成测试

完成[集成测试](../../testing/integration-testing.md)，可确保应用中各独立模块正确协作。 通常情况下，可以使用单位测试测试的内容也可以使用集成测试测试，但反之不成立。 但是，集成测试比单位测试慢。 因此，最好使用单位测试测试其可测内容，并在设计多个协作者时使用集成测试。

虽然它们也可能起作用，但是 mock 对象很少在集成测试中使用。 在单位测试中，mock 对象是出于测试目的控制测试单位外协作者行为的有效方法。 在集成测试中，使用真实协作者确定整个子系统以正确的方式协同工作。

### <a name="application-state"></a>应用程序状态

执行集成测试时一个重要考虑因素是如何设置应用状态。 测试需要互相独立地运行，因此每个测试都应从处于已知状态的应用开始。 如果应用不使用数据库或有任何持久性，这可能不是问题。 但是，大多实际应用将其状态保存到某些类型的数据存储，所以一个测试作出的任意修改都可能影响到另一个测试，除非重置数据存储。 使用内置 `TestServer`，在集成测试中承载 ASP.NET Core 应用非常简单，但是这并不一定会授予对其要使用的数据的访问权限。 如果正在使用实际数据库，则一种方法是让应用连接到测试可以访问的测试数据库，并确保其在每个测试执行前重置到已知状态。

在此示例应用程序中，我是用的是 Entity Framework Core 的 InMemoryDatabase 支持，所以我不能从测试项目直接连接到它。 相反，我公开应用 `Startup` 类的 `InitializeDatabase` 方法，我在应用在 `Development` 环境中启动时调用该类。 只要集成测试将环境设置为 `Development`，就能自动从中获益。 我无需担心重置数据库，因为每次应用重启时都会重置 InMemoryDatabase。

`Startup` 类：

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Startup.cs?highlight=19,20,34,35,43,52)]

在以下集成测试中，可以看到频繁使用 `GetTestSession` 方法。

### <a name="accessing-views"></a>访问视图

每个集成测试类都会配置将运行 ASP.NET Core 应用的 `TestServer`。 默认情况下，`TestServer` 会将 Web 应用存储在运行该应用的文件夹（本例中即测试项目文件夹）。 因此，当尝试测试返回 `ViewResult` 的控制器操作时，可能看到此错误：

```
The view 'Index' wasn't found. The following locations were searched:
(list of locations)
```

若要更正此问题，需要配置服务器的内容根，使其可以定位到被测试项目的视图。 这可以通过调用 `TestFixture` 类中的 `UseContentRoot` 完成，如下所示：

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/TestFixture.cs?highlight=30,33)]

`TestFixture` 类负责配置和创建 `TestServer`，设置 `HttpClient` 与 `TestServer` 通信。 每个集成测试使用 `Client` 属性连接到测试服务器并发出请求。

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/HomeControllerTests.cs?highlight=20,26,29,30,31,35,38,39,40,41,44,47,48)]

在上面的第一个测试中，`responseString` 保持“视图”的实际呈现 HTML，可以检查它以确定其包含所需结果。

第二个测试构造具有唯一会话名称的窗体 POST，并将其发布到应用，然后验证返回了预期重定向。

### <a name="api-methods"></a>API 方法

如果应用公开 Web API，最好用自动测试确定它们按照预期执行。 内置 `TestServer` 使得测试 Web API 更简单。 如果 API 方法使用模型绑定，则应始终检查 `ModelState.IsValid`，并用集成测试确定模型验证正确工作。

以下测试集以 [IdeasController](xref:mvc/controllers/testing#ideas-controller) 类中的 `Create` 方法为目标，如下所示：

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/ApiIdeasControllerTests.cs)]

不同于返回 HTML 视图的操作集成测试，返回结果的 Web API 方法常常可以反序列化为强类型对象，如上面的最后一个测试所示。 在此情况下，测试将结果反序列化到 `BrainstormSession` 实例，并确定想法正确添加到其想法集。

可在本文的[示例项目](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)中找到集成测试的其他示例。
