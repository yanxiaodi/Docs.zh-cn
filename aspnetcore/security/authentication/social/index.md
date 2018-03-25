---
title: "ASP.NET Core 中的 Facebook、Google 和外部提供程序身份验证"
author: rick-anderson
description: "本教程演示如何使用外部身份验证提供程序通过 OAuth 2.0 生成 ASP.NET Core 2.x 应用。"
manager: wpickett
ms.author: riande
ms.date: 11/01/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/social/index
ms.openlocfilehash: 76433f814d6850a449434c29eb0bd27570ce193a
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a>ASP.NET Core 中的 Facebook、Google 和外部提供程序身份验证

<a name="security-authentication-social-logins"></a>

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

本教程演示如何生成 ASP.NET Core 2.x 应用，该应用可让用户使用外部身份验证提供程序提供的凭据通过 OAuth 2.0 登录。

以下几节中介绍了 [Facebook](facebook-logins.md)、[Twitter](twitter-logins.md)、[Google](google-logins.md) 和 [Microsoft](microsoft-logins.md) 提供程序。 第三方程序包中提供了其他提供程序，例如 [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) 和 [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers)。

![Facebook、Twitter、Google plus 和 Windows 的社交媒体图标](index/_static/social.png)

使用户能够使用其当前凭据登录对用户来说十分便利，并且这样做可以将管理登录进程许多复杂操作转移给第三方。 有关社交登录如何驱动流量和客户转换的示例，请参阅 [Facebook](https://www.facebook.com/unsupportedbrowser) 和 [Twitter](https://dev.twitter.com/resources/case-studies) 的案例分析。

请注意：此处展示的程序包提取了 OAuth 身份验证流的大量复杂操作，但进行故障排除时，可能需要了解相关详细信息。 有许多资源可用，例如，请参阅 [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2)（OAuth 2 简介）或 [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/)（了解 OAuth 2）。 某些问题可通过查看 [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/dev/src)（提供程序包的 ASP.NET Core 源代码）解决。

## <a name="create-a-new-aspnet-core-project"></a>创建新的 ASP.NET Core 项目

* 在 Visual Studio 2017 中，从“开始”页创建新项目，或通过“文件”>“新建”>“项目”进行创建。

* 选择“Visual C#”>“.NET Core”分类中提供的“ASP.NET Core Web 应用程序”模板：

![“新建项目”对话框](index/_static/new-project.png)

* 点击“Web 应用程序”，验证“身份验证”是否设置为“单个用户帐户”：

![“新建 Web 应用程序”对话框](index/_static/select-project.png)

请注意：本教程适用于可从向导顶部选择的 ASP.NET Core 2.0 SDK 版本。

## <a name="apply-migrations"></a>应用迁移

* 运行应用并选择“登录”链接。
* 选择“以新用户身份注册”链接。
* 输入新帐户的电子邮件地址和密码，再选择“注册”。
* 按照说明操作来应用迁移。

## <a name="require-ssl"></a>要求 SSL

OAuth 2.0 需要使用 SSL 通过 HTTPS 协议进行身份验证。

请注意：如果如上图所示，在项目向导的“更改身份验证”对话框上选择“单个用户帐户”选项，使用 ASP.NET Core 2.x 的“Web 应用程序”或“Web API”项目模板创建的项目自动配置为启用 SSL 并通过 http URL 启动。

* 按照 [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl)（在 ASP.NET Core 应用中强制实施 SSL）主题中的步骤进行操作，要求在站点上使用 SSL。

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a>使用 SecretManager 存储登录提供程序分配的令牌

社交登录提供程序在注册过程中分配“应用程序 ID”和“应用程序机密”（确切命名因提供程序而异）。

这些值实际上是应用程序用于访问其 API 的“用户名”和“密码”，它们组成的“机密”可在“机密管理器”的帮助下链接到应用程序配置，而不是直接存储在配置文件中或被硬编码。

按照 [Safe storage of app secrets during development in ASP.NET Core](xref:security/app-secrets)（在 ASP.NET Core 中进行开发期间安全存储应用机密）主题中的步骤进行操作，以便可以存储以下每个登录提供程序分配的令牌。

## <a name="setup-login-providers-required-by-your-application"></a>应用程序所需的安装登录提供程序

使用以下主题配置应用程序，以使用相应的提供程序：

* [Facebook](facebook-logins.md) 相关说明
* [Twitter](twitter-logins.md) 相关说明
* [Google](google-logins.md) 相关说明
* [Microsoft](microsoft-logins.md) 相关说明
* [其他提供程序](other-logins.md)相关说明

## <a name="optionally-set-password"></a>选择性地设置密码

使用外部登录提供程序注册，即表明还没有向应用注册密码。 这可让用户无需创建和记住站点密码，但也会使用户依赖外部登录提供程序。 如果外部登录提供程序不可用，则无法登录网站。

使用外部提供程序在登录过程中设置的电子邮箱创建密码和登录：

* 点击右上角的“Hello <email alias>”链接，导航到“管理”视图。

![Web 应用程序“管理”视图](index/_static/pass1a.png)

* 点击“创建”

![“设置密码”页](index/_static/pass2a.png)

* 设置一个有效密码，可以用此密码和邮箱登录。

## <a name="next-steps"></a>后续步骤

* 本文介绍了外部身份验证，并说明了向 ASP.NET Core 应用添加外部登录所需的先决条件。

* 引用特定于提供程序的页，为应用所需的提供程序配置登录。
