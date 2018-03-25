---
title: "Mac、Linux 或 Windows 上的 ASP.NET Core MVC 简介"
author: rick-anderson
description: "Mac、Linux 和 Windows 上的 ASP.NET Core MVC 和 Visual Studio Code 入门"
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 4771555b66f328a819f17a32eb3959f9ecf33d44
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a>Mac、Linux 或 Windows 上的 ASP.NET Core MVC 入门

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本教程将介绍基础知识，指导你使用 [Visual Studio Code](https://code.visualstudio.com) (VS Code) 构建 ASP.NET Core MVC Web 应用。 本教程假定用户熟悉 VS Code。 有关详细信息，请参阅 [VS Code 入门](https://code.visualstudio.com/docs) 和 [Visual Studio Code 帮助](#visual-studio-code-help)。 

[!INCLUDE[consider RP](../../includes/razor.md)]

本教程提供 3 个版本：

* macOS：[使用 Visual Studio for Mac 创建 ASP.NET Core MVC 应用](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows：[使用 Visual Studio 创建 ASP.NET Core MVC 应用](xref:tutorials/first-mvc-app/start-mvc)
* macOS、Linux 和 Windows：[使用 Visual Studio Code 创建 ASP.NET Core MVC 应用](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="install-vs-code-and-net-core"></a>安装 VS Code 和 .NET Core

本教程需要 [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更高版本。 请参阅适用于 ASP.NET Core 1.1 版本的 [PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf)。

安装以下组件：

* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更高版本。
* [Visual Studio Code](https://code.visualstudio.com)
* VS Code [C# 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 

## <a name="create-a-web-app-with-dotnet"></a>通过 dotnet 创建 Web 应用

从终端运行以下命令：

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

在 Visual Studio Code (VS Code) 中打开“MvcMovie”文件夹并选择“Startup.cs”文件。

- 对于警告消息 -“"MvcMovie" 中缺少进行生成和调试所需的资产。是否添加它们?”，。 请选择“是”
- 对于信息性消息 -“存在未解析的依赖项”，请选择“还原”。

![带有“"MvcMovie" 中缺少进行生成和调试所需的资产。是否添加它们?” 警告的 VS Code “不再询问”、“稍后再说”、“是”以及“信息 - 存在未解决的依赖项”-“还原”-“关闭”](../web-api-vsc/_static/vsc_restore.png)

按“调试”(F5) 生成并运行程序。

![正在运行的应用](../first-mvc-app/start-mvc/_static/1.png)

VS Code 启动 [Kestrel](xref:fundamentals/servers/kestrel) Web 服务器并运行应用。 请注意，地址栏显示 `localhost:5000` 而非 `example.com` 等内容。 这是因为 `localhost` 是本地计算机的标准主机名。

默认模板提供可用的“主页”、“关于”和“联系”链接。 上面的浏览器图像未显示这些链接。 根据浏览器的大小，可能需要单击导航图标才能显示这些链接。

![右上角的导航图标](../first-mvc-app/start-mvc/_static/2.png)

在本教程的下一部分中，我们将了解 MVC 并开始编写一些代码。

## <a name="visual-studio-code-help"></a>Visual Studio Code 帮助

- [入门](https://code.visualstudio.com/docs)
- [调试](https://code.visualstudio.com/docs/editor/debugging)
- [集成终端](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [键盘快捷键](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Mac 键盘快捷键](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Linux 键盘快捷键](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Windows 键盘快捷键](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[下一篇 - 添加控制器](adding-controller.md)
