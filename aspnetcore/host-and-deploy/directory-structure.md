---
title: "ASP.NET 核心目录结构"
author: guardrex
description: "请参阅发布的 ASP.NET Core 应用程序的目录结构。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 55e1e0dac32609446243098dbb4a4373f06b4212
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>已发布的 ASP.NET Core 应用的目录结构

作者：[Luke Latham](https://github.com/guardrex)

在 ASP.NET 核，应用程序目录中，*发布*，组成应用程序文件、 配置文件、 静态资产、 包和的运行时 （独立的应用）。

| 应用类型                       | 目录结构 |
| ------------------------------ | ------------------- |
| 依赖于框架的部署 | <ul><li>publish\*<ul><li>日志\*（如果包含在 publishOptions）</li><li>refs\*</li><li>运行时\*</li><li>视图\*（如果包含在 publishOptions）</li><li>wwwroot\* （如果包含在 publishOptions）</li><li>.dll 文件</li><li>myapp.deps.json</li><li>myapp.dll</li><li>myapp.pdb</li><li>myapp。PrecompiledViews.dll （如果预编译 Razor 视图）</li><li>myapp。PrecompiledViews.pdb （如果预编译 Razor 视图）</li><li>myapp.runtimeconfig.json</li><li>（如果包含在 publishOptions） 的 web.config</li></ul></li></ul> |
| 自包含的部署      | <ul><li>publish\*<ul><li>日志\*（如果包含在 publishOptions）</li><li>refs\*</li><li>视图\*（如果包含在 publishOptions）</li><li>wwwroot\* （如果包含在 publishOptions）</li><li>.dll 文件</li><li>myapp.deps.json</li><li>myapp.exe</li><li>myapp.pdb</li><li>myapp。PrecompiledViews.dll （如果预编译 Razor 视图）</li><li>myapp。PrecompiledViews.pdb （如果预编译 Razor 视图）</li><li>myapp.runtimeconfig.json</li><li>（如果包含在 publishOptions） 的 web.config</li></ul></li></ul> |
\*指示目录

内容*发布*目录表示*内容的根路径*，也称为*应用程序基路径*，部署。 任何名称提供给*发布*部署中的目录，其位置用作托管的应用程序服务器的物理路径。 *Wwwroot*目录中，如果存在，仅包含静态资产。 *日志*目录可能包含在部署项目中创建它并添加`<Target>`到下面显示的元素你*.csproj*文件或通过以物理方式上创建目录服务器。

```xml
<Target Name="CreateLogsFolder" AfterTargets="Publish">
  <MakeDir Directories="$(PublishDir)Logs" 
           Condition="!Exists('$(PublishDir)Logs')" />
  <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                    Lines="Generated file" 
                    Overwrite="True" 
                    Condition="!Exists('$(PublishDir)Logs\.log')" />
</Target>
```

`<MakeDir>`元素创建一个空*日志*中已发布的输出文件夹。 元素使用`PublishDir`属性来确定用于创建文件夹的目标位置。 多种部署方法，例如 Web 部署，在部署过程中跳过空文件夹。 `<WriteLinesToFile>`元素生成的文件中*日志*文件夹中，这可确保部署到服务器的文件夹。 请注意，如果工作进程不具有写访问权限的目标文件夹的文件夹创建可能仍会失败。

部署目录中需要读取/Execute 权限，而*日志*目录需要读/写权限。 将在其中写入资产的附加目录需要读/写权限。
