---
title: "使用单页应用程序模板"
author: SteveSandersonMS
description: "了解如何安装并开始使用 ASP.NET Core 单页应用程序 (SPA) 项目模板。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: 63b56de101199e9ea0d66d89d2dd7288e47902f6
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/23/2018
---
# <a name="use-the-single-page-application-templates"></a>使用单页应用程序模板

> [!NOTE]
> 已发布的 .NET Core 2.0.x SDK 包括 Angular、React 以及带 Redux 的 React 早期项目模板。 本文档并不涉及这些早期项目模板。 本文档适用于最新的 Angular、React 以及带 Redux 的 React 模板，这些模板可手动安装到 ASP.NET Core 2.0 中。 ASP.NET Core 2.1 中默认包含这些模板。

## <a name="prerequisites"></a>系统必备

* [.NET Core SDK](https://www.microsoft.com/net/download) 版本 2.0.0 或更高版本
* [Node.js](https://nodejs.org) 版本 6 或更高版本

## <a name="installation"></a>安装

如果拥有 ASP.NET Core 2.0，请运行以下命令，安装 Angular、React 以及带 Redux 的 React 更新 ASP.NET Core 模板：

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

## <a name="use-the-templates"></a>使用模板

- [使用 Angular 项目模板](xref:spa/angular)
- [使用 React 项目模板](xref:spa/react)
- [使用带 Redux 的 React 项目模板](xref:spa/react-with-redux)
