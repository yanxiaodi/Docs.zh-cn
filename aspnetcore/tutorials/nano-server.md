---
title: "Nano Server 上的 ASP.NET Core"
author: shirhatti
description: "了解如何采用现有的 ASP.NET Core 应用并将其部署到运行 IIS 的 Nano Server 实例。"
manager: wpickett
ms.author: riande
ms.date: 11/04/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/nano-server
ms.openlocfilehash: 4fc5f6874f86130da9f66d13778516d984ff8b46
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-with-iis-on-nano-server"></a>ASP.NET Core 与 Nano Server 上运行的 IIS

作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti)

在本文中，将采用现有的 ASP.NET Core 应用并将其部署到运行 IIS 的 Nano Server 实例。

## <a name="introduction"></a>介绍

Nano Server 是 Windows Server 2016 中的一个安装选项，它占用内存较小、提供更强的安全性，并提供比服务器核心或完全服务器更出色的服务。 请参阅官方 [Nano Server 文档](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server)获取更多详细信息，并获取 180 天评估版本的下载链接。 

可通过三个简单的方法来试用 Nano Server。 使用 MS 帐户登录时：

1. 可以下载 Windows Server 2016 ISO 文件，并生成 Nano Server 映像。

2. 下载 Nano Server VHD。

3. 使用 Azure 库中的 Nano Server 映像在 Azure 中创建 VM。 提供 Azure 免费试用版。

在本教程中，我们将使用第二个选项，即 Windows Server 2016 中的预生成 Nano Server VHD。

在继续阅读本教程之前，需要现有 ASP.NET Core 应用程序的[发布输出](xref:host-and-deploy/directory-structure)。 确保生成的应用程序在 64 位进程中运行。

## <a name="setting-up-the-nano-server-instance"></a>设置 Nano Server 实例

在开发计算机上，借助之前下载的 VHD [使用 HYPER-V 创建新虚拟机](https://technet.microsoft.com/library/hh846766.aspx)。 在登录之前，计算机会要求你设置管理员密码。 在 VM 控制台中，在首次登录前按 F11 设置密码。 还需要检查新 VM 的 IP 地址，可以通过检查在预配 VM 时所提供的 DHCP 服务器的固定 IP，也可以在 Nano Server 恢复控制台的网络设置中执行此操作。

> [!NOTE]
> 假设你的新 VM 使用本地 V4 IP 地址 192.168.1.10 运行。

现在能够使用 PowerShell 远程处理来管理它，这是完全管理 Nano Server 的唯一方法。

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a>使用 PowerShell 远程处理连接到 Nano Server 实例

打开提升的 PowerShell 窗口，向 `TrustedHosts` 列表添加你的远程 Nano Server 实例。

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> 使用正确的 IP 地址替换变量 `$nanoServerIpAddress`。

将 Nano Server 实例添加到 `TrustedHosts` 后，可以使用 PowerShell 远程处理连接到它。

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

成功连接后，会出现一个提示，格式如下：`[192.168.1.10]: PS C:\Users\Administrator\Documents>`

## <a name="creating-a-file-share"></a>创建文件共享

在 Nano Server 上创建文件共享，以便可以将发布的应用程序复制到该文件。 在远程会话中运行以下命令：

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

运行上述命令后，应能够通过访问主计算机的 Windows 资源管理器中的 `\\192.168.1.10\AspNetCoreSampleForNano` 来访问此共享。

## <a name="open-port-in-the-firewall"></a>在防火墙中打开端口

在远程会话中运行以下命令，打开防火墙中的端口以允许 IIS 侦听端口 TCP/8000 上的 TCP 流量。

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a>安装 IIS

从 PowerShell 库添加 `NanoServerPackage` 提供程序。 安装和导入提供程序后，便可以安装 Windows 程序包。

在先前创建的 PowerShell 会话中运行以下命令：

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

若要快速验证是否已正确设置 IIS，可以访问 URL `http://192.168.1.10/`，应该会看到欢迎页。 安装 IIS 时，会默认创建用于侦听端口 80 的名为 `Default Web Site` 的网站。

## <a name="installing-the-aspnet-core-module-ancm"></a>安装 ASP.NET Core 模块 (ANCM)

ASP.NET Core 模块是一个 IIS 7.5+ 模块，它负责 ASP.NET Core HTTP 侦听器的进程管理，并将请求代理到它所管理的进程。 目前，为 IIS 安装 ASP.NET Core 模块的过程为手动操作。 需要在常规（而不是 Nano）计算机上安装 [.NET Core Windows Server 托管捆绑包](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe)。 在常规计算机上安装捆绑包之后，需要将以下文件复制到之前创建的文件共享。

在使用 IIS 的常规（而不是 Nano）服务器上，运行以下复制命令：

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

在 Windows 10 计算机上，将 `C:\windows\system32\inetsrv` 替换为 `C:\Program Files\IIS Express`。

在 Nano 端，需要将以下文件从我们先前创建的文件共享复制到有效位置。 因此，运行以下复制命令：

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

在远程会话中运行以下脚本：

```PowerShell
# Backup existing applicationHost.config
Copy-Item -Path C:\Windows\System32\inetsrv\config\applicationHost.config -Destination  C:\Windows\System32\inetsrv\config\applicationHost_BeforeInstallingANCM.config

Import-Module IISAdministration

# Initialize variables
$aspNetCoreHandlerFilePath="C:\windows\system32\inetsrv\aspnetcore.dll"
Reset-IISServerManager -confirm:$false
$sm = Get-IISServerManager

# Add AppSettings section 
$sm.GetApplicationHostConfiguration().RootSectionGroup.Sections.Add("appSettings")

# Set Allow for handlers section
$appHostconfig = $sm.GetApplicationHostConfiguration()
$section = $appHostconfig.GetSection("system.webServer/handlers")
$section.OverrideMode="Allow"

# Add aspNetCore section to system.webServer
$sectionaspNetCore = $appHostConfig.RootSectionGroup.SectionGroups["system.webServer"].Sections.Add("aspNetCore")
$sectionaspNetCore.OverrideModeDefault = "Allow"
$sm.CommitChanges()

# Configure globalModule
Reset-IISServerManager -confirm:$false
$globalModules = Get-IISConfigSection "system.webServer/globalModules" | Get-IISConfigCollection
New-IISConfigCollectionElement $globalModules -ConfigAttribute @{"name"="AspNetCoreModule";"image"=$aspNetCoreHandlerFilePath}

# Configure module
$modules = Get-IISConfigSection "system.webServer/modules" | Get-IISConfigCollection
New-IISConfigCollectionElement $modules -ConfigAttribute @{"name"="AspNetCoreModule"}
```

> [!NOTE]
> 完成上述步骤后，从共享中删除文件 aspnetcore.dll 和 aspnetcore_schema.xml。

## <a name="installing-net-core-framework"></a>安装 .NET Core Framework

如果应用作为[依赖框架的部署 (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd) 发布，则服务器上必须安装 .NET Core。 若要在 Nano Server 上安装 .NET Core，请在远程 PowerShell 会话中使用 [dotnet-install.ps1 PowerShell 脚本](https://dot.net/v1/dotnet-install.ps1)。 通过 `-Version` 开关传递 CLI 版本：

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a>发布应用程序

将现有应用程序的已发布输出复制到文件共享的根目录。

可能需要更改 web.config 才能指向在其中提取 dotnet.exe 的位置。 或者，可以将 dotnet.exe 添加到你的路径。

以下示例演示 dotnet.exe 不在路径中的情况下，web.config 的可能显示方式：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="C:\dotnet\dotnet.exe" arguments=".\AspNetCoreSampleForNano.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\aspnetcore-stdout" forwardWindowsAuthToken="true" />
  </system.webServer>
</configuration>
```

在远程会话中运行以下命令，在不同的端口（而不是默认网站）上为已发布的应用创建一个新的站点。 此外，还需要打开该端口来访问 Web。 为简单起见，此脚本使用 `DefaultAppPool`。 关于在应用程序池下运行的更多注意事项，请参阅[应用程序池](xref:host-and-deploy/iis/index#application-pools)。

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a>在 Nano Server 上运行 .NET Core CLI 的已知问题和解决方法
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a>运行应用程序

可以在浏览器中转到 `http://192.168.1.10:8000` 访问已发布的 Web 应用。 如果你已按照[日志创建和重定向](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)中所述设置了日志记录，则可以在 C:\PublishedApps\AspNetCoreSampleForNano\logs 中查看日志。
