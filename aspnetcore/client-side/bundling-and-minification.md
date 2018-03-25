---
title: "绑定和缩减中 ASP.NET 核心"
author: scottaddie
description: "了解如何通过应用绑定和缩减技术优化 ASP.NET 核心 web 应用程序中的静态资源。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: 6c233d0957ce9974adbc6112e6194c072aab0b41
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="bundling-and-minification"></a>绑定和缩减

作者：[Scott Addie](https://twitter.com/Scott_Addie)

本文介绍了应用绑定和缩减，包括如何使用 ASP.NET Core web apps 使用这些功能的好处。

## <a name="what-is-bundling-and-minification"></a>绑定和缩减是什么？

绑定和缩减是可以应用的 web 应用中的两个不同的性能优化。 一起使用时，绑定和缩减提高性能通过减少服务器请求数和减少的请求的静态资产的大小。

绑定和缩减主要提高第一个页面请求加载时间。 一旦已请求网页上，浏览器缓存静态资产 （JavaScript、 CSS 和图像）。 因此，绑定和缩减不时提高性能请求的同一页上或页，请求相同的资产在同一站点上。 如果过期在资产上未正确设置标头和如果未使用绑定和缩减，浏览器的新鲜度试探方法将标记资产陈旧在几天后。 此外，浏览器需要为每个资产的验证请求。 在这种情况下，绑定和缩减提供在第一个页面请求后的提高性能。

### <a name="bundling"></a>绑定

绑定将多个文件合并到单个文件。 绑定可减少的所需呈现 web 资产，例如 web 页的服务器请求数。 可以专门为 CSS、 JavaScript 等创建任意数量的各项捆绑。少选一些文件意味着从浏览器到服务器或提供你的应用程序的服务的少数几个 HTTP 请求。 第一个页面加载性能改善中的此结果。

### <a name="minification"></a>缩减

缩减从代码中移除而无需更改功能的不必要的字符。 结果是显著的大小减少请求资产 （如 CSS、 映像和 JavaScript 文件） 中。 常见的缩减的负面影响包括缩短为一个字符的变量名和删除注释和多余的空格。

请考虑下面的 JavaScript 函数：

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

缩减缩小为以下函数：

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

除了删除注释和多余的空格，则以下参数和变量已重命名名称，如下所示：

原始 | 重命名
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>绑定和缩减的影响

下表概述了单独加载资产与使用绑定和缩减之间的差异：

操作 | 与 B/M | 而无需 B/M | 更改
--- | :---: | :---: | :---:
文件请求  | 7   | 18     | 157%
传输的 KB | 156 | 264.68 | 70%
加载时间 (ms) | 885 | 2360   | 167%

浏览器是相当详细方面 HTTP 请求标头。 发送的总字节数度量值绑定时见到显著减少。 加载时显示的重大进步，但此示例中本地运行。 使用资产的绑定和缩减通过网络传输时，在实现更高的性能增益。

## <a name="choose-a-bundling-and-minification-strategy"></a>选择绑定和缩减策略

MVC 和 Razor 页项目模板提供的绑定和缩减包含的 JSON 配置文件的现成可用解决方案。 第三方工具，如[Gulp](xref:client-side/using-gulp)和[Grunt](xref:client-side/using-grunt)任务流道，完成相同任务的更多的复杂性。 第三方工具开发工作流需要超出绑定和缩减的处理时非常适合&mdash;如 linting 和映像的优化。 通过使用设计时绑定和缩减，在应用程序的部署之前创建缩减的文件。 绑定和在部署前贴图层提供减少的服务器负载的优点。 但是，务必要识别该设计时绑定，缩减会增加生成复杂性，并且仅适用于静态文件。

## <a name="configure-bundling-and-minification"></a>配置绑定和缩减

MVC 和 Razor 页项目模板提供了*bundleconfig.json*配置文件用于定义每个捆绑包的选项。 默认情况下，一个捆绑包配置定义的自定义 javascript (*wwwroot/js/site.js*) 和样式表 (*wwwroot/css/site.css*) 文件：

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

配置选项包括：

* `outputFileName`： 要输出的捆绑文件名称。 可以包含中的相对路径*bundleconfig.json*文件。 **required**
* `inputFiles`： 要将捆绑在一起的文件的数组。 这些是配置文件的相对路径。 **可选**，* 空值会在空的输出文件。 [组合](http://www.tldp.org/LDP/abs/html/globbingref.html)支持模式。
* `minify`： 输出类型缩减选项。 **可选**，*默认值-`minify: { enabled: true }`*
  * 每个输出文件类型有配置选项。
    * [CSS Minifier](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [JavaScript Minifier](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [HTML Minifier](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`： 指示是否将生成的文件添加到项目文件的标志。 **可选**，*默认-false*
* `sourceMap`： 指示是否生成捆绑的文件的源映射的标志。 **可选**，*默认-false*
* `sourceMapRootPath`： 用于存储生成的源代码映射文件的根路径。

## <a name="build-time-execution-of-bundling-and-minification"></a>生成时执行的绑定和缩减

[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet 包启用的绑定执行并在生成时的缩减。 包插入[MSBuild 目标](/visualstudio/msbuild/msbuild-targets)在生成和清理时间运行。 *Bundleconfig.json*文件分析由生成过程以生成基于定义的配置的输出文件。

> [!NOTE]
> BuildBundlerMinifier 属于在为其 Microsoft 不提供支持的 GitHub 上的社区主导项目。 应归档问题[此处](https://github.com/madskristensen/BundlerMinifier/issues)。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

添加*BuildBundlerMinifier*包到你的项目。

生成项目。 以下内容出现在输出窗口：

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>  Minified wwwroot/css/site.min.css
1>  Minified wwwroot/js/site.min.js
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

清除该项目。 以下内容出现在输出窗口：

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

添加*BuildBundlerMinifier*包到你的项目：

```console
dotnet add package BuildBundlerMinifier
```

如果使用 ASP.NET 核心 1.x，还原新添加的包：

```console
dotnet restore
```

生成项目：

```console
dotnet build
```

将显示以下：

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

清除该项目：

```console
dotnet clean
```

显示以下输出：

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>绑定和缩减的即席执行

它是无法在临时上, 运行的绑定和缩减的任务，而不生成项目。 添加[BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/)到你的项目的 NuGet 包：

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> BundlerMinifier.Core 属于在为其 Microsoft 不提供支持的 GitHub 上的社区主导项目。 应归档问题[此处](https://github.com/madskristensen/BundlerMinifier/issues)。

此包扩展以包括.NET 核心 CLI *dotnet 捆绑*工具。 在包管理器控制台 (PMC) 窗口中或在命令行界面，可以执行以下命令：

```console
dotnet bundle
```

> [!IMPORTANT]
> NuGet 包管理器将依赖项添加到 *.csproj 文件作为`<PackageReference />`节点。 `dotnet bundle`命令注册.NET 核心 CLI 时，才`<DotNetCliToolReference />`使用节点。 相应地修改 *.csproj 文件。

## <a name="add-files-to-workflow"></a>将文件添加到工作流

考虑在其中一个示例附加*custom.css*文件会添加与下面类似的：

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

若要 minify *custom.css*和捆绑其与*site.css*到*site.min.css*文件中，添加的相对路径*bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> 或者，可使用以下的组合模式：
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> 此组合模式匹配所有 CSS 文件，并排除缩减的文件模式。

生成应用程序。 打开*site.min.css* ，并注意的内容*custom.css*追加到文件末尾。

## <a name="environment-based-bundling-and-minification"></a>基于环境的绑定和缩减

作为最佳做法，应在生产环境中使用你的应用的捆绑和缩减型文件。 在开发期间，为方便调试应用程序将使原始文件。

指定要通过使用在您的网页中包括哪些文件[环境标记帮助器](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)在您的视图。 环境标记帮助器只呈现其内容，在特定运行时[环境](xref:fundamentals/environments)。

以下`environment`标记将呈现的未处理的 CSS 文件，在运行时`Development`环境：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

以下`environment`标记呈现捆绑和缩减型 CSS 文件，而不在环境中运行时`Development`。 例如，在运行`Production`或`Staging`触发这些样式表的呈现：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a>使用从 Gulp bundleconfig.json

在情况下在其中应用的绑定和缩减工作流需要额外的处理。 示例包括映像优化、 缓存清除功能，和 CDN 资产处理。 若要满足这些要求，则可以将绑定和缩减工作流使用 Gulp。

### <a name="use-the-bundler--minifier-extension"></a>使用捆绑包 （&） Minifier 扩展

Visual Studio[捆绑包 （&) Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier)扩展会转换为 Gulp。

> [!NOTE]
> 捆绑包 （&） Minifier 扩展属于在为其 Microsoft 不提供支持的 GitHub 上的社区主导项目。 应归档问题[此处](https://github.com/madskristensen/BundlerMinifier/issues)。

右键单击*bundleconfig.json*文件在解决方案资源管理器中，然后选择**捆绑包 （&) Minifier** > **转换到的 Gulp...**:

![将转换到的 Gulp 上下文菜单项](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

*Gulpfile.js*和*package.json*文件添加到项目。 支持[npm](https://www.npmjs.com/)中列出的包*package.json*文件的`devDependencies`部分安装。

为全局依赖项安装的 Gulp CLI PMC 窗口中运行以下命令：

```console
npm i -g gulp-cli
```

*Gulpfile.js*文件读取*bundleconfig.json*输入、 输出和设置的文件。

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>手动转换

如果 Visual Studio 和/或捆绑包 （&） Minifier 扩展都不可用，将转换手动。

添加*package.json*文件中的，替换为以下`devDependencies`，与项目根目录：

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

通过在相同的级别中运行以下命令安装依赖项*package.json*:

```console
npm i
```

为全局依赖项安装的 Gulp CLI:

```console
npm i -g gulp-cli
```

复制*gulpfile.js*到项目根目录下面文件：

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>运行 Gulp 任务

若要触发 Gulp 缩减任务之前生成 Visual Studio 中的项目，添加以下[MSBuild 目标](/visualstudio/msbuild/msbuild-targets)*.csproj 文件：

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

在此示例中，在内定义的任何任务`MyPreCompileTarget`目标之前预定义运行`Build`目标。 在 Visual Studio 输出窗口将显示类似于下面的输出：

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
1>[14:17:49] Using gulpfile C:\BuildBundlerMinifierApp\gulpfile.js
1>[14:17:49] Starting 'min:js'...
1>[14:17:49] Starting 'min:css'...
1>[14:17:49] Starting 'min:html'...
1>[14:17:49] Finished 'min:js' after 83 ms
1>[14:17:49] Finished 'min:css' after 88 ms
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

或者，Visual Studio 任务运行程序资源管理器可能用于将 Gulp 任务绑定到特定的 Visual Studio 事件。 请参阅[正在运行的默认任务](xref:client-side/using-gulp#running-default-tasks)有关执行此操作的说明。

## <a name="additional-resources"></a>其他资源

* [使用 Gulp](xref:client-side/using-gulp)
* [使用 Grunt](xref:client-side/using-grunt)
* [使用多个环境](xref:fundamentals/environments)
* [标记帮助程序](xref:mvc/views/tag-helpers/intro)
