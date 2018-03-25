---
title: "使用 Visual Studio for Mac 向 Razor 页面应用添加模型"
author: rick-anderson
description: "使用 Visual Studio for Mac 在 ASP.NET Core 中向 Razor 页面应用添加模型"
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 9600392b47fb8b1dded06faefaff1bf87d67af4e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-code"></a>使用 Visual Studio Code 在 ASP.NET Core 中向 Razor 页面应用添加模型

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>添加数据模型

* 添加名为“Models”的文件夹。
* 将类添加到名为“Movie.cs”的“Models”文件夹。
* 将以下代码添加到“Models/Movie.cs”文件：

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

生成项目以确定没有任何错误。

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>用于进行迁移的 Entity Framework Core NuGet 包

[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) 中提供了适用于命令行接口 (CLI) 的 EF 工具。 若要安装此包，请将它添加到 .csproj 文件中的 `DotNetCliToolReference` 集合。 注意：必须通过编辑 .csproj 文件来安装此包；不能使用 `install-package` 命令或程序包管理器 GUI。

编辑 RazorPagesMovie.csproj 文件：

* 选择“文件” > “打开文件”，然后选择 RazorPagesMovie.csproj 文件。
* 将 `Microsoft.EntityFrameworkCore.Tools.DotNet` 的 工具引用添加至第二个 \<ItemGroup>：

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE[model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>搭建“电影”模型的基架

* 打开项目目录（包含 Program.cs、Startup.cs 和 .csproj 文件的目录）中的命令窗口。
* 运行下面的命令：

**注意：请在 Windows 上运行以下命令。对于 MacOS 和 Linux，请参阅下一个命令**

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* 在 MacOS 和 Linux 上，请运行以下命令：

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

如果收到错误：
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

退出 Visual Studio，然后重新运行命令。

[!INCLUDE[model 4](../../includes/RP/model4.md)] 下一个教程介绍基架创建的文件。

>[!div class="step-by-step"]
[上一篇：入门](xref:tutorials/razor-pages-vsc/razor-pages-start)
[下一篇：已搭建基架的 Razor 页面](xref:tutorials/razor-pages-vsc/page)
