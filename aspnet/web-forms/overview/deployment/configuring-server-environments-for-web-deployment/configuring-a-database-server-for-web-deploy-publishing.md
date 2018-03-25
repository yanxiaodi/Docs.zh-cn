---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: 数据库服务器配置 web 部署发布 |Microsoft 文档
author: jrjlee
description: 本主题介绍如何配置 SQL Server 2008 R2 数据库服务器以支持 web 部署和发布。 本主题中所述的任务都 co...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: 98fd728f48f6fb64a61686bc58824b9fb3a28b13
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="configuring-a-database-server-for-web-deploy-publishing"></a>Web 部署发布更改为配置数据库服务器
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何配置 SQL Server 2008 R2 数据库服务器以支持 web 部署和发布。
> 
> 本主题中所述的任务共有的每个部署方案和 #x 2014年; 你的 web 服务器配置为使用 IIS Web 部署工具 （Web 部署） 的远程代理服务、 Web 部署处理程序或脱机的部署是否并不重要或应用程序在单个 web 服务器或服务器场上运行。 部署数据库的方式可能更改根据安全要求和其他注意事项。 例如，你可能部署数据库或不包含示例数据，并可能部署用户角色映射，或在部署后手动配置。 但是，配置数据库服务器的方式保持不变。


你无需安装任何其他产品或工具，配置要支持 web 部署的数据库服务器。 假设你的数据库服务器和 web 服务器运行在不同计算机上，你只需要：

- 允许 SQL Server 以使用 TCP/IP 进行通信。
- 允许 SQL Server 流量通过任何防火墙。
- 为 web 服务器计算机帐户提供的 SQL Server 登录名。
- 计算机帐户登录名映射到任何所需的数据库角色。
- 提供将运行部署 SQL Server 登录名和数据库创建者权限的帐户。
- 若要支持重复的部署，将部署帐户登录名映射到**db\_所有者**数据库角色。

本主题将演示如何执行每个这些过程。 任务和本主题中的演练假定你刚开始使用 Windows Server 2008 R2 上运行的 SQL Server 2008 R2 的默认实例。 在继续之前，确保：

- 安装 Windows Server 2008 R2 Service Pack 1 和所有可用的更新。
- 服务器已加入域。
- 服务器具有静态 IP 地址。
- 安装 SQL Server 2008 R2 Service Pack 1 和所有可用的更新。

SQL Server 实例仅需要包括**数据库引擎服务**角色，它自动包含在任何 SQL Server 安装。 但是，为了便于配置和维护，我们建议你包括**管理工具-基本**和**管理工具 – 完整**服务器角色。

> [!NOTE]
> 有关将计算机加入到域的详细信息，请参阅[将计算机加入到域并登录](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)。 有关配置静态 IP 地址的详细信息，请参阅[配置静态 IP 地址](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)。 有关安装 SQL Server 的详细信息，请参阅[安装 SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx)。


## <a name="enable-remote-access-to-sql-server"></a>启用对 SQL Server 的远程访问

SQL Server 使用 TCP/IP 与远程计算机通信。 如果你的数据库服务器和 web 服务器位于不同的计算机上，你需要：

- 配置 SQL Server 网络设置，以允许通过 TCP/IP 进行通信。
- 将任何硬件或软件的防火墙允许 TCP 流量 （并在某些情况下用户数据报协议 (UDP) 流量） 上的 SQL Server 实例使用的端口的配置。

若要启用 SQL Server 通信通过 TCP/IP，请使用 SQL Server 配置管理器来更改您的 SQL Server 实例的网络配置。

**若要启用 SQL Server 以使用 TCP/IP 进行通信**

1. 上**启动**菜单上，指向**所有程序**，单击**Microsoft SQL Server 2008 R2**，单击**配置工具**，然后单击**SQL Server 配置管理器**。
2. 在树视图窗格中，展开**SQL Server 网络配置**，然后单击**MSSQLSERVER 的协议**。

    > [!NOTE]
    > 如果已安装多个 SQL Server 实例，你将看到 **协议***[实例名称]* 每个实例的项。 你需要配置网络设置基于实例的实例。
3. 在细节窗格中，右键单击**TCP/IP**行，然后依次**启用**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. 在**警告**对话框中，单击**确定**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. 你需要重新启动 MSSQLSERVER 服务，然后新的网络配置将会生效。 从服务控制台中，或从 SQL Server Management Studio，你可以将在命令提示符下执行该操作。 在此过程中，你将使用 SQL Server Management Studio。
6. 关闭 SQL Server 配置管理器。
7. 上**启动**菜单上，指向**所有程序**，单击**Microsoft SQL Server 2008 R2**，然后单击**SQL Server Management Studio**。
8. 在**连接到服务器**对话框中，在**服务器名称**框中，键入数据库服务器的名称，然后单击**连接**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. 在**对象资源管理器**窗格中，右键单击父服务器节点 (例如， **TESTDB1**)，然后单击**重新启动**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. 在**Microsoft SQL Server Management Studio**对话框中，单击**是**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. 在重新启动该服务，已关闭 SQL Server Management Studio。

若要允许 SQL Server 流量通过防火墙，请首先需要知道您的 SQL Server 实例所使用的端口。 这将取决于如何在 SQL Server 实例已在其中创建和配置：

- A*默认实例*的 SQL Server 侦听 （和响应） TCP 端口 1433年上的请求。
- A*命名实例*的 SQL Server 侦听 （和响应） 动态分配的 TCP 端口上的请求。
- 如果启用了 SQL Server Browser 服务，客户端可以查询 UDP 端口 1434 以查找要使用特定的 SQL Server 实例的 TCP 端口上的服务。 但是，出于安全原因通常禁用此服务。

假设你正在使用的 SQL Server 的默认实例，你需要将防火墙配置为允许流量。

| 方向 | 从端口 | 到端口 | 端口类型 |
| --- | --- | --- | --- |
| 入站 | 任意 | 1433 | TCP |
| 出站 | 1433 | 任意 | TCP |
  

> [!NOTE]
> 从技术上讲，客户端计算机将使用 1024年和 5000 之间随机分配的 TCP 端口进行通信使用 SQL Server，并且你可以相应地限制防火墙规则。 SQL Server 端口和防火墙的详细信息，请参阅[to SQL 通过防火墙进行通信所需的 TCP/IP 端口号](https://go.microsoft.com/?linkid=9805125)和[如何： 配置服务器以侦听特定 TCP 端口 （SQL Server 配置管理器）](https://msdn.microsoft.com/library/ms177440.aspx)。


在大多数 Windows Server 环境中，你可能需要在数据库服务器上配置 Windows 防火墙。 默认情况下，Windows 防火墙允许所有出站流量，除非规则专门禁止它。 若要启用你的 web 服务器来访问你的数据库，你需要配置 SQL Server 实例使用的端口号允许 TCP 流量的入站的规则。 如果你正在使用的 SQL Server 的默认实例，可以使用下一个过程来配置此规则。

**若要配置 Windows 防火墙以允许与默认 SQL Server 实例的通信**

1. 在数据库服务器上，在**启动**菜单上，指向**管理工具**，然后单击**高级安全 Windows 防火墙**。
2. 在树视图窗格中，单击**入站规则**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. 在**操作**窗格中，在**入站规则**，单击**新规则**。
4. 在新入站规则向导中，在**规则类型**页上，选择**端口**，然后单击**下一步**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. 上**协议和端口**页上，确保**TCP**选择，然后在**特定本地端口**框中，键入**1433年**，然后单击**下一步**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. 上**操作**页上，保留**允许连接**选择和单击**下一步**。
7. 上**配置文件**页上，保留**域**选择，清除**私有**和**公共**复选框，并依次**下一步**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. 上**名称**页上，为规则提供适当描述性名称 (例如， **SQL Server 默认实例 – 网络访问**)，然后单击**完成**。

有关详细信息对于 SQL Server，配置 Windows 防火墙，尤其是当你需要通过非标准或动态端口与 SQL Server 进行通信，请参阅[如何： 为数据库引擎访问配置 Windows 防火墙](https://technet.microsoft.com/library/ms175043.aspx)。

## <a name="configure-logins-and-database-permissions"></a>配置登录名和数据库权限

在部署 web 应用程序到 Internet 信息服务 (IIS) 时，应用程序使用的应用程序池标识运行。 在域环境中，应用程序池标识使用在其上运行来访问网络资源的服务器的计算机帐户。 计算机帐户采用格式 *[域名]***\***[计算机名称]***$**和#x2014; 例如， **FABRIKAM\TESTWEB1$**。 若要允许跨网络访问的数据库将 web 应用程序，你需要：

- 将 web 服务器计算机帐户的登录名添加到 SQL Server 实例。
- 计算机帐户登录名映射到任何所需的数据库角色 (通常**db\_datareader**和**db\_datawriter**)。

如果在服务器场中，而不是一台服务器上运行 web 应用程序，你将需要重复执行这些过程，以服务器场中每个 web 服务器。

> [!NOTE]
> 应用程序池标识和访问网络资源的详细信息，请参阅[应用程序池标识](https://go.microsoft.com/?linkid=9805123)。


您可以达到这些任务以各种方式。 若要创建登录名，您可以：

- 使用 TRANSACT-SQL 或 SQL Server Management Studio 在数据库服务器上手动创建登录名。
- 使用 SQL Server 2008 服务器项目在 Visual Studio 中创建和部署该登录名。

SQL Server 登录名是一个服务器级对象，而不只是数据库级别对象，因此不依赖于你想要部署的数据库。 在这种情况下，你可以在任何时刻，创建登录名和最简单的方法通常是登录名之前手动创建数据库服务器上开始部署数据库。 下一步过程可用于在 SQL Server Management Studio 中创建一个登录名。

**创建 web 服务器计算机帐户的 SQL Server 登录名**

1. 在数据库服务器上，在**启动**菜单上，指向**所有程序**，单击**Microsoft SQL Server 2008 R2**，然后单击**SQL Server Management Studio**.
2. 在**连接到服务器**对话框中，在**服务器名称**框中，键入数据库服务器的名称，然后单击**连接**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. 在**对象资源管理器**窗格中，右键单击**安全**，指向**新建**，然后单击**登录**。
4. 在**登录名-新建**对话框中，在**登录名**框中，键入你的 web 服务器计算机帐户的名称 (例如， **FABRIKAM\TESTWEB1$**)。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. 单击 **“确定”**。

此时，你的数据库服务器是可供 Web 部署发布。 但是，你将部署任何解决方案将不起作用之前机帐户登录名映射到所需的数据库角色。 将登录名映射到数据库角色需要大量更认为，作为你不能映射角色直到检查完已部署数据库。 若要将计算机帐户登录名映射到所需的数据库角色中，您可以：

- 数据库角色为登录名分配手动之后你已将数据库部署第一次。
- 使用后期部署脚本将数据库角色分配给该登录名。

自动创建登录名和数据库角色映射的详细信息，请参阅[到测试环境中部署数据库角色成员](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)。 或者下, 一步过程可用于手动将计算机帐户登录名映射到所需的数据库角色。 请记住，你无法执行此过程之前*后*已部署数据库。

**若要将数据库角色映射到 web 服务器计算机帐户登录**

1. 像以前那样打开 SQL Server Management Studio。
2. 在**对象资源管理器**窗格中，展开**安全**节点，展开**登录名**节点，然后双击机帐户登录名 (例如， **FABRIKAM\TESTWEB1$**)。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. 在**登录属性**对话框中，单击**用户映射**。
4. 在**映射到此登录名的用户**表中，选择你的数据库的名称 (例如， **ContactManager**)。
5. 在**数据库角色成员身份：** *[数据库名称]*列表中，选择所需的权限。 在联系人管理器示例解决方案中，您必须选择**db\_datareader**和**db\_datawriter**角色。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. 单击 **“确定”**。

时手动将数据库角色映射通常绰绰有余测试环境，它是自动进行的或一键式部署到过渡环境或生产环境不需要的。 你可以找到有关自动执行这种使用中的后期部署脚本的任务的详细信息[到测试环境中部署数据库角色成员](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)。

> [!NOTE]
> 服务器项目和数据库项目的详细信息，请参阅[Visual Studio 2010 SQL Server 数据库项目](https://msdn.microsoft.com/library/ff678491.aspx)。


## <a name="configure-permissions-for-the-deployment-account"></a>配置部署帐户的权限

如果你将用于运行部署的帐户不是 SQL Server 管理员，你将需要创建此帐户的登录名。 若要创建数据库，帐户必须属于**dbcreator**服务器角色或拥有同等权限。

> [!NOTE]
> 当你使用 Web 部署或 VSDBCMD 将数据库部署时，你可以使用 Windows 凭据或 SQL Server 凭据，（如果您的 SQL Server 实例配置为支持混合的模式身份验证）。 下一步过程假设你想要使用 Windows 凭据，但无需进行任何停止你从在你配置的部署时连接字符串中指定的 SQL Server 用户名和密码。


**若要设置的部署帐户的访问权限**

1. 像以前那样打开 SQL Server Management Studio。
2. 在**对象资源管理器**窗格中，右键单击**安全**，指向**新建**，然后单击**登录**。
3. 在**登录名-新建**对话框中，在**登录名**框中，键入你部署的帐户的名称 (例如， **FABRIKAM\matt**)。
4. 在**选择页**窗格中，单击**服务器角色**。
5. 选择**dbcreator**，然后单击**确定**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

若要支持后续的部署，你将还需要部署将帐户添加到**db\_所有者**上首次部署后的数据库角色。 这是因为在后续部署上您要修改现有数据库架构，而不是创建新的数据库。 如前一部分中所述，无法将用户添加到数据库角色，直到你已创建数据库，原因很明显。

**若要部署的帐户登录名映射到的数据库\_所有者数据库角色**

1. 像以前那样打开 SQL Server Management Studio。
2. 在**对象资源管理器**窗口中，展开**安全**节点，展开**登录名**节点，然后双击机帐户登录名 (例如， **FABRIKAM\matt**)。
3. 在**登录属性**对话框中，单击**用户映射**。
4. 在**映射到此登录名的用户**表中，选择你的数据库的名称 (例如， **ContactManager**)。
5. 在**数据库角色成员身份：** *[数据库名称]*列表中，选择**db\_所有者**角色。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. 单击 **“确定”**。

## <a name="conclusion"></a>结束语

现在，你的数据库服务器应准备就绪可以接受远程数据库部署，以允许远程 IIS web 服务器以访问您的数据库。 在尝试部署和使用数据库之前，你可能想要检查这些要点：

- 已配置为接受远程 TCP/IP 连接的 SQL Server 吗？
- 已配置任何防火墙以允许 SQL Server 流量吗？
- 你是否已创建将访问 SQL Server 的每个 web 服务器计算机帐户登录？
- 您的数据库部署是否包括一个脚本以创建用户角色映射，或你是否需要在首次部署数据库后手动创建这些？
- 你已创建部署帐户的登录名并添加到**dbcreator**服务器角色？

## <a name="further-reading"></a>其他阅读材料

有关部署数据库项目的指南，请参阅[部署数据库项目](../web-deployment-in-the-enterprise/deploying-database-projects.md)。 通过运行后期部署脚本创建数据库角色成员身份的指南，请参阅[到测试环境中部署数据库角色成员](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)。 有关如何迎接唯一的部署带来成员资格数据库会带来的挑战的指南，请参阅[将成员资格数据库部署到企业环境](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md)。

>[!div class="step-by-step"]
[上一页](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
[下一页](creating-a-server-farm-with-the-web-farm-framework.md)
