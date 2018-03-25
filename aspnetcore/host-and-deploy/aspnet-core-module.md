---
title: "ASP.NET 核心模块配置参考"
author: guardrex
description: "了解如何配置 ASP.NET 核心模块用于承载 ASP.NET Core 应用。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 5aac5cf2b8fd4bc53ba7201645b9bb02a5d1ecae
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-core-module-configuration-reference"></a>ASP.NET 核心模块配置参考

通过[Luke Latham](https://github.com/guardrex)， [Rick Anderson](https://twitter.com/RickAndMSFT)，和[Sourabh Shirhatti](https://twitter.com/sshirhatti)

本文档将说明了如何配置 ASP.NET 核心模块用于承载 ASP.NET Core 应用。 有关 ASP.NET 核心模块和安装说明简介，请参阅[ASP.NET 核心模块概述](xref:fundamentals/servers/aspnet-core-module)。

## <a name="configuration-with-webconfig"></a>Web.config 配置

使用配置 ASP.NET 核心模块`aspNetCore`部分`system.webServer`在站点的节点*web.config*文件。

以下*web.config*发布文件以便进行[framework 相关部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)和配置 ASP.NET 核心模块，以处理站点请求：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

以下*web.config*为发布[独立的部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

当应用程序部署到[Azure App Service](https://azure.microsoft.com/services/app-service/)、`stdoutLogFile`路径设置为`\\?\%home%\LogFiles\stdout`。 路径将保存到 stdout 日志*LogFiles*文件夹，它是一个位置自动创建的服务。

请参阅[子应用程序配置](xref:host-and-deploy/iis/index#sub-application-configuration)的与配置相关的重要说明*web.config*子应用程序中的文件。

### <a name="attributes-of-the-aspnetcore-element"></a>AspNetCore 元素的特性

| 特性 | 描述 | 默认 |
| --------- | ----------- | :-----: |
| `arguments` | <p>可选的字符串属性。</p><p>可执行文件中指定的自变量**processPath**。</p>| |
| `disableStartUpErrorPage` | “真”或“假”。</p><p>如果为 true， **502.5-进程失败**页被禁止显示，并且 502 状态代码页中配置*web.config*优先。</p> | `false` |
| `forwardWindowsAuthToken` | “真”或“假”。</p><p>如果为 true，该令牌将转发到侦听作为每个请求的标头 MS ASPNETCORE WINAUTHTOKEN 的 %aspnetcore_port%的子进程。 它是该进程可以在每个请求此令牌上调用 CloseHandle 责任。</p> | `true` |
| `processPath` | <p>必需的字符串属性。</p><p>将启动侦听 HTTP 请求的进程的可执行文件的路径。 支持相对路径。 如果路径以开始`.`，考虑使用该路径是相对站点根。</p> | |
| `rapidFailsPerMinute` | <p>可选的整数属性。</p><p>指定在指定的进程的次数**processPath**允许每分钟崩溃。 如果超出此限制，该模块将停止启动剩余秒数的进程。</p> | `10` |
| `requestTimeout` | <p>可选的 timespan 属性。</p><p>指定为其 ASP.NET 核心模块等待侦听 %aspnetcore_port%的进程响应的持续时间。</p><p>`requestTimeout`必须指定整分钟数，否则它将默认为 2 分钟。</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>可选的整数属性。</p><p>正常关闭的可执行文件的模块等待的秒数的持续时间时*app_offline.htm*检测到文件。</p> | `10` |
| `startupTimeLimit` | <p>可选的整数属性。</p><p>持续时间以启动侦听端口的进程的可执行文件的模块等待的秒数。 如果超过此时间限制，该模块可终止进程。 模块将尝试重新启动该过程，在它接收新请求，并将继续尝试重新启动此过程在后续的传入请求，除非应用程序启动失败时**rapidFailsPerMinute**次数在最后一个滚动分钟。</p> | `120` |
| `stdoutLogEnabled` | <p>可选布尔属性。</p><p>如果为 true， **stdout**和**stderr**中指定的进程的**processPath**重定向到中指定的文件**stdoutLogFile**。</p> | `false` |
| `stdoutLogFile` | <p>可选的字符串属性。</p><p>为其指定的相对或绝对文件路径**stdout**和**stderr**中指定的进程从**processPath**记录。 相对路径是相对于站点的根目录。 从任何路径`.`是相对于站点根和所有其他路径被视为绝对路径。 路径中提供的任何文件夹必须存在于要创建的日志文件的模块的顺序。 使用下划线分隔符、 时间戳、 进程 ID 和文件扩展名 (*.log*) 添加到最后一个段**stdoutLogFile**路径。 如果`.\logs\stdout`提供一个值，作为示例 stdout 日志另存为*stdout_20180205194132_1934.log*中*日志*文件夹保存在 19:41:32 并且进程 ID 为 1934年 2/5/2018年。</p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a>设置环境变量

环境变量可以为中的过程指定`processPath`属性。 指定的环境变量`environmentVariable`的子元素`environmentVariables`集合元素。 在本部分中设置的环境变量优先于系统环境变量。

下面的示例设置两个环境变量。 `ASPNETCORE_ENVIRONMENT` 配置应用程序的环境以`Development`。 开发人员可能将此值暂时设置*web.config*文件以便强制[开发人员异常页](xref:fundamentals/error-handling)以便在调试应用程序异常时加载。 `CONFIG_DIR` 是一种用户定义的环境变量中，开发人员曾读取启动窗体中用于加载应用程序的配置文件的路径上的值的代码。

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!WARNING]
> 只能设置`ASPNETCORE_ENVIRONMENT`envirnonment 变量`Development`过渡和测试不到不受信任的网络，例如 Internet 可访问的服务器上。

## <a name="appofflinehtm"></a>app_offline.htm

如果具有名称的文件*app_offline.htm*中检测到一个应用程序的根目录下 ASP.NET 核心模块尝试正常关闭应用程序，并停止处理传入请求。 如果应用程序中定义的秒数后仍在运行`shutdownTimeLimit`，ASP.NET 核心模块将终止正在运行的进程。

虽然*app_offline.htm*存在文件，则 ASP.NET 核心模块响应请求通过发回的内容*app_offline.htm*文件。 当*app_offline.htm*删除文件，则下一个请求启动应用程序。

## <a name="start-up-error-page"></a>启动错误页

如果 ASP.NET 核心模块无法启动后端进程或后端进程启动但不能在配置的端口上侦听*502.5 进程失败*状态代码页将出现。 若要禁止显示此页并还原为默认 IIS 502 状态代码页，请使用`disableStartUpErrorPage`属性。 有关配置自定义错误消息的详细信息，请参阅[HTTP 错误`<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/)。

![502.5 进程失败状态代码页](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>日志创建和重定向

ASP.NET 核心模块将重定向`stdout`和`stderr`日志磁盘如果`stdoutLogEnabled`和`stdoutLogFile`属性`aspNetCore`元素设置。 中的任何文件夹`stdoutLogFile`路径必须存在于要创建的日志文件的模块的顺序。 创建日志文件时，将自动添加时间戳和文件扩展名。 日志不轮换，除非进程回收/重新启动时发生。 它负责的托管商来限制日志使用的磁盘空间。 使用`stdout`日志仅建议进行故障诊断应用程序启动问题。 不要使用用于常规应用程序日志记录的 stdout 日志。 对于例程日志记录在 ASP.NET Core 应用程序，使用限制日志文件大小和旋转日志的日志记录库。 有关详细信息，请参阅[第三方日志记录提供程序](xref:fundamentals/logging/index#third-party-logging-providers)。

日志文件名称由后面追加时间戳、 进程 ID 和文件扩展名 (*.log*) 到的最后一段`stdoutLogFile`路径 (通常*stdout*) 由下划线分隔。 如果`stdoutLogFile`路径以结束*stdout*，pid 为 1934 在 19:42:32 2/5/2018年上创建的应用程序日志中的文件名称*stdout_20180205194132_1934.log*。

下面的示例`aspNetCore`元素会配置`stdout`已在 Azure App Service 中托管的应用程序的日志记录。 本地路径或网络共享路径是可接受的本地日志记录。 确认应用程序池用户标识有权写入提供的路径。

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

请参阅[web.config 配置](#configuration-with-webconfig)有关的示例`aspNetCore`中的元素*web.config*文件。

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>代理配置使用 HTTP 协议和配对令牌

在 ASP.NET 核心模块和 Kestrel 之间创建的代理服务器使用 HTTP 协议。 使用 HTTP 是一种性能优化，其中模块和 Kestrel 之间的通信发生在上环回地址从网络接口中移出。 没有任何风险的窃听模块和 Kestrel 从服务器中移出的位置之间的通信。

配对令牌用于保证 Kestrel 收到的请求已由 IIS 代理且不来自某些其他源。 创建并设置环境变量到配对的令牌 (`ASPNETCORE_TOKEN`) 由模块。 此外，配对令牌还设置到每个代理请求的标头 (`MSAspNetCoreToken`)。 IIS 中间件检查它所接收的每个请求，以确认配对令牌标头值与环境变量值相匹配。 如果令牌值不匹配，则将记录请求并拒绝该请求。 配对的令牌的环境变量和模块和 Kestrel 之间的通信无法访问从服务器中移出的位置。 如果不知道配对令牌值，攻击者就无法提交绕过 IIS 中间件中的检查的请求。

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>ASP.NET 核心模块与 IIS 共享配置

ASP.NET 核心模块安装程序使用的特权运行**系统**帐户。 安装程序的本地系统帐户不具有修改权限使用 IIS 共享配置的共享路径，因为达到拒绝访问错误时尝试进行配置中的模块设置*applicationHost.config*共享上。 在将非 IIS 共享配置，请按照下列步骤：

1. 禁用 IIS 共享的配置。
1. 运行安装程序。
1. 导出已更新*applicationHost.config*到共享的文件。
1. 重新启用 IIS 共享的配置。

## <a name="module-version-and-hosting-bundle-installer-logs"></a>模块版本和托管捆绑安装程序日志

若要确定已安装 ASP.NET 核心模块的版本：

1. 在托管系统上，导航到*%windir%\System32\inetsrv*。
1. 找到*aspnetcore.dll*文件。
1. 右键单击该文件并选择**属性**从上下文菜单。
1. 选择**详细信息**选项卡。**文件版本**和**产品版本**表示已安装的模块版本。

Windows 服务器承载捆绑安装程序日志模块位于*c:\\用户\\%username%\\AppData\\本地\\Temp*。将文件命名为*dd_DotNetCoreWinSvrHosting__\<时间戳 > _000_AspNetCoreModule_x64.log*。

## <a name="module-schema-and-configuration-file-locations"></a>模块、 架构和配置文件位置

### <a name="module"></a>模块

**IIS (x86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86/amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

### <a name="schema"></a>架构

**IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>配置

**IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * .vs\config\applicationHost.config

可以通过搜索找到文件*aspnetcore.dll*中*applicationHost.config*文件。 IIS express， *applicationHost.config*文件不存在默认情况下。 在创建文件 *\<application_root >\\.vs\\配置*时从任何 web 应用程序项目开始在 Visual Studio 解决方案。
