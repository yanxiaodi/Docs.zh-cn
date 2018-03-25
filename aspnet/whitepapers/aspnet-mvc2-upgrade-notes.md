---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: "升级到 ASP.NET MVC 2 1.0 的 ASP.NET MVC 应用程序 |Microsoft 文档"
author: rick-anderson
description: "本文档描述了如何将手动和使用向导升级到 ASP.NET MVC 2 ASP.NET MVC 1.0 应用程序。 本文档还为 d 提供..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: d1227f0738b2d2a396fed942f32ae5e75579596e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>升级到 ASP.NET MVC 2 1.0 的 ASP.NET MVC 应用程序
====================
> 本文档描述了如何将手动和使用向导升级到 ASP.NET MVC 2 ASP.NET MVC 1.0 应用程序。 本文档还为提供[下载](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>介绍

ASP.NET MVC 2 可以在同一台服务器上的与 ASP.NET MVC 1.0 并行安装。 这使应用程序开发人员灵活地选择何时升级到 ASP.NET MVC 2 的 ASP.NET MVC 1.0 应用程序。

Visual Studio 2010 包括一个向导，该升级现有的 ASP.NET MVC 1.0 项目生成使用 Visual Studio 2008 升级到 ASP.NET MVC 2。 通过在 Visual Studio 2010 中打开一个 ASP.NET MVC 1.0 项目启动升级向导。

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>面向 Visual Studio 2008 SP1 上的 ASP.NET MVC 1.0 升级向导

若要升级到 Visual Studio 2008 SP1 中的 ASP.NET MVC 2 的 ASP.NET MVC 1.0 应用程序，使用 （不受支持） MvcAppConverter 应用程序。 你可以从以下 URL 下载此应用程序：

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>将 ASP.NET MVC 1.0 项目手动升级

若要手动升级到版本 2 现有的 ASP.NET MVC 1.0 应用程序，请按照下列步骤：

1. 创建现有项目的备份。
2. 在文本编辑器中，打开项目文件 （.csproj 或.vbproj 文件扩展名的文件） 和查找 ProjectTypeGuid 元素。 在该元素的值，将 GUID {603c0e0b-db56-11dc-be95-000d561079b0} 与 {F85E285D-A4E0-4152-9332-AB1D724D3325}。 完成后，该元素的值应如下所示： 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. 在 Web 应用程序根文件夹中，编辑 Web.config 文件。 搜索 System.Web.Mvc，版本 = 1.0.0.0，并将所有实例都替换为 System.Web.Mvc，版本 = 2.0.0.0。
4. 为位于 Views 文件夹中的 Web.config 文件中重复上述步骤。
5. 打开项目使用 Visual Studio 中，然后在**解决方案资源管理器**，展开**引用**节点。 删除对 System.Web.Mvc （用于指向 1.0 版程序集） 的引用。 添加到 System.Web.Mvc (v2.0.0.0) 的引用。
6. 将以下 bindingRedirect 元素添加到 Web.config 文件中的配置部分下的应用程序根目录：   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. 创建新的空 ASP.NET MVC 2 应用程序。 从新到脚本文件夹应用程序的现有应用程序的脚本文件夹中复制的文件。
8. 更新现有即™ s Site.css 文件中的 CSS 样式定义的 CSS 文件。
9. 编译应用程序并运行它。 如果出现任何错误，请参阅的重大更改部分[What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038)页。
