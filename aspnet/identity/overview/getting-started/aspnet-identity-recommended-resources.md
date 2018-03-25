---
uid: identity/overview/getting-started/aspnet-identity-recommended-resources
title: "ASP.NET 标识的推荐资源 |Microsoft 文档"
author: Rick-Anderson
description: "本主题提供有关如何使用 ASP.NET Identity 的文档资源的链接。 如果你知道出色的博客文章、 堆栈溢出的线程或任何其他链接..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2015
ms.topic: article
ms.assetid: 0f78aec2-f509-46fa-b20f-d5208425d8ec
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-recommended-resources
msc.type: authoredcontent
ms.openlocfilehash: cb6e378d6b1de7d43bbc4272b64f9029f4e92924
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-identity-recommended-resources"></a>ASP.NET 标识的推荐资源
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> 本主题提供有关如何使用 ASP.NET Identity 的文档资源的链接。
> 
> 如果你知道出色的博客文章， [stackoverflow](http://stackoverflow.com)线程或都非常有用的任何其他链接[向我们发送一封电子邮件](mailto:aspnetue@microsoft.com?subject=Identity recommended resources)与链接或只是将保留在此页底部的消息。


- [ASP.NET 标识入门](#gettingstarted)
- [新的特色必须读取文章](#feat)
- [中间 ASP.NET 标识](#adv)
- [视频](#video)
- [若要提问、 请求功能，报告 bug，并每夜生成的位置](#samp)
- [标识上的博客文章](#blog)
- [ASP.NET 标识的自定义的存储提供程序](#cust)
- [附加的标识资源](#additional)
- [Q &amp; （问题/答案）](#qand)

<a id="gettingstarted"></a>
## <a name="getting-started-with-aspnet-identity"></a>Getting Started with ASP.NET 标识

- [使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登录的 MVC 5 应用程序](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)本教程演示如何编写使用 Facebook 和 Google oauth2 授权 ASP.NET MVC 5 应用程序。 它还演示如何向标识数据库中添加额外的数据。
- [将包含成员资格、 OAuth 和 SQL 数据库的安全 ASP.NET MVC 应用程序部署到 Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 本教程将添加 Azure 部署，如何保护使用角色，对应用程序如何使用成员资格 API 来添加用户和角色，以及其他安全功能。
- [ASP.NET 标识简介](introduction-to-aspnet-identity.md)
- [创建具有登录、 电子邮件确认及密码重置安全的 ASP.NET MVC 5 web 应用程序](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)
- [具有 SMS 和电子邮件双因素身份验证的 ASP.NET MVC 5 应用](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)

<a id="feat"></a>
## <a name="new-featured-must-read-articles"></a>新的特色必须读取文章

- [演练： 使用 Microsoft 帐户身份验证的 ASP.NET MVC 标识](http://www.benday.com/2014/02/25/walkthrough-asp-net-mvc-identity-with-microsoft-account-authentication/)通过[Benjamin 天](http://www.benday.com/about/)
- [ASP.NET 标识 2.0 扩展标识模型和而不是字符串使用整数键](http://typecastexception.com/post/2014/07/13/ASPNET-Identity-20-Extending-Identity-Models-and-Using-Integer-Keys-Instead-of-Strings.aspx)
- [使用 ASP.NET Web API 2、 Owin，以及标识 AngularJS 令牌身份验证](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
- [Thinktecture.IdentityManager WSAT 替代](http://www.hanselman.com/blog/ThinktectureIdentityManagerAsAReplacementForTheASPNETWebSiteAdministrationTool.aspx)
- [ASP.NET 标识 2.0： 自定义用户和角色](http://typecastexception.com/post/2014/06/22/ASPNET-Identity-20-Customizing-Users-and-Roles.aspx)

<a id="adv"></a>
## <a name="intermediate-aspnet-identity"></a>中间 ASP.NET 标识

- [帐户确认和密码恢复 ASP.NET 标识](../features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [使用 SMS 和 ASP.NET 标识的双因素身份验证](../features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [将现有网站从 SQL 成员身份迁移到 ASP.NET 标识](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [向空的或现有的 Web 窗体项目添加 ASP.NET 标识](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md)
- MSDN 杂志[外部身份验证与 ASP.NET 标识](https://msdn.microsoft.com/magazine/dn745860.aspx)通过 Dino Esposito
- MSDN 杂志[首先查看一下 ASP.NET 标识](https://msdn.microsoft.com/magazine/dn605872.aspx)通过 Dino Esposito
- [ASP.NET 标识 – 用户锁定](http://tech.trailmax.info/2014/06/asp-net-identity-user-lockout/)

<a id="samp"></a>
## <a name="where-to-ask-questions-request-features-report-a-bug-and-nightly-builds"></a>若要提问、 请求功能，报告 bug，并每夜生成的位置

- 对于堆栈溢出，使用标记[aspnet 标识](http://stackoverflow.com/questions/tagged/asp.net-identity)
- 为 ASP.NET 论坛中，发布到[安全论坛](https://forums.asp.net/25.aspx)并添加**ASP.NET 标识**标题。
- [在 GitHub 上的 ASP.NET 标识](https://github.com/aspnet/AspNetIdentity)Get 每夜生成，请求功能，打开 bug。

<a id="blog"></a>
## <a name="blog-posts-on-identity"></a>标识上的博客文章

- [在 ASP.NET Identity 中的 SecurityStamp 是什么？](http://stackoverflow.com/questions/19487322/what-is-asp-net-identitys-iusersecuritystampstoretuser-interface/19505060#19505060)
- 通过[John 输入](https://twitter.com/xivSolutions)

    - [ASP.NET 标识 2.0 扩展标识模型和而不是字符串使用整数键](http://typecastexception.com/post/2014/07/13/ASPNET-Identity-20-Extending-Identity-Models-and-Using-Integer-Keys-Instead-of-Strings.aspx)
    - [ASP.NET 标识 2.0： 自定义用户和角色](http://typecastexception.com/post/2014/06/22/ASPNET-Identity-20-Customizing-Users-and-Roles.aspx)
    - [ASP.NET MVC 和标识 2.0： 了解基础知识](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)
    - [设置帐户验证和双因素授权](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)
    - [配置数据库连接和 ASP.NET MVC 5 中的标识帐户的代码优先迁移和 Visual Studio 2013](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx)
- 通过[Taiseer Joudeh](http://bitoftech.net/taiseer-joudeh-blog/)

    - [令牌基于身份验证使用 ASP.NET Web API 2、 Owin 中间件和 ASP.NET 标识](http://bitoftech.net/2014/06/01/token-based-authentication-asp-net-web-api-2-owin-asp-net-identity/)
    - [使用 ASP.NET Web API 2、 Owin，以及标识 AngularJS 令牌身份验证](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
    - [启用 OAuth AngularJS 应用使用 ASP.NET Web API 2 和 Owin – 第 3 部分中的刷新令牌。](http://bitoftech.net/2014/07/16/enable-oauth-refresh-tokens-angularjs-app-using-asp-net-web-api-2-owin/)
- 通过[Anders Abel](https://twitter.com/anders_abel)

    - [了解 Owin 外部身份验证管道](http://coding.abel.nu/2014/06/understanding-the-owin-external-authentication-pipeline/)
    - [ASP.NET 标识和 Owin 概述](http://coding.abel.nu/2014/06/asp-net-identity-and-owin-overview/)

 通过[k。 Scott Allen](https://twitter.com/OdeToCode)上 Ode 到代码

    - [ASP.NET 核心标识](http://odetocode.com/blogs/scott/archive/2013/11/25/asp-net-core-identity.aspx)此博客检查核心抽象，包括 IUser、 IUserStore 和 I\*存储区接口。
    - [与实体框架的 ASP.NET 标识](http://odetocode.com/blogs/scott/archive/2014/01/03/asp-net-identity-with-the-entity-framework.aspx)MVC 5、 Web API 和 SPA 应用、 连接字符串和管理上下文中的单个用户帐户
    - [具有 ASP.NET 标识的自定义选项](http://odetocode.com/blogs/scott/archive/2014/01/09/customization-options-with-asp-net-identity.aspx)
    - [实现 ASP.NET 标识](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- [Benjamin 天](http://www.benday.com/about/)[演练： 使用 Microsoft 帐户身份验证的 ASP.NET MVC 标识](http://www.benday.com/2014/02/25/walkthrough-asp-net-mvc-identity-with-microsoft-account-authentication/)
- [Brock Allen](https://twitter.com/BrockLAllen)

    - [OWIN/Katana 身份验证中间件的外部登录提供程序 （社交登录名） 入门](http://brockallen.com/2014/01/09/a-primer-on-external-login-providers-social-logins-with-owinkatana-authentication-middleware/)
    - [引入 IdentityReboot](http://brockallen.com/2014/02/11/introducing-identityreboot/)： 一组扩展到 ASP.NET 标识实现已抱怨主要缺少的功能。
- [Pranav Rastogi](https://twitter.com/rustd)

    - [获取从社交提供程序的详细信息](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [@beabigrockstar](https://twitter.com/beabigrockstar)(Jerrie Pelser)

    - [2 因素身份验证](http://www.beabigrockstar.com/blog/2-factor-authentication-with-asp-net-identity-2-0-beta-1/)
    - [具有 ASP.NET 标识使用 Google 身份验证器](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)
    - [ASP.NET MVC 5 身份验证指南](http://www.beabigrockstar.com/)
- [从 VS 2013 项目模板中使用的社交提供商获取详细信息](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [生成包含 ASP.NET Identity 的简单 ToDo 应用程序并将用户与 ToDoes 关联](https://blogs.msdn.com/b/webdev/archive/2013/10/20/building-a-simple-todo-application-with-asp-net-identity-and-associating-users-with-todoes.aspx)
- [与 ASP.NET Identity 的 Google OpenId 集成问题](http://blog.technovert.com/2014/01/google-openid-integration-issues-asp-net-identity/)如果收到错误： HTTP 错误 404.15-找不到请求筛选模块被配置为拒绝查询字符串是否太长的请求
- [Thinktecture.IdentityManager WSAT 替代](http://www.hanselman.com/blog/ThinktectureIdentityManagerAsAReplacementForTheASPNETWebSiteAdministrationTool.aspx)
- [使用 ASP.NET Web API 2、 Owin，以及标识 AngularJS 令牌身份验证](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
- [而无需实体框架的简单 Asp.net 标识核心](https://code.msdn.microsoft.com/Simple-Aspnet-Identiy-Core-7475a961)
- [使用在 MVC 的 ASP.NET Identity 中的角色](http://www.dotnetfunda.com/articles/show/2898/working-with-roles-in-aspnet-identity-for-mvc)通过[Sheo Narayan](http://www.dotnetfunda.com/profile/sheonarayan.aspx)
- [将从 ASP.NET 成员资格，移到 ASP.NET 标识](http://webdojo.sharepoint.com/ajmatthews/_layouts/15/start.aspx#/Lists/Posts/Post.aspx?ID=2)通过 Alistair Matthews

<a id="video"></a>
## <a name="videos"></a>视频

- 第 9 频道[保护 ASP.NET 应用程序和服务： 现代应用程序的安全 Facelift](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B421#fbid=PhVT9E1WRtr?hashlink=fbid)通过 Ido Flatow
- 第 9 频道[ASP.NET 标识简介](https://channel9.msdn.com/Events/dotnetConf/2014/ASP-NET-Identity-Security)通过 Pranav Rastogi
- 第 9 频道[ASP.NET 身份验证使用 ASP.NET 标识](https://channel9.msdn.com/Shows/Web+Camps+TV/Special-Movember-Episode-ASPNET-Authentication-Provider)通过 Cory Fowler
- 第 9 频道[生成现代应用 Web 程序： ASP.NET 标识](https://channel9.msdn.com/Series/Building-Modern-Web-Apps/03)通过 Jeff Koch
- 第 9 频道[使用 ASP.NET 标识保护您的网站](https://channel9.msdn.com/Events/TechDays/Techdays-2014-the-Netherlands/Securing-your-website-with-ASP-NET-Identity)通过 Alex Thissen
- [在现有的数据库模型上使用 ASP.NET 标识](https://www.youtube.com/watch?v=elfqejow5hM)通过 Alexander Schmidt
- [ASP.NET 一个标识](https://www.youtube.com/watch?v=w8GD-QIusKk)通过的 Telerik Ivaylo Kenov
- [捷克语 ASP.NET 标识](https://www.youtube.com/watch?v=tVbZp5brcpY)本课在我们将演示如何部署基本身份验证、 如何添加支持 Twitter 或 Facebook 等外部标识提供程序以及如何使用一次性密码 (OTP)。 [ASP.NET 标识 je nástupce 成员身份角色提供程序 &#367; v ASP.NET，tedy knihovna pro zajišt &#283; ní autentizace uživatel &#367;。 V této p &#345; ednášce si ukážeme，jak nasad]

<a id="cust"></a>
## <a name="custom-storage-providers-for-aspnet-identity"></a>ASP.NET 标识的自定义的存储提供程序

如果你想要编写自己的提供程序，读取[概述的自定义存储提供程序 ASP.NET 标识](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)和[实现 ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx) ，然后检查列出的 OSS 项目之一的源下面。

- 教程： [ASP.NET 标识的自定义的存储提供程序的概述](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)通过 Tom FitzMacken
- 博客：[实现 ASP.NET 标识](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- 教程：[设置了基本的标识帐户，并指向外部 DB](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx)。 通过[ @xivSolutions ](https://twitter.com/xivSolutions)。
- 教程[： 实现自定义 MySQL ASP.NET 标识存储提供程序](../extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [CodeFluent 实体](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/)通过[SoftFluent](http://www.softfluent.com/)
- [Azure 表存储](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/)James 美丽通过。
- Azure 表存储： [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage)通过[ @stuartleeks ](https://twitter.com/stuartleeks)。
- [CouchDB / 通过 Daniel Wertheim Cloudant。](https://github.com/danielwertheim/mycouch.aspnet.identity)
- 弹性搜索[h： 弹性标识](https://github.com/bmbsqd/elastic-identity)通过 Bombsquad AB.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) Jonathan Sheely Jonathan Sheely 通过。
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) Antônio Milesi Bastos 通过。
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0)通过[ @tourismgeek ](https://twitter.com/tourismgeek)。
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity)通过[ILMServices](http://www.ilmservice.com/)。
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- T4 模板生成 EF 的代码"第一个数据库"的用户存储区： [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)

<a id="additional"></a>
## <a name="additional-aspnet-identity-resources"></a>其他 ASP.NET 标识资源

- [引入 owin 在 Yahoo 和 LinkedIn OAuth 安全提供程序](http://blog.beabigrockstar.com/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/)通过 Jerrie Pelser 有关 Yahoo 和 LinkedIn 说明。

<a id="qand"></a>
## <a name="qampa-questionanswer"></a>Q&amp;（问题/答案）

- 问： 锁定退出启用了"记住我"（以便他们不需要在该计算机/浏览器上经历 2FA） 的用户不锁定。为何以及如何我避免这种？ 应答[此处](http://stackoverflow.com/questions/24312247/locked-out-users-can-login-if-they-have-auth-cookie)。
- **Q**： 如何将自定义声明，如用户的真实名称，存储在 ASP.NET Identity cookie，以避免对每个请求的不必要的数据库查询。 应答[此处](http://stackoverflow.com/questions/23622047/identity-cookie-loses-custom-claim-information-after-a-period-of-time)。
- **问： 更新 AspNetUser 密码哈希**： 我有 2 个项目。 其中一个使用 ASP.NET 身份验证，另一个则使用 Windows 身份验证，这是管理方。 我想要能够管理其他用户的管理员项目。 我可以修改密码以外的所有内容。 [此处回答](http://stackoverflow.com/questions/23880666/updating-aspnetuser-password-hash)。
- **Q**： 如何重新密码设置为其他用户的管理员？ 应答[此处](http://stackoverflow.com/questions/23783249/identity-2-0-reset-password-by-admin/24211766#24211766)。
- **Q**： 可以更改 ASP.NET MVC IdentityUser 中的用户名字段的显示的名称？ 应答[此处](http://stackoverflow.com/questions/23256650/can-i-change-the-displayed-name-of-the-username-field-in-asp-net-mvc-identityuse)。
- **Q**： 如何 gran 用户权限，以将其他用户添加到某些角色？ 应答[此处](http://stackoverflow.com/questions/23695373/allow-users-to-grant-permissions-to-other-users-for-their-account-in-asp-net-ide)。
- **Q**: AspNetUserClaims 表与 AspNetUsers 表中存储配置文件信息。 应答[此处](http://stackoverflow.com/questions/23215727/is-there-any-benefit-to-storing-user-information-in-aspnetuserclaims-with-asp-ne)。
- **Q**： 记住我使用外部身份验证提供程序时。 应答[此处](http://stackoverflow.com/questions/23180896/how-to-remember-the-login-in-mvc5-when-an-external-provider-is-used)。
- **Q**： 为什么每个请求需要 ApplicationDBContext，不是太多开销？。 回答： 否，开销会较低。
- 问： 如何获取在已登录用户的列表？ 应答[此处](http://stackoverflow.com/questions/22995653/getting-a-list-of-logged-in-users-in-asp-net-identity/)。
- 问： 如何检测当用户登录时 Microsoft.AspNet.Identity？ 应答[此处](http://stackoverflow.com/questions/22956486/how-can-i-detect-when-a-user-logs-in-with-microsoft-aspnet-identity/22970698#22970698)。
- 问： 如何标识获取本地化的错误消息？ 应答[此处](http://stackoverflow.com/questions/22835981/asp-net-identity-localization-publickeytoken/22845864#22845864)。
- 问： 如何配置以获取每隔 30 分钟的全新的声明 CookieMiddleware？ 应答[此处](http://stackoverflow.com/questions/22682663/how-to-hold-the-cookies-claims-updated-with-mcv5-owin/22796932#22796932)。
- 问： 如何修改用户的声明后已登录？ 应答[此处](http://stackoverflow.com/questions/22768836/can-i-modify-claims-in-asp-net-identity-with-owin-after-calling-signin/22769963#22769963)。
- 问： 如何使安全令牌的失效？ 应答[此处](http://stackoverflow.com/questions/22755700/revoke-token-generated-by-usertokenprovider-in-asp-net-identity-2-0/22767286#22767286)。
- 问： 如何执行是存储在 cookie 中间件的声明？ 应答[此处](http://stackoverflow.com/questions/22320632/storing-retrieving-user-data-without-database-when-using-owin-cookie-authenticat/22541856#22541856)。
- 问： 我想要具有 PIN 或安全检查我 MVC 的应用程序中每个操作方法，但我想要存储用户成功，以便他们不需要到该操作方法对每个请求输入 PIN。 应答[此处](http://stackoverflow.com/questions/22479958/security-check-an-user-to-a-access-controller-action/22486075#22486075)。
- 问： 我想，到保存到的数据库，从社交提供程序返回电子邮件地址我该怎么办？ 应答[此处](http://stackoverflow.com/questions/22888397/save-claims-to-db-on-login/22970969#22970969):
- 问： 如何检测这两个与/与扩展"记住我"cookie 用户登录时？ 应答[此处](http://stackoverflow.com/questions/22956486/how-can-i-detect-when-a-user-logs-in-with-microsoft-aspnet-identity/22970698#22970698)。
- 问： 是否可以在调用 SignIn 后修改声明，在带 OWIN ASP.NET Identity 中？ 答案是： 调用 SignIn 正是你想要修改的用户的声明时执行的操作是应。 它基本上可使 ClaimsIdentity 要序列化到该 cookie，这正是你查看显示在后续请求的新声明。
