---
uid: whitepapers/aspnet-and-iis6
title: "向 IIS 6.0 中运行 ASP.NET 1.1 |Microsoft 文档"
author: rick-anderson
description: "虽然 Windows Server 2003 包含 IIS 6.0 和 ASP.NET 1.1，这些组件在默认情况下处于禁用状态。 该白皮书介绍了如何启用 IIS 6.0..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1fcac7b8bc295ccf4e36189295b6bc2e4d328623
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="running-aspnet-11-with-iis-60"></a>向 IIS 6.0 中运行 ASP.NET 1.1
====================
> 虽然 Windows Server 2003 包含 IIS 6.0 和 ASP.NET 1.1，这些组件在默认情况下处于禁用状态。 本白皮书介绍如何启用 IIS 6.0 和 ASP.NET 1.1 版中，并建议多个配置设置以从 IIS 和 ASP.NET 中获取最佳性能。
> 
> 适用于 ASP.NET 1.1 和 IIS 6.0。


ASP.NET 1.1 附带还包括最新版本的 Internet 信息服务器 (IIS) 6.0 版的 Windows Server 2003。 IIS 6.0 和 ASP.NET 1.1 旨在无缝集成，ASP.NET 现在默认值为新的 IIS 6.0 辅助进程模型。

## <a name="aspnet-11-is-not-installed-by-default"></a>默认情况下不安装 ASP.NET 1.1

与以前版本的 Microsoft 的服务器操作系统，不同的是 Internet 信息服务器 (IIS) 未启用默认设置。也不是 ASP.NET 1.1。 有两个选项用于启用 IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>启用 IIS，选项 #1-配置服务器向导

Windows Server 2003 附带新配置服务器向导来帮助你正确配置服务器所需模式。

若要启动向导-请注意，若要运行的向导必须以管理员身份的身份登录，请转到： 启动 |程序 |管理工具和选择配置服务器。

选择后，你应看到配置服务器向导开始屏幕：

![](aspnet-and-iis6/_static/image1.jpg)

单击下一步&gt;:

![](aspnet-and-iis6/_static/image2.jpg)

单击下一步&gt;

![](aspnet-and-iis6/_static/image3.jpg)

在此屏幕上，你将需要选择应用程序服务器 （IIS，ASP.NET） 作为配置的选项。

单击下一步&gt;。

![](aspnet-and-iis6/_static/image4.jpg)

选择后将服务器配置为应用程序服务器，此屏幕将显示提示应安装哪些其他功能。 默认情况下选择任何选项。 若要自动启用 ASP.NET，你需要选择启用 ASP。NET。

单击下一步&gt;。

![](aspnet-and-iis6/_static/image5.jpg)

此屏幕显示要安装的选项。

单击下一步&gt;。

![](aspnet-and-iis6/_static/image6.jpg)

在安装你选择的选项时，你将看到此屏幕。 它是正常的请参阅框显示为正在安装服务的其他对话框。 可能此外会提示你为 Windows 2003 Server 安装 CD 的位置。

单击下一步&gt;完成时。

![](aspnet-and-iis6/_static/image7.jpg)

单击完成-Windows Server 2003 现在已配置为支持 IIS 6.0 和 ASP.NET 1.1 版。

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>启用 IIS，选项 #2-手动配置 IIS 和 ASP.NET

如果不希望使用配置服务器向导您可以选择安装 IIS 6.0 和 ASP.NET 1.1 使用添加或删除程序从控制面板。

首次打开控制面板:

![](aspnet-and-iis6/_static/image8.jpg)

接下来，单击添加/删除 Windows 组件这将打开 Windows 组件向导:

![](aspnet-and-iis6/_static/image9.jpg)

突出显示并检查应用程序服务器，然后单击详细信息？ 按钮：

![](aspnet-and-iis6/_static/image10.jpg)

若要安装 ASP.NET，请检查 ASP。NET。

单击确定以返回到 Windows 组件向导。 单击下一步&gt;从 Windows 组件向导以开始安装：

![](aspnet-and-iis6/_static/image11.jpg)

它是正常的请参阅框显示为正在安装服务的其他对话框。 可能此外会提示你为 Windows 2003 Server 安装 CD 的位置。

安装完成后你将看到 Windows 组件向导的最后一个屏幕：

![](aspnet-and-iis6/_static/image12.jpg)

IIS 6.0 和 ASP.NET 1.1 现已配置并且可用。

## <a name="recommended-settings"></a>建议的设置

使用 IIS 6.0 中运行 ASP.NET 1.1 时有多个配置设置，建议从 ASP.NET 获得最佳性能：

- 配置辅助进程内存限制
- 配置辅助进程回收

### <a name="configuring-worker-process-memory-limits"></a>配置辅助进程内存限制

默认情况下 IIS 6.0 不允许 IIS 使用的内存量设置限制。 ASP。NET 的缓存功能依赖于内存的限制，以便从内存中缓存可以主动删除未使用的项。

建议你配置的内存回收 IIS 6.0 的功能。 若要配置此打开的 Internet Information Services 管理器 (开始 |程序 |管理工具 |Internet Information Services)。 后打开，展开应用程序池文件夹：

每个应用程序池：

![](aspnet-and-iis6/_static/image13.jpg)

1. 例如右键单击应用程序池DefaultAppPool 并选择属性:

![](aspnet-and-iis6/_static/image14.jpg)

2. 接下来，启用单击任何一个的内存回收最大内存使用 （以兆字节为单位）:。 值不应为多个服务器上的物理 （不是虚拟的） 内存量，极佳近似值为 60%的物理内存，即对于具有 512 MB 的物理内存选择 310 的服务器。 建议使用 2 GB 地址空间时，最大值将不超过 800 MB。 如果服务器的内存地址空间为 3 GB，可以最高可达 1，800 MB 的工作进程的最大内存限制：

![](aspnet-and-iis6/_static/image15.jpg)

单击应用和确定退出属性对话框。 对于所有可用的应用程序池重复此操作。

### <a name="configuring-worker-recycling"></a>配置辅助回收

默认情况下 IIS 6.0 配置以回收其工作进程每 29 个小时。 这是有点主动运行的 ASP.NET 应用程序，建议已禁用的自动工作进程回收。

若要禁用自动工作进程回收，请先打开 Internet Information Services 管理器 (开始 |程序 |管理工具 |Internet Information Services)。 后打开，展开应用程序池文件夹：

![](aspnet-and-iis6/_static/image16.jpg)

每个应用程序池：

1. 例如右键单击应用程序池DefaultAppPool 并选择属性:

![](aspnet-and-iis6/_static/image17.jpg)

2. 取消选中回收工作进程 （以分钟为单位）::

![](aspnet-and-iis6/_static/image18.jpg)

单击应用和确定退出属性对话框。 对于所有可用的应用程序池重复此操作。

## <a name="granting-write-access-to-the-file-system"></a>授予对文件系统的写访问权限

如果你的应用程序需要写到文件系统的访问权限，并且使用 NTFS 你将需要修改访问控制列表 (ACL) 上的文件夹或文件授予对 ASP.NET 访问权限。

例如，若要授予 ASP.NET 写访问权限 c:\inetpub\wwwroot 首先打开资源管理器并导航到的目录：

![](aspnet-and-iis6/_static/image19.jpg)

接下来，右键单击该目录，例如，wwwroot 并选择属性。 属性对话框打开后，选择安全选项卡：

![](aspnet-and-iis6/_static/image20.jpg)

C:\inetpub\wwwroot\ 目录是一个特殊目录中的特殊 IIS 6.0 组 IIS\_WPG 已授予读取&amp;执行、 列出文件夹内容和读取权限。 但是，若要授予写入权限，您需要单击允许复选框以进行写入：

![](aspnet-and-iis6/_static/image21.jpg)

IIS 6.0 现在对此文件夹具有写入权限。 若要授予写权限的其他文件夹，请按照下列步骤-请注意，你可能需要添加 IIS\_WPG 组如果不存在。

> [!CAUTION]
> 授予写权限 IIS\_WPG 将允许任何 ASP.NET 应用程序写入到该目录。

## <a name="supporting-integrated-authentication-with-sql-server"></a>支持与 SQL Server 的集成身份验证

集成身份验证允许 SQL Server 利用 Windows NT 身份验证来验证 SQL Server 登录帐户。 这允许用户跳过在标准的 SQL Server 登录过程。 使用此方法时，网络用户可以访问 SQL Server 数据库，而无需提供单独的登录标识或密码，因为 SQL Server 从 Windows NT 网络的安全过程获取的用户和密码信息。

选择用于 ASP.NET 应用程序的集成身份验证是一个不错的选择，因为没有凭据曾经存储在你的应用程序连接字符串。 而是用来连接到 SQL 连接字符串将如下所示：

`"server=localhost; database=Northwind;Trusted_Connection=true"`

此连接字符串告知 SQL Server 以使用应用程序尝试访问 SQL Server 的 Windows 凭据。 在 ASP.NET/IIS 6 的情况下，这将是在 IIS 中的帐户\_WPG 组。

若要启用 SQL Server 和 ASP.NET 之间的集成身份验证，你将需要首先确保 SQL Server 是否配置为集成身份验证或混合模式身份验证-咨询 DBA 以确定这一点。 如果这两种模式之一中有 SQL Server，你可以使用集成身份验证。

打开 SQL Server 企业管理器 (开始 |程序 |Microsoft SQL Server |企业管理器），选择适当的服务器，然后展开安全性文件夹：

![](aspnet-and-iis6/_static/image22.jpg)

如果 BUILTINT\IIS\_WPG 未列出组、 登录名右键单击并选择新建登录名:

![](aspnet-and-iis6/_static/image23.jpg)

在名称: 文本框中输入 [服务器/域名] \IIS\_WPG 或单击省略号按钮以打开 Windows NT 用户/组选取器：

![](aspnet-and-iis6/_static/image24.jpg)

选择在当前计算机的 IIS\_WPG 组，然后单击添加和确定以关闭选择器。

然后，你需要设置为默认数据库和访问数据库的权限。 若要设置的默认数据库选择从下拉列表中，选择以下 Northwind 例如：

![](aspnet-and-iis6/_static/image25.jpg)

接下来，单击数据库访问选项卡：

![](aspnet-and-iis6/_static/image26.jpg)

单击你想要允许访问每个数据库的允许复选框。 你还需要选择数据库角色检查 db\_所有者将确保你的登录名具有所有必要的权限管理，使用所选的数据库。

单击确定退出属性对话框。 ASP.NET 应用程序现在已配置为支持集成的 SQL Server 身份验证。

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>不在 IIS 6.0 纯模式下运行 ASP.NET 1.0

在 IIS 5 兼容性模式下仅支持 IIS 6.0 上的 ASP.NET 1.0。

若要配置在 IIS 5.0 兼容性模式下运行的 ASP.NET 1.0，打开 Internet 服务管理器，右键单击网站并选择属性：

![](aspnet-and-iis6/_static/image27.jpg)

切换到服务选项卡，并检查？以 IIS 5.0 隔离模式运行 WWW 服务？:

![](aspnet-and-iis6/_static/image28.jpg)
