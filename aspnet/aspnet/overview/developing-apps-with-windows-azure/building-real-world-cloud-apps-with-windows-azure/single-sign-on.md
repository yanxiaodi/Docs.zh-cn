---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: "单一登录 （使用 Azure 构建真实世界云应用） |Microsoft 文档"
author: MikeWasson
description: "构建真实世界云应用程序与 Azure 的电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: b3640c94a8ae9ede330c0fe6a392acb5843cb65c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>单一登录 （使用 Azure 构建真实世界云应用）
====================
通过[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下载修复此错误项目](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下载电子书](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **构建真实世界云应用程序与 Azure**电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，从而帮助你为成功开发适用于云中的 web 应用。 有关电子书的信息，请参阅[第一章](introduction.md)。


有很多安全问题，可考虑时，你要开发云应用程序，但这一系列我们将重点仅仅一个： 单一登录。 问题人经常询问的这是:"我要主要构建应用的我的公司; 员工如何承载这些应用程序在云中并仍使其能够使用相同的安全模型，我的员工知道它们正在运行应用程序时，在本地环境中使用托管防火墙内部？" 一种我们启用此方案的称为 Azure Active Directory (Azure AD)。 Azure AD，可使企业业务线 (LOB) 应用可通过 Internet，并且它使您能够为业务合作伙伴也提供这些应用。

## <a name="introduction-to-azure-ad"></a>Azure AD 简介

[Azure AD](https://docs.microsoft.com/azure/active-directory/)提供[Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx)在云中。 主要功能包括：

- 它与本地 Active Directory 集成。
- 它使与你的应用程序的单一登录。
- 它支持开放标准，如[SAML](http://en.wikipedia.org/wiki/SAML_2.0)，[的是 Ws-fed](http://en.wikipedia.org/wiki/WS-Federation)，和[OAuth 2.0](http://oauth.net/2/)。
- 它支持企业[Graph REST API](https://msdn.microsoft.com/library/hh974476.aspx)。

假设你有使用以使员工能够登录到 Intranet 应用程序的本地 Windows Server Active Directory 环境：

![](single-sign-on/_static/image1.png)

新增功能 Azure AD，您可以执行是在云中创建一个目录。 它是免费功能且易于设置。

它可以是完全独立于你的本地 Active Directory;你可以放置任何人都想在其中和 Internet 应用程序进行身份验证。

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

也可以将它与你在本地 AD。

![AD 和 WAAD 集成](single-sign-on/_static/image3.png)

现在可以进行本地身份验证的所有员工可以还进行身份都验证通过 Internet – 您无需打开防火墙或部署你的数据中心中的任何新服务器。 你可以继续利用所有现有 Active Directory 环境，您了解并使用其目前用于提供有关功能的内部应用单一登录。

一旦你所做此 AD 与 Azure AD 之间的连接，你还可以启用你的 web 应用和移动设备进行身份验证你的员工在云中，并且你可以启用第三方应用程序，如 Office 365、 SalesForce.com 或 Google 应用，以接受你员工的凭据。 如果你使用 Office 365，你在已设置与 Azure AD 因为 Office 365 使用 Azure AD 进行身份验证和授权。

![第三方应用程序](single-sign-on/_static/image4.png)

此方法的优点是的只要你的组织将添加或删除用户，或者用户更改密码，则使用在你的本地环境中现在使用的相同流程。 所有的本地 AD 更改会自动传播到云环境。

如果使用或移动到 Office 365 好消息你的公司，必须自动设置因为 Office 365 使用 Azure AD 进行身份验证的 Azure AD。 因此你可以轻松地使用你自己的应用程序中的 Office 365 使用的相同身份验证。

## <a name="set-up-an-azure-ad-tenant"></a>设置 Azure AD 租户

Azure AD 目录调用 Azure AD[租户](https://technet.microsoft.com/library/jj573650.aspx)，而且设置租户非常简单。 我们将介绍您如何它为了在 Azure 管理门户中阐释的概念，但这是当然的与其他门户函数类似你还可以执行它通过使用脚本或管理 API。

在管理门户中单击 Active Directory 选项卡。

![在门户中的 WAAD](single-sign-on/_static/image5.png)

自动为你的 Azure 帐户，拥有一个 Azure AD 租户，可以单击**添加**页后，可以创建其他目录底部的按钮。 需要一个用于测试环境，一个用于生产，例如。 请仔细考虑有关命名新目录。 如果你使用你目录的名称，然后你使用你再次对其中一个名称的用户，可能容易引起混淆。

![添加目录](single-sign-on/_static/image6.png)

门户具有用于创建、 删除和管理此环境中的用户的完整支持。 例如，若要添加用户转到**用户**选项卡，单击**添加用户**按钮。

![添加用户按钮](single-sign-on/_static/image7.png)

![添加用户对话框](single-sign-on/_static/image8.png)

你可以创建仅在此目录中存在的新用户，或可以为此目录中的用户作为此目录中或注册中的用户或用户从另一个 Azure AD 目录中注册 Microsoft 帐户。 （在真实的目录，默认域为 ContosoTest.onmicrosoft.com。你还可以使用你自己选择，如 contoso.com 的域。）

![用户类型](single-sign-on/_static/image9.png)

![添加用户对话框](single-sign-on/_static/image10.png)

你可以为用户分配角色。

![用户配置文件](single-sign-on/_static/image11.png)

并使用一个临时密码创建帐户。

![临时密码](single-sign-on/_static/image12.png)

以此方式创建的用户立即可以登录到你的 web 应用使用此云目录所造成。

什么是非常适合企业单一登录，不过，是**目录集成**选项卡：

![目录集成选项卡](single-sign-on/_static/image13.png)

如果你启用目录集成和[下载的工具，](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx)，你可以同步与你现有的本地 Active Directory，你已在组织内部使用此云目录。 然后所有存储在你的目录中的用户将显示在此云目录。 你的云应用现在可以验证所有员工使用其现有的 Active Directory 凭据。 以及所有这些操作的可用 – 同步工具和 Azure AD 本身。

该工具是一个是易于使用，可从这些屏幕截图中看到的向导。 这些不是完整的说明，只是一个示例显示基本过程。 有关更详细操作方法-到-执行-it 信息，请参阅中的链接[资源](#resources)章结尾部分。

![WAAD 同步工具配置向导](single-sign-on/_static/image14.png)

单击**下一步**，然后输入你的 Azure Active Directory 凭据。

![WAAD 同步工具配置向导](single-sign-on/_static/image15.png)

单击**下一步**，然后输入你的本地 AD 凭据。

![WAAD 同步工具配置向导](single-sign-on/_static/image16.png)

单击**下一步**，然后表示如果你想要在云中存储的 AD 密码哈希值。

![WAAD 同步工具配置向导](single-sign-on/_static/image17.png)

你可以在云中存储密码哈希是单向哈希;实际密码永远不会存储在 Azure AD 中。 如果你决定对在云中存储哈希，你将需要使用[Active Directory 联合身份验证服务](https://technet.microsoft.com/library/hh831502.aspx)(ADFS)。 也有[其他因素时，应考虑选择是否使用 ADFS](https://technet.microsoft.com/library/jj573653.aspx)。 ADFS 选项需要几个其他配置步骤。

如果您选择将哈希存储在云中，完毕后，此工具将启动同步目录，单击时**下一步**。

![WAAD 同步工具配置向导](single-sign-on/_static/image18.png)

并在几分钟内已完成。

![WAAD 同步工具配置向导](single-sign-on/_static/image19.png)

只需在组织中在 Windows 2003 或更高版本的一个域控制器上运行此。 而无需重新启动。 当完毕后，你的所有用户都位于云中，你可以从任何 web 或移动应用程序，使用 SAML、 OAuth、 或的是 Ws-fed 的单一登录。

有时我们获取要求这是有关如何安全 – Microsoft 使用它自己敏感业务数据？ 和问题的回答是是执行操作。 例如，如果你转到内部 Microsoft SharePoint 站点在[https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/)，你让系统提示你登录。

![Office 365 登录](single-sign-on/_static/image20.png)

Microsoft 已启用 ADFS，因此当你输入 Microsoft ID，你获取重定向到 ADFS 登录页。

![ADFS 登录](single-sign-on/_static/image21.png)

和后输入存储在内部 Microsoft AD 帐户的凭据后，你有权访问此内部应用程序。

![MS SharePoint 站点](single-sign-on/_static/image22.png)

主要是因为我们已经有 ADFS 设置 Azure AD 变为可用，但登录过程将通过在云中的 Azure AD 目录之前，我们将使用 AD 在登录服务器。 我们将我们重要文档、 源代码管理、 性能管理文件、 销售报表、 和的详细信息，在云中，并且要使用此确切的同一个解决方案来确保它们的安全。

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>创建一个 ASP.NET 应用程序使用 Azure AD 进行单一登录

Visual Studio 使极为简便地创建应用程序使用 Azure AD 进行单一登录，你可从几个屏幕截图中看到。

当你创建一个新的 ASP.NET 应用，MVC 或 Web 窗体的默认身份验证方法是 ASP.NET 标识。 若要将程序更改为 Azure AD，请单击**更改身份验证**按钮。

![更改身份验证](single-sign-on/_static/image23.png)

选择组织帐户，输入你的域名，然后选择单一登录。

![配置身份验证对话框](single-sign-on/_static/image24.png)

也可以提供应用程序读取或读/写目录数据的权限。 如果你这样做，它可以使用[Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx)若要查找用户的电话号码，了解它们是否在办公室中，当最后一个记录，等等。

这就是你所要做的所有 Visual Studio 会要求提供的凭据为你的 Azure AD 租户的管理员，然后配置你的项目和 Azure AD 租户的新应用程序。

运行项目时，你将看到在登录页面，并在你的 Azure AD 目录中可以使用的用户凭据登录。

![组织帐户登录](single-sign-on/_static/image25.png)

![登录](single-sign-on/_static/image26.png)

将应用程序部署到 Azure 时，所要做时，选择**启用组织身份验证**复选框，然后再一次 Visual Studio 将负责为你的所有配置。

![发布网站](single-sign-on/_static/image27.png)

这些屏幕截图来自演示如何生成使用 Azure AD 身份验证的应用程序的完整分步教程：[开发的 ASP.NET 应用程序与 Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)。

## <a name="summary"></a>摘要

在本章你了解 Azure Active Directory、 Visual Studio 和 ASP.NET，使其容易安装在你组织的用户的 Internet 应用程序中的单一登录。 你的用户可在 Internet 应用程序使用他们用于登录在内部网络中使用 Active Directory 的相同凭据登录。

[下一章](data-storage-options.md)考察可用于云应用程序的数据存储选项。

<a id="resources"></a>
## <a name="resources"></a>资源

有关更多信息，请参见以下资源：

- [Azure Active Directory 文档](https://docs.microsoft.com/azure/active-directory/)。 有关 windowsazure.com 站点上的 Azure AD 文档门户页面。 分步教程，请参阅**开发**部分。
- [Azure 多因素身份验证](https://docs.microsoft.com/azure/multi-factor-authentication/)。 有关在 Azure 中的多因素身份验证的文档的门户页。
- [组织帐户身份验证选项](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions)。 在 Visual Studio 2013 新项目对话框中的 Azure AD 身份验证选项的说明。
- [Microsoft 模式和实践-联合标识模式](https://msdn.microsoft.com/library/dn589790.aspx)。
- [如何： 安装 Azure Active Directory 同步工具](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx)。
- [Active Directory 联合身份验证服务 2.0 内容导航图](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx)。 链接到有关 ADFS 2.0 的文档。
- [Windows Azure AD 应用程序中基于角色的和基于 ACL 的授权](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1)。 示例应用程序。
- [Azure Active Directory Graph API 博客](https://blogs.msdn.com/b/aadgraphteam/)。
- [访问控制中 BYOD 和混合标识基础结构中的目录集成](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=)。 技术 Ed 2014 会话视频通过 Gayana Bagdasaryan。

>[!div class="step-by-step"]
[上一页](web-development-best-practices.md)
[下一页](data-storage-options.md)
