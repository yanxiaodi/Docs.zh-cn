---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: "从 MSBuild 项目文件中运行 Windows PowerShell 脚本 |Microsoft 文档"
author: jrjlee
description: "本主题介绍如何生成和部署过程的一部分运行的 Windows PowerShell 脚本。 你可以在本地运行脚本 (换而言之，在 b..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: afee7b0621df42a8bc70fc6f7c4a8fd0383fa83a
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>从 MSBuild 项目文件中运行 Windows PowerShell 脚本
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何生成和部署过程的一部分运行的 Windows PowerShell 脚本。 你可以 （换而言之，在生成服务器上） 中运行脚本本地或远程，如目标 web 服务器或数据库服务器上。
> 
> 有很多可能需要运行 Windows PowerShell 的部署后脚本的原因。 例如，你可能希望：
> 
> - 向注册表添加自定义事件源。
> - 生成用于上载的文件系统目录。
> - 清除生成目录。
> - 将条目写入到自定义的日志文件。
> - 发送电子邮件邀请用户的新设置的 web 应用程序。
> - 创建具有适当权限的用户帐户。
> - 配置 SQL Server 实例之间的复制。
> 
> 本主题将演示如何从 Microsoft Build Engine (MSBuild) 项目文件中的自定义目标本地和远程运行 Windows PowerShell 脚本。


本主题窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器解决方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; 来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序Communication Foundation (WCF) 服务和数据库项目。

这些教程的核心的部署方法取决于中介绍的拆分项目文件方法[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在其中生成过程控制由两个项目文件 （&） #x 2014; 一个包含生成适用于每种目标环境和一个包含特定于环境的生成和部署设置的说明。 在生成期间，特定于环境的项目文件合并到环境无关的项目文件中以形成一组完整的生成说明。

## <a name="task-overview"></a>任务概述

若要自动进行的或单步执行部署过程的一部分运行的 Windows PowerShell 脚本，你将需要完成这些高级任务：

- 将 Windows PowerShell 脚本添加到你的解决方案和源代码管理。
- 创建用于将调用 Windows PowerShell 脚本的命令。
- 在命令中的任何保留的 XML 字符进行转义。
- 在你自定义 MSBuild 项目文件中创建的目标和使用**Exec**任务以运行你的命令。

本主题将演示如何执行这些过程。 任务和本主题中的演练假定你已经熟悉 MSBuild 目标和属性，并确保你了解如何使用自定义 MSBuild 项目文件驱动器生成和部署过程。 有关详细信息，请参阅[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)和[了解该生成过程](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。

## <a name="creating-and-adding-windows-powershell-scripts"></a>创建并添加 Windows PowerShell 脚本

本主题中的任务使用名为的示例 Windows PowerShell 脚本**LogDeploy.ps1**演示如何从 MSBuild 运行脚本。 **LogDeploy.ps1**脚本包含的单行条目写入日志文件的简单函数：


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


**LogDeploy.ps1**脚本接受两个参数。 第一个参数表示你想要添加一个条目的日志文件的完整路径和第二个参数表示你想要记录日志文件中的部署目标。 运行脚本时，它会将行添加到此格式中的日志文件中：


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


若要使**LogDeploy.ps1**脚本供 MSBuild，你需要：

- 将脚本添加到源代码管理。
- 将脚本添加到你在 Visual Studio 2010 中的解决方案。

你不需要部署你的解决方案内容，无论你是否计划在生成服务器上或者远程计算机上运行该脚本的脚本。 一个选项是将脚本添加到解决方案文件夹。 在联系人管理器示例中，由于你想要使用 Windows PowerShell 脚本作为部署过程中，最好将脚本添加到发布解决方案文件夹。

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

解决方案文件夹中的内容被复制到源材料的生成服务器。 但是，它们构成任何项目输出的任何部分。

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>在生成服务器上执行 Windows PowerShell 脚本

在某些情况下，你可能想要生成你的项目的计算机上运行 Windows PowerShell 脚本。 例如，你可能使用 Windows PowerShell 脚本以清理生成文件夹或将条目写入到自定义的日志文件。

根据语法，MSBuild 项目文件从运行 Windows PowerShell 脚本等同于常规的命令提示符下运行的 Windows PowerShell 脚本。 你需要调用 powershell.exe 可执行文件，并使用**– 命令**交换机以提供所需 Windows PowerShell 以运行的命令。 (在 Windows PowerShell v2，你还可以使用**– 文件**切换)。 该命令应采取以下格式：


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


例如:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


如果你的脚本的路径包含空格，您需要将文件路径括在单引号前面加 &。 不能使用双引号括起来，因为您已使用它们括起命令：


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


在调用此命令从 MSBuild 时，有一些额外的注意事项。 首先，应包括**– NonInteractive**标志来确保安静地执行脚本。 接下来，你应包括**– ExecutionPolicy**具有相应的自变量值的标志。 这将指定 Windows PowerShell 将应用到你的脚本，并允许你重写默认执行策略，这可能会阻止脚本执行的执行策略。 你可以从这些自变量值中进行选择：

- 值为**不受限制的**将允许 Windows PowerShell 执行你的脚本，而不考虑是否已签名的脚本。
- 值为**RemoteSigned**将允许执行未签名的脚本在本地计算机上创建 Windows PowerShell。 但是，在其他位置中创建的脚本必须进行签名。 （在实践中，你已创建的 Windows PowerShell 脚本本地生成服务器上的可能性非常小）。
- 值为**AllSigned**将允许 Windows PowerShell 执行签名的脚本。

默认执行策略是**Restricted**，它防止 Windows PowerShell 运行所有脚本文件。

最后，你需要在 Windows PowerShell 命令中发生任何保留的 XML 字符进行转义：

- 将使用单引号 **&amp;a p o s;**
- 将使用双引号 **&amp;q u o t;**
- 将使用 &  **&amp;amp;**

- 当你进行这些更改时，你的命令将类似如下：


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


在你自定义 MSBuild 项目文件中，你可以创建新的目标和使用**Exec**任务才能运行此命令：


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


在此示例中，请注意:

- 任何变量，如参数值和 Windows PowerShell 可执行文件的位置被声明为 MSBuild 属性。
- 条件包含以使用户能够重写这些值从命令行。
- **MSDeployComputerName**在项目文件中其他位置声明属性。

作为生成过程的一部分执行此目标时，Windows PowerShell 将运行你的命令和日志条目写入指定的文件。

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>在远程计算机上执行 Windows PowerShell 脚本

Windows PowerShell 是能够在通过远程计算机上运行脚本[Windows 远程管理](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx)(WinRM)。 若要执行此操作，你需要使用[Invoke-command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet。 这使你无需将脚本复制到远程计算机执行你的脚本对一个或多个远程计算机。 任何结果返回到你从中运行脚本的本地计算机。

> [!NOTE]
> 在使用之前**Invoke-command** cmdlet 来执行 Windows PowerShell 脚本在远程计算机上，你需要配置 WinRM 侦听器，用于接受远程消息。 你可以通过运行命令执行此**winrm quickconfig**远程计算机上。 有关详细信息，请参阅[安装和配置 Windows 远程管理的](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx)。


从 Windows PowerShell 窗口中，将使用此语法来运行**LogDeploy.ps1**远程计算机上的脚本：


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> 有多种其他方法的使用**Invoke-command**运行脚本文件，但这种方法是最简单何时需要提供参数值和管理带空格的路径。


当你运行此命令提示符下时，你需要调用 Windows PowerShell 可执行文件，并使用**– 命令**参数来提供你的说明进行操作：


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


如之前，你需要提供一些其他开关和任何保留的 XML 字符进行转义，MSBuild 中运行命令时：


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


最后，如前所述，可以使用**Exec**内用于执行命令的自定义 MSBuild 目标的任务：


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


作为生成过程的一部分执行此目标时，Windows PowerShell 将在指定的计算机上运行你的脚本**– computername**自变量。

## <a name="conclusion"></a>结束语

本主题介绍如何从 MSBuild 项目文件中运行 Windows PowerShell 脚本。 此方法可用于自动进行的或单步执行的生成和部署过程的一部分运行的 Windows PowerShell 脚本，本地或远程计算机上。

## <a name="further-reading"></a>其他阅读材料

有关 Windows PowerShell 脚本签名和管理执行策略的指南，请参阅[运行 Windows PowerShell 脚本](https://technet.microsoft.com/library/ee176949.aspx)。 有关从远程计算机运行 Windows PowerShell 命令的指南，请参阅[运行远程命令](https://technet.microsoft.com/library/dd819505.aspx)。

使用自定义 MSBuild 项目文件来控制部署过程的详细信息，请参阅[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)和[了解该生成过程](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。

>[!div class="step-by-step"]
[上一页](taking-web-applications-offline-with-web-deploy.md)
[下一页](troubleshooting-the-packaging-process.md)
