---
title: "ASP.NET Core 1.1 入门"
author: rick-anderson
description: "介绍如何使用 ASP.NET Core 1.1 创建并运行简单的 Hello World 应用的快速教程。"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: 895e91efbba931923540e4cd182862cbc1851585
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-11"></a>ASP.NET Core 1.1 入门

> [!NOTE]
> 这些说明适用于 ASP.NET Core 1.1。 在寻找最新版本？ 请参阅[本教程的当前版本](xref:getting-started)。

1. 从 [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 下载页面](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md)安装 SDK 1.0.4 的 .NET Core SDK 安装程序。

2. 为新 .NET Core 项目创建文件夹。

   在 macOS 和 Linux 上，打开终端窗口。 在 Windows 上，打开命令提示符。

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. 如果已计算机上安装了 SDK 的更高版本，则创建一个 global.json 文件以选择 1.0.4 SDK。

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. 创建新的 .NET Core 项目。

   ```terminal
   dotnet new web
   ```
   
3.  还原程序包。

    ```terminal
    dotnet restore
    ```

4. 运行应用。

   如有需要，`dotnet run` 命令会首先生成应用。

   ```terminal
   dotnet run
   ```

5. 浏览到 `http://localhost:5000`

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a>后续步骤

有关入门教程，请参阅 [ASP.NET Core 教程](tutorials/index.md)

有关 ASP.NET Core 概念和体系结构的简介，请参阅 [ASP.NET Core 简介](index.md)和 [ASP.NET Core 基础知识](fundamentals/index.md)。

ASP.NET Core 应用可使用 .NET Core 或.NET Framework 基类库和运行时。 有关详细信息，请参阅[在 .NET Core 和 .NET Framework 之间进行选择](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)。
