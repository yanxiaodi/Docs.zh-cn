---
title: "JavaScriptServices 用于在 ASP.NET Core 中创建单页面应用程序"
author: scottaddie
description: "了解有关使用 JavaScriptServices 创建由 ASP.NET Core 单页面应用程序 (SPA) 的好处。"
manager: wpickett
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/spa-services
ms.openlocfilehash: c962fc160cf39ad1c69f4269616c993fde420035
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a>JavaScriptServices 用于在 ASP.NET Core 中创建单页面应用程序

通过[Scott Addie](https://github.com/scottaddie)和[Fiyaz Hasan](http://fiyazhasan.me/)

单页面应用程序 (SPA) 是一种流行的 web 应用程序，因为其固有的丰富用户体验。 将客户端 SPA 框架或库，如集成[角](https://angular.io/)或[做出反应](https://facebook.github.io/react/)，与服务器端框架，如 ASP.NET Core 可能很困难。 [JavaScriptServices](https://github.com/aspnet/JavaScriptServices)开发的目的是减少在集成过程中的问题。 它可让在不同的客户端和服务器技术堆栈之间的无缝操作。

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a>什么是 JavaScriptServices？

JavaScriptServices 是 ASP.NET Core 的客户端技术的集合。 其目标是将 ASP.NET Core 定位为开发人员的首选服务器端平台，用于构建 Spa。

JavaScriptServices 包含三个不同的 NuGet 包：
* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)
* [Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)

这些包的作用是如果你：
* 在服务器上运行 JavaScript
* 使用 SPA 框架或库
* 生成客户端 Webpack 资产

在本文中将焦点大部分位于使用 SpaServices 包。

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a>什么是 SpaServices？

用于将 ASP.NET Core 定位为开发人员的首选服务器端平台，用于构建 Spa，SpaServices 而创建。 SpaServices 不需要开发与 ASP.NET 核心的 Spa，它不会将您限制在一个特定的客户端框架。

SpaServices 提供有用的基础结构，如所示：
* [服务器端预呈现](#server-prerendering)
* [Webpack 开发人员中间件](#webpack-dev-middleware)
* [热模块更换](#hot-module-replacement)
* [路由的帮助器](#routing-helpers)

总体来说，这些基础结构组件来提高开发工作流和运行时体验。 可以单独采用组件。

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a>使用 SpaServices 的先决条件

若要使用 SpaServices，安装以下项：
* [Node.js](https://nodejs.org/) （6 或更高版本） 与 npm
    * 若要验证这些组件安装，并找不到，运行以下命令从命令行：

    ```console
    node -v && npm -v
    ```

注意： 如果你要部署到 Azure 网站，你不需要此处执行任何操作&mdash;Node.js 已安装并且可用的服务器环境中。

* [.NET 核心 SDK](https://www.microsoft.com/net/download/core) 1.0 （或更高版本）
    * 如果你在 Windows 上，这可以安装通过选择 Visual Studio 2017 **.NET 核心跨平台开发**工作负荷。

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet 包

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a>服务器端预呈现

通用的 (也称为 isomorphic) 应用程序是能够在服务器和客户端上同时运行的 JavaScript 应用程序。 角、 响应和其他常用框架提供一个通用平台此应用程序的开发风格。 目的是第一次呈现 Node.js，通过在服务器上的 framework 组件，然后将进一步委托到客户端执行。

ASP.NET 核心[标记帮助程序](xref:mvc/views/tag-helpers/intro)由 SpaServices 简化通过调用服务器上的 JavaScript 函数的服务器端预呈现的实现。

### <a name="prerequisites"></a>系统必备

安装以下组件：
* [aspnet 预呈现](https://www.npmjs.com/package/aspnet-prerendering)npm 包：

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a>配置

标记帮助程序都在项目的命名空间注册通过可发现*_ViewImports.cshtml*文件：

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

这些标记帮助程序抽象化通过利用在 Razor 视图类似于 HTML 的语法直接与低级别 Api 进行通信的复杂性：

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a>`asp-prerender-module`标记帮助器

`asp-prerender-module`标记帮助器，使用在前面的代码示例中，执行*ClientApp/dist/main-server.js*通过 Node.js 服务器上。 为清晰起见， *main server.js*文件是 TypeScript JavaScript transpilation 任务中的项目[Webpack](http://webpack.github.io/)生成过程。 Webpack 定义的入口点别名`main-server`; 并且，在开始此别名的依赖项关系图的遍历*ClientApp/启动 server.ts*文件：

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

在以下的角度示例中， *ClientApp/启动 server.ts*文件利用`createServerRenderer`函数和`RenderResult`类型`aspnet-prerendering`npm 包以配置通过 Node.js 服务器呈现。 发送到服务器端呈现传递给解析函数调用，从而将包装在强类型化的 JavaScript 中的 HTML 标记`Promise`对象。 `Promise`对象的基数是它以异步方式提供到 DOM 的占位符元素中注入的页的 HTML 标记。

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a>`asp-prerender-data`标记帮助器

结合了`asp-prerender-module`标记帮助器，`asp-prerender-data`标记帮助器可以用于将上下文信息从 Razor 视图传递到服务器端 JavaScript。 例如，以下标记将传递到的用户数据`main-server`模块：

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

接收`UserName`自变量使用内置的 JSON 序列化程序序列化和存储在`params.data`对象。 在以下的角度示例中，数据用于构造中的个性化的问候语`h1`元素：

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

注意： 在标记帮助程序中传递的属性名称使用表示**PascalCase**表示法。 相比之下，到 JavaScript，相同的属性名称由**驼峰匹配**。 默认 JSON 序列化配置负责这种差异。

若要展开在前面的代码示例时，数据可以从服务器由传递给视图 hydrating`globals`属性提供给`resolve`函数：

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

`postList`数组内部定义`globals`对象附加到浏览器的全局`window`对象。 为全局作用域此变量提升消除重复的工作量，，尤其当它与加载一次在服务器上，再次在客户端上相同的数据。

![附加到窗口对象的全局 postList 变量](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a>Webpack 开发人员中间件

[Webpack 开发人员中间件](https://webpack.github.io/docs/webpack-dev-middleware.html)引入了 Webpack 按需生成资源的凭此简化的开发工作流。 该中间件自动编译，并在浏览器中重新加载页面时提供客户端资源。 一种替代方法是手动将 Webpack 通过项目的 npm 生成脚本调用，第三方依赖关系或自定义代码更改时。 Npm 中生成脚本*package.json*文件显示在下面的示例：

[!code-json[](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a>系统必备

安装以下组件：
* [aspnet webpack](https://www.npmjs.com/package/aspnet-webpack) npm 包：

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a>配置

Webpack 开发人员中间件注册到中的以下代码通过 HTTP 请求管道*Startup.cs*文件的`Configure`方法：

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

`UseWebpackDevMiddleware`前，必须调用扩展方法[注册静态文件承载](xref:fundamentals/static-files)通过`UseStaticFiles`扩展方法。 出于安全原因，注册该中间件，仅当应用程序在开发模式下运行时。

*Webpack.config.js*文件的`output.publicPath`属性告知要监视的中间件`dist`更改的文件夹：

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a>热模块更换

思考的 Webpack 的[热模块更换](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html)(HMR) 功能作为演变而来的[Webpack 开发人员中间件](#webpack-dev-middleware)。 HMR 引入了完全相同的好处，但它进一步，从而简化了开发工作流自动编译所做的更改后更新页面内容。 不要混淆这与刷新浏览器中，这会干扰的当前内存中状态和 SPA 的调试会话。 没有 Webpack 开发人员中间件服务与浏览器中，这意味着更改推送到浏览器之间的实时链接。

### <a name="prerequisites"></a>系统必备

安装以下组件：
* [webpack 热 middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm 包：

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a>配置

HMR 组件必须注册到 MVC 的 HTTP 请求管道中`Configure`方法：

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

为已对如此[Webpack 开发人员中间件](#webpack-dev-middleware)、`UseWebpackDevMiddleware`前，必须调用扩展方法`UseStaticFiles`扩展方法。 出于安全原因，注册该中间件，仅当应用程序在开发模式下运行时。

*Webpack.config.js*文件必须定义`plugins`，即使它保留为空数组：

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

在加载浏览器中的应用程序之后, 的开发人员工具的控制台选项卡提供 HMR 激活的确认：

![热模块更换连接的消息](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a>路由的帮助器

在大多数基于 ASP.NET Core 的 Spa，您需要客户端路由除了服务器端路由。 SPA 和 MVC 路由系统可以独立处理不受干扰。 没有，但是，一个边缘案例是否会造成面临的挑战： 标识 404 HTTP 响应。

请考虑在该方案中的无扩展名路由`/some/page`使用。 假定该请求不模式匹配的服务器端路由，但其模式匹配的客户端路由。 现在请考虑对的传入请求`/images/user-512.png`，它通常需要查找服务器上的图像文件。 如果该请求的资源路径不匹配任何服务器端路由或静态文件，它不太客户端应用程序将处理它，你通常想要返回 HTTP 状态代码为 404。

### <a name="prerequisites"></a>系统必备

安装以下组件：
* 客户端路由 npm 包。 使用角作为示例：

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a>配置

名为的扩展方法`MapSpaFallbackRoute`中使用`Configure`方法：

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

提示： 路由中配置它们的顺序进行计算。 因此，`default`模式匹配的第一次使用在前面的代码示例中的路由。

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a>创建新项目

JavaScriptServices 提供了预配置的应用程序模板。 SpaServices 中这些模板，与不同的框架和库如角、 Aurelia、 Knockout、 响应，和 Vue 结合使用。

可以通过.NET 核心 CLI 安装这些模板，通过运行以下命令：

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

显示可用的 SPA 模板的列表：

| 模板                                 | 短名称 | 语言 | Tags        |
|:------------------------------------------|:-----------|:---------|:------------|
| 带有角度的 MVC ASP.NET 核心             | angular    | [C#]     | Web/MVC/SPA |
| 带有 Aurelia 的 MVC ASP.NET 核心             | aurelia    | [C#]     | Web/MVC/SPA |
| 带有 Knockout.js 的 MVC ASP.NET 核心         | knockout   | [C#]     | Web/MVC/SPA |
| 带有 React.js 的 MVC ASP.NET 核心            | react      | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core React.js 和回顾  | reactredux | [C#]     | Web/MVC/SPA |
| 带有 Vue.js 的 MVC ASP.NET 核心              | vue        | [C#]     | Web/MVC/SPA | 

若要创建新项目使用 SPA 模板之一时，包含**短名称**中的模板的[dotnet 新](/dotnet/core/tools/dotnet-new)命令。 以下命令将创建与 ASP.NET 核心 MVC 配置为在服务器端角度的应用程序：

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a>设置运行时配置模式

存在两个主运行时配置模式：
* **开发**:
    * 包括源映射，以便于调试。
    * 不优化性能的客户端代码。
* **生产**:
    * 排除源映射。
    * 优化通过绑定和缩减的客户端代码。

ASP.NET 核心使用名为的环境变量`ASPNETCORE_ENVIRONMENT`来存储配置模式。 请参阅**[将环境设置](xref:fundamentals/environments#setting-the-environment)**有关详细信息。

### <a name="running-with-net-core-cli"></a>使用.NET Core CLI 运行

在项目根目录中运行以下命令，以还原所需的 NuGet 和 npm 包：

```console
dotnet restore && npm i
```

生成并运行应用程序：

```console
dotnet run
```

在根据本地主机上的应用程序启动[运行时配置模式下](#runtime-config-mode)。 导航到`http://localhost:5000`浏览器中显示登录页。

### <a name="running-with-visual-studio-2017"></a>使用 Visual Studio 2017 运行

打开*.csproj*生成文件[dotnet 新](/dotnet/core/tools/dotnet-new)命令。 在项目打开时自动还原所需的 NuGet 和 npm 包。 此还原过程可能需要几分钟时间，并已准备好它完成时要运行的应用程序。 单击绿色运行的按钮或按`Ctrl + F5`，应用程序的登录页将打开浏览器。 在根据本地主机上运行应用程序[运行时配置模式下](#runtime-config-mode)。 

<a name="app-testing"></a>

## <a name="testing-the-app"></a>测试应用程序

SpaServices 模板是预配置为运行客户端测试使用[Karma](https://karma-runner.github.io/1.0/index.html)和[Jasmine](https://jasmine.github.io/)。 Jasmine 是常用于单元测试框架 JavaScript，而 Karma 是这些测试的测试运行程序。 Karma 配置为使用[Webpack 开发人员中间件](#webpack-dev-middleware)以便开发人员不需要停止并运行测试，每次进行更改。 无论是针对测试用例或测试用例本身运行的代码，则测试将自动运行。

使用作为示例的角度的应用程序，已为提供两个 Jasmine 测试用例`CounterComponent`中*counter.component.spec.ts*文件：

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

打开命令提示符下，在项目根目录位置，并运行以下命令：

```console
npm test
```

该脚本将启动 Karma 测试运行程序，读取中定义的设置*karma.conf.js*文件。 除了其他设置以外， *karma.conf.js*标识要执行通过的测试文件其`files`数组：

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a>发布应用程序

将生成的客户端资产和已发布的 ASP.NET Core 项目合并为准备就绪，可以部署包可能会很麻烦。 SpaServices 幸运的是，具有名为自定义 MSBuild 目标安排该整个发布进程`RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

MSBuild 目标具有以下职责：
1. 还原 npm 包
1. 创建第三方、 客户端资产的生产级版本
1. 创建自定义客户端资产的生产级版本
1. 将 Webpack 生成资产复制到发布文件夹

运行时，将调用 MSBuild 目标：

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>其他资源

* [角度文档](https://angular.io/docs)
