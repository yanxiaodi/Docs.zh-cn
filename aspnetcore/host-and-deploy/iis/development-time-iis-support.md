---
title: "Visual Studio 中针对 ASP.NET Core 的开发时 IIS 支持"
author: shirhatti
description: "发现的用于调试 ASP.NET Core 应用时在 Windows Server 上运行之后 IIS 的支持。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: a8bdf4c0c0399c62666e6e61e70c0298a42c2c12
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/19/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Visual Studio 中针对 ASP.NET Core 的开发时 IIS 支持

作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti)

本指南介绍了[Visual Studio](https://www.visualstudio.com/vs/)支持的用于调试在 IIS 后面运行 Windows Server 上的 ASP.NET Core 应用。 本主题将指导完成启用此功能，并设置项目。

## <a name="prerequisites"></a>系统必备

* Visual Studio（2017/版本 15.3 或更高版本）
* ASP.NET 和 Web 开发工作负荷或 .NET Core 跨平台开发工作负荷

## <a name="enable-iis"></a>启用 IIS

启用 IIS。 导航到“控制面板” > “程序” > “程序和功能” > “打开或关闭 Windows 功能”（位于屏幕左侧）。 选中“Internet Information Services”复选框。

![Windows 功能将选中的“Internet Information Services”复选框显示为实心方形（而不是复选标记），指示已启用某些 IIS 功能](development-time-iis-support/_static/enable_iis.png)

如果 IIS 安装需要重新启动，重新启动系统。

## <a name="enable-development-time-iis-support"></a>启用开发时 IIS 支持

启动 Visual Studio 安装程序。 选择**IIS 支持的开发时间**组件。 列出为可选中了该组件**摘要**面板**ASP.NET 和 web 开发**工作负荷。 这将安装[ASP.NET 核心模块](xref:fundamentals/servers/aspnet-core-module)，即运行 ASP.NET Core 应用所需的本机 IIS 模块。

![修改 Visual Studio 功能：选择“工作负荷”选项卡。 在“Web 和云”部分，选择“ASP.NET 和 Web 开发”面板。 在摘要面板中的可选区域右侧，没有一个复选框的 IIS 支持的开发时间。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>配置项目

创建新的启动配置文件以添加开发时 IIS 支持。 在 Visual Studio 的“解决方案资源管理器”中，右键单击项目，然后选择“属性”。 选择“调试”选项卡。从“启动”下拉列表中选择“IIS”。 确认已为“启动浏览器”功能配置了正确的 URL。

![选择了“调试”选项卡的“项目属性”窗口。 将“配置文件”和“启动”设置设为 IIS。 为“启动浏览器”功能配置了 http://localhost/WebApplication2 地址。 启用“启用匿名身份验证”后，Web Server 设置区域的“应用 URL”字段中也提供相同的地址。](development-time-iis-support/_static/project_properties.png)

或者，手动添加到的启动配置文件[launchSettings.json](http://json.schemastore.org/launchsettings)应用程序中的文件：

```json
{
    "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iis": {
            "applicationUrl": "http://localhost/WebApplication2",
            "sslPort": 0
        }
    },
    "profiles": {
        "IIS": {
            "commandName": "IIS",
            "launchBrowser": "true",
            "launchUrl": "http://localhost/WebApplication2",
            "environmentVariables": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    }
}
```

如果未以管理员身份运行 visual Studio 可能会提示重新启动。 如果出现提示，请重启 Visual Studio。

## <a name="additional-resources"></a>其他资源

* [使用 IIS 在 Windows 上托管 ASP.NET Core](xref:host-and-deploy/iis/index)
* [ASP.NET Core 模块简介](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core 模块配置参考](xref:host-and-deploy/aspnet-core-module)
