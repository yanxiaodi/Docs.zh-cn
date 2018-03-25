---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: "持续集成和持续交付 （使用 Azure 构建真实世界云应用） |Microsoft 文档"
author: MikeWasson
description: "构建真实世界云应用程序与 Azure 的电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 4a5433a7dd70e27b59163822ba427b026c3f4ce0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>持续集成和持续交付 （使用 Azure 构建真实世界云应用）
====================
通过[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下载修复此错误项目](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下载电子书](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **构建真实世界云应用程序与 Azure**电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，从而帮助你为成功开发适用于云中的 web 应用。 有关电子书的信息，请参阅[第一章](introduction.md)。


前两个建议开发过程模式已[使一切自动化](automate-everything.md)和[源代码管理](source-control.md)，第三个过程模式将它们合并。 持续集成 (CI) 意味着只要开发人员将签入到源代码存储库的代码，将自动触发生成。 持续交付 (CD) 采用此一次性进一步： 在生成并自动的单元测试均可成功完成后，自动部署到环境可以执行更多深入测试应用程序。

云中可用于维护测试环境，因为你只为环境资源付费，只要你使用它们的成本降到最低。 CD 过程可以设置测试环境，你需要它，并完成后，可以使环境下测试。

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>持续集成和持续交付工作流

通常，我们建议你到你的开发和过渡环境的持续交付。 大多数团队，甚至是在 Microsoft，需要手动审查和批准流程的生产部署。 对于生产部署，你可能想要确保其发生的开发团队的关键人员以获得支持，或在低流量期间可用时。 但没有设置为验收测试的执行任何操作来阻止您完全自动执行你的开发和测试环境，以便所有开发人员都只需为签入更改和环境。

下图出自[Microsoft 模式和实践电子书有关持续交付](http://aka.ms/ReleasePipeline)说明了典型的工作流。 单击图像以查看它在其原始上下文中的完整大小。

[![持续交付工作流](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>云经济高效的 CI 和 CD 的启用

在 Azure 中的这些流程的自动化很容易。 因为你在云中运行的所有内容，你无需购买或管理服务器的生成或测试环境。 和无需等待可执行你在测试服务器。 对于每次你执行的生成，您无法启动了使用你的自动化脚本，运行的验收测试或更多深入测试反对这样做，在 Azure 中的测试环境，然后完成后只需即将其拆散。 并且，如果您仅为 2 小时或 8 小时一次或一天运行该服务器，你必须为其付费的钱的总额最小，因为即可计算机实际运行的一次只支付。 例如，环境所需的修补程序应用程序它基本上成本大约 1 美分每小时，如果你转到一个层上从可用的级别。 在每月的过程中，如果一次只运行一小时环境你的测试环境所可能成本在星巴克购买拿小于。

## <a name="visual-studio-team-services-vsts"></a>Visual Studio Team Services (VSTS)

VSTS 提供了大量功能以帮助你从规划到部署的应用程序开发。

- 它支持 （所有分布） 的 Git 和 TFVC （集中式） 源控件。
- 它提供弹性生成服务，这意味着它会动态创建生成服务器，在需要时并关闭完成时采用的方式。 你可以自动启动生成当某人将签入源的代码更改，并且无需具有分配并为你自己的生成服务器，以为空闲大部分时间付费。 生成服务是免费的只要不超过一定数量的生成。 如果你想执行大量的生成，可以将很少的额外支付保留的生成服务器。
- 它支持向 Azure 持续交付。
- 它支持自动的负载测试。 负载测试对云应用程序至关重要，但直到太迟通常被忽略。 负载测试模拟由数以千计的用户，使你能够找出瓶颈并提高吞吐量的应用程序大量使用-在发布到生产应用之前。
- 它支持团队聊天室协作，该工具可帮助实时通信和对于小的敏捷团队协作。
- 它支持 agile 项目管理。


持续集成和 VSTS 传递功能的详细信息，请参阅[Visual Studio Team Services](https://www.visualstudio.com/team-services/)。

如果你正在寻找统项目管理，团队协作和源代码管理解决方案，将签出 VSTS。 该服务是免费的最多 5 个用户，并可以为其在注册[Visual Studio Team Services](https://www.visualstudio.com/team-services/)。

## <a name="summary"></a>摘要

有关如何实现使用低的周期时间可重复、 可靠且可预测的开发过程已被第三个云开发模式。 在[下一章](web-development-best-practices.md)我们开始寻找体系结构和编码模式。

## <a name="resources"></a>资源

有关详细信息，请参阅[部署在 Azure App Service web 应用](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)。

另请参阅以下资源：

- [生成使用 Team Foundation Server 2012 的发行管道](http://aka.ms/ReleasePipeline)。 电子书，动手实验和由 Microsoft 模式和实践，示例代码提供对持续交付的深入介绍。 介绍使用 Visual Studio 实验室管理工具版和 Visual Studio Release Management。
- [ALM Rangers DevOps 工具和指南](https://aka.ms/vsarsolutions/)。 ALM Rangers 引入的 DevOps Workbench 示例助理解决方案和模式的协作的实践性指南&amp;实践书籍*生成与 TFS 2012 发行管道*，作为启动的好方法学习 DevOps 的概念&amp;Release Management 适用于 TFS 2012 和以便启动试。 本指南演示如何生成一次并部署到多个环境。
- [使用 Visual Studio 2012 对持续交付进行测试](https://msdn.microsoft.com/library/jj159345.aspx)。 Microsoft 模式与实践中的电子书说明如何将自动测试使用连续交付进行集成。
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). 工具，用于捕获 （基于一个标签） 的 TFS 的生成、 生成、 将其打包、 允许有人在要配置的特定方面的 DevOps 角色中和将其推送到 Azure 的源代码。 该工具要使"回滚"到以前部署的版本的操作跟踪部署过程。 该工具没有外部依赖关系，并且可以运行独立使用 TFS Api 和 Azure SDK。
- [持续交付： 通过生成、 测试和部署自动化可靠软件释放](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361)。 通过 Jez 普通的书籍。
- [发布 ！设计和部署生产就绪软件](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213)。 由 Michael t。 Nygard 的书籍。

>[!div class="step-by-step"]
[上一页](source-control.md)
[下一页](web-development-best-practices.md)
