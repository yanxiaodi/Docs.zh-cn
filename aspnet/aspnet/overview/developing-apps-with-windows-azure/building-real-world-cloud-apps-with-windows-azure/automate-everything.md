---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: "自动执行所有内容 （构建使用 Azure 真实世界云应用） |Microsoft 文档"
author: MikeWasson
description: "构建真实世界云应用程序与 Azure 的电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: aa8bb895ed6eaa0ef4c5752f475ea7c911544ef2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>自动执行所有内容 （构建真实世界云应用与 Azure）
====================
通过[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下载修复此错误项目](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下载电子书](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **构建真实世界云应用程序与 Azure**电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，从而帮助你为成功开发适用于云中的 web 应用。 电子书的简介，请参阅[第一章](introduction.md)。


我们将考察的前三个模式实际适用于任何软件开发项目一样，但特别是用于云项目。 此模式为有关自动执行开发任务。 它是一个重要的主题，因为手动流程速度慢而且容易出错;尽可能多的它们作为设置快速、 可靠且 agile 工作流的可能有助于实现自动化。 它是云开发的唯一重要，因为你可以轻松地自动处理许多很难或无法在本地环境中自动运行的任务。 例如，可以设置整个测试环境包括新的 web 服务器和后端 Vm，数据库，blob 存储 （文件存储）、 队列等。

## <a name="devops-workflow"></a>DevOps 工作流

越来越多地听到术语"DevOps。" 术语开发外，您需要将集成开发和操作任务，以便高效地开发软件识别。 你想要启用的工作流的类型是指在其中你可以开发应用程序、 将其部署、 了解从它的生产使用情况、 将其更改以响应您已了解，并快速且可靠地重复这一循环。

一些成功的云开发团队将部署每天到实时环境多次。 用于部署一个较大 Azure 团队更新每 2-3 个月，但现在它版本较少更新的每个 2-3 天和主要释放每 2-3 周。 进入该频率确实可以帮助您立即响应客户反馈。

为此，你必须启用开发和部署周期可重复、 可靠且可预测，并具有低的周期时间。

![DevOps 工作流](automate-everything/_static/image1.png)

换而言之，如果你具有了解功能和当客户使用它，且提供反馈之间的时间段必须尽可能短。 前三个模式 – 使一切，源代码管理自动化和持续集成和传递-就是指最佳做法，我们建议要使该类型的过程。

## <a name="azure-management-scripts"></a>Azure 管理脚本

在[简介此电子书](introduction.md)，你看到的基于 web 的控制台中，Azure 管理门户。 管理门户，你可以监视和管理所有已部署在 Azure 的资源。 它是创建和删除服务，如 web apps 和 Vm、 配置这些服务、 监视服务操作和等等的简单办法。 它是一个强大的工具，但使用它是一个手动过程。 如果你要开发生产应用程序的任何大小，尤其是在团队环境中，我们建议你通过门户 UI 以便了解和探索 Azure，然后自动将重复执行你的进程。

也可以通过调用 REST 管理 API 几乎所有可以在管理门户或从 Visual Studio 手动操作。 你可以编写脚本，使用[Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx)，或者可以使用一种开放源代码框架，如[Chef](http://www.opscode.com/chef/)或[Puppet](http://puppetlabs.com/puppet/what-is-puppet)。 你还可以在 Mac 或 Linux 的环境中使用 Bash 命令行工具。 Azure 具有对所有这些不同的环境，脚本编写 Api，它具有[.NET 管理 API](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx)以防你想要编写代码而不是脚本。

为 Fix It 应用我们已创建了自动执行过程的创建测试环境并将项目部署到该环境中，一些 Windows PowerShell 脚本和我们将回顾一下这些脚本的内容。

## <a name="environment-creation-script"></a>环境创建脚本

我们将查看的第一个脚本名为*新建 AzureWebsiteEnv.ps1*。 它将创建 Azure 环境，你可以解决它将应用部署到用于测试。 此脚本将执行的主要任务如下所示：

- 创建 web 应用。
- 创建存储帐户。 （要求为 blob 和队列，正如在后面的章节看到）。
- 创建 SQL 数据库服务器和两个数据库： 应用程序数据库和成员资格数据库。
- 设置存储在 Azure 应用程序将用于访问存储帐户和数据库。
- 创建将用于自动部署的设置文件。

### <a name="run-the-script"></a>运行脚本


> [!NOTE]
> 本章本部分将显示脚本和你输入才能运行这些命令的示例。 此演示，并不提供你需要知道才能运行的脚本的所有内容。 操作方法-到-执行-it 的分步说明，请参阅[附录： 修复它示例应用程序](the-fix-it-sample-application.md#deploybase)。


若要运行 PowerShell 脚本，用于管理 Azure 服务，必须安装 Azure PowerShell 控制台，配置它以使用你的 Azure 订阅。 你在设置后，你可以使用类似此命令的命令运行修复它环境创建脚本：

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

`Name`参数指定要创建数据库和存储帐户时使用的名称和`SqlDatabasePassword`参数指定将为 SQL 数据库创建的管理员帐户的密码。 有您可以使用，我们将考察更高版本的其他参数。

![PowerShell 窗口](automate-everything/_static/image2.png)

该脚本完成后你可以在管理门户中看到已创建的内容。 你将找到两个数据库：

![数据库](automate-everything/_static/image3.png)

存储帐户：

![存储帐户](automate-everything/_static/image4.png)

和 web 应用：

![网站](automate-everything/_static/image5.png)

上**配置**选项卡对于 web 应用中，你可以看到，它具有存储帐户设置和 SQL 数据库连接字符串设置为修复它应用。

![appSettings 和 connectionStrings](automate-everything/_static/image6.png)

*自动化*文件夹现在还包含 *&lt;websitename&gt;.pubxml*文件。 此文件存储 MSBuild 将用于将应用部署到刚创建的 Azure 环境的设置。 例如:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

如你所见，脚本已创建完整的测试环境中，并在大约 90 秒钟内完成整个过程。

如果你的团队的其他人想要创建的测试环境，它们只需运行该脚本。 不仅是快，而且还它们可以确信它们使用与你正在使用相同的环境。 无法完全确信，如果每个人都已进行设置手动使用管理门户 UI 进行。

### <a name="a-look-at-the-scripts"></a>查看脚本

有实际执行此操作的三个脚本。 调用一个从命令行，并自动使用与其他两个执行的一些任务：

- *新 AzureWebSiteEnv.ps1*是主要的脚本。

    - *新 AzureStorage.ps1*创建存储帐户。
    - *新 AzureSql.ps1*创建数据库。

### <a name="parameters-in-the-main-script"></a>主脚本中的参数

主脚本中，*新建 AzureWebSiteEnv.ps1*，定义多个参数：

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

两个参数是必需的：

- 该脚本创建的 web 应用的名称。 (这也适用于 URL: `<name>.azurewebsites.net`。)
- 数据库服务器脚本创建新管理用户的密码。

可选参数，你可以指定的数据中心位置 （默认为"美国西部"）、 数据库服务器管理员名称 （默认为"dbuser"） 和数据库服务器的防火墙规则。

### <a name="create-the-web-app"></a>创建 web 应用

脚本执行的第一件事是通过调用创建 web 应用`New-AzureWebsite`cmdlet，在向其 web 应用名称和位置将参数值传递：

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>创建存储帐户

然后运行主脚本*新建 AzureStorage.ps1*编写脚本，请指定"*&lt;websitename&gt;*存储"的存储帐户名称，和的相同数据中心位置为web 应用。

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*新 AzureStorage.ps1*调用`New-AzureStorageAccount`cmdlet 来创建存储帐户中，和它返回的帐户名称和访问密钥值。 若要访问的 blob 和队列的存储帐户中，应用程序将需要这些值。

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

你可能并不始终希望创建新的存储帐户;你无法通过添加参数，根据需要将它以使用现有存储帐户定向增强脚本。

### <a name="create-the-databases"></a>创建数据库

然后运行主脚本的数据库创建脚本，*新建 AzureSql.ps1*、 后设置默认数据库和防火墙规则名称：

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

数据库创建脚本检索的开发计算机的 IP 地址，设置防火墙规则，以便开发计算机可以连接到和管理服务器。 数据库创建脚本然后将经历几个步骤来设置数据库：

- 创建服务器时使用`New-AzureSqlDatabaseServer`cmdlet。

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- 创建防火墙规则以启用开发计算机来管理服务器并使 web 应用程序连接到它。 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- 创建包含服务器名称和凭据，通过使用的数据库上下文`New-AzureSqlDatabaseServerContext`cmdlet。

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText`是一个函数调用的脚本中`ConvertTo-SecureString`cmdlet 可加密的密码和返回`PSCredential`对象，相同的类型， `Get-Credential` cmdlet 将返回。
- 通过使用创建应用程序数据库和成员资格数据库`New-AzureSqlDatabase`cmdlet。

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- 调用为每个数据库的本地定义的函数 tocreates 连接字符串。 应用程序将使用这些连接字符串访问数据库。 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get SQLAzureDatabaseConnectionString 是用于从提供给它的参数值中创建的连接字符串的脚本中定义的函数。

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- 返回具有数据库服务器名称和连接字符串的哈希表。

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

修复它应用使用单独的成员身份和应用程序数据库。 还有可能将单个数据库中的成员资格和应用程序数据。

### <a name="store-app-settings-and-connection-strings"></a>应用商店应用程序设置和连接字符串

Azure 具有一种功能，使你能够存储设置和自动重写时它会尝试来读取返回到应用程序的连接字符串`appSettings`或`connectionStrings`Web.config 文件中的集合。 这是与应用的替代[Web.config 转换](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)部署时。 有关详细信息，请参阅[在 Azure 中存储敏感数据](source-control.md#appsettings)此电子书中更高版本。

环境创建脚本将存储在 Azure 所有`appSettings`和`connectionStrings`应用程序需要访问的存储帐户和数据库，它在 Azure 中运行时的值。

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[New Relic](http://newrelic.com/)是我们在中演示的遥测框架[监视和遥测](monitoring-and-telemetry.md)章。 环境创建脚本还会重新启动 web 应用程序，以确保它拾取 New Relic 设置。

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>为部署准备

在过程结束时，环境创建脚本调用两个函数来创建部署脚本将使用的文件。

这些函数之一可创建的发布配置文件*(&lt;websitename&gt;.pubxml*文件)。 该代码调用 Azure REST API，以获取发布设置，并将保存中的信息*.publishsettings*文件。 然后，它使用的模板文件以及该文件中的信息 (*pubxml.template*) 创建*.pubxml*包含发布配置文件的文件。 此两步过程模拟你在 Visual Studio 中所执行的操作： 下载*.publishsettings*文件并导入，若要创建的发布配置文件。

其他函数使用另一个模板文件 (网站 environment.template) 来创建*网站 environment.xml*包含部署脚本将一起使用的设置文件*.pubxml*文件。

### <a name="troubleshooting-and-error-handling"></a>疑难解答和错误处理

脚本就像程序： 它们可能会失败，并且当他们执行你想要了解有关失败和引起可以非常相似的。 值更改为此，环境创建脚本`VerbosePreference`变量从`SilentlyContinue`到`Continue`，以便显示所有详细消息。 它也会更改的值`ErrorActionPreference`变量从`Continue`到`Stop`，以便脚本将停止甚至时遇到非终止错误：

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

它执行任何工作之前，该脚本将存储的开始时间，以便它可以完成后计算经过的时间：

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

它完成其工作后，该脚本将显示的运行时间：

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

和执行每个密钥操作脚本写入详细消息，例如：

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>部署脚本

什么*新建 AzureWebsiteEnv.ps1*脚本执行的环境的创建，*发布 AzureWebsite.ps1*脚本执行的应用程序部署。

部署脚本获取 web 应用程序的名称*网站 environment.xml*创建环境创建脚本文件。

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

它获取部署用户密码从*.publishsettings*文件：

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

它会执行[MSBuild](http://msbuildbook.com/)用于构建和部署项目的命令：

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

如果你指定`Launch`命令行上的参数，它调用`Show-AzureWebsite`cmdlet 打开默认浏览器到网站的 URL。

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

可以使用类似此命令的命令来运行部署脚本：

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

完成后，浏览器将打开与在云中运行的站点和`<websitename>.azurewebsites.net`URL。

![修复此错误应用程序部署到 Windows Azure](automate-everything/_static/image7.png)

## <a name="summary"></a>摘要

使用这些脚本可以确信与使用相同的选项相同的顺序将始终执行相同的步骤。 这有助于确保团队每个开发人员不缺少某些内容或内容扰乱或部署自定义自己不会实际工作方式类似，在另一个团队成员的环境中或在生产环境中的计算机上的内容。

以类似的方式，你可以自动执行大多数 Azure 的管理功能，你可以在管理门户中，执行通过 REST API、 Windows PowerShell 脚本、.NET 语言 API 或一个 Bash 实用程序，你可以运行在 Linux 或 mac。

在[下一章](source-control.md)我们将查看源代码，并解释了为什么很重要，在你源代码存储库中包括你的脚本。

## <a name="resources"></a>资源

- [安装和配置 Windows PowerShell 以 Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1)。 说明如何安装 Azure PowerShell cmdlet 和如何安装，你需要在你的计算机以便管理你的 Azure 帐户的证书。 这是很好的位置，若要开始，因为它还具有用于学习 PowerShell 本身的资源链接。
- [Azure 脚本中心](https://docs.microsoft.com/azure/automation/automation-runbook-gallery)。 WindowsAzure.com 门户开发管理 Azure 服务，其中包含指向入门教程、 cmdlet 参考文档和源代码和示例脚本的脚本资源
- [周末脚本编写器： 开始使用 Azure 和 PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx)。 在专用于 Windows PowerShell 博客中，此文章提供了使用 PowerShell for Azure 的管理功能的出色介绍。
- [安装和配置 Azure 跨平台命令行界面](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)。 关于适用于 Mac 和 Linux 以及 Windows 系统的 Azure 脚本框架入门教程。
- [下载 Azure Sdk 和工具主题的命令行工具部分](https://azure.microsoft.com/downloads/)。 有关文档和下载 azure 命令行工具与相关门户页面。
- [使用 Azure 管理库和.NET 使一切自动化](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx)。 Scott Hanselman 介绍 Azure.NET 管理 API。
- [使用 Windows PowerShell 脚本发布到开发和测试环境](https://msdn.microsoft.com/library/azure/dn642480.aspx)。 说明如何使用的 MSDN 文档发布 Visual Studio 自动生成用于 web 项目的脚本。
- [PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597)。 在 Visual Studio 中添加 Windows PowerShell 语言支持的 visual Studio 扩展。

>[!div class="step-by-step"]
[上一页](introduction.md)
[下一页](source-control.md)
