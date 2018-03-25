---
uid: whitepapers/denied-access-to-iis-directories
title: "ASP.NET 拒绝了对 IIS 目录的访问 |Microsoft 文档"
author: rick-anderson
description: "该白皮书介绍了你必须执行的操作如果对 ASP.NET 应用程序的请求将返回错误，\"拒绝访问 DirectoryName 目录。 未能 s..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: 64118ac7a5f280775106d2dc7636923b08f28d89
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-denied-access-to-iis-directories"></a>ASP.NET 拒绝了对 IIS 目录的访问
====================
> 该白皮书介绍了如果向 ASP.NET 应用程序发出的请求将返回错误，必须执行"拒绝访问*DirectoryName*目录。 无法开始监视目录 chaanges。"
> 
> 适用于 ASP.NET 1.0 和 ASP.NET 1.1。


ASP.NET V1 RTM 现在运行在使用较低特权 windows 帐户的注册为本地计算机上的"ASPNET"帐户。

在某些情况下系统锁定状态，此帐户可能不是由默认具有读取安全访问的网站内容目录、 应用程序根目录中或网站根目录。 在这种情况下从给定的 web 应用程序请求页时将收到以下错误：

![](denied-access-to-iis-directories/_static/image1.jpg)

若要解决此问题将需要更改相应目录的安全权限。

具体而言，ASP.NET 需要读取、 执行和列出网站根 ASPNET 帐户访问权限 (例如： c:\inetpub\wwwroot 或可能已在 IIS 中配置的任何备用站点目录)，内容的目录和应用程序根目录下若要监视的配置文件更改。 与 IIS 管理工具 （inetmgr） 访问） 中的应用程序虚拟目录关联的文件夹路径相对应的应用程序根目录。

例如，考虑下的 wwwroot 文件夹的以下应用程序层次结构。

`C:\inetpub\wwwroot\myapp\default.aspx`

对于此示例中，ASPNET 帐户需要为 myapp 和的 wwwroot 目录中的内容定义的以上的读取的权限。 在根文件夹上的单一继承的 ACL 可以 （可选） 如果也使用为这两个目录，则它们在嵌套。

若要添加到目录的权限，请执行以下步骤：

- 使用 Windows 资源管理器，导航到目录
- 右键单击相应的目录文件夹，然后选择"属性"
- 导航到属性对话框的"安全"选项卡
- 单击"添加"按钮并输入计算机名称后跟 ASPNET 帐户名称。 例如，在计算机上名为"webdev"，将输入 webdev\ASPNET 并按"确定"。
- 确保 ASPNET 帐户具有"读取&amp;执行"，"列出文件夹内容"，并选中"读取"复选框。
- 点击确定以关闭对话框并保存所做的更改。

![](denied-access-to-iis-directories/_static/image2.jpg)

如果需要，可以将脚本或附带的"cacls.exe"工具用于 Windows 自动这些更改。 有关 ASPNET 帐户的详细信息，请参阅[常见问题文档](https://go.microsoft.com/fwlink/?LinkId=5828)。

如果给定的 web 应用程序依赖于具有写入或修改到特定文件夹或文件的权限，这可以通过在相同的过程并检查"写入"和/或"修改"复选框来授予。

在计算机上，允许每个人或用户组，对这些目录 （这是默认配置） 读取访问，将会遇到任何问题，不需要上述步骤。
