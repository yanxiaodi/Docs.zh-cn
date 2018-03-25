---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
title: 配置 Web 服务器的 Web 部署发布 （脱机部署） |Microsoft 文档
author: jrjlee
description: 本主题介绍如何配置 IIS web 服务器以支持脱机 web 发布和部署。 当你处理的 Internet Information Services (我...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ba92788f-9f03-44b1-b6b2-af8413e6a35d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
msc.type: authoredcontent
ms.openlocfilehash: dfd3ab41e44a3b000bf2c25a5a71db4344617bf2
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="configuring-a-web-server-for-web-deploy-publishing-offline-deployment"></a>配置 Web 服务器的 Web 部署发布 （脱机部署）
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何配置 IIS web 服务器以支持脱机 web 发布和部署。
> 
> 当使用 Internet 信息服务 (IIS) Web 部署工具 （Web 部署） 2.0 或更高版本时，有三种主要方法可用于获取应用程序或 web 服务器上的站点。 你可以：
> 
> - 使用*Web 部署远程代理服务*。 此方法需要较少配置 web 服务器，但你需要提供本地服务器管理员的凭据，以便将任何内容部署到服务器。
> - 使用*Web 部署处理程序*。 此方法更加复杂，并且需要更多的初始工作来设置 web 服务器。 但是，当你使用此方法时，你可以配置 IIS 以允许非管理员用户执行部署。 Web 部署处理程序只是在 IIS 7 或更高版本中可用。
> - 使用*脱机部署*。 此方法需要 web 服务器，最小配置，但服务器管理员必须手动复制到服务器上的 web 包并将其导入通过 IIS 管理器。
> 
> 主要功能的优点，这些方法的缺点的详细信息，请参阅[选择用于 Web 部署的右方法](choosing-the-right-approach-to-web-deployment.md)。


是，如果你的网络基础结构或安全限制会阻止远程部署。 这是最有可能是这种情况在 web 服务器是独立的面向 Internet 的生产环境中和 #x 2014; 请以物理方式或通过防火墙和子网和 #x 2014年; 从服务器基础结构的其余部分。

显然，此方法变得不够理想，如果定期更新 web 应用程序。 如果你的基础结构允许，你可能想要考虑启用远程部署，使用 Web 部署处理程序或 Web 部署远程代理服务。

## <a name="task-overview"></a>任务概述

若要配置 web 服务器以支持脱机导入和部署 web 包，你将需要：

- 安装 IIS 7.5 和 IIS 7 的建议的配置。
- 安装 Web 部署 2.1 或更高版本。
- 创建一个 IIS 网站来承载部署的内容。
- 禁用 Web 部署代理服务。

若要专门承载的示例解决方案，你将还需要：

- 安装.NET Framework 4.0。
- 安装 ASP.NET MVC 3。

本主题将演示如何执行每个这些过程。 任务和本主题中的演练假定你刚开始使用干净服务器生成运行 Windows Server 2008 R2。 在继续之前，确保：

- 安装 Windows Server 2008 R2 Service Pack 1 和所有可用的更新。
- 服务器已加入域。
- 服务器具有静态 IP 地址。

> [!NOTE]
> 有关将计算机加入到域的详细信息，请参阅[将计算机加入到域并登录](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)。 有关配置静态 IP 地址的详细信息，请参阅[配置静态 IP 地址](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)。


## <a name="install-products-and-components"></a>安装产品和组件

本部分将指导你完成 web 服务器上安装必需的产品和组件。 在开始之前，很好的做法是运行 Windows 更新，以确保你的服务器是完全最新。

在这种情况下，你需要安装以下操作：

- **IIS 7 建议的配置**。 这使**Web 服务器 (IIS)** web 服务器上的角色和安装的 IIS 模块和托管 ASP.NET 应用程序所需的组件组。
- **.NET framework 4.0**。 这是运行在此版本的.NET Framework 生成的应用程序所需要的。
- **Web 部署工具 2.1 或更高版本**。 这在你的服务器上安装 Web 部署 （和其基础可执行文件，MSDeploy.exe）。 Web 部署与 IIS 集成，并允许您导入和导出 web 包。
- **ASP.NET MVC 3**。 这将安装你需要运行 MVC 3 应用程序的程序集。

> [!NOTE]
> 本演练介绍了利用 Web 平台安装程序来安装和配置各种组件。 尽管你无需使用 Web 平台安装程序，它，从而简化安装过程自动检测依赖关系并确保你始终获得最新的产品版本。 有关详细信息，请参阅[Microsoft Web 平台安装程序 3.0](https://go.microsoft.com/?linkid=9805118)。


**若要安装必需的产品和组件**

1. 下载并安装[Web 平台安装程序](https://go.microsoft.com/?linkid=9805118)。
2. 安装完成后，Web 平台安装程序将自动启动。

    > [!NOTE]
    > 你现在可以随时从启动 Web 平台安装程序**启动**菜单。 若要执行此操作，在**启动**菜单上，单击**所有程序**，然后单击**Microsoft Web 平台安装程序**。
3. 在顶部**Web Platform Installer 3.0**窗口中，单击**产品**。
4. 在左侧的窗口中，在导航窗格中，单击**框架**。
5. 在**Microsoft.NET Framework 4**行，如果尚未安装.NET Framework，请单击**添加**。

    > [!NOTE]
    > 你可能已安装.NET Framework 4.0 通过 Windows 更新。 如果已安装的产品或组件，则 Web 平台安装程序将指示这一点通过将**添加**按钮，其文本**已安装**。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image1.png)
6. 在**ASP.NET MVC 3 (Visual Studio 2010)**行中，单击**添加**。
7. 在导航窗格中，单击**服务器**。
8. 在**IIS 7 建议配置**行中，单击**添加**。
9. 在**Web 部署工具 2.1**行中，单击**添加**。
10. 单击“安装” 。 Web 平台安装程序将显示列表的产品和 #x 2014年; 以及要安装任何关联的依赖关系 （&） #x 2014; 并将提示你接受许可条款。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image2.png)
11. 查看许可条款，然后如果同意条款，单击**我接受**。
12. 安装完成后，单击**完成**，然后关闭**Web Platform Installer 3.0**窗口。

如果你安装.NET Framework 4.0 安装 IIS 之前，你将需要运行[ASP.NET IIS 注册工具](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx)(aspnet\_regiis.exe) 以向 IIS 注册 ASP.NET 的最新版本。 如果不这样做，你将找到 IIS 将 （如 HTML 文件） 中提供静态内容而无需任何问题，但它将返回**HTTP 错误 404.0-未找到**当你尝试浏览到 ASP.NET 内容。 下一步过程可用于确保注册 ASP.NET 4.0。

**若要向 IIS 注册 ASP.NET 4.0**

1. 单击**启动**，然后键入**命令提示符**。
2. 在搜索结果中，右键单击**命令提示符**，然后单击**以管理员身份运行**。
3. 在命令提示符窗口中，导航到**%WINDIR%\Microsoft.NET\Framework\v4.0.30319**目录。
4. 键入以下命令，然后按 Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample1.cmd)]
5. 如果你计划到托管 64 位 web 应用程序的任何时刻，你应该向 IIS 注册 ASP.NET 的 64 位版本。 为此，请在命令提示符窗口中，导航到**%WINDIR%\Microsoft.NET\Framework64\v4.0.30319**目录。
6. 键入以下命令，然后按 Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample2.cmd)]

作为一种良好做法，Windows Update 再次使用此时若要下载并安装的新产品和组件已安装所有可用更新。

## <a name="configure-the-iis-website"></a>配置 IIS 网站

可以将 web 内容部署到你的服务器之前，你需要创建和配置 IIS 网站来承载的内容。 Web 部署可以仅将 web 包部署到现有 IIS 网站;它不能为你创建的网站。 在高级别中，你将需要完成这些任务：

- 在要承载内容的文件系统上创建一个文件夹。
- 创建 IIS 网站来提供的内容，并将其与本地文件夹关联。
- 授予读取权限的本地文件夹上的应用程序池标识。

尽管没有执行任何操作停止您从将内容部署到 IIS 中的默认网站，但不建议使用此方法对于测试或演示方案以外的任何内容。 若要模拟生产环境，应使用特定于你的应用程序的要求的设置创建新的 IIS 网站。

**若要创建和配置 IIS 网站**

1. 在本地文件系统上，创建一个文件夹来存储你的内容 (例如， **C:\DemoSite**)。
2. 上**启动**菜单上，指向**管理工具**，然后单击**Internet Information Services (IIS) Manager**。
3. 在 IIS 管理器中，在**连接**窗格中，展开服务器节点 (例如， **PROWEB1**)。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image3.png)
4. 右键单击**站点**节点，，然后单击**添加网站**。
5. 在**站点名称**框中，键入为 IIS 网站的名称 (例如， **DemoSite**)。
6. 在**物理路径**框中，键入 （或浏览到） 您的本地文件夹的路径 (例如， **C:\DemoSite**)。
7. 在**端口**框中，键入你要在其托管网站的端口号 (例如， **85**)。

    > [!NOTE]
    > 标准端口号为 80 用于 HTTP 和 HTTPS 的 443。 但是，如果承载此网站在端口 80 上的，你将需要停止默认网站，然后才能访问你的站点。
8. 保留**主机名**框保留为空，除非你想要配置该网站的域名系统 (DNS) 记录，然后单击**确定**。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image4.png)

    > [!NOTE]
    > 在生产环境中，你可能需要托管你的网站在端口 80 上并配置主机标头，以及匹配的 DNS 记录。 在 IIS 7 中配置主机标头的详细信息，请参阅[网站 (IIS 7) 配置主机头](https://technet.microsoft.com/library/cc753195(WS.10).aspx)。 Windows Server 2008 R2 中的 DNS 服务器角色的详细信息，请参阅[DNS 服务器概述](https://technet.microsoft.com/en-gb/library/cc770392.aspx)和[DNS 服务器](https://technet.microsoft.com/windowsserver/dd448607)。
9. 在**操作**窗格中，在**编辑站点**，单击**绑定**。
10. 在**站点绑定**对话框中，单击**添加**。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image5.png)
11. 在**添加网站绑定**对话框中，设置**IP 地址**和**端口**以与你现有的站点配置匹配。
12. 在**主机名**框中，键入你的 web 服务器的名称 (例如， **PROWEB1**)，然后单击**确定**。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image6.png)

    > [!NOTE]
    > 第一个站点绑定允许你访问本地使用的 IP 地址和端口的网站或`http://localhost:85`。 第二个站点绑定，可从其他计算机上使用计算机名称 (例如，http://proweb1:85) 的域中访问站点。
13. 在**站点绑定**对话框中，单击**关闭**。
14. 在**连接**窗格中，单击**应用程序池**。
15. 在**应用程序池**窗格中，右键单击应用程序池的名称，然后单击**基本设置**。 默认情况下，应用程序池的名称将匹配你的网站的名称 (例如， **DemoSite**)。
16. 在**.NET Framework 版本**列表中，选择**.NET Framework v4.0.30319**，然后单击**确定**。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image7.png)

    > [!NOTE]
    > 示例解决方案需要.NET Framework 4.0。 这是不要求用于 Web 部署一般情况下。

为了使你的网站以提供内容，应用程序池标识必须具有读取权限的本地文件夹，将内容存储。 在 IIS 7.5 中应用程序池具有唯一的应用程序池标识默认运行 （与以前版本的 IIS，其中应用程序池将通常使用的网络服务帐户运行）。 应用程序池标识不是真实的用户帐户和未显示于的用户或组和 #x 2014年任何列表; 相反，它动态启动时创建的应用程序池。 每个应用程序池标识添加到本地**IIS\_IUSRS**作为隐藏的项的安全组。

若要授予对应用程序池标识的文件或文件夹上的权限，你有两个选项：

- 将权限分配给应用程序池标识直接，使用格式 **IIS 应用程序池\***[应用程序池名称]*(例如， **IIS AppPool\DemoSite**)。
- 向其分配权限**IIS\_IUSRS**组。

最常用的方法是将权限分配给本地**IIS\_IUSRS**组，因为这种方法可让你更改应用程序池，而不用重新配置文件系统权限。 下一个过程中使用此基于组的方法。

> [!NOTE]
> IIS 7.5 中的应用程序池标识的详细信息，请参阅[应用程序池标识](https://go.microsoft.com/?linkid=9805123)。


**若要配置 IIS 网站的文件夹权限**

1. 在 Windows 资源管理器，浏览到你的本地文件夹的位置。
2. 右键单击文件夹，并依次**属性**。
3. 上**安全**选项卡上，单击**编辑**，然后单击**添加**。
4. 单击**位置**。 在**位置**对话框中，选择本地服务器，然后单击**确定**。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image8.png)
5. 在**选择用户或组**对话框中，键入**IIS\_IUSRS**，单击**检查名称**，然后单击**确定**。
6. 在 **权限***[文件夹名称]* 对话框中，请注意，在分配新组 **读取 &amp; 执行**， **列出文件夹内容**， 和 **读取** 默认情况下的权限。 保留此保持不变，然后单击**确定**。
7. 单击 **确定** 关闭 *[文件夹名称]***属性** 对话框。

## <a name="disable-the-remote-agent-service"></a>禁用了远程代理服务

在安装 Web 部署时，Web 部署代理服务安装，并自动启动。 此服务允许你部署和发布远程位置中的 web 包。 你将不使用远程部署功能在此方案中，因此应停止并禁用服务。

> [!NOTE]
> 你不需要停止为了导入和手动部署 web 包的远程代理服务。 但是，最好先停止并禁用该服务，如果你不打算使用它。


你可以停止和禁用某项服务采用多种方式，使用各种命令行实用程序或 Windows PowerShell cmdlet。 本过程描述了简单的基于 UI 的方法。

**若要停止并禁用了远程代理服务**

1. 在“开始”  菜单上，指向“管理工具” ，然后单击“服务” 。
2. 在服务控制台中，找到**Web 部署代理服务**行。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image9.png)
3. 右键单击**Web 部署代理服务**，然后单击**属性**。
4. 在**Web 部署代理服务属性**对话框中，单击**停止**。
5. 在**启动类型**列表中，选择**禁用**，然后单击**确定**。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image10.png)

## <a name="conclusion"></a>结束语

此时，你的 web 服务器可供脱机 web 包部署。 在尝试导入到 IIS 网站的 web 包之前，你可能想要检查这些要点：

- 已向 IIS 注册 ASP.NET 4.0？
- 应用程序池标识没有读取访问权限的源文件夹的你的网站？
- 已停止 Web 部署代理服务？

>[!div class="step-by-step"]
[上一页](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
[下一页](configuring-a-database-server-for-web-deploy-publishing.md)
