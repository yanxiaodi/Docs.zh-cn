---
title: "使用响应项目模板"
author: SteveSandersonMS
description: "了解如何开始使用 ASP.NET 核心单页面应用程序 (SPA) 项目模板用于响应和创建响应应用程序。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: cda9f52d1f5fa1d240e210488bf1bd5c76e49be7
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="use-the-react-project-template"></a>使用响应项目模板

> [!NOTE]
> 本文档不有关响应项目模板包括在 ASP.NET 核心 2.0。 它是有关与其则可手动更新较新响应模板。 默认情况下，该模板包含在 ASP.NET 核心 2.1。

已更新的响应项目模板提供了方便起点 ASP.NET Core 应用使用响应和[创建响应应用](https://github.com/facebookincubator/create-react-app)(CRA) 约定，可以实现的丰富的客户端用户界面 (UI)。

该模板是等效于创建充当 API 的后端，将 ASP.NET Core 项目和标准 CRA 做出反应项目操作作为 UI 中，但承载在能够生成和发布作为一个单元的单个应用程序项目中的便利。

## <a name="create-a-new-app"></a>创建新的应用程序

如果使用 ASP.NET 核心 2.0，请确保你已[安装更新的响应项目模板](xref:spa/index#installation)。 如果你有 ASP.NET 核心 2.1，则不需要安装它。

从命令提示符下使用命令创建新项目`dotnet new react`空的目录中。 例如，以下命令创建的应用程序中*我新应用*目录并切换到该目录：

```console
dotnet new react -o my-new-app
cd my-new-app
```

从 Visual Studio 或.NET Core CLI 运行应用程序：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

打开生成*.csproj*文件中，并从那里运行正常的应用程序。

生成过程还原首次运行，可能需要几分钟的 npm 依赖关系。 后续的生成处于快得多。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

确保你具有调用的环境变量`ASPNETCORE_Environment`值为`Development`。 在 Windows 上 （在非 PowerShell 提示），运行`SET ASPNETCORE_Environment=Development`。 在 Linux 或 macOS 上，运行`export ASPNETCORE_Environment=Development`。

运行[dotnet 生成](/dotnet/core/tools/dotnet-build)以验证你的应用程序正确生成。 首次在运行，生成过程还原 npm 依赖关系，可能需要几分钟。 后续的生成处于快得多。

运行[dotnet 运行](/dotnet/core/tools/dotnet-run)以启动应用程序。

---

项目模板创建一个 ASP.NET Core 应用和响应应用程序。 ASP.NET Core 应用旨在用于数据访问、 授权和其他服务器端问题。 响应应用程序中，驻留在*ClientApp*子目录，旨在用于所有 UI 问题。

## <a name="add-pages-images-styles-modules-etc"></a>添加页面、 映像，样式、 模块、 等。

*ClientApp*目录是标准的 CRA 做出响应的应用程序。 请参阅官方[CRA 文档](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md)有关详细信息。

略有差异通过此模板创建的响应应用程序和项目之间创建由 CRA 然后重试。但是，应用程序的功能保持不变。 由模板创建此应用程序包含[Bootstrap](https://getbootstrap.com/)-基于布局和一个基本的路由示例。

## <a name="install-npm-packages"></a>安装 npm 包

若要安装第三方 npm 包，使用命令提示符处， *ClientApp*子目录。 例如:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>发布和部署

在开发中，应用将在优化为开发人员方便起见模式下运行。 例如，JavaScript 捆绑包包括源地图 （以便在调试时，你可以看到原始源代码）。 该应用监视 JavaScript、 HTML 和 CSS 文件在磁盘上的更改自动重新编译并重新加载当它发现更改这些文件。

在生产中，提供您针对性能进行了优化的应用程序的版本。 这被配置为自动发生这种情况。 发布时，生成配置会发出缩减，transpiled 生成的客户端代码。 与不同的开发生成、 生产生成不需要在服务器上安装的 Node.js。

您可以使用标准[ASP.NET Core 托管和部署方法](xref:host-and-deploy/index)。

## <a name="run-the-cra-server-independently"></a>独立运行 CRA 服务器

将项目配置为在开发模式下启动 ASP.NET Core 应用时在后台启动自己 CRA development server 的实例。 这很方便，因为它意味着无需手动运行单独的服务器。

没有此默认设置的缺陷。 每次修改你的 C# 代码和应用程序需要重新启动，将 ASP.NET 核心 CRA 服务器重新启动。 几秒钟后需要重新启动。 如果您正在进行频繁的 C# 代码编辑，并且不希望等待 CRA 服务器重新启动，运行 CRA 服务器外部，独立于 ASP.NET 核心进程。 若要这样做：

1. 在命令提示符下，切换到*ClientApp*子目录，并启动 CRA 开发服务器：

    ```console
    cd ClientApp
    npm start
    ```

2. 修改 ASP.NET Core 应用程序而不是启动其自身的一个使用外部 CRA 服务器实例。 在你*启动*类中，替换`spa.UseReactDevelopmentServer`替换为以下调用：

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

当你启动 ASP.NET Core 应用程序时，它不会启动 CRA 服务器。 将改为使用手动启动实例。 这使它能够启动并重新启动速度更快。 它不再正在等待响应应用程序，以重新生成每次。
