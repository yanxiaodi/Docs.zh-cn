---
uid: webhooks/diagnostics/debugging
title: "调试 ASP.NET Webhook |Microsoft 文档"
author: rick-anderson
description: "如何调试 ASP.NET Webhook。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 524cdf0246eda9ef213414923cd23a92a01f211e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-debugging"></a>调试 ASP.NET Webhook  

## <a name="debugging-in-azure"></a>在 Azure 中调试

若要在 Azure 中运行时调试 Web 应用程序，请参阅本教程[对 Azure App Service 中使用 Visual Studio 中的 web 应用进行故障排除](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)。

## <a name="debugging-with-source-and-symbols"></a>使用的源和符号进行调试

除了调试你自己的代码，就可以调试 Microsoft ASP.NET webhook 启动，直接在和中的事实所有.NET。 这适用无论是否在本地或远程调试。 首先，将 Visual Studio 查找源文件和符号通过转到配置为**调试**然后**选项和设置**。 设置的选项如下：

![选项和设置](_static/SourceSymbols.png)

然后添加一个链接到[symbolsource.org](http://symbolsource.org)下载的源和符号。 转到**符号**上方的菜单选项卡，添加以下内容作为符号位置：

```
http://srv.symbolsource.org/pdb/Public
```

此外，请确保缓存目录具有一个短名称;否则文件名称可以太长这将导致要加载的符号。 示例路径为：

```
C:\SymCache
```

设置应类似于此：

![选项符号文件位置示例](_static/SymSource.png)
