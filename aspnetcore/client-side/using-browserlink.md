---
title: "在 ASP.NET 核心中的浏览器链接"
author: ncarandini
description: "说明如何浏览器链接是链接使用一个或多个 web 浏览器在开发环境的 Visual Studio 功能。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-browserlink
ms.openlocfilehash: 3e62bdd180bb1f5e2ce0645a8cf13c9ffe76197e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="browser-link-in-aspnet-core"></a>在 ASP.NET 核心中的浏览器链接 

通过[Nicolò Carandini](https://github.com/ncarandini)， [Mike Wasson](https://github.com/MikeWasson)，和[Tom Dykstra](https://github.com/tdykstra)

浏览器链接是创建开发环境和一个或多个 web 浏览器之间的通信通道的 Visual Studio 中的功能。 你可以使用浏览器链接刷新 web 应用程序在多个浏览器中的，这是适用于跨浏览器测试。

## <a name="browser-link-setup"></a>浏览器链接安装程序

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET 核心 2.x **Web 应用程序**，**空**，和**Web API**模板项目使用[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)元包，其中包含的包引用[Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)。 因此，使用`Microsoft.AspNetCore.All`元包无需任何进一步的操作，以使浏览器链接可供使用。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ASP.NET 核心 1.x **Web 应用程序**项目模板具有的包引用[Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)包。 **空**或**Web API**模板项目要求你添加到的包引用`Microsoft.VisualStudio.Web.BrowserLink`。

由于这是一种 Visual Studio 功能，最简单的方法添加到包**空**或**Web API**模板项目是打开**程序包管理器控制台**(**视图** > **其他窗口** > **程序包管理器控制台**) 并运行以下命令：

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

或者，可以使用**NuGet 包管理器**。 右键单击中的项目名称**解决方案资源管理器**选择**管理 NuGet 包**:

![打开 NuGet 包管理器](using-browserlink/_static/open-nuget-package-manager.png)

查找和安装包：

![添加包使用 NuGet 包管理器](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a>配置

在`Configure`方法*Startup.cs*文件：

```csharp
app.UseBrowserLink();
```

代码通常位于`if`块只在开发环境中，启用浏览器链接，如下所示：

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

有关详细信息，请参阅[使用多个环境](xref:fundamentals/environments)。

## <a name="how-to-use-browser-link"></a>如何使用浏览器链接

如果你拥有打开 ASP.NET Core 项目，Visual Studio 将旁边显示浏览器链接工具栏控件**调试目标**工具栏控件：

![浏览器链接下拉列表菜单](using-browserlink/_static/browserLink-dropdown-menu.png)

通过浏览器链接工具栏控件，你可以：

* 刷新 web 应用程序在多个浏览器中的一次。
* 打开**浏览器链接仪表板**。
* 启用或禁用**Browser Link**。 注意： 默认情况下，Visual Studio 2017 (15.3) 中禁用浏览器链接。
* 启用或禁用[CSS 自动同步](#enable-or-disable-css-auto-sync)。

> [!NOTE]
> 某些 Visual Studio 插件，最值得注意的是*Web 扩展包 2015年*和*Web 扩展包 2017年*，提供扩展的功能的浏览器链接，但某些其他功能不与 ASP 配合使用。NET 核心项目。

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a>刷新 web 应用程序在多个浏览器中的一次

若要选择单个 web 浏览器以启动启动项目时，使用中的下拉列表菜单**调试目标**工具栏控件：

![F5 下拉列表菜单](using-browserlink/_static/debug-target-dropdown-menu.png)

若要同时打开多个浏览器，选择**浏览与...**从相同的下拉列表。 按住 CTRL 键以选择所需的浏览器，然后单击**浏览**:

![同时打开许多浏览器](using-browserlink/_static/open-many-browsers-at-once.png)

下面是 Visual Studio 显示与索引视图打开的屏幕快照以及两个打开的浏览器：

![两个浏览器的示例与同步](using-browserlink/_static/sync-with-two-browsers-example.png)

将鼠标悬停在该浏览器链接工具栏控件，以查看连接到的项目的浏览器：

![将鼠标悬停提示](using-browserlink/_static/hoover-tip.png)

更改索引视图中，并单击浏览器链接刷新按钮后，将更新所有连接的浏览器：

![browsers-sync-to-changes](using-browserlink/_static/browsers-sync-to-changes.png)

浏览器链接也适用于浏览器，从 Visual Studio 外部启动并导航到应用程序 URL。

### <a name="the-browser-link-dashboard"></a>浏览器链接仪表板

从浏览器链接下拉菜单来管理与打开的浏览器的连接打开浏览器链接仪表板：

![open-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

如果连接没有浏览器，您可以通过选择启动非调试会话*用浏览器查看*链接：

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

否则，连接的浏览器在显示时带有显示每个浏览器的页面的路径：

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

如果您愿意，你可以单击要刷新该单个浏览器的列出的浏览器名称。

### <a name="enable-or-disable-browser-link"></a>启用或禁用浏览器链接

当重新启用 Browser Link 禁用它之后时，您必须刷新浏览器以将它们重新连接。

### <a name="enable-or-disable-css-auto-sync"></a>启用或禁用 CSS 自动同步

启用 CSS 自动同步后，对 CSS 文件进行任何更改时自动刷新连接的浏览器。

## <a name="how-does-it-work"></a>Application Insights 如何工作？

浏览器链接使用 SignalR 来创建 Visual Studio 和浏览器之间的通信通道。 启用浏览器链接后，Visual Studio 将充当多个客户端 （浏览器） 可以连接到的 SignalR 服务器。 浏览器链接也在 ASP.NET 请求管道中注册的中间件组件。 此组件将插入特殊`<script>`到每个页请求从服务器的引用。 您可以通过选择查看脚本引用**查看源**在浏览器和左边缘滚动`<body>`标记内容：

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

不修改你的源文件。 中间件组件动态插入脚本引用。 

由于浏览器端代码是所有 JavaScript，则它将适用于 SignalR 支持而无需浏览器插件的所有浏览器。
