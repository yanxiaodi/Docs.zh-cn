---
title: "Razor 页单元和集成测试，在 ASP.NET 核心"
author: guardrex
description: "了解如何创建用于 Razor 页的应用程序的单元和集成测试。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: testing/razor-pages-testing
ms.openlocfilehash: e4f87a8151e378717aa9198e4629711c4ea6ef77
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="razor-pages-unit-and-integration-testing-in-aspnet-core"></a>Razor 页单元和集成测试，在 ASP.NET 核心

作者：[Luke Latham](https://github.com/guardrex)

ASP.NET 核心支持单元和集成测试的 Razor 页应用。 测试数据访问层 (DAL)、 页面模型和集成页上组件可帮助确保：

* 部分 Razor 页面应用程序在应用程序构造期间工作独立以及组合在一起作为一个单元。
* 类和方法具有有限的作用域的责任。
* 上应用的行为方式存在其他文档。
* 回归，不由更新对代码的错误，将自动的生成和部署期间发现。

本主题假定你已基本了解 Razor 页应用、 单元测试，以及集成测试。 如果你熟悉 Razor 页应用或测试概念，请参阅以下主题：

* [Razor 页面介绍](xref:mvc/razor-pages/index)
* [Razor 页面入门](xref:tutorials/razor-pages/razor-pages-start)
* [单元测试 C# 中使用 dotnet 测试和 xUnit 的.NET 核心](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [集成测试](xref:testing/integration-testing)

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/razor-pages-testing/sample/)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

示例项目组成两个应用程序：

| 应用         | 项目文件夹                        | 描述 |
| ----------- | ------------------------------------- | ----------- |
| 消息应用程序 | *src/RazorPagesTestingSample*         | 允许用户添加、 删除其中一个，删除所有，和分析消息。 |
| 测试应用程序    | *tests/RazorPagesTestingSample.Tests* | 用于测试消息应用程序。<ul><li>单元测试： 数据访问层 (DAL)，索引页模型</li><li>集成测试： 索引页</li></ul> |

可以使用内置的测试功能的一个 IDE，如运行测试[Visual Studio](https://www.visualstudio.com/vs/)。 如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令行中，执行以下命令在命令提示符中*tests/RazorPagesTestingSample.Tests*文件夹：

```console
dotnet test
```

## <a name="message-app-organization"></a>消息应用组织

消息应用程序是一个简单的 Razor 页消息系统，具有以下特征：

* 应用程序的索引页 (*Pages/Index.cshtml*和*Pages/Index.cshtml.cs*) 提供一个用户界面和网页模型方法，用于控制添加、 删除和分析消息 （每个消息的平均词）.
* 一条消息都由描述`Message`类 (*Data/Message.cs*) 具有两个属性： `Id` （密钥） 和`Text`（消息）。 `Text`属性是必需的限制为 200 个字符。
* 消息存储使用[实体框架的内存中数据库](/ef/core/providers/in-memory/)&#8224;。
* 此应用程序包含在其数据库上下文类中，数据访问层 (DAL) `AppDbContext` (*Data/AppDbContext.cs*)。 DAL 方法将被标记`virtual`，这样，模拟在测试中使用的方法。
* 如果数据库为空应用程序启动时，消息存储区初始化与三条消息。 这些*设定种子消息*也用在测试。

&#8224;EF 主题，[测试以及 InMemory](/ef/core/miscellaneous/testing/in-memory)，说明如何使用内存中数据库来使用 MSTest 测试。 本主题使用[xUnit](https://xunit.github.io/)测试框架。 测试概念和测试跨不同测试框架的实现是类似，但不是完全相同。

尽管应用程序不使用[存储库模式](http://martinfowler.com/eaaCatalog/repository.html)并不是有效的示例[工作单元 (UoW) 模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)，Razor 页中支持的开发模式。 有关详细信息，请参阅[设计基础结构持久性层](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)，[在 ASP.NET MVC 应用程序中实现的存储库和单元的工作模式](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)，和[测试控制器逻辑](/aspnet/core/mvc/controllers/testing)（此示例实现的存储库模式）。

## <a name="test-app-organization"></a>测试应用程序的组织

测试应用程序是在一个控制台应用*tests/RazorPagesTestingSample.Tests*文件夹：

| 测试应用程序文件夹    | 描述 |
| ------------------ | ----------- |
| *IntegrationTests* | <ul><li>*IndexPageTest.cs*包含索引页的集成测试。</li><li>*TestFixture.cs*创建测试主机测试消息应用程序。</li></ul> |
| *UnitTests*        | <ul><li>*DataAccessLayerTest.cs* DAL 包含单元测试。</li><li>*IndexPageTest.cs*包含针对索引页模型的单元测试。</li></ul> |
| *实用程序*        | *Utilities.cs*包含:<ul><li>`TestingDbContextOptions` 用于创建新的数据库上下文对于每个 DAL 单元测试的选项，以便数据库重置为其基线条件的每个测试方法。</li><li>`GetRequestContentAsync` 方法用于准备`HttpClient`和内容的集成测试在发送到消息应用程序的请求。</li></ul>

测试框架为[xUnit](https://xunit.github.io/)。 模拟框架的对象是[Moq](https://github.com/moq/moq4)。 集成测试使用执行[ASP.NET 核心测试主机](xref:testing/integration-testing#the-test-host)。

## <a name="unit-testing-the-data-access-layer-dal"></a>单元测试的数据访问层 (DAL)

消息应用程序中包含的四个方法与出现 DAL`AppDbContext`类 (*src/RazorPagesTestingSample/Data/AppDbContext.cs*)。 每个方法中测试应用程序包含一个或两个单元测试。

| DAL 方法               | 函数                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | 获取`List<Message>`从数据库按排序`Text`属性。 |
| `AddMessageAsync`        | 将添加`Message`到数据库。                                          |
| `DeleteAllMessagesAsync` | 删除所有`Message`从数据库的条目。                           |
| `DeleteMessageAsync`     | 将删除单个`Message`从数据库`Id`。                      |

DAL 的单元测试需要[DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions)时创建新`AppDbContext`为每个测试。 创建的一种方法`DbContextOptions`每个测试是使用[DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

使用此方法的问题是，每个测试就会收到数据库中以前的测试中保留它的任何状态。 在尝试写入不会相互影响的原子单元测试时，这可能会产生问题。 若要强制`AppDbContext`若要对每个测试中使用新的数据库上下文，提供`DbContextOptions`基于新的服务提供程序的实例。 测试应用程序演示如何执行此操作使用其`Utilities`类方法`TestingDbContextOptions`(*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet1)]

使用`DbContextOptions`DAL 单元中测试允许每个测试运行以原子方式与全新的数据库实例：

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

在每个测试方法`DataAccessLayerTest`类 (*UnitTests/DataAccessLayerTest.cs*) 遵循类似的排列 Act 断言模式：

1. 排列： 测试配置了数据库和/或定义预期的结果。
1. Act： 执行测试。
1. 断言： 断言可确定测试结果是否成功。

例如，`DeleteMessageAsync`方法负责删除单个消息由其`Id`(*src/RazorPagesTestingSample/Data/AppDbContext.cs*):

[!code-csharp[](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/AppDbContext.cs?name=snippet4)]

没有为此方法的两个测试。 一个测试检查数据库中存在消息时，该方法将删除一条消息。 如果不会更改数据库的其他方法测试消息`Id`删除不存在。 `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`方法如下所示：

[!code-csharp[](razor-pages-testing/sample_snapshot/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

首先，该方法执行准备步骤中，执行步骤准备发生。 获取并保存在种子设定消息`seedMessages`。 种子设定的消息会保存到数据库中。 将消息与`Id`的`1`设置为删除。 当`DeleteMessageAsync`执行方法时，预期的消息应具有所有除外与消息`Id`的`1`。 `expectedMessages`变量表示此预期的结果。

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

方法是运行：`DeleteMessageAsync`执行方法中传递`recId`的`1`:

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

最后，此方法获取`Messages`上下文中，并将其到`expectedMessages`两个相等的断言：

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

若要比较的两个`List<Message>`相同：

* 消息有序`Id`。
* 消息对比较上`Text`属性。

类似的测试方法，`DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound`检查正在尝试删除一条消息，不存在的结果。 在这种情况下，在数据库中预期的消息应等于后的实际消息`DeleteMessageAsync`执行方法。 应没有更改数据库的内容：

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-testing-the-page-model-methods"></a>单元测试页模型方法

另一个组的单元测试负责测试页模型方法。 在消息应用中，索引页模型中找到`IndexModel`类*src/RazorPagesTestingSample/Pages/Index.cshtml.cs*。

| 页模型方法 | 函数 |
| ----------------- | -------- | 
| `OnGetAsync` | 从 UI 使用 DAL 获取消息`GetMessagesAsync`方法。 |
| `OnPostAddMessageAsync` | 如果`ModelState`无效，请调用`AddMessageAsync`向数据库添加一条消息。 | 
| `OnPostDeleteAllMessagesAsync` | 调用`DeleteAllMessagesAsync`删除所有数据库中的消息。 |
| `OnPostDeleteMessageAsync` | 执行`DeleteMessageAsync`若要删除的消息`Id`指定。 |
| `OnPostAnalyzeMessagesAsync` | 如果一个或多个消息是在数据库中，将计算每个消息的单词的平均的数目。 |

使用七个测试中的测试页模型方法`IndexPageTest`类 (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*)。 测试使用熟悉的排列断言 Act 模式。 这些测试的重点：

* 确定方法是否遵循正确的行为时`ModelState`无效。
* 确认方法生成正确`IActionResult`。
* 正在检查属性的值分配都正确。

测试此组通常模拟 DAL 来生成 Act 步骤执行页模型方法的预期的数据的方法。 例如，`GetMessagesAsync`方法`AppDbContext`模拟以生成输出。 当页模型方法执行此方法时，模型将返回的结果。 数据不会从数据库中。 这将创建在页模型测试中使用 DAL 的可预测、 可靠的测试条件。

`OnGetAsync_PopulatesThePageModel_WithAListOfMessages`测试显示如何`GetMessagesAsync`方法模拟的页面模型：

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet1&highlight=3-4)]

当`OnGetAsync`Act 步骤中执行方法时，它调用页模型`GetMessagesAsync`方法。

单元测试 Act 步骤 (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*):

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet2)]

`IndexPage` 页模型`OnGetAsync`方法 (*src/RazorPagesTestingSample/Pages/Index.cshtml.cs*):

[!code-csharp[](razor-pages-testing/sample/src/RazorPagesTestingSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

`GetMessagesAsync`中 DAL 方法不返回此方法调用的结果。 该方法的模拟的版本返回的结果。

在`Assert`步骤，实际的消息 (`actualMessages`) 从分配`Messages`页模型的属性。 在将消息分配时也执行类型检查。 通过进行比较的预期和实际的消息其`Text`属性。 测试断言，这两个`List<Message>`实例包含对相同消息。

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet3)]

此组中的其他测试创建页包括的模型对象`DefaultHttpContext`、 `ModelStateDictionary`、`ActionContext`建立`PageContext`、 `ViewDataDictionary`，和一个`PageContext`。 这些可用于执行测试。 例如，消息应用程序建立`ModelState`错误`AddModelError`检查是否是有效`PageResult`时返回`OnPostAddMessageAsync`执行：

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="integration-testing-the-app"></a>测试应用程序的集成

集成测试在测试应用程序的组件配合工作的焦点。 集成测试使用执行[ASP.NET 核心测试主机](xref:testing/integration-testing#the-test-host)。 完整的请求-响应生命周期处理进行测试。 这些测试断言页生成正确的状态代码和`Location`标头，如果设置。

集成测试的示例的示例检查请求的消息应用程序索引页的结果 (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet1)]

没有排列步。 `GetAsync`方法调用`HttpClient`将 GET 请求发送到终结点。 测试断言，结果是 200 OK 状态代码。

对消息应用任何 POST 请求必须满足 antiforgery 检查自动是由应用程序的开发[数据保护 antiforgery 系统](xref:security/data-protection/introduction)。 若要安排某个测试的 POST 请求，测试应用程序必须：

1. 请对页的请求。
1. 分析 antiforgery cookie 和响应的请求验证令牌。
1. 到位，请使用 antiforgery 的 cookie 与请求验证的 POST 请求令牌。

`Post_AddMessageHandler_ReturnsRedirectToRoot`测试方法：

* 准备一条消息和`HttpClient`。
* 向应用程序发出的 POST 请求。
* 检查响应重定向回索引页面。

`Post_AddMessageHandler_ReturnsRedirectToRoot ` 方法 (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet2)]

`GetRequestContentAsync`实用工具方法管理准备客户端使用 antiforgery cookie 和请求验证令牌。 请注意如何方法接收`IDictionary`允许调用测试方法通过数据用于编码以及请求验证令牌的请求中 (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet2&highlight=1-2,8-9,29)]

集成测试还可以将错误数据传递给应用程序以测试应用程序的响应行为。 消息应用程序限制到 200 个字符的消息长度 (*src/RazorPagesTestingSample/Data/Message.cs*):

[!code-csharp[](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/Message.cs?name=snippet1&highlight=7)]

`Post_AddMessageHandler_ReturnsSuccess_WhenMessageTextTooLong`测试`Message`显式传递中带有 201"X"字符的文本。 这会导致`ModelState`错误。 发布不重定向回索引页面。 它将返回 200 OK 与`null``Location`标头 (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet3&highlight=7,16-17)]

## <a name="see-also"></a>请参阅

* [单元测试 C# 中使用 dotnet 测试和 xUnit 的.NET 核心](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [集成测试](xref:testing/integration-testing)
* [测试控制器](xref:mvc/controllers/testing)
* [单元测试代码](/visualstudio/test/unit-test-your-code)(Visual Studio)
* [xUnit.net](https://xunit.github.io/)
* [入门 xUnit.net （.NET Core/ASP.NET 核）](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Moq 快速入门](https://github.com/Moq/moq4/wiki/Quickstart)
