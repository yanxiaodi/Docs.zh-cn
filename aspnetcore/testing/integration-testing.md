---
title: "在 ASP.NET Core 中测试的集成"
author: ardalis
description: "如何使用 ASP.NET Core 的集成测试以确保应用程序的组件正常工作。"
manager: wpickett
ms.author: riande
ms.date: 09/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: testing/integration-testing
ms.openlocfilehash: 8c28f1b4f66433eaebd9e474e784ecf3f1ac271b
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="integration-testing-in-aspnet-core"></a>在 ASP.NET Core 中测试的集成

作者：[Steve Smith](https://ardalis.com/)

集成测试可确保应用程序的组件组合在一起时正常工作。 ASP.NET Core 使用单元测试框架和可用于处理请求（无网络费用）的内置测试 web 主机支持集成测试。

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

## <a name="introduction-to-integration-testing"></a>集成测试的简介

集成测试来验证应用程序的不同部分正常会在一起。 与不同[单元测试](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)，集成测试经常涉及应用程序基础结构问题，如数据库、 文件系统、 网络资源，或 web 请求和响应。 单元测试使用 fakes 或使用模拟对象来替代这些问题，但集成测试的目的是确认系统按预期的这些系统。

集成测试，因为在执行较大代码段，并且它们依赖于基础结构元素倾向于数量级慢于单元测试。 因此，它是一个好办法限制多少集成测试编写，尤其是可以使用单元测试中测试相同的行为。

> [!NOTE]
> 如果某些行为可以使用单元测试或集成测试进行测试，更愿意相应单元测试，因为它将为几乎始终是更快。 你可能有几十或数百个使用许多不同的输入的单元测试，但只需少量的集成测试介绍最重要的方案。

测试你自己的方法中的逻辑通常是域的单元测试。 测试你的应用程序在其框架，例如，使用 ASP.NET Core，或与数据库工作原理是集成测试的其中派上用场。 它并不需要太多的集成测试，以确认你能够将行写入到数据库和读回。 你不需要进行测试每个可能的排列的数据访问代码-只需测试足以为你提供你的应用程序正常的置信度。

## <a name="integration-testing-aspnet-core"></a>集成测试 ASP.NET 核心

若要获取设置为运行的集成测试，你将需要创建的测试项目、 添加到 ASP.NET 核心 web 项目中，引用和安装的测试运行程序。 此过程所述[单元测试](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)文档，以及运行测试和命名你的测试和测试类的建议的更多详细说明。

> [!NOTE]
> 分隔单元测试和集成测试使用不同的项目。 这有助于确保你不会意外地引入到你的单元测试的基础结构问题，并可以轻松地选择要运行的测试集。

### <a name="the-test-host"></a>测试主机

ASP.NET 核心包括可以添加到集成测试项目，并用于托管 ASP.NET Core 应用程序，而无需实际 web 宿主请求的服务测试的测试主机。 提供的示例包括集成测试项目已配置为使用[xUnit](https://xunit.github.io)和测试主机。 它使用`Microsoft.AspNetCore.TestHost`NuGet 包。

一次`Microsoft.AspNetCore.TestHost`包包括在项目中后，你将能够创建和配置`TestServer`在测试中。 下面的代码演示如何验证对站点的根目录的请求返回"Hello World ！" 并且应成功运行针对默认值由 Visual Studio 创建的 ASP.NET 核心空 Web 模板。

[!code-csharp[](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

此测试使用排列 Act 断言模式。 在创建的实例的构造函数中完成准备步骤`TestServer`。 一个已配置`WebHostBuilder`将用于创建`TestHost`; 在此示例中，`Configure`方法从待测试 (SUT) 系统`Startup`类传递给`WebHostBuilder`。 此方法将用于配置的请求管道`TestServer`到 SUT 服务器将配置方式相同。

在测试的 Act 部分中，向发出请求`TestServer`"/"路径和响应的实例返回读入字符串。 此字符串与"Hello World ！"的预期字符串进行比较。 如果它们匹配，则测试通过;否则，它将失败。

现在你可以添加了几个其他集成测试，以确认检查功能主要通过 web 应用程序能否正常运行：

[!code-csharp[](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

请注意，你实际上不尝试使用这些测试进行测试的质数检查程序的正确性但而应假定 web 应用程序执行的操作，你的预期。 你已为你提供中的置信度的单元测试覆盖率`PrimeService`，如你可以在此处看到：

![测试资源管理器](integration-testing/_static/test-explorer.png)

你可以了解有关中的单元测试[单元测试](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)文章。


### <a name="integration-testing-mvcrazor"></a>集成测试 Mvc/Razor

测试项目，其中包含 Razor 视图需要`<PreserveCompilationContext>`设置为 true 中*.csproj*文件：


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

缺少此元素的项目将生成类似于以下错误：
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a>重构以使用中间件

重构是应用程序的代码更改为不会更改其行为改进其设计的过程。 它应理想情况下进行一套传递测试，因为这些帮助确保系统的行为保持不变之前, 和之后所做的更改时。 在其中检查逻辑质数实现中 web 应用程序的方式查看`Configure`方法，你会看到：

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.Run(async (context) =>
    {
        if (context.Request.Path.Value.Contains("checkprime"))
        {
            int numberToCheck;
            try
            {
                numberToCheck = int.Parse(context.Request.QueryString.Value.Replace("?", ""));
                var primeService = new PrimeService();
                if (primeService.IsPrime(numberToCheck))
                {
                    await context.Response.WriteAsync($"{numberToCheck} is prime!");
                }
                else
                {
                    await context.Response.WriteAsync($"{numberToCheck} is NOT prime!");
                }
            }
            catch
            {
                await context.Response.WriteAsync("Pass in a number to check in the form /checkprime?5");
            }
        }
        else
        {
            await context.Response.WriteAsync("Hello World!");
        }
    });
}
```

此代码有效，但它并不如何你想要在 ASP.NET Core 应用程序，即使简单，因为这是实现此类功能。 假设什么`Configure`方法将如下所示如果您需要将这么多代码添加到其中，每次您添加另一个 URL 终结点 ！

要考虑的一个选项添加[MVC](xref:mvc/overview)与应用程序创建控制器来处理主要检查。 但是，假定当前不需要任何其他 MVC 的功能，即一个位浪费。

你可以但是，充分利用 ASP.NET Core[中间件](xref:fundamentals/middleware/index)，这将帮助我们封装检查其自己的类中的逻辑校准并更好地实现[关注点分离](http://deviq.com/separation-of-concerns/)中`Configure`方法。

你想要允许该中间件用于指定作为参数，因此中间件类需要的路径`RequestDelegate`和`PrimeCheckerOptions`其构造函数中的实例。 如果请求的路径不匹配此中间件是配置需要，你只需调用链中的下一步的中间件并执行任何进一步操作。 中的实现代码的其余部分`Configure`现已在`Invoke`方法。

> [!NOTE]
> 因为取决于该中间件`PrimeService`服务，还所请求的构造函数使用此服务实例。 框架将提供此服务通过[依赖关系注入](xref:fundamentals/dependency-injection)，假设它已配置，例如，在`ConfigureServices`。

[!code-csharp[](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

由于其路径与匹配时，此中间件将充当请求委托链中的终结点，因此时不需要调用`_next.Invoke`时此中间件将处理该请求。

使用位置和一些有用的扩展方法中创建以方便将其配置，重构此中间件`Configure`方法如下所示：

[!code-csharp[](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

此重构，以下确信，web 应用程序仍然正常工作和前面一样，因为所有通过集成测试。

> [!NOTE]
> 它是一个好办法之后完成重构，你的测试都通过将所做的更改提交到源代码管理。 如果您一起练习测试驱动开发，[考虑向你红-绿-重构循环添加提交](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development)。

## <a name="resources"></a>资源

* [单元测试](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [中间件](xref:fundamentals/middleware/index)
* [测试控制器](xref:mvc/controllers/testing)
