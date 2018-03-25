---
title: "将在 ASP.NET 中的 machineKey"
author: rick-anderson
description: "发现如何替换 machineKey ASP.NET 以允许使用新且更安全的数据保护系统中。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 8113780dd7b8b3010cf77d14abb45730a2f28959
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="replace-machinekey-in-aspnet"></a>将在 ASP.NET 中的 machineKey

<a name="compatibility-replacing-machinekey"></a>

实现`<machineKey>`在 ASP.NET 中的元素[是可替换](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)。 这允许对 ASP.NET 加密例程的大多数调用，以通过替换数据保护机制，包括新的数据保护系统路由。

## <a name="package-installation"></a>包安装

> [!NOTE]
> 新的数据保护系统只能安装到现有的 ASP.NET 应用程序面向.NET 4.5.1 或更高版本。 安装将失败，如果应用程序面向.NET 4.5 或降低。

若要安装新的数据保护系统到现有的 ASP.NET 4.5.1+ 项目之后，安装包 Microsoft.AspNetCore.DataProtection.SystemWeb。 这将实例化数据保护系统使用[默认配置](xref:security/data-protection/configuration/default-settings)设置。

当你安装此程序包时，它将插入行到*Web.config*这是告诉 ASP.NET，以将其用于[最加密操作](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)，包括窗体身份验证、 视图状态和调用MachineKey.Protect。 插入的行，如下所示读取。

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> 你可以判断是否处于活动状态通过检查类似的字段访问新的数据保护系统`__VIEWSTATE`，这应该以开头"CfDJ8"以下示例所示。 "CfDJ8"是标识受数据保护系统的负载的幻"09 F0 C9 F0"标头的 base64 表示。

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a>包配置

数据保护系统是具有默认零安装程序配置实例化。 但是，由于默认情况下会将密钥保存到本地文件系统，这不起作用的应用程序的场中部署的状态。 若要解决此问题，你可以通过创建一种类型的子类 DataProtectionStartup 提供配置，并重写其 ConfigureServices 方法。

下面是一个示例配置将在其中保留密钥，又如何要加密对静止的自定义的数据保护启动类型。 它还通过提供其自己的应用程序名称来重写默认应用程序隔离策略。

```csharp
using System;
using System.IO;
using Microsoft.AspNetCore.DataProtection;
using Microsoft.AspNetCore.DataProtection.SystemWeb;
using Microsoft.Extensions.DependencyInjection;

namespace DataProtectionDemo
{
    public class MyDataProtectionStartup : DataProtectionStartup
    {
        public override void ConfigureServices(IServiceCollection services)
        {
            services.AddDataProtection()
                .SetApplicationName("my-app")
                .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\myapp-keys\"))
                .ProtectKeysWithCertificate("thumbprint");
        }
    }
}
```

>[!TIP]
> 你还可以使用`<machineKey applicationName="my-app" ... />`代替 SetApplicationName 显式调用。 这是不会强制执行开发人员可以创建 DataProtectionStartup 派生类型，如果他们想要配置所有已设置应用程序名称的方便机制。

若要启用此自定义配置，请返回到 Web.config，并查找`<appSettings>`包安装到的配置文件添加的元素。 它将类似以下的标记：

```xml
<appSettings>
  <!--
  If you want to customize the behavior of the ASP.NET Core Data Protection stack, set the
  "aspnet:dataProtectionStartupType" switch below to be the fully-qualified name of a
  type which subclasses Microsoft.AspNetCore.DataProtection.SystemWeb.DataProtectionStartup.
  -->
  <add key="aspnet:dataProtectionStartupType" value="" />
</appSettings>
```

填写的空白值与你刚刚创建的 DataProtectionStartup 派生的类型的程序集限定名称。 如果应用程序的名称是 DataProtectionDemo，这将如下所示下面。

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

最新配置数据保护系统现已在应用程序中使用。
