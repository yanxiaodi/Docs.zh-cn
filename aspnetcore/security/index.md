---
title: "ASP.NET Core 安全性概述"
author: rachelappel
description: "了解 ASP.NET Core 中的身份验证、授权和安全基础知识。"
manager: wpickett
ms.author: rachelap
ms.date: 11/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/index
ms.openlocfilehash: e03256d7b8b442569b0b0126983732c10817e20f
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="aspnet-core-security-overview"></a>ASP.NET Core 安全性概述

通过 ASP.NET Core，开发者可轻松配置和管理其应用的安全性。 ASP.NET Core 的功能包括管理身份验证、授权、数据保护、SSL 强制、应用机密、请求防伪保护及 CORS 管理。 通过这些安全功能，可以生成安全可靠的 ASP.NET Core 应用。

## <a name="aspnet-core-security-features"></a>ASP.NET Core 安全性功能

ASP.NET Core 提供许多用于保护应用安全的工具和库（包括内置标识提供程序），但也可使用第三方标志服务（如 Facebook、Twitter 或 LinkedIn）。 利用 ASP.NET Core 可以轻松管理应用机密，无需将机密信息暴露在代码中就可存储和使用它们。

## <a name="authentication-vs-authorization"></a>身份验证 vs授权

身份验证是这样一个过程：由用户提供凭据，然后将其与存储在操作系统、数据库、应用或资源中的凭据进行比较。 在授权过程中，如果凭据匹配，则用户身份验证成功，可执行已向其授权的操作。 授权指判断允许用户执行的操作的过程。

对身份验证的另一种理解是将其看作进入某一空间（如服务器、数据库、应用或资源）的方式，而将授权看作用户可对该空间（服务器、数据库或应用）内的对象执行的操作。

## <a name="common-vulnerabilities-in-software"></a>软件中的常见漏洞

ASP.NET Core 和 EF 提供维护应用安全、预防安全漏洞的功能。 下表中链接的文档详细介绍了在 Web 应用中避免最常见安全漏洞的技术：

* [跨站点脚本攻击](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting)
* [SQL 注入式攻击](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [跨站点请求伪造 (CSRF)](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery)
* [打开重定向攻击](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects)

还应注意其他漏洞。 有关详细信息，请参阅本文档中关于 ASP.NET Core 安全文档的部分。

## <a name="aspnet-security-documentation"></a>ASP.NET Core 安全文档

*   [身份验证](authentication/index.md)
    *   [标识简介](authentication/identity.md)
    *   [启用使用 Facebook、Google 和其他外部提供程序的身份验证](authentication/social/index.md)
    *   [通过 WS 联合身份验证启用身份验证](authentication/ws-federation.md)
    * [配置 Windows 身份验证](authentication/windowsauth.md)
    *   [帐户确认和密码恢复](authentication/accconfirm.md)
    *   [使用 SMS 设置双因素身份验证](authentication/2fa.md)
    *   [在没有标识的情况下使用 cookie 身份验证](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [将 Azure AD 集成到 ASP.NET Core Web 应用中](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [使用 Azure AD 从 WPF 应用调用 ASP.NET Core Web API](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [使用 Azure AD 在 ASP.NET Core Web 应用中调用 Web API](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [带有 Azure AD B2C 的 ASP.NET Core Web 应用](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [使用 IdentityServer4 保护 ASP.NET Core 应用](https://identityserver4.readthedocs.io)
*   [授权](authorization/index.md)
    *   [介绍](authorization/introduction.md)
    *   [通过授权保护的用户数据创建应用](xref:security/authorization/secure-data)
    *   [简单授权](authorization/simple.md)
    *   [基于角色的授权](authorization/roles.md)
    *   [基于声明的授权](authorization/claims.md)
    *   [基于策略的授权](authorization/policies.md)
    *   [要求处理程序中的依赖关系注入](authorization/dependencyinjection.md)
    *   [基于资源的授权](authorization/resourcebased.md)
    *   [基于视图的授权](authorization/views.md)
    *   [使用方案限制标识](authorization/limitingidentitybyscheme.md)
*   [数据保护](data-protection/index.md)
    *   [数据保护简介](data-protection/introduction.md)
    *   [数据保护 API 入门](data-protection/using-data-protection.md)
    *   [使用者 API](data-protection/consumer-apis/index.md)
        *   [使用者 API 概述](data-protection/consumer-apis/overview.md)
        *   [目标字符串](data-protection/consumer-apis/purpose-strings.md)
        *   [目标层次结构和多租户](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [密码哈希](data-protection/consumer-apis/password-hashing.md)
        *   [限制受保护负载的生存期](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [取消保护已撤消其密钥的负载](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [配置](data-protection/configuration/index.md)
        *   [配置数据保护](data-protection/configuration/overview.md)
        *   [默认设置](data-protection/configuration/default-settings.md)
        *   [计算机范围的策略](data-protection/configuration/machine-wide-policy.md)
        *   [非 DI 感知方案](data-protection/configuration/non-di-scenarios.md)
    *   [扩展性 API](data-protection/extensibility/index.md)
        *   [核心加密扩展性](data-protection/extensibility/core-crypto.md)
        *   [密钥管理扩展性](data-protection/extensibility/key-management.md)
        *   [其他 API](data-protection/extensibility/misc-apis.md)
    *   [实现](data-protection/implementation/index.md)
        *   [已验证的加密详细信息](data-protection/implementation/authenticated-encryption-details.md)
        *   [子项派生和已验证的加密](data-protection/implementation/subkeyderivation.md)
        *   [上下文标头](data-protection/implementation/context-headers.md)
        *   [密钥管理](data-protection/implementation/key-management.md)
        *   [密钥存储提供程序](data-protection/implementation/key-storage-providers.md)
        *   [静态密钥加密](data-protection/implementation/key-encryption-at-rest.md)
        *   [密钥永久性和更改设置](data-protection/implementation/key-immutability.md)
        *   [密钥存储格式](data-protection/implementation/key-storage-format.md)
        *   [短数据保护提供程序](data-protection/implementation/key-storage-ephemeral.md)
    *   [兼容性](data-protection/compatibility/index.md)
        *   [在 ASP.NET 中替换 <machineKey>](data-protection/compatibility/replacing-machinekey.md)
*   [通过授权保护的用户数据创建应用](xref:security/authorization/secure-data)
*   [在开发期间安全存储应用密钥](app-secrets.md)
*   [Azure Key Vault 配置提供程序](key-vault-configuration.md)
*   [强制实施 SSL](enforcing-ssl.md)
*   [防请求伪造](anti-request-forgery.md)
*   [阻止打开重定向攻击](preventing-open-redirects.md)
*   [阻止跨站点脚本编写](cross-site-scripting.md)
*   [启用跨域请求 (CORS)](cors.md)
*   [在应用之间共享 Cookie](cookie-sharing.md)
