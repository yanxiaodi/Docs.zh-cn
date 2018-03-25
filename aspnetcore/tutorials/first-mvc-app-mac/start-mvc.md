---
title: "ASP.NET Core MVC 和 Visual Studio for Mac 入门"
author: rick-anderson
description: "ASP.NET Core MVC 和 Visual Studio 入门"
manager: wpickett
ms.author: riande
ms.date: 8/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 05a2323851c58c95667066a74c11f1d015405e6f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>ASP.NET Core MVC 和 Visual Studio for Mac 入门

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本教程介绍使用 [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) 生成 ASP.NET Core MVC Web 应用所涉及的基础知识。 

[!INCLUDE[consider RP](../../includes/razor.md)]

本教程提供 3 个版本：

* macOS：[使用 Visual Studio for Mac 生成 ASP.NET Core MVC 应用](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows：[使用 Visual Studio 生成 ASP.NET Core MVC 应用](xref:tutorials/first-mvc-app/start-mvc)
* Linux、macOS和 Windows：[使用 Visual Studio Code 生成 ASP.NET Core MVC 应用](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="prerequisites"></a>系统必备

本教程需要 [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更高版本。

安装以下组件：

- [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更高版本
- [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a>创建 Web 应用

在 Visual Studio 中，选择“文件”>“新建解决方案”。

![macOS 新建解决方案](../first-web-api-mac/_static/sln.png)

选择“.NET Core App”>“ASP.NET Core”>“Web 应用”>“下一步”。

![macOS“新建项目”对话框](start-mvc/1.png)

将项目命名为“MvcMovie”，然后选择“创建”。

![macOS“新建项目”对话框](start-mvc/2.png)

### <a name="launch-the-app"></a>启动应用

在 Visual Studio 中，选择“运行”>“开始执行(不调试)”以启动应用。 Visual Studio 启动 [Kestrel](xref:fundamentals/servers/index#kestrel)，启动浏览器并导航到 `http://localhost:port`，其中的“端口”是随机选择的端口号。

![具有新项目的浏览器](start-mvc/b1.png)

* 地址栏显示 `localhost:port#`，而不是显示 `example.com`。 这是因为 `localhost` 是本地计算机的标准主机名。 Visual Studio 创建 Web 项目时，Web 服务器使用的是随机端口。 运行应用时，将看到不同的端口号。
* 可以从“运行”菜单中以调试或非调试模式启动应用。

默认模板提供了“主页”、“关于”和“联系人”的链接。 上面的浏览器图像未显示这些链接。 根据浏览器的大小，可能需要单击导航图标才能显示这些链接。

![具有新项目的浏览器](start-mvc/b2.png)

在本教程的下一部分中，你将了解 MVC 并开始撰写一些代码。

>[!div class="step-by-step"]
[下一篇](adding-controller.md)  
