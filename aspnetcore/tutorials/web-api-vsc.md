---
title: "使用 ASP.NET Core 和 VS Code 创建 Web API"
author: rick-anderson
description: "在 macOS、Linux 或 Windows 上使用 ASP.NET Core MVC 和 Visual Studio Code 构建 Web API"
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 44566c4014400aa2ca3d512eeaa226637b5f0b97
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a>在Linux、macOS 或 Windows 上使用 ASP.NET Core MVC 和 Visual Studio Code 创建 Web API

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson)

本教程将生成一个用于管理“待办事项”列表的 Web API。 不构造 UI。

本教程提供 3 个版本：

* macOS、Linux、Windows：使用 Visual Studio Code 创建 Web API（本教程）
* macOS：[使用 Visual Studio for Mac 创建 Web API](xref:tutorials/first-web-api-mac)
* Windows：[使用 Visual Studio for Windows 创建 Web API](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a>设置开发环境

下载和安装：
- [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更高版本。
- [Visual Studio Code](https://code.visualstudio.com)
- Visual Studio Code [C# 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

## <a name="create-the-project"></a>创建项目

从控制台运行以下命令：

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

在 Visual Studio Code (VS Code) 中打开“TodoApi”文件夹并选择“Startup.cs”文件。

- 对于警告消息 -“"TodoApi" 中缺少进行生成和调试所需的资产。是否添加它们?”，。 请选择“是”
- 对于信息性消息 -“存在未解析的依赖项”，请选择“还原”。

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![带有“"TodoApi" 中缺少进行生成和调试所需的资产。是否添加它们?” 警告的 VS Code 不再询问、稍后再说、是](web-api-vsc/_static/vsc_restore.png)

按“调试”(F5) 生成并运行程序。 在浏览器中，导航到 http://localhost:5000/api/values。 将显示以下内容：

`["value1","value2"]`

有关 VS Code 的使用技巧，请参阅 [Visual Studio Code 帮助](#visual-studio-code-help)。

## <a name="add-support-for-entity-framework-core"></a>添加对 Entity Framework Core 的支持

在 .NET Core 2.0 中创建新项目会在 TodoApi.csproj 文件中添加“Microsoft.AspNetCore.All”提供程序。 无需单独安装 [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) 数据库提供程序。 此数据库提供程序允许将 Entity Framework Core 和内存数据库一起使用。

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a>添加模型类

模型是表示应用程序中的数据的对象。 在此示例中，唯一的模型是待办事项。

添加名为“模型”的文件夹。 可将模型类置于项目的任意位置，但按照惯例会使用“模型”文件夹。

添加带有以下代码的 `TodoItem` 类：

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

创建 `TodoItem` 时，数据库将生成 `Id`。

## <a name="create-the-database-context"></a>创建数据库上下文

数据库上下文是为给定数据模型协调 Entity Framework 功能的主类。 将通过从 `Microsoft.EntityFrameworkCore.DbContext` 类派生的方式创建此类。

在“模型”文件夹中添加 `TodoContext` 类：

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>添加控制器

在“控制器”文件夹中，创建名为 `TodoController` 的类。 添加以下代码：

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>启动应用

在 VS Code 中，按 F5 启动应用。 导航到 http://localhost:5000/api/todo（之前创建的 `Todo` 控制器）。

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Visual Studio Code 帮助

- [入门](https://code.visualstudio.com/docs)
- [调试](https://code.visualstudio.com/docs/editor/debugging)
- [集成终端](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [键盘快捷键](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Mac 键盘快捷键](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Linux 键盘快捷键](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Windows 键盘快捷键](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


