---
title: "使用角速度项目模板"
author: SteveSandersonMS
description: "了解如何开始使用 ASP.NET 核心单页面应用程序 (SPA) 项目模板用于角和角速度 CLI。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/angular
ms.openlocfilehash: 07cfd20809acb67bdae6561b6ccd6edf1e70a3fe
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="use-the-angular-project-template"></a>使用角速度项目模板

> [!NOTE]
> 本文档不有关角度项目模板包括在 ASP.NET 核心 2.0。 它是有关与其则可手动更新较新角度模板。 默认情况下，该模板包含在 ASP.NET 核心 2.1。

更新的角度项目模板提供了 ASP.NET Core 应用使用角和角速度 CLI 可以实现的丰富的客户端用户界面 (UI) 的方便的起始点。

该模板是等效于创建充当 API 后端的 ASP.NET Core 项目和一个角度 CLI 项目，以充当用户界面。 模板提供承载这两种项目类型中的单个应用程序项目的便利性。 因此，可以生成应用程序项目并将其发布为单个单元。

## <a name="create-a-new-app"></a>创建新的应用程序

如果使用 ASP.NET 核心 2.0，请确保你已[安装更新的角度项目模板](xref:spa/index#installation)。 如果你有 ASP.NET 核心 2.1，则不需要安装它。

从命令提示符下使用命令创建新项目`dotnet new angular`空的目录中。 例如，以下命令创建的应用程序中*我新应用*目录并切换到该目录：

```console
dotnet new angular -o my-new-app
cd my-new-app
```

从 Visual Studio 或.NET Core CLI 运行应用程序：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

打开生成*.csproj*文件中，并从那里运行正常的应用程序。

生成过程还原首次运行，可能需要几分钟的 npm 依赖关系。 后续的生成处于快得多。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

确保你具有调用的环境变量`ASPNETCORE_Environment`值为`Development`。 在 Windows 上 （在非 PowerShell 提示），运行`SET ASPNETCORE_Environment=Development`。 在 Linux 或 macOS 上，运行`export ASPNETCORE_Environment=Development`。

运行[dotnet 生成](/dotnet/core/tools/dotnet-build)以验证该应用程序正确生成。 首次在运行，生成过程还原 npm 依赖关系，可能需要几分钟。 后续的生成处于快得多。

运行[dotnet 运行](/dotnet/core/tools/dotnet-run)以启动应用程序。 类似于以下的消息将记录：

```console
Now listening on: http://localhost:<port>
```

导航到此 URL 在浏览器。

应用程序启动的后台中的角度 CLI 服务器实例。 记录类似于以下消息： *NG Live 开发服务器正在侦听 localhost:&lt;otherport&gt;，打开浏览器上 http://localhost:&lt;otherport&gt; /*. 忽略此消息&mdash;它具有**不**组合 ASP.NET Core 和角速度 CLI 应用的 URL。

---

项目模板创建 ASP.NET Core 应用和角速度应用。 ASP.NET Core 应用旨在用于数据访问、 授权和其他服务器端问题。 角度的应用程序，驻留在*ClientApp*子目录，旨在用于所有 UI 问题。

## <a name="add-pages-images-styles-modules-etc"></a>添加页面、 映像，样式、 模块、 等。

*ClientApp*目录包含标准的角度 CLI 应用程序。 请参阅官方[角度文档](https://github.com/angular/angular-cli/wiki)有关详细信息。

略有差异由此模板创建的角度的应用程序和创建角度 cli 本身之间 (通过`ng new`); 但是，应用程序的功能保持不变。 由模板创建此应用程序包含[Bootstrap](https://getbootstrap.com/)-基于布局和一个基本的路由示例。

## <a name="run-ng-commands"></a>运行 ng 命令

在命令提示符下，切换到*ClientApp*子目录：

```console
cd ClientApp
```

如果你有`ng`全局安装的工具，你可以运行任何其命令。 例如，你可以运行`ng lint`， `ng test`，或任何其他[角度 CLI 命令](https://github.com/angular/angular-cli/wiki#additional-commands)。 没有无需运行`ng serve`不过，因为 ASP.NET Core 应用程序处理为服务器端和客户端部分你的应用程序提供服务。 在内部，它使用`ng serve`开发中。

如果你没有`ng`工具安装，运行`npm run ng`相反。 例如，你可以运行`npm run ng lint`或`npm run ng test`。

## <a name="install-npm-packages"></a>安装 npm 包

若要安装第三方 npm 包，使用命令提示符处， *ClientApp*子目录。 例如:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>发布和部署

在开发中，应用将在优化为开发人员方便起见模式下运行。 例如，JavaScript 捆绑包包括源地图 （以便在调试时，你可以看到原始 TypeScript 代码）。 应用程序监视的磁盘上的 TypeScript、 HTML 和 CSS 文件更改自动重新编译并重新加载当它发现更改这些文件。

在生产中，提供您针对性能进行了优化的应用程序的版本。 这被配置为自动发生这种情况。 发布时，生成配置发出缩减，预时间的 (AoT) 编译生成的客户端代码。 与开发内部版本，不同生产生成不需要在服务器上安装的 Node.js (除非已启用[服务器端预呈现](#server-side-rendering))。

您可以使用标准[ASP.NET Core 托管和部署方法](xref:host-and-deploy/index)。

## <a name="run-ng-serve-independently"></a>独立运行"ng 提供"

将项目配置为在开发模式下启动 ASP.NET Core 应用时在后台启动自己的角度 CLI 服务器实例。 这很方便，因为无需手动运行单独的服务器。

没有此默认设置的缺陷。 每次修改你的 C# 代码和应用程序需要重新启动，将 ASP.NET 核心角度 CLI 服务器重新启动。 启动备份需要大约为 10 秒钟。 如果您正在进行频繁的 C# 代码编辑，并且不希望等待重新启动的角度 CLI，运行角度 CLI 服务器外部，独立于 ASP.NET 核心进程。 若要这样做：

1. 在命令提示符下，切换到*ClientApp*子目录，并启动角度 CLI 开发服务器：

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > 使用`npm start`不启动的角度 CLI 开发服务器`ng serve`，以便中的配置*package.json*遵守。 若要将附加参数传递到角度 CLI 服务器，请将它们添加到相关`scripts`行中你*package.json*文件。

2. 修改 ASP.NET Core 应用程序而不是启动其自身的一个使用外部的角度 CLI 实例。 在你*启动*类中，替换`spa.UseAngularCliServer`替换为以下调用：

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

当你启动 ASP.NET Core 应用程序时，它不会启动角度 CLI 服务器。 将改为使用手动启动实例。 这使它能够启动并重新启动速度更快。 它不再等待角度 CLI 来每次重新生成客户端应用。

## <a name="server-side-rendering"></a>服务器端呈现

作为一种性能功能，你可以选择预呈现上服务器以及运行客户端上的应用程序角度。 这意味着浏览器接收表示您的应用程序的初始用户界面，使其能够显示它即使在下载并执行 JavaScript 捆绑包之前的 HTML 标记。 大部分此操作的实现调用角度功能带来[角度通用](https://universal.angular.io/)。

> [!TIP]
> 启用服务器端呈现 (SSR) 引入了大量的额外复杂性同时在开发和部署过程。 读取[SSR 缺点](#drawbacks-of-ssr)确定 SSR 是否适合你的要求。

若要启用 SSR，你需要进行大量添加到你的项目。

在*启动*类，*后*配置行`spa.Options.SourcePath`，和*之前*调用`UseAngularCliServer`或`UseProxyToSpaDevelopmentServer`，添加以下：

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

在开发模式下，此代码将尝试通过运行脚本创建 SSR 捆绑`build:ssr`中, 定义*ClientApp\package.json*。 这将生成名为的角度应用`ssr`，这并不尚未定义。 

在结束`apps`数组中*ClientApp/.angular-cli.json*，定义具有名称的额外应用`ssr`。 使用以下选项：

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

此新 SSR 启用应用程序配置需要两个其他文件： *tsconfig.server.json*和*main.server.ts*。 *Tsconfig.server.json*文件指定 TypeScript 编译选项。 *Main.server.ts*文件期间 SSR 作为代码入口点。

添加新的文件称为*tsconfig.server.json*内*ClientApp/src* (与现有一起*tsconfig.app.json*)，包含以下：

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

此文件将配置角的 AoT 编译器对于调用模块看起来`app.server.module`。 通过创建一个新文件添加这*ClientApp/src/app/app.server.module.ts* (与现有一起*app.module.ts*) 包含以下： 

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

此模块继承自客户端`app.module`并定义哪些额外角度模块 SSR 中均可用。

回想一下，新`ssr`中的条目*.angular cli.json*引用一个入口点文件称为*main.server.ts*。 你尚未尚未添加该文件中，并现在是时候，若要这样做。 创建一个新文件*ClientApp/src/main.server.ts* (与现有一起*main.ts*)，包含以下：

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

此文件的代码是什么 ASP.NET Core 执行每个请求运行时`UseSpaPrerendering`添加到的中间件*启动*类。 处理接收`params`从.NET 代码 （如所请求的 URL) 以及角度 SSR Api 调用以获取生成的 HTML。 

严格来讲，这不足以在开发模式下启用 SSR。 若要使一个最终的更改，以便你的应用程序能否正常工作时发布至关重要。 在您的应用程序的 main 中*.csproj*文件中，将`BuildServerSideRenderer`属性值设置为`true`:

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

这会将配置生成过程运行`build:ssr`在发布过程并将 SSR 文件部署到服务器。 如果你不启用此功能，SSR 将在生产环境中失败。

你的应用程序运行时在开发或生产模式下，角度代码预先将呈现为 HTML 服务器上。 客户端代码执行正常。

### <a name="pass-data-from-net-code-into-typescript-code"></a>将数据从.NET 代码传递到 TypeScript 代码

在 SSR，你可能需要将每个请求数据从 ASP.NET Core 应用程序传递到应用程序角度。 例如，你无法传递 cookie 信息或内容从数据库读取。 若要执行此操作，编辑你*启动*类。 中的回调`UseSpaPrerendering`，设置的值`options.SupplyData`如下所示：

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

`SupplyData`回调允许传递任意、 每个请求、 JSON 序列化数据 （如字符串、 布尔值或数字）。 你*main.server.ts*代码接收为`params.data`。 例如，上面的代码示例将传递一个布尔值作为`params.data.isHttpsRequest`到`createServerRenderer`回调。 你可以将传递给你的应用以支持角任何方式的其他部分。 有关示例，请参阅如何*main.server.ts*传递`BASE_URL`到其构造函数声明为接收它的任何组件的值。

### <a name="drawbacks-of-ssr"></a>SSR 的缺点

并非所有应用都受益于 SSR。 主要优点是任何可察觉的性能。 达到你的应用程序，通过慢速网络连接或慢速的移动设备上的访问者看到初始 UI 快，即使它未采用一段时间才能提取或分析 JavaScript 捆绑包。 但是，许多 Spa 主要用于通过快速的计算机上的快速、 内部公司网络应用程序几乎立刻出现的位置。

同时，没有启用 SSR 明显的缺点。 它向你的开发过程的复杂性。 你的代码必须在两个不同环境中运行： 客户端和服务器端 （在 Node.js 环境中从 ASP.NET Core 调用）。 下面是一些需要牢记的事项：

* SSR 要求 Node.js 安装生产服务器上。 这将自动对于某些部署方案，例如 Azure 应用程序服务，但对于其他操作系统，如 Azure Service Fabric 这种情况。
* 启用`BuildServerSideRenderer`生成标志原因你*node_modules*目录发布。 此文件夹包含 20000 多文件，这提高了部署时间。
* 若要在 Node.js 环境中运行你的代码，它不能依赖于是否存在特定浏览器的 JavaScript Api 例如`window`或`localStorage`。 如果你的代码 （或你引用某些第三方库） 尝试使用这些 Api，你将获得 SSR 期间出错。 例如，不使用 jQuery 因为它引用了在多个位置的特定浏览器的 Api。 若要防止出现错误，您必须避免 SSR 或避免特定浏览器的 Api 或库。 可以将任何对此类 Api 的调用包装在检查以确保它们不在 SSR 过程中调用。 例如，在 JavaScript 或 TypeScript 代码中使用如下所示检查：

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
