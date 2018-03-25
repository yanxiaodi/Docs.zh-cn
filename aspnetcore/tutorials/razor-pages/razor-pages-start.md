---
title: "在 ASP.NET Core 中开始使用 Razor Pages"
author: rick-anderson
description: "在 ASP.NET Core 中开始使用 Razor Pages"
manager: wpickett
ms.author: riande
ms.date: 12/22/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 54fa970d136de3903ae08b710b55f15f96f9a012
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a>在 ASP.NET Core 中开始使用 Razor Pages

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本教程介绍构建 ASP.NET Core Razor Pages Web 应用的基础知识。 Razor Pages 是在 ASP.NET Core 中为 Web 应用生成 UI 时建议使用的方法。

本教程提供 3 个版本：

* Windows：本教程
* MacOS：[借助 Visual Studio for Mac 开始使用 Razor 页面](xref:tutorials/razor-pages-mac/razor-pages-start)
* macOS、Linux 和 Windows：[利用 Visual Studio Code 在 ASP.NET Core 中开始使用 Razor Pages](xref:tutorials/razor-pages-vsc/razor-pages-start)

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

## <a name="prerequisites"></a>系统必备

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a>创建 Razor Web 应用

* 从 Visual Studio“文件”菜单中选择“新建” > “项目”。
* 创建新的 ASP.NET Core Web 应用程序。 将项目命名为“RazorPagesMovie”。 将项目命名为“RazorPagesMovie”，以便命名空间在你复制/粘贴代码时相互匹配。
  ![新建 ASP.NET Core Web 应用程序](../../mvc/razor-pages/index/_static/np.png)
* 在下拉列表中选择“ASP.NET Core 2.0”，然后选择“Web 应用程序”。

  [!INCLUDE[install 2.0](../../includes/dotnetcore-on-dotnetfx-vs.md)]

Visual Studio 模板创建初学者项目：

![“解决方案资源管理器”](razor-pages-start/_static/se.png)

按 F5 在调试模式下运行应用，或按 Ctrl-F5 在运行（不附加调试器）

![主页或索引页](razor-pages-start/_static/home.png)

* Visual Studio 启动 [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) 并运行你的应用。 地址栏显示 `localhost:port#`，而不显示 `example.com`。 这是因为 `localhost` 是本地计算机的标准主机名。 Localhost 仅为来自本地计算机的 Web 请求提供服务。 Visual Studio 创建 Web 项目时，为 Web 服务器使用随机端口。 在上图中，端口号为 5000。 运行应用时，将看到不同的端口号。
* 使用“Ctrl+F5”启动应用（非调试模式）后，可执行代码更改、保存文件、刷新浏览器和查看代码更改等操作。 许多开发人员更喜欢使用非调试模式快速启动应用并查看更改。

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[下一篇：添加模型](xref:tutorials/razor-pages/model)

>[!div class="step-by-step"]
[下一篇：添加模型](xref:tutorials/razor-pages/model)
