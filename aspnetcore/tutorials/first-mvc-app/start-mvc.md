---
title: "ASP.NET Core MVC 和 Visual Studio 入门"
author: rick-anderson
description: "ASP.NET Core MVC 和 Visual Studio 入门"
manager: wpickett
ms.author: riande
ms.date: 10/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: d07d133aa0ed83962b6dc60b9fa0c42993f87843
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a>ASP.NET Core MVC 和 Visual Studio 入门

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[consider RP](../../includes/razor.md)]

本教程提供 3 个版本：

* macOS：[使用 Visual Studio for Mac 创建 ASP.NET Core MVC 应用](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows：[使用 Visual Studio 创建 ASP.NET Core MVC 应用](xref:tutorials/first-mvc-app/start-mvc)
* macOS、Linux 和 Windows：[使用 Visual Studio Code 创建 ASP.NET Core MVC 应用](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="install-visual-studio-and-net-core"></a>安装 Visual Studio 和 .NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

安装 Visual Studio Community 2017。 选择社区下载。 如果已安装 Visual Studio 2017，请跳过此步骤。

* [Visual Studio 2017 主页安装程序](https://www.visualstudio.com/)

运行安装程序，然后选择以下工作负荷：

* “ASP.NET 和 Web 开发”（位于“Web 和云”下）
* “.NET Core 跨平台开发”（位于“其他工具集”下）

![**ASP.NET 和 Web 开发**（位于“Web 和云”下）](start-mvc/_static/web_workload.png)

![**.NET Core 跨平台开发**（位于“其他工具集”下）](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a>创建 Web 应用

在 Visual Studio 中，选择“文件”>“新建”>“项目”。

![“文件”>“新建”>“项目”](start-mvc/_static/alt_new_project.png)

填写“新建项目”对话框：

* 在左侧窗格中，点击“.NET Core”
* 在中间窗格中，点击“ASP.NET Core Web 应用程序(.NET Core)”
* 将项目命名为“MvcMovie”（请务必将项目命名为“MvcMovie”，以便在复制代码时可以与命名空间匹配。）
* 点击“确定”

![新项目对话框, 左侧窗格中的 .NET Core, ASP.NET Core Web ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

完成“新建 ASP.NET 核心 Web 应用程序(.NET Core) - MvcMovie”对话框：

* 在版本选择器下拉框中选择“ASP.NET Core 2.-”
* 选择“Web 应用程序(Model-View-Controller)”
* 点击“确定”

![新项目对话框, 左侧窗格中的 .NET Core, ASP.NET Core Web ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

完成“新建 ASP.NET 核心 Web 应用程序(.NET Core) - MvcMovie”对话框：

* 在版本选择器下拉框中，点击“ASP.NET Core 1.1”
* 点击“Web 应用程序”
* 保留默认的“无身份验证”
* 点击“确定”

![新建 ASP.NET Core Web 应用](start-mvc/_static/p3.png)

---

Visual Studio 为刚刚创建的 MVC 项目使用默认模板。 输入项目名称并选择几个选项后，就拥有了一个可正常运行的应用。 这是一个简单的初学者项目，适合入门使用。

点击“F5”在调试模式下运行应用，或按“Ctrl-F5”在非调试模式下运行应用。
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![运行应用](start-mvc/_static/1.png)

* Visual Studio 启动 [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) 并运行应用。 请注意，地址栏显示 `localhost:port#`，而不显示 `example.com` 之类的内容。 这是因为 `localhost` 是本地计算机的标准主机名。 Visual Studio 创建 Web 项目时，Web 服务器使用的是随机端口。 在上图中，端口号为 5000。 浏览器中的 URL 显示 `localhost:5000`。 运行应用时，将看到不同的端口号。
* 使用“Ctrl+F5”启动应用（非调试模式）后，可执行代码更改、保存文件、刷新浏览器和查看代码更改等操作。 许多开发人员更喜欢使用非调试模式快速启动应用并查看更改。
* 可以从“调试”菜单项中以调试或非调试模式启动应用：

![调试菜单](start-mvc/_static/debug_menu.png)

* 可以通过点击“IIS Express”按钮来调试应用

![IIS Express](start-mvc/_static/iis_express.png)

默认模板提供可用的“主页”、“关于”和“联系”链接。 上面的浏览器图像未显示这些链接。 根据浏览器的大小，可能需要单击导航图标才能显示这些链接。

![右上角的导航图标](start-mvc/_static/2.png)

如果正在调试模式下运行，点击“Shift-F5”以停止调试。

在本教程的下一部分中，我们将了解 MVC 并开始编写一些代码。

>[!div class="step-by-step"]
[下一篇](adding-controller.md)  
