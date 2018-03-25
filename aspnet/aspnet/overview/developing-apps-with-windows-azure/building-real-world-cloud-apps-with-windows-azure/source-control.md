---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: "源代码管理 （使用 Azure 构建真实世界云应用） |Microsoft 文档"
author: MikeWasson
description: "构建真实世界云应用程序与 Azure 的电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/23/2015
ms.topic: article
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: e3ce68b949199db35c18a09771d99d38562b74e9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>源代码管理 （使用 Azure 构建真实世界云应用）
====================
通过[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下载修复此错误项目](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下载电子书](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **构建真实世界云应用程序与 Azure**电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，从而帮助你为成功开发适用于云中的 web 应用。 有关电子书的信息，请参阅[第一章](introduction.md)。


源代码管理的所有云开发项目，而不仅仅是团队环境至关重要。 你不会认为的编辑源代码或甚至 Word 文档，无需撤消的功能和自动备份和源控件提供这些函数在项目级别它们可以在其中保存时出现问题的更多时间。 与云源控件服务，你不再需要担心复杂的设置，并可以使用 Visual Studio Online 为最多 5 个用户可用的源代码管理。

本章的第一部分介绍了三个关键的最佳做法要记住：

- [自动化脚本视为源代码](#scripts)和版本以及应用程序代码对其。
- [中的机密永远不会检查](#secrets)（例如凭据的敏感数据） 到源代码存储库。
- [设置源分支](#devops)启用 DevOps 工作流。

本章的其余部分提供了这些模式在 Visual Studio、 Azure 和 Visual Studio Online 中的某些示例实现：

- [将脚本添加到 Visual Studio 中的源控件](#vsscripts)
- [在 Azure 中存储敏感数据](#appsettings)
- [在 Visual Studio 和 Visual Studio Online 中使用 Git](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>自动化脚本视为源代码

在你使用在云项目时您要频繁更改操作，并且你想要能够快速响应由你的客户报告的问题。 快速响应涉及使用自动化脚本中所述[使一切自动化](automate-everything.md)章。 所有用于创建你的环境，脚本将部署到它，它等，需要与你的应用程序的源代码同步的小数位数。

若要使脚本与代码同步，请将其存储在源代码管理系统中。 然后如果某个时候需要回滚更改或对不同于开发代码的生产代码中进行快速解决问题，你不必浪费时间想要跟踪哪些设置已更改或哪些团队成员具有所需的版本的副本。 您就可以保证，你需要的脚本是与基本代码，你的需要您就可以保证所有团队成员都使用相同的脚本同步。 然后你是否需要自动测试和部署热修复的生产或开发新的功能，你将有正确的脚本需要更新的代码。

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>未签入机密

通常可太多的人，它是一个密码等敏感数据的适当安全位置到源代码存储库进行访问。 如果脚本依赖于机密例如密码，参数化这些设置，以便它们不保存在源代码中，并将存储您的机密信息的其他位置。

例如，Azure 可让你下载包含的文件以自动发布配置文件创建发布设置。 这些文件包括用户名和密码用于列出被授权管理你的 Azure 服务。 如果你使用此方法来创建发布配置文件，并签入到源代码管理这些文件时，如果有权访问你的存储库的任何人都可以看到这些用户名和密码。 你可以安全地将密码存储在发布配置文件本身由于对其进行加密，并且它是*。 pubxml.user*默认情况下不包括在源代码管理中的文件。

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>结构源分支来促进 DevOps 工作流

在你的存储库中实现分支如何影响你能够同时开发新功能和在生产中修复问题。 此处是中等的大量大小团队使用的模式：

![源分支结构](source-control/_static/image1.png)

主分支始终匹配是在生产环境中的代码。 在开发生命周期中，主分支对应于不同阶段。 Development 分支是实现新功能的位置。 对于小团队可能只是必须 master 和开发，但我们通常建议人具有过渡分支开发和主机之间。 用于最终集成测试更新移动到生产环境之前，可以使用过渡。

对于较大的团队可能针对每个新的功能; 单独的分支对于小团队中，你可能必须签入到 development 分支的每个人。

如果你有每项功能，分支功能 A 时准备你合并其源代码更改向上到开发分支和其他功能分支到向下。 此合并进程的源代码可以非常耗时，并且若要避免该工作的同时仍保持功能分离，一些团队实现一种替代方法，调用*[功能切换](http://en.wikipedia.org/wiki/Feature_toggle)* （也称为*功能标志*)。 这意味着所有的所有功能的代码都在同一个分支，但您启用或禁用通过使用代码中的交换机的每个功能。 例如，假设功能 A 是修复它应用任务，新的字段，并功能 B 添加缓存功能。 这两项功能的代码可以出现在 development 分支，但应用程序将仅显示新字段时变量设置为 true，并且将仅使用缓存设置不同的变量时为 true。 如果功能 A 未准备好进行升级，但功能 B 可以，你可以升级所有到生产环境的代码与功能 A 开关设置为 off 并且功能 B 切换。 你可以然后完成功能 A 并将其升级更高版本，所有与任何源代码合并。

无论使用分支或切换功能，如下的分支结构可以灵活和可重复的方式流动开发到生产环境中的代码。

此结构还可以以快速响应客户反馈。 如果你需要对生产进行快速解决问题，你可以还，有效地执行操作中以灵活的方式。 你可以创建从移出 master 或暂存，分支和何时准备好主数据时将其合并向上和向下到开发和功能的分支。

![修补程序分支](source-control/_static/image2.png)

没有具有其单独的生产和开发分支的分支结构如下，生产问题可能使您处于无需提升新功能的代码，以及生产修复的位置。 新功能代码中可能不是经过充分测试和准备好进行生产，而且可能需要执行大量工作正在放弃未准备好的更改。 或者，你可能必须延迟以便测试更改，并使其准备好部署修复。

接下来你将看到如何在 Visual Studio、 Azure 和 Visual Studio Online 中实现这些三种模式的示例。 这些是示例而不是详细的分步操作方法-到-执行-it 说明;提供所有必需的上下文的详细说明，请参阅[资源](#resources)章结尾部分。

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>将脚本添加到 Visual Studio 中的源控件

它们包含在 Visual Studio 解决方案文件夹中 （假定你的项目在源代码管理中），可以将脚本添加到 Visual Studio 中的源控件。 此处是一种方法来完成此操作。

在解决方案文件夹中创建的脚本的文件夹 (为相同文件夹中你*.sln*文件)。

![自动化文件夹](source-control/_static/image3.png)

将脚本文件复制到的文件夹中。

![自动化文件夹内容](source-control/_static/image4.png)

在 Visual Studio 中，将添加到项目的解决方案文件夹中。

![新的解决方案文件夹菜单选择](source-control/_static/image5.png)

然后，将脚本文件添加到的解决方案文件夹。

![添加现有项菜单选择](source-control/_static/image6.png)

![“添加现有项”对话框](source-control/_static/image7.png)

你的项目中现在包含脚本文件和源控件在跟踪以及相应的源代码更改其版本更改。

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>在 Azure 中存储敏感数据

如果在 Azure 网站中运行你的应用程序时，避免将凭据存储在源代码管理中的一种方法是将它们存储在 Azure 中。

例如，修复它应用程序将存储在其 Web.config 文件两个连接字符串，将要生产和访问你的 Azure 存储帐户的密钥中的密码。

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

如果将实际生产中的这些设置的值你*Web.config*文件，或如果将它们放在*Web.Release.config*文件来配置用于将其插入在部署期间，Web.config 转换它们将存储在源代码存储库中。 如果在生产中输入数据库连接字符串时发布配置文件，密码将在你*.pubxml*文件。 (你无法排除*.pubxml*从源代码管理文件，但共享所有其他部署设置的优势的丧失然后。)

Azure 允许你提供的备用**appSettings**和连接字符串的部分*Web.config*文件。 以下是相关的部分**配置**Azure 管理门户中的 web 站点的选项卡：

![appSettings 和在门户中的 connectionstrings 节](source-control/_static/image8.png)

在项目部署到此网站和应用程序运行时，具有在 Azure 中存储任何值重写任何值位于 Web.config 文件。

通过使用管理门户或脚本，可以在 Azure 中设置这些值。 在中看到环境创建自动化脚本[使一切自动化](automate-everything.md)章创建 Azure SQL 数据库、 获取的存储和 SQL 数据库连接字符串，并将这些机密存储在您的网站的设置。

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

请注意，以便实际值不保存到源存储库的脚本进行参数化。

当你在开发环境中本地运行时，应用程序读取本地 Web.config 文件和您的连接字符串中的 LocalDB SQL Server 数据库指向*应用\_数据*您的 web 项目的文件夹。 在 Azure 中运行应用程序和应用程序尝试从 Web.config 文件中读取这些值，它重新获取并使用时对于网站，不是什么实际在 Web.config 文件中存储的值。

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-visual-studio-online"></a>在 Visual Studio 和 Visual Studio Online 中使用 Git

你可以使用任何源控件环境实现前面提供的 DevOps 分支结构。 分布式团队[分布式版本控制系统](http://en.wikipedia.org/wiki/Distributed_revision_control)(DVCS) 可以达到最佳效果; 对于其他团队[集中式系统](http://en.wikipedia.org/wiki/Revision_control)可能更好地工作。

[Git](http://git-scm.com/)是 DVCS 变得非常流行。 当你使用 Git 进行源代码管理时，你将拥有其历史记录的所有存储库的完整副本，在本地计算机。 很多人喜欢，因为可以很方便可以继续工作时未连接到网络-你可以继续执行提交并回滚，创建和切换分支，和等等。 即使你要连接到网络，它是更方便和快捷以创建分支并切换分支的所有内容时本地。 此外可以执行本地提交和回滚不会影响在其他开发人员。 并且，你可以通过批处理提交之前将它们发送到服务器。

[Microsoft Visual Studio Online](https://www.visualstudio.com/)(VSO) 以前称为 Team Foundation Service 提供这两个 Git 和[Team Foundation 版本控制](https://msdn.microsoft.com/library/ms181237(v=vs.120).aspx)(TFVC; 集中式源代码管理)。 此处在 Microsoft Azure 的组中某些团队使用集中式的源代码管理，某些使用分布式，而某些连接使用混合 （对于某些项目集中式和分布式对其他项目）。 VSO 服务是免费的最多 5 个用户。 你可以注册免费计划[此处](https://go.microsoft.com/fwlink/?LinkId=307137)。

Visual Studio 2013 包含的内置第一类[Git 支持](https://msdn.microsoft.com/library/hh850437.aspx); 下面是快速的工作原理的演示。

在 Visual Studio 2013 中打开项目，右键单击该解决方案中的**解决方案资源管理器**，然后选择**将解决方案添加到源代码管理**。

![将解决方案添加到源代码管理](source-control/_static/image9.png)

Visual Studio 会要求你想要使用 TFVC （集中式的版本控制） 或 Git。

![选择源代码管理](source-control/_static/image10.png)

当您选择 Git 并单击**确定**，Visual Studio 在你的解决方案文件夹中创建新的本地 Git 存储库。 新的存储库有任何文件尚未;你必须通过执行 Git commit 将它们添加到存储库。 右键单击该解决方案中的**解决方案资源管理器**，然后单击**提交**。

![提交](source-control/_static/image11.png)

Visual Studio 自动转移所有提交的项目文件，并将它们在列出**团队资源管理器**中**包含的更改**窗格。 (如果存在一些你不想要包含在提交中，你可以选择它们，右键单击，然后单击**排除**。)

![Team Explorer](source-control/_static/image12.png)

输入提交注释，然后单击**提交**，Visual Studio 将执行提交并显示提交 id。

![团队资源管理器更改](source-control/_static/image13.png)

现在如果你更改某些代码，以便它与什么是存储库中都不同，你可以轻松地查看差异。 右键单击您已更改，请选择一个文件**与 Unmodified 比较**，并获取比较显示它显示在你未提交的更改。

![与未修改进行比较](source-control/_static/image14.png)

![显示更改的差异](source-control/_static/image15.png)

你可轻松看到哪些更改正在进行和签入。

假设你需要进行分支 – 你可以执行该操作在 Visual Studio 中太。 在**团队资源管理器**，单击**新分支**。

![团队资源管理器中新的分支](source-control/_static/image16.png)

输入分支名称，单击**创建分支**，并且选中**签出分支**，Visual Studio 自动签出新分支。

![团队资源管理器中新的分支](source-control/_static/image17.png)

你现在可以对文件进行更改并签入到此分支。 并且可以轻松地切换分支和 Visual Studio 之间自动同步到者为准分支你的文件已签出。在此示例中的网页标题中 *\_Layout.cshtml*已更改为"热修复补丁程序 1"中 HotFix1 分支。

![Hotfix1 分支](source-control/_static/image18.png)

如果你切换回主分支中的内容 *\_Layout.cshtml*文件自动还原到这些主分支中的凭据。

![主分支](source-control/_static/image19.png)

此方式可以快速创建分支和分支之间来回翻转的一个简单的示例。 此功能允许使用的分支结构高度 agile 工作流和自动化脚本中显示[使一切自动化](automate-everything.md)章。 例如，你可以在 Development 分支中工作，创建从 master 中移出热修复补丁程序分支，切换到新分支，使所做的更改那里和提交更改，然后切换回开发分支并继续正在执行的操作。

你已了解了您的工作方式与 Visual Studio 中的本地 Git 存储库。 在团队环境中你通常还将更改推送到的公共储存库。 Visual Studio 工具还使你以指向远程 Git 存储库。 你可以使用 GitHub.com 出于这个目的，或者可以使用[Visual Studio Online 中的 Git](https://msdn.microsoft.com/library/hh850437.aspx)与所有其他 Visual Studio Online 功能，例如工作项和 bug 跟踪集成在一起。

这不是你可以实现敏捷的分支策略，这是当然的唯一方法。 你可以启用使用集中式的源代码控制存储库的相同 agile 工作流。

## <a name="summary"></a>摘要

衡量成功的源代码管理系统基于多快的速度，你可以进行更改，并获取它实时安全且可预测的方式。 如果你发现自己害怕进行更改，因为你所要做一天或两个在其上的手动测试，您可能会问自己你需要执行 process-wise 或 test-wise，以便您可以使所做的更改，以分钟为单位或在最差不再比一小时。 执行该操作的一个策略是以实现持续集成和持续交付，我们将介绍在[下一章](continuous-integration-and-continuous-delivery.md)。

<a id="resources"></a>
## <a name="resources"></a>资源

[Visual Studio Online](https://www.visualstudio.com/)门户提供文档和支持服务，并且你可以注册一个帐户。 如果你有 Visual Studio 2012，并且想要使用 Git，请参阅[Visual Studio Tools for Git](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c)。

有关 （集中式的版本控制） TFVC 和 Git （分布式的版本控制） 的详细信息，请参阅以下资源：

- [应使用哪个版本控制系统： TFVC 或 Git？](https://msdn.microsoft.com/library/vstudio/ms181368.aspx#tfvc_or_git_summary) MSDN 文档，包括汇总 TFVC 和 Git 之间的差异的表。
- [嗯，我喜欢 Team Foundation Server 和我喜欢 Git，但这是更好？](https://blogs.msdn.com/b/visualstudiouk/archive/2013/08/05/well-i-like-team-foundation-server-and-i-like-git-but-which-is-better.aspx) Git 和 TFVC 的比较。

有关分支策略的详细信息，请参阅以下资源：

- [生成使用 Team Foundation Server 2012 的发行管道](https://msdn.microsoft.com/library/dn449957.aspx)。 Microsoft 模式和实践文档。 请参阅有关的分支策略讨论的第 6 章。 自身功能通过功能分支切换和如果使用的功能的分支，律师有帮助保持它们短生存期 （小时数或最多天）。
- [版本控制指南](https://aka.ms/vsarsolutions)。 由 ALM Rangers 指南分支策略。 在下载选项卡上，请参阅分支 Strategies.pdf。
- [使用功能切换软件开发](https://msdn.microsoft.com/magazine/dn683796.aspx)。 MSDN 杂志文章。
- [功能切换](http://martinfowler.com/bliki/FeatureToggle.html)。 功能简介切换/功能 Martin Fowler 博客上的标志。
- [功能在切换 vs 功能分支](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx)。 有关功能开关，通过 Dylan Smith 的另一个博客文章。

有关如何处理不应保存在源控件存储库中的敏感信息的详细信息，请参阅以下资源：

- [用于将密码和其他敏感数据部署到 ASP.NET 和 Azure App Service 的最佳做法](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。
- [Azure 网站： 如何应用程序字符串和连接字符串的工作](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)。 介绍了重写的 Azure 功能`appSettings`和`connectionStrings`中的数据*Web.config*文件。
- [自定义配置和应用程序设置在 Azure 网站-使用 Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/)。

若要保持出源控件的敏感信息的其他方法有关的信息，请参阅[ASP.NET MVC： 外出时的源代码管理中保留私有设置](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx)。

>[!div class="step-by-step"]
[上一页](automate-everything.md)
[下一页](continuous-integration-and-continuous-delivery.md)
