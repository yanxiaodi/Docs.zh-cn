---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: "构建真实世界云应用与 Azure |Microsoft 文档"
author: MikeWasson
description: "构建真实世界云解决方案中，此电子书指导你完成基于模式的方法。 模式适用于开发过程以及与..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: 4de0b52e0b4ae7ce00e7b07bce2decfc5068964a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="building-real-world-cloud-apps-with-azure"></a>使用 Azure 构建真实世界云应用
====================
通过[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下载修复此错误项目](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下载电子书](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> 构建真实世界云解决方案中，此电子书指导你完成基于模式的方法。 模式将应用于在开发过程以及体系结构和编码做法。
> 
> 内容基于由 Scott Guthrie 开发，而由他在挪威语开发人员大会 （（ndc）） 在 2013 年 6 月中的演示文稿 ([第 1 部分](http://vimeo.com/68215538)，[第 2 部分](http://vimeo.com/68215602))，和中的 Microsoft 技术 Ed 澳大利亚2013 年 9 月，([第 1 部分](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324)，[第 2 部分](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325))。 [许多其他](more-patterns-and-guidance.md#acknowledgments)更新和从视频将它转换为写入形式时扩充内容。


## <a name="intended-audience"></a>目标的受众

开发人员想要知道的云开发考虑迁移到云，或以云开发将在此找到的最重要的概念和实践他们需要知道的简要概述。 并附带具体示例，以及每个章节链接到其他资源的更深入的信息说明了概念。 示例和其他资源的链接适用于 Microsoft 框架和服务，但是不适用于其他 web 开发框架和云以及环境中所述的原理。

开发人员已为云开发可能会发现想法此处有助于使它们更成功地。 因此，你可以选取并选择你感兴趣的主题，可以独立，读取序列中的每一章。

监视 Scott Guthrie 的任何人*构建真实世界云应用程序与 Azure*演示文稿并希望获得更多详细信息和更新的信息将在此找到的。

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>云开发模式

此电子书说明十三个推荐使用云开发的模式。 "模式"所用此处广义上讲表示建议的方法是执行操作： 如何最好地着手开发、 设计和编码云应用程序。 这些是关键的模式，它将帮助你"属于成功的 pit"，如果你遵循它们。

- [使一切自动化](automate-everything.md)。

    - 使用脚本来最大化效率并最大程度减少重复的操作过程中的错误。
    - 演示： Azure 的管理脚本。
- [源代码管理](source-control.md)。 

    - 设置源代码管理中的分支结构有助于 DevOps 工作流。
    - 演示： 将脚本添加到源代码管理。
    - 演示： 保留出源控件的敏感数据。
    - 演示： 使用 Visual Studio 中的 Git。
- [持续集成和传递](continuous-integration-and-continuous-delivery.md)。 

    - 自动生成和每个源控件中签入的部署。
- [Web 开发最佳做法](web-development-best-practices.md)。 

    - 保留 web 层无状态。
    - 演示： 缩放和自动缩放在 Azure App Service Web Apps 中。
    - 避免会话状态。
    - 使用 CDN。
    - 使用异步编程模型。
    - 演示： 在 ASP.NET MVC 和实体框架中的异步。
- [单一登录](single-sign-on.md)。 

    - Azure Active Directory 简介。
    - 演示： 创建 ASP.NET 应用程序使用 Azure Active Directory。
- [数据存储选项](data-storage-options.md)。 

    - 类型的数据存储。
    - 如何选择正确的数据存储。
    - 演示： Azure SQL 数据库。
- [数据分区策略](data-partitioning-strategies.md)。 

    - 垂直、 水平分区数据或这二者便于扩展关系数据库。
- [非结构化的 blob 存储](unstructured-blob-storage.md)。 

    - 使用 blob 服务在云中存储文件。
    - 演示： 使用修复它应用中的 blob 存储。
- [设计得以失败](design-to-survive-failures.md)。 

    - 类型的故障。
    - 失败作用域。
    - 了解 Sla。
- [监视和遥测](monitoring-and-telemetry.md)。 

    - 为什么应同时购买的遥测应用和编写您自己的代码来检测你的应用程序。
    - 演示： azure 的 New Relic
    - 演示： 在修复该应用程序日志记录代码。
    - 演示： 修复它应用中的依赖关系注入。
    - 演示： 在 Azure 中的内置日志记录支持。
- [暂时性故障处理](transient-fault-handling.md)。 

    - 使用智能重试/退让逻辑可缓解暂时性故障的影响。
    - 演示： 重试/退避 Entity Framework 6 中。
- [分布式缓存](distributed-caching.md)。 

    - 提高可伸缩性以及通过使用分布式缓存减少数据库事务成本。
- [以队列为中心的工作模式](queue-centric-work-pattern.md)。 

    - 启用高可用性，并通过松散耦合 web 和辅助层来提高可伸缩性。
    - 演示： 修复它应用中的 Azure 存储队列。
- [云应用程序模式和指南的更多](more-patterns-and-guidance.md)。
- [附录：“Fix It”示例应用程序](the-fix-it-sample-application.md)

    - 已知问题
    - 最佳做法
    - 如何下载、 生成、 运行和部署。

这些模式适用于所有云环境中，但我们将使用基于 Microsoft 技术和服务，如 Visual Studio、 Team Foundation Service、 ASP.NET 和 Azure 的示例加以说明。

此本章的其余部分介绍修复它示例应用程序和修复它应用在中运行的 Azure App Service 云环境中的 Web 应用。

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>该示例应用程序的修补程序

大部分的屏幕截图和此电子书中所示的代码示例基于修复它应用最初由[Scott Guthrie](https://weblogs.asp.net/scottgu/)来演示推荐的云应用开发模式和实践。

![修复此错误的应用程序主页](introduction/_static/image1.png)

示例应用程序是票证系统的简单工作项。 当你需要解决某些问题时，你创建票证和分配到某人，并且其他人可以登录并查看分配的票证到它们，并将票证标记为已完成时完成的工作。

它是一个标准的 Visual Studio web 项目。 它基于 ASP.NET MVC 构建，并使用 SQL Server 数据库。 它可以在 IIS Express 中本地运行，并且可以部署到 Azure 网站在云中运行。 你可以使用窗体身份验证和本地数据库或通过使用社交提供程序，例如 Google 登录。 （稍后我们将还演示如何能够使用 Active Directory 组织帐户进行登录。）

![登录页](introduction/_static/image2.png)

登录后您可以在创建票证、 将它分配给人，并上载你想要获取固定的图片。

![创建修复它任务](introduction/_static/image3.png)

![修复它创建的任务](introduction/_static/image4.png)

你可以跟踪你创建的工作项的进度，请参阅分配给你、 查看票证详细信息，以及将邮件标记为已完成的票证。

这是一个非常简单的应用，从功能角度看，但你将了解如何生成它，以便它可以扩展到数百万个用户，并将在恢复数据库故障和连接终止数量等。 你将了解如何创建可启动简单和高效地快速地循环开发周期，更好并更好地使应用程序自动化和敏捷开发工作流。

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>在 Azure App Service 中 web 应用

为 Fix It 应用程序使用的云环境是 azure 的我们调用网站服务。 此服务是一种方法，你可以在 Azure 中的将自己 web 应用程序承载而无需创建 Vm 并使其更新、 安装和配置 IIS，等等。我们在我们的虚拟机上托管您的网站，并自动提供备份和恢复以及为你的其他服务。 网站服务适用于 ASP.NET、 Node.js、 PHP 和 Python。 这样可以非常快速地使用 Visual Studio、 Webdeploy、 FTP、 Git 或 TFS 进行部署。 它通常是启动部署的时间与 Internet 上可用的更新后的时间之间只需几秒。 它是所有可用，若要开始，并根据流量的增长可以向上扩展。

在后台，在 Azure App Service Web Apps 提供了大量的体系结构组件和您将不得不自己生成，如果你已将托管你自己的 Vm 上使用 IIS 网站的功能。 一个组件是自动配置 IIS 并且在尽可能多的 Vm，当您想要在上运行你的站点上安装你的应用程序的部署结束点。

![部署服务](introduction/_static/image5.png)

当用户点击 web 站点时，它们不直接命中 IIS Vm，途经[应用程序请求路由 (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing)负载平衡器。 你可以使用其与你自己的服务器，但其优点在于，它们为你自动设置。 它们使用智能启发式方法，将考虑因素： 会话相关性，在 IIS 中，队列深度和上每个 CPU 使用率计算机直接流量发送到虚拟机承载您的网站。

![ARR 负载平衡器](introduction/_static/image6.png)

如果一台计算机出现故障，Azure 自动从轮转中提取、 新的 VM 实例，旋转和开始将流量定向到新的实例，所有与你的应用程序无需停机。

![从计算机发生故障的自动恢复](introduction/_static/image7.png)

所有这些自动发生。 你需要做是创建网站和应用程序部署到它，使用 Windows PowerShell、 Visual Studio 中或 Azure 管理门户。

快速而简单分步教程演示如何在 Visual Studio 中创建 web 应用程序并将其部署到 Azure 网站，请参阅[Azure 和 ASP.NET 入门](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)。

<a id="summary"></a>
## <a name="summary"></a>摘要

本简介已提供一个列表的主题将介绍书，屏幕快照的示例应用程序，并在 Azure App Service 云环境中的 Web 应用的简要概述。 开发应用和云环境中的最大好处之一是很容易地自动执行重复开发任务，如创建测试环境和部署代码到它。 如何执行该操作的使用者[下一章](automate-everything.md)。

## <a name="resources"></a>资源

在本章中的主题有关的详细信息，请参阅以下资源。

文档：

- [Web 应用在 Azure App Service 中的](https://azure.microsoft.com/services/app-service/web/)。 有关 Azure Web Apps 文档的门户页。
- [Web Apps、 云服务和 Vm： 何时使用何？](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) 仅可以在 Azure 中运行 web 应用程序的三种方式之一 WAWS 这一章中所示。 本文介绍的三种方式的区别，并提供有关如何选择哪一个最适合你的方案的指南。 如网站、 云服务是 Azure PaaS 功能。 Vm 是 IaaS 功能。 PaaS 和 IaaS 的说明，请参阅[数据选项](data-storage-options.md#paasiaas)章。

视频：

- [Scott Guthrie 开始步骤 0-什么是 Azure 云操作系统？](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [网站体系结构-使用 Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/)。
- [Azure 网站与 Nir Mashkowski 的内部结构](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski)。

>[!div class="step-by-step"]
[下一篇](automate-everything.md)
