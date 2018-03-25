---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: "选择 Web 部署到适当的方法 |Microsoft 文档"
author: jrjlee
description: "当使用 Internet 信息服务 (IIS) Web 部署工具 （Web 部署） 2.0 或更高版本时，有三种主要方法可用于获取..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: b77aa37160f3822f58908866e44497aea3d3bdc8
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="choosing-the-right-approach-to-web-deployment"></a>选择 Web 部署到适当的方法
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 当使用 Internet 信息服务 (IIS) Web 部署工具 （Web 部署） 2.0 或更高版本时，有三种主要方法可用于获取打包的 web 应用程序到 web 服务器上。 你可以：
> 
> - 通过以目标部署应用程序从远程位置*Web 部署代理服务*（也称为"远程代理"） 目标服务器上。
> - 部署应用程序从远程位置使用 Web 部署按需 （也称为"临时代理"）。
> - 通过以目标部署应用程序从远程位置*IIS Web 部署处理程序*目标服务器上。
> - 手动复制到目标服务器的 web 包并导入通过 IIS 管理器中部署应用程序。
> 
> 如何配置你的目标 web 服务器将取决于你想要使用哪种部署的方法。 本主题将帮助您决定哪种部署的方法最适合你。


下表显示的主要优点和缺点的每个部署方法，以及通常最适合每种方法的方案。

| 方法 | 优点 | 缺点 | 典型的方案 |
| --- | --- | --- | --- |
| 远程代理 | 很容易设置。 它是适用于 web 应用程序和内容的定期更新。 | 用户必须是目标服务器上的管理员。 用户不能提供备用凭据。 | 开发环境。 测试环境。 |
| 临时代理 | 没有无需在目标计算机上安装 Web 部署。 Web 部署的最新版本会自动使用。 | 用户必须是目标服务器上的管理员。 用户不能提供备用凭据。 | 开发环境。 测试环境。 |
| Web 部署处理程序 | 非管理员用户可以将内容部署。 它是适用于 web 应用程序和内容的定期更新。 | 它是更加复杂，无法设置。 | 过渡环境。 Intranet 生产环境。 托管的环境中。 |
| 脱机部署 | 它是很容易设置。 它是适用于独立的环境。 | 服务器管理员必须手动复制并导入的 web 包每次。 | 面向 Internet 的生产环境。 隔离的网络环境中。 |
  

## <a name="using-the-remote-agent"></a>使用远程代理

在安装 Web 部署使用默认设置在目标服务器上时，Web 部署代理服务 （"远程代理"） 将自动安装和启动。 默认情况下，远程代理公开在此地址的 HTTP 终结点：


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> 你可以替换 [*服务器*] 与 web 服务器的计算机名称，你的 web 服务器或主机名的 IP 地址解析为你的 web 服务器。


服务器管理员可以通过指定此终结点地址来部署 web 包从远程位置，如开发人员计算机或生成服务器。 例如，假设在 Fabrikam，Inc.的 Matt 婷已在其开发人员计算机上生成 ContactManager.Mvc web 应用程序项目。 生成过程生成了 web 包，连同*。 deploy.cmd*安装包所需的文件，其中包含 Web 部署命令。 如果 Matt TESTWEB1 服务器上的服务器管理员，他可以通过在其开发人员计算机上运行此命令来部署 web 应用程序与测试 web 服务器：


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


在实际事实中，Web 部署的可执行文件可以推导出的终结点地址的远程代理，如果你提供计算机名称，因此 Matt 只需键入：


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> 有关 Web 部署命令行语法的详细信息和*。 deploy.cmd*文件，请参阅[如何： 安装部署包 Using deploy.cmd 文件](https://msdn.microsoft.com/library/ff356104.aspx)。


远程代理提供了一个简单的方法，将从远程位置的内容部署，这种方法可以很好地配合一键式或自动部署。 但是，运行的部署命令的用户还必须是域管理员或目标服务器上的本地管理员组的成员。 此外，远程代理不支持基本身份验证，所以不能传递命令行上的备用凭据。

远程代理会提供到开发或测试方案，其中它并不常见的开发人员能够完全权限管理员控制测试服务器环境中，并且通常重新生成并重新非常部署的应用程序的部署很有用的方法频繁。 但是，这种方法是通常不太适用于过渡环境或生产环境。

有关使用远程代理方法的方案的端到端示例，请参阅[方案： 配置 Web 部署的测试环境](scenario-configuring-a-test-environment-for-web-deployment.md)。

## <a name="using-the-temp-agent"></a>使用临时代理

部署的临时代理方法是类似于远程代理方法。 但是，与远程代理方法中，你不需要在目标 web 服务器上安装 Web 部署。 相反，当执行部署时，Web 部署将在目标服务器上安装 web 部署代理服务的临时版本和将使用它来将你的内容部署到 IIS。 部署完成后，删除所有临时文件。

如果你想要使用临时代理提供程序设置中，添加**/g**标志设为你部署的命令：


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> 即使服务未运行，不能如果在目标计算机上安装 web 部署代理服务使用临时代理。


此方法的优点是，你不需要维护的 Web 部署目标服务器上的安装。 此外，你不需要确保源和目标计算机都运行相同版本的 Web 部署。 但是，此方法会受到与远程代理方法时，相同的主体限制即，你必须在目标服务器上的本地管理员才能部署内容，并且支持仅 NTLM 身份验证。 临时代理方法还要求目标环境的大量的初始的配置。

使用临时代理的详细信息，请参阅[如何： 安装部署包 Using deploy.cmd 文件](https://msdn.microsoft.com/library/ff356104.aspx)和[按需部署的 Web](https://technet.microsoft.com/library/ee517345(WS.10).aspx)。

## <a name="using-the-web-deploy-handler"></a>使用 Web 部署处理程序

对于 IIS 7 及以上版本，Web 部署提供通过 IIS Web 部署处理程序的另一种部署方法。 与 IIS Web 管理服务 (WMSvc)，它旨在允许用户从远程位置管理 IIS 网站紧密地集成 Web 部署处理程序。

默认情况下，远程代理公开在此地址的 HTTP 终结点：


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> 你可以替换 [*服务器*] 与 web 服务器的计算机名称，你的 web 服务器或主机名的 IP 地址解析为你的 web 服务器。


Web 部署处理程序通过远程代理、 和临时代理，其最大优点是，你可以配置 IIS 以允许非管理员用户部署到特定的 IIS 网站的应用程序和内容。 Web 部署处理程序还支持基本身份验证，因此你也可以在 Web 部署命令作为参数提供备用凭据。 主要缺点是，Web 部署处理程序并最初设置和配置大量更复杂。

对于非管理员用户，Web 管理服务 (WMSvc) 将仅允许用户连接到 IIS 使用的站点级连接，而不是服务器级连接。 若要访问特定站点，您可以在终结点地址包括特定于站点的查询字符串：


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


例如，假设生成过程配置为自动部署到过渡环境的 web 应用程序每次成功生成之后。 如果使用远程代理方法时，你将需要在目标服务器上进行生成进程标识管理员。 与此相反，使用 Web 部署处理程序方法你可以为提供的非管理员用户 （&） #x 2014;**FABRIKAM\stagingdeployer**在此用例和 #x 2014; 对仅，一个特定的 IIS 网站和生成过程的权限可以提供这些凭据以部署 web 包。


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> 有关 Web 部署命令行操作和语法的详细信息，请参阅[Web 部署命令行参考](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx)。 有关详细信息使用*。 deploy.cmd*文件，请参阅[如何： 安装部署包 Using deploy.cmd 文件](https://msdn.microsoft.com/library/ff356104.aspx)。


Web 部署处理程序提供到过渡环境、 托管的环境中，和基于 intranet 的生产环境中，远程访问服务器可用但不是管理员凭据的部署很有用的方法。

有关使用 Web 部署处理程序方法的方案的端到端示例，请参阅[方案： 配置 Web 部署的过渡环境](scenario-configuring-a-staging-environment-for-web-deployment.md)。

## <a name="using-offline-deployment"></a>使用脱机部署

在某些情况下，不可能或可行从远程位置将应用程序和内容部署到 IIS 网站。 例如，在源和目标计算机可能在隔离的网络或网络段，或者防火墙策略可能不允许远程访问。

在类似这样的情况下，你可以仍使用打包和发布功能的 Web 部署;你只需不能使用它们从远程位置。 相反，目标服务器上的管理员必须复制到服务器上的 web 包，并将其导入通过 IIS 管理器。

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

脱机的部署方法通常是很有用在面向 Internet 的生产环境中，其中外围网络中的服务器可能与内部网络中的计算机的连接受到限制。

有关使用脱机的部署方法的方案的端到端示例，请参阅[方案： 配置 Web 部署的生产环境](scenario-configuring-a-production-environment-for-web-deployment.md)。

## <a name="further-reading"></a>其他阅读材料

有关 Web 部署命令行操作和语法的详细信息，请参阅[Web 部署命令行参考](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx)。 有关详细信息使用*。 deploy.cmd*文件，请参阅[如何： 安装部署包 Using deploy.cmd 文件](https://msdn.microsoft.com/library/ff356104.aspx)。

你可以在其中部署 web 包从远程计算机的不同方法的更多常规指南，请参阅[使用 Web 部署远程](https://technet.microsoft.com/library/ee461175(WS.10).aspx)。 有关使用 Web 按需部署的详细信息，请参阅[按需部署的 Web](https://technet.microsoft.com/library/ee517345(WS.10).aspx)。

>[!div class="step-by-step"]
[上一页](configuring-server-environments-for-web-deployment.md)
[下一页](scenario-configuring-a-test-environment-for-web-deployment.md)
