---
title: "利用 Visual Studio Code 在 ASP.NET Core 中开始使用 Razor 页面"
author: rick-anderson
description: "利用 Visual Studio Code 在 ASP.NET Core 中开始使用 Razor 页面"
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 7c01d802e59951281c86c8eab64b7c6b9d646fbf
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a>利用 Visual Studio Code 在 ASP.NET Core 中开始使用 Razor 页面

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本教程介绍构建 ASP.NET Core Razor 页面 Web 应用的基础知识。 我们建议在学习本教程前先阅读 [Razor 页面介绍](xref:mvc/razor-pages/index)。 Razor 页面是在 ASP.NET Core 中为 Web 应用程序生成 UI 时建议使用的方法。

## <a name="prerequisites"></a>系统必备

安装以下组件：

* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更高版本
* [Visual Studio Code](https://code.visualstudio.com)
* VS Code [C# 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 

## <a name="create-a-razor-web-app"></a>创建 Razor Web 应用

从终端运行以下命令：

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

上述命令使用 [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) 创建并运行 Razor 页面项目。 打开浏览器，转到 http://localhost:5000 查看应用程序。

![主页或索引页](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>打开项目

按 Ctrl+C 关闭应用程序。

在 Visual Studio Code (VS Code) 中，选择“文件”>“打开文件夹”，然后选择“RazorPagesMovie”文件夹。

- 对警告消息“"RazorPagesMovie" 中缺少进行生成和调试所需的资产。是否添加它们?”选择“是” 。
- 对信息性消息“存在未解析的依赖项”选择“还原”。

### <a name="launch-the-app"></a>启动应用

按 Ctrl+F5 启动应用而不进行调试。 或者，从“调试”菜单中选择“开始执行(不调试)”。

在下一个教程中，我们将向项目添加模型。 

>[!div class="step-by-step"]
[下一篇：添加模型](xref:tutorials/razor-pages-vsc/model)  
